# Contributing to WellSourced Brand Data

## Adding a New Brand

1. Copy `templates/brand-template.json`
2. Name the file using the brand's slug (lowercase, hyphens): `my-brand.json`
3. Fill in all required fields (name, slug, url, shopify_domain, description, country_hq)
4. Add sources for any claims you make
5. Set trust_tiers to 1 (self-reported) unless you have citations for higher tiers
6. Submit a pull request

## Editing an Existing Brand

1. Edit the relevant JSON file
2. Add/update sources for any changed data
3. Update `last_verified` date
4. Add your GitHub username to `verified_by`
5. Submit a pull request

## Citation Standards

- Tier 1 (Self-Reported): No citation needed, but label clearly
- Tier 2 (Community-Verified): Must include a source URL
- Tier 3 (Independently Audited): Must link to third-party audit, certification directory, or regulatory filing

## Conflict of Interest

If you are affiliated with a brand you are editing, you must disclose this in your PR description. Self-reported data stays at Tier 1.