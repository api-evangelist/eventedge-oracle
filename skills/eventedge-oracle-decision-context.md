---
name: eventedge-decision-context
description: >-
  Pull richer read-only decision-support context for a topic across Polymarket and
  Kalshi from EventEdge Oracle, paying per request over x402.
api: EventEdge Oracle Agent Context API
generated: '2026-09-15'
method: generated
source: openapi/eventedge-oracle-openapi.json
operations:
  - agentDecisionContext
  - agentMarketContext
---

# EventEdge: topic decision context

Read-only decision support for autonomous agents. EventEdge never executes trades.

## Steps

1. **Preflight (free).** `GET /v1/agent/preflight` to read the live routes and prices.
   No payment. Confirms `decision_context` price ($0.01) and that x402 is on network
   `eip155:8453` (Base).
2. **Request decision context.** Call `agentDecisionContext`:
   `GET /v1/agent/decision-context?term=<topic>` (the `term` query param is required —
   e.g. `term=OpenAI`).
3. **Handle the 402.** The first call returns `402 Payment Required` with a
   `payment-required` header carrying the x402 `accepts` array (scheme `exact`, USDC
   asset, `payTo` address, amount, `maxTimeoutSeconds` 300). Settle the USDC
   micropayment on Base and retry with the x402 payment proof.
4. **Read the payload.** A `200` returns combined market pulse, topic comparison,
   change signal, and a teaser for the topic. Responses are plain `application/json`.

## Notes

- For cheaper recurring monitoring use `agentMarketContext`
  (`GET /v1/agent/market-context`, $0.005) instead.
- Errors are not RFC 9457 problem+json; the only documented failure is the 402
  payment challenge.
- No API key, no signup — access is economic (x402), not credentialed.
