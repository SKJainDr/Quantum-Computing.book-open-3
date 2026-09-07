## A–Z Index of Terms

**A**

<div class="box box-equation">
<p><strong>AC Josephson effect —</strong> §1.2.2 (Ch.1) — V applied → phase oscillates → supercurrent at f = V/Φ₀ = 483,597.9 GHz/V</p>
<p><strong>ADAPT-VQE (Adaptive VQE) —</strong> §9.3.2 (Ch.9) — ansatz grows operator by operator selecting largest gradient; avoids barren plateaus by construction</p>
<p><strong>AFC (Atomic Frequency Comb) protocol —</strong> §7.5.3 (Ch.7) — comb tooth spacing Δ → storage time τ = 1/Δ; Er:YSO at 1532 nm; T₂ > 6 hours (nuclear)</p>
<p><strong>Amplitude-damping channel —</strong> §3.2.1 (Ch.3) — Kraus: K₀=[[1,0],[0,√(1−γ)]], K₁=[[0,√γ],[0,0]]; models T₁ relaxation</p>
<p><strong>Anharmonicity α (transmon) —</strong> §1.3.2 (Ch.1) — α = E₁₂−E₀₁ ≈ −EC ≈ −200 to −300 MHz; enables selective qubit addressing; essential for 2-level qubit</p>
<p><strong>Ansatz (VQE) —</strong> §9.3.2 (Ch.9) — parameterised quantum circuit U(θ); types: HEA, UCCSD, ADAPT-VQE</p>
<p>Anticommutation relations (fermionic) - §9.2.1 (Ch.9) - { aₚ,aq †} = δₚq; { aₚ,aᵧ} = 0; { aₚ †,aq †} = 0</p>
</div>

**B**

<div class="box box-equation">
<p><strong>BBPSSW distillation —</strong> §8.2.2 (Ch.8) — F' = F²/(F²+(1−F)²); two noisy pairs → one higher-fidelity pair; first entanglement distillation protocol</p>
<p><strong>BB84 QKD protocol —</strong> §5.2 (Ch.5) — Bennett-Brassard 1984; two non-orthogonal bases; QBER < 11% → secure key extraction</p>
<p><strong>BCS superconductivity —</strong> §1.2.1 (Ch.1) — Cooper pairs in condensate; macroscopic phase φ; BCS gap 2Δ; Josephson effect basis</p>
<p><strong>BK (Bravyi-Kitaev) mapping —</strong> §9.2.3 (Ch.9) — hierarchical binary tree parity; average Pauli weight O(log₂M) vs O(M/2) for JW</p>
<p><strong>Barren plateau problem —</strong> §9.3.5 (Ch.9) — gradient variance ∝ 2^(−n) for random deep circuits; mitigation: ADAPT-VQE, layer initialisation</p>
<p><strong>Bell inequalities / CHSH —</strong> §5.3 (Ch.5) — |S| ≤ 2 classical; |S| ≤ 2√2 quantum (Tsirelson); S > 2 certifies entanglement</p>
<p>Binary entropy function h(Q) - §5.2.3 (Ch.5) - h(Q) = - Qlog₂Q - (1 - Q)log₂(1 - Q); h(0.5) = 1; h(0.11) ≈ 0.5 (BB84 threshold)</p>
<p><strong>Bloch sphere / Bloch vector —</strong> §3.2.1 (Ch.3) — ρ = (I + r⃗·σ⃗)/2; r⃗ = (⟨X⟩,⟨Y⟩,⟨Z⟩); |r⃗|=1 pure, <1 mixed</p>
<p><strong>Boson sampling —</strong> §2.3 (Ch.2) — output ∝ |Perm(U)|²; #P-hard classically; GBS uses squeezed states + hafnians</p>
<p><strong>Bravyi-Kitaev mapping —</strong> §9.2.3 — see BK mapping</p>
</div>

**C**

