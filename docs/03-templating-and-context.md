# Templating & Context Management

## Templating — KrakenD's Flexible Configuration

Instead of maintaining one giant `krakend.json` for every environment (dev, staging,
prod), KrakenD lets you break config into reusable pieces and use a **template system**
(Go templates) to assemble the final file at startup. Like a mail-merge letter — write
the template once with placeholders, feed in different "settings" files per environment,
and KrakenD stitches together the right final config automatically. This avoids
copy-pasting near-identical config files across environments.

## Context — The Data Available as a Request Travels Through

"Context" is a bundle of information attached to a request as it moves through the
gateway — the URL, headers, which backend got picked, what settings loaded. Templates,
plugins, and scripts can reach into this context to read or modify that data mid-flight.

In KrakenD, this is straightforward: a data object accessed with dot-notation inside
templates, plugins, or Lua scripts.

## WSO2's Version — Synapse Message Context

WSO2 has a direct equivalent, just more formalized. It's called the **Message Context**,
and instead of one flat bag of data, it defines several **scopes** that control how long a
piece of data stays visible:

- **Synapse scope** — default, message-wide info (timestamp, format, etc.)
- **Axis2 scope** — visible only for the current sequence
- **Transport scope** — the actual transport-layer headers
- **Operation scope** — visible for a single request/resource only

This gives finer-grained control over data visibility, at the cost of needing to learn
the scoping model.

## Practical Difference

KrakenD: simple, implicit context object, and a templating system purpose-built for
assembling config across environments. WSO2: a more formal, scoped context model with
finer-grained control, but no direct equivalent found to KrakenD's environment-templating
approach — worth a genuine point in KrakenD's favor for teams managing multiple
environments.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Config templating across environments | Built-in (Flexible Configuration) | No direct equivalent found |
| Request context model | Single data object, dot-notation access | Message Context with defined scopes |
| Data visibility control | Implicit (wherever you access it) | Explicit scopes (Synapse, Axis2, Transport, Operation) |
| Learning curve | Low | Higher — requires understanding scopes |
