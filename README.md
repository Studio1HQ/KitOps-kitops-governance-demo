<div align="center">

<img src="https://raw.githubusercontent.com/Studio1HQ/Studio1HQ/main/assets/banners/KitOps-kitops-governance-demo.webp" alt="AI Governance Gates with KitOps and OPA: CI governance gates for ModelKits using OPA policies and Cosign attestations" width="100%" />

<br />
<br />

# AI Governance Gates with KitOps and OPA

### CI governance gates for ModelKits using OPA policies and Cosign attestations

A GitHub Actions demo that packs ModelKits, checks governance metadata against OPA policies, blocks failing models and pushes passing ones to Jozu Hub with a signed attestation.

<br />

[![Built by Studio1](https://img.shields.io/badge/Built_by_Studio1-f97316?style=for-the-badge&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNjAgMTYwIiBmaWxsPSJub25lIj48cGF0aCBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGNsaXAtcnVsZT0iZXZlbm9kZCIgZD0iTTAgMzVDMCAxNi42ODA3IDE0LjA3NDQgMS42NDg0NCAzMiAwLjEyNjk1M1YxNDFINTEuMDUyM0M1Mi40MzA4IDEzNy4zNDQgNTQuNzQ0NiAxMzMuNjg5IDU3Ljk5MzggMTMwLjAzM0M2MS4xNDQ2IDEyNi41MDggNjQuNjg5MiAxMjMuNDQgNjguNjI3NyAxMjAuODI5QzcyLjU2NjIgMTE4LjM0OSA3Ni4zNTY5IDExNi43ODIgODAgMTE2LjEyOVY4Ni43NTQ0Qzc1LjQ3MDggODcuOTI5MiA3MS4zODQ2IDg5LjY5MTkgNjcuNzQxNSA5Mi4wNDE1QzY0IDk0LjUyMjUgNjAuNzUwOCA5Ny40NiA1Ny45OTM4IDEwMC44NTRDNTYuNTM0NSAxMDIuNzIgNTUuMjU0NSAxMDQuNjU5IDU0LjE1MzkgMTA2LjY3MVYwSDEyNUMxNDQuMzMgMCAxNjAgMTUuNjY5OSAxNjAgMzVWMTI1QzE2MCAxNDQuMzMgMTQ0LjMzIDE2MCAxMjUgMTYwSDEyNFYxOEgxMDQuOTQ4QzEwMy41NjkgMjEuNjgxNiAxMDEuMjU1IDI1LjM2MjggOTguMDA2MiAyOS4wNDQ0Qzk0Ljg1NTQgMzIuNTk0MiA5MS4zMTA4IDM1LjY4NDEgODcuMzcyMyAzOC4zMTRDODMuNDMzOCA0MC44MTIgNzkuNjQzMSA0Mi4zODk2IDc2IDQzLjA0NzRWNzIuNjMwNEM4MC41MjkyIDcxLjQ0NzMgODQuNjE1NCA2OS42NzI0IDg4LjI1ODUgNjcuMzA1N0M5MiA2NC44MDc2IDk1LjI0OTIgNjEuODQ5MSA5OC4wMDYyIDU4LjQzMDdDOTkuNDY1NSA1Ni41NTEzIDEwMC43NDUgNTQuNTk4NiAxMDEuODQ2IDUyLjU3MjNWMTYwSDM1QzE1LjY3IDE2MCAwIDE0NC4zMyAwIDEyNVYzNVoiIGZpbGw9IiNmZmZmZmYiLz48L3N2Zz4K)](https://studio1hq.com)
[![Built with Jozu](https://img.shields.io/badge/Built_with_Jozu-141414?style=for-the-badge)](https://jozu.com)

[Jozu docs](https://jozu.com/docs) &nbsp;·&nbsp; [Jozu](https://jozu.com) &nbsp;·&nbsp; [KitOps](https://kitops.org) &nbsp;·&nbsp; [More examples](https://github.com/Studio1HQ) &nbsp;·&nbsp; [studio1hq.com](https://studio1hq.com)

</div>

<br />

## Overview

A demo of real CI/CD governance gates for AI models using [KitOps](https://kitops.org), [Open Policy Agent](https://openpolicyagent.org) (OPA) and Cosign attestations, with [Jozu Hub](https://jozu.com) as the registry. A GitHub Actions workflow runs four scenarios in parallel, one compliant model and three that should be blocked.

Governance inputs are read from structured `model.parameters.governance` metadata in each Kitfile, not from free-text descriptions. Passing ModelKits are pushed to Jozu Hub with a signed attestation that downstream policy engines can use.

## Features

- **Pack**: `kit pack` builds a ModelKit from `model-artifacts/` with a scenario Kitfile.
- **Policy checks**: Four OPA modules cover license compliance, approval validation, bias monitoring and security thresholds.
- **Block in CI**: Any failed policy stops the workflow before the push.
- **Push and attest**: Only passing models are tagged, pushed to Jozu Hub and attested with keyless Cosign.
- **Evidence artifact**: Each job writes `policy-results/policy-summary.json`.
- **Four scenarios**: Compliant, bias violation, expired approval and license violation.

## Tech stack

| Layer | Tool |
| --- | --- |
| Packaging | [KitOps](https://kitops.org) (`jozu-ai/gh-kit-setup` action) |
| Policy engine | [Open Policy Agent](https://openpolicyagent.org) (Rego) |
| Signing | [Cosign](https://docs.sigstore.dev/cosign/) via Sigstore |
| Registry | [Jozu Hub](https://jozu.ml) |
| CI | [GitHub Actions](https://github.com/features/actions) |

## Getting started

### Prerequisites

- A GitHub account to fork the repo and run Actions
- A Jozu Hub account and repository
- [OPA](https://www.openpolicyagent.org/docs/latest/#running-opa) for local checks (`brew install opa`)

### Installation

```bash
git clone https://github.com/Studio1HQ/KitOps-kitops-governance-demo.git
cd KitOps-kitops-governance-demo
brew install opa
```

### Environment variables

Set these as GitHub repository secrets for the workflow:

| Variable | Description |
| --- | --- |
| `JOZU_USERNAME` | Jozu Hub username used to log in from the workflow. |
| `JOZU_PASSWORD` | Jozu Hub password. |

Keep the workflow permissions `contents: read` and `id-token: write` (needed for keyless Cosign), and change the image references in `.github/workflows/main.yml` to your own Jozu repository (they currently point to `jozu.ml/arnabchat2001/kitops-governance-demo`).

### Run

In CI: fork the repo, open **Actions**, choose **AI Governance Gate** and select **Run workflow**. Four matrix scenarios run in parallel:

| Scenario | Result | Primary reason |
| --- | --- | --- |
| Pass - Compliant Model | PASS | All policies pass |
| Block - Bias Violation | FAIL | `demographic_parity` below `0.80` |
| Block - Expired Approval | FAIL | `approval_date` older than 90 days |
| Block - License Violation | FAIL | `GPL-3.0` not in allowed license set |

The license violation scenario also carries `high_count: 8` to exercise the security policy.

Locally, evaluate a policy against a scenario Kitfile:

```bash
opa eval --data policies/bias-monitoring.rego \
  --input kitfile-examples/fraud-detection-bias-violation.yaml \
  --format pretty "data.governance.bias.deny"
```

## How it works

```text
.github/workflows/main.yml   pack -> validate -> push -> attest
kitfile-examples/            Four governance scenarios (YAML)
policies/                    OPA modules (*.rego)
model-artifacts/             Kitfile, model.py, inference.py, MODEL_CARD.md
mock-scan-results/           clean-scan.json, vulnerable-scan.json
```

Governance metadata contract used by the policies:

```yaml
model:
  parameters:
    governance:
      risk_level: high
      regulatory_scope: "EU AI Act Article 12"
      bias_metrics:
        demographic_parity: 0.96
        equalized_odds: 0.94
      performance_metrics:
        precision: 0.94
        recall: 0.89
      approval:
        approved_by: alice@company.com
        approval_date: "2026-01-15T10:00:00Z"
      scan_metrics:
        vulnerability:
          critical_count: 0
          high_count: 2
        malware:
          threats_detected: 0
        secrets:
          count: 0
```

For PASS scenarios the workflow creates a keyless attestation:

```bash
cosign attest --yes \
  --predicate policy-results/policy-summary.json \
  --type "https://jozu.com/attestations/kitops-governance/v1" \
  jozu.ml/arnabchat2001/kitops-governance-demo:<git-sha>
```

The predicate includes the scenario name, git SHA, policy deny arrays per control (license, approval, bias, security) and the extracted governance scan metrics. See `SETUP.md` and `JOZU_AUTH.md` for setup notes.

## Resources

- [KitOps documentation](https://kitops.org/docs/overview/): ModelKit and Kitfile reference.
- [Jozu documentation](https://jozu.com/docs): Jozu Hub guides.
- [Jozu](https://jozu.com): Product site.
- [Open Policy Agent](https://openpolicyagent.org): Rego policy language docs.

<br />

---

<div align="center">

<a href="https://studio1hq.com">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-dark.svg" />
    <img src="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-light.svg" alt="Studio1" width="160" />
  </picture>
</a>

Built by **[Studio1](https://studio1hq.com)**, the technical content and DevRel partner for devtool teams. Made with [Jozu](https://jozu.com).<br />
Browse more open-source examples at [github.com/Studio1HQ](https://github.com/Studio1HQ).

[![Website](https://img.shields.io/badge/studio1hq.com-0b0807?style=for-the-badge&logo=googlechrome&logoColor=white)](https://studio1hq.com)
[![Book a call](https://img.shields.io/badge/Book_a_strategy_call-f97316?style=for-the-badge&logo=googlecalendar&logoColor=white)](https://cal.com/studio1/collab)
[![X](https://img.shields.io/badge/%40Studio1HQ-141414?style=for-the-badge&logo=x&logoColor=white)](https://x.com/Studio1HQ)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/studio1hq/)

<sub><a href="https://studio1hq.com">studio1hq.com</a> · Technical content, docs, video and DevRel for devtools</sub>

</div>
