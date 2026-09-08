# UNIT 4 | CHAPTER 7

# Quantum Network Architecture and Repeater Technology

<div class="box box-key-concept">
<p class="box-title"><strong>📋  Learning Objectives</strong></p>
<p>After completing this chapter you will be able to: (1) Describe the full architecture of a quantum network, distinguishing end nodes, repeater nodes, quantum channels, and the classical control layer; (2) Explain why the no-cloning theorem prevents classical amplification of quantum signals and how this motivates the quantum repeater paradigm; (3) Derive the exponential loss formula for optical fibre and calculate per-segment transmission for a multi-node repeater chain; (4) Explain the entanglement swapping protocol mathematically, starting from the two-pair tensor product state and deriving the post-BSM entangled state; (5) Compare first-, second-, and third-generation quantum repeater architectures by their physical operations, rate-scaling laws, and hardware requirements; (6) Describe in physical detail the three leading quantum memory platforms — atomic ensembles (DLCZ), NV-centres in diamond, and rare-earth doped crystals (AFC); (7) Evaluate which memory platform is most suitable for a given distance and application.</p>
</div>

## 7.1 Introduction: The Quantum Communication Problem

The ambition of quantum networking is to connect quantum devices — computers, sensors, memory nodes — across arbitrary distances, distributing entanglement as the fundamental resource for secure communication, distributed computing, and quantum-enhanced sensing. The core obstacle is photon loss. Any quantum channel — whether optical fibre or free space — attenuates the quantum signals that carry entanglement. Classical networks solve loss by signal amplification: an erbium-doped fibre amplifier (EDFA) copies and boosts a classical signal without loss of information. Quantum mechanics forbids this. The no-cloning theorem (Wootters & Zurek, 1982) states that no physical process can create an exact copy of an unknown quantum state: |ψ⟩|0⟩ → |ψ⟩|ψ⟩ for all |ψ⟩ is impossible.

This prohibition has a profound implication: a photon lost in a fibre is lost forever. We cannot amplify, copy, or regenerate it. The probability that a photon survives a fibre of length L km with attenuation coefficient α dB/km is:

<div class="box box-generic">
<p class="box-title"><em>Fibre survival probability</em></p>

</div>

For standard telecommunications fibre at 1550 nm, α = 0.2 dB/km and L\_att ≈ 22 km. At 1000 km, η = 10^{−20} — an astronomically small probability. Even at 10 GHz repetition rates, the expected time to transmit a single photon at 1000 km is ~3 years. This is physically hopeless for direct transmission.

The solution was conceived independently by Briegel, Dür, Cirac and Zoller (BDCZ, 1998) and Duan, Lukin, Cirac and Zoller (DLCZ, 2001): the quantum repeater. Rather than transmitting a photon the full distance, we divide the channel into N short segments, generate entanglement independently over each segment, and use entanglement swapping to combine them into a single long-range entangled pair. No photon ever travels the full distance. This chapter develops the full architecture needed to implement this idea: network nodes, quantum channels, classical control, entanglement swapping, and quantum memory.

## 7.2 Quantum Network Architecture: Nodes, Channels, and Classical Control

A quantum network has a layered architecture analogous to classical networks, but with fundamentally different physical requirements at each layer. Figure 7.1 illustrates the essential elements.

<div class="box box-generic">

<img class="fig-img" src="content/images/image55.png" alt="">
</div>

**Figure 7.1: Quantum Network Architecture — Complete System View**

A four-node quantum network: two end nodes (Alice, teal; Bob, green) and two intermediate quantum repeater nodes (purple). The upper blue arrows represent quantum channels carrying photon pairs; orange boxes represent quantum memories at each repeater node. The lower orange line represents the classical control channel (authenticated, public) carrying heralding signals, BSM outcomes, and timing synchronisation. The dual-layer architecture — quantum channels for entanglement and a classical channel for control — is universal across all quantum network designs. Without the classical channel there is no way to post-process the entanglement and make it useful.

### 7.2.1 End Nodes

End nodes are the terminal points of a quantum network — the parties that use quantum resources for cryptography, computation, or sensing. An end node must be capable of: (a) generating photonic qubits, either by a single-photon emitter (quantum dot, trapped ion, NV centre) or by weak coherent pulse sources with decoy states; (b) storing qubits in local quantum memory to wait for heralding signals; (c) performing local quantum gates for distillation and error correction; (d) measuring qubits in multiple bases and transmitting measurement outcomes via the classical channel.

The most demanding end nodes are quantum computers, which require all of the above plus the ability to perform universal quantum computation. Simpler end nodes — such as QKD terminals — may need only single-photon detection and polarisation analysis. The network architecture must be designed so that simpler end nodes can participate with more complex ones (a principle called delegated quantum computation).

<div class="box box-key-concept">
<p class="box-title"><strong>🔑  Why Memory at the End Node?</strong></p>
<p>Entanglement distribution over a multi-hop network is probabilistic: each segment may or may not succeed on a given attempt. Without quantum memory at the end nodes, a successful segment cannot be 'held' while adjacent segments complete. The end node memory stores the entangled qubit received from its nearest repeater node, waiting for the entire chain to succeed, at which point it holds one half of the end-to-end entangled pair ready for application use.</p>
</div>

### 7.2.2 Quantum Channels: Optical Fibre and Free-Space Links

Quantum channels are the physical links that carry photons between adjacent network nodes. Two implementations are used in practice, each with distinct loss scaling and engineering challenges:

Optical fibre channels offer low cost, existing infrastructure, and controlled routing. The attenuation is α = 0.2 dB/km at 1550 nm for standard single-mode fibre, and α = 0.15 dB/km for ultra-low-loss fibre (Corning SMF-28 ULL). For polarisation-encoded qubits, polarisation mode dispersion (PMD) must be compensated, as differential group delay rotates the qubit state. Time-bin encoding is immune to PMD and is preferred for long-distance fibre QKD. The maximum practical distance for direct single-photon transmission in fibre is ~200–300 km, limited by the product of loss and detector dark count rate.

Free-space optical channels operate in air or vacuum. Terrestrial free-space links (building-to-building, ~1 km) have higher loss per km than fibre due to beam divergence and atmospheric turbulence, but are useful for mobile terminals and locations where fibre is unavailable. Satellite-to-ground links (discussed in Chapter 8) bypass fibre loss scaling entirely: the effective loss from a 500 km orbit is comparable to only ~15–20 km of standard fibre, because loss scales quadratically with distance (geometric diffraction) rather than exponentially.

| Channel Type | Attenuation | Range (direct) | Advantage | Limitation |
|---|---|---|---|---|
| SM fibre (1550 nm) | 0.2 dB/km | ~300 km direct | Low cost, stable, deployed | Exponential loss; PMD for polarisation qubits |
| Ultra-low-loss fibre | 0.15 dB/km | ~400 km direct | Lower loss, large L_att ~29 km | Higher cost, limited availability |
| Free-space (ground) | Variable (turbulence) | ~1–10 km | Mobile, no fibre needed | Atmospheric turbulence; weather-dependent |
| Satellite (downlink) | ~3–6 dB total | 1000+ km | Quadratic (not exponential) loss |  |

*Table 7.1: Comparison of quantum channel types and their properties.*

### 7.2.3 Classical Control Layer and Heralding

The classical control layer is the backbone of every quantum network. It performs four essential functions: (1) Heralding — it announces to the network which photon transmissions succeeded, enabling repeater nodes to know when to attempt entanglement swapping; (2) BSM outcome forwarding — the 2-bit Bell state measurement result from each repeater must reach the distant receiver so the appropriate Pauli correction can be applied; (3) Timing synchronisation — each node must know the precise timing of every pulse at every other node, typically synchronised to GPS or optical atomic clocks; (4) Authentication — classical messages must be authenticated to prevent man-in-the-middle attacks; in QKD networks this uses a small pre-shared secret key (bootstrapped from an initial QKD session).

