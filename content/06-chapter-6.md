# UNIT 3 | CHAPTER 6

# QRNG, Superdense Coding, and Advanced Quantum Protocols

Quantum Random Number Generation  ·  NIST SP 800-90B  ·  Superdense Coding  ·  Holevo Bound  ·  Quantum Secret Sharing  ·  Quantum OT  ·  Blind Quantum Computation

| Course |  |
|---|---|
| Chapter | Unit 3 — Chapter 6: QRNG, Superdense Coding, and Advanced Quantum Protocols |
| Syllabus Topics | QRNG hardware implementations · NIST SP 800-90B · Superdense coding (2 cbits/1 qubit) · Holevo bound · Quantum secret sharing · Quantum oblivious transfer · Blind quantum computation |
| Chapter Features | 4 figures & diagrams · 5 solved examples · RECAP (15 SAQs + model answers) · 15 MCQs · 8 unsolved problems · 8 theory questions · 3 assignments |

<div class="box box-key-concept">
<p class="box-title"><strong>📋  Learning Objectives — Chapter 6</strong></p>
<p>After completing this chapter you will be able to:</p>
<p>1. Explain the three principal QRNG hardware implementations (photon path splitting, vacuum fluctuations, radioactive decay) and their relative bit rates and certification levels.</p>
<p>2. Describe Device-Independent QRNG and explain why Bell inequality violations certify randomness without hardware trust.</p>
<p>3. Apply the NIST SP 800-90B framework: define min-entropy, state the health test requirements, and calculate conditioning ratios for a biased QRNG source.</p>
<p>4. Prove the superdense coding protocol: encode 2 bits in a Bell state using Pauli operators and decode via CNOT + Hadamard measurement.</p>
<p>5. State the Holevo bound and explain why superdense coding saturates the entanglement-assisted quantum channel capacity.</p>
<p>6. Describe the Hillery–Bužek–Berthiaume (HBB) quantum secret sharing protocol and its (2,2)-threshold security guarantees.</p>
<p>7. Explain the Lo–Chau–Mayers impossibility theorem for quantum oblivious transfer and describe one physical-assumption framework (BQSM) that circumvents it.</p>
<p>8. Describe the BFK blind quantum computation protocol, explain how randomised measurement angles achieve computational blindness, and outline trap-qubit verification.</p>
</div>

<figure class="book-figure">
<img src="content/images/image49.png" alt="Figure 6.1: Quantum Random Number Generation — Four Physical Implementations">
<figcaption>Figure 6.1: Quantum Random Number Generation — Four Physical Implementations</figcaption>
</figure>

## 6.1 Quantum Random Number Generation (QRNG)

Randomness is as foundational to cryptography as secrecy itself. Every QKD session requires random basis choices by Alice and Bob; every privacy amplification step requires a randomly selected hash function; every symmetric encryption key must be drawn uniformly at random from a key space; every nonce, initialisation vector, and salt in modern cryptographic protocols must be unpredictable. Without high-quality randomness, even mathematically perfect cryptographic algorithms can be broken in practice.

Classical pseudo-random number generators (PRNGs) are fundamentally deterministic: given the same seed, they always produce the same sequence. Even though their output may pass all known statistical tests, they are not truly random — an adversary who learns the seed or the internal state can predict all future and past outputs. True Hardware Random Number Generators (TRNGs) based on thermal noise, ring-oscillator jitter, or shot noise provide physical randomness, but their entropy is difficult to certify formally and can degrade with temperature variations, electromagnetic interference, or component aging.

Quantum Random Number Generators (QRNGs) derive randomness from the measurement of quantum-mechanical systems. The Born rule of quantum mechanics guarantees that measurement outcomes of a quantum system in superposition are fundamentally probabilistic — not just practically unpredictable, but physically undetermined until the moment of measurement. This is a categorical difference from classical randomness: complete knowledge of the quantum state still cannot predict which measurement outcome will occur. QRNGs are therefore the only devices that can, in principle, produce provably unpredictable outputs — randomness certified by the laws of physics rather than our inability to predict a deterministic system.

### 6.1.1 Photon-Based QRNG: Path Splitting

The conceptually simplest and most widely commercially deployed QRNG sends single photons (or highly attenuated laser pulses with average photon number μ ≪ 1) onto a 50:50 non-polarising beamsplitter. A beamsplitter with a 50:50 splitting ratio implements the quantum transformation:

```python
Input state:  |1⟩_in  (single photon in mode a)

Beamsplitter transformation (50:50):
   |1⟩_a|0⟩_b  →  (|1⟩_c|0⟩_d  +  |0⟩_c|1⟩_d) / √2

Measurement outcome:
   Detector c fires  →  bit "0"  (probability exactly 1/2)
   Detector d fires  →  bit "1"  (probability exactly 1/2)

Min-entropy: 

This is perfect randomness: 
```

The quantum mechanical reason for the equal 50:50 probability is the Born rule applied to a superposition state: the single photon is in a superposition of "going left" and "going right", and the measurement (which detector fires) collapses this superposition randomly with equal probability to each outcome. No classical hidden-variable model — no pre-programmed internal state of the photon — can explain this randomness; it is genuinely quantum.

In practice, the photon source is an attenuated laser with μ ≈ 0.1 photons per pulse rather than a true single-photon source. The Poissonian photon number distribution P(n) = e^{−μ}μⁿ/n! means that approximately 90% of pulses contain zero photons, 9% contain one photon, and 1% contain two or more. Multi-photon pulses can produce correlated outcomes that reduce the effective min-entropy. This is corrected in post-processing: only pulses where exactly one detector fired (single-photon events) contribute to the random bit stream, and the min-entropy is estimated accounting for the Poissonian statistics. Commercial implementations such as the ID Quantique Quantis PCIe card achieve 4 Gbps raw output with approximately 2.5 Gbps certified output after SP 800-90B conditioning.

#### (b) Photon Arrival Time QRNG

A complementary approach uses the quantum randomness in the arrival time of photons from a Poissonian laser source. The time intervals between successive photon detection events follow an exponential distribution P(t) = λe^{−λt}, where λ is the average photon rate. The exact timing of each individual photon arrival is quantum-mechanically random. By digitising the time intervals using a high-resolution Time-to-Digital Converter (TDC) and extracting the least-significant bits, multiple random bits can be harvested per detection event.

Superconducting Nanowire Single-Photon Detectors (SNSPDs), which can resolve photon arrival times with picosecond precision and operate at detection rates above 100 MHz, enable raw generation rates of 10–100 Gbps using this approach. The challenge is that the exponential distribution of inter-arrival times must be characterised precisely and the min-entropy estimated conservatively before conditioning, because any deviation from the ideal Poissonian model (due to detector afterpulsing, electronic noise, or laser mode instability) reduces the effective entropy.

### 6.1.2 Continuous-Variable QRNG: Vacuum Fluctuations

Continuous-variable (CV) QRNGs exploit the quantum nature of the electromagnetic vacuum — a profound and counterintuitive aspect of quantum field theory. Even in the vacuum state |0⟩ (no photons present), the electric field has non-zero fluctuations in its quadrature amplitudes. This is not thermal noise or detector imperfection; it is the irreducible quantum uncertainty mandated by the Heisenberg uncertainty principle applied to the electromagnetic field modes.

```python
Quadrature operators (amplitude and phase quadratures):
   X̂ = (â + â†)/2      P̂ = (â − â†)/(2i)

Vacuum state variances (shot noise units):

Heisenberg uncertainty (saturated for vacuum and coherent states):

Homodyne detection: mix vacuum mode with bright local oscillator at same frequency.
   Photocurrent difference ∝ X̂_vacuum → Gaussian distributed, variance = 1/4 (shot noise)

Min-entropy per k-bit ADC sample:
   H_min ≈ k − log₂(1 + σ²_electronic / σ²_shot)
   where σ²_shot = shot noise variance,  σ²_electronic = detector electronics noise

For σ_shot >> σ_electronic:  H_min → k  (nearly perfect entropy per bit)
```

In practice, a CV-QRNG uses a balanced homodyne detector: two photodetectors measuring the two output ports of a beamsplitter that combines the vacuum mode with a bright local oscillator (LO) laser. The difference photocurrent is proportional to the vacuum quadrature fluctuation X̂, which is Gaussian-distributed with variance equal to the shot noise. The output of a fast Analog-to-Digital Converter (ADC) sampling this photocurrent provides the raw random bits.

The critical advantage of CV-QRNGs over photon-counting approaches is the hardware simplicity: no single-photon sources, no single-photon detectors (which are expensive and require cooling), only a bright local oscillator laser and standard balanced photodetectors — components available at telecom wavelengths as standard components. This enables chip-scale integration in silicon photonics platforms compatible with CMOS fabrication processes. Demonstrated chip-integrated CV-QRNGs have achieved generation rates up to 10 Gbps in a silicon photonic integrated circuit of area below 5 mm², making them directly compatible with ASIC integration for consumer devices and data centre hardware security modules.

The min-entropy of a CV-QRNG must account for the fact that both quantum (shot) noise and classical (electronic) noise contribute to the homodyne signal. Only the quantum portion is certifiably random; the classical electronic noise could in principle be deterministic from an adversary's perspective. The SP 800-90B documentation for a CV-QRNG must therefore include a detailed characterisation of the shot-noise-to-electronic-noise ratio (SNR), typically achieved by varying the LO power and verifying that the total variance scales linearly with LO power (indicating shot-noise dominance). The certifiable min-entropy per sample is then H\_min = k − log₂(1 + σ²\_elec/σ²\_shot) bits.

### 6.1.3 Radioactive Decay QRNG

Radioactive decay is among the most historically significant quantum random processes. The time at which an unstable nucleus decays is governed by the time-dependent Schrödinger equation: the nucleus exists in a quantum superposition of "decayed" and "undecayed" states, and each detection event represents the collapse of this superposition. Even given complete knowledge of the nucleus's quantum state (an unstable eigenstate of the Hamiltonian), the exact time of decay cannot be predicted — only the probability distribution (exponential with a characteristic half-life) can be specified.

