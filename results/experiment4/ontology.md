 I found no deletion-safety conclusion in these files. The data separates observed query text from structural snapshots; a dependency alone is not usage evidence.

  1. [INTERNAL_ASSET_12]

  Conclusion: no observed-usage evidence was found.

  - Structural evidence: asset inventory (rhs_last_month_2026-08-11_to_2026-09-11/platform_observability__rhs__shared_source__asset_inventory_snapshot.csv) fields
    asset_name, asset_type list it as BASE TABLE. View snapshot (rhs_last_month_2026-08-11_to_2026-09-11/
    platform_observability__rhs__shared_source__view_definition_snapshot.csv) fields view_name=premium and view_definition contain the asset name.

  - Observed evidence: no matching asset reference, nor a premium view query, was found in access events (rhs_last_month_2026-08-11_to_2026-09-11/
    platform_observability__ehs__data_access_event__ws_claims_dk_bi__2026-09.csv) query_sql.

  This means “not observed in this supplied access-event export,” not unused, obsolete, or safe to delete. It could have been accessed outside this namespace, before
  September, through unlogged/dynamic SQL, or by a representation not matched by the search.

  2. [INTERNAL_ASSET_18]

  Conclusion: no, it cannot be concluded safe to delete.

  - Structural evidence: inventory fields asset_name, asset_type identify it as a BASE TABLE; view-snapshot view_name=structurecomponent has a view_definition containing
    this asset.

  - Observed evidence: neither field says the asset is obsolete, unused, deletable, or reviewed for deletion. The view dependency is a reason for caution, not evidence of
    runtime use.

  3. Does a view-to-asset relationship prove use?

  Conclusion: no.

  - Structural/dependency evidence: view_definition_snapshot.view_name plus view_definition show that a view definition references an asset.
  - Observed evidence is separate: access-event fields event_type, event_timestamp, observed_at, and especially query_sql record a trino_query.

  A view can be defined without being queried during the observed interval. A query of the view plus its definition can support an indirect attribution to an underlying
  asset, subject to identifier matching.

  4. [INTERNAL_ASSET_10]

  Conclusion: there is conditional, indirect attribution evidence; there is no direct event naming this base asset.

  Evidence chain:

  1. Access event: access_event_id=idis:20260902_225604_23872_nj8h2, event_type=trino_query, observed_at=2026-09-02T22:56:56.951+00:00; its query_sql queries
     "shared_source_layer"."t360_2624"."PartyRoleInClaim".

  2. View snapshot dated 2026-09-02: view_name=partyroleinclaim; its view_definition contains [INTERNAL_ASSET_10].
  3. Inventory snapshot: asset_name identifies that object and asset_type=BASE TABLE.

  The first step is observed query evidence; the second is structural dependency evidence. The final attribution is conditional because the files do not explicitly map
  quoted PartyRoleInClaim to lowercase partyroleinclaim; their case-normalization relationship must not be assumed.

  5. root_view_name and target_asset_name

  Conclusion: those exact fields do not exist.

  Closest supported concepts:

  - view_definition_snapshot.view_name: the view whose definition is captured.
  - Table identifiers embedded in view_definition: structural references to underlying assets.
  - asset_inventory_snapshot.asset_name: an inventory item, not a per-dependency target field.

  Thus, view_name is the wrapper/view object; an embedded table reference is a dependency target; asset_name is merely a separately inventoried asset.

  6. dependency_depth

  Conclusion: no dependency_depth field or equivalent explicit classification exists in the supplied schemas.

  Therefore its meaning cannot be established from these files, and it cannot be used as usage evidence. The available structural relationship is the textual reference in
  view_definition; usage evidence remains in access-event records.

  7. Assets with both direct and view-attributed usage

  Conclusion: none can be identified conclusively from the supplied fields.

  No field classifies an asset as directly used or view-attributed. query_sql can directly evidence a query of the object named in that SQL; view_definition can
  structurally map a view to a base asset. The supplied files do not provide normalized runtime lineage from a query to a physical asset. The examples above are indirect/
  conditional attributions, not direct base-asset observations.

  8. Assets with no supported usage evidence

  Conclusion: a complete list cannot be produced safely.

  The access-event metadata in event metadata (rhs_last_month_2026-08-11_to_2026-09-11/ehs__data_access_event__ws_claims_dk_bi__2026-09_metadata.json) limits the query
  evidence to namespace ws-claims-dk-bi and the September 2026 monthly slice. Other export files use the broader August 11–September 11 filter, but they are data extracts,
  not access observations.

  Within that access export:

  - [INTERNAL_ASSET_12]: not observed.
  - [INTERNAL_ASSET_18]: not observed.
  - The PartyRoleInClaim asset has only the conditional indirect chain described above.

  “Not observed in this dataset” does not mean unused, obsolete, or safe to delete.

  9. candidate_for_review

  Conclusion: no candidate_for_review field or classification exists in the supplied files.

  Even if such a label existed, it would require its own documented semantics before it could establish obsolescence or deletion safety. No such evidence is present here.

  10. Clearest view-to-base attribution candidate

  The clearest candidate is [INTERNAL_ASSET_19]:

  1. Access event file, fields event_type, observed_at, query_sql:
      - Event idis:20260910_160434_08880_nsesy
      - event_type=trino_query
      - observed_at=2026-09-10T16:04:36.705000+00:00
      - query_sql contains from "shared_source_layer"."t360_2624"."TxSurveyQuestion" as t
      - Establishes: a query of that named view-like object was observed.

  2. View snapshot, fields snapshot_date, view_name, view_definition:
      - snapshot_date=2026-09-10
      - view_name=txsurveyquestion
      - view_definition references [INTERNAL_ASSET_19]
      - Establishes: the captured lower-case view definition structurally references that base asset.

  3. Asset inventory, fields asset_name, asset_type:
      - Lists the target as BASE TABLE.
      - Establishes: the referenced object is inventoried as a base table.

  This is indirect attribution, not direct physical-runtime lineage. As with question 4, the case difference between TxSurveyQuestion and txsurveyquestion is not explicitly
  normalized by the data, so the final connection is conditional.

  Inferred evidence model:

  - Direct usage evidence: an access-event record whose query_sql explicitly queries the relevant object.
  - Indirect usage evidence: an observed query of a view, combined with that view’s captured view_definition referencing a base asset.
  - Structural-only evidence: inventory presence, asset_type, and an asset occurrence in view_definition.
  - Unsupported conclusions: successful execution, rows actually read, complete consumer coverage, non-use, obsolescence, deletion safety, or case-normalized identity
    between differently spelled identifiers.

  Changed files: none.

─ Worked for 9m 32s ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

• Completed `/root/dependency_model_retry`

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.153.4)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            ~/cursor_projects/experiment/ontology                                     │
│  Permissions:          Full Access                                                               │
│  Agents.md:            ~/.codex/AGENTS.md                                                        │
│  Thread name:          Inspect linked data files                                                 │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a091bb-bab0-7200-a81b-2eb1a608331a                                      │
│                                                                                                  │
│  Token usage:          136K total  (124K input + 12.1K output)                                   │
│  Context window:       54% left (124K used / 258K)                                               │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
