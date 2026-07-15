# Architecture & Deployment: How Each One Is Actually Put Together

The best way to think about this: how many "pieces" do you have to set up, and how many
do you have to keep running?

## KrakenD Enterprise — One Piece

KrakenD is a single program you start up — like running one app. It doesn't remember
anything between requests (that's what "stateless" means — no memory of past requests, no
database backing it). Every request comes in, KrakenD reads its config file to decide what
to do, forwards it to the right backend, and forgets about it.

Why this matters: if you need more capacity, you just run more copies of that same program
side by side, with a load balancer splitting traffic between them. No copies need to talk
to each other or share a database — they're all independent. That's what makes it
lightweight and easy to scale.

Self-hosted, that load balancer is something we'd run and maintain ourselves (e.g. an
NGINX container in front of several KrakenD containers) — on a cloud platform, a managed
load balancer service does this for you automatically. Worth noting: KrakenD's own plain
backend-host list has no built-in health monitoring of its own (it won't stop routing to a
dead target on its own — that needs an explicitly configured Circuit Breaker), and KrakenD's
own documentation recommends placing an external balancer in front of a multi-instance
KrakenD deployment rather than chaining KrakenD instances together for that job.

Enterprise doesn't add a running admin console. Its "UI" is **KrakenD Designer**, a
browser-based editor you use to *write* the config file — not a live dashboard you log
into to manage traffic. The actual gateway doing the traffic work is still that one
simple program.

## WSO2 API Manager — Several Pieces Working Together

WSO2 isn't one program — it's a team of programs, each with its own job:

- **Publisher** — lets your team publish APIs
- **Developer Portal** — lets other developers browse/subscribe to APIs
- **Gateway** — actually enforces traffic rules (the piece closest to what KrakenD does)
- **Key Manager** — handles logins/tokens
- **Traffic Manager** — enforces rate-limiting/throttling policies, a separate piece from the Gateway
- **Analytics** — handles usage stats

Unlike KrakenD, these pieces need to remember things — who's registered, who's subscribed
to what, usage history — so WSO2 needs an actual **database** behind it to store that
information.

You can run all six pieces on one server (simpler, but that server carries the whole
load), or spread them across multiple servers (more resilient, but more to set up and
maintain) — in practice, the Gateway is usually the piece scaled out the most, since it
sees far more traffic than the portals.

## The Practical Difference

Setting up KrakenD is like setting up one app and pointing it at your services. Setting up
WSO2 is like setting up a small team of connected apps plus a database for them to share —
more initial work, but you get portal/login/analytics pieces built-in rather than needing
to add them yourself.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Number of moving parts | One (the gateway) | Six (Publisher, Developer Portal, Gateway, Key Manager, Traffic Manager, Analytics) |
| Memory between requests | None — stateless | Yes — needs a database |
| Scaling approach | Run more copies of the same program | Scale each piece independently |
| Setup effort | Low — one binary + config file | Higher — multiple components + database |
| Deployment layout options | Single binary/container everywhere | All-in-one (one server) or distributed (spread across servers) |
