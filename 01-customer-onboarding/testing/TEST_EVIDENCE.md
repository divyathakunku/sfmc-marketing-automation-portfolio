# ACME Rewards Journey — Test evidence

**Platform:** Salesforce Marketing Cloud Engagement  
**Journey:** ACME Rewards Journey, Version 2 (draft during simulation)  
**Entry source:** `DE_Customer_onboarding`  
**Sample size:** 3 synthetic customer profiles using controlled test inboxes  
**Scope:** Journey Builder Test Mode and Content Builder subscriber preview; **not** production delivery evidence.

## Test scenarios

| Customer ID | Loyalty tier | Intended branch | Screenshot |
| --- | --- | --- | --- |
| C1001 | Gold | Gold welcome email | `screenshots/11-journey-test-gold-path.png` |
| C1002 | Silver | Silver welcome email | `screenshots/12-journey-test-silver-path.png` |
| C1003 | Platinum | Platinum welcome email | `screenshots/13-journey-test-platinum-path.png` |

The screenshots document the three selected test contacts and their journey paths. The Silver and Platinum screenshots visibly highlight their respective branches. Confirm the Gold path and the individual message definitions before asserting that all three paths passed.

## Screenshot inventory

- `10-journey-test-selected-contacts.png` — three contacts selected for a simulated journey test.
- `11-journey-test-gold-path.png` — first test contact, C1001 (Gold).
- `12-journey-test-silver-path.png` — second test contact, C1002 (Silver), with highlighted Silver route.
- `13-journey-test-platinum-path.png` — third test contact, C1003 (Platinum), with highlighted Platinum route.
- `14-gold-email-preview-final.png` — personalised Gold email preview for C1001, showing the rendered greeting.

Screenshots belong under `01-customer-onboarding/screenshots/`. Ensure all exported images are redacted before making them public.

## Further validation required

1. Confirm each Decision Split branch condition and that its Message Definition points to the corresponding Gold, Silver, or Platinum Content Builder email.
2. Capture personalised subscriber previews for the Silver and Platinum emails.
3. Save a successful full-journey validation result and confirm the Gold route highlight.
4. Test the CTA links, sender classification, footer, and unsubscribe behaviour before live sending.
5. If sending is deliberately enabled, record delivery analytics separately. Simulation does **not** send email or demonstrate a delivery/conversion rate.
