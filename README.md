# GTVpn checkout page

This is a standalone website, not a Telegram Mini App. It contains no payment or panel credentials.

## Checkout flow

1. The user opens this page from the bot using a short-lived checkout token.
2. The backend verifies that the user started the bot and creates a RollyPay payment.
3. RollyPay sends a signed webhook to the bot backend.
4. On `payment.paid`, the backend creates or renews the H1/VLESS client and sends its subscription link to the user.

## H1 bot environment

Set these variables on the bot service. Never put their values in this repository:

- `ROLLYPAY_API_KEY` — RollyPay API key.
- `ROLLYPAY_SIGNING_SECRET` — callback signing secret from the RollyPay terminal.
- `H1_PANEL_API_URL` — the API address shown in H1/VLESS, including `/api`.
- `H1_PANEL_API_TOKEN` — the H1/VLESS panel API token, not the H1 account token.
- `PORT` — H1 app port; defaults to `25116`.
- `PAYMENT_PAGE_ORIGIN` — defaults to `https://sweetydsem.github.io`.
- `ROLLYPAY_TERMINAL_ID` and `ROLLYPAY_API_URL` are optional; defaults are set in the bot.
- `ROLLYPAY_TEST_MODE=true` optionally creates sandbox payments. Test payments never provision a subscription.

Set the RollyPay terminal callback URL to:

```text
https://de-bots8.h1cloud.net:25116/webhooks/rollypay
```

The H1/VLESS API URL and token must be copied from that panel's API access settings. For clients, the bot uses the panel's default inbound/channel settings and sets the tariff's device and traffic limits.
