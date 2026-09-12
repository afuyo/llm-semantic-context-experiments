Scope: “unobserved” below means not observed in the supplied access log, whose metadata filters namespace = 'ws-claims-dk-bi' for September 2026—not globally unused.

   Asset                                                               Direct query evidence    Observed through queried view    Conclusion
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   [INTERNAL_ASSET_12]                                              No    No                               No observed usage
  ──────────────────────────────────────────────────────────────────  ───────────────────────  ───────────────────────────────  ───────────────────────────────────────────
   [INTERNAL_ASSET_18]                       No    No                               No observed usage, but not safe to delete
  ──────────────────────────────────────────────────────────────────  ───────────────────────  ───────────────────────────────  ───────────────────────────────────────────
   [INTERNAL_ASSET_10]                               No    PartyRoleInClaim                 View-derived usage

  1. Q1: Neither direct nor view-derived usage was observed for the premium asset. It is structurally included in the premium view, but the access log contains neither the
     base-asset identifier nor a query of Premium.
     Support: access log query_sql; view snapshot view_name, view_definition.

  2. Q2: No. It is not safe to delete based on this export: structurecomponent’s definition explicitly references the asset. It has no observed access, but deletion could
     invalidate that view.
     Support: view snapshot view_name=structurecomponent, view_definition; inventory asset_type=BASE TABLE.

  3. Q3: No. A dependency occurrence does not prove observed use; it needs corroborating query_sql evidence. The porch-dimensions asset is the counterexample: it is in the
     structurecomponent definition but has no observed direct or view query.
     Support: view snapshot view_definition; access log query_sql.

  4. Q4: Its observed usage was produced by queries of the PartyRoleInClaim view, not direct queries of the base table. The view definition includes the target asset.
     Support: access log query_sql contains FROM ... "PartyRoleInClaim"; view snapshot view_name=partyroleinclaim, view_definition.

  5. Q5: The exact fields root_view_name and target_asset_name are absent, so their formal definitions cannot be established from this directory. The supplied analogue is
     view_name (the view) and the referenced table text inside view_definition.
     Support: view snapshot headers; absence of the requested bridge file/fields.

  6. Q6: No. dependency_depth is not present here, and structural dependency alone is not usage evidence.
     Support: absence of that field; porch-dimensions counterexample above.

  7. Q7: No asset among the three named assets has both kinds of evidence. An inventory-wide list cannot be determined because mart__view_asset_dependency_bridge.csv is not
     supplied.
     Support: access-log query_sql; view snapshot view_definition.

  8. Q8: Among the named assets, premium and porch-dimensions should be considered unobserved in this log scope. The claim-item asset is observed via PartyRoleInClaim. A
     complete inventory-wide unobserved list is insufficiently supported.
     Support: access log query_sql; access-event metadata filter and exported_at.

  9. Q9: No. candidate_for_review is absent from the available files, so it cannot be defined or treated as proof of obsolescence. In any case, a candidate flag would not
     override a current view dependency without validation.
     Support: absence of the field/file; porch-dimensions reference in view_definition.

  10. Q10: Attribute observed usage in two steps: find a query in access-log query_sql, then connect its queried view to the asset through snapshot view_name and
     view_definition. Example: the claim-item table is attributed through the queried view PartyRoleInClaim; that view’s definition contains [INTERNAL_ASSET_10]
     2.edhtiacodanclaitem.2624.

  Primary files: access events (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv), access metadata
  (rhs_last_month_2026-08-11_to_2026-09-11/ehs__data_access_event__ws_claims_dk_bi__2026-09_metadata.json), view snapshot (rhs_last_month_2026-08-11_to_2026-09-11/
  platform_observability__rhs__shared_source__view_definition_snapshot.csv), and asset inventory (rhs_last_month_2026-08-11_to_2026-09-11/
  platform_observability__rhs__shared_source__asset_inventory_snapshot.csv).

─ Worked for 10m 19s ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/vocabulary                                   │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Analyze RHS usage data                                                    │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a09085-09a2-7911-a048-0e78a50cd034                                      │
│                                                                                                  │
│  Token usage:          110K total  (90.9K input + 18.9K output)                                  │
│  Context window:       68% left (90.7K used / 258K)                                              │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
