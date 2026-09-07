# CHAPTER 4

# Error Mitigation Techniques

*Unit 2 · Zero-Noise Extrapolation · Probabilistic Error Cancellation · Pauli Twirling · Readout Mitigation · Qiskit Estimator*

<div class="box box-learning-objectives">
<p class="box-title"><strong>📋 Learning Objectives — Chapter 4</strong></p>
<p>(1) Explain the principle of Zero-Noise Extrapolation (ZNE): gate folding for noise amplification and Richardson extrapolation for zero-noise estimation.</p>
<p>(2) Describe Probabilistic Error Cancellation (PEC): quasi-probability decomposition, negative coefficients, one-norm γ, and sampling overhead γ².</p>
<p>(3) Explain how Pauli Twirling converts arbitrary (coherent) noise into stochastic Pauli noise channels.</p>
<p>(4) Implement readout error mitigation using the calibration matrix and explain the M3 matrix-free method.</p>
<p>(5) Use the Qiskit Runtime Estimator with resilience levels 0, 1, and 2 for automatic error mitigation.</p>
<p>(6) Identify the regime of applicability and limitations of each mitigation method.</p>
</div>

## 4.1 Introduction to Error Mitigation

Error mitigation is a collection of techniques that reduce the effective noise in quantum computations on NISQ hardware without the full overhead of quantum error correction (QEC). The fundamental idea: trade increased classical post-processing effort (and, in some methods, increased circuit executions) for reduced bias in estimated expectation values of quantum observables.

The critical distinction: error mitigation is NOT error correction. Quantum error correction (QEC) encodes logical qubits into many physical qubits and actively corrects errors during computation, enabling arbitrarily long fault-tolerant computations. Error mitigation operates on the output of noisy circuits, partially reducing the bias, but cannot suppress errors exponentially or enable arbitrarily long computations. Error mitigation is appropriate for NISQ-era computations where: (a) qubit count is insufficient for QEC overhead; (b) gate fidelities are high enough that noise is a systematic perturbation rather than completely overwhelming; (c) the computation produces expectation values ⟨O⟩ that can be bias-corrected.

<table>
<thead><tr>
<th><strong>Method</strong></th>
<th><strong>What it corrects</strong></th>
<th><strong>Overhead</strong></th>
<th><strong>Key Limitation</strong></th>
</tr></thead>
<tbody>
<tr>
<td>Zero-Noise Extrapolation (ZNE)</td>
<td>All gate errors (coherent + incoherent)</td>
<td>~3–10× more circuits</td>
<td>Fails for long circuits; assumes smooth noise scaling</td>
</tr>
<tr>
<td>Probabilistic Error Cancellation (PEC)</td>
<td>Any specific error model</td>
<td>γ² exponential in circuit depth</td>
<td>Requires accurate noise model; exponential overhead</td>
</tr>
<tr>
<td>Pauli Twirling</td>
<td>Converts coherent to Pauli noise</td>
<td>~10–50× more circuits</td>
<td>Does NOT reduce noise magnitude; must combine with other methods</td>
</tr>
<tr>
<td>Readout Error Mitigation</td>
<td>Measurement classification errors</td>
<td>Calibration circuits + matrix inversion</td>
<td>Exponential calibration cost for n > 15 qubits</td>
</tr>
<tr>
<td>Qiskit Estimator (resilience=2)</td>
<td>ZNE + readout combined automatically</td>
<td>~5–10× total overhead</td>
<td>Abstracts away implementation; limited control over extrapolation</td>
</tr>
</tbody></table>

***Table 4.1:** Summary of major error mitigation methods, what each corrects, the overhead required, and the primary limitation. All methods are most effective in the "few-error regime" where total circuit error n·p << 1.*

## 4.2 Zero-Noise Extrapolation (ZNE)

Zero-Noise Extrapolation (ZNE) is the most widely used and conceptually straightforward error mitigation method. The core idea: if we can evaluate the expectation value of an observable at multiple noise levels, we can fit a curve and extrapolate to zero noise — without needing to know the details of the noise model.

The expectation value of an observable O under a noisy circuit with noise factor λ (λ = 1 for the original circuit, λ > 1 for amplified noise) is approximately:

<div class="box box-equation">
<p><strong>E(λ) = E_ideal + a₁λ + a₂λ² + a₃λ³ + ... (Taylor expansion in λ)</strong> <em>[Noisy expectation value vs noise factor]</em></p>
</div>

where the coefficients aₖ depend on the circuit, noise model, and observable. At λ = 0: E(0) = E_ideal (the zero-noise answer we want). At λ = 1: E(1) = E_actual (the noisy measurement). By measuring E(λ) at multiple values of λ > 1, we can fit the polynomial and extrapolate to λ = 0.

### 4.2.1 Gate Folding: Amplifying Noise Controllably

The central challenge of ZNE is controllably amplifying the noise by a known factor λ, while keeping the ideal quantum circuit operation unchanged. Gate folding achieves this by replacing each gate G with a product that is equal to G in the noiseless limit but applies (2k+1) times the noise for noise factor λ = 2k+1:

<div class="box box-equation">
<p>G → G ·G †·G (λ = 3) or G → G ·(G †·G)ᵏ (λ = 2k + 1) <em>[Gate folding for noise amplification]</em></p>
</div>

Since G†G = I for unitary gates, the product G·G†·G = G ideally — the circuit operation is unchanged. But each gate application contributes noise, so G·G†·G applies 3× as much noise as a single G. The expectation value E(λ) at noise factor λ = 3 is measured by running the folded circuit. Gate folding can be applied to all gates (global folding), or to random subsets (random folding), or to specific high-error gates.

### 4.2.2 Richardson Extrapolation and Polynomial Fitting

Given measurements at N different noise factors λ₁, λ₂, ..., λN, Richardson extrapolation computes a weighted linear combination that exactly cancels the polynomial noise terms up to order N−1:

<div class="box box-equation">
<p>E_mit = Σᵢ γᵢ · E(λᵢ) where Σᵢ γᵢ = 1 and Σᵢ γᵢ ·λᵢᵏ = 0 for k = 1,...,N - 1 <em>[Richardson extrapolation formula]</em></p>
</div>

For linear extrapolation (N=2) with λ₁=1, λ₂=3:

<div class="box box-equation">
<p><strong>E_mit = (3/2)·E(1) − (1/2)·E(3)</strong> <em>[Richardson linear extrapolation (λ₁=1, λ₂=3)]</em></p>
</div>

This eliminates the linear noise term a₁λ, leaving residual error O(a₂λ²). For quadratic extrapolation (N=3) with λ₁=1, λ₂=2, λ₃=3:

<div class="box box-equation">
<p><strong>E_mit = (1/2)·E(1) − 2·E(2) + (5/2)·E(3)</strong> <em>[Richardson quadratic extrapolation]</em></p>
</div>

This eliminates a₁λ and a₂λ² terms, leaving residual O(a₃λ³). Each additional noise factor reduces the residual error by one order in λ, at the cost of one additional set of circuit executions. For small λ (low noise), linear extrapolation is usually sufficient. The ZNE mitigated estimate has standard error that grows with the number of noise factors (variance inflation from the Richardson coefficients).

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image34.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Zero-Noise Extrapolation — Gate Folding and Richardson Extrapolation</strong></p>
<p><em><strong>Figure 4.1:</strong> Left: Gate folding demonstration. Original circuit with 3 CNOT gates (λ=1, each shown as blue box). Folded circuit at λ=3 — each CNOT replaced by CNOT·CNOT†·CNOT (three physical gates implementing one ideal CNOT with 3× the noise). Centre: ZNE workflow — three circuits are executed: λ=1 (original), λ=2 (some gates folded), λ=3 (all gates folded). Measured expectation values E(1), E(2), E(3) are plotted vs noise factor λ (blue dots with error bars). Richardson quadratic extrapolation (orange curve through the three points) is extrapolated to λ=0 to obtain the mitigated estimate E_mit (orange star). The unmitigated value E(1) (blue dashed) and ideal value E_ideal (gold dashed) are shown for comparison. Right: Variance comparison — ZNE increases the statistical variance of the estimator by a factor ~4 (for linear extrapolation with λ₁=1, λ₂=3), requiring ~4× more shots for the same statistical accuracy.</em></p>
</div>

