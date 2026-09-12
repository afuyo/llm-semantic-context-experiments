# Ontology Context Experiments

## Objective

The experiments test a simple hypothesis:

> **Does giving an AI agent explicit vocabulary and ontology context improve its ability to reason over data compared with giving it the data alone?**

Each experiment compared three progressively richer context conditions:

```text
RAW
data only

RAW + VOCABULARY
data + semantic definitions

RAW + VOCABULARY + ONTOLOGY
data + semantic definitions + explicit ontology
```

The evaluation gradually moved beyond simple correctness toward:

- correctness;
- evidence discipline;
- completeness / evidence recall;
- semantic clarity;
- reasoning effort;
- execution time;
- token usage.

## Important experimental caveat

The three experiments are **not directly comparable as performance benchmarks across rounds**.

Two things changed:

1. **The input data changed.**
   - Experiment 1 used prepared analytical marts.
   - Experiments 2 and 3 used lower-level exports much closer to the raw evidence.

2. **Experiment 3 used a harder prompt and slightly different questions.**
   - The agent had to identify the supporting files and fields.
   - It had to distinguish observed evidence from structural evidence.
   - It had to classify direct versus indirect evidence.
   - It had to reconstruct evidence chains.
   - It was explicitly forbidden from assuming unsupported meanings.
   - It had to state `insufficient information` when appropriate.

The Codex version also differed between Experiment 1 and the later experiments.

Therefore:

> **Performance comparisons should primarily be made between RAW, Vocabulary and Ontology within each experiment, not between experiments.**

---

# Experiment 1 — Prepared analytical data

## Setup

The first experiment used already-prepared analytical datasets such as:

```text
mart__asset_usage_assessment.csv
mart__table_usage_attribution.csv
mart__view_asset_dependency_bridge.csv
```

These files already contained semantic concepts such as:

```text
direct_access_event_count
view_dependency_access_event_count
evidence_type
root_view_name
target_asset_name
dependency_depth
candidate_for_review
```

Much of the interpretation had therefore already been performed upstream.

Conceptually:

```text
raw evidence
     ↓
ETL / semantic interpretation
     ↓
analytical marts
     ↓
agent
```

The RAW agent was not really reasoning over raw evidence. It was consuming data that already encoded the answers.

## Results

All three variants answered the important questions correctly.

They identified, among other things:

- 14 assets with both direct and view-derived evidence;
- 545 assets with no observed usage in the assessment period;
- structural dependency as different from observed usage;
- `candidate_for_review` as a review state rather than proof of obsolescence or deletion safety.

### Performance

| Context | Time | Total tokens | Output tokens |
|---|---:|---:|---:|
| RAW | **1m 27s** | **37.1K** | **5.54K** |
| Vocabulary | 2m 57s | 59.6K | 6.72K |
| Ontology | 2m 13s | 49.6K | 5.88K |

RAW was both fastest and cheapest.

## Conclusion

Experiment 1 showed little measurable benefit from adding vocabulary or ontology.

But this is itself useful.

The analytical marts had already encoded much of the semantic model:

```text
evidence_type
root_view_name
dependency_depth
candidate_for_review
...
```

A strong agent could simply read those fields.

### Lesson

> **If semantic interpretation has already been materialized into the data, adding an ontology may provide relatively little additional value for answering straightforward questions.**

The experiment was therefore too easy to provide strong evidence for ontology-assisted reasoning.

---

# Experiment 2 — Move closer to raw evidence

## Setup

The second experiment removed the prepared analytical marts and instead exposed lower-level exports including:

```text
access events
query_sql
view definitions
asset inventory
export metadata
```

Concepts such as:

```text
root_view_name
target_asset_name
dependency_depth
candidate_for_review
```

were no longer explicitly materialized.

The agent now had to reconstruct relationships itself.

For example:

```text
observed query
      ↓
queried view
      ↓
view definition
      ↓
underlying base asset
```

This was a much harder reasoning problem.

## Results

All three agents eventually reconstructed the essential evidence model:

```text
query observed
      =
runtime evidence


view references table
      =
structural evidence


observed view
      +
view definition references asset
      =
view-derived usage evidence
```

They also correctly distinguished:

```text
dependency ≠ usage

not observed ≠ unused

not observed ≠ obsolete

not observed ≠ safe to delete
```

The RAW agent was capable of reconstructing surprisingly much of the semantic structure directly from the files.

### Performance

| Context | Time | Total tokens | Output tokens |
|---|---:|---:|---:|
| RAW | 14m 37s | **97.5K** | 19.5K |
| Vocabulary | 10m 19s | 110K | **18.9K** |
| Ontology | **10m 15s** | 126K | 20.9K |

