# CHAPTER 3

# Noise Sources and Noise Characterisation

*Unit 2 · Kraus Operators · Pauli Channels · Randomised Benchmarking · Gate Set Tomography · XEB · Quantum Process Tomography*

<div class="box box-learning-objectives">
<p class="box-title"><strong>📋 Learning Objectives — Chapter 3</strong></p>
<p>(1) Describe the physical origin of each major noise source — gate errors (coherent/incoherent), SPAM errors, crosstalk, ZZ coupling, leakage, T₁/T₂ decoherence.</p>
<p>(2) Write any quantum noise process in Kraus operator representation; verify CPTP conditions.</p>
<p>(3) Identify depolarising, amplitude-damping, and dephasing channels from their Kraus operators.</p>
<p>(4) Compute the Pauli Transfer Matrix (PTM) for single-qubit channels and interpret diagonal elements as error rates.</p>
<p>(5) Design, execute, and analyse a Standard Clifford Randomised Benchmarking (RB) experiment; derive EPC and relate to average gate fidelity.</p>
<p>(6) Explain Interleaved RB and derive the gate-specific error rate formula EPC(G) = (1−r_G/r_ref)×(d−1)/d.</p>
<p>(7) Explain Gate Set Tomography: gate set formalism, germ amplification, SPAM-free nature, gauge ambiguity.</p>
<p>(8) Define linear XEB fidelity F_XEB and critically evaluate its role in quantum supremacy claims.</p>
<p>(9) Perform Quantum Process Tomography on a single-qubit gate; reconstruct the chi matrix.</p>
</div>

## 3.1 Introduction: The NISQ Era and the Noise Problem

Every physical qubit interacts with its environment. Every gate operation is imperfect. Every measurement introduces some probability of misclassifying the qubit state. These imperfections — collectively called quantum noise — are the central engineering challenge of the NISQ era, a term coined by John Preskill in 2018 to describe quantum processors with 50–1000 qubits, no quantum error correction, and gate fidelities high enough to run interesting but not yet fault-tolerant computations.

Understanding quantum noise is not merely academic. Every practical quantum algorithm — from the Variational Quantum Eigensolver (VQE) to the Quantum Approximate Optimisation Algorithm (QAOA) — produces results corrupted by noise. Without rigorous understanding of where noise comes from, how large it is, and how to mitigate its effects, one cannot distinguish a genuine quantum result from a noise artefact. This chapter provides that understanding.

Quantum noise is fundamentally different from classical noise. In classical computers, a bit flip is a discrete, irreversible event. In quantum systems, noise is a continuous process described by quantum channels — linear, completely positive, trace-preserving (CPTP) maps that act on the density matrix. The same noise channel that causes |0⟩ to decay to |1⟩ also dephases the superposition |+⟩ = (|0⟩+|1⟩)/√2, converting pure quantum states into mixed states and destroying the interference that gives quantum computers their power.

<div class="box box-anecdote">
<p class="box-title"><strong>📜 The NISQ Term — John Preskill, 2018</strong></p>
<p>"Noisy Intermediate-Scale Quantum" was coined in Preskill's 2018 paper "Quantum Computing in the NISQ Era and Beyond" (Quantum 2, 79). Preskill argued that NISQ devices, despite limitations, would be the first to demonstrate quantum advantage for specific practical problems. Google's quantum supremacy claim (Arute et al., Nature 2019) and IBM's quantum utility result (Kim et al., Nature 2023) confirmed that NISQ-era devices can challenge or exceed classical simulation for specific tasks. "NISQ is a label for the current era, not a strategy for the future" — the long-term goal remains full fault-tolerant quantum computing.</p>
</div>

## 3.2 Noise Sources in NISQ Quantum Hardware

### 3.2.1 The Quantum Channel Formalism: Kraus Operators and Pauli Channels

**Density Matrix and Open Quantum Systems**

A pure quantum state |ψ⟩ is a complete description of an isolated quantum system. Real qubits interact with their environment. The density matrix ρ — a positive semidefinite, trace-1 Hermitian matrix — can describe both pure states (ρ = |ψ⟩⟨ψ|, Tr(ρ²) = 1) and mixed states (Tr(ρ²) < 1). For a single qubit:

<div class="box box-equation">
<p><strong>ρ = (I + r⃗·σ⃗)/2 where r⃗ = (⟨X⟩, ⟨Y⟩, ⟨Z⟩), |r⃗| ≤ 1</strong> <em>[Density matrix Bloch vector form]</em></p>
</div>

The Bloch vector r⃗ = (Tr(ρX), Tr(ρY), Tr(ρZ)) has |r⃗| = 1 for pure states and |r⃗| < 1 for mixed states. The components: rx = Re(ρ₀₁), ry = −Im(ρ₀₁) (off-diagonal coherences), rz = ρ₀₀ − ρ₁₁ (population difference). Noise processes shrink the Bloch vector toward the origin (maximally mixed state ρ = I/2).

**Kraus Operators and CPTP Maps**

A quantum channel E is any physically allowable evolution of a quantum state. It must be completely positive (CP) — maps positive operators to positive operators, even when applied to a subsystem — and trace-preserving (TP) — preserves Tr(ρ) = 1. Every CPTP map E has a Kraus representation:

<div class="box box-equation">
<p><strong>E(ρ) = Σₖ Kₖ ρ Kₖ† with Σₖ Kₖ†Kₖ = I (completeness / TP condition)</strong> <em>[Kraus operator representation]</em></p>
</div>

The operators {Kₖ} are Kraus operators. For a single qubit, at most 4 Kraus operators are needed. The Kraus representation is not unique — any unitary mixing of Kraus operators gives an equivalent channel. Each Kₖ represents one possible "quantum jump" — a particular environmental interaction. The term KₖρKₖ† is the (un-normalised) state of the qubit after jump k; Σₖ averages over all possible jumps. The completeness condition Σₖ Kₖ†Kₖ = I ensures probability conservation.

<div class="box box-key-concept">
<p class="box-title"><strong>🔑 Three Fundamental Noise Channels</strong></p>
<p>Three quantum channels appear throughout quantum computing. (1) DEPOLARISING: E_dep(ρ) = (1−p)ρ + (p/3)(XρX + YρY + ZρZ). Kraus operators: K₀=√(1−p)I, K₁=√(p/3)X, K₂=√(p/3)Y, K₃=√(p/3)Z. Bloch vector: r⃗ → (1−4p/3)r⃗. The most symmetric channel — uniform shrinkage in all directions. (2) AMPLITUDE-DAMPING: K₀=[[1,0],[0,√(1−γ)]], K₁=[[0,√γ],[0,0]] where γ=1−e^(−t/T₁). Models T₁ relaxation — asymmetric (always drives toward |0⟩). Bloch: rz → (1−γ)rz + (γ→ground), rx,ry → √(1−γ)·rx,ry. (3) DEPHASING: E_deph(ρ) = (1−pφ)ρ + pφZρZ†. K₀=√(1−pφ)I, K₁=√pφZ. Models pure dephasing (T_φ processes). Bloch: rz→rz (unchanged), rx,ry→(1−2pφ)rx,ry. Destroys off-diagonal coherences without changing populations.</p>
</div>

**The Pauli Transfer Matrix (PTM)**

The Pauli Transfer Matrix (PTM) is a 4×4 real matrix T representing how a quantum channel transforms Pauli operators:

<div class="box box-equation">
<p><strong>Tᵢⱼ = (1/2)·Tr(Pᵢ·E(Pⱼ)) where P₀=I, P₁=X, P₂=Y, P₃=Z</strong> <em>[Pauli Transfer Matrix definition]</em></p>
</div>

For the depolarising channel: T_dep = diag(1, 1−4p/3, 1−4p/3, 1−4p/3). Diagonal elements give the decay factor for each Pauli component; off-diagonal elements indicate coherent errors or axis misalignments. The PTM framework is central to Gate Set Tomography (Section 3.4), where all gate PTMs are simultaneously characterised from experimental data.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image27.png" alt="">
<figcaption></figcaption>
</figure>
<p><em>Taxonomy of Noise Sources in NISQ Quantum Hardware</em></p>
<p><em><strong>Figure 3.1:</strong> Hierarchical taxonomy of noise sources in a superconducting NISQ processor. Six major categories are shown: (1) Gate Errors — subdivided into coherent (systematic miscalibration: pulse amplitude, frequency, phase errors; accumulate linearly with gate number m) and incoherent (stochastic: TLS fluctuators, thermal noise, shot-to-shot jitter; accumulate as √m random walk); (2) SPAM Errors — state preparation errors from thermal excitation (~1% at 20 mK) and measurement errors from limited dispersive shift resolution (~0.5–1%); (3) Crosstalk and ZZ Coupling — always-on residual qubit-qubit coupling causing conditional phase errors during idle periods, scaling with qubit density; (4) Leakage — population of non-computational states (|2⟩, |3⟩...) due to finite transmon anharmonicity and fast gate pulses; (5) Decoherence — T₁ relaxation (spontaneous emission via Purcell effect, TLS coupling) and T₂ dephasing (1/f flux noise, charge noise); (6) Crosstalk in readout — qubit-qubit dispersive coupling during simultaneous readout. Each noise source is labelled with its typical magnitude on IBM transmon hardware (2024).</em></p>
</div>