## 4.3 Probabilistic Error Cancellation (PEC)

Probabilistic Error Cancellation (PEC) is a more powerful but more expensive error mitigation method that directly inverts the noise process. Instead of estimating and extrapolating the noise-dependent expectation value, PEC reconstructs the ideal expectation value by sampling circuits from a quasi-probability distribution that exactly cancels the noise.

### 4.3.1 Quasi-Probability Decomposition

The idea: decompose the ideal (noiseless) gate G_ideal as a linear combination of implementable noisy operations {G_noisy,k}:

<div class="box box-equation">
<p><strong>G_ideal = Σₖ cₖ · G_noisy,k where Σₖ cₖ = 1</strong> <em>[PEC quasi-probability decomposition]</em></p>
</div>

The coefficients cₖ are real (possibly negative) — they form a quasi-probability distribution. Negative coefficients arise because the ideal operation generally cannot be expressed as a convex (all-positive) combination of noisy operations: if it could, no mitigation would be needed. The ideal expectation value is reconstructed by:

<div class="box box-equation">
<p><strong>⟨O⟩_ideal = γ · Σₖ sign(cₖ) · |cₖ|/γ · ⟨O⟩_noisy,k where γ = Σₖ |cₖ| ≥ 1</strong> <em>[PEC estimator formula]</em></p>
</div>

In practice: sample circuit k with probability |cₖ|/γ, compute the expectation ⟨O⟩_noisy,k from that circuit, and multiply by γ·sign(cₖ). Averaging over many such samples gives the unbiased estimator of ⟨O⟩_ideal.

### 4.3.2 Monte Carlo Sampling and Sampling Overhead

The sampling overhead arises from the variance of the stochastic PEC estimator. The total number of circuit executions N_PEC required to achieve estimation accuracy ε is:

<div class="box box-equation">
<p>N_PEC = γ² · N_ideal / ε² or equivalently SNR ∝ 1/γ <em>[PEC sampling overhead]</em></p>
</div>

For a single gate with error rate p: γ_gate ≈ 1 + 2p. For a circuit with n independent gates each of error rate p: γ_total = (1 + 2p)ⁿ ≈ e^(2np). The total overhead γ_total² = e^(4np) grows exponentially with circuit depth n·p. For 100 gates at 0.1% error: γ = e^0.2 ≈ 1.22, overhead = 1.49× — manageable. For 1000 gates: γ = e^2 ≈ 7.4, overhead = 55× — significant but feasible for targeted circuits. For 10,000 gates at 0.1%: γ = e^20 ~ 5×10⁸ — impractical. PEC is therefore limited to circuits where the total error n·p ≪ 1.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image35.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Probabilistic Error Cancellation — Quasi-Probability Decomposition and Sampling Overhead</strong></p>
<p><em><strong>Figure 4.2:</strong> Left: Quasi-probability decomposition of an ideal CNOT gate as a linear combination of noisy operations. The ideal CNOT (gold) is decomposed into: (a) noisy CNOT with coefficient c₁ = γ > 0; (b) CNOT preceded by Pauli corrections with negative coefficients cₖ < 0. The one-norm γ = Σ|cₖ| quantifies the decomposition difficulty — larger γ means more overhead. Centre: PEC Monte Carlo sampling workflow — for each "PEC circuit execution", sample operation k with probability |cₖ|/γ; run the corresponding noisy circuit; multiply result by γ·sign(cₖ); average N_PEC times. The average unbiasedly recovers ⟨O⟩_ideal. Right: Sampling overhead γ² vs circuit depth n for three error rates p = 0.1%, 0.5%, 1.0%. Dashed red line at γ² = 100 shows the practical limit of ~100× overhead — at this limit, PEC is still useful for small p at moderate depth but becomes impractical for large p or deep circuits.</em></p>
</div>

## 4.4 Pauli Twirling

Pauli Twirling is an error mitigation technique that converts arbitrary (possibly coherent) noise into a stochastic Pauli noise channel. The technique does not reduce the noise magnitude — it only changes the noise character from coherent (systematic) to incoherent (random), which is easier to characterise and mitigate.

Twirling protocol for a two-qubit gate G: (1) Before G, apply a random Pauli operator Pᵢ ∈ {I,X,Y,Z}⊗{I,X,Y,Z} (16 possibilities for 2 qubits). (2) Apply the noisy gate G·E (where E is the noise channel). (3) After G, apply the conjugate Pauli P̃ᵢ = G·Pᵢ†·G† (the Pauli that "undoes" Pᵢ through the ideal gate, maintaining the correct circuit). (4) Average the results over all 16 random Paulis. The noise channel E(ρ) transforms under averaging to:

<div class="box box-equation">
<p><strong>E_twirled(ρ) = Σₚ qₚ · P ρ P† (Pauli channel — all cross terms vanish)</strong> <em>[Pauli twirled channel]</em></p>
</div>

where qₚ are the probabilities of each Pauli error. The key result: any noise channel (including coherent errors, axis misalignments, and systematic over-rotations) becomes a Pauli channel after twirling. Pauli channels are completely characterised by N = 4ⁿ − 1 error probabilities for n qubits, and are the standard noise model assumed by Randomised Benchmarking, Pauli eigenvalue estimation, and many other characterisation tools. After twirling, these tools give accurate characterisation — enabling more precise ZNE and PEC.

<div class="box box-key-concept">
<p class="box-title"><strong>🔑 What Twirling Does and Does NOT Do</strong></p>
<p>Pauli Twirling DOES: convert coherent gate errors (systematic rotation angle errors, axis misalignments) into stochastic Pauli noise; randomise the error direction so that coherent errors do not accumulate directionally in structured circuits; make the noise channel compatible with Pauli noise models assumed by RB and PEC.</p>
<p>Pauli Twirling does NOT: reduce the total noise magnitude — the average gate infidelity F_avg is preserved or may slightly increase after twirling; provide a direct reduction in observable error without combining with ZNE or PEC; help with T₁ relaxation noise (amplitude-damping is not a Pauli channel).</p>
<p>Best practice: apply Pauli twirling first to convert coherent noise to Pauli noise, then apply ZNE or PEC to reduce the magnitude of the resulting Pauli noise.</p>
</div>

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image36.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Pauli Twirling — Converting Coherent to Pauli Noise</strong></p>
<p><em><strong>Figure 4.3:</strong> Left: Untwirled noisy CNOT circuit. The noise channel E after the CNOT contains both coherent and incoherent error components. The Bloch sphere evolution shows a systematic rotation (coherent) combined with shrinkage (incoherent). Centre: Twirled CNOT circuit. A random Pauli P (blue dashed boxes) is applied before the CNOT and the conjugate P̃ = CNOT·P†·CNOT after. Averaging over all 16 two-qubit Paulis P converts E to E_twirled — a Pauli channel. The Bloch sphere evolution now shows only isotropic shrinkage (no coherent rotation). Right: Comparison of noise characterisation accuracy before and after twirling. RB applied to untwirled gates gives a biased EPC estimate due to coherent errors. After twirling, RB gives an unbiased EPC estimate of the Pauli channel error rates. The twirling overhead (16× more circuits per gate) is the cost of this improved characterisation.</em></p>
</div>

## 4.5 Readout Error Mitigation

### 4.5.1 Calibration Matrix Method

Readout errors are misclassifications of the qubit state during measurement: |0⟩ is measured as |1⟩ with probability p₀₁ (the "0→1 confusion rate"), and |1⟩ is measured as |0⟩ with probability p₁₀. For a single qubit, the calibration (confusion) matrix A is:

