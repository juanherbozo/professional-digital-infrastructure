# Guide 05 — Thunderbird and SMTP Configuration

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/05-thunderbird-smtp.md)

## 1. Objective

Configure a professional sending identity in Thunderbird using an external SMTP server.

Reference architecture:

Thunderbird → SMTP provider → External recipient.

This guide describes email client operations. It does not turn Cloudflare Email Routing into an SMTP service or automatically provide a business mailbox.

## 2. Requirements

- An installed, updated Thunderbird client.
- An existing email account configured in Thunderbird.
- A professional address under an administered domain.
- An SMTP provider that permits the intended message type.
- SMTP credentials issued by that provider.
- Domain authentication where required.

Adding a sending identity does not require changing incoming IMAP server settings.

## 3. Separate identity, mailbox and transport

An identity defines the display name and address in the From header.

A mailbox stores incoming messages.

An SMTP server transports outgoing messages.

These are separate functions. A professional address may receive forwarded mail and use another service for sending, provided the applicable provider terms permit it.

## 4. Choose an SMTP provider

Before configuring Thunderbird, verify:

1. The provider permits the intended message category.
2. The sender address or domain is authorized.
3. SMTP authentication is supported.
4. Encrypted connections are available.
5. Usage limits and policies are suitable.
6. Recovery and support provisions meet operational needs.

For general business correspondence, consider a professional mailbox provider with included SMTP service.

## 5. Add an SMTP server

In Thunderbird:

1. Open Account Settings.
2. Locate Outgoing Server (SMTP).
3. Select Add.
4. Enter the provider's hostname and port.
5. Select the required connection security.
6. Configure the authentication method and username.
7. Save.

Do not invent server addresses or reuse credentials belonging to another provider.

## 6. Technical example: Brevo

Brevo documents these settings for its transactional SMTP relay:

| Setting | Reference value |
|---|---|
| Server | `smtp-relay.brevo.com` |
| Port | `587` |
| Connection security | STARTTLS |
| Authentication | Normal password over an encrypted connection |
| Username | Brevo-assigned SMTP login |
| Password | SMTP key, not API key |

Always consult the current settings in the provider account.

The SMTP login may differ from the visible sender address.

**Important limitation:** this service is documented for transactional messages. General individual professional correspondence should not be assumed to be an authorized use without applicable confirmation from Brevo.

The JH-Web implementation demonstrates technical connectivity, not universal contractual permission.

## 7. Add an additional identity

In Thunderbird:

1. Open the receiving account's settings.
2. Select Manage Identities.
3. Select Add.
4. Enter the visible display name.
5. Enter the authorized professional address.
6. Select the appropriate outgoing SMTP server.
7. Review signature and reply address.
8. Save.

Illustrative example:

- Name: Professional Contact.
- Address: `contact@example.com`.
- SMTP server: the previously configured authorized provider.

Do not replace the personal identity's SMTP server if the identities use different providers.

## 8. Credential security

- Create dedicated keys where supported.
- Do not use API keys as SMTP passwords.
- Keep secrets out of GitHub, public documents and screenshots.
- Use encrypted connections.
- Revoke compromised keys.
- Monitor expiration and rotation.
- Protect the local Thunderbird profile.

Never publish screenshots displaying secret values or tokens.

## 9. First sending test

1. Create a new message.
2. Explicitly choose the professional identity.
3. Use an external recipient account under your control.
4. Send a clearly identifiable test message.
5. Confirm reception.
6. Inspect the From address.
7. Inspect Reply-To if configured.
8. Reply from the recipient account.
9. Confirm the reply reaches the intended mailbox.

Avoid personal or confidential information in test messages.

## 10. Authentication and headers

Successful SMTP login does not necessarily mean the sender domain passes SPF, DKIM or DMARC.

Inspect the received message:

- SPF checks the envelope sender domain.
- DKIM verifies a cryptographic signature.
- DMARC requires alignment of the visible From domain with either SPF or DKIM.

In one JH-Web test, Outlook reported SPF PASS for the Brevo envelope domain, DKIM PASS for the professional domain and DMARC PASS through DKIM alignment.

These results are limited to that test and do not guarantee future delivery.

Guide 06 will examine these mechanisms in detail.

## 11. Common issues

| Symptom | Check |
|---|---|
| Authentication failure | SMTP login, key and account status |
| TLS error | Port and required STARTTLS/SSL mode |
| Sender rejected | Verified sender address or domain |
| Incorrect identity | SMTP server assigned to that identity |
| Message not delivered | Bounces, limits, policies and provider logs |
| Replies reach another mailbox | From and Reply-To |
| Spam classification | Authentication, reputation and content |
| Missing sent messages | Copies and folders settings |

SMTP acceptance does not necessarily mean inbox delivery.

## 12. Privacy and tracking

Some transactional email providers offer open, link or event tracking.

Review these features before sending professional correspondence, especially where privacy obligations apply.

Disable tracking when it is not legitimately needed and review the provider's terms.

## 13. Limitations

Combining independent services may require additional administration.

This model does not automatically include:

- Shared business mailboxes.
- Centralized retention.
- Contractual availability guarantees.
- Enterprise archiving or e-discovery.
- Integrated business support.

When those capabilities are required, consider a complete business email platform.

## 14. Expected outcome

Thunderbird can select a professional sender identity and send messages using an authorized SMTP service while keeping incoming and outgoing configurations separate.

## 15. References

- [Mozilla Thunderbird — Manual account configuration](https://support.mozilla.org/en-US/kb/manual-account-configuration)
- [Mozilla Thunderbird — Identity settings](https://support.mozilla.org/en-US/kb/configuration-options-identity)
- [Brevo — Transactional SMTP](https://help.brevo.com/hc/en-us/articles/7924908994450-Send-transactional-emails-using-Brevo-SMTP)
- [Brevo — SMTP keys](https://help.brevo.com/hc/en-us/articles/7959631848850-Create-and-manage-your-SMTP-keys)
- [JH-Web implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)