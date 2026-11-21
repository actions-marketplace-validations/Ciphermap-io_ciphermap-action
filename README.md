# CipherMap PQC & CBOM Scan — GitHub Action

[![Validate Action Metadata & Shell Safety](https://github.com/Ciphermap-io/ciphermap-action/actions/workflows/ci.yml/badge.svg)](https://github.com/Ciphermap-io/ciphermap-action/actions/workflows/ci.yml)
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-CipherMap%20PQC%20%26%20CBOM%20Scan-7c3aed?logo=github)](https://github.com/marketplace/actions/ciphermap-pqc-cbom-scan)
[![Latest Release](https://img.shields.io/github/v/release/Ciphermap-io/ciphermap-action?label=release&color=7c3aed)](https://github.com/Ciphermap-io/ciphermap-action/releases/latest)
[![CycloneDX CBOM](https://img.shields.io/badge/CycloneDX-1.6%20%26%201.7%20CBOM-0ea5e9)](https://cyclonedx.org/tool-center/)
[![SARIF 2.1.0](https://img.shields.io/badge/GitHub%20Code%20Scanning-SARIF%202.1.0-10b981)](https://www.ciphermap.io/github?utm_source=github_action&utm_medium=badge&utm_campaign=marketplace_action&utm_content=sarif_badge)
[![License: MIT](https://img.shields.io/badge/License-MIT-slate.svg)](LICENSE)

Zero-egress post-quantum cryptography (PQC) discovery across **12 languages**, `.jar`/`.whl` archives, `.jks`/`.p12` keystores, and TLS/IaC configs directly inside your GitHub Actions runner — uploading **SARIF 2.1.0** to **GitHub Security → Code Scanning** and generating deterministic **CycloneDX 1.7 (ECMA-424) CBOM** artifacts with **zero source code leaving your workflow**.

- **5-Second Copy-Paste Quickstart** below · **Zero-Install Public Repo Scorecard:** [ciphermap.io/scan](https://www.ciphermap.io/scan?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=public_scan)
- **Signed Offline CLI & Guided Evaluation Key (`CIPHERMAP_LICENSE`):** [ciphermap.io/eval-pack](https://www.ciphermap.io/eval-pack?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=eval_pack) · [Book 20-Min Live Repo Proof](https://www.ciphermap.io/book?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=book_proof) · [Commercial Plans](https://www.ciphermap.io/pricing?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=pricing)
- **Dedicated GitHub Actions & Air-Gapped Runner Guide:** [ciphermap.io/github](https://www.ciphermap.io/github?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=github_guide)
- **3PAO / QSA Assessor Workbench & 1-Page InfoSec Zero-Egress Memo:** [ciphermap.io/for-assessors](https://www.ciphermap.io/for-assessors?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=for_assessors) · [ciphermap.io/procurement-memo](https://www.ciphermap.io/procurement-memo?utm_source=github_action&utm_medium=readme&utm_campaign=marketplace_action&utm_content=procurement_memo)

---

## 1. Quickstart (`5 Seconds` — Zero Secrets via GitHub OIDC)

Add `.github/workflows/ciphermap-pqc.yml` with `id-token: write` to run **keyless Free 12-language PQC Discovery + GitHub Security Code Scanning SARIF 2.1.0** across Linux, macOS, and Windows (`x64` & `arm64`) runners — no API keys or secrets required:

```yaml
name: PQC Security Scan (Keyless OIDC)
on: [push, pull_request]

permissions:
  contents: read
  id-token: write          # Keyless GitHub OIDC runner verification (zero secrets)
  security-events: write   # Uploads SARIF 2.1.0 to GitHub Security -> Code Scanning

jobs:
  pqc-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Ciphermap-io/ciphermap-action@v1
        with:
          fail-on: high
```

### Add `CIPHERMAP_LICENSE` for CNSA 2.0 Gate + CycloneDX 1.7 CBOM (Guided Evaluation or Commercial License)

Store your guided evaluation, Standalone Auditor, or Enterprise Fleet license key as `CIPHERMAP_LICENSE` in **Settings → Secrets and variables → Actions** to unlock `--target cnsa-2.0`, CycloneDX 1.7 CBOMs, HTML CBOM Explorer, and 16-mandate compliance reports:

```yaml
      - uses: Ciphermap-io/ciphermap-action@v1
        with:
          fail-on: high
          target: cnsa-2.0
          cbom-file: ciphermap-cbom-1.7.json
          license-key: ${{ secrets.CIPHERMAP_LICENSE }}
```

---

## 2. Enterprise Supply-Chain Hardened Workflow (`StepSecurity` + 40-Char SHA Pin)

For regulated financial, defense (`CMMC 2.0` / `CNSA 2.0`), and `FedRAMP` environments enforcing immutable commit-SHA pinning and `step-security/harden-runner` egress policies:

```yaml
name: CipherMap PQC, CNSA 2.0 & Signed CBOM Gate
on:
  pull_request:
  push:
    branches: [main]

permissions:
  contents: read
  security-events: write

jobs:
  pqc:
    runs-on: ubuntu-latest
    steps:
      - name: Harden GitHub Runner (StepSecurity)
        uses: step-security/harden-runner@ec9f2d5744a09debf3a187a3f4f675c53b671911 # v2.13.0
        with:
          egress-policy: block
          allowed-endpoints: >
            github.com:443
            api.github.com:443
            www.ciphermap.io:443

      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2

      - name: Run CipherMap Zero-Egress PQC & CBOM Gate
        uses: Ciphermap-io/ciphermap-action@v1 # Or pin to the v1.0.6 40-char commit SHA listed on the Release page
        with:
          fail-on: high
          target: cnsa-2.0
          include-archives: "true"
          cbom-file: ciphermap-cbom-1.7.json
          cbom-spec-version: "1.7"
          compliance-framework: oscal
          license-key: ${{ secrets.CIPHERMAP_LICENSE }}
```

### Air-Gapped Self-Hosted Runners (`0` Outbound Network Calls)
If `/usr/local/bin/ciphermap` (or `~/.local/bin/ciphermap`) matching `cli-version` (`v1.0.6`) is pre-baked onto a self-hosted GitHub Enterprise Server runner image, `ciphermap-action` automatically detects the local binary and makes **zero** outbound `curl` calls — running 100% under `egress-policy: block` without even needing `www.ciphermap.io:443` allowlisted.

On ephemeral `ubuntu-latest`, `macos-latest`, or `windows-latest` (`amd64` / `arm64`) runners without a pre-baked binary, `license-key` (or keyless GitHub Actions OIDC when `id-token: write` is enabled) authenticates against `https://www.ciphermap.io/api/dl/ci-cli`, verifies the `64`-hex `X-CipherMap-SHA256` digest + pinned `CipherMap Version v1.0.6` banner, and runs `scan` / `cbom` with `--offline --no-telemetry`.

---

## 3. Inputs & Outputs

### Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `fail-on` | No | `high` | Severity threshold that fails the workflow (`info`, `low`, `medium`, `high`, `critical`, `none`) |
| `target` | No | `""` | Compliance target profile (`cnsa-2.0`, `fips-140-3`, `nist-sp-800-56b`, `bsi-tr-02102`, `anssi-pqc`) |
| `license-key` | No | `""` | Signed `CIPHERMAP_LICENSE` secret (`CIPHERMAP-TEAM-...`, `CIPHERMAP-ENTERPRISE-...`, or guided evaluation key) |
| `cli-version` | No | `v1.0.6` | Exact CipherMap CLI version verified prior to scan execution |
| `sarif-file` | No | `ciphermap.sarif` | Output path for the SARIF 2.1.0 report |
| `upload-sarif` | No | `true` | Upload SARIF 2.1.0 to GitHub Security → Code Scanning (`if: always()`) |
| `cbom-file` | No | `""` | Emit deterministic CycloneDX `1.6` or `1.7` (ECMA-424) CBOM JSON |
| `cbom-spec-version` | No | `1.7` | CycloneDX CBOM specification version (`1.6` or `1.7`) |
| `cbom-html-file` | No | `""` | Emit standalone zero-backend Interactive HTML CBOM Explorer |
| `attest-slsa` | No | `false` | Emit Hybrid PQC (`Ed25519 + ML-DSA-65`) SLSA v1.1 DSSE provenance (with `pqc-device-key`) |
| `pqc-device-key` | No | `""` | Path to composite `Ed25519 + ML-DSA-65` seed JSON file when `attest-slsa: "true"` |
| `compliance-framework` | No | `""` | Emit regulatory report (`cnsa-2.0`, `pci-dss-4`, `fedramp`, `cmmc`, `eu-dora`, `oscal`, `crosswalk`, `all`, etc.) |
| `compliance-output` | No | `""` | Custom output path for `compliance-framework` report |
| `wrapper-target` | No | `""` | Generate zero-code PQC edge wrapper (`auto`, `envoy`, `nginx`, `haproxy`, `caddy`, `traefik`, `apache`, `stunnel`, `k8s-sidecar`, `ipsec`, `cert-manager`, `cnsa-2.0`) |
| `wrapper-output-dir` | No | `""` | Output directory for `wrapper-target` artifacts |
| `include-archives` | No | `false` | Inspect packaged `.jar`, `.war`, `.ear`, `.whl`, `.apk`, `.aab`, `.nupkg`, `.zip` archives |
| `baseline-file` | No | `""` | Compare against a prior scan JSON baseline to gate PRs on continuous PQC drift |
| `vendor-sla` | No | `false` | Emit Third-Party Vendor PQC SLA procurement scorecard (`ciphermap-vendor-sla.json`) |
| `args` | No | `""` | Extra whitespace-delimited CLI flags passed safely as argv elements |

### Outputs

| Output | Description |
|---|---|
| `sarif-file` | Path of the SARIF 2.1.0 report written by the scan |
| `cbom-file` | Path of the generated CycloneDX 1.6 / 1.7 CBOM JSON (when `cbom-file` is set) |
| `cbom-html-file` | Path of the standalone Interactive HTML CBOM Explorer (when `cbom-html-file` is set) |
| `drift-file` | Path of the continuous PQC drift report (`ciphermap-drift.json` when `baseline-file` is set) |
| `vendor-sla-file` | Path of the Vendor PQC SLA scorecard (`ciphermap-vendor-sla.json` when `vendor-sla` is enabled) |
| `wrapper-output-dir` | Directory containing generated zero-code PQC edge wrappers (when `wrapper-target` is set) |

---

## 4. OWASP Dependency-Track (`v4.12–4.14` & `v5.x Hyades`) CBOM Ingestion

CipherMap supports **both** native OWASP Dependency-Track `v5.x` (`Hyades` `cryptographic-asset` ingestion) **and** Dependency-Track `v4.12.x–4.14.x` (`--dt-compat auto` projects `cryptographic-asset` components into lossless `cdx:cbom:*` file carriers with async token polling and readback verification so `4.x` never silently drops cryptographic assets):

```yaml
      - name: Push Signed CBOM to OWASP Dependency-Track (v4 or v5 Auto-Detect)
        env:
          CIPHERMAP_LICENSE: ${{ secrets.CIPHERMAP_LICENSE }}
          CIPHERMAP_DEPENDENCY_TRACK_API_KEY: ${{ secrets.DTRACK_API_KEY }}
          DTRACK_URL: ${{ secrets.DTRACK_URL }}
          DTRACK_PROJECT_UUID: ${{ secrets.DTRACK_PROJECT_UUID }}
        run: |
          if [ -n "${CIPHERMAP_DEPENDENCY_TRACK_API_KEY:-}" ]; then
            "${CIPHERMAP_EXECUTABLE:-ciphermap}" cbom . \
              --offline \
              --include-archives \
              --spec-version 1.6 \
              --dt-compat auto \
              --push-url "${DTRACK_URL}" \
              --project-id "${DTRACK_PROJECT_UUID}"
          fi
```

---

## 5. Shift-Left Companion: Local `stdio` MCP Referee (`Claude Code`, `Cursor`, `Windsurf`)

Pair this CI gate with [`Ciphermap-io/ciphermap-mcp`](https://github.com/Ciphermap-io/ciphermap-mcp) (`io.github.Ciphermap-io/ciphermap-mcp` on the Official MCP Registry) and `ciphermap immunity emit-agent-rules` so AI coding assistants never introduce Round-3 `kyber768` drafts, `RSA-2048`/`SHA-1` regressions, or Trojan Unicode `.cursorrules` / `CLAUDE.md` configs (`IMM-AICFG-001`) before opening a PR.