Ontology and Vocabulary were faster than RAW, but consumed more total tokens.

## Conclusion

Experiment 2 was much more informative, but still inconclusive.

The semantic context appeared to reduce some reasoning time, but there was no token-efficiency advantage.

More importantly, the experiment demonstrated that:

> **A capable model can reconstruct a surprising amount of semantic structure from sufficiently rich raw evidence.**

This changes the hypothesis.

The value of ontology may not primarily be:

> "The AI can now answer something it could never answer before."

The value may instead be:

> **The AI does not have to reconstruct the same meaning repeatedly for every question.**

Potential benefits therefore include:

- more consistent interpretation;
- less semantic reconstruction;
- more disciplined evidence use;
- more predictable reasoning;
- reusable context across questions.

---

# Experiment 3 — Evidence-first reasoning

## Setup

Experiment 3 used essentially the same lower-level evidence as Experiment 2, but the prompt and questions were sharpened.

This is important.

Experiment 3 was **not simply a repeat of Experiment 2**.

The agent was now explicitly instructed:

> Answer using only the available files.

For every answer it had to:

- state the conclusion;
- identify the supporting file;
- identify the supporting field;
- distinguish observed evidence from structural information;
- distinguish direct from indirect evidence;
- avoid unsupported assumptions;
- explicitly say when information was insufficient.

Several questions were also made more demanding.

For example, instead of simply asking whether an asset was used, the agent had to reconstruct the full evidence chain:

```text
observed access
      ↓
queried object
      ↓
view definition
      ↓
underlying asset
```

Question 8 was also sharpened to ask for assets with **no supported evidence of usage**, while explicitly requiring the distinction between:

```text
"not observed in this dataset"

and

"unused"
"obsolete"
"safe to delete"
```

This required considerably more reasoning than the earlier formulation.

Therefore the longer RAW runtime in Experiment 3 compared with Experiment 2 should **not** be interpreted as performance degradation.

The meaningful comparison is between RAW, Vocabulary and Ontology **inside Experiment 3**, where all three received exactly the same harder task.

---

# Experiment 3 — Results

All three agents converged on essentially the same core evidence model.

## Direct usage

```text
AccessEvent.query_sql
        ↓
physical asset
```

## View-derived usage

```text
AccessEvent.query_sql
        ↓
queried View
        ↓
ViewDefinition
        ↓
BaseAsset
```

## Structural evidence only

```text
ViewDefinition
        ↓
BaseAsset
```

All three also understood that:

```text
StructuralDependency
       does NOT imply
ObservedUsage


NotObserved
       does NOT imply
Unused


NotObserved
       does NOT imply
Obsolete


NotObserved
       does NOT imply
SafeToDelete
```

This is important because the experiment was no longer testing only whether the final answer was right.

It was testing whether the agent could distinguish:

```text
what is known

what is inferred

what is merely structurally related

what cannot safely be concluded
```

---

# Experiment 3 — Performance

| Context | Time | Total tokens | Output tokens |
|---|---:|---:|---:|
| RAW | 19m 40s | 149K | 26.6K |
| Vocabulary | 12m 34s | 152K | 25.4K |
| Ontology | **9m 32s** | **136K** | **12.1K** |

This is the first experiment where a meaningful efficiency signal appears.

Compared with RAW, Ontology was approximately:

- **52% faster**;
- about **9% lower in total token consumption**;
- about **55% lower in output tokens**.

It is therefore not correct to say that Ontology used half the total tokens.

It did, however, produce the answer using roughly half the **output tokens** and approximately half the elapsed time.

That is interesting because Experiment 3 was also the most reasoning-intensive test.

---

# The interesting failure — completeness

The most interesting difference appeared in Question 8.

## RAW

The RAW agent calculated:

> For the narrower captured-query slice, **1,311 of 2,627 unique inventoried base tables** had no direct physical-access evidence and were not reachable from any of the 43 captured view accesses.

It then correctly qualified this:

> This means only **"not observed in this captured query dataset"**, not unused, obsolete or safe to delete.

This is a useful answer because it extracts everything that can safely be supported while maintaining the appropriate caveat.

## Vocabulary

The Vocabulary agent went even further.

It found a provisional set of:

```text
1,429 inventory assets
├── 1,311 base tables
└──   118 views
```

It also discovered that 13 observed root relations could not be mapped to an available view definition, and therefore correctly described the result as provisional rather than authoritative.

## Ontology

The Ontology agent instead answered:

> **Conclusion: a complete list cannot be produced safely.**

It correctly recognized the limitations of the data.

