# FiberOS R14.1 — Sales Core: Verification Proof

> **Proof-only repository.**
>
> This repository contains **no source code** and **no delivery archives**.
> The product lives in a **private** repository and cannot be downloaded from here.
>
> Purpose: let a prospective buyer verify that the final delivery tree passed tests and verification gates, then return to the sales site to purchase and receive the package.

---

## 1) Verified tree

| Item | Value |
|---|---|
| Package | FiberOS R14.1 — Sales Core |
| Version | `1.0.0-rc.1+commercial.2026-09-25` |
| Reference commit | `dac01a2ef20b37f5262af151bb200479e2e32e75` |
| Commit date | 2026-09-30 |
| Production migration chain | `001–116` (contiguous, SHA-256 ledger backed) |
| Status | Commercial Release Candidate (RC) |

---

## 2) Live verification results (session 2026-09-30)

Session environment: **Node.js `v24.21.0`** (as declared in `package.json` / `.nvmrc`), **Redis `7.0.15`**, install via `npm ci` from the lockfile.

Literal `npm test` summary:

```
ℹ tests 316
ℹ suites 1
ℹ pass 316
ℹ fail 0
ℹ cancelled 0
ℹ skipped 0
ℹ todo 0
ℹ duration_ms 5690.246808
```

**316/316 tests passed — zero failures, zero skips.**

Full `npm run verify` gate (exit code = 0):

```
Release integrity:                PASS
Migration sequence + SHA ledger:  PASS — 116/116 contiguous
Production-console boundary:      PASS
Architecture boundary/map:        PASS — 41 packages
JavaScript syntax:                PASS
Root-cause contracts:             PASS
Formal verification:              PASS_CORE — 259 checks
Capability parity:                PASS — 101 capabilities
Domain neutrality:                PASS — multi-buyer
i18n quality gate:                PASS — 114 locales, 101 keys, 2 verified
Manifest parity gate:             PASS — 1155 files
Commercial release gate:          ok: true
```

Private-repo CI run IDs (documentation only; private workflows are not publicly browsable):

| Run ID | Workflow | Commit | Result |
|---|---|---|---|
| `36676646280` | verify-gate | `dac01a2` | **success** |
| `36676646302` | sales-security | `dac01a2` | **success** |

---

## 3) Reproducible build fingerprint

The package is built with a deterministic builder (`scripts/build-delivery-archive.py` inside the product tree): stable entry order, timestamps fixed from `SOURCE_DATE_EPOCH`, fixed permissions/`uid`/`gid`, fixed compression level, no extra fields.

Two consecutive builds from the same tree:

```
$ python3 scripts/build-delivery-archive.py --output final_A.zip \
    --commit dac01a2ef20b37f5262af151bb200479e2e32e75
sha256: 6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff

$ python3 scripts/build-delivery-archive.py --output final_B.zip \
    --commit dac01a2ef20b37f5262af151bb200479e2e32e75
sha256: 6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff
```

**Hashes match exactly:**

```
6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff  final_A.zip
6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff  final_B.zip
```

A buyer who rebuilds the delivered package with the same tool and commit should obtain **the same byte fingerprint**. The archive includes `MANIFEST_BUILD_STAMP.txt`:

```
package=FiberOS R14.1 Sales Core
build.epoch=1790746233
build.stamp=2026-09-30T05:30:33Z
tree.commit=dac01a2ef20b37f5262af151bb200479e2e32e75
```

---

## 4) Honesty boundaries

- “Zero failures” means: **all in-session tests (316/316) and static verification gates passed** in the reference verification environment — not a warranty that every possible customer environment is defect-free.
- Static verification does **not** replace buyer production acceptance: PostgreSQL/PostGIS, OIDC/JWKS, TLS/ingress, Redis/HA, storage, and backups remain the buyer’s responsibility before production use (see `FINAL_VERIFICATION_STATUS.md` inside the paid package).
- The built-in rate limiter is `PROCESS_LOCAL`, not a shared multi-instance limiter.
- Tenant isolation is enforced at the database layer (`tenant_id` + RLS); organizational JWT claims are advisory only where documented.
- Marketing visuals and any interactive demo are **illustrative / simulated** unless stated otherwise in the paid package docs.
- This is a **source package / RC**, not a hosted SaaS subscription.

---

## 5) What is not in this repository

- No source code
- No delivery ZIP
- No GitHub Releases and no downloadable product artifacts
- Proof record only: this README, `verification.json`, and `SHA256SUMS.txt`

The full source is private. After purchase from the authorized sales channel, you receive the locked Sales Core package bound to the published fingerprint and can rebuild and verify the hash yourself.

---

## Files in this proof repository

| File | Role |
|---|---|
| `README.md` | Verification narrative and scope limits (English) |
| `verification.json` | Machine-readable evidence from the 2026-09-30 re-verification session |
| `SHA256SUMS.txt` | Fingerprints for offline comparison; verify with `sha256sum -c SHA256SUMS.txt` where applicable |

**Reference commit:** `dac01a2ef20b37f5262af151bb200479e2e32e75`

**Commercial terms (sales channel):** Release Candidate · non-exclusive commercial source license · campaign pricing and support window are defined on the seller’s checkout page, not in this proof repo.
