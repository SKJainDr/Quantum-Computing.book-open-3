# UNIT 5 | CHAPTER 9

# Quantum Chemistry, Finance and Materials Simulation

<div class="box box-key-concept">
<p class="box-title"><strong>📋  Learning Objectives</strong></p>
<p>After completing this chapter you will be able to: (1) Explain why the electronic structure problem scales exponentially and why quantum computers offer a natural solution; (2) Convert a fermionic Hamiltonian to qubit operators using the Jordan-Wigner and Bravyi-Kitaev mappings and compare their Pauli weight; (3) Describe the VQE algorithm, compare HEA, UCCSD and ADAPT-VQE ansatze, and interpret the H₂ binding curve; (4) Explain Quantum Phase Estimation and estimate resources required for FeMoco drug-discovery-scale calculations; (5) Formulate a portfolio optimisation problem as QUBO and implement QAOA; (6) Derive the quadratic speedup of Quantum Amplitude Estimation over classical Monte Carlo; (7) Describe the Ising and Hubbard models and explain why quantum simulation offers advantages over classical methods; (8) Analyse the IBM quantum utility result (2023) and its significance for the field.</p>
</div>

## 9.1 The Quantum Chemistry Revolution: Why Molecules Need Quantum Computers

### 9.1.1 The Electronic Structure Problem and Classical Scaling

The central challenge of quantum chemistry is the electronic structure problem: given the fixed positions of atomic nuclei in a molecule, find the lowest-energy configuration of the electrons — the ground-state energy and wavefunction. This determines the molecule's shape, reactivity, optical spectrum, and how it binds to biological receptors. It is the gateway to drug discovery, catalyst design, battery chemistry, and materials engineering.

The exact solution requires solving the many-body Schrodinger equation H|psi> = E|psi> for N electrons. The wavefunction |psi> lives in a Hilbert space of dimension 2^M (M spin-orbitals, each either empty or occupied by a spin-up or spin-down electron). This is the core of the problem: the complexity is exponential in M. For water (H₂O, 10 electrons in 13 spin-orbitals with STO-3G basis), the exact full-CI wavefunction requires about 1,700 Slater determinants. For caffeine (102 electrons), it has ~10^{50} determinants — impossible for any classical computer, now or ever.

<div class="box box-generic">
<p class="box-title">Exponential Hilbert space scaling</p>

</div>

Classical quantum chemistry has developed a hierarchy of approximations to tame this exponential scaling:

- Hartree-Fock (HF): replaces the N-body problem with N coupled one-body problems via a mean-field approximation. Scales as O(M^4) but misses electron correlation entirely. Errors of 30-50 kcal/mol, far too large for drug design (chemical accuracy = 1 kcal/mol).

- MP2 (Moller-Plesset perturbation theory, 2nd order): adds perturbative correlation. Scales as O(M^5). Errors approximately 8 kcal/mol. Good for weakly correlated systems but fails for stretched bonds.

- CCSD (Coupled Cluster Singles and Doubles): adds single and double excitations iteratively. Scales as O(M^6). Errors approximately 2 kcal/mol. Accurate and widely used in the pharmaceutical industry.

- CCSD(T): adds perturbative triple excitations. Scales as O(M^7). The "gold standard" of quantum chemistry with errors approximately 0.3 kcal/mol. Completely intractable beyond ~50 correlated electrons.

- Full CI (exact): includes all excitations. Scales exponentially. Intractable beyond ~20 electrons even on the world's fastest supercomputers.

<div class="box box-warning">
<p class="box-title"><strong>⚠️  The Accuracy-Scalability Wall</strong></p>
<p>Drug discovery requires bond energies accurate to 1 kcal/mol = 0.0016 Hartree (chemical accuracy). Hartree-Fock misses this by 20-50x. CCSD(T) achieves it but scales as O(M^7): a molecule with 100 orbitals requires 10^{14} floating-point operations per energy evaluation. Even more critically, CCSD(T) completely fails for strongly correlated systems (stretched bonds, transition metal active sites, cuprate superconductors) where multiple determinants become nearly degenerate. The most pharmaceutically and industrially relevant molecules — FeMoco (54 electrons), cytochrome P450 (~200 electrons), Rubisco (~1000 electrons) — all lie beyond the classical scaling wall.</p>
</div>

### 9.1.2 Why Quantum Computers Are Naturally Suited

A quantum computer with N qubits naturally represents a quantum state in a 2^N-dimensional Hilbert space. This is exactly the Hilbert space needed to represent the electronic wavefunction of a molecule with N spin-orbitals. The quantum computer does not simulate the molecule classically — it is a physical quantum system whose state space perfectly matches the molecule's wavefunction space. Richard Feynman foresaw this in his famous 1982 lecture: "Nature isn't classical, dammit, and if you want to make a simulation of nature, you'd better make it quantum mechanical."

The practical implication is profound: a 100-qubit fault-tolerant quantum computer could in principle represent and manipulate the electronic wavefunction of any 100-spin-orbital molecule exactly, using resources polynomial in the number of qubits. This would unlock drug discovery at scales impossible for classical computers: nitrogen-fixing enzyme catalysts (redesigning the Haber-Bosch process), high-efficiency solar cells, room-temperature superconductors, and personalised cancer drugs.

<div class="box box-anecdote">
<p class="box-title"><strong>📜  Feynman's Vision (1982) to Today</strong></p>
<p>In his 1982 paper "Simulating Physics with Computers", Feynman argued that simulating quantum mechanical systems on a classical probabilistic computer requires exponential resources, but a computer that is itself quantum mechanical could do it in polynomial resources. This insight sat largely dormant until Aspuru-Guzik et al. (2005) showed how to map molecular Hamiltonians to qubit operators. Peruzzo et al. (2014) demonstrated the first VQE on a photonic chip. IBM demonstrated the first VQE on a superconducting chip in 2016. Today, routine VQE experiments on 4-50 qubit devices are performed globally. The 40-year journey from Feynman's vision to IBM's utility-era result in 2023 is one of the great intellectual arcs of modern physics.</p>
</div>

## 9.2 Second Quantisation and Qubit Mappings

<div class="box box-generic">

<img class="fig-img" src="content/images/image63.png" alt="">
</div>

**Figure 9.1: Second Quantisation: Jordan-Wigner Mapping and JW vs BK Pauli Weight Scaling**

Left panel: The Jordan-Wigner (JW) mapping converts fermionic creation and annihilation operators into qubit Pauli tensor products. For orbital p, the creation operator maps to (1/2)(X\_p - iY\_p) tensored with a Z-string acting on all orbitals below p. The colour-coded grid shows the structure: red Z boxes form the growing string for each orbital index, while the purple X box marks the target qubit. The Z-string enforces fermionic antisymmetry by tracking the parity of all lower-indexed occupied orbitals. This O(N) growth in Pauli weight is the main cost of JW. Right panel: Average Pauli operator weight vs number of spin-orbitals N for Jordan-Wigner (red line, linear O(N/2)) and Bravyi-Kitaev (green line, logarithmic O(log2 N)) mappings. For N=20 orbitals, JW requires average weight 10 while BK requires only about 4.3, translating directly to shorter quantum circuits. For N=50 (typical drug-molecule active space), JW weight is 25 vs BK weight approximately 5.6: a 4.5x advantage. The green shaded region shows the BK advantage zone, which grows with system size.

### 9.2.1 Fock Space and the Second-Quantised Hamiltonian

Second quantisation reformulates the N-electron problem using occupation number representation. Instead of tracking the spatial coordinates of N electrons (first quantisation), we define a Fock space basis |n\_0, n\_1, ..., n\_{M-1}> where n\_p in {0,1} is the occupation number of spin-orbital p (0 = empty, 1 = occupied by one electron). The full Hilbert space of 2^M states is spanned by all occupation-number vectors.

The fundamental operators in second quantisation are:

- Creation operator a\_p-dagger: adds an electron to spin-orbital p. If p is already occupied, a\_p-dagger |...1\_p...> = 0 (Pauli exclusion). If empty, a\_p-dagger |...0\_p...> = (-1)^{sum\_{q<p} n\_q} |...1\_p...>, where the phase (-1)^{...} encodes fermionic antisymmetry.

- Annihilation operator a\_p = (a\_p-dagger)-dagger: removes an electron from spin-orbital p.

- Number operator n-hat\_p = a\_p-dagger a\_p: eigenvalues 0 (empty) or 1 (occupied).

<div class="box box-generic">
<p class="box-title">Fermionic anticommutation relations</p>

</div>

The molecular electronic Hamiltonian in second-quantised form is:

<div class="box box-generic">
<p class="box-title">Molecular Hamiltonian (second quantised)</p>

</div>

where h\_pq = <p|T+V\_nuc|q> are one-electron integrals (kinetic energy plus nuclear-electron attraction) and h\_pqrs = <pq|rs> are two-electron repulsion integrals. Both are computed classically by evaluating Gaussian basis function integrals using packages like PySCF or Psi4. The challenge is finding the lowest eigenvalue of this operator — the ground-state energy.

### 9.2.2 The Jordan-Wigner Mapping

The Jordan-Wigner (JW) mapping, proposed in 1928, converts fermionic operators to tensor products of Pauli matrices acting on qubits. Each qubit represents one spin-orbital: qubit state |0> means unoccupied, |1> means occupied.

<div class="box box-generic">
<p class="box-title">a_p^dag  -&gt;  (1/2)(X_p - i Y_p) tensor Z_{p-1} tensor Z_{p-2} tensor ... tensor Z_0</p>
<p>JW creation operator mapping</p>
</div>

<div class="box box-generic">
<p class="box-title">a_p      -&gt;  (1/2)(X_p + i Y_p) tensor Z_{p-1} tensor Z_{p-2} tensor ... tensor Z_0</p>
<p>JW annihilation operator mapping</p>
</div>

The Z-string (Z\_{p-1} tensor ... tensor Z\_0) encodes the fermionic phase. When an electron is added to orbital p, it must anticommute past all electrons in lower orbitals. The Z operator evaluates to +1 on an empty qubit and -1 on an occupied qubit, so the product of Z gates on all lower qubits correctly counts the number of occupied orbitals below p and applies the (-1)^count fermionic sign.

The cost of JW: the Pauli weight (number of non-identity Paulis) of the operator a\_p-dagger is p+1, growing linearly with orbital index. For M spin-orbitals, the average Pauli weight across all operators is M/2. The Hamiltonian H decomposes into O(M^4) Pauli strings, each of average length M/2. Each Pauli string requires a separate quantum circuit in VQE, making the measurement cost O(M^5) overall.

<div class="box box-key-concept">
<p class="box-title"><strong>🔑  Physical Intuition for the Z-String</strong></p>
<p>The Z-string in JW is the quantum version of the fermionic "Jordan-Wigner string". When you move an electron from orbital q to orbital p (p&gt;q), you must antisymmetrise it past all electrons between q and p. Each occupied orbital between q and p contributes a factor of (-1). The Pauli Z operator has eigenvalue +1 on |0&gt; (empty) and -1 on |1&gt; (occupied), so the product Z_{p-1}...Z_0 counts occupied orbitals below p mod 2 and applies the correct sign. This is the origin of the fermionic minus sign that makes electrons behave so differently from bosons.</p>
</div>

### 9.2.3 The Bravyi-Kitaev Mapping

The Bravyi-Kitaev (BK) mapping, developed by Bravyi and Kitaev (2002) and made practical by Seeley, Richard, and Love (2012), achieves O(log M) average Pauli weight — an exponential improvement over JW for large systems. The key idea: instead of storing occupation numbers directly in individual qubits (JW), BK stores parity information in a hierarchical binary tree structure.

In BK, qubit k stores the parity (sum mod 2) of the occupations of a set of orbitals determined by the binary representation of k. Specifically, qubit k stores the parity of orbitals {j : j <= k and the k-th bit of j is set in the BK tree}. This allows both the occupation of any orbital and the parity of any set of orbitals to be read out in O(log M) operations rather than O(M).