<div class="box box-warning">
<p class="box-title"><strong>⚠  The Heralding Principle</strong></p>
<p>A quantum repeater node receives photons from its left and right channels probabilistically — fibre transmission is a random success/failure event per pulse. The heralding protocol works as follows: when a photon arrives at a detector at node R, the node sends a classical 'click' signal to the adjacent nodes within one light-travel-time. When both the left and right detectors of a repeater node have 'clicked' (both photons arrived), the node performs a Bell state measurement on its two stored qubits. The heralding click ensures that entanglement swapping is only attempted when both input photons are actually present, dramatically improving the efficiency of the protocol compared to non-heralded swapping.</p>
</div>

## 7.3 Quantum Repeaters: Extending Entanglement Beyond Direct Transmission

The quantum repeater is the central enabling technology for a quantum internet. Its purpose is to distribute entanglement between two distant nodes A and B, using a chain of intermediate nodes that each perform local operations and communicate classically, without ever transmitting quantum information directly between A and B.

### 7.3.1 The Fundamental Problem: Exponential Loss in Fibre

For direct transmission, the entanglement generation rate R\_direct is proportional to the fibre transmission η(L) = 10^{−αL/10}. For L = 1000 km and α = 0.2 dB/km, η = 10^{−20}. Even with a 10 GHz source, the expected time to distribute one entangled pair is ~10^{10} s ≈ 300 years. This is completely impractical.

The quantum repeater solution: divide the total distance L into N segments of length L/N. Each segment is an independent entanglement generation attempt. The per-segment probability is:

<div class="box box-generic">
<p class="box-title"><em>Per-segment transmission probability</em></p>

</div>

For N = 100 segments, each of length 10 km: η\_seg = 10^{−2} = 1%. This is 10^{18} times more likely than direct transmission at 1000 km. The chain then performs entanglement swapping across all N−1 intermediate nodes to assemble the end-to-end entangled pair. The challenge is (a) that all N segments must succeed (possibly at different times), requiring quantum memory, and (b) that swapping operations accumulate errors, requiring distillation.

<div class="box box-generic">

<img class="fig-img" src="content/images/image56.png" alt="">
</div>

**Figure 7.2: Quantum Repeater Chain — Segment Architecture and Loss Scaling**

Left: A 4-segment quantum repeater chain. Each segment has a quantum memory at each end (orange boxes) and a fibre channel in between. Photon sources (diamond emitters) create entangled pairs; successful arrivals are heralded. Entanglement swapping (BSM, purple diamonds at middle nodes) connects adjacent segments. Right: Log-scale plot of entanglement generation rate vs. distance. Direct transmission (red) scales as O(η), failing near 300 km. A 10-segment repeater (blue) scales as O(√η), maintaining useful rates to 2000 km. A 100-segment repeater (green) extends this further, approaching a rate nearly independent of distance (third-generation, gold, with full QEC).

### 7.3.2 Entanglement Swapping: The Core Repeater Operation

Entanglement swapping is the quantum operation that extends entanglement from A–B and B–C to A–C, with B as an intermediate node. It requires no quantum channel between A and C — only local operations at B and classical communication from B to C. The derivation proceeds as follows.

Start with two independent Bell pairs: |Phi^+⟩\_AB between Alice's qubit (A) and B's left qubit (B\_L), and |Phi^+⟩\_{B\_RC} between B's right qubit (B\_R) and Charlie's qubit (C). The full four-qubit state is:

<div class="box box-generic">
<p class="box-title">|Ψ⟩_{AB_LB_RC} = |Φ^+⟩_{AB_L} ⊗ |Φ^+⟩_{B_RC}</p>
<p><em>Initial four-qubit state</em></p>
</div>

Expanding in the Bell basis, the 4-qubit state can be rewritten as a sum over the four Bell states of (B\_L, B\_R):

<div class="box box-generic">
<p class="box-title">|Ψ⟩ = (1/2)[ |Φ^+⟩_{B_LB_R}|Φ^+⟩_{AC} + |Φ^-⟩_{B_LB_R}|Φ^-⟩_{AC} + |Ψ^+⟩_{B_LB_R}|Ψ^+⟩_{AC} + |Ψ^-⟩_{B_LB_R}|Ψ^-⟩_{AC} ]</p>
<p><em>Bell basis expansion</em></p>
</div>

Step 3: Node B performs a Bell state measurement (BSM) on (B\_L, B\_R). This projects A and C into one of the four Bell states, conditioned on B's measurement outcome. B obtains 2 classical bits (the BSM result). Step 4: B sends the 2-bit result to Charlie (C) via the classical channel. Step 5: Charlie applies a local Pauli correction based on B's result:

| B's BSM Result | Resulting A–C State | Charlie's Correction |
|---|---|---|
| \|Phi^+⟩ | \|Phi^+⟩_{AC} | I (do nothing) |
| \|Phi^-⟩ | \|Phi^-⟩_{AC} | Z gate |
| \|Psi^+⟩ | \|Psi^+⟩_{AC} | X gate |
| \|Psi^-⟩ | \|Psi^-⟩_{AC} | XZ gate |

*Table 7.2: Entanglement swapping corrections at Charlie's node.*

The outcome: A and C share a maximally entangled Bell state, even though they never directly interacted. The entanglement has been 'swapped' through node B using only local quantum operations and classical communication. This is the fundamental operation of every quantum repeater.

<div class="box box-generic">

<img class="fig-img" src="content/images/image57.png" alt="">
</div>

**Figure 7.3: Entanglement Swapping Circuit — Step-by-Step Protocol**

Quantum circuit for entanglement swapping at repeater node B. Top two lines: Alice (A) and Bob's left qubit (B\_L), initialised in Bell state |Phi^+⟩\_AB. Bottom two lines: Bob's right qubit (B\_R) and Charlie (C), initialised in Bell state |Phi^+⟩\_{B\_RC}. At node B, a CNOT gate (B\_L control, B\_R target) followed by Hadamard on B\_L implements the Bell measurement. The two measurement outcomes (0 or 1 for each qubit) are sent classically to Charlie. Charlie applies I, X, Z, or XZ based on the outcomes. Final state: A and C share |Phi^+⟩\_{AC}. The coloured arrows indicate the direction of quantum information flow versus classical communication.

### 7.3.3 First, Second, and Third-Generation Quantum Repeater Architectures

Quantum repeater architectures are classified into three generations based on their approach to correcting the two types of errors that degrade long-distance entanglement: photon loss and operation errors (gate infidelities, memory decoherence). Each generation represents a different trade-off between hardware complexity and communication rate.

<div class="box box-real-world">
<p class="box-title"><strong>📡  First-Generation Repeaters: Memory-Assisted Heralding</strong></p>
<p>First-generation repeaters address photon loss but not gate errors. The key innovation is the use of quantum memories to store entanglement from successful segment transmissions while other segments attempt. The protocol is:</p>
<p>1. Each segment attempts entanglement generation repeatedly until a heralded success is obtained.</p>
<p>2. Successful segments store their entangled pair in quantum memory.</p>
<p>3. Once all adjacent segments have succeeded, the repeater node performs a BSM (entanglement swapping) to connect them.</p>
<p>4. Errors accumulate from: memory decoherence during the waiting time, imperfect BSM (incomplete Bell measurements), and multi-photon events in probabilistic sources.</p>
<p>Rate scaling: R_1st ∝ O(η^{1/2}) for a chain of N=2 nodes, improving to O(η^{1/N}) for N nodes. This is a polynomial improvement over direct O(η), extending the useful range from ~200 km to ~1000+ km.</p>
</div>

