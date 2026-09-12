Q1. Both. It has 3 direct events and 133 view-derived events (136 total), with root_view_name=premium.

  Support: mart__asset_usage_assessment.csv fields direct_access_event_count, view_dependency_access_event_count, access_event_count, and root_view_name;
  mart__table_usage_attribution.csv has both direct_query_log and view_dependency evidence types.

  Q2. No. It is unobserved in this observation period, but is not proven safe to delete. It is structurally referenced by the structurecomponent view.

  Support: its assessment row has all three access counts at 0 and usage_status=candidate_for_review; mart__view_asset_dependency_bridge.csv shows root_view_name=structurecomponent,
  dependency_depth=1. CONTEXT.md says a Review Candidate is not necessarily obsolete or approved for deletion.

  Q3. No. Presence in mart__view_asset_dependency_bridge.csv proves only a structural view-to-asset dependency, not use.

  Support: CONTEXT.md, definitions “Asset Dependency” and “Usage Evidence.”

  Q4. Its observed usage was produced entirely by access through the partyroleinclaim view: 331 view-derived events, zero direct events. Four actors generated that evidence:
  ws_bi_rapportering_biprivatdk, ws_claims_dk_bi_cldktrino, ws_marketing_automation_dk_dataexportdbt, and ws_sdp_nordic_group_sdp.

  Support: mart__asset_usage_assessment.csv fields access_event_count=331, direct_access_event_count=0, view_dependency_access_event_count=331, root_view_name=partyroleinclaim,
  distinct_actor_count=4; matching attribution rows have evidence_type=view_dependency and root_asset_name=partyroleinclaim.

  Q5. target_asset_name is the table receiving the attributed evidence. root_view_name is the aggregated view through which view-derived evidence reached that table. At the
  individual-event level, root_asset_name identifies the queried view.

  Support: mart__table_usage_attribution.csv fields target_asset_name, root_asset_name, evidence_type; mart__asset_usage_assessment.csv field root_view_name; definitions of “Queried
  Asset” and “Attributed Asset” in CONTEXT.md.

  Q6. No. dependency_depth describes structural distance in a view dependency; it is not usage evidence. Usage requires an attributed access event and its evidence_type.

  Support: CONTEXT.md, “Asset Dependency” definition; bridge dependency_depth versus attribution evidence_type.

  Q7. Assets with both direct and view-derived evidence:

  - [INTERNAL_ASSET_12]
  - [INTERNAL_ASSET_05]
  - [INTERNAL_ASSET_17]
  - [INTERNAL_ASSET_04]
  - [INTERNAL_ASSET_08]
  - [INTERNAL_ASSET_02]
  - [INTERNAL_ASSET_15]
  - [INTERNAL_ASSET_03]
  - [INTERNAL_ASSET_13]
  - [INTERNAL_ASSET_01]
  - [INTERNAL_ASSET_16]
  - [INTERNAL_ASSET_07]
  - [INTERNAL_ASSET_06]
  - [INTERNAL_ASSET_14]

  Support: mart__asset_usage_assessment.csv, where both direct_access_event_count > 0 and view_dependency_access_event_count > 0 (14 rows).

  Q8. The complete unobserved set is the 545 rows in mart__asset_usage_assessment.csv with all access-count fields zero; each is marked usage_status=candidate_for_review. This
  includes [INTERNAL_ASSET_18].

  Support: access_event_count=0, direct_access_event_count=0, view_dependency_access_event_count=0, and usage_status=candidate_for_review; “Unobserved Asset” definition in
  CONTEXT.md. This means no evidence during the observation period—not “never used.”

  Q9. No. candidate_for_review means an unobserved asset selected for human investigation. It does not mean obsolete or deletable.

  Support: CONTEXT.md, “Review Candidate” definition.

  Q10. To explain a table’s observed usage, use its attribution rows:

  - target_asset_name: the table attributed with evidence.
  - evidence_type: whether the event was direct or view-derived.
  - For view_dependency, root_asset_name is the queried view.
  - Confirm totals and date range in the matching assessment row.

  For example, the party-role table in Q4 was attributed from queries of partyroleinclaim; its 331 events are all view_dependency. Conversely, the premium table in Q1 has three
  direct_query_log rows where both target_asset_name and root_asset_name are the table itself, plus 133 view-derived rows attributed from the queried premium view.

  Support: mart__table_usage_attribution.csv fields above and the corresponding aggregate fields in mart__asset_usage_assessment.csv.

─ Worked for 2m 57s ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


› status


• Complete. All 10 questions were answered from CONTEXT.md and the three CSV files, with supporting fields identified for each conclusion.

/status

╭──────────────────────────────────────────────────────────────────────────────────────────────────╮
│  >_ OpenAI Codex (v0.152.0)                                                                      │
│                                                                                                  │
│  Model:                gpt-5.6-terra (reasoning high, summaries auto)                            │
│  Model provider:       Tryg LiteLLM - https://ai-framework-litellm-prod-oke-thor.k8s.tryg.net/v1 │
│  Directory:            C:\tmp\experiment\vocabulary                                              │
│  Permissions:          Full Access                                                               │
│  Agents.md:            <none>                                                                    │
│  Thread name:          Assess asset usage evidence                                               │
│  Collaboration mode:   Default                                                                   │
│  Session:              01a08ebe-9194-7273-b8a3-24613225c5e2                                      │
│                                                                                                  │
│  Token usage:          59.6K total  (52.9K input + 6.72K output)                                 │
│  Context window:       83% left (52.9K used / 258K)                                              │
│  Limits:               not available for this account                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────╯
