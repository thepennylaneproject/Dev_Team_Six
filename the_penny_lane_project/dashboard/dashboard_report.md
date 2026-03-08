## SECTION 1: PROJECT BASICS

1. **Project name**: Sarah's Medical Dashboard [VERIFIED from app title in [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):14,75]
2. **Project type**: Personal medical dashboard and record-extraction toolkit built in Streamlit [VERIFIED from [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):1-3 and the extraction scripts]
3. **URL**: [REQUIRES MANUAL INPUT] No live URL or deployment config was found in the local project.
4. **Date range**: Earliest local authored file timestamps are March 2, 2026; package install activity appears on March 7, 2026 [VERIFIED from local file mtimes]. Whether it began earlier in another location or is still actively maintained beyond that is [REQUIRES MANUAL INPUT].
5. **Current state**: In-progress local prototype, not polished or fully complete [VERIFIED]. The app has eight navigation labels, but only three pages are implemented, and two generated CSVs are currently empty [VERIFIED from [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):61-70, 375-376 and local CSV contents].
6. **One-paragraph description**: This is a personal health dashboard built to turn scattered medical records into a clear, specialist-ready case presentation. It combines PDF extraction scripts, CSV outputs, and a Streamlit interface to summarize lab trends, cardiac decline, eosinophil progression, organ involvement, and treatment context for a Mayo Clinic consultation. The project is less a consumer health app than a self-advocacy tool: a way to force coherence onto a medical history that the healthcare system presents in fragments. [VERIFIED + INFERRED from code structure and copy]

## SECTION 2: THE "WHY" — ORIGIN & MOTIVATION

1. **What problem was this built to solve for the creator personally?** It appears to solve a patient-side evidence problem: how to assemble years of fragmented records into a persuasive, legible timeline that a specialist cannot easily dismiss. [INFERRED from the extraction pipeline, “Executive Summary,” and urgency framing in [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):73-176]
2. **What circumstances led to its creation?** Health crisis is the clear driver. The code references severe hypereosinophilic syndrome, TTN cardiomyopathy, kidney disease, disability-related job loss, an ER visit described as “dismissed,” and a Mayo Clinic appointment on April 1, 2026. [VERIFIED from [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):75-76, 136-175 and [extract_events.py](/Users/sarahsahl/Desktop/medical_dashboard/extract_events.py):24-39]
3. **What was the creator's situation when they started building this?** The project suggests someone under time pressure, dealing with escalating multi-system illness, preparing for high-stakes specialty review, and no longer trusting raw institutional records to tell the story accurately on their own. [INFERRED]
4. **Did the scope or purpose evolve over time?** There are signs of scope expansion from “extract records” to “argue the case.” The repo contains parser scripts, derived CSVs, and a narrative-heavy dashboard with executive summary, urgency framing, family comparison, and statistical claims. Without git history, the exact sequence is [REQUIRES MANUAL INPUT], but the structure strongly suggests a shift from record organization to strategic medical advocacy. [INFERRED]
5. **What tension or frustration does this project respond to?** The underlying frustration appears to be that patients are expected to navigate serious disease with unusable data, fragmented records, and clinicians who may under-read patterns unless they are explicitly visualized and framed. [INFERRED]

## SECTION 3: SKILLS & CAPABILITIES DEMONSTRATED

1. **Technical skills on display**: Python for the app and extraction scripts; Streamlit for dashboard UI; pandas for tabular transformation; Plotly for charting; `pdfplumber` and regex for mining values from raw PDFs; lightweight statistical analysis via SciPy linear regression; basic ETL orchestration in `run_all.py`. [VERIFIED]
2. **Non-technical skills on display**: Clinical synthesis, argument framing, information design, pattern recognition across longitudinal records, prioritization under urgency, and the ability to translate personal crisis into a structured decision-support artifact. [INFERRED from the dashboard copy and layout]
3. **Complexity level**: Moderate, but meaningful. Technically it is a small single-user app, not a platform. What makes it complex is the problem domain: turning heterogeneous PDFs and partial data into a coherent medical narrative with timelines, comparison logic, and specialist-facing framing. [VERIFIED + INFERRED]
4. **Most impressive element**: The combination of extraction plus interpretation. The standout is not the UI itself, but the attempt to convert raw records into a defensible clinical story with trend lines, reference comparisons, and urgency signals. That is systems thinking applied to a domain where ambiguity has real consequences. [INFERRED]
5. **Design and brand sensibility**: Functional, direct, and evidence-first. The visual language favors clear cards, clinical color coding, and high-information hierarchy over decorative design. It signals someone who optimizes for comprehension and leverage, not polish for its own sake. [VERIFIED + INFERRED from [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):20-47, 80-124]

## SECTION 4: CONTENT & SUBSTANCE INVENTORY

1. **Data sources**: Ten local medical PDFs in `/raw_pdfs`, plus generated CSVs for labs, medications, and timeline events. The extractor currently produces ten lab rows in [extracted_labs.csv](/Users/sarahsahl/Desktop/medical_dashboard/extracted_labs.csv), while [medications.csv](/Users/sarahsahl/Desktop/medical_dashboard/medications.csv) and [timeline_events.csv](/Users/sarahsahl/Desktop/medical_dashboard/timeline_events.csv) are empty. [VERIFIED]
2. **Key features**: An executive summary with high-risk metrics; a cardiac progression chart comparing patient vs. father; an eosinophil trend page with statistical framing; an organ involvement table; and scripts intended to automate lab, medication, and timeline extraction. [VERIFIED]
3. **User experience**: Designed primarily for personal use in a medical advocacy context, though the pattern could be generalized into a broader patient evidence dashboard. The current implementation is clearly single-user and case-specific. [VERIFIED + INFERRED]
4. **Data insights**: The strongest surfaced insight is longitudinal trend compression: eosinophils rising from baseline to crisis levels, EF decline framed against family history, and multi-organ involvement rendered as a single systems picture rather than isolated incidents. Some of these are hardcoded narrative claims rather than fully data-driven outputs in the current build. [VERIFIED]
5. **What materially exists**: Three implemented pages, ten source PDFs, three extraction scripts, one orchestration script, and CSV outputs. Five additional nav sections are named but not implemented. [VERIFIED]

