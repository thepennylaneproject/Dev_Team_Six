## SECTION 1: PROJECT BASICS

1. **Project name**: The Restoration Project `[VERIFIED]`
2. **Project type**: Political publishing website / issue briefings and action-oriented publication `[VERIFIED]`
3. **URL**: [therestorationproject.substack.com](https://therestorationproject.substack.com) `[VERIFIED in code]`; standalone site URL `[REQUIRES MANUAL INPUT]`
4. **Date range**: Repository started **January 24, 2026**; latest verified repo activity **February 4, 2026** via git history `[VERIFIED]`. Whether it is still actively maintained after February 4, 2026 `[REQUIRES MANUAL INPUT]`
5. **Current state**: Early-stage but launch-ready/live-oriented political media site with published core content, draft policy blueprints, and a Substack-backed article system `[VERIFIED + INFERRED]`
6. **One-paragraph description**: The Restoration Project is a political publishing project that documents abuses or failures of power, translates them into plain language, and pairs those briefings with concrete policy blueprints and action prompts. The site frames itself not as neutral commentary but as a “repair shop” for people who want evidence, analysis, and practical next steps rather than outrage cycles or pundit spin. By February 2026 it had evolved from a small set of hand-authored pages into a broader publishing system that pulls dispatches from Substack while keeping the site as the structured home for framing, navigation, and calls to action. `[VERIFIED + INFERRED]`

## SECTION 2: THE "WHY" — ORIGIN & MOTIVATION

1. **What problem was this built to solve for the creator personally?**: It appears to solve a personal need to make political harm legible: to move from diffuse outrage and media noise toward documented, structured, actionable analysis. The recurring pattern is “receipts first,” plain language, and mechanisms over vibes. `[INFERRED from [app/about/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/about/page.tsx#L19), [app/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/page.tsx#L53)]`
2. **What circumstances led to its creation?**: The explicit trigger is political frustration with distraction, captured institutions, and narratives that protect power rather than ordinary people. Specific personal circumstances such as health, career, or finances are not stated in the repo. `[VERIFIED for political motive; REQUIRES MANUAL INPUT for personal circumstances]`
3. **What was the creator's situation when they started building this?**: The repo suggests an early, fast-moving founder-build phase: the project was initialized on **January 24, 2026**, core content shipped immediately, and the site repeatedly describes itself as “early” and “in its early stages.” Beyond that, the founder’s personal situation is not documented. `[VERIFIED + REQUIRES MANUAL INPUT]`
4. **Did the scope or purpose evolve over time?**: Yes. The initial version was a tightly scoped, hand-maintained set of briefings and blueprints. On **February 3, 2026**, the project explicitly shifted to a “Substack-powered content architecture,” adding a dynamic `/articles` system and archiving the original hardcoded content for “potential resurrection.” The purpose broadened from a fixed political microsite into an ongoing publishing operation with distribution and discovery built in. `[VERIFIED from git history and [legacy/content-archived.ts](/Users/sarahsahl/Desktop/restoration.project/legacy/content-archived.ts#L1)]`
5. **What tension or frustration does this project respond to?**: It responds to the sense that mainstream political discourse hides real power inside process, bureaucracy, and spectacle; that institutions “captured by money” are not self-correcting; and that ordinary people are asked to absorb harm without usable tools. `[VERIFIED in language, especially [app/about/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/about/page.tsx#L19) and [app/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/page.tsx#L107)]`

## SECTION 3: SKILLS & CAPABILITIES DEMONSTRATED

1. **Technical skills on display**:

- Next.js App Router used to build the site and article routes `[VERIFIED in [package.json](/Users/sarahsahl/Desktop/restoration.project/package.json#L1)]`
- React and TypeScript used for the page system and shared components `[VERIFIED]`
- Tailwind CSS used for layout, typography, and visual system `[VERIFIED]`
- RSS ingestion via `fast-xml-parser` to turn Substack into a structured on-site publication feed `[VERIFIED in [components/Feed.tsx](/Users/sarahsahl/Desktop/restoration.project/components/Feed.tsx#L1)]`
- DOMPurify used to sanitize imported HTML, showing security awareness around third-party content ingestion `[VERIFIED]`
- Sentry integrated for runtime monitoring and error capture `[VERIFIED]`
- Netlify deployment configuration and security headers show production-minded deployment discipline `[VERIFIED in [netlify.toml](/Users/sarahsahl/Desktop/restoration.project/netlify.toml#L1)]`

2. **Non-technical skills on display**:

- Sharp political writing and editorial framing `[VERIFIED]`
- Research sensibility centered on primary sources and evidentiary language `[VERIFIED in [app/about/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/about/page.tsx#L38)]`
- Policy translation: turning a case into a mechanism, then into an action kit `[VERIFIED]`
- Brand strategy: consistent language, stance, and audience targeting `[VERIFIED]`
- Information architecture: briefing -> blueprint -> action -> subscription flow `[VERIFIED + INFERRED]`

3. **Complexity level**: Technically this is a modest web application, not a deep software platform. What makes it more than a static site is the integration of live publication feeds, sanitization, routing, content metadata, and a deliberate editorial architecture. The real complexity is conceptual: it systematizes political analysis into reusable formats rather than posting disconnected commentary. `[VERIFIED + INFERRED]`

4. **Most impressive element**: The strongest signal is the creator’s ability to productize political analysis. The project does not just publish opinions; it builds a repeatable structure where evidence becomes briefing, briefing becomes blueprint, and blueprint becomes action. That is a product thinker’s move, not just a writer’s. `[INFERRED from repo structure]`

5. **Design and brand sensibility**: The visual language is intentionally protest-print inspired: bold uppercase headlines, black/white base, restrained red accents, editorial hero art, and tight copy hierarchy. It signals seriousness, urgency, and restraint rather than startup polish or activist chaos. `[VERIFIED in [app/globals.css](/Users/sarahsahl/Desktop/restoration.project/app/globals.css#L1), git commit `6d6aef1`, and brand assets]`

## SECTION 4: CONTENT & SUBSTANCE INVENTORY

1. **Site structure**: Home, About, Articles, Briefings, Blueprints, Get Involved, Privacy, plus individual pages for **2 briefings** and **2 blueprints**. `[VERIFIED]`
2. **Content highlights**:

- “State-Sanctioned Terror” is the clearest example of the project’s method: event, source basis, official narrative, public record, analysis. `[VERIFIED]`
- “Automatic Independent Investigations” is the strongest policy artifact because it translates outrage into mechanism and enforcement. `[VERIFIED]`
- “When the Court Stops Checking Power” broadens the project from incident analysis to institutional analysis. `[VERIFIED]`

3. **Positioning**: The creator positions the project as explicitly non-neutral, evidence-led, anti-spin, anti-surveillance, and oriented toward ordinary people rather than institutions. Professional identity projected: politically serious systems analyst with product instincts. `[VERIFIED + INFERRED]`

4. **Calls to action**: Subscribe on Substack, save and share primary-source “receipts,” pressure legislators and institutions, bring demands to meetings, and support people directly affected by policy harm. `[VERIFIED in [app/get-involved/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/get-involved/page.tsx#L16) and blueprint pages]`

5. **Content inventory**: Verified on-repo inventory is **2 authored briefings**, **2 authored blueprints**, and a dynamic `/articles` publication layer sourced from Substack. Total live dispatch count on Substack `[REQUIRES MANUAL INPUT]` because it was not independently retrievable from the current environment.
6. **Editorial voice**: Direct, accusatory without being chaotic, morally explicit, structurally focused, and rhetorically disciplined. It prefers systems language over personality drama. `[VERIFIED]`
7. **Strongest pieces**:

- “State-Sanctioned Terror” `[VERIFIED]`
- “Automatic Independent Investigations” `[VERIFIED]`
- “When the Court Stops Checking Power” `[VERIFIED]`

8. **Themes**: Institutional capture, democratic accountability, anti-distraction politics, primary-source evidence, consequences for power, ordinary people as the real end-user. `[VERIFIED]`
9. **Research depth**: Moderate but intentional. The project repeatedly claims primary-source grounding and names source types; the briefings include some source basis language, but there is not yet a full citation apparatus or deep reference system inside the repo. `[VERIFIED]`
10. **Audience**: Politically engaged readers who distrust media spin, want clear framing, and are willing to act; likely readers who are movement-adjacent but not necessarily policy experts. `[INFERRED from copy]`

## SECTION 5: THE THROUGH-LINE

1. **Shared DNA**: `[INFERRED from this repo + user-supplied portfolio context]` The same traits that would matter in founder-led technology products are visible here: translating complexity for users, building around individual agency, exposing hidden system incentives, structuring information into usable workflows, and treating institutions as failure-prone systems rather than benevolent defaults.
2. **Evolution markers**: This looks like an earlier or parallel expression of capabilities that later show up in product form elsewhere: research becomes interface, argument becomes workflow, outrage becomes tool. The project demonstrates the founder learning to package analysis into systems that people can actually use. `[INFERRED]`
3. **Narrative bridge**: The Restoration Project reads like a precursor to a broader mission of returning leverage to individuals. It taught the founder how to identify where institutions hide power, how to translate that into legible user-facing structure, and how to design around action instead of abstraction. From there, it is a short conceptual jump to building technology products that do the same in work, AI, or labor systems. `[INFERRED]`
4. **Thematic connection**: The shared worldview is that institutions optimize for themselves unless forced otherwise, and that individuals need better tools, better information, and better leverage. This project applies that logic politically; the larger portfolio appears to apply the same logic economically and technologically. `[INFERRED]`

## SECTION 6: USABLE ASSETS

1. **Quotable lines**:

- “We are not building another brand. We are building a set of tools...” `[VERIFIED, source: [app/about/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/about/page.tsx#L28)]`
- “This is not a think tank. This is a repair shop.” `[VERIFIED, source: [app/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/page.tsx#L107)]`
- “A system that keeps hurting people and then asks for patience is not confused. It is functioning.” `[VERIFIED, source: [data/content.ts](/Users/sarahsahl/Desktop/restoration.project/data/content.ts#L36)]`
- “If the system can ignore the mechanism, the mechanism does not exist.” `[VERIFIED, source: [app/blueprints/001-independent-investigations/page.tsx](/Users/sarahsahl/Desktop/restoration.project/app/blueprints/001-independent-investigations/page.tsx#L121)]`

2. **Visual assets**:

- Masthead/logo: [public/brand/masthead/masthead.svg](/Users/sarahsahl/Desktop/restoration.project/public/brand/masthead/masthead.svg)
- Primary logo: [public/brand/logo-primary/rp_logo_clean-v1.svg](/Users/sarahsahl/Desktop/restoration.project/public/brand/logo-primary/rp_logo_clean-v1.svg)
- Hero image: [public/brand/hero-primary/rp_hero_clean-v1.png](/Users/sarahsahl/Desktop/restoration.project/public/brand/hero-primary/rp_hero_clean-v1.png)
- Briefing editorial art: [public/editorial/briefing/briefing-001-protection-crossed_3_1.png](/Users/sarahsahl/Desktop/restoration.project/public/editorial/briefing/briefing-001-protection-crossed_3_1.png)
- Blueprint editorial art: [public/editorial/blueprints/blueprint-001-justice-clean_3_1.png](/Users/sarahsahl/Desktop/restoration.project/public/editorial/blueprints/blueprint-001-justice-clean_3_1.png)

3. **Data points**:

- Repo start date: **January 24, 2026** `[VERIFIED]`
- Latest verified repo update: **February 4, 2026** `[VERIFIED]`
- Verified authored content: **2 briefings, 2 blueprints** `[VERIFIED]`
- Verified social footprint in footer: **5 linked channels** `[VERIFIED in [components/Footer.tsx](/Users/sarahsahl/Desktop/restoration.project/components/Footer.tsx#L2)]`
- Audit docs claim **30+ issues fixed** and **13/13 routes verified** `[VERIFIED in repo docs, though self-reported]`

4. **Proof of work**:

- Rapid build cadence: **15 commits** between January 24 and February 4, 2026 `[VERIFIED from git log]`
- Clear product pivot on **February 3, 2026** to a scalable publication architecture `[VERIFIED]`
- Multiple audit/fix summary docs show iterative refinement rather than a one-off concept drop `[VERIFIED]`

## SECTION 7: HONEST ASSESSMENT

1. **What this project does well**: It demonstrates moral clarity, editorial discipline, systems thinking, and the ability to turn political frustration into structured, reusable artifacts. It also shows the founder can package analysis into an interface and distribution system rather than leaving it as raw text. `[VERIFIED + INFERRED]`
2. **What it doesn't do**: It does not yet prove large audience traction, deep original reporting infrastructure, comprehensive citation systems, or a mature policy research operation. The content base is still small and some blueprints are explicitly drafts. `[VERIFIED]`
3. **What it signals to an investor**: The founder is not only technical; they are mission-forming. They appear capable of identifying structural problems, building crisp narrative frameworks, and shipping coherent products around those frameworks quickly. This signals conviction, synthesis, and execution under ambiguity. `[INFERRED]`
4. **Risk of inclusion**: The obvious risk is political sharpness. Some readers will see the rhetoric as polarizing or outside the frame of a technology portfolio. The mitigation is to present it as origin-story evidence: this is where the founder’s anti-capture, pro-individual, systems-first worldview was made explicit, not a distraction from the portfolio but a precursor to it. Also, any fact-sensitive claims in the briefings should be reviewed before investor-facing reuse. `[INFERRED + VERIFIED]`

## SECTION 8: NARRATIVE SUMMARY

**Version A — First person (founder voice):**  
I built The Restoration Project because I was tired of watching real harm get buried under spectacle, euphemism, and process. I wanted a way to document what was happening, translate it into plain language, and turn analysis into something people could actually use. That project taught me how often institutions hide power inside complexity, and how important it is to build tools that return leverage to the individual. The products I build now come from the same place.

**Version B — Third person (site copy voice):**  
The Restoration Project is an early political systems project built around a simple premise: ordinary people need clearer evidence, sharper analysis, and more usable tools than institutions are willing to provide. It began as a set of issue briefings and policy blueprints, then evolved into a broader publication architecture anchored by Substack. In the context of the founder’s larger body of work, it marks the point where personal frustration hardened into method: document the system, expose the incentives, and build for the individual rather than the institution.

---

EXTRACTION METADATA
Project: The Restoration Project
Project type: Political publishing website / publication
Date: March 7, 2026
Agent: GPT-5 Codex
Access level: full codebase
Confidence level: medium — the repo strongly supports the project’s structure, language, timeline, and evolution, but the founder’s personal circumstances, standalone live URL, current live publication count, and post-February 4 maintenance status are not fully verifiable from local materials alone.
Sections with gaps: 1, 2, 4, 5, 6, 7
Narrative readiness: needs founder input

---
