 Sources: mart__asset_usage_assessment.csv, mart__table_usage_attribution.csv, and mart__view_asset_dependency_bridge.csv.

  1. Both. [INTERNAL_ASSET_12] has 3 direct_query_log events and 133 view_dependency events (136 total).
     Support: assessment counts direct_access_event_count=3, view_dependency_access_event_count=133; attribution evidence_type.

  2. No—safe deletion is not supported. It has zero observed events and is candidate_for_review, but the bridge records it as a dependency target of view structurecomponent at depth
     1. That establishes a current dependency relationship, even though it does not establish observed use.
     Support: assessment zero-count fields and usage_status; bridge root_view_name=structurecomponent, dependency_depth=1.

  3. No. The bridge proves a recorded dependency relationship, not observed access. The Norkart porch-dimensions table is a direct counterexample: it appears in the bridge but has
     zero attribution events and zero assessment access counts.
     Support: bridge target_asset_name; assessment access-count fields; absence of matching attribution rows.

  4. Its observed usage was entirely view-derived: 331 view_dependency events, attributed through root view partyroleinclaim; no direct events were recorded.
     Support: assessment direct_access_event_count=0, view_dependency_access_event_count=331, root_view_name=partyroleinclaim; attribution evidence_type=view_dependency and
     root_asset_name=partyroleinclaim.

  5. target_asset_name identifies the asset/table whose usage is being assessed. root_view_name identifies the root view through which view-derived evidence was attributed. In the
     attribution file, the corresponding field is named root_asset_name.
     Example: target [INTERNAL_ASSET_12] received 133 view-derived events through root premium.
     Support: bridge/attribution target_asset_name; assessment/bridge root_view_name; attribution root_asset_name.

  6. No. dependency_depth describes the recorded dependency path only. The porch-dimensions table has dependency_depth=1 but zero observed access events.
     Support: bridge dependency_depth=1; assessment zero-count fields; no matching attribution rows.

  7. Assets with both direct and view-derived evidence (positive values in both assessment count fields) are:
      - [INTERNAL_ASSET_12]
      - [INTERNAL_ASSET_05]
      - [INTERNAL_ASSET_17]
      - [INTERNAL_ASSET_04]
      - [INTERNAL_ASSET_08]
      - [INTERNAL_ASSET_02]
      - [INTERNAL_ASSET_15]
      - [INTERNAL_ASSET_03]
      - [INTERNAL_ASSET_11]
      - [INTERNAL_ASSET_01]
      - [INTERNAL_ASSET_16]
      - [INTERNAL_ASSET_07]
      - [INTERNAL_ASSET_06]
      - [INTERNAL_ASSET_14]

     Support: assessment direct_access_event_count > 0 and view_dependency_access_event_count > 0.

  8. There is no literal unobserved status. Within these extracts, the unobserved set is the 545 assessment rows with usage_status=candidate_for_review: all have zero total, direct,
     and view-derived event counts, and zero root_view_count. This is “unobserved in these files,” not proof of no use anywhere.
     Support: assessment usage_status and all four count fields.

  9. No. candidate_for_review records a review candidate, not obsolescence or deletion safety. The porch-dimensions example is such a candidate yet still has a recorded bridge
     dependency.
     Support: assessment usage_status=candidate_for_review; bridge dependency row.

  10. To explain a table’s observed usage, use attribution rows matching its target_asset_name, then separate by evidence_type:

  - direct_query_log = direct recorded evidence.
  - view_dependency = attributed evidence; root_asset_name identifies the recorded root view.
  - Confirm totals and the summarized root view in the assessment’s direct/view count fields and root_view_name.
  - Use the bridge only to show the dependency relationship and depth—not usage.

  For example, the premium table has 133 view-derived events attributed through premium, plus 3 direct events; the PartyRoleInClaim table has 331 view-derived events through
  partyroleinclaim and no direct events. The files do not contain SQL text, so they support attribution via the recorded root-view fields, not the exact query text.

─ Worked for 1m 27s ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.152.0)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            C:\tmp\experiment\raw                                                     │
│  Permissions:          Full Access                                                               │
│  Agents.md:            <none>                                                                    │
│  Thread name:          Audit asset usage and deletion                                            │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a08ebe-7064-7590-8643-c7f3e2afda30                                      │
│                                                                                                  │
│  Token usage:          37.1K total  (31.5K input + 5.54K output)                                 │
│  Context window:       91% left (34.3K used / 258K)                                              │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
