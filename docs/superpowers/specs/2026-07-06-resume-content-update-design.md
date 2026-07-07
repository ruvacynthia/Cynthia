# Portfolio Resume Content Update — Design

## Context

The live portfolio (`ruvacynthia.github.io/Cynthia/`, built from `index.html` on `main`)
is out of date relative to Ruvarashe's current resume
(`~/Desktop/essentials/Ruvarashe Cynthia Mabika_Resume.pdf`). It has no Education or
Experience sections at all, and its Projects section lists a project not on the resume
while missing two that are. This change brings the site's content in line with the
resume without changing its visual design system (dark theme, purple/pink gradient,
Tailwind CDN, glowing-card style).

Work happens in a separate local clone (`~/Desktop/Cynthia-portfolio-update`) on branch
`resume-content-update`, delivered via a pull request against `main` — the live site is
not touched until the user reviews and merges.

## Scope

In scope:
- `index.html` content and nav updates (the live page)
- Deleting `ruva.html` (orphaned draft, unlinked from the live site)
- Compressing `images/ruva.jpg` (5.3MB → ~200-400KB, same photo, no visible quality
  change at its 256px display size)

Out of scope:
- Visual/layout redesign — current design system is kept as-is
- Real project links — buttons remain `#` placeholders (user has none to provide yet)
- Contact section changes — email-only stays, no phone number added

## Content Changes

### Hero
Subtitle changes from "MSc Data Analytics & Visualizations Student" to reflect the
current role, e.g. "Graduate Assistant & MSc Data Analytics Student".

### About
Rewrite the three paragraphs to mention the current Yeshiva Graduate Assistant role,
and broaden the named tools beyond "Python, SQL, Tableau, and Power BI" to include
Neo4j, R, and HubSpot.

### Education (new section)
Placed after About. Two entries, styled as glowing-cards matching the Projects section:

1. **Yeshiva University, Katz School of Science and Health** — New York, NY
   M.S. in Data Analytics and Visualization | GPA: 3.8 — *Expected December 2026*
2. **University of Zimbabwe** — Harare, Zimbabwe
   B.Sc. (Honors) in Data Science and Informatics — *June 2024*

### Experience (new section)
Placed after Education, before Skills. Four entries, most recent first, each a
glowing-card with title/org/location/dates and its resume bullets verbatim:

1. **Graduate Assistant** — Yeshiva University, Katz School, Admissions Office | New
   York, NY | *Sept 2025 – Present*
   - Managed 200+ daily applicant records in Recruit, maintaining end-to-end pipeline
     accuracy across all admissions funnel stages
   - Built a Neo4j-powered chatbot to handle 200+ daily student inquiries, reducing
     manual response load for the 8-person admissions team
   - Built monthly Tableau dashboards across 5+ departments to track admissions
     trends, informing marketing decisions and improving enrollment performance
   - Cleaned, validated, and standardized multi-source admissions datasets to ensure
     downstream reporting integrity and analytical accuracy
   - Monitored engagement signals across 500+ prospective students in HubSpot,
     identifying drop-off points to improve admissions conversion rates
2. **Data Analytics Intern (Volunteer)** — Jersey STEM | Jersey City, NJ | *June 2025 –
   Aug 2025*
   - Built weekly Looker Studio dashboards tracking progress across 30+ employer
     partners, giving program leads real-time visibility into participant activity
   - Used SQL Server Management Studio to query employee activity data and flag 50+
     inactive records, improving database hygiene and reporting accuracy
3. **Junior Data Analyst** — Dataal Africa | South Africa (Hybrid) | *June 2024 – Dec
   2024*
   - Led development of a tax fraud detection system for ZIMRA, building ML models to
     identify anomalous VAT and customs declarations across 10,000+ transaction
     records
   - Wrote complex SQL queries to extract and transform large datasets across 3+
     client projects, directly supporting product and business decisions
   - Performed EDA on marketing campaign datasets to surface performance trends and
     patterns, driving measurable strategy adjustments for clients
4. **Information Systems Intern** — CAFCA Pvt Ltd. | Zimbabwe | *Aug 2022 – Apr 2023*
   - Maintained the company Asset Register, ensuring accurate tracking of hardware and
     software inventory across departments

### Projects
Replace the current 3 cards with the resume's 4, buttons kept as `#` placeholders:

1. **Flight Delay and Cancellation Rate Analysis** — Tableau dashboard analyzing 500K+
   flight records to identify delay root causes and highest-risk carrier-route
   combinations by season.
2. **Customer Churn Prediction** — Binary classification model in Python
   (scikit-learn) on a 7,000+ record telecom dataset, using feature selection, SMOTE,
   87% accuracy with Random Forest.
3. **GenAI RAG Knowledge Lab** — Retrieval-Augmented Generation lab using Neo4j as a
   knowledge graph backend, enabling LLM-powered Q&A over structured course data for
   20+ students.
4. **Graph-Based Fraud Detection System** — Fraud detection system using Neo4j GDS,
   modeling transaction networks as graphs to identify suspicious entity relationships.

### Skills ticker
Add R, Neo4j, and HubSpot icons/entries alongside the existing set (Python, Pandas,
NumPy, Scikit-learn, PostgreSQL, MySQL, Tableau, Power BI, Git). Tools without a
natural icon (Jira, MS Excel, SSMS, A/B Testing) stay as text mentions in About rather
than being forced into the icon ticker.

## Navigation & Page Flow

Add "Education" and "Experience" links to the desktop nav, mobile menu, and footer.
New section order:

```
Hero → About → Education → Experience → Skills → Projects → Contact → Footer
```

## Housekeeping

- Delete `ruva.html` — an earlier draft, not linked from `index.html` or GitHub Pages
  config.
- Compress `images/ruva.jpg` from 5.3MB to a web-appropriate size (~200-400KB) using
  the same image, since it's rendered at a small (256px) circular avatar and the
  current file size is an unnecessary load-time cost.

## Delivery

1. Work on branch `resume-content-update` in `~/Desktop/Cynthia-portfolio-update`.
2. Commit changes with clear messages.
3. Push branch and open a PR against `main` on `ruvacynthia/Cynthia`.
4. User reviews the PR (and can preview via GitHub or locally) and merges when ready.
   `main` / the live GitHub Pages site is unaffected until merge.

## Testing / Verification

- Open `index.html` locally in a browser (or via a simple static server) and check:
  - New nav links scroll to the correct sections
  - Education and Experience sections render correctly on desktop and mobile widths
  - Projects section shows exactly the 4 resume projects
  - Skills ticker includes the added items and still scrolls smoothly
  - No console errors
  - `ruva.jpg` still displays correctly after compression
