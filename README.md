# GTVpn checkout page

This is a standalone website, not a Telegram Mini App. It contains no payment or panel credentials.

## Checkout flow

1. The user either opens this page from the bot using a short-lived checkout token or enters the recipient's Telegram ID.
2. The backend validates the checkout token when present, then creates a RollyPay payment bound to that Telegram ID. The checkout saves the order and immediately redirects to the secure RollyPay payment page.
3. RollyPay sends a signed webhook to the bot backend.
4. On `payment.paid`, the backend creates a one-time activation code. The page shows it after polling the verified order status, and the bot attempts to send it to the recipient's chat.
5. The buyer enters `/vpn CODE` in the bot. The bot creates or renews the H1/VLESS client and sends the subscription link.
6. An admin can assign a paid code to another Telegram ID with `/vpn TELEGRAM_ID CODE` in a private chat with the bot. If that user has not started the bot, the code remains queued and the link is sent after they use `/start`.

## H1 bot environment

Set these variables on the bot service. Never put their values in this repository:

- `BOT_TOKEN` — a newly issued Telegram bot token; revoke any token previously embedded in source.
- `ADMIN_PASSWORD` — a new admin password; do not reuse the previous source value.
- `PAYMENT_SIGNING_SECRET` — independent random secret for Telegram payment payload signatures.
- `ROLLYPAY_API_KEY` — RollyPay API key.
- `ROLLYPAY_SIGNING_SECRET` — callback signing secret from the RollyPay terminal.
- `H1_PANEL_API_URL` — the API address shown in H1/VLESS, including `/api`.
- `H1_PANEL_API_TOKEN` — the H1/VLESS panel API token, not the H1 account token.
- `PORT` — H1 app port; defaults to `25116`.
- `PAYMENT_PAGE_ORIGIN` — defaults to `https://sweetydsem.github.io`.
- `ROLLYPAY_TERMINAL_ID` and `ROLLYPAY_API_URL` are optional; defaults are set in the bot.
- `ROLLYPAY_TEST_MODE=true` optionally creates sandbox payments. Test payments never provision a subscription.

The previous source version contained credentials. Rotate the Telegram bot token through BotFather, replace the admin password, and remove old credentials from any published repository history before restarting the bot. Set the replacement values in the H1 service's private environment settings, not in chat or source files.

Configure the RollyPay success and failure return URLs as:

- `https://sweetydsem.github.io/gtvpn-payment/success.html`
- `https://sweetydsem.github.io/gtvpn-payment/fail.html`

Both pages link to the bot and list `sweetydjemo@gmail.com` for support. The success page is only a return page; payment is confirmed by the signed webhook.

The activation code is random, single-use, and only becomes valid after a signed `payment.paid` webhook. The checkout page enables payment when it has a bot-issued checkout token or a valid Telegram ID; the backend still validates the token when one is provided. Order-status requests use a separate random status token.

Set the RollyPay terminal callback URL to:

```text
https://bot176991.48-103-231-150.sslip.io/webhooks/rollypay
```

The H1/VLESS API URL and token must be copied from that panel's API access settings. For clients, the bot uses the panel's default inbound/channel settings and sets the tariff's device and traffic limits.
