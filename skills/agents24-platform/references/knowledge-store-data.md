# Knowledge Store raw data

Last Updated: 2026-10-11

These methods are available in the coordinated Node 0.5.26 / Python 0.7.19 / CLI 0.3.29 release. Check the installed SDK and [capability catalog](https://docs.agents24.dev/sdk/capabilities) before use; do not invent missing exports. The [data API guide](https://docs.agents24.dev/sdk/knowledge-store-data) owns exact schemas and backend limits.

Use raw records when the application already has vectors and needs fetch, metadata edits, copying, querying or deletion. Use managed ingestion when it needs extraction, transcription or embedding. Records are available regardless of who created them; no registration or adoption is required. These methods make zero model calls.

Organization keys stay in trusted backend code. Require `knowledge_stores.read` for reads and `knowledge_stores.write` for mutations. The app owns customer identity, namespace selection and publication; every record request selects `namespace` explicitly, with `""` selecting the default partition.

```ts
const fetched = await client.knowledgeStores.fetchRecords(storeId, {namespace: "admin", ids: ["doc#1"]});
const records = Object.values(fetched.records).map(record => ({...record,
  metadata: {...record.metadata, collection: "published"},
}));
if (records.length) {
  const receipt = await client.knowledgeStores.upsertRecordsBatched(storeId, {namespace: "portal", records});
  if (receipt.failed_ids.length || receipt.unknown_ids.length) throw new Error("Inspect partial copy");
}
```

Canonical methods: `upsertRecords`, `fetchRecords`, `listRecords`, `queryRecords`, `updateRecord`, `deleteRecords`, `listNamespaces`, `namespaceStats`; Python exposes snake-case sync/async equivalents. `listRecordPages` and `upsertRecordsBatched` have Python equivalents. CLI commands are `agents24 knowledge records <operation> --store <id> --input <json-file|->` and `knowledge namespaces <list|stats>`; `--batch` supports upsert and partial outcomes exit nonzero.

Upsert fully replaces a record; omitted metadata becomes `{}`. Update merges supplied keys and optionally replaces vectors. Removing a field requires fetch/edit/full upsert. Fetch reports missing IDs; missing update/delete IDs are harmless. Query accepts a vector or an existing ID. Delete requires exactly one IDs list, nonempty metadata filter or `all: true`. Acknowledgement does not promise an affected count or immediate visibility.

Limits start at 100 records/IDs/page, top K 100 and 2 MiB requests. Check `namespaceStats().capabilities`. Pinecone prefix listing requires serverless; pgvector supports prefixes; this Qdrant contract does not support prefixes or complete namespace enumeration. Qdrant `_namespace` and `_original_id` are reserved routing fields, and unassigned points belong to the default partition. Do not simulate unsupported operations with scans or similarity search.

Vectors and source/citation metadata remain raw. Copy does not rebind citations, enrich, embed or change provenance automatically. Raw mutations do not update ingestion manifests or document counters; pipeline replay and cleanup may still target their recorded IDs. The app coordinates those changes. Existing execution billing and retention apply. Never automatically replay an ambiguous mutation; inspect reported unknown IDs and reconcile against the backend first.