<div class="box box-real-world">
<p class="box-title"><strong>📡  Second-Generation Repeaters: QEC at Each Node</strong></p>
<p>Second-generation repeaters add quantum error correction (QEC) within each repeater node to suppress gate errors, while still using heralding for photon loss. Each node encodes its qubits into a quantum error-correcting code (typically the 5-qubit perfect code or the 7-qubit Steane code), performs fault-tolerant entanglement swapping, and performs QEC before storing in memory.</p>
<p>Rate scaling: R_2nd ∝ O(η^{1/4}) for a 4-node chain, faster than first-generation. The improvement comes from allowing multiple distillation rounds within each encoded block.</p>
<p>Hardware requirement: gate fidelities exceeding 99.9% within each repeater node, achievable with trapped ions or silicon spin qubits but not yet with current NV-centre systems.</p>
</div>

<div class="box box-real-world">
<p class="box-title"><strong>📡  Third-Generation Repeaters: All-Optical QEC</strong></p>
<p>Third-generation repeaters correct both loss and gate errors using quantum error-correcting codes that can handle photon loss as an erasure error. The key insight: in an optical channel, photon loss is a known erasure (we know when a photon failed to arrive, unlike a depolarising error which we don't know about). Erasure codes such as the 5-qubit bosonic code or the cat-qubit code can recover from known erasures with far fewer redundant qubits than general-purpose QEC codes.</p>
<p>Rate scaling: R_3rd approaches a constant rate independent of distance — the quantum channel has essentially been made lossless by the QEC encoding.</p>
<p>Hardware requirement: full fault-tolerant quantum computation at every network node, with gate fidelities exceeding 99.99% and thousands of physical qubits per logical qubit. Currently at the research frontier; projected for 2035–2040.</p>
</div>

## 7.4 Quantum Memory Platforms for Repeater Nodes

Quantum memory is the central hardware requirement for first- and second-generation quantum repeaters. A quantum memory must store a quantum state (typically a single-photon Fock state entangled with a photon at a distant node) for a duration much longer than the time needed to generate and herald all adjacent segments. The key figures of merit are: storage efficiency η\_M (fraction of photons successfully stored and retrieved), storage lifetime T\_2 (coherence time, limited by decoherence), multimode capacity M (number of time-bin or frequency modes that can be stored simultaneously), and spin-photon coupling efficiency (how efficiently the memory interfaces with flying photons).

<div class="box box-generic">

<img class="fig-img" src="content/images/image58.png" alt="">
</div>

**Figure 7.4: Quantum Memory Platforms — Physical Mechanisms**

Three leading quantum memory implementations. Left (DLCZ atomic ensemble): A cloud of ~10^6 laser-cooled rubidium atoms in a magneto-optical trap (MOT). A weak 'Write' laser pulse excites the ensemble, creating a small probability p\_s ≈ 0.01 of emitting a Stokes photon (780 nm for Rb) while storing a collective spin excitation across all atoms. A 'Read' pulse retrieves the stored excitation on demand, re-emitting the photon. The collective enhancement factor √N improves emission rate. Centre (NV centre in diamond): Nitrogen-vacancy point defect in diamond crystal. The electron spin (S=1) serves as the qubit, interacting with the 637 nm zero-phonon line (ZPL) photons for spin-photon entanglement. A nearby ^13C nuclear spin (I=1/2) provides long-term storage via hyperfine coupling. Right (AFC in rare-earth crystal): Europium- or erbium-doped Y\_2SiO\_5 crystal. The inhomogeneous absorption profile is sculpted by spectral hole burning into a periodic 'comb'. An incoming photon excites the comb and is re-emitted at time τ = 1/δ (the comb period). Transfer to nuclear spin extends coherence from microseconds to hours.

### 7.4.1 Atomic Ensemble Memories: The DLCZ Protocol

The DLCZ protocol (Duan, Lukin, Cirac, Zoller, Nature 2001) uses cold atomic ensembles as both entanglement sources and quantum memories. The Write-Stokes process: a weak laser pulse (the 'Write' beam) illuminates an ensemble of N atoms with probability p\_s ≪ 1 of exciting a single collective spin excitation. The emitted Stokes photon carries which-ensemble information; by routing Stokes photons from two ensembles L and R to a 50:50 beamsplitter with single-photon detectors, a single click heralds the creation of the entangled state:

<div class="box box-generic">
<p class="box-title">|ψ⟩_LR = (|1_L, 0_R⟩ + e^{iφ}|0_L, 1_R⟩) / √2</p>
<p><em>DLCZ heralded entangled state</em></p>
</div>

The Read-Anti-Stokes process retrieves the stored spin excitation: a strong 'Read' pulse converts the spin excitation back into a photon (the anti-Stokes photon), which is sent to a BSM station for entanglement swapping. The DLCZ memory thus acts simultaneously as an entanglement source (generating the heralded entanglement via the Write-Stokes click) and a quantum memory (storing the collective spin excitation until the Read pulse is applied).

Physical characteristics of DLCZ memories: coherence time T\_2 ~ 1 ms for magneto-optically trapped (MOT) cold atoms (limited by motional dephasing); up to T\_2 ~ 100 ms for spin-echo techniques; storage efficiency ~50% for simple retrieval, up to ~90% with cavity enhancement. Wavelengths: 780 nm (^{87}Rb), 852 nm (^{133}Cs). Multimode capacity M ~ 10–100 temporal modes per ensemble. Demonstrated entanglement lifetime: ~0.5 ms (Sangouard et al., 2007); demonstrated 50 km entanglement distribution using DLCZ memories (Bao et al., 2012).

<div class="box box-real-world">
<p class="box-title"><strong>🌐  DLCZ in Practice</strong></p>
<p>The practical advantage of DLCZ is its simplicity: it requires only cold atoms, lasers, and single-photon detectors — no single-photon sources, no cavity QED, no nanofabrication. This made it the first experimentally viable quantum repeater protocol.</p>
<p>Its key limitation: the collective spin excitation lifetime is limited by atomic motion, magnetic field fluctuations, and collisions. For the Delhi–Pune corridor (1400 km, ~10 ms classical latency for 100 km segments), DLCZ memories with T_2 ~ 1 ms are marginal. Rare-earth crystal memories (T_2 &gt; hours) are far better suited for such distances.</p>
</div>

### 7.4.2 NV Centre in Diamond: A Solid-State Spin-Photon Interface

The nitrogen-vacancy (NV) centre in diamond is one of the most extensively studied quantum memory platforms, owing to its unique combination of optical addressability at room temperature and relatively long spin coherence times. An NV centre consists of a substitutional nitrogen atom adjacent to a lattice vacancy in the diamond crystal. The ground state of the negatively charged NV^- centre is a spin-1 triplet (S=1), with the m\_s = 0 and m\_s = ±1 sublevels separated by 2.87 GHz by the zero-field splitting.

The spin-photon entanglement protocol: (1) A microwave π/2 pulse creates the electron spin superposition |−⟩ = (|0⟩ + |1⟩)/√2; (2) A laser pulse resonant with the |1⟩ → |e⟩ optical transition (637 nm, zero-phonon line) causes photon emission only when the spin is in |1⟩; (3) This creates the spin-photon entangled state (|0⟩|vac⟩ + |1⟩|1 photon⟩)/√2. A key challenge: only ~4% of NV fluorescence is in the zero-phonon line at room temperature; the rest goes into phonon sidebands at other wavelengths, limiting the spin-photon entanglement fidelity.

Room-temperature performance: T\_2 ~ 1 ms (electron spin, spin-echo); T\_2 ~ 10 ms (– 50 ms with dynamical decoupling). At cryogenic temperatures (T ~ 4 K), T\_2 can reach ~1 s and ZPL emission fraction increases dramatically. The nearby ^{13}C nuclear spin provides long-term ancilla storage with T\_2 > 1 minute at 4 K.

Key demonstrations: (1) Hensen et al. (2015, Delft): loophole-free Bell inequality violation between two NV centres 1.3 km apart — the first loophole-free test; (2) Pompili et al. (2021, QuTech): three-node quantum network with NV centres, demonstrating entanglement distribution and teleportation between Alice–Bob–Charlie connected by ~25–30 m fibres; (3) Bhaskar et al. (2020, Harvard): memory-enhanced quantum communication with SiV centres (silicon-vacancy, a diamond colour centre better suited for telecom wavelengths).

### 7.4.3 Rare-Earth Doped Crystals: The AFC Protocol

Rare-earth ion (REI) doped crystals are currently the most attractive quantum memory platform for long-distance quantum networks, owing to their extraordinary coherence times, large multimode capacity, and — crucially for Indian and European networks — compatibility with telecommunications wavelengths. Erbium (Er^{3+}) doped in Y\_2SiO\_5 (Er:YSO) has a ^4I\_{13/2} → ^4I\_{15/2} optical transition at 1532 nm, precisely in the telecommunications C-band, enabling direct coupling to existing fibre infrastructure without wavelength conversion.

The Atomic Frequency Comb (AFC) protocol stores photons using a spectrally tailored absorption profile in the rare-earth crystal. The preparation phase uses spectral hole burning: a series of narrow laser pulses depletes the inhomogeneous absorption profile everywhere except at periodic narrow peaks (‘comb teeth’) spaced by δ in frequency. An absorbed photon at any comb tooth frequency re-emits after a predetermined delay τ = 1/δ, because all comb teeth re-phase together at this time — the AFC is a quantum delay line.

<div class="box box-generic">
<p class="box-title"><em>AFC storage time</em></p>

</div>

On-demand retrieval: the basic AFC protocol is a fixed-delay memory, not on-demand. To achieve on-demand retrieval, the atomic coherence is transferred to a nuclear spin mode (I = 5/2 for Eu^{3+}) using a pair of RF pulses (the spin-wave storage technique). The nuclear spin coherence time can exceed T\_2 > 6 hours at 4 K (demonstrated in Eu:YSO by Ma et al., ECNU, 2021 — the longest quantum memory ever demonstrated). Retrieval on demand is then triggered by a second RF pulse that transfers the spin-wave back to the optical mode.

Performance: multimode capacity M > 1000 temporal modes (all stored simultaneously in different time bins of the AFC); storage efficiency up to 56% (Lago-Rivera et al., ICFO, 2021, in Nd:YVO); telecom-compatible wavelength (Er:YSO, 1532 nm). These properties make AFC memories the leading candidate for intermediate-scale quantum network nodes operating over distances of 100–1000 km.

| Platform | T_2 (spin) | Efficiency | Modes M | Wavelength | TRL |
|---|---|---|---|---|---|
| Atomic ensemble (DLCZ) | ~1 ms (cold) | ~50% | ~10-100 | 780 nm (Rb) | 4-5 |
| NV centre (diamond) | ~1 ms (RT), 1 s (4K) | ~10% | 1 | 637 nm (ZPL) | 5-6 |
| AFC rare-earth (Er:YSO) | >6 hours (4K, Eu) | >50% | >1000 | 1532 nm (telecom) | 4 |
| Trapped ion (Ca+, Yb+) | min-hours | >99% | N (chain) | 729 nm, 370 nm | 4-5 |

*Table 7.3: Quantum memory platform comparison for repeater nodes (2024 state of the art). TRL = Technology Readiness Level.*

**RECAP**

*Chapter 7: Quantum Network Architecture and Repeater Technology — Short Answer Questions & Model Answers*

## Short Answer Questions — Chapter 7

*Instructions: Answer each question in 3–6 lines.*

**Q1.**  What is the main objective of a quantum network?

*[§7.1 — Introduction: The Quantum Communication Problem]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q2.**  State the no-cloning theorem and explain its importance in quantum communication.

*[§7.1 — Introduction: The Quantum Communication Problem]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q3.**  Write the expression for photon survival probability in an optical fibre.

*[§7.2 — Quantum Network Architecture]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q4.**  Why is direct long-distance quantum communication through optical fibre impractical?

*[§7.2 — Quantum Network Architecture]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q5.**  What is the role of a quantum repeater in a quantum network?

*[§7.3 — Quantum Repeaters]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q6.**  Define entanglement swapping.

*[§7.3 — Quantum Repeaters]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q7.**  What are the essential functions of an end node in a quantum network?

*[§7.2 — Quantum Network Architecture]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q8.**  Why is quantum memory necessary in repeater-based communication?

*[§7.4 — Quantum Memory Platforms]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q9.**  Compare optical fibre channels and free-space channels for quantum communication.

*[§7.2 — Quantum Network Architecture]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q10.**  What is heralding in a quantum repeater protocol?

*[§7.3 — Quantum Repeaters]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q11.**  What is a Bell State Measurement (BSM)?

*[§7.3 — Quantum Repeaters]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q12.**  Differentiate between first-generation and second-generation quantum repeaters.

*[§7.3 — Quantum Repeaters]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q13.**  What is the key idea behind third-generation quantum repeaters?

*[§7.3 — Quantum Repeaters]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q14.**  What are the important performance parameters of a quantum memory?

*[§7.4 — Quantum Memory Platforms]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q15.**  What is the DLCZ protocol and why is it important?

*[§7.4 — Quantum Memory Platforms]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Model Answers — Chapter 7

**Answer 1:**

The main objective of a quantum network is to distribute entanglement between distant quantum devices such as quantum computers, sensors, and memory nodes for secure communication, distributed computation, and quantum sensing.

**Answer 2:**

The no-cloning theorem states that an unknown quantum state cannot be copied perfectly. It is important because it prevents amplification of quantum signals in the same way as classical communication systems.

**Answer 3:**

The photon survival probability in an optical fibre is: η(L) = 10^(−αL/10) = e^(−L/L\_att), where α is the attenuation coefficient and L\_att is the attenuation length.

**Answer 4:**

Direct long-distance quantum communication is impractical because photon loss in optical fibre increases exponentially with distance. At very long distances, the probability of photon survival becomes extremely small.

**Answer 5:**

A quantum repeater divides a long communication channel into smaller segments, distributes entanglement in each segment, and combines them using entanglement swapping to enable long-distance quantum communication.

**Answer 6:**

Entanglement swapping is a process in which two independent entangled pairs are connected so that two particles that never interacted become entangled.

**Answer 7:**

End nodes generate photonic qubits, store qubits in memory, perform local quantum gates, measure qubits in different bases, and exchange classical information.

**Answer 8:**

Quantum memory is necessary because entanglement generation is probabilistic. Successfully generated entangled states must be stored while waiting for neighbouring segments to succeed.

**Answer 9:**

Optical fibre channels provide stable and low-cost communication but suffer exponential loss with distance. Free-space channels avoid fibre loss and are useful for satellites, but they are affected by atmospheric turbulence and weather.

**Answer 10:**

Heralding is the process of sending a classical signal to confirm successful photon arrival or successful entanglement generation in a repeater segment.

**Answer 11:**

A Bell State Measurement is a joint quantum measurement that projects two qubits onto one of the four Bell states. It is the key operation in entanglement swapping.

**Answer 12:**

First-generation repeaters mainly correct photon loss using quantum memory and heralding, while second-generation repeaters additionally use quantum error correction to reduce gate and memory errors.

**Answer 13:**

Third-generation repeaters use full quantum error correction to correct both photon loss and operational errors, aiming for communication rates nearly independent of distance.

**Answer 14:**

Important performance parameters of quantum memory include storage efficiency, coherence time, multimode capacity, and spin-photon coupling efficiency.

**Answer 15:**

The DLCZ protocol uses atomic ensembles to generate and store entanglement through collective spin excitations. It was one of the first experimentally feasible quantum repeater protocols.

**Solved Examples — Chapter 7**

**Example 7.1  Fibre Loss Calculation**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>An optical fibre has attenuation α = 0.2 dB/km. (a) Calculate the photon survival probability at L = 50, 100, 300, 1000 km. (b) Find the attenuation length L_att. (c) At a source rate R = 10 GHz, how long (on average) to transmit one photon at 1000 km?</p>
</div>

**Solution:**

(c) Expected time = 1/(η × R) = 1/(10^{−20} × 10^{10}) = 10^{10} s ≈ 317 years

Completely impractical — this is why quantum repeaters are essential.

**Example 7.2  Repeater Segment Efficiency**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A 1000 km quantum repeater chain has N = 50 segments. Fibre attenuation α = 0.2 dB/km. (a) Calculate the per-segment efficiency η_seg. (b) How does this compare to direct transmission? (c) If each segment generates at R_rep = 100 MHz, what is the raw entanglement rate per segment?</p>
</div>

**Solution:**

(a) Segment length = 1000/50 = 20 km

(b) η\_seg/η\_direct = 0.398/10^{−20} = 4 × 10^{19} times better!

(c) Entanglement rate per segment: R = η\_seg × R\_rep = 0.398 × 10^8 ≈ 4 × 10^7 pairs/s = 40 MHz

The bottleneck in a real chain is quantum memory lifetime and swapping success, not the per-segment rate.

**Example 7.3  Entanglement Swapping State Calculation**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>Alice (A), repeater node B (with qubits B_L and B_R), and Bob (C) start with state |Φ^+⟩_{AB_L} ⊗ |Φ^+⟩_{B_RC}. B measures in the Bell basis and obtains result |Ψ^+⟩. What correction does Bob apply, and what is the final A–C state before correction?</p>
</div>

**Solution:**

Before correction: expanding the initial 4-qubit state in the Bell basis (B\_L, B\_R) gives:

|initial⟩ = (1/2)[ |Φ^+⟩\_{B\_LB\_R}|Φ^+⟩\_{AC} + |Φ^-⟩\_{B\_LB\_R}|Φ^-⟩\_{AC} + |Ψ^+⟩\_{B\_LB\_R}|Ψ^+⟩\_{AC} + |Ψ^-⟩\_{B\_LB\_R}|Ψ^-⟩\_{AC} ]

B measures |Ψ^+⟩\_{B\_LB\_R}. The AC state collapses to |Ψ^+⟩\_{AC} = (|01⟩ + |10⟩)/√2.

To recover the standard Bell state |Φ^+⟩\_{AC} = (|00⟩ + |11⟩)/√2, Bob applies the X gate:

X ⊗ I |Ψ^+⟩\_{AC} = (|11⟩ + |00⟩)/√2 = |Φ^+⟩\_{AC} ✓

Result: Bob applies X gate. The final A–C state is |Φ^+⟩\_{AC}.

**Example 7.4  DLCZ Entanglement Generation Rate**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A DLCZ repeater has: Write pulse probability p_s = 0.01, laser repetition rate R_rep = 10 MHz, link transmission η_link = 0.4 (for a 20 km segment at 0.2 dB/km including coupling losses), detector efficiency η_det = 0.90. Calculate the heralded entanglement generation rate.</p>
</div>

**Solution:**

Probability of a 'click' per arm: p\_click = p\_s × η\_link × η\_det = 0.01 × 0.4 × 0.9 = 0.0036

For the HOM beamsplitter, probability of exactly one click (from either ensemble L or R, but not both):

p\_herald = 2 × p\_click × (1 - p\_click) ≈ 2 × 0.0036 × 0.9964 = 0.00717

(Multi-photon events from both arms are discarded as errors.)

Entanglement rate = R\_rep × p\_herald = 10 × 10^6 × 0.00717 = 71,700 pairs/s ≈ 72 kHz

With M = 100 temporal modes (AFC-based multimode storage): R\_multi = 100 × 72 kHz = 7.2 MHz

Multimode storage is thus a key multiplier for entanglement generation rate.

**Example 7.5  Memory Lifetime Requirement**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A 5-segment quantum repeater chain spans 500 km (100 km per segment). The classical communication speed in fibre is c/n = 2×10^8 m/s. (a) What is the one-way light travel time per segment? (b) In the worst case, how long must the memory at a middle node store a qubit while waiting for the most distant segment to also succeed? (c) Which memory platform from Table 7.3 satisfies this requirement?</p>
</div>

**Solution:**

(a) t\_segment = 100 km / (2×10^8 m/s) = 100×10^3 / (2×10^8) = 0.5 ms

(b) In a 5-segment chain, in the worst case the first segment succeeds, and the remaining 4 must succeed while the first waits. Maximum wait time = 4 × (time per attempt + classical latency). Classical round-trip for 400 km = 4 × 0.5 ms × 2 = 4 ms. In practice, with probabilistic generation (rate R\_seg), the average wait is ~4/(R\_seg) per step. For conservative design, require T\_2 ≥ 100 × 1/R\_seg. If R\_seg = 100 kHz, T\_2 ≥ 1 ms is marginal.

(c) From Table 7.3: Atomic ensemble (T\_2 ~ 1 ms) – marginal; NV centre at RT (T\_2 ~ 1 ms) – marginal;

AFC rare-earth at 4K (T\_2 > 6 hours) – excellent; Trapped ion (T\_2 > minutes) – excellent.

Recommendation: AFC rare-earth (Er:YSO) for telecom wavelength and T\_2 >> requirement.

**Example 7.6  NV Centre Spin-Photon Entanglement Fidelity**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>An NV centre at room temperature is used for spin-photon entanglement. The zero-phonon line (ZPL) emission fraction is f_ZPL = 0.04, photon collection efficiency is η_coll = 0.12 (using a solid immersion lens), and the BSM uses linear optics with efficiency η_BSM = 0.5. (a) What is the total photon detection probability per attempt? (b) If the desired entanglement generation rate is 1 kHz, what laser repetition rate is needed? (c) What would change at cryogenic temperature (f_ZPL = 0.3)?</p>
</div>

**Solution:**

(a) Total detection probability: η\_det = f\_ZPL × η\_coll × η\_fibre(1km) × η\_detector

= 0.04 × 0.12 × 10^{-0.02} × 0.85 = 0.04 × 0.12 × 0.955 × 0.85 = 0.00195 = 0.195%

(b) For heralded entanglement via BSM: rate = R\_rep × η\_det^2 × η\_BSM

1000 = R\_rep × (0.00195)^2 × 0.5 = R\_rep × 1.9×10^{-6}

R\_rep = 1000 / (1.9×10^{-6}) = 5.3 × 10^8 Hz = 530 MHz

(c) At 4 K, f\_ZPL = 0.3: η\_det = 0.3/0.04 × 0.00195 = 7.5 × 0.00195 = 0.0146 = 1.46%

Needed R\_rep = 1000 / (0.0146^2 × 0.5) = 1000 / 1.07×10^{-4} = 9.4 MHz

Cryogenic NV reduces the needed laser rate by 56× — a major practical improvement.

**Example 7.7  AFC Memory Time Calculation**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>An AFC memory in Er:YSO is prepared with comb tooth spacing δ = 50 MHz. (a) Calculate the storage time τ_AFC. (b) If the free spectral range of the comb is 10 GHz, how many temporal modes can be stored simultaneously? (c) If nuclear spin transfer is used with a dephasing time T_2^{spin} = 6 hours, what is the maximum segment length that this memory can support in a quantum repeater chain?</p>
</div>

**Solution:**

(a) τ\_AFC = 1/δ = 1/(50×10^6) = 20 ns

(This is the basic AFC storage time; nuclear spin transfer extends this to hours.)

(b) Number of temporal modes: M = T\_window / τ\_AFC where T\_window = 1/(mode spacing in time domain)

A single AFC crystal can simultaneously store 200 time-bin modes.

(c) Maximum segment length is set by T\_2^{spin} ≥ latency for the full chain:

For a 2-segment chain (just one repeater), latency ≈ 2 × (L/c) = 2 × L/(2×10^8 m/s)

Max L = 21,600 × (2×10^8) / 2 = 2.16×10^{12} m = 2.16×10^9 km

This far exceeds any terrestrial distance — the memory lifetime is not the bottleneck for current networks;

the bottleneck is entanglement generation rate and BSM efficiency.

**Example 7.8  Three-Generation Repeater Rate Comparison**

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A quantum repeater chain spans L = 2000 km with N = 20 segments (each 100 km). Per-segment efficiency η_seg = 10^{−2} = 1%. Compare the end-to-end entanglement generation rates for first-, second-, and third-generation repeaters.</p>
</div>

**Solution:**

Direct transmission:

First-generation (N segments, heralded generation, O(√η) per pair of nodes):

R\_1st ∝ η\_seg^{1/2} / N^2 ≈ 0.1 / 400 = 2.5×10^{-4} (relative units)

This corresponds to ~1–10 pairs/second for typical system parameters.

Second-generation (with QEC in nodes, O(η^{1/4}) scaling):

~64× faster than first-generation; useful rates to 5000+ km.

Third-generation (all-optical QEC, constant rate):

R\_3rd ∝ constant (independent of η\_seg) ≈ 1 (relative units)

~4000× faster than second-generation; essentially distance-independent.

Conclusion: Generation choice has a dominant impact on practical utility. Third-generation is needed for the quantum internet at global scale.

**Multiple Choice Questions — Chapter 7**

*Note: Answers are collected at the end of this chapter.*

<div class="box box-generic">
<p class="box-title"><strong>1. The no-cloning theorem is fundamental to quantum repeater design because it implies that:</strong></p>
<p>(A) Quantum information can be teleported but not copied, allowing repeaters to forward information</p>
<p>(B) Quantum signals cannot be amplified without measurement, so classical repeater amplifiers cannot be used for quantum channels</p>
<p>(C) Entanglement is a conserved quantity that flows from one node to the next without being duplicated</p>
<p>(D) All quantum channels must use photon pairs rather than single photons to carry information</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>2. In a quantum network with N=10 segments, each of length 100 km with α=0.2 dB/km, the per-segment transmission η_seg is approximately:</strong></p>
<p>(A) 10^{-20}</p>
<p>(C) 10^{-10}</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>3. The classical control layer in a quantum network is NOT responsible for:</strong></p>
<p>(A) Carrying Bell state measurement outcomes from repeater nodes to end users</p>
<p>(B) Cloning quantum states to prevent photon loss on the quantum channel</p>
<p>(C) Distributing timing synchronisation between network nodes</p>
<p>(D) Authenticating classical messages to prevent man-in-the-middle attacks</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>4. In the entanglement swapping protocol, after B performs the Bell state measurement on (B_L, B_R) and communicates the result, Alice and Charlie share a Bell state. The communication from B to Charlie is necessary because:</strong></p>
<p>(A) Without it, Charlie would not know which qubit to measure to test the Bell inequality</p>
<p>(B) The measurement at B collapses A and C into one of four Bell states; the 2-bit result tells Charlie which Pauli correction to apply to recover |Φ^+⟩_{AC}</p>
<p>(C) It allows the entanglement to travel faster than light from A to C through classical communication</p>
<p>(D) The Bell state measurement at B is projective; Charlie needs the result to confirm that A is measured</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>5. The heralding principle in quantum repeaters means that:</strong></p>
<p>(A) Photons are pre-measured before transmission to determine whether they will be transmitted successfully</p>
<p>(B) A classical 'click' signal announces successful photon arrival, allowing entanglement swapping to proceed only when both input photons are present</p>
<p>(C) The quantum memory automatically heals errors introduced during photon transmission</p>
<p>(D) All network nodes send simultaneous signals to announce their readiness for entanglement swapping</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>6. The DLCZ protocol uses atomic ensembles as quantum memories because:</strong></p>
<p>(A) Individual atoms can store many photons simultaneously due to their high density</p>
<p>(B) The collective spin excitation across N atoms enhances emission rate by √N, enabling heralded entanglement without single-photon sources</p>
<p>(C) Atomic ensembles have T_2 &gt; 1 hour, which is required for long-distance quantum networks</p>
<p>(D) The Write-Stokes emission from a single atom is sufficient to create a heralded Bell pair</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>7. The Atomic Frequency Comb (AFC) protocol achieves long storage times by:</strong></p>
<p>(A) Cooling the crystal to millikelvin temperatures to reduce phonon noise below the quantum limit</p>
<p>(B) Transferring optical atomic coherence to a long-lived nuclear spin mode using RF pulses, extending storage from nanoseconds to hours</p>
<p>(C) Using a feedback loop to continuously correct the atomic state during storage</p>
<p>(D) Storing photons as standing waves in an optical cavity formed by the crystal facets</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>8. The rate scaling O(η^{1/2}) for first-generation quantum repeaters (compared to O(η) for direct transmission) arises because:</strong></p>
<p>(A) Quantum error correction eliminates half of all photon losses</p>
<p>(B) With two heralded segments, the waiting time scales as the product of two independent η factors, and the dominant bottleneck is the minimum of two exponential random variables, scaling as √η</p>
<p>(C) The BSM at intermediate nodes boosts the signal by a factor of √η</p>
<p>(D) The no-cloning theorem allows up to √η copies of each photon to be distributed simultaneously</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>9. A quantum memory with T_2 = 1 ms is evaluated for use in a 2-node repeater spanning 400 km with c/n = 2×10^8 m/s. The classical round-trip latency for this link is:</strong></p>
<p>(A) 0.5 ms</p>
<p>(B) 2 ms</p>
<p>(C) 4 ms</p>
<p>(D) 1 ms</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>10. NV centres in diamond are preferred over atomic ensembles for some quantum network demonstrations because:</strong></p>
<p>(A) They operate at room temperature and provide a deterministic single-photon emitter with a well-defined optical interface</p>
<p>(B) They have longer coherence times than any other solid-state system at room temperature (T_2 &gt; 1 hour)</p>
<p>(C) Their emission wavelength (637 nm) is ideally matched to standard telecommunications fibre at 1550 nm</p>
<p>(D) Each NV centre can store multiple temporal modes simultaneously, giving high multimode capacity</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>11. Third-generation quantum repeaters achieve distance-independent communication rates because:</strong></p>
<p>(A) They use quantum teleportation to forward quantum states without any photon loss</p>
<p>(B) They correct photon loss using quantum error-correcting codes that treat loss as a known erasure, effectively converting a lossy channel into a lossless one</p>
<p>(C) They use satellite links that bypass fibre loss scaling entirely</p>
<p>(D) They require no quantum memory, so memory decoherence does not limit performance</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>12. For a 5-segment quantum repeater chain at 500 km, the minimum quantum memory lifetime required for robust operation is approximately:</strong></p>
<p>(A) 100 ns (one photon transit time)</p>
<p>(B) &gt; 10 ms (several classical round-trip latencies)</p>
<p>(C) &gt; 1 year (longer than the mission duration)</p>
<p>(D) 1 ns (faster than decoherence)</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>13. The QuTech three-node quantum network (Delft, 2021) demonstrated all of the following EXCEPT:</strong></p>
<p>(A) Entanglement distribution between two non-adjacent nodes</p>
<p>(B) Quantum teleportation across the three-node network</p>
<p>(C) Device-independent QKD between Alice and Bob with loophole-free Bell inequality violation at 1.3 km</p>
<p>(D) Simultaneous shared entanglement between all three nodes</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>14. Rare-earth doped crystals (Er:YSO) are particularly suitable for the proposed Delhi–Pune quantum link because:</strong></p>
<p>(A) They operate at room temperature and can be easily deployed in standard telecom equipment</p>
<p>(B) Their telecom-wavelength (1532 nm) emission is compatible with existing fibre infrastructure, and their T_2 &gt; 6 hours greatly exceeds the classical latency of the link</p>
<p>(C) They require only low-power lasers and can be powered by solar panels at relay stations</p>
<p>(D) Their large multimode capacity allows more QKD key bits per second than any other platform</p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>15. The write-stokes process in the DLCZ protocol is deliberately kept at very low excitation probability (p_s ≪ 1) because:</strong></p>
<p>(A) Low probability reduces the heating of the atomic ensemble from absorbing photons</p>
<p>(B) High p_s would create multi-photon Stokes events, which introduce errors since the heralding click cannot distinguish 1-photon from 2-photon events without photon-number-resolving detectors</p>
<p>(C) Low p_s increases the lifetime of the collective spin excitation stored in the ensemble</p>
<p>(D) The Stokes photon must be entangled with a specific single atom, which requires low probability to avoid exciting multiple atoms</p>
</div>

**MCQ Answers — Chapter 7**

| Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 | Q8 | Q9 | Q10 |
|---|---|---|---|---|---|---|---|---|---|
| B | B | B | B | B | B | B | B | C | A |
| Q11 | Q12 | Q13 | Q14 | Q15 |  |  |  |  |  |
| B | B | C | B | B |

**Unsolved Problems — Chapter 7**

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.1</strong></p>
<p>Ultra-low-loss fibre has α = 0.15 dB/km. (a) Calculate η at L = 100, 500, 1000 km. (b) With a 20-node repeater chain, find per-segment efficiency at 1000 km. (c) Estimate the end-to-end rate advantage over direct transmission at 1000 km.</p>
<p><em>[Ans: (a) η(100)=10^{-1.5}=3.16×10^{-2}; η(500)=10^{-7.5}=3.16×10^{-8}; η(1000)=10^{-15}; (b) L_seg=50 km, η_seg=10^{-0.75}=0.178=17.8%; (c) R_1st/R_direct = η_seg^{-1/2}/η(1000)^{-1} ≈ 10^{7.5}; gain~3×10^7]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.2</strong></p>
<p>In an entanglement swapping experiment, two Werner state Bell pairs are generated with F_AB = F_BC = 0.94. BSM efficiency is η_BSM = 0.85 (probability that both photons are detected). (a) What fraction of swapping attempts succeed? (b) What is the expected end-to-end fidelity F_AC for a Werner state input?</p>
<p><em>[Ans: (a) P_success = η_BSM^2 = 0.85^2 = 0.7225 = 72.25%; (b) F_AC ≈ F_AB×F_BC (simplified Werner model) = 0.94×0.94=0.884; more precisely with Werner state F_AC = F_AB×F_BC + (1-F_AB)(1-F_BC)/3 ≈ 0.884+0.001≈0.885]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.3</strong></p>
<p>A DLCZ repeater has p_s = 0.005, R_rep = 50 MHz, η_link = 0.5, η_det = 0.90. (a) Calculate the single-click probability per arm. (b) Calculate the heralded entanglement rate. (c) If M = 200 temporal modes are used (multimode AFC), what is the multiplexed rate?</p>
<p><em>[Ans: (a) p_click = 0.005×0.5×0.9 = 0.00225; (b) p_herald = 2×0.00225×(1-0.00225) = 0.00449; R = 50×10^6 × 0.00449 = 225 kHz; (c) R_multi = 200 × 225 kHz = 45 MHz]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.4</strong></p>
<p>An AFC memory is prepared with tooth spacing δ = 100 MHz. (a) What is the storage time τ_AFC? (b) With a 5 GHz absorption bandwidth, how many temporal modes can be stored? (c) After RF-transfer to nuclear spin with T_2^{nuc} = 1 hour, what is the maximum repeater chain latency this memory can support?</p>
<p><em>[Ans: (a) τ = 1/10^8 = 10 ns; (b) M = 5 GHz/100 MHz = 50 modes; (c) T_2^{nuc} = 3600 s; max latency = 3600 s ≫ any terrestrial chain; AFC not limited by T_2 for any realistic network]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.5</strong></p>
<p>Compare three quantum repeater architectures for a 2000 km link. Take N = 10 segments, per-segment η_seg = 10^{-2}. (a) Direct transmission rate: R ∝ η^{10}? No, η(2000). (b) First-generation rate ∝ η_seg^{1/2}. (c) Second-generation rate ∝ η_seg^{1/4}. Express as ratios with respect to direct.</p>
<p><em>[Ans: (a) R_direct ∝ η(2000km) = 10^{-40}; (b) R_1st ∝ η_seg^{1/2} = 10^{-1} = 0.1; ratio = 10^{39}; (c) R_2nd ∝ η_seg^{1/4} = 10^{-0.5} ≈ 0.316; ratio = 0.316/0.1 = 3.16× over first-generation]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.6</strong></p>
<p>An NV centre at 4K has f_ZPL = 0.35, collection efficiency 0.30 with a photonic crystal cavity, and detector efficiency 0.95. (a) Total photon detection probability per attempt. (b) For a 1 kHz entanglement rate target (single-photon BSM, η_BSM = 0.5), required R_rep. (c) How does this compare to the room-temperature result from Solved Example 7.6?</p>
<p><em>[Ans: (a) η_det = 0.35×0.30×0.95 = 0.0998 ≈ 10%; (b) R = 1000/(0.1^2×0.5) = 200 kHz; (c) vs. 530 MHz at RT: 2650× improvement from cryogenic operation plus cavity]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.7</strong></p>
<p>A heralded entanglement source generates 1 entangled pair per 10 μs. The quantum memory coherence time is T_2 = 1 ms. A 3-segment repeater requires all 3 segments to succeed before swapping. (a) Expected number of attempts before success at each segment. (b) Expected maximum wait time at a middle node for both neighbors to succeed. (c) Is T_2 = 1 ms sufficient?</p>
<p><em>[Ans: (a) Mean attempts = 1/p_seg; if p_seg = 0.01, mean = 100 attempts = 100×10μs = 1 ms; (b) Max wait = 2 segment times = 2 ms (for both neighbours); (c) T_2 = 1 ms &lt; 2 ms wait time, marginally insufficient; need T_2 ≥5 ms for robust operation]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.8</strong></p>
<p>The DLCZ state |ψ⟩_LR = (|1_L,0_R⟩ + e^{iφ}|0_L,1_R⟩)/√2. (a) Calculate the fidelity F = ⟨Ψ^+|ρ_{LR}|Ψ^+⟩ if φ = 0. (b) If the phase φ drifts by π/4 due to fibre phase noise before the Read pulse, what is the new fidelity with respect to |Ψ^+⟩? (c) Why is phase stability critical for DLCZ entanglement and how is it maintained?</p>
<p><em>[Ans: (a) |ψ⟩ = (|10⟩+|01⟩)/√2 = |Ψ^+⟩; F = 1.00; (b) |ψ'⟩ = (|10⟩+e^{iπ/4}|01⟩)/√2; F = |⟨Ψ^+|ψ'⟩|^2 = |(1+e^{iπ/4})/2|^2 = |cos(π/8)|^2 = 0.854; (c) Phase stability critical because entangled state depends on relative phase between two arms; maintained by active phase-locking of fibre arms, common-mode rejection using shared reference laser, or entangling in phase-insensitive frequency modes]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.9</strong></p>
<p>The QuTech 3-node network (Pompili et al., Science 2021) used NV centres connected by ~25 m fibres. If the links are extended to 10 km each, estimate: (a) classical latency for heralding; (b) required T_2 for end-to-end entanglement generation; (c) success probability per attempt if η_link(10km)=0.4, η_BSM=0.5.</p>
<p><em>[Ans: (a) t = 2 × 10 km / (2×10^8) = 100 μs; (b) T_2 ≥ 3 × t_latency = 300 μs minimum, recommend &gt;5 ms; (c) P_success = η_link^2 × η_BSM = 0.4^2 × 0.5 = 0.08 = 8% per attempt]</em></p>
</div>

<div class="box box-generic">
<p class="box-title"><strong>Problem 7.10</strong></p>
<p>For the full first-generation quantum repeater rate formula R_e2e = R_rep × η_seg^N (worst case, N segments must all succeed simultaneously), calculate for N=5, η_seg=0.10, R_rep=10 MHz: (a) R_e2e. (b) How does R_e2e scale if segment length is halved (N doubles) at constant total distance? (c) Is there an optimal N?</p>
<p><em>[Ans: (a) R_e2e = 10^7 × 0.1^5 = 10^7 × 10^{-5} = 100 pairs/s; (b) New N=10, η_seg(new)=0.1^{0.5}=0.316; R_e2e = 10^7×0.316^{10} = 10^7×8.8×10^{-6} = 88 pairs/s; rate slightly drops; (c) There is an optimal N balancing per-segment efficiency vs. number of segments; for a simple model, R_e2e ∝ N/(-logη(L/N)) is maximised at moderate N]</em></p>
</div>

**Theory Questions — Chapter 7**

- Derive the entanglement swapping protocol in complete mathematical detail. Start from the four-qubit state |Φ^+⟩\_{AB} ⊗ |Φ^+⟩\_{BC}. Re-express in the Bell basis for qubits B\_L and B\_R. Identify what state A and C are projected into for each BSM outcome. Derive the correction operations and prove that after correction, |Φ^+⟩\_{AC} is always obtained.

- Explain why quantum states cannot be amplified classically, using the no-cloning theorem. Derive the impossibility of a universal quantum amplifier: starting with the assumption that U|x⟩|0⟩ = |x⟩|x⟩ for all |x⟩, show this leads to a contradiction for a superposition state |x⟩ = (α|0⟩+β|1⟩).

- Compare the three quantum memory platforms (DLCZ atomic ensembles, NV centres in diamond, and rare-earth AFC crystals) in terms of: (a) physical spin-photon interaction mechanism; (b) coherence time T\_2 at room temperature and at 4 K; (c) storage efficiency; (d) multimode capacity; (e) operating wavelength and compatibility with telecom fibres. State which platform is most suitable for a 1000 km repeater chain and justify.

- The heralding protocol in a quantum network requires that heralding signals travel at the speed of light in fibre (c/n = 2×10^8 m/s). For a 1000 km chain with 10 segments, derive the optimal polling sequence for heralding (which segment should report first, and how does this affect memory lifetime requirements?). Calculate the minimum T\_2 needed for a memory at the central node.

- Describe the DLCZ protocol in full detail. Explain the Write pulse, Stokes photon emission, HOM beamsplitter click heralding, the quantum state generated, and the Read pulse retrieval. Derive the heralded entanglement state |ψ⟩\_LR = (|1\_L,0\_R⟩ + e^{iφ}|0\_L,1\_R⟩)/√2 from first principles of the beamsplitter transformation.

- Explain what distinguishes first-, second-, and third-generation quantum repeaters. For each generation, give: (a) the types of errors it corrects (loss, gate errors, or both); (b) the specific physical operations performed at each node; (c) the scaling of the end-to-end entanglement rate with channel transmission η; (d) the minimum gate fidelity and coherence time required.

- The AFC (Atomic Frequency Comb) protocol was described in this chapter. Derive from the theory of photon echo in a two-level atomic system why a periodic absorption profile (a comb) leads to re-emission at time τ = 1/δ. How does the memory efficiency depend on the optical depth of the comb?

- A quantum network node receives photons from two adjacent segments. The arrival times are Poisson-distributed with rates r\_L (left) and r\_R (right). Derive the expected waiting time until both a left and a right photon arrive. How does this relate to the quantum memory lifetime requirement? What is the optimal segment balance (r\_L = r\_R vs. r\_L ≠ r\_R)?

- The NV centre spin-photon entanglement protocol relies on the selective optical excitation of only the |1⟩ spin state. Explain the physical mechanism by which the optical selection rule creates spin-photon entanglement. Why is the zero-phonon line (ZPL) important, and what is the consequence of phonon-sideband emission for fidelity? How does a photonic crystal cavity improve performance?

- Evaluate the claim that the QuTech three-node quantum network (2021) represents 'Stage 2' of the quantum internet (Wehner-Elkouss-Hanson framework). What specific capabilities were demonstrated? What additional capabilities are needed to call a system 'Stage 2'? What hardware upgrades are necessary to scale from 3 nodes to 30 nodes while maintaining entanglement fidelity > 0.9?

**Assignments and Project Suggestions — Chapter 7**

### Assignment 7.1 Entanglement Swapping Simulation (Marks: 10)

Implement a quantum repeater simulation in Python using the QuTiP library: (a) Simulate a 3-node system where Alice–B (repeater)–Charlie. Generate Bell pairs with noise (Werner states, F = 0.85). (b) Implement the BSM using the CNOT+H quantum circuit and simulate measurement noise (gate error ε = 0.02). (c) Calculate the end-to-end fidelity F\_AC and compare with the theoretical prediction F\_AC = F\_AB × F\_BC × (1 − ε). (d) Plot F\_AC as a function of F\_AB for F\_BC = 0.90 and compare with and without gate noise. Submit code, plots, and a 1500-word analysis.

### Assignment 7.2 Quantum Memory Platform Technical Assessment (Marks: 10)

Write a 2000-word technical brief evaluating the three quantum memory platforms (DLCZ, NV-centre, AFC) for use in the proposed India NQM Delhi–Agra pilot QKD link (200 km). Your assessment must cover: (a) T\_2 requirement for 200 km classical latency; (b) telecom wavelength compatibility (fibre-coupled operation at 1550 nm); (c) Technology Readiness Level for near-term deployment (2026–2028); (d) expected storage efficiency and multimode capacity; (e) a recommendation with justification. Include a comparison table and at least 5 literature references.

### Project Suggestion 7.A NetSquid Quantum Network Simulation

Build a complete quantum network simulation using the NetSquid (Network Simulator for Quantum Information using Discrete events) Python package: (a) Implement a 5-node network with realistic NV-centre memories (T\_2 = 5 ms, efficiency = 0.12, ZPL fraction = 0.04). (b) Simulate DLCZ-type heralded entanglement generation with fibre links of 20 km each. (c) Implement entanglement swapping at each intermediate node and measure end-to-end fidelity as a function of link length. (d) Compare simulation results with analytical predictions from the rate scaling formulas in Section 7.3.3. Write a 3000-word simulation report including all code, figures, and analysis of discrepancies.

### Project Suggestion 7.B Quantum Repeater Hardware Roadmap

Prepare a detailed technology roadmap for quantum repeater hardware development (2024–2035): (a) Survey current experimental state of the art for each memory platform, citing at least 15 peer-reviewed papers published since 2020. (b) Identify the three most critical technical barriers to first-generation repeater deployment (e.g., memory efficiency, BSM efficiency, memory lifetime). (c) Estimate, based on current improvement trends, when each barrier will be overcome. (d) Propose a 5-year R&D programme for India's NQM quantum communications hub to address these barriers. Deliver a 10-slide presentation and a 2500-word supporting report.

### Project Suggestion 7.C Entanglement Swapping Experimental Design

Design a tabletop entanglement swapping experiment using spontaneous parametric down-conversion (SPDC) photon pair sources: (a) Specify the optical layout: two BBO crystals, fibres, waveplates, and beam splitters. (b) Calculate the expected coincidence detection rates for typical parameters (pump power 100 mW, crystal length 3 mm, detector efficiency 0.80). (c) Estimate the entanglement fidelity including multi-pair events, dark counts, and optical mode mismatch. (d) Describe how you would verify entanglement swapping using quantum state tomography of the final A–C pair. Submit a detailed design document with diagrams, calculations, and a bill of materials.

**QUANTUM HARDWARE, ERROR CORRECTION & APPLICATIONS**

A Comprehensive Textbook for M.Sc. Physics

Quantum Computing Specialization  ·  Semester IV  ·  4 Credits

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**UNIT 4  |  CHAPTER 8**

**Entanglement Distillation, Quantum Internet Vision, Satellite QKD, and India’s Quantum Network**

BBPSSW & DEJMPS Protocols  ·  Quantum Internet Stages  ·  Micius Satellite  ·  India NQM  ·  Delhi–Pune Link

8 Original Figures  ·  8 Solved Examples  ·  15 MCQs  ·  10 Problems  ·  10 Theory Questions  ·  3 Assignments
