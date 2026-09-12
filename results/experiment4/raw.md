 I inspected the supplied dataset, including all 2,968,521 access-event records.

  1. [INTERNAL_ASSET_12]

  Conclusion: no captured query-access evidence supports that it was used.

  - Observed evidence: rhs_last_month_2026-08-11_to_2026-09-11/finance__rhs_premium_pms.csv has rows whose kafka_topic is this value; its kafka_timestamp is within the
    export period. rhs_last_month_2026-08-11_to_2026-09-11/export_metadata.json says that export is filtered by kafka_timestamp from 2026-08-11 to 2026-09-11.

  - Structural evidence: view_definition_snapshot.csv (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__view_definition_snapshot.csv),
    view_name=premium, view_definition, includes the asset.

  - No direct access-log query_sql reference to the physical asset was found, and no captured query referenced the premium view.

  The topic/data-row evidence is evidence of exported records, not evidence that a user or process queried the asset.

  2. [INTERNAL_ASSET_18]

  Conclusion: no, the files do not support a conclusion that it is safe to delete.

  - Structural evidence: asset_inventory_snapshot.csv (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__asset_inventory_snapshot.csv),
    asset_name and asset_type=BASE TABLE, lists it throughout the snapshots.

  - Structural/dependency evidence: view_definition_snapshot.csv (rhs_last_month_2026-08-11_to_2026-09-11/
    platform_observability__rhs__shared_source__view_definition_snapshot.csv), view_name=structurecomponent, view_definition, contains it.

  - Observed access evidence: none was captured directly or through structurecomponent.

  That absence does not establish safety: deleting it could break the defined view, and the access log does not cover all consumers or all of August.

  3. View dependency alone

  Conclusion: no. A view-to-asset relationship is structural evidence, not evidence of observed use.

  - view_definition_snapshot.csv (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__view_definition_snapshot.csv) records view_name and
    static SQL in view_definition.

  - data_access_event CSV (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv) records observed queries
    through query_sql.

  For example, premium and structurecomponent have dependencies but no captured view query. partyroleinclaim has both a dependency and captured queries. The dependency
  therefore cannot establish use by itself.

  4. [INTERNAL_ASSET_10]

  Conclusion: yes—indirect, derived usage evidence exists. There is no direct physical-asset query reference.

  Evidence chain:

  1. data_access_event CSV (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv), fields event_timestamp,
     query_id, source_observation_count, and query_sql:
      - 2026-09-02T22:56:56.951000+00:00
      - query ID 20260902_225604_23872_nj8h2
      - source_observation_count=1
      - query reads shared_source_layer.t360_2624.PartyRoleInClaim.
      - There are 55 syntactic source-relation references to this view in the captured log.

  2. view_definition_snapshot.csv (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__view_definition_snapshot.csv), fields
     snapshot_date=2026-09-02, view_name=partyroleinclaim, and view_definition:
      - its UNION ALL definition explicitly includes the requested asset.

  3. asset_inventory_snapshot.csv (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__asset_inventory_snapshot.csv), fields asset_name and
     asset_type=BASE TABLE, establishes the referenced object is an inventoried base table.

  This attributes the observed view access to the asset through the static definition. It does not provide a query plan proving that this individual branch executed.

  5. root_view_name and target_asset_name

  Conclusion: those exact fields are absent.

  Closest supported concepts:

  - view_name in view_definition_snapshot.csv: the defined view.
  - asset_name in asset_inventory_snapshot.csv: an inventoried asset.
  - view_definition in view_definition_snapshot.csv: the SQL from which a view-to-asset relationship can be derived.

  No supplied field formally labels a view as “root” or an asset as “target.”

  6. dependency_depth

  Conclusion: absent. No field or supported equivalent describes dependency depth.

  Therefore dependency depth cannot be used as usage evidence here; it is not present at all.

  7. Assets with both direct and view-attributed usage

  Conclusion: none are supported under a strict observed-query rule.

  The only apparent physical-asset text match was [INTERNAL_ASSET_09]. All three occurrences were inside quoted error/log text in
  INSERT INTO pod_logs queries, rather than source-relation access to that asset. They are not direct usage evidence.

  8. Assets with no supported usage evidence

  A complete “not observed during 2026-08-11 to 2026-09-11” list cannot be responsibly produced because the access log covers only September 2026:

  - ehs metadata (rhs_last_month_2026-08-11_to_2026-09-11/ehs__data_access_event__ws_claims_dk_bi__2026-09_metadata.json), filter, limits observed_at to September.
  - Inventory snapshots cover 2026-08-11 through 2026-09-10.

  For the narrower captured-query slice, 1,311 of 2,627 unique inventoried base tables had no direct physical-access evidence and were not reachable from any of the 43
  captured view accesses. That means only “not observed in this captured query dataset,” not unused, obsolete, or safe to delete.

  The PMS and Norkart assets are among that narrower set; the Tiacodan PartyRoleInClaim asset is not.

  9. candidate_for_review

  Conclusion: absent. No field or classification with that name was found in the supplied files.

  Accordingly, the files provide no supported meaning for such a classification and cannot establish obsolescence or deletion safety from it.

  10. Clearest view-to-base-asset example

  PartyRoleInClaim → [INTERNAL_ASSET_10]

   Step                       File and field                                                          Establishes
  ━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   Observed query             data_access_event…csv: event_timestamp, query_id, query_sql             On 2026-09-02, query 20260902_225604_23872_nj8h2 read
                                                                                                      shared_source_layer.t360_2624.PartyRoleInClaim.
  ─────────────────────────  ──────────────────────────────────────────────────────────────────────  ───────────────────────────────────────────────────────────────────────
   Observed-query metadata    Same file: source_observation_count=1                                   The logged record carries one source observation; the field does not
                                                                                                      itself prove per-branch execution.
  ─────────────────────────  ──────────────────────────────────────────────────────────────────────  ───────────────────────────────────────────────────────────────────────
   Static dependency          view_definition_snapshot.csv: snapshot_date, view_name,                 The 2026-09-02 definition for partyroleinclaim contains a UNION ALL
                              view_definition                                                         source reference to the requested base asset.
  ─────────────────────────  ──────────────────────────────────────────────────────────────────────  ───────────────────────────────────────────────────────────────────────
   Asset identity             asset_inventory_snapshot.csv: asset_name, asset_type                    The requested name is an inventoried BASE TABLE.

  Evidence model:

  - Direct usage evidence: an access-event query_sql referencing a physical asset as a SQL source relation. None was validated here.
  - Indirect usage evidence: an observed query of a view plus that view’s static view_definition referencing an asset. The Tiacodan asset is the clear example.
  - Structural-only evidence: inventory membership, asset type, application/topic mappings, and view-definition references without a corresponding observed query.
  - Unsupported conclusions: unused, obsolete, safe to delete, access outside the captured namespace/time window, successful execution, and execution of every branch of a
    view’s definition.

  No repository files were changed.

─ Worked for 19m 40s ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/raw                                          │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Analyze linked data files                                                 │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a091bb-4e57-75d2-9763-8d543ead33ef                                      │
│                                                                                                  │
│  Token usage:          149K total  (123K input + 26.6K output)                                   │
│  Context window:       54% left (125K used / 258K)                                               │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