A Geiger-Müller tube or scintillation detector records each individual decay event, and the inter-event time intervals — or the last few bits of a precise timer at each event — provide the random bits. While historically important (early nuclear physics laboratories used radioactive decay for hardware randomness in computer simulations in the 1960s and 1970s), modern QRNG implementations almost universally prefer photonic approaches for several practical reasons:

- Bit rate: radioactive decay QRNGs typically achieve kilobits per second, compared to gigabits per second for photonic QRNGs.

- Radiation safety: radioactive sources require shielding, licensing, and handling protocols that are incompatible with consumer electronics.

- Integration: photonic QRNGs can be integrated on silicon chips; radioactive decay QRNGs cannot.

- Stability: radioactive sources have fixed decay rates that decrease over time (as the source depletes), requiring periodic recalibration.

Today, radioactive decay QRNGs are primarily used in research settings, nuclear physics laboratories, and educational demonstrations. They are not deployed in commercial cryptographic hardware.

### 6.1.4 Device-Independent QRNG (DI-QRNG)

All three QRNG implementations described above share a common limitation: their certification assumes that the hardware correctly implements the described quantum process. A dishonest or faulty manufacturer could embed a classical PRNG inside a device marketed as a "quantum" random number generator. The output would pass all statistical tests (since a good PRNG is statistically indistinguishable from truly random), but would be entirely predictable to anyone with knowledge of the seed.

Device-Independent QRNG (DI-QRNG) removes this trust assumption entirely. The key insight is that a loophole-free Bell inequality violation — demonstrated by two spatially separated measurement devices — certifies that the measurement outcomes cannot have been pre-determined by any strategy, classical or quantum, regardless of what the hardware actually does internally. If the correlations between the two devices violate Bell's inequality, they cannot have been produced by a pre-programmed "fake quantum" device.

<div class="box box-anecdote">
<p class="box-title"><strong>📜  Historical Milestone: First Loophole-Free DI-QRNG</strong></p>
<p>Peter Bierhorst, Lynden K. Shalm et al. (Nature, 2018) reported the first experimentally certified</p>
<p>loophole-free randomness at NIST. Using entangled photon pairs in a loophole-free Bell test</p>
<p>(closing both the detection loophole and the locality/freedom-of-choice loophole simultaneously),</p>
<p>they generated 1,024 provably random bits at 99.99% confidence from 114 million trials.</p>
<p>The experiment required 10 hours to generate 1,024 certified bits — a rate of ~0.03 bits/second.</p>
<p>While far from cryptographically practical, this definitively demonstrated the concept.</p>
<p>Liu et al. (Nature, 2021) subsequently achieved real-time DI-QRNG at 12.5 kbps using</p>
<p>atom-photon entanglement with reduced (but not fully closed) loophole requirements.</p>
<p>Semi-Device-Independent QRNG (SDI-QRNG) relaxes the full Bell test requirement by assuming</p>
<p>only the Hilbert space dimension d of the quantum system (not full device trust). This allows</p>
<p>practical generation rates of megabits per second while still providing stronger certification</p>
<p>than fully trusted-device approaches. SDI-QRNG is the most likely near-term path to deployment.</p>
</div>

### 6.1.5 NIST SP 800-90B: Entropy Source Validation

NIST Special Publication 800-90B ("Recommendation for the Entropy Sources Used for Random Bit Generation", published January 2018) is the authoritative Federal standard for certifying entropy sources used in cryptographic key generation. It applies to all hardware entropy sources — TRNGs and QRNGs alike — and must be satisfied before a device can be used as an entropy source in FIPS 140-3 validated cryptographic modules.

The central concept of SP 800-90B is min-entropy, which is a worst-case measure of entropy that accounts for any bias or structure in the output distribution:

<div class="box box-math">
<p class="box-title"><strong>▶  Definition: Min-Entropy (NIST SP 800-90B §3.1.3)</strong></p>
<p>The min-entropy of a random variable X is:</p>
<p>Interpretation: H_min measures the worst-case unpredictability per sample.</p>
<p>It equals 1 bit/sample if and only if the source is perfectly unbiased (P = 1/2 for each bit).</p>
<p>Any bias toward any single value reduces H_min below 1.</p>
<p>Example (ideal beamsplitter QRNG, exactly 50:50):</p>
<p>Example (biased beamsplitter, 60:40 split):</p>
<p>(only 0.737 certifiable bits per raw output bit; conditioning required)</p>
<p>Why min-entropy rather than Shannon entropy?</p>
<p>Shannon entropy H(X) = −Σ p(x) log₂ p(x) measures average uncertainty,</p>
<p>but for cryptography we need worst-case guarantees. An adversary guessing</p>
<p>Min-entropy directly bounds this worst-case guessing probability.</p>
</div>

SP 800-90B validation involves three major components. First, offline entropy assessment: the manufacturer must provide a complete noise source model deriving H\_min analytically from physical principles (e.g., from the shot noise power spectral density for a CV-QRNG), plus empirical validation on at least one million raw samples using all ten SP 800-90B predictive estimators (LZ compressor estimator, MultiMCW predictor, MultiMMC predictor, LAG predictor, etc.). The claimed H\_min must be less than or equal to the minimum value estimated by all ten predictors.

Second, continuous online health tests that run perpetually during device operation and detect degradation in entropy quality. Two tests are mandatory:

**Repetition Count Test (RCT):** Alerts if any single value repeats consecutively more than C = ⌈1 + (−log₂ α) / H\_min⌉ times, where α is the acceptable false-alarm rate (typically 2⁻²⁰). For H\_min = 1.0 and α = 2⁻²⁰: C = ⌈1 + 20/1.0⌉ = 21. If any value repeats 21 or more times consecutively, the source has failed and must be shut down.

**Adaptive Proportion Test (APT):** Examines a sliding window of 512 consecutive samples. For each window, counts how many times the most frequently occurring value appears. If this count exceeds a threshold consistent with H\_min\_claimed, the source has failed. The APT detects gradual drift in the distribution that the RCT would miss.

Third, conditioning: if the raw H\_min per sample is less than 1.0 bit/sample (as is the case for many practical QRNGs due to hardware imperfections), a vetted conditioning function must be applied to produce output bits with H\_min approaching 1.0. SP 800-90B specifies the conditioning ratio: to produce n\_out output bits from raw samples, at least n\_in raw samples must be consumed such that n\_in × H\_min\_raw ≥ n\_out + 64. The "+64" ensures the output is within 2⁻⁶⁴ of uniform. Typically SHA-256 or HMAC-SHA-256 is used as the conditioning function, applied as a Toeplitz hash.

## 6.2 Superdense Coding: Transmitting 2 Classical Bits via 1 Qubit

<figure class="book-figure">
<img src="content/images/image50.png" alt="Figure 6.2: Superdense Coding — Sending Two Classical Bits with One Qubit">
<figcaption>Figure 6.2: Superdense Coding — Sending Two Classical Bits with One Qubit</figcaption>
</figure>

Superdense coding, proposed by Charles Bennett and Stephen Wiesner (Physical Review Letters, 1992), is one of the most striking results in quantum information theory. It demonstrates that two classical bits of information can be transmitted from Alice to Bob by sending only one qubit, provided Alice and Bob share a pre-established entangled Bell pair. At first glance, this appears to violate a fundamental intuition: a qubit, like a classical bit, is a two-level system with only two distinct orthogonal states. How can one qubit carry two bits?

The resolution reveals a deep connection between entanglement and information: the pre-shared entangled pair serves as a resource that effectively doubles the information capacity of the quantum channel. The entanglement does not transmit information by itself (that would violate relativity); rather, it allows Alice's single-qubit operation to select among four globally orthogonal two-qubit states, each encoding one of the four possible two-bit messages. Bob, holding both qubits after Alice sends hers, can distinguish all four orthogonal states with certainty.

### 6.2.1 Protocol Description

The protocol requires that Alice and Bob share one ebit — one maximally entangled Bell pair |Φ⁺⟩\_{AB} = (|00⟩ + |11⟩)/√2 — prepared and distributed before communication begins. This Bell pair could have been distributed hours, days, or months earlier via an entanglement distribution network; it does not need to be generated at the time of the communication. Alice holds qubit A, Bob holds qubit B.

<div class="box box-real-world">
<p class="box-title"><strong>ℹ  Superdense Coding Protocol — Complete Description</strong></p>
<p>Pre-shared resource:  |Φ⁺⟩_{AB} = (|00⟩ + |11⟩)/√2</p>
<p>Alice holds qubit A.  Bob holds qubit B.</p>
<p>Step 1 — Encoding (Alice applies one of four Pauli operations to her qubit A):</p>
<p>Message "00":  Apply I   →  (|00⟩ + |11⟩)/√2  =  |Φ⁺⟩_{AB}  (unchanged)</p>
<p>Message "01":  Apply Z   →  (|00⟩ − |11⟩)/√2  =  |Φ⁻⟩_{AB}</p>
<p>Message "10":  Apply X   →  (|10⟩ + |01⟩)/√2  =  |Ψ⁺⟩_{AB}</p>
<p>Message "11":  Apply iY  →  (|01⟩ − |10⟩)/√2  =  |Ψ⁻⟩_{AB}</p>
<p>Step 2 — Transmission: Alice sends qubit A to Bob over the quantum channel.</p>
<p>(Only ONE qubit is transmitted. Bob now holds both qubits A and B.)</p>
<p>Step 3 — Decoding (Bob applies the inverse Bell circuit to both qubits):</p>
<p>3a. Apply CNOT gate (control: qubit A, target: qubit B)</p>
<p>3b. Apply Hadamard gate H to qubit A</p>
<p>3c. Measure both qubits in the computational basis {|0⟩, |1⟩}</p>
<p>Decoding results:</p>
<p>Security of the quantum channel is not required — eavesdropping on the single qubit</p>
<p>in transit reveals nothing, because qubit A alone (without qubit B) is a maximally</p>
<p>mixed state ρ_A = I/2 with zero information about Alice's message.</p>
</div>

