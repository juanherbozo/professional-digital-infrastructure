# Guide 06 — SPF, DKIM and DMARC

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/06-spf-dkim-dmarc.md)

## 1. Objective

Understand and validate email authentication for messages sent from a professional domain.

This guide covers SPF, DKIM and DMARC. All configuration examples use illustrative domains and must be adapted before production use.

## 2. Three distinct mechanisms

| Mechanism | Purpose |
|---|---|
| SPF | Authorizes infrastructure to send using an SMTP envelope domain |
| DKIM | Verifies a cryptographic signature associated with a domain |
| DMARC | Checks SPF or DKIM alignment with the visible From domain and publishes a policy |

These mechanisms help defend against spoofing but do not guarantee inbox placement.

## 3. Message identities

An email can contain several domain identities:

- RFC 5322 Header From: the sender address visible to users.
- Envelope sender / MAIL FROM: SMTP identity normally evaluated by SPF.
- DKIM d=: the signing domain.

These domains can differ.

An SPF PASS result alone does not prove that the visible sender domain passes DMARC through SPF.

## 4. SPF

SPF uses a DNS TXT policy to identify infrastructure authorized to send using the evaluated domain.

Illustrative example:

```dns
example.com TXT "v=spf1 include:_spf.example-provider.net ~all"
```

The included domain is fictional.

Basic practices:

1. Follow the SMTP provider's instructions.
2. Identify the actual envelope sender domain.
3. Avoid multiple independent SPF records at the same DNS name.
4. Observe the SPF DNS lookup limit.
5. Do not authorize unused sending services.

The root domain's SPF record may not determine the SPF outcome of an external SMTP provider using its own return-path domain.

## 5. DKIM

DKIM uses a private key to sign email and a DNS-published public key to verify that signature.

The signing domain appears in `d=` and the selector in `s=`.

Example:

```text
DKIM-Signature: ... d=example.com; s=mail2026; ...
```

Depending on the provider, DNS uses TXT records or delegated CNAME records.

General procedure:

1. Verify the sending domain.
2. Obtain the provider's DKIM records.
3. Publish the exact supplied records.
4. Validate DNS resolution.
5. Send a test message.
6. Confirm DKIM PASS and the signing domain.

Never publish private DKIM keys in repositories.

## 6. DMARC

DMARC compares the visible From domain with authenticated SPF and DKIM identifiers.

DMARC PASS requires at least one of:

- SPF PASS with an aligned SPF domain.
- DKIM PASS with an aligned DKIM domain.

Both are not required.

### Policies

| Policy | Purpose |
|---|---|
| p=none | Monitor without requesting quarantine or rejection |
| p=quarantine | Request suspicious treatment of failing mail |
| p=reject | Request rejection of failing mail |

Illustrative monitoring record:

```dns
_dmarc.example.com TXT "v=DMARC1; p=none"
```

Do not deploy restrictive policies without reviewing every legitimate sending service.

Receivers retain discretion over final message disposition.

## 7. Relaxed and strict alignment

DMARC supports two alignment modes:

- Relaxed: organizational domain comparison.
- Strict: exact DNS domain matching.

Relaxed alignment is the default unless changed with `aspf` or `adkim`.

A valid DKIM signature from an unrelated domain may pass DKIM verification but not satisfy DMARC alignment.

## 8. JH-Web reference test

A real message sent through Thunderbird and Brevo SMTP and received by Outlook showed:

| Check | Result | Interpretation |
|---|---|---|
| SPF | PASS | Provider envelope sender domain authenticated |
| DKIM | PASS | Professional domain signature authenticated |
| DMARC | PASS | DKIM aligned with the visible From domain |

The accurate conclusion is that DMARC passed through DKIM alignment.

SPF PASS must not be described as aligned with the professional domain without verifying the underlying identities.

This was a specific test, not a future deliverability guarantee.

## 9. DNS checks

On Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type TXT
Resolve-DnsName _dmarc.example.com -Type TXT
Resolve-DnsName mail2026._domainkey.example.com -Type TXT
```

Replace the domain and selector with actual values.

Also query CNAME when the provider uses delegated DKIM records.

Correct DNS records do not independently prove that messages are being signed or sent correctly.

## 10. End-to-end verification

1. Send from the professional identity.
2. Receive the message at an external provider.
3. Open the complete message headers.
4. Locate `Authentication-Results`.
5. Record SPF, DKIM and DMARC results.
6. Identify the domains involved.
7. Check alignment.
8. Record the test date and results without exposing sensitive information.

Review bounces and delivery notifications as appropriate.

## 11. Common issues

| Symptom | Possible cause |
|---|---|
| SPF FAIL | Unauthorized sender IP or incorrect SPF identity |
| SPF PASS and DMARC FAIL | SPF not aligned and no valid aligned DKIM |
| DKIM FAIL | Message changes, incorrect configuration or DNS key |
| DKIM PASS and DMARC FAIL | Valid but unaligned signing domain |
| Missing DMARC | Absent or malformed policy record |
| Spam despite DMARC PASS | Reputation, content or other filters |

Forwarding can affect SPF, and some message modifications can invalidate DKIM.

## 12. Security and operations

- Inventory all legitimate sending services.
- Authorize only necessary infrastructure.
- Monitor DMARC aggregate reports when enabled.
- Increase policy enforcement gradually.
- Recheck DKIM after migrations.
- Protect SMTP credentials.
- Document changes and test results.

## 13. Expected outcome

Ability to distinguish authentication results, understand identifier alignment and reproducibly validate outgoing messages from a professional domain.

## 14. References

- [RFC 7489 — DMARC](https://www.rfc-editor.org/rfc/rfc7489)
- [RFC 7208 — SPF](https://www.rfc-editor.org/rfc/rfc7208)
- [RFC 6376 — DKIM](https://www.rfc-editor.org/rfc/rfc6376)
- [Cloudflare — Email authentication](https://developers.cloudflare.com/email-service/concepts/email-authentication/)
- [JH-Web reference implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)