# A–Z Index of Terms

**A**

**AC Josephson effect —** §1.2.2 (Ch.1) — V applied → phase oscillates → supercurrent at f = V/Φ₀ = 483,597.9 GHz/V

**ADAPT-VQE (Adaptive VQE) —** §9.3.2 (Ch.9) — ansatz grows operator by operator selecting largest gradient; avoids barren plateaus by construction

**AFC (Atomic Frequency Comb) protocol —** §7.5.3 (Ch.7) — comb tooth spacing Δ → storage time τ = 1/Δ; Er:YSO at 1532 nm; T₂ > 6 hours (nuclear)

**Amplitude-damping channel —** §3.2.1 (Ch.3) — Kraus: K₀=[[1,0],[0,√(1−γ)]], K₁=[[0,√γ],[0,0]]; models T₁ relaxation

**Anharmonicity α (transmon) —** §1.3.2 (Ch.1) — α = E₁₂−E₀₁ ≈ −EC ≈ −200 to −300 MHz; enables selective qubit addressing; essential for 2-level qubit

**Ansatz (VQE) —** §9.3.2 (Ch.9) — parameterised quantum circuit U(θ); types: HEA, UCCSD, ADAPT-VQE

**B**

**BBPSSW distillation —** §8.2.2 (Ch.8) — F' = F²/(F²+(1−F)²); two noisy pairs → one higher-fidelity pair; first entanglement distillation protocol

**BB84 QKD protocol —** §5.2 (Ch.5) — Bennett-Brassard 1984; two non-orthogonal bases; QBER < 11% → secure key extraction

**BCS superconductivity —** §1.2.1 (Ch.1) — Cooper pairs in condensate; macroscopic phase φ; BCS gap 2Δ; Josephson effect basis

**BK (Bravyi-Kitaev) mapping —** §9.2.3 (Ch.9) — hierarchical binary tree parity; average Pauli weight O(log₂M) vs O(M/2) for JW

**Barren plateau problem —** §9.3.5 (Ch.9) — gradient variance ∝ 2^(−n) for random deep circuits; mitigation: ADAPT-VQE, layer initialisation

**Bell inequalities / CHSH —** §5.3 (Ch.5) — |S| ≤ 2 classical; |S| ≤ 2√2 quantum (Tsirelson); S > 2 certifies entanglement

**Bloch sphere / Bloch vector —** §3.2.1 (Ch.3) — ρ = (I + r⃗·σ⃗)/2; r⃗ = (⟨X⟩,⟨Y⟩,⟨Z⟩); |r⃗|=1 pure, <1 mixed

**Boson sampling —** §2.3 (Ch.2) — output ∝ |Perm(U)|²; #P-hard classically; GBS uses squeezed states + hafnians

**Bravyi-Kitaev mapping —** §9.2.3 — see BK mapping

**C**

**CCSD(T) — gold standard —** §9.1.1 (Ch.9) — coupled cluster singles, doubles, perturbative triples; O(M⁷); ~0.3 kcal/mol error; fails FeMoco

**CHSH inequality —** §5.3 (Ch.5) — see Bell inequalities

**Chemical accuracy —** §9.1.1 (Ch.9) — 1 kcal/mol = 1.6 mHa; threshold for drug discovery and reaction rate reliability

**Circuit QED (cQED) —** §1.6 (Ch.1) — Jaynes-Cummings coupling; dispersive limit χ = g²/Δ; QND qubit readout via resonator frequency shift

**CNOT / CX gate —** §1.5 (Ch.1) — two-qubit entangling gate; implemented via CR gate in IBM; native entangling gate for many platforms

**Cooper pair box (CPB) —** §1.3.1 (Ch.1) — EC >> EJ regime; charge-noise sensitive; predecessor to transmon; T₂ ~ ns

**Cooper pairs —** §1.2.1 (Ch.1) — electron pairs bound via phonon-mediated interaction; BCS ground state; carry supercurrent

**CPTP map —** §3.2.1 (Ch.3) — completely positive trace-preserving; Kraus: E(ρ) = ΣₖKₖρKₖ†; Σₖ Kₖ†Kₖ = I

**Cross-Resonance (CR) gate —** §1.5.2 (Ch.1) — IBM native 2Q gate; drive control at target frequency; ZX interaction → CNOT

**CRYSTALS-Dilithium (ML-DSA) —** §5.5, §10.3.3 (Ch.5/10) — FIPS 204; digital signature; M-LWE + M-SIS lattice; code signing, TLS certificates

