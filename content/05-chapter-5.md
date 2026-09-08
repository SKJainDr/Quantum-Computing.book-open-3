# UNIT 3 | CHAPTER 5

# Advanced QKD Protocols and Post-Quantum Cryptography

BB84 Security Proof  ·  E91  ·  TF-QKD  ·  MDI-QKD  ·  Satellite QKD  ·  CRYSTALS-Kyber  ·  Dilithium  ·  SPHINCS+  ·  NIST PQC

| Course |  |
|---|---|
| Chapter | Unit 3 — Chapter 5: Advanced QKD Protocols and Post-Quantum Cryptography |
| Syllabus Topics | BB84 full security proof · E91 & Bell inequality · TF-QKD · MDI-QKD · Micius satellite · CRYSTALS-Kyber (KEM) · CRYSTALS-Dilithium (signatures) · SPHINCS+ · NIST PQC standardisation 2022–2024 |
| Chapter Features | 5 figures & diagrams · 6 solved examples · RECAP (15 SAQs + model answers) · 15 MCQs · 8 unsolved problems · 8 theory questions · 3 assignments |

<div class="box box-key-concept">
<p class="box-title"><strong>📋  Learning Objectives — Chapter 5</strong></p>
<p>After completing this chapter you will be able to:</p>
<p>1. Construct the full information-theoretic security proof of BB84: no-cloning, QBER threshold, privacy amplification, and the Leftover Hash Lemma.</p>
<p>2. Explain the E91 protocol and prove that Bell inequality violation (|S| &gt; 2) certifies security against any local hidden-variable eavesdropping strategy.</p>
<p>3. Describe MDI-QKD and TF-QKD architectures, and compare key-rate scalings O(η), O(η²), and O(√η) with channel transmittance η.</p>
<p>4. Summarise the Micius satellite QKD demonstrations: key parameters, achieved secure key rates, and distance records.</p>
<p>5. Derive the Module-LWE problem and explain how CRYSTALS-Kyber (ML-KEM) encapsulation and decapsulation exploit it.</p>
<p>6. Describe the Fiat–Shamir-with-Aborts signature construction underlying CRYSTALS-Dilithium (ML-DSA).</p>
<p>7. Explain SPHINCS+ as a stateless hash-based signature scheme and evaluate its trade-offs relative to lattice schemes.</p>
<p>8. Trace the NIST PQC standardisation process (2016–2024) to the final FIPS 203/204/205/206 standards, and state key implementation requirements.</p>
</div>

## 5.1 Introduction: Beyond Prepare-and-Measure — The Security Landscape

Chapter 4 introduced the mechanics of quantum key distribution (QKD): qubits are prepared by one party (Alice), transmitted across a quantum channel, and measured by another party (Bob). Classical post-processing — basis sifting, error estimation, error correction, and privacy amplification — converts the raw measurement data into a shared secret key that both parties can use for secure symmetric encryption. This initial picture, while correct in its essential features, leaves two major questions unanswered: Why exactly is BB84 secure? And what happens to the classical computers that process and use the quantum-generated keys?

This chapter deepens the picture in two directions. First, we provide a rigorous, mathematically complete proof of BB84 security. The proof is not merely a collection of physical intuitions but a formal demonstration using the tools of quantum information theory: the no-cloning theorem, the quantum bit error rate (QBER), the binary entropy function, and the Leftover Hash Lemma. Understanding this proof is crucial because it reveals precisely how the 11% QBER threshold arises, what "security" means formally in the composable framework, and how privacy amplification converts partial eavesdropper knowledge into zero knowledge.

Second, we confront a harder and more immediate problem: even a perfect QKD system secures only the quantum channel. The classical computers that store, process, and communicate the resulting keys will soon be vulnerable to Shor's algorithm running on a cryptographically relevant quantum computer (CRQC). RSA encryption, Diffie-Hellman key exchange, and elliptic-curve cryptography — the protocols that protect virtually all internet communication today — can all be broken in polynomial time by a sufficiently powerful quantum computer. Post-quantum cryptography (PQC) is the field that addresses this threat by designing classical algorithms that are believed to be secure against quantum attacks.

Together, QKD for key establishment over quantum channels and PQC for the classical infrastructure that surrounds those channels form the two complementary pillars of a quantum-safe communication architecture. A fully quantum-safe network of the future will use QKD to distribute keys over fibre and satellite links and PQC algorithms to authenticate the classical messages that support the QKD protocol itself, to secure data in transit and at rest, and to sign software and firmware. Neither alone is sufficient; both together provide defence in depth.

<div class="box box-anecdote">
<p class="box-title"><strong>📜  Historical Context: The Road to Rigorous QKD Security</strong></p>
<p>BB84 was proposed by Charles Bennett (IBM) and Gilles Brassard (Université de Montréal) in 1984 at a coding theory conference held in Bangalore, India. For over a decade after its publication, its security rested primarily on physical intuition rather than formal mathematical proof. The community understood why it should be secure, but a rigorous proof proved elusive.</p>
<p>The breakthrough came from Dominic Mayers (1996, published in Journal of the ACM, 2001), who gave the first fully rigorous information-theoretic security proof of BB84 using a direct argument about Eve's accessible information. Independently, Lo and Chau (1999, Science) proved security by a different route: constructing a quantum error-correcting code argument that reduced BB84 to an entanglement distillation protocol.</p>
<p>The landmark simplification came from Shor and Preskill (2000, Physical Review Letters), who used the CSS quantum error-correcting code framework to reduce BB84 security to a 6-state entanglement distillation protocol. Their result yielded the clean 11% QBER threshold that is still quoted today, in a proof short enough to be taught in a graduate course.</p>
<p>The composable security framework — ensuring that a QKD key remains secure when used as a subroutine in a larger protocol (such as a one-time pad or AES-GCM) — was developed by Ben-Or, Horodecki, Leung, Mayers, and Oppenheim (2005) and Renner (PhD thesis, ETH Zurich, 2005). Composability is the accepted gold standard for QKD security proofs because it guarantees that the key is safe not just in isolation but in realistic deployments.</p>
</div>

## 5.2 BB84 Full Security Proof via Information-Theoretic Arguments

### 5.2.1 What Does "Information-Theoretic Security" Mean?

A cryptographic protocol's security can be defined in two fundamentally different ways, and understanding the distinction is essential before we can appreciate why QKD is special.

A protocol is computationally secure if breaking it requires solving a problem believed to be computationally intractable — for instance, factoring a large integer or solving the discrete logarithm problem. This definition has a critical weakness: if a sufficiently fast algorithm for these problems is discovered, or if a quantum computer is built, the security guarantee evaporates. Computational security is conditional on the state of the art in algorithm design.

A protocol is information-theoretically (IT) secure, also called unconditionally secure, if breaking it is impossible regardless of the adversary's computational resources. The adversary simply does not have enough information to violate the security guarantee, regardless of how much computation they perform. QKD targets IT security: the proof assumes only that quantum mechanics is correct — not that any mathematical problem is hard.

To make this formal, we use the composable security framework. Let ρ\_{K\_A K\_B E} denote the joint quantum state of Alice's key K\_A, Bob's key K\_B, and Eve's quantum register E after the protocol completes. Eve may have arbitrary quantum memory, can perform any quantum operation, and may have obtained all classical communication during the protocol. The protocol is called ε-secure if two conditions hold:

<div class="box box-generic">
<p class="box-title">Correctness:</p>
<p>Secrecy:</p>
<p>‖·‖₁  =  trace norm  (sum of absolute values of eigenvalues)</p>
<p>Total security parameter:</p>
<p>Typical target:</p>
</div>

The correctness condition says that Alice's and Bob's keys disagree with at most negligible probability. The secrecy condition is more subtle: it says that Alice's key is statistically indistinguishable — up to trace-norm distance ε — from a perfectly uniform random string that is completely uncorrelated with Eve's entire quantum system. This is a very strong guarantee: Eve gains essentially no information about the key, regardless of which measurement she performs on her register.

The trace norm is the right mathematical tool here because it is directly related to the probability that any statistical test can distinguish the real key from a perfectly random key. A trace-norm distance of ε means that no experiment on Eve's side can distinguish the actual key from a random one with probability more than ε better than guessing. The composable framework further guarantees that if ε ≤ 2⁻¹⁰⁰, the protocol remains secure even when used as a building block in a larger communication system, such as a one-time pad encryption or an authenticated key agreement.

### 5.2.2 The No-Cloning Theorem: The Physical Reason Eavesdropping Is Detectable

Before presenting the full four-step security proof, we must understand the fundamental physical principle that makes QKD possible. Why does eavesdropping disturb the channel? The answer lies in the quantum no-cloning theorem, independently proved by Wootters and Zurek (Nature, 1982) and Dieks (Physics Letters A, 1982).

The theorem states, informally, that it is impossible to create an exact duplicate of an unknown quantum state. More precisely:

<div class="box box-math">
<p class="box-title"><strong>▶  Theorem 5.1 — No-Cloning Theorem (Wootters–Zurek–Dieks, 1982)</strong></p>
<p>Statement: There exists no unitary operator U acting on H_qubit ⊗ H_ancilla such that</p>
<p>Proof by contradiction:</p>
<p>Assume such U exists. Apply it to two distinct states |ψ⟩ and |φ⟩:</p>
<p>Taking the inner product of both equations (U is unitary, ⟨0|0⟩ = 1):</p>
<p>Left side:</p>
<p>Right side: ⟨ψ|φ⟩ · ⟨ψ|φ⟩ = (⟨ψ|φ⟩)²</p>
<p>Therefore ⟨ψ|φ⟩ = (⟨ψ|φ⟩)², which holds only if ⟨ψ|φ⟩ = 0 or ⟨ψ|φ⟩ = 1.</p>
<p>neither condition holds, so perfect cloning is impossible. QED.</p>
</div>

In the context of BB84, Alice sends one of four states: {|0⟩, |1⟩, |+⟩, |−⟩}. The critical point is that states from different bases are non-orthogonal: ⟨0|+⟩ = 1/√2, ⟨0|−⟩ = 1/√2, ⟨1|+⟩ = 1/√2, ⟨1|−⟩ = −1/√2. By the no-cloning theorem, Eve cannot perfectly copy any of these states. Any attempt to extract information from an intercepted qubit must disturb it — if Eve measures in the wrong basis, she collapses the qubit to a random state in that basis and forwards an incorrect state to Bob, introducing a measurable error. If she measures in the correct basis, she forwards the correct state — but she cannot know which basis to use without prior knowledge of Alice's choice, which is the secret she is trying to steal. The intercept-resend attack, analysed below, formalises this dilemma.

