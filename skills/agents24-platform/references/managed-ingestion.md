# Managed ingestion and customer partitions

Last Updated: 2026-10-08

Use this reference for customer libraries, document/media processing, indexed-output cleanup and grounded chat. Keep application authentication, customer identity, originals and publication in the consuming application.

## Availability and discovery

Check installed SDK types and the [capability catalog](https://docs.agents24.dev/sdk/capabilities). Multipart/indexed-content operations are assigned first-supported Node `0.5.24` and Python `0.7.17`; assignment does not establish registry availability. Require the promoted coordinated packages and deployed backend before using them. Report a release mismatch instead of inventing HTTP wrappers or fallback exports.

Use the [managed-ingestion guide](https://docs.agents24.dev/sdk/managed-ingestion) and its downloadable Node/Python scripts and resource packages. Author/publish resources with the CLI; application code uses typed SDK methods against a published executable. Retired operator versions fail explicitly: prepare and republish current graphs, without silently adapting old executables.

## One provider organization, application-owned customers

Keep one organization key on the trusted backend. Configure stable `integrationId` for the app. Resolve the authenticated user independently, and select an optional `chatScope` after membership checks; scope is not the user's identity. BFF resolution returns `chatScope` and `knowledgeNamespaces` in Node, `chat_scope` and `knowledge_namespaces` in Python. Omitted scope selects only unscoped history; scoped thread ownership remains user-specific and immutable.

Select knowledge namespaces through a trusted per-store map. Enable Dynamic on applicable ingestion/retrieval nodes so runtime mappings take effect. A library and published portal can have separate app-selected namespaces. Namespace selection does not establish membership or publication policy. Record customer-to-job/upload/indexed-generation associations in the app before authorizing organization-key operations. Direct browser runtime does not provide this dynamic scope contract.

## Upload, process and recover

- Discovery/job reads use `pipelines.read`; upload/start/resume/cancel use `pipelines.write`.
- Node `ingestionJobs` exposes `createUpload`, `getUpload`, `putUploadPart`, `completeUpload`, `abortUpload`; Python `ingestion_jobs` uses their snake_case counterparts.
- Persist session IDs and mutation keys for uncertain delivery. Send bounded file slices to returned part URLs; do not attach the organization key to object-storage signed URLs. Avoid loading a complete large source into memory.
- Submit opaque `input_ref` values with app-owned `content_id` and `revision_id` together. A source can also carry a title and external reference. Do not send private host paths.
- Use `file_source` or credential-backed `s3_source`, then `document_extract` for PDF/DOCX/TXT or `speech_to_text` for audio/video. Use separate document and media templates; mixed-format routing is deferred.
- Docling handles document extraction/OCR/tables. STT uses published OpenRouter Models through Agents24's shared model/accounting services. `timed` requires actual segment timestamps; `transcript_only` supplies searchable text without time ranges. Do not substitute models or fabricate timing.
- Poll `get`/`listBatches`, inspect safe per-source outcomes, and resume eligible failed/partially failed/cancelled jobs. Reuse the identical request and idempotency key after uncertain command delivery. No exactly-once provider execution is promised.
- Ambiguous dispatched speech requests are not automatically replayed. Preserve the safe failure/request IDs; do not retry by changing configuration to bypass the checkpoint.

File admission is at most 500,000,000 bytes, subject to storage quota. Active jobs retain their inputs; terminal jobs keep seven days of recovery availability. Unused inputs and unfinished upload sessions expire after 24 hours. Applications retain originals. The two-hour duration ceiling and 200-file selections are not certified live throughput/quality/cost promises; verify the actual deployment and selected models before advertising capacity.

## Indexed-output cleanup and replacement

Node `knowledgeStores` exposes `listIndexedContent`, `getIndexedContent`, `deleteIndexedContent`, `getCleanupJob`, `resumeCleanupJob`, `cancelCleanupJob`; Python `knowledge_stores` uses snake_case. Inspection/status use `knowledge_stores.read`; deletion and cleanup controls use `knowledge_stores.write`, not pipeline authority.

List by explicit namespace with content/revision filters, paginate every matching generation, then delete the returned generation IDs. Deletion returns managed cleanup jobs; poll for terminal acknowledgment. Resume eligible cleanup through its store namespace. Store archive is a separate soft lifecycle operation and does not delete indexed documents.

Replacement is ingest revision B, confirm success, select B in the application, then delete A. Backend cleanup acknowledgment does not promise immediate query visibility. Applications enforce immediate unpublication through current corpus/revision selection and decide whether historical chats retain text or continue with revoked sources.

## Evidence and boundaries

Preserve canonical evidence through retrieval, source/citation events and persisted message blocks. It includes content/revision/generation, chunk/segments, physical PDF pages or original-media ranges; DOCX has semantic references without invented page numbers. Render and authorize links in the app. Citation identity validation does not prove semantic claim support.

Managed Drive import, Supabase output delivery and completion webhooks are deferred. Polling is supported. Do not imply those connectors exist, require a new customer registry, or create an Agents24 document-management UI.
