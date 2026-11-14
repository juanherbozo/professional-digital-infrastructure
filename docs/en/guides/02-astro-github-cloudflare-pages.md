# Guide 02 — Astro, GitHub and Cloudflare Pages

[Home](../../../README.md) | [Architecture](../architecture.md) | [Español](../../es/guides/02-astro-github-cloudflare-pages.md)

## 1. Objective

Create, version and deploy a static website using Astro, GitHub and Cloudflare Pages.

This guide describes a reproducible new project. JH-Web — Personal Digital Platform is the reference implementation, but its private repository is not distributed as a starter template.

## 2. Deployment architecture

Local development → Git → GitHub → Cloudflare Pages → Visitor.

A custom domain will be addressed in a later guide. Cloudflare Pages initially provides a `pages.dev` address.

## 3. Requirements

- A Node.js version compatible with the selected Astro version.
- npm and Git.
- Code editor.
- GitHub account.
- Cloudflare account.

Verify:

```powershell
node --version
npm.cmd --version
git --version
```

Check current Astro requirements in the official documentation. Do not assume a specific Node.js release will remain supported indefinitely.

## 4. Create the project

On Windows PowerShell:

```powershell
npm.cmd create astro@latest
```

Follow the interactive setup wizard, select a project directory and choose the appropriate options.

Then:

```powershell
Set-Location ".\your-project-name"
npm.cmd install
npm.cmd run dev
```

Skip the installation command if the wizard already installed dependencies.

Open the local URL reported by Astro, typically `http://localhost:4321`.

On macOS or Linux, use `npm` instead of `npm.cmd`.

## 5. Basic structure

A typical structure:

```text
public/
src/
  pages/
  components/
  layouts/
astro.config.mjs
package.json
package-lock.json
```

The exact layout depends on the starter template and integrations.

- `src/pages`: pages and routes.
- `src/components`: reusable components.
- `src/layouts`: shared layouts.
- `public`: unprocessed public assets.
- `package.json`: scripts and dependencies.
- `astro.config.mjs`: configuration, when using this filename.

## 6. Content and components

Create pages under `src/pages` and reuse components when appropriate.

Astro supports Markdown content; organization mechanisms depend on the selected version and requirements.

For multilingual sites, plan routes, navigation, metadata and translation management before producing extensive content.

## 7. Local build

Run:

```powershell
npm.cmd run build
npm.cmd run preview
```

The `build` script must exist in `package.json`.

For a standard static Astro project, generated output is usually located in `dist/`.

Validate pages, images and links in the built version, not only in the development server.

## 8. Version control

Check repository status:

```powershell
git status
```

If the project has not been initialized:

```powershell
git init
git branch -M main
```

Review `.gitignore` before staging files. Exclude installed dependencies, environment files containing secrets and other private assets.

After reviewing the changes:

```powershell
git add .
git diff --cached --check
git diff --cached --stat
git commit -m "feat: initialize Astro website"
```

Only use `git add .` once the complete staging scope has been reviewed.

## 9. Push to GitHub

Create a new GitHub repository.

If the local project already contains a commit, use your own repository URL:

```powershell
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git push -u origin main
```

Replace `USERNAME` and `REPOSITORY`.

If `origin` already exists, inspect `git remote -v` before changing it.

Do not commit keys, credentials, private `.env` files or third-party confidential data.

## 10. Configure Cloudflare Pages

In the Cloudflare dashboard:

1. Open Workers & Pages.
2. Create a Pages application.
3. Choose Git integration.
4. Authorize access to the necessary repository.
5. Select the repository.
6. Configure build settings.
7. Save and deploy.

Typical static Astro settings:

| Setting | Value |
|---|---|
| Production branch | `main` |
| Build command | `npm run build` |
| Output directory | `dist` |
| Root directory | Project root unless otherwise structured |

Select a Node.js version supported by both the project and the Cloudflare build environment.

For monorepos, verify the root directory carefully.

## 11. Automatic deployments

With Git integration enabled, commits pushed to the production branch trigger builds and deployments.

Other branches and pull requests may receive preview deployments depending on configuration.

Recommended workflow:

1. Edit locally.
2. Run the build.
3. Review changes.
4. Commit.
5. Push to GitHub.
6. Inspect the Cloudflare deployment.
7. Validate the published website.

## 12. Production validation

Verify:

- Successful deployment.
- Home page and secondary routes.
- Navigation links.
- Images and static files.
- Metadata and titles.
- Mobile and desktop layout.
- Languages, where applicable.
- No exposed secrets.

A successful deployment status does not replace functional verification.

## 13. Common issues

| Symptom | Check |
|---|---|
| `npm` blocked in PowerShell | Use `npm.cmd` without unnecessarily changing execution policy |
| Dependency errors | Check Node.js version and lockfile |
| Failed build | Inspect complete build logs |
| Page not found | Check routes and generated pages |
| Missing images | Check paths and case sensitivity |
| Old website content | Verify branch, commit and active deployment |
| Incorrect directories | Check configured Pages root and output |

## 14. Security and maintenance

- Enable MFA in GitHub and Cloudflare.
- Limit integration permissions.
- Keep secrets out of public source code.
- Review dependency updates.
- Maintain a recoverable Git history.
- Assess configuration and DNS recovery needs.

This workflow is not a complete disaster recovery plan.

## 15. Expected outcome

A static website available through Cloudflare Pages, connected to a GitHub repository and ready for ongoing updates.

The next guide will cover custom domains and DNS.

## 16. Official references

- [Install Astro](https://docs.astro.build/en/install-and-setup/)
- [Deploy Astro on Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-an-astro-site/)
- [Cloudflare Pages Git integration](https://developers.cloudflare.com/pages/get-started/git-integration/)
- [JH-Web reference implementation](https://juanherbozo.com/en/projects/jh-web-personal-digital-platform/)