But it stopped there.

It did not produce the narrower but still supportable result that RAW and Vocabulary discovered.

---

# What does completeness mean here?

For this experiment:

> **Completeness is how much of the relevant, supportable evidence needed to answer the question the agent actually surfaces.**

It is not:

```text
answer length
```

and it is not:

```text
willingness to speculate
```

A correct answer can still be incomplete.

For example:

```text
Evidence supports:

1,311 assets are not observed
within this specific captured dataset
```

An answer saying only:

```text
"A complete answer is impossible"
```

is epistemically cautious, but it leaves out a valid conclusion that the evidence supports.

Conversely:

```text
insufficient information
```

is a **fully complete answer** when the evidence genuinely does not exist.

So:

```text
completeness
    =
supportable answer components surfaced
--------------------------------------
supportable answer components available
```

---

# Precision versus recall

Experiment 3 suggests that two properties may be separating.

## Evidence precision

How often are the conclusions the agent makes genuinely supported?

## Evidence recall

How much of the supportable evidence does the agent discover and use?

Conceptually:

| | RAW | Vocabulary | Ontology |
|---|---:|---:|---:|
| Evidence precision | High | High | **Very high** |
| Evidence recall | **Very high** | High | Lower |
| Exploration | **Very high** | High | Lower |
| Semantic discipline | High | High | **Very high** |

The Ontology agent appears to become more disciplined about unsupported conclusions.

That is desirable.

But it may also become **too conservative about exploring evidence outside the semantic paths it has been given**.

---

# Why might ontology reduce completeness?

One possible explanation is that the ontology becomes a strong semantic prior.

With RAW:

```text
data
 ↓
explore
 ↓
discover relationships
 ↓
construct semantics
 ↓
reason
```

With ontology:

```text
ontology
   ↓
known semantic paths
   ↓
reason
```

This is normally exactly what we want.

But an LLM may implicitly begin treating:

```text
not represented in ontology
```

as something closer to:

```text
not safely knowable
```

That can create a form of **semantic authority bias**.

An incomplete ontology can therefore potentially create:

> **schema-induced blindness**

The ontology helps the agent avoid bad inference, but can also discourage useful exploration.

This appears to be what happened in Question 8.

---

# An important distinction — two kinds of evidence gap

The experiments reveal that "missing evidence" actually describes two very different situations.

## 1. Data gap

```text
question
   ↓
required evidence genuinely does not exist
```

Correct response:

```text
Insufficient information.
```

The ontology must **not fill this gap through invention or unsupported inference**.

## 2. Semantic gap

```text
question
   ↓
required evidence exists
   ↓
semantic model does not express
the reasoning path cleanly
```

This is different.

The evidence exists.

The agent simply lacks an explicit semantic route to use it confidently.

Question 8 appears to expose this kind of gap.

That gives us a very useful distinction:

> **DATA GAP — the evidence is missing.**

versus:

> **SEMANTIC GAP — the evidence exists, but our metadata/ontology cannot express it cleanly enough.**

---

# Results across all three experiments

The progression is useful:

```text
EXPERIMENT 1

Prepared semantic marts
        ↓
answers already encoded in data
        ↓
ontology adds little


EXPERIMENT 2

Lower-level evidence
        ↓
agent must reconstruct semantics
        ↓
ontology/vocabulary begin to help
        ↓
but efficiency result is mixed


EXPERIMENT 3

Lower-level evidence
        +
harder evidence-first questions
        ↓
much more reasoning required
        ↓
ontology is significantly faster
and much more concise
        ↓
but becomes more conservative
and loses some completeness
```

The important point is that the strongest ontology signal appears in the **hardest reasoning experiment**, not in the easiest one.

---

# Qualitative assessment of Experiment 3

The following scores are an interpretation of the observed responses, not a scientific benchmark.

| Dimension | RAW | Vocabulary | Ontology |
|---|---:|---:|---:|
| Correctness | 9/10 | 9/10 | 9/10 |
| Evidence discipline | 9/10 | 9.5/10 | **10/10** |
| Completeness / evidence recall | **10/10** | 9.5/10 | 8/10 |
| Semantic clarity | 8/10 | 9/10 | **10/10** |
| Conciseness | 6/10 | 7/10 | **10/10** |
| Reasoning efficiency | 6/10 | 8/10 | **10/10** |

The result should therefore not be summarized as simply:

```text
Ontology wins.
```

It is more interesting than that.

The apparent trade-off is:

```text
                  ONTOLOGY
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
better discipline       less exploration
better precision        lower evidence recall
less reconstruction     possible semantic blindness
```

---

# What the experiments currently suggest