<div class="box box-equation">
<p><strong>A = [[1−p₀₁, p₁₀], [p₀₁, 1−p₁₀]] with P_measured = A · P_ideal</strong> <em>[Readout calibration matrix]</em></p>
</div>

Calibration: prepare |0⟩ (~1000 shots) and measure; the fraction of |1⟩ outcomes = p₀₁. Prepare |1⟩ (via X gate) and measure; fraction of |0⟩ outcomes = p₁₀. Mitigation is performed by matrix inversion:

<div class="box box-equation">
<p><strong>P_mitigated = A⁻¹ · P_measured</strong> <em>[Readout error correction by matrix inversion]</em></p>
</div>

For n qubits: A is a 2ⁿ × 2ⁿ matrix requiring 2ⁿ calibration experiments (one for each computational basis state). For n = 5: 32 experiments. For n = 10: 1024 experiments. For n = 20: 1,048,576 experiments — exponential, clearly impractical. This exponential scaling motivates the M3 approach.

### 4.5.2 M3: Matrix-Free Measurement Mitigation

M3 (Matrix-free Measurement Mitigation, Nation et al., PRX Quantum 2021) is a scalable readout mitigation method that avoids constructing the full 2ⁿ × 2ⁿ calibration matrix. The key insight: for a typical NISQ circuit output, only a small number of bitstrings s (typically s << 2ⁿ) appear with significant probability. M3 performs calibration only for those s prominent bitstrings (sparse calibration), constructs a reduced (s×s) confusion matrix, and solves the linear system iteratively.

M3 scaling: O(n·s) calibration circuits where s is the number of non-negligible output bitstrings, compared to O(2ⁿ) for the full method. For most NISQ circuits with n ~ 20–100 qubits, s ~ 1000–10000 << 2ⁿ, making M3 practical. M3 is implemented in the mthree Python package and is integrated into Qiskit Runtime as the default readout mitigation method.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image37.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Readout Error Calibration Matrix and M3 Correction</strong></p>
<p><em><strong>Figure 4.4:</strong> Left: Single-qubit confusion matrix A visualised as a 2×2 heatmap. Diagonal elements (1−p₀₁) = 0.993 and (1−p₁₀) = 0.988 represent correct identification probability; off-diagonal elements p₀₁ = 0.007 and p₁₀ = 0.012 represent confusion rates. Centre: Multi-qubit confusion matrix for 3 qubits (8×8) — heatmap showing both individual qubit readout errors (block-diagonal) and correlated 2-qubit readout errors (off-diagonal cross-correlations). M3 identifies which blocks are significant and constructs the reduced confusion matrix for sparse mitigation. Right: M3 workflow for a 20-qubit circuit. Step 1: Run circuit, collect 4096 shots; top 200 bitstrings are identified (sparse output). Step 2: Calibrate only those 200 bitstrings using ~200 calibration circuits (vs 2²⁰ = 1,048,576 for full calibration). Step 3: Solve the reduced 200×200 linear system to correct bitstring probabilities. Corrected expectation values are 2–5× more accurate than uncorrected for typical superconducting qubit readout errors of ~0.5–1%.</em></p>
</div>

## 4.6 Qiskit Estimator with Resilience Levels

The Qiskit Runtime Estimator primitive provides a unified, hardware-agnostic interface for measuring expectation values ⟨O⟩ = ⟨ψ|O|ψ⟩ of quantum operators O with built-in error mitigation. The resilience_level parameter controls the degree of mitigation applied automatically:

<div class="box box-generic">
<p class="box-title"><strong>Qiskit Estimator Resilience Levels</strong></p>
<p>Level 0 (No mitigation): Raw expectation values from direct measurement counts. No calibration circuits, no folding, no post-processing. Fastest — 1× circuit overhead. Use when: benchmarking raw hardware, testing circuits, or when statistical uncertainty dominates over systematic noise bias.</p>
<p>Level 1 (Readout mitigation only): Applies M3 readout error mitigation automatically. Calibration circuits (~10–50 extra circuits) are run automatically; the confusion matrix is measured and used to correct count distributions before computing expectation values. ~1.1–1.5× total circuit overhead. Use when: readout errors dominate (p₀₁, p₁₀ > 0.5%) but gate fidelities are high.</p>
<p>Level 2 (ZNE + readout mitigation): Applies both Zero-Noise Extrapolation (gate folding at λ=1, 2, 3 with Richardson linear extrapolation) AND M3 readout mitigation. Provides the best error reduction for typical NISQ circuits. ~5–10× total circuit overhead (3× for ZNE + ~1.5× for readout calibration). Use when: computing accurate expectation values for VQE, QAOA, quantum chemistry, or machine learning applications on real hardware.</p>
</div>

Typical Qiskit code for using the Estimator with resilience level 2:

<div class="box box-equation">
<p><strong>from qiskit_ibm_runtime import EstimatorV2, Options</strong> <em>[Python import]</em></p>
<p><strong>options = Options(); options.resilience_level = 2; options.optimization_level = 1</strong> <em>[Options setup]</em></p>
<p><strong>estimator = EstimatorV2(backend=backend, options=options)</strong> <em>[Estimator instantiation]</em></p>
<p><strong>job = estimator.run([(circuit, observable)]); result = job.result()</strong> <em>[Run and get results — mitigated ⟨O⟩]</em></p>
</div>

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image38.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Qiskit Estimator Resilience Levels 0/1/2 Workflow</strong></p>
<p><em><strong>Figure 4.5:</strong> Flowchart showing the Qiskit Estimator workflow for each resilience level. Centre column: main computation circuit (blue box) is always executed. Level 0 (top branch): single circuit execution → raw counts → raw ⟨O⟩ (no mitigation). Level 1 (middle branch): main circuit + M3 calibration circuits (readout calibration matrix A measured) → M3-corrected counts → readout-corrected ⟨O⟩. Level 2 (bottom branch): three circuit executions (λ=1, λ=2, λ=3 via gate folding) + M3 calibration → three noisy expectation values E(1), E(2), E(3) → Richardson linear extrapolation to E(λ=0) → readout-corrected E_mit. Right panel: bar chart comparing ⟨ZZ⟩ for a two-qubit Bell state at Level 0 (noisy, ~0.75), Level 1 (readout-corrected, ~0.83), and Level 2 (ZNE+readout, ~0.94) vs the ideal value (1.0). ZNE reduces systematic gate error bias by ~3–4× for this circuit.</em></p>
</div>

<div class="box box-roadmap">
<p class="box-title"><strong>RECAP</strong></p>
<p><em>Chapter 4: Error Mitigation Techniques — Short Answer Questions & Model Answers</em></p>
</div>

## Short Answer Questions — Chapter 4

*Instructions: Answer each question in 3–6 lines.*

**Q1.** What is error mitigation and how does it fundamentally differ from quantum error correction?

*[§4.1]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q2.** Explain the gate folding technique for ZNE. Write the folded circuit for noise factor λ = 3.

*[§4.2.1]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q3.** State the Richardson linear extrapolation formula for ZNE with λ₁ = 1, λ₂ = 3.

*[§4.2.2]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q4.** What is the quasi-probability decomposition in PEC? Why can the coefficients cₖ be negative?

*[§4.3.1]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q5.** State the PEC sampling overhead formula. Calculate N_PEC for 100 gates at p = 0.1% error.

*[§4.3.2]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q6.** What does Pauli Twirling do to a quantum noise channel? What does it NOT do?

*[§4.4]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q7.** Write the readout calibration matrix A for a single qubit. How is mitigation performed?

*[§4.5.1]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q8.** What is M3 and why is it preferred over the full calibration matrix for n > 15 qubits?

*[§4.5.2]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q9.** List the three Qiskit Estimator resilience levels and the mitigation applied at each level.

*[§4.6]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q10.** For what circuit conditions does ZNE fail or become unreliable? Give two conditions.

*[§4.2]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q11.** Why does PEC sampling overhead grow exponentially with circuit depth n?

*[§4.3.2]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q12.** Does Pauli Twirling reduce the average gate infidelity? Explain why or why not.

