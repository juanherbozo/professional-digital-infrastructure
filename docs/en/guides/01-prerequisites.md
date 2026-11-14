# Guide 01 — Preparation and Prerequisites

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/01-prerequisites.md)

## 1. Objective

This guide covers the requirements and decisions involved in preparing a professional digital presence using static web technologies and independent cloud services.

JH-Web — Personal Digital Platform is the reference implementation.

Not every service needs to be purchased or configured before development begins. Preparation should reflect the project's actual requirements.

## 2. Define the scope

Before selecting technologies, identify:

- Target audience.
- Required content and functionality.
- Publication languages.
- Need for forms or dynamic services.
- Professional email requirements.
- Available budget.
- Ability to administer the solution.

An informational website does not necessarily have the same requirements as an online store or enterprise application.

## 3. Development tools

The reference implementation uses:

| Tool | Purpose |
|---|---|
| Code editor | Modify project files |
| Node.js and npm | Manage dependencies and run Astro |
| Astro | Generate the static website |
| TypeScript | Develop components and logic |
| Markdown | Manage content |
| Git | Track changes |
| GitHub | Host the repository |

Check current compatibility requirements in the official documentation before installing tools.

### Initial checks

Run:

```powershell
node --version
npm --version
git --version
```

Installed versions must be compatible with the intended project.

## 4. Cloud services

| Service | Architecture role |
|---|---|
| GitHub | Repository and collaboration |
| Cloudflare Pages | Website publishing |
| Cloudflare DNS | Domain management |
| Cloudflare Email Routing | Incoming email forwarding |
| Gmail | Verified destination mailbox |
| Brevo SMTP | Outgoing email transport |
| Thunderbird | Email client |

Not every service is required to publish a website.

Incoming and outgoing email are independent components that require additional evaluation.

## 5. Domain

A custom domain is recommended for professional identity.

Before configuration:

1. Confirm ownership.
2. Identify the registrar.
3. Review renewal costs.
4. Determine DNS administration.
5. Inventory existing DNS records.
6. Assess the effects of changes on active services.

Do not replace MX, SPF, DKIM or DMARC records without understanding their impact.

## 6. Email decisions

The reference configuration separates two functions:

- Incoming: Cloudflare Email Routing forwards messages to Gmail.
- Outgoing: Thunderbird uses an external SMTP server.

These functions are not equivalent to a complete hosted business mailbox.

Before adoption, review provider terms, data protection, operational limits and support requirements.

The suitability of Brevo SMTP for individual professional correspondence remains subject to confirmation under its service terms.

## 7. Security preparation

Recommended practices:

- Enable multifactor authentication where available.
- Use unique passwords and a credential manager.
- Keep SMTP keys and tokens out of repositories.
- Review GitHub and provider permissions.
- Secure the development computer.
- Plan credential renewal and revocation.
- Avoid publishing screenshots containing private information.

SPF, DKIM and DMARC will be covered in a separate guide.

## 8. Cost assessment

Free tiers can reduce recurring expenses, but they do not eliminate domain, administration or maintenance costs.

Also consider:

- Usage limits.
- Upgrade costs.
- Contractual changes.
- Support time.
- Service disruption risks.
- Provider dependencies.

## 9. Readiness checklist

Before implementation, confirm:

- [ ] Functional scope defined.
- [ ] Domain available or registered.
- [ ] Administrative access to DNS.
- [ ] Local environment prepared.
- [ ] GitHub account available.
- [ ] Email requirements identified.
- [ ] Provider terms reviewed.
- [ ] Credentials protected.
- [ ] Budget and maintenance considered.

## 10. Next stage

The next guide will cover building and deploying a static website using Astro, GitHub and Cloudflare Pages.

[Explore the actual JH-Web implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)