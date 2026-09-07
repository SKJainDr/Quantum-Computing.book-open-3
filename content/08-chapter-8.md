# CHAPTER 8

# Entanglement Distillation, Quantum Internet, Satellite QKD, and India’s Quantum Network

<div class="box box-learning-objectives">
<p class="box-title"><strong>📋 Learning Objectives</strong></p>
<p>After completing this chapter you will be able to: (1) Explain why entanglement distillation is necessary in real quantum networks and what physical processes cause entanglement degradation; (2) Derive the BBPSSW fidelity improvement formula F’ = F²/(F²+(1−F)²) from the protocol steps and explain its physical meaning; (3) Apply the BBPSSW formula to multi-round distillation calculations and determine the number of initial pairs required; (4) Describe the DEJMPS protocol and explain when it performs better than BBPSSW; (5) State the Hashing bound and explain its significance as the fundamental limit of entanglement distillation; (6) Describe the three stages of the quantum internet (Wehner, Elkouss & Hanson 2018) and specify the hardware milestones separating each stage; (7) Analyse the Micius satellite experiments quantitatively (link budget, Bell parameter, key rate); (8) Explain the physics advantage of satellite-based QKD over fibre for long distances; (9) Describe India’s National Quantum Mission goals, key institutions, and the Delhi–Pune quantum link design.</p>
</div>

## 8.1 Introduction: Why Pure Entanglement Is Never Free

Chapter 7 developed the quantum repeater architecture and showed how entanglement can, in principle, be distributed over arbitrarily long distances using a chain of nodes with quantum memory and entanglement swapping. However, the resulting end-to-end entanglement is invariably noisy. Every physical operation in the chain — photon generation, fibre transmission, beamsplitter interference, Bell state measurement, memory storage and retrieval — introduces some error. These errors accumulate along the chain, degrading the fidelity F = ⟨Φ^+|ρ|Φ^+⟩ of the shared state ρ below unity.

Why does fidelity matter so much? Consider quantum teleportation: if Alice teleports a qubit state |ψ⟩ to Bob using an entangled pair of fidelity F, the teleported state arrives at Bob with fidelity F (for Werner state noise). For F = 0.7, Bob receives a state that is only 70% likely to be the correct state — unacceptable for quantum computing or device-independent QKD applications. Most practical applications require F > 0.99.

Entanglement distillation (also called entanglement purification) solves this problem. By performing local quantum operations on multiple copies of a noisy entangled state and comparing measurement results, Alice and Bob can identify and discard corrupted pairs while concentrating the entanglement into a smaller number of higher-fidelity pairs. The price: multiple pairs are consumed to produce fewer, better pairs. This is analogous to the Shannon data compression and error correction: using redundancy to combat noise. But unlike classical error correction, quantum distillation requires only LOCC — local operations and classical communication — no additional quantum channel.

## 8.2 Entanglement Distillation: BBPSSW Protocol

The BBPSSW (Bennett-Brassard-Popescu-Schumacher-Smolin-Wootters) protocol, published in Physical Review Letters in 1996, was the first practical entanglement distillation protocol. It operates on Werner states, which are the most natural class of noisy entangled states arising from depolarising noise on Bell pairs. A Werner state is defined as:

<table>
<thead><tr>
<th>ρ_Werner = F |Φ⁺⟩⟨Φ⁺ | + (1 - F) · I_4/4</th>
<th><em>Werner state with fidelity F</em></th>
</tr></thead>
<tbody>
</tbody></table>

The Werner state is a convex mixture of the maximally entangled Bell state |Φ^+⟩ (with weight F) and the completely mixed state I_4/4 (with weight 1−F). It arises naturally when a Bell pair passes through a depolarising channel ε with depolarisation probability p: the output fidelity is F = 1 − (3/4)p. Werner states can be distilled by BBPSSW if and only if F > 1/2.

### 8.2.1 Protocol Steps

The BBPSSW protocol requires Alice and Bob each to have two copies of ρ_Werner (pairs labelled 1 and 2). It proceeds in three steps:

1. Step 1 (Bilateral CNOT): Alice applies a CNOT gate with pair 1 as control and pair 2 as target on her side. Bob does the same with his qubits. This 'bilateral CNOT' correlates the error syndrome of the two pairs.
2. Step 2 (Measurement): Alice measures her qubit from pair 2 in the Z-basis and obtains result m_A ∈ {0,1}. Bob does likewise on his qubit from pair 2 and obtains m_B.
3. Step 3 (Classical comparison): Alice and Bob communicate their measurement results via the classical channel. If m_A = m_B (the results agree), pair 1 has been successfully distilled to higher fidelity and is kept. If m_A ≠ m_B, pair 1 is discarded.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image51.png" alt="">
<figcaption></figcaption>
</figure>
</div>

**Figure 8.1: BBPSSW Entanglement Distillation Protocol and Fidelity Curves**

Left: The BBPSSW protocol circuit for one distillation round. Alice (top) and Bob (bottom) each start with two copies of Werner state ρ_F. Alice applies CNOT(pair1, pair2), Bob applies CNOT(pair1, pair2). Both measure pair 2 in Z-basis and compare results via classical channel (orange arrow). If equal (success, probability P_s), pair 1 is retained with improved fidelity F'. Right: Fidelity improvement curves. Blue curve: BBPSSW output fidelity F' vs. input fidelity F for Werner states. Green curve: DEJMPS protocol. The diagonal dashed line marks F_out = F_in (no improvement). Both protocols improve fidelity for F > 0.5. The gold marker shows the worked example: F = 0.75 → F' = 0.90. Multiple rounds (shown by arrows) converge toward F = 1.

### 8.2.2 BBPSSW Fidelity Formula Derivation

To derive the output fidelity, we track how the bilateral CNOT correlates the two Werner states. The key observation: for a Werner state, the error part I_4/4 can be decomposed as a uniform mixture of the four Pauli errors (I, X, Y, Z) applied to one qubit. The bilateral CNOT creates a correlation such that: if both pairs have the same Pauli error (or both are error-free), the measurement results agree (m_A = m_B). If the pairs have different Pauli errors, the results disagree.

More precisely, after the bilateral CNOT:

1. Pair 1 is error-free AND pair 2 is error-free → m_A = m_B = 0 (success); pair 1 remains error-free.
2. Pair 1 has an X error AND pair 2 has an X error → m_A = m_B = 1 (success); the X errors on pair 1 and pair 2 propagate through CNOT and cancel. The retained pair 1 is error-free!
3. Pair 1 is error-free AND pair 2 has an X error → m_A ≠ m_B (failure); pair 1 is discarded.
4. Other Pauli error combinations contribute to both success and failure cases.

For Werner states, the probability of success (m_A = m_B) and the conditional fidelity of the retained pair 1 work out to:

<table>
<thead><tr>
<th>F' = F² / (F² + (1−F)²)</th>
<th><em>BBPSSW output fidelity</em></th>
</tr></thead>
<tbody>
<tr>
<td>P_success = F² + (1 - F)²</td>
<td><em>BBPSSW success probability</em></td>
</tr>
</tbody></table>

These formulas have a clear physical interpretation: F² is the probability that both pairs are 'correct' (both in the |Φ^+⟩ component), and (1−F)² is the probability that both pairs are in the same error state (which also cancels out after the bilateral CNOT). The ratio F²/(F²+(1−F)²) is the fraction of successful attempts that correspond to a correctly distilled pair.

Numerical example: For F = 0.75: F' = 0.5625/(0.5625+0.0625) = 0.5625/0.625 = 0.900. Two pairs at 75% fidelity yield one pair at 90% fidelity with 62.5% probability. For F = 0.90: F' = 0.81/(0.81+0.01) = 0.81/0.82 = 0.988. The protocol is increasingly effective at high fidelity.

<div class="box box-key-concept">
<p class="box-title"><strong>🔑 Why Does F > 0.5 Guarantee Improvement?</strong></p>
<p>For the BBPSSW formula, F' > F whenever F > 0.5. This can be verified by computing dF'/dF|_{F=F}. The derivative equals 2F(1-F)/(F^2+(1-F)^2)^2, which is positive for all F ∈ (0,1). So any starting fidelity above the threshold of F > 1/2 can be improved by BBPSSW.</p>
<p>The threshold F = 0.5 corresponds to the 'separable' boundary: a Werner state with F ≤ 0.5 is not entangled (it has no distillable entanglement). Below F = 0.5, the state is separable — it has no quantum correlations to distill.</p>
<p>Physical intuition: F > 0.5 means the |Φ^+⟩ component is the largest component of the mixture. The bilateral CNOT exploits this to enhance the |Φ^+⟩ fraction at the expense of success probability.</p>
</div>

## 8.3 The DEJMPS Protocol

The DEJMPS (Deutsch-Ekert-Jozsa-Macchiavello-Palma-Sanpera) protocol was proposed independently and almost simultaneously with BBPSSW, also in 1996. It can be understood as a variant of BBPSSW with a different basis choice for the measurement in Step 2. The DEJMPS protocol uses:

1. Step 1: Each party applies a local Y-rotation (by π/4) to their qubit from pair 2, transforming the error basis.
2. Step 2: Bilateral CNOT, as in BBPSSW.
3. Step 3: Both parties measure pair 2 in the X-basis (instead of Z-basis as in BBPSSW) and compare results.

The DEJMPS protocol was designed to operate on a more general class of Bell-diagonal states (not just Werner states), defined as:

