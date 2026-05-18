# Community-plugin bounty program

xpay✦ pays bounties for **accepted, community-maintained** plugins for the platforms below. The goal is long-tail platform coverage — the 8 first-party plugins handle the head; bounties fund the rest.

> **TL;DR:** ship a working `agentic-commerce-for-<platform>` plugin, submit it to this list, get paid. Amounts below.

## Tiers

| Tier | Reward | Platforms in scope |
|---|---|---|
| **A** — high-traffic + clean API | **$500** | OpenCart, Shopware 6, Wix Stores, Squarespace Commerce |
| **B** — moderate scale + decent API | **$300** | Spree, Sylius, nopCommerce, Drupal Commerce, Ecwid, Ecwid by Lightspeed |
| **C** — regional / long-tail | **$200** | VTEX (LatAm), Salla (MENA), Cafe24 (KR), Tiendanube / Nuvemshop (LatAm), Bagisto, CS-Cart, X-Cart |
| **D** — additive packaging | **$100** | Hosted-platform adapters (Webflow eCommerce, Big Cartel, Square Online), Headless storefront SDKs (Hydrogen, Next-Commerce, Vue Storefront integrations) |

If your platform isn't listed and has a non-trivial merchant base, [open an issue](https://github.com/xpaysh/awesome-agentic-commerce/issues) — we'll set a tier.

## Acceptance criteria (every tier)

A plugin earns a bounty when **all** of these are true:

1. **Implements the full [`PlatformAdapter`](https://www.npmjs.com/package/@xpaysh/adapter-contract) interface** — `listProducts`, `getProduct`, `createCart`, `updateCart`, `completeCheckout`, `getOrder`. No `throw new Error("not implemented")`.
2. **Serves at least UCP** (`/.well-known/ucp` + the shopping REST surface). ACP and AP2 are nice-to-haves; UCP is mandatory.
3. **Real-standard discovery** — emits `/llms.txt`, schema.org JSON-LD, `robots.txt`. Does not emit any fictitious well-known URIs (the [`@xpaysh/lint-wellknowns`](https://www.npmjs.com/package/@xpaysh/lint-wellknowns) Action passes on `main`).
4. **RFC 9421 signature verification** on `/ucp/*` and `/acp/*` routes via [`@xpaysh/http-message-signatures`](https://www.npmjs.com/package/@xpaysh/http-message-signatures) (env-gated is fine).
5. **Conformance suite passes** — `tests/conformance.test.ts` validates ACP + UCP fixtures against the schemas. `npm test` is green.
6. **README** explains: which protocol versions, auth model, install instructions, env vars, supported platform versions, known limitations.
7. **License** is Apache-2.0, MIT, BSD, or GPL-family.
8. **Smoke-tested against a real sandbox store** — record a 90-second screen capture or a tested-commit checklist in the PR.

## Process

1. **Claim** (optional but recommended) — open an issue titled `Bounty claim: <platform>` so two contributors don't duplicate work.
2. **Build** in your own GitHub repo. Use [`xpaysh/agentic-commerce-plugin-template`](https://github.com/xpaysh/agentic-commerce-plugin-template) as the starting point — see [`CONTRIBUTING.md`](./CONTRIBUTING.md) for the walkthrough.
3. **Submit** a PR adding your repo to [`README.md`](./README.md) under the right platform heading.
4. **Reference** the bounty in your PR description (`Closes #<issue>, claims bounty for <platform>`).
5. **Review** — xpay✦ reviewers verify the acceptance criteria. Typical turnaround: 1 week.
6. **Payment** — once merged, we pay via the GitHub Sponsors profile you list in your PR, or via direct bank transfer (international supported). Crypto on request.

## What forfeits a bounty

- Submitting a fork of an existing plugin without meaningful new work (we check the diff).
- Plugin fails the conformance test or emits any fictitious well-known URI.
- Plugin is unmaintained 30 days after merge (we won't take it down, but bounty is reduced 50% — community-of-one isn't sustainable).
- Vendoring / sublicensing schemas under terms incompatible with their upstream Apache-2.0 licenses.

## What an excellent submission looks like

- All 6 PlatformAdapter methods land within ~500-1500 LOC for typical REST platforms; ~2000 LOC for ones with quirky APIs (Magento, SFCC).
- The README is concrete: "Tested against Spree 4.6 sandbox at spree-demo.herokuapp.com — see screencast." not "should work with Spree."
- A short "Known limitations" section beats a long "Roadmap" section.
- Conformance test runs in CI on every PR.
- Open-issue triage cadence — community plugins die when issues pile up.

## Tax / legal notes

Bounties are paid as freelance contractor fees. We can issue 1099-NEC (US) or equivalents (international) on request. You're responsible for your local tax treatment. By accepting a bounty you affirm the submitted code is yours to license under the chosen open-source license.

## Updates

Bounty amounts and the platform list are reviewed quarterly. Subscribe to releases on this repo to be notified.
