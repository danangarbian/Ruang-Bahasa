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
2. **Strict Dual-Key Approval (Four-Eyes Principle)**: A user cannot approve their own submission (
eviewer_id != submitter_id).
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
1. Suryanto, T. L. M., Wahuni, E. D., Saídiyah, I., Wibawa, A. P., & Sulistyo, D. A. (2026). Transforming sports knowledge: Chatbot AI-based systems for improved training and education in sports.. *Journal of Physical Education & Sport 26 (1), 2026*. https://scholar.google.com/citations?view_op=view_citation&hl=id&user=Y3TMwyUAAAAJ&pagesize=100&citation_for_view=Y3TMwyUAAAAJ:mvPsJ3kp5DgC
2. M, F. & Sulistyo, D. A. (2026). Pemanfaatan NLP Untuk Ekstrasi Profil Kompetensi Dari Portofolio Digital Sebagai Dasar Sistem Rekomendasi. *Bulletin of Data Science 5 (3), 295-303, 2026*. https://doi.org/10.47065/bulletinds.v5i3.10251
3. Tampubolon, S. V. & Sulistyo, D. A. (2026). Analisis Sentimen Ulasan Game Stardew Valley pada Steam dan Google Play. *JURNAL FASILKOM 16 (1), 57-67, 2026*. https://doi.org/10.37859/jf.v16i1.11217
4. Ahda, F. A., Wibawa, A. P., Prasetya, D. D., Sulistyo, D. A., & Nafalski, A. (2026). Advanced Machine Translation-Based Stammer for Preserving the Minangkabau Traditional Medicine Domain. *JOIV: International Journal on Informatics Visualization 10 (3), 959-969, 2026*. https://doi.org/10.62527/joiv.10.3.4042
5. Sulistyo, D. A., Prasetya, D. D., Ahda, F. A., & Wibawa, A. P. (2025). Pivoted low resource multilingual translation with NER optimization. *ACM Transactions on Asian and Low-Resource Language Information Processing*. https://doi.org/10.1145/3727876
6. Sulistyo, D. A., Wibawa, A. P., Prasetya, D. D., Ahda, F. A., & Astawa, I. (2025). Multilingual Parallel Corpus for Indonesian Low-Resource Languages. *JOIV: International Journal on Informatics Visualization 9 (5), 2176-2182, 2025*. https://doi.org/10.62527/joiv.9.5.3412
7. Ahda, F. A., Wibawa, A. P., Prasetya, D. D., Sulistyo, D. A., & Nafalski, A. (2025). Minangkabau language stemming: A new approach with modified enhanced confix stripping. *Jurnal RESTI (Rekayasa Sistem dan Teknologi Informasi) 9 (3), 677-687, 2025*. https://doi.org/10.29207/resti.v9i3.6511
8. Sulistyo, D. A., Wibawa, A. P., Prasetya, D. D., & Ahda, F. A. (2025). Indonesian cross-linguistic named entity recognition. *Research Methods in Applied Linguistics 4 (3), 100236, 2025*. https://doi.org/10.1016/j.rmal.2025.100236
9. Mahendra, K. D. & Sulistyo, D. A. (2025). Improving SpaCy-based NER Accuracy using Conditional Random Fields (CRF) as Advanced Features for Javanese Legends. *ICoBITS 1, 508-516, 2025*. https://doi.org/10.32664/icobits.v1.57
10. Sulistyo, D. A., Wibawa, A. P., Mahmudy, W. F., Ahda, F. A., & Nafalski, A. (2025). Empowering Low-Resource Languages: Javanese Machine Translation. *RESTI (Rekayasa Sistem dan Teknologi Informasi) 9 (5), 1210-1221, 2025*. https://doi.org/10.29207/resti.v9i5.6887
11. Fahrudillah, M. F. & Sulistyo, D. A. (2025). Analisis Sentimen Ulasan Pemain Genshin Impact Menggunakan Kombinasi TF-IDF, Lexicon, dan Support Vector Machine. *JURNAL FASILKOM 15 (3), 580-593, 2025*. https://doi.org/10.37859/jf.v15i3.10553
12. Setiadi, E. & Sulistyo, D. A. (2025). Analisis Sentimen Kebijakan Makan Bergizi Gratis Menggunakan IndoBERT dan Machine Learning. *JURNAL FASILKOM 15 (3), 507-513, 2025*. https://doi.org/10.37859/jf.v15i3.10546
13. Sulistyo, D. A., Wibawa, A. P., Prasetya, D. D., & Ahda, F. A. (2025). An enhanced pivot-based neural machine translation for low-resource languages.. *International Journal of Advances in Intelligent Informatics 11 (2), 258, 2025*. https://doi.org/10.26555/ijain.v11i2.2115
14. Sulistyo, D. A., Wibawa, A. P., Prasetya, D. D., Ahda, F. A., & Utama, A. B. P. (2025). A Comparative Study of Machine Learning Models for Javanese Wuku Classification: Exploring SVM, Naïve Bayes, and CNN for Cultural Texts. *Knowledge Engineering and Data Science 8 (1), 104-117, 2025*. https://doi.org/10.17977/um018v8i12025p104-117
15. Ashari, M., Sulistyo, D. A., & Ahda, F. A. (2024). Stemming in Madurese language using Nazief and Adriani algorithm. *Jurnal Teknik Informatika (Jutif) 5 (4), 695-702, 2024*. https://doi.org/10.52436/1.jutif.2024.5.4.2012
16. Ahda, F. A., Wibawa, A. P., Prasetya, D. D., & Sulistyo, D. A. (2024). Comparison of Adam optimization and RMS prop in Minangkabau-Indonesian bidirectional translation with neural machine translation. *JOIV: International Journal on Informatics Visualization 8 (1), 231-238, 2024*. https://doi.org/10.62527/joiv.8.1.1818
17. Sulistyo, D. A., Wibawa, A. P., Prasetya, D. D., & Ahda, F. A. (2023). LSTM-based machine translation for Madurese-Indonesian. *Journal of Applied Data Sciences 4 (3), 189-199, 2023*. https://doi.org/10.47738/jads.v4i3.113
18. Sulistyo, D. A., Wibawa, A. P., Prasetya, D. D., & Nafalski, A. (2023). Autogenerated Indonesian-Madurese parallel corpus dataset using neural machine translation. *Data in Brief (Elsevier) / SSRN*. https://doi.org/10.1016/j.dib.2025.112046
19. Sulistyo, D., Ahda, F., & Fitria, V. A. (2021). Epistomologi dalam Natural Language Processing. *Jurnal Inovasi Teknologi dan Edukasi Teknik 1 (9), 652-664, 2021*. https://doi.org/10.17977/um068v1i92021p652-664

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