## SECTION 5: THE THROUGH-LINE

1. **Shared DNA**: Based on the portfolio framing in the prompt, the shared DNA appears to be user advocacy, turning opaque systems into usable interfaces, and building tools that restore agency to the person forced to navigate the system. [INFERRED from this repo + prompt context]
2. **Evolution markers**: This looks like an earlier, more personal form of the same capability: ingest messy institutional data, structure it, surface the hidden pattern, and build an interface that helps an individual act. In a larger portfolio, that becomes product strategy; here, it is survival strategy. [INFERRED]
3. **Narrative bridge**: Building this project appears to have taught the same lesson that drives the later work: institutions routinely hold the data but fail the individual. This dashboard is what that lesson looks like at human scale, before it becomes a broader platform thesis. [INFERRED]
4. **Thematic connection**: The shared worldview is that systems are optimized for institutional convenience, not for the person bearing the consequences, and that software can rebalance that power by making complexity legible to the individual. [INFERRED]

## SECTION 6: USABLE ASSETS

1. **Quotable lines**:
   - “**URGENT INTERVENTION NEEDED**” [VERIFIED, [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):165]
   - “This is NOT ‘test variability.’ This is documented progressive disease.” [VERIFIED, [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):323-325]
   - “The Smoking Gun of Hypereosinophilic Syndrome” [VERIFIED, [medical_dashboard.py](/Users/sarahsahl/Desktop/medical_dashboard/medical_dashboard.py):254-255]
2. **Visual assets**: Screenshots of the executive summary cards, the EF comparison chart, the eosinophil trend chart, and the organ involvement table would all be useful. No standalone design assets or logos were found. [VERIFIED]
3. **Data points**:
   - Ten raw medical PDFs in the repository [VERIFIED]
   - Ten extracted lab rows [VERIFIED]
   - Hardcoded core claims in the app include eosinophils `3% -> 15%`, EF `56% -> 50%`, eGFR `73`, and `8+` organs affected [VERIFIED as present in code, not fully verified clinically]
4. **Proof of work**: The strongest proof is the existence of a custom extraction pipeline plus a dashboard assembled in a narrow time window around a specialist consultation. What cannot be shown here is sustained commit history, because no `.git` history is present in this workspace. [VERIFIED]

## SECTION 7: HONEST ASSESSMENT

1. **What this project does well**: It compresses a complicated medical history into an intelligible case, shows initiative under pressure, and demonstrates the instinct to build tooling when institutions fail to provide usable interfaces.
2. **What it doesn't do**: It is not a polished product, not broadly reusable in its current form, and not yet fully reliable as software. `run_all.py` writes to `raw_data`, but the working CSVs are in the repo root; the medication pipeline depends on pharmacy PDFs that are not present; and the timeline builder uses placeholder dates like `2020-09-XX`, which likely breaks parsing. [VERIFIED from [run_all.py](/Users/sarahsahl/Desktop/medical_dashboard/run_all.py):20-21,42,76,112 and [extract_events.py](/Users/sarahsahl/Desktop/medical_dashboard/extract_events.py):24,26,28,33-39]
3. **What it signals to an investor**: The signal is not “venture-ready health startup.” The signal is founder behavior: high agency, systems thinking, willingness to learn tools quickly, and the instinct to create order from institutional chaos when the stakes are personal and real.
4. **Risk of inclusion**: The main risk is that the project is intimate, unfinished, and medically specific; presented badly, it could read as personal documentation rather than founder evidence. The mitigation is to frame it as an origin artifact: a concrete example of how necessity sharpened the founder’s core pattern of building tools that return power to the individual. [INFERRED]

## SECTION 8: NARRATIVE SUMMARY

**Version A — First person (founder voice):**  
I built this when my medical records were too fragmented to tell the truth about what was happening to me. I needed a way to pull labs, imaging, family history, and treatment failure into one place and make the pattern impossible to ignore before a high-stakes specialist appointment. It wasn’t a product exercise; it was a tool for self-advocacy under pressure. Looking back, it also made something obvious: I keep building software in places where institutions expect people to navigate complexity without real leverage.

**Version B — Third person (site copy voice):**  
Sarah's Medical Dashboard began as a personal necessity: a way to convert years of scattered records into a coherent evidence system during a worsening health crisis. The project pairs lightweight extraction tooling with a tightly argued dashboard designed to surface patterns that institutional workflows left buried. It is an early expression of the same builder instinct behind the broader portfolio: take opaque, high-stakes systems and make them legible to the person forced to live inside them. In that sense, it is less an outlier than a source code artifact for the larger mission.

---

EXTRACTION METADATA  
Project: Sarah's Medical Dashboard  
Project type: Personal medical dashboard and record-extraction toolkit  
Date: 2026-03-07  
Agent: GPT-5 Codex  
Access level: full codebase  
Confidence level: medium, because the local codebase clearly establishes purpose, scope, and current state, but there is no git history, README, live URL, or founder-authored narrative explaining origin directly.  
Sections with gaps: 1, 2, 5, 6, 7  
Narrative readiness: needs founder input

---
