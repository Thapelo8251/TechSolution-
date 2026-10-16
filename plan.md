# TechSolutions IT Consulting — Implementation Plan

## Product and delivery approach

Build the requested complete customer-facing website and operational workflows in the initialized Manus Webdev project, using its existing React 19 + TypeScript + Vite frontend, Express/tRPC server, Drizzle ORM and managed MySQL. Keep public website content open and indexable; use the supplied Manus OAuth/session/user-role foundation for admin authentication and enforce admin-only data access in server procedures. Repair requests, quote enquiries, settings, services, portfolio projects and internal status notes are persisted through additive Drizzle schema/migrations. No customer success state or reference number is shown until server persistence succeeds.

The brief omits a business phone/WhatsApp number, business email, verified service area, public domain, actual prices and notification-provider credentials. Keep these as explicit editable unconfigured placeholders. Do not invent them. Contact CTAs become active only after valid contact details are configured. Forms may persist requests in the managed database, but must explain that a request is not an appointment confirmation. Do not represent the Manus owner-only alert API as customer email. Automated business-email notifications remain an integration to configure and verify before they can be claimed as working. The admin can be secured at the API boundary, but the owner identity must be supplied through the existing `OWNER_OPEN_ID` environment setting to grant the first admin role; no public default credentials are created.

## Page structure and user experience

Implement the 15 requested public routes: Home, About, Services, IT Consulting, Web Design & Development, Computer & Laptop Repairs, Cell Phone Unlocking, Laptop Access Recovery, Branding & Digital Marketing, Portfolio, Book a Repair, Get a Quote, Contact, Privacy Policy and Terms and Conditions. Add a private `/admin` workspace for authenticated owner/staff operations. Use a shared responsive header/footer and reusable service, process, FAQ, contact, form and editorial-page components. Every route will have route-specific title/description and meaningful content. Public pages explain quote-first pricing, device authorization/eligibility, remote/on-site availability distinctions and the unconfirmed service area. Demo portfolio work is marked as demonstration only. No unsupported credentials, reviews, history, prices, turnaround times, parts, warranties, outcomes or client claims.

Repair form fields: full name; email; phone/WhatsApp; desktop/laptop/mobile/other category; manufacturer/model; requested service; issue description; preferred contact; optional date; optional service-area/collection/delivery preference; and privacy consent. Quote form fields: name; email; phone/WhatsApp; optional business name; service category; project/device description; optional budget; preferred contact; and privacy consent. Validate on client and server, use accessible errors/loading/success/failure states, honeypot and request throttling, retain only needed data, and generate a reference only after a successful database insert. Never collect passwords, PINs, OTPs or recovery codes.

Admin procedures use `adminProcedure` and Manus OAuth roles, not client-side secrecy. Support request searching, customer contact access for authorized staff, all requested statuses, internal notes, confirmed permanent deletion of an enquiry and its notes, and owner-editable contact/service catalogue/portfolio/operating-hour/service-area/notification settings. Avoid seeding admin credentials. Email notifications are not falsely claimed where no customer-email provider or recipient is configured.

## Information architecture and project files

- `client/src/App.tsx`: route table, shared shell, public and admin routing.
- `client/src/components/site/`: header, footer, WhatsApp/contact actions, service cards, process/FAQ sections, SEO helper and reusable forms.
- `client/src/pages/`: route components for all required pages, repair/quote forms and admin workspace.
- `client/src/index.css`: design tokens, responsive layout, typography, focus and motion rules.
- `client/public/images/`: custom generated technology consulting, repair and digital-solutions imagery.
- `client/public/manus-routes.json`, `sitemap.xml`, `robots.txt`, and favicon: accurate public page declarations, crawler support and brand asset.
- `shared/siteContent.ts`: editable content, SEO metadata, services, demo portfolio, contact placeholders and consent copy shared by browser/server.
- `shared/validators.ts`: request/quote schemas and server-shared field rules.
- `drizzle/schema.ts` and `drizzle/*.sql`: additive tables for repair/quote submissions, editable settings/catalogue/portfolio, and internal notes/status history.
- `server/siteDb.ts`: parameterized persistence/search/update helpers for dedicated site tables.
- `server/routers.ts` plus focused feature modules: public submission mutations and admin-only query/update procedures.
- `server/publicPages.ts`: metadata/content-bearing initial HTML for crawler-visible page responses, while retaining the React application for interactive browser rendering.
- `server/_core/index.ts`: register public page HTML before SPA fallback, keep APIs/health routes separate, and preserve the starter's OAuth, tRPC and preview-compatible cookie/session behavior.
- `.env.example` and `SETUP.md`: placeholder-only environment sample, credential and notification/domain setup, content update instructions, integration checklist and honest verification/status record.