<div class="box box-generic">
<p class="box-title">Pauli weight (JW) = O(M/2)     vs     Pauli weight (BK) = O(log2 M)</p>
<p>JW vs BK average Pauli weight</p>
</div>

Concrete comparison for a 50-spin-orbital molecule: JW gives average Pauli weight 25; BK gives average weight approximately 5.6. This means BK circuits for the same molecular Hamiltonian are approximately 4.5x shallower on average, reducing gate errors by the same factor and improving VQE convergence substantially.

### 9.2.4 JW vs BK: Summary Comparison

| Property | Jordan-Wigner | Bravyi-Kitaev | Parity Encoding |
|---|---|---|---|
| Pauli weight (avg) | O(M/2) linear | O(log2 M) logarithmic | O(log M) |
| Number of Pauli terms | O(M^4) | O(M^4) | O(M^4) |
| Qubit count | M qubits | M qubits | M-2 qubits |
| Implementation | Simple; default in Qiskit Nature | Moderate; OpenFermion, QDK | Complex; PySCF-QC |
| Best use case | Small molecules (<20 orbitals) | Large molecules (>20 orbitals) | Parity-sector algorithms |

Table 9.1: Fermionic-to-qubit mapping comparison. BK is preferred for larger molecules due to its logarithmic Pauli weight scaling.

## 9.3 The Variational Quantum Eigensolver (VQE)

<div class="box box-generic">

<img class="fig-img" src="content/images/image64.png" alt="">
</div>

**Figure 9.2: Variational Quantum Eigensolver (VQE) Algorithm Architecture**

VQE operates as a quantum-classical hybrid loop. The quantum computer (teal, left) prepares the parameterised ansatz state |psi(theta)> = U(theta)|0> and measures expectation values of each Pauli string P\_i in the Hamiltonian decomposition H = sum\_i w\_i P\_i. The Hamiltonian measurement block (purple, centre) manages the decomposition and aggregates results into the energy estimate E(theta). The classical optimiser (green, right) updates the parameter vector theta to minimise E(theta) via COBYLA, SPSA, or gradient-based methods. The loop repeats until convergence. Below: three ansatz families are compared. Hardware-Efficient Ansatz (HEA, teal): uses native gate sets in shallow depth, NISQ-compatible but physically unmotivated with frequent barren plateaus. UCCSD (purple): chemically motivated, systematically captures electron correlation through single and double excitations from the Hartree-Fock reference state, but requires O(M^4) depth circuits. ADAPT-VQE (green): grows the circuit adaptively by selecting the operator from a predefined pool with the largest energy gradient, achieving chemical accuracy with 5-10x shorter circuits than UCCSD. The gold equation box states the variational principle: <psi(theta)|H|psi(theta)> >= E\_ground for all theta, ensuring VQE always provides an upper bound on the true ground state energy.

### 9.3.1 Variational Principle and VQE Architecture

VQE, proposed by Peruzzo et al. (Nature Communications 2014), exploits the variational principle of quantum mechanics: for any normalised state |psi>, the expectation value <psi|H|psi> is always greater than or equal to the true ground-state energy E\_0. Equality holds if and only if |psi> = |Psi\_0>, the true ground state.

<div class="box box-generic">
<p class="box-title">&lt;psi(theta)|H|psi(theta)&gt; &gt;= E_ground   for all theta,   equality iff |psi(theta)&gt; = |Psi_0&gt;</p>
<p>Variational Principle (Rayleigh-Ritz)</p>
</div>

The VQE procedure in detail:

- Step 1 (Hamiltonian decomposition): Express H = sum\_i w\_i P\_i as a weighted sum of Pauli strings P\_i with real coefficients w\_i. This is performed classically using JW or BK mapping on the two-electron integrals from a classical chemistry package.

- Step 2 (Ansatz preparation): Choose a parameterised unitary U(theta) and prepare |psi(theta)> = U(theta)|0>^{tensor N} on the quantum processor.

- Step 3 (Expectation value measurement): For each Pauli string P\_i, rotate into the P\_i eigenbasis and measure. The energy is E(theta) = sum\_i w\_i <P\_i>. Each Pauli string requires repeated circuit runs (shots) to estimate <P\_i> statistically.

- Step 4 (Classical optimisation): Pass E(theta) to a classical optimiser that updates theta to decrease E. Repeat from Step 2 until convergence (delta E < threshold).

The measurement overhead is a key practical consideration. For a molecule with M spin-orbitals, the Hamiltonian has O(M^4) Pauli strings. For H₂ in STO-3G (4 spin-orbitals), there are 15 Pauli strings. For LiH (12 spin-orbitals), there are 631. For H₂O in STO-3G (14 spin-orbitals), there are about 1086 Pauli strings. With 1000 shots each, a single energy evaluation for H₂O requires over 10^6 circuit executions. Grouping commuting Paulis and using classical shadow tomography can reduce this by 10-100x.

### 9.3.2 Ansatz Design: HEA, UCCSD, and ADAPT-VQE

**Hardware-Efficient Ansatz (HEA)**

HEA uses parameterised single-qubit rotations (typically Ry gates) interleaved with entangling layers (CNOT gates arranged in a hardware-native topology) repeated L times. For N qubits and L layers, HEA has L\*N parameters. HEA is shallow enough for NISQ devices and has high expressibility in principle, but it is not physically motivated: it has no direct connection to the chemistry of the molecule. Random initialisations in deep HEA circuits typically sit in "barren plateaus" where all gradients are exponentially small.

**UCCSD Ansatz (Unitary Coupled Cluster Singles and Doubles)**

UCCSD prepares the state by applying the UCC operator to the Hartree-Fock reference state:

<div class="box box-generic">
<p class="box-title">|psi_UCCSD(theta)&gt; = exp( T(theta) - T-dagger(theta) ) |Psi_HF&gt;</p>
<p>UCCSD ansatz state</p>
</div>

where T = T1 + T2 is the cluster operator with single excitations T1 = sum\_{ia} t\_i^a a\_a^dag a\_i and double excitations T2 = sum\_{ijab} t\_{ij}^{ab} a\_a^dag a\_b^dag a\_i a\_j. The UCCSD ansatz is chemically motivated: it systematically includes single and double electronic excitations from occupied orbitals i,j to virtual orbitals a,b. Implementing exp(T-T-dagger) on a quantum computer requires Trotterisation into a product of small unitaries, leading to circuits of depth O(M^4). For small molecules on current hardware, this depth is manageable; for large systems, it requires fault-tolerant quantum computing.

**ADAPT-VQE: Adaptive Ansatz Construction**

ADAPT-VQE (Grimsely et al., Nature Communications 2019) constructs the ansatz circuit iteratively. Starting from |Psi\_HF>, it: (1) computes the energy gradient dE/d(theta\_k) for every operator G\_k in a predefined operator pool (typically all single and double excitation operators); (2) selects and appends the operator with the largest |gradient|; (3) re-optimises all parameters via COBYLA; (4) repeats until the norm of all gradients falls below a threshold. ADAPT-VQE achieves chemical accuracy with circuits 5-10x shorter than UCCSD for the same molecule, at the cost of more classical gradient evaluations. It is the state-of-the-art approach in 2024.

### 9.3.3 H₂ Binding Curve: Benchmark Results

<div class="box box-generic">

<img class="fig-img" src="content/images/image65.png" alt="">
</div>

**Figure 9.3: H₂ Binding Curve and Energy Method Accuracy Comparison**

Left: Ground-state energy of H₂ (Hartree) vs bond distance (Angstrom). White: exact Full CI (the true quantum mechanical answer). Red dashed: Hartree-Fock, which severely underestimates correlation energy at stretched geometries (r > 1.5 Angstrom) where the molecule dissociates into two H atoms. The HF error diverges near dissociation because the wavefunction becomes strongly multi-reference (two near-degenerate determinants). Green dash-dot: CCSD(T), the classical gold standard, closely matches Full CI across the entire curve. Gold: VQE-UCCSD, nearly indistinguishable from Full CI at equilibrium (r = 0.74 Angstrom, marked by blue dotted line) and at stretched geometries. Right: Energy error vs Full CI (milli-Hartree) for seven methods applied to H₂. Gold dashed line: chemical accuracy threshold at 1.6 mHa = 1 kcal/mol. Hartree-Fock error = 35.2 mHa (22x above chemical accuracy). MP2 = 8.6 mHa. CCSD = 2.1 mHa. CCSD(T) = 0.3 mHa (below chemical accuracy, gold standard). VQE-STO-3G = 2.8 mHa (above chemical accuracy; limited by basis set). VQE-cc-pVDZ = 0.8 mHa (within chemical accuracy; larger basis set). VQE achieves chemical accuracy when combined with an appropriate basis set and ansatz, matching or exceeding CCSD with polynomial quantum resources.

The H₂ molecule in the STO-3G minimal basis set has only 4 spin-orbitals, reducible to 2 qubits after exploiting particle-number and spin symmetries. This makes it the standard first benchmark for quantum chemistry on hardware. The landmark experiment by Peruzzo et al. (2014) demonstrated VQE for H₂ on a 2-qubit photonic processor, obtaining ground-state energies with milli-Hartree accuracy. O'Malley et al. (2016) repeated this on a superconducting processor from Google, achieving chemical accuracy with a hardware-efficient ansatz.

The critical test of quantum chemistry methods is the dissociation limit: as the H-H bond is stretched beyond 1.5 Angstrom, the electronic wavefunction becomes strongly multi-reference and classical single-reference methods (HF, MP2, CCSD) fail catastrophically. VQE-UCCSD correctly describes this regime because the UCCSD ansatz can represent multi-reference states. This is the regime where quantum simulation provides genuine advantage over classical methods.

### 9.3.4 LiH Benchmark vs CCSD(T)

Lithium hydride (LiH) is the standard second benchmark for quantum chemistry hardware: 4 electrons, 12 spin-orbitals in STO-3G, reducible to 4 qubits after symmetry reduction. LiH was simulated by Kandala et al. (2017) on a 6-qubit Rigetti processor using the hardware-efficient ansatz, and by Hempel et al. (2018) using trapped-ion hardware.

| Method | LiH Energy (Ha) | Error (mHa) | Scaling | Feasibility |
|---|---|---|---|---|
| Hartree-Fock | -7.8627 | 106 | O(M^4) | Always feasible |
| MP2 | -7.9508 | 15 | O(M^5) | Up to ~500 electrons |
| CCSD(T) | -7.9648 | 1.3 | O(M^7) | Up to ~50 electrons |
| VQE-UCCSD (STO-3G) | -7.9636 | 2.5 | Poly. (QC) | 4 qubits today |
| Full CI (exact) | -7.9661 | 0 | Exponential | < 20 electrons only |

Table 9.2: LiH ground-state energy comparison (STO-3G basis, equilibrium geometry). Chemical accuracy threshold = 1.6 mHa. VQE-UCCSD achieves 2.5 mHa error on 4 qubits; with cc-pVDZ basis, VQE reaches chemical accuracy.

### 9.3.5 Classical Optimisers and the Barren Plateau Problem

The classical optimiser in VQE faces a fundamental challenge: it must minimise E(theta) given noisy, shot-limited estimates of the expectation value. Several optimisers are standard:

- COBYLA: Constrained Optimisation BY Linear Approximations. Gradient-free; fits a linear model of E(theta) to evaluate the objective. Robust to shot noise, converges in 50-500 function evaluations for small systems. Default in Qiskit VQE.

- SPSA: Simultaneous Perturbation Stochastic Approximation. Estimates the full gradient with only 2 circuit evaluations per step by simultaneously perturbing all parameters. Very efficient on noisy hardware. Requires careful tuning of step sizes.

- Parameter Shift Rule: Computes exact analytical gradients via dE/d(theta\_k) = [E(theta + pi/2 \* e\_k) - E(theta - pi/2 \* e\_k)] / 2. Requires 2N circuit evaluations per gradient step for N parameters. Exact but expensive for large N.

