# Focus Knowledge Model

## Status

**Version:** 0.1\
**Status:** Conceptual model\
**Scope:** Initial Focus product

This document defines the initial conceptual knowledge model for Focus.
It describes the entities, their purpose, relationships, and the
principles that guide the model.

This is a conceptual model, not yet a physical database schema.
Implementation details such as foreign keys, junction tables, indexes,
storage engines, and exact constraints will be defined during the
logical and physical data-modeling stages.

------------------------------------------------------------------------

## Core Principle

Focus combines automated documentation with a persistent knowledge model
that continuously builds and organizes knowledge about a software system
from reliable sources and validated human context.

The Knowledge Model is the central layer of the product.

Sources provide information. Evidence represents concrete information
obtained from those sources. Knowledge consolidates and relates
validated information. Documents are generated from the resulting
knowledge.

The conceptual flow is:

**Source → Evidence → Knowledge → Document**

Knowledge acts as the central model through which information is
contextualized and consumed by future capabilities.

------------------------------------------------------------------------

# Entities

## Person

### Purpose

Represents a person involved in the activities, validation, or context
of a software project.

### Relevant information

A Person may contain information such as:

-   Identity
-   Contact information
-   Organizational role

The organizational role can be used to infer responsibilities according
to the organization's configured policies.

### Relationships

-   A Person can participate in multiple Projects.
-   A Person can participate in multiple ChangeEvents.
-   A Person can review multiple Documents.
-   A Person can approve multiple Documents.

### Important principle

Approval authority should not be hard-coded into the Person entity. It
should be determined from the person's role and the organization's
configured validation policies.

------------------------------------------------------------------------

## Project

### Purpose

Represents a project worked on by the organization.

A Project provides context for a set of related Features and the people
participating in the project.

### Relationships

-   A Project can have multiple Persons.
-   A Project contains multiple Features.

### Important principle

A Project does not directly own a System or Rule in the initial model
when those relationships can be derived through Features.

------------------------------------------------------------------------

## Feature

### Purpose

Represents a functionality or capability associated with a Project.

A Feature can represent a small or large change in the functionality of
a Project.

### Relationships

-   A Feature belongs to one Project.
-   A Feature can be affected by multiple ChangeEvents.
-   A Feature can relate to multiple Systems.
-   A Feature can be associated with multiple Knowledge units.
-   A Feature can have multiple Rules applied to it.
-   A Feature can appear in multiple Documents.

------------------------------------------------------------------------

## ChangeEvent

### Purpose

Represents an event in which a change occurs or is detected in the
software system.

Examples may include:

-   Code changes
-   Releases
-   Deployments
-   Snapshots
-   Other relevant system changes

### Relationships

-   A ChangeEvent can affect multiple Features.
-   A ChangeEvent can impact multiple Systems.
-   A ChangeEvent can be detected from multiple Sources.
-   A ChangeEvent can relate to multiple Rules.
-   A ChangeEvent can involve multiple Persons.

### Important principle

A ChangeEvent represents what happened. It should not automatically
imply why it happened, whether it is correct, or whether it violates a
Rule.

------------------------------------------------------------------------

## System

### Purpose

Represents a software system or broader technical system involved in the
organization's activities.

A System can exist independently of a Project and can be involved in
multiple Projects through their Features.

### Relationships

-   A System can be related to multiple Features.
-   A System can be related to multiple Knowledge units.
-   A System can have multiple Rules applied to it.
-   A System can be impacted by multiple ChangeEvents.
-   A System can be described by multiple Documents.

------------------------------------------------------------------------

## Source

### Purpose

Represents a source from which Focus obtains information.

Examples include:

-   Git repositories
-   Production environments
-   Documentation repositories
-   Snapshots
-   External systems
-   Other trusted information sources

### Relationships

-   A Source can provide multiple Evidence items.
-   A Source can be associated with multiple ChangeEvents as the source
    from which those events were detected.

### Important principle

A Source is not automatically equivalent to Knowledge. It provides
information that must be represented as Evidence and incorporated into
the Knowledge Model.

------------------------------------------------------------------------

## Evidence

### Purpose

Represents a concrete piece of information obtained from one or more
Sources.

