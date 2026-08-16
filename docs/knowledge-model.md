# Focus Knowledge Model

## Status

**Version:** 0.1
**Status:** Conceptual model
**Scope:** Initial Focus product

This document defines the conceptual knowledge model for Focus. It describes the domain entities, their purpose, relationships, and the principles that guide the model.

This is not a physical database schema. Implementation details such as identifiers, foreign keys, junction tables, indexes, storage engines, and exact constraints will be decided during logical and physical data modeling.

---

## Core Principle

Focus builds a persistent, traceable understanding of a software system from sources, source-level events, evidence, facts, validation, and human context.

Sources provide information. A **Commit** is a source-level event, especially a Git commit. A **ChangeEvent** is a change identified by Focus and can be derived from one or more source events. **Evidence** is concrete information obtained from sources. **Facts** are concrete assertions derived from evidence. **Knowledge** is accepted and consolidated understanding; a Fact does not become Knowledge automatically.

The conceptual flow is:

**Source → Commit → ChangeEvent**
**Source → Evidence → Fact → Knowledge → Document**

Evidence can originate independently from a ChangeEvent. The two paths can therefore meet through shared evidence and facts, but neither requires the other.

---

# Entities

## Person

### Purpose

Represents a person involved in project activity, review, validation, approval, or human context.

### Relationships

- A Person can participate in multiple Projects.
- A Person can be associated with multiple Commits and ChangeEvents.
- A Person can provide or validate human context represented as Evidence and Facts.
- A Person can review multiple Facts and Documents.
- A Person can approve multiple Documents.

### Important principle

Approval and validation authority are determined by organizational role and configured policies, not hard-coded into Person.

---

## Project

### Purpose

Represents a project worked on by the organization. It provides context for related Features and participating Persons.

### Relationships

- A Project can have multiple Persons.
- A Project contains multiple Features.

### Important principle

A Project does not directly own a System or Rule in the initial model when those relationships can be derived through Features.

---

## Feature

### Purpose

Represents a functionality or capability associated with a Project.

### Relationships

- A Feature belongs to one Project.
- A Feature can be affected by multiple ChangeEvents.
- A Feature can relate to multiple Systems, Knowledge units, Rules, and Documents.

---

## Source

### Purpose

Represents a source from which Focus obtains information.

Examples include Git repositories, production environments, documentation repositories, snapshots, external systems, and other trusted information sources.

### Relationships

- A Source can contain or provide multiple Commits.
- A Source can provide multiple Evidence items.

### Important principle

A Source is not Knowledge. It provides information that must be represented as Evidence and assessed through Facts and validation.

---

## Commit

### Purpose

Represents a source-level event, especially a Git commit. It records what was committed at the source level; it is not the same thing as the change Focus identifies from that source information.

### Relationships

- A Commit originates from one Source; a Source can contain multiple Commits.
- A Commit can be associated with multiple Persons, such as authors or committers.
- A Commit can contribute to one or more ChangeEvents.

### Important principle

A Commit is an immutable historical source event. New commits do not overwrite earlier commits or the historical information derived from them.

---

## ChangeEvent

### Purpose

Represents a change identified by Focus. A ChangeEvent may be derived from one or more source events, including Commits.

Examples include an identified functional change, release, deployment, snapshot difference, or other relevant system change.

### Relationships

- A ChangeEvent can be derived from multiple Commits, and a Commit can contribute to multiple ChangeEvents.
- A ChangeEvent can affect multiple Features and impact multiple Systems.
- A ChangeEvent can relate to multiple Rules and involve multiple Persons.
- A ChangeEvent can be associated with Evidence, but Evidence does not require a ChangeEvent.

### Important principle

A ChangeEvent represents what Focus identified as a change. It does not itself determine why it happened, whether it is correct, whether it violates a Rule, or whether a resulting Fact is true.

---

## Evidence

### Purpose

Represents concrete information obtained from one or more Sources.

Evidence may originate from a Commit or ChangeEvent, but it is not required to do so. For example, Evidence can be obtained independently from a CRM, production system, documentation, snapshot, or validated human context.

### Relationships

- A Source can provide multiple Evidence items, and an Evidence item can be supported by multiple Sources.
- Evidence can be associated with a ChangeEvent when relevant.
- An Evidence item can support multiple Facts.
- Evidence remains traceable from the Facts and Knowledge it supports.

### Important principle