### 3.2.2 Gate Errors: Coherent and Incoherent

Gate errors are deviations of the implemented quantum operation from the ideal unitary. They fall into two fundamentally different classes with different physical origins, experimental signatures, and mitigation strategies.

Coherent errors arise from systematic miscalibrations: if the intended gate is Rx(π) but the implemented gate is Rx(π+ε), the error angle ε is perfectly reproducible. Coherent errors accumulate linearly: after m gates, total rotation error is m×ε. They appear as oscillatory signals in repeated-gate experiments (not simple exponential decay). They can in principle be identified and corrected by recalibration.

Incoherent (stochastic) errors arise from random fluctuations — thermal noise, shot-to-shot control jitter, coupling to fluctuating TLS defects. These push the qubit randomly in different Bloch sphere directions each shot. They accumulate as a random walk: error magnitude ∝ √m. Incoherent errors are modelled by depolarising or amplitude-damping channels and cannot be corrected by recalibration alone.

<div class="box box-key-concept">
<p class="box-title"><strong>🔑 Coherent vs Incoherent: Key Distinction</strong></p>
<p>Coherent errors and incoherent errors have different experimental signatures. A coherent error Rx(ε) on a qubit produces a population that oscillates sinusoidally with m (because m×ε eventually reaches π and reverses). An incoherent (depolarising) error produces a population that decays exponentially with m toward 1/2, without oscillations. In Randomised Benchmarking, coherent errors are converted to effectively incoherent contributions by the randomisation — making RB report an effective error rate that can underestimate coherent error damage in structured (non-random) circuits. This is an important limitation of RB for characterising coherent errors.</p>
</div>

### 3.2.3 SPAM Errors: State Preparation and Measurement

SPAM (State Preparation and Measurement) errors are systematic errors in preparing the initial qubit state and measuring the final state. State preparation errors: at operating temperature T ~ 20 mK, the thermal excitation probability P(|1⟩_thermal) = 1/(exp(ℏω₀₁/kBT) + 1) ~ exp(−ℏω₀₁/kBT) ~ 10⁻⁵ for ω₀₁/(2π) = 5 GHz at 20 mK — negligible. However, residual photons in the control lines can excite qubits, contributing ~0.3–1% state preparation error. Active reset protocols (feedback π-pulse conditioned on readout outcome) reduce preparation errors below 0.1%.

Measurement errors: the principal error is misclassification — reading |1⟩ when the qubit is in |0⟩ (p₀₁) or reading |0⟩ when in |1⟩ (p₁₀). These arise from: (a) T₁ relaxation during the readout pulse (the excited state decays before the readout signal integrates); (b) limited signal-to-noise ratio from the dispersive shift χ being comparable to the resonator linewidth κ; (c) thermal excitation of the resonator. Modern IBM systems achieve p₀₁ ~ p₁₀ ~ 0.5–1%, with JPA-equipped systems reaching <0.3%. SPAM errors are the dominant challenge for protocols that require many shots of high-fidelity state discrimination.

### 3.2.4 Crosstalk, ZZ Coupling, and Addressability Errors

Crosstalk refers to unwanted interactions between qubits that are intended to be idle or independently addressed. In superconducting circuits, the dominant crosstalk mechanism is ZZ coupling — an always-on interaction that shifts the frequency of qubit i conditioned on the state of qubit j:

<div class="box box-equation">
<p><strong>H_ZZ = (ξ_ij/2)·Zᵢ⊗Zⱼ where ξ_ij ~ 10 kHz – 1 MHz (always-on, even when gates are off)</strong> <em>[ZZ coupling Hamiltonian]</em></p>
</div>

ZZ crosstalk causes uncontrolled conditional phase accumulation during circuit execution: if qubit j is in |1⟩ while qubit i idles for time t, qubit i accumulates a phase of ξ_ij·t/ℏ relative to the |0⟩ state. For ξ_ij/(2π) = 50 kHz and t = 1 μs: accumulated phase = 0.05×2π = 18° — a significant systematic error for deep circuits. IBM Heron's tunable coupler architecture reduces ξ_ij to below 5 kHz during idle periods, dramatically improving crosstalk performance.

### 3.2.5 Leakage Beyond the Computational Subspace

Leakage is the population of non-computational states outside the qubit subspace {|0⟩, |1⟩}. For transmon qubits, this means the |2⟩, |3⟩, ... levels of the anharmonic oscillator. Leakage is particularly insidious because: (a) it cannot be detected by standard qubit measurements (which only distinguish |0⟩ from |1⟩ without resolving higher levels); (b) it can spread to neighbouring qubits through the crosstalk mechanism; (c) it violates the qubit model assumed by randomised benchmarking, biasing the EPC estimate; (d) quantum error correction codes cannot correct leakage without specific leakage-reduction protocols (LRU — Leakage Reduction Units). DRAG pulse shaping (Section 1.4.2) is the primary mechanism for leakage suppression, and additional "leakage randomised benchmarking" protocols measure the leakage rate separately.

### 3.2.6 Decoherence: T₁, T₂, and T₂*

Decoherence is the process by which a qubit loses its quantum character through interaction with its environment, converting pure states into classical mixtures. Two characteristic timescales govern decoherence:

- T₁ (longitudinal relaxation / energy relaxation time): starting in |1⟩, P₁(t) = e^(−t/T₁). Physical mechanisms: spontaneous emission via the Purcell effect (coupling to the readout resonator provides a decay channel: Γ_Purcell = g²κ/(Δ²), controlled by resonator design), coupling to TLS defects in junction oxide (random telegraph signals), and quasiparticle tunnelling. Typical values: T₁ ~ 100–500 μs for IBM transmon qubits (2024).
- T₂ (transverse coherence time / total dephasing): starting in |+⟩ = (|0⟩+|1⟩)/√2, |ρ₀₁(t)| = e^(−t/T₂). Bounded by: 1/T₂ = 1/(2T₁) + 1/Tφ, where Tφ is the pure dephasing time. Physical mechanisms for Tφ: 1/f flux noise (universal in SC circuits), charge noise (suppressed but not eliminated in transmon), and low-frequency TLS fluctuations. Typical: T₂ ~ 50–300 μs.
- T₂* (free-induction-decay time): measured in a Ramsey experiment (two π/2 pulses with free precession). T₂* ≤ T₂ because inhomogeneous broadening (qubit frequency drift shot-to-shot) adds additional dephasing. T₂* typically 50–80% of T₂. Spin-echo sequences (Hahn echo) refocus low-frequency noise, recovering T₂ from T₂*.

<div class="box box-equation">
<p>1/T₂ = 1/(2T₁) + 1/Tφ (always: T₂ ≤ 2T₁) <em>[T₂ constraint]</em></p>
</div>

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image28.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Bloch Sphere T₁/T₂ Decay and Exponential Decay Curves</strong></p>
<p><em><strong>Figure 3.2:</strong> Left: Bloch sphere representation showing T₁ relaxation (red arrow: Bloch vector component rz decaying from +1 toward ground state −1, exponential timescale T₁) and T₂ dephasing (blue arrow: equatorial components rx, ry shrinking toward zero, timescale T₂). Pure dephasing from 1/f flux noise shrinks only rx, ry without affecting rz. Right: Exponential decay curves. Gold: T₁ measurement — P₁(t) = e^(−t/T₁), population of excited state after inversion by π-pulse; fitted to extract T₁. Teal: T₂ measurement — |ρ₀₁(t)| = e^(−t/T₂), off-diagonal coherence measured via Ramsey oscillations (fringe contrast vs delay); fitted to extract T₂. The constraint T₂ ≤ 2T₁ is always satisfied; in practice T₂ < 2T₁ because pure dephasing from 1/f noise contributes additionally (1/T₂ = 1/(2T₁) + 1/Tφ).</em></p>
</div>

## 3.3 Randomised Benchmarking (RB)

### 3.3.1 Standard Clifford RB: Protocol, Theory, and Fitting

<div class="box box-anecdote">
<p class="box-title"><strong>📜 RB Origins — Emerson 2005; Knill 2008</strong></p>
<p>Randomised Benchmarking was introduced by Emerson, Alicki, and Życzkowski in 2005 as a technique for characterising average gate error without SPAM-error contamination. The modern form with Clifford group and exponential decay theory was developed by Knill et al. (2008). RB has since become the universal standard for reporting qubit quality — IBM, Google, IonQ, and every major quantum computing company report gate fidelities using RB or its variants. The key insight: random sequences of Clifford gates create a known exponential decay in the survival probability that is independent of SPAM errors.</p>
</div>

Randomised Benchmarking (RB) is the most widely used experimental method for characterising the average error rate of quantum gates. Its key advantage over quantum process tomography is SPAM-robustness: RB is insensitive to state preparation and measurement errors, which often contribute ~1% error and would otherwise dominate the characterisation.

