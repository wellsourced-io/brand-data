# WellSourced — Brand Data Repository

> **Repo:** `wellsourced-io/brand-data`
> **Role:** The community wiki layer. One JSON file per brand. Version-controlled, auditable, open.
> **This is the source of truth for all brand metadata in WellSourced.**

## Critical Rules

- **Never store secrets here.** No Shopify tokens, no API keys, no credentials. This is a fully public repo. Storefront Access Tokens are stored in env vars in the `wellsourced` repo.
- **Every brand file must validate** against `schema/brand.schema.json`. CI blocks PRs with invalid JSON.
- **One file per brand.** File name = brand slug: `brands/{slug}.json` (lowercase, hyphens only).
- **Self-reported data stays Tier 1.** If a brand submits their own data, trust tiers remain at 1 until independently verified by a contributor.
- **Citations required for Tier 2+.** Every field claimed as Tier 2 or 3 must have a corresponding entry in the `sources` array with a working URL.
- **Conflict of interest disclosure required.** PR authors affiliated with the brand must disclose this in the PR description.

## Repository Structure

```
brand-data/
├── brands/                     # One JSON file per brand
│   ├── patagonia.json
│   ├── allbirds.json
│   └── ...
├── schema/
│   └── brand.schema.json       # JSON Schema (draft-07) for validation
├── templates/
│   └── brand-template.json     # Blank template for new brands
├── scripts/
│   └── setup-hooks.sh          # Install gitleaks pre-commit hook
├── .github/workflows/
│   ├── validate.yml            # JSON Schema validation on PRs
│   └── secret-scan.yml         # Gitleaks scanning
├── CONTRIBUTING.md             # How to add/edit brands
├── COI_POLICY.md               # Conflict of interest policy
└── .gitleaks.toml              # Secret scanning config
```

## Brand JSON Schema (Required Fields)

```json
{
  "name": "string (required)",
  "slug": "string (required, lowercase, hyphens, must match filename)",
  "url": "string (required, brand website URL)",
  "shopify_domain": "string (required, e.g. 'brand-name.myshopify.com')",
  "description": "string (required)",
  "country_hq": "string (required)"
}
```

Optional fields: `categories`, `ownership_type`, `certifications`, `country_manufactured`, `worker_conditions`, `ceo_worker_ratio`, `price_range`, `sources`, `verified_by`, `last_verified`, `trust_tiers`.

See `schema/brand.schema.json` for the full schema with enums and validation rules.

## Trust Tier System

Trust tiers are **per-field**, not per-brand:

| Tier | Label | Meaning | Requirement | Badge |
|---|---|---|---|---|
| 1 | Self-Reported | Brand or submitter provided this | No citation needed | Gray |
| 2 | Community-Verified | A contributor researched and cited sources | Source URL in `sources` array | Blue |
| 3 | Independently Audited | Confirmed via third-party data | Link to audit, certification directory, or regulatory filing | Green |

## How This Repo Connects to the App

```
brand-data/brands/*.json
    │
    ├── (Docker volume mount) → wellsourced app reads at /app/brand-data/
    ├── (Docker volume mount) → sync-worker reads at /app/brand-data/
    │
    └── (CI trigger) → merge to main triggers search reindex in wellsourced repo
```

The `wellsourced` app and sync-worker never write to this repo. Data flows one way: contributors edit JSON here → app reads it.

## Adding a New Brand

1. Copy `templates/brand-template.json` to `brands/{slug}.json`
2. Fill in required fields
3. Add sources for any claims at Tier 2+
4. Submit a pull request
5. CI validates JSON against schema
6. A moderator reviews and merges

## Ownership Types (Enum)

`indie` | `worker-owned` | `co-op` | `employee-owned` | `b-corp` | `public`

## Price Ranges (Enum)

`$` | `$$` | `$$$` | `$$$$`
