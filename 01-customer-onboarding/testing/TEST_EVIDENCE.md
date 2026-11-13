# ACME Rewards | Salesforce Journey Builder test evidence

**Evidence captured:** November 2026  
**Journey:** ACME Rewards Journey, Version 2  
**Entry source:** `DE_Customer_onboarding` (3 controlled test records)  
**Test mode:** Journey Builder simulation and Content Builder Subscriber Preview; **no actual delivery demonstrated**

## Evidence index

| Group | Evidence | Screenshot |
| --- | --- | --- |
| Audience | Selected 3 contacts | [10](../screenshots/10-journey-test-selected-contacts.png) |
| Rules | Gold / Silver / Platinum Decision Split | [15](../screenshots/15-decision-split-rules.png) |
| Canvas | Final Version 2 topology | [19](../screenshots/19-journey-canvas-version2.png) |
| Validation | One non-blocking High-Throughput Sending recommendation | [18](../screenshots/18-journey-validation-recommendation.png) |

## Positive path tests

| Test ID | Customer | Tier | Simulated route | Result | Evidence |
| --- | --- | --- | --- | --- | --- |
| RT-01 | C1001 / John | Gold | Gold email activity highlighted | Pass | [21](../screenshots/21-gold-routing-test.png) |
| RT-02 | C1002 / Sarah | Silver | Silver email activity highlighted | Pass | [22](../screenshots/22-silver-routing-test.png) |
| RT-03 | C1003 / David | Platinum | Platinum email activity highlighted | Pass | [23](../screenshots/23-platinum-routing-test.png) |

**Routing test assertion pass rate: 3 / 3 = 100%.** This rate only measures the three selected simulated test cases. It is not delivery, open, click, conversion or production reliability evidence.

Earlier simulation captures are preserved for traceability: [Gold](../screenshots/11-journey-test-gold-path.png), [Silver](../screenshots/12-journey-test-silver-path.png), [Platinum](../screenshots/13-journey-test-platinum-path.png).

## Message Definition mapping

The Activity Summary screens show these message assets attached to the intended branches:

| Branch | Activity | Message Definition | Subject | Screenshot |
| --- | --- | --- | --- | --- |
| Gold | `Email_01_Welcome_Gold` | `Welcome_Email_Gold` | Welcome to ACME Gold Rewards! | [24](../screenshots/24-gold-message-definition.png) |
| Silver | `Email_02_Welcome_Silver` | `Welcome_Email_Silver - 20261104_111803` | Welcome to ACME Silver Rewards! | [25](../screenshots/25-silver-message-definition.png) |
| Platinum | `Email_03_Welcome_Platinum` | `Welcome_Email_Platinum - 20261104_111803 - 20261104_112351` | Welcome to ACME Platinum Rewards! | [26](../screenshots/26-platinum-message-definition.png) |

## Subscriber preview tests

| Test contact | Intended email | Observed output | Result | Screenshot |
| --- | --- | --- | --- | --- |
| C1001 / John | Gold | 'Hi John' and Gold content | Pass | [14](../screenshots/14-gold-email-preview-final.png) |
| C1002 / Sarah | Silver | 'Hi Sarah' and Silver content | Pass | [16](../screenshots/16-silver-email-preview.png) |
| C1003 / David | Platinum | 'Hi David' and Platinum content; Platinum CTA label corrected | Pass | [20](../screenshots/20-platinum-email-preview.png) |

The earlier [Gold personalisation test](../screenshots/09-gold-personalisation-test.png) is also retained.

### Negative test / correction history

A preliminary [Platinum-customer-in-Silver-preview](../screenshots/17-negative-test-platinum-in-silver-email.png) capture showed **David** being used to preview a **Silver message**. This was a wrong template selection for the preview, not evidence of an incorrectly configured journey route. A later independent Platinum preview and Message Definition check demonstrated the intended Platinum email. This negative test is kept as a concrete example of why content and journey activities must be verified separately.

## Interpretation of validation

[Validation screenshot](../screenshots/18-journey-validation-recommendation.png): one recommendation for **High-Throughput Sending**; no blocking error is shown. That is not equivalent to completing operational sign-off or performing a send.

## Still not verified for production use

- Entry filter `MarketingOptIn = True` visibly enabled and tested with an opt-out record.
- Lawful consent provenance, suppression/subscription status, delivery/reply profiles and sender/domain approval.
- CTA URLs and unsubscribe functionality; the link labels alone do not prove functionality.
- Re-entry and version-transition behavior, schedule/timezone handling and a controlled send authorization.
- Missing first name, invalid/unknown tier, invalid email, duplicate ID and opt-out negative cases.
- Live email send/delivery/bounce/engagement telemetry; none is claimed.

See [TEST_PLAN.md](TEST_PLAN.md) for acceptance criteria and the release gate.

## Public evidence hygiene

The published screenshots have undergone redaction preparation. Before sharing outside GitHub, inspect them for personal contact addresses, account-specific sender/organisation information, internal identifiers, business unit links and token-like values.

## Journey activity progression — November 13, 2026

A subsequent Journey Builder screenshot shows activity-level contact counts following the scheduled entry evaluation:

| Activity | Count visible | Interpretation |
| --- | ---: | --- |
| Loyalty Decision Split | 3 | Three contacts reached split activity |
| Gold Email activity | 1 | One contact appears on Gold email path |
| Silver Email activity | 1 | One contact appears on Silver email path |
| Platinum Email activity | 1 | One contact appears on Platinum email path |
| Gold/Silver/Platinum exit activities | 1 each | Each path shows one contact exit |

**Evidence file pending upload:** `../screenshots/27-journey-execution-results.png`. The screenshot was prepared locally but is not yet verified in this GitHub repository.

These counters demonstrate **journey progression**, but do not alone prove emails were delivered. Check Salesforce Email Analytics (sent, delivered, bounced and not-sent) and the controlled inboxes before claiming successful delivery.
