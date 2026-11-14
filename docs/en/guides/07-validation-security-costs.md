# Guide 07 — Validation, Security, Costs and Limitations

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/07-validation-security-costs.md)

## 1. Objective

Evaluate a professional digital infrastructure before considering it ready for use.

This guide complements the implementation instructions. It is not a security audit, compliance certification or availability guarantee.

## 2. Testing scope

The assessment covers:

- Website publishing and functionality.
- DNS resolution and HTTPS.
- Incoming and outgoing email.
- SPF, DKIM and DMARC authentication.
- Credentials and administrative access.
- Operational dependencies and costs.
- Recovery from errors.

Record results with dates, environments and verifiable evidence without disclosing private information.

## 3. Validation matrix

| ID | Check | Expected result |
|---|---|---|
| WEB-01 | Local build | Completes without errors |
| WEB-02 | Deployment | Intended version is published |
| WEB-03 | Navigation | Main routes accessible |
| WEB-04 | Languages | Correct content and links |
| DNS-01 | Domain | Consistent resolution |
| DNS-02 | HTTPS | Valid certificate |
| MAIL-01 | Reception | External message received |
| MAIL-02 | Sending | Test message accepted and received |
| MAIL-03 | Reply | Reaches intended mailbox |
| AUTH-01 | SPF | Result and identity documented |
| AUTH-02 | DKIM | Signature and domain verified |
| AUTH-03 | DMARC | Result and alignment verified |

Never mark a test as passed without executing it.

## 4. Website tests

Check:

1. Local build.
2. Deployment and published commit.
3. HTTPS access.
4. Home page and internal routes.
5. Images and assets.
6. Mobile navigation.
7. Metadata and languages.
8. No exposed confidential information.

A successful build does not prove every production page works correctly.

## 5. DNS tests

On Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type NS
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Type TXT
Resolve-DnsName _dmarc.example.com -Type TXT
```

Replace the domain and record the expected values. Confirm that changes have not disrupted other services.

## 6. Email tests

Perform at least:

1. Send from an external provider to the professional address.
2. Confirm forwarding to the destination mailbox.
3. Send using the professional identity and an authorized SMTP provider.
4. Confirm receipt at a different provider.
5. Reply and verify the destination.
6. Inspect errors, bounces and spam folders.
7. Inspect authentication headers.

SMTP acceptance does not guarantee inbox placement.

## 7. JH-Web evidence

The reference implementation was tested for:

- Bilingual static page publishing.
- Deployment through GitHub and Cloudflare Pages.
- Incoming forwarding through Cloudflare Email Routing to Gmail.
- Outgoing delivery through Thunderbird and Brevo SMTP.
- Receipt of a test message in Outlook.
- Replies reaching the professional address.
- SPF PASS for the provider return-path domain.
- DKIM PASS using the professional domain.
- DMARC PASS through DKIM alignment.

These are observations from specific tests, not service-level commitments or exhaustive verification.

The suitability of Brevo SMTP for individual professional correspondence remains subject to confirmation under its applicable terms.

## 8. Security and privacy

Review:

- MFA for GitHub, Cloudflare and email accounts.
- Unique credentials and a password manager.
- Tokens and keys kept outside repositories.
- Least-privilege integration permissions.
- Key rotation and revocation.
- Thunderbird and dependency updates.
- Local device security.
- Privacy and data processing obligations.
- SMTP provider open and link tracking features.

SPF, DKIM and DMARC do not replace account security or proper handling of sensitive information.

## 9. Total cost

Separate monetary expenses from administration effort.

| Component | Assessment |
|---|---|
| Domain | Registration and renewal |
| Hosting | Free-tier limits and terms |
| DNS and forwarding | Limits and provider dependency |
| SMTP | Permitted use, quotas and paid options |
| Maintenance | Support and update effort |
| Continuity | Backups, recovery and migration |
| Scaling | Future services and capabilities |

Free tiers can reduce recurring expenditure but do not eliminate total cost of ownership.

Avoid universal monthly price claims because pricing and limits change.

## 10. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Compromised account | MFA and access review |
| Exposed SMTP key | Revoke and replace |
| Incorrect DNS | Inventory and rollback procedure |
| Faulty deployment | Validate and restore a known version |
| Forwarding outage | Monitoring and alternate contact channel |
| Terms change | Periodic provider review |
| Multiple provider dependencies | Document responsibilities and recovery |

Keep recoverable content copies and understand how to restore important configurations.

## 11. When to consider this architecture

It may suit:

- Static informational websites.
- Professional portfolios.
- Small projects with available technical maintenance.
- Scenarios that accept email and hosting limitations.

Consider a complete enterprise solution when requirements include:

- Shared mailboxes or centralized administration.
- Contractual availability guarantees.
- Advanced retention, auditing or specific compliance.
- Support with committed response times.
- Critical processes or particularly sensitive information.

Select technology based on requirements, not only initial savings.

## 12. Acceptance checklist

- [ ] Website deployed and validated.
- [ ] Domain and certificate checked.
- [ ] DNS records documented.
- [ ] Incoming email tested.
- [ ] Outgoing email tested and authorized.
- [ ] Replies received correctly.
- [ ] SPF, DKIM and DMARC reviewed.
- [ ] MFA and permissions evaluated.
- [ ] Credentials protected.
- [ ] Costs and limits documented.
- [ ] Recovery procedure defined.
- [ ] Risks accepted by the responsible owner.

## 13. Conclusion

A simple infrastructure can meet professional needs when requirements are well defined and limitations are understood.

Solution quality depends less on the number of purchased services than on fitness for purpose, security, maintainability and evidence of correct operation.

## 14. References

- [Cloudflare Pages](https://developers.cloudflare.com/pages/)
- [Cloudflare Email Routing](https://developers.cloudflare.com/email-service/)
- [Astro Documentation](https://docs.astro.build/)
- [RFC 7489 — DMARC](https://www.rfc-editor.org/rfc/rfc7489)
- [JH-Web implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)