<div class="box box-warning">
<p class="box-title"><strong>⚠️  The Barren Plateau Problem</strong></p>
<p>For randomly initialised deep parameterised circuits (depth O(N)), the gradient dE/d(theta_k) vanishes exponentially as exp(-c*N) where N is the number of qubits. This "barren plateau" phenomenon means VQE gradients become undetectable through measurement noise for large systems. Mitigation strategies: (1) Problem-inspired initialisations starting near the Hartree-Fock state; (2) Layer-by-layer training where each layer is optimised before adding the next; (3) Local cost functions that use qubit-local observables rather than the full global energy; (4) ADAPT-VQE's greedy gradient-based operator selection. Barren plateaus are one of the most active research areas in variational quantum algorithms (2023-2024).</p>
</div>

## 9.4 Quantum Phase Estimation for Exact Energies

<div class="box box-generic">

<img class="fig-img" src="content/images/image66.png" alt="">
</div>

**Figure 9.4: Quantum Phase Estimation (QPE) Circuit for Exact Molecular Energies**

The QPE circuit for extracting the ground-state energy E\_0 to t-bit precision. Top four wires: ancilla (clock) register with t qubits, each initialised to |0>. Hadamard gates create equal superpositions |+> on all ancilla qubits. Controlled-U^{2^k} gates (green boxes on the target register wire) apply the molecular time-evolution operator U = exp(-iHt) raised to increasing powers of 2, using each ancilla qubit as the control. This encodes the binary digits of the phase phi = E\_0 \* tau / (2\*pi) onto the ancilla register through phase kickback: when the target is in eigenstate |psi\_j>, the controlled gate applies the phase e^{i\*2\*pi\*phi\_j\*2^k} to the k-th ancilla qubit. The teal block applies the inverse Quantum Fourier Transform to the ancilla register, decoding the binary phase into a measurement outcome. Measuring the ancilla register yields the binary representation of phi\_j = E\_j\*tau/(2\*pi) to t-bit precision: energy resolution delta\_E = 2\*pi/(2^t \* tau). The red resource box at the bottom states the drug-discovery scale requirement: simulating FeMoco (the iron-molybdenum cofactor of nitrogenase) requires approximately 111 logical qubits plus ancilla and about 10^{13} Toffoli gates, requiring millions of physical qubits with quantum error correction.

### 9.4.1 QPE Algorithm and Phase Kickback

Quantum Phase Estimation (QPE) directly extracts eigenvalues of a unitary operator. For quantum chemistry, we use the time-evolution operator U = exp(-iH\*tau): an energy eigenstate |Psi\_j> satisfies U|Psi\_j> = exp(-i\*E\_j\*tau)|Psi\_j>. QPE extracts the phase phi\_j = E\_j\*tau/(2\*pi) in binary form on the ancilla register.

QPE protocol step by step:

- Initialise: Ancilla register in |0>^{tensor t}, target register in initial state |psi\_0> with good overlap with the desired eigenstate (typically the Hartree-Fock state |Psi\_HF>).

- Hadamard layer: Apply H to all t ancilla qubits, creating |+>^{tensor t} tensor |psi\_0>.

- Controlled-U^{2^k} gates: For k = 0, 1, ..., t-1, apply the controlled-U^{2^k} gate with ancilla qubit k as control. This can be implemented by Trotterising U = exp(-iH\*tau) into a product of elementary gates.

- Inverse QFT: Apply QFT-dagger to the t ancilla qubits. This decodes the Fourier-encoded phase into the binary representation of phi\_j.

- Measure: Measure ancilla register. The result, converted to decimal, gives phi\_j = E\_j\*tau/(2\*pi) to t-bit precision, so E\_j = 2\*pi\*phi\_j/tau.

<div class="box box-generic">
<p class="box-title">QPE energy resolution (t ancilla bits)</p>

</div>

Phase kickback is the core mechanism: when ancilla qubit k is in state |1> and the controlled-U^{2^k} gate is applied to the target in eigenstate |Psi\_j>:

<div class="box box-generic">
<p class="box-title">CU^{2^k}( |1&gt;_k tensor |Psi_j&gt; ) = exp(i*2*pi*phi_j*2^k) |1&gt;_k tensor |Psi_j&gt;</p>
<p>Phase kickback in QPE</p>
</div>

The phase exp(i\*2\*pi\*phi\_j\*2^k) is kicked back onto the ancilla qubit k, encoding the k-th binary digit of phi\_j. After all controlled-U gates, the ancilla register holds a quantum Fourier series over phi\_j; the inverse QFT disentangles this into the binary representation of phi\_j.

Key advantage of QPE over VQE: QPE gives the exact eigenvalue (to t-bit precision) without any classical optimisation loop. The energy is extracted in a single run (assuming perfect overlap with the desired eigenstate). Key disadvantage: QPE requires fault-tolerant quantum computing. The controlled-U^{2^k} gates require deep Trotterised circuits that accumulate substantial errors without quantum error correction.

### 9.4.2 Resource Estimates for Drug Discovery: FeMoco

<div class="box box-generic">

<img class="fig-img" src="content/images/image67.png" alt="">
</div>

**Figure 9.5: Quantum Chemistry Resource Estimates Across Molecular Complexity**

Physical and logical qubit requirements for quantum chemistry simulations across increasing molecular complexity. Blue bars: NISQ-era VQE physical qubits (no error correction). Orange bars: fault-tolerant QPE logical qubits (x10^6 scale for visibility; actual values annotated). H₂ (2 electrons, 2 orbitals): 4 physical qubits, demonstrated on hardware in 2014-2016. LiH (4 electrons, 12 orbitals): 4-12 physical qubits, demonstrated 2017-2018. H₂O (10 electrons, 13 orbitals) and BeH₂ (14 electrons, 14 orbitals): 26-28 physical qubits, demonstrated 2019-2022. FeMoco (54 electrons, 54 orbitals, the nitrogen-fixation catalyst active site): 108 physical qubits for VQE but approximately 4 million logical qubits for full QPE (Reiher et al. 2017, updated by von Burg et al. 2021). Caffeine (102 electrons, ~200 orbitals) and penicillin (~210 electrons, ~800 orbitals) require resources firmly in the fault-tolerant future. The IBM Condor milestone (1121 physical qubits) is shown for reference. The enormous gap between NISQ VQE capabilities (accessible today with H₂-LiH scale) and fault-tolerant QPE targets (FeMoco and beyond) defines the current quantum chemistry research agenda.

Reiher et al. (2017) performed the first detailed resource estimation for a chemically relevant QPE calculation: simulating FeMoco, the iron-molybdenum cofactor of nitrogenase (the enzyme responsible for biological nitrogen fixation). FeMoco has 54 electrons in 54 active spin-orbitals, far beyond any classical exact method. Their estimate: 111 logical qubits, approximately 10^{13} Toffoli gates.

Updated estimates using more efficient algorithms (von Burg et al., 2021) reduced this to approximately 10^{10} Toffoli gates and about 630 physical qubits at 0.1% error rate, representing 4 days of continuous quantum computation. This would allow calculation of the nitrogen-fixation reaction mechanism at chemical accuracy, potentially enabling the rational design of artificial nitrogen-fixing catalysts. The Haber-Bosch process currently consumes 1-2% of global energy output to fix nitrogen for fertilisers; a quantum-designed biological catalyst could eliminate this energy cost and transform global agriculture.

<div class="box box-real-world">
<p class="box-title"><strong>🌎  The Drug Discovery Prize</strong></p>
<p>The global pharmaceutical industry spends $2.6 billion and 12 years to bring a single drug from discovery to market, with a 90% failure rate in clinical trials. Most failures stem from insufficient understanding of molecular interactions between drug candidates and protein targets. QPE-quality simulation of drug-receptor binding energies at chemical accuracy would predict which candidate molecules will bind, reducing the failure rate dramatically. McKinsey (2021) estimates quantum chemistry could unlock $50-100 billion in annual value in the pharmaceutical sector alone by 2035, with the biotechnology, materials, and energy sectors adding comparable value.</p>
</div>

## 9.5 Quantum Finance and Optimisation

### 9.5.1 Portfolio Optimisation: From Markowitz to QUBO

Portfolio optimisation is one of the most studied quantum computing applications in finance. The problem: given N assets with expected returns r\_i and covariance matrix Sigma describing how asset prices move together, find portfolio weights w\_i in [0,1] with sum(w\_i) = 1 that maximise the risk-adjusted return. This is the Markowitz mean-variance model (Nobel Prize, 1990):

<div class="box box-generic">
<p class="box-title">Maximise:  sum_i w_i r_i  -  lambda * sum_{ij} w_i Sigma_{ij} w_j     s.t.  sum_i w_i = 1, w_i &gt;= 0</p>
<p>Markowitz mean-variance portfolio optimisation</p>
</div>

For continuous weights, this is a convex quadratic programme solvable classically in O(N^3) time — no quantum speedup needed. However, real portfolio management has additional integer constraints that make the problem NP-hard: minimum investment lots (integer or binary weights), cardinality constraints (invest in at most K of N assets), transaction costs, liquidity tiers, and regulatory capital constraints.

To map to a quantum computer, continuous weights are discretised into binary variables. Each asset allocation is encoded in b bits, giving N\*b binary variables x\_{ik} in {0,1}. With b=1 (each asset either selected or not), the problem has N binary variables. The objective and constraints become a Quadratic Unconstrained Binary Optimisation (QUBO) problem:

<div class="box box-generic">
<p class="box-title">QUBO formulation</p>

</div>

where Q\_{ij} encodes both the objective (return minus risk) and constraints (as quadratic penalty terms). The QUBO maps directly to an Ising Hamiltonian H\_Ising = sum\_{ij} J\_{ij} Z\_i Z\_j + sum\_i h\_i Z\_i via the substitution x\_i = (1 - Z\_i)/2, yielding a quantum circuit implementable on a quantum annealer or gate-based quantum computer.

<div class="box box-generic">

<img class="fig-img" src="content/images/image68.png" alt="">
</div>

**Figure 9.6: Portfolio Optimisation Pipeline: Markowitz Model to QUBO to QAOA Circuit**

Three-step quantum portfolio optimisation pipeline. Step 1 (teal, left): Classical Markowitz continuous optimisation. N assets, expected returns r\_i, covariance matrix Sigma. The objective is to maximise risk-adjusted return sum\_i w\_i r\_i - lambda \* w^T Sigma w subject to budget constraint and non-negativity. Step 2 (purple, centre): QUBO formulation. Asset weights are discretised to binary variables x\_i in {0,1}, constraints are encoded as quadratic penalty terms, and the problem becomes H\_QUBO = x^T Q x. The matrix Q combines the negative return (diagonal terms -r\_i), risk (off-diagonal Sigma terms), and penalty terms for constraint violations. Step 3 (green, right): QAOA implementation. The QUBO maps to an Ising cost Hamiltonian H\_C and a transverse-field mixer H\_B. The QAOA circuit (bottom) shows three parallel qubit wires for three assets, Hadamard initialisation to the uniform superposition |+>, and p=2 alternating layers: ZZ cost evolution gates exp(-i\*gamma\*H\_C) and single-qubit X-rotation mixer gates Rx(2\*beta). The classical optimiser updates (beta, gamma) to maximise <H\_C>, and measurement of the optimal state gives the selected portfolio x\*. The status panel confirms that for small N (fewer than 20 assets) and low p (fewer than 5 layers), QAOA has been tested on IBM Quantum; advantage over classical solvers is expected for large N and large p with fault-tolerant hardware.

### 9.5.2 QAOA for Combinatorial Finance Problems

The Quantum Approximate Optimisation Algorithm (QAOA), proposed by Farhi, Goldstone, and Gutmann (2014), is a variational algorithm designed for combinatorial optimisation expressed as Ising models. QAOA prepares a quantum state using p alternating layers of cost and mixer unitaries:

