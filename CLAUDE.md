# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the App

This is a zero-dependency static site — no build step required.

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## Architecture

The entire application lives in a single file: `index.html` (≈1,800 lines, 72 KB). There is no bundler, framework, or package manager.

The file is structured in three co-located layers:

1. **HTML** — page skeleton: nav, hero, sidebar filters, product grid, cart drawer, checkout modal, FAQ, footer
2. **CSS** (`<style>` tag) — ~7,000 lines; uses CSS custom properties for the color palette (`--pink`, `--cream`, `--lavender`, `--coral`, `--dark`, `--usa-blue`); two responsive breakpoints at 900 px and 480 px
3. **JavaScript** (`<script>` tag, lines ~1573–1842) — vanilla ES6+, no imports

### JS Data & State

```js
const products = [/* 6 items: 3 mystery scoops + 3 handbags */];
let cart = [];           // [{id, name, price, emoji, qty}]
let cartTotal, cartCount, activeTag, sortMode, billingStep;
```

### JS Functional Groups

| Group | Key functions |
|---|---|
| Rendering | `renderProducts()`, `renderCartItems()` |
| Filtering/sorting | `getFiltered()`, `filterTag()`, `sortProducts()`, `applyPrice()` |
| Cart | `addToCart()`, `changeQty()`, `removeItem()`, `recalcCart()` |
| Shipping bar | `updateShippingBar()` — free shipping threshold is $300 |
| Checkout (4-step modal) | `openBilling()`, `goStep()`, `buildReview()`, `placeOrder()` |
| Utilities | `showToast()`, `toggleFaq()`, `formatCard()`, `formatExpiry()`, `validateStep1/2()` |

Checkout steps: 1 = shipping info → 2 = payment → 3 = order review → 4 = confirmation.
