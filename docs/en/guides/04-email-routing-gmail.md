# Guide 04 — Cloudflare Email Routing and Gmail

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/04-email-routing-gmail.md)

## 1. Objective

Receive messages addressed to a custom domain by using Cloudflare Email Routing and forwarding them to an existing Gmail account.

Reference architecture:

External sender → Cloudflare Email Routing → Gmail.

This provides email forwarding, not a dedicated business mailbox or an outgoing SMTP service.

## 2. Requirements

- A domain using Cloudflare DNS.
- Administrative access to Cloudflare.
- An active Gmail account as the destination.
- Knowledge of existing MX records.
- Multifactor authentication where available.

Before proceeding, determine whether the domain already receives mail through another provider. Changing MX records may interrupt that service.

## 3. Initial assessment

Identify:

- Required professional addresses.
- Who will manage incoming messages.
- Which mailbox stores the messages.
- Privacy and retention obligations.
- A fallback if forwarding becomes unavailable.

Example addresses:

- Public address: `contact@example.com`.
- Destination: `recipient@gmail.com`.

These are illustrative placeholders.

## 4. Enable Email Routing

In Cloudflare:

1. Select the appropriate account.
2. Open Compute > Email Service > Email Routing.
3. Select the domain.
4. Start domain onboarding if needed.
5. Review the DNS records Cloudflare proposes to manage.
6. Complete activation.

Navigation labels may change as Cloudflare updates its dashboard.

Do not replace an existing provider's MX records without planning a migration.

## 5. Verify the destination

1. Open Destination Addresses.
2. Add `recipient@gmail.com`.
3. Open Gmail.
4. Find the Cloudflare verification email.
5. Confirm the destination address.
6. Verify its status in Cloudflare.

Rules depending on an unverified destination cannot forward normally.

## 6. Create the routing rule

Under Routing Rules:

1. Choose Create routing rule.
2. Enter the local part `contact`.
3. Select `example.com`.
4. Choose the send-to-email action.
5. Select `recipient@gmail.com`.
6. Save the rule.
7. Confirm it is active.

The resulting mapping is:

`contact@example.com` → `recipient@gmail.com`.

Catch-all is not required for individually configured addresses.

## 7. DNS records

Cloudflare Email Routing uses MX records to receive domain email.

Example service targets:

- `route1.mx.cloudflare.net`
- `route2.mx.cloudflare.net`
- `route3.mx.cloudflare.net`

Always obtain exact values and priorities from the provider's current configuration rather than copying another domain.

Cloudflare may also require SPF and DKIM TXT records.

Do not create multiple independent SPF records for the same hostname. Review proper consolidation when multiple services must be authorized.

Outgoing email and detailed domain authentication are covered in Guides 05 and 06.

## 8. DNS verification

On Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Type TXT
```

Replace `example.com` with the appropriate domain.

Confirm MX records match the selected incoming mail service.

Correct DNS responses alone do not guarantee successful email delivery.

## 9. First reception test

From a separate external email account:

1. Send a message to `contact@example.com`.
2. Use a unique subject.
3. Confirm arrival at `recipient@gmail.com`.
4. Inspect sender, original recipient, date and content.
5. Check spam folders if necessary.
6. Record the test result.

When possible, repeat with another sending provider.

The JH-Web reference implementation included external sending and successful Gmail reception tests.

## 10. Troubleshooting

| Symptom | Check |
|---|---|
| No incoming message | Inspect MX, rule and destination |
| Pending destination | Complete verification |
| Disabled rule | Check Routing Rules status |
| Rejected recipient | Check exact address matching |
| Message marked as spam | Inspect destination mailbox filters |
| Previous mail service disrupted | Review MX migration |
| Forwarding failure | Inspect service activity and errors |

During migration, external systems may use cached DNS information. Avoid declaring success based on only one test.

## 11. Catch-all

Catch-all rules can process messages addressed to otherwise unconfigured local parts.

They may be useful for some workflows but can also attract unwanted or mistakenly addressed mail.

The JH-Web reference implementation kept catch-all disabled.

## 12. Limitations

- Cloudflare Email Routing is not a dedicated IMAP mailbox.
- It does not independently support sending from the professional address.
- Gmail stores forwarded mail.
- Sender authentication and spam policies can affect forwarding.
- It does not automatically provide enterprise mail administration.
- Provider terms, service limits and data protection obligations must be reviewed.

For critical operations, sensitive information or contractual support requirements, consider a fully managed business email service.

## 13. Expected outcome

A professional address receives external messages and forwards them into a verified Gmail account.

Outgoing messages from that address are not configured yet.

Guide 05 will cover Thunderbird and SMTP sending options, including provider usage restrictions.

## 14. Official references

- [Cloudflare — Route emails](https://developers.cloudflare.com/email-service/get-started/route-emails/)
- [Cloudflare — Routing rules and addresses](https://developers.cloudflare.com/email-service/configuration/email-routing-addresses/)
- [Cloudflare — Domain configuration](https://developers.cloudflare.com/email-service/configuration/domains/)
- [Cloudflare — Email troubleshooting](https://developers.cloudflare.com/email-service/reference/troubleshooting/)
- [JH-Web implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)