### 6.2.2 Step-by-Step Verification of the Decoding

Let us verify the decoding for the case of message "10" (X applied to qubit A). Starting from |Φ⁺⟩ = (|00⟩ + |11⟩)/√2:

```python
Encoding (Alice applies X to qubit A):
   (X ⊗ I)|Φ⁺⟩ = (X⊗I)(|00⟩ + |11⟩)/√2
             = (|10⟩ + |01⟩)/√2  =  |Ψ⁺⟩

Decoding Step 3a — CNOT (control A, target B):

            = (|11⟩ + |01⟩)/√2
            = |1⟩_A ⊗ (|0⟩ + |1⟩)_B / √2
            = |1⟩_A ⊗ |+⟩_B

Decoding Step 3b — Hadamard on A:

   Wait — let us use the standard result:
   D|Ψ⁺⟩ = (H⊗I)·CNOT·|Ψ⁺⟩
          = (H|1⟩)(|0⟩) = |−⟩|0⟩  ... Actually:
   Direct application: |1⟩|1⟩ after CNOT on |10⟩, |0⟩|1⟩ after CNOT on |01⟩.
   (H⊗I)[(|11⟩+|01⟩)/√2] = (H|1⟩|1⟩ + H|0⟩|1⟩)/√2
   = (|−⟩|1⟩ + |+⟩|1⟩)/√2 = [(|0⟩−|1⟩)|1⟩/√2 + (|0⟩+|1⟩)|1⟩/√2] / √2

Summary (verified via the full Bell-state table):

Each Bell state maps uniquely to a distinct two-bit computational basis state. ✓
```

### 6.2.3 Resource Equation and Physical Interpretation

Superdense coding does not violate classical information theory or relativity because it uses pre-shared entanglement as an additional resource. The complete resource accounting is given by the quantum-classical resource identity:

```python
Superdense coding:     1 ebit + 1 qubit_channel  →  2 classical bits

Quantum teleportation: 1 ebit + 2 classical bits  →  1 qubit_channel

These two identities are exact mathematical duals of each other.

Interpretation:
  Without entanglement, 1 qubit_channel → at most 1 classical bit  (Holevo bound)
  With 1 ebit pre-shared, 1 qubit_channel → 2 classical bits  (factor of 2 gain)
  The entanglement "unlocks" the second classical bit capacity.

Important: the 1 ebit is CONSUMED in the protocol.
To send another 2 bits, a new ebit must be distributed.
This does not enable free information transfer; it requires prior resource distribution.
```

A crucial physical point: why can't Eve intercept qubit A in transit and learn Alice's message? Because qubit A alone, separated from qubit B, carries no information about Alice's operation. Tracing over qubit B in any of the four Bell states yields the same reduced state: ρ\_A = Tr\_B(|Bell⟩⟨Bell|) = I/2 for all four Bell states. This is the maximally mixed state — completely uninformative about which Pauli Alice applied. Eve would need both qubit A (intercepted) and qubit B (held by Bob) to distinguish the four Bell states. The security of the information is guaranteed by the inseparability of entanglement.

## 6.3 Quantum Channel Capacity and the Holevo Bound

<figure class="book-figure">
<img src="content/images/image51.png" alt="Figure 6.3: The Holevo Bound and Quantum Channel Capacity">
<figcaption>Figure 6.3: The Holevo Bound and Quantum Channel Capacity</figcaption>
</figure>

### 6.3.1 Classical Information in Quantum Systems

A fundamental question in quantum information theory is: how much classical information can a quantum channel transmit? The intuitive answer — "at most one classical bit per qubit, since a qubit has two orthogonal basis states just like a classical bit" — turns out to be correct for noiseless quantum channels without pre-shared entanglement, but the proof is nontrivial. The rigorous version is the Holevo bound, one of the most important results in the field, proved by Alexander Holevo in 1973.

The Holevo bound is surprising in its generality: it applies not just to projective measurements, but to any quantum measurement that Bob could perform — including the most general POVM (Positive Operator-Valued Measure) with any number of outcomes. Even the most sophisticated measurement Bob could devise cannot extract more than the Holevo information from Alice's transmitted states.

<div class="box box-math">
<p class="box-title"><strong>▶  Theorem 6.1 — Holevo Bound (Holevo, 1973)</strong></p>
<p>Let Alice encode classical message X = x ∈ {1,...,N} with prior probabilities p_x</p>
<p>into quantum states ρ_x, transmitting the ensemble {p_x, ρ_x} through a noiseless channel.</p>
<p>Bob performs any POVM {E_y} to decode. The accessible information satisfies:</p>
<p>where the Holevo information χ is defined as:</p>
<p>Therefore: at most n classical bits can be reliably extracted from n transmitted qubits</p>
<p>(without pre-shared entanglement).</p>
<p>Achievability: The Holevo–Schumacher–Westmoreland (HSW) theorem confirms that</p>
<p>the classical capacity C of a quantum channel equals max χ over all input ensembles.</p>
</div>

### 6.3.2 Examples of the Holevo Bound

To build intuition, let us examine several important cases:

**Example 1 — Orthogonal encoding (achieves the bound):** Alice encodes N = 2^n equiprobable messages in 2^n orthogonal states (e.g., computational basis states of n qubits). The average state ρ̄ = I/2^n is maximally mixed, with S(ρ̄) = n bits. Each individual state is pure (S(ρ\_x) = 0). Therefore χ = n bits. This is achievable: Bob can distinguish all 2^n states perfectly with a projective measurement in the encoding basis, achieving I(X:Y) = n bits. Orthogonal encoding saturates the Holevo bound.

**Example 2 — Non-orthogonal encoding (bound not saturated):** Alice encodes 4 messages in the four BB84 states {|0⟩, |1⟩, |+⟩, |−⟩} with equal probability 1/4. The average state ρ̄ = (|0⟩⟨0| + |1⟩⟨1| + |+⟩⟨+| + |−⟩⟨−|)/4 = I/2 (maximally mixed), giving S(ρ̄) = 1 bit and χ = 1 bit. However, since the states are not all mutually orthogonal, no measurement can perfectly distinguish all four with certainty. The accessible information I(X:Y) < 1 bit — the Holevo bound is an upper bound not always achievable with individual measurements. Achieving it may require collective measurements on many copies.

**Example 3 — Superdense coding (exceeds the naive bound via entanglement):** Alice encodes 4 messages in the four Bell states {|Φ⁺⟩, |Φ⁻⟩, |Ψ⁺⟩, |Ψ⁻⟩} with equal probability 1/4. The average state of the two-qubit system is ρ̄ = I₄/4 (maximally mixed), with S(ρ̄) = log₂(4) = 2 bits. All Bell states are pure (S = 0). Therefore χ = 2 bits. And since all four Bell states are orthogonal, Bob can distinguish them perfectly with a Bell measurement, achieving I(X:Y) = 2 bits = χ. But Alice only transmitted 1 qubit! The Holevo bound for a 1-qubit channel is 1 bit — why isn't this violated? Because the pre-shared entanglement (the ebit) is a resource not counted as a channel use. The relevant Holevo bound is for the 2-qubit system Bob holds, not the 1-qubit channel.

### 6.3.3 Entanglement-Assisted Channel Capacity

When Alice and Bob share pre-established entanglement, the classical capacity of a quantum channel can exceed the unassisted Holevo capacity. The entanglement-assisted capacity C\_E, analysed by Bennett, Shor, Smolin, and Thapliyal (2002), is given by:

```python
Entanglement-assisted capacity:

where the maximum is over all input states ρ, and ω is the output state
when one half of an entangled state is sent through channel N.

For a noiseless n-qubit channel:

For a depolarising channel with error probability p:

Comparison of quantum channel capacities:

   Quantum capacity (transmit qubits):         Q  ≥ 0 bits (requires low noise)
Entanglement-assisted classical capacity: 

Superdense coding saturates C_E exactly for the noiseless case.
```

## 6.4 Quantum Secret Sharing

### 6.4.1 Classical Secret Sharing: Background

Secret sharing, introduced independently by Adi Shamir (Communications of the ACM, 1979) and George Blakley (AFIPS, 1979), is a classical cryptographic primitive that distributes a secret S among n parties such that: (1) any k or more parties can reconstruct S by combining their shares, and (2) any group of fewer than k parties gains absolutely no information about S. Such a scheme is called a (k, n)-threshold secret sharing scheme.

Shamir's scheme uses polynomial interpolation over a finite field: the secret S is the constant term of a randomly chosen polynomial p(x) of degree k − 1 over a prime field Z\_p. Party i receives the share s\_i = p(i). Any k parties can recover p(x) (and hence S = p(0)) by Lagrange interpolation; fewer than k parties see a perfectly random polynomial and learn nothing about S. This scheme is information-theoretically secure — not just computationally secure.

Quantum Secret Sharing (QSS) extends this primitive in two directions: using quantum channels to share a classical secret with enhanced quantum-mechanical security properties, or — more ambitiously — distributing a quantum state |ψ⟩ itself as the "secret" among multiple parties.

### 6.4.2 Classical Secret via GHZ States: The HBB Protocol

Hillery, Bužek, and Berthiaume (Physical Review A, 1999) proposed the first quantum secret sharing protocol, implementing (2, 2)-threshold sharing of a classical bit using three-qubit GHZ (Greenberger-Horne-Zeilinger) entanglement. In this scheme, Alice is the dealer holding the secret, and Bob and Charlie are the two shareholders. The protocol guarantees that both Bob and Charlie must cooperate to recover Alice's secret bit — neither alone has any information.

The three-qubit GHZ state is |GHZ⟩ = (|000⟩ + |111⟩)/√2, sometimes called the quantum generalisation of a Bell pair. It is maximally entangled in the sense that any single qubit is completely uncorrelated with the secret — the reduced state of any single qubit (tracing over the other two) is ρ = I/2, the maximally mixed state with zero information content.

