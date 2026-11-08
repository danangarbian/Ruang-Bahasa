# Ruang Bahasa: Community Sovereign AI & Regional Language Knowledge Governance Protocol

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Specification](https://img.shields.io/badge/Specification-v1.0_Pre--Launch-green.svg)](#)
[![Mozilla Fellowship 2027-2028](https://img.shields.io/badge/Mozilla_Fellowship-Theme_6_Sovereign_AI-orange.svg)](#)
[![Datasets](https://img.shields.io/badge/Mozilla_Data_Collective-LegendNER--ID-blueviolet.svg)](https://mozilladatacollective.com/datasets/cmpcp7cl800ydmg077wte52mv)

> **Pre-Launch Open Architecture Specification & Community Knowledge Governance Protocol**  
> Designed and maintained by **Dr. Danang Arbian Sulistyo, S.Kom., M.Kom.**  
> *Principal Investigator & Builder | Mozilla Foundation Fellowship 2027–2028 Applicant (Theme 6: Sovereign AI, Public Infrastructure, and Maintenance)*

---

## 1. Executive Overview

**Ruang Bahasa** (*Language Space*) is an open, sovereign AI infrastructure and human-in-the-loop knowledge governance platform engineered for regional and low-resource languages in Indonesia (commencing with **Madurese, Javanese, and Minangkabau**, scaling to Sundanese, Balinese, and Buginese).

In low-resource linguistic ecosystems, commercial monolithic LLMs exhibit severe cultural hallucinations, lexical contamination, and disregard for honorific speech levels (*undha-usuk / tingkatan tutur*). Crucially, proprietary clouds extract community data without persistent governance or community consent.

Ruang Bahasa solves this through an **asymmetric, community-governed architecture**:
1. **Teacher-in-the-Loop Human Veto**: AI acts solely as a drafting assistant. No AI-generated regional text enters classrooms or public knowledge without rigorous human editorial sign-off.
2. **Immutable Revisions & Anti-Self-Approval**: All edits are version-tracked with cryptographic hashing (sha256); self-approval by submitters is strictly prohibited by relational constraints.
3. **Granular 4-Flag Source Rights**: Every source text records explicit consent flags (llow_external_ai, llow_internal_reuse, llow_public_release, llow_training = False).
4. **Contamination-Free Knowledge Engine**: Strict architectural isolation guarantees that test and evaluation benchmark splits (TEST) are zero-contaminated and never injected into generative prompts or knowledge releases.
5. **Provider Agnosticism & Local Deployment**: Generation runs via a pluggable provider interface supporting local open-weight models (via Ollama/vLLM on commodity desktop hardware) as well as cloud endpoints, ensuring digital sovereignty even during internet disruption.

---

## 2. High-Level System Architecture

The following block diagram illustrates the 5 core swimlanes of Ruang Bahasa, spanning input consent, sovereign generation, human editorial review, knowledge governance release, and classroom export.

![Ruang Bahasa Architecture](assets/ruang_bahasa_architecture_diagram.png)

*(For Indonesian version, see [assets/blok_diagram_ruang_bahasa_id.png](assets/blok_diagram_ruang_bahasa_id.png))*

### Architectural Swimlanes:

| Swimlane | Name | Function & Governance Invariant |
| :--- | :--- | :--- |
| **Block 1** | **Teacher Input & Granular Rights** | Teachers submit standard Indonesian source texts, selecting target language, speech level (*Enja'-Iya / Engghi-Enten / Engghi-Bhunten*), and grade level. 4 source flags are recorded. |
| **Block 2** | **Sovereign AI Generation Engine** | Injects curated community knowledge (glossary + approved exemplar corrections) into an agnostic AI provider (MockProvider, Local vLLM/Ollama, or Cloud API). Output adheres to strict JSON Schema. |
| **Block 3** | **Human-in-the-Loop Review & Veto** | Side-by-side comparative editor. Saves immutable revisions. Strictly enforces **Anti-Self-Approval** (requires a second verified peer/linguist). Rejection with feedback loop. |
| **Block 4** | **Curated Community Knowledge Release** | Approved correction pairs and validated terminology are assembled into immutable KnowledgeRelease snapshots. Zero contamination of benchmark data. Published as open datasets. |
| **Block 5** | **Multi-Format Classroom Export & Local Tuning** | Teachers export classroom-ready materials (Bilingual Reading, Vocabulary, Formative Assessment, Answer Keys) to PDF, DOCX, and Markdown. Open data feeds local model fine-tuning. |

---

## 3. Core Architectural Invariants (Non-Negotiable)

1. **Human Editorial Supremacy**: No raw LLM generation is ever promoted directly to the public knowledge base or exported as an official classroom resource without verified human approval.
2. **Strict Dual-Key Approval (Four-Eyes Principle)**: A user cannot approve their own submission (eviewer_id != submitter_id).
3. **Immutable Content Hash Validation**: Approved items store content_hash = sha256(text + metadata). Modifying any field invalidates the verification state.
4. **Zero-Contamination Benchmark Isolation**: Sentences marked as TEST split are excluded from prompt retrieval, RAG injection, and knowledge release datasets.
5. **Community Data Sovereignty**: By default, llow_training = False. Datasets released to the public require explicit community elders/teachers authorization.

---

## 4. Pre-Existing Foundation & Track Record

Ruang Bahasa is not a hypothetical proposal; it builds upon over 7 years of empirical computational linguistics research and open datasets curated by the Principal Investigator:

### Open Datasets:
1. **Indonesian–Madurese Bible-Domain Parallel Corpus with Religious-Cultural Entity Spans**  
   Mendeley Data, DOI: [10.17632/jmpv59pnj8.1](https://data.mendeley.com/datasets/jmpv59pnj8/1)
2. **Bahasa Madura Corpus Dataset**  
   Mendeley Data, DOI: [10.17632/cgtg4bhrtf/3](https://data.mendeley.com/datasets/cgtg4bhrtf/3)
3. **Madurese Sentence Dataset**  
   Mendeley Data, DOI: [10.17632/z7kxfzzc8g/3](https://data.mendeley.com/datasets/z7kxfzzc8g/3)
4. **LegendNER-ID (Javanese)**  
   Hosted on the **Mozilla Data Collective**: [https://mozilladatacollective.com/datasets/cmpcp7cl800ydmg077wte52mv](https://mozilladatacollective.com/datasets/cmpcp7cl800ydmg077wte52mv)

### Selected Peer-Reviewed Publications:
- **Sulistyo, D. A.**, et al. (2023). *A low-resource language dataset for Madurese with entity annotations and benchmark evaluations*. **PeerJ Computer Science**, 9:e1628. [DOI: 10.7717/peerj-cs.1628](https://doi.org/10.7717/peerj-cs.1628) (Scopus Q2).
- **Sulistyo, D. A.**, et al. (2020). *Part-of-speech tagging for Madurese language using Hidden Markov Model and rule-based post-processing*. **Telkomnika**, 18(6), 2910–2918. [DOI: 10.12928/telkomnika.v18i6.14815](https://doi.org/10.12928/telkomnika.v18i6.14815) (Scopus Q3).
- **Sulistyo, D. A.**, et al. (2020). *Named entity recognition in Indonesian cultural domain texts*. **International Journal of Advances in Intelligent Informatics (IJAIN)**, 6(3), 285–296. (Scopus Q3).
- **Sulistyo, D. A.**, & Winarko, E. (2020). *Named entity recognition on Indonesian text using bidirectional LSTM*. **IJCCS (Indonesian Journal of Computing and Cybernetics Systems)**, 14(4), 407–418.
- **Sulistyo, D. A.**, & Purnomo, D. (2021). *Text classification for regional Indonesian folk literature*. **SinkrOn: Jurnal dan Penelitian Teknik Informatika**, 6(1), 112–120.

---

## 5. Repository Structure

`	ext
ruang-bahasa/
├── README.md                                  # English & Indonesian architecture manifest
├── LICENSE                                    # Apache 2.0 Open Source License
├── .gitignore                                 # Standard Python, Django & environment gitignore
├── assets/
│   ├── ruang_bahasa_architecture_diagram.png  # English 300 DPI Block Diagram
│   └── blok_diagram_ruang_bahasa_id.png       # Indonesian 300 DPI Block Diagram
├── docs/
│   ├── BLUEPRINT_RUANG_BAHASA_ID.md           # Full Technical Blueprint (Indonesian, 606 lines)
│   └── ARCHITECTURE_OVERVIEW_EN.md            # Comprehensive Architecture Specification (English)
├── schemas/
│   ├── ai_draft_output.schema.json            # JSON Schema for AI Generation outputs
│   ├── correction_candidate.schema.json       # JSON Schema for Human Review & Candidates
│   └── knowledge_release.schema.json          # JSON Schema for Curated Knowledge Releases
└── examples/
    └── sample_knowledge_release_madurese_demo.json # Sample verification dataset (Madurese demo)
`

---

## 6. Implementation Roadmap (Mozilla Fellowship 2027–2028)

| Phase | Milestone | Focus Areas | Deliverables |
| :---: | :--- | :--- | :--- |
| **Phase A** | Core Domain & Auth | Identity, Roles (Teacher, Reviewer, Admin), 4-Flag Source Rights | Django models, RBAC, tenant isolation |
| **Phase B** | Ingestion & Agnostic AI | Teacher Input UI, MockProvider, OpenAI/Anthropic/Ollama adapters | Agnostic generation pipeline, schema validation |
| **Phase C** | Human Review & Curation | Side-by-side editor, immutable revision log, 4-eyes approval | Review dashboard, anti-self-approval checks |
| **Phase D** | Knowledge Governance | KnowledgeRelease compiler, test split contamination guard | Automated JSON/Parquet release generator |
| **Phase E** | Classroom Export | PDF, DOCX, Markdown renderers for multi-format lesson plans | One-click classroom export suite |
| **Phase F** | Pilot & Community Field Test | 10 schools in East Java / Madura (30 teachers, 500+ students) | User feedback, usability metric reports |
| **Phase G** | Open Sovereign Release | Public deployment, open data distribution, local edge LLM guide | Apache 2.0 code release, OpenAlex/Mendeley data |

---

## 7. Ringkasan Eksekutif (Bahasa Indonesia)

**Ruang Bahasa** adalah protokol dan arsitektur perangkat lunak terbuka (*open-source software architecture*) yang dirancang untuk mengatasi marginalisasi bahasa daerah Indonesia dalam revolusi kecerdasan artifisial. 

Melalui pendekatan **Sovereign AI** dan tata kelola pengetahuan berbasis komunitas:
1. Guru sekolah dasar dan menengah dapat mengonversi materi kurikulum Bahasa Indonesia menjadi bahan ajar bahasa daerah yang akurat secara leksikal dan santun sesuai tingkatan tutur (*undha-usuk*).
2. Hasil AI selalu berstatus  draf mentah dan tunduk pada **Hak Veto Guru**; tidak ada teks yang dirilis atau diekspor ke kelas tanpa telaah manusia.
3. Semua koreksi yang disetujui ditransformasikan menjadi dataset terbuka (*Community Knowledge Release*) yang terlindungi dari kontaminasi data uji, menjadi aset publik untuk melatih model-model lokal berbobot terbuka (*open-weight models*) secara mandiri tanpa ketergantungan abadi pada vendor komputasi awan global.

---

## 8. License & Attribution

This specification and all associated schemas and documentation are released under the **Apache License 2.0**.  
Copyright (c) 2026 Dr. Danang Arbian Sulistyo.

For collaborations, academic inquiries, or Mozilla Fellowship correspondences:  
**Dr. Danang Arbian Sulistyo, S.Kom., M.Kom.**  
Email: danang@asia.ac.id | Malang, East Java, Indonesia
