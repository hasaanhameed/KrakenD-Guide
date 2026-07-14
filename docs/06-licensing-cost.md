# Licensing & Cost Model

## KrakenD

- **Community Edition** — free, Apache 2.0 licensed. Covers the full gateway core:
  routing, rate limiting, transformation, aggregation. No cost to download, run, or
  modify.
- **Enterprise Edition** — adds things production teams typically need at scale: audit
  logging, SSO/SAML, immediate security patches, custom Go plugins, FIPS compliance, and
  direct support with an SLA. Requires a paid license; pricing isn't published, requires
  contacting sales.
- Support is included with Enterprise; optional as a paid add-on for Community users.

## WSO2

- **Fully Apache 2.0 licensed**, including the full API Manager product — no separate
  paid-only edition with withheld features. You can self-host the complete product for
  free.
- The **subscription** you pay for isn't unlocking features — it's for support and
  services: early security patches, upgrades, and SLA-backed support.
- Subscription pricing is based on a "per core" model, tied to how many cores your
  deployment uses and what level of support you want. Pricing isn't public — requires a
  quote from WSO2.

## Practical Difference

The two companies structure cost differently:

- **KrakenD** draws a hard line — Community is genuinely free, but certain features
  (custom Go plugins, SSO/SAML, audit logging, FIPS compliance) simply don't exist unless
  you pay for Enterprise.
- **WSO2** doesn't gate features behind payment — the whole product is open source and
  usable for free. What you pay for is support, faster security patches, and coverage
  scaled to your infrastructure (cores).

So the real question for the company isn't just "which is cheaper" — it's "do we need
KrakenD's Enterprise-only features, or would WSO2's free-but-self-supported model cover
what we need without a subscription?" Neither publishes transparent pricing, so an actual
quote from each would be needed for a real cost comparison.

## Comparison Table

| Aspect | KrakenD | WSO2 API Manager |
|---|---|---|
| Core license | Apache 2.0 (Community Edition) | Apache 2.0 (full product) |
| Do paid tiers unlock features, or just support? | Yes — some features (custom Go plugins, SSO/SAML, audit logging, FIPS) require Enterprise | No — full product is free; payment is for support/patches/SLA |
| What triggers a cost | Needing an Enterprise-only feature | Wanting official support/faster patches, or scaling to more cores |
| Pricing transparency | Not published — contact sales | Not published — contact sales, priced per core |
| Free self-support option | Community Edition, community support only | Full product free, community support only |