The Clifford group (single-qubit): a finite group of 24 unitary operations generated by {H, S, X}. It has the key property that a random sequence of Clifford gates acts like a random rotation on the qubit. The Clifford group maps Pauli operators to Pauli operators under conjugation — it is the normaliser of the Pauli group. This property makes the decay of the RB survival probability exactly exponential and independent of specific gate errors (to first order).

Standard Clifford RB protocol — four steps:

1. Choose a random sequence of m Clifford gates C₁, C₂, ..., Cₘ uniformly from the 24-element single-qubit Clifford group.
2. Compute the inversion gate Cₘ₊₁ = (Cₘ·...·C₁)⁻¹ (this is always also a Clifford gate — Clifford group is closed under inversion).
3. Apply the sequence C₁, C₂, ..., Cₘ, Cₘ₊₁ to the qubit initialised in |0⟩. Measure the survival probability P(|0⟩). If all gates were perfect, the qubit would return exactly to |0⟩ after Cₘ₊₁. Any deviation reflects gate errors.
4. Repeat for many (30–100) random sequences at each sequence length m. Average P_surv(m) over random sequences. Repeat for multiple values of m (typically m = 1, 5, 10, 20, 50, 100, 200, ...). Fit the averaged data to extract p.

<div class="box box-equation">
<p>P_surv(m) = A · pᵐ + B <em>[RB decay formula]</em></p>
</div>

The SPAM-dependent constants A and B are determined by fitting — they absorb state preparation and measurement errors. The depolarising parameter p = 1 − (d−1)ε/d (d = 2 for single qubit) is the key physical quantity, uncontaminated by SPAM. From p:

<div class="box box-equation">
<p><strong>EPC = (d−1)(1−p)/d = (1−p)/2 [single qubit, d = 2]</strong> <em>[Error Per Clifford from RB]</em></p>
</div>

The average gate fidelity is F_avg = 1 − EPC. For IBM's Heron processor: EPC ~ 0.1% per Clifford, corresponding to F_avg = 99.9%. The EPC measured by RB is a gate-averaged infidelity — it correctly weights the most common gates (which in random Clifford circuits appear roughly equally). Note: EPC is in units of error per Clifford; since a typical Clifford is implemented as ~1.5 native gates, the per-native-gate error is EPC/1.5.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image29.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Randomised Benchmarking Decay Curve and EPC Extraction</strong></p>
<p><em><strong>Figure 3.3:</strong> Left: RB experiment data — survival probability P_surv vs sequence length m for 50 random sequences at each m (individual sequences shown as light blue dots; their mean as dark blue squares). Error bars show the standard error of the mean across sequences. The exponential fit A·pᵐ + B (red curve) is plotted over the data; p = 0.999 corresponds to EPC = (1−0.999)/2 = 0.05%. Centre: Residuals plot showing the fit quality — residuals randomly distributed around zero, confirming the exponential model is adequate and no coherent error oscillations are present. Right: EPC extraction workflow — from fitted p to EPC = (1−p)/2, and conversion to average gate fidelity F_avg = 1 − EPC = 99.95%. The 95% confidence interval on EPC from the fitting is shown, typically ±0.02% for good RB statistics (50 sequences × 1000 shots per circuit).</em></p>
</div>

### 3.3.2 Interleaved Randomised Benchmarking (IRB)

Standard RB gives the average error per Clifford gate — an average over all 24 single-qubit Clifford operations. To characterise the error rate of a specific gate G (e.g., the Hadamard, T gate, or CX gate), Interleaved RB (IRB) is used. The IRB protocol interleaves the gate G between every random Clifford gate in the sequence: C₁, G, C₂, G, C₃, G, ..., Cₘ, G, C_inv.

The interleaved survival probability decays with rate p_G (slower than the reference rate p_ref due to the additional error from G). The gate-specific EPC is:

<div class="box box-equation">
<p><strong>EPC(G) = (d−1)/d · (1 − p_G/p_ref)</strong> <em>[Interleaved RB gate error formula]</em></p>
</div>

The ratio p_G/p_ref divides out the contribution of the random Clifford gates, leaving only the error introduced by G itself. Example: if p_ref = 0.999 (from standard RB) and p_G = 0.997 (from IRB with H interleaved), then EPC(H) = (1/2)(1 − 0.997/0.999) = (1/2)(0.002) = 0.001 → 0.1% error per Hadamard gate.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image30.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Interleaved RB — Separating Gate-Specific Error from Reference</strong></p>
<p><em><strong>Figure 3.4:</strong> Left: Two RB decay curves plotted on the same axes: standard Clifford RB (blue, decay rate p_ref) and Interleaved RB with the Hadamard gate (orange, decay rate p_H). The IRB curve decays faster because H contributes additional error per step. Centre: EPC comparison table — standard Clifford RB gives the average Clifford error; IRB gives the specific H error via EPC(H) = (d−1)/d × (1 − p_H/p_ref). Right: IRB workflow for multiple target gates (X, H, S, CX, CZ) showing how each requires separate IRB experiments to extract gate-specific error rates. A colour-coded processor topology map shows the gate error heatmap (higher = more red) for the CX gate across all 144 coupling edges of IBM Eagle, revealing spatial variation in gate quality across the processor.</em></p>
</div>

### 3.3.3 Two-Qubit and Simultaneous RB

Standard RB extends naturally to two-qubit gate characterisation using the two-qubit Clifford group (11,520 elements for 2 qubits). Two-qubit RB characterises the average error of the two-qubit gate set including CNOT, CZ, and single-qubit gates simultaneously. The RB formula is the same, but EPC now refers to error per two-qubit Clifford. The average two-qubit fidelity extracted from 2Q-RB is reported as the headline gate fidelity by IBM and Google (e.g., "99.5% CNOT fidelity" from Eagle).

Simultaneous RB (SimRB) runs multiple single-qubit RB sequences simultaneously on different qubits of a processor, measuring whether gate errors increase when multiple qubits are driven simultaneously (indicative of crosstalk). If EPC from SimRB significantly exceeds EPC from individual RB, crosstalk is present. IBM routinely reports SimRB results as part of the daily processor calibration to detect qubit-qubit crosstalk changes.

## 3.4 Gate Set Tomography (GST)

Gate Set Tomography (GST) is a comprehensive self-consistent tomographic protocol that simultaneously characterises a complete set of gate operations, state preparations, and measurements — without assuming any of them are ideal. This "SPAM-free" property makes GST uniquely powerful for identifying the precise nature of errors in a quantum gate set.

In GST, the user specifies a "gate set" consisting of: (1) a fiducial state ρ₀ (state preparation, treated as unknown); (2) a fiducial measurement {E₀, I−E₀} (POVM, treated as unknown); (3) a set of "target" gates {Gᵢ} (each treated as approximately known but with unknown errors). The experiment applies sequences of the form ρ₀ → (preparation fiducial) → (germ sequence repeated k times) → (measurement fiducial) → E₀.

"Germ" sequences are carefully chosen short gate sequences (typically 1–4 gates) that amplify specific error types when repeated k times. The key insight: repeating a germ k times amplifies errors linearly in k, providing high sensitivity with manageable overhead. The data is analysed by maximum-likelihood estimation, returning PTM estimates for all gates simultaneously with formally rigorous error bars. The standard software is PyGSTi (Sandia National Laboratories, open-source).

<div class="box box-key-concept">
<p class="box-title"><strong>🔑 Gauge Freedom in GST</strong></p>
<p>Gauge freedom is a subtlety unique to GST. The same set of observable outcome probabilities can be produced by multiple different gate set descriptions, related by an invertible transformation T (the "gauge transformation"): {ρ → T⁻¹ρ, E → ET, Gᵢ → TGᵢT⁻¹}. The observable Tr(E·Gₘ·...·G₁·ρ) = Tr(ET·TGₘT⁻¹·...·TG₁T⁻¹·T⁻¹ρ) is identical — gauge changes are unobservable. To report a unique gate set estimate, gauge optimisation chooses T to make the estimated gate set as close as possible to the target (ideal) gate set, typically by minimising the Frobenius-norm distance. Gauge-invariant quantities (like trace distances between gate PTMs) can be reported without fixing a gauge.</p>
</div>

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image31.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Gate Set Tomography — Germ Table and Gauge Orbit Visualisation</strong></p>
<p><em><strong>Figure 3.5:</strong> Left: GST germ table for a single-qubit gate set {Gx, Gy, Gi} (X/2 rotation, Y/2 rotation, identity). Each row is a germ — a short gate sequence that amplifies a specific error mode when repeated k times. The germ repetitions (k = 1, 2, 4, 8, 16, 32, 64) are shown in columns. The full experiment is all fiducial-preparation combinations × all germs × all repetitions × all fiducial-measurement combinations. For this gate set: ~448 distinct circuits, each repeated 1000 times. Centre: Maximum-likelihood estimate of gate error — process matrix (PTM) heatmap for the Gx gate, showing both ideal (diagonal) and estimated (off-diagonal error terms) contributions. Colour-coded deviations from identity reveal coherent errors (rotation axis tilt) and stochastic errors (diagonal decay). Right: Gauge orbit visualisation — the ellipsoidal constraint surface in gate-set parameter space, showing that only the gauge-optimised estimate (red star) is reported while the full gauge orbit (blue torus) represents all equivalent physically indistinguishable descriptions.</em></p>
</div>