The experiments do **not** show that ontology magically makes an AI agent smarter.

A sufficiently capable model can reconstruct much of the semantic structure from raw data.

But that reconstruction has a cost.

Without reusable context:

```text
question 1
   ↓
rediscover semantics

question 2
   ↓
rediscover semantics

question 3
   ↓
rediscover semantics
```

With semantic context:

```text
declare semantics once
        ↓
reuse
        ↓
question 1
question 2
question 3
```

So a better hypothesis is:

> **Ontology shifts work from repeated runtime reconstruction to reusable declared context.**

Experiment 3 provides the strongest evidence for that hypothesis so far.

The Ontology agent reached essentially the same core conclusions as RAW while:

- finishing in approximately half the time;
- producing roughly half as many output tokens;
- showing stronger evidence discipline;
- making fewer unsupported interpretive leaps.

But it also exposed a failure mode:

> **The semantic model can become too authoritative and discourage the agent from discovering valid evidence that lies outside explicitly represented semantic paths.**

---

# Implication for ontology development

The answer is not simply to make the ontology larger.

It should be sharpened where experiments reveal actual reasoning friction.

Examples include:

```text
Evidence
├── DirectUsageEvidence
├── ViewDerivedUsageEvidence
└── StructuralDependency


ObservationScope
├── namespace
├── observed_from
├── observed_to
└── coverage


Attribution
AccessEvent
   ↓
QueriedAsset
   ↓
ViewDefinition
   ↓
AttributedAsset


IdentityResolution
PartyRoleInClaim
      =
partyroleinclaim
```

The ontology should also explicitly represent important non-implications:

```text
StructuralDependency
    does NOT imply
ObservedUsage


NotObserved
    does NOT imply
Unused


NotObserved
    does NOT imply
Obsolete


NotObserved
    does NOT imply
SafeToDelete
```

But equally important:

> **Absence from the ontology must not be treated as evidence of absence from reality.**

The ontology should guide reasoning, not bound what the agent is allowed to discover.

---

# SELF-STUDY opportunity

This suggests a practical SELF-STUDY loop for metadata and ontology development.

```text
competency questions
        ↓
RAW / VOCAB / ONTOLOGY runs
        ↓
score responses
        ↓
compare reasoning
        ↓
identify gaps
        │
        ├── DATA GAP
        │      evidence missing
        │
        └── SEMANTIC GAP
               evidence exists
               but model cannot express it
                    ↓
          propose metadata /
          ontology improvement
                    ↓
             rerun benchmark
                    ↓
           keep change only if
           results improve
```

Useful scoring dimensions would be:

| Measure | Meaning |
|---|---|
| Correctness | Was the conclusion right? |
| Evidence precision | Were conclusions genuinely supported? |
| Evidence recall | How much supportable evidence was discovered? |
| Completeness | Were all supportable answer components surfaced? |
| Unsupported inference | Did the agent invent missing links? |
| Semantic-gap detection | Did it recognise evidence the ontology could not express? |
| Time | How long did the investigation take? |
| Tokens | How much context/reasoning did it consume? |

This turns ontology development into something testable.

Instead of:

```text
"We think this ontology is useful."
```

we can ask:

```text
Did this ontology change
make the agent:

more correct?
more complete?
more disciplined?
faster?
cheaper?
```

---

# Overall conclusion

Across the three experiments, semantic context becomes more valuable as the task moves away from prepared analytical data and toward reasoning over lower-level evidence.

Experiment 1 showed that ontology adds little when semantic conclusions are already materialized in marts.

Experiment 2 showed that a strong model can reconstruct much of the missing semantic structure from raw evidence, although doing so requires substantial reasoning.

Experiment 3 deliberately made the task harder by strengthening both the prompt and the questions. The agent had to explain not only **what** it concluded, but **why the evidence justified that conclusion**.

Under those conditions, the ontology-equipped agent showed the clearest advantage so far:

> **approximately half the elapsed time, roughly half the output tokens, and stronger evidence discipline than the RAW agent.**

At the same time, Question 8 revealed an important limitation:

> **Ontology-guided reasoning can become overly conservative and fail to surface valid conclusions that are supported by the underlying evidence.**

That is not an argument against ontology-guided reasoning.

It is an argument for treating ontology and metadata like every other engineered component:

> **test it, score it, validate it, find the gaps, improve it, and rerun the same competency questions.**

The current working conclusion is:

> **Ontology does not replace inference. It provides reusable context that can make inference faster, more consistent and more disciplined — provided we validate that it does not accidentally suppress useful exploration.**

Or, in one line:

> **The ontology should guide inference, not constrain reality.**