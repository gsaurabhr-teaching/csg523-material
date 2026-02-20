# Lab: Firmware Architecture Design  
## From Behavioral Models to Structured Implementation

> **Course:** CS G523 – Software for Embedded Systems  
> **Submission Type:** Group document with individual module ownership  
> **Objective:** Translate behavioral models into disciplined firmware structure

---

## Purpose of This Lab

By the end of this lab, your team will produce:

- A justified architectural structure
- A hierarchical control decomposition
- Explicit dependency constraints
- Module ownership and specifications
- Light structural safeguards
- Module-level test definitions

## Submission
Use the template provided [here](https://github.com/gsaurabhr-teaching/csg523-material/blob/main/labs/lab4-group-sample.md) to complete the tasks given below. Then add it to your repository under `docs/sections/architecture.md`.

---

## Step 1 – Identify Software Blocks (Exploratory Sketch)

Create a preliminary block diagram (hand-drawn or digital).

This sketch should:

- Identify major software blocks
- Show rough relationships
- Be exploratory (no need for polish)

Purpose: Encourage unconstrained architectural thinking before formalization.

---

## Step 2 – Hierarchy of Control Diagram (Mermaid)

Construct a hierarchical of control diagram showing the relationship between modules.

Each node must include:

- Responsibility (what it does, ideally a single item)
- Encapsulated state (placeholder initially)
- Public interface (placeholder initially)
- Named owner (after task split,which you will do later)

You must explicitly declare:

System control authority resides in: ____________  
System state is owned by: ____________

---

## Step 3 – Dependency Constraints

Explicitly define:

- Allowed dependency directions
- Forbidden dependencies
- Global state policy
- Policy on cross-module data sharing

---

## Step 4 – Behavioral Mapping Table

Map structure back to modeling artifacts.

| Module | Related States | Related Transitions | Related Sequence Diagrams |
|--------|---------------|--------------------|---------------------------|

Every structural module must be behaviorally justified.

---

## Step 5 – Interaction Summary

Create a coupling summary table:

| Module | Calls | Called By | Shared Data? |
|--------|-------|----------|-------------|

This reveals structural tight coupling early.

## Step 6 - Architectural rationale

## Step 7 – Task Split

Divide modules among team members.

| Member | Module(s) Owned |
|--------|------------------|

Each member is responsible for:

- Interface definition
- Encapsulation rules
- Safeguards
- Module-level tests

> Commit and add your document to the repository at this stage.
> All team members can work on their own parts from here onwards in the document.
---

## Step 8 – Individual Module Specification

Each member must add a section for their module:

## Module: ___________________

### Purpose and Responsibilities
- ___________________________

### Inputs
- Events received:
- Data received:
- Assumptions about inputs:

### Outputs
- Events emitted:
- Commands issued:
- Guarantees provided:

### Internal State (Encapsulation)
- State variables:
- Configuration parameters:
- Internal invariants:

### Initialization / Deinitialization
- Init requirements:
- Shutdown behavior:
- Reset behavior:

### Basic Protection Rules (Light Safeguards)

- What inputs are validated?
- What invalid conditions are rejected?
- What invariants are enforced?
- Where are errors escalated?

Full fault handling is NOT required yet.

### Module-Level Tests

| Test ID | Purpose | Stimulus | Expected Outcome |
|--------|---------|----------|------------------|

---

## Step 9 – Update Hierarchy Diagram

After module specifications are completed:

- Update the hierarchy diagram  
**Each student must do it for their own modules and push the commit to GitHub.**
- Replace placeholders with finalized interfaces
- Ensure consistency between module specs and diagram

---

## Step 10 – Architectural Risk Statement

Identify one architectural risk.

Explain:

- Why it is a risk
- How it might be mitigated later

---

## Submission Checklist

- [ ] Exploratory block sketch included
- [ ] Architectural rationale provided
- [ ] Hierarchical control diagram complete
- [ ] Dependency constraints defined
- [ ] Behavioral mapping table complete
- [ ] Interaction summary table complete
- [ ] Task split defined
- [ ] Individual module specifications complete
- [ ] Hierarchy diagram updated
- [ ] Architectural risk identified

---

A weak architecture here will create persistent downstream problems.