### 5.2.3 Quantum Bit Error Rate (QBER) and the Security Threshold

The Quantum Bit Error Rate (QBER) is the single most important observable in a QKD session. It is defined as the fraction of sifted key bits — those for which Alice and Bob used matching measurement bases — on which their values disagree:

```python
QBER:   Q = (number of mismatched sifted bits) / (total number of sifted bits)

Binary entropy function:   h(Q) = −Q log₂Q − (1−Q) log₂(1−Q)

Devetak–Winter secure key rate bound (asymptotic limit, per sifted bit):

r > 0  iff  h(Q) < 1/2  iff  Q < h⁻¹(1/2) ≈ 11.0%

Numerical examples:

   Q = 0.12:  h(0.12) ≈ 0.530  →  r < 0      (abort — no key possible)
```

<figure class="book-figure">
<img src="content/images/image44.png" alt="Figure 5.1: BB84 Security — QBER Threshold and Devetak–Winter Secret Key Rate">
<figcaption>Figure 5.1: BB84 Security — QBER Threshold and Devetak–Winter Secret Key Rate</figcaption>
</figure>

In an ideal channel with no eavesdropping, the QBER is zero. In practice, photon loss, detector dark counts, optical misalignment, and environmental noise contribute a background QBER of typically 1–3% even without any eavesdropping. The security proof must account for this background noise: any excess QBER above background is attributed to Eve. The 11% threshold represents the point at which, even in the most pessimistic case — where all errors are due to Eve — she has gained so much information that no privacy amplification can produce a provably secret key.

To understand why 11% and not some other number, consider the structure of h(Q). The binary entropy function h(Q) measures the uncertainty in a single biased coin flip. At Q = 0 (no errors), h(0) = 0 — no uncertainty, no information leaked. At Q = 0.5 (random), h(0.5) = 1 — maximum uncertainty. The condition r ≥ 1 − 2h(Q) > 0 requires h(Q) < 0.5. The factor of 2 appears because two terms of h(Q) enter the bound: one for Eve's information from her quantum measurement, and one for the information leaked during error correction.

<div class="box box-key-concept">
<p class="box-title"><strong>🔑  The BB84 QBER Security Threshold — Physical Interpretation</strong></p>
<p>BB84 can generate a provably secure key if and only if Q &lt; Q_threshold ≈ 11.0%.</p>
<p>Physical interpretation of the threshold formula r = 1 − 2h(Q):</p>
<p>The "1" = total information Alice and Bob can share per sifted bit (1 bit maximum)</p>
<p>First h(Q) = information leaked to Eve via error-correction syndrome data</p>
<p>Second h(Q) = upper bound on Eve's additional information from quantum measurements</p>
<p>When 2h(Q) ≥ 1, Eve's information is at least as large as Alice and Bob's shared</p>
<p>information, and privacy amplification cannot produce a net secret key.</p>
<p>Practical operating regimes:</p>
<p>Q &lt; 3%:  excellent channel, r &gt; 70%, practical for metropolitan QKD</p>
<p>3% ≤ Q &lt; 8%:  good channel, positive key rate, suitable for long-haul QKD (&gt;100 km)</p>
<p>8% ≤ Q &lt; 11%:  marginal channel, small positive key rate, requires careful monitoring</p>
<p>Q ≥ 11%:  abort — no secure key can be generated regardless of processing.</p>
<p>Note: The intercept-resend attack on BB84 introduces Q = 25%, far above the threshold.</p>
<p>This means any attempt by Eve to intercept and resend qubits is immediately and decisively detected.</p>
</div>

### 5.2.4 The Four-Step BB84 Security Proof

With the definitions above, we can now present the complete four-step proof of BB84 security. Each step is physically motivated and mathematically rigorous. The proof follows the structure of the Shor-Preskill approach simplified for the prepare-and-measure setting.

**Step 1 — Sifting:** Alice and Bob publicly announce their basis choices (Z or X) for each transmitted qubit over the authenticated classical channel. They discard all bits where their basis choices differed — typically about 50% of the raw transmitted bits. The remaining bits form the sifted key. On average, about half the transmitted qubits survive sifting. Crucially, announcing the basis does not reveal the bit value, only which conjugate pair of states was used.

**Step 2 — Parameter Estimation:** Alice and Bob randomly select a subset (typically 10–20%) of the sifted key bits and publicly compare their values. The fraction of disagreements is the estimated QBER, denoted Q. If Q exceeds the threshold of 11%, the protocol aborts immediately — no key is generated, and Alice and Bob restart after investigating the cause. If Q < 11%, the protocol continues with the remaining sifted bits forming the raw key of length n.

**Step 3 — Information Reconciliation (Error Correction):** Even with matching bases, Alice and Bob may disagree on some bits due to channel noise. Using an authenticated classical channel, they apply an error-correcting code — such as low-density parity-check (LDPC) codes, turbo codes, or the Cascade protocol — to ensure their keys agree bit-for-bit. The error correction reveals at most nh(Q) bits of classical information about the key (equal to the Shannon entropy of the error pattern). This information is transmitted publicly and may be observed by Eve.

**Step 4 — Privacy Amplification:** After error correction, Alice and Bob hold identical n-bit strings, but Eve has partial knowledge: at most 2nh(Q) bits (from her quantum measurement plus the error correction syndrome). Privacy amplification compresses the n-bit key down to m ≈ n[1−2h(Q)] bits by applying a random universal-2 hash function (typically a random Toeplitz matrix). By the Leftover Hash Lemma, the resulting m-bit string is ε-close to a perfectly uniform random key that is completely independent of Eve's state. This is the final provably secret key.

<div class="box box-math">
<p class="box-title"><strong>▶  Leftover Hash Lemma (Bennett–Brassard–Crépeau–Maurer, 1995; Renner 2005)</strong></p>
<p>(Equivalently, Eve knows at most k bits about X.)</p>
<p>Choose a function h: {0,1}ⁿ → {0,1}^m uniformly at random from a 2-universal family.</p>
<p>Then the output h(X) satisfies:</p>
<p>Practical implementation: multiplication by a random binary Toeplitz matrix,</p>
<p>applied in O(n log n) time using the Fast Fourier Transform.</p>
<p>The Toeplitz matrix is specified by n random bits (its first row/column),</p>
<p>which Alice and Bob agree on over the authenticated channel.</p>
</div>

The beauty of this four-step proof is that it is unconditional: the security bound ε depends only on n (the sifted key length), Q (the QBER), and the security parameter, but not on Eve's computational power, her quantum memory size, or the specific attack she uses. The proof holds against all conceivable quantum adversaries, including those with arbitrary quantum computers and quantum memories. This is fundamentally different from RSA or AES security proofs, which rely on unproven hardness assumptions.

## 5.3 The E91 Protocol: Entanglement-Based QKD

### 5.3.1 Protocol Overview and Motivation

Artur Ekert's 1991 paper (Physical Review Letters, 67, 661) introduced a fundamentally different approach to quantum key distribution that has proven to be equally important — and in some ways more powerful — than BB84. Rather than having Alice prepare states and send them to Bob, the E91 protocol relies on a source that generates pairs of maximally entangled qubits — Bell pairs — and distributes one qubit from each pair to Alice and one to Bob. The source can be placed anywhere: in a trusted third-party location, at an intermediary node, or even potentially controlled by an adversary.

The paradigm shift introduced by E91 is profound: security in BB84 rests on physical assumptions about the qubit source (Alice prepares the correct states) and the no-cloning theorem. In E91, security is certified by a statistical test — the violation of a Bell inequality. If the measured correlations between Alice's and Bob's outcomes exceed the classical bound, no eavesdropper could have produced those correlations using classical pre-shared randomness, pre-programmed qubits, or any other local strategy. The Bell inequality violation is a direct certificate of genuine quantum entanglement.

### 5.3.2 Bell States and the SPDC Source

The four maximally entangled two-qubit states — the Bell states — are:

```python
|Φ⁺⟩ = (|00⟩ + |11⟩)/√2   (Phi-plus)
|Φ⁻⟩ = (|00⟩ − |11⟩)/√2   (Phi-minus)
|Ψ⁺⟩ = (|01⟩ + |10⟩)/√2   (Psi-plus)
|Ψ⁻⟩ = (|01⟩ − |10⟩)/√2   (Psi-minus)  ← used in E91
```

These four states form an orthonormal basis for the two-qubit Hilbert space C² ⊗ C². Each is maximally entangled: tracing over either qubit leaves the other in a maximally mixed state ρ = I/2, with no definite polarisation in any direction. This is the critical property that ensures security: before measurement, neither Alice's nor Bob's qubit carries any definite bit value — the values are created jointly and randomly at the moment of measurement.

In practice, Bell pairs are generated by Spontaneous Parametric Down-Conversion (SPDC) in a nonlinear crystal such as Beta Barium Borate (BBO) or Potassium Titanyl Phosphate (KTP). A pump photon at frequency 2ω enters the crystal and — with some probability — spontaneously splits into two photons each at frequency ω, entangled in polarisation. The two photons are emitted at angles determined by phase-matching conditions in the crystal. For the |Ψ⁻⟩ state used in E91, type-II SPDC with specific orientation gives photon pairs with anti-correlated polarisations.

### 5.3.3 Measurement Scheme and Key Extraction

Alice measures her qubit by passing it through a polariser set at one of three angles chosen uniformly at random: a ∈ {0°, 45°, 90°}. Bob independently chooses a measurement angle from b ∈ {45°, 90°, 135°}. For a polarisation qubit, a measurement at angle θ projects onto the states |+\_θ⟩ = cos(θ)|0⟩ + sin(θ)|1⟩ (outcome +1) and |−\_θ⟩ = −sin(θ)|0⟩ + cos(θ)|1⟩ (outcome −1).

After many measurement rounds, Alice and Bob publicly announce their basis choices (but not their outcomes). Rounds are divided into two groups:

- Same-basis rounds (a = b = 45° or a = b = 90°): For the |Ψ⁻⟩ state, outcomes are perfectly anti-correlated — when Alice gets +1, Bob gets −1, and vice versa, with probability 1. After a sign flip, these rounds provide the raw key bits. The perfect anti-correlation provides the key.

- Different-basis rounds (all other a, b combinations): These rounds are used to compute the CHSH Bell parameter S and verify that the source is genuinely entangled. These bits are sacrificed for security verification and do not contribute to the key.

