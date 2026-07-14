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
  a gateway. It bundles several parts together:
  - **Publisher** — where teams design and publish APIs
  - **Developer Portal** — where developers discover and subscribe to APIs
  - **Gateway** — the traffic-enforcement layer (the part most comparable to KrakenD)
  - **Key Manager** — handles authentication/tokens
  - **Analytics** — usage dashboards and reporting
- Needs a database to run — not a standalone binary.

## What KrakenD Does NOT Do

KrakenD is not a tool for building APIs — it has no business logic and no database. It
sits in front of APIs you already have. What it actually does:

- **Routes** requests to existing backend services
- **Aggregates** multiple backend responses into one response (its signature feature)
- **Transforms** data — renames/filters fields, reshapes JSON
- **Enforces** auth (JWT/OAuth), rate limiting, caching, CORS
- **Protects** real backends — clients only ever talk to KrakenD, never directly to backends

In short: a traffic cop + translator in front of your APIs, not a tool for creating them.

## Where KrakenD Actually Differs From WSO2's Gateway

Most of the list above — routing, auth enforcement, rate limiting, caching, CORS, hiding
real backends — is standard gateway duty. WSO2's Gateway component does these too.

The real difference is **aggregation**. In KrakenD, combining multiple backend calls into
one response is a config block — list the backends under one endpoint, done. In WSO2,
this isn't a built-in declarative feature; it takes custom mediation logic (a mediator/
sequence you write) to achieve the same thing.

So it's less "WSO2 can't do this" and more: KrakenD makes a few things (especially
aggregation) much faster to set up, while WSO2 gives more built-in surrounding
infrastructure (portal, identity, analytics) at the cost of being heavier to run.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Product scope | API Gateway (+ Enterprise add-ons) | Full API lifecycle platform |
| Core components | Single gateway binary + dashboard | Publisher, Developer Portal, Gateway, Key Manager, Analytics |
| State | Stateless | Requires a database |
| Developer portal | Not core; limited via Enterprise | Built-in |
| Identity/OAuth | Via config/plugins | Built-in Key Manager |
| Analytics | Enterprise dashboard | Built-in |
