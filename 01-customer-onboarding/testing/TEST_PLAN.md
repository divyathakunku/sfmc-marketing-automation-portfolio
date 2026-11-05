# Test plan and release readiness

## Test strategy

This is a controlled validation of a Salesforce Marketing Cloud Journey Builder loyalty onboarding experience. The current evidence includes UI configuration, preview rendering and Test Mode path simulation; it **does not** prove a send, delivery, open, click or conversion.

### Acceptance criteria

1. Contacts with `LoyaltyTier` Gold, Silver and Platinum route to their corresponding Email activities.
2. Each branch's Message Definition references its own welcome email.
3. Preview for each test customer resolves the intended first name and tier-specific content.
4. Unexpected tier and ineligible-consent cases are handled safely **before live use**.
5. No activation occurs until link, sender, subscription, data and timezone checks are complete.

## Executed tests

| ID | Test case | Contact | Observed result | Evidence |
| --- | --- | --- | --- | --- |
| RT-01 | Gold routing | C1001 / John | PASS; Gold highlighted | [21](../screenshots/21-gold-routing-test.png) |
| RT-02 | Silver routing | C1002 / Sarah | PASS; Silver highlighted | [22](../screenshots/22-silver-routing-test.png) |
| RT-03 | Platinum routing | C1003 / David | PASS; Platinum highlighted | [23](../screenshots/23-platinum-routing-test.png) |
| MAP-01 | Gold Message Definition | Gold activity | PASS; Gold message attached | [24](../screenshots/24-gold-message-definition.png) |
| MAP-02 | Silver Message Definition | Silver activity | PASS; Silver message attached | [25](../screenshots/25-silver-message-definition.png) |
| MAP-03 | Platinum Message Definition | Platinum activity | PASS; Platinum message attached | [26](../screenshots/26-platinum-message-definition.png) |
| PRE-01 | Gold preview | C1001 | PASS; Hi John, Gold message | [14](../screenshots/14-gold-email-preview-final.png) |
| PRE-02 | Silver preview | C1002 | PASS; Hi Sarah, Silver message | [16](../screenshots/16-silver-email-preview.png) |
| PRE-03 | Platinum preview | C1003 | PASS; Hi David, Platinum message | [20](../screenshots/20-platinum-email-preview.png) |
| VAL-01 | Journey Builder validation | Version 2 | No blocking errors visible; 1 High-Throughput Sending recommendation | [18](../screenshots/18-journey-validation-recommendation.png) |

**Positive routing test pass rate:** 3 of 3 selected test contacts = 100% for this synthetic sample.

## Negative / boundary tests to execute

| ID | Scenario | Expected result |
| --- | --- | --- |
| NEG-01 | `MarketingOptIn = False` | Excluded from entry where filter is configured |
| NEG-02 | Unknown tier, e.g. Bronze | Remainder branch and no welcome email |
| NEG-03 | Null or blank LoyaltyTier | Remainder branch and exception logging in production design |
| NEG-04 | Duplicate CustomerID | Upstream rejection or defined upsert, no accidental duplicate welcome |
| NEG-05 | Missing FirstName | Approved greeting fallback, no literal token |
| NEG-06 | Invalid email address | Excluded/quarantined upstream |
| NEG-07 | Email unsubscribed/suppressed | No unauthorized message sent |
| NEG-08 | Re-entered contact / Version migration | No unwanted duplicate welcome |
| NEG-09 | Wrong email template chosen | Correct asset mapping caught by review; historic example retained as [17](../screenshots/17-negative-test-platinum-in-silver-email.png) |

## Production activation gate (not yet passed)

- [ ] Owner has reviewed actual consent provenance and suppression/subscription rules.
- [ ] All CTA URLs, tracking parameters and unsubscribe links tested with approved inboxes.
- [ ] Send Classification, sender/reply profiles and footer details approved.
- [ ] Entry source email key mapping, audience freshness and re-entry verified.
- [ ] Expected schedule and timezone approved.
- [ ] Negative and edge cases executed with evidence.
- [ ] Cross-device accessibility/content QA completed.
- [ ] Appropriate monitoring, ownership, incident response and stop/pause procedure defined.
- [ ] Only then authorize a controlled real send and separately evaluate delivery metrics.

## Evidence discipline

Screenshots demonstrate exact UI state at capture time. Do not report a 100% delivery rate, revenue uplift, production throughput, or successful activation from Test Mode results. All real email addresses and account-specific identifiers must be redacted from public artifacts.