Evidence is concrete source material, not an accepted assertion or Knowledge by itself.

---

## Fact

### Purpose

Represents a concrete assertion derived from one or more Evidence items.

### Classification

Every Fact has one of these classifications:

- `OBSERVED` — directly observed from source-derived evidence.
- `HUMAN_CONTEXT` — context supplied by a person.
- `INFERRED` — assertion inferred from available evidence.

### Lifecycle state

Every Fact has one of these lifecycle states:

- `NOT_CHECKED`
- `IN_REVIEW`
- `CHECKED`
- `REJECTED`
- `SUPERSEDED`

### Relationships

- A Fact is derived from one or more Evidence items; Evidence can support multiple Facts.
- A Fact can support one or more Knowledge units only when accepted or consolidated through the applicable review and validation process.
- A Fact can participate in multiple Discrepancies.
- A Fact can be reviewed or validated by Persons.
- A newer Fact can supersede a previous Fact without overwriting it.

### Important principles

A Fact does not automatically become Knowledge. `CHECKED` means its review or validation state has been recorded; acceptance into Knowledge remains a separate consolidation decision.

Human-context Facts require validation before they can support trusted Knowledge. Rejected Facts remain historically traceable with their `REJECTED` state; they are not physically deleted.

---

## Knowledge

### Purpose

Represents accepted and consolidated knowledge about the software system, functionality, rules, context, or behavior.

### Relationships

- Knowledge can be supported by multiple Facts and the Evidence underlying them.
- Knowledge can relate to multiple Features, Systems, and Rules.
- Knowledge can evolve through versions that supersede previous versions.
- Knowledge can be used to generate multiple Documents.

### Important principles

Knowledge is not a synonym for Evidence or Fact. It represents accepted/consolidated understanding and must remain traceable to its supporting Facts and Evidence.

When new commits or other information change the system, new Facts and/or Knowledge versions are created as needed. Historical information is not overwritten or destroyed.

---

## Evaluator

### Purpose

Represents a capability that evaluates Evidence and Facts for inconsistencies, insufficient evidence, and potential discrepancies.

### Relationships

- An Evaluator can assess multiple Evidence items and Facts.
- An Evaluator can identify or report multiple Discrepancies.

### Important principle

An Evaluator does not determine truth and does not directly create, modify, accept, reject, or otherwise change Knowledge. Validation and review decisions remain explicit and traceable through Fact lifecycle state and human review.

---

## Discrepancy

### Purpose

Represents a conflict or potential conflict involving one or more Facts.

### Relationships

- A Discrepancy involves one or more Facts; a Fact can participate in multiple Discrepancies.
- A Discrepancy can be detected or reported by an Evaluator.

### Important principle

There is no separate Resolution entity in v0.1. A discrepancy is addressed through explicit validation/review decisions and the affected Facts' lifecycle state, while preserving the historical record.

---

## System

### Purpose

Represents a software system or broader technical system involved in the organization's activities.

### Relationships

- A System can relate to multiple Features, Knowledge units, Rules, ChangeEvents, and Documents.

---

## Rule

### Purpose

Represents a business rule, regulation, policy, or other rule that applies to the software system or its Features.

### Relationships

- A Rule can apply to multiple Features and Systems.
- A Rule can relate to multiple ChangeEvents and Knowledge units.

### Important principle

A ChangeEvent related to a Rule does not automatically violate that Rule. Violation, risk, and compliance analysis are future analytical capabilities.

---

## Document

### Purpose

Represents documentation generated by Focus from the Knowledge Model.

### Relationships

- A Document can be generated from multiple Knowledge units.
- A Document can document multiple Features and Systems.
- A Document can be reviewed and approved by multiple Persons.
- A Document can supersede a previous Document version.

### Important principle

Documents are representations of Knowledge, not Knowledge itself. They preserve traceability to the Knowledge from which they were generated.

---

# Relationship Summary