*[§4.4]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q13.** For a 5-qubit system, how many calibration experiments does the full readout matrix method require?

*[§4.5.1]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q14.** What is the primary advantage of Qiskit's Estimator primitive over manual expectation value computation?

*[§4.6]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q15.** In which era of quantum computing is error mitigation most useful, and why not in the fault-tolerant era?

*[§4.1]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

## Model Answers — Chapter 4

**Answer 1:**

<div class="box box-equation">
<p>Error mitigation uses classical post-processing and circuit modifications to reduce bias in quantum computation outputs on NISQ hardware, without encoding logical qubits. Unlike quantum error correction (QEC), which encodes one logical qubit into many physical qubits and actively detects/corrects errors during computation, error mitigation does not protect quantum information during computation and cannot enable arbitrarily long fault-tolerant computations. QEC suppresses errors exponentially with code size; error mitigation only partially reduces errors and cannot overcome the noise floor. QEC requires ancilla qubits, syndrome measurements, and fault-tolerant gate sets (large overhead); error mitigation requires only additional circuit executions and classical computation (manageable overhead). Error mitigation is the practical approach for current NISQ devices where QEC overhead is not yet affordable.</p>
</div>

**Answer 2:**

<div class="box box-equation">
<p>Gate folding replaces each unitary gate G with a product that implements the same ideal unitary but applies more physical gates (and hence more noise). Since G†G = I for any unitary gate G, the product G·G†·G equals G in the noiseless limit, but with three physical gate applications it incurs 3× the noise. For noise factor λ = 3: G → G·G†·G (three gate applications, 3× noise, same ideal operation). For λ = 5: G → G·G†·G·G†·G (five applications). For general odd λ = 2k+1: G → G·(G†·G)ᵏ. The expectation value E(λ=3) is measured by running the folded circuit, and the measurements at different λ values are used in Richardson extrapolation to estimate E(λ=0).</p>
</div>

**Answer 3:**

<div class="box box-equation">
<p>Richardson linear extrapolation uses two noise-factor data points to estimate the zero-noise expectation value by eliminating the linear noise term a₁λ. Given E(λ₁=1) and E(λ₂=3), assuming E(λ) = E_ideal + a₁λ + O(λ²): Richardson coefficients are γ₁ = λ₂/(λ₂−λ₁) = 3/(3−1) = 3/2 and γ₂ = −λ₁/(λ₂−λ₁) = −1/2. The formula: E_mit = (3/2)·E(1) − (1/2)·E(3). Verification: E_mit = (3/2)(E_ideal+a₁) − (1/2)(3E_ideal+3a₁) = (3/2−3/2)E_ideal + (3/2−3/2)a₁ = E_ideal. The residual error is O(a₂λ²), which is second-order in noise — a significant improvement over the first-order unmitigated error.</p>
</div>

**Answer 4:**

<div class="box box-equation">
<p>In PEC, the ideal noiseless gate G_ideal is decomposed as a linear combination of implementable noisy operations: G_ideal = Σₖ cₖ·G_noisy,k. The coefficients cₖ sum to 1 (Σₖcₖ=1) but may be negative — they form a quasi-probability distribution, not a physical probability distribution. Negative coefficients are unavoidable because the ideal operation generally cannot be expressed as a convex (all-positive) combination of noisy operations: if it could, the noisy operations already span the ideal, meaning no error mitigation was needed (the noisy gates are already ideal). Negative coefficients allow the PEC estimator to "subtract" noise contributions: measurements from circuits with negative cₖ are multiplied by −1 and subtracted from positive contributions. The one-norm γ = Σₖ|cₖ| ≥ 1 quantifies the difficulty of the decomposition and determines the sampling overhead γ².</p>
</div>

**Answer 5:**

<div class="box box-equation">
<p>PEC sampling overhead formula: N_PEC = γ² × N_ideal, where γ = Σₖ|cₖ| = (1+2p)ⁿ for n independent gates each with error rate p. Calculation for 100 gates, p = 0.1% = 0.001: γ = (1+2×0.001)^100 = (1.002)^100 ≈ e^(0.002×100) = e^0.2 ≈ 1.221. N_PEC = γ² × N_ideal = 1.221² × N_ideal ≈ 1.49 × N_ideal. Interpretation: PEC requires approximately 49% more shots than the unmitigated circuit to achieve the same statistical accuracy. This is manageable for 100 gates at 0.1% error. For comparison: 1000 gates at 0.1%: γ = e^2 ≈ 7.4, overhead = 55×; 10,000 gates: γ = e^20 ≈ 5×10⁸, overhead = 10^18 — completely impractical.</p>
</div>

**Answer 6:**

<div class="box box-equation">
<p>Pauli Twirling converts an arbitrary noise channel (which may contain coherent errors, axis misalignments, systematic rotations) into a stochastic Pauli noise channel where errors are random Pauli operators (I, X, Y, Z on each qubit) applied with specific probabilities. This is achieved by inserting random Pauli gates before and after each target gate: before G apply Pᵢ; after G apply conjugate P̃ᵢ = G·Pᵢ†·G†. Averaging over all random Paulis transforms E(ρ) → E_twirled(ρ) = Σₚ qₚ·PρP†. What twirling does NOT do: it does NOT reduce the total noise magnitude — the average gate infidelity F_avg is preserved or may slightly increase. It does not help circuits with T₁ relaxation (amplitude-damping is not a Pauli channel). The benefit is qualitative: converting coherent errors to Pauli noise makes them compatible with RB, PEC, and other tools that assume Pauli noise models.</p>
</div>

**Answer 7:**

<div class="box box-equation">
<p>Readout calibration matrix for a single qubit: A = [[1−p₀₁, p₁₀], [p₀₁, 1−p₁₀]], where p₀₁ = P(measure 1 | state is 0) and p₁₀ = P(measure 0 | state is 1). This satisfies P_measured = A·P_ideal. Mitigation performs matrix inversion: P_corrected = A⁻¹·P_measured. Calibration experiments: (1) prepare |0⟩ (~1000 shots) and measure — the fraction of |1⟩ outcomes estimates p₀₁; (2) prepare |1⟩ via X gate (~1000 shots) and measure — the fraction of |0⟩ outcomes estimates p₁₀. The inverse A⁻¹ = (1/det(A))·[[1−p₁₀, −p₁₀], [−p₀₁, 1−p₀₁]], with det(A) = (1−p₀₁)(1−p₁₀) − p₀₁p₁₀. For n qubits: the matrix is 2ⁿ×2ⁿ, requiring 2ⁿ calibration experiments.</p>
</div>

**Answer 8:**

<div class="box box-equation">
<p>M3 (Matrix-free Measurement Mitigation) is a scalable readout mitigation method that avoids constructing the full 2ⁿ×2ⁿ calibration matrix. For most NISQ circuits with n >> 15 qubits, only a small number of bitstrings s (s << 2ⁿ) appear with significant probability in the output. M3 performs calibration only for those s prominent bitstrings (sparse calibration), constructs a reduced s×s confusion matrix, and solves the linear system iteratively. Scaling: O(n·s) calibration circuits vs O(2ⁿ) for the full method. For n=20 and s=1000: M3 needs ~20,000 calibration circuits vs 1,048,576 for full calibration — ~50× less overhead. M3 is preferred because the full method becomes impractical beyond n~15 (1,048,576 = 2²⁰ experiments at n=20) while M3 scales polynomially with the number of prominent output bitstrings.</p>
</div>

**Answer 9:**

<div class="box box-equation">
<p>Qiskit Estimator resilience levels: Level 0 (No mitigation) — raw expectation values from direct measurement, no post-processing. Single circuit execution per observable, fastest, 1× overhead. Used for debugging or when statistical uncertainty dominates. Level 1 (Readout mitigation) — applies M3 readout error mitigation automatically. Calibration circuits (~10–50) are automatically run and the confusion matrix is used to correct measurement distributions. ~1.1–1.5× total overhead. Level 2 (ZNE + readout mitigation) — applies both ZNE (gate folding at λ=1,2,3 with Richardson linear extrapolation) AND M3 readout mitigation. Provides the best systematic error reduction for typical NISQ circuits. ~5–10× total circuit overhead (3× for ZNE folding + ~1.5× for calibration). Standard choice for VQE, QAOA, and quantum chemistry expectation value computations.</p>
</div>