Evidence is the factual material from which Knowledge can be constructed
or supported.

### Relationships

-   A Source can provide multiple Evidence items.
-   An Evidence item can be supported by multiple Sources.
-   An Evidence item can support multiple Knowledge units.

### Important principle

Evidence should not be forced to originate from a ChangeEvent. Evidence
can exist independently, for example when information comes from a CRM,
production system, documentation, or validated human context.

------------------------------------------------------------------------

## Knowledge

### Purpose

Represents a unit of knowledge about the software system, its
functionality, rules, context, or behavior.

Knowledge is the central component of the Focus Knowledge Model.

### Relationships

-   A Knowledge unit can be supported by multiple Evidence items.
-   A Knowledge unit can relate to multiple Features.
-   A Knowledge unit can relate to multiple Systems.
-   A Knowledge unit can relate to multiple Rules.
-   Knowledge can evolve through versions that supersede previous
    versions.
-   Knowledge can be used to generate multiple Documents.

### Important principle

Knowledge should remain traceable to its supporting Evidence.

Focus should avoid presenting unsupported inference as fact. Where
evidence is insufficient, the model should preserve that uncertainty
rather than inventing context.

### Versioning

Knowledge is intended to preserve historical versions.

When new validated information changes an existing unit of knowledge, a
new version can supersede the previous version rather than overwriting
or destroying historical information.

Historical versions are important because Focus should eventually be
able to represent how the system was understood at a specific point in
time.

Retention, archival, export, and automatic deletion policies are outside
the scope of v0.1.

------------------------------------------------------------------------

## Document

### Purpose

Represents the documentation generated by Focus from the Knowledge
Model.

Documents are a user-facing product output and must be understandable,
traceable, and suitable for human validation.

### Relationships

-   A Document can be generated from multiple Knowledge units.
-   A Document can document multiple Features.
-   A Document can describe multiple Systems.
-   A Document can be reviewed by multiple Persons.
-   A Document can be approved by multiple Persons.
-   A Document can supersede a previous Document version.

### Important principle

Documents are representations of the Knowledge Model. They are not the
Knowledge Model itself.

A Document should preserve traceability to the Knowledge from which it
was generated.

### Versioning

Documents are versioned.

A new document version should not automatically destroy the previous
version. Historical documentation can be relevant for understanding how
a Project or System changed over time.

------------------------------------------------------------------------

## Rule

### Purpose

Represents a business rule, regulation, policy, or other rule that
applies to the software system or its Features.

The initial model uses a type or classification to distinguish different
kinds of rules.

Examples include:

-   Business Rule
-   Regulation
-   Internal Policy

### Relationships

-   A Rule can apply to multiple Features.
-   A Rule can apply to multiple Systems.
-   A Rule can relate to multiple ChangeEvents.
-   A Rule can relate to multiple Knowledge units.

### Important principle

A ChangeEvent being related to a Rule does not automatically mean that
the ChangeEvent violates the Rule.

Violation, risk, or compliance analysis belongs to a future analytical
capability.

------------------------------------------------------------------------

