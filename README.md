# UAPL — Unified African Payment Layer

> One API. Every African payment provider. Zero fragmentation.

---

## The Problem

Africa has some of the most innovative mobile money ecosystems in the world. MTN MoMo operates across 17 countries. Orange Money covers 17 markets. M-Pesa dominates East Africa. Flutterwave, Wave, Airtel Money — the list goes on.

But for a developer trying to build a product that works across the continent, this is a nightmare.

To accept payments in Cameroon, Kenya, and Ghana, you need to:
- Read 3 different API documentations
- Handle 3 different authentication flows
- Parse 3 different webhook formats
- Maintain 3 separate integrations every time a provider changes their API
- Repeat this for every new country you expand into

**African developers spend 80% of their time on payment integrations and 20% building their actual product.** UAPL flips that.

---

## The Solution

UAPL is a unified abstraction layer that sits between your application and every African payment provider.

You write one integration. We handle the rest.

```js
const result = await startPayment({
  amount: 2500,
  currency: 'XAF',
  phone: '+237670000000',
  reference: 'order-123',
});

if (result.status === 'success') {
  // That's it. Money received.
}
```

One function call. Works in Cameroon, Kenya, Ghana, Nigeria, Senegal — and every new country we add.

---

## What Makes UAPL Different

### 🌍 Built for Africa, not adapted for it
Most payment infrastructure is built for Western markets and bolted onto Africa as an afterthought. UAPL is designed from the ground up for the realities of the continent — unstable networks, USSD-based flows, multi-currency transactions, and the dominance of mobile money over cards.

### ⚡ Real-time by default
No polling. No "check status" buttons. When a user confirms their payment on their phone, your UI updates instantly. The confirmation just appears — like magic.

### 🔄 Provider redundancy — your platform never goes dark
MTN goes down for 2 hours. If it's your only payment method, your revenue stops completely. With UAPL, you integrate once and offer every available provider. If a user's network is unavailable, they're immediately offered an alternative — Orange Money, M-Pesa, whatever is live. Your platform keeps accepting payments. No downtime, no lost sales, no emergency hotfixes at 2am.

### 🧩 Modular and extensible
Every provider is an isolated adapter. Adding a new country or provider doesn't touch existing code. The system grows with the continent.

### 🛡️ Secure by design
Webhook signature verification, idempotency protection against double charges, and encrypted communication at every layer. Financial-grade security without the complexity.

### 🧑‍💻 Developer experience first
If a developer can't make their first payment in under 10 minutes, we've failed. The SDK is designed to be the simplest payment integration on the continent.

---

## Supported Providers

| Provider | Region | Status |
|---|---|---|
| MTN Mobile Money | CM, GH, RW, UG, CI, BJ | 🔄 In Development |
| Orange Money | CM, SN, CI, ML, BF | 🔄 In Development |
| Campay | CM | 🔄 In Development |
| M-Pesa (Safaricom) | KE, TZ | 🔄 In Development |
| Flutterwave | 20+ countries | 🗓️ Planned |
| Stripe | Global (cards) | 🗓️ Planned |
| Paystack | NG, GH, ZA, KE | 🗓️ Planned |
| Wave | SN, CI, ML, BF | 🗓️ Planned |

> Don't see your country or provider? [Open an issue](../../issues) — community requests shape our roadmap.

---

## The SDK

### React / Next.js

```bash
npm install @uapl/sdk-react
```

```tsx
import { UAPLProvider, usePayment } from '@uapl/sdk-react';

// 1. Wrap your app
export default function App() {
  return (
    <UAPLProvider serverUrl="https://your-uapl-instance.com">
      <YourApp />
    </UAPLProvider>
  );
}

// 2. Accept payments anywhere
function CheckoutButton() {
  const { startPayment } = usePayment();

  return (
    <button onClick={() => startPayment({
      amount: 5000,
      currency: 'XAF',
      phone: '+237670000000',
      reference: 'order-456',
    })}>
      Pay 5,000 XAF
    </button>
  );
}
```

### Vanilla JavaScript

```bash
npm install @uapl/sdk-js
```

```js
import { UAPLClient } from '@uapl/sdk-js';

const uapl = new UAPLClient({ serverUrl: 'https://your-uapl-instance.com' });

const result = await uapl.pay({
  amount: 5000,
  currency: 'KES',
  phone: '+254712345678',
  reference: 'order-789',
});
```

---

## The Universal Payment Object

Every transaction — regardless of provider or country — is represented in the same format:

```typescript
// What you send
{
  amount: 5000,           // Smallest currency unit
  currency: 'XAF',        // ISO 4217
  phone: '+237670000000', // E.164 format
  reference: 'order-123', // Your idempotency key — prevents double charges
  description: 'Order payment',
  webhookUrl: 'https://yourapp.com/webhook', // Optional: get notified server-side
}

// What you get back
{
  transactionId: 'uapl_...',
  status: 'success',
  provider: 'mtn-momo',
  country: 'CM',
  amount: 5000,
  currency: 'XAF',
  providerRef: 'MTN-REF-...',
  fee: 50,
  timestamp: '2026-04-14T12:00:00Z',
}
```

---

## Architecture

```
Your App
   │
   ▼
UAPL SDK          ← One integration. That's all you write.
   │
   ▼
UAPL Server       ← Routes, retries, real-time updates, failover
   │
   ├── MTN MoMo
   ├── Orange Money
   ├── M-Pesa
   └── + more
```

The server handles provider selection, automatic failover, webhook processing, and real-time status delivery. Your app only ever talks to UAPL.

---

## Why Now

- Mobile money transaction value in Africa exceeded **$900 billion** in 2023
- There are **over 700 million** registered mobile money accounts on the continent
- Cross-border payment costs in Africa average **8%** — the highest in the world
- The African Continental Free Trade Area (AfCFTA) is creating demand for seamless cross-border commerce
- African developers are building world-class products — they deserve world-class infrastructure

The infrastructure gap is real. The market is ready. The timing is now.

---

## Project Status

UAPL is currently in active development. The core architecture is defined, the SDK interface is stable, and the first provider integrations are underway.

This repository contains the public SDK packages and documentation. Development is open and transparent — follow along, contribute, or reach out.

---

## Get Involved

- ⭐ Star this repo to follow progress
- 🐛 [Open an issue](../../issues) to request a provider or report a bug
- 💬 [Start a discussion](../../discussions) for questions or ideas
- 🤝 Interested in partnering, investing, or integrating early? Reach out directly.

---

## Roadmap

- [x] Architecture design & specification
- [x] Universal Payment Object definition
- [x] SDK interface design
- [ ] Mock adapter (for local development without real credentials)
- [ ] Campay adapter — first live integration (Cameroon)
- [ ] MTN MoMo adapter
- [ ] Orange Money adapter
- [ ] `@uapl/sdk-react` v0.1.0-alpha on npm
- [ ] `@uapl/sdk-js` v0.1.0-alpha on npm
- [ ] Developer dashboard
- [ ] M-Pesa adapter (Kenya)
- [ ] Multi-country routing
- [ ] Public beta

---

*Building the payment infrastructure Africa deserves.*