| Relationship | Cardinality | Meaning |
| --- | --- | --- |
| Person ↔ Project | N:N | People can participate in multiple projects. |
| Project → Feature | 1:N | A project contains features; a feature belongs to one project. |
| Source → Commit | 1:N | A source contains source-level commits. |
| Person ↔ Commit | N:N | People can be associated with commits. |
| Commit ↔ ChangeEvent | N:N | A ChangeEvent can be derived from multiple commits; a commit can contribute to multiple identified changes. |
| Feature ↔ ChangeEvent | N:N | A feature can be affected by many changes. |
| Feature ↔ System | N:N | A feature can relate to multiple systems. |
| ChangeEvent ↔ System | N:N | A change can impact multiple systems. |
| ChangeEvent ↔ Rule | N:N | A change can relate to multiple rules. |
| Person ↔ ChangeEvent | N:N | A person can be involved in multiple identified changes. |
| Rule ↔ Feature / System | N:N | A rule can apply to multiple features and systems. |
| Source ↔ Evidence | N:N | Evidence can have one or more supporting sources. |
| ChangeEvent ↔ Evidence | N:N, optional | Evidence can be associated with a ChangeEvent, but does not require one. |
| Evidence ↔ Fact | N:N | Facts are derived from one or more evidence items. |
| Person ↔ Fact | N:N | People can review or validate Facts. |
| Fact ↔ Knowledge | N:N | Accepted/consolidated Facts can support Knowledge; no automatic promotion occurs. |
| Evidence ↔ Knowledge | N:N | Knowledge preserves traceability to its supporting evidence. |
| Fact → Fact | Versioned | A newer Fact can supersede, without overwriting, an earlier Fact. |
| Fact ↔ Discrepancy | N:N | A discrepancy involves one or more Facts. |
| Evaluator ↔ Evidence / Fact | N:N | An evaluator assesses evidence and facts for insufficiency or inconsistency. |
| Evaluator ↔ Discrepancy | N:N | An evaluator can identify or report discrepancies. |
| Knowledge ↔ Feature / System / Rule | N:N | Knowledge is contextualized through these domain concepts. |
| Knowledge → Knowledge | Versioned | A newer Knowledge version can supersede an older one. |
| Document ↔ Knowledge | N:N | A document can be generated from multiple Knowledge units. |
| Document ↔ Feature / System | N:N | A document can cover multiple features and systems. |
| Person ↔ Document | N:N | People can review or approve documents. |
| Document → Document | Versioned | A newer document version can supersede an older one. |

---

# Deliberately Excluded Relationships

The following direct relationships remain outside the initial conceptual model because they would introduce redundancy or bypass the Knowledge Model:

- Source → Project
- Project → System
- Project → Rule
- Evidence → Feature
- Evidence → System
- Evidence → Rule

For example, `Evidence → Fact → Knowledge → Feature / System / Rule` keeps accepted Knowledge as the contextual layer. Evidence can still be independently associated with a ChangeEvent when that association is useful and traceable.

No separate **Resolution** entity is introduced. Review and validation decisions, Fact lifecycle state, and historical traceability model how a discrepancy is addressed.

---

# Model Principles

1. **Evidence before inference.** Focus prefers assertions supported by reliable evidence over unsupported inference.
2. **Commit and ChangeEvent are distinct.** A Commit is a source-level event; a ChangeEvent is the change Focus identifies from one or more source events.
3. **Facts are explicit and classified.** Every Fact records its classification and lifecycle state.
4. **Facts are not automatically Knowledge.** Knowledge requires explicit acceptance/consolidation and remains traceable to Facts and Evidence.
5. **Human context requires validation.** Human-context Facts cannot support trusted Knowledge until validated.
6. **Evaluators assist, not decide truth.** Evaluators detect insufficient evidence, inconsistencies, and potential discrepancies; they do not directly modify Knowledge.
7. **Historical information is preserved.** Rejected and superseded Facts, earlier Commits, Knowledge versions, and Document versions remain traceable rather than overwritten or deleted.
8. **Avoid redundant relationships.** Store a direct relationship only when it carries information that cannot be reliably derived through existing relationships.
9. **Conceptual model first.** Database-specific implementation decisions are deliberately deferred.

---

# Scope of v0.1

This model supports the initial Focus product flow:

**Source → Commit → ChangeEvent (when identified)**

**Source → Evidence → Fact → review and validation → Knowledge → Document**

A ChangeEvent can be associated with Evidence when relevant, but the Evidence path remains independent.

The model deliberately does not define a physical PostgreSQL schema, automatic truth determination, automatic Fact-to-Knowledge promotion, a separate Resolution entity, detailed evaluator implementation, retention policy, or deletion behavior beyond the requirement to preserve historical traceability.

Potential future capabilities include knowledge interaction/chat, system diagrams, risk analysis, security analysis, onboarding assistance, historical system exploration, advanced retention and archival, and additional source integrations.
