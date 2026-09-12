 Scope: “observed” means the supplied September access-event export, not all-time usage. The event export has 2,968,521 rows.

  Let E = access events (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv) (query_sql), V = view
  snapshot (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__view_definition_snapshot.csv) (view_name, view_definition), and I =
  inventory (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__asset_inventory_snapshot.csv) (snapshot_date, asset_name, asset_type).

  1. [INTERNAL_ASSET_12] has neither observed direct nor view-derived usage. It is in V’s premium definition, but E contains no query of that table or
     of the Premium view. Support: E.query_sql, V.view_name/view_definition, I identifies it as a BASE TABLE.

  2. No—its lack of observed use does not establish safe deletion. [INTERNAL_ASSET_18] is a current BASE TABLE and remains
     referenced by the structurecomponent view definition. It has no observed direct or view-derived use in this export, but deletion safety is not recorded. Support: I and
     V; absence in E.query_sql.

  3. Insufficient information. mart__view_asset_dependency_bridge.csv is not in the supplied directory, so the files do not define what occurrence in it would prove.
  4. [INTERNAL_ASSET_10] was observed through the queried PartyRoleInClaim view, not directly. E records dbt queries of that view,
     including source-not-null tests; V’s partyroleinclaim definition includes this table. Support: E.access_event_id/query_sql (for example
     idis:20260902_225604_23872_nj8h2, idis:20260903_225636_25289_nj8h2, and idis:20260904_225624_25252_nj8h2), V.view_name/view_definition.

  5. Insufficient information to define root_view_name versus target_asset_name: neither field exists in the supplied files. The closest recorded concepts are V.view_name
     and the referenced table names inside V.view_definition.

  6. No supported conclusion. dependency_depth is absent, and no file defines it as usage evidence.
  7. None, under the available evidence rule: 0 assets have both direct and view-derived evidence. The event queries reference 43 snapshot views; those map to 1,316 assets
     through V, while no inventoried base table is directly named in E. Support: E.query_sql, V, I.

  8. 1,311 of the 2,627 latest-inventory base tables are unobserved under that same limited rule: no direct E evidence and no path from an observed V view. This includes
     the Premium PMS table and the Norkart porch-dimensions table above. This is a period-specific observation, not an obsolescence classification. Support: I snapshot
     2026-09-10, E.query_sql, V definitions.

  9. Insufficient information. candidate_for_review does not appear in the supplied files, so it cannot be interpreted as “obsolete” or “safe to delete.”
  10. The supported attribution chain is:

  E.query_sql queries PartyRoleInClaim
    → V.view_name = partyroleinclaim
    → V.view_definition references [INTERNAL_ASSET_10]

  Thus that table’s observed usage is view-derived through PartyRoleInClaim. A table directly named in E.query_sql would have direct evidence; a table with neither
  condition is unobserved only within this export’s scope.

─ Worked for 10m 15s ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/ontology                                     │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Answer usage and asset questions                                          │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a09084-c7a8-7a60-90a5-b7b08a045af8                                      │
│                                                                                                  │
│  Token usage:          126K total  (105K input + 20.9K output)                                   │
│  Context window:       62% left (106K used / 258K)                                               │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
