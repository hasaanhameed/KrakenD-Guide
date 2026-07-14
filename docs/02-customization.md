# Customization: How You Configure and Extend Each One

## KrakenD Enterprise — Plain Config File, Code Only When Needed

KrakenD's whole setup lives in one JSON file (`krakend.json`). You describe *what* you
want ("this endpoint should call these backends, apply this rate limit, require this
auth") rather than writing logic. Turning on a feature is just adding a named block under
`extra_config` — no coding required for common things like auth, caching, or rate limiting.

If you need something custom that config alone can't do, KrakenD lets you write
**plugins** — small pieces of code in Go, or simpler scripts in Lua, that plug into
specific points in the request flow.

So the pattern is: config first, code only as an escape hatch.

## WSO2 — A More Powerful but Code-Like Config Language

WSO2 doesn't use simple JSON — it uses its own configuration language (XML-based) where
you build a "sequence" out of building blocks called **mediators** (prebuilt steps like
"transform this," "call this backend," "check this condition"). You chain mediators
together to describe request handling — more like writing a small program using building
blocks, even before touching real code.

If the built-in mediators aren't enough, you can write your own in Java — but that's a
heavier lift (compiling, packaging, deploying) than KrakenD's plugin model.

## The Practical Difference

KrakenD: simple text config covers most cases, with small optional code for the rest.
WSO2: a more powerful but more verbose config language that can express more complex logic
out of the box, but with a heavier path when you need true custom code.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Config format | Plain JSON | XML-based mediation sequences |
| Turning on common features | Add a block under `extra_config` | Add/chain a built-in mediator |
| Custom logic | Go or Lua plugins | Custom Java mediators |
| Effort for custom code | Lightweight (write plugin, drop in) | Heavier (compile, package, deploy) |
| Learning curve | Low — read config, understand it | Higher — mediation language + building blocks |
