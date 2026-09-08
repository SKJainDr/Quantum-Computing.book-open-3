<img class="fig-img" src="content/images/image1.png" alt="figure">

**Q. C. Series | Volume III**

**First**

<img class="fig-img" src="content/images/image2.png" alt="figure">

**QUANTUM HARDWARE, ERROR CORRECTION & APPLICATIONS**

**Physical Qubits, Noise Mitigation & Quantum Computing Ecosystem**

**| BAREILLY, U.P. (INDIA)**

**Dedicated to**

My beloved Wife - Dr. Uma Jain

My caring son - Tanmaya Jain

My sweet Daughter-in-law - Shrishti Jain

*And*

*to every student who chooses to understand the machine, not just operate it.*

## Preface

This textbook has been crafted specifically for students of the M.Sc. Physics programme pursuing specialisation in Quantum Computing, corresponding to course: Quantum Hardware, Error Correction and Applications. It serves as a companion to Quantum Algorithms and Complexity and the advanced laboratory course Quantum Computing Laboratory II. Together, these courses form the capstone of a rigorous two-year quantum computing specialisation.

The field of quantum computing stands at a remarkable inflection point. We are squarely in the Noisy Intermediate-Scale Quantum (NISQ) era — a moment in history when quantum processors with tens to hundreds of qubits are available to researchers worldwide via cloud platforms, yet they remain imperfect, noisy machines that fall short of fault-tolerant operation. Understanding the landscape — the physics of real quantum hardware, the nature and management of noise, and the ingenious applications being demonstrated today — is the mission of this course.

The textbook is organised into five units spanning ten chapters, covering the complete syllabus:

- **Unit 1 (Chapters 1–2):** Quantum Hardware Technologies — superconducting qubits (Josephson junctions, transmon, DRAG, CZ/CR gates, IBM Eagle/Heron/Condor), trapped-ion qubits (Paul trap, laser cooling, Mølmer-Sørensen gate, IonQ, Quantinuum), photonic, silicon spin, topological, and neutral atom platforms. 20 original figures, 16 solved examples, complete MCQ and problem sets.

- **Unit 2 (Chapters 3–4):** Noise Sources, Characterisation and Error Mitigation — quantum channel formalism, Kraus operators, randomised benchmarking (standard, interleaved, two-qubit), gate set tomography, XEB, quantum process tomography, zero-noise extrapolation, probabilistic error cancellation, Pauli twirling, readout error mitigation, and the Qiskit Estimator interface with resilience levels.

- **Unit 3 (Chapter 5):** Quantum Cryptography and Communications — BB84 security proof using binary entropy, E91 protocol and Bell inequality, MDI-QKD, twin-field QKD, privacy amplification and the Leftover Hash Lemma, QRNG, and NIST PQC standards (CRYSTALS-Kyber, Dilithium, SPHINCS+). Note: Chapter 6 is a placeholder in this numbering; this textbook follows the uploaded chapter structure directly.

- **Unit 4 (Chapters 7–8):** Quantum Network Architecture and Satellite QKD — quantum repeater generations, entanglement swapping derivation, DLCZ/NV-centre/AFC memory platforms, satellite QKD (Micius), BBPSSW entanglement distillation, quantum internet stages, and India NQM network vision.

- **Unit 5 (Chapters 9–10):** Quantum Applications and Industry Landscape — VQE, UCCSD, QPE, QAOA, QAE, Ising model, Hubbard model, IBM utility 2023, global quantum market, NQM hubs, NIST PQC, career pathways, Qiskit certification, and GitHub portfolio strategy.

Every chapter is self-contained yet interconnected. Physical derivations are presented with full mathematical rigour, but each equation is accompanied by physical intuition in plain language. Historical anecdotes anchor abstract concepts: from Josephson's 1962 prediction to Feynman's 1982 quantum simulation vision, from the first qubit coherence measurements to IBM's 127-qubit utility demonstration of 2023. Every chapter ends with a RECAP section containing 15 short answer questions and full model answers, followed by 15 MCQs with answers and 10 unsolved problems.

A comprehensive set of reference materials appears before Chapter 1: table of contents, figure index, table index, and symbol table. A complete alphabetical index of important terms appears at the end of the book. Page numbers follow Roman numeral convention (i, ii, iii...) for front matter, Arabic (1, 2, 3...) for main content, and capital Roman (I, II, III...) for the end index.

*—* ***Dr. Sanjeev Kumar Jain***

## Course Syllabus – Quantum Hardware, Error Correction and Applications

| Parameter | Details |
|---|---|
| Course Title | Quantum Hardware, Error Correction and Applications |
| Semester | IV (Second Year, Second Semester) |
| Teaching Scheme | 30 Hours Lecture + 10 Hours Tutorial |
| Credits | 4 |
| Marks | CA: 30  \|  EE: 70  \|  Total: 100 |
| Prerequisites | Quantum Computers – Foundational Theory, Quantum Computing Laboratory — I |
| Co-requisite | Quantum Algorithms & Complexity, Quantum Computing Laboratory — II |