<div class="box box-equation">
<p><strong>CCSD(T) — gold standard —</strong> §9.1.1 (Ch.9) — coupled cluster singles, doubles, perturbative triples; O(M⁷); ~0.3 kcal/mol error; fails FeMoco</p>
<p><strong>CHSH inequality —</strong> §5.3 (Ch.5) — see Bell inequalities</p>
<p><strong>Chemical accuracy —</strong> §9.1.1 (Ch.9) — 1 kcal/mol = 1.6 mHa; threshold for drug discovery and reaction rate reliability</p>
<p><strong>Circuit QED (cQED) —</strong> §1.6 (Ch.1) — Jaynes-Cummings coupling; dispersive limit χ = g²/Δ; QND qubit readout via resonator frequency shift</p>
<p><strong>CNOT / CX gate —</strong> §1.5 (Ch.1) — two-qubit entangling gate; implemented via CR gate in IBM; native entangling gate for many platforms</p>
<p><strong>Cooper pair box (CPB) —</strong> §1.3.1 (Ch.1) — EC >> EJ regime; charge-noise sensitive; predecessor to transmon; T₂ ~ ns</p>
<p><strong>Cooper pairs —</strong> §1.2.1 (Ch.1) — electron pairs bound via phonon-mediated interaction; BCS ground state; carry supercurrent</p>
<p><strong>CPTP map —</strong> §3.2.1 (Ch.3) — completely positive trace-preserving; Kraus: E(ρ) = ΣₖKₖρKₖ†; Σₖ Kₖ†Kₖ = I</p>
<p><strong>Cross-Resonance (CR) gate —</strong> §1.5.2 (Ch.1) — IBM native 2Q gate; drive control at target frequency; ZX interaction → CNOT</p>
<p><strong>CRYSTALS-Dilithium (ML-DSA) —</strong> §5.5, §10.3.3 (Ch.5/10) — FIPS 204; digital signature; M-LWE + M-SIS lattice; code signing, TLS certificates</p>
<p><strong>CRYSTALS-Kyber (ML-KEM) —</strong> §5.5, §10.3.3 (Ch.5/10) — FIPS 203; KEM; Module-LWE; TLS session keys; deployed Chrome 2023</p>
<p><strong>CZ gate —</strong> §1.5.1 (Ch.1) — diag(1,1,1,−1); conditional phase −1 on |11⟩; implemented via flux tuning avoided crossing</p>
</div>

**D**

<div class="box box-equation">
<p><strong>DC Josephson effect —</strong> §1.2.2 (Ch.1) — I = Ic sin(δ); zero-voltage supercurrent; macroscopic quantum tunnelling of Cooper pairs</p>
<p>Decoherence (T₁, T₂, Tφ) - §3.2.6 (Ch.3) - 1/T₂ = 1/(2T₁) + 1/Tφ; T₂ ≤ 2T₁; physical: Purcell, TLS, 1/f flux noise</p>
<p><strong>Density matrix ρ —</strong> §3.2.1 (Ch.3) — ρ = (I + r⃗·σ⃗)/2; Tr(ρ²) = 1 (pure); Bloch vector r⃗; positive semidefinite Hermitian</p>
<p><strong>Depolarising channel —</strong> §3.2.1 (Ch.3) — E(ρ) = (1−p)ρ + (p/3)(XρX+YρY+ZρZ); r⃗ → (1−4p/3)r⃗; symmetric Pauli channel</p>
<p><strong>Dephasing channel —</strong> §3.2.1 (Ch.3) — E(ρ) = (1−pφ)ρ + pφZρZ†; rz preserved; rx,ry shrink; models pure dephasing</p>
<p><strong>Dispersive readout —</strong> §1.6 (Ch.1) — resonator shifts ±χ = ±g²/Δ; QND; homodyne detection; F > 99% on IBM 2024</p>
<p><strong>DiVincenzo criteria —</strong> §1.1 (Ch.1) — scalability, initialisation, coherence, universal gates, measurement; five necessary conditions</p>
<p><strong>DLCZ protocol —</strong> §7.5.1 (Ch.7) — cold Rb ensemble; write Stokes + read anti-Stokes; T₂ ~ 1–100 ms; 780 nm</p>
<p><strong>Doppler cooling —</strong> §1.9.1 (Ch.1) — radiation pressure; T_D = ℏΓ/(2kB); limit n̄ ~ 10–30; precursor to sideband cooling</p>
<p><strong>DRAG pulse shaping —</strong> §1.4.2 (Ch.1) — Ωᵧ = β·dΩ/dt; β = −1/α; suppresses |2⟩ leakage; reduces leakage 10–100×</p>
</div>

