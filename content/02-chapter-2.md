# UNIT 1 | CHAPTER 2

# Photonic, Spin, Topological and Neutral Atom Platforms

*KLM Theorem · HOM Effect · Boson Sampling · EDSR · Exchange Gate · Majorana Zero Modes · Rydberg Blockade · Optical Tweezers*

<div class="box box-key-concept">
<p class="box-title"><strong>📋  Learning Objectives — Chapter 2</strong></p>
<p>(1) Describe photonic qubit encodings (polarisation, dual-rail, time-bin) and explain the KLM theorem for universal linear optical quantum computing.</p>
<p>(2) Explain the Hong-Ou-Mandel effect mathematically and describe its role in linear optical gates.</p>
<p>(3) Describe boson sampling and Gaussian Boson Sampling and explain classical intractability via matrix permanents (#P-hard).</p>
<p>(4) Explain silicon spin qubit operation: quantum dot architecture, EDSR single-qubit control, and the exchange interaction two-qubit gate.</p>
<p>(5) Describe Majorana zero modes and explain how topological protection arises from non-local qubit encoding.</p>
<p>(6) Explain the Rydberg blockade mechanism and optical tweezer array architecture for neutral atom quantum computing.</p>
<p>(7) Compare all six qubit platforms quantitatively using a unified set of performance metrics.</p>
<p>(8) Select the appropriate platform for a given quantum computing application based on requirements.</p>
</div>

## 2.1 Introduction: The Landscape of Qubit Technologies

Chapter 1 examined the two dominant commercial platforms: superconducting and trapped-ion qubits. This chapter explores four additional platforms that are advancing rapidly and may prove decisive in specific application domains. Photonic qubits use individual photons and are naturally suited for quantum communication and room-temperature computation. Silicon spin qubits leverage decades of semiconductor fabrication expertise and offer a path to CMOS-compatible large-scale integration. Topological qubits based on Majorana zero modes promise inherent protection from local errors. Neutral atom qubits in optical tweezer arrays offer reconfigurable geometries and long coherence times. Understanding this full landscape is essential for a physicist entering the quantum technology sector.

<div class="box box-key-concept">
<p class="box-title"><strong>💡  Why Multiple Platforms?</strong></p>
<p>The history of computing offers an analogy: in the 1950s, vacuum tubes, relays, and early transistors competed. No single technology won all battles simultaneously. The quantum computing ecosystem may evolve similarly — different platforms optimised for different domains. Algorithms requiring the highest gate fidelity favour trapped ions. Many qubits and fast gates favour superconducting systems. Room-temperature operation favours photonics. Inherent fault tolerance may eventually be provided by topological qubits. Understanding each platform's physics is a prerequisite for knowing which to deploy.</p>
</div>

## 2.2 Photonic Qubits: Light as a Qubit Carrier

### 2.2.1 Why Photons?

Photons offer compelling advantages: they travel at the speed of light, rarely interact with the environment (long coherence), can be transmitted through optical fibre over long distances, and naturally operate at room temperature. At optical frequencies (λ = 1550 nm, ν ~ 193 THz), the mean thermal photon number n̄ = 1/(exp(ℏω/kBT)−1) ~ 10⁻³² at 300 K — essentially zero — meaning no thermal noise. The flip side: photons do not interact with each other in linear optical media (they pass through each other without mutual interaction), making deterministic entangling operations difficult without auxiliary resources.

Single-qubit gates in photonic systems are straightforward: beam splitters and phase shifters implement arbitrary rotations in the dual-rail encoding. The challenge is two-qubit entangling gates, which require either a strong optical nonlinearity at the single-photon level (technically extremely difficult) or the probabilistic approach of the KLM theorem.

### 2.2.2 Photonic Qubit Encodings

A photonic qubit can be encoded in several degrees of freedom of a single photon:

- Polarisation encoding: |0⟩ = |H⟩ (horizontal polarisation), |1⟩ = |V⟩ (vertical polarisation). Single-qubit gates use wave plates: a half-wave plate (HWP) at 22.5° implements the Hadamard; a quarter-wave plate implements phase gates. Polarising beam splitters perform measurement. Natural for free-space and satellite QKD. Fragile over long fibres due to polarisation mode dispersion (PMD).

- Dual-rail (path) encoding: |0⟩ = |1,0⟩\_ab = photon in mode a, vacuum in mode b; |1⟩ = |0,1⟩\_ab = vacuum in a, photon in b. A single photon distributed across two spatial waveguide modes. Single-qubit gates are beam splitters and phase shifters. Most natural for integrated photonic circuits on silicon chips. Loss of the photon is immediately detectable as an erasure — a useful error detection property.

- Time-bin encoding: |0⟩ = photon in early time slot, |1⟩ = photon in late time slot. Implemented with unbalanced Mach-Zehnder interferometers. Immune to PMD in fibres — preferred for long-distance fibre QKD. Used by Toshiba, ID Quantique, and QuantumCTek commercial QKD systems.

- Continuous-variable (CV) encoding: quadratures of squeezed light states (q̂, p̂). Used by Xanadu's Gaussian Boson Sampling processors and PsiQuantum's fusion-based approach. Enables GKP (Gottesman-Kitaev-Preskill) fault-tolerant encoding.

### 2.2.3 The KLM Theorem: Linear Optics is (Probabilistically) Universal

<div class="box box-anecdote">
<p class="box-title"><strong>📜  KLM Theorem — Knill, Laflamme &amp; Milburn, Nature 2001</strong></p>
<p>In 2001, Emanuel Knill, Raymond Laflamme, and Gerard Milburn published a landmark paper in Nature (409, 46) showing that universal quantum computation is achievable using only: (1) linear optical elements (beam splitters, phase shifters), (2) single-photon Fock state sources, (3) photon-number-resolving (PNR) detectors, and (4) classical feed-forward control (using measurement outcomes to control subsequent gate elements). The key insight: while a single application of a linear optical network cannot implement a deterministic photon-photon entangling gate, the gate can be made near-deterministic using ancilla photons, post-selection, and feed-forward. Gate success probability approaches 1 with polynomial ancilla overhead. KLM established the theoretical possibility of photonic quantum computing without strong optical nonlinearities.</p>
</div>

The beam splitter transformation (50:50 unitary):

*[Beam splitter unitary (dual-rail basis)]*

PsiQuantum's practical approach uses "fusion gates" — probabilistic two-photon Bell-state measurements that succeed 50% of the time — combined with resource-state generation to implement fusion-based quantum computing. This avoids the enormous ancilla overhead of the original KLM proposal while maintaining fault tolerance.

### 2.2.4 The Hong-Ou-Mandel Effect

The Hong-Ou-Mandel (HOM) effect is a quantum interference phenomenon that is the fundamental building block of linear optical quantum computing and quantum communication. When two identical photons simultaneously enter the two input ports of a 50:50 beam splitter, quantum interference causes both photons to always exit from the same output port — they bunch together.

Mathematical derivation: two photons in state a†b†|vac⟩ (one in each input mode) undergo the beam splitter transformation a† → (c†+d†)/√2, b† → (c†−d†)/√2:

**a†b†|vac⟩ → (c†+d†)(c†−d†)|vac⟩/2 = (c†²-d†²)|vac⟩/2 = (|2,0⟩ − |0,2⟩)/√2**   *[HOM output state]*

The |1,1⟩ term vanishes — the photons never exit from different ports. This requires perfect photon indistinguishability (identical frequency, polarisation, spatial mode, and arrival time). Experimentally, the HOM dip — coincidence count rate vs path length delay — drops to zero at perfect indistinguishability, with visibility V = (C\_max − C\_min)/C\_max = 1 for perfectly identical photons. HOM visibility >99% is required for high-fidelity photonic entangling gates and quantum teleportation at repeater nodes.

<figure class="book-figure">
<img src="content/images/image20.png" alt="Figure 2.1: Four photonic qubit encoding schemes. Far left: Polarisation encoding — |H⟩ encodes |0⟩, |V⟩ encodes |1⟩. A half-wave plate (HWP) at 22.5° implements the Hadamard; a quarter-wave plate implements phase gates. Centre-left: Dual-rail (path) encoding — photon in upper waveguide = |0⟩, lower = |1⟩. A 50:50 beam splitter implements the Hadamard gate on the dual-rail qubit. Centre-right: Time-bin encoding — early time slot = |0⟩, late = |1⟩; unbalanced Mach-Zehnder interferometers implement single-qubit gates. Far right: Hong-Ou-Mandel effect at a 50:50 beam splitter — two identical photons entering from opposite ports always exit from the same port (photon bunching), quantum interference cancelling the |1,1⟩ coincidence term.">
<figcaption>Figure 2.1: Four photonic qubit encoding schemes. Far left: Polarisation encoding — |H⟩ encodes |0⟩, |V⟩ encodes |1⟩. A half-wave plate (HWP) at 22.5° implements the Hadamard; a quarter-wave plate implements phase gates. Centre-left: Dual-rail (path) encoding — photon in upper waveguide = |0⟩, lower = |1⟩. A 50:50 beam splitter implements the Hadamard gate on the dual-rail qubit. Centre-right: Time-bin encoding — early time slot = |0⟩, late = |1⟩; unbalanced Mach-Zehnder interferometers implement single-qubit gates. Far right: Hong-Ou-Mandel effect at a 50:50 beam splitter — two identical photons entering from opposite ports always exit from the same port (photon bunching), quantum interference cancelling the |1,1⟩ coincidence term.</figcaption>
</figure>

<figure class="book-figure">
<img src="content/images/image21.png" alt="Figure 2.2: Left: HOM experimental setup — two photons from an SPDC source enter a 50:50 beam splitter from opposite sides. As path length difference Δx → 0 (photons become temporally indistinguishable), coincidence counts C₁₂ dip to zero — the HOM dip. Visibility V = 1 for perfectly identical photons, V = 0 for fully distinguishable photons. Centre: KLM post-selected two-qubit gate — four photons (two data + two ancilla) enter a linear optical network. The gate succeeds when ancilla photons are detected in specific output ports (post-selection). Single-gate success probability is 1/4, improvable to near-unity using gate teleportation and cluster state ancillae. Right: KLM teleportation-based gate circuit — using Bell pairs and Bell measurements to implement near-deterministic CZ gates with polynomial photon overhead.">
<figcaption>Figure 2.2: Left: HOM experimental setup — two photons from an SPDC source enter a 50:50 beam splitter from opposite sides. As path length difference Δx → 0 (photons become temporally indistinguishable), coincidence counts C₁₂ dip to zero — the HOM dip. Visibility V = 1 for perfectly identical photons, V = 0 for fully distinguishable photons. Centre: KLM post-selected two-qubit gate — four photons (two data + two ancilla) enter a linear optical network. The gate succeeds when ancilla photons are detected in specific output ports (post-selection). Single-gate success probability is 1/4, improvable to near-unity using gate teleportation and cluster state ancillae. Right: KLM teleportation-based gate circuit — using Bell pairs and Bell measurements to implement near-deterministic CZ gates with polynomial photon overhead.</figcaption>
</figure>

## 2.3 Boson Sampling and Photonic Quantum Advantage

### 2.3.1 The Boson Sampling Problem

Boson sampling, proposed by Aaronson and Arkhipov (2011), is a specific computational task that appears hard for classical computers but natural for photonic devices. Given an m-mode optical network described by unitary U and n single photons injected into n input modes, the task is to sample the output photon-number distribution. The probability of a specific output configuration S = (s₁,...,sₘ) given input configuration T is:

**Pr(S | T) ∝ |Perm(U\_{S,T})|²**   *[Boson sampling output probability]*

where Perm(U\_{S,T}) is the matrix permanent of the n×n submatrix of U selected by the input-output configuration. The matrix permanent of an n×n matrix A is:

*[Matrix permanent definition]*

Unlike the determinant (which has alternating signs and is computable in O(n³) via Gaussian elimination), the permanent has all positive terms and no algebraic structure to exploit for fast computation. Valiant (1979) proved that computing the permanent of a 0-1 matrix is #P-complete — at least as hard as any counting problem in the class #P, which is believed computationally harder than NP. The best classical algorithm (Ryser's formula) requires O(n·2ⁿ) time.

### 2.3.2 Gaussian Boson Sampling

Gaussian Boson Sampling (GBS) replaces single-photon Fock state inputs with squeezed vacuum states (Gaussian quantum states generated by optical parametric oscillators). Output probabilities are related to matrix hafnians instead of permanents — also #P-hard. GBS is experimentally more accessible because high-quality squeezed states (squeezing r ~ 1) are more reliably generated than on-demand single-photon Fock states. Xanadu's Borealis photonic processor (Nature, June 2022) demonstrated GBS with 216 squeezed modes, completing the task in ~36 μs vs an estimated ~9000 years classical computation — a claimed quantum advantage of ~10¹⁶×.

<div class="box box-warning">
<p class="box-title"><strong>⚠  Healthy Scientific Debate</strong></p>
<p>Quantum advantage claims in boson sampling have been contested. Classical simulation algorithms have improved significantly. The specific structure of time-bin multiplexed Borealis may make it somewhat easier to simulate than worst-case random circuits. This debate is healthy — it drives better photonic hardware AND better classical algorithms. True, application-relevant quantum advantage remains the target.</p>
</div>

<figure class="book-figure">
<img src="content/images/image22.png" alt="Figure 2.3: Left: Gaussian Boson Sampling device schematic. Squeezed vacuum sources inject squeezed states into an N-mode interferometer (random unitary U implemented by beam splitters and phase shifters). Photon-number-resolving (PNR) detectors at output measure photon-number distributions. Output probabilities require computing matrix hafnians, which are #P-hard classically. Centre: The matrix permanent Perm(A) for an n×n matrix — sum of n! terms with no sign cancellations. For n = 50, Perm has ~3×10⁶⁴ terms, making exact classical computation intractable. Right: Quantum computational advantage comparison showing Borealis (Xanadu, 2022) completing GBS in 36 μs vs estimated ~9000 classical years on a supercomputer — claimed ~10¹⁶× quantum advantage, with ongoing scientific debate about classical simulation improvements.">
<figcaption>Figure 2.3: Left: Gaussian Boson Sampling device schematic. Squeezed vacuum sources inject squeezed states into an N-mode interferometer (random unitary U implemented by beam splitters and phase shifters). Photon-number-resolving (PNR) detectors at output measure photon-number distributions. Output probabilities require computing matrix hafnians, which are #P-hard classically. Centre: The matrix permanent Perm(A) for an n×n matrix — sum of n! terms with no sign cancellations. For n = 50, Perm has ~3×10⁶⁴ terms, making exact classical computation intractable. Right: Quantum computational advantage comparison showing Borealis (Xanadu, 2022) completing GBS in 36 μs vs estimated ~9000 classical years on a supercomputer — claimed ~10¹⁶× quantum advantage, with ongoing scientific debate about classical simulation improvements.</figcaption>
</figure>

## 2.4 Silicon Spin Qubits: CMOS-Compatible Quantum Computing

Silicon spin qubits encode quantum information in the spin states of individual electrons (|↑⟩ = |0⟩, |↓⟩ = |1⟩) confined in silicon quantum dots — nanoscale regions where electrons are electrostatically trapped by metallic gate electrodes. The principal appeal is compatibility with CMOS (Complementary Metal-Oxide-Semiconductor) semiconductor fabrication technology — the same process used for classical microprocessors — enabling potentially high-density, high-yield quantum processors manufactured at existing semiconductor foundries. If the physics works out, quantum dot arrays could in principle be manufactured at the billion-qubit scale using existing CMOS processes.

### 2.4.1 Quantum Dots and Spin Encoding

•	Electron spin qubit: |0⟩ = spin-up, |1⟩ = spin-down, split by Zeeman effect: ΔE = g·μ\_B·B ~ 28 GHz/T for g ~ 2. Control by ESR or EDSR.

•	Singlet-triplet qubit: two-electron states; |0⟩ = singlet (↑↓−↓↑)/√2, |1⟩ = triplet (↑↑). Controlled by exchange interaction J.

•	Nuclear spin qubit (³¹P donor in ²⁸Si): T₂ > 30 s at millikelvin — the longest-lived qubit in any solid-state system.

### 2.4.2 Quantum Dot Architecture

A silicon spin qubit is typically formed in a Si/SiGe (silicon/silicon-germanium) heterostructure or silicon-on-insulator (SOI) device. Metallic gate electrodes define electrostatically-controlled potential wells, confining a controllable number of electrons (usually one) in a ~30–50 nm diameter dot. The Zeeman splitting in an applied magnetic field B gives the qubit frequency:

**ΔE = g·μB·B    (g ≈ 2 for electron in Si, μB = 9.274×10⁻²⁴ J/T)**   *[Zeeman splitting]*

For B = 1 T: ΔE/h ≈ 28 GHz. This qubit frequency, combined with the ~100 mK operating temperature of dilution refrigerators used for spin qubits (T << ΔE/kB ~ 1.3 K), ensures thermal spin polarisation >99% — reliable qubit initialisation. Readout is performed by spin-to-charge conversion: Pauli spin blockade (or Elzerman spin readout) maps the spin state to a measurable charge state using single-electron transistor (SET) detectors with sensitivity ~10⁻⁵ e/√Hz.

### 2.4.3 EDSR Single-Qubit Control

Single-qubit gates are implemented using EDSR (Electric Dipole Spin Resonance). Generating a local oscillating magnetic field at ~28 GHz with spatial resolution ~30 nm (needed to address a single quantum dot without disturbing neighbours) is technically extremely challenging. EDSR exploits spin-orbit coupling (or synthetic spin-orbit coupling from a slanted micromagnet): an oscillating electric field at the spin resonance frequency moves the electron's spatial wavefunction, and through spin-orbit interaction, this couples to the electron spin. When the electric field oscillates at ωRF = gμBB/ℏ, it drives Rabi oscillations between |↑⟩ and |↓⟩. Single-qubit gate fidelities of 99.9% have been demonstrated in Si/SiGe (Mills et al., Science 2022).

### 2.4.4 The Exchange Gate: Two-Qubit Operations

Two-qubit gates between adjacent spin qubits exploit the Heisenberg exchange interaction. When two quantum dots are tunnel-coupled (by reducing the inter-dot potential barrier with a gate voltage VB), the Heisenberg exchange Hamiltonian is activated:

**H\_ex = J(t)·S₁·S₂    where J(t) = 4t²\_tunnel/U (tunnelling energy / on-site Coulomb repulsion)**   *[Exchange interaction Hamiltonian]*

A controlled pulse of J(t) implements a SWAP gate (full exchange) or √SWAP (half exchange, the minimal entangling gate). Gate time ~ 1–10 ns for J ~ 100 MHz, making exchange gates among the fastest two-qubit operations in any qubit technology. The main limitation is nearest-neighbour connectivity — long-range gates require SWAP chains or cavity-mediated coupling. Two-qubit fidelities of 99.5% demonstrated in Si/SiGe (Xue et al., Nature 2022).

<figure class="book-figure">
<img src="content/images/image23.png" alt="Figure 2.4: Left: False-colour SEM micrograph-style schematic of a Si/SiGe double quantum dot. Two quantum dot regions (red circles) are defined by metallic gate electrodes (blue). A single electron is confined in each dot (arrows show spin states |↑⟩ and |↓⟩). A slanted micromagnet (purple) creates a local magnetic field gradient, providing synthetic spin-orbit coupling for EDSR control. Centre: EDSR control — the oscillating gate voltage VRF at spin resonance frequency drives spatial oscillation of the electron, which couples to spin via spin-orbit interaction, driving Rabi rotations between |↑⟩ and |↓⟩. This implements single-qubit gates without requiring a local RF magnetic antenna. Right: Exchange gate operation — reducing the barrier gate voltage VB activates the exchange coupling J(VB). A timed pulse of J implements SWAP (full exchange at area Jτ = π) or √SWAP (entangling, at Jτ = π/2). Gate time ~1–10 ns for J ~ 100 MHz.">
<figcaption>Figure 2.4: Left: False-colour SEM micrograph-style schematic of a Si/SiGe double quantum dot. Two quantum dot regions (red circles) are defined by metallic gate electrodes (blue). A single electron is confined in each dot (arrows show spin states |↑⟩ and |↓⟩). A slanted micromagnet (purple) creates a local magnetic field gradient, providing synthetic spin-orbit coupling for EDSR control. Centre: EDSR control — the oscillating gate voltage VRF at spin resonance frequency drives spatial oscillation of the electron, which couples to spin via spin-orbit interaction, driving Rabi rotations between |↑⟩ and |↓⟩. This implements single-qubit gates without requiring a local RF magnetic antenna. Right: Exchange gate operation — reducing the barrier gate voltage VB activates the exchange coupling J(VB). A timed pulse of J implements SWAP (full exchange at area Jτ = π) or √SWAP (entangling, at Jτ = π/2). Gate time ~1–10 ns for J ~ 100 MHz.</figcaption>
</figure>

### 2.4.5 Key Challenges

•	Valley degeneracy: silicon has six conduction band minima. Residual valley splitting (0.1–1 meV) can cause leakage if comparable to Zeeman splitting.

•	Hyperfine noise: natural silicon contains 4.7% ²⁹Si (nuclear spin I=1/2). The magnetic noise from the nuclear spin bath limits T₂\* to ~100 ns. Isotopically purified ²⁸Si (99.9% spin-zero) extends T₂\* to >1 ms — a 10,000× improvement.

<img class="fig-img" src="content/images/image24.png" alt="figure">

***Figure 2.5: Isotopic Purification of ²⁸Si — 10,000× Coherence Enhancement***

*Comparison of spin qubit coherence in natural silicon vs. isotopically purified ²⁸Si. Top: Crystal structure showing ²⁸Si atoms (grey circles, nuclear spin I=0, no magnetic moment — 95.3% abundance in natural Si) and ²⁹Si atoms (red circles, nuclear spin I=1/2, magnetic moment μ ≈ −0.555 μ\_N — 4.7% in natural Si). The random ²⁹Si nuclear spins create a fluctuating effective magnetic field (hyperfine noise field B\_nuc ~ 1–3 mT) that dephases the electron spin qubit. Bottom-left: Free Induction Decay (FID) of spin qubit in natural Si — rapid Gaussian decay with T₂\* ~ 100 ns due to the random hyperfine field from surrounding ²⁹Si nuclei. Bottom-right: FID in isotopically purified ²⁸Si (99.95% pure, ²⁹Si reduced to <0.05%) — dramatically extended coherence T₂\* > 1 ms, since the hyperfine noise scales as √(f₂₉) where f₂₉ is the ²⁹Si fraction. Additional dynamical decoupling (Hahn echo) extends T₂ to >100 ms in purified ²⁸Si. This is why Intel, UNSW/SQC, Delft and all high-performance silicon spin qubit groups use ²⁸Si substrates.*

## 2.5 Topological Qubits and Majorana Zero Modes

<div class="box box-anecdote">
<p class="box-title"><strong>📜  Ettore Majorana, 1937 — The Physicist Who Vanished</strong></p>
<p>In 1937, Italian physicist Ettore Majorana proposed a fermion that is its own antiparticle — now called a Majorana fermion. His paper was submitted just months before his mysterious disappearance during a sea voyage to Naples — one of the great unsolved mysteries of physics history. He was 32. Eighty-seven years later, his theoretical prediction provides the basis for what may be the most fundamentally robust form of quantum computing. Majorana particles have not been conclusively observed as elementary particles, but their quasiparticle analogues — Majorana zero modes — are predicted and pursued in condensed matter systems.</p>
</div>

### 2.5.1 Majorana Zero Modes and Topological Protection

A Majorana zero mode (MZM) is a quasiparticle excitation in certain condensed matter systems that is its own antiparticle — the operator creating a MZM satisfies γ† = γ. In Alexei Kitaev's 2001 theoretical model, two MZMs appear as bound states at the two ends of a one-dimensional topological superconducting wire. Together, they define a fermionic mode whose occupation number (0 or 1) encodes one qubit of quantum information — the parity qubit.

The remarkable property: the qubit information is stored non-locally in the joint parity of two spatially separated MZMs. A local perturbation — electromagnetic fluctuation, phonon, voltage spike — at one end of the wire cannot change the global parity because it would need to simultaneously affect both ends. The error rate scales as exp(−L/ξ), where L is wire length and ξ is the coherence length. This is topological protection: immunity from local errors by the topology of the quantum state, not by active error correction.

MZMs obey non-Abelian braiding statistics: exchanging (braiding) two MZMs implements a gate operation that depends on the sequence of exchanges. This enables computation by braiding — gate operations that are inherently topologically protected, not just the stored information. The proposed physical platform is an InAs or InSb semiconductor nanowire with strong spin-orbit coupling, proximitised by an s-wave superconductor (Al) under an applied magnetic field.

### 2.5.2 Experimental Status (2024)

Achieving true MZMs experimentally is extremely challenging. In 2023, Microsoft published evidence of topological superconductivity in a hybrid Al-InAs device with signatures consistent with Majorana fermions (a topological gap protocol). However, definitive proof of non-Abelian braiding statistics — the property required for topological quantum gates — has not yet been demonstrated. Microsoft continues to invest heavily in this platform through its Azure Quantum programme as a long-term bet on inherently error-protected computation. The expected timeline for a functional topological qubit is the early 2030s.

<figure class="book-figure">
<img src="content/images/image25.png" alt="Figure 2.6: Left: Physical device schematic — an InAs semiconductor nanowire (blue) with strong spin-orbit coupling covered by an aluminium superconducting shell (grey). A magnetic field B along the wire axis drives a topological phase transition. Two Majorana zero modes γ₁ and γ₂ (orange stars) appear at the wire endpoints in the topological phase. Centre: Energy spectrum vs magnetic field showing the topological phase transition: bulk gap closes and reopens as the Zeeman energy exceeds the topological threshold, and in-gap MZM states appear at zero energy. Right: Braiding operations — exchanging (braiding) two MZMs (sequence: γ₁ around γ₂) implements a non-Abelian unitary gate on the qubit space. The key property: the gate implemented depends on the topology of the path, not its precise shape, providing inherent error protection. Multiple braiding operations in sequence implement fault-tolerant quantum gates.">
<figcaption>Figure 2.6: Left: Physical device schematic — an InAs semiconductor nanowire (blue) with strong spin-orbit coupling covered by an aluminium superconducting shell (grey). A magnetic field B along the wire axis drives a topological phase transition. Two Majorana zero modes γ₁ and γ₂ (orange stars) appear at the wire endpoints in the topological phase. Centre: Energy spectrum vs magnetic field showing the topological phase transition: bulk gap closes and reopens as the Zeeman energy exceeds the topological threshold, and in-gap MZM states appear at zero energy. Right: Braiding operations — exchanging (braiding) two MZMs (sequence: γ₁ around γ₂) implements a non-Abelian unitary gate on the qubit space. The key property: the gate implemented depends on the topology of the path, not its precise shape, providing inherent error protection. Multiple braiding operations in sequence implement fault-tolerant quantum gates.</figcaption>
</figure>

## 2.6 Neutral Atom Qubits: Rydberg Blockade and Optical Tweezers

Neutral atom qubit platforms use individual atoms trapped in optical tweezer arrays as qubits. The qubit is encoded in two long-lived hyperfine states of the atomic ground state. Key advantages: all atoms of the same species are perfectly identical (no fabrication variation), reconfigurable array geometry, long coherence times, and all-to-all connectivity via the Rydberg blockade. Commercial systems: QuEra Computing (256-atom Aquila), Atom Computing (1180 atoms, launched 2023), Pasqal (100 atoms, France), and ColdQuanta.

### 2.6.1 Optical Tweezers and Array Reconfigurability

An optical tweezer is a tightly focused laser beam that traps a single atom at its intensity maximum via the optical dipole force. The trap depth U₀ = α·I/(2ε₀c), where α is the atomic polarisability and I the laser intensity, creates a ~1 mK deep potential well for typical parameters (~100 mW in a 1 μm waist). Arrays of hundreds of tweezers are created by splitting a laser beam using spatial light modulators (SLMs) or acousto-optic deflectors (AODs). Atom positions can be rearranged in real time (~10 ms), changing the qubit connectivity graph to match the quantum algorithm. After each computation, atoms are released, the array is reloaded from a background MOT, and sorted into the desired pattern for the next shot.

### 2.6.2 The Rydberg Blockade Gate

When an atom is excited to a Rydberg state (principal quantum number n ~ 50–100), its electric polarisability is enormous — scaling as n⁷ — giving rise to a strong long-range van der Waals interaction between Rydberg atoms:

*[Rydberg van der Waals interaction]*

The Rydberg blockade: if V(R) >> ℏΩ (where Ω is the laser Rabi frequency for the Rydberg transition), exciting one atom to |r⟩ shifts the Rydberg state of a nearby atom by V(R), detuning it out of resonance. Double excitation is blockaded. This conditional excitation enables a two-qubit CZ gate in three steps: (1) π-pulse on control: excites control to |r⟩ if in |1⟩, does nothing if in |0⟩; (2) 2π-pulse on target: if blockaded (control in |1⟩), does nothing; if not blockaded (control in |0⟩), returns to |1⟩ and picks up phase π; (3) π-pulse on control: returns control from |r⟩. Net effect: |11⟩ → −|11⟩, others unchanged — a CZ gate. Rydberg CZ gate fidelities of 99.5% demonstrated experimentally.

In 2023, QuEra Computing in collaboration with Harvard and MIT demonstrated 48 logical qubits encoded in 228 physical atoms using the [[7,1,3]] Steane code, achieving logical gate fidelities below the break-even point — the first demonstration of a logical quantum gate with better performance than the underlying physical qubits. This landmark result demonstrated the viability of neutral atom platforms for fault-tolerant quantum computing.

<figure class="book-figure">
<img src="content/images/image26.png" alt="Figure 2.7: Left: Optical tweezer array showing 49 (7×7) individual atoms (bright spots) trapped at the foci of focused laser beams. Each atom (grey sphere) is individually addressable with focused gate laser beams. Inter-atom spacing is ~5–10 μm. Atoms can be rearranged in real time using AOD-controlled tweezer positions, enabling arbitrary connectivity patterns. Centre: Rydberg blockade mechanism — when atom A is excited to Rydberg state |r⟩ (shown as large glowing sphere), the van der Waals interaction V(R) = C₆/R⁶ shifts the energy of Rydberg state at atom B by Δ &gt;&gt; linewidth, preventing simultaneous excitation of B (blockade zone shown as red sphere of radius Rb = (C₆/ℏΩ)^(1/6)). Right: QuEra Aquila processor — 256 ⁸⁷Rb atoms in a programmable 2D array, demonstrated below-break-even logical qubit operation in 2023.">
<figcaption>Figure 2.7: Left: Optical tweezer array showing 49 (7×7) individual atoms (bright spots) trapped at the foci of focused laser beams. Each atom (grey sphere) is individually addressable with focused gate laser beams. Inter-atom spacing is ~5–10 μm. Atoms can be rearranged in real time using AOD-controlled tweezer positions, enabling arbitrary connectivity patterns. Centre: Rydberg blockade mechanism — when atom A is excited to Rydberg state |r⟩ (shown as large glowing sphere), the van der Waals interaction V(R) = C₆/R⁶ shifts the energy of Rydberg state at atom B by Δ &gt;&gt; linewidth, preventing simultaneous excitation of B (blockade zone shown as red sphere of radius Rb = (C₆/ℏΩ)^(1/6)). Right: QuEra Aquila processor — 256 ⁸⁷Rb atoms in a programmable 2D array, demonstrated below-break-even logical qubit operation in 2023.</figcaption>
</figure>

## 2.7 Comparative Platform Analysis: The Grand Table

Each qubit platform occupies a distinct position in the technology landscape. The table below provides a comprehensive comparison across key performance metrics (2024 data).

| Platform | Gate time (2Q) | Connectivity | T₂ | 2Q Fidelity | Scalability | Temp. |
|---|---|---|---|---|---|---|
| Superconducting (IBM Heron) | ~100–200 ns |  | 50–500 μs | 99.9% | High (CMOS-compat.) | ~20 mK |
| Trapped Ion (Quant. H2) | ~200 μs | All-to-all | >1 second | 99.9% | Challenging (>100) | RT (UHV) |
| Photonic (PsiQuantum) | ~ns (prob.) | Reconfigurable | N/A (travels at c) | <99% per gate | Challenging | RT |
| Silicon Spin (Intel) | ~10 ns (exchange) | Fixed NN | ~1 ms (e spin) | 99.5% | Very High (CMOS) | ~100 mK |
| Topological (Microsoft) | TBD | TBD | Expected >1 s | Expected >99.99% | High (SC fab) | ~20 mK |
| Neutral Atom (QuEra) | ~100–300 μs | Reconfigurable | ~1–10 s | 99.5% | High (tweezer) | ~10 μK |

***Table 2.1:*** *Grand comparison of all six major qubit platforms across seven performance dimensions (2024 data). No platform dominates all metrics simultaneously, justifying the diverse hardware development landscape.*

<figure class="book-figure">
<img src="content/images/image27.png" alt="Figure 2.8: Radar/spider chart comparing all six major qubit platforms across eight performance dimensions (2024): T₂ Coherence Time, 2Q Gate Fidelity, Gate Speed, Qubit Count, Scalability Potential, Connectivity Flexibility, Room-Temperature Operation, and Commercial Maturity. Each axis is normalised to the best-known value across all platforms. Superconducting (IBM/Google, blue) leads in gate speed, qubit count, and commercial maturity. Trapped ions (IonQ/Quantinuum, orange) lead in fidelity and coherence. Neutral atoms (QuEra/Pasqal, green) show rapidly improving coherence and reconfigurability. Silicon spin (Intel, red) shows enormous long-term CMOS scalability. Topological (Microsoft, purple) shown at projected values pending experimental demonstration. Photonic (PsiQuantum, teal) shown for communication-oriented metrics.">
<figcaption>Figure 2.8: Radar/spider chart comparing all six major qubit platforms across eight performance dimensions (2024): T₂ Coherence Time, 2Q Gate Fidelity, Gate Speed, Qubit Count, Scalability Potential, Connectivity Flexibility, Room-Temperature Operation, and Commercial Maturity. Each axis is normalised to the best-known value across all platforms. Superconducting (IBM/Google, blue) leads in gate speed, qubit count, and commercial maturity. Trapped ions (IonQ/Quantinuum, orange) lead in fidelity and coherence. Neutral atoms (QuEra/Pasqal, green) show rapidly improving coherence and reconfigurability. Silicon spin (Intel, red) shows enormous long-term CMOS scalability. Topological (Microsoft, purple) shown at projected values pending experimental demonstration. Photonic (PsiQuantum, teal) shown for communication-oriented metrics.</figcaption>
</figure>

### 2.7.1 Application-Guided Platform Selection

<figure class="book-figure">
<img src="content/images/image28.png" alt="Figure 2.9: Decision flowchart for qubit platform selection based on algorithm requirements. Root node: &quot;What does your algorithm need most?&quot; branches into: High gate fidelity → Trapped Ion; Many qubits + speed → Superconducting; Room-temperature + communication → Photonic; CMOS-scale integration → Silicon Spin; Reconfigurable connectivity + moderate scale → Neutral Atom; Inherent error protection (future) → Topological. For each branch, specific system recommendations (IBM, IonQ, QuEra, etc.) are shown with 2024 performance benchmarks. A parallel comparison matrix shows the five most important metrics for each application domain: chemistry simulation, cryptography, optimisation, error correction, and quantum networking.">
<figcaption>Figure 2.9: Decision flowchart for qubit platform selection based on algorithm requirements. Root node: &quot;What does your algorithm need most?&quot; branches into: High gate fidelity → Trapped Ion; Many qubits + speed → Superconducting; Room-temperature + communication → Photonic; CMOS-scale integration → Silicon Spin; Reconfigurable connectivity + moderate scale → Neutral Atom; Inherent error protection (future) → Topological. For each branch, specific system recommendations (IBM, IonQ, QuEra, etc.) are shown with 2024 performance benchmarks. A parallel comparison matrix shows the five most important metrics for each application domain: chemistry simulation, cryptography, optimisation, error correction, and quantum networking.</figcaption>
</figure>

Choosing the right qubit platform depends critically on the application requirements:

- High-fidelity algorithms with shallow circuits (e.g., fault-tolerant error correction demonstrations, small quantum chemistry benchmarks): Trapped ions — highest fidelity, all-to-all connectivity, long coherence.

- Deep circuits with moderate fidelity (e.g., NISQ-era VQE, QAOA, Grover): Superconducting — fast gates, high qubit count, cloud access via IBM, Google.

- Quantum communication and QKD (long-distance key distribution): Photonic — photons are the only flying qubits, operate at room temperature, compatible with fibre and free-space.

- Large qubit count needed for error correction (>1000 physical qubits): Superconducting (IBM Condor 1121 qubits) or Silicon spin (CMOS scalability).

- Maximum reconfigurability and moderate scale: Neutral atoms — optical tweezer arrays, programmable connectivity, below-break-even logical qubits demonstrated (QuEra 2023).

Inherent error protection (long-term, post-2030): Topological qubits — if successfully demonstrated, non-Abelian braiding provides gate operations immune to local noise.

<img class="fig-img" src="content/images/image29.png" alt="figure">

**Figure 2.10: Qubit Technology Roadmap — Quantum Volume Trajectories (2017–2024)**

Timeline of Quantum Volume (logarithmic scale) for major quantum computing platforms from 2017 to 2024, showing the rapid and consistent improvement across all leading systems. IBM superconducting (blue squares): QV doubling approximately annually, from QV=4 (2017, 5-qubit devices) to QV=262,144 (2024, Heron with tunable couplers). IonQ trapped ion (orange circles): rapid growth from QV=16 (2020) to >4,194,304 (2023, Forte processor) — currently the fastest-improving commercial trajectory. Quantinuum (green triangles): consistent leadership in QV per qubit, reaching QV>1,048,576 (H2, 2023) with 56 qubits. Neutral atom (purple diamonds): emerging trajectory accelerating after 2023 Harvard-QuEra demonstration of 48 error-corrected logical qubits. The consistent doubling of QV across all platforms — analogous to Moore's Law for classical computing — suggests a quantum hardware scaling era has begun, even before fault-tolerant computation is achieved.

<div class="box box-generic">
<p class="box-title"><strong>RECAP</strong></p>
<p><em>Chapter 2: Photonic, Spin, Topological and Neutral Atom Platforms — Short Answer Questions &amp; Model Answers</em></p>
</div>

## Short Answer Questions — Chapter 2

*Instructions: Answer each question in 3–6 lines.*

**Q1.**  Why are photons attractive candidates for quantum information carriers?

*[§2.2 — Photonic Qubits]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q2.**  What is polarisation encoding in photonic qubits? Write the computational basis states.

*[§2.2.2 — Polarisation Encoding]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q3.**  Define dual-rail encoding and write the basis states |0⟩ and |1⟩.

*[§2.2.2 — Dual-Rail Encoding]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q4.**  State the KLM theorem and its significance for photonic quantum computing.

*[§2.2.3 — KLM Theorem]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q5.**  Explain the Hong-Ou-Mandel effect. Derive the output state a†b†|vac⟩ after a 50:50 beam splitter.

*[§2.2.4 — HOM Effect]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q6.**  What is boson sampling and why is it computationally hard classically?

*[§2.3 — Boson Sampling]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q7.**  Define matrix permanent and state its computational complexity class.

*[§2.3 — Matrix Permanent]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q8.**  What is Gaussian Boson Sampling? How does it differ from standard boson sampling?

*[§2.3 — GBS]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q9.**  What is the main scalability advantage of silicon spin qubits for quantum computing?

*[§2.4 — Silicon Spin Qubits]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q10.**  What does EDSR stand for and how is it used in silicon spin qubit single-qubit control?

*[§2.4.2 — EDSR]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q11.**  What are Majorana zero modes and what property makes them attractive for quantum computing?

*[§2.5 — Majorana Zero Modes]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q12.**  Explain topological protection in quantum computing. Why is error suppression exponential?

*[§2.5 — Topological Protection]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q13.**  State the Rydberg blockade condition. Write the van der Waals interaction formula V(R).

*[§2.6.2 — Rydberg Blockade]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q14.**  What are optical tweezers used for in neutral atom quantum computers?

*[§2.6.1 — Optical Tweezers]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q15.**  Which qubit platform most naturally enables room-temperature operation, and why?

*[§2.7 — Platform Comparison]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Model Answers — Chapter 2

**Answer 1:**

Photons have several properties making them natural quantum information carriers. First, they have extremely long coherence times because they interact very weakly with thermal environments: at optical frequencies, the thermal photon number nTh ≈ exp(−ℏω/kBT) ~ 10⁻³² at room temperature, so thermal noise is negligible. Second, photons can travel long distances without significant decoherence through optical fibre or free space, making them ideal for quantum communication. Third, photonic systems operate at room temperature without dilution refrigerators. Fourth, photons offer multiple encodable degrees of freedom: polarisation, path, time-bin, frequency, and orbital angular momentum. The main challenge is implementing deterministic photon-photon entangling gates, which require strong optical nonlinearities at the single-photon level.

**Answer 2:**

Polarisation encoding uses the transverse polarisation state of a single photon as the qubit. The computational basis states are |0⟩ = |H⟩ (horizontal polarisation) and |1⟩ = |V⟩ (vertical polarisation). The general state is |ψ⟩ = α|H⟩ + β|V⟩. Single-qubit gates are implemented with wave plates: a half-wave plate (HWP) at 22.5° implements the Hadamard (H gate); HWP at 45° implements the X gate (swapping H and V); quarter-wave plate implements phase gates. Measurement uses a polarising beam splitter followed by single-photon detectors. This encoding is intuitive and experimentally straightforward in free-space optics but is susceptible to polarisation mode dispersion (PMD) in optical fibres, which randomly rotates the polarisation state, making time-bin encoding preferable for long-distance fibre QKD.

**Answer 3:**

Dual-rail encoding represents a qubit using a single photon distributed across two distinct spatial modes (paths or waveguides a and b). The computational basis states are: |0⟩ = |1⟩\_a ⊗ |0⟩\_b = |1,0⟩\_ab (one photon in mode a, vacuum in mode b) and |1⟩ = |0⟩\_a ⊗ |1⟩\_b = |0,1⟩\_ab (vacuum in mode a, one photon in mode b). The total photon number is always 1, providing a natural error detection mechanism: if a photon is lost, the state falls outside the qubit subspace and this erasure error is immediately detectable. Single-qubit gates are beam splitters and phase shifters acting on the two waveguide modes. Dual-rail encoding is the standard for integrated photonic quantum computing because it maps naturally to waveguide interferometer networks.

**Answer 4:**

The KLM theorem (Knill, Laflamme, Milburn, Nature 2001) states that universal quantum computation is achievable using only: (1) linear optical elements (beam splitters and phase shifters), (2) single-photon Fock state sources, (3) photon-number-resolving (PNR) detectors, and (4) classical feed-forward control. The gate operations are probabilistic but can be made near-deterministic using ancilla photons and post-selection. Significance: (a) established that photonic quantum computing requires no strong optical nonlinearities; (b) motivated practical photonic architectures (PsiQuantum's fusion-based computation, Xanadu's GBS); (c) showed that measurement-based quantum computing with photons is possible. However, the ancilla photon overhead is large, motivating more efficient "fusion gate" approaches.

**Answer 5:**

HOM effect: two identical photons entering the two input ports of a 50:50 beam splitter always exit from the same output port. Mathematical derivation — input state a†b†|vac⟩ transforms under beam splitter UBS (a† → (c†+d†)/√2, b† → (c†−d†)/√2): a†b†|vac⟩ → (c†+d†)/√2 × (c†−d†)/√2 × |vac⟩ = (c†²−d†²)/2 × |vac⟩ = (|2,0⟩ − |0,2⟩)/√2. The |1,1⟩ term vanishes — quantum interference cancels the split-output amplitude. Physical requirement: photons must be perfectly indistinguishable (identical frequency, polarisation, spatial mode, arrival time). Measured via HOM dip: coincidence counts drop to zero at perfect indistinguishability. Visibility V = (C\_max − C\_min)/C\_max = 1 for perfect photons.

**Answer 6:**

Boson sampling is a photonic computational task where n identical photons are input to n modes of a linear optical network (random unitary U) and the output photon configuration is sampled. The output probability P(S|T) ∝ |Perm(U\_{S,T})|² is proportional to the square of the matrix permanent of a submatrix. Computing matrix permanents is #P-hard (Valiant 1979) — believed computationally harder than NP-complete problems. The best classical algorithm (Ryser's formula) requires O(n·2ⁿ) operations; for n=100, this is ~10³² operations. A photonic boson sampler generates samples from this distribution in time proportional to the photon evolution time, claimed to provide exponential advantage. This makes it an example of a task where quantum systems outperform classical computers for a specific sampling problem.

**Answer 7:**

The matrix permanent of an n×n matrix A is Perm(A) = Σ\_σ ∏ᵢ Aᵢ,σ(ᵢ), where the sum is over all n! permutations σ of {1,...,n}. Unlike the determinant (which has alternating ± signs enabling cancellations and Gaussian elimination in O(n³)), the permanent has all positive terms and no convenient algebraic structure to exploit. Valiant (1979) proved that computing the permanent of a 0-1 matrix is #P-complete — the counting version of NP-hard, believed computationally harder than any NP-complete problem. No polynomial-time classical algorithm is known, and the best exact algorithm (Ryser) runs in O(n·2ⁿ). This hardness is the source of boson sampling's classical intractability.

**Answer 8:**

Gaussian Boson Sampling (GBS) uses squeezed vacuum states (Gaussian quantum states) as inputs to the linear optical network, rather than the Fock state (single-photon) inputs of standard boson sampling. In GBS, output probabilities are related to matrix hafnians (rather than permanents) — also #P-hard. Differences: (a) GBS is experimentally more accessible because high-quality squeezed states (r ~ 1) are produced by optical parametric oscillators operating below threshold, more reliably than on-demand single-photon Fock states; (b) the effective photon number in GBS can be larger; (c) GBS output probabilities involve hafnians rather than permanents. Xanadu's Borealis (2022) demonstrated GBS with 216 squeezed modes, claiming quantum advantage for this specific sampling task.

**Answer 9:**

The main scalability advantage of silicon spin qubits is compatibility with CMOS (Complementary Metal-Oxide-Semiconductor) semiconductor fabrication technology — the same industrial process used for classical transistors. Silicon spin qubits are formed in Si/SiGe heterostructures or silicon-on-insulator devices using standard e-beam lithography, thin-film deposition, and gate patterning. Manufacturing could leverage existing multi-billion-dollar semiconductor foundries (TSMC, Intel, Samsung), enabling high-yield, high-volume production. The qubits themselves are extremely small (~30–50 nm quantum dots), potentially allowing millions of qubits on a single chip. Intel's Horse Ridge cryogenic control chip already demonstrates CMOS electronics co-integration. This is unmatched by any other qubit platform.

**Answer 10:**

EDSR stands for Electric Dipole Spin Resonance. It is a technique for controlling the spin state of an electron in a quantum dot using an oscillating electric field, rather than a directly applied oscillating magnetic field. The challenge: generating a local oscillating magnetic field at ~28 GHz with spatial resolution ~30 nm (needed to address a single quantum dot without disturbing neighbours) is extremely difficult. EDSR solves this by exploiting spin-orbit coupling: an oscillating gate voltage moves the electron's spatial wavefunction in a magnetic field gradient (from a slanted micromagnet), and through spin-orbit interaction, this spatial motion couples to the electron spin. When the gate oscillates at the spin resonance frequency ω = gμBB/ℏ, Rabi oscillations are driven between |↑⟩ and |↓⟩. EDSR gate fidelities of 99.9% demonstrated in Si/SiGe (Mills et al., Science 2022).

**Answer 11:**

Majorana zero modes (MZMs) are quasiparticle excitations in topological superconductors that are their own antiparticles (γ† = γ). In topological superconducting nanowires, two MZMs appear at the wire endpoints. Together they define a fermionic mode whose occupation (0 or 1) encodes one qubit. MZMs are attractive for quantum computing because: (1) Topological protection — qubit information is stored non-locally (jointly in both MZMs) and immune to local perturbations; error rate scales as exp(−L/ξ). (2) Non-Abelian braiding statistics — exchanging two MZMs implements a quantum gate that depends on the sequence of exchanges (not the precise path), making gate operations inherently topologically protected. This could, in principle, eliminate the need for active quantum error correction entirely.

**Answer 12:**

Topological protection refers to the immunity of quantum information stored in global, non-local topological properties of a quantum system against local perturbations. In a Majorana qubit, the qubit information (fermion parity) is stored jointly in two spatially separated endpoints of a topological superconducting wire. Any local perturbation at one endpoint cannot flip the parity because doing so would require simultaneously affecting both endpoints (which are macroscopically separated by the wire length L). The error suppression is exponential: error rate ∝ exp(−L/ξ), where ξ is the coherence length. For L = 2 μm and ξ = 100 nm: suppression = exp(−20) ~ 2×10⁻⁹ — nine orders of magnitude reduction in error rate. This is fundamentally different from all other qubit types where local perturbations can directly flip the qubit state.

**Answer 13:**

The Rydberg blockade condition: if V(R) = C₆/R⁶ >> ℏΩ (where Ω is the Rabi frequency for Rydberg excitation), exciting atom A to |r⟩ energetically shifts atom B's Rydberg transition out of resonance by V(R), preventing double excitation. Van der Waals interaction formula: V(R) = C₆/R⁶, where C₆ ∝ n¹¹ (n = principal quantum number, n ~ 50–100 for Rydberg states). Blockade radius Rb = (C₆/ℏΩ)^(1/6): atoms within Rb experience blockade. Typical values: Rb ~ 5–15 μm for Ω/(2π) ~ 1–10 MHz. This enables two-qubit gates with all-to-all connectivity (any pair within blockade range), and reconfigurable connectivity by choosing which atoms to address with the Rydberg excitation laser.

**Answer 14:**

Optical tweezers in neutral atom quantum computers serve multiple essential functions: (1) Qubit trapping — tightly focused laser beams (typical power ~100 mW, waist ~1 μm) trap individual atoms at intensity maxima via the optical dipole force with trap depth ~1 mK; (2) Array creation — splitting laser beams using spatial light modulators (SLMs) or acousto-optic deflectors (AODs) creates programmable 2D/3D arrays of up to 1000+ atoms; (3) Reconfigurability — atom positions can be rearranged in real time (~10 ms) by moving tweezer beams, changing the qubit connectivity graph to match the algorithm; (4) Individual addressing — focused gate laser beams address specific atoms for qubit operations; (5) Sorting — after stochastic loading from a background MOT, real-time rearrangement sorts atoms into desired configurations for computation.

**Answer 15:**

Photonic qubits most naturally support room-temperature operation. At optical frequencies (400–800 THz), the mean thermal photon number is nTh = 1/(exp(ℏω/kBT)−1) ≈ exp(−ℏω/kBT) ~ 10⁻³² at room temperature — thermal optical photons are essentially absent. Therefore photonic quantum states can be prepared and manipulated at room temperature without any thermal noise contribution. No cryogenic cooling is needed. All other qubit types require cooling: superconducting (20 mK), silicon spin (~100 mK), and neutral atoms (μK via laser cooling, though the vacuum chamber operates at room temperature). Photons are also the only "flying" qubits — they propagate at the speed of light over long distances, making them uniquely suitable for quantum communication regardless of computation temperature.

## Solved Examples — Chapter 2

<div class="box box-example">
<p class="box-title"><strong>Example 2.1  HOM Visibility and Photon Indistinguishability</strong></p>
<p>Problem: An HOM dip experiment measures C_max = 500 coincidences/s and C_min = 25 coincidences/s. Find the HOM visibility and the fraction of distinguishable photon pairs.</p>
<p>Solution: V = (C_max − C_min)/C_max = (500 − 25)/500 = 475/500 = 0.95 = 95%</p>
<p>For perfectly indistinguishable photons: V = 1. For classically distinguishable: V = 0.</p>
<p>Indistinguishable fraction:</p>
<p>Or equivalently, 5% of photon pairs contribute distinguishable photons (different frequencies, polarisations, or arrival times) — these contribute to the residual coincidences.</p>
<p>Implication: for a linear optical CZ gate requiring V &gt; 99%, this source needs factor ~4× improvement in photon indistinguishability.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.2  Boson Sampling Permanent Complexity</strong></p>
<p>Problem: Estimate the classical computation time for an n = 50 boson sampling experiment using Ryser's algorithm (O(n·2ⁿ) operations) on a classical supercomputer at 10¹⁸ FLOPs/s.</p>
<p>Solution: Operations = n·2ⁿ = 50 × 2⁵⁰ ≈ 50 × 1.13×10¹⁵ = 5.63×10¹⁶ operations.</p>
<p>Time = 5.63×10¹⁶ / 10¹⁸ FLOPs/s = 0.056 seconds — still tractable for n=50!</p>
<p>For n = 100: 100 × 2¹⁰⁰ ≈ 1.27×10³² operations → 1.27×10¹⁴ seconds (~4 million years).</p>
<p>Takeaway: quantum advantage in boson sampling requires n ~ 50–100+ photons with low loss, emphasising why photon loss and source efficiency are the key challenges.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.3  Silicon Spin Qubit Zeeman Splitting</strong></p>
<p>Problem: A silicon spin qubit has g-factor g = 1.998 (measured). Find (a) qubit frequency at B = 1 T, and (b) the temperature required for kBT &lt;&lt; ΔE/10 (cold enough for spin polarisation).</p>
<p>Solution: (a) ΔE = gμBB = 1.998 × 9.274×10⁻²⁴ × 1.0 = 1.853×10⁻²³ J</p>
<p>f = ΔE/h = 1.853×10⁻²³/(6.626×10⁻³⁴) = 27.96 GHz ≈ 28 GHz</p>
<p>(b) For kBT &lt;&lt; ΔE/10: T &lt;&lt; ΔE/(10kB) = 1.853×10⁻²³/(10×1.38×10⁻²³) = 134 mK.</p>
<p>At 100 mK (standard dilution fridge temperature), kBT = 8.6 μeV &lt;&lt; ΔE = 115 μeV — sufficient polarisation.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.4  Rydberg Blockade Radius</strong></p>
<p>Problem: For ⁸⁷Rb Rydberg state with n = 70, C₆ = 862 GHz·μm⁶, and Rabi frequency Ω/(2π) = 2 MHz, find the blockade radius Rb.</p>
<p>Solution:</p>
<p>C₆/Rb⁶ = ℏΩ → Rb = (C₆/(ℏΩ))^(1/6)</p>
<p>ℏΩ = ℏ × 2π × 2×10⁶ s⁻¹ = 8.37×10⁻²⁷ J (in units: ℏΩ/h = Ω/2π = 2 MHz = 2×10⁶ Hz)</p>
<p>In GHz: ℏΩ/h = 2×10⁻³ GHz; C₆/ℏΩ = 862/0.002 = 431,000 μm⁶</p>
<p>Rb = (431,000)^(1/6) μm = 8.9 μm</p>
<p>Physical insight: atoms within Rb = 8.9 μm experience blockade. Typical tweezer spacing of 5–10 μm is within the blockade radius, enabling reliable gate operations.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.5  Topological Protection — Error Rate Scaling</strong></p>
<p>Problem: A topological qubit in a Majorana wire of length L = 2 μm has coherence length ξ = 100 nm. Estimate the error rate from local perturbations compared to a physical qubit error rate of 0.1%.</p>
<p>Solution: Error rate suppression ~ exp(−L/ξ) = exp(−2000nm/100nm) = exp(−20) ≈ 2×10⁻⁹.</p>
<p>Physical qubit error rate = 10⁻³; Topological qubit error rate ~ 10⁻³ × 2×10⁻⁹ = 2×10⁻¹² — 9 orders of magnitude better!</p>
<p>Caveat: This assumes perfect wire uniformity and no long-range perturbations. In practice, disorder, finite-temperature effects, and quasi-particle poisoning reduce this advantage. Still, even 4–5 orders of magnitude suppression would make topological qubits effectively error-free without active QEC.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.6  Exchange Gate Speed vs Superconducting CZ</strong></p>
<p>Problem: A silicon spin exchange gate has J/(2π) = 100 MHz and achieves SWAP in time τ_SWAP = h/(2J). Compare to IBM Heron CZ at 100 ns. How many sequential SWAP gates fit within T₂ = 1 ms?</p>
<p>Solution: τ_SWAP = h/(2J) = 1/(2×100×10⁶ Hz) = 5 ns.</p>
<p>Compare: IBM Heron CZ ~ 100 ns → spin exchange SWAP is 20× faster!</p>
<p>Gate budget: N = T₂/τ = 1×10⁻³ s / 5×10⁻⁹ s = 200,000 sequential SWAP gates.</p>
<p>Implication: silicon spin qubits combine the fast gates of superconducting systems with coherence times approaching trapped ions — ideal for fault-tolerant surface code error correction.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.7  KLM Overhead Estimate</strong></p>
<p>Problem: The KLM scheme achieves gate success probability P = (1 − 1/N²) using N ancilla photons. For P = 99%, find N and total photon number for a 100-gate quantum circuit.</p>
<p>Solution:</p>
<p>Total photons per gate = 2 data + 10 ancilla = 12 photons.</p>
<p>For 100 gates: ~1200 photons needed, but photons are consumed probabilistically.</p>
<p>PsiQuantum's fusion-based approach reduces this overhead significantly using resource states and topological codes, targeting ~10,000 physical photonic components per logical qubit.</p>
</div>

<div class="box box-example">
<p class="box-title"><strong>Example 2.8  Quantum Volume Comparison — Six Platforms</strong></p>
<p>Problem: Estimate QV for (a) silicon spin qubit (12 qubits, F₂Q = 99.5%), (b) neutral atom (20 qubits, F₂Q = 99.5%), (c) photonic (10 modes, per-gate F = 98%). Which platform achieves highest QV at equal qubit count?</p>
<p>Solution: Using QV = 2ⁿ where (F₂Q)^(n²/2) &gt; 0.67:</p>
<p>(c) Photonic at 98%: n_max where (0.98)^(n²/2) &gt; 0.67 → n² &lt; 2ln(0.67)/ln(0.98) ~ 38 → n_max = 6, QV = 64.</p>
<p>At equal qubit count, trapped ions and neutral atoms win on QV due to highest fidelity. Photonic platforms face the greatest challenge for high-QV metrics due to per-gate loss.</p>
</div>

## Multiple Choice Questions — Chapter 2

*Instructions: Select the single best answer. Answers at end of section.*

**1. The KLM theorem uses which combination of resources for universal quantum computation?**

- (A) Josephson junctions and superconducting cavities

- (B) Linear optics, single-photon sources, PNR detectors, and classical feed-forward

- (C) Rydberg atoms and optical tweezers only

- (D) Majorana fermions and topological insulators

**2. In dual-rail encoding, the state |1⟩ of the photonic qubit is represented as:**

- (A) Two photons in mode a

- (B) One photon in mode a, vacuum in mode b: |1,0⟩

- (C) Vacuum in mode a, one photon in mode b: |0,1⟩

- (D) Vacuum in both modes

**3. The Hong-Ou-Mandel effect at a 50:50 beam splitter results in:**

- (A) Each photon exiting from a random port independently

- (B) Both photons always exiting from the same output port (photon bunching)

- (C) Photon anti-bunching — they always exit from different ports

- (D) Polarisation rotation of both photons

**4. Boson sampling output probabilities are proportional to the square of the:**

- (A) Matrix determinant

- (B) Matrix trace

- (C) Matrix permanent

- (D) Matrix hafnian (for standard Fock state input)

**5. EDSR in silicon spin qubits uses which field for single-qubit control?**

- (A) Static magnetic field only

- (B) Oscillating magnetic field from a local antenna

- (C) Oscillating electric field coupled via spin-orbit interaction

- (D) Laser pulses in the optical domain

**6. The Rydberg van der Waals interaction C₆/R⁶ — the coefficient C₆ scales with principal quantum number n as:**

- (A) n²

- (B) n⁷

- (C) n¹¹

- (D) n⁴

**7. Majorana zero modes appear at the ends of:**

- (A) Silicon quantum dots

- (B) Topological superconducting nanowires (e.g., InAs/Al hybrid)

- (C) NV centres in diamond

- (D) Rydberg photonic waveguides

**8. The exchange interaction H\_ex = J(t)·S₁·S₂ used in silicon spin qubits implements:**

- (A) Single-qubit EDSR gates

- (B) SWAP or √SWAP two-qubit entangling gates

- (C) Rydberg blockade

- (D) Dispersive readout

**9. QuEra's 2023 demonstration of 48 logical qubits below break-even used how many physical atoms?**

- (A) 48

- (B) 96

- (C) 228

- (D) 256

**10. Gaussian Boson Sampling uses which type of input state (rather than single-photon** Fock states)?

- (A) Coherent states (laser)

- (B) Thermal states

- (C) Squeezed vacuum states

- (D) Cat states (Schrödinger cats)

**11. Topological protection of Majorana qubits arises because:**

- (A) Active quantum error correction is applied continuously

- (B) Qubit information is encoded non-locally in the joint parity of two spatially separated MZMs

- (C) The qubit operates at absolute zero temperature

- (D) Majorana particles are intrinsically stable due to their mass

**12. Optical tweezer reconfiguration time in neutral atom processors is approximately:**

- (A) 1 nanosecond

- (B) 1 microsecond

- (C) 10 milliseconds

- (D) 1 second

**13. Which qubit platform naturally enables room-temperature operation without any cryogenic equipment?**

- (A) Superconducting qubits (require 20 mK)

- (B) Silicon spin qubits (require ~100 mK)

- (C) Photonic qubits (photons travel and interact at room temperature)

- (D) Trapped ions (require room temp, but need UHV and laser cooling)

**14. Time-bin encoding is preferred over polarisation encoding for long-distance fibre QKD because it is:**

- (A) Faster to encode and decode

- (B) Immune to polarisation mode dispersion (PMD) in optical fibres

- (C) Compatible with standard CMOS detectors

- (D) Cheaper to implement with off-the-shelf components

**15. Matrix permanent computation is classified in which computational complexity class?**

- (A) P (polynomial time, tractable)

- (B) NP-complete (decision version)

- (C) #P-hard (computationally harder than NP-complete — counting problem)

- (D) BQP (efficiently solvable on quantum computers)

## MCQ Answers — Chapter 2

Q1: B  |  Q2: C  |  Q3: B  |  Q4: C  |  Q5: C  |  Q6: C  |  Q7: B  |  Q8: B  |  Q9: C  |  Q10: C  |  Q11: B  |  Q12: C  |  Q13: C  |  Q14: B  |  Q15: C

## Unsolved Problems — Chapter 2

2.1  A beam splitter has reflectivity R = 1/3. Write its 2×2 unitary matrix and verify unitarity. What single-qubit rotation does it implement in the dual-rail basis?

*[Ans: U = [[√(2/3), i/√3],[i/√3, √(2/3)]]; U†U = I (verified). Implements Rᵧ rotation by θ = arccos(2/3) ≈ 48.2°]*

2.2  In GBS with squeezing parameter r = 1.0 (mean photon number n̄ = sinh²(r)), calculate (a) n̄ per mode and (b) P(detecting exactly 2 photons in a single mode).

*[Ans: (a) n̄ = sinh²(1.0) = 1.381; (b) P(2) = n̄²/(2(n̄+1)³) = 1.907/(2×2.381³) = 0.071 = 7.1%]*

2.3  A silicon spin qubit in natural Si has T₂\* = 100 ns at B = 0.5 T. (a) Find the Larmor frequency for g = 2. (b) How many Larmor oscillations occur within one T₂\*?

*[Ans: (a) f\_L = gμ\_B B/h = 2×9.274×10⁻²⁴×0.5/6.626×10⁻³⁴ = 14.0 GHz; (b) N = f\_L×T₂\* = 14×10⁹×100×10⁻⁹ = 1400 oscillations]*

2.4  Two ⁸⁷Rb atoms with n=60 have C₆ = 1.6×10¹¹ GHz·μm⁶ separated by R = 8 μm. Calculate (a) U\_vdW and (b) required Rabi frequency Ω for blockade condition.

*[Ans: (a) U/h = C₆/R⁶ = 1.6×10¹¹/8⁶ = 610 MHz; (b) For blockade: Ω << U/h = 610 MHz — blockade is strong for all practical Rabi frequencies (typically Ω/(2π) ~ 5 MHz)]*

2.5  For Majorana braid operator B₁₂ = exp(−iπZ/4), compute B₁₂² and show it implements the Z gate (up to global phase).

*[Ans: B₁₂² = exp(−iπZ/2) = cos(π/2)·I − i·sin(π/2)·Z = −iZ; modulo global phase −i this is the Z gate. Non-Abelian: B₁₂·B₁₃ ≠ B₁₃·B₁₂.]*

2.6  A linear optical CNOT (KLM) has success probability p = 1/4. For a 10-gate circuit (all CNOTs), what total gate attempts are needed on average? Comment on why this makes direct KLM impractical.

*[Ans: (1/p)¹⁰ = 4¹⁰ = 1,048,576 total attempts on average — over one million attempts for just 10 gates! This exponential overhead makes direct gate-by-gate KLM completely impractical; cluster state / measurement-based approaches are essential.]*

2.7  A neutral atom tweezer at λ = 852 nm has detuning Δ/(2π) = 7.2 THz from the ⁸⁷Rb D2 line (Γ/(2π) = 6 MHz, Ω\_trap/(2π) = 50 MHz). Calculate the photon scattering rate Γ\_sc = (Ω\_trap/Δ)²·Γ/2 and T₂ = 1/Γ\_sc.

*[Ans: Γ\_sc = (50×10⁶/7.2×10¹²)²×3×10⁶ = (6.94×10⁻⁶)²×3×10⁶ = 1.45×10⁻⁴ s⁻¹; T₂ = 6900 s ≈ 2 hours — far-detuned tweezers give coherence times of thousands of seconds]*

2.8  Intel's Tunnel Falls chip: 12 spin qubits on 100 μm² area. If this density filled a 300-mm wafer (area π×(150 mm)² = 7.07×10¹⁰ μm²), how many qubits?

*[Ans: Density = 0.12 qubits/μm². Total = 0.12×7.07×10¹⁰ = 8.5 billion qubits. Theoretical promise is extraordinary, but wiring density, control electronics crosstalk, and refrigerator constraints are formidable practical limits.]*

2.9  Compare photon loss error for (a) 50 km fibre (0.2 dB/km) and (b) 1 m silicon photonic waveguide (loss = 1 dB/cm). Which is better for QC vs. QKD?

*[Ans: (a) 10 dB; η = 10%; error = 90% — usable for QKD with quantum repeaters, not for computation; (b) 100 dB; η = 10⁻¹⁰ ≈ 0 — catastrophic for any application. Si₃N₄ waveguides with <0.001 dB/cm needed for on-chip photonic QC]*

2.10  100 physical neutral atom qubits encode logical qubits using [[7,1,3]] Steane code. (a) How many logical qubits? (b) If physical error p = 0.1% and threshold p\_th = 1%, estimate p\_L using p\_L ~ (p/p\_th)^{(d+1)/2}.

*[Ans: (a) 100/7 = 14 logical qubits; (b) p\_L = (0.001/0.01)^{(3+1)/2} = (0.1)² = 0.01% per logical gate — already 10× better than physical! At p = 0.01%: p\_L = (0.001)² = 10⁻⁶ — excellent logical performance.]*

## Theory Questions — Chapter 2

- 1.  Describe the three main photonic qubit encodings — polarisation, dual-rail, and time-bin — and compare their advantages for (a) gate operations, (b) long-distance fibre transmission, and (c) integration on photonic chips. Which encoding is used by Xanadu's Borealis and why?

- 2.  State and explain the KLM theorem. What does it mean for linear optics to be 'universal for quantum computation'? Explain the role of feed-forward and why it is essential. Why is the direct KLM approach not practical despite being theoretically universal? What alternative approaches have superseded it?

- 3.  Define the matrix permanent and explain why its computation is #P-hard. Why does this hardness make boson sampling a candidate for demonstrating quantum computational advantage? What are the current limitations and controversies in boson sampling experiments, particularly regarding classical simulation algorithms?

- 4.  Describe the physics of a silicon spin qubit in a quantum dot. Explain (a) how single-qubit gates are implemented via EDSR using a micromagnet gradient; (b) how the exchange interaction gate works and why it requires barrier gate voltage control; (c) why isotopic purification of silicon improves T₂\* by ~10,000×. What are the three biggest remaining challenges for silicon spin qubits?

- 5.  Explain the concept of topological quantum computing and Majorana zero modes. Define non-local qubit encoding and explain why it provides intrinsic protection against local errors. What is meant by 'non-Abelian anyonic statistics' and why is braiding important for implementing topologically protected quantum gates?

- 6.  Derive the Rydberg blockade radius R\_b from the condition U\_vdW(R\_b) = ℏΩ for ⁸⁷Rb atoms at n=70. Explain physically why the blockade implements a controlled-phase gate. Describe the complete pulse sequence for a Rydberg CZ gate between two atoms in separate tweezers.

- 7.  Explain the concept of atom sorting in optical tweezer arrays and why it is necessary. Describe the complete feedback loop: imaging to detect atom positions, algorithm to calculate target moves, and physical atom movement using AOD-controlled tweezers. What are the time and fidelity constraints and how do they affect the duty cycle of the quantum processor?

- 8.  The 2023 Harvard-QuEra experiment demonstrated 48 logical qubits with 'below break-even' error rates in a neutral atom processor. Explain what 'below break-even' means quantitatively, what quantum error correction code was used, and what this milestone implies for the timeline to practical fault-tolerant quantum computing.

- 9.  Compare the scalability approaches of PsiQuantum (photonic), Intel (silicon spin), and QuEra (neutral atom). What is each company's stated path to one million qubits? What are the key engineering milestones required for each, and what are the most significant physical and engineering obstacles?

- 10.  Critically evaluate the claim that topological qubits are 'the only viable path to quantum computing at scale'. What experimental milestones would constitute definitive proof of topological qubit operation (specifically, non-Abelian braiding statistics)? What is the current state of experimental evidence as of 2024, and what does the retraction of the 2018 Delft Nature paper tell us about the challenges?

## Assignments — Chapter 2

### Assignment 2.1 Platform Comparison Analysis (Marks: 10)

Choose any two platforms from: photonic, neutral atom, silicon spin, or topological qubits. Write a 2000-word comparative analysis including: (a) a table of current performance metrics with citations from primary literature (2020–2024); (b) a sample quantum circuit (Bell state or GHZ state) showing implementation on each platform with all hardware constraints; (c) one specific application domain where each platform has an advantage; (d) the three biggest engineering challenges remaining for each platform.

### Assignment 2.2 Boson Sampling Classical Simulation (Marks: 10)

Using Python (NumPy): (a) Generate a random 5×5 unitary matrix using QR decomposition of a complex Gaussian matrix. (b) Compute boson sampling output probabilities for all configurations of 3 photons in 5 modes using the Ryser formula for matrix permanents. (c) Sample from this distribution 1000 times. (d) Plot computed vs. sampled probabilities as a bar chart. (e) Measure execution time vs. photon number n = 2,3,4,5 and plot the scaling.

### Project Suggestion 2.A Neutral Atom Quantum Optimisation

Using QuEra's Aquila processor via Amazon Braket SDK (pip install amazon-braket-sdk), implement analog quantum annealing for the Maximum Independent Set (MIS) problem. Map a graph with 10–15 nodes to a 2D atom array (atoms on nodes, blockade radius covering edges). Run Rydberg Hamiltonian evolution for different adiabatic schedules. Measure success probability vs. schedule duration. Compare with classical brute-force solution. Write a 3000-word analysis.

### Project Suggestion 2.B Silicon Spin Qubit QuTiP Simulation

Using QuTiP (pip install qutip), simulate the double quantum dot system H = ε·σ\_z + J(t)·σ\_x (detuning + exchange). (a) Compute charge stability diagram (energy levels vs. ε). (b) Simulate a √SWAP gate via pulsed J(t). (c) Add Gaussian charge noise (random ε fluctuations) and compute T₂\* via ensemble averaging. (d) Apply spin echo sequence and compute T₂\_echo. Submit all code and a 2500-word report.

### Project Suggestion 2.C Photonic Circuit Design with Perceval

Install Quandela's Perceval simulator (pip install perceval-quandela). (a) Implement a 2-photon HOM experiment — simulate coincidence counts vs. temporal delay and plot the HOM dip. (b) Design a linear optical circuit for Bell state generation using a beam splitter, wave plates, and post-selection. (c) Implement a post-selected CNOT gate using 4 photons (2 data + 2 ancilla) and measure the success probability. (d) Explore how photon indistinguishability V < 1 degrades HOM visibility and gate fidelity. Write a 2000-word analysis.

## References and Further Reading

1. Krantz, P., Kjaergaard, M., Yan, F., Orlando, T. P., Gustavsson, S., & Oliver, W. D. (2019). A Quantum Engineer's Guide to Superconducting Qubits. Applied Physics Reviews, 6(2). [Josephson junctions, transmon design, DRAG pulses]

2. Blais, A., Grimsmo, A. L., Girvin, S. M., & Wallraff, A. (2021). Circuit Quantum Electrodynamics. Reviews of Modern Physics, 93(2). [Circuit QED, dispersive readout]

3. Bruzewicz, C. D., Chiaverini, J., McConnell, R., & Sage, J. M. (2019). Trapped-Ion Quantum Computing: Progress and Challenges. Applied Physics Reviews, 6(2). [Paul traps, laser cooling, Molmer-Sorensen gates]

4. Wineland, D. J. (2013). Nobel Lecture: Superposition, Entanglement, and Raising Schrodinger's Cat. Reviews of Modern Physics, 85(3). [Foundational trapped-ion manipulation]

5. Kok, P., Munro, W. J., Nemoto, K., Ralph, T. C., Dowling, J. P., & Milburn, G. J. (2007). Linear Optical Quantum Computing with Photonic Qubits. Reviews of Modern Physics, 79(1). [KLM scheme, photonic qubits]

6. Hendrickx, N. W. et al. (2021). A Four-Qubit Germanium Quantum Processor. Nature, 591. [Spin qubits in semiconductor quantum dots]

7. Nayak, C., Simon, S. H., Stern, A., Freedman, M., & Das Sarma, S. (2008). Non-Abelian Anyons and Topological Quantum Computation. Reviews of Modern Physics, 80(3). [Majorana modes, topological qubits]

8. Saffman, M., Walker, T. G., & Molmer, K. (2010). Quantum Information with Rydberg Atoms. Reviews of Modern Physics, 82(3). [Neutral atom arrays, Rydberg blockade]

9. IBM Quantum (2024). IBM Quantum Roadmap and Heron/Condor Processor Technical Notes. https://www.ibm.com/quantum/roadmap [Superconducting qubit scaling]

10. IonQ & Quantinuum (2024). Trapped-Ion System Technical Specifications and Benchmarking Reports. [Industrial trapped-ion hardware]

<img class="fig-img" src="content/images/image30.png" alt="figure">
