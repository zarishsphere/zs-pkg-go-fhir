# PRD-MVP — `zs-pkg-go-fhir`

> **Document:** Product Requirements (MVP) | **Version:** 1.0.0-mvp
> **Repository:** [https://github.com/zarishsphere/zs-pkg-go-fhir](https://github.com/zarishsphere/zs-pkg-go-fhir)
> **Layer:** Layer 1B | **Catalog #:** 17
> **Language:** Go 1.26.1 | **License:** Apache 2.0

---

## Executive Summary

**Shared FHIR R5 types, builders, and utility functions wrapping fhir-toolbox-go.**

This document defines the **Minimum Viable Product (MVP)** scope for `zs-pkg-go-fhir` within the ZarishSphere sovereign digital health platform. It covers what must be built first, acceptance criteria, user stories, and the complete repository file structure.


### Platform Non-Negotiables (apply to every repository)

| Constraint | Rule |
|-----------|------|
| **Zero Cost** | All tooling, hosting, and services must use genuinely free tiers |
| **Open Source** | Apache 2.0 license; all code public |
| **FHIR R5 Native** | All clinical data modelled as FHIR R5 resources |
| **Offline-First** | Must function without network connectivity |
| **No-Coder Friendly** | GUI-first, template-driven, automatable |
| **Documentation as Code** | All decisions in GitHub via RFC/ADR |
| **Multi-tenant** | tenant_id scoping on all data operations |
| **HIPAA/GDPR** | AuditEvent on all PHI access; field-level encryption |

---

## Problem Statement

Shared FHIR R5 types, builders, and utility functions wrapping fhir-toolbox-go.

## MVP Goals

1. Export a stable, documented public API
2. Achieve >80% unit test coverage
3. Zero external dependencies beyond what is absolutely necessary
4. Provide usage examples for all major exported symbols
5. Publish as an importable Go module

## MVP Functional Requirements

| ID | Requirement | Acceptance Criteria | Priority |
|----|------------|---------------------|---------|
| M-01 | Exported types compile without error | `go build ./...` succeeds | P0 |
| M-02 | Unit tests pass with race detector | `go test -race ./...` passes | P0 |
| M-03 | GoDoc on all exported symbols | `godoc` shows documentation | P0 |
| M-04 | Example usage in `examples/` or `_test.go` | Example compiles and runs | P1 |
| M-05 | Semantic versioning via release workflow | `v1.0.0` tag triggers CHANGELOG | P1 |

## Exported API (MVP)

### Types
- `Patient, Encounter, Observation, Condition (all FHIR R5 types)`
- `Builder interface for resource construction`
- `FHIRError struct for operation outcomes`

### Functions
- `NewPatient(id, tenantID string) *fhir.Patient`
- `BuildObservation(loincCode, value string, unit string) *fhir.Observation`
- `ToBundle(resources []interface{}) *fhir.Bundle`
- `OperationOutcome(issue string, severity string) *fhir.OperationOutcome`

## MVP Complete Repository Tree

```
zs-pkg-go-fhir/
├── README.md
├── LICENSE
├── go.mod
├── go.sum
├── .github/CODEOWNERS
├── .github/workflows/ci.yml
├── fhir.go
├── builders.go
├── builders_test.go
├── errors.go
├── errors_test.go
└── examples/
    └── create_patient/
        └── main.go
```

---


## Owners & Governance

| Role | GitHub Handle | Responsibility |
|------|--------------|----------------|
| Platform Lead | `@arwa-zarish` | Final approval, RFC votes |
| Technical Lead | `@code-and-brain` | Architecture, Go/TS review |
| DevOps Lead | `@DevOps-Ariful-Islam` | CI/CD, infra, deployment |
| Health Programs | `@BGD-Health-Program` | Clinical content, country programs |

**PR Policy:** All changes via Pull Request. Minimum 1 owner review. CI must pass. No direct commits to `main`.


---

## MVP Acceptance Checklist

- [ ] All MVP files exist in repository with real content (not placeholders)
- [ ] CI pipeline passes on `main` branch
- [ ] No secrets, credentials, or PHI committed
- [ ] README.md reflects current state with setup instructions
- [ ] CODEOWNERS file present
- [ ] All MVP functional requirements verified manually or via automated tests
- [ ] Linked to `CATALOGS.md` and `TODO.md` in `zs-docs-platform`

---

*This document is the authoritative MVP specification for `zs-pkg-go-fhir`.*
*Changes require a Pull Request with at least 1 owner approval.*