<figure class="book-figure">
<img src="content/images/image45.png" alt="Figure 5.2: E91 Entanglement-Based QKD and the CHSH Bell Test">
<figcaption>Figure 5.2: E91 Entanglement-Based QKD and the CHSH Bell Test</figcaption>
</figure>

### 5.3.4 CHSH Bell Inequality as Security Certificate

The Clauser–Horne–Shimony–Holt (CHSH) inequality, derived in 1969, is the central mathematical tool of E91 security analysis. It provides an upper bound on the correlations achievable by any local hidden-variable (LHV) theory — a category that includes all classical strategies including any sophisticated eavesdropping attack that pre-programmes measurement outcomes.

Consider any classical strategy: the source distributes "hidden variables" to Alice's and Bob's qubits that determine, deterministically or probabilistically, what outcome each will produce given a particular measurement direction. Such a strategy is called a local hidden-variable model. The CHSH inequality bounds the correlations that any such model can produce:

```python
Correlation function:  E(a,b) = P(++|a,b) + P(−−|a,b) − P(+−|a,b) − P(−+|a,b)

CHSH parameter:  S = E(a₁,b₁) − E(a₁,b₂) + E(a₂,b₁) + E(a₂,b₂)

Bell–CHSH inequality (all local hidden-variable theories):  |S| ≤ 2

Tsirelson bound (maximum allowed by quantum mechanics):  |S| ≤ 2√2 ≈ 2.828
```

<div class="box box-key-concept">
<p class="box-title"><strong>🔑  Bell Inequality Violation as an Eavesdropping Detector</strong></p>
<p>The security logic of E91 is elegant and powerful:</p>
<p>|S| &gt; 2: The correlations cannot be explained by ANY local hidden-variable model.</p>
<p>Since every classical eavesdropping strategy — including quantum intercept-resend</p>
<p>attacks that introduce "effective" pre-shared randomness — is an LHV strategy,</p>
<p>a Bell violation proves the source has not been compromised.</p>
<p>The key extracted from same-basis rounds is provably secure.</p>
<p>|S| ≤ 2: Bell inequality not violated. The observed correlations could in principle</p>
<p>be explained by a classical pre-shared-randomness model. Either the source has</p>
<p>been tampered with, or the channel quality has degraded severely.</p>
<p>The protocol should abort.</p>
<p>Note:</p>
<p>violating the classical bound |S| ≤ 2 by 4 standard deviations from space!</p>
<p>This definitively confirmed long-distance quantum entanglement is achievable.</p>
</div>

### 5.3.5 BB84 vs E91: Comparison

| Feature | BB84 | E91 |
|---|---|---|
| Qubit source | Alice prepares single qubits (prepare-and-measure) | Entangled-pair source (SPDC) |
| Security basis | No-cloning theorem + QBER statistical bound | Bell inequality violation (CHSH test) |
| Device independence | No — trust hardware and state preparation | Yes — security from statistical test only |
| Bases per party | 2 (Z and X) | 3 per party, 6 total combinations |
| Key sifting yield | ~50% of raw bits | ~33% of measured rounds |
| Channel requirement | One quantum channel (Alice to Bob) | Two quantum channels (source to Alice, source to Bob) |
| Experimental range | ~100 km fibre (decoy-state) | ~1200 km free-space (Micius satellite) |
| Main vulnerability | Detector side-channels (addressed by MDI-QKD) | Source side-channels (multi-photon pairs from SPDC) |

## 5.4 Distance-Extended QKD: MDI-QKD, TF-QKD, and Satellite QKD

### 5.4.1 The Fundamental Distance Problem in Fibre QKD

Standard single-mode optical fibre at the telecom wavelength of 1550 nm has an attenuation of approximately 0.2 dB per kilometre. This means that a photon has a probability of surviving a fibre of length d kilometres equal to:

<div class="box box-generic">
<p class="box-title">Examples:</p>

</div>

Since the secret key rate in BB84 scales proportionally to η (the probability that a photon reaches Bob and produces a detection event), the key rate decreases exponentially with distance. At 300 km, the key rate is approximately 10,000 times lower than at 200 km. Without a quantum repeater — a device that can entangle, store, and teleport quantum states — the range of standard fibre QKD is practically limited to about 200 km.

This distance limitation is not merely an engineering challenge; it is a fundamental consequence of quantum mechanics. Unlike classical signals that can be amplified by measuring and re-transmitting them, quantum signals cannot be amplified without introducing noise (because amplification would require cloning, which is prohibited). Quantum repeaters, which would solve this problem using quantum memory and entanglement distillation, are an active research area but remain far from commercial deployment. In the absence of quantum repeaters, two protocol innovations — MDI-QKD and TF-QKD — extend the practical range significantly.

### 5.4.2 Measurement-Device-Independent QKD (MDI-QKD)

In 2010, Lydersen and colleagues demonstrated a devastating practical attack on commercial QKD systems: by shining bright laser light into the detector, an eavesdropper could "blind" it — making the detector responsive only to bright classical pulses rather than single photons. This allowed Eve to control Bob's measurement results without introducing any detectable QBER. Multiple commercial systems from different vendors were shown to be vulnerable.

This vulnerability motivated Lo, Curty, and Qi to propose Measurement-Device-Independent QKD (MDI-QKD) in 2012. The elegant insight was to completely remove all detectors from Alice's and Bob's stations. Instead, both parties prepare and send states; the actual measurements are performed by an untrusted central relay node C, which may be controlled by an adversary.

<div class="box box-real-world">
<p class="box-title"><strong>ℹ  MDI-QKD Protocol — Step by Step</strong></p>
<p>Setup: Alice and Bob each independently prepare BB84 states (Z or X basis, random bit)</p>
<p>and simultaneously send them to the central relay node C.</p>
<p>The relay C may be entirely malicious — Eve controls it completely.</p>
<p>Step 1: Alice prepares qubit |ψ_A⟩ in one of four BB84 states and sends to C.</p>
<p>Step 2: Bob prepares qubit |ψ_B⟩ in one of four BB84 states and sends to C.</p>
<p>Step 3: C performs a Bell State Measurement (BSM) on the two received photons,</p>
<p>using linear optics (a beamsplitter implementing Hong–Ou–Mandel interference)</p>
<p>and announces which BSM result was obtained (or failure).</p>
<p>Step 4: Alice and Bob announce their chosen bases. For matching-basis rounds where</p>
<p>C announced a specific BSM outcome, their bit values are correlated in a</p>
<p>known way. They extract key bits accordingly.</p>
<p>Step 5: Standard error correction and privacy amplification yield the final secret key.</p>
<p>Security guarantee: Even a fully malicious C cannot learn Alice's or Bob's bit values</p>
<p>from the BSM announcement alone. Any cheating attempt by C introduces detectable errors.</p>
<p>Key rate scaling: O(η_AC · η_BC) ≈ O(η²) for symmetric placement (Alice and Bob</p>
<p>equidistant from C). This is worse than BB84 by a factor of η, but eliminates</p>
<p>all detector side-channel vulnerabilities by design.</p>
</div>

The reason MDI-QKD is secure even with a malicious relay is subtle but important. The relay C only announces which Bell state it measured — a classical four-valued result. From this announcement alone, C cannot determine Alice's or Bob's individual bit values; it only knows their correlation. More formally, security is proved by showing that MDI-QKD is equivalent to a virtual entanglement-based protocol (similar to E91) followed by measurement, and the relay's role reduces to entanglement distribution — a task that can be performed without learning the key bits.

The practical consequence of MDI-QKD is profound: the detector is the most attack-prone component of a QKD system, responsible for a long history of side-channel attacks. By removing detectors from trusted endpoints, MDI-QKD closes an entire class of attacks structurally rather than by engineering fixes. This makes it suitable for deployments where the measurement apparatus might be in a less secure location (e.g., a shared relay node in a metropolitan QKD network).

### 5.4.3 Twin-Field QKD (TF-QKD) — The O(√η) Breakthrough

Twin-Field QKD (TF-QKD), proposed by Lucamarini, Yuan, Dynes, and Shields in Nature (2018), represents one of the most significant theoretical advances in QKD since BB84. Its key rate scales as O(√η) — the square root of the channel transmittance — rather than O(η) for BB84 or O(η²) for MDI-QKD. This O(√η) scaling matches what would be achievable with an ideal quantum repeater, but without requiring quantum memory.

<figure class="book-figure">
<img src="content/images/image46.png" alt="Figure 5.3: Distance-Extended QKD — MDI-QKD, TF-QKD and Satellite Links">
<figcaption>Figure 5.3: Distance-Extended QKD — MDI-QKD, TF-QKD and Satellite Links</figcaption>
</figure>

```python
Channel transmittance at distance d km (0.2 dB/km fibre):

Key rate scaling comparison (per channel use):

MDI-QKD: 
TF-QKD: 

   BB84:    R ∝ 10⁻⁶    (too low for practical use)
   MDI-QKD: R ∝ 10⁻¹²  (negligible)
   TF-QKD:  R ∝ 10⁻³   (viable — ~kbit/s demonstrated experimentally)
```

The physical mechanism underlying TF-QKD's superior performance is single-photon interference rather than two-photon interference (as in MDI-QKD). Alice and Bob each send weak coherent pulses (WCPs) with average photon number μ ≈ 0.1, phase-locked to a shared reference laser, toward the relay C. The relay implements a simple interferometric measurement — measuring which of two detectors fires — and announces the result. Because only the single-photon component of each WCP contributes to the key, the effective channel transmittance for key generation is √η rather than η.

Intuitively, TF-QKD works because the key generation event requires only one photon to arrive at the relay (from either Alice or Bob), rather than one photon from each side (as in MDI-QKD). The probability of one photon arriving from one side is proportional to √η (the amplitude transmission, not the intensity transmission), which is much larger than η at long distances.

The security of TF-QKD was not immediately obvious and took several years of theoretical work to establish rigorously. The proof reduces TF-QKD to MDI-QKD via a virtual entanglement argument: Alice and Bob can be imagined as post-selecting on having sent exactly one photon, which is equivalent to distributing an entangled state, and the security follows from MDI-QKD security.

Experimentally, TF-QKD has achieved remarkable milestones. In 2020, Chen et al. (Nature Photonics) demonstrated TF-QKD over 509 km of ultra-low-loss fibre with a secret key rate of approximately 4 bits per second — the first positive key rate beyond 500 km of fibre. In 2022, Liu et al. extended this record to 833 km using specialised ultra-low-loss fibre (0.149 dB/km) and sophisticated phase stabilisation. These demonstrations represent the current world record for the longest fibre-based QKD link without quantum repeaters.