<div class="box box-generic">
<p class="box-title">|psi(beta,gamma)&gt; = exp(-i*beta_p*H_B) exp(-i*gamma_p*H_C) ... exp(-i*beta_1*H_B) exp(-i*gamma_1*H_C) |+&gt;^{tensor n}</p>
<p>QAOA state (p layers)</p>
</div>

where H\_C = H\_QUBO is the cost Hamiltonian and H\_B = sum\_i X\_i is the mixer Hamiltonian. The state |psi(beta,gamma)> is a superposition over all 2^n binary strings, and measuring it samples from a probability distribution biased toward low-energy (high-quality) solutions. The angles (beta, gamma) are classically optimised to maximise <psi|H\_C|psi>.

QAOA properties: (1) At p=1, QAOA provably achieves at least 0.692 approximation ratio for MaxCut on 3-regular graphs. (2) As p approaches infinity, QAOA converges to the exact solution (adiabatic theorem). (3) The circuit depth is O(p), controllable by choosing p. (4) QAOA naturally encodes constraints via penalty terms in H\_C.

Finance applications demonstrated with QAOA (JPMorgan Chase, Goldman Sachs, HSBC, 2020-2024):

- Portfolio cardinality-constrained selection: Given 50 assets, select the 10 with highest Sharpe ratio. QAOA with 50 qubits and p=5 tested on IBM Quantum by JPMorgan (2022).

- Index tracking: Select a sub-portfolio of K stocks that tracks the S&P 500 with minimal tracking error. Formulated as QUBO with covariance penalty.

- Options portfolio hedging: Select a delta-neutral portfolio from N derivatives under transaction cost constraints. QAOA tested on 12 qubits by JPMorgan (Bravyi et al., 2020).

<div class="box box-real-world">
<p class="box-title"><strong>🌐  JPMorgan Chase Quantum Finance Research (2020-2024)</strong></p>
<p>JPMorgan Chase has published the most comprehensive quantum finance research of any financial institution. Key results: (1) 2020: QAOA for portfolio optimisation on 12 qubits, showing QAOA matches classical solvers for small N but does not yet outperform them. (2) 2021: Quantum amplitude estimation for Monte Carlo risk calculations, achieving quadratic speedup in small-scale numerical tests. (3) 2022: Quantum generative adversarial networks (QGAN) for financial time series simulation. (4) 2023: Comprehensive study estimating that quantum advantage in finance requires approximately 1 million fault-tolerant logical qubits, achievable in the 2030s. Goldman Sachs (Stamatopoulos et al., 2020) independently published QPE-based derivative pricing algorithms achieving a 10x speedup over classical methods in numerical tests.</p>
</div>

### 9.5.3 Quantum Amplitude Estimation for Monte Carlo Acceleration

Monte Carlo simulation is the workhorse of computational finance: computing expected values E[f(X)] where X follows a complex multi-dimensional distribution. Option pricing, Value-at-Risk (VaR), Expected Shortfall, and credit risk models all rely on MC. Classical MC requires O(1/epsilon^2) samples to achieve absolute error epsilon, converging slowly as 1/sqrt(N).

<div class="box box-generic">

<img class="fig-img" src="content/images/image69.png" alt="">
</div>

**Figure 9.7: Quantum Finance: QAE Monte Carlo Speedup and Application Landscape**

Left: Convergence comparison between classical Monte Carlo (red, O(1/sqrt(N)) scaling) and Quantum Amplitude Estimation (green, O(1/N) quadratic speedup). Gold dashed line: 1% accuracy target. Classical MC reaches 1% accuracy at 10,000 oracle calls (red circle); QAE reaches the same accuracy at only 100-314 oracle calls (green star), a 32-100x speedup. The log-log axes reveal the constant speedup factor: QAE error decreases twice as fast per order-of-magnitude increase in oracle calls. The green shaded region highlights the quantum speedup zone. Right: Quantum finance application landscape. Six major financial domains are shown with their quantum algorithms and industry leaders: Portfolio Optimisation (QAOA, JPMorgan), Risk Analysis (QAE for VaR, Goldman Sachs), Option Pricing (QAE/QPE, Goldman Sachs), Credit Scoring (quantum ML, HSBC), Fraud Detection (quantum anomaly detection, Barclays), and High-Frequency Trading (QAOA order-book optimisation). Industry companies are shown at the bottom: JPMorgan Chase, Goldman Sachs, HSBC, Barclays (traditional banks with quantum programmes) and QC Ware and Multiverse Computing (pure-play quantum finance software startups).

Quantum Amplitude Estimation (QAE), developed by Brassard, Hoyer, Mosca, and Tapp (2002), provides a quadratic speedup over classical MC:

<div class="box box-generic">
<p class="box-title">epsilon_QAE ~ 1/N     vs     epsilon_classical ~ 1/sqrt(N)     (N = oracle calls)</p>
<p>QAE vs classical MC error scaling</p>
</div>

QAE constructs a quantum oracle A that prepares a state encoding the random variable X: A|0> = sqrt(1-a)|0>|phi\_0> + sqrt(a)|1>|phi\_1>, where a = E[f(X)] is the quantity to be estimated. Using Grover-like amplitude amplification with the operator Q = A\*S\_0\*A-dagger\*S\_psi, QPE applied to Q extracts the angle theta\_a = arcsin(sqrt(a)) in binary, from which a is recovered. For N uses of oracle A, QAE achieves error O(1/N).

Finance applications of QAE demonstrated on real quantum hardware:

- European option pricing: E[max(S\_T - K, 0)]. Classical MC requires 10^6 paths for 0.1% accuracy. QAE achieves the same with ~10^3 oracle calls: 1000x speedup in oracle calls. Demonstrated by Stamatopoulos et al. (2020) on IBM Quantum.

- Value at Risk estimation: P(loss > L) = 5%. QAE directly estimates this probability amplitude. Demonstrated by Woerner and Egger (2019) on IBM Quantum with 7 qubits.

- Credit risk: Expected portfolio loss E[L] over correlated credit instruments. Demonstrated by Egger et al. (2021) on 6 qubits.

Current limitation: Each QAE oracle call requires a deeper quantum circuit than a single classical MC sample. In 2024, QAE oracle depth is 100-1000 gates, while classical MC samples are effectively O(1) floating-point operations. The practical wall-clock speedup from QAE is negative today: the quantum hardware overhead dominates. QAE will become practically advantageous when fault-tolerant quantum gates achieve nanosecond-scale execution times, projected for the early 2030s.

### 9.5.4 Credit Risk, Derivative Pricing and the Quantum Finance Landscape

Beyond portfolio optimisation and Monte Carlo, quantum finance covers several other domains:

- Credit risk modelling: Estimating the expected loss on a portfolio of correlated bonds and loans. The loss distribution has heavy tails that require many MC samples. QAE provides quadratic speedup in sampling this distribution.

- Derivative pricing: Options, futures, swaps, and structured products with path-dependent payoffs. Monte Carlo is the standard method for exotic derivatives; QAE provides quadratic speedup. Goldman Sachs (2020) demonstrated a quantum algorithm for pricing Asian options and barrier options.

- Fraud detection: Identifying anomalous transactions in high-dimensional transaction data. Quantum machine learning (QSVM, QNN) may offer speedup for certain kernel methods, though the advantage is currently limited to specific problem structures.

- High-frequency trading: Order-book optimisation problems (selecting bid/ask prices and quantities to maximise profit subject to inventory constraints) are natural combinatorial problems for QAOA.

| Company | Founded/Division | Key Application Area | Algorithm / Status |
|---|---|---|---|
| JPMorgan Chase | QC Division 2017 | Portfolio optimisation, Monte Carlo, option pricing | QAE, QAOA; 12-qubit demo 2020 |
| Goldman Sachs | QC Research 2020 | Derivative pricing, risk analysis | QPE derivatives; 10x speedup (numerical) |
| HSBC | Quantum team 2022 | FX hedging, climate risk modelling | QAOA; IBM Quantum partnership |
| QC Ware | Startup 2014 | Full-stack quantum finance platform | Forge platform; Goldman Sachs partnership |
| Multiverse Computing | Startup 2019 Spain | Portfolio opt., risk, fraud detection | Singularity platform; BBVA, Credit Agricole |

Table 9.3: Quantum finance industry landscape (2024). Highlighted rows are pure-play quantum software companies.

## 9.6 Quantum Materials and Simulation

### 9.6.1 The Ising Model: Quantum Magnetism and Phase Transitions

The Ising model, introduced by Wilhelm Lenz in 1920 and exactly solved in 2D by Lars Onsager in 1944, is the canonical model of magnetism and classical phase transitions. Each lattice site i holds a spin sigma\_i = +1 (up) or -1 (down). The Hamiltonian is:

<div class="box box-generic">
<p class="box-title">H_Ising = -J * sum_{&lt;ij&gt;} sigma_i sigma_j  -  h * sum_i sigma_i</p>
<p>Classical Ising Hamiltonian (J: exchange coupling, h: external field)</p>
</div>

For ferromagnetic coupling J > 0, neighbouring spins prefer to align; for antiferromagnetic J < 0, they prefer to anti-align. In 2D, the model undergoes a phase transition at the Onsager critical temperature T\_c = 2J / [k\_B \* ln(1 + sqrt(2))] ~= 2.269 J/k\_B. Below T\_c, spontaneous symmetry breaking produces long-range ferromagnetic order (magnetisation <M> > 0). Above T\_c, thermal fluctuations disorder the spins (<M> = 0). Near T\_c, the magnetisation follows the power law <M> ~ (T\_c - T)^{1/8} with a universal critical exponent.

The quantum transverse-field Ising model (TFIM) adds a perpendicular magnetic field that drives quantum fluctuations:

<div class="box box-generic">
<p class="box-title">H_TFIM = -J * sum_{&lt;ij&gt;} Z_i Z_j  -  h * sum_i X_i</p>
<p>Transverse-Field Ising Model (TFIM) Hamiltonian</p>
</div>

Here Z\_i and X\_i are Pauli operators acting on qubit i. The transverse field h competes with the exchange coupling J. At T=0, a quantum phase transition occurs at h\_c/J = 1 in 1D: for h < J, the ground state is ferromagnetically ordered (<Z\_i> is nonzero); for h > J, quantum fluctuations dominate and <Z\_i> = 0. This is a quantum phase transition driven by zero-temperature quantum fluctuations, not thermal fluctuations.

<div class="box box-generic">

<img class="fig-img" src="content/images/image70.png" alt="">
</div>

**Figure 9.8: Ising Model Phase Transition and Hubbard Model on a Lattice**

Left: 2D Ising model magnetisation |<M>| vs temperature T (in units of J/k\_B). The white curve is the exact Onsager solution. Below T\_c = 2.269 J/k\_B (red dashed vertical line), the system is ferromagnetically ordered: the blue shaded region shows |<M>| > 0, decreasing from 1 at T=0 to 0 at T\_c following the Onsager critical exponent |<M>| ~ (T\_c - T)^{1/8}. Above T\_c, thermal fluctuations disorder all spins and |<M>| = 0 (paramagnetic phase). The gold shaded band near T\_c highlights the region where quantum simulation offers the greatest advantage: classical Monte Carlo algorithms suffer "critical slowing down" (autocorrelation times diverge as t ~ |T-T\_c|^{-z} with dynamic exponent z ~ 2), requiring exponentially many steps near the critical point. This is precisely where quantum simulators have an advantage. Right: Hubbard model schematic on a 4x4 square lattice. Blue and purple circles represent lattice sites; black lines show nearest-neighbour hopping connections. Coloured arrows indicate electron spins (up and down). The Hamiltonian (teal box) includes hopping with amplitude t and on-site Coulomb repulsion U. The bar chart compares maximum system sizes accessible by classical methods: Exact Diagonalisation (N=4 sites), DMRG (N=16), Quantum Monte Carlo (N=32), DFT+U (N=100), and quantum simulator (N=50), highlighting where quantum simulation extends beyond classical reach.