## 3.5 Cross-Entropy Benchmarking (XEB)

Cross-Entropy Benchmarking (XEB) is the method used by Google to characterise the overall fidelity of multi-qubit random circuit instances, and is the basis of Google's quantum supremacy claim in 2019. Unlike RB (which characterises individual gate errors), XEB characterises the overall output distribution fidelity of random quantum circuits.

Protocol: run N random n-qubit circuits with uniformly random single-qubit rotations and two-qubit entangling gates. For each circuit C, classically compute the ideal output probabilities p(x) = |⟨x|U_C|0⟩ⁿ|² for all 2ⁿ bitstrings x. Measure the actual output bitstrings from the hardware. Compute the linear XEB fidelity:

<div class="box box-equation">
<p><strong>F_XEB = 2ⁿ · ⟨p(xᵢ)⟩_measured − 1</strong> <em>[Linear XEB fidelity]</em></p>
</div>

where the average is over all measured output bitstrings xᵢ. Physical interpretation: for a perfect quantum computer, F_XEB = 1 (hardware faithfully samples from the ideal distribution). For a fully depolarised circuit (completely random output), F_XEB = 0 (measured bitstrings are uncorrelated with ideal probabilities). For Google's Sycamore at ~53 qubits and 20 cycle depth: F_XEB ≈ 0.002 — very small, but this demonstrates that the hardware output has genuine quantum structure that classical simulation cannot efficiently reproduce at this circuit size.

<div class="box box-warning">
<p class="box-title"><strong>⚠ XEB Controversy and Limitations</strong></p>
<p>Google's quantum supremacy claim based on XEB has been contested. The claimed classical simulation time (10,000 years on Summit supercomputer) was disputed when Alibaba and USTC researchers showed improved classical simulation algorithms reducing the estimate dramatically. Pan and Zhang (2022) claimed classical simulation in ~15 hours. The debate is ongoing and healthy — it drives both better quantum hardware and better classical algorithms. XEB measures a specific sampling task, not general computation. True "application-relevant" quantum advantage is a higher bar.</p>
</div>

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image32.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>XEB Fidelity vs Circuit Depth — Google Sycamore Quantum Supremacy Data</strong></p>
<p><em><strong>Figure 3.6:</strong> Left: Linear XEB fidelity F_XEB vs number of circuit cycles (depth) for Google's 53-qubit Sycamore processor. Three curves shown: model prediction (gold dashed, accounting for known gate error rates); experimental data (blue dots with error bars, from ~10⁶ circuit executions per depth); and classical simulation benchmark (red, showing where classical simulation becomes prohibitively slow at current hardware). F_XEB decreases exponentially with depth (F_XEB ≈ (1−ε_avg)^n_gates) but remains statistically significant even at 20 cycles where classical simulation is claimed to require ~10,000 years. Centre: Porter-Thomas distribution of output probabilities p(x) — ideal quantum circuit outputs follow a Porter-Thomas (exponential) distribution over bitstrings, while classical random outputs are uniform. The measured distribution (histogram) matches the Porter-Thomas prediction, confirming quantum character. Right: Extrapolation of classical simulation cost vs circuit depth, showing the crossover point (red star) where quantum runtime surpasses classical runtime on Summit supercomputer.</em></p>
</div>

## 3.6 Quantum Process Tomography (QPT)

Quantum Process Tomography (QPT) is the most complete but resource-intensive method for characterising a quantum gate. QPT reconstructs the full quantum process map — the chi matrix χ — which describes the gate's action on any input state including all possible error modes. While RB provides a scalar error rate and GST provides PTMs, QPT provides the full 4ⁿ×4ⁿ complex process matrix for n-qubit gates.

QPT protocol for a single-qubit gate G:

5. Prepare four linearly independent input states {ρ₁=|0⟩⟨0|, ρ₂=|1⟩⟨1|, ρ₃=|+⟩⟨+|, ρ₄=|+i⟩⟨+i|}. These span the space of single-qubit density matrices.
6. Apply the unknown gate G each input state: σⱼ = G(ρⱼ) = E(ρⱼ).
7. Perform full quantum state tomography (QST) on each output σⱼ by measuring ⟨X⟩, ⟨Y⟩, ⟨Z⟩ (3 measurements × 4 inputs = 12 measurement settings). This reconstructs the Bloch vector for each output state.
8. Reconstruct the chi matrix χ using linear inversion or maximum-likelihood estimation. The chi matrix satisfies E(ρ) = Σₘₙ χₘₙ PₘρPₙ† where {Pₘ} = {I,X,Y,Z}. Process fidelity with the ideal gate U: F_process = Tr(χ_ideal · χ_exp).

<div class="box box-equation">
<p><strong>F_process = Tr(χ_ideal · χ_exp) [process fidelity with ideal gate U]</strong> <em>[QPT process fidelity]</em></p>
</div>

Scaling: QPT requires 4ⁿ input states × 3ⁿ measurement settings = 12ⁿ total measurement circuits. For 1 qubit: 12 circuits. For 2 qubits: 144 circuits. For 3 qubits: 1728 circuits. For n qubits: exponential — QPT is practical only for 1–2 qubit gates.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image33.png" alt="">
<figcaption></figcaption>
</figure>
<p><strong>Quantum Process Tomography — Chi Matrix Reconstruction</strong></p>
<p><em><strong>Figure 3.7:</strong> Quantum Process Tomography (QPT) for a Hadamard gate. Left: QPT experimental workflow — four input states (|0⟩, |1⟩, |+⟩, |+i⟩) are prepared; each passes through the gate H; the output is characterised by quantum state tomography (measuring X, Y, Z expectation values). Right: Chi matrix heatmap visualisation (real part, top; imaginary part, bottom). For an ideal Hadamard gate, only the I·X + X·I + I·Z − Z·I terms are nonzero (in the Pauli basis). Experimental chi matrix shows the ideal contributions (bright squares) plus small off-diagonal elements representing coherent and incoherent errors. Process fidelity F_process = Tr(χ_ideal·χ_exp) = 99.2% for this example. The chi matrix diagonal elements represent which Pauli errors occur, enabling identification of dominant error mechanisms beyond the scalar EPC from RB.</em></p>
</div>

<div class="box box-roadmap">
<p class="box-title"><strong>RECAP</strong></p>
<p><em>Chapter 3: Noise Sources and Noise Characterisation — Short Answer Questions & Model Answers</em></p>
</div>

## Short Answer Questions — Chapter 3

*Instructions: Answer each question in 3–6 lines.*

**Q1.** Define the density matrix ρ for a single qubit. Write it in Bloch vector form.

*[§3.2.1 — Density Matrix]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q2.** What is a CPTP map? State the Kraus completeness condition.

*[§3.2.1 — CPTP Maps]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q3.** Write the Kraus operators for the amplitude-damping channel. What physical process does it model?

*[§3.2.1 — Amplitude Damping]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q4.** Write the single-qubit depolarising channel E_dep(ρ). How does it act on the Bloch vector?

*[§3.2.1 — Depolarising Channel]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q5.** Distinguish between coherent and incoherent gate errors. How does each scale with gate number m?

*[§3.2.2 — Gate Errors]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q6.** Define T₁, T₂, and Tφ. Write the relation between them and state which bound is always satisfied.

*[§3.2.6 — Decoherence Times]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q7.** Describe the Standard Clifford RB protocol in four steps.

*[§3.3.1 — RB Protocol]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q8.** Write the RB survival probability formula P_surv(m) and define the Error Per Clifford (EPC).

*[§3.3.1 — RB Decay Formula]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q9.** What is Interleaved RB? Write the gate-specific EPC formula.

*[§3.3.2 — Interleaved RB]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q10.** Why is Standard Clifford RB considered SPAM-robust?

*[§3.3.1 — SPAM Robustness]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q11.** What is Gate Set Tomography and how does it differ from standard process tomography?

*[§3.4 — GST]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q12.** Define gauge freedom in GST and explain how gauge optimisation resolves it.

*[§3.4 — Gauge Freedom]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q13.** Write the linear XEB fidelity formula. What do F_XEB = 0 and F_XEB = 1 mean physically?

*[§3.5 — XEB Fidelity]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q14.** Describe the QPT protocol for a single-qubit gate in three stages.

*[§3.6 — QPT Protocol]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q15.** Why does QPT scale exponentially with qubit number? How many circuits does a 2-qubit QPT require?

*[§3.6 — QPT Scaling]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

## Model Answers — Chapter 3

**Answer 1:**

