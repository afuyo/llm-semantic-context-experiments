Only CONTEXT.md is available. The named CSVs, asset IDs, and fields are absent, so asset-specific results cannot be determined.

  1. Insufficient information. No evidence is available for [INTERNAL_ASSET_12], so it cannot be classified as direct, view-derived, or both.
     Support: no matching asset record/file; the possible classifications are defined in CONTEXT.md:79.

  2. Insufficient information. There is no record for [INTERNAL_ASSET_18]; therefore neither “unused” nor “safe to delete” is
     supported.
     Support: CONTEXT.md:87 says even a Review Candidate is not necessarily obsolete or approved for deletion.

  3. No. A row in mart__view_asset_dependency_bridge.csv would describe a structural dependency, not prove usage.
     Support: CONTEXT.md:27 explicitly says an Asset Dependency is not evidence that either asset was used.

  4. Insufficient information. No observed-usage/evidence record exists for [INTERNAL_ASSET_10], so its usage producer cannot be
     identified.
     Support: the available directory has no record for that asset; usage requires evidence during the observation period (CONTEXT.md:35).

  5. Insufficient information for those exact fields. root_view_name and target_asset_name are not defined or present in the available files. The closest supported
     distinction is:
      - a Queried Asset is explicitly referenced by the access event;
      - an Attributed Asset receives the evidence—identical to the queried asset for direct evidence, but a contributing table for view-derived evidence.
        Support: CONTEXT.md:51 and CONTEXT.md:55.

  6. No. dependency_depth is not defined in the available files, and dependency information alone is explicitly not usage evidence.
     Support: CONTEXT.md:27.

  7. Insufficient information. No asset-level usage-evidence data is available to identify assets with both forms of evidence.
     Support: the relevant classification is DIRECT_AND_VIEW_DERIVED (CONTEXT.md:79), but no records are supplied.

  8. No specific assets can be identified as unobserved. An unobserved asset has no Usage Evidence in the assessed observation period.
     Support: CONTEXT.md:91. No asset/evidence dataset is present.

  9. No. The exact field candidate_for_review is unavailable, but the corresponding glossary concept, Review Candidate, explicitly does not establish obsolescence or
     deletion approval.
     Support: CONTEXT.md:87.

  10. For a specific table, the evidence required is unavailable. Generically:

  - Direct usage: an access event explicitly queried the table; the queried and attributed asset are the same.
  - View-derived usage: an access event queried a view, and evidence was attributed to the contributing table.
  - The conclusion applies only within the observation period.
    Support: CONTEXT.md:43, CONTEXT.md:47, CONTEXT.md:55, and CONTEXT.md:75. The queried-view field itself is not available, so no view name can be supplied.

    ╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/vocabulary                                   │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Assess asset usage                                                        │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a0906c-3361-7990-8acf-910e0d0ebf8c                                      │
│                                                                                                  │
│  Token usage:          30.3K total  (27.5K input + 2.74K output)                                 │
│  Context window:       93% left (28.9K used / 258K)                                              │
│  Limits:               not available for this account                                            │