The TFIM is exactly solvable in 1D (by Jordan-Wigner transformation to free fermions), but the 2D TFIM is not. It is therefore an ideal benchmark problem for quantum simulation: we know the answer in 1D and can verify quantum devices, while 2D provides a genuine target for quantum advantage. The IBM quantum utility experiment (2023) directly simulated the 1D and 2D TFIM on the 127-qubit Eagle processor.

### 9.6.2 The Hubbard Model: Correlated Electrons and High-Tc Superconductivity

The Hubbard model, proposed independently by Hubbard, Kanamori, and Gutzwiller in 1963, captures the fundamental competition between itinerant kinetic energy (electron hopping) and localised Coulomb repulsion (on-site interaction):

<div class="box box-generic">
<p class="box-title">H_Hubbard = -t * sum_{&lt;ij&gt;,sigma} (c_{i,sigma}^dag c_{j,sigma} + h.c.)  +  U * sum_i n_{i,up} n_{i,down}</p>
<p>Hubbard Hamiltonian (t: hopping amplitude, U: on-site repulsion)</p>
</div>

Despite its apparent simplicity (just two parameters: t for hopping and U for on-site repulsion), the Hubbard model displays an extraordinarily rich phase diagram: Mott insulator transition (U >> t: electrons localise, conductivity vanishes), ferromagnetism (Stoner criterion), antiferromagnetism at half-filling (one electron per site on average), d-wave superconductivity near half-filling, and charge-density waves. The 2D Hubbard model at intermediate U/t (~4-8) and ~10-20% hole doping is believed to contain the essential physics of cuprate high-temperature superconductors.

Why the Hubbard model is so hard for classical computers:

- Exact diagonalisation (ED): The Hilbert space dimension for L sites at half-filling is C(L, L/2)^2. For L=10: C(10,5)^2 = 63,504 states. For L=20: ~3.4 \* 10^{10} states. Only L ~= 12-14 sites are tractable with ED.

- Density Matrix Renormalisation Group (DMRG): Extremely accurate in 1D (up to 1000 sites). For 2D, the required bond dimension chi grows exponentially with the system width, making 2D systems beyond 6x6 sites intractable.

- Quantum Monte Carlo (QMC): Efficient but suffers the "sign problem" at finite doping (away from half-filling). The sign problem causes the statistical variance to grow exponentially with system size and inverse temperature, making low-temperature, finite-doping simulations intractable. This is precisely the regime relevant to high-Tc superconductivity.

- Tensor network methods (PEPS, iPEPS): Access 2D systems but have uncontrolled approximation errors for strongly correlated ground states in the difficult U/t regime.

A fault-tolerant quantum computer with 50-100 logical qubits could represent the full 2D Hubbard model wavefunction exactly, computing ground-state energies and correlation functions at arbitrary U/t and filling. This could definitively settle the high-Tc mechanism and enable rational design of room-temperature superconductors.

<div class="box box-anecdote">
<p class="box-title"><strong>📜  The High-Temperature Superconductivity Mystery</strong></p>
<p>Cuprate high-temperature superconductors (La2CuO4 discovered by Bednorz and Muller in 1986, Nobel Prize 1987) become superconducting at up to 133 K in Hg-Ba-Ca-Cu-O compounds. Despite 38 years and tens of thousands of research papers, the pairing mechanism remains debated. The leading theory involves strong electronic correlations in the CuO2 planes, described by the 2D Hubbard model at 10-20% hole doping. Classical simulation of this regime is impossible due to the sign problem in QMC and the exponential bond dimension in DMRG. A fault-tolerant quantum computer with 500+ logical qubits could simulate the relevant Hubbard model parameters exactly, potentially solving the greatest open problem in condensed matter physics and enabling design of room-temperature superconductors. The energy, transportation, and electronics implications would be truly revolutionary.</p>
</div>

### 9.6.3 IBM Quantum Utility Era Result (Kim et al., Nature 2023)

<div class="box box-generic">

<img class="fig-img" src="content/images/image71.png" alt="">
</div>

**Figure 9.9: IBM Quantum Utility: 127-Qubit Eagle (Kim et al., Nature 2023) and IBM Progress**

Left: The quantum utility experiment. Magnetisation <Z> vs Trotter steps (circuit depth) for simulation of the transverse-field Ising model on IBM Eagle (127 qubits). Blue curve: classical TEBD (Tensor-Network Based Evolution in Block Decimation) simulation, accurate for short circuits (few Trotter steps, shallow depth) but failing beyond ~30 Trotter steps as the required tensor-network bond dimension grows exponentially with circuit depth. Gold data points: IBM Eagle quantum hardware results with Zero-Noise Extrapolation (ZNE) applied using noise scale factors lambda=1,3,5. The white dashed curve shows the exact (Trotter) result. The red shaded region (depth > 30 layers) marks the quantum utility regime: the quantum hardware with ZNE produces physically accurate results where classical TEBD has broken down. This is the first demonstrated instance of a quantum computer producing scientifically useful information beyond what classical tensor-network methods can provide with reasonable resources. Right: IBM hardware progress 2016-2024. Blue circles (left axis, log scale): Quantum Volume growth from QV=4 (2017, 5-qubit devices) to QV=65,536 (2024, Heron processor with tunable couplers). Orange squares (right axis): 2Q gate error rate declining from 1.5% to 0.15%. The gold dashed line marks QV = 10^6, estimated as the threshold where fault-tolerant applications become practical (projected 2027-2029).

<div class="box box-real-world">
<p class="box-title"><strong>🌐  IBM Quantum Utility Result — Kim et al., Nature 618 (2023)</strong></p>
<p>In June 2023, IBM published a landmark result in Nature demonstrating that a 127-qubit Eagle quantum processor outperformed state-of-the-art classical simulation (TEBD tensor networks) for simulating the transverse-field Ising model at circuit depths beyond ~30 Trotter steps. Zero-Noise Extrapolation reduced the effective error rate by approximately 10x. The result was initially controversial: critics pointed out that improved classical algorithms (Tindall et al., PRX Quantum 2023; Begusic et al., Science Advances 2023) could match certain IBM results using more sophisticated tensor-network techniques. IBM responded that these classical methods required exponentially growing computational resources with depth, while the quantum computation used fixed hardware resources. The consensus interpretation: this is the first credible demonstration of quantum utility — a quantum computer producing scientifically valuable results at scales beyond easy classical simulation, even if not yet a definitive "quantum advantage" in the strict algorithmic complexity sense.</p>
</div>

The broader significance of the IBM utility result:

- Error mitigation is practical at scale: ZNE applied to a 127-qubit, 60-layer circuit reduced the effective noise by 10x, enabling scientifically useful results on a NISQ device. This validates the error mitigation roadmap for near-term quantum computing.

- The classical simulation boundary is real and accessible: Classical tensor-network methods do have a computational boundary for 2D quantum dynamics. Quantum hardware can access regimes beyond this boundary today, even with NISQ-level noise.

- The utility era is well-defined: IBM explicitly adopted a pragmatic definition of "quantum utility": a quantum computer provides utility when it enables scientifically useful computations that would be impractical classically, regardless of formal algorithmic complexity arguments. This is a lower bar than "quantum advantage" but a practically important one.

### 9.6.4 Timeline to Quantum Advantage in Materials

<div class="box box-generic">

<img class="fig-img" src="content/images/image72.png" alt="">
</div>

**Figure 9.10: Quantum Computing Applications: Impact vs Timeline Map (2024 Assessment)**

Impact-versus-timeline scatter plot for quantum computing applications. Horizontal axis: estimated years from 2024 until quantum advantage is demonstrated for each application. Vertical axis: potential economic and scientific impact (Minimal to Transformative). Bubble positions and colours indicate specific applications. Near-term accessible region (0-3 years, teal shading): QKD/post-QC security, QRNG devices, quantum annealing, and small VQE (H₂, LiH). These are either already deployed (QKD) or demonstrable on current NISQ hardware. Mid-term region (3-7 years, purple): QAOA optimisation, quantum Monte Carlo finance, drug discovery for medium molecules (H₂O, BeH₂, small drug fragments), and materials design (small Hubbard model). Long-term transformative region (7+ years, green): FeMoco nitrogen fixation simulation via QPE, Shor's algorithm for RSA-2048 cryptanalysis, full quantum chemistry for large drug molecules (penicillin, caffeine scale), and quantum-enhanced AI/ML. The map provides an honest assessment: highest-impact applications require the longest timeline, and near-term advantages are real but modest. This should calibrate expectations appropriately for students entering the field.

| Timeline | Milestone | Materials / Chemistry Application | Hardware Required |
|---|---|---|---|
| 2024-2026 | Utility era: 100-433 qubits + ZNE | TFIM dynamics, small Hubbard (4x4), VQE for LiH and H₂O, QAOA portfolio N<20 | IBM Osprey/Condor, IonQ Forte |
| 2027-2030 | Early fault-tolerant logical qubits | Hubbard model 6x6-8x8, VQE for drug fragments, accurate TFIM dynamics at large depth | ~1000-10000 physical qubits with QEC |
| 2030-2035 | 100+ logical qubits fault-tolerant | 2D Hubbard phase diagram, FeMoco QPE, small drug molecules (paracetamol, caffeine) | 10^5-10^6 physical qubits |
| 2035+ | Full fault-tolerant quantum computer | Room-temperature superconductors, arbitrary drug design, industrial catalyst optimisation | > 10^6 physical qubits |

Table 9.4: Projected timeline for quantum advantage in materials and chemistry (2024 assessment).

**RECAP**

*Chapter 9: Quantum Chemistry, Finance and Materials Simulation — Short Answer Questions & Model Answers*

## Short Answer Questions — Chapter 9

*Instructions: Answer each question in 3–6 lines.*

**Q1.**  What is the electronic structure problem in quantum chemistry?

*[§9.1 — The Quantum Chemistry Revolution]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q2.**  Why does the Hilbert space dimension grow exponentially in quantum chemistry?

*[§9.1 — The Quantum Chemistry Revolution]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q3.**  What is meant by chemical accuracy?

*[§9.1 — The Quantum Chemistry Revolution]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q4.**  Why are quantum computers naturally suited for molecular simulation?

*[§9.1 — The Quantum Chemistry Revolution]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q5.**  Define second quantisation.

*[§9.2 — Second Quantisation and Qubit Mappings]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q6.**  What are fermionic anticommutation relations?

*[§9.2 — Second Quantisation and Qubit Mappings]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q7.**  What is the purpose of the Jordan-Wigner mapping?

*[§9.2 — Second Quantisation and Qubit Mappings]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q8.**  What is the main advantage of the Bravyi-Kitaev mapping over Jordan-Wigner mapping?

*[§9.2 — Second Quantisation and Qubit Mappings]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q9.**  What is the Variational Quantum Eigensolver (VQE)?

*[§9.3 — The Variational Quantum Eigensolver (VQE)]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q10.**  State the variational principle used in VQE.

*[§9.3 — The Variational Quantum Eigensolver (VQE)]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q11.**  What is an ansatz in VQE?

*[§9.3 — The Variational Quantum Eigensolver (VQE)]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q12.**  What is Quantum Phase Estimation (QPE)?

*[§9.4 — Quantum Phase Estimation]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q13.**  What is QAOA and where is it used?

*[§9.5 — Quantum Finance and Optimisation]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q14.**  What is Quantum Amplitude Estimation (QAE)?

*[§9.5 — Quantum Finance and Optimisation]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q15.**  What is the significance of the Hubbard model in condensed matter physics?

*[§9.6 — Quantum Materials and Simulation]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Model Answers — Chapter 9

**Answer 1:**

The electronic structure problem involves determining the ground-state energy and wavefunction of electrons in a molecule for fixed nuclear positions.

**Answer 2:**

The Hilbert space dimension grows exponentially because each spin-orbital can either be occupied or unoccupied, giving approximately 2^M possible configurations for M orbitals.

**Answer 3:**

Chemical accuracy refers to an energy accuracy of about 1 kcal/mol or 0.0016 Hartree, required for reliable molecular predictions.

**Answer 4:**