<div class="box box-equation">
<p>The density matrix ρ is a 2×2 positive semidefinite Hermitian matrix with Tr(ρ) = 1, providing the most general description of a qubit including both pure and mixed states. In Bloch vector form: ρ = (I + r⃗·σ⃗)/2, where r⃗ = (rₓ, rᵧ, rᵤ) = (⟨X⟩, ⟨Y⟩, ⟨Z⟩) = (Tr(ρX), Tr(ρY), Tr(ρZ)) is the Bloch vector with |r⃗| ≤ 1. The surface of the Bloch sphere (|r⃗| = 1) corresponds to pure states (Tr(ρ²) = 1); the interior (|r⃗| < 1) to mixed states (Tr(ρ²) < 1); the origin (r⃗ = 0) to the maximally mixed state ρ = I/2 (maximum uncertainty). Components: ρ₀₀ = (1+rᵤ)/2 (ground state population), ρ₁₁ = (1−rᵤ)/2, ρ₀₁ = (rₓ−irᵧ)/2 (coherence).</p>
</div>

**Answer 2:**

<div class="box box-equation">
<p>A CPTP (Completely Positive Trace-Preserving) map is the most general physically allowed quantum evolution. "Completely positive" (CP) means the map sends positive semidefinite operators to positive semidefinite operators, even when applied to one part of a larger composite system. "Trace-preserving" (TP) means Tr(E(ρ)) = Tr(ρ) = 1 — probability conservation. Every CPTP map E has a Kraus operator representation: E(ρ) = Σₖ KₖρKₖ†. The Kraus completeness condition is Σₖ Kₖ†Kₖ = I, which is equivalent to the trace-preserving condition: Tr(E(ρ)) = Tr(Σₖ KₖρKₖ†) = Tr(ρ·Σₖ Kₖ†Kₖ) = Tr(ρ·I) = 1. For a single qubit, at most 4 Kraus operators are needed. Unitary operations (no noise) have a single Kraus operator K = U with U†U = I.</p>
</div>

**Answer 3:**

<div class="box box-equation">
<p>Amplitude-damping Kraus operators: K₀ = [[1,0],[0,√(1−γ)]] and K₁ = [[0,√γ],[0,0]], where γ = 1 − e^(−t/T₁) is the decay probability over time t. K₀ represents no decay occurring (the excited state persists with reduced amplitude). K₁ represents the decay event |1⟩ → |0⟩: it maps |1⟩ → √γ|0⟩, transferring population to the ground state. The channel models T₁ relaxation — the spontaneous emission or energy relaxation of the excited qubit state to the ground state, driven by coupling to environmental modes (Purcell decay through the readout resonator, TLS defects, quasiparticle tunnelling). The channel is asymmetric — |0⟩ is unaffected, |1⟩ decays. Bloch vector: rᵤ → (1−γ)rᵤ − γ (drift toward −1), rₓ,rᵧ → √(1−γ)·rₓ,rᵧ.</p>
</div>

**Answer 4:**

<div class="box box-equation">
<p>The single-qubit depolarising channel with error probability p: E_dep(ρ) = (1−p)ρ + (p/3)(XρX + YρY + ZρZ). Kraus operators: K₀ = √(1−p)·I, K₁ = √(p/3)·X, K₂ = √(p/3)·Y, K₃ = √(p/3)·Z. Action on the Bloch vector: r⃗ → (1−4p/3)·r⃗. The channel uniformly shrinks the Bloch vector by factor (1−4p/3) in all directions, preserving its direction. At p = 3/4, the shrinkage factor is 0 and the qubit is fully depolarised to ρ = I/2. The depolarising channel is the most symmetric noise model — all three Pauli errors equally likely — and is the standard model in randomised benchmarking theory. Each application reduces Bloch vector length by the same factor, regardless of the qubit state direction.</p>
</div>

**Answer 5:**

<div class="box box-equation">
<p>Coherent errors: systematic, reproducible deviations from the ideal gate. Example: Rx(π+ε) instead of Rx(π). The error angle ε is identical every shot. Accumulation: linear with gate number m — after m gates, total rotation error = m×ε. Experimental signature: oscillatory population vs m (not simple decay). Can be corrected by recalibration. Incoherent (stochastic) errors: random, irreproducible fluctuations (thermal noise, TLS fluctuators, control electronics jitter). No fixed error direction — pushes the qubit randomly. Accumulation: random walk — error magnitude ∝ √m. Modelled by depolarising or amplitude-damping channels. Cannot be corrected by recalibration. In RB, coherent errors are effectively converted to incoherent contributions by randomisation, making RB report an average error rate that may underestimate coherent error damage in structured circuits.</p>
</div>

**Answer 6:**

<div class="box box-equation">
<p>T₁ = longitudinal relaxation time (energy relaxation): timescale for excited state |1⟩ to decay to ground state |0⟩. P₁(t) = e^(−t/T₁). T₂ = transverse coherence time (total dephasing): timescale for off-diagonal density matrix element ρ₀₁ to decay. |ρ₀₁(t)| ∝ e^(−t/T₂). Tφ = pure dephasing time: additional phase randomisation from low-frequency noise (1/f flux noise, charge noise) beyond what is expected from T₁ alone. The relation: 1/T₂ = 1/(2T₁) + 1/Tφ. The always-satisfied bound: T₂ ≤ 2T₁ (because Tφ ≥ 0, so 1/T₂ ≥ 1/(2T₁)). Equality T₂ = 2T₁ holds when Tφ → ∞ (no pure dephasing). For superconducting qubits, T₁ >> Tφ is common, so 1/T₂ ≈ 1/Tφ and T₂ << 2T₁.</p>
</div>

**Answer 7:**

<div class="box box-equation">
<p>Standard Clifford RB — four steps: Step 1: Randomly sample m Clifford gates C₁,...,Cₘ uniformly from the 24-element single-qubit Clifford group (generated by H, S, X). Each Clifford maps Pauli operators to Pauli operators under conjugation. Step 2: Compute the inversion gate Cₘ₊₁ = (Cₘ·...·C₁)⁻¹ (always a Clifford gate — the Clifford group is closed under inversion). Step 3: Initialise qubit in |0⟩, apply C₁,...,Cₘ,Cₘ₊₁ in sequence, measure in Z basis. Record whether outcome is |0⟩ (success) or |1⟩ (failure). Step 4: Repeat steps 1–3 for many (30–100) random sequences of the same length m; average the success probability P_surv(m); repeat for multiple values of m; fit P_surv(m) = A·pᵐ + B to extract p, then EPC = (1−p)/2.</p>
</div>

**Answer 8:**

<div class="box box-equation">
<p>RB survival probability formula: P_surv(m) = A·pᵐ + B, where m is the sequence length (number of Clifford gates), p = 1 − (d−1)ε/d is the depolarising parameter (d=2 for single qubit), A and B are SPAM-dependent fitting constants. The Error Per Clifford (EPC), also called average gate infidelity: EPC = (d−1)(1−p)/d = (1−p)/2 for single qubits. The average gate fidelity is F_avg = 1 − EPC. Key property: A and B depend on SPAM errors but p does not — the EPC is extracted from the exponential decay rate, which is independent of the overall scale and offset of the survival curve. This SPAM-robustness is the primary advantage of RB over process tomography.</p>
</div>

**Answer 9:**

<div class="box box-equation">
<p>Interleaved RB (IRB) is a variant of standard RB that characterises the error rate of a specific gate G, rather than the average over all Clifford gates. The gate G is interleaved between every random Clifford gate in the sequence: C₁,G,C₂,G,...,Cₘ,G,C_inv. The IRB survival probability decays with rate p_G; the reference RB (without G) gives rate p_ref. Gate-specific EPC formula: EPC(G) = (d−1)/d × (1 − p_G/p_ref). The ratio p_G/p_ref divides out the contribution of the random Clifford gates, isolating the error introduced by G. Example: p_ref = 0.999, p_G = 0.997 for Hadamard → EPC(H) = (1/2)(1 − 0.997/0.999) = (1/2)(0.002) = 0.001 → 0.1% error per H gate.</p>
</div>

**Answer 10:**

<div class="box box-equation">
<p>RB is SPAM-robust because state preparation and measurement errors affect only the constants A and B in P_surv(m) = A·pᵐ + B, not the exponential decay rate p. The EPC = (1−p)/2 is extracted from p — the slope of the ln(P_surv − B) vs m plot — which depends only on how the survival probability changes with sequence length, not on its absolute value. A measurement error of probability r maps P to r·(1−P) + (1−r)·P = P + r(1−2P), which changes A and B but not the fitted slope p. This is the fundamental reason RB became the universal standard for gate fidelity reporting: it directly measures the gate error rate without contamination from imperfect state preparation or measurement equipment.</p>
</div>

**Answer 11:**

<div class="box box-equation">
<p>Gate Set Tomography (GST) simultaneously characterises all gates, state preparation (ρ₀), and measurement (POVM {E,I−E}) in a gate set — without assuming any of them are ideal. Standard QPT assumes ρ₀ and POVM are known/ideal and characterises only the gate; this means QPT results are contaminated by SPAM errors. GST uses "germ" sequences (short gate sequences that amplify specific errors when repeated k times) to achieve high sensitivity. Maximum-likelihood estimation yields PTM estimates for all gates simultaneously with rigorous error bars. GST is truly SPAM-free because both SPAM and gate errors are treated as unknowns. The trade-off: GST requires significantly more experiments (hundreds to thousands of circuits) vs tens for standard RB.</p>
</div>

