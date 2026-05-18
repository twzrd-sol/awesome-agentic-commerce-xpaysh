# Contributing to `awesome-agentic-commerce`

This list aims for **signal over volume**. Two ways to contribute:

1. [Add an existing plugin or library](#1-add-an-existing-plugin-or-library) to the curated index.
2. [Build a new per-platform plugin](#2-build-a-new-per-platform-plugin) from the [`xpaysh/agentic-commerce-plugin-template`](https://github.com/xpaysh/agentic-commerce-plugin-template) starter kit (bounty-eligible — see [`BOUNTIES.md`](./BOUNTIES.md)).

---

## 1. Add an existing plugin or library

Open a PR adding your repo to the relevant section of [`README.md`](./README.md).

**Acceptance criteria:**

- Implements at least one of **ACP / UCP / AP2** against a real eCommerce platform, OR provides a meaningful SDK / tool for the ecosystem.
- Emits real-standard discovery files (`/llms.txt`, `schema.org` JSON-LD, optionally `/.well-known/ucp` for UCP plugins). **Does NOT emit** any of the [fictitious well-known URIs](./README.md#files-to-not-emit). Run [`npx @xpaysh/lint-wellknowns scan`](https://www.npmjs.com/package/@xpaysh/lint-wellknowns) in CI to keep this honest.
- Actively maintained (commit within the last 90 days) **OR** is the only known plugin for that platform.
- Open-source license — Apache-2.0, MIT, BSD, GPL-family, or equivalent.

We sort by **last-commit-date** within each platform.

---

## 2. Build a new per-platform plugin

The 8 first-party plugins ([WooCommerce, commercetools, BigCommerce, Magento, Saleor, PrestaShop, Shopify, SFCC](./README.md#per-platform-plugins)) all share the same TypeScript skeleton. Adding a new platform is well-paved.

### Walkthrough

#### Step 1 — Fork the template

```bash
gh repo create yourname/agentic-commerce-for-<platform> \
  --template xpaysh/agentic-commerce-plugin-template --public
git clone https://github.com/yourname/agentic-commerce-for-<platform>
cd agentic-commerce-for-<platform>
```

(The template is a monorepo. For a single-platform plugin, copy `packages/template-adapter` into the new repo's `src/` and discard the workspace skeleton — see [`packages/template-adapter/README.md`](https://github.com/xpaysh/agentic-commerce-plugin-template/tree/main/packages/template-adapter#pattern-for-contributing-a-new-platform) for the full pattern.)

#### Step 2 — Implement the adapter

Implement the [`PlatformAdapter`](https://www.npmjs.com/package/@xpaysh/adapter-contract) interface against your platform's native client:

```ts
import type { PlatformAdapter, Cart, Order, Product } from '@xpaysh/adapter-contract';

export class YourPlatformAdapter implements PlatformAdapter {
  async listProducts(query) { /* GraphQL / REST call */ }
  async getProduct(id)      { /* … */ }
  async createCart(input)   { /* … */ }
  async updateCart(id, mut) { /* … */ }
  async completeCheckout(i) { /* … */ }
  async getOrder(id)        { /* … */ }
}
```

Reference impl: [`@xpaysh/template-adapter`](https://www.npmjs.com/package/@xpaysh/template-adapter) (in-memory) + any sibling plugin (commercetools is the TS reference).

#### Step 3 — Reuse the protocol layer

Don't reimplement protocol mechanics. Pull from the published packages:

| Need | Package |
|---|---|
| `/.well-known/ucp` body | [`@xpaysh/ucp-schemas`](https://www.npmjs.com/package/@xpaysh/ucp-schemas) `generateUcpProfile()` |
| `/llms.txt`, robots, schema.org JSON-LD, A2A agent-card, RFC 9728 | [`@xpaysh/discovery`](https://www.npmjs.com/package/@xpaysh/discovery) |
| HS256 cart-handoff JWTs | [`@xpaysh/cart-deeplinks`](https://www.npmjs.com/package/@xpaysh/cart-deeplinks) |
| RFC 9421 signature verify on `/ucp/*` + `/acp/*` | [`@xpaysh/http-message-signatures`](https://www.npmjs.com/package/@xpaysh/http-message-signatures) |
| ACP session storage (in-memory dev / DDB prod) | [`@xpaysh/acp-session-store`](https://www.npmjs.com/package/@xpaysh/acp-session-store) |
| Schemas to validate against | [`@xpaysh/acp-schemas`](https://www.npmjs.com/package/@xpaysh/acp-schemas), [`@xpaysh/ucp-schemas`](https://www.npmjs.com/package/@xpaysh/ucp-schemas) |

#### Step 4 — Wire CI

Copy from a sibling:

- `.github/workflows/ci.yml` — typecheck + build + `npm test`
- `tests/conformance.test.ts` — runs [`@xpaysh/conformance-fixtures`](https://www.npmjs.com/package/@xpaysh/conformance-fixtures) through Ajv; no platform credentials needed
- Use the [`lint-wellknowns` GitHub Action](https://github.com/xpaysh/agentic-commerce-plugin-template/tree/main/.github/actions/lint-wellknowns) to enforce no-fictitious-URI on every PR

#### Step 5 — Submit

When your plugin passes the conformance test, the lint action is clean, and you've smoke-tested against a sandbox store:

1. Open a PR to **this repo** adding your plugin under the right `### Platform` heading in `README.md`.
2. Mention the linked PR if you want to claim a [bounty](./BOUNTIES.md).
3. Tag `@xpaysh` for review.

### What we look for in review

- All required PlatformAdapter methods implemented (not `throw new Error('not implemented')`).
- Wire shapes match the conformance fixtures.
- README explains: which protocol versions, which auth model, how to install, env vars.
- License = Apache-2.0 or MIT (preferred) or GPL-family (acceptable).
- No invented well-known URIs (the linter catches this).

---

## Questions

Open an issue on this repo, or ping [xpay✦](https://www.xpay.sh) directly.