<table>
<thead><tr>
<th>ρ = λF |Φ⁺⟩⟨Φ⁺ | + λ_1 |Φ⁻⟩⟨Φ⁻ | + λ_2 |Ψ⁺⟩⟨Ψ⁺ | + λ_3 |Ψ⁻⟩⟨Ψ⁻ |</th>
<th><em>Bell-diagonal state (most general two-qubit mixed state in the Bell basis)</em></th>
</tr></thead>
<tbody>
</tbody></table>

For Bell-diagonal states, the DEJMPS output fidelity formula is more complex but can be computed analytically. For Werner states specifically (where λ_1 = λ_2 = λ_3 = (1−F)/4), DEJMPS and BBPSSW give comparable performance. DEJMPS shows a significant advantage over BBPSSW for Bell-diagonal states where λ_2 (the |Ψ^+⟩ component) is large, as can arise from coherent errors in the quantum channel.

In practice, the choice between BBPSSW and DEJMPS depends on the noise model of the specific quantum channel. For channels with primarily depolarising noise (Werner states), BBPSSW is typically used due to its simpler analysis. For channels with significant Z-phase errors (which can occur in long fibre runs due to birefringence), DEJMPS or its generalisations may be more efficient. Most experimental demonstrations use BBPSSW-type protocols due to their simplicity.

## 8.4 Multi-Round Distillation and the Hashing Bound

A single round of BBPSSW increases fidelity from F to F', consuming 2 pairs to produce 1 pair with probability P_success. To approach F → 1, multiple rounds are required. After k rounds of BBPSSW, the protocol consumes 2^k initial pairs to produce one pair with probability P_total = ∏_{i=1}^{k} P_{success}^{(i)}.

The resource cost grows exponentially with the number of rounds: 2 pairs per round, so 2^k pairs consumed after k rounds. This is acceptable for small k (2–3 rounds is practical) but becomes prohibitive for large k. The practical question is: how many rounds are needed to reach a target fidelity F_target, given a starting fidelity F_0?

Iterated application of the BBPSSW map: F_1 = F_0^2/(F_0^2+(1-F_0)^2), F_2 = F_1^2/(F_1^2+(1-F_1)^2), etc. This converges monotonically to F = 1 as k → ∞ for any starting F_0 > 0.5.

<table>
<thead><tr>
<th>k_rounds ≈ log_2[log_{F′/F}(F_{target}/(1-F_{target}))] + const</th>
<th><em>Approximate rounds needed (see derivation in Theory Question 8.4)</em></th>
</tr></thead>
<tbody>
</tbody></table>

<div class="box box-generic">
<p class="box-title"><strong>📖 The Hashing Bound: Fundamental Limit of Entanglement Distillation</strong></p>
<p>The hashing protocol (Bennett et al., 1996) is an asymptotically optimal distillation protocol that achieves the Hashing bound:</p>
<p>E_D(ρ) ≤ 1 - S(ρ) (for Bell - diagonal states)</p>
<p>where S(ρ) = -Tr(ρ log_2 ρ) is the von Neumann entropy of the joint state. For a Werner state with fidelity F:</p>
<p>S(ρ_Werner) = - F log_2 F - (1 - F) log_2 ((1 - F)/3) - (1 - F)/3 log_2 ((1 - F)/3) × 3</p>
<p>The Hashing bound gives the maximum number of Bell pairs that can be extracted per input copy, using LOCC. For F = 0.75: S(ρ) ≈ 0.81 bits, so E_D ≤ 1 − 0.81 = 0.19 Bell pairs per copy. BBPSSW and DEJMPS are recurrence protocols that use only a finite number of copies and are generally sub-optimal compared to the Hashing bound. For large ensembles of pairs, hashing-based protocols approach the theoretical limit.</p>
</div>

## 8.5 The Quantum Internet Vision: Three Stages

The term 'quantum internet' refers to a global network of quantum devices connected by quantum channels that distribute entanglement. The quantum internet is not a replacement for the classical internet; rather, it is a complementary infrastructure that enables capabilities fundamentally impossible classically: provably secure cryptography certified by quantum mechanics, blind quantum computation (computing on a server without the server seeing the input), distributed quantum sensing, and quantum-enhanced clock synchronisation.

In a landmark 2018 paper in Science, Stephanie Wehner, David Elkouss, and Ronald Hanson (QuTech, Delft University) classified the quantum internet into six stages of development, from 'Trusted-Node Networks' (Stage 1) to 'Fully Quantum Computing Networks' (Stage 6). For clarity, we group these into three broad stages, each representing a qualitative step in capability.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image52.png" alt="">
<figcaption></figcaption>
</figure>
</div>

**Figure 8.2: The Quantum Internet — Three Stages of Development**

The three-stage quantum internet framework (after Wehner, Elkouss & Hanson, Science 2018). Stage 1 (Trusted-Node QKD, teal bar): today’s deployed networks. QKD links between adjacent nodes; keys forwarded through trusted relays. Deployed globally in China, Europe, South Korea, Japan, and India. Stage 2 (Entanglement Distribution Networks, purple bar): genuine entanglement distributed between any two nodes without trusting intermediaries. Enables device-independent QKD, quantum teleportation, and distributed quantum sensing. Currently at laboratory demonstration stage (QuTech 3-node, 2021). Stage 3 (Quantum Computing Networks, green bar): distributed quantum computation with fault-tolerant quantum computers at every node. Enables blind quantum computing, full quantum internet applications. Long-term vision (2040+). The timeline arrow shows where major national programmes currently stand: India NQM targets Stage 1 deployment and Stage 2 research by 2031.

### 8.5.1 Stage 1: Trusted-Node QKD Networks

In Stage 1 networks, each link between adjacent nodes performs QKD (BB84, TF-QKD, or similar protocols) to generate a shared secret key. This key is used to encrypt and forward the final key to the next node, creating a chain of encrypted key hops. Security rests entirely on the assumption that each intermediate relay node is physically secure and not compromised — the 'trusted node' assumption. This is a significant limitation: in a government communications network, trusting every relay node on a 1400 km corridor is a major security risk if any node is infiltrated.

Despite this limitation, Stage 1 networks represent a major practical achievement and are being deployed at scale globally:

1. China (2017–2021): The 2000 km Beijing–Shanghai backbone, operated by QuantumCTek, uses ~32 trusted relay stations and delivers AES-256-encrypted commercial services. Extended in 2021 to a 4600 km integrated space-ground network incorporating the Micius satellite.
2. Europe (2022–present): The OpenQKD consortium (10 countries) and the EuroQCI (European Quantum Communication Infrastructure) programme, targeting a pan-European quantum-secured network by 2027.
3. South Korea: SK Telecom commercial QKD network integrated into their 5G backbone, serving banking and government clients.
4. Japan: NTT-Toshiba consortium quantum communications network for data centre interconnects in Tokyo.
5. India (2022–present): IIT Delhi and DRDO have deployed a 100 km QKD link in the Delhi metropolitan area on existing fibre infrastructure. C-DOT (Centre for Development of Telematics) has demonstrated 20 km free-space QKD on the Deccan Plateau with indigenous BB84 hardware.

<div class="box box-real-world">
<p class="box-title"><strong>🌐 Stage 1 Security: Trusted Nodes vs. End-to-End Quantum Security</strong></p>
<p>A Stage 1 trusted-node network provides quantum-secured links between adjacent nodes, but the end-to-end security is only as strong as the weakest relay node. If an adversary compromises a trusted relay, they can read all keys passing through it.</p>
<p>Stage 2 and beyond eliminate this vulnerability: by distributing genuine entanglement end-to-end (without trusting intermediate nodes), Stage 2 networks achieve security certified by quantum mechanics itself — device-independent QKD.</p>
<p>For the India Delhi–Pune link: Stage 1 (trusted relay QKD) is the near-term target (by 2027–2028). Transitioning to Stage 2 requires quantum memory with T_2 > 10 ms, which is achievable with AFC rare-earth memories — the primary research target of the QuCryptoS hub.</p>
</div>

### 8.5.2 Stage 2: Entanglement Distribution Networks

Stage 2 networks distribute genuine quantum entanglement between any two nodes in the network, without requiring any intermediate node to be trusted. This is achieved using the quantum repeater technology developed in Chapter 7. The key distinction from Stage 1: in a Stage 2 network, a Bell inequality violation can be observed between the end nodes, certifying that the distributed state is genuinely entangled. No classical communication between the nodes can simulate this — it is a quantum property.

Stage 2 enables several capabilities impossible in Stage 1: (i) Device-independent QKD (DI-QKD), where security is certified by the Bell violation without trusting the measurement devices; (ii) Quantum teleportation for quantum state transfer between distant quantum computers; (iii) Quantum-enhanced clock synchronisation (Komar et al., Nature Physics, 2014), where optical clocks connected by an entanglement channel achieve sub-femtosecond precision exceeding classical GPS; (iv) Secure multiparty quantum computation using shared GHZ states.

Hardware requirements for Stage 2 transition: quantum memories with T_2 >> classical link latency (T_2 > 10 ms for 100 km segments); BSM efficiency > 50%; entanglement distillation capability; heralded entanglement generation with fidelity F_raw > 0.7 per segment. These are technically challenging but achievable with current NV-centre and AFC rare-earth memory technology.