**Answer 12:**

<div class="box box-equation">
<p>Gauge freedom in GST: the same observable outcome probabilities can be produced by multiple different gate set descriptions related by an invertible transformation T (gauge transformation). If {ρ₀, E, {Gᵢ}} is a gate set, then {T⁻¹ρ₀, ET, {TGᵢT⁻¹}} produces identical probabilities for all experiments, because Tr(E·Gₘ·...·G₁·ρ) = Tr(ET·TGₘT⁻¹·...·TG₁T⁻¹·T⁻¹ρ). This gauge freedom arises because we can only observe inner products (probabilities), not quantum states and gates themselves. Gauge optimisation resolves this by choosing T to make the estimated gate set as close as possible to the target (ideal) gate set, typically by minimising the Frobenius-norm distance ||G_target − TG_estT⁻¹||_F. Gauge-invariant quantities (process infidelities, trace distances) can be reported unambiguously without fixing a gauge.</p>
</div>

**Answer 13:**

<div class="box box-equation">
<p>Linear XEB fidelity: F_XEB = 2ⁿ·⟨p(xᵢ)⟩_measured − 1, where n = qubit count, p(x) = ideal (classically computed) probability of observing bitstring x, and the average is over all observed output bitstrings from the hardware. F_XEB = 1: the hardware output perfectly matches the ideal quantum distribution — perfect quantum computation. F_XEB = 0: the hardware output is completely uniform (maximally mixed) — fully depolarised circuit, no quantum structure. Physical interpretation: for a random quantum circuit, the ideal output follows a Porter-Thomas distribution p(x) ~ 2ⁿ·e^(−2ⁿp) — highly non-uniform. If the hardware faithfully samples this distribution, measured bitstrings will preferentially include high-probability bitstrings, giving ⟨p(x)⟩ > 1/2ⁿ and F_XEB > 0.</p>
</div>

**Answer 14:**

<div class="box box-equation">
<p>QPT protocol for a single-qubit gate G — three stages: Stage 1 (Input state preparation): prepare four linearly independent input density matrices {|0⟩⟨0|, |1⟩⟨1|, |+⟩⟨+|, |+i⟩⟨+i|}, which span the single-qubit density matrix space (any density matrix is a linear combination of these four). Stage 2 (Gate application): apply the unknown gate G to each input state, producing outputs σⱼ = E(ρⱼ) = G·ρⱼ·G† (including all noise processes). Stage 3 (Output state tomography): for each output σⱼ, measure expectation values of X, Y, Z (3 Pauli measurements × 4 input states = 12 measurement settings total, ~1000 shots each). Reconstruct the chi matrix χ using linear inversion or maximum-likelihood estimation. Process fidelity with ideal gate U: F_process = Tr(χ_ideal·χ_exp).</p>
</div>

**Answer 15:**

<div class="box box-equation">
<p>QPT scales exponentially because preparing a tomographically complete set of input states requires 4ⁿ linearly independent density matrices (4 per qubit), and each output state requires QST with 3ⁿ Pauli measurements (3 per qubit): total = 4ⁿ × 3ⁿ = 12ⁿ measurement settings. For 1 qubit: 12 circuits. For 2 qubits: 12² = 144 circuits (plus ~1000 shots each = 144,000 total shots). For 3 qubits: 12³ = 1728 circuits (1.7 million shots). For n = 5 qubits: 12⁵ ≈ 250,000 circuits — completely impractical. For 2-qubit QPT: specifically, 4² = 16 input state combinations × 3² = 9 Pauli measurement settings = 144 unique quantum circuits. With 1000 shots each: 144,000 total quantum circuit executions. This exponential overhead limits QPT to 1–2 qubit characterisation; RB is used for routine multi-qubit gate quality assessment.</p>
</div>

## Solved Examples — Chapter 3

<div class="box box-example">
<p class="box-title"><strong>Example 3.1 Kraus Operators: Amplitude-Damping at t = T₁/2</strong></p>
<p>Problem: Write the Kraus operators for amplitude-damping at t = T₁/2. Find ρ(t) if initial state is |+⟩.</p>
<p>Solution: At t = T₁/2: γ = 1 - e^- t/T₁ = 1 - e^- 1/2 = 0.393.</p>
<p>K₀ = 1, 0, 0, √(1 - γ) = 1, 0, 0, 0.779</p>
<p>K₁ = 0, √(γ), 0, 0 = 0, 0.627, 0, 0</p>
<p>Initial |+⟩ = (|0⟩+|1⟩)/√2, ρ₀ = [[0.5, 0.5], [0.5, 0.5]]</p>
<p>ρ(t) = K₀ρ₀K₀† + K₁ρ₀K₁† = [[0.5+0.393×0.5, 0.779×0.5], [0.779×0.5, 0.779²×0.5]] = [[0.697, 0.390], [0.390, 0.303]]</p>
<p>Bloch vector: rz = 0.697−0.303 = 0.394; rx = 2×0.390 = 0.780 (shrunk from 1). |r⃗| = √(0.780²+0.394²) = 0.874 < 1 (mixed state).</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.2 Depolarising Channel Bloch Vector Shrinkage</strong></p>
<p>Problem: The depolarising channel with p = 0.01 acts 100 times on |+⟩ = (|0⟩+|1⟩)/√2. Find the final Bloch vector.</p>
<p>Solution: Each application shrinks r⃗ → (1−4p/3)r⃗. Initial |+⟩: r⃗₀ = (1, 0, 0).</p>
<p>After 100 applications: r⃗₁₀₀ = (1−4×0.01/3)¹⁰⁰ × (1, 0, 0) = (1−0.01333)¹⁰⁰ × (1,0,0)</p>
<p>= (0.98667)¹⁰⁰ × (1,0,0) = 0.259 × (1,0,0) = (0.259, 0, 0)</p>
<p>Final state: ρ = (I + 0.259X)/2 — coherence reduced from 1 to 0.259.</p>
<p>Purity: Tr(ρ²) = (1 + 0.259²)/2 = 0.534 (significantly mixed). The qubit has not fully decohered.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.3 Randomised Benchmarking: EPC Extraction</strong></p>
<p>Problem: RB data gives P_surv(m = 1) = 0.991, P_surv(m = 20) = 0.851, P_surv(m = 100) = 0.602. Fit A ·pᵐ + B extract EPC. Assume B = 0.5.</p>
<p>Solution: With B = 0.5 (SPAM offset), fit A ·pᵐ = P_surv - 0.5:</p>
<p>m=1: A·p = 0.491; m=20: A·p²⁰ = 0.351; m=100: A·p¹⁰⁰ = 0.102.</p>
<p>m = 1 and m = 100: p¹⁰⁰/p¹ = p⁹⁹ = 0.102/0.491 = 0.208 → p = 0.208^1/99 = 0.9984.</p>
<p>EPC = (1−p)/2 = (1−0.9984)/2 = 0.0008 → 0.08% per Clifford.</p>
<p>F_avg = 1 - EPC = 99.92% per Clifford gate.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.4 Interleaved RB: Gate-Specific Error Rate</strong></p>
<p>Problem: Standard RB gives p_ref = 0.9990. Interleaved RB with Hadamard gate gives p_H = 0.9972. Find EPC(H).</p>
<p>Solution: EPC(H) = (d−1)/d × (1 − p_H/p_ref) = (1/2) × (1 − 0.9972/0.9990)</p>
<p>= (1/2) × (1 − 0.99820) = (1/2) × 0.001800 = 0.00090 → 0.09% per Hadamard gate.</p>
<p>F(H) = 1 − EPC(H) = 99.91%.</p>
<p>Compare to average Clifford: EPC_avg = (1−0.9990)/2 = 0.05%. H is slightly worse than average.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.5 Pauli Transfer Matrix for Depolarising Channel</strong></p>
<p>Problem: Write the full 4×4 PTM for the depolarising channel with error probability p = 0.02.</p>
<p>Solution: Tᵢⱼ = (1/2)Tr(Pᵢ·E_dep(Pⱼ)). Using E_dep(ρ) = (1−p)ρ + (p/3)(XρX+YρY+ZρZ):</p>
<p>T₀₀ = (1/2)Tr(I·E(I)) = (1/2)Tr(I·I) = 1 (always)</p>
<p>T₁₁ = (1/2)Tr(X·E(X)) = (1/2)Tr(X·[(1−p)X+(p/3)(X³+YXY+ZXZ)]) = (1−4p/3) = 1−4×0.02/3 = 0.9733</p>
<p>By symmetry: T₂₂ = T₃₃ = 1−4p/3 = 0.9733. All off-diagonal Tᵢⱼ (i≠j) = 0 for depolarising.</p>
<p>PTM_dep = diag(1, 0.9733, 0.9733, 0.9733).</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.6 T₁ and T₂ Calculation from Tomography</strong></p>
<p>Problem: A qubit shows P₁(t = 100μs) = 0.368 and |ρ₀₁(t = 100μs) | = 0.135 (Ramsey). Find T₁, T₂, Tφ.</p>
<p>Solution: T₁: P₁(t) = e^- t/T₁ → T₁ = - t/ln(P₁(t)) = - 100/ln(0.368) = - 100/( - 1.0) = 100 μs.</p>
<p>T₂: |ρ₀₁(t) | = e^- t/T₂ → T₂ = - 100/ln(0.135) = - 100/( - 2.0) = 50 μs.</p>
<p>1/Tφ = 1/T₂ − 1/(2T₁) = 1/50 − 1/200 = (4−1)/200 = 3/200 → Tφ = 200/3 = 66.7 μs.</p>
<p>Check: 1/T₂ = 1/50 = 0.020 μs⁻¹; 1/(2T₁) = 1/200 = 0.005 μs⁻¹; 1/Tφ = 0.015 μs⁻¹. Σ = 0.020 ✓.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.7 XEB Fidelity Interpretation</strong></p>
<p>Problem: For a 10-qubit random circuit with F_XEB = 0.78, (a) what does this imply about gate fidelity? (b) what is the expected F_XEB for 100 gates with average 2Q gate error 0.5%?</p>
<p>Solution: (a) F_XEB ≈ (1−ε)^n_2Q where n_2Q is the number of two-qubit gates. For 10 qubits and 20-cycle circuit: n_2Q ~ 80. (1−ε)⁸⁰ = 0.78 → ε = 1 − 0.78^(1/80) = 1 − 0.9970 = 0.003 → 0.3% per 2Q gate. Consistent with circuit of depth 20.</p>
<p>(b) ε = 0.005 per 2Q gate, n_2Q = 100: F_XEB = (1−0.005)¹⁰⁰ = (0.995)¹⁰⁰ = 0.606.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 3.8 QPT Process Fidelity from Chi Matrix</strong></p>
<p>Problem: QPT of an X gate gives χ_exp with χ₁₁ = 0.995 (in the Pauli basis, where χ_ideal has χ₁₁ = 1 only). Estimate the process fidelity and dominant error.</p>
<p>Solution: F_process = Tr(χ_ideal · χ_exp) = χ_exp,11 = 0.995 (since χ_ideal = δ₁₁).</p>
<p>Process fidelity = 99.5%. Dominant error: remaining 0.5% is distributed across other χ_mn elements.</p>
<p>If χ₀₀ = 0.003 (identity error = X was not applied at all in 0.3% of shots), χ₃₃ = 0.002 (ZX error = bit-flip with phase), the error is primarily coherent rotation error.</p>
<p>Average gate fidelity F_avg = (d·F_process + 1)/(d+1) = (2×0.995+1)/3 = 0.997 = 99.7% for d=2.</p>
</div>

