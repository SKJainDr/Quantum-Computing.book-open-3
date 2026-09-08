# UNIT 5 | CHAPTER 10

# Quantum Industry and Career Landscape

<div class="box box-key-concept">
<p class="box-title"><strong>📋  Learning Objectives</strong></p>
<p>After completing this chapter you will be able to: (1) Describe the global quantum technology market by segment and project its growth to 2035; (2) Map the quantum industry ecosystem from hardware through cloud, software, applications, and consulting; (3) Explain the quantum threat to classical cryptography (Shor's algorithm) and describe the NIST PQC standards (CRYSTALS-Kyber, Dilithium, SPHINCS+); (4) Describe India's National Quantum Mission structure, four hubs, budget, and 2031 milestones; (5) Compare four major quantum career pathways with required skills and education; (6) Construct a personalised quantum skill development plan using the skill matrix; (7) Prepare for the IBM Qiskit Developer Certification exam; (8) Design and execute a quantum GitHub portfolio strategy.</p>
</div>

## 10.1 The Global Quantum Market: Size, Segments and Growth Projections

<div class="box box-generic">

<img class="fig-img" src="content/images/image73.png" alt="">
</div>

**Figure 10.1: Global Quantum Technology Market 2024-2035 and Government Investment by Country**

Left: Stacked area chart showing the global quantum technology market 2024-2035 by segment. Hardware (blue): quantum processors, cryogenic systems, photonic devices, and ancillary equipment; growing from $2.1B (2024) to $72B (2035). Software & Platforms (teal): quantum SDKs, cloud platforms, algorithm libraries, and simulation tools; $1.2B to $65B. Services & Consulting (orange): system integration, use-case development, training, and QC-as-a-service; $0.8B to $48B. Post-Quantum Security (purple): PQC software, hardware security modules, and quantum-safe network equipment -- the largest and fastest-growing segment driven by regulatory mandates; $1.5B to $70B. Gold line: total market growing from $5.6B (2024) to $255B (2035). Right: Horizontal bar chart of cumulative government quantum investment by country to 2024. China leads at $15B (national quantum initiative since 2016). EU at $7.2B (Quantum Flagship). USA at $3.7B (NQIA 2018 + IRA supplements). India at $730M (NQM 2023), positioned mid-tier with the orange dashed marker. The chart contextualises India's investment relative to global competition and highlights the strategic importance of the NQM for maintaining quantum parity.

### 10.1.1 Market Size and Growth Projections

The global quantum technology market is at an inflection point. McKinsey & Company (2021) projected a $450-850 billion quantum value creation opportunity by 2040. Boston Consulting Group (2022) estimated $450 billion in annual value by 2035 across pharmaceuticals, chemicals, finance, and logistics. More conservative estimates from IDC (2023) project the quantum computing market alone (hardware + software + services) at $8.6 billion by 2027, growing at a CAGR of 50.9%.

The market has four distinct segments with different growth drivers and timelines:

- Quantum Hardware: Physical quantum systems (superconducting processors, trapped-ion systems, photonic chips, neutral atom processors) and supporting infrastructure (dilution refrigerators, microwave electronics, laser systems). Currently the largest segment by revenue, dominated by IBM, IonQ, Quantinuum, QuEra, D-Wave, and Rigetti. Growth driven by qubit count and fidelity improvements.

- Quantum Software and Cloud Platforms: IBM Quantum Network, Amazon Braket, Microsoft Azure Quantum, Google Quantum AI Cloud, and IonQ Cloud. Also includes SDK revenues (Qiskit, PennyLane, Cirq), quantum simulation software (Qiskit Aer, QuTiP), and quantum algorithm libraries. Growth driven by cloud quantum access democratisation.

- Quantum Services and Consulting: Use-case development, quantum feasibility studies, proofs-of-concept, training programmes, and system integration. Major consultancies (McKinsey, Accenture, IBM Consulting, PwC Quantum Hub) have established quantum practices. Growing fastest in percentage terms as enterprises begin quantum exploration.

- Post-Quantum Cryptography (PQC): The largest and most urgent segment due to regulatory mandates. US federal agencies are mandated by NIST to migrate to PQC by 2030. Financial regulators in the UK, EU, and India are issuing PQC migration guidelines. Hardware Security Module (HSM) upgrades, VPN software updates, TLS library patches, and PKI infrastructure replacement are all driving large PQC procurement.

<div class="box box-real-world">
<p class="box-title"><strong>🌐  McKinsey Global Quantum Value Study (2021)</strong></p>
<p>McKinsey estimated four industries will capture the bulk of quantum value creation: pharmaceuticals &amp; chemicals ($100-170B), finance ($60-170B), automotive, aerospace &amp; defence ($50-110B), and electronics &amp; IT ($40-80B). Of these, pharmaceuticals and chemistry are expected to be the earliest and largest beneficiaries, primarily through quantum chemistry simulation (VQE, QPE -- covered in Chapter 6). The 2023 McKinsey update revised total opportunity upward to $450-850B by 2040, reflecting faster-than-expected hardware progress.</p>
</div>

### 10.1.2 Leading Hardware Companies (2024)

| Company | Technology | Qubit Count | Best 2Q Fidelity | Key Differentiator |
|---|---|---|---|---|
| IBM Quantum | Superconducting (transmon) | 1121 (Condor) | 99.9% (Heron) | Largest qubit count; heavy-hexagonal topology; cloud access |
| Google Quantum AI | Superconducting (transmon) | 70 (Sycamore) | 99.7% | Tunable couplers; quantum supremacy claim 2019; Willow 2024 |
| IonQ | Trapped ion (Yb+) | 35 (Forte) | 99.9% | All-to-all connectivity; QV > 4,000,000; Nasdaq listed (IONQ) |
| Quantinuum | Trapped ion (QCCD) | 56 (H2) | 99.9% | Highest gate fidelity commercially; QCCD ion shuttling; QV 8192+ |
| QuEra Computing | Neutral atom (Rb tweezers) | 256 (Aquila) | 99.5% | Reconfigurable arrays; 48 logical qubits below break-even (2023) |
| D-Wave Systems | Quantum annealing (flux qubits) | 5000+ (Adv. 2) | N/A (analog) | Commercial annealing; Advantage for optimisation; Nasdaq listed |

Table 10.1: Leading quantum hardware companies (2024). Highlighted rows show trapped-ion leaders with highest gate fidelity.

## 10.2 The Quantum Industry Ecosystem

<div class="box box-generic">

<img class="fig-img" src="content/images/image74.png" alt="">
</div>

**Figure 10.2: Global Quantum Industry Ecosystem -- Companies Across All Layers**

Four-layer ecosystem map of the global quantum technology industry. Hardware Layer (top, blue border): leading quantum computer manufacturers including IBM Quantum (Eagle/Heron/Condor superconducting processors), Google Quantum AI (Sycamore), IonQ (Aria/Forte trapped-ion), Quantinuum (H2 QCCD), QuEra (Aquila neutral atom), Rigetti (Ankaa-2), and D-Wave (Advantage annealer). Software & Cloud Layer (second, green border): quantum SDKs and cloud platforms -- Qiskit (IBM), Cirq (Google), PennyLane (Xanadu), Azure Quantum (Microsoft), Amazon Braket (AWS), Q# (Microsoft), and OpenFermion (Google). Applications Layer (third, gold border): domain-specific quantum applications -- quantum chemistry (VQE/QPE), quantum finance (QAOA/QAE), combinatorial optimisation (QUBO), quantum machine learning (QNN/QSVM), materials simulation (Ising/Hubbard), post-quantum cryptography (NIST PQC), and quantum networking (QKD/repeaters). Services & Consulting Layer (bottom, pink border): management consultancies and specialised quantum firms -- McKinsey Quantum Technology, Accenture Quantum, IBM Consulting Quantum, PwC Quantum Hub, BCG Quantum, QC Ware (finance focus), and Multiverse Computing.

### 10.2.1 Cloud Quantum Platforms

Cloud quantum computing has democratised access to real quantum hardware. Any researcher, student, or company can now run quantum circuits on real processors without owning hardware. The major platforms are:

- IBM Quantum Platform (quantum.ibm.com): Free tier provides access to up to 127-qubit processors with 10 minutes of compute per month. IBM Quantum Premium provides dedicated access to Heron and larger processors. The platform uses Qiskit Runtime with Sampler and Estimator primitives. IBM has over 400 quantum network partners globally.

- Amazon Braket (AWS): Multi-hardware platform supporting IonQ, Rigetti, Oxford Quantum Circuits, and QuEra processors. Uses Python Braket SDK. Useful for comparing results across hardware platforms. Pay-per-task pricing.

- Microsoft Azure Quantum: Provides access to IonQ, Quantinuum, and Rigetti hardware. Also provides quantum-inspired optimisation solvers. Uses Q# language and Azure Quantum Development Kit.

- Google Quantum AI: Research-focused access (not generally public) via Google Cloud. The Cirq SDK is open-source. Google recently announced the Willow chip (2024) with below-threshold error correction.

- IonQ Cloud (via AWS, Azure, Google): Direct access to IonQ Aria and Forte processors from multiple cloud marketplaces. IonQ has the highest published Quantum Volume among commercial systems.

<div class="box box-real-world">
<p class="box-title"><strong>🌐  IBM Quantum Network -- The World's Largest Quantum Community</strong></p>
<p>The IBM Quantum Network comprises over 400 organisations including universities, research laboratories, startups, and Fortune 500 companies. Members include universities (MIT, Stanford, IIT, Tokyo), national laboratories (Argonne, Oak Ridge, Fraunhofer), and industry partners (JPMorgan, Boeing, Samsung, Mitsubishi, ExxonMobil, Daimler). Network members receive dedicated quantum compute time, early access to new processors, collaboration opportunities, and technical support from IBM Quantum researchers. India has multiple IBM Quantum Network member institutions, including IIT Bombay, IIT Madras, and TIFR.</p>
</div>

### 10.2.2 Application Companies and Vertical Use Cases

A growing ecosystem of companies applies quantum computing to specific industry problems:

- Finance: QC Ware (quantum-enhanced machine learning and portfolio optimisation, Goldman Sachs partnership), Multiverse Computing (Singularity platform for finance, insurance, and energy optimisation -- BBVA, Credit Agricole, Total Energies), Quantinuum (InQuanto chemistry platform, HSBC partnership).

- Chemistry and Drug Discovery: ProteinQure (protein design using quantum ML), Menten AI (quantum-assisted enzyme engineering), Qubit Pharmaceuticals (quantum simulation for drug-receptor binding, Quantinuum partnership), Algorithmiq (quantum advantage for quantum chemistry with tensor network hybrid methods).

- Logistics and Optimisation: Volkswagen (traffic flow optimisation using D-Wave annealing), Airbus (aircraft loading optimisation with QAOA), DHL (quantum-optimised logistics routing), BMW (quantum-assisted autonomous driving route planning).

- Quantum Security: Post-Quantum (PQC products for VPNs and HSMs), SandboxAQ (quantum-safe cryptography and quantum sensing, Google spin-out 2022), ISARA Corporation (quantum-safe certificate authority products).

<div class="box box-anecdote">
<p class="box-title"><strong>📜  2023-2024 Industry Milestones</strong></p>
<p>Key commercial milestones: (1) IBM Quantum (2023): 1121-qubit Condor, 133-qubit Heron with 99.9% 2Q fidelity, and the first "quantum utility" result in Nature. (2) Quantinuum (2023): H2 processor achieves record QV &gt; 1,000,000; first demonstration of non-Abelian anyons for topological qubits (Microsoft collaboration). (3) QuEra (2023): 48 logical qubits below break-even error threshold -- first useful-scale error correction. (4) Google (2024): Willow chip demonstrates below-threshold error correction (errors reduce as code distance increases for the first time). (5) IonQ (2024): Forte processor, 35 algorithmic qubits, QV &gt; 4,000,000.</p>
</div>

## 10.3 Post-Quantum Cryptography: Securing the Quantum Future

<div class="box box-generic">

<img class="fig-img" src="content/images/image75.png" alt="">
</div>

**Figure 10.3: Post-Quantum Cryptography Timeline and NIST PQC Standards (FIPS 2024)**

Left: Cryptographic timeline from 1977 to 2035. RSA published (1977): the dominant public-key cryptosystem based on the hardness of integer factorisation. Shor's algorithm published (1994): Peter Shor demonstrated that a fault-tolerant quantum computer could factor N-bit integers in O(N^3 log N) time, threatening RSA, ECC, and Diffie-Hellman. NIST PQC competition launched (2016): 82 initial submissions, 7 years of cryptanalysis. NIST Draft Standards (2022): CRYSTALS-Kyber and CRYSTALS-Dilithium selected. NIST Final Standards FIPS 203-205 (2024): the new baseline for quantum-safe cryptography. The red shaded region (2024-2035) marks the Harvest-Now-Decrypt-Later window: adversaries can intercept encrypted communications today and store them, decrypting them once a fault-tolerant quantum computer exists. Data with 10-30 year confidentiality requirements (medical records, state secrets, military communications) is already at risk. Right: The four NIST PQC standards with type, mathematical basis, and use case. CRYSTALS-Kyber (FIPS 203): KEM based on module learning with errors (M-LWE), replacing RSA/DH for key exchange in TLS 1.3, HTTPS, and VPN. CRYSTALS-Dilithium (FIPS 204): digital signature based on module-LWE, replacing RSA and ECDSA for authentication. SPHINCS+ (FIPS 205): stateless hash-based signature using SHA-3, ideal for long-term archival applications. FALCON (future FIPS): compact lattice signature based on NTRU, optimal for IoT and constrained devices.

### 10.3.1 The Quantum Threat to Classical Cryptography

Most modern Internet security depends on two mathematical problems believed to be hard for classical computers: (1) Integer Factorisation: RSA encryption relies on the difficulty of factoring large integers N = p\*q into primes p and q. RSA-2048 would take 10^18 years for the best classical algorithm. (2) Discrete Logarithm: Elliptic Curve Cryptography (ECC) and Diffie-Hellman key exchange rely on the discrete logarithm problem in finite groups. ECC-256 would take 10^15 years classically.

In 1994, Peter Shor published his quantum algorithm for integer factorisation. Shor's algorithm runs in polynomial time O(n^3 log n) on a quantum computer -- an exponential speedup over the best classical algorithm O(exp(n^{1/3})). The same algorithm (with minor modification) solves the discrete logarithm problem. A fault-tolerant quantum computer with approximately 4000 logical qubits and 10^10 quantum operations could break RSA-2048 in a matter of hours.

<div class="box box-generic">
<p class="box-title">Quantum vs classical RSA factoring complexity</p>

</div>

### 10.3.2 Harvest-Now-Decrypt-Later Attacks

The "Q-Day" threat — the day a fault-tolerant quantum computer breaks RSA — may be 10-20 years away, but the risk is immediate. Adversaries (nation-state intelligence agencies, sophisticated criminal groups) are already conducting "harvest-now-decrypt-later" (HNDL) attacks: intercepting and storing encrypted communications today, planning to decrypt them once quantum computers become available.

Data at risk from HNDL:

- Medical records (typically held for 30+ years): personal health information, genomic data, mental health records.

- Government and military secrets with long-term strategic relevance.

- Intellectual property: drug compound patents, semiconductor designs, source code with 10-20 year competitive value.

- Financial transaction logs and long-term account identifiers.

- Personal identity credentials (biometrics, national ID data) that cannot be changed.

<div class="box box-warning">
<p class="box-title"><strong>⚠️  Urgency: Migrate to PQC Now</strong></p>
<p>The US National Security Agency (NSA) issued a Cybersecurity Advisory in 2022 requiring all National Security Systems to plan PQC migration. US OMB (Office of Management and Budget) Memorandum M-23-02 (2022) mandated all US federal agencies to inventory cryptographic assets and begin migration to quantum-resistant algorithms. The EU's ENISA recommends starting PQC migration planning immediately for long-lived secrets. In India, CERT-In and the NQM QuCryptoS hub are coordinating the national PQC assessment programme. The migration will take 5-10 years -- starting now is essential.</p>
</div>

### 10.3.3 NIST PQC Standards: CRYSTALS-Kyber, Dilithium and SPHINCS+

After a 7-year global competition (2016-2024), NIST published three final PQC standards in August 2024:

| Standard | FIPS | Type | Mathematical Basis | Key Use Case |
|---|---|---|---|---|
| CRYSTALS-Kyber | FIPS 203 | KEM (Key Encapsulation) | Module-LWE lattice | TLS 1.3, HTTPS, VPN, SSH |
| CRYSTALS-Dilithium | FIPS 204 | Digital Signature | Module-LWE lattice | Code signing, auth., email |
| SPHINCS+ | FIPS 205 | Hash-based Signature | SHA-3 hash functions | Long-term archival signing |
| FALCON | Future FIPS | Compact Lattice Signature | NTRU lattice problem | IoT, constrained devices |

Table 10.2: NIST Post-Quantum Cryptography Standards (FIPS 2024). Highlighted rows are primary deployment targets.

CRYSTALS-Kyber for Key Exchange: Kyber replaces RSA and Diffie-Hellman for establishing session keys. It is based on the Module Learning With Errors (M-LWE) problem. Public key size: 800-1568 bytes; ciphertext: 768-1568 bytes -- larger than RSA-2048 keys (256 bytes) but manageable for modern networks. Security level: 128-256 bits quantum security. Already deployed in Chrome, Cloudflare, and AWS KMS (AWS Key Management Service).

CRYSTALS-Dilithium for Digital Signatures: Dilithium replaces RSA-PSS and ECDSA for digital signatures. Based on Module-LWE and Module-SIS (Short Integer Solution). Signature size: 2420-4595 bytes -- significantly larger than ECDSA-256 (64 bytes), requiring larger TLS certificates. Security level: 128-256 bits quantum security. Public key: 1312-2592 bytes. Recommended for general-purpose signature applications.

SPHINCS+: The Conservative Choice: SPHINCS+ is based solely on the security of hash functions (SHA-3), making it the most conservative choice -- its security does not depend on any mathematical assumption beyond hash function collision resistance. Ideal for long-term archival applications (legal documents, medical records, long-lived code signing certificates) where signature size is less critical than long-term security assurance.

## 10.4 India's National Quantum Mission (NQM)

<div class="box box-generic">

<img class="fig-img" src="content/images/image76.png" alt="">
</div>

**Figure 10.4: India's National Quantum Mission -- Structure, Technology Hubs and Timeline**

Comprehensive overview of India's National Quantum Mission (approved by Union Cabinet, April 2023). Top: Central mission structure under the Ministry of Science & Technology, with Rs.6003 crore budget (~$730M) for 2023-2031. The NQM connects to four Technology Innovation Hubs via bidirectional arrows (gold lines): QuST (Quantum Computing & Simulation, hosted at IIT Bombay + IISc Bangalore), QuCryptoS (Quantum Communications & Cryptography, IIT Delhi + C-DOT + DRDO), QuNAT (Quantum Sensing & Metrology, IIT Madras + NPL India), QuMAT (Quantum Materials & Devices, JNCASR + IISc). Middle: Timeline of NQM milestones from 2023 (NQM approved) through 2025-26 (first quantum computer prototypes), 2026-28 (indigenous quantum satellite Q-Sat launch), 2027-29 (2000 km QKD network deployment), to 2031 (full mission completion). The milestone boxes are colour-coded by phase and connected by sequential arrows. Bottom: Budget allocation bar showing proportional distribution: QuST Rs.1800Cr (30%), QuCryptoS Rs.1440Cr (24%), QuNAT Rs.1440Cr (24%), QuMAT Rs.960Cr (16%), Infrastructure/Admin Rs.363Cr (6%).

### 10.4.1 NQM Structure and Governance

India's National Quantum Mission was approved by the Union Cabinet on April 19, 2023, with a budget of Rs.6003 crore (~$730 million) for an 8-year period (2023-2031). The NQM is administered by the Department of Science and Technology (DST) under the Ministry of Science and Technology. The mission is overseen by a Mission Governing Board chaired by the Principal Scientific Adviser to the Government of India.

The NQM has four primary objectives:

- Quantum Computing: Develop indigenous quantum computers with 50-1000 qubit processors, satellite-based quantum computing capabilities, and supporting quantum software ecosystem.

- Quantum Communication: Deploy a 2000 km terrestrial QKD network, launch an indigenous quantum satellite (Q-Sat) for satellite-based QKD, and establish QRNG certification for banking and government.

- Quantum Sensing and Metrology: Develop quantum-enhanced gravimeters, magnetometers, atomic clocks, and quantum-enhanced imaging systems with defence and civilian applications.

- Quantum Materials: Develop indigenous superconducting qubit fabrication, topological qubit research, quantum-grade materials synthesis, and cryogenic infrastructure.

### 10.4.2 The Four Technology Innovation Hubs

| Hub | Budget (Cr) | Lead Institutions | Key Research Focus | 2031 Target |
|---|---|---|---|---|
| QuST (Quantum Computing & Simulation) | Rs.1800 | IIT Bombay, IISc Bangalore, TIFR Mumbai | Superconducting & trapped-ion qubits, error correction, VQE/QAOA algorithms, quantum software | 50-qubit indigenous QC prototype; 1000-qubit roadmap; Qiskit integration |
| QuCryptoS (Quantum Communications) | Rs.1440 | IIT Delhi, C-DOT, DRDO, CDAC | QKD systems, QRNG devices, post-quantum cryptography, quantum satellite communications | 2000 km QKD fibre; Q-Sat launch; PQC standards for India |
| QuNAT (Quantum Sensing) | Rs.1440 | IIT Madras, NPL India, IIT Roorkee | Quantum gravimeters, magnetometers, atomic clocks, quantum-enhanced lidar, navigation | Field-deployable quantum gravimeter; atomic clock accuracy 10^{-18} |
| QuMAT (Quantum Materials) | Rs.960 | JNCASR, IISc, IIT Bombay, IIT Kanpur | Superconducting thin films, topological materials, 2D materials, cryogenic packaging, qubit fabrication | Indigenous SC qubit fabrication; topological qubit prototypes; qubit foundry capability |

Table 10.3: India NQM four technology hubs with budget, institutions, focus areas, and 2031 targets.

### 10.4.3 Key Indian Institutions in Quantum Technology

India has a strong foundation of quantum physics research across its premier institutions:

- IIT Delhi (IIT-D): Quantum cryptography laboratory (Prof. Pramod Hemrajani group), QKD implementations, quantum optics. IBM Quantum Network member. Partner to QuCryptoS hub.

- IISc Bangalore: Quantum condensed matter (Prof. Vijay Shenoy), superconducting materials, quantum photonics (Prof. Akshay Naik). Partner to both QuST and QuMAT hubs.

- TIFR Mumbai: Atomic physics (BEC group), quantum materials, quantum information theory. Pioneer of quantum optics research in India. Partner to QuST hub.

- IIT Bombay: Quantum computing (Prof. Himanshu Tyagi group), quantum information, quantum machine learning. Host institution for QuST hub.

- IIT Madras: Quantum sensing (Prof. Anil Prabhakar group), quantum optics, integrated photonics. Host of QuNAT hub.

- C-DOT (Centre for Development of Telematics): India's premier telecom R&D institution. Developed the first indigenous BB84 QKD system. Critical partner for QuCryptoS.

- DRDO (Defence Research and Development Organisation): Quantum sensing for defence applications, secure quantum communications for military. Partner across multiple NQM hubs.

<div class="box box-real-world">
<p class="box-title"><strong>🇮🇳  India's Quantum Workforce Challenge</strong></p>
<p>India trains approximately 22,000 Ph.D. students per year in STEM, but currently fewer than 500 specialise in quantum physics/computing. The NQM includes a dedicated workforce development programme targeting 10,000 quantum-skilled professionals by 2031 -- researchers, engineers, software developers, and quantum-literate industry practitioners. This includes: National Quantum Scholarships (100 per year for Ph.D. in quantum technology at NQM hub institutions); Quantum Fellowships for postdoctoral researchers returning from abroad; Quantum Short Courses for industry professionals (3-6 month intensive programmes); and the Qiskit India Certification Programme (in partnership with IBM Quantum).</p>
</div>

## 10.5 Career Pathways in Quantum Computing

<div class="box box-generic">

<img class="fig-img" src="content/images/image77.png" alt="">
</div>

**Figure 10.5: Quantum Computing Career Pathways -- Four Primary Roles, Skills and Salary Bands**

Four primary quantum career pathways shown as panels with skills, requirements, and salary bands. Quantum Hardware Engineer (teal): Designs, fabricates, and characterises quantum processors. Key skills: qubit fabrication techniques (lithography, thin-film deposition), cryogenic systems engineering (dilution refrigerators, cryogenic wiring), microwave engineering (signal generation, low-noise amplification), device physics (Josephson junctions, ion trap design). Requirements: Ph.D. in Physics or Electrical Engineering, clean-room experience, LabVIEW/Python instrumentation programming. Quantum Algorithm Researcher (purple): Develops new quantum algorithms, analyses complexity, designs error correction codes. Key skills: quantum algorithm design (circuit synthesis, amplitude amplification), computational complexity theory (BQP, QMA complexity classes), quantum error correction (stabiliser codes, fault-tolerance), mathematical proof writing. Requirements: Ph.D. in CS, Mathematics, or Physics, strong publication record, proficiency in Qiskit or Cirq. Quantum Software Developer (blue): Builds quantum SDK components, compilers, transpilers, and cloud integration. Key skills: quantum SDK development (circuit representation, transpilation), compiler design (gate decomposition, routing), cloud architecture (REST APIs, containerisation), systems programming. Requirements: M.Sc./Ph.D. in Computer Science, Python/Rust/C++ proficiency, GitHub portfolio. Quantum Applications Scientist (green): Implements quantum algorithms for specific industry domains (chemistry, finance, logistics), benchmarks results. Requirements: M.Sc./Ph.D. plus domain expertise, Qiskit Developer Certification, industry sector experience. Bottom: salary bands from Entry (0-2yr, Rs.8-15L/$15-30K) through Research Director (Rs.1.5-3Cr+/$150-400K).

### 10.5.1 Quantum Hardware Engineer

Quantum hardware engineers design, build, and characterise the physical quantum computing systems. This is the most specialised and currently most in-demand career path due to the rapid pace of hardware development across multiple qubit modalities.

Day-to-day work: Designing qubit layouts using CAD tools (Sonnet, HFSS for microwave simulation); performing device fabrication in cleanroom environments (electron beam lithography, ALD, wet etching); setting up and maintaining dilution refrigerator systems (BlueFors, Oxford Instruments); characterising qubit parameters (T1, T2, frequency, anharmonicity) using microwave test equipment; implementing pulse-level control for quantum gates using AWGs and IQ mixers.

Key employers (2024): IBM Quantum (USA, Germany), Google Quantum AI (USA), IonQ (USA, Germany), Quantinuum (USA, UK), Intel Labs (USA, Netherlands), D-Wave (Canada), Microsoft Station Q (USA), Oxford Quantum Circuits (UK), Pasqal (France), QuEra (USA). In India: IIT Delhi, IISc, TIFR, C-DOT, DRDO, and the NQM hubs will create 200-300 hardware engineering positions by 2027.

### 10.5.2 Quantum Algorithm Researcher

Quantum algorithm researchers develop new quantum algorithms, prove their correctness and complexity, and design error correction protocols. This is the most mathematically demanding career path, requiring fluency in quantum information theory, complexity theory, and abstract algebra.

Key sub-specialisations: Quantum complexity theory (classifying problems by quantum hardness, proving BQP vs QMA separations); Quantum error correction (designing new stabiliser codes, threshold theorems, fault-tolerant protocols); Variational algorithms (VQE, QAOA, QML -- the NISQ-era focus); Quantum cryptography (protocols, security proofs, post-quantum cryptanalysis); Quantum simulation algorithms (Hamiltonian simulation methods, Trotterisation, LCU).

Key employers: IBM Research (Zurich, Yorktown Heights, Tokyo), Google DeepMind/Quantum AI, Microsoft Research, Bell Labs (Nokia), national laboratories (NIST, Sandia, Argonne, Brookhaven), academia globally. Typical career path: Ph.D. (4-6 years) + postdoc (2-3 years) + faculty/research scientist role.

### 10.5.3 Quantum Software Developer

Quantum software developers build the infrastructure that makes quantum computers programmable. This includes quantum SDKs, circuit transpilers, compilers (from high-level algorithms to hardware-native gates), noise-aware optimization tools, and cloud APIs.

Required technical skills: Python (advanced: metaclasses, C extensions, async programming); systems programming in Rust or C++ for performance-critical SDK components; compiler theory (IR design, gate synthesis, optimization passes); graph algorithms (qubit routing is a graph problem); quantum information theory (to correctly implement quantum operations); cloud architecture (REST APIs, gRPC, Kubernetes, Docker).

Key employers: IBM (Qiskit core team), Xanadu (PennyLane), Google (Cirq), Microsoft (Q# + Azure Quantum), AWS (Braket SDK), Quantinuum (TKET/Pytket), Rigetti, QC Ware, and quantum startups globally. Also classical tech companies with quantum programmes: Accenture, Capgemini, SAP, Siemens. The quantum software developer role has the largest overlap with classical software engineering and is the most accessible entry point for computer science graduates.

### 10.5.4 Quantum Applications Scientist

Quantum applications scientists (also called "quantum use case engineers" or "quantum solutions architects") work at the intersection of quantum computing and specific industry domains. They translate domain problems into quantum circuits, benchmark quantum solutions against classical baselines, and communicate results to non-technical stakeholders.

Sub-specialisations by domain: Quantum chemistry applications (requires Ph.D. in computational chemistry or physics; implements VQE/QPE for drug discovery, catalyst design); Quantum finance (requires quantitative finance background; implements QAOA, QAE for portfolio optimisation, risk analysis -- JPMorgan, Goldman Sachs, HSBC); Quantum logistics (implements QAOA for vehicle routing, supply chain -- Volkswagen, DHL, BMW); Quantum machine learning (implements QSVM, QNN for ML acceleration -- IBM, AWS, academic groups).

<div class="box box-real-world">
<p class="box-title"><strong>💼  Quantum Careers in India: Near-Term Outlook (2024-2028)</strong></p>
<p>The NQM and its four hubs will create approximately 3000-5000 direct quantum technology jobs in India by 2028. Indirect employment (quantum-aware software engineers, PQC implementers, quantum-aware chemists and financial analysts) will be 10-20x this number. Key hiring sectors: (1) NQM hub institutions: faculty, postdocs, research engineers, lab technicians; (2) IT sector: TCS, Infosys, Wipro, HCL all have quantum programmes hiring quantum developers and consultants; (3) Defence: DRDO, ISRO quantum sensing and secure communication programmes; (4) Finance: HDFC Bank, SBI, SEBI-regulated institutions migrating to PQC; (5) Pharma: Sun Pharma, Cipla, Biocon evaluating quantum chemistry for drug discovery.</p>
</div>

## 10.6 Quantum Career Skills: The Skill Matrix

<div class="box box-generic">

<img class="fig-img" src="content/images/image78.png" alt="">
</div>

**Figure 10.6: Quantum Career Skill Requirements by Role (1=Low, 5=Essential)**

Skill importance heatmap for six quantum career roles across ten skill dimensions. Rows (roles): Hardware Engineer, Algorithm Researcher, Software Developer, Applications Scientist, QC Consultant, Research Scientist. Columns (skills): Python, Qiskit/SDK, Linear Algebra, Quantum Mechanics, Machine Learning, Circuit Design, Error Correction, Cloud Platforms, C++/Rust, Domain Expertise. Colour scale: red (low importance) through yellow (medium) to green (essential). Key insights from the matrix: Python is essential (5) or high (4) for all roles. Qiskit/SDK knowledge is essential for Software Developer, Applications Scientist, and Research Scientist. Linear Algebra is critical across all roles -- the single most universally important mathematical skill. Quantum Mechanics is essential for Hardware Engineer, Algorithm Researcher, and Research Scientist but less critical for Software Developer and Consultant. C++/Rust is essential for Hardware Engineer and Software Developer but less important for other roles. Domain Expertise (chemistry, finance, or materials knowledge) is critical for Applications Scientist and Consultant but less so for pure hardware/algorithm roles. The matrix should guide personalised skill development: identify your target role, find the red cells in that row, and prioritise those skills.

Based on the skill matrix, quantum careers share several universal prerequisites:

- Python programming (advanced level): All quantum SDKs are Python-based. Data structures, object-oriented programming, scientific computing (NumPy, SciPy, Matplotlib), and asynchronous programming are all required.

- Linear algebra: Quantum states are vectors; quantum operations are matrices; quantum measurements are projectors. Eigenvalues, tensor products, unitary matrices, Hermitian operators, and the spectral theorem are essential tools for every quantum role.

- Quantum mechanics foundations: Dirac notation (|psi>, bra-ket calculus), superposition, entanglement, measurement postulate, density matrices, quantum channels. The equivalent of an M.Sc.-level quantum mechanics course.

- Quantum circuit design: Understanding of single-qubit and multi-qubit gates, circuit decomposition, the Bloch sphere, and common circuit patterns (CNOT, Toffoli, Grover, QFT) is required for all technical roles.

Differentiated skills by role:

- Hardware roles: Cryogenics, microwave engineering, materials science, device fabrication, LabVIEW instrumentation.

- Algorithm roles: Computational complexity theory, abstract algebra, quantum error correction codes, mathematical proof writing.

- Software roles: Compiler design, graph algorithms, software architecture, performance optimisation, API design.

- Applications roles: Domain expertise (e.g., computational chemistry, quantitative finance), benchmarking methodology, technical communication.

## 10.7 Qiskit Developer Certification: Exam Guide and Preparation

<div class="box box-generic">

<img class="fig-img" src="content/images/image79.png" alt="">
</div>

**Figure 10.7: Qiskit Ecosystem Architecture and Qiskit Developer Certification Exam Structure**

Left: Qiskit ecosystem five-layer architecture. Layer 1 - Qiskit Terra (core, blue): the foundation layer providing the QuantumCircuit class, quantum gates, the transpiler pipeline, execution backends, and the quantum\_info module for density matrices, Pauli operators, and quantum channels. Layer 2 - Qiskit Aer (simulation, teal): high-performance quantum circuit simulation including the StatevectorSimulator, QasmSimulator, pulse simulation, and noise model integration with T1/T2 decoherence and depolarising errors. Layer 3 - Domain Libraries (green): Qiskit Nature (VQE, UCCSD, molecular drivers), Qiskit Finance (QAOA portfolio, QAE), Qiskit Optimization (QUBO, converters), Qiskit Machine Learning (QSVM, QNN). Layer 4 - Qiskit Runtime (purple): IBM cloud execution with Session management, Sampler primitive (probability distributions), Estimator primitive (expectation values), and error mitigation (ZNE, readout correction). Layer 5 - Transpiler & Pulse (orange): circuit compilation to hardware-native gate sets, routing for device connectivity, and pulse-level microwave control via OpenPulse. Right: Qiskit Developer Certification exam structure. Five topic areas with percentage weights: Quantum Circuits (20%, Qiskit QuantumCircuit, gates, statevectors, measurement), Executing Circuits (20%, backends, transpilation, Aer simulation, IBM Quantum job management), Quantum Information (15%, Bloch sphere, density matrices, entanglement, fidelity), Algorithms (30%, the highest weight -- Grover, QFT, QPE, VQE, QAOA, Bernstein-Vazirani, Deutsch-Jozsa, Simon), Noise and Mitigation (15%, noise models, T1/T2, ZNE, readout mitigation). Bottom bar: exam format -- 60 questions, 90 minutes, Pearson VUE proctored, passing score 60%.

### 10.7.1 Exam Format and Topics

The IBM Certified Associate: Qiskit Developer certification validates foundational competency in programming quantum circuits with Qiskit. It is the industry's most recognised quantum programming credential and is valued by employers including IBM, QC Ware, Multiverse Computing, and technology consultancies entering quantum.

Exam logistics: Administered by Pearson VUE (online proctored or test centre). 60 multiple-choice questions. 90-minute time limit. Passing score: 60% (36/60 questions). Cost: approximately $200 USD. The exam tests programming ability, not just conceptual knowledge -- expect code-reading questions where you must predict circuit output.

Exam topics in depth:

- Quantum Circuits (20%): Create and manipulate QuantumCircuit objects. Apply single-qubit gates (H, X, Y, Z, S, T, Rx, Ry, Rz, U). Apply multi-qubit gates (CNOT, CZ, SWAP, Toffoli, CX). Initialise states. Insert barriers. Measure qubits. Visualise circuits (circuit.draw()). Compose circuits.

- Executing Circuits (20%): Use QasmSimulator and StatevectorSimulator. Set shots. Retrieve results (job.result(), result.get\_counts()). Transpile circuits for specific backends. Use IBM Quantum backends. Understand Qiskit Runtime's Sampler and Estimator primitives.

- Quantum Information (15%): Compute statevectors. Understand the Bloch sphere representation of single-qubit states. Compute density matrices for pure and mixed states. Measure entanglement (concurrence, Schmidt rank). Compute state fidelity. Apply quantum channels.

- Algorithms (30%, highest weight): Implement Deutsch-Jozsa algorithm. Implement Bernstein-Vazirani algorithm. Implement Simon's algorithm. Implement Quantum Fourier Transform (QFT). Implement Quantum Phase Estimation (QPE). Implement Grover's search algorithm (oracle construction, diffuser). Implement Variational Quantum Eigensolver (VQE) at a conceptual level.

- Noise and Error Mitigation (15%): Understand depolarising noise model. Understand T1 and T2 decoherence. Create noise models in Qiskit Aer. Apply readout error mitigation (calibration matrices). Apply Zero-Noise Extrapolation (ZNE) at a basic level.

### 10.7.2 Study Resources and Preparation Strategy

Recommended preparation sequence (6-month plan for physics M.Sc. students):

- Months 1-2 (Foundation): Complete the IBM Quantum Learning platform (learning.quantum.ibm.com) -- specifically the "Basics of Quantum Information" and "Fundamentals of Quantum Algorithms" courses. Study Nielsen & Chuang "Quantum Computation and Quantum Information" Chapters 1-4. Run all code examples in Qiskit.

- Months 3-4 (Core Qiskit): Work through the Qiskit Textbook (qiskit.org/learn) chapters on circuits, measurements, and algorithms. Implement Deutsch-Jozsa, Bernstein-Vazirani, Simon, QFT, QPE, and Grover from scratch. Run them on both simulators and real IBM Quantum hardware. Study the Qiskit API documentation.

- Months 5-6 (Exam Preparation): Take the official IBM Quantum Learning practice exam. Review weak areas. Implement VQE for H2 (Qiskit Nature). Study noise models and ZNE in Qiskit Aer. Time yourself on practice questions. Aim for 80%+ on practice exams before attempting the real exam.

<div class="box box-key-concept">
<p class="box-title"><strong>💡  Top 5 Qiskit Exam Tips</strong></p>
<p>(1) Read code, not just concepts: ~50% of questions involve reading Qiskit code and predicting output. Practice extensively with actual code. (2) Know measurement outcomes: Understand that QuantumCircuit.measure() adds classical bits, and that result.get_counts() returns bitstrings in little-endian order (right qubit is qubit 0). (3) Understand transpilation: Know the difference between circuit.decompose(), transpile(), and PassManager. Know what layout and routing do. (4) Grover's algorithm is tested heavily: Know how to construct oracles for specific problems, how to build the diffuser, and what the optimal number of iterations is. (5) The Estimator vs Sampler distinction matters: Sampler returns quasi-probability distributions; Estimator returns expectation values of observables. Know when to use each.</p>
</div>

## 10.8 Building a Quantum GitHub Portfolio

<div class="box box-generic">

<img class="fig-img" src="content/images/image80.png" alt="">
</div>

**Figure 10.8: Quantum GitHub Portfolio Strategy -- Projects by Level and Repository Best Practices**

Four-level portfolio progression. Level 1 - Beginner Projects (teal, left column): Bell state and GHZ state circuit generation and visualisation; Grover's search for a 3-5 qubit database with oracle construction; Deutsch-Jozsa and Bernstein-Vazirani algorithm implementations; BB84 QKD protocol simulation with eavesdropping detection. These demonstrate quantum circuit basics and should be completed in the first 1-2 months of Qiskit study. Level 2 - Intermediate Projects (blue): VQE for H2 or LiH using Qiskit Nature with energy vs bond length curve; QAOA MaxCut on a 5-10 node random graph; Quantum Fourier Transform and Phase Estimation implementation with analysis; Quantum SVM implementation following sklearn interface conventions. Level 3 - Advanced Projects (purple): ADAPT-VQE benchmarking study comparing ansatz quality vs circuit depth; Portfolio QAOA using real NSE historical data for 6-10 assets; ZNE error mitigation pipeline benchmarked on real IBM Quantum hardware; Quantum repeater protocol simulation using realistic memory and gate models. Level 4 - Research-Level (green): Novel ansatz design with a writeup suitable for arXiv submission; Quantum ML benchmark suite comparing classical vs quantum on standard datasets; New algorithm implementation from a recent paper; Merged pull request to Qiskit, PennyLane, or Cirq. Bottom panel: repository structure guidelines and community strategy.

### 10.8.1 Project Ideas and Progressive Development

A strong quantum GitHub portfolio demonstrates both technical ability and understanding of the physics. Each project should include: (1) a clear README explaining the quantum physics motivating the project; (2) well-commented Qiskit code; (3) results comparing quantum vs classical solutions; (4) visualisations (circuit diagrams, energy landscapes, probability distributions); and (5) conclusions discussing limitations and potential improvements.

Beginner project guide -- Bell State and Quantum Teleportation:

- Create Bell pairs |Phi+> using H + CNOT. Visualise with Bloch sphere before and after.

- Implement quantum teleportation: prepare arbitrary |psi> on Alice's qubit, create shared Bell pair, perform Bell measurement, send classical bits, apply corrections on Bob's side.

- Verify by running on both Aer simulator (exact) and IBM Quantum hardware (with noise). Show the effect of noise on teleportation fidelity.

- Extend to 3-qubit GHZ state: show genuine tripartite entanglement via Mermin inequality violation.

Intermediate project guide -- QAOA for MaxCut:

- Generate a random weighted graph G=(V,E) with N=10 nodes. Formulate MaxCut as an Ising Hamiltonian H\_C = sum\_{(i,j) in E} w\_{ij} (1 - Z\_i Z\_j) / 2.

- Implement QAOA with p=1,2,3 layers. Optimise (beta,gamma) with COBYLA for each p. Plot <H\_C> vs iterations.

- Compare QAOA solution quality with classical greedy and semidefinite programming (SDP) solutions. Plot cut value vs p.

- Run on IBM Quantum hardware for p=1 with 10 qubits. Apply ZNE mitigation. Discuss hardware noise impact.

### 10.8.2 Repository Structure and Documentation

Good repository structure makes the difference between a portfolio that impresses employers and one that is ignored:

- README.md (most important file): Start with the physics motivation (one paragraph answering "why is this quantum problem interesting?"). Include a clear diagram or figure. Provide installation instructions (conda/pip). Show example usage with expected output. Include results and conclusions. Add references to papers.

- requirements.txt or pyproject.toml: Pin all dependencies with specific versions (qiskit==1.2.0, qiskit-aer==0.14.0, etc.). Include a conda environment file (environment.yml) for easy reproduction.

- notebooks/: Jupyter notebooks for exploratory analysis and result visualisation. One notebook per major result. Name clearly: "01\_circuit\_generation.ipynb", "02\_vqe\_h2\_results.ipynb".

- src/: Modular Python source code for reusable components. Separate circuits, optimisers, and analysis into different modules. Include docstrings for all functions.

- tests/: Unit tests using pytest. Test that circuits produce expected statevectors. Test that algorithms converge on known problems.

- results/: Pre-computed results for figures (so reviewers can reproduce plots without re-running QPU jobs). Include raw data files and a data README.

### 10.8.3 Community Engagement and Open-Source Contributions

Contributing to open-source quantum projects is the single most effective way to advance a quantum career. It demonstrates real software engineering skills, provides public evidence of competency, and builds relationships with leading researchers and engineers.

How to contribute to Qiskit:

- Start with documentation contributions (fixing typos, improving examples): these are always welcome and require no deep codebase knowledge.

- Tackle "good first issue" tagged GitHub issues in the qiskit-terra, qiskit-nature, or qiskit-finance repositories.

- Write new tutorials for the Qiskit Textbook or IBM Quantum Learning platform.

- Implement a recently published quantum algorithm in Qiskit and submit a pull request with tests and documentation.

- Report bugs with detailed reproduction code -- this is a valuable contribution even without a fix.

Community engagement platforms:

- Qiskit Slack (ibm.co/joinqiskitslack): Active community of 20,000+ developers. Ask questions, share projects, find collaborators.

- QHack (annual hackathon by Xanadu, typically February-March): 72-hour quantum computing hackathon with prizes and job visibility.

- IBM Quantum Network Hackathons: Annual events for Network member institutions.

- Qiskit Advocate Program: Apply after 6+ months of community contribution; provides mentoring, early hardware access, and IBM conference invitations.

- arXiv (quant-ph section): Read new papers weekly. Even without original research, a paper summary blog on Medium or LinkedIn builds community presence.

<div class="box box-generic">

<img class="fig-img" src="content/images/image81.png" alt="">
</div>

**Figure 10.9: Open-Source Quantum Computing Ecosystem -- Libraries, SDKs and Contribution Opportunities**

Overview of the open-source quantum computing software ecosystem organised by category. Quantum Computing SDKs (top row): Qiskit (IBM, most widely used; >500,000 monthly downloads; Python; gates + transpiler + runtime); Cirq (Google; Python; gate-model focus; strong on superconducting hardware); PennyLane (Xanadu; Python; differentiable quantum computing; QML focus); Q# (Microsoft; domain-specific language; full language + Azure integration); Amazon Braket SDK (Python; multi-hardware; AWS integration); Pytket (Quantinuum; Python + C++; compilation-focused; hardware agnostic). Quantum Chemistry Libraries (second row): OpenFermion (Google; fermionic-to-qubit mapping, Hamiltonians); PySCF-QC (interface between classical PySCF and quantum circuits); Tangelo (good-chemistry; drug discovery focus); and three additional chemistry packages. Quantum Error Correction Tools (third row): Stim (Google; fastest stabiliser code simulator; millions of shots per second); PyMatching (minimum weight perfect matching decoder); FusionBlossom, Plaquette, and Qiskit QEC. Finance and Optimisation (bottom row): D-Wave Ocean (annealing problems), Qiskit Finance (QAOA/QAE for finance), QC Ware Forge (commercial), Multiverse Computing, and others. Stars indicate community adoption level (three stars = highly popular). All libraries are freely available via pip install.

## 10.9 The Quantum Computing Roadmap: Looking to 2040

<div class="box box-generic">

<img class="fig-img" src="content/images/image82.png" alt="">
</div>

**Figure 10.10: Quantum Computing Roadmap 2024-2040: Physical Qubits, Logical Qubits and Gate Error Rate**

Dual-axis roadmap chart showing three key hardware metrics from 2024 to 2040. Blue line (left axis, log scale): physical qubit count trajectory from 1121 (IBM Condor, 2024) to an estimated 5 million (2040), following an approximate annual doubling trend. Gold dashed line (left axis): logical qubit count, near zero today (only early demonstrations), growing to 100 logical qubits (2030 target) and 20,000 (2040). Orange line (right axis): 2Q gate error rate declining from 0.15% (2024) to 0.01% (2030, approaching fault-tolerant threshold) and ultimately 0.001% (2040). Key milestone markers: IBM Condor 1121 qubits (2024, blue star); Google Willow chip with below-threshold error correction (2025); early fault-tolerant logical qubits from multiple groups (2027); 100 logical qubit target for first useful fault-tolerant calculations (2030); drug discovery QPE demonstration for a medium-sized molecule (2033); full FeMoco simulation via QPE (2035). Three era bands: NISQ era (2024-2028, teal), Early Fault-Tolerant era (2028-2033, purple), Full Fault-Tolerant era (2033+, green). The gold horizontal dashed line at 10^6 physical qubits marks the estimated fault-tolerant threshold for practically relevant RSA-breaking capabilities.

The quantum computing roadmap reveals three distinct eras, each with different implications for practitioners and students:

- NISQ Era (2024-2028): 100-10,000 physical qubits, no fault tolerance, error mitigation (ZNE, PEC) essential, gate error rates 0.1-0.01%. Applications: utility-scale simulations (IBM 2023 result extended), QAOA for small optimisation problems, VQE for small molecules, quantum ML research. Career focus: error mitigation algorithms, NISQ-compatible ansatze, hardware characterisation.

- Early Fault-Tolerant Era (2028-2033): 10,000-1,000,000 physical qubits, 10-500 logical qubits, first fault-tolerant algorithms, gate error rates 0.001-0.01%. Applications: QPE for small drug molecules, QAOA for larger optimisation problems, quantum-enhanced Monte Carlo for finance, small-scale Hubbard model simulation. Career focus: quantum error correction codes, logical gate compilation, fault-tolerant algorithm design.

- Full Fault-Tolerant Era (2033+): Millions of physical qubits, thousands of logical qubits, arbitrary fault-tolerant computation. Applications: FeMoco QPE, RSA cryptanalysis (Shor's), large-scale chemistry simulation, full 2D Hubbard model, Grover-accelerated database search. Career focus: algorithm implementation, domain-specific quantum advantage, quantum-classical hybrid architectures.

For students entering the quantum field in 2024: the NISQ era will define your first 5-7 years of professional work. Deep expertise in variational algorithms (VQE, QAOA), error mitigation, Qiskit programming, and quantum chemistry/finance applications will be highly valued throughout this period. The transition to fault-tolerant computing will require a second phase of learning (quantum error correction, fault-tolerant gate sets, logical qubit architectures) -- but the foundations built in the NISQ era will remain essential.

**RECAP**

*Chapter 10: Quantum Industry and Career Landscape — Short Answer Questions & Model Answers*

## Short Answer Questions — Chapter 10

*Instructions: Answer each question in 3–6 lines.*

**Q1.**  What is meant by the global quantum technology market?

*[§10.1 — The Global Quantum Market]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q2.**  Name the four major segments of the quantum technology market.

*[§10.1 — The Global Quantum Market]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q3.**  What is the role of quantum hardware companies?

*[§10.2 — The Quantum Industry Ecosystem]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q4.**  What are cloud quantum computing platforms?

*[§10.2 — The Quantum Industry Ecosystem]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q5.**  What is post-quantum cryptography (PQC)?

*[§10.3 — Post-Quantum Cryptography]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q6.**  Why is Shor’s algorithm considered a threat to classical cryptography?

*[§10.3 — Post-Quantum Cryptography]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q7.**  What is a Harvest-Now-Decrypt-Later attack?

*[§10.3 — Post-Quantum Cryptography]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q8.**  Name the major NIST PQC standards.

*[§10.3 — Post-Quantum Cryptography]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q9.**  What is India’s National Quantum Mission (NQM)?

*[§10.4 — India's National Quantum Mission (NQM)]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q10.**  Name the four major technology hubs under NQM.

*[§10.4 — India's National Quantum Mission (NQM)]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q11.**  What is the role of Qiskit in quantum computing?

*[§10.7 — Qiskit Developer Certification]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q12.**  What skills are important for a quantum software developer?

*[§10.6 — Quantum Career Skills: The Skill Matrix]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q13.**  What is meant by quantum utility?

*[§10.9 — The Quantum Computing Roadmap]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q14.**  What is Quantum Volume (QV)?

*[§10.6 — Quantum Career Skills: The Skill Matrix]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Q15.**  Why is open-source contribution important in a quantum computing career?

*[§10.8 — Building a Quantum GitHub Portfolio]*

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Model Answers — Chapter 10

**Answer 1:**

The global quantum technology market refers to the worldwide ecosystem of quantum hardware, software, services, and security technologies.

**Answer 2:**

The four major segments are quantum hardware, quantum software and cloud platforms, quantum services and consulting, and post-quantum cryptography.

**Answer 3:**

Quantum hardware companies develop physical quantum computers and supporting infrastructure such as cryogenic systems and control electronics.

**Answer 4:**

Cloud quantum computing platforms provide remote access to quantum processors through the internet.

**Answer 5:**

Post-quantum cryptography consists of cryptographic algorithms designed to remain secure against attacks from quantum computers.

**Answer 6:**

Shor’s algorithm can factor large integers and solve discrete logarithm problems efficiently, threatening RSA and ECC cryptosystems.

**Answer 7:**

In a Harvest-Now-Decrypt-Later attack, encrypted data is collected today and stored until future quantum computers become capable of decrypting it.

**Answer 8:**

Major NIST PQC standards include CRYSTALS-Kyber, CRYSTALS-Dilithium, and SPHINCS+.

**Answer 9:**

India’s National Quantum Mission is a government initiative launched to develop quantum technologies, infrastructure, research hubs, and skilled manpower.

**Answer 10:**

The four hubs are QuST, QuCryptoS, QuNAT, and QuMAT.

**Answer 11:**

Qiskit is an open-source quantum software development kit used for creating, simulating, and executing quantum circuits.

**Answer 12:**

Important skills include Python programming, linear algebra, quantum algorithms, quantum hardware understanding, and software development.

**Answer 13:**

Quantum utility refers to the stage where quantum computers perform useful tasks beyond the practical capabilities of classical systems for specific applications.

**Answer 14:**

Quantum Volume is a performance metric that measures the effective computational capability of a quantum computer by combining qubit count, connectivity, and gate fidelity.

**Answer 15:**

Open-source contribution helps students gain practical experience, showcase projects, collaborate with the quantum community, and improve employability.

## Solved Examples — Chapter 10

### Example 10.1 Quantum Market Segment Sizing

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>In 2024, the global quantum technology market has hardware at $2.1B, software at $1.2B, services at $0.8B, and PQC at $1.5B. (a) What is the total market? (b) What percentage is PQC? (c) If PQC grows at 55% CAGR to 2030, what will its 2030 value be?</p>
</div>

**Solution:**

(a) Total = 2.1 + 1.2 + 0.8 + 1.5 = $5.6 billion (2024)

(b) PQC percentage = 1.5/5.6 = 26.8% of total market

This explosive growth reflects the urgency of the migration mandate for all RSA-dependent systems.

Note: the PQC market grows faster than hardware because: (1) it affects ALL encrypted systems (not just QC adopters); (2) regulatory mandates create non-optional demand; (3) migration is required even before a quantum threat materialises.

### Example 10.2 Shor's Algorithm Resource Estimate

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>Shor's algorithm for factoring an n-bit RSA key requires O(n^3) quantum gates. (a) Compare the number of quantum gates needed for RSA-1024 vs RSA-2048. (b) If RSA-2048 requires approximately 10^10 Toffoli gates on a fault-tolerant quantum computer with a logical Toffoli gate time of 10 microseconds, how long does it take to break RSA-2048?</p>
</div>

**Solution:**

(a) Gates scale as n^3: RSA-1024 requires (1024)^3 ~ 1.07 x 10^9 gates.

RSA-2048 requires (2048)^3 ~ 8.59 x 10^9 gates.

Ratio:

(b) Time = number of gates x gate time (in series)

= 10^10 gates x 10 x 10^{-6} s/gate = 10^5 seconds = 28 hours

This assumes serial gate execution; parallelism reduces this further.

Implication: A fault-tolerant quantum computer could break RSA-2048 in approximately 1 day.

Modern estimates (Banegas et al. 2021): 4.2 billion physical qubits, 4300 logical qubits, ~10 hours.

### Example 10.3 CRYSTALS-Kyber Key Size Comparison

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>Compare CRYSTALS-Kyber-768 with RSA-2048 for key encapsulation. Kyber-768 has: public key 1184 bytes, secret key 2400 bytes, ciphertext 1088 bytes. RSA-2048 has: public key 256 bytes, ciphertext 256 bytes. (a) What is the size overhead of Kyber vs RSA? (b) For HTTPS, where a TLS 1.3 handshake transmits one public key and one ciphertext, what is the bandwidth overhead?</p>
</div>

**Solution:**

(a) Public key overhead: Kyber 1184 / RSA 256 = 4.6x larger

Ciphertext overhead: Kyber 1088 / RSA 256 = 4.25x larger

(b) TLS 1.3 handshake (PQ KEM): public key + ciphertext

RSA-2048: 256 + 256 = 512 bytes

Kyber-768: 1184 + 1088 = 2272 bytes

Bandwidth overhead: 2272/512 = 4.44x more bandwidth for Kyber vs RSA

In practice: 2272 - 512 = 1760 extra bytes per TLS handshake.

At 100 Mbps connection, 1760 bytes = 0.14 ms overhead. For most applications: negligible.

For high-frequency trading (microsecond latency requirements), PQC migration requires careful optimisation.

### Example 10.4 NQM Budget Allocation

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>India NQM total budget is Rs.6003 crore over 8 years (2023-2031). (a) Average annual budget in Rs. crore. (b) Average annual budget in USD (assuming 1 USD = Rs.83). (c) Compare with the US National Quantum Initiative annual budget. (d) How many Ph.D. stipends at Rs.35,000/month could the QuST hub (Rs.1800 crore) fund over 8 years?</p>
</div>

**Solution:**

(a) Annual NQM budget = 6003 / 8 = Rs.750.4 crore per year

(b) In USD: Rs.750.4 crore / 83 = $90.4 million per year

(c) US NQI annual funding ~ $900 million/year (FY2024, including agency programmes).

India NQM is approximately $90M/$900M = 10% of US quantum investment -- significant but substantially smaller.

Context: India's R&D spending is ~0.7% of GDP vs USA's 3.5%, so NQM at $90M/year represents a relatively large commitment.

(d) QuST budget = Rs.1800 crore over 8 years = Rs.225 crore/year

Ph.D. stipend + overhead ~ Rs.35,000/month x 12 = Rs.4.2 lakh/year, plus Rs.1 lakh overhead = Rs.5.2 lakh/student/year

Number of Ph.D. students = Rs.225 crore / Rs.5.2 lakh = 4327 equivalent student-years.

If each Ph.D. takes 5 years: QuST could fund ~865 Ph.D. students simultaneously in steady state.

### Example 10.5 Qiskit Certification Preparation Planning

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>An M.Sc. Physics student with Python proficiency (intermediate) and quantum mechanics background (good) wants to pass the Qiskit Developer exam in 6 months. Design a weekly study plan specifying: (a) resources, (b) weekly hours commitment, (c) key milestones.</p>
</div>

**Solution:**

Recommended 6-month plan (assuming 10 hours/week):

Months 1-2 (Foundations, 80 hours): IBM Quantum Learning courses "Basics of Quantum Info" + "Fundamentals of Algorithms" (40h). Qiskit Textbook Chapters 1-4 (20h). Practice: implement circuits from every chapter, run on IBM Quantum hardware (20h).

Milestone: Can build any single-qubit and basic multi-qubit circuit, run on simulator and real hardware.

Months 3-4 (Algorithms, 80 hours): Implement Deutsch-Jozsa, Bernstein-Vazirani, Simon, QFT, QPE, Grover from scratch (50h). Run all on simulator + hardware. Implement VQE for H2 using Qiskit Nature (30h).

Milestone: Can implement all standard algorithms without referring to documentation.

Months 5-6 (Exam Prep, 80 hours): Complete all Qiskit API documentation (30h). Practice 200+ exam-style questions (IBM practice exam + community resources) (30h). Review noise models, ZNE in Aer (20h).

Milestone: Consistently scoring 80%+ on practice exams.

Total: 240 hours over 6 months = 10 hours/week. Adjust based on background.

### Example 10.6 GitHub Portfolio Career ROI

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A student has 3 months to build a quantum GitHub portfolio before job applications. They can invest 15 hours/week. (a) How many projects can they realistically complete? (b) Rank the following by ROI for job applications: (i) 10 beginner circuits, (ii) 1 complete VQE project with hardware results, (iii) 1 merged PR to Qiskit, (iv) 5 intermediate projects.</p>
</div>

**Solution:**

(a) Projects per 3 months at 15 hours/week = 195 total hours.

Beginner project: 5-10 hours each; Intermediate: 20-40 hours each; Advanced: 60-100 hours each.

Realistic: 2-3 intermediate projects OR 1 advanced project + 2 beginner projects.

(b) ROI ranking for job applications (highest first):

1. One merged PR to Qiskit (best): demonstrates actual engineering skill to the community; permanent public record; referenced by Qiskit team; signals both skill and initiative. ~20-30 hours for a substantive contribution.

2. One complete VQE project with hardware results: shows end-to-end competency (chemistry motivation, Qiskit Nature, optimiser, hardware run, ZNE, analysis). ~40 hours.

3. Five intermediate projects: breadth demonstrates versatility and sustained effort. ~100-150 hours.

4. Ten beginner circuits: low ROI because any student can do this; demonstrates basics but not depth.

Recommendation: 1 Qiskit PR + 1 VQE project + 2 intermediate projects = highest ROI for 195 hours.

### Example 10.7 Post-Quantum Security Timeline for Indian Banks

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A large Indian bank has: 5 million RSA-2048 encrypted customer records; 200,000 TLS connections/day using ECDH-256; 10 million RSA-signed transactions/year. (a) Estimate the data volume at HNDL risk. (b) What is the migration timeline urgency given a 10-year Q-Day estimate? (c) What is the estimated PQC migration cost?</p>
</div>

**Solution:**

(a) HNDL risk estimate:

Customer records: RSA-2048 encrypted data -- if stored for 30 years, all are at risk immediately.

TLS connections: 200,000/day x 365 = 73 million connections/year. Each connection has session keys that protect 1 MB of data on average = 73 TB/year of transaction data at HNDL risk.

Transactions: 10 million RSA signatures/year -- authentication data at risk if records kept >10 years.

(b) Timeline urgency (Q-Day = 2035 estimate):

Migration time typically 5-7 years for a large bank (assessment, procurement, testing, deployment, certification).

To complete by 2035: must start NOW (2024-2025). This is exactly the message from RBI and CERT-In guidelines.

(c) PQC migration cost estimate (industry benchmark: $50-200M for a large global bank):

For a mid-size Indian bank: (1) HSM upgrades: 500 HSMs x $10,000 = $5M; (2) TLS library updates: 200 servers x $5,000 staff cost = $1M; (3) PKI infrastructure overhaul: $3-8M; (4) Testing and certification: $2-5M.

Total estimate: $11-19M for migration. Annual PQC licensing: $500K-2M ongoing.

### Example 10.8 Quantum Career Salary Negotiation

<div class="box box-solved-problem">
<p class="box-title"><strong>📝  Problem</strong></p>
<p>A student completes an M.Sc. in Physics with Qiskit Developer Certification and a GitHub portfolio including one VQE project and one QAOA project. They receive two offers: (A) IIT-NQM hub postdoc: Rs.70,000/month + Rs.50,000 annual travel grant; (B) TCS Quantum Practice: Rs.12L/year base + 20% performance bonus potential. (a) Which offers more compensation in year 1? (b) After 5 years, project likely trajectories. (c) What non-monetary factors should they consider?</p>
</div>

**Solution:**

(a) Year 1 comparison:

Offer A (NQM Postdoc): Rs.70,000/month x 12 = Rs.8.4L + Rs.0.5L travel = Rs.8.9L/year = ~$10,700/year

Offer B (TCS Quantum): Rs.12L base + 20% bonus target = Rs.14.4L/year = ~$17,300/year

Offer B pays 62% more in year 1.

(b) Five-year trajectories:

Offer A: Postdoc typically leads to academic or senior research position in 2-3 years. After 5 years: likely Research Scientist at NQM hub or faculty position at Rs.1.2-2L/month (Rs.14-24L/year). But 2 more years of postdoc + faculty job search uncertainty.

Offer B: TCS to senior consultant/specialist in 3-5 years. After 5 years at TCS Quantum: Rs.25-40L/year. Potential to move to IBM Consulting or QC Ware at Rs.50-80L.

Industry pays more at all career stages in India currently.

(c) Non-monetary considerations: NQM postdoc offers publication record, academic freedom, long-term research depth, international conference exposure, and positions for eventual faculty role. TCS offers commercial exposure, client-facing skills, and faster skills breadth. Recommend: take TCS if financial stability is priority; take postdoc if research/academia is the long-term goal.

## Multiple Choice Questions — Chapter 10

Note: Answers are collected at the end of this chapter.

**1. Shor's algorithm threatens RSA encryption because it can factor N-bit integers in:**

(A) O(N^2) time classically, making RSA computationally feasible for large N

(B) O(N^3) quantum gate operations, an exponential speedup over classical O(exp(N^{1/3})) algorithms

(C) O(2^N) quantum operations, the same as a brute-force search

(D) O(N log N) operations using the same principle as the Fast Fourier Transform

**2. The "harvest-now-decrypt-later" attack on RSA-encrypted data is concerning TODAY because:**

(A) Current classical computers are already fast enough to break RSA-2048 given sufficient time

(B) Adversaries intercept and store encrypted data now, planning to decrypt it when fault-tolerant quantum computers become available, meaning long-lived secrets are already at risk

(C) Quantum computers have already broken RSA-1024 in laboratory conditions

(D) RSA-2048 keys expire after 5 years, so all currently encrypted data will become vulnerable at key rotation time

**3. CRYSTALS-Kyber (FIPS 203) is selected as a NIST PQC standard for key encapsulation because:**

(A) It is based on the same lattice problem (LWE) as RSA, providing a smooth migration path

(B) It is based on the Module Learning With Errors (M-LWE) lattice problem, which is believed hard for both classical and quantum computers, while providing efficient key generation and small ciphertext sizes

(C) It relies solely on the security of SHA-3 hash functions, making it the most conservative choice

(D) It achieves smaller key sizes than RSA-2048 while providing 256-bit classical security

**4. India's National Quantum Mission (NQM) budget of Rs.6003 crore is distributed across four hubs. Which hub receives the LARGEST budget allocation?**

(A) QuCryptoS (Quantum Communications) at Rs.1800 crore, reflecting QKD deployment costs

(B) QuST (Quantum Computing and Simulation) at Rs.1800 crore, reflecting the priority of building indigenous quantum computers

(C) QuNAT (Quantum Sensing) at Rs.2000 crore, reflecting defence and navigation applications

(D) QuMAT (Quantum Materials) at Rs.1800 crore, reflecting semiconductor fabrication requirements

**5. In the Qiskit Developer Certification exam (60 questions, 90 minutes, 60% passing score), which topic area carries the HIGHEST weight?**

(A) Quantum Circuits at 30%, because writing circuits is the most fundamental Qiskit skill

(B) Quantum Algorithms at 30%, because implementing Grover, QFT, QPE, VQE, and QAOA demonstrates both quantum understanding and Qiskit proficiency

(C) Noise and Error Mitigation at 30%, because NISQ-era applications require noise awareness

(D) Executing Circuits at 30%, because knowledge of IBM Quantum backends and job management is essential

**6. A Quantum Hardware Engineer role primarily requires which combination of skills?**

(A) Python programming, financial modelling, and QAOA algorithm implementation

(B) Qubit fabrication techniques, cryogenic systems engineering, microwave electronics, and device physics characterisation

(C) Compiler design, graph algorithms, REST API development, and cloud platform integration

(D) Quantum error correction codes, mathematical proof writing, and computational complexity theory

**7. The IBM Quantum Network's commercial significance is that it:**

(A) Provides free unlimited quantum computing resources to all universities worldwide

(B) Connects over 400 organisations including universities, national labs, and Fortune 500 companies with IBM Quantum hardware access, research collaboration, and co-development of quantum applications

(C) Competes directly with AWS Braket and Azure Quantum by offering the lowest per-shot pricing

(D) Restricts access to IBM Quantum hardware exclusively to IBM Research employees and NQM hub institutions

**8. The "utility era" in quantum computing (IBM 2023) is defined as:**

(A) The period when quantum computers achieve fault-tolerant operation with logical error rates below 10^{-6}

(B) The period when quantum computers produce scientifically useful results beyond easy classical simulation, even before full fault-tolerance, enabled by error mitigation techniques

(C) The period when quantum computing becomes commercially profitable for at least one Fortune 500 company

(D) The period when NISQ devices achieve quantum volume above 10^6, enabling broad application deployment

**9. SPHINCS+ (FIPS 205) is particularly well-suited for long-term archival applications because:**

(A) It has the smallest key sizes of all NIST PQC standards, minimising storage overhead

(B) Its security relies only on the collision resistance of hash functions (SHA-3), requiring no lattice or code-based assumptions -- providing maximum long-term security assurance with minimal mathematical risk

(C) It is a key encapsulation mechanism that provides perfect forward secrecy for archived data

(D) It is stateful (unlike XMSS), making it easy to implement in systems that lack persistent state

**10. A "good first contribution" strategy for contributing to Qiskit on GitHub should start with:**

(A) Rewriting the core transpiler in Rust for performance improvement

(B) Documentation improvements, fixing typos, improving code examples, or implementing a small feature tagged "good first issue" on GitHub

(C) Forking the entire repository and creating a competing quantum SDK

(D) Reporting theoretical bugs in the quantum error correction framework based on published papers

**11. CRYSTALS-Dilithium (FIPS 204) differs from CRYSTALS-Kyber (FIPS 203) in that:**

(A) Dilithium is a hash-based signature scheme while Kyber is a lattice-based key encapsulation mechanism

(B) Dilithium is a digital signature algorithm for authentication and code signing, while Kyber is a key encapsulation mechanism for establishing session keys; both are lattice-based

(C) Dilithium provides 512-bit quantum security while Kyber provides only 128-bit quantum security

(D) Dilithium replaces symmetric encryption (AES) while Kyber replaces public-key encryption (RSA)

**12. In the NQM budget, the QuCryptoS hub (Quantum Communications) focuses primarily on:**

(A) Developing novel quantum error correction codes for superconducting qubit processors

(B) QKD system development, QRNG device certification, post-quantum cryptography R&D, and the indigenous quantum satellite (Q-Sat)

(C) Theoretical quantum information and quantum computing algorithm research

(D) Quantum materials and superconducting qubit fabrication for the indigenous quantum computer

**13. For a Quantum Applications Scientist role at a quantum finance startup, the most valuable differentiating qualification (beyond the degree) is:**

(A) A Ph.D. in hardware physics with clean-room experience

(B) Combined Qiskit Developer Certification plus domain expertise (e.g., CFA or relevant quantitative finance knowledge) plus demonstrated QAOA/QAE implementations in a GitHub portfolio

(C) A compiled language programming background (C++/Rust) for performance-critical circuit simulation

(D) A pure mathematics Ph.D. with focus on number theory and cryptographic hardness proofs

**14. The NIST PQC competition selected lattice-based algorithms (Kyber, Dilithium, FALCON) as the primary standards because:**

(A) Lattice problems are proven NP-hard, providing information-theoretic security guarantees

(B) Lattice problems (LWE, NTRU) have resisted decades of classical and quantum cryptanalysis, offer the best balance of key/signature sizes and computational efficiency, and benefit from rich mathematical theory

(C) Lattice-based algorithms can be implemented using the same hardware as RSA, minimising migration costs

(D) NIST exclusively selected lattice algorithms and rejected all hash-based and code-based candidates

**15. A quantum computing roadmap predicts 100 logical qubits will be achievable by 2030. The significance for quantum chemistry is that:**

(A) 100 logical qubits can simulate any molecule on Earth exactly via Quantum Phase Estimation

(B) 100 logical qubits would allow fault-tolerant QPE for medium-sized drug molecules (20-50 orbital active spaces) that are beyond CCSD(T) but too small for full pharmaceutical relevance -- an important stepping stone

(C) 100 logical qubits is sufficient to break RSA-2048 using Shor's algorithm

(D) 100 logical qubits can only simulate H2 and LiH, the same as current NISQ VQE approaches

## MCQ Answers — Chapter 10

| Q1 | B | Q2 | B | Q3 | B | Q4 | B | Q5 | B |
|---|---|---|---|---|---|---|---|---|---|
| Q6 | B | Q7 | B | Q8 | B | Q9 | B | Q10 | B |
| Q11 | B | Q12 | B | Q13 | B | Q14 | B | Q15 | B |

## Unsolved Problems — Chapter 10

10.1  The global PQC market was $1.5B in 2024. (a) At 55% CAGR, calculate the 2025, 2027, and 2030 market values. (b) Compare the 2030 PQC market with the 2030 hardware market ($14.5B projected). (c) Explain economically why PQC grows faster than quantum hardware.

[Ans: (a) 2025: $1.5x1.55=$2.33B; 2027: $1.5x1.55^3=$5.58B; 2030: $1.5x1.55^6=$31.9B. (b) PQC 2030 ($31.9B) >> Hardware 2030 ($14.5B); PQC is 2.2x the hardware market by 2030. (c) PQC addresses ALL encrypted systems (billions of devices); hardware addresses only QC adopters; PQC is regulatory-mandated while hardware is discretionary investment.]

10.2  Shor's algorithm requires approximately n^3 quantum operations for n-bit RSA. (a) How many gates for RSA-512? RSA-1024? RSA-4096? (b) If fault-tolerant gates cost $0.001 per gate-second (hypothetical), what is the cost to break RSA-2048 at 10^10 gates x 1 microsecond/gate? (c) Why does this economic analysis suggest nation-state actors are the primary Q-Day threat?

[Ans: (a) RSA-512: 512^3=1.34x10^8; RSA-1024: 1.07x10^9; RSA-4096: 6.87x10^10 gates. (b) Cost = 10^10 x 10^{-6} s x $0.001/gate-s = $10. At fault-tolerant scale RSA breaking will be essentially free, making cryptographic migration urgently necessary. (c) Initial quantum computers will be expensive (billions USD to build); only nation-states with strategic intelligence motives can justify this cost; later, costs drop.]

10.3  NQM QuST hub has Rs.1800 crore over 8 years. (a) Annual budget in USD. (b) Estimate the cost of one 50-qubit superconducting quantum processor (including dilution refrigerator, electronics, infrastructure) at approximately $3M per system. How many complete systems can QuST fund? (c) Compare with IBM's quantum R&D spending (~$1.5B/year reported). What does this imply?

[Ans: (a) Rs.1800Cr/8 = Rs.225Cr/year = $27M/year. (b) $27M/$3M per system = 9 complete systems per year in principle (unrealistic -- must also fund salaries, consumables, facilities, software). Realistic: 2-3 systems plus extensive personnel and operations. (c) IBM spends $1.5B/year vs QuST $27M: IBM is 55x larger. India needs strategic focus on differentiating areas (applications, algorithms, materials) rather than competing on hardware count.]

10.4  A student passes the Qiskit Developer exam with score 72/100. (a) What is the pass/fail outcome? (b) The exam has 5 topic areas with weights 20/20/15/30/15%. If the student scored 90% on Algorithms (30%), 60% on Circuits (20%), 50% on Executing (20%), 70% on Quantum Info (15%), and 80% on Noise (15%), verify the total score. (c) What should the student study to improve to 80%+ overall?

[Ans: (a) Pass threshold = 60% = 36/60 questions. 72/100 = 43.2/60 equivalent. Pass. (b) Weighted score = 0.20x60 + 0.20x50 + 0.15x70 + 0.30x90 + 0.15x80 = 12 + 10 + 10.5 + 27 + 12 = 71.5% = 71.5/100. (c) To reach 80%: Executing Circuits (currently 50%) needs most improvement -- study transpilation, Aer backends, Sampler/Estimator primitives. Circuits (60%) also needs work -- more circuit composition practice.]

10.5  A student has 200 GitHub repository stars across 8 projects. (a) What is the average stars per project? (b) If one VQE project has 150 stars, what is the average of the remaining 7 projects? (c) GitHub analytics show 85% of views come from the VQE project. Why does this support a "fewer high-quality projects" strategy over a "many mediocre projects" strategy?

[Ans: (a) 200/8 = 25 stars average per project. (b) (200-150)/7 = 7.1 stars average for others. (c) Power-law attention distribution: 85% of value from 1 high-quality project (75% of stars). Employers review top-1 or top-2 repositories; breadth below 3 good projects adds little value. Quality >> quantity for portfolio ROI.]

10.6  IBM Heron processor has 133 qubits and 2Q gate fidelity 99.9%. (a) What is the 1-qubit gate error rate (typically 10x better than 2Q fidelity)? (b) For a VQE circuit with 50 qubits, 200 two-qubit gates, and 400 single-qubit gates, what is the raw circuit fidelity without error mitigation? (c) With ZNE reducing effective error by 5x, what is the corrected observable fidelity?

[Ans: (a) 1Q error = 0.1%/10 = 0.01% = 10^{-4}. (b) Raw fidelity = (1-0.001)^{200} x (1-0.0001)^{400} = 0.819 x 0.961 = 0.787. (c) ZNE reduces error rate by 5x: effective 2Q error = 0.002%; 1Q error = 0.002%. ZNE fidelity = (0.9998)^{200} x (0.99998)^{400} = 0.961 x 0.992 = 0.954.]

10.7  Kyber-768 has a public key of 1184 bytes. Compare with RSA-2048 (256 bytes). (a) Extra data per HTTPS connection handshake (one public key exchange). (b) A web server handles 10,000 HTTPS connections per second. Extra bandwidth per second for PQC migration. (c) At 1 Gbps uplink bandwidth, what percentage of bandwidth is consumed by this extra PQC data?

[Ans: (a) Extra per handshake = 1184 - 256 = 928 bytes. (b) Extra per second = 928 bytes x 10,000 = 9.28 MB/s. (c) 9.28 MB/s = 74.2 Mbps. Percentage = 74.2/1000 = 7.4%. Non-trivial for high-traffic servers but manageable with bandwidth upgrades. For most servers: negligible.]

10.8  The QHack hackathon is a 72-hour quantum computing challenge. (a) If a team of 3 students has 72 hours total, what is the effective person-hours? (b) QAOA for MaxCut on N=15 nodes requires a 15-qubit circuit with O(p\*E) gates where E=30 edges and p=2. How many 2-qubit gates? (c) If the team uses IBM Quantum with 10 minutes of free compute at 900,000 shots/second, and each circuit run needs 1024 shots, how many distinct circuits can they test?

[Ans: (a) 72 hours x 3 people = 216 person-hours. (b) Cost layer: E=30 ZZ gates per layer x p=2 = 60 ZZ gates = 60 two-qubit gates for the cost unitaries. Mixer: 15 Rx gates per layer x p=2 = 30 single-qubit gates. Total 2Q gates = 60. (c) Total shots = 10 min x 60 s/min x 900,000 shots/s = 540,000,000 shots. Circuits = 540,000,000 / 1024 shots/circuit = 527,344 circuits. Sufficient for hyperparameter search.]

10.9  India plans a 2000 km Delhi-Bangalore quantum-secured fibre network under NQM. (a) At fibre attenuation 0.2 dB/km, what is the total link loss? (b) At 0.2 dB/km, the maximum QKD distance without repeaters is ~200 km. How many trusted relay nodes are needed for 2000 km? (c) If each relay station costs Rs.5 crore (infrastructure + QKD systems), what is the relay station capital cost?

[Ans: (a) Total loss = 0.2 x 2000 = 400 dB -- completely opaque without repeaters. (b) Segments = 2000/200 = 10 segments; relay nodes = 10 - 1 = 9 relay nodes needed. (c) Capital cost = 9 nodes x Rs.5 crore = Rs.45 crore. Within the QuCryptoS Rs.1440 crore budget, relay infrastructure is a small fraction.]

10.10  Compare career trajectories for two quantum computing professionals after 10 years: (A) Academic-track (B.Tech + Ph.D. 5yr + Postdoc 2yr = 7 years, then faculty position at IIT); (B) Industry-track (B.Tech + M.Sc. 2yr + TCS Quantum 8yr, reaching Principal Consultant). (a) Estimate year-10 salary (INR) for each. (b) Estimate total lifetime earnings to age 40 from the start of their career for each. (c) Identify two non-monetary advantages of each track.

[Ans: (a) Academic (year 10 = faculty IIT): Rs.1.5-2L/month = Rs.18-24L/year. Industry (year 10 = Principal Consultant): Rs.80L-1.5Cr/year. Industry wins on salary by 4-6x at year 10. (b) Academic: postdoc Rs.70K/month x 24 months + faculty rising from Rs.75K to Rs.1.5L = cumulative ~Rs.1.2-1.5Cr at year 10. Industry: TCS rising from Rs.12L to Rs.80L over 8 years, cumulative ~Rs.3-4Cr. (c) Academic advantages: intellectual freedom, publication record, global reputation, sabbaticals. Industry advantages: salary, commercial impact, team scale, product ownership.]

## Theory Questions — Chapter 10

- 1.  Explain Shor's algorithm at a high level: what mathematical problem does it solve, what is its quantum complexity class, and why does it threaten RSA, ECC, and Diffie-Hellman simultaneously? Calculate precisely how many logical qubits are needed to run Shor's algorithm against RSA-2048 at chemical precision using the best known quantum circuit constructions.

- 2.  Describe the harvest-now-decrypt-later threat model. What types of data are most at risk? Why does the 10-20 year Q-Day estimate NOT mean we have 10-20 years to address the threat? Design a risk prioritisation framework that a CTO could use to rank which systems to migrate to PQC first.

- 3.  Compare CRYSTALS-Kyber (FIPS 203), CRYSTALS-Dilithium (FIPS 204), and SPHINCS+ (FIPS 205) in terms of: (a) mathematical hardness assumption; (b) key and signature sizes; (c) primary use cases; (d) implementation challenges. For each, explain what attack would break the scheme and why that attack is believed to be hard for quantum computers.

- 4.  Describe the structure of India's National Quantum Mission: governance, four hubs, budget allocation, and 2031 targets. Compare the NQM quantum computing targets (50-1000 qubit processors) with IBM's current capabilities (1121 qubits, Heron). What are the three most critical technical milestones India must achieve to realise the QuST hub goals?

- 5.  Compare four quantum career pathways (Hardware Engineer, Algorithm Researcher, Software Developer, Applications Scientist) using a structured analysis covering: required educational background, daily job responsibilities, key technical skills, typical employers (India and global), salary trajectory from entry to senior, and long-term career ceiling. Which pathway has the best prospects in India by 2030 and why?

- 6.  Explain the Qiskit ecosystem layer structure (Terra, Aer, domain libraries, Runtime, Pulse). For each layer, describe: what it does, what Python classes/functions are most important to know for the Qiskit certification exam, and one practical task you would use that layer for. Then design a VQE experiment for H2 specifying which Qiskit layers and specific functions you would use at each step.

- 7.  Design a 6-month quantum GitHub portfolio strategy for a final-year M.Sc. Physics student aiming for a Quantum Applications Scientist role at a quantum finance startup. Specify: (a) exactly 5 projects with physics motivation, Qiskit implementation plan, and expected deliverables; (b) estimated time allocation per project; (c) community engagement plan (conferences, Slack, open-source contributions); (d) how to write the project README to maximise employer appeal.

- 8.  Evaluate the quantum technology readiness of three sectors in India for 2024-2028: (a) banking and financial services (QKD migration, PQC mandate, quantum risk modelling); (b) defence and aerospace (quantum sensing, secure comms, quantum-enhanced navigation); (c) pharmaceuticals (quantum chemistry for drug discovery). For each sector, assess the timeline to commercial quantum adoption, key Indian institutions involved, and one specific quantum application that could be demonstrated within 5 years.

- 9.  Analyse the competitive landscape for quantum computing globally. Compare China, USA, EU, and India on: (a) government investment; (b) number of leading quantum hardware companies; (c) quantum workforce (Ph.D. graduates per year in quantum-related fields); (d) quantum patent filings. What are India's comparative advantages and disadvantages? What strategy would you recommend for the NQM to maximise India's competitive position?

- 10.  Critically evaluate the claim that the quantum computing industry will undergo 'quantum winter' (period of reduced investment and interest) before delivering commercial value. Argue both for and against this possibility, citing: (a) the history of AI winters and AI spring as an analogy; (b) current investment levels and funding trajectory; (c) quantum utility result 2023 as evidence for or against winter; (d) PQC mandate as a quantum-proof revenue source regardless of QC timelines.

## Assignments — Chapter 10

### Assignment 10.1 PQC Migration Assessment (Marks: 10)

Perform a post-quantum cryptography migration assessment for a hypothetical Indian university: (a) Inventory 5 cryptographic systems currently using RSA or ECC (VPN gateway, web server TLS, email signing, VoIP encryption, storage encryption). For each, identify: current algorithm, key size, data confidentiality lifetime, and HNDL risk level. (b) Using NIST PQC standards, recommend specific replacement algorithms (Kyber-768, Dilithium-3, or SPHINCS+-256) for each system with justification. (c) Estimate migration cost (staff time, software licensing, testing) for each system. (d) Create a 3-year migration roadmap prioritised by risk. Submit a 2000-word technical report.

### Assignment 10.2 Quantum Career Development Plan (Marks: 10)

Create a personalised quantum career development plan: (a) Select one of the four quantum career pathways (Hardware/Algorithm/Software/Applications). Justify your choice based on your current skills, educational background, and career goals. (b) Use the skill matrix (Figure 10.6) to identify your 3 strongest and 3 weakest relevant skills. Design a 12-month skill development plan addressing the weakest skills. (c) Identify 5 target employers (India or global) for this career pathway with job description analysis. (d) Plan your Qiskit Developer Certification preparation: create a 6-month study schedule. (e) Design a 5-project GitHub portfolio with physics motivation, Qiskit implementation, and expected deliverables for each. Submit a 2000-word plan plus a 1-page portfolio visualisation.

### Assignment 10.3 India NQM Impact Assessment (Marks: 10)

Write a 2500-word policy analysis of India's National Quantum Mission: (a) Compare NQM goals with the quantum technology programmes of China (National Quantum Initiative), USA (NQIA 2018), and EU (Quantum Flagship) on: budget, focus areas, timeline, and governance structure. (b) Assess the feasibility of three specific NQM targets: (i) 50-qubit indigenous quantum computer by 2026; (ii) 2000 km QKD network by 2029; (iii) indigenous quantum satellite Q-Sat by 2028. For each, discuss technological readiness, institutional capacity, and supply chain constraints. (c) Identify three specific recommendations for strengthening NQM implementation, supported by evidence from comparable international programmes. Include a structured comparative table.

## References and Further Reading

- 1.  McKinsey & Company (2021). Quantum technology: See who is preparing now. McKinsey Digital. https://www.mckinsey.com/quantum

- 2.  Boston Consulting Group (2022). The coming quantum leap in computing. BCG Henderson Institute.

- 3.  IDC (2023). Worldwide Quantum Computing Forecast 2023-2027. International Data Corporation.

- 4.  NIST (2024). Post-Quantum Cryptography Standards. FIPS 203, 204, 205. https://csrc.nist.gov/pqcrypto

- 5.  Banegas, G. et al. (2021). Concrete quantum cryptanalysis of binary elliptic curves. IACR Transactions on Cryptographic Hardware and Embedded Systems.

- 6.  National Security Agency (2022). Commercial National Security Algorithm Suite 2.0. NSA Cybersecurity Advisory.

- 7.  Ministry of Science & Technology, Government of India (2023). National Quantum Mission: Approved by Union Cabinet, April 2023. DST Press Release.

- 8.  Department of Science & Technology (2023). NQM Implementation Framework and Hub Structure. DST Technical Report.

- 9.  IBM Quantum (2024). IBM Quantum Development Roadmap 2024-2033. IBM Research Blog.

- 10. Google Quantum AI (2024). Quantum error correction below the surface code threshold. Nature, December 2024.

- 11. Kim, Y. et al. (2023). Evidence for the utility of quantum computing before fault tolerance. Nature, 618, 500-505.

- 12. IonQ (2024). IonQ Forte Enterprise: 35 algorithmic qubits with QV > 4,000,000. IonQ Technical Specification Sheet.

- 13. Quantinuum (2023). H2 quantum processor: 56 qubits, 99.9% two-qubit gate fidelity. Quantinuum Technical Briefing.

- 14. QuEra (2023). Logical quantum processor based on reconfigurable atom arrays. Nature, 626, 58-65.

- 15. Gidney, C., & Ekera, M. (2021). How to factor 2048 bit RSA integers in 8 hours using 20 million noisy qubits. Quantum, 5, 433.

- 16. Cloudflare (2023). Cloudflare and CRYSTALS-Kyber: Post-quantum key agreement in TLS 1.3. Cloudflare Blog.

- 17. CERT-In (2023). Guidelines on Quantum-Safe Cryptography for Indian Organisations. CERT-In Advisory 2023-40.

- 18. Qiskit Community (2024). Qiskit Developer Certification Exam Guide. IBM Quantum Learning Platform.

- 19. Deutsch, D., & Jozsa, R. (1992). Rapid solution of problems by quantum computation. Proceedings of the Royal Society A, 439(1907), 553-558.

- 20. QHack (2024). Annual Quantum Computing Hackathon. Xanadu. https://qhack.ai
