# Awesome Agentic Commerce

> Curated index of open-source plugins, libraries, and reference implementations for **agentic commerce** — the layer where AI agents (ChatGPT, Claude, Gemini, custom) interact with merchant storefronts on behalf of human buyers.

Covers the active protocols ([ACP](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol), [UCP](https://github.com/Universal-Commerce-Protocol/ucp), [AP2](https://github.com/google-agentic-commerce/AP2)), the discovery standards ([llms.txt](https://llmstxt.org), [A2A agent-card](https://a2a-protocol.org/), schema.org JSON-LD), the payment rails ([MPP](https://mpp.dev), [x402](https://x402.org), cards, stablecoins), and the per-platform integrations that tie them together.

Maintained by [xpay✦](https://www.xpay.sh). PRs welcome.

## Contents

- [Protocols](#protocols)
- [Per-platform plugins](#per-platform-plugins)
- [Reference implementations & SDKs](#reference-implementations--sdks)
- [Payment rails for agents](#payment-rails-for-agents)
- [Discovery standards](#discovery-standards)
- [Conformance & tooling](#conformance--tooling)
- [Related curated lists](#related-curated-lists)

---

## Protocols

| Protocol | Spec | Sponsor | Surface |
|---|---|---|---|
| **ACP** — Agentic Commerce Protocol | [agentic-commerce-protocol/agentic-commerce-protocol](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) | OpenAI · Stripe · Meta (TSC) | Cart, checkout, delegated payment, discounts, fulfillment |
| **UCP** — Universal Commerce Protocol | [Universal-Commerce-Protocol/ucp](https://github.com/Universal-Commerce-Protocol/ucp) | Tech Council + Governance Council (vendor-neutral) | Cart, checkout, order, catalog, refunds, disputes — RFC 9421 signed requests, MCP/A2A bindings |
| **AP2** — Agent Payments Protocol | [google-agentic-commerce/AP2](https://github.com/google-agentic-commerce/AP2) | Google | Signed mandates, A2A transport |
| Forter's TACP — Trusted Agentic Commerce | [forter/trusted-agentic-commerce-protocol](https://github.com/forter/trusted-agentic-commerce-protocol) | Forter | Parallel agent-authentication / trust-signal overlay (JWS+JWE, JWKS, fraud-signal envelopes). Not a fork of ACP; orthogonal to the cart/checkout surface. |

Side-by-side technical comparison: **[ACP vs UCP vs AP2](https://docs.xpay.sh/agentic-commerce-protocols/comparison)**.

---

## Per-platform plugins

Each row: which protocols it speaks, license, and link.

### WooCommerce

- **[xpaysh/agentic-commerce-for-woocommerce](https://github.com/xpaysh/agentic-commerce-for-woocommerce)** — v0.2.x (PHP reference). ACP + UCP + AP2. WordPress plugin. GPLv2.

### commercetools

- **[xpaysh/agentic-commerce-for-commercetools](https://github.com/xpaysh/agentic-commerce-for-commercetools)** — v0.2.1. TS reference. ACP + UCP + AP2. RFC 9421 signature verification middleware on `/ucp/*` + `/acp/*` (env-gated).

### BigCommerce

- **[xpaysh/agentic-commerce-for-bigcommerce](https://github.com/xpaysh/agentic-commerce-for-bigcommerce)** — v0.2.1. ACP + UCP + AP2. BigCommerce App via App Marketplace.

### Magento / Adobe Commerce

- **[xpaysh/agentic-commerce-for-magento](https://github.com/xpaysh/agentic-commerce-for-magento)** — v0.2.1. ACP + UCP + AP2. Consolidates two unmaintained upstream ACP modules (28★, 13★) onto multi-protocol coverage.

### Shopify

- **[Shopify/Shopify-AI-Toolkit](https://github.com/Shopify)** *(name approximate)* — official Shopify AI/UCP path. Vendor-maintained.
- **[xpaysh/agentic-commerce-for-shopify](https://github.com/xpaysh/agentic-commerce-for-shopify)** — v0.2.1. ACP + UCP + AP2. Shopify App composing with Shopify's UCP-native flow (composes, doesn't compete). App Store packaging in a follow-on private repo.

### Salesforce Commerce Cloud / Demandware

- **[xpaysh/agentic-commerce-for-salesforce-commerce](https://github.com/xpaysh/agentic-commerce-for-salesforce-commerce)** — v0.2.1. ACP + UCP + AP2. B2C Commerce cartridge + PWA Kit extension.

### Saleor

- **[saleor/saleor-mcp](https://github.com/saleor)** — official Saleor MCP server (the MCP transport binding).
- **[xpaysh/agentic-commerce-for-saleor](https://github.com/xpaysh/agentic-commerce-for-saleor)** — v0.2.1. ACP + UCP + AP2 protocol layer; composes with saleor-mcp, doesn't replace it.

### PrestaShop

- **[xpaysh/agentic-commerce-for-prestashop](https://github.com/xpaysh/agentic-commerce-for-prestashop)** — v0.2.1. ACP + UCP + AP2. PrestaShop module; strongest EU-regional presence in the family.

### OpenCart, Shopware, Spree, Sylius, nopCommerce, Drupal Commerce, Ecwid

*Template-based, community-contributed.* See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for the walkthrough, [`BOUNTIES.md`](./BOUNTIES.md) for reward amounts on accepted plugins, and [`xpaysh/agentic-commerce-plugin-template`](https://github.com/xpaysh/agentic-commerce-plugin-template) for the starter kit.

---

## Reference implementations & SDKs

- **[vercel/acp-handler](https://github.com/vercel/acp-handler)** — generic Next.js handler for ACP. Platform-agnostic.
- **[NVIDIA-AI-Blueprints/Retail-Agentic-Commerce](https://github.com/NVIDIA-AI-Blueprints/Retail-Agentic-Commerce)** — NVIDIA blueprint covering ACP + UCP.
- **[Universal-Commerce-Protocol/js-sdk](https://github.com/Universal-Commerce-Protocol/js-sdk)** — official UCP TypeScript SDK with RFC 9421 verifier.
- **[Universal-Commerce-Protocol/python-sdk](https://github.com/Universal-Commerce-Protocol/python-sdk)** — official UCP Python SDK.
- **[Universal-Commerce-Protocol/ucp-schema](https://github.com/Universal-Commerce-Protocol/ucp-schema)** — Rust validator.
- **[xpaysh/agentic-commerce-plugin-template](https://github.com/xpaysh/agentic-commerce-plugin-template)** — TypeScript monorepo template for new per-platform plugins. Publishes the `@xpaysh/*` package family below.

### `@xpaysh/*` packages (npm)

| Package | What it ships |
|---|---|
| [`@xpaysh/acp-schemas`](https://www.npmjs.com/package/@xpaysh/acp-schemas) | Real ACP JSON Schemas vendored from upstream `spec/2026-04-17`. 7 bundles, 140 type defs. |
| [`@xpaysh/ucp-schemas`](https://www.npmjs.com/package/@xpaysh/ucp-schemas) | 83 UCP schemas vendored from `Universal-Commerce-Protocol/ucp` source. `generateUcpProfile()` + `registerForValidation(ajv)`. |
| [`@xpaysh/ap2-schemas`](https://www.npmjs.com/package/@xpaysh/ap2-schemas) | AP2 namespace (`SPEC_VERSION='draft'` while upstream is pre-stable). |
| [`@xpaysh/adapter-contract`](https://www.npmjs.com/package/@xpaysh/adapter-contract) | The `PlatformAdapter` TS interface every per-platform plugin implements. |
| [`@xpaysh/template-adapter`](https://www.npmjs.com/package/@xpaysh/template-adapter) | Working reference `PlatformAdapter` backed by a deterministic in-memory catalog — copy as the starting point for a new plugin. |
| [`@xpaysh/discovery`](https://www.npmjs.com/package/@xpaysh/discovery) | Pure-function generators for `/llms.txt`, schema.org JSON-LD, `robots.txt`, A2A `agent-card.json`, RFC 9728. Zero deps. |
| [`@xpaysh/cart-deeplinks`](https://www.npmjs.com/package/@xpaysh/cart-deeplinks) | HS256-signed JWT cart-handoff URLs. |
| [`@xpaysh/storefront-audit`](https://www.npmjs.com/package/@xpaysh/storefront-audit) | Discovery-layer auditor + `ac-doctor` CLI; rejects fictitious well-known URIs. |
| [`@xpaysh/http-message-signatures`](https://www.npmjs.com/package/@xpaysh/http-message-signatures) | RFC 9421 sign/verify; ed25519 + hmac-sha256; targets the component set UCP REST uses. |
| [`@xpaysh/conformance-fixtures`](https://www.npmjs.com/package/@xpaysh/conformance-fixtures) | Golden ACP + UCP request/response payloads; every fixture cross-validates against the canonical schemas via Ajv. |
| [`@xpaysh/acp-session-store`](https://www.npmjs.com/package/@xpaysh/acp-session-store) | ACP checkout-session storage: InMemorySessionStore + DynamoDBSessionStore behind a single interface. |
| [`@xpaysh/lint-wellknowns`](https://www.npmjs.com/package/@xpaysh/lint-wellknowns) | CI linter + GitHub Action that fails builds emitting fictitious well-known URIs. |

---

## Payment rails for agents

These sit *below* the commerce protocols. Any commerce protocol can plug into any rail.

- **[Stripe MPP](https://mpp.dev)** — Machine Payments Protocol. Cards, stablecoins, fiat acceptance.
- **[x402](https://x402.org)** — HTTP 402 payment rail. Stablecoin settlement.
- **[TWZRD Agent Intel](https://intel.twzrd.xyz)** — Trust scoring and x402 micropayment verification for AI agents on Solana. Verify counterparty wallets before transacting; free `resolve_agent`/`score_agent`/`preflight_check` and paid `get_trust_receipt` via HTTP 402. MCP endpoint: `https://intel.twzrd.xyz/mcp`. (`pip install twzrd-agent-intel`)
- **Cards** — Stripe, Adyen, Braintree, etc.
- **Stablecoin direct** — Bridge, Tempo, Privy.

---

## Discovery standards

Real, externally-verifiable standards (no invented well-known URIs):

- **[llmstxt.org](https://llmstxt.org)** — `/llms.txt`, Markdown, LLM-readable site map. Use this.
- **[a2a-protocol.org](https://a2a-protocol.org/)** — `/.well-known/agent-card.json` (A2A 1.0, IANA-registered 2025-08-01).
- **[schema.org](https://schema.org)** — JSON-LD `Product`, `Offer`, `AggregateOffer`, `BreadcrumbList`.
- **[RFC 9309](https://datatracker.ietf.org/doc/rfc9309/)** — `robots.txt`. Real AI user-agents to allowlist or block: `GPTBot`, `ClaudeBot`, `Google-Extended`, `PerplexityBot`, `CCBot`, `Amazonbot`.
- **[RFC 9728](https://datatracker.ietf.org/doc/rfc9728/)** — `/.well-known/oauth-protected-resource` for agent OAuth.
- **[UCP business profile](https://ucp.dev/latest/specification/overview/) / [Google's UCP guide](https://developers.google.com/merchant/ucp/guides/ucp-profile)** — `/.well-known/ucp` (no extension). Spec status: `draft` upstream; the on-the-wire `version` field is a YYYY-MM-DD date (e.g. `2026-05-18`). The capability-negotiation entry point that Google + Shopify + Etsy + Wayfair + Target + Walmart fetch.

### Files to **not** emit

`/.well-known/agentic-commerce.json`, `/.well-known/ucp.json` (the real UCP profile path is `/.well-known/ucp` — no extension), `/.well-known/acp.json`, `/.well-known/ap2.json`, `/.well-known/mcp.json`, `/.well-known/ai-plugin.json` (deprecated), `/agents.txt`, `/ai.txt` — none of these are in any active spec. If you see a plugin emitting one of these, please [open an issue](https://github.com/xpaysh/awesome-agentic-commerce/issues).

---

## Conformance & tooling

- **[Universal-Commerce-Protocol/conformance](https://github.com/Universal-Commerce-Protocol/conformance)** — UCP conformance suite.
- **[Universal-Commerce-Protocol/samples](https://github.com/Universal-Commerce-Protocol/samples)** — sample agents & merchants.

---

## Related curated lists

- **[Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)** — sister ecosystem; many entries are MCP servers exposed by commerce platforms.
- **[xpaysh/awesome-agentic-economy](https://github.com/xpaysh/awesome-agentic-economy)** — broader: agentic-economy resources (monetization, identity, payment).
- **[Awesome x402](https://github.com/xpaysh/awesome-x402)** — x402-specific resources.

---

## Contributing

This list aims for **signal over volume**. Two ways in:

- **Add an existing plugin or library** — open a PR adding your repo to the relevant section. Acceptance criteria + sort rules in [`CONTRIBUTING.md`](./CONTRIBUTING.md).
- **Build a new per-platform plugin** — bounties pay $100–500 per accepted plugin. Tiers, eligibility, and process in [`BOUNTIES.md`](./BOUNTIES.md). Starter kit: [`xpaysh/agentic-commerce-plugin-template`](https://github.com/xpaysh/agentic-commerce-plugin-template).

## License

[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) — public domain dedication. Use this list freely.
