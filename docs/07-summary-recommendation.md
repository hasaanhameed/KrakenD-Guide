# Summary & Recommendation

A rollup of every chapter, for anyone who wants the short version before diving into the
details.

## Chapter Summaries

**[00 — Framing](00-framing.md)**
KrakenD and WSO2 aren't the same category of product. KrakenD's core is just the gateway.
WSO2 API Manager bundles the gateway with a Publisher, Developer Portal, Key Manager, and
Analytics — a full API lifecycle platform.

**[01 — Architecture & Deployment](01-architecture-deployment.md)**
KrakenD is one stateless binary — scale by running more copies, no database needed. WSO2
is several connected components backed by a database, deployable all-in-one or spread
across servers.

**[02 — Customization](02-customization.md)**
KrakenD uses plain JSON config for most needs, with Go/Lua plugins as an escape hatch for
custom logic. WSO2 uses a more powerful but more verbose XML-based mediation language,
with Java for anything beyond built-in mediators.

**[03 — Templating & Context](03-templating-and-context.md)**
KrakenD's Flexible Configuration templates config across environments from one source,
with no direct WSO2 equivalent found. Both have a "context" concept for request data
in-flight — KrakenD's is a simple object, WSO2's Message Context uses formal scopes.

**[04 — AI Gateway](04-ai-gateway.md)**
The closest thing to feature parity in this comparison — both products have a real AI
Gateway covering provider routing, cost/token control, and prompt-level guardrails.

**[05 — Roles & Permissions](05-roles-permissions.md)**
The one that started this whole investigation, confirmed: WSO2 has built-in roles for
governing your own team's access to APIs (who can create/publish/edit/view). KrakenD's
roles are genuinely capable (JWT-role-based conditional routing — e.g. moderators get a
premium model, others get a cheaper one) but scoped to the caller, not to internal team
governance — no equivalent for who on our own team can create/edit/publish an API.

**[06 — Licensing & Cost](06-licensing-cost.md)**
KrakenD gates several features (SSO/SAML, audit logging, custom Go plugins, FIPS) behind
a paid Enterprise tier. WSO2's entire product is open source — its subscription buys
support and faster patches, not features.

*(A dedicated Feature Set chapter and a Performance & Scalability chapter were not
completed due to time constraints.)*

## Shortcomings: What KrakenD Doesn't Do As Well

**1. No Built-In Identity/Key Manager**
WSO2 has its own Key Manager component for handling logins, tokens, and OAuth flows as
part of the platform. KrakenD expects you to bring your own auth — validating JWTs,
API keys, or integrating an external identity provider through config. That's not
necessarily bad (it avoids being locked into one auth system), but it means auth
infrastructure isn't included — the company would need an existing identity solution
already in place.

**2. No Built-In Internal Team Governance**
As covered in the roles/permissions chapter: KrakenD's roles govern external API
consumers, not who on our own team can create, edit, or view APIs. WSO2 has that
governance layer built in (Admin/Creator/Publisher/Subscriber roles, Publisher access
control, API visibility rules). Without it, KrakenD relies on us building our own process
for who's allowed to touch what.

**3. Several Meaningful Features Are Enterprise-Only**
Audit logging, SSO/SAML, custom Go plugins, and FIPS compliance are all gated behind
KrakenD's paid Enterprise tier. WSO2's entire product, including equivalents of most of
these, is available in the fully open-source edition — payment there buys support, not
features. If cost is a concern, KrakenD's free tier is more limited than WSO2's free tier
in this specific way.

## The Actual Trade-Off

Every chapter points to the same underlying pattern:

- **KrakenD** is a fast, simple, lightweight gateway. Easy to run, easy to configure,
  cheap to operate for its Community tier — but it expects you to bring or build your own
  developer portal, identity system, and internal governance process, and gates several
  production-grade features behind Enterprise pricing.
- **WSO2** is a full API management platform. Heavier to run and operate, but it comes
  with a developer portal, identity management, and internal team governance built in,
  and its entire feature set is available for free — you pay for support, not features.

## Recommendation

There isn't a universally "better" answer here — it depends on what the company actually
needs:

- If the goal is **a fast gateway in front of existing microservices**, with the company
  already having (or not needing) a developer portal, identity provider, and internal API
  governance process, **KrakenD is the better fit** — lighter, cheaper to run, and
  simpler to operate.
- If the goal is **a full self-service API platform** — multiple teams publishing and
  discovering APIs, formal internal governance over who can touch what, built-in identity
  and analytics — **WSO2 covers more out of the box**, at the cost of being heavier to
  deploy and operate.

Before finalizing a recommendation to the team, it's worth confirming with the
supervisor/tech lead which of these two needs is actually driving this decision — that
answer determines which product's trade-offs matter more.