Quantum computers naturally represent quantum states in exponentially large Hilbert spaces, making them well suited for simulating molecular wavefunctions.

**Answer 5:**

Second quantisation is a formalism that describes many-particle systems using occupation numbers and creation-annihilation operators.

**Answer 6:**

The fermionic anticommutation relations are {a\_p, a\_q†} = δ\_pq, {a\_p, a\_q} = 0, {a\_p†, a\_q†} = 0. These relations ensure Pauli exclusion and fermionic antisymmetry.

**Answer 7:**

The Jordan-Wigner mapping converts fermionic operators into qubit Pauli operators so that molecular Hamiltonians can be implemented on quantum computers.

**Answer 8:**

The Bravyi-Kitaev mapping reduces the average Pauli operator weight from linear scaling O(M) to logarithmic scaling O(log M), leading to shorter quantum circuits.

**Answer 9:**

VQE is a hybrid quantum-classical algorithm used to estimate the ground-state energy of quantum systems by minimising the expectation value of the Hamiltonian.

**Answer 10:**

The variational principle states that for any trial state |ψ⟩, the expectation value ⟨ψ|H|ψ⟩ is always greater than or equal to the true ground-state energy.

**Answer 11:**

An ansatz is a parameterised quantum circuit used to prepare a trial wavefunction in the VQE algorithm.

**Answer 12:**

Quantum Phase Estimation is a quantum algorithm used to estimate eigenvalues of unitary operators with high precision.

**Answer 13:**

QAOA (Quantum Approximate Optimisation Algorithm) is used to solve combinatorial optimisation problems such as portfolio optimisation and scheduling.

**Answer 14:**

Quantum Amplitude Estimation is a quantum algorithm that provides quadratic speedup for Monte Carlo estimation problems.

**Answer 15:**

The Hubbard model describes interacting electrons in a lattice and is important for studying strongly correlated systems and high-temperature superconductivity.

## Solved Examples — Chapter 9

### Example 9.1 Jordan-Wigner Pauli Weights

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A molecule has M = 20 spin-orbitals. (a) How many Pauli strings does the JW-mapped Hamiltonian have? (b) What is the average JW Pauli weight? (c) What is the average BK Pauli weight? (d) What circuit-depth reduction does BK provide on average?</p>
</div>

**Solution:**

(a) The molecular Hamiltonian has O(M^4) Pauli strings after JW mapping. Symmetry-reduced estimate: M^4/8 = 20^4/8 = 160,000/8 = 20,000 Pauli strings.

(b) JW average Pauli weight = M/2 = 20/2 = 10 Pauli operators per string on average.

(c) BK average Pauli weight = log2(M) = log2(20) = 4.32 Pauli operators per string.

(d) Circuit-depth reduction: JW/BK = 10 / 4.32 = 2.3x shallower BK circuits on average.

For M=50 (FeMoco active space): JW weight = 25, BK weight = 5.64, reduction = 4.4x.

This translates directly: each BK circuit has 4.4x fewer two-qubit gates, improving VQE fidelity substantially on NISQ hardware.

### Example 9.2 VQE Shot Budget for H₂O

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>H₂O in STO-3G has 1086 Pauli strings. Each needs 1000 shots for 3% statistical error. (a) Shots per energy evaluation. (b) Shots for 200 VQE iterations. (c) Wall-clock time at 900,000 shots/second (IBM Quantum rate). (d) How does this compare to the molecule's Full CI computation time on a classical computer?</p>
</div>

**Solution:**

(a) Shots per evaluation = 1086 strings x 1000 shots/string = 1,086,000 shots per evaluation.

(b) Total shots = 200 iterations x 1,086,000 = 2.17 x 10^8 shots.

(c) Wall-clock time = 2.17 x 10^8 / 9 x 10^5 = 241 seconds = approximately 4 minutes.

This is the optimistic lower bound assuming perfect grouping. Real VQE for H₂O on IBM Quantum hardware typically takes 2-6 hours including circuit compilation, calibration, queue time, and multiple optimiser restarts.

(d) Full CI for H₂O (STO-3G, 14 spin-orbitals) requires diagonalising a ~1700 x 1700 matrix: trivial (milliseconds) on a classical computer. VQE for H₂O is much SLOWER than classical exact diagonalisation for small systems. VQE's advantage only emerges for systems where classical methods fail: typically beyond 50 correlated electrons.

### Example 9.3 VQE Accuracy and Correlation Energy

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>VQE simulation of LiH gives E_VQE = -7.9636 Ha. Full CI energy is E_FCI = -7.9661 Ha, Hartree-Fock is E_HF = -7.8627 Ha. (a) Absolute error in kcal/mol. (b) Percentage of correlation energy captured. (c) Is chemical accuracy achieved?</p>
</div>

**Solution:**

In kcal/mol: 0.0025 Ha x 627.5 kcal/mol/Ha = 1.57 kcal/mol

Fraction captured = 0.1009 / 0.1034 = 97.6%

(c) Chemical accuracy = 1.0 kcal/mol = 0.00159 Ha. VQE error = 1.57 kcal/mol > 1.0 kcal/mol.

This result does NOT achieve chemical accuracy with STO-3G basis.

With cc-pVDZ basis, VQE-UCCSD typically reaches 0.8 mHa = 0.5 kcal/mol: chemical accuracy achieved.

### Example 9.4 QPE Ancilla Register Size for Chemical Accuracy

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>Quantum Phase Estimation estimates molecular ground state energy to chemical accuracy (delta_E = 1.6 mHa = 0.0016 Ha). (a) How many ancilla qubits t are required with evolution time tau = 10 Ha^-1? (b) Verify the energy resolution formula.</p>
</div>

**Solution:**

We need delta\_E <= 0.0016 Ha, with tau = 10 Ha^{-1} (atomic units, hbar = 1).

2^t >= 2\*pi / (delta\_E \* tau) = 6.283 / (0.0016 x 10) = 6.283 / 0.016 = 392.7

t >= log2(392.7) = 8.62 -> t = 9 ancilla qubits.

(b) With t = 9 and tau = 10: delta\_E = 2\*pi / (2^9 x 10) = 6.283 / 5120 = 0.001227 Ha

0.001227 Ha x 627.5 kcal/mol/Ha = 0.77 kcal/mol < 1 kcal/mol. Chemical accuracy achieved.

Note: Each additional ancilla qubit doubles the precision. For spectroscopic accuracy (0.1 mHa), 12 ancilla qubits are needed with tau = 10.

### Example 9.5 QUBO Portfolio Optimisation by Enumeration

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>4-asset binary portfolio (select exactly 2 assets). Expected returns r = [0.12, 0.08, 0.15, 0.06]. Diagonal covariance sigma^2 = [0.04, 0.02, 0.09, 0.01]. Risk aversion lambda = 2. Objective: maximise r_i + r_j - lambda*(sigma_i^2 + sigma_j^2) for selected pair {i,j}. Find the optimal portfolio.</p>
</div>

**Solution:**

Enumerate all C(4,2) = 6 two-asset portfolios. Objective = r\_i + r\_j - 2\*(sigma\_i^2 + sigma\_j^2):

{1,2}: 0.12+0.08 - 2\*(0.04+0.02) = 0.20 - 0.12 = 0.080

{1,3}: 0.12+0.15 - 2\*(0.04+0.09) = 0.27 - 0.26 = 0.010

{1,4}: 0.12+0.06 - 2\*(0.04+0.01) = 0.18 - 0.10 = 0.080

{2,3}: 0.08+0.15 - 2\*(0.02+0.09) = 0.23 - 0.22 = 0.010

{2,4}: 0.08+0.06 - 2\*(0.02+0.01) = 0.14 - 0.06 = 0.080

{3,4}: 0.15+0.06 - 2\*(0.09+0.01) = 0.21 - 0.20 = 0.010

Maximum objective = 0.080, achieved by portfolios {1,2}, {1,4}, and {2,4}.

Optimal: {1,2} (assets with highest return and lowest risk). Asset 3 (highest return 15%) is penalised by its high risk (sigma^2=0.09) at lambda=2.

### Example 9.6 QAE vs Classical MC: Sample Count Comparison

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>An investment bank needs E[portfolio loss] to accuracy epsilon = 0.5%. (a) Classical MC samples needed. (b) QAE oracle calls needed. (c) Speedup factor. (d) If QAE oracle takes 100 us and classical MC sample takes 10 ns, which is faster in wall-clock time?</p>
</div>

**Solution:**

(b) QAE: using N >= pi/(2\*epsilon) calls for single-sided estimation:

(c) Quantum speedup in sample count = 40,000 / 314 = 127x.

(Close to the theoretical sqrt(40000) = 200x quadratic speedup.)

(d) Classical wall-clock: 40,000 x 10 ns = 0.4 ms.

Quantum wall-clock: 314 x 100 us = 31.4 ms.

Classical wins by 31.4/0.4 = 78x on wall-clock despite needing 127x more samples!

Lesson: QAE sample-count advantage does not yield wall-clock speedup until quantum gates achieve nanosecond speed (fault-tolerant era, ~2030s).

### Example 9.7 Ising Model Critical Temperature

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A ferromagnetic 2D Ising model has exchange coupling J = 5 meV. (a) Calculate T_c in Kelvin. (b) Is the system ordered at 100 K? (c) For the TFIM, what transverse field h in Tesla is needed to drive the quantum phase transition at T=0 (g=2)?</p>
</div>

**Solution:**

= 2 x 5x10^{-3} x 1.602x10^{-19} J / (1.381x10^{-23} J/K x 0.8814)

= 1.602x10^{-21} / 1.217x10^{-23} = 131.6 K

(b) At T = 100 K < T\_c = 131.6 K: the system is in the ferromagnetically ordered phase.

B = J / (g \* mu\_B) = 5x10^{-3} x 1.602x10^{-19} / (2 x 9.274x10^{-24})

= 8.01x10^{-22} / 1.855x10^{-23} = 43.2 T

A very large field of 43 Tesla is needed for J = 5 meV, accessible in pulsed-field facilities.

### Example 9.8 IBM Utility Circuit Fidelity Analysis

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>The IBM Eagle experiment used 127 qubits, 60 Trotter steps, and 2Q gate depth d = 4 per step. The 2Q gate error rate was 0.1%. (a) Total 2Q gates per qubit. (b) Naive circuit fidelity per qubit without ZNE. (c) Effective fidelity per qubit after ZNE reduces error 10x. (d) Why does ZNE work for expectation values even when circuit fidelity is near zero?</p>
</div>

**Solution:**

(a) Total 2Q layers per qubit = 60 steps x 4 = 240 layers. Gates per qubit = 240.

(b) Naive fidelity per qubit = (1 - 0.001)^{240} = (0.999)^{240} = exp(-0.240) = 0.787.

ZNE fidelity per qubit = (0.9999)^{240} = exp(-0.024) = 0.976.

(d) ZNE corrects observable expectation values, not the full state fidelity.

Even when the quantum state is highly mixed (global fidelity ~0), local observables <Z\_i> can still be accurately estimated if the dominant noise model is Pauli/depolarising.

ZNE extrapolates <O>(lambda) as a polynomial in lambda (noise scale) and extrapolates to lambda=0.

This works because Pauli noise preserves the functional form: <O>(lambda) = <O>\_ideal \* exp(-c\*lambda) or similar, which can be extrapolated accurately even when exp(-c\*lambda) is small.

## Multiple Choice Questions — Chapter 9

Note: Answers are collected at the end of this chapter.

**1. The Jordan-Wigner mapping enforces fermionic anticommutation relations using:**

(A) The Y gate on every orbital below p, which introduces the necessary imaginary unit

(B) A Z-string Z\_{p-1} tensor ... tensor Z\_0 on all orbitals below p, tracking the parity of occupied orbitals

(C) A CNOT chain from qubit 0 to qubit p, which propagates the fermionic phase

(D) Phase gates on even-numbered orbitals compensating for the bosonic commutation relations

