# TRADEWEDGE

A ready-to-run React/Vite trading terminal prototype for Deriv Options.

## Included

- Markets: Rise/Fall, Digits (Matches/Differs), Over/Under (Over/Under/Even/Odd)
- Live public Deriv tick stream
- Demo balance and simulated demo trades
- Trade history
- Demo/Real mode separation in the UI
- Deposit/Withdraw UI placeholders
- Security note and architecture boundary for real credentials

## Run it

Requirements: Node.js 20+.

```bash
npm install
npm run dev
```

Open the local URL printed by Vite.

## Real Deriv integration

Do NOT put a Deriv Personal Access Token in `src/main.jsx` or any browser JavaScript.

For a production deployment, add a server/API layer that:
1. Authenticates the user with Deriv OAuth 2.0 or a securely managed PAT.
2. Requests the authenticated WebSocket URL through Deriv's OTP endpoint.
3. Uses the demo account endpoint for demo trading and the real account endpoint only after explicit user authorization.
4. Performs proposal -> buy -> proposal_open_contract/sell operations server-side or through a securely authenticated session.
5. Handles wallet/payment operations with the required payment scope and verification flow.
6. Stores secrets only in server environment variables / a secret manager.
7. Adds HTTPS, CSRF protection, rate limits, audit logs, idempotency and server-side validation.

This prototype intentionally does not enable real-money trading or move money because those operations require authenticated Deriv credentials and payment permissions.

## Suggested production architecture

Browser (React)
  -> API server (Node/TypeScript)
     -> Deriv REST (OAuth/OTP/account/payment)
     -> Deriv WebSocket (market/trading)
  -> Database (users, sessions, trades, audit logs)

Recommended tables:
- users
- deriv_accounts
- sessions
- trades
- transactions
- audit_logs

## Important

The demo trade result is simulated. It is NOT a prediction of market outcomes and it does not place a Deriv order.