# Relationship Summary

  ------------------------------------------------------------------------
  Relationship                           Cardinality Meaning
  --------------------- ---------------------------- ---------------------
  Person ↔ Project                               N:N People can
                                                     participate in
                                                     multiple projects and
                                                     projects can have
                                                     multiple people.

  Project → Feature                              1:N A project contains
                                                     multiple features; a
                                                     feature belongs to
                                                     one project.

  Feature ↔ ChangeEvent                          N:N A feature can be
                                                     affected by many
                                                     changes and a change
                                                     can affect many
                                                     features.

  ChangeEvent ↔ Source                           N:N A change can be
                                                     detected from
                                                     multiple sources and
                                                     a source can detect
                                                     many changes.

  Source ↔ Evidence                              N:N A source can provide
                                                     many evidence items
                                                     and an evidence item
                                                     can have multiple
                                                     supporting sources.

  Evidence ↔ Knowledge                           N:N Knowledge can require
                                                     multiple evidence
                                                     items and evidence
                                                     can support multiple
                                                     knowledge units.

  Knowledge ↔ Feature                            N:N Knowledge can relate
                                                     to multiple features
                                                     and a feature can
                                                     have multiple
                                                     knowledge units.

  Knowledge ↔ System                             N:N Knowledge can relate
                                                     to multiple systems
                                                     and a system can have
                                                     multiple knowledge
                                                     units.

  Knowledge → Knowledge                    Versioned A newer knowledge
                                                     version can supersede
                                                     an older version.

  ChangeEvent ↔ Rule                             N:N A change can relate
                                                     to multiple rules and
                                                     a rule can relate to
                                                     multiple changes.

  Rule ↔ Feature                                 N:N A rule can apply to
                                                     multiple features and
                                                     a feature can have
                                                     multiple applicable
                                                     rules.

  Rule ↔ System                                  N:N A rule can apply to
                                                     multiple systems and
                                                     a system can have
                                                     multiple applicable
                                                     rules.

  Document ↔ Knowledge                           N:N A document can be
                                                     generated from
                                                     multiple knowledge
                                                     units and knowledge
                                                     can feed multiple
                                                     documents.

  Person ↔ Document                              N:N People can review
  (review)                                           multiple documents
                                                     and documents can be
                                                     reviewed by multiple
                                                     people.

  Person ↔ Document                              N:N People can approve
  (approval)                                         multiple documents
                                                     and documents can
                                                     have multiple
                                                     approvers.

  Document ↔ Feature                             N:N A document can cover
                                                     multiple features and
                                                     a feature can appear
                                                     in multiple
                                                     documents.

  Document ↔ System                              N:N A document can
                                                     describe multiple
                                                     systems and a system
                                                     can have multiple
                                                     documents.

  Document → Document                      Versioned A newer document
                                                     version can supersede
                                                     an older version.

  ChangeEvent ↔ System                           N:N A change can impact
                                                     multiple systems and
                                                     a system can receive
                                                     multiple changes.

  Person ↔ ChangeEvent                           N:N A person can
                                                     participate in
                                                     multiple events and
                                                     an event can involve
                                                     multiple people.
  ------------------------------------------------------------------------

------------------------------------------------------------------------

# Deliberately Excluded Relationships

The following direct relationships are not part of the initial
conceptual model because they would introduce redundancy or bypass the
Knowledge Model:

-   Source → Project
-   Project → System
-   Project → Rule
-   Evidence → Feature
-   Evidence → System
-   Evidence → Rule

For example:

**Rule → Feature → Project**

already provides the project context of a rule applied to a feature.

Likewise:

**Evidence → Knowledge → Feature / System / Rule**

keeps Knowledge as the central layer for contextualizing evidence.

------------------------------------------------------------------------

# Model Principles

## 1. Evidence before inference

Focus should prefer information supported by reliable sources over
unsupported inference.

## 2. Knowledge is traceable

Knowledge should preserve the Evidence that supports it.

## 3. Human context requires validation

Human-provided context can enrich the Knowledge Model, but it should not
automatically become trusted knowledge without the appropriate
validation.

## 4. Historical knowledge is preserved

Updating Knowledge or Documents should not require destroying their
previous versions.

## 5. Avoid redundant relationships

A relationship should only be stored directly when it provides
information that cannot be reliably derived through existing
relationships.

## 6. Conceptual model first

The current model describes the domain. Database-specific implementation
decisions will be made separately.

## 7. The model can evolve

The v0.1 model is a working hypothesis. Real implementation scenarios
may reveal missing relationships, incorrect cardinalities, or entities
that should be decomposed or merged. Such changes should be documented
and versioned rather than silently introduced.

------------------------------------------------------------------------

# Scope of v0.1

This model supports the initial Focus product flow:

**Source → ChangeEvent → Evidence → Knowledge → Document → Human Review
/ Approval**

The broader Knowledge Model is intentionally designed to support future
capabilities without requiring them to be implemented in v0.1.

Potential future capabilities include:

-   Knowledge interaction / chat
-   System diagrams
-   Risk analysis
-   Security analysis
-   Employee onboarding and terminology assistance
-   Historical system exploration
-   Advanced retention and archival
-   Additional source integrations

These capabilities are not part of the initial implementation unless
explicitly added to the product scope.
