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

## Additional evidence captured — November 5, 2026

The following images have been prepared with identifying email addresses and sender details redacted. Their filenames are planned paths; upload the PNGs before treating these links as published GitHub evidence.

| Planned screenshot | Observation | Test status |
| --- | --- | --- |
| `screenshots/15-decision-split-rules.png` | Rules display `LoyaltyTier equal Gold`, `Silver`, and `Platinum` | Configured (visual evidence) |
| `screenshots/16-silver-email-preview.png` | C1002 (Sarah, Silver) displays 'Hi Sarah' and Silver content | Preview passed |
| `screenshots/17-negative-test-platinum-in-silver-email.png` | C1003 (David, Platinum) displays 'Hi David' **but the Silver subject/content** | Expected negative test: wrong email template for Platinum, not a Platinum preview pass |
| `screenshots/18-journey-validation-recommendation.png` | Validation displays **1 recommendation** to enable High-Throughput Sending, and no blocking error visible | Validation review completed; optional recommendation pending |
| `screenshots/19-journey-canvas-version2.png` | Version 2 canvas shows all 3 named email branches and the Remainder exit | Structure captured |

### Remaining checks

- Preview the *Platinum* email asset with C1003 and verify it says 'Welcome to ACME Platinum Rewards' and 'Hi David'.
- Verify the Platinum activity is linked to the Platinum email definition, not Silver. The negative test alone does not prove the activity is misconfigured; it demonstrates that the wrong template was previewed for that customer.
- Confirm the final Gold/Silver/Platinum message definitions and CTA destinations.
- Do not publish actual email addresses or account-specific sender identities, and do not interpret Journey Builder Test Mode as proof of sending or delivery.