**E**

<div class="box box-equation">
<p><strong>EDSR (Electric Dipole Spin Resonance) —</strong> §2.4.2 (Ch.2) — oscillating electric field + spin-orbit coupling → spin rotation; silicon spin qubit single-qubit gate</p>
<p><strong>E91 protocol —</strong> §5.3 (Ch.5) — Ekert 1991; entangled pairs; Bell inequality violation certifies security; Micius: |S| = 2.37</p>
<p><strong>Electronic structure problem —</strong> §9.1.1 (Ch.9) — dim(H) = C(M,N) ≈ 2^M; FCI exact but exponential; classical hierarchy fails at M~50</p>
<p><strong>Entanglement distillation —</strong> §8.2 (Ch.8) — LOCC protocol: n low-F pairs → m < n high-F pairs; BBPSSW: F' = F²/(F²+(1−F)²)</p>
<p><strong>Entanglement swapping —</strong> §7.3 (Ch.7) — BSM at intermediate node; Pauli corrections; creates A–C entanglement without A–C interaction</p>
<p><strong>EPC (Error Per Clifford) —</strong> §3.3.1 (Ch.3) — EPC = (1−p)/2; from RB decay rate p; IBM Heron ~0.1%</p>
<p><strong>Er:YSO memory —</strong> §7.5.3 (Ch.7) — erbium-doped YSO; 1532 nm telecom; AFC τ = 1/Δ; nuclear T₂ > 6 hours (4K)</p>
<p><strong>Exchange gate (silicon spin) —</strong> §2.4.3 (Ch.2) — H_ex = J(t)·S₁·S₂; SWAP or √SWAP; t ~ 5–10 ns; nearest-neighbour connectivity</p>
</div>

**F**

<div class="box box-equation">
<p><strong>Fault-tolerant quantum computing —</strong> §10.9 (Ch.10) — T₂/t_gate > 10⁴; QEC; surface code ~10⁶ physical qubits for RSA-2048</p>
<p><strong>FeMoco (iron-molybdenum cofactor) —</strong> §9.1.1 (Ch.9) — nitrogenase active site; 54 active electrons; CCSD(T) intractable; key QPE target</p>
<p><strong>Flux quantum Φ₀ —</strong> §1.2.2 (Ch.1) — Φ₀ = h/(2e) = 2.068×10⁻¹⁵ Wb; magnetic flux quantum; international volt standard</p>
<p><strong>Fock space —</strong> §9.2.1 (Ch.9) — occupation number basis |n₀,...,n_{M-1}⟩; nₚ ∈ {0,1}; 2^M states = M-qubit Hilbert space</p>
</div>

**G**

<div class="box box-equation">
<p><strong>Gauge freedom (GST) —</strong> §3.4 (Ch.3) — transformation T: ρ→T⁻¹ρ, E→ET, Gᵢ→TGᵢT⁻¹; unobservable; fixed by gauge optimisation</p>
<p><strong>Gaussian Boson Sampling (GBS) —</strong> §2.3 (Ch.2) — squeezed vacuum inputs; hafnian probabilities; Borealis 2022 quantum advantage claim</p>
<p><strong>GST (Gate Set Tomography) —</strong> §3.4 (Ch.3) — simultaneous characterisation of gates+SPAM; germ amplification; SPAM-free; PyGSTi</p>
</div>

**H**

<div class="box box-equation">
<p><strong>Hartree-Fock (HF) —</strong> §9.1.1 (Ch.9) — mean-field; O(M⁴); ~30–50 kcal/mol error; fails for multi-reference/strongly correlated</p>
<p><strong>HNDL (Harvest Now Decrypt Later) —</strong> §10.3.2 (Ch.10) — intercept now, decrypt later with QC; data at risk: medical, military, IP, identity</p>
<p><strong>HOM effect (Hong-Ou-Mandel) —</strong> §2.2.4 (Ch.2) — a†b†|vac⟩ → (|2,0⟩−|0,2⟩)/√2; photon bunching; requires indistinguishable photons</p>
<p><strong>Hubbard model —</strong> §9.6.2 (Ch.9) — H = −tΣhop + UΣnᵢ↑nᵢ↓; QMC sign problem at finite doping; high-Tc target</p>
</div>

