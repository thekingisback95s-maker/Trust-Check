# TrustCheck: Think Before You Trust.

TrustCheck is an educational prototype that screens suspicious SMS messages, emails, job offers and investment offers for common scam warning signs and explains each one in plain language.

**Problem:** people, especially students and job seekers, often cannot tell whether a message is a scam before they pay or share a code. TrustCheck shows which patterns it sees and why they matter.

## Features
- Local, rule-based analysis in the browser. No backend, no API, no data leaves your device.
- Detects: requests for passwords/OTPs, upfront job or prize fees, threats of legal action tied to payment, urgency, guaranteed returns, sensitive-data requests, unusual payment methods, impersonation, moves to private chat apps, shortened or deceptive URLs, generic greetings (only alongside other signs).
- Results show the score, detected signs with quoted evidence, signs not detected, things TrustCheck cannot verify, and next steps.
- Copyable plain-text report (the message text is not included), fictional sample messages, Clear/reset, optional local history (summary only), Safety Center with checklist and official reporting links.

## How scoring works
Each rule has base points (code/password request 50, upfront fee 50, guaranteed returns 40, threat 35, unusual payment 30, sensitive data 30, urgency 20, shortened link 20, impersonation 12, private chat 8, generic greeting 4). Rules describing the same behaviour are grouped into a family (payment, secrets, pressure, reward, link, identity, channel). The strongest rule in a family counts fully, the others add 30%. Families are summed and capped at 100. No signs means 0. The score is a **heuristic estimate, not the probability of fraud**.

| Score | Risk level |
|---|---|
| 0–19 | Low |
| 20–44 | Moderate |
| 45–69 | High |
| 70–100 | Very High |

Low risk does not prove a message is genuine. High risk does not prove fraud. Thresholds live in `src/utils/scoring.js` and are used by the UI and tests.

## Commands
```
npm install      # install dependencies
npm run dev      # start dev server (usually http://localhost:5173)
npm test         # run automated tests (Node built-in test runner)
npm run build    # production build into dist/
npm run preview  # serve the production build
```

## Example
Input: "Congratulations! You are selected for the Remote Data Entry role. To secure your position, pay a ₦15,000 registration fee today. Reply on WhatsApp. Act now."
TrustCheck flags the upfront fee, urgency and private-chat move, quotes each sentence, scores it Very High, and advises not to pay and to verify the employer independently.

## Limitations
- Simple pattern matching: it can miss scams and flag legitimate messages.
- No live threat intelligence, sender or domain verification, URL reputation, bank-account checks or AI model.
- English-centric patterns. Tests cover the engine and source checks; there are no browser UI tests.
- No security audit or certification. Never paste real passwords, PINs or codes.
