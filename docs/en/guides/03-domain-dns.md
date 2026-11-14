# Guide 03 — Domain and DNS Configuration

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/03-domain-dns.md)

## 1. Objective

Connect a custom domain to a Cloudflare Pages website and understand the necessary DNS records without disrupting existing services.

This guide uses example names. It does not provide universal production DNS values.

## 2. Registrar and DNS provider

A domain registrar manages domain registration and renewal.

The authoritative DNS provider manages the DNS records used by services associated with that domain.

These functions may be handled by different companies.

## 3. Essential DNS records

| Record | Purpose |
|---|---|
| A | Maps a hostname to an IPv4 address |
| AAAA | Maps a hostname to an IPv6 address |
| CNAME | Creates an alias to another DNS hostname |
| MX | Identifies incoming mail servers |
| TXT | Publishes verification data and policies such as SPF or DMARC |
| CAA | Restricts certificate issuance authorities |
| NS | Identifies authoritative DNS servers or delegations |

Depending on the provider, DKIM can use TXT or CNAME records.

## 4. Initial inventory

Before changing DNS:

1. Identify the registrar.
2. Confirm current nameservers.
3. Export or document existing DNS records.
4. Identify active web and email services.
5. Review DNSSEC if enabled.
6. Prepare a recovery procedure.

Changing nameservers requires particular care because several services may be affected simultaneously.

## 5. Connect Cloudflare Pages

Start with an already deployed Pages project.

In Cloudflare:

1. Open Workers & Pages.
2. Select the project.
3. Open Custom domains.
4. Choose Set up a domain.
5. Enter the intended domain.
6. Complete the verification steps.

For an apex domain, the corresponding zone must be configured in the appropriate Cloudflare account.

Some subdomains can use external DNS with a CNAME record, but they must still be associated with the Pages project through its Custom domains settings.

Creating a DNS record alone is not sufficient.

## 6. DNS records and proxy status

A, AAAA and CNAME records used for web traffic may use Cloudflare's HTTP proxy.

MX and TXT records are not proxied.

Hostnames used exclusively for SMTP, IMAP or POP3 should resolve without Cloudflare's standard HTTP proxy.

Third-party verification CNAME records should generally use DNS only, following the provider's instructions.

## 7. Apex domains and CNAME flattening

Cloudflare supports CNAME flattening at the zone apex.

This allows root domains to work with CNAME-based services where a conventional apex CNAME would normally be unsuitable.

Do not invent or hardcode IP addresses to connect a Cloudflare Pages project.

## 8. Email coexistence

Web and email services can share a domain while operating independently.

Before changing MX records:

- Identify the existing incoming mail provider.
- Check whether Cloudflare Email Routing is enabled.
- Review destination hosts and priorities.
- Avoid combining incompatible mail configurations.

SPF, DKIM and DMARC will be addressed in a dedicated guide.

## 9. DNS validation

In Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type NS
Resolve-DnsName example.com -Type A
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Type TXT
```

Replace `example.com` with the intended domain.

Successful DNS responses do not independently prove that the website or email services are functioning correctly.

## 10. Website validation

After associating the domain:

- Check Custom domains status in Pages.
- Confirm HTTPS access.
- Test primary website routes.
- Inspect the TLS certificate.
- Validate apex and `www` redirects if configured.
- Check that existing email services still work.

## 11. Common issues

| Issue | Check |
|---|---|
| Inactive domain | Review nameservers and Pages status |
| Domain association error | Verify Custom domains configuration |
| Pending certificate | Review validation, DNS and CAA restrictions |
| Email disruption | Inspect MX and related mail hostnames |
| DKIM verification failure | Review CNAME, proxy and flattening |
| Inconsistent DNS results | Consider TTL and DNS caches |

## 12. Security and continuity

Do not modify production DNS records without understanding their dependencies.

Limit administrative access, enable MFA, maintain configuration inventories and prepare recoverable DNS changes.

## 13. References

- [Cloudflare Pages custom domains](https://developers.cloudflare.com/pages/configuration/custom-domains/)
- [Cloudflare DNS records](https://developers.cloudflare.com/dns/manage-dns-records/)
- [Cloudflare CNAME flattening](https://developers.cloudflare.com/dns/cname-flattening/)
- [Cloudflare email troubleshooting](https://developers.cloudflare.com/dns/troubleshooting/email-issues/)
- [JH-Web reference implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)