**I**

<div class="box box-equation">
<p><strong>IBM Quantum Network —</strong> §10.2.2 (Ch.10) — 400+ partner organisations; cloud access; Qiskit open-source; Eagle/Heron/Condor processors</p>
<p><strong>Information-theoretic security —</strong> §5.1 (Ch.5) — secure against unlimited computing including QC; physical laws not mathematics</p>
<p><strong>IQ modulation —</strong> §1.4.3 (Ch.1) — s(t)=I(t)cos(ωct)−Q(t)sin(ωct); arbitrary pulse shape synthesis; OpenPulse interface</p>
<p>Ising model (TFIM) - §9.6.1 (Ch.9) - H = - JΣZᵢZᵢ₊₁ - hΣXᵢ; QPT at h/J = 1; IBM utility 2023 benchmark</p>
<p><strong>IonQ Forte —</strong> §1.11 (Ch.1) — ¹⁷¹Yb⁺; 35 algorithmic qubits; all-to-all; QV > 4M; Nasdaq IONQ</p>
</div>

**J**

<div class="box box-equation">
<p><strong>Josephson energy EJ —</strong> §1.2.3 (Ch.1) — EJ = Ic·Φ₀/(2π); energy scale of JJ; EJ >> EC (transmon); cosine potential</p>
<p><strong>Josephson inductance LJ —</strong> §1.2.3 (Ch.1) — LJ(δ) = LJ0/cosδ; LJ0 = Φ₀/(2πIc); non-linear → anharmonic energy levels</p>
<p>Josephson junction (JJ) - §1.2 (Ch.1) - S - I - S tunnel junction; Al/AlOₓ/Al; DC: I = Ic ·sinδ; AC: dδ/dt = 2eV/ℏ</p>
<p><strong>Jordan-Wigner (JW) mapping —</strong> §9.2.2 (Ch.9) — aₚ† → (Xₚ−iYₚ)/2⊗Z_{p-1}⊗...⊗Z₀; Z-string encodes antisymmetry; weight O(M/2)</p>
</div>

**K**

<div class="box box-equation">
<p><strong>KLM theorem —</strong> §2.2.3 (Ch.2) — Knill-Laflamme-Milburn; linear optics+SPD+detectors→universal QC; feed-forward; Nature 2001</p>
<p><strong>Kraus operators —</strong> §3.2.1 (Ch.3) — E(ρ) = ΣₖKₖρKₖ†; ΣₖKₖ†Kₖ=I; quantum channel representation</p>
<p><strong>Kyber — see CRYSTALS-Kyber —</strong> §5.5 (Ch.5)</p>
</div>

**L**

<div class="box box-equation">
<p><strong>Lamb-Dicke parameter η —</strong> §1.10.2 (Ch.1) — η = k·x_zpf; k = laser wavevector; x_zpf = zero-point motion; used in MS gate</p>
<p><strong>Leakage (transmon) —</strong> §3.2.5 (Ch.3) — population of |2⟩, |3⟩,...; suppressed by DRAG; monitored by leakage RB</p>
<p><strong>Leftover Hash Lemma —</strong> §5.2.4 (Ch.5) — privacy amplification foundation; ε-close to uniform; ℓ ≤ H_∞ − 2log₂(1/ε)</p>
<p><strong>LOCC (Local Operations and Classical Communication) —</strong> §8.2 (Ch.8) — only allowed operations for entanglement distillation; cannot send quantum systems</p>
</div>

**M**