## Design direction

Reuse the explicitly requested clean white/royal-blue corporate visual system rather than introducing alternate styles.

- **Design Movement:** contemporary Swiss-inspired corporate minimalism with an editorial technology-studio edge.
- **Core Principles:** calm clarity; credible technical craft; accessible, quote-first guidance; consistent mobile-first hierarchy.
- **Color Philosophy:** white and cool-gray surfaces keep information open; navy carries trust and long-form readability; a distinctive royal blue signals action and technical confidence; restrained pale-blue gradients add depth without visual noise.
- **Layout Paradigm:** asymmetric editorial split hero and staggered service/story sections, with clear vertical rhythm and generous alignment rather than a wall of equal cards.
- **Signature Elements:** a connected-line/terminal motif in the mark; fine blue rules and outlined icon badges; quiet gradient glow behind key technology imagery.
- **Interaction Philosophy:** predictable navigation, clear labels and progress, no hidden hover-only controls; forms state what happens next, and every unconfigured contact channel is plainly disclosed.
- **Animation:** short, restrained opacity/translate reveals and subtle hover/focus transitions; honor `prefers-reduced-motion`; do not animate essential information or delay interactions.
- **Typography System:** DM Sans for interface and body text, with Manrope for display headings; strong H1/H2/body/caption hierarchy and comfortable line lengths.
- **Brand Essence:** practical technology support and digital delivery for South African businesses and individuals; dependable, clear, resourceful.
- **Brand Voice:** professional, direct and reassuring. Examples: “A clear next step for every technology challenge.” “Approve the quote before repair work begins.”
- **Wordmark & Logo:** a custom, text-based TechSolutions lockup paired with a compact connected-circuit T/S monogram and a matching SVG favicon; no third-party branding.
- **Signature Brand Color:** royal blue `#2457D6`.

## Forms, data and privacy

Use additive MySQL migrations in the same managed database used by development and publication. Store only submitted contact/device/project details needed to follow up, consent timestamp, reference ID, status, dates and authorized internal notes. Validate and bound every input server-side, normalize email/phone, escape rendered values, add a honeypot plus lightweight rate controls, and return generic failures without leaking database details. Keep private request data out of page HTML, public queries, logs and search indexes. Admin routes return noindex behavior. Include South African POPIA-aware editable policy language, privacy notice/consent, purpose and retention/deletion guidance, and terms that accurately describe eligibility, quotes, authorization and no guarantees; state that legal compliance has not been certified.

## SEO and deployment

Use meaningful route-specific titles/descriptions, semantic headings, accessible image descriptions, canonical/social metadata only when the true public origin is configured, no guessed/internal Preview URL, a route-synchronized sitemap and robots document, and `public/manus-routes.json` containing public page routes. For crawler-visible body content, serve content-bearing initial HTML from the Express page handler and keep client-side pages consistent with it. Use only verified details in structured data; omit fabricated LocalBusiness address/coverage. Keep `/api/health` public for the configured container health check and APIs out of the SPA page route map.

Build and run the existing production script (`pnpm build`, `pnpm start`), keep the Dockerfile/health route consistent, check app types/tests and public endpoints, and prepare an actual Manus checkpoint and publication only through the project workflow. `publishing.auto_publish` is currently false, so publication is an explicit separate step after a confirmed checkpoint. Report a Preview URL unless an actual published URL is confirmed. Explain custom-domain/DNS/HTTPS setup without claiming it has occurred.