<div class="box box-real-world">
<p class="box-title"><strong>🌐 QuTech Three-Node Quantum Network (Delft, 2021)</strong></p>
<p>In May 2021, Pompili et al. (QuTech, Delft University) demonstrated the world’s first genuine multi-node quantum network. Three nodes — Alice, Bob, and Charlie — each equipped with NV-centre quantum memories, were connected by ~25–30 m optical fibres.</p>
<p>The network demonstrated: (1) simultaneous entanglement between Alice–Bob and Bob–Charlie; (2) entanglement swapping to create Alice–Charlie entanglement (the first true two-hop quantum network); (3) quantum teleportation of a qubit state from Alice to Charlie through Bob, without Alice and Charlie sharing a quantum channel.</p>
<p>Performance metrics: end-to-end fidelity F_AC = 0.77 (above the 0.5 threshold for entanglement); entanglement generation rate ~1 Hz (limited by NV centre ZPL emission fraction and collection efficiency); T_2 of NV memories ~ 2 ms.</p>
<p>Significance: This is the first laboratory demonstration of a Stage 2 quantum network capability. Scaling to 30 nodes will require improving the NV emission efficiency (with photonic crystal nanocavities) and extending fibre links to 10–50 km (requiring telecom wavelength conversion or transition to AFC memories at 1532 nm).</p>
</div>

### 8.5.3 Stage 3: Quantum Computing Networks

Stage 3 networks connect quantum computers at distant nodes, enabling distributed quantum computation, blind quantum computing (Broadbent-Fitzsimons-Kashefi protocol, 2009), and distributed quantum algorithms. In a distributed quantum computer, logical qubits are physically located at different nodes connected by a quantum network; two-qubit gates between non-local qubits are implemented using entanglement swapping and quantum teleportation.

Blind quantum computing (BQC) is a particularly compelling Stage 3 application. In BQC, a client (with limited quantum capabilities) can delegate quantum computation to a powerful quantum server, without the server learning anything about the computation — the input, output, or algorithm remain perfectly hidden. The security proof relies on quantum mechanics: the client sends quantum states to the server and the server's operations are 'blind' to the client's encoding.

Stage 3 requirements are the most demanding of any quantum network stage: fault-tolerant quantum computation at every node (requiring ~1000 physical qubits per logical qubit and gate fidelities > 99.99%); entanglement distribution rates sufficient to maintain logical qubit coherence across the network (typically > 10^4 Bell pairs per second per qubit); global quantum time synchronisation. Stage 3 is a long-term vision (target 2040–2050), but laboratory breakthroughs in 2022–2024 (Harvard-QuEra neutral atom processors, Google Willow) are steadily closing the gap to fault-tolerant operation.

## 8.6 Satellite-Based QKD: Micius and the Path to Global Quantum Communication

The fundamental limitation of ground-based fibre QKD networks is the exponential photon loss: η_fibre(L) = 10^{−αL/10}. Even with quantum repeaters, the engineering complexity and cost of deploying quantum memories at every relay station over thousands of kilometres is enormous. Satellite-based QKD offers a fundamentally different approach: instead of propagating quantum signals through kilometres of glass, route them through space, where the dominant loss mechanism is geometric beam divergence rather than material absorption.

### 8.6.1 Why Satellites? The Free-Space Loss Scaling Advantage

The transmission of a photon beam from a satellite at altitude h to a ground receiver involves two loss mechanisms: geometric (diffraction) loss and atmospheric loss. For a transmitter aperture d_T, wavelength λ, and altitude h, the beam radius at the ground is r = 2.44λh/d_T (Rayleigh criterion). The geometric efficiency is:

<table>
<thead><tr>
<th>η_geo = (π d_R^2/4) / (π r^2) = (d_R/2r)^2 = (d_R d_T / (2.44λh))^2</th>
<th><em>Satellite geometric channel efficiency</em></th>
</tr></thead>
<tbody>
</tbody></table>

The key insight: η_geo scales as h^{-2} (quadratic in distance), whereas η_fibre scales as e^{−αh} (exponential). For large h (>200 km), quadratic loss is dramatically less severe than exponential loss. At h = 500 km, η_geo is roughly equivalent to the fibre transmission over only ~15–20 km of fibre.

The atmospheric loss η_atm is a fixed multiplicative factor (independent of orbital altitude) that accounts for absorption and scattering in the ~10 km thick atmosphere. Typical values: η_atm ≈ 0.3–0.7 at night, 0.1–0.4 during daytime (due to solar background photons increasing the noise). The total satellite channel efficiency is:

<table>
<thead><tr>
<th>η_satellite = η_geo × η_atm × η_pointing</th>
<th><em>Total satellite channel efficiency</em></th>
</tr></thead>
<tbody>
</tbody></table>

The pointing efficiency η_pointing accounts for imperfect tracking: the satellite must track the ground station to < 0.5 μrad accuracy during a 100-second overhead pass. This requires active tip-tilt mirrors and gyroscopic stabilisation systems.

### 8.6.2 Micius Satellite: Key Experiments and Results

The Micius (MoZi) satellite, launched by China's QUESS (Quantum Experiments at Space Scale) mission in August 2016, represents the most ambitious quantum communication experiment ever conducted. Developed by a team led by Jian-Wei Pan at the University of Science and Technology of China (USTC), Micius was the first dedicated quantum communication satellite in history.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image53.png" alt="">
<figcaption></figcaption>
</figure>
</div>

**Figure 8.3: Micius Quantum Satellite — Experiments and Ground Network**

Global map of the Micius satellite network. The satellite (teal icon, 500 km altitude) is shown over the Earth with its ground station connections (blue lines): Ngari Observatory, Tibet (4000 m altitude, lowest atmospheric turbulence); Xinglong Station near Beijing; Nanshan Observatory, Xinjiang; Delingha Station, Qinghai; and the Vienna, Austria ground station (gold line, intercontinental QKD). The 2000 km Beijing–Shanghai fibre backbone (teal ground line) is shown for context. The five major milestones achieved by Micius (2016–2022) are listed in callout boxes: (1) 2017: satellite-to-ground QKD at >1 kbps, (2) 2017: Bell test S=2.37 over 1203 km, (3) 2017: ground-to-satellite quantum teleportation >1400 km, (4) 2021: intercontinental secure video call Beijing–Vienna, (5) 2022: first daytime satellite QKD.

Technical specifications: Micius has a mass of 631 kg and orbits at 500 km altitude in a sun-synchronous orbit with an orbital period of ~94 minutes. Each overhead pass of a ground station lasts approximately 100 seconds. The quantum payload includes: a single-photon polarisation modulator (entangled photon source) at 780 nm; a quantum teleportation receiver; and a pointing-acquisition-tracking (PAT) system with < 0.5 μrad accuracy. Ground stations have 0.6–1.2 m aperture telescopes and superconducting nanowire single-photon detectors (SNSPDs).

<div class="box box-real-world">
<p class="box-title"><strong>🌐 Micius — Five Historic Firsts</strong></p>
<p>2017: Satellite-to-ground QKD (Liao et al., Nature 549, 2017). Secure key was distributed at 1.1 kbps from Micius to Xinglong ground station over 1200 km, representing a 20-order-of-magnitude improvement in secure key rate compared to direct fibre at that distance.</p>
<p>2017: Space-based entanglement distribution (Yin et al., Science 356, 2017). Entangled photon pairs were distributed between the Delingha and Lijiang ground stations (1203 km apart) via Micius. The measured Bell parameter S = 2.37 significantly violated the classical bound |S| ≤ 2, certifying genuine quantum entanglement from space.</p>
<p>2017: Ground-to-satellite quantum teleportation (Ren et al., Nature 549, 2017). Photonic qubits were teleported from the Ngari ground station to Micius at 1400 km altitude, achieving the longest-range quantum teleportation ever demonstrated.</p>
<p>2021: Intercontinental secure video call (Chen et al., Nature 589, 2021). A 75-minute quantum-encrypted video conference between Beijing and Vienna was conducted using Micius as a trusted relay, with AES-256 encryption keys distributed via satellite QKD.</p>
<p>2022: Daytime satellite QKD. By reducing the detector noise using narrow-band spectral filtering (1 GHz bandwidth), daytime QKD was demonstrated for the first time, extending the operational window from ~100 s per night pass to continuous 24/7 operation.</p>
</div>

### 8.6.3 Future Satellite Quantum Networks

The Micius mission demonstrated proof-of-principle for all three fundamental quantum networking capabilities — QKD, entanglement distribution, and quantum teleportation — from space. The next step is transitioning from single-satellite demonstrations to constellation networks providing continuous global coverage. A single satellite in low Earth orbit (LEO) has a visibility window of ~100 s per pass at each ground station; to maintain continuous quantum links, multiple satellites in different orbital planes are needed.

<table>
<thead><tr>
<th><strong>Country/Agency</strong></th>
<th><strong>Programme</strong></th>
<th><strong>Satellites</strong></th>
<th><strong>Target Date</strong></th>
<th><strong>Capability</strong></th>
</tr></thead>
<tbody>
<tr>
<td>China</td>
<td>QSS-2 / SQSS</td>
<td>6+ LEO</td>
<td>2025–2028</td>
<td>24/7 global QKD constellation</td>
</tr>
<tr>
<td>Europe (ESA)</td>
<td>SAGA</td>
<td>2 MEO</td>
<td>2026–2028</td>
<td>Quantum-secured satellite comm. demo</td>
</tr>
<tr>
<td>UK</td>
<td>UKspace-QKDN</td>
<td>2 LEO</td>
<td>2026–2028</td>
<td>Satellite QKD for UK government</td>
</tr>
<tr>
<td>USA</td>
<td>DOE/NSF</td>
<td>Multiple</td>
<td>2027–2030</td>
<td>Quantum network testbed in space</td>
</tr>
<tr>
<td>India (NQM)</td>
<td>Q-Sat</td>
<td>1 LEO</td>
<td>2026–2028</td>
<td>Satellite QKD and entanglement demo</td>
</tr>
</tbody></table>

