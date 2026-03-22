# TECH-DESIGN-MVP — `zs-pkg-go-fhir`

> **Document:** Technical Design (MVP) | **Version:** 1.0.0-mvp
> **Repository:** [https://github.com/zarishsphere/zs-pkg-go-fhir](https://github.com/zarishsphere/zs-pkg-go-fhir)
> **Layer:** Layer 1B | **Catalog #:** 17
> **Language:** Go 1.26.1 | **License:** Apache 2.0

---

## Technical Summary

**Shared FHIR R5 types, builders, and utility functions wrapping fhir-toolbox-go.**

This document defines the **technical architecture, implementation design, complete repository tree, and acceptance criteria** for the MVP of `zs-pkg-go-fhir`.

---

## Design Principles

1. **No global state** — all dependencies injected via constructors
2. **Context propagation** — every function accepts `context.Context` as first arg
3. **Error wrapping** — all errors wrapped with `fmt.Errorf("...: %w", err)`
4. **Minimal imports** — only stdlib + one direct dependency maximum for MVP
5. **Interface-first** — define interface before struct implementation

## Package Structure

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

## Testing Approach

```go
// Every exported function has a table-driven test
func TestFunctionName(t *testing.T) {
    tests := []struct{
        name    string
        input   InputType
        want    OutputType
        wantErr bool
    }{
        // test cases
    }
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := FunctionName(tt.input)
            if tt.wantErr {
                assert.Error(t, err)
                return
            }
            assert.NoError(t, err)
            assert.Equal(t, tt.want, got)
        })
    }
}
```

## CI Pipeline

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with: {go-version-file: go.mod, cache: true}
      - run: go test ./... -race -coverprofile=coverage.out
      - run: go vet ./...
      - uses: golangci/golangci-lint-action@v6
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
