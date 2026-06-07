# Planning Governance Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Establish traceability, risk management, architectural decision control, and cross-domain conventions before technical implementation begins.

**Architecture:** Governance is maintained as a small set of working Markdown documents. It blocks work only for risks involving tenant isolation, permissions, money, inventory, offline sync, public content, privacy, irreversible architecture, or production reliability.

**Tech Stack:** Markdown artifacts under `docs/superpowers/governance`, requirement identifiers, ADR identifiers, risk identifiers, and checklist-driven release gates.

---

### Task 1: Establish Requirements Traceability

**Files:**
- Create: `docs/superpowers/governance/requirements-traceability-matrix.md`

- [ ] Map every approved design area to a requirement identifier, owning domain, implementation phase, and verification type.
- [ ] Reject any phase plan that introduces an unmapped core requirement or silently drops an existing one.

### Task 2: Establish Decision and Risk Control

**Files:**
- Create: `docs/superpowers/governance/risk-register.md`
- Create: `docs/superpowers/governance/adr-log.md`
- Create: `docs/superpowers/governance/domain-dependency-map.md`

- [ ] Record only material risks with an owner and mitigation.
- [ ] Record architecture decisions that are expensive to reverse.
- [ ] Keep dependency direction explicit and prevent circular domain ownership.

### Task 3: Establish Security and Commercial Rules

**Files:**
- Create: `docs/superpowers/governance/permission-entitlement-matrix.md`
- Create: `docs/superpowers/governance/data-classification-privacy-map.md`
- Create: `docs/superpowers/governance/money-time-localization-conventions.md`

- [ ] Distinguish roles from subscription entitlements.
- [ ] Classify personal, private business, public, and sensitive operational data.
- [ ] Fix money, currency, timezone, and Arabic/RTL conventions.

### Task 4: Establish Integration and Media Rules

**Files:**
- Create: `docs/superpowers/governance/external-integration-adapter-policy.md`
- Create: `docs/superpowers/governance/media-asset-policy.md`

- [ ] Keep provider SDKs outside domain logic.
- [ ] Define asset ownership, visibility, validation, and deletion behavior.

### Task 5: Establish Release Gates

**Files:**
- Create: `docs/superpowers/governance/release-gates.md`

- [ ] Define the minimum evidence required to complete each phase.
- [ ] Keep gates executable and risk-based rather than ceremonial.

### Task 6: Verify Governance Pack

- [ ] Run:

```powershell
$files = @(
  'requirements-traceability-matrix.md',
  'risk-register.md',
  'adr-log.md',
  'domain-dependency-map.md',
  'permission-entitlement-matrix.md',
  'data-classification-privacy-map.md',
  'money-time-localization-conventions.md',
  'external-integration-adapter-policy.md',
  'media-asset-policy.md',
  'release-gates.md'
)
$files | ForEach-Object { Test-Path (Join-Path 'docs/superpowers/governance' $_) }
```

Expected: ten `True` values.