### Course Outcomes

| CO | Outcome |
|---|---|
| CO1 | Compare the physics, gate fidelities, coherence times, and scalability of superconducting, trapped-ion, photonic, spin, and topological qubit technologies, and justify device selection for given algorithms. |
| CO2 | Apply quantum error mitigation techniques (ZNE, PEC, Pauli twirling, readout correction) to reduce effective noise on NISQ hardware without additional qubits. |
| CO3 | Describe the quantum cryptography landscape: BB84 security proof, E91, MDI-QKD, post-quantum cryptography (Kyber, Dilithium, SPHINCS+), and QRNG. |
| CO4 | Apply quantum computing to chemistry (VQE, UCCSD, QPE), finance (portfolio QUBO, QAOA, QAE), and logistics; implement solutions in Qiskit. |
| CO5 | Explain quantum network architecture: quantum repeaters, entanglement swapping, satellite QKD, memory platforms; assess India NQM goals. |

### Unit-Wise Syllabus

| Unit | Chapter(s) | Topics | Hours |
|---|---|---|---|
| 1 | 1–2 | Superconducting qubits (Josephson junction, transmon, DRAG, CZ/CR gates, IBM Eagle/Heron/Condor); Trapped-ion qubits (Paul trap, laser cooling, Mølmer-Sørensen gate, IonQ, Quantinuum); Photonic, spin, topological, neutral atom platforms | 8 |
| 2 | 3–4 | Noise sources (gate errors, SPAM, crosstalk, T₁/T₂); Kraus operators, Pauli channels, PTM; RB, IRB, 2Q-RB, GST, XEB, QPT; ZNE, PEC, Pauli Twirling, Readout Mitigation, Qiskit Estimator | 8 |
| 3 | 5 | BB84 security proof, binary entropy, E91, MDI-QKD, TF-QKD, satellite QKD, QRNG, PQC (Kyber, Dilithium, SPHINCS+) | 6 |
| 4 | 7–8 | Quantum network architecture, quantum channels, entanglement swapping, 1st/2nd/3rd gen repeaters, DLCZ, NV-centre, AFC memories, satellite QKD (Micius), quantum internet, NQM network | 5 |
| 5 | 9–10 | VQE, QPE, 2nd quantisation, JW/BK mapping; QUBO, QAOA, QAE; Ising model, Hubbard model, IBM utility 2023; Quantum market, NQM hubs, PQC standards, career pathways, Qiskit cert | 5 |

## The various information boxes in this textbook

Throughout this volume, recurring boxes flag a specific kind of content so you always know, at a glance, what you are about to read.

<div class="box box-key-concept">
<p class="box-title"><strong>📋  LEARNING OBJECTIVES</strong></p>
<p>Opens every chapter — the specific, measurable outcomes you should master by the end of the chapter.</p>
</div>

<div class="box box-key-concept">
<p class="box-title"><strong>🔑  KEY CONCEPT BOXES</strong></p>
<p>The single most important idea in a section, distilled for quick review and exam preparation.</p>
</div>

<div class="box box-anecdote">
<p class="box-title"><strong>📜  ANECDOTE / HISTORICAL BOXES</strong></p>
<p>The human and historical story behind a discovery — who found it, when, and why it mattered.</p>
</div>

<div class="box box-real-world">
<p class="box-title"><strong>🌍  REAL WORLD BOXES</strong></p>
<p>How the concept is used in an actual quantum device, company, or published experiment today.</p>
</div>

<div class="box box-warning">
<p class="box-title"><strong>⚠  WARNING BOXES</strong></p>
<p>Common misconceptions and subtle traps that catch students (and sometimes textbooks).</p>
</div>

<div class="box box-math">
<p class="box-title"><strong>🧮  MATHEMATICS BOXES</strong></p>
<p>A complete, step-by-step derivation set apart from the main narrative for careful study.</p>
</div>

<div class="box box-math">
<p class="box-title"><strong>▶  DEFINITION / THEOREM BOXES</strong></p>
<p>A formal statement of a definition, theorem, or protocol, given precisely and concisely.</p>
</div>

<div class="box box-key-concept">
<p class="box-title"><strong>💡  TIP BOXES</strong></p>
<p>Practical advice — on problem-solving, hardware intuition, or working efficiently in Qiskit.</p>
</div>

<div class="box box-real-world">
<p class="box-title"><strong>ℹ  ROADMAP / PROTOCOL BOXES</strong></p>
<p>A short orientation to what a section or protocol covers before you dive into the detail.</p>
</div>
