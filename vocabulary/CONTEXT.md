# Platform Observability Domain

This glossary defines the language used to describe data assets and the evidence used to assess their usage.

## Language

**Asset**:
A queryable relation whose usage is assessed. An Asset is a Table or a View; applications, actors, event streams, and evidence producers are not Assets.
_Avoid_: Resource, object, query-log source

**Business Concept View**:
A View that represents a business concept and exposes the Tables that contribute data to that concept.
_Avoid_: Semantic view, owning view

**Business Concept**:
A domain meaning represented by exactly one Business Concept View within the assessed Asset population. A Relationship View does not represent a single Business Concept.
_Avoid_: View, table

**Relationship View**:
A View that represents an association between business concepts rather than a business concept itself.
_Avoid_: Join view, join-table view

**Contributing Table**:
A Table that supplies data to exactly one Business Concept View or Relationship View.
_Avoid_: Owned table, child table

**Asset Dependency**:
A structural relationship in which a View definition references another Asset. An Asset Dependency is not evidence that either Asset was used.
_Avoid_: Ownership, usage

**Usage Evidence**:
An observation supporting that an Asset was accessed directly or through a queried View.
_Avoid_: Asset dependency, view definition

**Observed Usage**:
Asset usage supported by Usage Evidence during an Observation Period.
_Avoid_: Actual usage, lifetime usage

**Declared Consumption**:
A registered relationship stating that a consumer is expected to consume an Asset. Declared Consumption is not evidence that access occurred.
_Avoid_: Usage evidence, observed usage

**Direct Usage Evidence**:
Usage Evidence produced when an access event explicitly references the Asset.
_Avoid_: Direct query-log evidence

**View-derived Usage Evidence**:
Usage Evidence attributed to a Contributing Table because an access event references its View.
_Avoid_: Indirect usage, inferred ownership

**Queried Asset**:
The Asset explicitly referenced by an access event.
_Avoid_: Root asset

**Attributed Asset**:
The Asset to which Usage Evidence is assigned. It is the Queried Asset for direct evidence and a Contributing Table for view-derived evidence.
_Avoid_: Target asset

**Actor**:
A person, service account, or application that initiates an access to an Asset.
_Avoid_: User

**Evidence Producer**:
A system that reports an observed operation from which Usage Evidence may be derived. An Evidence Producer is not necessarily the Actor.
_Avoid_: Actor, data source

**Observed Operation**:
A successful operation reported by an Evidence Producer. It may represent maintenance, metadata inspection, or access to an Asset.
_Avoid_: Access event, usage

**Access Event**:
An eligible Observed Operation that explicitly references at least one Queried Asset and can produce Usage Evidence.
_Avoid_: Query-log record, observed operation

**Observation Period**:
The bounded interval during which Access Events are considered when assessing Asset usage.
_Avoid_: Lifetime, retention period

**Usage Evidence Profile**:
The classification of an Asset as `UNOBSERVED`, `DIRECT_ONLY`, `VIEW_DERIVED_ONLY`, or `DIRECT_AND_VIEW_DERIVED` within an Observation Period.
_Avoid_: Precedence-based usage status

**Usage Assessment**:
The evaluation of an Asset's Usage Evidence Profile for an Observation Period.
_Avoid_: Lifetime usage status

**Review Candidate**:
An Unobserved Asset selected for human investigation, regardless of whether Declared Consumption exists. A Review Candidate is not necessarily obsolete or approved for deletion.
_Avoid_: Deletion candidate, unused asset

**Unobserved Asset**:
An Asset for which no Usage Evidence exists within the assessed observation period.
_Avoid_: Unused asset, never-used asset