**CRYSTALS-Kyber (ML-KEM) —** §5.5, §10.3.3 (Ch.5/10) — FIPS 203; KEM; Module-LWE; TLS session keys; deployed Chrome 2023

**CZ gate —** §1.5.1 (Ch.1) — diag(1,1,1,−1); conditional phase −1 on |11⟩; implemented via flux tuning avoided crossing

**D**

**DC Josephson effect —** §1.2.2 (Ch.1) — I = Ic sin(δ); zero-voltage supercurrent; macroscopic quantum tunnelling of Cooper pairs

**Density matrix ρ —** §3.2.1 (Ch.3) — ρ = (I + r⃗·σ⃗)/2; Tr(ρ²) = 1 (pure); Bloch vector r⃗; positive semidefinite Hermitian

**Depolarising channel —** §3.2.1 (Ch.3) — E(ρ) = (1−p)ρ + (p/3)(XρX+YρY+ZρZ); r⃗ → (1−4p/3)r⃗; symmetric Pauli channel

**Dephasing channel —** §3.2.1 (Ch.3) — E(ρ) = (1−pφ)ρ + pφZρZ†; rz preserved; rx,ry shrink; models pure dephasing

**Dispersive readout —** §1.6 (Ch.1) — resonator shifts ±χ = ±g²/Δ; QND; homodyne detection; F > 99% on IBM 2024

**DiVincenzo criteria —** §1.1 (Ch.1) — scalability, initialisation, coherence, universal gates, measurement; five necessary conditions

**DLCZ protocol —** §7.5.1 (Ch.7) — cold Rb ensemble; write Stokes + read anti-Stokes; T₂ ~ 1–100 ms; 780 nm

**Doppler cooling —** §1.9.1 (Ch.1) — radiation pressure; T\_D = ℏΓ/(2kB); limit n̄ ~ 10–30; precursor to sideband cooling

**DRAG pulse shaping —** §1.4.2 (Ch.1) — Ωᵧ = β·dΩ/dt; β = −1/α; suppresses |2⟩ leakage; reduces leakage 10–100×

**E**

**EDSR (Electric Dipole Spin Resonance) —** §2.4.2 (Ch.2) — oscillating electric field + spin-orbit coupling → spin rotation; silicon spin qubit single-qubit gate

**E91 protocol —** §5.3 (Ch.5) — Ekert 1991; entangled pairs; Bell inequality violation certifies security; Micius: |S| = 2.37

**Electronic structure problem —** §9.1.1 (Ch.9) — dim(H) = C(M,N) ≈ 2^M; FCI exact but exponential; classical hierarchy fails at M~50

**Entanglement distillation —** §8.2 (Ch.8) — LOCC protocol: n low-F pairs → m < n high-F pairs; BBPSSW: F' = F²/(F²+(1−F)²)

**Entanglement swapping —** §7.3 (Ch.7) — BSM at intermediate node; Pauli corrections; creates A–C entanglement without A–C interaction

**EPC (Error Per Clifford) —** §3.3.1 (Ch.3) — EPC = (1−p)/2; from RB decay rate p; IBM Heron ~0.1%

**Er:YSO memory —** §7.5.3 (Ch.7) — erbium-doped YSO; 1532 nm telecom; AFC τ = 1/Δ; nuclear T₂ > 6 hours (4K)

**Exchange gate (silicon spin) —** §2.4.3 (Ch.2) — H\_ex = J(t)·S₁·S₂; SWAP or √SWAP; t ~ 5–10 ns; nearest-neighbour connectivity

**F**

**Fault-tolerant quantum computing —** §10.9 (Ch.10) — T₂/t\_gate > 10⁴; QEC; surface code ~10⁶ physical qubits for RSA-2048

**FeMoco (iron-molybdenum cofactor) —** §9.1.1 (Ch.9) — nitrogenase active site; 54 active electrons; CCSD(T) intractable; key QPE target

**Flux quantum Φ₀ —** §1.2.2 (Ch.1) — Φ₀ = h/(2e) = 2.068×10⁻¹⁵ Wb; magnetic flux quantum; international volt standard

**Fock space —** §9.2.1 (Ch.9) — occupation number basis |n₀,...,n\_{M-1}⟩; nₚ ∈ {0,1}; 2^M states = M-qubit Hilbert space

**G**

**Gauge freedom (GST) —** §3.4 (Ch.3) — transformation T: ρ→T⁻¹ρ, E→ET, Gᵢ→TGᵢT⁻¹; unobservable; fixed by gauge optimisation

**Gaussian Boson Sampling (GBS) —** §2.3 (Ch.2) — squeezed vacuum inputs; hafnian probabilities; Borealis 2022 quantum advantage claim

