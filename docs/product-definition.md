# Product Definition

## Product Overview

Acervo combines automated documentation with a persistent knowledge model that continuously builds and organizes knowledge about a software system from reliable sources and validated human context.

## Problem

Organizations struggle to maintain an accurate, accessible, and continuously evolving understanding of their software systems because knowledge is distributed across code, documentation, changes, and people.

## Target Users

Acervo is intended for software teams and organizations that need to create, maintain, understand, or communicate knowledge about their software systems.

The initial product is particularly relevant to development teams because it addresses the recurring effort of creating and maintaining software documentation. However, the resulting knowledge is not restricted to developers and may provide value to technical and non-technical roles, including technical leads, architects, product and project roles, QA and testing teams, and new team members.

Acervo does not restrict its users based on organizational role. If a person can obtain meaningful value from the knowledge generated about a software system, they are a potential user of the product.

The specific roles that derive the most value from Acervo, as well as the eventual buyer or decision-maker, remain commercial hypotheses to be validated.

## Core Value

Acervo is built around three core sources of value:

### Time

Acervo aims to reduce the time teams spend creating, maintaining, and recovering knowledge about their software systems, allowing them to dedicate more time to development, analysis, design, and other high-value activities.

### Knowledge

Acervo centralizes and structures knowledge about software systems, connecting information from reliable sources and validated human context so that understanding the system does not depend solely on isolated documents or individual people.

### Automation

Acervo automates repetitive knowledge-management activities, starting with the creation and maintenance of software documentation and progressively enabling additional capabilities built on top of the same knowledge model.

These three values are interconnected: Acervo uses automation to transform distributed information into accessible knowledge, with the goal of reducing the time required to understand and maintain software systems.

## Initial Product Flow

The initial Acervo product will follow a simple knowledge-to-documentation workflow:

1. **Source ingestion**  
   Acervo receives information from a supported source, such as a Git repository, existing documentation, or a system snapshot.

2. **System analysis**  
   Acervo analyzes the available source and identifies relevant system information and changes.

3. **Knowledge model update**  
   The extracted information is incorporated into the Acervo knowledge model together with its supporting sources and context.

4. **Documentation generation**  
   Acervo generates standardized documentation from the available knowledge.

5. **Human validation**  
   The generated documentation is delivered for review by the appropriate people within the organization.

6. **Knowledge confirmation and enrichment**  
   Validated information and relevant corrections or context are incorporated back into the knowledge model.

The initial validation interface may use email. This is considered an implementation choice for the MVP rather than a fundamental part of the product.

## Product Boundaries

Acervo is designed to build and maintain knowledge from available sources while preserving traceability, human oversight, and clear boundaries around what the product is responsible for.

### Acervo does not modify the customer's system

Acervo does not modify source code, configurations, deployments, or other elements of the customer's software system as part of its initial product.

Its role is to observe, analyze, organize, and generate knowledge and documentation from available sources.

### Acervo does not invent knowledge

Acervo should not present unsupported information as fact.

When available evidence is insufficient, Acervo should identify the lack of evidence rather than fabricate missing context.

Inferences may be used when appropriate, but they must remain distinguishable from directly observed or explicitly provided information.

### Acervo does not replace human validation

Generated knowledge and documentation remain subject to review and approval by the appropriate people within the organization.

Acervo supports and scales organizational knowledge; it does not assume responsibility for determining whether business or technical information is ultimately correct.

### Acervo does not assume knowledge when sources are insufficient

Acervo should be able to operate with incomplete information without presenting an artificially complete representation of a system.

Lack of evidence is itself relevant information and should be reflected when it affects the reliability of the resulting knowledge.

### Acervo does not attempt to implement the entire product family initially

Capabilities such as security analysis, risk analysis, system visualization, conversational interfaces, impact analysis, and other potential capabilities are part of the long-term product direction but are outside the initial product scope.

### Acervo does not promise infallible knowledge

Acervo aims to maximize the reliability and traceability of its knowledge through reliable sources, supporting evidence, contextual information, and human validation.

However, the correctness of the resulting knowledge ultimately depends on the quality and accuracy of the sources and context available to the system.

## Future Capabilities

The Acervo product family can progressively expand by building new capabilities on top of the same underlying knowledge model.

Potential future capabilities include:

### Knowledge Access

Provide conversational and interactive access to the knowledge of a software system, allowing technical and non-technical users to explore system behavior, terminology, relationships, and business context while maintaining traceability to supporting sources.

### System Visualization

Generate diagrams and visual representations of system architecture, dependencies, components, and processes from the relationships represented in the knowledge model.

### Onboarding

Provide new team members with structured and contextual access to the knowledge required to understand a software system, its terminology, processes, and architecture.

### Risk and Security Analysis

Use the knowledge model to identify potential risks, vulnerabilities, dependencies, and security concerns within the context of the system.

### Impact Analysis

Analyze how changes to components, services, APIs, or business processes may affect other parts of the system and its associated documentation or projects.

### Additional Knowledge-Based Capabilities

The knowledge model may support additional capabilities as new organizational needs are identified and validated.

These capabilities are potential directions rather than commitments for the initial product. Their development will depend on the maturity of the knowledge model, validated user needs, and future product research.