<div class="box box-equation">
<p><strong>M3 (Matrix-free Measurement Mitigation) —</strong> §4.5.2 (Ch.4) — sparse calibration; O(n·s) vs O(2ⁿ); scalable for n > 15 qubits</p>
<p><strong>Majorana zero modes (MZMs) —</strong> §2.5.1 (Ch.2) — γ† = γ; appear at topological SC wire ends; non-local qubit encoding; non-Abelian braiding</p>
<p><strong>Markowitz portfolio optimisation —</strong> §9.5.1 (Ch.9) — max rᵀw−λwᵀΣw; QUBO with binary xᵢ; cardinality constraint; QAOA input</p>
<p><strong>Matrix permanent —</strong> §2.3 (Ch.2) — Perm(A)=Σ_σ Πᵢ Aᵢ,σ(ᵢ); #P-hard (Valiant 1979); boson sampling output probability</p>
<p><strong>MDI-QKD —</strong> §5.4.1 (Ch.5) — Lo-Curty-Qi 2012; no detectors at Alice/Bob; eliminates all detector side-channel attacks</p>
<p><strong>Micius satellite —</strong> §8.1.2 (Ch.8) — China QUESS 2016; 500 km LEO; QKD 1.1kbps; entanglement 1203km; teleportation 1400km</p>
<p><strong>Mølmer-Sørensen (MS) gate —</strong> §1.10.2 (Ch.1) — bichromatic sideband drive; closed phase-space loop; robust to thermal heating; IonQ/Quantinuum</p>
</div>

**N**

<div class="box box-equation">
<p><strong>NISQ era —</strong> §3.1 (Ch.3) — Preskill 2018; 50–10,000 qubits; no QEC; high fidelity but noisy; VQE, QAOA practical</p>
<p><strong>NIST PQC standards —</strong> §5.5, §10.3.3 (Ch.5/10) — FIPS 203/204/205 (Aug 2024): Kyber, Dilithium, SPHINCS+; FALCON FIPS 206</p>
<p><strong>No-cloning theorem —</strong> §5.1 (Ch.5) — Wootters-Zurek 1982; U|ψ⟩|0⟩=|ψ⟩|ψ⟩ impossible; QKD security foundation</p>
<p><strong>NQM (National Quantum Mission, India) —</strong> §8.4, §10.4 (Ch.8/10) — Rs.6003 crore/$730M; 2023–2031; QuST/QuCryptoS/QuNAT/QuMAT</p>
<p><strong>NV centre in diamond —</strong> §7.5.2 (Ch.7) — S=1 electron spin; 2.87 GHz ZFS; T₂ ~ 1ms (RT); ZPL 637 nm; ¹³C nuclear storage</p>
</div>

**O**

<div class="box box-equation">
<p><strong>Optical tweezer array —</strong> §2.6.1 (Ch.2) — focused laser traps single atoms; SLM/AOD creates programmable 2D arrays; reconfigurable</p>
</div>

**P**

<div class="box box-equation">
<p><strong>Paul trap —</strong> §1.8 (Ch.1) — RF oscillating quadrupole for radial confinement; DC endcaps for axial; linear ion crystal</p>
<p><strong>Pauli Transfer Matrix (PTM) —</strong> §3.2.1 (Ch.3) — Tᵢⱼ = (1/2)Tr(PᵢE(Pⱼ)); 4×4 real matrix; off-diagonal = coherent errors</p>
<p><strong>Pauli Twirling —</strong> §4.4 (Ch.4) — converts coherent to Pauli noise; doesn't reduce magnitude; improves RB/PEC accuracy</p>
<p><strong>PEC (Probabilistic Error Cancellation) —</strong> §4.3 (Ch.4) — quasi-probability decomposition; G_ideal=ΣcₖG_k; overhead γ²=(Σ|cₖ|)²</p>
<p><strong>Phase kickback (QPE) —</strong> §9.4.1 (Ch.9) — controlled-U transfers phase e^(i2^kφ) to ancilla qubit k; binary encoding of φ = Eτ/(2π)</p>
<p>Photon survival probability - §7.1 (Ch.7) - η(L) = 10^- αL/10 = e^- L/Latt; Latt ≈ 22 km for α = 0.2 dB/km</p>
<p><strong>Privacy amplification —</strong> §5.2.4 (Ch.5) — universal hash; Leftover Hash Lemma; compresses reconciled key to ℓ ≤ H_∞−2log(1/ε)</p>
</div>

**Q**