**GST (Gate Set Tomography) —** §3.4 (Ch.3) — simultaneous characterisation of gates+SPAM; germ amplification; SPAM-free; PyGSTi

**H**

**Hartree-Fock (HF) —** §9.1.1 (Ch.9) — mean-field; O(M⁴); ~30–50 kcal/mol error; fails for multi-reference/strongly correlated

**HNDL (Harvest Now Decrypt Later) —** §10.3.2 (Ch.10) — intercept now, decrypt later with QC; data at risk: medical, military, IP, identity

**HOM effect (Hong-Ou-Mandel) —** §2.2.4 (Ch.2) — a†b†|vac⟩ → (|2,0⟩−|0,2⟩)/√2; photon bunching; requires indistinguishable photons

**Hubbard model —** §9.6.2 (Ch.9) — H = −tΣhop + UΣnᵢ↑nᵢ↓; QMC sign problem at finite doping; high-Tc target

**I**

**IBM Quantum Network —** §10.2.2 (Ch.10) — 400+ partner organisations; cloud access; Qiskit open-source; Eagle/Heron/Condor processors

**Information-theoretic security —** §5.1 (Ch.5) — secure against unlimited computing including QC; physical laws not mathematics

**IQ modulation —** §1.4.3 (Ch.1) — s(t)=I(t)cos(ωct)−Q(t)sin(ωct); arbitrary pulse shape synthesis; OpenPulse interface

**IonQ Forte —** §1.11 (Ch.1) — ¹⁷¹Yb⁺; 35 algorithmic qubits; all-to-all; QV > 4M; Nasdaq IONQ

**J**

**Josephson energy EJ —** §1.2.3 (Ch.1) — EJ = Ic·Φ₀/(2π); energy scale of JJ; EJ >> EC (transmon); cosine potential

**Josephson inductance LJ —** §1.2.3 (Ch.1) — LJ(δ) = LJ0/cosδ; LJ0 = Φ₀/(2πIc); non-linear → anharmonic energy levels

**Jordan-Wigner (JW) mapping —** §9.2.2 (Ch.9) — aₚ† → (Xₚ−iYₚ)/2⊗Z\_{p-1}⊗...⊗Z₀; Z-string encodes antisymmetry; weight O(M/2)

**K**

**KLM theorem —** §2.2.3 (Ch.2) — Knill-Laflamme-Milburn; linear optics+SPD+detectors→universal QC; feed-forward; Nature 2001

**Kraus operators —** §3.2.1 (Ch.3) — E(ρ) = ΣₖKₖρKₖ†; ΣₖKₖ†Kₖ=I; quantum channel representation

**Kyber — see CRYSTALS-Kyber —** §5.5 (Ch.5)

**L**

**Lamb-Dicke parameter η —** §1.10.2 (Ch.1) — η = k·x\_zpf; k = laser wavevector; x\_zpf = zero-point motion; used in MS gate

**Leakage (transmon) —** §3.2.5 (Ch.3) — population of |2⟩, |3⟩,...; suppressed by DRAG; monitored by leakage RB

**Leftover Hash Lemma —** §5.2.4 (Ch.5) — privacy amplification foundation; ε-close to uniform; ℓ ≤ H\_∞ − 2log₂(1/ε)

**LOCC (Local Operations and Classical Communication) —** §8.2 (Ch.8) — only allowed operations for entanglement distillation; cannot send quantum systems

**M**

**M3 (Matrix-free Measurement Mitigation) —** §4.5.2 (Ch.4) — sparse calibration; O(n·s) vs O(2ⁿ); scalable for n > 15 qubits

**Majorana zero modes (MZMs) —** §2.5.1 (Ch.2) — γ† = γ; appear at topological SC wire ends; non-local qubit encoding; non-Abelian braiding

**Markowitz portfolio optimisation —** §9.5.1 (Ch.9) — max rᵀw−λwᵀΣw; QUBO with binary xᵢ; cardinality constraint; QAOA input

**Matrix permanent —** §2.3 (Ch.2) — Perm(A)=Σ\_σ Πᵢ Aᵢ,σ(ᵢ); #P-hard (Valiant 1979); boson sampling output probability

**MDI-QKD —** §5.4.1 (Ch.5) — Lo-Curty-Qi 2012; no detectors at Alice/Bob; eliminates all detector side-channel attacks

**Micius satellite —** §8.1.2 (Ch.8) — China QUESS 2016; 500 km LEO; QKD 1.1kbps; entanglement 1203km; teleportation 1400km