**2. The Bravyi-Kitaev mapping achieves O(log N) Pauli weight compared to O(N) for Jordan-Wigner because:**

(A) BK discards long-range orbital interactions below a truncation threshold

(B) BK stores parity information in a hierarchical binary tree structure so each orbital occupation requires only log N Pauli operations to access

(C) BK applies a global unitary rotation that diagonalises the non-interacting hopping terms

(D) BK uses only Z and X Paulis, eliminating the Y operators that increase circuit depth

**3. The variational principle underpinning VQE states that for any normalised state |psi(theta)>:**

(A) <psi(theta)|H|psi(theta)> = E\_ground when the ansatz is deep enough

(B) <psi(theta)|H|psi(theta)> >= E\_ground for all theta, providing an upper bound on the ground-state energy

(C) <psi(theta)|H|psi(theta)> <= E\_ground for all theta, providing a lower bound

(D) <psi(theta)|H|psi(theta)> = E\_ground only when |psi(theta)> is the Hartree-Fock reference state

**4. The UCCSD ansatz |psi\_UCCSD(theta)> = exp(T - T-dagger)|Psi\_HF> is preferred over Hardware-Efficient Ansatz because:**

(A) UCCSD requires fewer parameters than HEA for the same accuracy on NISQ hardware

(B) UCCSD is chemically motivated by coupled-cluster theory and avoids the worst barren plateaus near the Hartree-Fock reference state, but requires deeper circuits than HEA

(C) UCCSD circuits are shallower than HEA because they use only nearest-neighbour connectivity

(D) UCCSD is the only ansatz that can achieve chemical accuracy, while HEA is limited to Hartree-Fock accuracy

**5. The ADAPT-VQE algorithm constructs the ansatz circuit by:**

(A) Adding all UCCSD operators simultaneously and then pruning those with small parameters

(B) Iteratively appending the operator from a predefined pool with the largest gradient of the energy, growing the circuit adaptively

(C) Randomly selecting gates from a hardware-native library and optimising via global search

(D) Starting from the exact full-CI wavefunction and compressing it into a short circuit using tensor decomposition

**6. Quantum Phase Estimation extracts the molecular ground-state energy by measuring:**

(A) The expectation value <psi|H|psi> through repeated Pauli string measurements

(B) The phase phi\_j = E\_j \* tau / (2\*pi) accumulated on ancilla qubits via the phase kickback mechanism from controlled time-evolution gates

(C) The two-qubit correlation functions <Z\_i Z\_j> for all pairs of molecular orbitals

(D) The probability distribution over all Fock-space basis states by measuring all qubits in the computational basis

**7. The Reiher et al. (2017) resource estimate for FeMoco QPE found approximately 10^{13} Toffoli gates because:**

(A) The JW mapping produces 10^{13} Pauli strings for the 54-orbital FeMoco Hamiltonian

(B) Each Trotterised time-evolution step requires many elementary gates for 54 orbitals, multiplied by the 2^t repetitions needed for QPE precision, multiplied by quantum error correction overhead

(C) VQE for FeMoco requires 10^{13} classical optimisation iterations to converge

(D) Full CI diagonalisation for the 54-electron system requires 10^{13} floating-point multiplications

**8. In the Markowitz to QUBO to QAOA pipeline, the QUBO formulation introduces binary variables x\_i in {0,1} representing:**

(A) Whether the log-return of asset i was positive or negative on the training data

(B) Discretised portfolio allocation weights, with investment constraints encoded as quadratic penalty terms in the objective function

(C) The sign of the price-correlation coefficient between assets i and j

(D) The presence of arbitrage opportunities detected by a classical machine learning classifier

**9. Quantum Amplitude Estimation provides a quadratic speedup over classical Monte Carlo, meaning:**

(A) QAE requires O(1/epsilon) oracle calls while classical MC needs O(1/epsilon^2) samples to achieve absolute error epsilon

(B) QAE requires O(1/epsilon^2) oracle calls while classical MC needs O(1/epsilon) samples

(C) QAE requires O(log(1/epsilon)) oracle calls for any epsilon, an exponential speedup over classical MC

(D) QAE and classical MC have the same O(1/epsilon^2) scaling but QAE's oracle is faster to evaluate

**10. IBM's 2023 quantum utility result (Kim et al., Nature 618) demonstrated that a 127-qubit quantum processor:**

(A) Computed the ground-state energy of H₂O to chemical accuracy in less time than classical CCSD(T)

(B) Simulated transverse-field Ising model dynamics using Trotter evolution, producing accurate results in a circuit-depth regime where classical TEBD tensor-network simulation broke down

(C) Factored a 127-bit RSA key faster than the best classical factoring algorithms

(D) Generated provably random numbers at a rate exceeding any classical pseudorandom generator

**11. The "barren plateau" problem in VQE refers to:**

(A) A flat region in the Born-Oppenheimer potential energy surface near the transition state of a reaction

(B) The phenomenon where gradients of the cost function E(theta) vanish exponentially with system size for random deep circuits, making classical optimisation infeasible

(C) The saturation of the variational energy at the Hartree-Fock value when the ansatz has insufficient expressibility

(D) The loss of quantum coherence in the ancilla register during repeated QPE measurements

**12. The Hubbard model is relevant to high-temperature superconductivity because:**

(A) The model predicts BCS-type phonon-mediated pairing at high temperatures when the lattice vibration frequency is large

(B) The 2D Hubbard model at intermediate U/t and 10-20% hole doping is believed to capture the strongly correlated electronic physics of CuO2 planes in cuprate superconductors, but its phase diagram is unsolved due to the fermionic sign problem in QMC

(C) The Hubbard model reduces exactly to the BCS gap equation in the weak-coupling limit U << t, explaining superconductivity in all cuprates

(D) Hubbard model simulations on classical computers have already confirmed d-wave pairing at room temperature; quantum computers are not needed for this problem

**13. The transverse-field Ising model (TFIM) undergoes a quantum phase transition at T=0 when h/J = 1 because:**

(A) The classical thermal fluctuations become strong enough to overcome the ferromagnetic exchange coupling at this temperature

(B) At h/J = 1, quantum fluctuations from the transverse field (X terms) exactly balance the ferromagnetic exchange (ZZ terms), creating a quantum critical point where the ground state changes from ordered to disordered

(C) The Hamiltonian becomes exactly solvable by Jordan-Wigner transformation only at h/J = 1

(D) The Brillouin zone boundary condition creates a level crossing in the single-particle spectrum at exactly h/J = 1 for any lattice

**14. The classical gold standard CCSD(T) scales as O(M^7). For M increasing from 10 to 50 spin-orbitals, the computational cost increases by:**

(A) 50/10 = 5 times

(B) (50/10)^3 = 125 times (cubic scaling of exact diagonalisation)

**15. Quantum Monte Carlo (QMC) methods fail for the 2D Hubbard model at finite doping because of the "sign problem", which arises from:**

(A) Two-dimensional lattices having too many sites for any stochastic sampling method

(B) Fermionic wavefunctions changing sign under electron exchange, causing near-perfect cancellation between positive and negative weight configurations in the path integral, leading to exponentially growing statistical variance

(C) The Trotter decomposition of the imaginary-time evolution accumulating O(beta^2) errors that diverge at low temperature (large beta)

(D) The Hubbard interaction term U\*n\_{up}\*n\_{down} creating complex-valued matrix elements in the path integral that cannot be sampled probabilistically

## MCQ Answers — Chapter 9

| Q1 | B | Q2 | B | Q3 | B | Q4 | B | Q5 | B |
|---|---|---|---|---|---|---|---|---|---|
| Q6 | B | Q7 | B | Q8 | B | Q9 | A | Q10 | B |
| Q11 | B | Q12 | B | Q13 | B | Q14 | C | Q15 | B |

## Unsolved Problems — Chapter 9

9.1  H₂O in the 6-31G basis has 14 spin-orbitals and 10 electrons. Determine (a) Qubits needed with JW mapping. (b) Approximate number of Pauli strings. (c) UCCSD singles and doubles parameter count. (d) If VQE converges in 300 iterations with 2000 shots per Pauli string, total shot count and wall-clock time at 900 kshots/s.

[Ans: (a) 14 qubits; (b) ~14^4/8 = 3430 Pauli strings; (c) singles = occ x virt = 5 x 9 = 45 params; doubles = C(5,2) x C(9,2) = 10 x 36 = 360 params; total = 405; (d) 3430 x 2000 x 300 = 2.06x10^9 shots; time = 2.06x10^9/9x10^5 = 2290 s = 38 min]

9.2  Prove the variational principle for any normalised state. Then show VQE applied to H₂ (STO-3G, exact ground state E\_FCI = -1.1745 Ha, HF energy E\_HF = -1.1174 Ha) must give energy above -1.1745 Ha. Calculate the total and percent correlation energy.

[Ans: Expand |psi> = sum\_n c\_n|n> in energy eigenbasis; <H> = sum\_n |c\_n|^2 E\_n >= E\_0 (since E\_n >= E\_0). E\_corr = E\_FCI - E\_HF = -0.0571 Ha = -35.8 mHa = -22.6 kcal/mol. VQE captures this correlation energy variationally.]

9.3  QAOA with N=6 binary variables (6 assets, 1 bit each) and p=3 layers. (a) Number of QAOA parameters. (b) Circuit depth in 2-qubit gates for all-to-all ZZ connectivity. (c) Minimum circuit execution time if 2Q gate = 50 ns (trapped-ion).

[Ans: (a) 2p = 6 parameters (3 beta + 3 gamma); (b) Cost layer: C(6,2) = 15 ZZ gates per layer, 3 layers = 45 total; mixer: 6 Rx gates per layer = 18 total; (c) 45 x 50 ns = 2.25 us cost gates + mixer; roughly 3-4 us total circuit execution.]

9.4  European call option: Monte Carlo needs N\_cl = 10^6 paths for 0.1% accuracy. (a) QAE oracle calls for same accuracy. (b) Speedup. (c) If oracle = 500 ns per call (quantum) and classical path = 5 ns, which is faster in wall-clock?

[Ans: (a) N\_q = pi/(2 x 0.001) = 1571 oracle calls; (b) speedup = 10^6/1571 = 637x in oracle count; (c) QAE wall-clock = 1571 x 500 ns = 786 us; classical = 10^6 x 5 ns = 5 ms. Classical wins (6.4x faster). At 5 ns/oracle QAE wins by 127x. Need sub-10 ns quantum gates.]

9.5  In a triangular lattice Ising model (coordination number z=6, J > 0), the mean-field critical temperature is T\_c = zJ/k\_B. (a) Calculate T\_c for J = 3 meV. (b) How does this compare to the square lattice (z=4) value? (c) Why does higher z raise T\_c?

[Ans: (a) T\_c(triangle) = 6 x 3x10^{-3} x 1.602x10^{-19} / 1.381x10^{-23} = 208 K (mean field); exact T\_c slightly lower; (b) square lattice T\_c = 4 x 3 meV / k\_B (mean field) = 139 K. Ratio = 6/4 = 1.5; (c) More exchange bonds per site means each spin is held in place by more neighbours, requiring more thermal energy to disorder.]

9.6  For a 1D Hubbard model chain of L=12 sites at half-filling (6 up, 6 down spins), compute (a) Hilbert space dimension; (b) number of non-zero Hamiltonian matrix elements for the hopping term (each hop changes one site occupation); (c) for L=20 sites, compare the Hilbert space sizes.

[Ans: (a) L=12: C(12,6)^2 = 924^2 = 854,296 states; (b) Hopping connects nearest-neighbour sites: ~2L x N\_half states; non-zeros approx 2 x 12 x 854,296 = 20.5 million; (c) L=20: C(20,10)^2 = 184,756^2 = 3.41x10^{10} states. A 40,000x increase from L=12 to L=20.]

9.7  ZNE is applied with three noise scale factors lambda = 1, 2, 3 giving measured values <Z>(1) = 0.55, <Z>(2) = 0.39, <Z>(3) = 0.23. (a) Fit a linear extrapolation to lambda=0. (b) Fit a quadratic and extrapolate to lambda=0. (c) Comment on which is more reliable.