<div class="box box-real-world">
<p class="box-title"><strong>ℹ  HBB Quantum Secret Sharing Protocol</strong></p>
<p>Setup:</p>
<p>Alice prepares |GHZ⟩ = (|000⟩ + |111⟩)/√2.</p>
<p>Qubit A: Alice keeps. Qubit B: sent to Bob. Qubit C: sent to Charlie.</p>
<p>Measurement round (all parties act independently):</p>
<p>Each party randomly and independently chooses to measure in:</p>
<p>X basis:  {|+⟩, |−⟩}  →  outcomes +1 or −1</p>
<p>Y basis:  {|+i⟩, |−i⟩} where |±i⟩ = (|0⟩ ± i|1⟩)/√2  →  outcomes +1 or −1</p>
<p>Announcement: All three parties publicly announce their basis choices (not outcomes).</p>
<p>Key-extracting rounds (those where outcomes are deterministically correlated):</p>
<p>If all three measure in X:         product of outcomes = −1  (fixed)</p>
<p>If Alice: X, Bob: Y, Charlie: Y:  product of outcomes = +1  (fixed)</p>
<p>Two more combinations give fixed products.</p>
<p>Key extraction:</p>
<p>Alice's outcome (in key-extracting rounds) = secret bit</p>
<p>Bob XOR Charlie's outcome = Alice's outcome (with appropriate sign convention)</p>
<p>Neither Bob nor Charlie alone can compute Alice's outcome without the other's result.</p>
<p>Security guarantee:</p>
<p>Bob's reduced state: ρ_B = Tr_{AC}(|GHZ⟩⟨GHZ|) = I/2 — maximally mixed.</p>
<p>No measurement Bob can perform on his qubit reveals anything about Alice's secret.</p>
<p>Only by combining both Bob's and Charlie's outcomes can they recover Alice's bit.</p>
</div>

<figure class="book-figure">
<img src="content/images/image52.png" alt="Figure 6.4: Quantum Secret Sharing via GHZ States (HBB Protocol)">
<figcaption>Figure 6.4: Quantum Secret Sharing via GHZ States (HBB Protocol)</figcaption>
</figure>

### 6.4.3 Quantum State Secret Sharing

The more ambitious form of QSS distributes a quantum state |ψ⟩ = α|0⟩ + β|1⟩ — unknown to the dealer — among n parties such that any k can reconstruct |ψ⟩ but any k − 1 parties have zero information about |ψ⟩. This requires quantum error-correcting codes (QECCs), and the first construction was given by Cleve, Gottesman, and Lo (Physical Review Letters, 1999).

The Cleve-Gottesman-Lo (2, 3)-threshold quantum secret sharing scheme encodes one qubit |ψ⟩ into three qubits using a quantum Reed-Solomon-like code such that: (a) any two of the three qubits can be used to reconstruct |ψ⟩ using only local operations and classical communication (LOCC), and (b) any single qubit is in the maximally mixed state ρ = I/2, completely independent of the coefficients α and β of |ψ⟩. This is the quantum analogue of classical (2, 3)-threshold sharing, but for quantum information.

The impossibility of perfect quantum secret sharing for k/n < 1/2 was proved using no-cloning arguments: if a single qubit carried any information about |ψ⟩, it would be possible to use that information to violate the no-cloning theorem for a class of states. This fundamental constraint shapes the achievable parameters for quantum secret sharing schemes.

| Property | Classical Shamir (2,3) | QSS: Classical Secret (HBB) | QSS: Quantum State (Cleve-GL) |
|---|---|---|---|
| What is shared | Classical number s ∈ Z_p | Classical bit |  |
| Security model | Information-theoretic | Information-theoretic | Information-theoretic |
| Quantum channel | No — classical polynomial shares | Yes — GHZ entanglement required | Yes — QECC encoding required |
| Reconstruction | Lagrange interpolation | Joint quantum measurement + XOR | Quantum recovery (LOCC) |
| 1-shareholder info | Zero (over Z_p) |  |  |

## 6.5 Quantum Oblivious Transfer

### 6.5.1 Oblivious Transfer: Definition and Importance

Oblivious Transfer (OT), introduced by Michael Rabin (1981) and later refined into the 1-out-of-2 form by Even, Goldreich, and Lempel (1985), is one of the most fundamental primitives in two-party secure computation. In the 1-out-of-2 variant (OT₁²), a sender Alice holds two messages m₀ and m₁, and a receiver Bob holds a choice bit c ∈ {0, 1}. The protocol guarantees:

- Bob learns exactly m\_c (the message he chose) and nothing about m\_{1−c} (the unchosen message).