**Answer 10:**

<div class="box box-equation">
<p>ZNE fails or becomes unreliable in two key conditions: (1) High total circuit error (n·p >> 1): if the total circuit error is so large that the expectation value E(λ) is completely dominated by noise at all accessible noise factors λ, there is no reliable signal to extrapolate. ZNE requires n·p << 1 (the "few-error regime") to be effective. Specifically, when E(λ) is already close to the fully mixed value at λ=1, folding to λ=3 provides no additional signal. (2) Non-smooth noise scaling: gate folding assumes that noise scales smoothly and proportionally with the fold factor. This breaks down when: (a) noise is non-Markovian (memory effects make the folded noise non-proportional); (b) different error mechanisms (T₁ relaxation vs coherent errors) scale differently with folding; (c) the circuit structure changes under folding (e.g., if folded gates experience different crosstalk patterns than original gates).</p>
</div>

**Answer 11:**

<div class="box box-equation">
<p>PEC sampling overhead grows exponentially with circuit depth because the one-norm γ compounds multiplicatively across gates. Each gate contributes a local one-norm factor γ_gate = (1+2p) > 1. For n independent gates each with error rate p: γ_total = Πᵢ γᵢ = (1+2p)ⁿ ≈ e^(2np). The total sampling overhead is γ_total² = e^(4np) — exponential in circuit depth n. Physical reason: PEC must independently "undo" each gate's noise channel by sampling from its quasi-probability representation. The sampling variance from each gate compounds independently across all n gates, requiring exponentially more samples to maintain fixed estimation accuracy ε. Unlike ZNE which runs at most N+1 circuit versions, PEC requires an ensemble of exponentially many circuit variants when n·p >> 1. This is the fundamental limitation of PEC for deep NISQ circuits.</p>
</div>

**Answer 12:**

<div class="box box-equation">
<p>Pauli Twirling does NOT reduce the average gate infidelity. The total noise strength — measured by the average gate infidelity F_avg = 1 − F — is preserved or may slightly increase under twirling. Physically, twirling randomises the direction of errors (converting systematic rotation toward a fixed direction into errors equally distributed in X, Y, Z) but does not reduce their total probability. A systematic over-rotation of magnitude ε becomes a random Pauli error of roughly the same total probability ε. The benefit is qualitative, not quantitative: (1) Pauli noise is well-characterised by simple scalar probabilities, making ZNE and PEC more accurate; (2) Coherent errors that would accumulate linearly in structured circuits are randomised into incoherent contributions that accumulate as √m; (3) Twirled noise channels are compatible with the Pauli noise models assumed by RB and PEC. For actual noise reduction, twirling must be combined with ZNE or PEC.</p>
</div>

**Answer 13:**

<div class="box box-equation">
<p>For a 5-qubit system, the full readout calibration matrix is 2⁵ × 2⁵ = 32 × 32. It requires 2⁵ = 32 calibration experiments — one for each of the 32 computational basis states |00000⟩, |00001⟩, ..., |11111⟩. Each experiment prepares a specific basis state and measures the full 5-qubit output distribution. With ~1000 shots per experiment: 32 × 1000 = 32,000 total shots. At 1000 shots/second on IBM cloud: ~32 seconds — entirely manageable. For comparison: n=10: 2¹⁰ = 1024 experiments, ~1024 seconds (~17 minutes, still feasible). n=20: 2²⁰ = 1,048,576 experiments, ~12 days — impractical. n=5 is one of the largest systems where the full calibration matrix is routinely feasible, motivating M3 for any n > 15.</p>
</div>

**Answer 14:**

<div class="box box-equation">
<p>The primary advantage of Qiskit's Estimator primitive is that it provides a unified, hardware-agnostic interface for computing expectation values ⟨ψ|O|ψ⟩ with automatic error mitigation, operator decomposition, and result post-processing — without requiring users to manually implement these steps. Key advantages: (1) Automatic error mitigation — setting resilience_level=2 automatically applies ZNE + readout mitigation without writing folding code, calibration circuits, or Richardson extrapolation manually; (2) Automatic operator decomposition — the Estimator accepts SparsePauliOp operators and automatically decomposes them into executable Pauli measurement circuits; (3) Hardware portability — the same code runs unchanged on simulators (for development/testing) and real hardware (for production) by changing only the backend parameter; (4) Qiskit Runtime parallelisation — circuits are automatically batched and parallelised for efficient cloud execution; (5) Reproducibility — error mitigation parameters and methods are standardised across experiments.</p>
</div>

**Answer 15:**

<div class="box box-equation">
<p>Error mitigation is most useful in the NISQ era (approximately 2024–2028): (1) Gate fidelities of 99–99.9% are achievable, so noise is a systematic perturbation correctible by mitigation rather than completely overwhelming the computation; (2) Physical qubit counts (50–10,000) are insufficient for QEC overhead (~10–1,000 physical qubits per logical qubit); (3) Shallow circuits (<1000 gates) keep mitigation overhead manageable; (4) Specific tasks requiring expectation values (VQE, QAOA, quantum chemistry) benefit directly from reduced bias. In the full fault-tolerant era (2033+), active quantum error correction will suppress errors to below 10⁻¹⁵ per logical gate — orders of magnitude below any remaining systematic bias that error mitigation could address. The overhead of error mitigation (~5–10× more circuits) would then be wasted effort when the logical error rate is already negligible. Error mitigation is thus a transitional technology for the NISQ-to-fault-tolerant transition.</p>
</div>

## References and Further Reading

1. Nielsen, M. A. & Chuang, I. L. (2000). Quantum Computation and Quantum Information. Cambridge University Press. [Kraus operators, quantum channels]

2. Emerson, J., Alicki, R., & Zyczkowski, K. (2005). Scalable Noise Estimation with Random Unitary Operators. Journal of Optics B, 7(10). [Randomised benchmarking]

3. Magesan, E., Gambetta, J. M., & Emerson, J. (2011). Scalable and Robust Randomized Benchmarking of Quantum Processes. Physical Review Letters, 106(18). [RB protocol formalism]

4. Merkel, S. T. et al. (2013). Self-Consistent Quantum Process Tomography. Physical Review A, 87(6). [Gate set tomography]

5. Cross, A. W., Magesan, E., Bishop, L. S., Smolin, J. A., & Gambetta, J. M. (2016). Scalable Randomised Benchmarking of Non-Clifford Gates. npj Quantum Information, 2. [Cross-entropy and non-Clifford RB]

6. Temme, K., Bravyi, S., & Gambetta, J. M. (2017). Error Mitigation for Short-Depth Quantum Circuits. Physical Review Letters, 119(18). [Zero-noise extrapolation, probabilistic error cancellation]

7. van den Berg, E., Minev, Z. K., Kandala, A., & Temme, K. (2023). Probabilistic Error Cancellation with Sparse Pauli-Lindblad Models on Noisy Quantum Processors. Nature Physics, 19. [PEC at scale]

8. Kandala, A. et al. (2019). Error Mitigation Extends the Computational Reach of a Noisy Quantum Processor. Nature, 567. [ZNE demonstrated on hardware]

9. IBM Quantum (2024). Qiskit Runtime Estimator Primitive and Error Suppression/Mitigation Documentation. https://docs.quantum.ibm.com [Readout mitigation, Pauli twirling in practice]

10. Preskill, J. (2018). Quantum Computing in the NISQ Era and Beyond. Quantum, 2, 79. [NISQ-era noise context]

<figure class="book-figure">
<img src="content/images/image39.png" alt="">
<figcaption></figcaption>
</figure>

