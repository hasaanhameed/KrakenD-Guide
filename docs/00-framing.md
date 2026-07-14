# Framing: Are KrakenD and WSO2 the Same Kind of Product?

Before comparing features line-by-line, it's worth checking whether we're comparing two
things in the same category. If they're not, that mismatch is itself a useful finding.

## KrakenD Enterprise

- Core product: an **API Gateway** — a single stateless binary, configured declaratively
  (`krakend.json` + `extra_config` blocks for features like auth, rate limiting, caching).
- Enterprise edition builds on top of the open-source gateway core with: a dashboard/UI,
  an AI Gateway (LLM traffic control), plugin marketplace, and additional governance
  features.
- No built-in database — stays stateless even in Enterprise.

## WSO2

- Core product (WSO2 API Manager): a **full API lifecycle management platform**, not just
  a gateway. It typically bundles several components:
  - **Publisher** — where teams design/document/publish APIs
  - **Developer Portal / Store** — where consumers discover and subscribe to APIs
  - **Gateway** — the actual traffic-enforcement layer (this is the part that's most
    comparable to KrakenD)
  - **Key Manager / Identity** — OAuth2/OIDC, app registration
  - **Analytics** — usage dashboards, reporting
- Typically needs a database and more moving parts to run the full platform.

## What KrakenD Does NOT Do

KrakenD is not a tool for building APIs — it has no business logic and no database. It
sits in front of APIs you already have. What it actually does:

- **Routes** requests to existing backend services
- **Aggregates** multiple backend responses into one response (its signature feature)
- **Transforms** data — renames/filters fields, reshapes JSON
- **Enforces** auth (JWT/OAuth), rate limiting, caching, CORS
- **Protects** real backends — clients only ever talk to KrakenD, never directly to backends

In short: a traffic cop + translator in front of your APIs, not a tool for creating them.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Product scope | API Gateway (+ Enterprise add-ons) | Full API lifecycle platform |
| Core components | Single gateway binary + dashboard | Publisher, Store, Gateway, Key Manager, Analytics |
| State | Stateless | Requires a database |
| Developer portal | Not core; limited via Enterprise | Built-in (Store) |
| Identity/OAuth | Via config/plugins | Built-in (Key Manager) |
| Analytics | Enterprise dashboard | Built-in |