[Ans: (a) Linear: gradient = (0.23-0.55)/(3-1) = -0.16; intercept = 0.55 - (-0.16)(1) = 0.71. ZNE\_linear = 0.71; (b) Quadratic through 3 points: use Lagrange: <Z>(0) = 1.5 x 0.55 - 3 x 0.39 + 1.5 x 0.23 = 0.825 - 1.17 + 0.345 = 0.000... let me recompute: 0.55(1-2)(1-3)/((0-1)(0-2)) + 0.39(0-1)(0-3)/((2-1)(2-3)) + 0.23(0-1)(0-2)/((3-1)(3-2)) = 0.55(6/2) + 0.39(-3/-1) + 0.23(2/2) = 0.55 x 3 - 0.39 x 3 + 0.23 = 1.65 - 1.17 + 0.23 = 0.71. Same result. (c) Consistent linear/quadratic extrapolation validates that noise model is well-described by linear lambda-dependence.]

9.8  The H₂ molecule UCCSD ansatz has 3 parameters for the 4-qubit (STO-3G) system. Using the parameter shift rule, (a) how many circuits per gradient step? (b) Assuming COBYLA uses 50 function evaluations and gradient descent uses 30 steps, which needs fewer total circuit runs? (c) At 1000 shots each, compute total shot counts.

[Ans: (a) Parameter shift: 2 circuits per parameter + 1 base = 2 x 3 + 1 = 7 circuits/gradient step; over 30 steps: 210 total; (b) COBYLA: 50 evaluations (fewer). For 3 parameters, gradient-free wins. (c) COBYLA: 50 x 1000 = 50,000 shots; gradient descent: 210 x 1000 = 210,000 shots.]

9.9  Black-Scholes call option: S=100, K=100, r=0.05, T=0.5, sigma=0.2. (a) Calculate the classical price. (b) If QAE reprices 5000 options daily with 100x speedup, what is the equivalent classical compute saved per year?

[Ans: (a) d1 = (ln(1) + (0.05+0.02)\*0.5)/(0.2\*sqrt(0.5)) = (0+0.035)/0.1414 = 0.247; d2 = 0.247-0.1414 = 0.106; C = 100\*N(0.247) - 100\*e^{-0.025}\*N(0.106) = 100\*0.598 - 97.53\*0.542 = 59.8 - 52.9 = 6.9. (b) Classical calls saved = 5000 x 100 x 252 = 126 million per year.]

9.10  Estimate classical and quantum resources for simulating the active site of cytochrome P450 (drug metabolism enzyme) with 75 electrons in 75 active orbitals. (a) CCSD(T) FLOPs assuming O(M^7) scaling with M=150 spin-orbitals. (b) Physical qubits needed for JW VQE. (c) Logical qubits for fault-tolerant QPE (estimate proportional to active space size relative to FeMoco: 111 qubits for 54 orbitals). (d) Why is this a critical pharmaceutical target?

[Ans: (a) CCSD(T) FLOPs = 150^7 = 1.7 x 10^{15}: borderline at petaflop scale but memory = 150^8/8 bytes = 4 x 10^{17} bytes: impossible. (b) JW qubits = 150; (c) logical qubits (linear scaling estimate) = 111 x (75/54) = 154 logical qubits; (d) Cytochrome P450 metabolises ~50% of all drugs — accurate simulation predicts drug metabolism rates, side effects, and drug-drug interactions, potentially replacing expensive Phase I clinical trials for metabolism studies.]

## Theory Questions — Chapter 9

- 1.  Explain why the classical computational complexity of exact quantum chemistry scales exponentially with the number of electrons. Describe the hierarchy HF -> MP2 -> CCSD -> CCSD(T) -> Full CI in terms of accuracy and scaling. Identify precisely where the classical "accuracy wall" lies and why it is relevant to drug discovery.

- 2.  Derive the Jordan-Wigner mapping for a three-orbital system (orbitals 0, 1, 2). Write the full qubit Pauli representation of creation operators a\_0-dagger, a\_1-dagger, a\_2-dagger. Verify that the fermionic anticommutation relation {a\_1, a\_2-dagger} = 0 is satisfied by the mapped operators.

- 3.  Explain the Bravyi-Kitaev mapping and why it achieves O(log N) Pauli weight. Describe the hierarchical binary tree structure conceptually. For a four-orbital system (M=4), explicitly write the BK-mapped creation operators and compare the Pauli weight at each orbital with the JW mapping.

- 4.  Describe the VQE algorithm in complete detail. Prove the variational principle mathematically. Compare the UCCSD and Hardware-Efficient ansatze for a 4-qubit (H₂) system: write the explicit UCCSD operator, count the parameters, and explain why UCCSD avoids the barren plateau problem better than random HEA initialisation.

- 5.  Explain the barren plateau problem mathematically. Under what conditions do gradients vanish exponentially? Which of HEA, UCCSD, and ADAPT-VQE is most susceptible? Describe three mitigation strategies with physical justification for each.

- 6.  Describe the Quantum Phase Estimation algorithm for molecular energies. Starting from |0>^t tensor |Psi\_HF>, trace through the circuit: Hadamards, controlled-U^{2^k} gates, and inverse QFT. Show how the binary representation of phi\_j = E\_j\*tau/(2\*pi) appears in the ancilla register via phase kickback. Compare QPE accuracy and resource requirements with VQE.

- 7.  Formulate a four-asset, two-asset-selection portfolio optimisation problem as a QUBO. Write the full QUBO matrix Q including the return objective and cardinality constraint penalty. Map to an Ising Hamiltonian. Design a QAOA circuit with p=1 for this problem, specifying all gates and the classical optimisation loop.

- 8.  Derive the quadratic speedup of Quantum Amplitude Estimation over classical Monte Carlo. Define the amplitude estimation problem formally, describe the Grover operator Q used in QAE, and explain how QPE applied to Q yields an O(1/N) error estimate. Calculate the explicit speedup for pricing a European option to 0.1% accuracy.

- 9.  Explain the Ising model phase transition at T\_c = 2J/[k\_B ln(1+sqrt(2))] in 2D. Define the order parameter, describe spontaneous symmetry breaking, and derive the mean-field approximation for T\_c. Then describe how the transverse-field Ising model undergoes a quantum phase transition at h/J = 1 and why this is relevant to IBM's 2023 quantum utility experiment.

- 10.  Critically analyse the IBM quantum utility result (Kim et al., Nature 2023). What specific computational task was performed? What is the evidence that the quantum device outperformed classical simulation? What are the two main critiques of the result? What experimental improvements would constitute a stronger demonstration of quantum utility in materials simulation?

## Assignments — Chapter 9

### Assignment 9.1 VQE for H₂ on IBM Quantum (Marks: 10)

(a) Install Qiskit Nature and set up the H₂ problem in STO-3G basis at bond length 0.74 Angstrom using PySCF as the classical driver. Apply the JW mapping and UCCSD ansatz. Run VQE on the Qiskit Aer statevector simulator with COBYLA. Record E\_VQE, iterations to convergence, and final parameters theta\*. (b) Run VQE on a real IBM Quantum processor via IBM Quantum platform free access. Apply ZNE with noise factors 1, 2, 3 and extrapolate. (c) Plot E(theta) vs optimiser iterations for both simulation and hardware. (d) Plot the H₂ binding curve (E vs r from 0.5 to 3.5 Angstrom) from simulation only. Compare HF, VQE-UCCSD, and exact FCI results. Submit all code and a 1500-word analysis.

### Assignment 9.2 QAOA Portfolio Optimisation (Marks: 10)

(a) Select 8 assets from NSE (National Stock Exchange of India) using 3 years of historical daily returns. Compute expected returns r and covariance matrix Sigma using numpy. (b) Formulate the QUBO for selecting 4 from 8 assets minimising portfolio variance at fixed return target (Sharpe ratio maximisation). (c) Implement QAOA with p = 1, 2, 3 layers using Qiskit and optimise with COBYLA. Compare the QAOA solution quality with classical Markowitz optimal. (d) Plot <H\_C> vs iterations for each p. (e) Calculate and compare Sharpe ratios of QAOA vs classical portfolios. Submit 2000-word analysis and all code.

### Assignment 9.3 Quantum Chemistry Benchmarking (Marks: 10)

Using Qiskit Nature and PySCF, benchmark VQE-UCCSD for H₂O (H2O) at equilibrium geometry in STO-3G basis: (a) Map to qubits using both JW and BK mappings. Compare the number of Pauli strings and average Pauli weight. (b) Run VQE-UCCSD with COBYLA on the Aer simulator. Record energy, iterations, and parameters. (c) Add depolarising noise (error rates 0.1%, 0.5%, 1.0%) and measure energy error vs noise level. (d) Apply ZNE post-processing and measure improvement. (e) Compare VQE energy with classical HF, MP2, and CCSD(T) values computed by PySCF. Write a 2000-word report including all Python code submitted to a public GitHub repository.

## References — Chapter 9

- 1.  Feynman, R. P. (1982). Simulating physics with computers. International Journal of Theoretical Physics, 21(6-7), 467-488.

- 2.  Peruzzo, A. et al. (2014). A variational eigenvalue solver on a photonic quantum processor. Nature Communications, 5, 4213.

- 3.  O'Malley, P. J. J. et al. (2016). Scalable quantum simulation of molecular energies. Physical Review X, 6(3), 031007.

- 4.  Kandala, A. et al. (2017). Hardware-efficient variational quantum eigensolver for small molecules and quantum magnets. Nature, 549, 242-246.

- 5.  Hempel, C. et al. (2018). Quantum chemistry calculations on a trapped-ion quantum simulator. Physical Review X, 8(3), 031022.

- 6.  Reiher, M., Wiebe, N., Svore, K. M., Wecker, D., & Troyer, M. (2017). Elucidating reaction mechanisms on quantum computers. PNAS, 114(29), 7555-7560.

- 7.  Bravyi, S. B., & Kitaev, A. Yu. (2002). Fermionic quantum computation. Annals of Physics, 298(1), 210-226.

- 8.  Seeley, J. T., Richard, M. J., & Love, P. J. (2012). The Bravyi-Kitaev transformation for quantum computation of electronic structure. Journal of Chemical Physics, 137(22), 224109.

- 9.  Farhi, E., Goldstone, J., & Gutmann, S. (2014). A quantum approximate optimization algorithm. arXiv:1411.4028.

- 10. Brassard, G., Hoyer, P., Mosca, M., & Tapp, A. (2002). Quantum amplitude amplification and estimation. AMS Contemporary Mathematics, 305, 53-74.

- 11. Woerner, S., & Egger, D. J. (2019). Quantum risk analysis. npj Quantum Information, 5, 15.

- 12. Stamatopoulos, N. et al. (2020). Option pricing using quantum computers. Quantum, 4, 291.

- 13. Grimsley, H. R. et al. (2019). An adaptive variational algorithm for exact molecular simulations on a quantum computer. Nature Communications, 10, 3007.

- 14. McArdle, S. et al. (2020). Quantum computational chemistry. Reviews of Modern Physics, 92(1), 015003.

- 15. Kim, Y. et al. (2023). Evidence for the utility of quantum computing before fault tolerance. Nature, 618, 500-505.

- 16. von Burg, V. et al. (2021). Quantum computing enhanced computational catalysis. Physical Review Research, 3(3), 033055.

- 17. Hubbard, J. (1963). Electron correlations in narrow energy bands. Proceedings of the Royal Society A, 276, 238-257.

- 18. Onsager, L. (1944). Crystal statistics I: A two-dimensional model with an order-disorder transition. Physical Review, 65, 117.

- 19. Cerezo, M. et al. (2021). Variational quantum algorithms. Nature Reviews Physics, 3, 625-644.

- 20. Cao, Y. et al. (2019). Quantum chemistry in the age of quantum computing. Chemical Reviews, 119(19), 10856-10915.
