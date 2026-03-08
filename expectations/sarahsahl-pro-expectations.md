# sarahsahl.pro — Expectations Document

> Source: `the_penny_lane_project/sarahsahl_pro/portfolio_report.md`
> Last reviewed: 2026-03-08

---

## 1. Architecture Constraints

### 1.1 Static HTML/CSS/JS site
This is a static site — no server-side rendering, no backend framework, no Node.js server. File `critical` for any server-side component, build tool generating SSR output, or backend server configuration added to this project.

### 1.2 Hosted on Netlify; custom domain `sarahsahl.pro`
The site is hosted on Netlify with the custom domain `sarahsahl.pro`. File `suggestion` for any configuration change targeting a different hosting provider.

---

## 2. Media Assets

### 2.1 Cloudinary for image/video hosting
All images and videos must be hosted on Cloudinary. Do not self-host media files in the repository or on the server. File `warning` for any media file committed to the repository or referenced from a non-Cloudinary URL.

### 2.2 `videos.json` format must remain stable
`videos.json` is the data layer for the video gallery. Its schema (field names, structure) must not change in a breaking way. File `critical` for any change to `videos.json` that removes or renames existing fields without updating all consumers.

---

## 3. Contact and Forms

### 3.1 Netlify contact form must remain functional
The contact form must use Netlify's built-in form handling. Do not replace Netlify form handling with a custom backend. File `critical` for any change that disables or replaces the Netlify contact form.

---

## 4. Content Integrity

### 4.1 Quantified metrics must not be modified without owner approval
All quantified metrics in the site copy (e.g., years of experience, project counts, performance numbers) are self-reported by Sarah Sahl. Do not modify these values without explicit owner approval. File `critical` for any change to quantified metrics without documented approval.

### 4.2 Resume PDF must remain available at its current path
The resume PDF download must remain available at its existing URL path. File `warning` for any change that moves or removes the resume PDF without establishing a redirect.

---

## 5. Brand Identity

### 5.1 Brand language: cream backgrounds, muted teal/green accents, warm/restrained visuals
The brand identity uses cream backgrounds, muted teal/green accents, and a warm, restrained visual language. Do not introduce high-contrast color palettes, aggressive accent colors, or visually loud UI elements. File `warning` for any CSS change that departs significantly from the established brand palette.

---

## 6. Privacy and Analytics

### 6.1 No tracking pixels, analytics scripts, or third-party embeds without owner approval
No tracking pixels, analytics scripts (Google Analytics, Meta Pixel, etc.), or third-party embeds may be added without explicit owner approval. File `critical` for any tracking or analytics addition without documented approval.

---

## 7. Out-of-Scope Constraints

- Do not add user authentication or accounts
- Do not add a CMS or database backend
- Do not add JavaScript build tooling (Webpack, Vite, etc.) — this is a plain static site