**Mølmer-Sørensen (MS) gate —** §1.10.2 (Ch.1) — bichromatic sideband drive; closed phase-space loop; robust to thermal heating; IonQ/Quantinuum

**N**

**NISQ era —** §3.1 (Ch.3) — Preskill 2018; 50–10,000 qubits; no QEC; high fidelity but noisy; VQE, QAOA practical

**NIST PQC standards —** §5.5, §10.3.3 (Ch.5/10) — FIPS 203/204/205 (Aug 2024): Kyber, Dilithium, SPHINCS+; FALCON FIPS 206

**No-cloning theorem —** §5.1 (Ch.5) — Wootters-Zurek 1982; U|ψ⟩|0⟩=|ψ⟩|ψ⟩ impossible; QKD security foundation

**NQM (National Quantum Mission, India) —** §8.4, §10.4 (Ch.8/10) — Rs.6003 crore/$730M; 2023–2031; QuST/QuCryptoS/QuNAT/QuMAT

**NV centre in diamond —** §7.5.2 (Ch.7) — S=1 electron spin; 2.87 GHz ZFS; T₂ ~ 1ms (RT); ZPL 637 nm; ¹³C nuclear storage

**O**

**Optical tweezer array —** §2.6.1 (Ch.2) — focused laser traps single atoms; SLM/AOD creates programmable 2D arrays; reconfigurable

**P**

**Paul trap —** §1.8 (Ch.1) — RF oscillating quadrupole for radial confinement; DC endcaps for axial; linear ion crystal

**Pauli Transfer Matrix (PTM) —** §3.2.1 (Ch.3) — Tᵢⱼ = (1/2)Tr(PᵢE(Pⱼ)); 4×4 real matrix; off-diagonal = coherent errors

**Pauli Twirling —** §4.4 (Ch.4) — converts coherent to Pauli noise; doesn't reduce magnitude; improves RB/PEC accuracy

**PEC (Probabilistic Error Cancellation) —** §4.3 (Ch.4) — quasi-probability decomposition; G\_ideal=ΣcₖG\_k; overhead γ²=(Σ|cₖ|)²

**Phase kickback (QPE) —** §9.4.1 (Ch.9) — controlled-U transfers phase e^(i2^kφ) to ancilla qubit k; binary encoding of φ = Eτ/(2π)

**Privacy amplification —** §5.2.4 (Ch.5) — universal hash; Leftover Hash Lemma; compresses reconciled key to ℓ ≤ H\_∞−2log(1/ε)

**Q**

**QAE (Quantum Amplitude Estimation) —** §9.5.3 (Ch.9) — N\_q ~ π/(2ε) vs MC N\_cl ~ 1/ε²; quadratic speedup; options pricing, risk

**QAOA —** §9.5.2 (Ch.9) — Farhi 2014; cost+mixer unitaries; QUBO problem solving; portfolio, MaxCut, scheduling

**QBER (Quantum Bit Error Rate) —** §5.2.2 (Ch.5) — fraction of erroneous sifted bits; BB84 threshold < 11%; intercept-resend → 25%

**QPE (Quantum Phase Estimation) —** §9.4.1 (Ch.9) — extracts E₀ = 2πφ/τ; δE = 2π/(2ᵗτ); t=9 ancilla for chemical accuracy

**QPT (Quantum Process Tomography) —** §3.6 (Ch.3) — chi matrix reconstruction; 12ⁿ circuits; F\_process = Tr(χ\_ideal·χ\_exp)

**QRNG (Quantum Random Number Generator) —** §5.6 (Ch.5) — intrinsic QM randomness; beam splitter or vacuum fluctuations; CERT-In mandated

**Quantinuum H2 —** §1.11 (Ch.1) — ⁴⁰Ca⁺ QCCD; 56 qubits; 99.9% 2Q; QV > 8192; ion shuttling between zones

**Quantum internet stages —** §8.3.1 (Ch.8) — Stage 1: trusted-node QKD; Stage 2: entanglement distribution; Stage 3: fault-tolerant

**Quantum phase transition (TFIM) —** §9.6.1 (Ch.9) — T=0; h/J=1 quantum critical point; h/J<1 ferromagnetic; h/J>1 paramagnetic

**Quantum repeater —** §7.1 (Ch.7) — divides distance into N segments; heralded entanglement + swapping; rates O(η^(1/N))

**Quantum utility era —** §10.9 (Ch.10) — Kim et al. 2023; NISQ + ZNE solves physics problem beyond classical; marks utility beginning

