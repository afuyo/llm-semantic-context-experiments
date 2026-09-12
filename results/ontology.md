 All conclusions are scoped to the supplied assessment data.

  1. Q1 — Both. [INTERNAL_ASSET_12] has 3 direct and 133 view-derived access events. The usage_status says observed_direct, but the two count fields establish
     both evidence types.
     Support: mart__asset_usage_assessment.csv fields direct_access_event_count, view_dependency_access_event_count; mart__table_usage_attribution.csv field evidence_type.

  2. Q2 — No. It is unobserved in this assessment (all three event counts are zero) and marked candidate_for_review, but that does not establish it is safe to delete. It also appears
     structurally as a dependency of structurecomponent, which is not usage evidence.
     Support: assessment fields access_event_count=0, direct_access_event_count=0, view_dependency_access_event_count=0, usage_status=candidate_for_review; bridge row with
     root_view_name=structurecomponent, dependency_depth=1; CONTEXT.md definitions of Asset Dependency and Review Candidate.

  3. Q3 — No. Presence in mart__view_asset_dependency_bridge.csv proves only a view-to-asset structural dependency. The Q2 table is a direct counterexample: it occurs in the bridge
     but has zero usage events.
     Support: CONTEXT.md “Asset Dependency … is not evidence that either Asset was used”; the two CSV rows above.

  4. Q4 — View-derived usage through partyroleinclaim. The table has 331 view-dependency events, zero direct events, and one root view: partyroleinclaim. Attribution rows identify
     root_asset_name=partyroleinclaim, evidence_type=view_dependency, and dependency_depth=1.
     Support: mart__asset_usage_assessment.csv fields access_event_count=331, direct_access_event_count=0, view_dependency_access_event_count=331, root_view_name=partyroleinclaim;
     mart__table_usage_attribution.csv fields above.

  5. Q5 — target_asset_name is the asset receiving the attribution; root_view_name identifies the view through which view-derived attribution arose. In the bridge, they describe the
     structural path from a root view to its dependent asset. In attribution data, the corresponding source-view field is named root_asset_name; for direct evidence it equals the
     target table, while for view-derived evidence it is the queried view.
     Support: CSV headers; CONTEXT.md definitions of “Queried Asset,” “Attributed Asset,” and “View-derived Usage Evidence.”

  6. Q6 — No. dependency_depth describes the length/depth of a dependency path; it is not usage evidence. A depth-1 bridge relationship can still have zero observed access, as Q2
     demonstrates.
     Support: platform-observability-ontology.ttl defines dependencyDepth on AssetDependency; CONTEXT.md says Asset Dependency is not Usage Evidence; Q2 rows.

  7. Q7 — 14 assets have both evidence types:
      - [INTERNAL_ASSET_12] — 3 direct, 133 view-derived
      - [INTERNAL_ASSET_05] — 1, 18,482
      - [INTERNAL_ASSET_17] — 1, 49
      - [INTERNAL_ASSET_04] — 1, 36
      - [INTERNAL_ASSET_08] — 1, 1,468
      - [INTERNAL_ASSET_02] — 6, 3,728
      - [INTERNAL_ASSET_15] — 1, 49
      - [INTERNAL_ASSET_03] — 2, 3,412
      - [INTERNAL_ASSET_13] — 1, 133
      - [INTERNAL_ASSET_01] — 1, 3,430
      - [INTERNAL_ASSET_16] — 1, 49
      - [INTERNAL_ASSET_07] — 1, 1,468
      - [INTERNAL_ASSET_06] — 5, 1,468
      - [INTERNAL_ASSET_14] — 1, 133

     Support: mart__asset_usage_assessment.csv, selecting rows where both direct_access_event_count and view_dependency_access_event_count are greater than zero.

  8. Q8 — The unobserved assets are exactly the 545 assessment rows with access_event_count=0 (and both component counts zero). In this dataset, all 545 are labelled
     candidate_for_review; Q2’s table is one of them. The files do not label these as “never used,” only unobserved during the assessment period.
     Support: mart__asset_usage_assessment.csv count fields and usage_status; CONTEXT.md definition of Unobserved Asset.

  9. Q9 — No. candidate_for_review means an unobserved asset was selected for human investigation. It expressly does not mean obsolete, approved for deletion, or safe to delete.
     Support: CONTEXT.md definition of Review Candidate; platform-observability-ontology.ttl comment on edu:ReviewCandidate.

  10. Q10 — Explain it by separating the queried view from the attributed table. Filter mart__table_usage_attribution.csv on the table’s target_asset_name; then use evidence_type,
     root_asset_name, and dependency_depth to state whether access was direct or view-derived and name the queried view. Validate totals with the assessment’s direct/view-derived
     count fields and root_view_name.

  For the Q4 table: the table is the attributed target, while partyroleinclaim is the queried view; its 331 observed events were attributed through that depth-1 view dependency, not
  through direct table queries.
  Support: mart__table_usage_attribution.csv fields named above; mart__asset_usage_assessment.csv count and root_view_name fields.

─ Worked for 2m 13s ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.152.0)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            C:\tmp\experiment\ontology                                                │
│  Permissions:          Full Access                                                               │
│  Agents.md:            <none>                                                                    │
│  Thread name:          Answer asset dependency questions                                         │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a08ebe-b34e-7d41-b29f-690489fa0c9c                                      │
│                                                                                                  │
│  Token usage:          49.6K total  (43.7K input + 5.88K output)                                 │
│  Context window:       86% left (45.6K used / 258K)                                              │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
