# Templating & Context Management

## Templating — KrakenD's Flexible Configuration

Instead of maintaining one giant `krakend.json` for every environment (dev, staging,
prod), KrakenD lets you break config into reusable pieces and use a **template system**
(Go templates) to assemble the final file at startup. Like a mail-merge letter — write
the template once with placeholders, feed in different "settings" files per environment,
and KrakenD stitches together the right final config automatically. This avoids
copy-pasting near-identical config files across environments.

**Some example templates KrakenD supports:**

- **Pulling a value from a settings file** — given `settings/urls.json` has
  `"users_api": "https://users-api.mycompany.com"`, a config template can insert it with
  `{{ .urls.users_api }}`
- **Including a reusable snippet (partial)** — `{{ include "dir1/dir2/partial_file_name.txt" }}`
  drops in the contents of another file, so common blocks aren't repeated everywhere
- **Looping over a list** — e.g. generating one JSON entry per endpoint automatically:
  `{{ range $index, $endpoint := .endpoints_list }} ... {{ end }}`
- **Reading an environment variable** — `{{ env "SECRET_VARIABLE_NAME" }}` pulls a value
  straight from the environment instead of hardcoding it
- **Conditionals** — `{{ if CONDITION }}yes{{else}}no{{end}}`, e.g. only enabling a feature
  block when a setting is turned on

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
| Managing config for dev/staging/prod | Write one templated config, split settings per environment into separate files, KrakenD builds the final file per environment automatically | No built-in feature for this — you'd maintain/edit separate config sets manually per environment |
| How you read request data mid-flight | Write `.Request.Headers` (or similar dot-notation) directly in a template, plugin, or Lua script | Call a function like `get-property("Transport", "...")`, specifying which scope the data lives in |
| How long a piece of data stays visible | Not restricted — same object visible everywhere you have access to context | Depends on which scope you stored it in: whole message, current sequence only, transport headers only, or just the current request |
| What you need to learn to use it | Basic dot-notation, same everywhere | The 4 scopes (Synapse, Axis2, Transport, Operation) and which one to use for a given case |