## Solved Examples — Chapter 4

<div class="box box-example">
<p class="box-title"><strong>Example 4.1 ZNE Richardson Extrapolation (Linear)</strong></p>
<p>Problem: ZNE with gate folding gives E(λ=1) = 0.62 and E(λ=3) = 0.38. Apply Richardson linear extrapolation.</p>
<p>Solution: Richardson coefficients for λ₁=1, λ₂=3:</p>
<p>γ₁ = λ₂/(λ₂ - λ₁) = 3/(3 - 1) = 3/2 = 1.5</p>
<p>γ₂ = - λ₁/(λ₂ - λ₁) = - 1/(3 - 1) = - 1/2 = - 0.5</p>
<p>E_mit = γ₁·E(1) + γ₂·E(3) = 1.5×0.62 + (−0.5)×0.38 = 0.930 − 0.190 = 0.740</p>
<p>Interpretation: the unmitigated value of 0.62 is improved to 0.74 by eliminating the leading-order linear noise term.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.2 PEC Sampling Overhead for a 200-Gate Circuit</strong></p>
<p>Problem: A 200-gate circuit has average 2Q gate error p = 0.2%. (a) Compute γ and sampling overhead. (b) How many PEC shots vs unmitigated shots for accuracy ε = 1%?</p>
<p>Solution: γ = (1+2p)^n = (1+0.004)^200 = (1.004)^200 ≈ e^(0.004×200) = e^0.8 ≈ 2.226.</p>
<p>Sampling overhead = γ² = (2.226)² = 4.95 ≈ 5× more shots needed for PEC.</p>
<p>(b) Unmitigated shots for ε = 1%: N = 1/ε² = 10,000 shots.</p>
<p>PEC shots: N_PEC = γ² × N = 5 × 10,000 = 50,000 shots.</p>
<p>Feasibility: 50,000 shots at ~100 shots/second on IBM cloud = ~500 seconds (~8 minutes). Practical!</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.3 Pauli Twirling Decomposition</strong></p>
<p>Problem: A noisy CNOT gate has noise channel E = (1−p)I + p·(ZI). Is this a Pauli channel? If not, write the twirled version.</p>
<p>Solution: E(ρ) = (1−p)ρ + p·(ZI)ρ(ZI)† — YES, this IS already a Pauli channel (ZI is a Pauli operator). Twirling a Pauli channel leaves it unchanged (Pauli channels are closed under Clifford twirling).</p>
<p>But if E = (1−p)I + p·Rz(θ) (a coherent rotation error), this is NOT a Pauli channel.</p>
<p>After 2-qubit Pauli twirling: E_twirled = (1−p_eff)I + (p_eff/4)[XI + IX + XX + ZI + IZ + ZZ + YI + IY + YY + ...]</p>
<p>The coherent rotation is converted to an equal mixture of Pauli errors with probability p_eff ≈ p. Noise magnitude preserved, noise character changed.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.4 Readout Matrix Inversion</strong></p>
<p>Problem: A single qubit has p₀₁ = 0.008, p₁₀ = 0.015. A circuit gives P_measured = [0.63, 0.37]. Find the corrected probabilities.</p>
<p>Solution: A = [[1−0.008, 0.015], [0.008, 1−0.015]] = [[0.992, 0.015], [0.008, 0.985]]</p>
<p>det(A) = 0.992×0.985 − 0.015×0.008 = 0.9771 − 0.00012 = 0.97698</p>
<p>A⁻¹ = (1/0.97698)×[[0.985, −0.015], [−0.008, 0.992]] = [[1.0082, −0.01536], [−0.00819, 1.0154]]</p>
<p>P_corrected = A⁻¹ × [0.63, 0.37]ᵀ = [1.0082×0.63 − 0.01536×0.37, −0.00819×0.63 + 1.0154×0.37]</p>
<p>= [0.6352 − 0.00568, −0.00516 + 0.3757] = [0.6295, 0.3706]</p>
<p>Note: P₀ + P₁ = 0.6295 + 0.3706 = 1.0001 ≈ 1.000 ✓ (rounding). Correction is small but important for precision.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.5 ZNE Quadratic Extrapolation with Three Noise Levels</strong></p>
<p>Problem: ZNE gives E(λ=1) = 0.720, E(λ=2) = 0.650, E(λ=3) = 0.580. Apply quadratic Richardson extrapolation.</p>
<p>Solution: Three noise factors, quadratic extrapolation cancels a₁λ and a₂λ² terms.</p>
<p>Richardson coefficients: γ₁ = (λ₂λ₃)/((λ₂−λ₁)(λ₃−λ₁)) = (2×3)/((1)(2)) = 3</p>
<p>γ₂ = (λ₁λ₃)/((λ₁−λ₂)(λ₃−λ₂)) = (1×3)/((−1)(1)) = −3</p>
<p>γ₃ = (λ₁λ₂)/((λ₁−λ₃)(λ₂−λ₃)) = (1×2)/((−2)(−1)) = 1</p>
<p>E_mit = 3×0.720 − 3×0.650 + 1×0.580 = 2.160 − 1.950 + 0.580 = 0.790</p>
<p>Compare linear (λ=1,3): E_mit_lin = 1.5×0.720 − 0.5×0.580 = 1.080 − 0.290 = 0.790 (same here by coincidence).</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.6 M3 vs Full Calibration Matrix Comparison</strong></p>
<p>Problem: A 20-qubit circuit produces 500 distinct bitstrings with significant probability. Compare M3 vs full calibration matrix: (a) calibration circuit count, (b) linear system size.</p>
<p>Solution: Full calibration matrix:</p>
<p>(a) Calibration circuits: 2²⁰ = 1,048,576. At 100 shots each: ~10⁸ shots — completely impractical.</p>
<p>(b) Linear system: 2²⁰ × 2²⁰ = 10⁶ × 10⁶ matrix — requires ~10¹² bytes = 1 TB RAM.</p>
<p>M3 (sparse, 500 bitstrings):</p>
<p>(a) Calibration circuits: ~500 × n_bits = 500 × 20 = 10,000 circuits. At 100 shots: 10⁶ shots — feasible in <3 hours.</p>
<p>(b) Linear system: 500 × 500 = 250,000 elements — trivially invertible on a laptop.</p>
<p>M3 reduces calibration cost by ~100,000× for 20 qubits.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.7 Qiskit Estimator Overhead Calculation</strong></p>
<p>Problem: A VQE computation requires 10,000 shots per Pauli string, with 200 Pauli strings in the Hamiltonian. Compare total shots for resilience levels 0, 1, and 2.</p>
<p>Solution: Total Pauli measurements = 200 Pauli strings.</p>
<p>Level 0: 200 × 10,000 = 2,000,000 shots. No extra circuits.</p>
<p>Level 1: 2,000,000 (main) + 40 calibration circuits × 10,000 shots = 2,400,000 shots. 1.2× overhead.</p>
<p>Level 2: ZNE = 3 noise levels × 2,000,000 = 6,000,000 shots (main circuits folded)</p>
<p>+ 40 calibration circuits × 10,000 = 400,000 shots.</p>
<p>Total = 6,400,000 shots. 3.2× overhead vs Level 0.</p>
<p>At 1000 shots/second on IBM cloud: Level 0 = 2000 s; Level 2 = 6400 s ≈ 107 minutes. Acceptable for VQE.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 4.8 Combined ZNE + Twirling Advantage</strong></p>
<p>Problem: A 50-gate circuit has coherent error ε_coh = 0.005 and incoherent error ε_incoh = 0.003 per gate. (a) What EPC does RB measure? (b) After twirling and ZNE, what bias remains?</p>
<p>Solution: (a) RB measures: EPC = incoherent error + (fraction of coherent error that "looks" incoherent to RB).</p>
<p>Coherent errors from random Clifford averaging appear as effective depolarising: EPC_coh ≈ ε_coh/2 (approximate).</p>
<p>EPC_RB ≈ ε_incoh + ε_coh/2 = 0.003 + 0.0025 = 0.0055 per gate.</p>
<p>(b) After Pauli twirling: coherent error converts to incoherent Pauli noise of magnitude ~ε_coh = 0.005.</p>
<p>Total effective noise after twirling: ε_total = 0.003 + 0.005 = 0.008 per gate.</p>
<p>After linear ZNE (2 noise factors, λ=1,3): residual bias ~ a₂λ² term ≈ (ε_total)² × (quadratic coefficient).</p>
<p>For 50 gates: residual bias ≈ n × ε²_total = 50 × 0.000064 = 0.0032 (much better than uncorrected 0.4 total error).</p>
</div>