### 5.4.4 Satellite QKD: The Micius Satellite

The exponential loss of optical fibre motivates an alternative approach: free-space transmission through the atmosphere and space. For a Gaussian beam propagating through free space, the dominant loss mechanism is diffraction (beam spreading), which scales as (λ·d/A)² where λ is wavelength, d is propagation distance, and A is the receiver aperture area. This is polynomial rather than exponential in distance, offering dramatically lower loss at long distances.

For a 500 km satellite-to-ground link, the total transmission loss is approximately 30–50 dB (depending on telescope aperture and atmospheric conditions), roughly equivalent to 150–250 km of optical fibre. While this is still substantial, it is far less than the fibre loss over 500 km (100 dB). Moreover, the satellite link is not limited to the fibre route between two points — a single satellite can serve multiple ground stations across a continent in a single orbital pass.

The Micius satellite (墨子, named after the 5th century BC Chinese philosopher who conducted early studies of optics and logic), launched on August 16, 2016, is the world's first dedicated quantum science satellite. It was developed by the Chinese Academy of Sciences and the University of Science and Technology of China (USTC) under the leadership of Pan Jianwei. The satellite operates in a Sun-synchronous low Earth orbit at an altitude of approximately 500 km with an orbital period of 94 minutes.

| Micius Milestone | Year | Key Parameters | Reference |
|---|---|---|---|
| Satellite-to-ground BB84 QKD | 2017 | QBER ≈ 1.1%, secure key rate ≈ 1.1 kbps, distance 645–1200 km | Liao et al., Nature 549 (2017) |
| Intercontinental QKD relay | 2018 | Beijing–Vienna videoconference, 7600 km total, Micius as trusted relay | Liao et al., Nature 556 (2018) |
| Entanglement distribution | 2017 |  | Yin et al., Science 356 (2017) |
| Quantum teleportation | 2017 | Ground-to-satellite teleportation, fidelity 0.80 ± 0.01 | Ren et al., Nature 549 (2017) |
| Day-time QKD | 2017 | Key exchange under 10⁵ lux sunlight using 10 GHz spectral filter | Liao et al., Nature Photon. 11 (2017) |
| Ground-to-satellite uplink | 2020 | QKD with satellite as receiver; raw key ~2 kbps over 488–647 km | Yin et al., PRL (2020) |

The Micius experiments demonstrated for the first time that QKD, Bell-inequality testing, and quantum teleportation are all achievable over intercontinental distances using space. The satellite-to-ground QKD experiment used a three-intensity decoy-state BB84 protocol, with the satellite transmitting attenuated laser pulses at 850 nm wavelength. Ground station telescopes with 1.0 m apertures collected the photons, and a combination of timing gating (2 ns windows) and spectral filtering (0.01 nm bandwidth) suppressed background light to below the quantum signal level.

The achieved QBER of approximately 1.1% is exceptionally low — comparable to metropolitan fibre QKD systems — demonstrating that the pointing accuracy of the satellite (better than 0.5 microradians, equivalent to hitting a 25 cm target from 500 km) is sufficient to maintain high-quality quantum communication. The 1.1 kbps secure key rate applies to the ~100 second window when the satellite passes overhead; since the satellite makes approximately 5 overhead passes per month for any given ground station pair, the daily average key rate is much lower.

The most significant near-term application of the Micius technology is secure intercontinental key distribution. In 2018, the Micius team demonstrated a Beijing–Vienna intercontinental secure videoconference using Micius as a trusted relay: the satellite established separate QKD sessions with Beijing and Vienna ground stations during successive orbital passes, then combined the keys to encrypt the communication. While this required trusting the satellite itself (as opposed to device-independent security), it demonstrated the practical utility of satellite QKD at global scale.

## 5.5 Post-Quantum Cryptography: Lattice-Based Algorithms

### 5.5.1 Why Classical Public-Key Cryptography Is Vulnerable

RSA encryption, Diffie-Hellman key exchange, and elliptic-curve cryptography (ECC) form the security foundation of virtually all encrypted internet communication today. RSA, used in HTTPS, email encryption, and digital signatures, derives its security from the difficulty of factoring large composite integers: given n = p × q where p and q are large primes, it is computationally infeasible (with best classical algorithms) to recover p and q. Diffie-Hellman and ECC rely on the discrete logarithm problem: given g^x mod p, finding x is computationally hard.

In 1994, Peter Shor (then at Bell Laboratories) published a quantum algorithm that can solve both integer factorisation and the discrete logarithm problem in polynomial time on a quantum computer. The algorithm exploits quantum Fourier transforms and quantum parallelism to find the period of a function related to the factoring problem exponentially faster than any known classical algorithm. A quantum computer with approximately 4000 logical qubits (corresponding to roughly 4 million physical qubits with current error rates) could break RSA-2048 in a matter of hours.

Such machines do not exist today — the largest demonstrated quantum computers as of 2024 have a few hundred to a few thousand noisy physical qubits, far from the millions needed to run Shor's algorithm against real keys. However, the threat is real for two reasons: (1) the timeline to Cryptographically Relevant Quantum Computers (CRQCs) is uncertain but plausibly within 10–20 years, and (2) the "harvest now, decrypt later" (HNDL) threat means that data encrypted today with RSA or ECC is already at risk from future decryption.

<div class="box box-warning">
<p class="box-title"><strong>⚠  The Harvest-Now, Decrypt-Later (HNDL) Threat</strong></p>
<p>An adversary with large-scale data collection capability (nation-state level) can:</p>
<p>1. Intercept and store RSA/ECC-encrypted ciphertext today at low cost.</p>
<p>2. Wait until a sufficiently powerful quantum computer becomes available (5–20 years).</p>
<p>3. Decrypt all stored ciphertext retroactively using Shor's algorithm.</p>
<p>This threat is most severe for data requiring long-term secrecy:</p>
<p>● Classified government communications and intelligence</p>
<p>● Medical records and genetic data</p>
<p>● Financial contracts and trade secrets</p>
<p>● Critical infrastructure control systems</p>
<p>NIST recommends: Begin PQC migration no later than 2026 for critical systems.</p>
<p>NSA CNSA 2.0 Suite: Mandates PQC for all National Security Systems by 2033.</p>
<p>US Federal agencies: Required to inventory quantum-vulnerable cryptography by 2024.</p>
</div>

### 5.5.2 Lattice Problems: Learning With Errors (LWE) and Module-LWE

Post-quantum cryptography requires mathematical problems that are hard for both classical and quantum computers. The most successful candidate — the foundation of the CRYSTALS family — is the Learning With Errors (LWE) problem, introduced by Oded Regev at STOC 2005. LWE is believed to be quantum-hard because the best known quantum algorithms for solving it — lattice sieving algorithms — still require exponential time 2^{O(n)}, with no known polynomial-time quantum attack and only a modest constant-factor speedup from quantum techniques.

The LWE problem can be understood physically: Alice has a secret vector s ∈ Z\_q^n (n integers modulo q). She repeatedly draws a random vector a ∈ Z\_q^n and gives Bob the value b = ⟨a, s⟩ + e (mod q), where e is a small "error" drawn from a Gaussian distribution. The problem is to recover s from many (a, b) pairs. If there were no error term, this would be easily solved by Gaussian elimination in O(n³) time. The tiny error term e makes the system "noisy" in a way that defeats all known efficient algorithms.

<div class="box box-math">
<p class="box-title"><strong>▶  Definition: Learning With Errors (LWE — Regev, 2005)</strong></p>
<p>Parameters: dimension n, modulus q, error distribution χ (discrete Gaussian, σ ≈ √n)</p>
<p>Secret:      s ∈ Z_q^n  (uniformly random n-dimensional integer vector)</p>
<p>LWE sample: (a, b) where  a ← Z_q^n uniformly random</p>
<p>e ← χ  (small error)</p>
<p>Search-LWE:   Given polynomially many samples (aᵢ, bᵢ), find secret s.</p>
<p>Decision-LWE: Distinguish LWE samples from uniformly random pairs (a, u).</p>
<p>Hardness result (Regev 2005): LWE is at least as hard as quantum-approximating</p>
<p>worst-case lattice problems (GapSVP, SIVP) to within a polynomial factor.</p>
<p>This is the strongest quantum hardness reduction known in cryptography.</p>
</div>

Module-LWE (M-LWE), the variant used in CRYSTALS-Kyber and Dilithium, works over polynomial rings R\_q = Z\_q[x]/(x^n + 1) where n is a power of 2. Instead of dealing with vectors in Z\_q^n directly, secrets and errors are represented as matrices of polynomials with small coefficients. This ring structure allows polynomial multiplication to be performed using the Number Theoretic Transform (NTT) — the modular arithmetic analogue of the Fast Fourier Transform — reducing multiplication time from O(n²) to O(n log n). The practical benefit is that ML-KEM can be implemented efficiently on standard processors, achieving competitive performance with RSA despite the much larger parameter sizes.

<figure class="book-figure">
<img src="content/images/image47.png" alt="Figure 5.4: Lattice-Based Cryptography — the LWE Problem and NIST PQC Algorithms">
<figcaption>Figure 5.4: Lattice-Based Cryptography — the LWE Problem and NIST PQC Algorithms</figcaption>
</figure>

### 5.5.3 CRYSTALS-Kyber: Key Encapsulation Mechanism (ML-KEM, FIPS 203)

CRYSTALS-Kyber, standardised as ML-KEM (Module-Lattice Key Encapsulation Mechanism) in NIST FIPS 203 (August 2024), is the primary post-quantum key encapsulation mechanism. A KEM allows two parties to establish a shared secret over a public channel. Unlike traditional Diffie-Hellman key exchange (which is interactive), a KEM involves three algorithms: key generation (Alice creates a key pair), encapsulation (Bob uses Alice's public key to create a ciphertext and shared secret), and decapsulation (Alice uses her private key to recover the shared secret from Bob's ciphertext).

The Kyber KEM is built on M-LWE using the Fujisaki-Okamoto (FO) transform, which converts an IND-CPA secure public-key encryption scheme into an IND-CCA2 secure KEM. IND-CCA2 security (Indistinguishability under Chosen Ciphertext Attack 2) is the gold standard for public-key encryption: even an adversary that can query a decryption oracle for arbitrary ciphertexts cannot learn anything about the target ciphertext.

