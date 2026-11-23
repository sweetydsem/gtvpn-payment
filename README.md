# GTVpn checkout page

This is a standalone website, not a Telegram Mini App. It contains no RollyPay credentials.

## GitHub Pages

Publish this folder from the `main` branch at the repository root. After the H1 backend has a public HTTPS address, replace the placeholder in `config.js` with:

```js
window.GTVPN_PAYMENT_API = "https://YOUR-H1-PUBLIC-URL/api/rollypay/create-payment";
```

## H1 backend settings

Set these environment variables on the bot service:

- `ROLLYPAY_API_KEY` — RollyPay API key.
- `PAYMENT_PAGE_ORIGIN` — `https://sweetydsem.github.io`.
- `ROLLYPAY_TERMINAL_ID` — optional; defaults to the terminal ID supplied for this integration.
- `ROLLYPAY_API_URL` — optional; defaults to the create-payment endpoint supplied for this integration.
- `PORT` — the public HTTP port assigned by H1 Cloud.

The backend creates a pending payment and returns its `pay_url`. Payment verification and subscription delivery are manual: match the order ID in the RollyPay dashboard with the Telegram ID embedded in it, then grant the subscription in the bot's developer panel. No webhook signing secret is used in this manual flow.