*Table 8.1: Planned satellite quantum communication constellations (2024–2030).*

The long-term vision is a 'quantum GPS' constellation: a network of quantum satellites that can distribute Bell pairs to any two points on Earth simultaneously, enabling genuinely entanglement-based global QKD without any trusted relay. Such a network would require ~20–40 satellites in medium Earth orbit (MEO, ~2000–20000 km), each with an entangled photon source and a pointing system accurate to 0.1 μrad.

## 8.7 India’s Quantum Network: NQM Programme and Delhi–Pune Link

India’s National Quantum Mission (NQM) represents the most substantial national quantum technology investment in the country’s history. Approved by the Union Cabinet in April 2023 with a budget of ₹6003 crore (~$730 million) for 2023–2031, the NQM is designed to place India among the top-four global quantum powers by 2031, with explicit milestones in quantum computing, quantum communications, quantum sensing, and quantum materials.

<div class="figure-block">
<figure class="book-figure">
<img src="content/images/image54.png" alt="">
<figcaption></figcaption>
</figure>
</div>

**Figure 8.4: India’s National Quantum Mission — Quantum Communication Network**

Map of India showing the proposed NQM quantum communication network. Key institutional nodes (gold stars): IIT Delhi (quantum repeater research); IISc Bangalore (entanglement sources); TIFR Mumbai (photonic quantum computing); C-DOT (telecom QKD deployment); DRDO (defence applications). Primary fibre QKD corridor (gold line): Delhi–Agra–Gwalior–Nagpur–Pune–Mumbai (1400 km), using existing RailTel and BSNL OFC infrastructure. Secondary corridors (blue lines): Delhi–Kolkata, Mumbai–Hyderabad–Bangalore, Chennai–Hyderabad. The proposed Q-Sat (teal box, top right) shows the satellite QKD link (dashed gold line) connecting to the Delhi ground station. The NQM budget breakdown: ₹6003 cr total, ~₹1200 cr for QuCryptoS (quantum communications hub), ~₹1500 cr for quantum computing (QuST), ~₹1500 cr for sensing (QuNAT), ~₹800 cr for materials (QuMAT).

### 8.7.1 National Quantum Mission: Structure and Mandate

<div class="box box-real-world">
<p class="box-title"><strong>🌐 India’s National Quantum Mission (NQM) — Overview</strong></p>
<p>Budget: ₹6003 crore (~$730 million), 2023–2031.</p>
<p>Governance: Department of Science and Technology (DST), Ministry of Science and Technology; National Mission Governance Board chaired by Principal Scientific Adviser.</p>
<p>Four Technology Innovation Hubs (TIHs):</p>
<p>• QuST (Quantum Computing): hosted at IIT-X, targeting 1000-qubit quantum computers by 2031.</p>
<p>• QuCryptoS (Quantum Cryptography & Communications): responsible for QKD, QRNG, and quantum networking.</p>
<p>• QuNAT (Quantum Sensing & Metrology): quantum gravimeters, magnetometers, and atomic clocks.</p>
<p>• QuMAT (Quantum Materials): topological quantum materials and photonic quantum devices.</p>
<p>The quantum communications hub (QuCryptoS) targets: (a) indigenous QKD systems for government and defence use; (b) QRNG (quantum random number generators) for banking encryption; (c) a 2000 km quantum-secured fibre backbone; (d) an indigenous quantum satellite (Q-Sat) by 2026–2028.</p>
</div>

### 8.7.2 Key Institutions and Their Contributions

India’s quantum networking ecosystem spans academic, research, and defence institutions, each contributing specific expertise:

<table>
<thead><tr>
<th><strong>Institution</strong></th>
<th><strong>Role</strong></th>
<th><strong>Key Achievement</strong></th>
</tr></thead>
<tbody>
<tr>
<td>IIT Delhi + DRDO</td>
<td>QKD link deployment, repeater R&D</td>
<td>100 km QKD on deployed metro fibre (2022); BB84 with SNSPD, 1 Mbps key rate</td>
</tr>
<tr>
<td>C-DOT (Telematics)</td>
<td>Indigenous QKD hardware</td>
<td>First indigenous BB84 system; 20 km free-space QKD (Deccan Plateau)</td>
</tr>
<tr>
<td>TIFR Mumbai</td>
<td>Entanglement sources, photonics</td>
<td>Integrated photonic QKD chips; entangled photon sources at 810 nm</td>
</tr>
<tr>
<td>IISc Bangalore</td>
<td>Quantum repeater components</td>
<td>Rare-earth crystal AFC memories; entanglement distillation protocols</td>
</tr>
<tr>
<td>IIT Bombay</td>
<td>Photonic chip QKD</td>
<td>On-chip silicon photonics QKD transmitter (1550 nm)</td>
</tr>
<tr>
<td>ISRO (future)</td>
<td>Quantum satellite (Q-Sat)</td>
<td>Planned satellite QKD and entanglement demo by 2026–2028</td>
</tr>
<tr>
<td>RBI + SEBI</td>
<td>QRNG for banking</td>
<td>Pilot deployment of QRNG in Mumbai securities exchange (2023)</td>
</tr>
</tbody></table>

*Table 8.2: India’s quantum networking institution landscape and key achievements (2024).*

### 8.7.3 The Delhi–Pune Quantum Link: Design and Milestones

The Delhi–Pune quantum link is the flagship terrestrial quantum networking project of India’s NQM. The full corridor spans approximately 1400 km via existing optical fibre infrastructure (BSNL OFC and RailTel national railway network), passing through Agra, Gwalior, Jhansi, Nagpur, and Pune. The link design is based on a trusted-node BB84 QKD architecture with SNSPD detectors and decoy-state source, targeting secure key rates of 1 kbps end-to-end.

Link architecture: The 1400 km corridor will be divided into approximately 15–20 trusted relay nodes, each housing: (a) a QKD transmitter/receiver pair (BB84 with decoy states at 1550 nm, pulsed at 1 GHz); (b) SNSPD detectors (efficiency > 90%, dark count rate < 10 cps); (c) a secure key management system with HSM (Hardware Security Module); (d) an authenticated classical communication channel. Key material generated at each trusted node is used to relay-encrypt the end-to-end key using one-time pad encryption of each link key with the next.

<table>
<thead><tr>
<th><strong>Phase</strong></th>
<th><strong>Timeline</strong></th>
<th><strong>Distance</strong></th>
<th><strong>Milestones</strong></th>
</tr></thead>
<tbody>
<tr>
<td>Pilot (Delhi–Agra)</td>
<td>2025–2026</td>
<td>200 km</td>
<td>QKD on RailTel OFC; 10 kbps key rate; first operational test of NQM infrastructure</td>
</tr>
<tr>
<td>Phase 1 (Delhi–Nagpur)</td>
<td>2026–2027</td>
<td>800 km</td>
<td>5 trusted relay stations; SNSPD deployed; 2 kbps end-to-end key rate</td>
</tr>
<tr>
<td>Phase 2 (Full link)</td>
<td>2027–2028</td>
<td>1400 km</td>
<td>Full Delhi–Pune-Mumbai corridor; 15 relays; satellite backup via Q-Sat</td>
</tr>
<tr>
<td>Phase 3 (National backbone)</td>
<td>2029–2031</td>
<td>2000+ km</td>
<td>Five-city ring (Delhi–Kolkata–Chennai–Bangalore–Mumbai–Delhi); Stage 2 pilot</td>
</tr>
</tbody></table>

*Table 8.3: Delhi–Pune quantum link implementation phases and milestones.*

Applications: The Delhi–Pune quantum link will support: (a) Diplomatic communications between MoD, MEA, and PMO in Delhi and key defence installations in Pune (DRDO Defence Research & Development Establishment, DRDL); (b) Financial sector communications with BSE and RBI offices in Mumbai; (c) Research network connectivity between IIT Delhi, IIT Bombay, and TIFR; (d) Long-term transition to device-independent QKD (Stage 2) once AFC quantum memories are deployed at relay stations.

<div class="box box-warning">
<p class="box-title"><strong>⚠ Technical Challenges for the Delhi–Pune Link</strong></p>
<p>Fibre variability: deployed BSNL/RailTel fibres have mixed ages and quality, with some segments at 0.3–0.4 dB/km rather than 0.2 dB/km. A fibre characterisation survey across the full 1400 km corridor is needed before repeater spacing is finalised.</p>
<p>Polarisation management: deployed fibres accumulate polarisation mode dispersion (PMD) from temperature and mechanical stress. Time-bin QKD encoding (used by C-DOT’s system) is immune to PMD and is recommended over polarisation encoding for this link.</p>
<p>SNSPD cooling: SNSPD detectors require cryogenic cooling to ~1–2 K, consuming ~100 W per cryocooler. Scaling to 20 relay stations requires reliable cryogenic infrastructure at sites ranging from urban substations to rural railway junctions — a significant logistics challenge.</p>
<p>Key rate limitation: at 1400 km with trusted relay QKD, each relay adds time delays and potential key-rate bottlenecks. A careful link budget analysis is needed to ensure the 1 kbps target is achievable at the weakest link.</p>
</div>

**RECAP**

*Chapter 8: Entanglement Distillation, Quantum Internet Vision, Satellite QKD, and India’s Quantum Network — Short Answer Questions & Model Answers*

## Short Answer Questions — Chapter 8

*Instructions: Answer each question in 3–6 lines.*

**Q1.** What is satellite quantum communication?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q2.** Why are satellites useful for long-distance quantum communication?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q3.** What is meant by geometric diffraction loss in satellite links?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q4.** Define Quantum Key Distribution (QKD).

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q5.** What is the role of the Micius satellite in quantum communication research?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q6.** What is quantum teleportation?

