# CrestNode prototype

CrestNode is a self-contained, single-file school operations and family communication prototype for Nigerian schools. Open `index.html` directly in a browser; there is no build step, package manager, or backend. It includes a public marketing site, registration and school-finder flows, two fictional school portals, and role-based dashboard previews.

All names, school information, student records, fees, events and performance figures are fictional sample data. Demo role switching is a user-interface demonstration only, not authentication or authorization.

## Interaction checklist

| Feature | How to test |
| --- | --- |
| Public navigation and responsive menu | Open the landing page and use section links. At mobile width, activate the menu button. |
| School registration wizard | Select **Register Your School**. Complete the six steps, try an invalid or reserved portal code, then review the local-only completion message. |
| School finder | Select **Find My School Portal**, search for Greenwood or Bright Future, select a result and continue. Search for a missing school to see the empty state. |
| School-branded login | Visit either school from the finder. Submit non-empty demo values and select a role. Credentials are not checked or stored. |
| Dashboard role previews | In the dashboard, change the **Demo mode** role selector. Each role has a different module set. It is not a security boundary. |
| Tenant switch | Use the school chip in the dashboard header and choose Greenwood International School or Bright Future Academy. Their sample records and brand colours are distinct. |
| Command palette | Use the dashboard search button or press `Ctrl+K` / `Cmd+K`; search for a module and press Enter. Escape closes the palette. |
| Student search and filters | Open Students and search by name, ID or class; use the attendance status selector. The list is fictional and searchable. |
| Attendance controls | Open Attendance, change per-student status, use **Mark all present**, then **Save attendance**. The save is local to this prototype session. |
| CSV and calendar exports | Use **Export CSV** in the student/attendance views or `.ics` in Calendar. Both create actual downloadable files containing sample data. |
| Fees and payment demonstration | Open Fees and select **Pay now**. Follow the provider mock. No card details are collected and it never claims to verify payment. |
| Announcements | Open Announcements, type a message and submit. Large audiences request confirmation. No email, SMS, WhatsApp or push notification is sent. |
| Calendar | Open Calendar, select a day to inspect the sample event details, or download the `.ics` file. Month arrows currently explain that the sample calendar is fixed to October 2026. |
| Absence requests | Open Absence requests in the Parent role and submit the sample form. No school is notified. |
| Reduced motion and language | Use the dashboard preference controls or the landing-page footer motion toggle. Cycle the dashboard language control through English, Igbo, Yoruba and Hausa. Translation strings are samples and require native-speaker review. |
| Contact form and legal pages | Submit the contact form with valid values; it displays a local-only success message. Privacy and Terms links open draft placeholders pending legal review. |
| Prototype actions and sample errors | Several non-core CRUD operations explain that backend integration is required. The demo data is explicitly identified as fictional. |

## Local testing

1. Open `index.html` directly in a current desktop browser, or serve this folder with any static file server.
2. The landing page is `#/` (also the initial route); the school finder is `#/finder`; registration is `#/register`; school portals are `#/school/greenwood/login` and `#/school/brightfuture/login`; dashboards are `#/app/greenwood/overview` and `#/app/brightfuture/overview`. Platform preview routes include `#/platform/overview`, `#/platform/schools`, `#/platform/subscriptions` and `#/platform/security`.
3. Use DevTools responsive/device emulation at 320, 375, 414, 768, 1024, 1280 and 1536px. Check for horizontal scrolling and that dashboard navigation switches to the mobile bottom bar.
4. In Chromium DevTools, open **Rendering → Emulate CSS media feature prefers-reduced-motion** and select `reduce`. The footer/dashboard preference toggle also stores only a harmless UI preference in local storage.
5. Verify downloads in the browser download shelf. The CSV and iCalendar files are generated locally with `Blob`; no PDF or Excel generator is included.
6. Google Fonts and Tailwind are optional CDN enhancements. If unavailable, system fonts and the inline critical CSS preserve the core layout and controls. The prototype does not need a network connection for its local mock data and routes.

## Architecture and backend integration roadmap

The file separates demo data, translations, a role-capability matrix, mock asynchronous API methods, route parsing, view renderers and event handlers. `API` functions are the intended seam for replacing mock responses with authenticated REST calls. `can()` is only a UI affordance; a production server must independently authorize every request.

Recommended production direction: a TypeScript frontend and versioned API backed by PostgreSQL, with tenant context derived from a verified session rather than request-provided tenant IDs. Each tenant-owned table should carry a `tenant_id`; enforce tenant isolation with database row-level security and server-side authorization policies. Keep provider secrets and privileged operations server-side.

1. **Tenant data and isolation:** design migrations and tenant-scoped schemas for students, guardians, staff, classes, attendance, results, invoices, payments, events, documents, absence requests, announcements and audit events. Apply PostgreSQL row-level security and test cross-tenant access boundaries.
2. **Identity and authorization:** integrate a suitable identity provider, support school/user invitations and recovery, and implement server-side RBAC and policy checks for every resource and action. Never trust the demo role selector or frontend `can()` check.
3. **School onboarding and tenancy:** build a registration, verification and approval workflow; reserve and validate portal identifiers; configure wildcard subdomain DNS, TLS certificates and edge routing. Resolve a hostname only against an approved tenant registry.
4. **Payments and subscriptions:** integrate Paystack or Flutterwave through server-created payment sessions. Confirm payments only from authenticated provider webhooks, with idempotency and reconciliation. Add subscription billing, plan entitlements, invoices and secure billing operations.
5. **Communications:** provide queued email, SMS and WhatsApp integrations with opt-in/preference management, delivery status, retries, templates and audit trails. Do not expose provider credentials in the browser.
6. **Documents and reports:** store uploads in private object storage, authorize access and issue short-lived signed URLs. Generate PDF and Excel reports server-side, apply access controls and log sensitive exports.
7. **Calendar and SSO:** add authenticated calendar feeds with revocable tokens. Implement Google Workspace and Microsoft 365 SSO only after configuring identity-provider clients, redirect URIs, account linking and school-level policy.
8. **Operations:** add structured observability and alerting, privacy-conscious audit logs, encrypted backups with restore drills, environment separation, dependency scanning, automated tests, CI/CD and a documented incident-response process.

## Known limitations

- This is a front-end demonstration, not a deployable school-management service. There is no database, authentication, authorization enforcement, data persistence, API, email, SMS, WhatsApp, SSO, payment provider, tenant approval, file storage, or subscription billing.
- Several secondary CRUD, report, month-navigation and plan-management actions are illustrative rather than full workflows. Registration and absence requests are local demo flows.
- Mock records use small generated sets of 40 fictional student entries for each of the two sample schools. They are not representative real-world records.
- CDN features can be unavailable offline. No browser screenshots, cross-device certification, formal contrast audit, or performance profile are included as generated artifacts.
- Translations are demonstration copy and need native-speaker review before being used for school communication.