<div class="box box-equation">
<p><strong>QAE (Quantum Amplitude Estimation) —</strong> §9.5.3 (Ch.9) — N_q ~ π/(2ε) vs MC N_cl ~ 1/ε²; quadratic speedup; options pricing, risk</p>
<p><strong>QAOA —</strong> §9.5.2 (Ch.9) — Farhi 2014; cost+mixer unitaries; QUBO problem solving; portfolio, MaxCut, scheduling</p>
<p><strong>QBER (Quantum Bit Error Rate) —</strong> §5.2.2 (Ch.5) — fraction of erroneous sifted bits; BB84 threshold < 11%; intercept-resend → 25%</p>
<p><strong>QPE (Quantum Phase Estimation) —</strong> §9.4.1 (Ch.9) — extracts E₀ = 2πφ/τ; δE = 2π/(2ᵗτ); t=9 ancilla for chemical accuracy</p>
<p><strong>QPT (Quantum Process Tomography) —</strong> §3.6 (Ch.3) — chi matrix reconstruction; 12ⁿ circuits; F_process = Tr(χ_ideal·χ_exp)</p>
<p><strong>QRNG (Quantum Random Number Generator) —</strong> §5.6 (Ch.5) — intrinsic QM randomness; beam splitter or vacuum fluctuations; CERT-In mandated</p>
<p><strong>Quantinuum H2 —</strong> §1.11 (Ch.1) — ⁴⁰Ca⁺ QCCD; 56 qubits; 99.9% 2Q; QV > 8192; ion shuttling between zones</p>
<p><strong>Quantum internet stages —</strong> §8.3.1 (Ch.8) — Stage 1: trusted-node QKD; Stage 2: entanglement distribution; Stage 3: fault-tolerant</p>
<p><strong>Quantum phase transition (TFIM) —</strong> §9.6.1 (Ch.9) — T=0; h/J=1 quantum critical point; h/J<1 ferromagnetic; h/J>1 paramagnetic</p>
<p><strong>Quantum repeater —</strong> §7.1 (Ch.7) — divides distance into N segments; heralded entanglement + swapping; rates O(η^(1/N))</p>
<p><strong>Quantum utility era —</strong> §10.9 (Ch.10) — Kim et al. 2023; NISQ + ZNE solves physics problem beyond classical; marks utility beginning</p>
<p><strong>Quantum Volume (QV) —</strong> §10.2.1 (Ch.10) — QV = 2ⁿ where n from (F₂Q)^(n²/2) > 0.67; IBM Eagle QV~4096; IonQ > 4M</p>
<p><strong>QUBO (Quadratic Unconstrained Binary Optimisation) —</strong> §9.5.1 (Ch.9) — min xᵀQx + cᵀx; binary variables; maps to Ising H; input for QAOA/annealing</p>
</div>

**R**

<div class="box box-equation">
<p><strong>Rabi oscillations —</strong> §1.4.1 (Ch.1) — P₁(t)=sin²(Ωt/2); resonant drive; π-pulse inverts qubit; Ω/(2π) ~ 50–100 MHz</p>
<p><strong>RB (Randomised Benchmarking) —</strong> §3.3.1 (Ch.3) — P_surv(m)=A·pᵐ+B; SPAM-robust; EPC=(1−p)/2; universal gate quality standard</p>
<p><strong>Richardson extrapolation (ZNE) —</strong> §4.2.2 (Ch.4) — E_mit=Σγᵢ·E(λᵢ); linear: (3/2)E(1)−(1/2)E(3); cancels polynomial noise terms</p>
<p><strong>Rydberg blockade —</strong> §2.6.2 (Ch.2) — V(R)=C₆/R⁶; C₆∝n¹¹; V >> ℏΩ → double excitation blocked; 2Q gate mechanism</p>
</div>

**S**

