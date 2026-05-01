# Stripe Commerce Explorer
An interactive, browser-based demo showcasing Stripe's core payment infrastructure products. Built as a product management portfolio piece to demonstrate domain expertise in enterprise payments, checkout optimization, and fraud prevention.

**[Live Demo](https://jesimpsonjr-prodmgmt.github.io/stripe-commerce-explorer)** | **[Portfolio](https://johnsimpson.io)**

---

## Problem Statement

Stripe offers 30+ products spanning payments, billing, financial services, and infrastructure. For merchants evaluating Stripe, the learning curve is steep: documentation is developer-focused, sandbox testing requires API keys, and understanding how products work together requires significant integration effort.

This demo distills three of Stripe's most impactful checkout-facing products into a single interactive experience that anyone can explore without writing code, creating an account, or configuring API keys.

---

## Product Scope

### Why These Three Products

Stripe's product surface is massive. I selected these three because they represent the critical path of every card-not-present transaction: **accept the payment, reduce friction for returning customers, and block fraud before it costs you money.**

| Product | What It Does | Why It Matters |
|---|---|---|
| **Payment Element** | Unified embeddable checkout UI covering cards, wallets (Apple Pay, Google Pay), BNPL (Klarna, Afterpay), and bank debits | Replaces the old approach of integrating each payment method separately. One component, 125+ payment methods, automatic localization. |
| **Link** | One-click checkout using saved credentials across the Stripe merchant network | Addresses cart abandonment (avg. 70% in e-commerce). Returning customers skip manual entry entirely. Stripe reports Link increases conversion by 7%+ on average. |
| **Radar** | ML-powered fraud detection using risk scoring, rules engine, and network-wide signal sharing | Processes risk signals from millions of merchants simultaneously. Blocks fraud before authorization, reducing chargebacks and false declines. Default integration scores every transaction with no merchant configuration. |

### What I Chose Not to Include (and Why)

Scope discipline matters as much as feature selection. These products were considered and deliberately excluded from v1:

- **Stripe Terminal / EMV**: Requires physical hardware or device emulation that can't be meaningfully simulated in a browser. Would reduce demo credibility.
- **Stripe Connect**: Multi-party payment flows (platforms, marketplaces) add architectural complexity that obscures the core checkout story. Better suited as a standalone demo.
- **Stripe Billing**: Subscription and usage-based billing is a distinct domain from checkout. Including it here would blur the narrative focus.
- **Agentic Commerce (ACP)**: Stripe's new AI agent purchasing protocol (launched April 2026) is cutting-edge but too early-stage for a portfolio piece. Limited public documentation and no sandbox support yet.

---

## Architecture Decisions

### Client-Side Only (No Backend)

This demo runs entirely in the browser. No server. No database. No API keys required to view.

**Trade-off acknowledged:** A production Stripe integration requires a backend server to create PaymentIntents and confirm charges securely. This demo intentionally uses Stripe.js in sandbox/test mode and simulated responses to demonstrate the UX flow without requiring infrastructure.

**Why this is the right call for a portfolio piece:**
- Reviewers can access it instantly from any device, including locked-down corporate laptops
- GitHub Pages hosting is free and permanent, so there's no risk of the demo going down because a free-tier server expired
- The code demonstrates understanding of the integration architecture even though it simulates the server-side handshake

### Technology Choices

| Choice | Rationale |
|---|---|
| Vanilla HTML/CSS/JS | Zero build step. Reviewers can read the source directly. No framework knowledge required to understand. |
| Stripe.js (test mode) | Real Stripe SDK loaded from Stripe's CDN. Payment Element renders actual UI components, not mockups. |
| GitHub Pages | Free hosting, custom domain support, SSL included, zero maintenance. |

### What a Production Version Would Require

For transparency, here's what a real implementation adds:

1. **Backend server** (Node.js/Python/Go) to create PaymentIntents via Stripe's server-side SDK
2. **Webhook endpoint** to handle async payment confirmations, disputes, and refund events
3. **Stripe Dashboard** configuration for Radar rules, Link settings, and payment method enablement
4. **PCI compliance**: Stripe Elements handles PCI scope reduction, but the merchant still needs SAQ-A compliance

---

## Product Rationale: Domain Commentary

These annotations reflect 20 years of payments industry experience. They're the kind of context that doesn't appear in Stripe's documentation but matters when making integration decisions.

### On Payment Element

The Payment Element represents Stripe's strategic bet on a "universal checkout component." Before this, merchants had to individually integrate each payment method (cards, wallets, BNPL), each with its own UI component, validation logic, and error handling. The consolidation into a single adaptive element mirrors what happened in the EMV terminal world: semi-integrated solutions replaced bespoke integrations because the maintenance burden was unsustainable.

The key insight is **payment method presentation order**. Stripe's Optimized Checkout Suite uses 100+ signals to dynamically reorder which payment methods appear first based on the customer's device, location, and history. This is not cosmetic; it directly impacts conversion rates. A customer in Germany seeing SEPA Direct Debit first vs. Visa first can mean a 15-20% difference in completion rate.

### On Link

Link is Stripe's answer to a problem the industry has failed to solve for a decade: persistent customer identity across merchants. PayPal solved it by owning the customer relationship (merchants hate this). Apple Pay solved it for Apple device users only. Shop Pay solved it for Shopify merchants only.

Link's advantage is network breadth: any Stripe merchant gets it automatically, and Stripe processes payments for millions of businesses. The cold-start problem (not enough users to be useful, not enough merchants to attract users) is solved by Stripe's existing merchant density.

The risk: Link competes with the card networks' own click-to-pay initiatives (Visa CTP, Mastercard MDES). If issuers push their own tokenized checkout flows aggressively, Link could face headwinds. This is worth watching.

### On Radar

Fraud prevention is where Stripe's network scale creates a genuine moat. Traditional fraud tools (Kount, Ethoca, Verifi) operate on a merchant's data in isolation. Radar scores transactions using signals from the entire Stripe network. If a card was used fraudulently at Merchant A, Merchant B's Radar score reflects that within seconds, even though the two merchants have no relationship.

The practical implication: a small merchant with 1,000 transactions/month gets fraud intelligence calibrated on billions of transactions. That's a capability that previously required enterprise-grade contracts with processors or networks.

The Radar rules engine also represents a product design philosophy worth noting: start with ML defaults that work out of the box (zero-config fraud scoring), then expose a rules engine for merchants who want manual control. This "progressive disclosure of complexity" is a pattern I've applied in my own product work.

---

## Demo Walkthrough

### Tab 1: Payment Element

- Renders a live Stripe Payment Element in test mode
- Demonstrates dynamic payment method display based on simulated customer context
- Shows the adaptive UI responding to card brand detection (Visa vs. Amex field layouts differ)
- Includes test card numbers for simulating successful and declined transactions

### Tab 2: Link

- Demonstrates the one-click checkout flow for returning customers
- Side-by-side comparison: traditional checkout (12+ fields) vs. Link (email + OTP)
- Simulated conversion funnel showing the impact of reduced friction
- Annotated explanation of the session/cookie architecture that enables cross-merchant identity

### Tab 3: Radar

- Interactive fraud scoring simulation with adjustable risk parameters
- Visual breakdown of signal categories: card fingerprint, device, behavioral, network
- Rules engine builder demonstrating block/allow/review threshold configuration
- Sample scenarios: legitimate high-value transaction, stolen card velocity attack, friendly fraud pattern

---

## Running Locally

```
git clone https://github.com/jesimpsonjr-prodmgmt/stripe-commerce-explorer.git
cd stripe-commerce-explorer
open index.html
```

No build step. No dependencies. No API keys. Open the HTML file in any browser.

---

## Project Status

- [x] PRD and architecture documentation
- [ ] Payment Element demo (Tab 1)
- [ ] Link comparison demo (Tab 2)
- [ ] Radar simulation demo (Tab 3)
- [ ] GitHub Pages deployment
- [ ] Portfolio site integration

---

## About

Built by **John Simpson**, Senior Product Manager with 20 years in enterprise fintech and payments.

The code in this project is AI-assisted. The product thinking, domain expertise, architecture decisions, and strategic commentary are mine. This reflects how modern product managers ship: leveraging AI tooling for implementation velocity while applying human judgment for product direction.

- [Portfolio](https://johnsimpson.io)
- [LinkedIn](https://linkedin.com/in/johnsjr)
