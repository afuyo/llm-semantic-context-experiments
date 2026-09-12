# LLM Semantic Context Experiments

> **Shouldn't a capable LLM be able to construct the context it needs from raw information?**

This repository contains a small experiment exploring that question.

I compared an LLM answering the same enterprise data-usage questions under three conditions:

```text
RAW
data only

RAW + VOCABULARY
data + controlled vocabulary

RAW + VOCABULARY + ONTOLOGY
data + controlled vocabulary + ontology
```

## Why?

We spend considerable effort onboarding people into an enterprise: its terminology, systems, relationships, conventions and exceptions.

A foundation model does not automatically know that private, current enterprise context.

But perhaps a sufficiently capable model can reconstruct it from the underlying evidence.

This experiment asks not only whether it **can**, but what that reconstruction costs.

## What happened?

The RAW agent was surprisingly capable. Even from relatively low-level evidence, it reconstructed much of the required semantic context.

As the questions became harder and more evidence-oriented, however, explicit semantic context started to show benefits.

In the final experiment, compared with RAW, the ontology-equipped agent was approximately:

| | RAW | Vocabulary | Ontology |
|---|---:|---:|---:|
| Time | 19m 40s | 12m 34s | **9m 32s** |
| Total tokens | 149K | 152K | **136K** |
| Output tokens | 26.6K | 25.4K | **12.1K** |

It reached essentially the same core conclusions in about **half the time** and with roughly **half the output tokens**, while being more disciplined about separating observed evidence, structural relationships and unsupported conclusions.

There was also an interesting failure: the ontology-equipped agent became slightly **too cautious** and failed to surface one useful conclusion that the RAW agent could derive from the evidence.

That trade-off may be more interesting than the performance improvement itself.

## Working hypothesis

A strong LLM can reconstruct a surprising amount of enterprise context from raw evidence.

But reconstruction is work.

Explicit semantic context can reduce that work, make reasoning more consistent, and make the context reusable.

This feels much like onboarding people:

> **A capable employee could probably reverse-engineer much of the enterprise context too. We still onboard them.**

The engineering challenge is therefore not merely to provide context, but to validate that the context helps without becoming a constraint on exploration.

> **The ontology should guide inference, not constrain reality.**

## Repository contents

```text
competency-questions/   Questions used to anchor the ontology
ontology/               Generated ontology
vocabulary/             Controlled vocabulary
results/                Original RAW / Vocabulary / Ontology runs
reports/                Analysis and conclusions
```

No enterprise source data is included in this repository.

## Read more

Start with the report in [`reports/`](reports/) for the experiment design, detailed results, limitations and conclusions.

This is an exploratory experiment, not a scientific benchmark. The next step is repeated runs with fixed prompts, competency questions and model versions.