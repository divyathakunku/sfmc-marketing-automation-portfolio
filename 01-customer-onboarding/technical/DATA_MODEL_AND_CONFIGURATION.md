# Data model and configuration reference

This document separates **verified configuration** from **recommended hardening** for the ACME Rewards training implementation.

## Data Extension schema

Data Extension name: `DE_Customer_onboarding`  
Intended sendability: **Sendable**, with `CustomerID` associated to Subscriber Key. The relationship and exact field lengths should be rechecked in the account before production use.

| Field | Recommended SFMC type | Null? | Purpose | Validation |
| --- | --- | --- | --- | --- |
| `CustomerID` | Text(50), Primary Key | No | Stable contact key | Unique; trimmed; non-empty |
| `EmailAddress` | EmailAddress | No | Destination | Valid format, owned for tests |
| `FirstName` | Text(100) | Yes | Greeting | Avoid markup; fallback if blank |
| `LastName` | Text(100) | Yes | Profile | Optional |
| `State` | Text(20) | Yes | Profile/optional regional content | Controlled allowed values |
| `SignupDate` | Date | No for new signups | Onboarding event date | Consistent timezone/format |
| `LoyaltyTier` | Text(20) | No for this journey | Routing | Only Gold/Silver/Platinum accepted |
| `MarketingOptIn` | Boolean | No | Entry eligibility attribute | Must reflect verifiable consent |

The types above are a **recommended schema contract**, not an asserted field-by-field export of the account.

## Data classification and retention

- **CustomerID and contact attributes:** personally associated data in a real system; treat as confidential.
- **EmailAddress:** personal data; never put real addresses in version control.
- **MarketingOptIn:** consent-related operational attribute, not a complete consent record.
- **Source-of-truth design:** production should retain consent timestamp, source, notice/version, legal basis, suppression and audit history in an approved system.
- **Retention:** define with privacy/legal team; no blanket duration is asserted here.

## Record examples

The repository includes [synthetic CSV](../sample-data/test-customers.csv) containing `@example.com` values. This domain is reserved for documentation and should **not be used for real email send tests**. Replace with controlled addresses inside your permitted SFMC account; do not commit those replacements.

## Entry source

| Property | Implementation / qualification |
| --- | --- |
| Type | Data Extension |
| Name | `DE_Customer_onboarding` |
| Version | ACME Rewards Journey Version 2 |
| Records in demonstration | 3 |
| Schedule shown in screenshot | November 12, 2026 9:30 AM AUS Eastern Standard Time, Repeat Never |
| Entry filter | Intended: `MarketingOptIn = True`; verify on entry configuration screen |
| Re-entry | Not proven by evidence; inspect before activation |

**Run Once** is convenient for a controlled test but won't automatically enter records added afterward. A production recurring setup requires a defined ingestion cadence and duplicate/re-entry handling.

## Segmentation rules

The Decision Split uses **Journey Data → LoyaltyTier** with these branches, in priority order:

| Path | Expression | Outcome |
| --- | --- | --- |
| Gold | `LoyaltyTier = "Gold"` | Gold email |
| Silver | `LoyaltyTier = "Silver"` | Silver email |
| Platinum | `LoyaltyTier = "Platinum"` | Platinum email |
| Remainder | Everything else | Exit without sending |

Treat case, leading/trailing whitespace and null handling as explicit upstream data-quality requirements rather than assuming SFMC normalizes them.

## Message assets and send configuration

| Activity | Message Definition checked in UI |
| --- | --- |
| `Email_01_Welcome_Gold` | `Welcome_Email_Gold` |
| `Email_02_Welcome_Silver` | `Welcome_Email_Silver - 20261104_111803` |
| `Email_03_Welcome_Platinum` | `Welcome_Email_Platinum - 20261104_111803 - 20261104_112351` |

- Personalisation: `%%FirstName%%` renders correctly for John, Sarah and David in individual previews.
- Marketing messaging: Tier-specific subject and offer text are included in separate message assets.
- **Observed Send Classification:** `Chino Send Classification`. Its sender identity, domain, sender/reply profiles, commercial/transactional settings and compliance details **have not been verified** for real sends.
- **Observed Publication List:** `All Subscribers`. Confirm subscription, unsubscribe and suppression handling with the account owner.
- **CTA:** visual labels were reviewed; destination URL and click tracking require separate checks.
- **Footer:** an unsubscribe link is visible; end-to-end unsubscribe functionality is not yet validated.

## Operational configuration checklist

- [ ] Confirm sendable field relationship and Subscriber Key resolution.
- [ ] Verify entry filter, consent provenance and suppression behavior.
- [ ] Approve sender profile, send classification, reply handling and branded domain.
- [ ] Test CTA/benefits destination and unsubscribe links.
- [ ] Verify email preview in major devices/clients and missing attribute fallback.
- [ ] Check schedule conversion between AEST and AEDT, plus re-entry rules.
- [ ] Run validation and negative-path tests for unsupported tiers and opt-out.
- [ ] Confirm contact version migration and rollout policy before activation.
- [ ] Capture live telemetry only after a permitted test send; don't infer it from simulation.

## Example personalisation (demonstrated vs proposed)

**Used in email content and previewed:** `%%FirstName%%`. No AMPscript production code is claimed.

**Possible later enhancement (not deployed):** a guarded, encoded fallback for empty first names. Add this as a separate tested milestone rather than representing it as part of current implementation.