<div class="box box-real-world">
<p class="box-title"><strong>ℹ  ML-KEM (Kyber): Key Generation, Encapsulation, and Decapsulation</strong></p>
<p>KeyGen (Alice generates her key pair):</p>
<p>● Generate matrix A ∈ R_q^{k×k} from public seed ρ (expanded via SHAKE-128)</p>
<p>● Sample secret s ∈ R_q^k and error e ∈ R_q^k from centered binomial distribution</p>
<p>● Compute public key: t = As + e  (this is an M-LWE instance)</p>
<p>● Output: public key pk = (A, t),   private key sk = s</p>
<p>Encapsulate (Bob uses Alice's public key):</p>
<p>● Choose random 256-bit message m</p>
<p>● Sample r, e₁, e₂ from centered binomial</p>
<p>● Compute: u = Aᵀr + e₁     (ciphertext part 1)</p>
<p>v = tᵀr + e₂ + ⌊q/2⌋·m  (ciphertext part 2, encodes m)</p>
<p>● Shared secret: K = H(m)  (hash of random message)</p>
<p>● Send ciphertext c = (u, v) to Alice</p>
<p>Decapsulate (Alice uses her private key):</p>
<p>● Compute: m' = round((2/q)·(v − sᵀu))</p>
<p>The error terms cancel:</p>
<p>● Recover shared secret: K = H(m')</p>
<p>Correctness: The small error terms (e, e₁, e₂) cancel in decapsulation.</p>
<p>Security:    Any attempt to learn m from (u, v) without sk requires solving M-LWE.</p>
</div>

| Variant | Security Level | NIST Equivalent | Public Key (bytes) | Ciphertext (bytes) | Shared Secret |
|---|---|---|---|---|---|
| ML-KEM-512  (k=2) | Level 1 | ≈ AES-128 | 800 | 768 | 32 bytes |
| ML-KEM-768  (k=3) | Level 3 | ≈ AES-192 | 1184 | 1088 | 32 bytes |
| ML-KEM-1024 (k=4) | Level 5 | ≈ AES-256 | 1568 | 1568 | 32 bytes |

Performance comparison: ML-KEM-768 is dramatically faster than RSA-2048 for all cryptographic operations. Key generation takes approximately 0.03 ms for ML-KEM versus 5 ms for RSA — a factor of 167 faster. Encapsulation and decapsulation each take about 0.04 ms for ML-KEM versus 0.15 ms and 4 ms respectively for RSA. The price paid for quantum security is larger key and ciphertext sizes: ML-KEM-768 public keys are 1184 bytes versus 256 bytes for RSA-2048, a factor of 4.6 larger. For most applications — TLS handshakes, SSH key exchange, email encryption — this overhead is entirely acceptable given modern network bandwidth.

### 5.5.4 CRYSTALS-Dilithium: Digital Signatures (ML-DSA, FIPS 204)

CRYSTALS-Dilithium, standardised as ML-DSA (Module-Lattice Digital Signature Algorithm) in NIST FIPS 204 (August 2024), is the primary post-quantum digital signature scheme. Digital signatures are used for authentication: proving that a message was sent by a specific party without revealing their private key. ML-DSA is based on the hardness of both M-LWE (Module Learning With Errors) and M-SIS (Module Short Integer Solution) problems.

The key technique in Dilithium is the Fiat-Shamir with Aborts (FSA) paradigm. The signing algorithm works as follows: the signer masks their secret key s₁ with a random polynomial y, computes a commitment w = Ay, hashes the message together with w to get a challenge c (using the Fiat-Shamir transform, which replaces an interactive verifier with a hash function), and computes the response z = y + cs₁. The response z is "rejected" (the process is restarted with a new y) if z has large coefficients — this ensures that the distribution of accepted responses is independent of the secret s₁, preventing information leakage.

This rejection sampling step is critical for security: if responses were always accepted, an adversary could collect signatures and use them to deduce the signing key. The rejection ensures that signatures appear to come from a uniform distribution, revealing nothing about s₁. The expected number of rejections per signature is a small constant (typically 1-4), so the algorithm terminates quickly in practice.

| Variant | Security Level | Public Key (bytes) | Signature (bytes) | Private Key (bytes) |
|---|---|---|---|---|
| ML-DSA-44 (Dilithium2) | Level 2  (≈AES-128) | 1312 | 2420 | 2528 |
| ML-DSA-65 (Dilithium3) | Level 3  (≈AES-192) | 1952 | 3293 | 4000 |
| ML-DSA-87 (Dilithium5) | Level 5  (≈AES-256) | 2592 | 4595 | 4864 |

### 5.5.5 SPHINCS+: Stateless Hash-Based Signatures (SLH-DSA, FIPS 205)

SPHINCS+, standardised as SLH-DSA (Stateless Hash-Based Digital Signature Algorithm) in NIST FIPS 205 (August 2024), takes a completely different approach to post-quantum signatures. Instead of relying on algebraic hardness assumptions (lattice problems), its security reduces entirely to the one-wayness and collision-resistance of the underlying hash function — either SHA-256, SHA-512, or SHAKE. If the hash function is secure against quantum attacks (Grover's algorithm provides only a square-root speedup for hash inversion, so SHA-256 provides 128-bit quantum security), then SPHINCS+ is secure.

This makes SPHINCS+ the most conservative choice in the NIST portfolio: it makes no assumptions about the hardness of algebraic problems like LWE or SIS that might be broken by future cryptanalytic advances. Its security is based entirely on the well-studied hash function security, which has decades of cryptanalytic scrutiny behind it.

Architecturally, SPHINCS+ uses a hypertree: a multi-layer binary Merkle tree structure where each internal node is computed as a hash of its children, and digital signatures are generated by constructing a Merkle authentication path from a leaf to the root. The hypertree structure enables stateless signing — unlike earlier hash-based schemes such as XMSS (eXtended Merkle Signature Scheme), SPHINCS+ does not require the signer to maintain a counter of used leaf positions. Statefulness was a major deployment obstacle for XMSS: if the counter is reset (e.g., due to a backup restoration), the same one-time key would be reused, catastrophically breaking security.

The main trade-off of SPHINCS+ is signature size: at Level 1 security, signatures are approximately 17 KB — much larger than the 2.4 KB signatures of ML-DSA-44 or the 666-byte signatures of Falcon-512. Verification is fast (a few hash evaluations), but signing requires computing many hash function evaluations and is slower than lattice-based schemes. For applications where signatures are generated infrequently (certificate authority root signatures, firmware update signatures, code signing keys) and stored for long periods, the large signature size is acceptable in exchange for the conservative security assumptions.

<div class="box box-key-concept">
<p class="box-title"><strong>💡  When to Choose Each NIST PQC Signature Scheme</strong></p>
<p>ML-DSA-65 (Dilithium3):</p>
<p>Best general-purpose choice. Fast signing and verification (~0.4 ms), moderate</p>
<p>signature size (3.3 KB), Level 3 security. Recommended for TLS certificates,</p>
<p>code signing, S/MIME email, and most PKI applications.</p>
<p>SLH-DSA-SHA2-128f (SPHINCS+):</p>
<p>Choose when algebraic hardness assumptions are unacceptable or maximum long-term</p>
<p>security is required. Ideal for root CA certificates (signed rarely and archived),</p>
<p>firmware update signatures, and archival signatures with multi-decade validity.</p>
<p>FN-DSA-512 (Falcon-512, FIPS 206):</p>
<p>Smallest signatures (~666 bytes) and public keys (~897 bytes) in the FIPS suite.</p>
<p>Best for bandwidth-constrained environments: IoT devices, blockchain protocols,</p>
<p>embedded systems. Security based on NTRU lattices. Implementation requires</p>
<p>careful handling of constant-time floating-point arithmetic.</p>
</div>

## 5.6 NIST PQC Standardisation: Timeline and Implementation

### 5.6.1 The Standardisation Process

NIST launched the Post-Quantum Cryptography Standardisation project in December 2016, issuing a call for proposals for quantum-resistant public-key algorithms. The process was deliberately modelled on the successful AES (1997–2001) and SHA-3 (2007–2012) competitions: transparent, open, international, and based on public cryptanalytic scrutiny over multiple years. By involving the global cryptographic research community rather than a closed government committee, NIST maximised the probability that weaknesses would be discovered before standardisation.

The competition received 69 complete submissions in November 2017, spanning diverse mathematical foundations: lattice-based schemes, code-based schemes, hash-based schemes, multivariate polynomial schemes, supersingular isogeny-based schemes, and others. The diversity was deliberate — hedging against the possibility that any single mathematical approach might be broken by a future algorithm advance.

| Phase | Timeline | Key Event |
|---|---|---|
| Call for Proposals | Dec 2016 | NIST publishes requirements: IND-CCA2 KEMs, UF-CMA signatures, 5 security levels, side-channel resistance. |
| Round 1 submissions | Nov 2017 | 69 complete submissions received. Initial review eliminates weak submissions. |
| Round 2 selection | Jan 2019 | 26 candidates advance. Community attacks eliminate several (NewHope, ThreeBears, LUOV, Rainbow). |
| Round 3 Finalists | Jul 2020 | 4 finalists (Kyber, Dilithium, Falcon, SPHINCS+) plus 5 alternates (BIKE, HQC, McEliece, SIKE, NTRU). |
| SIKE broken | Jul 2022 | Castryck–Decru publish classical polynomial-time attack on SIKE. Eliminated entirely. |
| Initial selections | Jul 2022 | NIST announces Kyber and Dilithium as primary selections. Falcon and SPHINCS+ as additional signatures. |
| Draft FIPS published | Aug 2023 | Draft FIPS 203, 204, 205 for public comment. Minor parameter adjustments. |
| Final FIPS published | Aug 2024 | FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA), FIPS 206 (FN-DSA/Falcon) official. |
| Migration deadline | 2030 (target) | NIST recommends all US Federal systems complete PQC migration. NSA CNSA 2.0: NSS by 2033. |

<figure class="book-figure">
<img src="content/images/image48.png" alt="Figure 5.5: NIST Post-Quantum Cryptography Standardisation Timeline (2016–2024)">
<figcaption>Figure 5.5: NIST Post-Quantum Cryptography Standardisation Timeline (2016–2024)</figcaption>
</figure>

The most dramatic event of the standardisation process was the complete break of SIKE (Supersingular Isogeny Key Encapsulation) in July 2022. SIKE had survived four rounds of evaluation and was considered one of the most promising quantum-safe KEMs. A paper by Castryck and Decru (presented at EUROCRYPT 2023) described a classical polynomial-time attack — not even requiring a quantum computer — that could recover SIKE private keys. The attack exploited unexpected mathematical structure in the isogeny computation that had not been anticipated during the design phase. SIKE was immediately withdrawn from consideration.

The SIKE break is instructive for two reasons. First, it demonstrates that PQC security analysis remains an active area and that new mathematical attacks are possible even for schemes that have been publicly scrutinised for years. Second, it illustrates why NIST selected multiple algorithms based on different mathematical foundations (lattices, hash functions, NTRU) rather than a single winner: if one mathematical approach is broken, others remain secure.

### 5.6.2 Key Implementation Requirements from FIPS 203/204/205

The FIPS standards do not merely specify algorithm parameters — they mandate specific implementation practices that are essential for real-world security:

**Randomness source:** FIPS 203 §3.3 requires that the Random Bit Generator (RBG) used for key generation must be compliant with NIST SP 800-90A (deterministic RBGs) or SP 800-90B/C (non-deterministic entropy sources). Quantum random number generators (QRNGs) qualify under SP 800-90B if validated. Using a weak or predictable random number generator is the most common way that otherwise-correct cryptographic implementations are broken in practice.

**Constant-time implementation:** FIPS 203 and 204 mandate that implementations must not reveal secret information through timing side-channels. Rejection sampling in ML-DSA creates variable execution time that could leak information about the rejection pattern — which in turn could reveal the secret key. Implementations must pad or mask loop counts to equalise execution time. The FIPS standards include constant-time reference code in appendices.

**Key derivation:** Shared secrets from ML-KEM must not be used directly as symmetric encryption keys. FIPS 203 §7.2 requires processing through an approved Key Derivation Function (KDF) — specifically HKDF (HMAC-based KDF, RFC 5869) with SHA-256 or SHA-384. The raw ML-KEM output, while cryptographically secure, may have subtle correlations that a KDF removes.

**Hybrid deployment (strongly recommended):** NIST IR 8413 strongly recommends deploying PQC in hybrid mode alongside classical algorithms during the transition period. For example, TLS 1.3 key exchange should use X25519 + ML-KEM-768, combining the shared secrets with HKDF. This "belt-and-suspenders" approach ensures that even if ML-KEM is unexpectedly broken (as SIKE was), X25519 provides classical security. The IETF has standardised the X25519Kyber768 hybrid for TLS 1.3 (RFC 9370).

**RECAP**

*Short Answer Questions and Model Answers — Chapter 5*

## Short Answer Questions

Answer each question in 3–6 sentences. These questions test conceptual understanding of the core topics in Chapter 5.

**1. What is information-theoretic security in QKD?**

**2. State the No-Cloning Theorem.**

**3. Define Quantum Bit Error Rate (QBER).**

**4. What is the BB84 security threshold for QBER?**

**5. What is the binary entropy function used in BB84?**

**6. What is privacy amplification?**

**7. What is the Leftover Hash Lemma?**

**8. What is the main security principle behind the E91 protocol?**

**9. What is the CHSH inequality classical limit?**

**10. What is the Tsirelson bound?**

**11. What is Measurement-Device-Independent QKD (MDI-QKD)?**

**12. Why is TF-QKD important?**

**13. What is the Micius satellite?**

**14. What is the Learning With Errors (LWE) problem?**

**15. Name the major NIST-standardised post-quantum algorithms.**

## Model Answers

**1. What is information-theoretic security in QKD?**

**Answer:** Information-theoretic security means a protocol remains secure against an adversary with unlimited computational power because the adversary simply does not possess enough information to break it — no computation can help. In QKD, security derives solely from the laws of quantum mechanics rather than from the assumed hardness of mathematical problems. This contrasts with computational security (RSA, ECC), which can be broken if efficient quantum algorithms exist. The formal definition uses the trace-norm distance between the actual key state and an ideal perfectly random key, with the security parameter ε typically required to be below 2⁻¹⁰⁰.

**2. State the No-Cloning Theorem.**

**Answer:** The No-Cloning Theorem states that there is no physical operation (no unitary transformation) that can create a perfect copy of an arbitrary unknown quantum state. The proof follows from the linearity of quantum mechanics: assuming a cloning unitary U exists and applying it to two non-orthogonal states |ψ⟩ and |φ⟩, then taking the inner product of both sides yields ⟨ψ|φ⟩ = (⟨ψ|φ⟩)², which is impossible for states with inner product strictly between 0 and 1. In BB84, this theorem means Eve cannot copy intercepted qubits to retain information while forwarding a perfect duplicate to Bob — any interception attempt disturbs the qubit and introduces detectable errors.

**3. Define Quantum Bit Error Rate (QBER).**

**Answer:** The Quantum Bit Error Rate (QBER) is the fraction of sifted key bits — those for which Alice and Bob used the same measurement basis — on which their recorded values disagree: Q = (number of erroneous bits) / (total sifted bits). In an ideal noise-free channel with no eavesdropping, Q should be exactly zero. In practice, detector noise and optical imperfections give a background QBER of 1–3%, while any eavesdropping attempt adds additional errors on top of this background. The QBER is therefore the primary observable used to detect eavesdropping and quantify channel security in BB84 and related protocols.

**4. What is the BB84 security threshold for QBER?**

**Answer:** BB84 can generate a provably secure key if and only if the observed QBER satisfies Q < Q\_threshold ≈ 11.0%. This threshold is derived from the Devetak–Winter bound r ≥ 1 − 2h(Q), where h(Q) is the binary entropy function: setting r = 0 and solving h(Q) = 0.5 gives Q ≈ 11%. Above this threshold, Eve's information about the sifted key exceeds what Alice and Bob share, making it mathematically impossible to distil a secret key through privacy amplification. The intercept-resend attack causes Q = 25%, far above the threshold, ensuring any such attack is immediately and decisively detected.

**5. What is the binary entropy function used in BB84?**

**Answer:** The binary entropy function is h(Q) = −Q log₂Q − (1−Q) log₂(1−Q), which measures the Shannon entropy of a biased coin with probability Q of heads. In the BB84 security proof, h(Q) appears twice in the key rate formula r = 1 − 2h(Q): once to account for the information Eve learns from the error-correction syndrome (at most h(Q) bits per sifted bit), and once to bound Eve's additional information from her quantum measurements. The function equals 0 at Q = 0 (no uncertainty), reaches 1 at Q = 0.5 (maximum uncertainty), and satisfies h(Q) = 0.5 at Q ≈ 11%, defining the security threshold.

**6. What is privacy amplification?**

**Answer:** Privacy amplification is the final classical post-processing step in QKD that converts a partially compromised n-bit sifted key into a shorter but provably secret key. After error correction, both Alice and Bob hold identical keys, but Eve may have partial information gained from her quantum measurements and the error-correction syndrome. Privacy amplification applies a randomly selected universal-2 hash function — typically implemented as multiplication by a random Toeplitz matrix — to compress the key from n bits to m = n[1−2h(Q)] bits. By the Leftover Hash Lemma, the resulting compressed key is statistically indistinguishable from a perfectly random string with no correlation to Eve's quantum state, achieving ε-secrecy.

**7. What is the Leftover Hash Lemma?**

**Answer:** The Leftover Hash Lemma (Bennett, Brassard, Crépeau, Maurer 1995; Renner 2005) states that if X is a string with min-entropy H\_min(X|E) ≥ n − k (Eve knows at most k bits), then applying a randomly chosen 2-universal hash h: {0,1}ⁿ → {0,1}^m produces an output within trace-norm distance 2^{−(H\_min − m)/2} of a perfectly uniform key independent of Eve's register E. Setting m = H\_min − 2log₂(1/ε) achieves ε-secrecy. In BB84, the min-entropy satisfies H\_min ≥ n[1−h(Q)] (after accounting for error correction), giving a final key length m ≈ n[1−2h(Q)] that is the BB84 key rate formula. Practically, the hash is implemented as a Toeplitz matrix applied in O(n log n) time via FFT.

**8. What is the main security principle behind the E91 protocol?**

**Answer:** The E91 protocol uses the violation of the CHSH Bell inequality as a security certificate. Alice and Bob share maximally entangled Bell pairs (|Ψ⁻⟩) from a central source and measure in randomly chosen bases. By computing the CHSH parameter S = E(a₁,b₁) − E(a₁,b₂) + E(a₂,b₁) + E(a₂,b₂) from their non-key-generating rounds, they test whether |S| > 2. Any classical eavesdropping strategy — including sophisticated intercept-resend attacks — is a local hidden-variable model and therefore cannot produce |S| > 2. A measured violation |S| > 2 proves the correlations are genuinely quantum, certifying that no classical adversary could have pre-programmed the outcomes, and hence the key is secure.

**9. What is the CHSH inequality classical limit?**

**Answer:** The CHSH inequality states that for all local hidden-variable (classical) theories: |S| ≤ 2, where S is the CHSH parameter computed from four correlation measurements E(aᵢ, bⱼ). This inequality was derived by Clauser, Horne, Shimony, and Holt in 1969 to make Bell's theorem experimentally testable. Any classical strategy, including pre-shared randomness between Alice and Bob, cannot produce |S| > 2. Quantum mechanics — through entangled states — can violate this bound, with the maximum quantum value given by the Tsirelson bound |S| ≤ 2√2. The gap between the classical bound (2) and the quantum maximum (2√2 ≈ 2.828) is the window within which Bell inequality violation certifies quantum entanglement.

**10. What is the Tsirelson bound?**

**Answer:** The Tsirelson bound is the maximum value of the CHSH parameter |S| allowed by quantum mechanics: |S| ≤ 2√2 ≈ 2.828. This bound was proved by Boris Tsirelson (Cirel'son) in 1980 using the operator inequality for quantum measurements. The bound is tight — it is achieved by maximally entangled Bell states measured at the optimal angles (22.5° offsets). In E91, a measured S approaching −2√2 is the best possible outcome, confirming maximal entanglement and optimal security. Practical experiments show values somewhat below 2√2 due to measurement imperfections and noise, but any value with |S| > 2 still certifies security. The Micius satellite achieved S = 2.37 ± 0.09, violating the classical bound by 4 standard deviations.

**11. What is Measurement-Device-Independent QKD (MDI-QKD)?**

**Answer:** MDI-QKD (Measurement-Device-Independent Quantum Key Distribution), proposed by Lo, Curty, and Qi (2012), is a QKD protocol where both Alice and Bob send quantum states to an untrusted central relay node C, which performs a Bell State Measurement and announces the result. Neither Alice nor Bob ever measures anything — all detectors are at the relay. This design completely eliminates detector side-channel attacks by construction, since the endpoints have no detectors that could be "blinded" or otherwise manipulated. Even if the relay is fully controlled by Eve, she cannot learn Alice's or Bob's bit values from the BSM outcome. The key rate scales as O(η²) rather than O(η), which is a performance penalty, but the unconditional security against all detector attacks is the key advantage.

**12. Why is TF-QKD important?**

**Answer:** Twin-Field QKD (TF-QKD), proposed by Lucamarini et al. in 2018, achieves a key-rate scaling of O(√η) — the square root of the channel transmittance — rather than O(η) for standard BB84. This breakthrough means that at 300 km fibre, TF-QKD achieves a key rate roughly 1000 times higher than BB84 and enormously higher than MDI-QKD. The O(√η) scaling equals the rate achievable with an ideal quantum repeater, allowing positive key rates beyond 500 km of standard fibre (833 km demonstrated in 2022) without requiring quantum memory. TF-QKD works by exploiting single-photon interference at the relay rather than two-photon interference, so only one photon from either side needs to arrive per key generation event.

**13. What is the Micius satellite?**

**Answer:** Micius (墨子) is the world's first quantum communication satellite, launched by the Chinese Academy of Sciences and USTC on August 16, 2016. Operating in a 500 km sun-synchronous low Earth orbit, it has demonstrated satellite-to-ground QKD over distances up to 1200 km (achieving 1.1 kbps secure key rate with QBER ≈ 1.1%), quantum entanglement distribution over 1203 km (Bell parameter S = 2.37 ± 0.09, violating the classical bound), and the first intercontinental secure quantum videoconference between Beijing and Vienna (7600 km total link, 2018). Micius proved that quantum communication is feasible from space, overcoming the exponential loss limitation of optical fibres for intercontinental distances.

**14. What is the Learning With Errors (LWE) problem?**

**Answer:** Learning With Errors (LWE), introduced by Oded Regev at STOC 2005, is a computational problem where one is given noisy linear equations: from many samples (a, b = ⟨a,s⟩ + e mod q) — where s is a secret vector and e is a small random error — the task is to recover s. Without the error term, this is trivial linear algebra; the small error makes it exponentially hard. LWE is quantum-hard because no polynomial-time quantum algorithm is known for it — the best quantum attacks (lattice sieving) still require exponential time 2^{O(n)}. Regev proved a quantum worst-case hardness reduction: if LWE is easy, then worst-case lattice problems are also easy, which would contradict decades of computational geometry research. Module-LWE extends LWE to polynomial rings, enabling compact keys and NTT-accelerated arithmetic.

**15. Name the major NIST-standardised post-quantum algorithms.**

**Answer:** The four NIST-standardised post-quantum algorithms (August 2024) are: ML-KEM (FIPS 203, formerly CRYSTALS-Kyber) — a key encapsulation mechanism based on Module-LWE, providing quantum-safe key exchange to replace RSA and ECDH; ML-DSA (FIPS 204, formerly CRYSTALS-Dilithium) — a digital signature scheme based on Module-LWE and Module-SIS, replacing RSA and ECDSA signatures; SLH-DSA (FIPS 205, formerly SPHINCS+) — a stateless hash-based signature scheme whose security relies only on hash function one-wayness, making it the most conservative choice; and FN-DSA (FIPS 206, formerly Falcon) — a compact signature scheme based on NTRU lattices, offering the smallest signature sizes in the portfolio for bandwidth-constrained applications.

## Solved Examples — Chapter 5

<div class="box box-example">
<p class="box-title"><strong>Example 5.1 — QBER Threshold Calculation</strong></p>
<p>Problem: An experimental BB84 session observes a QBER of Q = 5%. Calculate:</p>
<p>(a) the binary entropy h(0.05),</p>
<p>(b) the secure key rate fraction r,</p>
<p>(c) whether the channel is secure,</p>
<p>(d) compare with the threshold case Q = 11%.</p>
<p>Solution:</p>
<p>(a) h(Q) = −Q log₂Q − (1−Q) log₂(1−Q)</p>
<p>h(0.05) = −0.05 × log₂(0.05) − 0.95 × log₂(0.95)</p>
<p>= −0.05 × (−4.322) − 0.95 × (−0.0740)</p>
<p>= 0.2161 + 0.0703 = 0.2864</p>
<p>(b) r = 1 − 2h(0.05) = 1 − 2 × 0.2864 = 1 − 0.5728 = 0.4272</p>
<p>Interpretation: 42.7% of sifted bits become secure key.</p>
<p>(c) Since r = 0.4272 &gt; 0, the channel is secure. Q = 5% &lt; 11% threshold.</p>
<p>The protocol is technically secure but highly inefficient — barely any key is produced.</p>
<p>In practice, systems abort if Q &gt; 8% to maintain useful key rates.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 5.2 — Privacy Amplification Key Length</strong></p>
<p>Problem: A BB84 session produces n = 10,000 sifted bits with QBER Q = 3%.</p>
<p>Calculate: (a) bits leaked in error correction, (b) privacy-amplified key length</p>
<p>Solution:</p>
<p>(a) Error correction leakage = n × h(Q) = 10000 × h(0.03)</p>
<p>h(0.03) = −0.03 × log₂(0.03) − 0.97 × log₂(0.97)</p>
<p>= −0.03 × (−5.059) − 0.97 × (−0.0439)</p>
<p>= 0.1518 + 0.0426 = 0.1944</p>
<p>(b) Privacy amplification output length:</p>
<p>Min-entropy lower bound:</p>
<p>= 10000 × [1 − 2 × 0.1944] = 10000 × 0.6112 = 6112 bits</p>
<p>m = 6112 − 200 = 5912 bits of provably secure key.</p>
<p>Efficiency: 5912/10000 = 59.1% of sifted bits become secure key at Q = 3%.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 5.3 — CHSH Parameter for E91 at Optimal Angles</strong></p>
<p>Problem:</p>
<p>calculate the CHSH parameter S and verify it saturates the Tsirelson bound.</p>
<p>Solution:</p>
<p>Calculate each term:</p>
<p>CHSH parameter:</p>
<p>S = E(a₁,b₁) − E(a₁,b₂) + E(a₂,b₁) + E(a₂,b₂)</p>
<p>= (−1/√2) − (+1/√2) + (−1/√2) + (−1/√2)</p>
<p>|S| = 2√2 ≈ 2.828, which equals the Tsirelson bound exactly.</p>
<p>This confirms these are the globally optimal measurement angles.</p>
<p>Classical bound: |S| ≤ 2. Quantum violation: 2.828 &gt; 2. Security certified.</p>
</div>

## Multiple Choice Questions — Chapter 5

Note: Answers are collected at the end of this chapter.

**1.  In the BB84 security proof, the QBER threshold of ~11% arises because:**

- (A)  The intercept-resend attack introduces exactly 11% errors

- (B)  The binary entropy function h(Q) = 1/2 at Q ≈ 11%, making the secure key rate r = 1−2h(Q) = 0

- (C)  Eve can only intercept 11% of qubits without being detected

- (D)  The no-cloning theorem limits copying efficiency to 89%

**2.  The Leftover Hash Lemma guarantees that the output of a 2-universal hash applied to a string with min-entropy H\_min satisfies:**

- (A)  It is compressed by exactly a factor of H\_min

- (B)  It is indistinguishable from uniform within trace-norm ε ≤ 2^{−(H\_min−m)/2} for output length m

- (C)  It is perfectly random regardless of Eve's information

- (D)  It is identical to the output of SHA-256 applied to the same string

**3.  In E91, measuring the CHSH parameter S = −2.7 implies:**

- (A)  The channel has been compromised — |S| < 2√2

- (B)  The protocol should abort because the Bell inequality is satisfied

- (C)  Strong Bell inequality violation — the qubits are entangled and the key is secure

- (D)  The measurement bases were incorrectly chosen

**4.  The key rate advantage of TF-QKD over MDI-QKD at distance d is approximately:**

- (A)  A constant factor of 2×

- (C)  O(√η) vs O(η) — an improvement scaling as 1/√η

- (D)  Identical — both scale as O(η²)

**5.  The Micius satellite QKD experiment demonstrated a secure key rate of approximately:**

- (A)  1 Mbps at 1200 km

- (B)  1.1 kbps at 645–1200 km with QBER ~1.1%

- (C)  300 bps at 500 km with QBER ~5%

- (D)  10 Gbps at 500 km using coherent detection

**6.  In ML-KEM (CRYSTALS-Kyber), the shared secret K is derived as:**

- (A)  K = As + e (the public key computation)

- (B)  K = H(m) where m is a random 256-bit message encrypted in the ciphertext

- (C)  K = XOR of Alice's and Bob's private keys

- (D)  K = SHA-256(ciphertext ‖ public key)

**7.  SPHINCS+ (SLH-DSA) is described as "stateless" because:**

- (A)  It does not require any state to be transmitted in the signature

- (B)  Unlike XMSS, it does not require the signer to maintain a counter of used one-time keys

- (C)  The private key has no state — it is regenerated from a seed each time

- (D)  It achieves statelessness by using a stateful server

**8.  The "harvest-now, decrypt-later" attack motivates PQC migration because:**

- (A)  Current quantum computers can already break AES-128

- (B)  Adversaries can store RSA-encrypted ciphertext now and decrypt it when quantum computers mature

- (C)  Hash functions will be broken by quantum computers within 5 years

- (D)  Symmetric encryption keys are immediately vulnerable to Grover's algorithm

**9.  FIPS 203 (ML-KEM) specifies that shared secrets from Kyber must be processed through:**

- (A)  SHA-3/SHAKE-256 directly

- (B)  An approved KDF such as HKDF before use as symmetric keys

- (C)  AES-256 in CBC mode to produce final keys

- (D)  No further processing — the Kyber output is already a uniformly random key

**10.  Module-LWE (M-LWE) improves on standard LWE by:**

- (A)  Using smaller moduli q to reduce key size at the cost of lower security

- (B)  Working over polynomial rings R\_q, enabling NTT-accelerated multiplication and compact keys

- (C)  Reducing the problem to the subset-sum problem, which is classically hard

- (D)  Replacing Gaussian error distributions with uniform distributions for faster sampling

**11.  The Tsirelson bound states that for any quantum state and CHSH measurement:**

- (D)  |S| can be arbitrarily large for multi-qubit entangled states

**12.  MDI-QKD is immune to detector side-channel attacks because:**

- (A)  The detectors are shielded by Faraday cages that block Eve's bright-light pulses

- (B)  Alice and Bob have no detectors — all measurements are made by the (potentially malicious) relay C

- (C)  The Bell state measurement at C is performed in a secure enclave

- (D)  Detector blinding attacks require knowledge of the measurement basis, which is secret

**13.  In the Fiat-Shamir with Aborts paradigm used in ML-DSA, the rejection sampling step ensures:**

- (A)  That invalid signatures are detected and rejected by the verifier

- (B)  That the distribution of accepted response vectors z = y + cs₁ is independent of the secret s₁

- (C)  That the signing algorithm terminates in constant time

- (D)  That the hash function used for the challenge is collision-resistant

**14.  The SIKE protocol was eliminated from NIST PQC Round 3 because:**

- (A)  Its performance was too slow for practical deployment

- (B)  A classical polynomial-time attack (Castryck–Decru) completely broke it in July 2022

- (C)  Its security reduction was found to be incorrect

- (D)  It required quantum hardware for key generation

**15.  For a hybrid X25519 + ML-KEM-768 TLS key exchange, security is guaranteed if:**

- (A)  Both X25519 and ML-KEM-768 are simultaneously broken

- (B)  Either X25519 or ML-KEM-768 remains secure — the shared secret requires breaking both

- (C)  X25519 is secure (ML-KEM provides no additional security in hybrid mode)

- (D)  The HKDF output length is at least 384 bits

## MCQ Answers — Chapter 5

| Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 | Q8 | Q9 | Q10 | Q11 | Q12 | Q13 | Q14 | Q15 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| B | B | C | B | B | B | B | B | B | B | A | B | B | B | B |

## Unsolved Problems — Chapter 5

**Problem 5.1:** A BB84 session observes QBER = 4.5%. (a) Compute h(0.045). (b) What fraction of sifted bits become secure key? (c) If the sifted key has 50,000 bits and ε = 2⁻¹²⁸, what is the final key length?

**Problem 5.2:** For E91 with |Ψ⁻⟩ at optimal angles, verify S = −2√2. If S\_measured = −2.61 ± 0.05, (a) is the Bell inequality violated? (b) What is the significance level in σ?

*[Hint: |−2.61| > 2.0 ✓; violation significance = (2.61−2.0)/0.05 = 12.2σ]*

**Problem 5.3:** For TF-QKD at d = 400 km and d = 600 km with 0.2 dB/km fibre, calculate η and the ratio R\_TF/R\_BB84. At what distance is TF-QKD exactly 10⁶× more efficient?

**Problem 5.4:** Compute the total key and ciphertext sizes for one TLS handshake using: (a) RSA-4096, (b) ML-KEM-1024, (c) hybrid P-521 + ML-KEM-1024.

*[Hint: (a) 1024 B; (b) 3136 B; (c) 3335 B. Hybrid offers best security.]*

**Problem 5.5:** An ML-DSA-65 private key is compromised. Describe which mathematical problem must be solved to forge a signature, and state the best known classical and quantum time complexities.

*[Hint: Module-SIS — finding short vectors. Classical: 2^{0.292n}; quantum: 2^{0.265n}. Effective complexity far beyond feasible.]*

**Problem 5.6:** SPHINCS+-SHA2-128f signs 1000 firmware updates per day for an IoT fleet. (a) Daily signature storage overhead. (b) Verification time if SHA-256 takes 1 μs/compression. (c) Compare with ML-DSA-44.

*[Hint: (a) 17.1 MB/day; (b) ~56 μs/verify; (c) ML-DSA-44: 2.42 MB/day, 7× smaller]*

**Problem 5.7:** A QKD fibre network links cities A–B (80 km) and B–C (120 km) with trusted relay at B. With MDI-QKD, where should the BSM relay be placed on each segment for maximum key rate?

*[Hint: Midpoint of each segment: relay at 40 km from A; relay at 60 km from B (or C)]*

**Problem 5.8:** Verify the no-cloning proof: given U|0⟩|0⟩ = |0⟩|0⟩ and U|+⟩|0⟩ = |+⟩|+⟩, compute ⟨0|+⟩ on both sides and show the contradiction. State the quantum cloning bound (Bužek–Hillery).

*[Hint: Before: 1/√2; after: (1/√2)² = 1/2. Contradiction. Max cloning fidelity = 5/6.]*

## Theory Questions — Chapter 5

- Explain the difference between computational and information-theoretic security. Why can QKD achieve IT security while RSA cannot? What assumptions does the BB84 security proof make?

- Derive the QBER threshold of ~11% for BB84 from the Devetak–Winter bound. Show the full calculation using the binary entropy function h(Q).

- State and prove the No-Cloning Theorem. Explain precisely how it connects to BB84 security: at what step of an eavesdropping attack does no-cloning make the attack detectable?

- Describe the Leftover Hash Lemma. What is min-entropy, and how is it estimated in BB84? What is the role of 2-universal hash functions, and give a practical example (Toeplitz matrix construction).

- Explain why MDI-QKD is immune to detector side-channel attacks by construction. Why does the key rate scale as O(η²) rather than O(η)? What is the practical consequence for network topology design?

- Derive the TF-QKD key rate scaling O(√η) intuitively using the single-photon interference argument. Compare with the PLOB bound. Does TF-QKD surpass the PLOB bound? Why or why not?

- Define the Module-LWE problem. Explain why M-LWE is hard for quantum computers, and why the ring structure R\_q = Z\_q[x]/(x^n+1) enables efficient implementation via NTT.

- Trace the NIST PQC competition from 2016 to 2024. What were the selection criteria? Why was SIKE eliminated in Round 3? What does the SIKE break teach us about trusting new mathematical hardness assumptions?

## Assignments & Project Suggestions — Chapter 5

### Assignment 5.1 BB84 Security Simulation (Marks: 15)

Implement a complete BB84 simulation in Python, including: (a) qubit preparation and transmission with adjustable eavesdropping fraction e ∈ [0, 1]; (b) sifting, QBER estimation, and abort condition; (c) error correction using a Hamming code or Cascade protocol; (d) privacy amplification via SHA-256 Toeplitz hashing. Plot the secret key rate r as a function of eavesdropping fraction e from 0 to 0.15. Verify the 11% QBER threshold numerically and confirm that the key rate drops to zero at the threshold.

### Assignment 5.2 E91 Bell Test Simulation (Marks: 10)

Simulate the E91 protocol: (a) generate |Ψ⁻⟩ pairs and compute measurement outcomes for all 9 angle-pair combinations; (b) compute the CHSH parameter S; (c) add depolarising noise at level p and plot S vs p; (d) find the noise threshold at which |S| ≤ 2 and the protocol would abort.

### Assignment 5.3 PQC Benchmarking (Marks: 15)

Install the NIST reference implementations of ML-KEM-768, ML-DSA-65, and SLH-DSA-SHA2-128f. Benchmark on your hardware: (a) key generation, signing/encapsulation, and verification/decapsulation time (1000 trials each); (b) key and signature sizes; (c) compare with RSA-2048 and ECDSA-P256. Write a 1500-word migration report recommending algorithms for: (i) email encryption, (ii) TLS, (iii) code signing.

## Chapter 5 Summary

- BB84 is information-theoretically secure: the Devetak–Winter bound r ≥ 1 − 2h(Q) gives a positive key rate for QBER < 11.0%, derived from the binary entropy function h(Q).

- Privacy amplification via the Leftover Hash Lemma converts a partially compromised n-bit key into m = n[1−2h(Q)] − 2log₂(1/ε) provably secret bits using 2-universal hashing (Toeplitz matrices).

- E91 uses entangled Bell pairs and CHSH inequality violations (|S| > 2) as a security certificate against all local hidden-variable strategies; optimal angles give S = −2√2, saturating the Tsirelson bound.

- MDI-QKD eliminates all detector side-channels by removing Alice's and Bob's detectors; an untrusted relay performs Bell-state measurements; key rate scales as O(η²).

- TF-QKD achieves O(√η) key rate scaling via single-photon interference, enabling positive key rates beyond 500 km without quantum repeaters (833 km world record demonstrated 2022).

- Micius satellite demonstrated 1200 km satellite-to-ground QKD (1.1 kbps, QBER ~1.1%), 7600 km intercontinental relay, and 1203 km entanglement distribution with S = 2.37 ± 0.09.

- CRYSTALS-Kyber (ML-KEM, FIPS 203) and Dilithium (ML-DSA, FIPS 204) are lattice-based PQC standards secured by Module-LWE; SPHINCS+ (SLH-DSA, FIPS 205) provides conservative hash-based signatures.

- NIST FIPS 203/204/205/206 were finalised in August 2024; US Federal systems must migrate by 2030; NIST recommends hybrid PQC + classical deployment during the transition period.

- The SIKE break in 2022 demonstrated that even thoroughly reviewed PQC candidates can be completely broken — emphasising the importance of algorithm diversity and mathematical rigour over intuition.

## References — Chapter 5

- Bennett, C. H., & Brassard, G. (1984). Quantum cryptography: Public key distribution and coin tossing. Proceedings of IEEE ICCSSP, Bangalore, 175–179.

- Shor, P. W., & Preskill, J. (2000). Simple proof of security of the BB84 quantum key distribution protocol. Physical Review Letters, 85(2), 441.

- Mayers, D. (2001). Unconditional security in quantum cryptography. Journal of the ACM, 48(3), 351–406.

- Renner, R. (2008). Security of quantum key distribution. International Journal of Quantum Information, 6(01), 1–127.

- Ekert, A. K. (1991). Quantum cryptography based on Bell's theorem. Physical Review Letters, 67(6), 661.

- Lo, H.-K., Curty, M., & Qi, B. (2012). Measurement-device-independent QKD. Physical Review Letters, 108(13), 130503.

- Lucamarini, M., Yuan, Z. L., Dynes, J. F., & Shields, A. J. (2018). Overcoming the rate–distance limit of QKD without quantum repeaters. Nature, 557, 400–403.

- Liao, S.-K. et al. (2017). Satellite-to-ground quantum key distribution. Nature, 549, 43–47.

- Yin, J. et al. (2017). Satellite-based entanglement distribution over 1200 kilometers. Science, 356(6343), 1140–1144.

- NIST (2024). FIPS 203: Module-Lattice-Based Key-Encapsulation Mechanism Standard. National Institute of Standards and Technology.

- NIST (2024). FIPS 204: Module-Lattice-Based Digital Signature Standard. National Institute of Standards and Technology.

- NIST (2024). FIPS 205: Stateless Hash-Based Digital Signature Standard. National Institute of Standards and Technology.

- Castryck, W., & Decru, T. (2022). An efficient key recovery attack on SIDH. EUROCRYPT 2023, LNCS 14008.

- Regev, O. (2005). On lattices, learning with errors, random linear codes, and cryptography. STOC 2005, 84–93.