## Multiple Choice Questions — Chapter 3

*Instructions: Select the single best answer.*

**1. The Kraus completeness condition Σₖ Kₖ†Kₖ = I ensures that the quantum channel is:**

(A) Completely positive

(B) Trace-preserving (TP)

(C) Unitary

(D) Reversible

**2. The amplitude-damping channel Kraus operators model which physical process?**

(A) Pure dephasing from 1/f flux noise

(B) T₁ relaxation (spontaneous emission, |1⟩ → |0⟩)

(C) Uniform depolarisation along all Bloch axes

(D) Phase-flip noise only

**3. The RB survival probability has the form P_surv(m) = A·pᵐ + B. The parameter B represents:**

(A) The error rate per Clifford gate

(B) The SPAM-dependent constant that absorbs measurement errors

(C) The depolarising parameter p directly

(D) The circuit depth m

**4. The Error Per Clifford (EPC) from standard single-qubit RB is:**

(A) EPC = (1−p)

(B) EPC = p

(C) EPC = (1−p)/2

(D) EPC = (1+p)/2

**5. Interleaved RB gives the error rate of a specific gate G as:**

(A) EPC(G) = (d−1)/d · (1 − p_G)

(B) EPC(G) = (d−1)/d · (1 − p_G/p_ref)

(C) EPC(G) = (d−1)/d · (p_G/p_ref)

(D) EPC(G) = 1 - p_G/p_ref

**6. The linear XEB fidelity F_XEB = 2ⁿ·⟨p(x)⟩ − 1. F_XEB = 0 means:**

(A) Perfect quantum computation

(B) Completely random output (fully depolarised circuit)

(C) Only SPAM errors present

(D) Coherent errors dominate

**7. The number of unique measurement circuits required for single-qubit QPT is:**

(A) 4

(B) 8

(C) 12

(D) 16

**8. Gauge freedom in GST arises because:**

(A) Quantum gates have random phases

(B) Observable probabilities are invariant under invertible gauge transformations T of the gate set

(C) Clifford gates form a non-abelian group

(D) Measurement outcomes are always destructive

**9. The depolarising channel acts on the Bloch vector as:**

(A) r⃗ → −r⃗ (full inversion)

(B) r⃗ → r⃗ rotated by random angle

(C) r⃗ → (1−4p/3)·r⃗ (uniform shrinkage)

(D) rz → rz, rx,ry → 0 (dephasing only)

**10. Which benchmarking method is simultaneously SPAM-robust AND provides a full gate-error PTM?**

(A) Randomised Benchmarking (RB)

(B) Gate Set Tomography (GST)

(C) Quantum Process Tomography (QPT)

(D) Cross-Entropy Benchmarking (XEB)

**11. The constraint T₂ ≤ 2T₁ arises from:**

(A) The Heisenberg uncertainty principle

(B) 1/T₂ = 1/(2T₁) + 1/Tφ, with Tφ ≥ 0

(C) The no-cloning theorem

(D) The Pauli exclusion principle

**12. ZZ crosstalk in superconducting processors causes:**

(A) Decoherence of idle qubits via spontaneous emission

(B) Conditional phase accumulation on qubit i depending on the state of qubit j, even during idle periods

(C) Leakage to the |2⟩ level of the transmon

(D) State preparation errors in the readout resonator

**13. In standard Clifford RB for single qubits, the number of elements in the Clifford group used is:**

(A) 4 (Pauli group)

(B) 16 (two-qubit Pauli group)

(C) 24 (single-qubit Clifford group)

(D) 11,520 (two-qubit Clifford group)

**14. Leakage to the |2⟩ level in a transmon qubit is primarily mitigated by:**

(A) Operating at lower temperature

(B) DRAG pulse shaping

(C) Increasing the resonator linewidth

(D) Using the Cross-Resonance gate instead of CZ

**15. The Pauli Transfer Matrix (PTM) T₀₀ element is always equal to:**

(A) 0

(B) p (the error probability)

(C) 1 (normalisation condition)

(D) Tr(ρ²) (purity)

<div class="box box-generic">
<p>Q1: B | Q2: B | Q3: B | Q4: C | Q5: B | Q6: B | Q7: C | Q8: B | Q9: C | Q10: B | Q11: B | Q12: B | Q13: C | Q14: B | Q15: C</p>
</div>

## Unsolved Problems — Chapter 3

3.1 An RB experiment gives P(m)=0.5·rᵐ+0.5 with r=0.9924. (a) Calculate EPC. (b) Calculate average physical gate error assuming 1.875 physical gates per Clifford. (c) How many Clifford gates before P drops to 0.6?

<div class="box box-equation">
<p><em>[Ans: (a) EPC=(1−0.9924)/2=0.0038=0.38%; (b) 0.38/1.875=0.203% per gate; (c) 0.6=0.5×r^m+0.5→r^m=0.2→m=ln(0.2)/ln(0.9924)=211 Cliffords≈396 physical gates]</em></p>
</div>

3.2 ZNE is applied at c=1,3,5: E₁=0.800, E₃=0.620, E₅=0.472. (a) Apply Richardson 3-point extrapolation. (b) Apply exponential fit E(c)=a+b·exp(k·c) to estimate E(0).

<div class="box box-equation">
<p><em>[Ans: (a) γ=(15/8,−5/4,3/8)=(1.875,−1.25,0.375); E(0)=1.875×0.800−1.25×0.620+0.375×0.472=1.500−0.775+0.177=0.902; (b) Exponential fit (2-parameter using c=1,5): 0.800=a+b·eᵏ, 0.472=a+b·e^{5k}; solving gives E(0)=a+b≈0.95 (approximate)]</em></p>
</div>

3.3 A single qubit has ε₀₁=1.5% and ε₁₀=3.5%. A measurement of ⟨Z⟩ gives raw result 0.680. Apply single-qubit readout correction to find ⟨Z⟩_corrected.

<div class="box box-equation">
<p><em>[Ans: A=[[0.985,0.035],[0.015,0.965]]; det=0.9508; A⁻¹≈[[1.0150,−0.0368],[−0.0158,1.0359]]; p(0)_raw=0.840, p(1)_raw=0.160; p_corr=[0.9150×0.840−0.0368×0.160, −0.0158×0.840+1.0359×0.160]=[0.855,0.145]; ⟨Z⟩_corr=0.855−0.145=0.710 (vs raw 0.680)]</em></p>
</div>