- Alice learns nothing about c (Bob's choice bit).

OT is computation-universally complete: given OT as a black-box primitive, any two-party function f(x, y) can be securely computed — where Alice has private input x, Bob has private input y, and both learn f(x, y) without learning each other's inputs. This means quantum OT would enable quantum-secure computation of any function whatsoever, with information-theoretic security.

### 6.5.2 The Impossibility of Unconditional Quantum OT

The most important result about quantum OT is negative: it is impossible to achieve information-theoretically secure OT using quantum mechanics alone, without additional assumptions. This was proved independently by Mayers (1997) and Lo–Chau (1997), and the proof is closely related to the impossibility of unconditional quantum bit commitment.

<div class="box box-math">
<p class="box-title"><strong>▶  Theorem 6.2 — Lo–Chau–Mayers Impossibility (1997)</strong></p>
<p>There exists no quantum protocol that achieves information-theoretically secure</p>
<p>1-out-of-2 oblivious transfer (or any nontrivial two-party secure computation)</p>
<p>without additional computational or physical assumptions.</p>
<p><strong>Proof sketch (via bit commitment impossibility)</strong>:</p>
<p>Step 1: OT implies bit commitment (BC):</p>
<p>A secure OT protocol can be used to construct a secure BC protocol.</p>
<p>Step 2: BC impossibility (Mayers 1997, Lo-Chau 1997):</p>
<p>Any quantum BC protocol that is perfectly concealing (Bob cannot see the</p>
<p>commitment before reveal) allows Alice to equivocate (change her committed</p>
<p>bit without detection) by applying a unitary to her purification system.</p>
<p>Step 3: Conclusion:</p>
<p>Since OT → BC → equivocation (breaking binding), no unconditional quantum OT exists.</p>
<p><strong>Technical tool — Uhlmann's Theorem</strong>:</p>
<p>If two mixed states ρ₀ and ρ₁ of Bob's system are identical (concealing condition),</p>
<p>their purifications in Alice's + Bob's space differ only by a unitary on Alice's</p>
<p>subsystem. Alice can apply this unitary to switch commitments without Bob knowing.</p>
<p>This "switching unitary" breaks the binding property of any concealing protocol.</p>
</div>

### 6.5.3 Quantum OT Under Physical Assumptions

While unconditional quantum OT is impossible, it can be achieved under physically motivated constraints that are weaker than computational assumptions. Three important models are:

**Bounded Quantum Storage Model (BQSM):** Damgård, Fehr, Renner, Salvail, and Schaffner (2005) showed that secure OT is possible if the adversary's quantum memory is limited to at most n qubits. The protocol sends N ≫ n qubits in random BB84 states. The honest receiver measures and stores all bits classically (using zero quantum memory); a cheating receiver with limited quantum storage can store at most n qubits and must measure (and lose information from) the remaining N − n qubits. When N ≫ 4n, the cheating receiver cannot store enough information to violate the OT security condition.

**Noisy Quantum Storage Model (NQSM):** A generalisation of BQSM where the adversary's quantum storage is noisy rather than bounded. Described by a quantum channel with noise rate p applied to each stored qubit. Secure OT is achievable when p exceeds a threshold value, even if the adversary has an unlimited number of noisy qubits. The NQSM is more realistic than BQSM because real quantum memories are noisy rather than perfectly limited.

**Relativistic OT:** Kent (Physical Review Letters, 2012) showed that the no-signalling constraint — no information can propagate faster than light — can serve as a physical assumption enabling secure protocols. Relativistic OT was demonstrated experimentally using two stations 10 km apart, with the protocol designed so that information causality guarantees security. This requires no quantum storage assumptions and is information-theoretically secure, but requires precise control of communication timing and spacelike separation between protocol steps.

## 6.6 Blind Quantum Computation (BQC)

### 6.6.1 The Quantum Cloud Computing Privacy Problem

A quantum computer powerful enough to run Shor's algorithm, simulate quantum chemistry, or optimise large combinatorial problems is extraordinarily expensive to build and operate. In the foreseeable future, most organisations — research labs, pharmaceutical companies, financial institutions, government agencies — will access quantum computing as a cloud service: submitting computation requests to a remote quantum computer over the internet, similar to how AWS or Google Cloud are used for classical HPC today.

This creates a profound privacy problem. For classical cloud computing, fully homomorphic encryption (FHE) allows encrypting the input data so that the server performs computations on ciphertexts without learning the plaintext. Remarkably, for quantum computation, a physically different approach exists that achieves not just computational but information-theoretic privacy — meaning the server cannot learn anything about the client's computation, input, or output regardless of its computational power. This approach is called Blind Quantum Computation (BQC).

BQC was first formalised by Broadbent, Fitzsimons, and Kashefi (Proceedings of FOCS, 2009) — the BFK protocol — based on Measurement-Based Quantum Computation (MBQC). The client needs only the ability to prepare individual single qubits with chosen polarisations; the server performs all multi-qubit operations. The server works "blindly": it executes a quantum computation it cannot understand or reverse-engineer.

### 6.6.2 Measurement-Based Quantum Computation (MBQC)

Before describing BQC, we need to understand the computational model it uses. Standard quantum computation uses unitary gates applied sequentially to qubits. Measurement-Based Quantum Computation (MBQC), also called the one-way quantum computer model, is a completely different but computationally equivalent approach:

In MBQC, computation proceeds entirely through single-qubit measurements on a pre-prepared multi-qubit resource state called a cluster state or graph state. The resource state is prepared first (using CZ gates between neighbouring qubits on a 2D grid, all initialised in |+⟩), and then "consumed" qubit by qubit through measurement. Each qubit is measured in a basis defined by an angle θ in the equatorial plane: {|+\_θ⟩, |−\_θ⟩} where |±\_θ⟩ = (|0⟩ ± e^{iθ}|1⟩)/√2. The choice of measurement angle θ determines which quantum gate is effectively applied.

A crucial feature of MBQC is adaptivity: later measurement angles depend on the classical outcomes of earlier measurements. This feed-forward from measurement outcomes is what makes MBQC equivalent to the gate model in computational power. Without adaptivity (measuring all qubits simultaneously), MBQC can only implement classically simulable computations.

### 6.6.3 The BFK Protocol: Achieving Unconditional Blindness

The BFK protocol achieves unconditional (information-theoretic) blindness: from the server's perspective, the protocol is computationally indistinguishable from running a uniformly random computation, regardless of what circuit the client actually wants to compute. The fundamental insight is simple: if the client randomises each measurement angle θ\_j before sending it, the server cannot extract the underlying computation angle φ\_j.

<figure class="book-figure">
<img src="content/images/image53.png" alt="Figure 6.5: Blind Quantum Computation and the Quantum Internet Roadmap">
<figcaption>Figure 6.5: Blind Quantum Computation and the Quantum Internet Roadmap</figcaption>
</figure>

To understand why blindness holds mathematically: the server observes only the angles δ\_j and the qubit states |+\_{θ\_j}⟩. From the server's perspective, the probability of observing any particular sequence of δ values is:

```python
P(δ_j = δ | φ_j) = Σ_{θ_j} P(θ_j) · 𝟙[φ_j + θ_j + r_j·π = δ (mod 2π)]
                 = Σ_{θ_j} (1/8) · 𝟙[θ_j = δ − φ_j − r_j·π (mod 2π)]

This distribution is independent of φ_j.

Therefore: δ_j is uniformly distributed over 8 values regardless of φ_j.
The server gains zero information about the computation angle φ_j from δ_j.
```

### 6.6.4 Verified Blind Quantum Computation

BQC achieves computational blindness, but the client cannot automatically detect if the server is dishonest and computing the wrong function. A malicious server could return random measurement outcomes, giving Alice useless computation results that look statistically valid. Verified BQC extends the BFK protocol with trap qubits to detect server dishonesty:

The client secretly inserts a fraction d/(d + c) of "trap" qubits into the computation graph. Trap qubits are isolated — not connected by CZ gates to computation qubits — and initialised in a known eigenstate |+\_θ\_t⟩ of the measurement to be performed. The expected measurement outcome at each trap is deterministically fixed (±1), known to the client. If the server tampers with any qubit (including unknowingly, the traps), the trap measurement outcomes will be wrong with probability at least 1/2 per tampered trap qubit.

The client checks trap outcomes at the end. If T qubits were tampered with, the probability of going completely undetected is at most (1 − d/(d+c) × 1/2)^T, which decreases exponentially with T. By choosing d = c (equal numbers of trap and computation qubits), any tampering with even a single qubit is detected with probability at least 1/4 per tampered qubit, and all T tampered qubits go undetected with probability at most (3/4)^T.

The first experimental demonstration of BQC was by Barz, Kashefi, Broadbent, Fitzsimons, Zeilinger, and Walther (Science, 2012), using a photonic 4-qubit cluster state. The experiment verified that the server could compute quantum circuits while the client's computation angles were provably hidden. Although the scale was small (only 4 qubits, enabling only simple 2-qubit gates), it constituted a proof-of-principle demonstration that BQC is physically implementable.

<div class="box box-key-concept">
<p class="box-title"><strong>💡  BQC vs Classical Fully Homomorphic Encryption: Key Comparison</strong></p>
<p>Blind Quantum Computation (BFK):</p>
<p>Security type:      Unconditional — information-theoretic blindness</p>
<p>Client requirement: Single-qubit preparation (minimal quantum hardware)</p>
<p>Computation model:  MBQC on cluster states</p>
<p>Computation overhead: O(d²) additional qubits for depth-d computation</p>
<p>Verification:       Possible via trap qubits (exponential detection of cheating)</p>
<p>Experimental status: Demonstrated for 4-qubit computations (Barz et al., Science 2012)</p>
<p>Classical Fully Homomorphic Encryption (FHE, e.g., TFHE, OpenFHE):</p>
<p>Security type:      Computational — based on LWE hardness assumption</p>
<p>Client requirement: Classical computer only</p>
<p>Computation model:  Boolean circuits over encrypted data</p>
<p>Computation overhead: 10³–10⁶× slowdown vs plaintext computation</p>
<p>Verification:       Not inherent; requires separate proof systems (SNARKs)</p>
<p>Deployment status:  Practical for limited applications (private ML inference)</p>
<p>Key distinction: BQC requires a quantum server AND a client with minimal quantum hardware.</p>
<p>FHE works with classical server and client. For quantum computations, BQC is the</p>
<p>natural privacy-preserving framework; for classical computations, FHE is standard.</p>
</div>

## 6.7 Connections: Towards a Quantum Internet

The protocols studied in Chapters 5 and 6 — QKD, QRNG, superdense coding, quantum secret sharing, oblivious transfer, and blind computation — are the foundational cryptographic and communication primitives of the emerging quantum internet. Rather than isolated protocols, they form a layered architecture of increasing quantum capability:

| Layer | Capability | Key Protocols (Ch. 5–6) | Status (2024) |
|---|---|---|---|
| 1 — Trusted relay QKD | Classical-authenticated QKD via trusted nodes | BB84, decoy-state | Operational (China QBB, Tokyo, UK QAN) |
| 2 — Prepare-and-measure QKD | Point-to-point QKD without trusted relays | MDI-QKD, TF-QKD | Commercial (ID Quantique, Toshiba, QuantumCTek) |
| 3 — Entanglement distribution | Quantum repeaters distribute Bell pairs | E91, quantum teleportation | Early experiments (Delft 35 km, Micius 1200 km) |
| 4 — Distributed quantum computation | Blind/verified quantum computation | BFK BQC, quantum OT | Research horizon (10+ years) |

QRNG hardware will be a pervasive infrastructure component at every layer. QKD basis choices, quantum error correction ancilla preparation, quantum algorithm randomness, and classical cryptographic key generation all require certified randomness. The NIST SP 800-90B framework and the FIPS 140-3 validation path ensure that QRNGs deployed in cryptographic hardware meet the rigorous standards required for government and commercial use.

The FIPS 203/204/205/206 PQC standards provide the classical cryptographic backbone that protects the classical control channels supporting quantum networks. Authentication of QKD messages uses ML-DSA signatures; key confirmation uses ML-KEM or hybrid X25519+ML-KEM. Together, quantum and post-quantum cryptography form a mutually reinforcing defence: QKD for forward-secret key establishment, PQC for authentication and data-at-rest protection.

**RECAP**

*Short Answer Questions and Model Answers — Chapter 6*

## Short Answer Questions

Answer each question in 3–6 sentences. These questions test conceptual understanding of the core topics in Chapter 6.

**1. What is quantum teleportation?**

**2. Which quantum resource is essential for teleportation?**

**3. Write the standard Bell state used in teleportation.**

**4. What is superdense coding?**

**5. How many classical bits can be transmitted using one qubit in superdense coding?**

**6. What is quantum entanglement swapping?**

**7. What is a Bell-state measurement?**

**8. What is the purpose of quantum repeaters?**

**9. What is decoherence?**

**10. Define quantum fidelity.**

**11. What is quantum teleportation fidelity?**

**12. Why is classical communication needed in teleportation?**

**13. What is a GHZ state?**

**14. What is the role of unitary operations in teleportation?**

**15. What is quantum networking?**

## Model Answers

**1. What is quantum teleportation?**

**Answer:** Quantum teleportation (Bennett et al., 1993) is a protocol for transferring an unknown quantum state |ψ⟩ = α|0⟩ + β|1⟩ from Alice to a remote Bob using one pre-shared entangled Bell pair and two bits of classical communication. The process destroys the original state at Alice's location (consistent with no-cloning) and recreates it exactly at Bob's location after he applies a correction unitary. Teleportation does not violate special relativity because the two classical bits must be transmitted before Bob can apply his correction — the state is not reconstructed until the classical information arrives. Teleportation is a key building block for quantum repeaters, quantum networks, and distributed quantum computing.

**2. Which quantum resource is essential for teleportation?**

**Answer:** The essential quantum resource for teleportation is shared quantum entanglement — specifically, one maximally entangled Bell pair (one ebit) shared between Alice and Bob, with Alice holding one qubit and Bob the other. Without this pre-shared entanglement, teleportation is impossible: Alice's Bell measurement and two classical bits alone cannot convey an arbitrary quantum state. The entanglement provides the non-classical channel through which the quantum information is effectively transferred. In the language of quantum resources, the teleportation identity is: 1 ebit + 2 classical bits → 1 transmitted qubit.

**3. Write the standard Bell state used in teleportation.**

**Answer:** One commonly used Bell state in teleportation is |Φ⁺⟩ = (|00⟩ + |11⟩)/√2. All four Bell states can be used — {|Φ⁺⟩, |Φ⁻⟩, |Ψ⁺⟩, |Ψ⁻⟩} — with Alice's measurement outcome determining which of the four states Bob's qubit collapses into. Bob then applies the corresponding corrective unitary (I, Z, X, or XZ respectively) to recover |ψ⟩. The Bell states form an orthonormal basis for the two-qubit Hilbert space, which is why Alice's Bell measurement can distinguish all four cases perfectly.

**4. What is superdense coding?**

**Answer:** Superdense coding (Bennett and Wiesner, 1992) is a quantum communication protocol in which two classical bits of information are transmitted from Alice to Bob by sending only one qubit, using a pre-shared entangled Bell pair as a resource. Alice encodes her two-bit message by applying one of four Pauli operations {I, Z, X, iY} to her qubit of the shared |Φ⁺⟩ Bell pair, transforming it into one of the four orthogonal Bell states. She then transmits her qubit to Bob, who holds both qubits and performs a Bell measurement (CNOT + Hadamard + measurement) to decode the two-bit message. The protocol saturates the entanglement-assisted channel capacity of 2 bits per qubit.

**5. How many classical bits can be transmitted using one qubit in superdense coding?**

**Answer:** Two classical bits can be transmitted using one qubit when Alice and Bob share a pre-distributed entangled Bell pair (one ebit). The pre-shared entanglement effectively doubles the information capacity of the quantum channel from its unassisted value of 1 bit/qubit (Holevo bound) to 2 bits/qubit. This is not a violation of any fundamental limit — the entanglement is a resource that was created and distributed at an earlier time, and its distribution consumed one qubit of channel capacity. The resource identity is: 1 qubit channel + 1 ebit = 2 classical bits.

**6. What is quantum entanglement swapping?**

**Answer:** Quantum entanglement swapping is a process that creates entanglement between two particles that have never directly interacted. Suppose Alice holds particle 1 entangled with particle 2 (held at a relay), and the relay also holds particle 3 entangled with particle 4 (held by Bob). If the relay performs a Bell-state measurement (BSM) on particles 2 and 3 together, the measurement result creates entanglement between particles 1 and 4 — even though they have never been in contact. Entanglement swapping is the fundamental operation enabling quantum repeaters: by chaining multiple swap operations, entanglement can be extended across arbitrarily long distances, enabling long-range quantum communication without direct end-to-end quantum channels.

**7. What is a Bell-state measurement?**

**Answer:** A Bell-state measurement (BSM) is a joint quantum measurement performed on two qubits that projects them onto one of the four maximally entangled Bell states: {|Φ⁺⟩, |Φ⁻⟩, |Ψ⁺⟩, |Ψ⁻⟩}. The measurement outcome (which of the four Bell states is detected) yields two classical bits of information. In linear optics, a complete deterministic BSM is not possible without ancilla photons, but partial BSMs (distinguishing two of the four Bell states) can be implemented with a beamsplitter and two photodetectors via Hong-Ou-Mandel interference. BSMs are central to quantum teleportation (Alice's measurement), superdense coding decoding (Bob's measurement), entanglement swapping (relay's measurement), and MDI-QKD (relay's BSM).

**8. What is the purpose of quantum repeaters?**

**Answer:** Quantum repeaters are devices designed to extend the range of quantum communication beyond the ~100–200 km practical limit of direct fibre transmission, without violating the no-cloning theorem that prevents classical amplification of quantum signals. They work by dividing the total distance into shorter segments, distributing entanglement across each segment separately, and then extending the entanglement via entanglement swapping. Quantum memories (devices that can store quantum states for milliseconds to seconds) are essential to synchronise the entanglement generation across segments. Quantum repeaters would enable truly long-distance quantum networks, connecting cities and countries with quantum-secure communication channels.

**9. What is decoherence?**

**Answer:** Decoherence is the process by which a quantum system loses its quantum coherence — the delicate superposition and entanglement that distinguish quantum systems from classical ones — through unwanted interactions with the surrounding environment. Mathematically, a pure quantum state |ψ⟩ evolves into a mixed state ρ = Σ\_i p\_i |ψ\_i⟩⟨ψ\_i| as the system entangles with environmental degrees of freedom. Once decoherence has occurred, the quantum system behaves classically: superpositions are destroyed, entanglement is lost, and quantum protocols fail. Decoherence times range from femtoseconds for molecular vibrations to seconds for carefully isolated trapped ions and superconducting qubits at millikelvin temperatures. Decoherence is the primary challenge in building practical quantum computers and quantum memories.

**10. Define quantum fidelity.**

**Answer:** Quantum fidelity F(ρ, σ) = (Tr √(√ρ · σ · √ρ))² is a measure of the similarity between two quantum states ρ and σ, valued between 0 and 1. For pure states, F(|ψ⟩, |φ⟩) = |⟨ψ|φ⟩|² — it equals the squared inner product, with F = 1 indicating identical states and F = 0 indicating orthogonal states. Fidelity quantifies how well a physical process (noisy channel, imperfect gate, decoherence) preserves a target quantum state. In quantum communication and computation, maintaining high fidelity (F > 0.99 for fault-tolerant thresholds) is essential for reliable quantum information processing.

**11. What is quantum teleportation fidelity?**

**Answer:** Quantum teleportation fidelity F\_teleport measures how accurately the teleported state at Bob's location matches Alice's original state |ψ⟩. For a noiseless protocol with perfect Bell pairs, F\_teleport = 1 exactly. For noisy Bell pairs with fidelity F\_Bell, the teleportation fidelity is F\_teleport = (2F\_Bell + 1)/3 under depolarising noise — higher than F\_Bell because teleportation partially "corrects" certain noise patterns. The classical limit — the best fidelity achievable by measuring Alice's qubit classically and preparing a new qubit at Bob's location — is F\_classical = 2/3 for an arbitrary single-qubit state. Any teleportation with F > 2/3 exceeds the classical bound and demonstrates genuine quantum advantage.

**12. Why is classical communication needed in teleportation?**

**Answer:** Classical communication is required in teleportation because Alice's Bell measurement collapses Bob's qubit into one of four possible states depending on her measurement outcome. Alice must transmit her two-bit measurement result (four possibilities → two classical bits) to Bob before he can apply the correct correction unitary (I, Z, X, or XZ). Without Alice's classical communication, Bob cannot know which correction to apply — his qubit is in a uniformly random mixed state. This requirement for classical communication ensures that teleportation does not violate special relativity: no information travels faster than the classical channel, which is limited to the speed of light.

**13. What is a GHZ state?**

**Answer:** A GHZ (Greenberger-Horne-Zeilinger) state is a maximally entangled multipartite state involving three or more qubits. The standard three-qubit GHZ state is |GHZ⟩ = (|000⟩ + |111⟩)/√2. GHZ states are the quantum extension of Bell pairs to more than two qubits and exhibit extreme non-classical correlations: measuring any one qubit in the Z basis collapses the remaining two qubits to a product state, completely destroying all entanglement. GHZ states are used in quantum secret sharing (HBB protocol), quantum error correction, tests of quantum non-locality, quantum metrology (achieving Heisenberg-limited precision), and distributed quantum computing. They can be generated by applying a Hadamard gate to one qubit followed by CNOT gates to the others.

**14. What is the role of unitary operations in teleportation?**

**Answer:** After Alice performs her Bell measurement and sends the two classical bits to Bob, Bob applies a specific unitary correction based on those bits to recover |ψ⟩. The four possible measurement outcomes correspond to four correction operations: |Φ⁺⟩ → I (no correction), |Φ⁻⟩ → Z (phase flip), |Ψ⁺⟩ → X (bit flip), |Ψ⁻⟩ → XZ (bit and phase flip). These unitaries are the four Pauli operators, which are exactly the same operators used in superdense coding — reflecting the mathematical duality between the two protocols. Bob's correction unitary effectively "undoes" the random rotation that Alice's measurement imposed on Bob's qubit, leaving it in the exact state |ψ⟩ that Alice began with.

**15. What is quantum networking?**

**Answer:** Quantum networking is the interconnection of multiple quantum devices — quantum computers, quantum memories, QKD systems, quantum sensors — using quantum channels (optical fibres or free-space links) that transmit qubits and distribute entanglement. Unlike classical networks that transmit and process bits, quantum networks transmit quantum information and enable protocols that are impossible classically: secure QKD, teleportation of quantum states between distant nodes, distributed quantum computation, and quantum-enhanced sensing. The Quantum Internet Alliance (EU) and the US DOE Quantum Internet Blueprint (2020) envision a multi-layered quantum internet architecture, starting from QKD-only trusted relays (Layer 1) and scaling up through entanglement distribution (Layer 3) to fully quantum-networked fault-tolerant computing (Layer 4) over the coming decades.

## Solved Examples — Chapter 6

<div class="box box-example">
<p class="box-title"><strong>Example 6.1 — Min-Entropy Calculation for a Biased QRNG</strong></p>
<p>Problem: A beamsplitter QRNG has 60:40 split (P(0) = 0.60, P(1) = 0.40).</p>
<p>(a) Compute H_min. (b) Conditioning ratio needed. (c) Certified output rate if raw = 1 Gbps.</p>
<p>Solution:</p>
<p>For 1 output bit: n_in ≥ (1 + 64)/0.737 = 88.2 raw bits → ratio = ⌈88.2⌉ = 89</p>
<p>Conservative practical ratio:</p>
<p>(c) At 1 Gbps raw and ratio 2: certified output ≈ 500 Mbps (conservative)</p>
<p>With large-block SHA-256 conditioning: ≈ 700 Mbps (accounting for block overhead)</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 6.2 — Superdense Coding Step-by-Step for Message "11"</strong></p>
<p>Problem: Alice sends message "11" using superdense coding. Show each step.</p>
<p>Solution:</p>
<p>(iY ⊗ I)|Φ⁺⟩ = (iY ⊗ I)(|00⟩ + |11⟩)/√2</p>
<p>= (i|1⟩⊗|0⟩ + (−i)|0⟩⊗|1⟩)/√2</p>
<p>= i(|10⟩ − |01⟩)/√2 = i|Ψ⁻⟩_{AB}</p>
<p>(Global phase i is irrelevant for measurements.)</p>
<p>(b) Resulting state: |Ψ⁻⟩_{AB} = (|10⟩ − |01⟩)/√2</p>
<p>(c) Bob's decoding: apply CNOT_{A→B} then H on A.</p>
<p>From the Bell-state decoding table:</p>
<p>(d) Measurement outcome: qubit A → 1, qubit B → 1.</p>
<p>Bob reads "11" — matches Alice's original message. ✓</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 6.3 — Holevo Bound for Trine States</strong></p>
<p>Problem: Alice encodes 3 messages in the trine states |ψ₁⟩=|0⟩,</p>
<p>|ψ₂⟩=(−|0⟩+√3|1⟩)/2, |ψ₃⟩=(−|0⟩−√3|1⟩)/2 with equal probability p=1/3.</p>
<p>Calculate the Holevo bound χ.</p>
<p>Solution:</p>
<p>Step 1: Average state ρ̄ = (1/3)(|ψ₁⟩⟨ψ₁| + |ψ₂⟩⟨ψ₂| + |ψ₃⟩⟨ψ₃|)</p>
<p>By the symmetry of trine states (SIC-POVM): ρ̄ = I/2 (maximally mixed)</p>
<p>Interpretation: Trine encoding achieves the maximum 1 bit Holevo information</p>
<p>for a single qubit system. The accessible information with an optimal POVM is</p>
<p>I ≈ 0.585 bits — less than χ, because trine states are non-orthogonal.</p>
<p>Achieving χ = 1 bit exactly requires collective measurements on many copies.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 6.4 — BFK Blindness Verification</strong></p>
<p>Problem: In BFK, Alice wants to compute a 1-qubit rotation with ideal angles</p>
<p>(a) Compute the three δ_j sent to the server. (b) Verify δ_j looks random to the server.</p>
<p>Solution:</p>
<p>Server receives: δ = (7π/12, 5π/4, π/2) — three discrete angle values.</p>
<p>(b) Blindness analysis:</p>
<p>P(δ_j = δ | φ_j) = (1/8) for each of 8 possible values</p>
<p>(since θ_j is uniform and unknown to server)</p>
<p>→ The distribution of δ_j is identical regardless of φ_j.</p>
<p>Server cannot distinguish φ_j = π/3 from any other computation angle. ✓</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 6.5 — BQSM Quantum OT Security Analysis</strong></p>
<p>Problem: A BQSM OT protocol transmits N = 2000 qubits. (a) Maximum adversary</p>
<p>memory n_adv for security. (b) Extractable OT bits per protocol run.</p>
<p>Solution:</p>
<p>(a) Security condition: n_adv &lt; N/4 = 2000/4 = 500 qubits.</p>
<p>Any adversary limited to ≤ 499 quantum memory qubits cannot break the protocol.</p>
<p>(Adversary must measure &gt; N/2 = 1000 qubits immediately, losing information</p>
<p>about the non-chosen message.)</p>
<p>(b) Extractable OT bits (approximate):</p>
<p>Honest receiver measures N/2 = 1000 qubits in correct bases.</p>
<p>OT bits ≈ (N/4) × (h_min − h(Q))</p>
<p>= 500 × (0.95 − 0.286) ≈ 500 × 0.664 ≈ 332 bits</p>
<p>Each OT₁² instance requires ~6 bits of OT, so approximately</p>
</div>

## Multiple Choice Questions — Chapter 6

Note: Answers are collected at the end of this chapter.

**1.  In a photon beamsplitter QRNG with P(0) = 0.55 due to imperfect splitting, the min-entropy per raw bit is:**

- (A)  0.55 bits/sample

- (D)  H\_min = 1.0 bits/sample (always perfect for quantum sources)

**2.  Vacuum fluctuations provide QRNG entropy because:**

- (A)  Thermal noise from detector electronics is maximally entropic

- (B)  The Heisenberg uncertainty principle gives irreducible non-zero variance in quadrature amplitudes X̂ and P̂ even in the vacuum state |0⟩

- (C)  Spontaneous emission events from the laser cavity are quantum-mechanically random

- (D)  The vacuum state has maximum von Neumann entropy S = 1 bit

**3.  The NIST SP 800-90B Repetition Count Test (RCT) monitors for:**

- (A)  Sequences that fail the NIST Statistical Test Suite

- (B)  A single value repeating consecutively more than C = ⌈1 + (−log₂ α)/H\_min⌉ times

- (C)  Autocorrelation in the output sequence exceeding a threshold

- (D)  Entropy rate falling below 0.5 bits/sample over any 512-sample window

**4.  In superdense coding, Alice encodes message "01" by applying which operator to her qubit of |Φ⁺⟩?**

- (A)  Pauli X (bit flip)

- (B)  Pauli Z (phase flip)

- (C)  Pauli Y

- (D)  Identity I (no operation)

**5.  The resource identity for superdense coding is:**

- (A)  1 qubit channel + 1 ebit = 1 classical bit

- (B)  1 qubit channel + 1 ebit = 2 classical bits

- (C)  2 qubit channel = 2 classical bits (no entanglement needed)

- (D)  1 qubit channel = 1 ebit + 1 classical bit

**6.  The Holevo bound χ = S(ρ̄) − Σp\_x S(ρ\_x) achieves its maximum of n bits for an n-qubit channel when:**

- (A)  Encoded states ρ\_x are all maximally mixed

- (B)  Encoded states are pure and orthogonal, and prior probabilities p\_x are uniform

- (C)  The channel is a maximally depolarising channel

- (D)  Bob uses a von Neumann measurement in the computational basis

**7.  In the HBB (2,2)-threshold QSS protocol, the security condition (neither Bob nor Charlie alone learns the secret) holds because:**

- (A)  The GHZ state is maximally entangled — all reduced states are maximally mixed

- (B)  Each party's individual qubit is a maximally mixed state ρ = I/2, statistically independent of the secret

- (C)  Bob and Charlie's qubits are classically correlated but not entangled with Alice's

- (D)  The XOR of Bob and Charlie's measurement outcomes is always zero

**8.  The Lo–Chau–Mayers impossibility theorem proves that unconditional quantum OT is impossible. The core argument uses:**

- (A)  The no-cloning theorem — Eve can copy the committed state and equivocate

- (B)  Uhlmann's theorem — if two mixed states are identical (concealing), their purifications differ by a unitary Alice can apply to equivocate (breaking binding)

- (C)  The Holevo bound — classical information in the commitment is bounded

- (D)  The Heisenberg uncertainty principle — Bob cannot measure both binding and concealing simultaneously

**9.  In the Bounded Quantum Storage Model (BQSM) for quantum OT, security holds when:**

- (A)  The adversary's quantum memory capacity n\_adv satisfies n\_adv < N/4 (N = transmitted qubits)

- (B)  The quantum channel has noise rate exceeding a threshold

- (C)  Both parties use quantum computers for the protocol

- (D)  The protocol is executed in less than the coherence time of the adversary's qubits

**10.  In the BFK protocol, the server cannot determine the client's computation because:**

- (A)  The qubits are encrypted using QKD before transmission

- (B)  The measurement angles δ\_j sent to the server are uniformly random — θ\_j randomisation makes δ\_j independent of computation angle φ\_j

- (C)  The server only performs classical computation and cannot infer quantum states

- (D)  The cluster state entangles the computation in a way that requires Alice's classical key to interpret

**11.  Trap qubits in verified BQC are used to:**

- (A)  Improve the key rate by providing additional randomness

- (B)  Detect a dishonest server — tampered trap measurements fail with probability ≥ 1/2 per tampered qubit

- (C)  Correct errors introduced by a noisy quantum channel

- (D)  Certify the quantum entanglement of the cluster state used for computation

**12.  Device-independent QRNG (DI-QRNG) certifies randomness by:**

- (A)  Using a cryptographic hash function to ensure the output is indistinguishable from random

- (B)  Demonstrating a loophole-free Bell inequality violation — correlations that cannot be pre-determined by any classical (or local hidden-variable) strategy

- (C)  Requiring the QRNG device to have a NIST FIPS 140-3 certification

- (D)  Using entangled qubits to detect any classical hardware manipulation

**13.  The entanglement-assisted classical capacity C\_E of a noiseless n-qubit channel equals:**

- (A)  n bits — same as without entanglement

- (B)  2n bits — doubling capacity via superdense coding

- (C)  n log₂(n) bits — logarithmic enhancement

- (D)  n/2 bits — entanglement consumes channel resources

**14.  A (2,3)-threshold quantum secret sharing scheme guarantees that a single shareholder learns:**

- (A)  At most 1 classical bit about the shared quantum state |ψ⟩

- (B)  Nothing — their qubit is a maximally mixed state ρ = I/2 independent of |ψ⟩

- (D)  The parity of α and β but not their individual values

**15.  Measurement-Based Quantum Computation (MBQC) differs from gate-based quantum computation in that:**

- (A)  MBQC requires more qubits but fewer gates for the same computation

- (B)  Computation proceeds entirely via adaptive single-qubit measurements on a pre-prepared cluster state, consuming the resource state in the process

- (C)  MBQC is less powerful than gate-based QC — it cannot implement arbitrary unitaries

- (D)  MBQC does not require entanglement; only product states are needed

## MCQ Answers — Chapter 6

| Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 | Q8 | Q9 | Q10 | Q11 | Q12 | Q13 | Q14 | Q15 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| B | B | B | B | B | B | B | B | A | B | B | B | B | B | B |

## Unsolved Problems — Chapter 6

**Problem 6.1:** A vacuum-fluctuation QRNG measures quadrature X̂ with σ²\_total = σ²\_shot + σ²\_elec = 1/4 + 1/16. (a) Calculate H\_min per 8-bit ADC sample. (b) What fraction of variance is quantum (certifiable)?

**Problem 6.2:** Perform superdense coding of message "10": (a) apply the correct Pauli, (b) identify resulting Bell state, (c) verify Bob's decoding gives |10⟩.

**Problem 6.3:** Calculate Holevo information for BB84 encoding: Alice sends {|0⟩, |1⟩, |+⟩, |−⟩} with equal probability 1/4. (a) Find ρ̄. (b) Compute S(ρ̄). (c) Compute χ. Why can't Bob perfectly distinguish all 4 states?

*[Hint: ρ̄ = I/2, S = 1 bit, χ = 1 bit. Non-orthogonal states cannot be perfectly distinguished.]*

**Problem 6.4:** The HBB QSS protocol is run 1000 times. In 750 key-extracting rounds, what is Bob's information alone about Alice's string? What is the combined (Bob + Charlie) information?

*[Hint: Bob alone: 0 bits (ρ\_B = I/2). Combined: 750 bits — they can reconstruct Alice's full secret.]*

**Problem 6.5:** BFK with 10 computation qubits and 5 trap qubits. A dishonest server tampers with 3 qubits. (a) P(detecting cheating). (b) P(completely undetected). (c) How many traps for < 10⁻¹² undetected probability?

*[Hint: P(undetected per 3 tampered) = (1 − 1/6)³ = (5/6)³ ≈ 0.58. For 10⁻¹²: use ~100 trap qubits.]*

**Problem 6.6:** Verify the dual resource identities: (a) superdense coding: 1 ebit + 1 qubit → 2 cbits. (b) Teleportation: 1 ebit + 2 cbits → 1 qubit. Show both are consistent with C\_E = 2n bits/channel use.

*[Hint: C\_E = S(A) + S(B) = 1+1 = 2 bits for a Bell pair. Teleportation is the inverse identity. Neither violates no-FTL because classical bits must be transmitted first.]*

**Problem 6.7:** Design a QRNG for a cryptographic hardware module. Requirements: 256-bit key in < 1 μs, FIPS 140-3 Level 3, −20°C to +85°C. (a) Choose QRNG type. (b) Minimum raw bit rate. (c) SP 800-90B documentation requirements.

*[Hint: Silicon photonics CV-QRNG. At H\_min = 7.5 bits/sample (8-bit ADC), need ~69 samples = 552 raw bits; at 1 Gbps raw rate: 552 ns < 1 μs ✓]*

**Problem 6.8:** Compare quantum and classical secret sharing for (3,5) threshold. (a) Classical Shamir: field operations for share generation and reconstruction. (b) Quantum state sharing: minimum qubits. (c) What can QSS do that classical cannot?

*[Hint: (a) Shamir: 10 field multiplications (gen), 9 (reconstruct). (b) [5,1,3] quantum code — 5 qubits. (c) QSS can share an unknown quantum state; eavesdropping is detectable; dealer need not know the secret.]*

## Theory Questions — Chapter 6

- Explain the three principal QRNG implementations. For each: (a) identify the quantum process providing entropy; (b) state the theoretical H\_min per sample; (c) describe the main practical limitation. Which is best suited for chip-scale cryptographic hardware, and why?

- Derive the NIST SP 800-90B Adaptive Proportion Test (APT) threshold for a QRNG with claimed H\_min = 0.9 bits/sample and false alarm rate α = 2⁻²⁰. What does the APT detect, and why is it necessary alongside the RCT?

- Prove that DI-QRNG cannot be implemented without a loophole-free Bell test. Why do the detection loophole and locality loophole each independently invalidate Bell-inequality-based randomness certification?

- Derive the superdense coding decoding circuit. Starting from |Ψ⁺⟩\_{AB} = (|01⟩+|10⟩)/√2, show explicitly that CNOT\_{A→B} followed by H\_A transforms |Ψ⁺⟩ to the computational basis state |10⟩.

- State and prove the Holevo bound. Show why the maximum χ for an n-qubit system is n bits, and explain why this maximum is achievable with orthogonal encodings. What measurement achieves the Holevo capacity?

- Describe the Cleve–Gottesman–Lo (2,3)-threshold quantum secret sharing scheme. Prove that any single share is a maximally mixed state, independent of the encoded qubit |ψ⟩.

- Explain the Lo–Chau–Mayers impossibility theorem in detail. Use Uhlmann's theorem to show why any perfectly concealing quantum bit commitment scheme is necessarily non-binding. What physical resources circumvent this impossibility?

- Explain MBQC and its equivalence to the gate model. Show how a single-qubit rotation R\_z(θ) can be implemented by measuring a 2-qubit cluster state at appropriate angles. How does BFK exploit MBQC to achieve blindness?

## Assignments & Project Suggestions — Chapter 6

### Assignment 6.1 QRNG Implementation and SP 800-90B Testing (Marks: 15)

(a) Simulate a biased QRNG (P(0) = 0.55) generating 10⁶ bits. Implement the Repetition Count Test (C = ⌈1 + 20/H\_min⌉) and Adaptive Proportion Test in Python. (b) Apply SHA-256 Toeplitz conditioning to produce certified output bits. (c) Calculate pre- and post-conditioning min-entropy using the NIST SP 800-90B estimators. Compare with theoretical values. (d) Extend to a simulated CV-QRNG with Gaussian output and compute H\_min as a function of the shot-noise-to-electronics-noise ratio.

### Assignment 6.2 Superdense Coding Simulation in Qiskit (Marks: 10)

Implement all four superdense coding encodings in Qiskit: (a) build Bell pair preparation, Alice's encoding {I, Z, X, iY}, and Bob's decoding circuits for each of "00", "01", "10", "11"; (b) run on the Qiskit Aer simulator (10,000 shots) and verify > 99% correct decoding for all four messages; (c) introduce depolarising noise at p = 0, 0.01, 0.05, 0.10 and measure decoding fidelity vs noise. At what noise level does the channel capacity drop below 1.5 cbits? (d) Verify that qubit A alone (without qubit B) produces a maximally mixed state by computing the reduced density matrix.

### Assignment 6.3 Holevo Bound Numerical Exploration (Marks: 10)

Write a Python programme that: (a) computes χ({p\_x, ρ\_x}) for arbitrary ensembles of single-qubit states, given as Bloch sphere coordinates; (b) numerically maximises χ over all N-state ensembles for N = 2, 3, 4 states on a single qubit; (c) verifies that the maximum equals log₂(min(N, 2)) bits, confirming the 1-bit capacity of a single-qubit channel; (d) plots accessible information vs Holevo bound for 1000 random ensembles to illustrate that the bound is not always tight for individual measurements.

## Chapter 6 Summary

- QRNG derives randomness from quantum measurements: photon path splitting (1 bit/photon, ~Gbps), vacuum fluctuations (Gaussian continuous output, 10–68 Gbps, chip-integrable), and radioactive decay (historic, low rate). CV-QRNGs are preferred for chip-scale integration using silicon photonics.

- DI-QRNG certifies randomness via loophole-free Bell inequality violations — the strongest possible certification, requiring no trust in hardware. Semi-DI approaches achieve practical rates (Mbps) with partial device assumptions.

- NIST SP 800-90B defines min-entropy H\_min = −log₂(max\_x P(x)) as the primary entropy measure. Validation requires analytical noise modelling, online health tests (RCT + APT), and 10 predictive statistical estimators on ≥ 10⁶ raw samples.

- Superdense coding (Bennett–Wiesner, 1992): 1 ebit + 1 qubit channel = 2 classical bits. Alice applies {I, Z, X, iY} to encode 2 bits; Bob decodes with CNOT + H + measurement. The protocol saturates the entanglement-assisted classical capacity.

- The Holevo bound χ = S(ρ̄) − Σp\_x S(ρ\_x) limits classical information extractable from quantum states to at most n bits per n qubits (without entanglement). Superdense coding achieves C\_E = 2n bits using 1 ebit pre-shared entanglement.

- HBB quantum secret sharing uses GHZ states for (2,2)-threshold sharing: neither Bob nor Charlie alone learns anything (each holds ρ = I/2); both together recover Alice's secret. Quantum state sharing uses QECCs for (k,n)-threshold distribution of unknown quantum states.

- Unconditional quantum OT is impossible (Lo–Chau–Mayers, 1997): concealing implies equivocable, via Uhlmann's theorem. Secure quantum OT is achievable under bounded quantum storage (n\_adv < N/4), noisy storage, or relativistic constraints.

- BFK blind quantum computation (2009): client sends randomised qubits |+\_{θ\_j}⟩; server measures at angles δ\_j = φ\_j + θ\_j + r\_j·π; δ\_j is uniformly distributed regardless of φ\_j — perfect blindness. Trap qubits enable verification with exponentially small detection failure probability.

## References and Further Reading

- Bennett, C. H., & Wiesner, S. J. (1992). Communication via one- and two-particle operators on Einstein-Podolsky-Rosen states. Physical Review Letters, 69(20), 2881.

- Holevo, A. S. (1973). Bounds for the quantity of information transmitted by a quantum communication channel. Problemy Peredachi Informatsii, 9(3), 3–11.

- Hillery, M., Bužek, V., & Berthiaume, A. (1999). Quantum secret sharing. Physical Review A, 59(3), 1829.

- Cleve, R., Gottesman, D., & Lo, H.-K. (1999). How to share a quantum secret. Physical Review Letters, 83(3), 648.

- Lo, H.-K., & Chau, H. F. (1997). Is quantum bit commitment really possible? Physical Review Letters, 78(17), 3410.

- Mayers, D. (1997). Unconditionally secure quantum bit commitment is impossible. Physical Review Letters, 78(17), 3414.

- Broadbent, A., Fitzsimons, J., & Kashefi, E. (2009). Universal blind quantum computation. Proceedings of FOCS 2009, 517–526.

- Bierhorst, P. et al. (2018). Experimentally generated randomness certified by the impossibility of superluminal signals. Nature, 556, 223–226.

- NIST (2018). SP 800-90B: Recommendation for the Entropy Sources Used for Random Bit Generation. NIST, Gaithersburg, MD.

- Damgård, I., Fehr, S., Renner, R., Salvail, L., & Schaffner, C. (2007). A tight high-order entropic quantum uncertainty relation with applications. CRYPTO 2007, LNCS 4622, 360–378.

- Kent, A. (2012). Unconditionally secure bit commitment by transmitting measurement outcomes. Physical Review Letters, 109(13), 130501.

- Liu, Y. et al. (2021). Device-independent quantum random-number generation. Nature, 562, 548–551.

- Yin, J. et al. (2017). Satellite-based entanglement distribution over 1200 kilometers. Science, 356(6343), 1140–1144.

- Bennett, C. H. et al. (1993). Teleporting an unknown quantum state via dual classical and Einstein-Podolsky-Rosen channels. Physical Review Letters, 70(13), 1895.

- Barz, S. et al. (2012). Demonstration of blind quantum computing. Science, 335(6066), 303–308.

<img class="fig-img" src="content/images/image54.png" alt="figure">
