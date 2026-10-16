# TechSolutions website — setup and owner handover

## Current implementation

This project is a React/TypeScript frontend with an Express/tRPC API and Drizzle/MySQL persistence. Repair requests and quote enquiries are designed to be stored in the project's managed database; successful references are created only after a storage insert succeeds. The private `/admin` dashboard reads and edits customer requests, statuses, notes, the service catalogue, portfolio and business contact settings through server-protected procedures.

The managed Webdev project was initialized with Server and Database enabled. The database schema migration creates additional inquiry, note, catalogue, project and settings tables. Development and published environments share the same managed database, so do not add test customer records or run destructive database tests against it.

## Required owner configuration

1. **Authorize the first administrator.** Set `OWNER_OPEN_ID` in the project environment to the intended authorized owner's Manus `openId`. Until set, the OAuth user receives the normal `user` role and `/admin` access is denied. Do not add a public/default password. After configuration, sign in at `/admin` and verify the account is authorized.
2. **Enter verified contact data.** In the dashboard's Business settings, add the real WhatsApp number, business email, optional phone, accurate operating hours, confirmed service area, and truthful remote/on-site availability. Blank WhatsApp/email actions are intentionally disabled until valid values are saved. Do not publish a physical address or coverage claim unless verified.
3. **Configure customer email notifications (not yet connected).** The Manus owner-notification channel is owner-only and must not be used as customer email. Choose a transactional email provider, configure its credential using protected project secrets (not source code), verify the sender/domain and business recipient, implement/enable the provider adapter, send a test from a safe non-customer event, and confirm delivery/error handling. The dashboard notification preference alone does not send email.
4. **Review legal and business copy.** Confirm the responsible business identity, privacy contact, operating hours, retention/deletion period, service area, repair availability, authorized unlocking terms, and actual service/pricing details. The provided privacy policy and terms are editable draft text and are not a legal compliance certification.
5. **Portfolio and prices.** Demonstration portfolio cards are labelled as examples, not client engagements. Add only genuine work with permission. Keep prices at “Request a Quote” until the business supplies actual figures and terms. Explain any diagnostic fees, parts, delivery, warranties or turnaround only after confirmation.

## Environment and integration checklist

`.env.example` contains placeholders only. The platform supplies the managed database DSN and Manus OAuth values. Never commit a populated `.env` or put a credential in a frontend variable. `OWNER_OPEN_ID` must be configured by the owner. Email-provider credential and any related signing configuration must be entered through protected project secret configuration, not chat or source files. Business WhatsApp/email details are editable public content, not secrets, and must be verified before enabling links. `SITE_PUBLIC_ORIGIN` must be set to the actual canonical HTTPS domain before absolute canonical/social URL metadata and sitemap locations are emitted.

Email notifications, verified business contact, service-area coverage, operating hours, prices, the custom domain and public deployment are not represented as configured merely because their fields or setup instructions exist.

## Editing and publishing

Use the owner dashboard for inquiry status/notes, published services and price labels, portfolio content and contact settings. Do not mark a demonstration project as genuine client work. The navigation, required pages and concise service copy live in `shared/siteContent.ts`; shared privacy/terms copy is in `client/src/pages/InfoPages.tsx`; server-rendered SEO summaries are in `server/publicPages.ts`.

The current project configuration has publication auto-publish disabled. To publish, complete and check the production build, push a confirmed Manus checkpoint, then use the actual Manus publish workflow. Verify the published website URL, page status/metadata, `/manus-routes.json`, `/sitemap.xml`, `/robots.txt`, form persistence, admin authorization and contact links before calling it live. Preview is not publication.

For a custom domain, add the domain using the actual Manus hosting/domain workflow, copy the exact DNS records Manus provides into the domain registrar's DNS zone, wait for DNS propagation and certificate issuance, configure the verified canonical origin, then check HTTPS and redirects. Do not copy example DNS records or guess IP addresses. A custom domain and DNS are not configured by this source project alone.


## Verification completed (2026-10-16)

- `pnpm check`: passed.
- `pnpm test`: passed, 6/6 existing tests.
- `pnpm build`: passed for client and server. Route-level chunks keep the main JavaScript chunk below 500 kB; Vite emits its known warning that the platform's non-module config script is intentionally not bundled as an ES module.
- The additive database migration was applied to the managed shared database. The public-content API was verified through the actual Preview database connection; no synthetic customer requests/quotes were written.
- All 15 public pages and the private `/admin` page returned HTTP 200 on Preview; the admin response is `noindex`. The `/manus-routes.json` manifest declares 16 paths (15 public plus admin); `/api/health`, `/robots.txt` and `/sitemap.xml` return HTTP 200. The three managed custom visuals are reachable on the stable Preview origin.
- Desktop and mobile Preview screenshots were inspected, including home, repairs, portfolio, admin sign-in and both form pages. An unauthenticated administrator API request returned HTTP 403.
- The repair/quote submission success path has **not** been exercised with a test customer record because development and publication share the same database. It is implemented to persist server-side, but have the owner perform a safe test only after admin identity and business follow-up are configured.

## Remaining before public launch

- Set `OWNER_OPEN_ID` to the intended owner's verified Manus openId. It is currently absent, so dashboard authorization cannot be activated yet.
- Enter verified WhatsApp/email/phone, operating hours, remote/on-site details and service-area coverage in the authorized dashboard. Until real contact values are set, their actions intentionally remain disabled with setup notes.
- Configure and verify a transactional email provider and recipient if business email notifications are required. The current dashboard preference is not a delivery integration.
- Confirm privacy contact, business identity, retention period, actual service terms, availability and pricing; obtain appropriate legal review of the policy/terms drafts.
- Set `SITE_PUBLIC_ORIGIN` only after a real canonical HTTPS domain is established. Until then canonical/social absolute URLs are omitted and the sitemap is a valid empty URL set rather than guessing a domain.
- The site is **not publicly published**. Publication auto-publish is disabled. Use the actual Manus Publish workflow only after the owner has provided the needed information and explicitly chosen to launch. Preview is not a live production deployment.