3.4 A qubit has T1=400 μs and T2=200 μs. (a) Calculate T_phi. (b) For a 2 μs circuit, find exp(−t/T2) and exp(−t/T1). (c) What fraction of 1000 runs are affected by at least one T1 event?

<div class="box box-equation">
<p><em>[Ans: (a) T_phi=400 μs; (b) exp(−2/200)=0.990 (1% dephasing), exp(−2/400)=0.995 (0.5% relaxation); (c) P(T1 event)=1−exp(−2/400)=0.5%; ~5 out of 1000 runs affected]</em></p>
</div>

3.5 Process tomography of a nominal X gate reveals PTM with R_ZZ=−0.95 (ideal: −1) and R_YY=−0.94. (a) Identify error types. (b) Calculate process fidelity F_process.

<div class="box box-equation">
<p><em>[Ans: (a) R_ZZ<1: Z-dephasing error, probability δ_Z=5%; R_YY<1: combined dephasing+amplitude damping; (b) F_process=Tr(R_ideal^T·R_actual)/4=(1+1+0.94+0.95)/4=3.89/4=97.3%]</em></p>
</div>

3.6 PEC for a CNOT with 2Q depolarising noise p=0.8%: (a) Calculate one-norm γ. (b) Total overhead for 20 CNOTs.

<div class="box box-equation">
<p><em>[Ans: (a) γ=1/(1−16p/15)=1/(1−0.00853)=1.00860; (b) γ^{40}=exp(40×0.00856)=exp(0.342)=1.408; need 41% more shots]</em></p>
</div>

3.7 A 20-qubit circuit needs readout correction. How many calibration circuits for: (i) full, (ii) tensored, (iii) M3? If each needs 1000 shots, what is total shot overhead for each?

<div class="box box-equation">
<p><em>[Ans: (i) 2^20=1,048,576 circuits (1.05 billion shots — completely impractical); (ii) 40 circuits (40,000 shots); (iii) M3 ~80 circuits (80,000 shots). Practical choice: M3 or tensored]</em></p>
</div>

3.8 A transmon has α/(2π)=−250 MHz and a Gaussian π-pulse with σ=8 ns. (a) Pulse bandwidth (FWHM). (b) Leakage risk? (c) Optimal DRAG parameter β.

<div class="box box-equation">
<p><em>[Ans: (a) Δf_FWHM=2.35/(2π×8ns)=46.7 MHz; (b) bandwidth (47 MHz) << anharmonicity (250 MHz) → leakage risk is low but DRAG reduces it 10× further; (c) β=−1/α=−1/(−250 MHz×2π)=+0.637 ns]</em></p>
</div>

3.9 Virtual distillation with M=2 copies: noisy state ρ=(1−ε)|ψ⟩⟨ψ|+ε·I/2 with ε=2%. (a) Show noise is O(ε²). (b) Noise reduction factor. (c) Shot overhead.

<div class="box box-equation">
<p><em>[Ans: (a) ⟨O⟩_{2-copy}=Tr(O·ρ²)/Tr(ρ²); ρ²=(1−ε)²|ψ⟩⟨ψ|+O(ε) mixed; leading noise term O(ε²); (b) ε/ε²=1/ε=50× reduction; (c) ~3× total overhead (2 copies + SWAP test)]</em></p>
</div>

3.10 An IBM Eagle (127 qubits) runs a 50-cycle circuit with linear XEB fidelity F_XEB=0.023. (a) Estimate circuit fidelity. (b) Solve for effective noise per cycle p_cycle. (c) Compare with RB gate errors.

<div class="box box-equation">
<p><em>[Ans: (a) F_circuit≈F_XEB=0.023; (b) (1−p_cycle)^50=0.023→p_cycle=1−0.023^{1/50}=1−exp(−0.074)=7.1% per cycle; (c) Each cycle ~63 CNOT gates at ~0.5% each: expected p_cycle≈1−(0.995)^63=0.27 — higher than measured, suggesting not all qubits active per cycle or modern gate errors are lower]</em></p>
</div>

## Theory Questions — Chapter 3

- 1. Derive the average gate fidelity formula F_avg=(d·F_process+1)/(d+1) for a d-dimensional system. Explain why the average fidelity over Haar-random inputs equals the process fidelity scaled by d/(d+1) plus a state-independent offset.
  2. Explain the Clifford group and why its properties (specifically, forming a unitary 2-design) make it ideal for randomised benchmarking. Why would RB with Haar-random unitaries instead of Cliffords be less practical?
  3. Describe the complete interleaved RB protocol. Starting from the RB decay master equation, derive the gate error rate e_G=(1−r_G/r_ref)·(d−1)/d. Under what noise model assumptions is this derivation exact, and where does it break down?
  4. Explain ZNE from first principles. Why is gate folding preferable to physical noise amplification (e.g., lengthening gate times)? Derive the Richardson extrapolation coefficients for 3-point cancellation of quadratic noise. Discuss the bias-variance trade-off in choosing extrapolation order.
  5. Derive the PEC sampling overhead. Show that for a Pauli noise channel with error p per gate, the quasi-probability one-norm γ=1/(1−4p/3) for a single qubit. Discuss how the exponential overhead γ^{2L} limits practical PEC circuit depth.
  6. Explain why Pauli twirling converts coherent errors into stochastic Pauli noise. Use the group-theoretic property of the Pauli group as a unitary 1-design. Why does this conversion change error accumulation from quadratic to linear with circuit depth?
  7. Describe the physical origins of ZZ coupling in superconducting qubit processors. Derive the rate of ZZ phase accumulation per unit time. Explain how tunable-coupler architectures (IBM Heron) suppress this coupling, and what engineering trade-offs are involved.
  8. Compare the M3 readout error mitigation approach with the full calibration matrix method. For an n-qubit device with independent readout errors, show that the tensored calibration matrix is the Kronecker product of n single-qubit matrices. Derive the condition under which tensored and full mitigation give identical results.
  9. Explain the concept of quantum utility as demonstrated by IBM Research in 2023. What was the specific physics calculation? What error mitigation techniques were used? Why was this a significant milestone, and what were the limitations and controversies?
  10. Compare ZNE, PEC, Pauli twirling, and readout correction in terms of: (a) computational overhead; (b) types of errors addressed; (c) circuit depth limitations; (d) bias vs. variance trade-offs; (e) Qiskit availability. Recommend a mitigation strategy for each of: (i) 50-qubit depth-20 VQE; (ii) 5-qubit depth-200 QPE; (iii) 100-qubit random circuit for XEB.

## Assignments — Chapter 3

### Assignment 3.1 RB on IBM Hardware (Marks: 10)

Using IBM Quantum and the Qiskit Experiments library: (a) Run standard single-qubit RB on a real IBM processor for m=1,5,10,20,50,100,200 with K=30 sequences per length. Plot P(m) vs m and fit to extract EPC and average gate error. (b) Run interleaved RB for the CNOT gate. Extract e_CNOT and compare with IBM dashboard calibration data. Report all results with error bars.

### Assignment 3.2 ZNE Implementation (Marks: 10)

Implement ZNE from scratch in Python using Qiskit: (a) Design a 3-qubit GHZ circuit and implement global gate folding at c=1,2,3,4,5. (b) Run all amplified circuits on a real IBM processor (8192 shots each). Plot E(c) vs c. (c) Apply linear, quadratic, and Richardson extrapolation to estimate E(0). Compare with ideal Qiskit Statevector simulation. Report mitigation effectiveness as (raw error − mitigated error)/raw error.

### Project Suggestion 3.A Comprehensive Noise Fingerprint

Characterise a real IBM processor comprehensively: (a) Run RB for all qubits — plot EPC heatmap over topology. (b) Measure T1/T2 for all qubits using Qiskit Experiments T1Experiment and T2Hahn. (c) Measure ZZ crosstalk for adjacent qubit pairs. (d) Calibrate readout errors using full 1-qubit calibration for all qubits. Create a 'noise fingerprint' report (2500 words) with colour-coded processor maps.

### Project Suggestion 3.B Mitigation Methods Benchmark

Systematically compare ZNE (resilience_level=2), Pauli twirling (Qiskit PauliTwirling pass), and M3 readout correction on the same circuits. Use 5 circuits of varying depth (5,10,20,50,100 gates) on a 4-qubit IBM subgraph. For each: measure (i) absolute error in ⟨ZZ⟩ vs ideal simulation; (ii) shot overhead; (iii) classical post-processing time. Write a 3000-word analysis comparing the three methods.

### Project Suggestion 3.C Error Budget Analysis

Perform a full error budget for a 4-qubit QFT circuit on IBM hardware. Decompose total circuit error into: (i) T1 relaxation; (ii) T2 dephasing; (iii) 1Q gate errors; (iv) 2Q gate errors; (v) ZZ crosstalk; (vi) SPAM errors. Measure each contribution experimentally and verify they sum to the total measured error. Discuss which source dominates and what hardware improvement would most improve fidelity.
