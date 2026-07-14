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

The only new piece Enterprise adds is a **dashboard** — a separate small tool for viewing
configs/analytics. The actual gateway doing the traffic work is still that one simple
program.

## WSO2 API Manager — Several Pieces Working Together

WSO2 isn't one program — it's a team of programs, each with its own job:

- **Publisher** — lets your team publish APIs
- **Developer Portal** — lets other developers browse/subscribe to APIs
- **Gateway** — actually enforces traffic rules (the piece closest to what KrakenD does)
- **Key Manager** — handles logins/tokens
- **Analytics** — handles usage stats

Unlike KrakenD, these pieces need to remember things — who's registered, who's subscribed
to what, usage history — so WSO2 needs an actual **database** behind it to store that
information.

You can run all five pieces on one server (simpler, but that server carries the whole
load), or spread them across multiple servers (more resilient, but more to set up and
maintain).

## The Practical Difference

Setting up KrakenD is like setting up one app and pointing it at your services. Setting up
WSO2 is like setting up a small team of connected apps plus a database for them to share —
more initial work, but you get portal/login/analytics pieces built-in rather than needing
to add them yourself.

## Comparison Table

| Aspect | KrakenD Enterprise | WSO2 API Manager |
|---|---|---|
| Number of moving parts | One (gateway) + dashboard | Five (Publisher, Developer Portal, Gateway, Key Manager, Analytics) |
| Memory between requests | None — stateless | Yes — needs a database |
| Scaling approach | Run more copies of the same program | Scale each piece independently |
| Setup effort | Low — one binary + config file | Higher — multiple components + database |
| Deployment layout options | Single binary/container everywhere | All-in-one (one server) or distributed (spread across servers) |