<div class="box box-equation">
<p><strong>Second quantisation —</strong> §9.2.1 (Ch.9) — occupation number basis; creation/annihilation operators aₚ†, aₚ; fermionic anticommutation</p>
<p><strong>Shor's algorithm —</strong> §10.3.1 (Ch.10) — O(n² log n); breaks RSA, ECC, DH; ~4000 logical qubits for RSA-2048; motivates PQC</p>
<p><strong>Sideband cooling —</strong> §1.9.2 (Ch.1) — red sideband drive removes phonons: |g,n⟩→|e,n−1⟩→|g,n−1⟩; n̄ < 0.05; TI ground state</p>
<p><strong>Sign problem (QMC) —</strong> §9.6.2 (Ch.9) — fermionic path integral at finite doping; SNR ~ e^(−βNf(μ)); 2D Hubbard intractable</p>
<p><strong>SPHINCS+ (SLH-DSA) —</strong> §5.5, §10.3.3 (Ch.5/10) — FIPS 205; hash-based; SHA-3 only; long-term archival; 8–50 kB signatures</p>
<p><strong>Superconducting condensate —</strong> §1.2.1 (Ch.1) — Ψ(r) = |Ψ|·e^(iφ); macroscopic phase φ; BCS ground state; Cooper pair density</p>
</div>

**T**

<div class="box box-equation">
<p>T₁ (relaxation time) - §3.2.6 (Ch.3) - P₁(t) = e^- t/T₁; spontaneous emission; Purcell, TLS; IBM 100 - 500 μs</p>
<p>T₂ (coherence time) - §3.2.6 (Ch.3) - 1/T₂ = 1/(2T₁) + 1/Tφ; T₂ ≤ 2T₁; dephasing; IBM 50 - 300 μs</p>
<p><strong>TF-QKD (Twin-Field QKD) —</strong> §5.4.2 (Ch.5) — Lucamarini 2018; R∝√η vs BB84 R∝η; > 600 km demonstrated</p>
<p><strong>Topological protection —</strong> §2.5 (Ch.2) — non-local qubit encoding; error ∝ exp(−L/ξ); no active QEC needed; Majorana</p>
<p><strong>Transmon qubit —</strong> §1.3 (Ch.1) — EJ/EC ~ 50–100; charge noise suppressed exponentially; Koch et al. Yale 2007; IBM/Google</p>
<p><strong>Tsirelson bound —</strong> §5.3 (Ch.5) — |S| ≤ 2√2 ≈ 2.828; quantum maximum for CHSH; Micius: S = 2.37</p>
</div>

**U**

<div class="box box-equation">
<p><strong>UCCSD ansatz —</strong> §9.3.2 (Ch.9) — exp(T−T†)|Ψ_HF⟩; T₁ singles, T₂ doubles; physically motivated; avoids barren plateaus</p>
</div>

**V**

<div class="box box-equation">
<p><strong>Variational principle —</strong> §9.3.1 (Ch.9) — ⟨ψ(θ)|H|ψ(θ)⟩ ≥ E_ground; Rayleigh-Ritz; VQE energy lower bound guarantees</p>
<p><strong>VQE (Variational Quantum Eigensolver) —</strong> §9.3.1 (Ch.9) — hybrid QC-classical; Peruzzo 2014; 4-step loop; ansatz + measurement + classical opt</p>
</div>

**X**

<div class="box box-equation">
<p><strong>XEB (Cross-Entropy Benchmarking) —</strong> §3.5 (Ch.3) — F_XEB = 2ⁿ⟨p(x)⟩−1; Google supremacy metric; F=0 random, F=1 perfect</p>
</div>

**Z**

<div class="box box-equation">
<p><strong>ZNE (Zero-Noise Extrapolation) —</strong> §4.2 (Ch.4) — gate folding G→G(G†G)ᵏ; Richardson extrapolation to λ=0; Qiskit resilience_level=2</p>
<p><strong>ZZ coupling (crosstalk) —</strong> §3.2.4 (Ch.3) — H=ξZᵢZⱼ; always-on phase accumulation; heavy-hex topology reduces ξ; Heron < 5 kHz</p>
</div>

**END OF TEXTBOOK**

Quantum Hardware, Error Correction and Applications

*Dr. Sanjeev Kumar Jain | Quantum Computing Specialization*

Units 1–5 · Chapters 1–10 · Comprise the Complete Textbook

<figure class="book-figure">
<img src="content/images/image75.png" alt="">
<figcaption></figcaption>
</figure>
