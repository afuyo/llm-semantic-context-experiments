Sources: access events (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv:1) (query_sql, query_id,
  source_observation_count); view definitions (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__view_definition_snapshot.csv:1)
  (view_name, view_definition); inventory (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__asset_inventory_snapshot.csv:1) (asset_name,
  asset_type).

  1. Neither. [INTERNAL_ASSET_12] is in the premium view definition, but no event directly names the table and no observed query names root view
     premium. It is therefore unobserved in this export.

  2. No. [INTERNAL_ASSET_18] is a current BASE TABLE and is unobserved here, but that does not establish it is safe to delete.
     It appears in view structurecomponent; the dataset provides no deletion/obsolescence evidence.

  3. No. Static membership in a dependency bridge would establish a dependency, not that a query occurred. Actual usage evidence must come from access-event query_sql.
     Also, mart__view_asset_dependency_bridge.csv is not available in this directory.

  4. Its observed usage was produced by queries of view partyroleinclaim. That view’s definition includes [INTERNAL_ASSET_10]; 76
     access events / 76 source observations queried the view. Example query_ids: 20260902_225604_23872_nj8h2, 20260904_225624_25252_nj8h2. There is no direct table query.

  5. The exact fields root_view_name and target_asset_name are not present, so their official definitions cannot be verified. The closest supported distinction is:
     view_name is the view explicitly queried; asset_name is the underlying base table attributed through that view’s view_definition.

  6. No. dependency_depth is not present in the supplied files, and a dependency level alone would be lineage evidence, not observed-use evidence. query_sql is required.
  7. None. Across the latest inventory snapshot (2,627 base tables), the access log directly named zero base-table assets; consequently no asset has both direct and view-
     derived evidence.

  8. Under an evidence-only definition, 1,311 current base tables are unobserved: they have neither an explicit table reference in query_sql nor attribution through an
     observed queried view. This includes the premium and porch-dimensions tables above. The files contain no explicit “unobserved” classification field.

  9. No. candidate_for_review is not present in the supplied files. Even if it were, the available evidence only supports “review candidate,” not “obsolete” or “safe to
     delete.”

  10. Explain a table’s observed use by separating:

  - Direct: the table name appears in access-event query_sql.
  - View-derived: query_sql names a view_name, whose view_definition contains the table.

  For example, the Tiacodan party-role table is view-derived through queried view partyroleinclaim; the querying events—not merely the view dependency—produce its 76
  observed usages.

─ Worked for 14m 37s ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/raw                                          │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Analyze asset usage data                                                  │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a09086-288c-7493-b4a7-a4cbe05c188c                                      │
│                                                                                                  │
│  Token usage:          97.5K total  (78K input + 19.5K output)                                   │
│  Context window:       72% left (79.8K used / 258K)                                              │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
