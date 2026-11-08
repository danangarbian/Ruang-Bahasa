# Ruang Bahasa: Technical Architecture Specification

**Document Version:** 1.0 (Pre-Launch Open Specification)  
**Author:** Dr. Danang Arbian Sulistyo, S.Kom., M.Kom.  
**Affiliation:** Institut Teknologi dan Bisnis Asia Malang  
**License:** Apache License 2.0  
**Target Fellowship:** Mozilla Foundation Fellowship 2027–2028 (Theme 6: Sovereign AI, Public Infrastructure, and Maintenance)

---

## 1. Architectural Motivation & Sovereign Philosophy

Modern generative Large Language Models (LLMs) are predominantly trained on web-scraped corpora where high-resource languages (English, Chinese, standard Indonesian) dominate representation. For Indonesia’s 700+ regional languages—such as Madurese (14M speakers), Javanese (80M speakers), and Minangkabau (5.5M speakers)—commercial monolithic models exhibit three critical failure modes:
1. **Hallucinatory Lexical Bleed**: LLMs blend regional vocabulary with Indonesian or Malay cognates, corrupting morphology and orthography.
2. **Honorific & Pragmatic Collapse**: In languages with strict honorific stratifications (*undha-usuk* in Javanese; *Enja'-Iya, Engghi-Enten, Engghi-Bhunten* in Madurese), commercial models frequently select inappropriate speech registers, violating social decorum.
3. **Data Extractivism Without Community Governance**: Centralized cloud providers extract local cultural knowledge to improve proprietary weights without returning sovereign control, provenance tracking, or offline capabilities to the communities of origin.

**Ruang Bahasa** reverses this dynamic by implementing **Sovereign AI Infrastructure**: a decentralized, human-governed protocol where AI is restricted to a subordinate drafting role, and the final arbiters of truth, pedagogical fitness, and linguistic purity are practicing local teachers and linguists.

---

## 2. High-Level Architecture (The 5 Swimlanes)

The platform is structured into five sequential, tightly-coupled architectural swimlanes:

`
[Block 1: Teacher Input & Rights] 
       │
       ▼
[Block 2: Sovereign AI Generation (Mock / Local / Cloud)] 
       │
       ▼
[Block 3: Human Review, Veto & Immutable Revisions] 
       │
       ├──► Rejected (Feedback Loop to Teacher)
       ▼ (Approved via 4-Eyes Rule)
[Block 4: Community Knowledge Governance Release] 
       │
       ▼
[Block 5: Classroom Export & Local Open-Weight Tuning]
`

### 2.1 Block 1: Teacher Input & Granular Source Rights
- **Input Corpus**: Elementary and secondary educators submit standard Indonesian texts (curriculum modules, folktales, civic lessons).
- **Metadata Tagging**: Target Language (mad, jav, min), Target Speech Level (*e.g.*, *Ngoko / Krama Lugu / Krama Alus* for Javanese; *Enja'-Iya / Engghi-Enten / Engghi-Bhunten* for Madurese), Grade Level (SD 1–6, SMP 7–9, SMA 10–12).
- **Granular Consent Flags**: Every submission records 4 immutable boolean flags:
  - llow_external_ai (Boolean): Authorizes sending text to external commercial cloud LLMs (if disabled, only on-premise local models may process the text).
  - llow_internal_reuse (Boolean): Authorizes sharing across the regional teacher network.
  - llow_public_release (Boolean): Authorizes inclusion in open-access research datasets.
  - llow_training (Boolean, default False): Explicit, revocable authorization for model fine-tuning.

### 2.2 Block 2: Agnostic Generation Engine & Dynamic Context Injection
- **Context Injection**: Generation requests dynamically query the local repository for:
  - Approved terminology pairs from active KnowledgeRelease snapshots.
  - Zero-shot / few-shot exemplar correction candidates with identical linguistic attributes.
  - Strict exclusion: Any data marked with split = 'TEST' is systematically banned from retrieval prompts.
- **Provider Interface (AIProvider)**:
  `python
  class AIProvider(ABC):
      @abstractmethod
      def generate(self, prompt: str, schema: dict, config: GenerationConfig) -> GenerationResult:
          pass
  `
  - MockProvider: Deterministic synthetic provider returning rule-based JSON fixtures for zero-cost continuous integration and headless acceptance testing.
  - LocalModelProvider: Connects via HTTP/gRPC to local inference runners (Ollama / vLLM) executing quantized open-weight checkpoints (*e.g.*, Qwen2.5-7B-Instruct, Llama-3.1-8B) on commodity workstation hardware.
  - CloudAPIProvider: Fallback client for external managed APIs (OpenAI, Anthropic) if explicitly authorized by llow_external_ai = True.
- **Structured Schema Enforcement**: Outputs must strictly conform to schemas/ai_draft_output.schema.json, providing:
  - eading_passage: Regional text with paragraph-level Indonesian alignment.
  - ocabulary_glossary: Difficult terms, root words, speech level categorization.
  - comprehension_questions: Multiple-choice and open-ended pedagogical questions.
  - nswer_key: Explanations in regional language and standard Indonesian.

### 2.3 Block 3: Human-in-the-Loop Review, Veto & Immutable Revisions
- **Side-by-Side Editorial Console**: Verified teachers and linguists review source text, AI draft, and lexicon concurrently.
- **Immutable Revision Ledger**: Every keystroke and correction creates an append-only revision entry:
  - evision_number: Monotonically increasing integer.
  - content_hash: Cryptographic digest sha256(passage + vocab + questions).
  - editor_id: Authenticated user ID.
  - delta: Structural unified diff.
- **Strict Anti-Self-Approval (The Four-Eyes Invariant)**:
  - Under no circumstances may the author or submitter of a text approve their own candidate.
  - Relational check: ssert reviewer_id != candidate.submitter_id.
  - Violations raise immediate domain-level permission exceptions (403 Forbidden).
- **Review Outcomes**:
  - APPROVED: Candidate advances to knowledge pool and classroom export.
  - REJECTED: Candidate is archived with mandatory structured feedback to prevent recurring errors.
  - REVISE_REQUESTED: Sent back to drafting pool for iterative refinement.

### 2.4 Block 4: Community Knowledge Governance Release
- **Knowledge Release Assembler**: Periodically bundles validated lexical entries, correction pairs, and parallel sentences into an immutable release artifact (KnowledgeRelease).
- **Contamination Shield**:
  - Evaluation benchmark sets are strictly quarantined (split = 'TEST').
  - Automated release builders verify: COUNT(sentences WHERE split = 'TEST' AND included_in_release = True) == 0.
- **Output Formats**: Validated artifacts are published as JSON-L, Apache Parquet, and CSV, versioned via Semantic Versioning (SemVer) with open licenses (CC-BY 4.0 / CC0) for community re-use.

### 2.5 Block 5: Classroom Export & Edge Distillation
- **Pedagogical Asset Compilation**: Instant rendering of validated units into:
  - Formatted Print PDFs (A4, high-contrast, dual-column layout for rural classrooms).
  - Microsoft Word (.docx) for teacher customization.
  - Markdown / HTML for mobile learning management systems (Moodle, Google Classroom).
- **Distillation to Open Weights**: Curated datasets are utilized to parameter-efficiently fine-tune (LoRA/QLoRA) local open models, progressively diminishing dependency on foreign proprietary cloud APIs.

---

## 3. Concurrency, Storage & Operational Simplicity

To ensure rapid onboarding and operational resilience in resource-constrained institutional environments (such as regional universities or school districts), the initial implementation avoids heavy external infrastructure dependencies:

1. **Transactional PostgreSQL Queue**:
   - Rather than deploying Redis/Celery during initial phases, job dispatch and AI generation queues are managed via PostgreSQL using:
     `sql
     SELECT * FROM generation_tasks 
     WHERE status = 'QUEUED' 
     ORDER BY priority DESC, created_at ASC 
     FOR UPDATE SKIP LOCKED 
     LIMIT 1;
     `
   - Eliminates distributed transaction failures and memory cache desynchronization.
2. **Heartbeat & Lease Expiration**:
   - Background worker processes obtain a 5-minute lease on tasks.
   - Workers emit a periodic heartbeat. If a worker crashes or loses network connectivity, the lease expires automatically, and the task safely reverts to QUEUED status for other workers.

---

## 4. Verification & Acceptance Testing (18 Invariants)

The specification dictates 18 non-negotiable acceptance tests implemented in pytest/Django TestCase:
1. 	est_source_text_requires_explicit_consent_flags
2. 	est_external_ai_blocked_when_flag_false
3. 	est_ai_output_strictly_validates_against_json_schema
4. 	est_mock_provider_runs_with_zero_network_calls
5. 	est_draft_creation_initializes_revision_one
6. 	est_editor_updates_produce_new_immutable_revision
7. 	est_content_hash_matches_sha256_digest
8. 	est_submitter_cannot_approve_own_candidate_raises_403
9. 	est_verified_peer_can_approve_valid_candidate
10. 	est_rejected_candidate_requires_diagnostic_reason
11. 	est_knowledge_release_strictly_excludes_test_split_data
12. 	est_knowledge_release_generates_valid_checksum
13. 	est_classroom_export_produces_valid_pdf_and_docx
14. 	est_queue_worker_locks_row_with_skip_locked
15. 	est_expired_worker_lease_is_reclaimed_by_scheduler
16. 	est_speech_level_tagging_enforces_regional_typology
17. 	est_unauthorized_user_cannot_trigger_batch_export
18. 	est_delete_source_text_cascades_safely_preserving_audit_hashes

---

## 5. Security & Privacy Considerations

- **Tenant & Language Data Isolation**: Distinct language communities retain exclusive governance rights over their vocabulary and review pipelines.
- **Zero PII Exposure**: Pedagogical text inputs are scrubbed of student/teacher Personally Identifiable Information (PII) before transmission to generation workers.
- **Local-First Capability**: The architecture is fully operable in an air-gapped intranet environment utilizing local LLM instances.

---

*This specification is part of the Mozilla Foundation Fellowship 2027–2028 application materials for Dr. Danang Arbian Sulistyo.*
