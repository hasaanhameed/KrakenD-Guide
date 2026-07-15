# AI Gateway

## KrakenD Enterprise — AI Gateway Module

KrakenD's AI Gateway is a dedicated Enterprise-only module for handling traffic to LLMs
(OpenAI, Anthropic, etc.) safely and predictably. It's built around four areas:

- **Unified LLM Interface** — one standard API shape regardless of which LLM provider is
  behind it, so switching or mixing providers doesn't mean rewriting client code
- **AI Security** — zero-trust handling of AI traffic, data masking, and preventing
  sensitive data from leaking out through prompts/responses
- **Cost Control** — tracking token usage per request/consumer and enforcing budgets, so
  LLM spend doesn't spiral unpredictably
- **Governance** — routing across multiple LLMs, validating prompts/responses, rate
  limiting, and enforcing compliance rules

Confirmed Enterprise-only: this module requires a license that specifically includes AI
Gateway capabilities.

Worth being precise about what's genuinely a dedicated feature versus a pattern: the
Unified LLM Interface (`ai/llm`) and token-based Cost Control (`governance/quota`) are
real, purpose-built config keys. "Governance"/guardrails is not a separate mechanism —
KrakenD's own documentation confirms it's built by combining features that already existed
anyway (JSON Schema validation, rate limiting, conditional/sequential backend chaining).
Nothing is guardrailed by default; it has to be assembled per endpoint. A rule-based
guardrail (blocking known bad patterns) is fully config-only. A smarter, AI-judged
guardrail (e.g. catching prompt injection) additionally needs an external classifier model
that isn't part of KrakenD at all — you bring and host that yourself, and KrakenD just
calls it like any other backend.

## WSO2 — AI Gateway

WSO2 also has its own dedicated AI Gateway, covering very similar ground:

- **Provider routing** — an "LLM Proxy" that routes to providers like OpenAI, Anthropic,
  Azure OpenAI, Mistral, and AWS Bedrock, with multi-provider load balancing and failover
- **Guardrails** — PII masking, prompt injection protection, content safety checks,
  semantic prompt/response validation
- **Cost control** — limits on request count, token count, and cost, both per-backend and
  per-consumer
- **Analytics** — a dashboard showing token usage, estimated costs, traffic by provider/
  model, guardrail triggers, and errors
- **Performance features** — semantic caching and adaptive/model routing

## Practical Difference

Both products now ship a real AI Gateway covering the same core ground: provider routing,
cost/token control, prompt-level guardrails, and governance. This is closer to feature
parity than most other areas we've compared so far — not an area where one side has an
obvious gap.

One difference worth noting: WSO2's public materials name specific supported providers
(OpenAI, Anthropic, Azure OpenAI, Mistral, AWS Bedrock) and specific techniques (semantic
caching, adaptive routing), while KrakenD's public description stays at a more general
capability level. That may just reflect a documentation style difference rather than an
actual capability gap.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Routing across multiple LLM providers | Yes — Unified LLM Interface | Yes — LLM Proxy, named providers (OpenAI, Anthropic, Azure OpenAI, Mistral, Bedrock) |
| Blocking sensitive data in prompts/responses | Yes — data masking under AI Security | Yes — PII masking, prompt injection protection |
| Limiting spend | Token usage tracking + budget enforcement | Limits on request count, token count, and cost, per-consumer |
| Seeing usage across providers | Not detailed publicly | Dashboard: token usage, cost, traffic by provider/model |
| Requires which edition | Enterprise only, with AI Gateway module | Part of WSO2's AI Gateway/API Platform offering |
