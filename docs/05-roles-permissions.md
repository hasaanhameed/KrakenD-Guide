# Roles & Permissions

This is the question that kicked off this whole comparison: does the gateway let
management control which roles/teams can interact with which APIs? Turns out KrakenD and
WSO2 answer very different questions when they talk about "roles."

## KrakenD Enterprise — Roles Control API Consumers, Not Your Team

KrakenD's role-based access control is about **who is allowed to call which endpoint**,
not about managing your internal team's permissions on the gateway itself.

How it works: roles come from JWT claims or declared API keys, checked per request. This
is more capable than a simple allow/deny gate — KrakenD can route a request differently
based on the caller's role. Our own playground demo does this for the AI Gateway: a
`moderator` JWT role gets routed to a premium model (OpenAI), while other roles get routed
to a cheaper one (Gemini) — same endpoint, different backend, picked by role.

So KrakenD's roles answer: *"What is this caller (app, user, or key) allowed to do or
access?"* — not *"Which of my teammates can create, edit, or publish this API in the
first place?"* It's a rich system, just scoped to the caller side, not to governing your
own team's access to the gateway/platform itself.

## WSO2 — Built-In Organizational Roles for Managing the Platform

WSO2 ships with a real set of default roles for **people on your team**, controlling what
they can do inside the platform itself:

- **Admin** — creates users, assigns roles, manages the whole platform
- **Creator** — technical role, builds/provisions APIs
- **Publisher** — managerial role, controls the API's lifecycle, subscriptions, and who
  can see/use it
- **Subscriber** — developers who browse the Developer Portal, discover APIs, and consume
  them

On top of that, WSO2 lets you restrict **who can even view or edit a given API** inside
the Publisher portal, based on role — so one team's APIs can be invisible to another
team's Creators, for example. It also has scope-based rules controlling finer actions,
like who can manage shared scopes.

## Practical Difference

This confirms the original framing from the workshop: WSO2 gives management real,
built-in control over which internal roles/teams can create, publish, or modify which
APIs. KrakenD doesn't have an equivalent — its role system is aimed at the outside world
(API consumers), not at governing your own team's access to the gateway.

If the company's concern is "which of our own teams/developers can touch which APIs,"
that's a built-in WSO2 strength with no direct KrakenD equivalent out of the box.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| What "roles" control | Which external API callers can hit which endpoint | Which internal team members can create/publish/edit/view which APIs |
| Where roles are defined | In the config file, tied to API keys/JWT claims | Built-in role system (Admin, Creator, Publisher, Subscriber) |
| Restricting who can edit an API | No built-in equivalent | Yes — Publisher Access Control, restrict by role |
| Restricting who can even see an API exists | No built-in equivalent | Yes — API visibility settings in Developer Portal |
| Managing external consumer access | Yes — API key + role based, with rate-limit quotas | Yes — via subscriptions and OAuth scopes |
