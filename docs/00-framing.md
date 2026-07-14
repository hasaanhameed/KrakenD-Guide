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

*(Flag: the above is my current understanding of WSO2 API Manager's architecture — needs
verification against WSO2's own docs, since we don't have a hands-on WSO2 instance the way
we do with the KrakenD playground.)*

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Product scope | API Gateway (+ Enterprise add-ons) | Full API lifecycle platform |
| Core components | Single gateway binary + dashboard | Publisher, Store, Gateway, Key Manager, Analytics |
| State | Stateless | Requires a database |
| Developer portal | Not core; limited via Enterprise | Built-in (Store) |
| Identity/OAuth | Via config/plugins | Built-in (Key Manager) |
| Analytics | Enterprise dashboard | Built-in |