*[§8.5 — The Quantum Internet Vision]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q7.** What is meant by a trusted-node satellite architecture?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q8.** Differentiate between uplink and downlink quantum communication.

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q9.** What is atmospheric turbulence and how does it affect free-space quantum communication?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q10.** What is the significance of entangled photon sources in satellite communication?

*[§8.6 — Satellite-Based QKD: Micius]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q11.** What is the purpose of classical communication in quantum teleportation?

*[§8.5 — The Quantum Internet Vision]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q12.** What is quantum internet?

*[§8.5 — The Quantum Internet Vision]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q13.** What are the main challenges in building a global quantum internet?

*[§8.5 — The Quantum Internet Vision]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q14.** What is quantum network synchronisation?

*[§8.5 — The Quantum Internet Vision]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

**Q15.** What is the advantage of satellite-based QKD over fibre-based QKD?

*[§8.7 — India's Quantum Network]*

____________________________________________________________________________________________________

____________________________________________________________________________________________________

____________________________________________________________________________________________________

## Model Answers — Chapter 8

**Answer 1:**

<div class="box box-equation">
<p>Satellite quantum communication uses satellites to transmit quantum states or entangled photons between distant ground stations through free-space optical links.</p>
</div>

**Answer 2:**

<div class="box box-equation">
<p>Satellites are useful because free-space loss scales more favourably than fibre loss, enabling quantum communication over thousands of kilometres.</p>
</div>

**Answer 3:**

<div class="box box-equation">
<p>Geometric diffraction loss occurs because an optical beam spreads as it propagates through space, reducing the fraction of photons collected by the receiver.</p>
</div>

**Answer 4:**

<div class="box box-equation">
<p>Quantum Key Distribution is a method of securely sharing cryptographic keys using quantum mechanical principles.</p>
</div>

**Answer 5:**

<div class="box box-equation">
<p>The Micius satellite demonstrated long-distance entanglement distribution, satellite QKD, and quantum teleportation over record distances.</p>
</div>

**Answer 6:**

<div class="box box-equation">
<p>Quantum teleportation is the transfer of an unknown quantum state from one location to another using shared entanglement and classical communication.</p>
</div>

**Answer 7:**

<div class="box box-equation">
<p>In a trusted-node architecture, the satellite temporarily stores or processes key information and must therefore be trusted by communicating parties.</p>
</div>

**Answer 8:**

<div class="box box-equation">
<p>In uplink communication, photons travel from ground to satellite, while in downlink communication photons travel from satellite to ground.</p>
</div>

**Answer 9:**

<div class="box box-equation">
<p>Atmospheric turbulence causes beam wandering, scattering, and phase fluctuations, reducing communication fidelity and transmission efficiency.</p>
</div>

**Answer 10:**

<div class="box box-equation">
<p>Entangled photon sources generate correlated photon pairs required for entanglement distribution, teleportation, and entanglement-based QKD.</p>
</div>

**Answer 11:**

<div class="box box-equation">
<p>Classical communication transmits the measurement results needed for the receiver to reconstruct the original quantum state during teleportation.</p>
</div>

**Answer 12:**

<div class="box box-equation">
<p>The quantum internet is a global network capable of distributing entanglement and enabling quantum communication and distributed quantum computing.</p>
</div>

**Answer 13:**

<div class="box box-equation">
<p>Major challenges include photon loss, decoherence, limited quantum memory, precise synchronisation, and scalable quantum repeater technology.</p>
</div>

**Answer 14:**

<div class="box box-equation">
<p>Quantum network synchronisation ensures that photon generation, transmission, and detection occur at precisely coordinated times across the network.</p>
</div>

**Answer 15:**

<div class="box box-equation">
<p>Satellite-based QKD can achieve global-scale communication with lower effective loss compared to very long optical fibres.</p>
</div>

**Solved Examples — Chapter 8**

**Example 8.1 BBPSSW Protocol: Single Round**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>Two Werner states with F = 0.80 are subjected to one round of the BBPSSW distillation protocol. (a) Calculate the output fidelity F'. (b) Calculate the success probability P_success. (c) If 1000 input pairs are prepared, how many high-fidelity pairs are expected?</p>
</div>

**Solution:**

(a) F' = F² / (F² + (1−F)²) = 0.64 / (0.64 + 0.04) = 0.64 / 0.68 = 0.941

(b) P_success = F² + (1 - F)² = 0.64 + 0.04 = 0.68

(c) Number of successful distillation attempts = (1000 pairs / 2) × P_success = 500 × 0.68 = 340 pairs

Each attempt uses 2 input pairs; 500 attempts from 1000 pairs; 340 succeed.

Result: Starting with 1000 pairs at F = 0.80, we get 340 pairs at F' = 0.941.

**Example 8.2 Multi-Round BBPSSW: Reaching F = 0.99**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>Starting with F_0 = 0.70, apply BBPSSW repeatedly until F ≥ 0.99. (a) Calculate F after each round. (b) Count the pairs consumed. (c) Calculate the overall yield P_total.</p>
</div>

**Solution:**

BBPSSW formula: F_n + 1 = F_n²/(F_n² + (1 - F_n)²), P_n = F_n² + (1 - F_n)²

Round 0: F_0 = 0.700, P_0 = 0.49 + 0.09 = 0.58

Round 1: F_1 = 0.49/0.58 = 0.845, P_1 = 0.714 + 0.024 = 0.738

Round 2: F_2 = 0.714/0.738 = 0.968, P_2 = 0.937 + 0.001 = 0.938

Round 3: F_3 = 0.937/0.938 = 0.999 ≥ 0.99 ✓

(b) Pairs consumed: 2^3 = 8 initial pairs per final pair

(c) P_total = P_0 × P_1 × P_2 × P_3 = 0.58 × 0.738 × 0.938 × 0.999 = 0.402

Result: 40.2% of 3-round attempts succeed. 8 initial pairs → one pair at F' = 0.999, 40.2% of the time.

**Example 8.3 Micius Satellite Link Budget**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>The Micius downlink uses: d_T = 0.3 m, d_R = 0.6 m, λ = 785 nm, h = 500 km, η_atm = 0.5, η_pointing = 0.7. (a) Calculate the beam radius at ground. (b) Calculate geometric efficiency. (c) Total channel efficiency. (d) Compare with 500 km fibre.</p>
</div>

**Solution:**

(a) Beam half-angle: θ = 2.44λ/d_T = 2.44×785×10^{-9}/0.3 = 6.38×10^{-6} rad = 6.38 μrad

Beam radius at ground: r = θ × h = 6.38×10^{-6} × 500×10^3 = 3.19 m

(b) η_geo = (d_R/2r)^2 = (0.6/(2×3.19))^2 = (0.094)^2 = 8.85×10^{-3} = 0.885%

(c) η_total = η_geo × η_atm × η_pointing = 0.00885 × 0.5 × 0.7 = 3.1×10^{-3} = 0.31%

(d) η_fibre(500km) = 10^- 0.02 ×500 = 10^- 10 = 10^- 8%

Satellite is 10^{-3}/10^{-10} = 10^7 times better — ten million times more efficient than fibre at 500 km!

**Example 8.4 Bell Parameter from Micius**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>The Micius experiment (Yin et al., Science 2017) measured the CHSH Bell parameter S = 2.37 over 1203 km. (a) What is the theoretical maximum of S for quantum mechanics? (b) What value of S certifies entanglement? (c) If the detectors have efficiency η_det = 0.85, what is the minimum true entanglement fidelity implied by S = 2.37?</p>
</div>

**Solution:**

(a) The maximum quantum mechanical value of the CHSH parameter (Tsirelson bound) is S_max = 2√2 ≈ 2.828.

(b) Any value S > 2 violates the classical CHSH inequality |S| ≤ 2 and certifies quantum entanglement.

(c) For a two-qubit state with fidelity F with respect to a Bell state, the maximum CHSH value is:

S_max(F) = 2√2 × (2F-1) for F > 0.5

S = 2.37 = 2√(2) × (2F - 1): (2F - 1) = S/(2√(2)) = 2.37/2.828 = 0.838

F = (0.838+1)/2 = 0.919

The distributed entanglement had fidelity F ≥ 0.919 1203 km space.

**Example 8.5 Delhi–Agra QKD Link Budget (Pilot Phase)**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>Delhi–Agra pilot link: 200 km fibre (α = 0.2 dB/km), BB84 with decoy states, 1 GHz source, μ = 0.5 photons/pulse, SNSPD efficiency 0.9, dark count rate d = 10^{-7}/pulse, alignment QBER = 1.0%. (a) Link efficiency. (b) Signal count rate. (c) Total QBER. (d) Secure key rate using R = 0.5(1-2H_2(QBER)).</p>
</div>

**Solution:**

(a) η_fibre = 10^- 0.02 ×200 = 10^- 4 = 0.01%

(b) Signal count rate = μ × η_fibre × η_det × R_source = 0.5 × 10^{-4} × 0.9 × 10^9 = 45,000 counts/s

(c) Dark count contribution: d × R = 10^{-7} × 10^9 = 100 dark counts/s

QBER_dark = 100/(2 × 45,000) = 0.11% (small, dark count dominated at short distance)

Total QBER = alignment + dark = 1.0% + 0.11% ≈ 1.11%

(d) H_2(0.0111) = -0.0111×log_2(0.0111) - 0.9889×log_2(0.9889) ≈ 0.089

R_key = 0.5 × (1 - 2×0.089) × 45,000 = 0.5 × 0.822 × 45,000 ≈ 18.5 kbps secure key

An 18.5 kbps secure key rate at 200 km is excellent for the Delhi–Agra pilot phase.

**Example 8.6 Werner State Von Neumann Entropy and Hashing Bound**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>A Werner state has fidelity F = 0.80. (a) Calculate the von Neumann entropy S(ρ). (b) State the Hashing bound for distillable entanglement E_D. (c) How many Bell pairs can theoretically be extracted per 100 input copies?</p>
</div>

**Solution:**

(a) Werner state eigenvalues: λ_1 = F+(1-F)/4 = F+(1-F)/4; wait, Werner state eigenvalues are:

For ρ = F |Φ⁺⟩⟨Φ⁺ | + (1 - F)I_4/4:

Eigenvalues: λ_1 = F + (1-F)/4 = (1+3F)/4, λ_{2,3,4} = (1-F)/4

For F = 0.80: λ_1 = (1+2.4)/4 = 0.85; λ_{2,3,4} = 0.05

S = - 0.85 log_2(0.85) - 3 ×0.05 log_2(0.05) = 0.234 + 0.649 = 0.883 bits

(b) Hashing bound: E_D ≤ 1 - S(ρ) = 1 - 0.883 = 0.117 Bell pairs per input copy

(c) From 100 input copies: at most 100 × 0.117 = 11.7 Bell pairs can be extracted.

Note: BBPSSW achieves much less than this (recurrence protocols are sub-optimal).

BBPSSW from F=0.80: after 2 rounds, F'≈0.999, consuming 4 pairs per output. Yield: ~1/4 = 0.25 pairs per input pair. Hashing bound allows up to 0.117 pairs per input — so BBPSSW is actually sub-optimal but better than 1/4 only for more rounds.

**Example 8.7 Quantum Internet Stage Classification**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>For each of the following quantum network capabilities, identify whether they belong to Stage 1, Stage 2, or Stage 3: (a) BB84 QKD over 1000 km using 10 trusted relay stations; (b) Device-independent QKD certified by a loophole-free Bell test between Alice and Bob; (c) Blind quantum computation on a remote quantum server; (d) Quantum teleportation of a logical qubit between two quantum computers.</p>
</div>

**Solution:**

(a) BB84 QKD with trusted relays: Stage 1 (Trusted-Node QKD Network).

Reasoning: no entanglement is distributed end-to-end; security requires trusting each relay.

(b) Device-independent QKD (DI-QKD): Stage 2 (Entanglement Distribution Network).

Reasoning: DI-QKD requires distributing genuine entanglement end-to-end and performing a

loophole-free Bell test. This is impossible with Stage 1 trusted-relay architecture.

(c) Blind quantum computation: Stage 3 (Quantum Computing Network).

Reasoning: BQC requires quantum state transmission between client and server (quantum channel)

and fault-tolerant quantum computation at the server. Both require Stage 3 capabilities.

(d) Logical qubit teleportation between quantum computers: Stage 3.

Reasoning: requires fault-tolerant quantum memory and QEC at both nodes, plus high-fidelity

entanglement distribution between them. Stage 3 minimum requirement.

**Example 8.8 India NQM Target Assessment**

<div class="box box-generic">
<p class="box-title"><strong>📝 Problem</strong></p>
<p>India’s NQM targets a 2000 km QKD network by 2031 with: 1 kbps secure key rate end-to-end, 15 trusted relay stations, BB84 with SNSPD detectors. (a) Calculate the minimum required key rate per link segment (assuming the links are the bottleneck). (b) If each segment is 133 km (≈ 2000/15 km), can a 1 kbps end-to-end rate be achieved with the specified system?</p>
</div>

**Solution:**

(a) With 15 relays creating 16 segments, and assuming the end-to-end rate equals the minimum

per-segment rate (since keys are relay-encrypted hop by hop): R_segment = R_e2e = 1 kbps.

(Each segment must generate keys at ≥ 1 kbps continuously; the limiting segment sets the rate.)

(b) Segment length = 2000/16 ≈ 125 km. Fibre attenuation: η = 10^{-0.02×125} = 10^{-2.5} = 3.16×10^{-3}

Signal count rate: μ × η × η_det × R = 0.5 × 3.16×10^{-3} × 0.9 × 10^9 = 1.42×10^6 counts/s

QBER ≈ 2% (alignment + dark counts); H_2(0.02) ≈ 0.142

R_key = 0.5 × (1-2×0.142) × 1.42×10^6 = 0.5 × 0.716 × 1.42×10^6 = 508 kbps

The 1 kbps target is easily achievable with ~508 kbps margin at 125 km per segment.

The true constraint is key management, trusted relay security, and not the raw QKD rate.

**Multiple Choice Questions — Chapter 8**

*Note: Answers are collected at the end of this chapter.*

<div class="box box-generic">
<p><strong>1. The BBPSSW fidelity formula F' = F²/(F²+(1-F)²) applies to Werner states. The distillation threshold F > 0.5 arises because:</strong></p>
</div>

<div class="box box-generic">
<p><strong>2. Applying BBPSSW to F = 0.80, the output fidelity is:</strong></p>
</div>

<div class="box box-generic">
<p><strong>3. The Hashing bound for entanglement distillation states that:</strong></p>
</div>

<div class="box box-roadmap">
<p class="box-title"><strong>4. The DEJMPS protocol differs from BBPSSW in that it:</strong></p>
</div>

<div class="box box-generic">
<p><strong>5. In the Wehner-Elkouss-Hanson three-stage quantum internet framework, device-independent QKD (DI-QKD) first becomes possible at:</strong></p>
</div>

<div class="box box-generic">
<p><strong>6. The primary security limitation of Stage 1 trusted-node QKD networks is that:</strong></p>
</div>

<div class="box box-generic">
<p><strong>7. The Micius satellite achieved an effective link efficiency equivalent to approximately:</strong></p>
</div>

<div class="box box-generic">
<p><strong>8. The geometric efficiency of a satellite QKD link scales as the satellite altitude h to the power:</strong></p>
</div>

<div class="box box-generic">
<p><strong>9. The Micius satellite’s Bell test (Yin et al., Science 2017) measured S = 2.37 over 1203 km. This violated the classical CHSH inequality because:</strong></p>
</div>

<div class="box box-generic">
<p><strong>10. India’s National Quantum Mission quantum communications hub (QuCryptoS) is primarily tasked with:</strong></p>
</div>

<div class="box box-generic">
<p><strong>11. The preferred QKD encoding for long-distance Indian fibre links (Delhi–Pune corridor) is time-bin rather than polarisation because:</strong></p>
</div>

<div class="box box-generic">
<p><strong>12. The QuTech three-node quantum network (Delft, 2021) was operating at approximately which stage of the quantum internet?</strong></p>
</div>

<div class="box box-generic">
<p><strong>13. For the Delhi–Agra pilot QKD link (200 km), the dominant source of QBER at typical system parameters is:</strong></p>
</div>

<div class="box box-generic">
<p><strong>14. The C-DOT (Centre for Development of Telematics) contribution to India’s quantum networking programme is significant because:</strong></p>
</div>

<div class="box box-generic">
<p><strong>15. In the context of entanglement distillation, the term LOCC means:</strong></p>
</div>

**MCQ Answers — Chapter 8**

<table>
<thead><tr>
<th><strong>Q1</strong></th>
<th><strong>Q2</strong></th>
<th><strong>Q3</strong></th>
<th><strong>Q4</strong></th>
<th><strong>Q5</strong></th>
<th><strong>Q6</strong></th>
<th><strong>Q7</strong></th>
<th><strong>Q8</strong></th>
<th><strong>Q9</strong></th>
<th><strong>Q10</strong></th>
</tr></thead>
<tbody>
<tr>
<td><strong>B</strong></td>
<td><strong>C</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>A</strong></td>
<td><strong>B</strong></td>
</tr>
<tr>
<td><strong>Q11</strong></td>
<td><strong>Q12</strong></td>
<td><strong>Q13</strong></td>
<td><strong>Q14</strong></td>
<td><strong>Q15</strong></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td><strong>B</strong></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>

**Unsolved Problems — Chapter 8**

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.1</strong></p>
<p>Starting with F_0 = 0.65, apply BBPSSW until F ≥ 0.95. (a) Give F after each round. (b) Total pairs consumed. (c) Overall success probability.</p>
<p><em>[Ans: (a) R1: F_1=0.65²/(0.65²+0.35²)=0.423/0.545=0.776; R2: F_2=0.776²/(0.776²+0.224²)=0.602/0.652=0.923; R3: F_3=0.923²/(0.923²+0.077²)=0.852/0.858=0.993≥0.95✓; (b) 2^3=8 pairs per output; (c) P=0.545×0.652×0.858=0.305=30.5%]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.2</strong></p>
<p>A Werner state with F = 0.72 is to be distilled to F’ = 0.90 using BBPSSW. (a) Is one round sufficient? (b) If not, how many rounds are needed? (c) Calculate the final yield (output pairs per 1000 initial pairs).</p>
<p><em>[Ans: (a) After 1 round: F_1=0.72²/(0.72²+0.28²)=0.518/0.597=0.868 < 0.90, not sufficient; (b) After 2 rounds: F_2=0.868²/(0.868²+0.132²)=0.753/0.771=0.977 > 0.90, 2 rounds needed; (c) P_round1=0.597, P_round2=0.771; from 1000 pairs: 500 attempts, 500×0.597=299 survive R1, 299/2=150 attempts for R2, 150×0.771=116 final pairs]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.3</strong></p>
<p>The Hashing bound gives E_D ≤ 1−S(ρ). For a Werner state with F = 0.90, (a) calculate S(ρ), (b) calculate E_D, (c) how does BBPSSW compare (BBPSSW yield after 1 round from F=0.90)?</p>
<p><em>[Ans: (a) λ_1=(1+3×0.9)/4=0.925; λ_{2,3,4}=0.025; S=-0.925log_2(0.925)-3×0.025log_2(0.025)=0.108+0.353=0.461 bits; (b) E_D≤1-0.461=0.539 pairs per input; (c) BBPSSW from F=0.90: F'=0.81/0.82=0.988, P_s=0.82; yield = 0.82/2=0.41 pairs per input pair. Hashing bound 0.539 > BBPSSW yield 0.41, confirming BBPSSW is sub-optimal]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.4</strong></p>
<p>A satellite at altitude h = 600 km uses λ = 810 nm, d_T = 0.2 m, d_R = 0.8 m, η_atm = 0.45, η_pointing = 0.65. (a) Geometric efficiency. (b) Total efficiency. (c) Equivalent fibre distance (α = 0.2 dB/km).</p>
<p><em>[Ans: (a) r = 2.44×810e-9/0.2×600e3 = 5.93 m; η_geo=(0.4/5.93)^2=0.00456=0.456%; (b) η_total=0.00456×0.45×0.65=0.00133=0.133%; (c) Equivalent fibre: -10log10(0.00133)=28.8 dB; L_eq = 28.8/0.2 = 144 km fibre equivalent]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.5</strong></p>
<p>India NQM Delhi–Nagpur segment (800 km, 5 relay stations). BB84 at 1 GHz, μ=0.5, SNSPD 0.90, alignment QBER=1.5%. (a) Per-segment length. (b) η_fibre per segment. (c) Key rate per segment. (d) End-to-end rate (limited by weakest segment, all equal here).</p>
<p><em>[Ans: (a) L_seg = 800/6 ≈ 133 km; (b) η = 10^{-0.02×133} = 10^{-2.67} = 2.14×10^{-3}; (c) Q_sig = 0.5×2.14×10^{-3}×0.9×10^9 = 963,000 counts/s; H_2(0.015) ≈ 0.112; R_key = 0.5×(1-0.224)×963,000 = 374 kbps; (d) End-to-end rate = weakest segment = 374 kbps >> 1 kbps target]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.6</strong></p>
<p>Show that the BBPSSW distillation threshold is exactly F = 0.5 by proving that F' > F iff F > 0.5.</p>
<p><em>[Ans: F' = F²/(F²+(1-F)²). F' > F iff F² > F(F²+(1-F)²) iff F > F²+(1-F)² iff F > F²+1-2F+F² = 2F²-2F+1 iff 0 > 2F²-3F+1 = (2F-1)(F-1) iff (2F-1)(F-1) < 0. Since F<1 always (physical state), F-1<0. So we need 2F-1>0, i.e., F>1/2. QED.]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.7</strong></p>
<p>The Micius satellite demonstrated intercontinental QKD at ~1 kbps between Beijing and Vienna (total path ~8000 km). Compare this rate with: (a) fibre QKD at 8000 km; (b) the China 2000 km backbone key rate.</p>
<p><em>[Ans: (a) η_fibre(8000km) = 10^{-0.02×8000} = 10^{-160}, essentially zero; QKD is impossible at 8000 km fibre without quantum repeaters; (b) China backbone: ~1000 kbps average per link (100 km segments at ~0.2 dB/km); end-to-end at 2000 km with 32 relays: limited by relay processing ~100 kbps; Micius 1 kbps is ~100x slower but covers 8000 km with no terrestrial infrastructure]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.8</strong></p>
<p>A Stage 2 quantum network segment generates entangled pairs with F_seg = 0.88 at rate 10 kHz. BBPSSW distillation (1 round) improves fidelity. (a) Post-distillation fidelity. (b) Post-distillation rate. (c) After entanglement swapping (F_BSM = 0.96), what is the end-to-end fidelity for a 2-segment chain?</p>
<p><em>[Ans: (a) F' = 0.88²/(0.88²+0.12²) = 0.774/(0.774+0.0144) = 0.982; (b) P_s = 0.774+0.0144=0.788; R_after = 0.788 × (10 kHz)/2 = 3.94 kHz (2 pairs consumed per output); (c) After swapping: F_e2e = F'^2 × F_BSM + correction ≈ 0.982^2 × 0.96 = 0.924]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.9</strong></p>
<p>Calculate the number of photons received at the ground station from Micius per second, given: source rate 6 MHz pair/s, η_total = 3.1×10^{-3} (from Solved Example 8.3), one photon per pair sent downward, detector efficiency 0.9.</p>
<p><em>[Ans: Received rate = source rate × η_total × η_det = 6×10^6 × 3.1×10^{-3} × 0.9 = 6×10^6×2.79×10^{-3} = 16,740 photons/s ≈ 16.7 kHz. At 100 s per pass: 1.67×10^6 photon pairs per pass, yielding ~1.67×10^6 × sifting × key fraction ≈ 1 kbps, consistent with Micius reported key rate.]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 8.10</strong></p>
<p>Evaluate the feasibility of a Stage 2 quantum network capability (genuine end-to-end entanglement) for the Delhi–Agra pilot link (200 km) by 2028. State the required T_2, BSM efficiency, and minimum entanglement generation rate per segment (100 km). Which memory platform from Chapter 7 is most suitable?</p>
<p><em>[Ans: Requirements: T_2 > round-trip latency = 2×100km/(2×10^8 m/s) = 1 ms; recommend T_2 > 50 ms for practical operation. BSM efficiency > 50%. Entanglement rate per 100 km segment: with η_seg=10^{-2}, DLCZ at R_rep=10MHz: R_ent≈0.01×10MHz=100 kHz (>1 kHz needed). Platform assessment: Atomic ensemble (T_2~1ms) marginal; NV at 4K (T_2~1s) adequate but non-telecom wavelength; AFC Er:YSO (T_2>6hr, 1532nm, telecom) — ideal. Recommendation: AFC rare-earth at 4K for Stage 2 Delhi–Agra by 2028.]</em></p>
</div>

**Theory Questions — Chapter 8**

1. Derive the BBPSSW fidelity formula F' = F²/(F²+(1-F)²) from first principles. Begin with two Werner states ρ = F|Φ^+⟩⟨Φ^+| + (1-F)I_4/4. Write out the Pauli error decomposition of each Werner state. Apply the bilateral CNOT circuit. Determine the measurement outcomes for each combination of Pauli errors and identify which lead to success (m_A = m_B) and which to failure. Derive both the success probability and conditional output fidelity.
2. Compare the BBPSSW and DEJMPS entanglement distillation protocols in detail. For Werner states, show algebraically that both protocols converge to F = 1 for any F > 0.5 under repeated application. For which noise model does DEJMPS outperform BBPSSW significantly? Describe a physical scenario (type of quantum channel) where this advantage is relevant.
3. Explain the concept of the Hashing bound E_D(F) = 1 - S(ρ_Werner) in terms of the Shannon entropy analogy. Why can hashing-based protocols asymptotically achieve this bound while recurrence protocols (BBPSSW, DEJMPS) cannot? What physical resources would a hashing protocol require that make it impractical for near-term networks?
4. Describe the three stages of the quantum internet (Wehner-Elkouss-Hanson framework) with reference to: (a) the quantum resource distributed (keys, entanglement, or logical qubits); (b) the trust assumptions required (trusted relays, trusted nodes, no trust); (c) the applications enabled at each stage; (d) the specific hardware threshold that separates Stage 1 from Stage 2, and Stage 2 from Stage 3. For India’s NQM, which stages are achievable by 2031 and which will require beyond-2031 technology?
5. Derive the satellite QKD geometric efficiency formula η_geo = (d_R/2r)² where r = 2.44λh/d_T is the beam radius at the ground. Starting from the Fraunhofer diffraction limit for a circular aperture, show that the loss scales as h⁻². Compare the loss at h = 500 km with the loss in 500 km of optical fibre (α = 0.2 dB/km). Calculate the ‘break-even distance’ beyond which satellite is more efficient than fibre.
6. Analyse the Micius satellite entanglement distribution experiment (Yin et al., Science 2017). What Bell inequality was tested, and what value of S = 2.37 signifies? Derive the minimum entanglement fidelity implied by S = 2.37. What loopholes remain in the Micius Bell test (communication loophole, detection loophole, freedom-of-choice loophole) and how do they affect the interpretation of the result?
7. Critically evaluate India’s NQM quantum communications programme. (a) Is the 2000 km fibre backbone target technically feasible by 2031, given the analysis of the Delhi–Pune link in Section 8.7.3? Justify using QKD link budget calculations. (b) Is the Q-Sat satellite target by 2026–2028 achievable, given that India’s ISRO has no prior space-qualified quantum photonics payload? (c) What single technical investment would most accelerate India’s transition from Stage 1 to Stage 2 quantum networking?
8. Explain why entanglement distillation (LOCC) cannot be used to increase the amount of entanglement on average. Prove the LOCC-monotone property: for any LOCC operation Λ, E(Λ(ρ)) ≤ E(ρ), where E is an entanglement measure. How does this reconcile with the apparent improvement in fidelity in BBPSSW? (Hint: consider the fraction of successful vs. failed attempts.)
9. The QuTech three-node network demonstrated Stage 2 quantum networking in a laboratory. Describe the scaling challenges in extending this from 3 nodes at 30 m separation to 30 nodes at 100 km separation. What are the three most critical technical improvements needed in each of: (a) quantum memory; (b) photon sources; (c) detectors; (d) classical control electronics?
10. Design a complete entanglement distillation protocol for a Stage 2 quantum network segment of 100 km, starting from entangled pairs with F_raw = 0.75 and using BBPSSW distillation. (a) Determine the number of distillation rounds needed to reach F ≥ 0.95. (b) Calculate the post-distillation pair generation rate given R_raw = 10 kHz per segment. (c) Determine the total resource overhead (initial pairs per final pair). (d) After distillation, the pairs are used for quantum teleportation. What is the teleportation fidelity?

**Assignments and Project Suggestions — Chapter 8**

### Assignment 8.1 Entanglement Distillation Protocol Comparison (Marks: 10)

Implement both the BBPSSW and DEJMPS protocols in Python using the QuTiP library: (a) Simulate both protocols on Werner states with F = 0.65, 0.75, 0.85, 0.95 for up to 5 rounds. (b) Plot the fidelity as a function of round number for both protocols on the same graph. (c) Compare the resource cost (initial pairs consumed per final pair at F > 0.99) for both protocols at each starting fidelity. (d) Simulate the effect of imperfect bilateral CNOT gates (gate error probability ε = 0.01) on the convergence of both protocols. Does BBPSSW or DEJMPS degrade more gracefully? Submit Python code, 4 figures, and a 1500-word analysis.

### Assignment 8.2 Satellite QKD Link Budget Design (Marks: 10)

Design a complete satellite QKD link budget for an Indian Q-Sat satellite at altitude h = 600 km communicating with the Delhi Cantt ground station: (a) Specify the optical parameters: transmitter aperture, wavelength, detector aperture, atmospheric model. (b) Calculate the link efficiency as a function of elevation angle (10° to 90°). (c) Using the standard BB84 QBER and key rate formula, calculate the secure key bits per satellite pass (100 s at zenith). (d) Design a minimal constellation of Q-Sat satellites needed for daily key refresh (~1 MB/day) between 5 Indian cities. Submit a 2000-word technical report with link budget table and constellation design diagram.

### Project Suggestion 8.A India Quantum Network Simulator

Build a complete simulation of India’s NQM quantum communication network using Python: (a) Implement the 2000 km Delhi–Pune–Mumbai corridor with configurable trusted relay node spacing. (b) Model BB84 QKD with decoy states on each segment, including fibre loss, dark counts, and alignment QBER. (c) Calculate the end-to-end secure key rate as a function of relay spacing and identify the optimal configuration. (d) Extend the simulation to include a satellite link (Q-Sat) as backup for the weakest segment. (e) Estimate the total capital cost of deploying the network (cost per km fibre ₹20 lakh/km, SNSPD per station ₹5 crore, relay station ₹10 crore). Write a 3500-word simulation report.

### Project Suggestion 8.B Comparative National Quantum Network Analysis

Prepare a detailed comparative analysis of quantum network programmes in three countries: China (Beijing–Shanghai backbone + Micius), Europe (OpenQKD + EuroQCI), and India (NQM + Delhi–Pune link): (a) Technical comparison: network topology, QKD protocol, distance, key rate, and current deployment status. (b) Institutional comparison: funding levels, lead organisations, and timeline. (c) Technology gap analysis: identify where India lags or leads relative to China and Europe in specific quantum networking technologies. (d) Strategic recommendations: propose three specific technical areas where India should invest to close the gap by 2031. Deliver a 3000-word report and a 10-slide presentation.

### Project Suggestion 8.C Blind Quantum Computing Protocol Study

Study the Broadbent-Fitzsimons-Kashefi (BFK) blind quantum computing protocol and its quantum networking requirements: (a) Describe the BFK protocol step by step, explaining how the server’s operations are ‘blind’ to the client’s encoding. (b) Identify the minimum quantum networking capability required (Stage 2 or Stage 3). (c) Design a simple demonstration of BFK for a 4-qubit blind computation using QuTiP. (d) Estimate the quantum channel requirements (entanglement rate, fidelity, memory lifetime) for a practical blind computation between TIFR Mumbai (server) and IIT Delhi (client). Write a 2500-word report.

**References — Unit 4**

1. Briegel, H. J., Dür, W., Cirac, J. I., & Zoller, P. (1998). Quantum repeaters: The role of imperfect local operations in quantum communication. Physical Review Letters, 81(26), 5932.
2. Duan, L.-M., Lukin, M. D., Cirac, J. I., & Zoller, P. (2001). Long-distance quantum communication with atomic ensembles and linear optics. Nature, 414, 413–418. [DLCZ Protocol]
3. Bennett, C. H., Brassard, G., Popescu, S., Schumacher, B., Smolin, J. A., & Wootters, W. K. (1996). Purification of noisy entanglement and faithful teleportation via noisy channels. Physical Review Letters, 76(5), 722. [BBPSSW]
4. Deutsch, D., Ekert, A., Jozsa, R., Macchiavello, C., Palma, G. M., & Sanpera, A. (1996). Quantum privacy amplification and the security of quantum cryptography over noisy channels. Physical Review Letters, 77(13), 2818. [DEJMPS]
5. Wehner, S., Elkouss, D., & Hanson, R. (2018). Quantum internet: A vision for the road ahead. Science, 362(6412), eaam9288.
6. Sangouard, N., Simon, C., de Riedmatten, H., & Gisin, N. (2011). Quantum repeaters based on atomic ensembles and linear optics. Reviews of Modern Physics, 83(1), 33.
7. Liao, S.-K. et al. (2017). Satellite-to-ground quantum key distribution. Nature, 549, 43–47.
8. Yin, J. et al. (2017). Satellite-based entanglement distribution over 1200 kilometers. Science, 356(6343), 1140–1144.
9. Ren, J.-G. et al. (2017). Ground-to-satellite quantum teleportation. Nature, 549, 70–73.
10. Chen, Y.-A. et al. (2021). An integrated space-to-ground quantum communication network over 4,600 kilometres. Nature, 589, 214–219.
11. Pompili, M. et al. (2021). Realization of a multinode quantum network of remote solid-state qubits. Science, 372(6539), 259–264. [QuTech 3-node network]
12. Hensen, B. et al. (2015). Loophole-free Bell inequality violation using electron spins separated by 1.3 kilometres. Nature, 526, 682–686.
13. Lago-Rivera, D. et al. (2021). Telecom-heralded entanglement between multimode solid-state quantum memories. Nature, 594, 37–41.
14. Ma, Y. et al. (2021). One-hour coherent optical storage in an atomic frequency comb memory. Nature Communications, 12, 2381.
15. Kómár, P. et al. (2014). A quantum network of clocks. Nature Physics, 10, 582–587.
16. Ministry of Science and Technology, Government of India (2023). National Quantum Mission: Mission Document and Implementation Plan. Department of Science and Technology, New Delhi.
17. Bhaskar, M. K. et al. (2020). Experimental demonstration of memory-enhanced quantum communication. Nature, 580, 60–64.
18. IIT Delhi – DRDO Quantum Key Distribution Laboratory (2022). Demonstration of 100 km QKD link on deployed optical fibre infrastructure in the Delhi metropolitan area. Technical Report QTDL-2022-04.
19. Broadbent, A., Fitzsimons, J., & Kashefi, E. (2009). Universal blind quantum computation. Proceedings of FOCS 2009, 517–526.
20. Bennett, C. H. et al. (1996). Mixed-state entanglement and quantum error correction. Physical Review A, 54(5), 3824. [Hashing protocol]

M.Sc. Physics — Quantum Computing Specialization | Semester IV

## References and Further Reading

1. Briegel, H.-J., Dur, W., Cirac, J. I., & Zoller, P. (1998). Quantum Repeaters: The Role of Imperfect Local Operations in Quantum Communication. Physical Review Letters, 81(26). [First/second-generation repeater theory]

2. Sangouard, N., Simon, C., de Riedmatten, H., & Gisin, N. (2011). Quantum Repeaters Based on Atomic Ensembles and Linear Optics. Reviews of Modern Physics, 83(1). [DLCZ protocol, atomic-ensemble memories]

3. Duan, L.-M., Lukin, M. D., Cirac, J. I., & Zoller, P. (2001). Long-Distance Quantum Communication with Atomic Ensembles and Linear Optics. Nature, 414. [DLCZ protocol]

4. Wehner, S., Elkouss, D., & Hanson, R. (2018). Quantum Internet: A Vision for the Road Ahead. Science, 362(6412). [Quantum internet stages, roadmap]

5. Liao, S.-K. et al. (2017). Satellite-to-Ground Quantum Key Distribution. Nature, 549. [Micius satellite QKD]

6. Yin, J. et al. (2020). Entanglement-Based Secure Quantum Cryptography over 1,120 Kilometres. Nature, 582. [Micius long-distance entanglement distribution]

7. Bennett, C. H., Brassard, G., Popescu, S., Schumacher, B., Smolin, J. A., & Wootters, W. K. (1996). Purification of Noisy Entanglement and Faithful Teleportation via Noisy Channels. Physical Review Letters, 76(5). [BBPSSW entanglement distillation]

8. Deutsch, D. et al. (1996). Quantum Privacy Amplification and the Security of Quantum Cryptography over Noisy Channels. Physical Review Letters, 77(13). [DEJMPS protocol]

9. National Quantum Mission, Government of India (2023). NQM Programme Document and Delhi-Pune QKD Testbed Reports. https://dst.gov.in [India's quantum network programme]

10. Kimble, H. J. (2008). The Quantum Internet. Nature, 453. [Foundational quantum-internet vision]
