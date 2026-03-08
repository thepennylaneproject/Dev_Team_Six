# Restoration Project — Expectations Document

> Source: `the_penny_lane_project/restoration-project/blog_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 Next.js App Router
The site uses Next.js with the App Router. File `warning` for any new route created using Pages Router conventions.

### 1.2 TypeScript
All source files must be TypeScript. File `warning` for any `.js` source file added to the application.

### 1.3 Tailwind CSS
Styling uses Tailwind CSS. File `suggestion` for style solutions that bypass Tailwind.

### 1.4 Netlify deployment
The site is deployed to Netlify. File `suggestion` for any configuration change targeting a different hosting provider.

---

## 2. Content Ingestion

### 2.1 RSS from Substack via `fast-xml-parser`
RSS content is ingested from Substack using `fast-xml-parser`. Do not replace this parser without explicit approval. File `warning` for any RSS parsing change that removes `fast-xml-parser`.

### 2.2 All imported HTML must pass through DOMPurify sanitization
Any HTML content imported from RSS feeds must be sanitized with DOMPurify before rendering. Unsanitized HTML rendered to the page is an XSS vulnerability. File `critical` for any HTML content rendered without DOMPurify sanitization.

---

## 3. Monitoring and Security

### 3.1 Sentry must remain integrated
Sentry is integrated for runtime monitoring. Do not remove Sentry from the project. File `warning` for any change that removes or disables Sentry.

### 3.2 Netlify security headers must not be weakened
Security headers defined in `netlify.toml` (Content-Security-Policy, X-Frame-Options, HSTS, etc.) must not be removed or weakened. File `critical` for any header policy relaxation.

---

## 4. Content Standards

### 4.1 Political/factual claims must be sourced before investor-facing reuse
All political or factual claims in briefings must be sourced and verified before they are reused in investor-facing materials. File `warning` for any content marked as investor-facing that contains unsourced claims.

### 4.2 Substack is the canonical content source
Content published to Substack is the canonical source. The on-site `/articles` section must remain a reflection layer — do not implement independent editorial tools that diverge from the Substack canonical. File `warning` for any content editing functionality added to the on-site layer.

---

## 5. Brand Assets

### 5.1 Brand assets in `public/brand/` must not be overwritten
Brand assets stored in `public/brand/` must not be modified or overwritten without explicit owner approval. File `critical` for any PR that modifies existing files in `public/brand/`.

---

## 6. Privacy

### 6.1 No user data collection beyond Netlify contact form
The site must not collect user data beyond what Netlify's built-in contact form handling provides. Do not add analytics scripts, tracking pixels, or third-party embeds that collect user data. File `critical` for any analytics or tracking addition.

---

## 7. Out-of-Scope Constraints

- Do not implement a CMS or editorial workflow on-site
- Do not add user accounts or authentication
- Do not replace Netlify's built-in form handling with a custom backend