## Multiple Choice Questions — Chapter 4

*Instructions: Select the single best answer.*

**16. Richardson linear extrapolation with noise factors λ₁=1, λ₂=3 gives the mitigated value as:**

(A) (3/2)·E(1) − (1/2)·E(3)

(B) (1/2)·E(1) + (1/2)·E(3)

(C) 2·E(1) − E(3)

(D) (1/2)·E(3) − (3/2)·E(1)

**17. The PEC sampling overhead for a circuit with one-norm γ is:**

(A) γ (linear in γ)

(B) γ² (quadratic in γ)

(C) γⁿ (exponential in qubit count)

(D) 1/γ (overhead decreases with γ)

**18. Pauli Twirling converts arbitrary noise into:**

(A) Zero noise (fully error-corrected circuit)

(B) Amplitude-damping channel

(C) A stochastic Pauli noise channel

(D) A unitary rotation error

**19. The calibration matrix A for a single qubit satisfies:**

(A) P_ideal = A · P_measured

(B) P_measured = A · P_ideal

(C) P_measured = A⁻¹ · P_ideal

(D) P_ideal = P_measured (A is always identity)

**20. Qiskit Estimator resilience level 2 applies which combination of mitigation methods?**

(A) PEC + GST

(B) Pauli Twirling only

(C) ZNE (gate folding + Richardson extrapolation) + M3 readout mitigation

(D) Full quantum error correction

**21. ZNE fails or becomes unreliable when:**

(A) The circuit depth is less than 5 gates

(B) The total circuit error n·p >> 1 (noise dominates over signal)

(C) The qubit count is less than 10

(D) The observable O commutes with the Hamiltonian

**22. The PEC one-norm γ for a circuit of n gates each with error rate p scales as:**

(A) 1 + 2np (linear in depth)

(B) (1+2p)ⁿ ≈ e^(2np) (exponential in depth)

(C) p^n (decreasing with depth)

(D) √n·p (random walk scaling)

**23. For a 5-qubit system, the full calibration matrix method requires how many calibration experiments?**

(A) 5 (one per qubit)

(B) 10 (pairwise)

(C) 32 = 2⁵ (one per computational basis state)

(D) 1024 = 2¹⁰ (one per 2-qubit pair state)

**24. M3 (Matrix-free Measurement Mitigation) achieves scalability by:**

(A) Ignoring readout errors for large n

(B) Calibrating only the bitstrings that appear with significant probability in the circuit output (sparse calibration)

(C) Using tensor network compression of the confusion matrix

(D) Assuming all qubits have identical readout errors

**25. Pauli Twirling does NOT:**

(A) Convert coherent errors to Pauli noise

(B) Reduce the total noise magnitude (average gate infidelity)

(C) Randomise the error direction on the Bloch sphere

(D) Make the noise compatible with RB and Pauli noise models

**26. Gate folding G → G·G†·G applies how much additional noise compared to a single gate G?**

(A) 2× more noise

(B) 3× more noise (three gate applications)

(C) 1× (same as single gate, since G†G = I cancels)

(D) √3× more noise

**27. The linear XEB fidelity from a perfect quantum computer executing a random circuit is:**

(A) 0

(B) 0.5

(C) 1

(D) 2ⁿ − 1

**28. Which Qiskit Estimator resilience level applies no error mitigation?**

(A) Level −1

(B) Level 0

(C) Level 1

(D) There is no zero-mitigation level

**29. The main advantage of ZNE over PEC for practical NISQ computations is:**

(A) ZNE provides exponentially better noise reduction

(B) ZNE does not require prior knowledge of the noise model

(C) ZNE has lower sampling overhead for all circuit depths

(D) ZNE is compatible with fault-tolerant error correction

**30. The total error mitigation circuit overhead for ZNE with 3 noise factors {λ=1, 2, 3} compared to the unmitigated circuit is approximately:**

(A) 1× (no overhead)

(B) 2× (one extra circuit)

(C) 3× (three circuit executions)

(D) 6× (including calibration overhead)

<div class="box box-generic">
<p>Q16: A | Q17: B | Q18: C | Q19: B | Q20: C | Q21: B | Q22: B | Q23: C | Q24: B | Q25: B | Q26: B | Q27: C | Q28: B | Q29: B | Q30: C</p>
</div>

## Unsolved Problems — Chapter 4

4.1 In a BB84 QKD experiment, 10⁶ qubits are transmitted. (a) After basis sifting (~50% discarded), how many sifted bits remain? (b) If 10% are used for QBER estimation and QBER = 4%, calculate H₂(0.04). (c) Calculate the maximum secure key length after error correction and privacy amplification.

<div class="box box-equation">
<p><em>[Ans: (a) ~500,000; (b) H₂(0.04) = 0.266; (c) r = 1−2×0.266 = 0.468; secure key = 0.468×450,000 = 210,600 bits]</em></p>
</div>

4.2 For an E91 experiment with Werner states ρ = F·|Ψ⁻⟩⟨Ψ⁻| + (1−F)·I/4, show that S(F) = −2√2·F. Find the minimum visibility F needed to violate |S| > 2.

<div class="box box-equation">
<p><em>[Ans: S(F) = −2√2·F (linearity of E(a,b) in F); F_min = 2/(2√2) = 1/√2 = 0.707; QBER at this F = (1−0.707)/2 = 14.6%]</em></p>
</div>

4.3 A QKD system uses WCP with μ = 0.4 at 1550 nm through 100 km fibre (α = 0.2 dB/km). Calculate: (a) transmission η; (b) P(0), P(1), P(2) by Poisson; (c) gain Q_μ assuming detection efficiency 0.8.

<div class="box box-equation">
<p><em>[Ans: (a) η = 10⁻² = 0.01; (b) P(0)=0.670, P(1)=0.268, P(2)=0.054; (c) Q_μ = μ·η·η_det = 0.4×0.01×0.8 = 3.2×10⁻³ bits/pulse]</em></p>
</div>

4.4 The PLOB bound for direct QKD is R ≤ −log₂(1−η). For TF-QKD with R_TF = √η·η_det, find the distance at which TF-QKD achieves 10⁴× the key rate of standard BB84, given α = 0.2 dB/km, η_det = 0.8.

<div class="box box-equation">
<p><em>[Ans: R_TF/R_BB84 = √η/η = 1/√η = 10⁴; √η = 10⁻⁴; η = 10⁻⁸; L = −10/(0.2)×log₁₀(10⁻⁸) = 400 km. At 400 km, TF-QKD gives 10,000× higher key rate than BB84.]</em></p>
</div>

4.5 A Kyber-768 key exchange is performed. (a) Total key exchange data (public key + ciphertext) in bytes. (b) Compare with ECDH P-256 (64+32 bytes). (c) Additional bandwidth at 1000 connections/s.

<div class="box box-equation">
<p><em>[Ans: (a) 1184+1088=2272 bytes; (b) ECDH total=96 bytes; ratio=23.7×; (c) additional BW=(2272−96)×1000×8=17.4 Mbps — easily handled by modern Gbps links]</em></p>
</div>

4.6 In superdense coding, Alice applies Z to her qubit of the |Φ⁺⟩ pair. (a) What message does she encode? (b) Show the resulting state is |Φ⁻⟩. (c) Describe Bob's Bell measurement circuit to recover the message.

