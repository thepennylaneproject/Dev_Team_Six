# Dashboard — Sarah's Medical Dashboard — Expectations Document

> Source: `the_penny_lane_project/dashboard/dashboard_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 Python
The dashboard is built in Python. File `warning` for any non-Python file added to the application source.

### 1.2 Streamlit framework
The UI is built with Streamlit. File `warning` for any replacement of Streamlit with another framework.

### 1.3 Core dependencies: pandas, Plotly, pdfplumber
The data processing and visualization stack is `pandas`, `Plotly`, and `pdfplumber`. Additional dependencies require explicit approval. File `suggestion` for any new dependency added without documentation.

---

## 2. Privacy and Deployment

### 2.1 Personal health data — no public cloud deployment without owner approval
This application processes personal/private health data belonging to Sarah Sahl. It must **not** be deployed to any public cloud service, shared hosting, or publicly accessible URL without explicit written approval from the owner. File `critical` for any deployment configuration (Dockerfile, `requirements.txt` with cloud SDK, `netlify.toml`, `vercel.json`, CI deploy step, etc.) added without documented owner approval.

### 2.2 No external API calls or data sharing
The dashboard must not make outbound API calls or transmit health data to any external service. File `critical` for any external HTTP call or data export to a third-party service.

---

## 3. ETL Pipeline Constraints

### 3.1 PDF extraction outputs must write to a consistent directory
`run_all.py` and the working CSV files must write outputs to the same, consistent directory. The current misalignment between `run_all.py` output paths and the working CSVs must be resolved before the ETL pipeline is considered reliable. File `warning` for any output path inconsistency between `run_all.py` and downstream scripts.

### 3.2 Placeholder dates must be resolved before ETL is considered reliable
Placeholder dates (e.g., `2020-09-XX`) in `extract_events.py` must be resolved with real data or marked explicitly as unknown. File `warning` for any unresolved placeholder date that persists in the ETL pipeline.

---

## 4. Clinical Data Integrity

### 4.1 Hardcoded clinical claims must be marked
Any hardcoded clinical claim (thresholds, reference ranges, diagnostic values) in the code must be marked with a comment: `# [HARDCODED — requires data verification]`. File `warning` for any hardcoded clinical value without this comment.

---

## 5. User Experience

### 5.1 Unimplemented navigation sections must not be presented as functional
The five unimplemented navigation sections of the dashboard must display a clear "Not yet implemented" message rather than appearing as functional features to the user. File `warning` for any unimplemented nav section that loads without a clear placeholder message.

---

## 6. Out-of-Scope Constraints

- Do not integrate any analytics, tracking, or telemetry
- Do not add user accounts or multi-user access without explicit approval
- Do not modify the core health data schema without owner review