**Quantum Volume (QV) —** §10.2.1 (Ch.10) — QV = 2ⁿ where n from (F₂Q)^(n²/2) > 0.67; IBM Eagle QV~4096; IonQ > 4M

**QUBO (Quadratic Unconstrained Binary Optimisation) —** §9.5.1 (Ch.9) — min xᵀQx + cᵀx; binary variables; maps to Ising H; input for QAOA/annealing

**R**

**Rabi oscillations —** §1.4.1 (Ch.1) — P₁(t)=sin²(Ωt/2); resonant drive; π-pulse inverts qubit; Ω/(2π) ~ 50–100 MHz

**RB (Randomised Benchmarking) —** §3.3.1 (Ch.3) — P\_surv(m)=A·pᵐ+B; SPAM-robust; EPC=(1−p)/2; universal gate quality standard

**Richardson extrapolation (ZNE) —** §4.2.2 (Ch.4) — E\_mit=Σγᵢ·E(λᵢ); linear: (3/2)E(1)−(1/2)E(3); cancels polynomial noise terms

**Rydberg blockade —** §2.6.2 (Ch.2) — V(R)=C₆/R⁶; C₆∝n¹¹; V >> ℏΩ → double excitation blocked; 2Q gate mechanism

**S**

**Second quantisation —** §9.2.1 (Ch.9) — occupation number basis; creation/annihilation operators aₚ†, aₚ; fermionic anticommutation

**Shor's algorithm —** §10.3.1 (Ch.10) — O(n² log n); breaks RSA, ECC, DH; ~4000 logical qubits for RSA-2048; motivates PQC

**Sideband cooling —** §1.9.2 (Ch.1) — red sideband drive removes phonons: |g,n⟩→|e,n−1⟩→|g,n−1⟩; n̄ < 0.05; TI ground state

**Sign problem (QMC) —** §9.6.2 (Ch.9) — fermionic path integral at finite doping; SNR ~ e^(−βNf(μ)); 2D Hubbard intractable

**SPHINCS+ (SLH-DSA) —** §5.5, §10.3.3 (Ch.5/10) — FIPS 205; hash-based; SHA-3 only; long-term archival; 8–50 kB signatures

**Superconducting condensate —** §1.2.1 (Ch.1) — Ψ(r) = |Ψ|·e^(iφ); macroscopic phase φ; BCS ground state; Cooper pair density

**T**

**TF-QKD (Twin-Field QKD) —** §5.4.2 (Ch.5) — Lucamarini 2018; R∝√η vs BB84 R∝η; > 600 km demonstrated

**Topological protection —** §2.5 (Ch.2) — non-local qubit encoding; error ∝ exp(−L/ξ); no active QEC needed; Majorana

**Transmon qubit —** §1.3 (Ch.1) — EJ/EC ~ 50–100; charge noise suppressed exponentially; Koch et al. Yale 2007; IBM/Google

**Tsirelson bound —** §5.3 (Ch.5) — |S| ≤ 2√2 ≈ 2.828; quantum maximum for CHSH; Micius: S = 2.37

**U**

**UCCSD ansatz —** §9.3.2 (Ch.9) — exp(T−T†)|Ψ\_HF⟩; T₁ singles, T₂ doubles; physically motivated; avoids barren plateaus

**V**

**Variational principle —** §9.3.1 (Ch.9) — ⟨ψ(θ)|H|ψ(θ)⟩ ≥ E\_ground; Rayleigh-Ritz; VQE energy lower bound guarantees

**VQE (Variational Quantum Eigensolver) —** §9.3.1 (Ch.9) — hybrid QC-classical; Peruzzo 2014; 4-step loop; ansatz + measurement + classical opt

**X**

**XEB (Cross-Entropy Benchmarking) —** §3.5 (Ch.3) — F\_XEB = 2ⁿ⟨p(x)⟩−1; Google supremacy metric; F=0 random, F=1 perfect

**Z**

**ZNE (Zero-Noise Extrapolation) —** §4.2 (Ch.4) — gate folding G→G(G†G)ᵏ; Richardson extrapolation to λ=0; Qiskit resilience\_level=2

**ZZ coupling (crosstalk) —** §3.2.4 (Ch.3) — H=ξZᵢZⱼ; always-on phase accumulation; heavy-hex topology reduces ξ; Heron < 5 kHz

**END OF TEXTBOOK**

Quantum Hardware, Error Correction and Applications

*Dr. Sanjeev Kumar Jain  |   Quantum Computing Specialization*

Units 1–5  ·  Chapters 1–10  ·  Comprise the Complete Textbook

<img class="fig-img" src="content/images/image83.png" alt="figure">