<div class="box box-equation">
<p><em>[Ans: (a) Message '10'; (b) Z_A|Φ⁺⟩=(I⊗Z)(|00⟩+|11⟩)/√2=(|00⟩−|11⟩)/√2=|Φ⁻⟩; (c) CNOT then H on q₁, measure both; outcome |10⟩ → recovers '10']</em></p>
</div>

4.7 A QRNG produces bits with bias P('1')=0.51. (a) Min-entropy per bit. (b) Output P('1') after Von Neumann extraction. (c) Extraction efficiency (output bits per input bit pair).

<div class="box box-equation">
<p><em>[Ans: (a) H_∞=−log₂(0.51)=0.972 bits/bit; (b) P(01)=P(10)=0.49×0.51=0.2499; output '1' when input='01': P_out('1')=0.5 (perfectly unbiased); (c) efficiency=2×0.2499=0.4998 bits per 2 input bits=0.25]</em></p>
</div>

4.8 A 400 km quantum repeater chain has 4 elementary links of 100 km each. F_Bell=0.97 per link. (a) F_teleport per segment. (b) End-to-end fidelity after 3 entanglement swappings. (c) Compare with F_direct=0.97⁴.

<div class="box box-equation">
<p><em>[Ans: (a) F=(2×0.97+1)/3=0.980; (b) F_end=0.980³=0.941; (c) F_direct=0.885; repeater advantage=5.6 pp — critical for quantum networking]</em></p>
</div>

4.9 An organisation encrypts data with RSA-2048 and has a 25-year secrecy requirement. CRQCs are expected by 2035. From what year must PQC migration be complete? Which NIST algorithm for key exchange?

<div class="box box-equation">
<p><em>[Ans: Data encrypted in year X is at risk if X+25>2035, i.e., X>2010. All RSA data from 2010 onward is at HNDL risk. Migration should be completed immediately for all new data. Use Kyber-768 (ML-KEM, FIPS 203) for key exchange — NIST Level 3, fastest performance.]</em></p>
</div>

4.10 In UBQC, the client sends qubits |+_θ⟩ with θ chosen uniformly from {0°, 45°, 90°, 135°, 180°, 225°, 270°, 315°}. (a) Show the server sees the maximally mixed state regardless of θ. (b) If the server correctly guesses θ, what probability does it have of determining the computation angle φ?

<div class="box box-equation">
<p><em>[Ans: (a) ρ=(1/8)·Σ_k|+_{kπ/4}⟩⟨+_{kπ/4}|=I/2 (maximally mixed) — server has zero information about θ; (b) Even knowing θ, the random bit r gives δ=φ+θ+r·π with two equally probable values of φ; P(correct φ)=1/2 — perfect computational privacy]</em></p>
</div>

## Theory Questions — Chapter 4

- 1. Prove that the BB84 protocol is secure against the intercept-resend attack for QBER < 11%. Start from the information-theoretic argument that Eve's information on the final key must be negligible after privacy amplification. Derive the Devetak-Winter secure key rate r ≥ 1 − 2H₂(QBER) and the 11% threshold.
  2. Describe the E91 protocol in detail. Derive the CHSH inequality |S| ≤ 2 for a classical local hidden variable model. Show that ideal |Ψ⁻⟩ Bell pairs violate this with S = −2√2. Explain why Bell violation serves as a device-independent security certificate.
  3. Explain the photon number splitting (PNS) attack on BB84 with weak coherent pulses. Show mathematically how decoy states allow Alice and Bob to bound Eve's information per photon number class, restoring the security of WCP-based BB84.
  4. Derive the PLOB bound on the secret key capacity of a lossy bosonic channel. Explain how Twin-Field QKD achieves O(√η) scaling, apparently violating this bound, and resolve the paradox (TF-QKD uses an intermediate relay, changing the channel model).
  5. Explain the Module Learning With Errors (Module-LWE) problem underlying CRYSTALS-Kyber. Describe key generation, encapsulation, and decapsulation at a high level. Why is the error vector e essential for security? What happens to security if e = 0?
  6. Compare CRYSTALS-Dilithium, FALCON, and SPHINCS+ as post-quantum digital signature schemes in terms of: (a) hardness assumption; (b) signature and key sizes; (c) signing/verification speed; (d) implementation security concerns. Give a specific application scenario where each would be preferred.
  7. Prove the Holevo bound I(A:B) ≤ χ using joint entropy and monotonicity of relative entropy. Show that superdense coding achieves the Holevo bound exactly for 1 qubit transmitted with a pre-shared Bell pair.
  8. Describe the UBQC (Universal Blind Quantum Computation) protocol in detail. Explain why the server learns nothing about: (a) the client's input, (b) the algorithm, and (c) the output. What quantum capabilities must the client have? What are current experimental limitations?
  9. Analyse India's NQM quantum communication goals. Describe the current state of QKD in India (IIT Delhi–DRDO link, C-DOT systems), the 2000 km national network milestones, and challenges of deploying QKD over India's telecom infrastructure. Compare with China's Beijing–Shanghai QKD backbone.
  10. The 'harvest now, decrypt later' threat implies data encrypted today with RSA or ECDH must be assumed compromised once CRQCs arrive. Analyse the migration urgency for: (a) government communications with 30-year secrecy; (b) financial transactions with 7-year secrecy; (c) consumer TLS with 1-day secrecy. Recommend NIST PQC algorithms and migration timelines for each.

## Assignments — Chapter 4

### Assignment 4.1 BB84 QKD Simulation (Marks: 10)

Implement a complete BB84 QKD simulation in Python: (a) Simulate transmission of 10⁵ qubits. Implement basis sifting, QBER estimation, and compute secure key rate for QBER values from 0% to 15% in steps of 1%. Plot secure key rate vs QBER and mark the 11% threshold. (b) Add an intercept-resend eavesdropper intercepting fraction f = 0.2 of qubits. Calculate theoretical QBER and compare with simulation. (c) Implement privacy amplification using SHA-256 hashing. Report the final secure key length.

### Assignment 4.2 Post-Quantum Cryptography Benchmarking (Marks: 10)

Using the liboqs library (pip install oqs): (a) Benchmark Kyber-512/768/1024 vs X25519 (ECDH): measure key generation, encapsulation, and decapsulation times (1000 operations). (b) Compare RSA-2048 vs CRYSTALS-Dilithium signing and verification speeds. (c) For a TLS 1.3-like handshake, calculate total data transfer for (Kyber-768 + Dilithium) vs (X25519 + RSA-2048) and the time overhead.

### Project Suggestion 4.A Full BB84 Simulation with Decoy States

Implement a comprehensive BB84 simulation including: (a) WCP source with decoy states (μ=0.5, ν=0.1); (b) PNS attack on multi-photon pulses; (c) Single-photon detection with efficiency 0.8 and dark count rate 10⁻⁷; (d) Decoy-state security analysis; (e) Cascade error reconciliation; (f) Privacy amplification via Toeplitz hashing. Plot secure key rate vs distance (10–200 km). Submit all code and a 3000-word analysis.

### Project Suggestion 4.B Post-Quantum TLS Analysis

Analyse the migration from classical to post-quantum TLS: (a) Using OQS-OpenSSL (open-quantum-safe.github.io), perform TLS 1.3 handshakes with Kyber-768 + Dilithium3 and measure latency; (b) Implement a hybrid handshake (classical + PQC simultaneously) as recommended by NIST for transition; (c) Report handshake times, data sizes, and deployment considerations for 10,000 simultaneous connections.

### Project Suggestion 4.C QRNG Statistical Testing

Build and test a QRNG: (a) Simulate a beam-splitter QRNG generating 10⁷ bits with slight bias P(1)=0.5001; (b) Apply Von Neumann debiasing; (c) Run NIST Statistical Test Suite (nist.gov/sts) on the bitstream — report which tests pass and at what significance level; (d) Apply AES-based conditioning (NIST SP 800-90B) and compare statistical properties before and after.
