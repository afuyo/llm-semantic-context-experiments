 I used the supplied export only. The access-log metadata limits observed access evidence to September 2026, despite the directory/export relation window starting 2026-08-
  11.

  Files used:

  - access-event metadata (rhs_last_month_2026-08-11_to_2026-09-11/ehs__data_access_event__ws_claims_dk_bi__2026-09_metadata.json)
  - access events (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv)
  - asset inventory (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__asset_inventory_snapshot.csv)
  - view definitions (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__view_definition_snapshot.csv)

  1. [INTERNAL_ASSET_12]

  Conclusion: no supported usage evidence in this dataset.

  - Observed evidence: the access-event file’s query_sql contains no occurrence of the physical asset name and no occurrence of the premium view reference.
  - Structural evidence: the asset inventory’s asset_name/asset_type identifies it as a BASE TABLE; view definitions’ view_name = premium and view_definition include it.
  - Therefore it is structurally depended on by premium, but the supplied access data does not show that asset or that view being queried.

  This absence means only “not observed by this access export.” It does not establish unused, obsolete, or safe to delete.

  2. [INTERNAL_ASSET_18]

  Conclusion: no, it cannot be concluded safe to delete.

  - Structural evidence: inventory fields asset_name and asset_type identify it as a BASE TABLE; the structurecomponent row(s) in view definitions include it in
    view_definition.

  - Observed evidence: no direct physical-name occurrence and no observed structurecomponent view reference were found in query_sql.

  The absence of observed use is not deletion approval. The active structural dependency alone means deleting it may break the view.

  3. Does a view-to-asset dependency prove actual use?

  Conclusion: no.

  - view_definition establishes only that the definition contains a reference to an asset.
  - Actual observed access is represented separately by access-event fields such as event_type, event_timestamp, observed_at, query_id, and especially query_sql.

  The premium example demonstrates the distinction: its definition includes the PMS asset, while the access export contains no observed query reference to premium.

  4. [INTERNAL_ASSET_10]

  Conclusion: there is indirect evidence, with an identifier-normalization caveat.

  Evidence chain:

  1. Access events: record access_event_id = idis:20260902_225604_23872_nj8h2 has event_type = trino_query, event_timestamp/observed_at = 2026-09-02T22:56:56.951000+00:00,
     and query_sql containing:

     from "shared_source_layer"."t360_2624"."PartyRoleInClaim"

  2. View definitions: the 2026-09-02 snapshot has asset_catalog = shared_source_layer, asset_schema = t360_2624, view_name = partyroleinclaim, and view_definition
     containing:

     [INTERNAL_ASSET_10]

  3. Asset inventory: the same 2026-09-02 snapshot identifies that asset_name as BASE TABLE.

  This is indirect evidence because the observed query names the view, not the physical asset. Strictly, the files do not explicitly state that quoted PartyRoleInClaim and
  lower-case partyroleinclaim are the same identifier; the attribution depends on case-normalizing that name. The physical asset name itself is absent from query_sql.

  5. root_view_name and target_asset_name

  Conclusion: those exact fields do not exist.

  Closest supported concepts:

  - view_name in view definitions: the defined view, such as partyroleinclaim.
  - asset_name in inventory: an independently inventoried asset and its asset_type.
  - A target asset in a dependency sense: a literal relation reference inside view_definition; it is not stored in a separate target-asset field.

  Thus, view_name identifies the view definition, while the physical asset reference within view_definition identifies a structural dependency.

  6. dependency_depth

  Conclusion: no such field or concept is present.

  The observability CSV headers contain no depth field, and the available view data contains only view_name, view_definition, and view_definition_hash. Even if a depth
  existed, it would describe structural distance, not observed access; it would not by itself prove use.

  7. Assets with both direct and view-attributed usage

  Conclusion: none can be identified with both forms of supported evidence.

  The access export shows 43 recognized views being queried and supports view-derived attribution for their referenced assets. It does not provide a validated direct
  physical-asset access event for those assets.

  A literal text search finds [INTERNAL_ASSET_09], and that asset is also in the partyroleinactivity definition. However, its
  occurrences are inside an INSERT INTO pod_logs ... VALUES (...) query’s log payload, not an observed source-table reference. The files do not provide a field that
  declares that embedded text to be an accessed source asset, so it is not valid direct-usage evidence.

  8. Assets with no supported evidence of use

  A complete, authoritative list cannot be produced from these files.

  The available matching yields a provisional “not observed by available evidence” set of 1,429 inventory assets: 1,311 base tables and 118 views. This comes from 2,968,521
  access-event records, 43 recognized accessed views, and 1,316 base assets structurally reached through those views.

  However, 13 observed root relation names do not match an inventory VIEW entry or a supplied view definition. Their dependencies are unavailable, so an inventory asset
  could potentially be reached through an unrepresented relation. Therefore the 1,429 set is a candidate set only, not a complete proof of non-use.

  Both requested assets are in that candidate set:

  - [INTERNAL_ASSET_12]
  - [INTERNAL_ASSET_18]

  “Not observed in this dataset” does not mean unused, obsolete, or safe to delete.

  9. candidate_for_review

  Conclusion: no such field or classification exists in the supplied data.

  No field named candidate_for_review, nor a candidate/review classification, appears in the supplied metadata, inventory, view-definition, application-topic, or access-
  event schemas. Consequently, no supplied classification establishes obsolescence or deletion safety.

  10. Clearest base-asset attribution chain

  The strongest available example is the Tiacodan partyroleinclaim asset from question 4:

   Step    File and fields                                                                   What it establishes
  ━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   1       Access events: access_event_id, event_type, event_timestamp, observed_at,         An observed Trino query on 2026-09-02 named PartyRoleInClaim.
           query_id, query_sql
  ──────  ────────────────────────────────────────────────────────────────────────────────  ────────────────────────────────────────────────────────────────────────────────
   2       View definitions: snapshot_date, asset_catalog, asset_schema, view_name,          The same-day partyroleinclaim definition includes the Tiacodan physical asset.
           view_definition, view_definition_hash
  ──────  ────────────────────────────────────────────────────────────────────────────────  ────────────────────────────────────────────────────────────────────────────────
   3       Asset inventory: snapshot_date, asset_catalog, asset_schema, asset_name,          The named target is inventoried as a BASE TABLE.
           asset_type

  This is the clearest indirect chain, not direct physical-asset access. The case difference between PartyRoleInClaim and partyroleinclaim remains an explicit unsupported-
  normalization caveat.

  Evidence model inferred:

  - Direct usage evidence: an observed access-event record whose query_sql actually references the physical asset as a query source. No validated example was found here.
  - Indirect/derived evidence: an observed query of a view plus a snapshot view_definition that contains the asset.
  - Structural-only evidence: inventory membership and a view-definition reference without an observed query of that view.
  - Unsupported conclusions: that an unobserved asset is unused, obsolete, deletable, or that a structural dependency/dependency depth proves runtime use.

  No supplied source data was changed.

─ Worked for 12m 34s ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/vocabulary                                   │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Inspect RHS data files                                                    │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a091bb-8229-77b2-968b-0159e06e0378                                      │
│                                                                                                  │
│  Token usage:          152K total  (126K input + 25.4K output)                                   │
│  Context window:       53% left (129K used / 258K)                                               │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
