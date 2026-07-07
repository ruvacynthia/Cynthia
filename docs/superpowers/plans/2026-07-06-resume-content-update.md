# Resume Content Update Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bring `index.html` (the live portfolio, served via GitHub Pages) in line with the current resume by adding Education and Experience sections, refreshing Projects and the Skills ticker, and updating Hero/About copy — without changing the existing visual design system.

**Architecture:** Single static HTML file (`index.html`) edited section by section. No build step, no JS framework — Tailwind via CDN `<script>` tag, vanilla JS for the mobile menu and ScrollReveal. Each task is a self-contained content change to that one file (plus two small repo-hygiene tasks: deleting an orphaned file and compressing an oversized image). There is no test runner; "testing" means grepping for expected markup and opening the file in a browser to visually confirm.

**Tech Stack:** Plain HTML, Tailwind CSS (CDN), ScrollReveal (CDN), vanilla JS. macOS `sips` for image compression. `git` + `gh` for branch/PR delivery.

## Global Constraints

- Repo: `~/Desktop/Cynthia-portfolio-update` (clone of `ruvacynthia/Cynthia`, kept separate from the live checkout).
- Branch: `resume-content-update` (already created and checked out; spec already committed on it).
- Do not touch `main` directly — all work lands via PR.
- Keep the existing dark theme / purple-pink gradient / glowing-card visual style. No redesign.
- New sections use the `glowing-card` component class already defined in the `<style>` block — do not invent new component styles.
- Section background alternation must stay consistent: About=`bg-gray-900`, Education=`bg-gray-950`, Experience=`bg-gray-900`, Skills=`bg-gray-950`, Projects=`bg-gray-900`, Contact=`bg-gray-950`.
- Project link buttons stay as `href="#"` placeholders (no real links available yet).
- No phone number added anywhere on the page.
- Source content (resume facts) is authoritative and must be copied verbatim from `docs/superpowers/specs/2026-07-06-resume-content-update-design.md` — do not paraphrase bullet points.

---

### Task 1: Update Hero subtitle and About copy

**Files:**
- Modify: `index.html:154` (hero subtitle), `index.html:178-186` (About paragraphs)

**Interfaces:** None — plain text/markup edit, no other task depends on exact wording here.

- [ ] **Step 1: Update the hero subtitle**

Find this exact line:
```html
            <p class="text-xl md:text-2xl text-gray-300 mb-8">MSc Data Analytics & Visualizations Student</p>
```
Replace with:
```html
            <p class="text-xl md:text-2xl text-gray-300 mb-8">Graduate Assistant &amp; MSc Data Analytics Student</p>
```

- [ ] **Step 2: Update the About paragraphs**

Find this exact block:
```html
                        <p>
                            I am a dedicated Master of Science student in Data Analytics and Visualization, honing my skills in extracting meaningful patterns from large datasets. My academic journey has built a strong foundation in statistical analysis, machine learning, and data visualization.
                        </p>
                        <p>
                            Proficient in <strong class="font-semibold text-purple-400">Python, SQL, Tableau, and Power BI</strong>, I have hands-on experience building analytical dashboards that transform complex data into clear, actionable insights. I thrive on solving real-world problems and am continuously exploring new technologies in the ever-evolving field of data science.
                        </p>
                        <p>
                            My goal is to leverage data to create impactful solutions and contribute to informed decision-making. I'm highly motivated and eager to learn new skills to deliver value through data.
                        </p>
```
Replace with:
```html
                        <p>
                            I am a Graduate Assistant at Yeshiva University's Katz School and a dedicated Master of Science student in Data Analytics and Visualization, honing my skills in extracting meaningful patterns from large datasets. My academic journey has built a strong foundation in statistical analysis, machine learning, and data visualization.
                        </p>
                        <p>
                            Proficient in <strong class="font-semibold text-purple-400">Python, SQL, R, Tableau, Power BI, and Neo4j</strong>, I have hands-on experience building analytical dashboards and graph-powered tools&mdash;plus working knowledge of HubSpot and Jira&mdash;that transform complex data into clear, actionable insights. I thrive on solving real-world problems and am continuously exploring new technologies in the ever-evolving field of data science.
                        </p>
                        <p>
                            My goal is to leverage data to create impactful solutions and contribute to informed decision-making. I'm highly motivated and eager to learn new skills to deliver value through data.
                        </p>
```

- [ ] **Step 3: Verify the edits landed**

Run:
```bash
grep -c "Graduate Assistant &amp; MSc Data Analytics Student" index.html
grep -c "Python, SQL, R, Tableau, Power BI, and Neo4j" index.html
```
Expected: both commands print `1`.

- [ ] **Step 4: Visually confirm**

Run: `open index.html`
Expected: Hero subtitle reads "Graduate Assistant & MSc Data Analytics Student"; About section mentions the Katz School role and the expanded tool list.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Update hero and about copy to reflect current role and tools"
```

---

### Task 2: Add Education section with nav links

**Files:**
- Modify: `index.html` — desktop nav (`index.html:122-127`), mobile nav (`index.html:138-143`), new `<section id="education">` inserted after the About section's closing `</section>` (originally `index.html:191`)

**Interfaces:**
- Produces: section anchor `#education`, nav links pointing to it (desktop + mobile). Task 3 will insert its own section immediately after this one.

- [ ] **Step 1: Add the desktop nav link**

Find:
```html
                    <a href="#about" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300">About</a>
                    <a href="#skills" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300">Skills</a>
```
Replace with:
```html
                    <a href="#about" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300">About</a>
                    <a href="#education" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300">Education</a>
                    <a href="#experience" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300">Experience</a>
                    <a href="#skills" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300">Skills</a>
```

- [ ] **Step 2: Add the mobile nav links**

Find:
```html
                <a href="#about" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">About</a>
                <a href="#skills" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">Skills</a>
```
Replace with:
```html
                <a href="#about" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">About</a>
                <a href="#education" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">Education</a>
                <a href="#experience" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">Experience</a>
                <a href="#skills" class="text-gray-300 hover:bg-gray-700 hover:text-white block px-3 py-2 rounded-md text-base font-medium">Skills</a>
```

- [ ] **Step 3: Insert the Education section**

Find the closing of the About section:
```html
                </div>
            </div>
        </div>
    </section>

    <section id="skills" class="py-24 bg-gray-950">
```
Replace with:
```html
                </div>
            </div>
        </div>
    </section>

    <section id="education" class="py-24 bg-gray-950">
        <div class="container mx-auto px-4">
            <div class="reveal">
                <h2 class="text-4xl font-bold mb-20 text-center text-white relative section-title">Education</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-4xl mx-auto">
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-1 text-white">Yeshiva University, Katz School of Science and Health</h3>
                            <p class="text-sm text-gray-500 mb-3">New York, NY</p>
                            <p class="text-gray-300">M.S. in Data Analytics and Visualization</p>
                            <p class="text-gray-400 text-sm mt-1">GPA: 3.8 &middot; Expected December 2026</p>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-1 text-white">University of Zimbabwe</h3>
                            <p class="text-sm text-gray-500 mb-3">Harare, Zimbabwe</p>
                            <p class="text-gray-300">B.Sc. (Honors) in Data Science and Informatics</p>
                            <p class="text-gray-400 text-sm mt-1">June 2024</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section id="skills" class="py-24 bg-gray-950">
```

Note: this temporarily leaves two adjacent sections both styled `bg-gray-950` (Education and Skills) — Task 3 fixes the alternation by inserting Experience (`bg-gray-900`) between them.

- [ ] **Step 4: Verify**

```bash
grep -c 'id="education"' index.html
grep -c 'href="#education"' index.html
```
Expected: `id="education"` → `1`, `href="#education"` → `2` (desktop + mobile nav).

- [ ] **Step 5: Visually confirm**

Run: `open index.html`
Expected: nav bar (desktop and mobile, resize window to check both) shows an "Education" link that scrolls to a new section listing both degrees.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "Add Education section with nav links"
```

---

### Task 3: Add Experience section with nav links and footer links

**Files:**
- Modify: `index.html` — desktop nav (link added in Task 2, insert Experience-specific bits here follow same pattern already done in Task 2's replacement — see note below), new `<section id="experience">` inserted between Education and Skills, footer nav links (`index.html:290-294`)

**Interfaces:**
- Consumes: `#education` section immediately preceding (from Task 2).
- Produces: section anchor `#experience`.

Note: the desktop/mobile nav links for both Education and Experience were already added together in Task 2, Steps 1–2. This task only adds the section markup and the footer links.

- [ ] **Step 1: Insert the Experience section**

Find:
```html
    </section>

    <section id="skills" class="py-24 bg-gray-950">
```
Replace with:
```html
    </section>

    <section id="experience" class="py-24 bg-gray-900">
        <div class="container mx-auto px-4">
            <div class="reveal">
                <h2 class="text-4xl font-bold mb-20 text-center text-white relative section-title">Experience</h2>
                <div class="max-w-4xl mx-auto space-y-8">
                    <div class="group glowing-card">
                        <div class="card-content">
                            <div class="flex flex-col md:flex-row md:justify-between md:items-baseline mb-3">
                                <h3 class="text-xl font-bold text-white">Graduate Assistant</h3>
                                <span class="text-sm text-gray-500">Sept 2025 &ndash; Present</span>
                            </div>
                            <p class="text-purple-400 text-sm mb-4">Yeshiva University, Katz School &mdash; Admissions Office | New York, NY</p>
                            <ul class="list-disc list-outside pl-5 space-y-2 text-gray-300 text-sm">
                                <li>Managed 200+ daily applicant records in Recruit, maintaining end-to-end pipeline accuracy across all admissions funnel stages</li>
                                <li>Built a Neo4j-powered chatbot to handle 200+ daily student inquiries, reducing manual response load for the 8-person admissions team</li>
                                <li>Built monthly Tableau dashboards across 5+ departments to track admissions trends, informing marketing decisions and improving enrollment performance</li>
                                <li>Cleaned, validated, and standardized multi-source admissions datasets to ensure downstream reporting integrity and analytical accuracy</li>
                                <li>Monitored engagement signals across 500+ prospective students in HubSpot, identifying drop-off points to improve admissions conversion rates</li>
                            </ul>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <div class="flex flex-col md:flex-row md:justify-between md:items-baseline mb-3">
                                <h3 class="text-xl font-bold text-white">Data Analytics Intern (Volunteer)</h3>
                                <span class="text-sm text-gray-500">June 2025 &ndash; Aug 2025</span>
                            </div>
                            <p class="text-purple-400 text-sm mb-4">Jersey STEM | Jersey City, NJ</p>
                            <ul class="list-disc list-outside pl-5 space-y-2 text-gray-300 text-sm">
                                <li>Built weekly Looker Studio dashboards tracking progress across 30+ employer partners, giving program leads real-time visibility into participant activity</li>
                                <li>Used SQL Server Management Studio to query employee activity data and flag 50+ inactive records, improving database hygiene and reporting accuracy</li>
                            </ul>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <div class="flex flex-col md:flex-row md:justify-between md:items-baseline mb-3">
                                <h3 class="text-xl font-bold text-white">Junior Data Analyst</h3>
                                <span class="text-sm text-gray-500">June 2024 &ndash; Dec 2024</span>
                            </div>
                            <p class="text-purple-400 text-sm mb-4">Dataal Africa | South Africa (Hybrid)</p>
                            <ul class="list-disc list-outside pl-5 space-y-2 text-gray-300 text-sm">
                                <li>Led development of a tax fraud detection system for ZIMRA, building ML models to identify anomalous VAT and customs declarations across 10,000+ transaction records</li>
                                <li>Wrote complex SQL queries to extract and transform large datasets across 3+ client projects, directly supporting product and business decisions</li>
                                <li>Performed EDA on marketing campaign datasets to surface performance trends and patterns, driving measurable strategy adjustments for clients</li>
                            </ul>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <div class="flex flex-col md:flex-row md:justify-between md:items-baseline mb-3">
                                <h3 class="text-xl font-bold text-white">Information Systems Intern</h3>
                                <span class="text-sm text-gray-500">Aug 2022 &ndash; Apr 2023</span>
                            </div>
                            <p class="text-purple-400 text-sm mb-4">CAFCA Pvt Ltd. | Zimbabwe</p>
                            <ul class="list-disc list-outside pl-5 space-y-2 text-gray-300 text-sm">
                                <li>Maintained the company Asset Register, ensuring accurate tracking of hardware and software inventory across departments</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section id="skills" class="py-24 bg-gray-950">
```

- [ ] **Step 2: Add footer nav links**

Find:
```html
                <div class="flex gap-6 text-sm text-gray-400">
                    <a href="#about" class="hover:text-purple-400 transition-colors">About</a>
                    <a href="#skills" class="hover:text-purple-400 transition-colors">Skills</a>
                    <a href="#projects" class="hover:text-purple-400 transition-colors">Projects</a>
                </div>
```
Replace with:
```html
                <div class="flex gap-6 text-sm text-gray-400">
                    <a href="#about" class="hover:text-purple-400 transition-colors">About</a>
                    <a href="#education" class="hover:text-purple-400 transition-colors">Education</a>
                    <a href="#experience" class="hover:text-purple-400 transition-colors">Experience</a>
                    <a href="#skills" class="hover:text-purple-400 transition-colors">Skills</a>
                    <a href="#projects" class="hover:text-purple-400 transition-colors">Projects</a>
                </div>
```

- [ ] **Step 3: Verify**

```bash
grep -c 'id="experience"' index.html
grep -c 'href="#experience"' index.html
grep -c 'Information Systems Intern' index.html
```
Expected: `id="experience"` → `1`, `href="#experience"` → `3` (desktop + mobile + footer), `Information Systems Intern` → `1`.

- [ ] **Step 4: Visually confirm**

Run: `open index.html`
Expected: page order is now Hero → About → Education → Experience → Skills → Projects → Contact → Footer. Footer links include Education and Experience. All 4 experience entries render with their bullets.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add Experience section with footer nav links"
```

---

### Task 4: Replace Projects section with resume's 4 projects

**Files:**
- Modify: `index.html` — projects grid (originally `index.html:229-263`)

**Interfaces:** None — self-contained content swap within the existing `<section id="projects">`.

- [ ] **Step 1: Replace the projects grid**

Find:
```html
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">Customer Churn Prediction</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Developed a hybrid ensemble learning classifier to handle imbalanced telecom data and predict customer churn effectively.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-purple-900/50 text-purple-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Python</span>
                                <span class="bg-purple-900/50 text-purple-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Scikit-learn</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-purple-600 transition duration-300 font-semibold text-sm">View on GitHub <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-4.3 1.4-4.3-2.5-6-3m12 5v-3.5c0-1 .1-1.4-.5-2 2.8-.3 5.5-1.4 5.5-6.1 0-1.3-.5-2.4-1.3-3.2.1-.3.5-1.5-.1-3.2 0 0-1.1-.3-3.5 1.3a12.3 12.3 0 0 0-6.2 0C6.5 2.8 5.4 3.1 5.4 3.1c-.6 1.7-.2 2.9-.1 3.2-.8.8-1.3 1.9-1.3 3.2 0 4.7 2.7 5.8 5.5 6.1-.6.5-.9 1.2-.9 2.2v3.5"/></svg></a>
                        </div>
                    </div>
                     <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">Flight Delay & Cancellation Dashboard</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Collaborated to create an interactive Tableau dashboard analyzing flight delays and cancellations, identifying key contributing factors.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-blue-900/50 text-blue-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Tableau</span>
                                <span class="bg-blue-900/50 text-blue-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">SQL</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-blue-600 transition duration-300 font-semibold text-sm">View Dashboard <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg></a>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">Chronic Disease Management System</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Designed a star schema data warehouse and developed ETL processes in PostgreSQL to manage and analyze patient data.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-green-900/50 text-green-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">PostgreSQL</span>
                                <span class="bg-green-900/50 text-green-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">ETL</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-green-600 transition duration-300 font-semibold text-sm">View on GitHub <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-4.3 1.4-4.3-2.5-6-3m12 5v-3.5c0-1 .1-1.4-.5-2 2.8-.3 5.5-1.4 5.5-6.1 0-1.3-.5-2.4-1.3-3.2.1-.3.5-1.5-.1-3.2 0 0-1.1-.3-3.5 1.3a12.3 12.3 0 0 0-6.2 0C6.5 2.8 5.4 3.1 5.4 3.1c-.6 1.7-.2 2.9-.1 3.2-.8.8-1.3 1.9-1.3 3.2 0 4.7 2.7 5.8 5.5 6.1-.6.5-.9 1.2-.9 2.2v3.5"/></svg></a>
                        </div>
                    </div>
                </div>
```
Replace with:
```html
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">Flight Delay and Cancellation Rate Analysis</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Engineered a Tableau dashboard analyzing 500K+ flight records to identify delay root causes and highest-risk carrier-route combinations by season.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-blue-900/50 text-blue-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Tableau</span>
                                <span class="bg-blue-900/50 text-blue-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">SQL</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-blue-600 transition duration-300 font-semibold text-sm">View Dashboard <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg></a>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">Customer Churn Prediction</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Built a binary classification model in Python (scikit-learn) on a 7,000+ record telecom dataset, using feature selection and SMOTE, achieving 87% accuracy with Random Forest.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-purple-900/50 text-purple-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Python</span>
                                <span class="bg-purple-900/50 text-purple-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Scikit-learn</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-purple-600 transition duration-300 font-semibold text-sm">View on GitHub <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-4.3 1.4-4.3-2.5-6-3m12 5v-3.5c0-1 .1-1.4-.5-2 2.8-.3 5.5-1.4 5.5-6.1 0-1.3-.5-2.4-1.3-3.2.1-.3.5-1.5-.1-3.2 0 0-1.1-.3-3.5 1.3a12.3 12.3 0 0 0-6.2 0C6.5 2.8 5.4 3.1 5.4 3.1c-.6 1.7-.2 2.9-.1 3.2-.8.8-1.3 1.9-1.3 3.2 0 4.7 2.7 5.8 5.5 6.1-.6.5-.9 1.2-.9 2.2v3.5"/></svg></a>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">GenAI RAG Knowledge Lab</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Co-built a Retrieval-Augmented Generation lab using Neo4j as a knowledge graph backend, enabling LLM-powered Q&amp;A over structured course data for 20+ students.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-green-900/50 text-green-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Neo4j</span>
                                <span class="bg-green-900/50 text-green-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">RAG</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-green-600 transition duration-300 font-semibold text-sm">View on GitHub <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-4.3 1.4-4.3-2.5-6-3m12 5v-3.5c0-1 .1-1.4-.5-2 2.8-.3 5.5-1.4 5.5-6.1 0-1.3-.5-2.4-1.3-3.2.1-.3.5-1.5-.1-3.2 0 0-1.1-.3-3.5 1.3a12.3 12.3 0 0 0-6.2 0C6.5 2.8 5.4 3.1 5.4 3.1c-.6 1.7-.2 2.9-.1 3.2-.8.8-1.3 1.9-1.3 3.2 0 4.7 2.7 5.8 5.5 6.1-.6.5-.9 1.2-.9 2.2v3.5"/></svg></a>
                        </div>
                    </div>
                    <div class="group glowing-card">
                        <div class="card-content">
                            <h3 class="text-xl font-bold mb-2 text-white">Graph-Based Fraud Detection System</h3>
                            <p class="text-gray-400 mb-4 text-sm h-20">Building a fraud detection system with Neo4j GDS, modeling transaction networks as graphs to identify suspicious entity relationships and anomalous patterns.</p>
                            <div class="flex flex-wrap gap-2 mb-4">
                                <span class="bg-pink-900/50 text-pink-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Neo4j GDS</span>
                                <span class="bg-pink-900/50 text-pink-300 text-xs font-semibold px-2.5 py-0.5 rounded-full">Graph Analytics</span>
                            </div>
                            <a href="#" class="inline-flex items-center gap-2 bg-gray-800 text-white py-2 px-4 rounded-full hover:bg-pink-600 transition duration-300 font-semibold text-sm">View on GitHub <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-4.3 1.4-4.3-2.5-6-3m12 5v-3.5c0-1 .1-1.4-.5-2 2.8-.3 5.5-1.4 5.5-6.1 0-1.3-.5-2.4-1.3-3.2.1-.3.5-1.5-.1-3.2 0 0-1.1-.3-3.5 1.3a12.3 12.3 0 0 0-6.2 0C6.5 2.8 5.4 3.1 5.4 3.1c-.6 1.7-.2 2.9-.1 3.2-.8.8-1.3 1.9-1.3 3.2 0 4.7 2.7 5.8 5.5 6.1-.6.5-.9 1.2-.9 2.2v3.5"/></svg></a>
                        </div>
                    </div>
                </div>
```

- [ ] **Step 2: Verify**

```bash
grep -c "Chronic Disease Management System" index.html
grep -c "GenAI RAG Knowledge Lab" index.html
grep -c "Graph-Based Fraud Detection System" index.html
grep -c "Flight Delay and Cancellation Rate Analysis" index.html
```
Expected: `Chronic Disease Management System` → `0` (removed), the other three → `1` each.

- [ ] **Step 3: Visually confirm**

Run: `open index.html`
Expected: Projects section shows exactly 4 cards, in order: Flight Delay, Customer Churn, GenAI RAG Knowledge Lab, Graph-Based Fraud Detection.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Replace Projects section with resume's 4 current projects"
```

---

### Task 5: Add R and Neo4j to the Skills ticker

**Files:**
- Modify: `index.html` — both copies of the skills ticker list (originally `index.html:199-207` and `index.html:210-218`; the second is an `aria-hidden="true"` duplicate used for the seamless scroll animation, so it must get the identical addition)

**Interfaces:** None.

- [ ] **Step 1: Add R and Neo4j after Git in the first (visible) list**

Find:
```html
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" class="h-16 mx-auto mb-2" alt="Git"/>Git</div>
                    </div>
                    <div class="flex w-max items-center justify-center animate-scroll" aria-hidden="true">
```
Replace with:
```html
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" class="h-16 mx-auto mb-2" alt="Git"/>Git</div>
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/r/r-original.svg" class="h-16 mx-auto mb-2" alt="R"/>R</div>
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/neo4j/neo4j-original.svg" class="h-16 mx-auto mb-2" alt="Neo4j"/>Neo4j</div>
                    </div>
                    <div class="flex w-max items-center justify-center animate-scroll" aria-hidden="true">
```

- [ ] **Step 2: Add R and Neo4j after Git in the second (duplicate, `aria-hidden`) list**

Find (this is the last item of the duplicate list, immediately before its closing tags):
```html
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" class="h-16 mx-auto mb-2" alt="Git"/>Git</div>
                    </div>
                </div>
            </div>
        </div>
    </section>
```
Replace with:
```html
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" class="h-16 mx-auto mb-2" alt="Git"/>Git</div>
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/r/r-original.svg" class="h-16 mx-auto mb-2" alt="R"/>R</div>
                        <div class="mx-6 text-center text-gray-400"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/neo4j/neo4j-original.svg" class="h-16 mx-auto mb-2" alt="Neo4j"/>Neo4j</div>
                    </div>
                </div>
            </div>
        </div>
    </section>
```

- [ ] **Step 3: Verify**

```bash
grep -c 'alt="R"' index.html
grep -c 'alt="Neo4j"' index.html
```
Expected: both → `2` (one in the visible ticker copy, one in the duplicate).

- [ ] **Step 4: Visually confirm**

Run: `open index.html`
Expected: Skills ticker scrolls smoothly with no visible seam, and R / Neo4j icons appear (with labels) alongside the existing tools.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add R and Neo4j to skills ticker"
```

---

### Task 6: Delete orphaned ruva.html

**Files:**
- Delete: `ruva.html`

**Interfaces:** None.

- [ ] **Step 1: Confirm nothing references it**

```bash
grep -rn "ruva.html" index.html README.md
```
Expected: no output (no matches).

- [ ] **Step 2: Delete the file**

```bash
git rm ruva.html
```

- [ ] **Step 3: Verify**

```bash
ls ruva.html
```
Expected: `ls: ruva.html: No such file or directory`

- [ ] **Step 4: Commit**

```bash
git commit -m "Remove orphaned ruva.html draft"
```

---

### Task 7: Compress the profile photo

**Files:**
- Modify (binary): `images/ruva.jpg` (currently 4284x5712px, 5.3MB)

**Interfaces:** None — same filename, same aspect ratio, only pixel dimensions and encoding change. `index.html` already references `images/ruva.jpg` and needs no edit.

- [ ] **Step 1: Resize and compress in place**

```bash
cd ~/Desktop/Cynthia-portfolio-update
sips -Z 900 -s formatOptions 80 images/ruva.jpg
```
(`-Z 900` caps the longest edge at 900px — comfortably sharp for a 256px circular avatar even at 2x pixel density; `-s formatOptions 80` sets JPEG quality to 80.)

- [ ] **Step 2: Verify size and dimensions**

```bash
ls -la images/ruva.jpg
sips -g pixelWidth -g pixelHeight images/ruva.jpg
```
Expected: file size well under 1MB (roughly 100-400KB); `pixelHeight` at or near 900 (longest edge), width scaled proportionally.

- [ ] **Step 3: Visually confirm**

Run: `open index.html`
Expected: the About section avatar photo still displays correctly with no visible quality loss at its rendered size.

- [ ] **Step 4: Commit**

```bash
git add images/ruva.jpg
git commit -m "Compress profile photo from 5.3MB to web-appropriate size"
```

---

### Task 8: Push branch and open PR

**Files:** None (delivery step only).

**Interfaces:** None.

- [ ] **Step 1: Push the branch**

```bash
cd ~/Desktop/Cynthia-portfolio-update
git push -u origin resume-content-update
```

- [ ] **Step 2: Open the PR**

```bash
gh pr create --repo ruvacynthia/Cynthia \
  --title "Update portfolio content to match current resume" \
  --body "$(cat <<'EOF'
## Summary
- Add Education and Experience sections (previously missing entirely)
- Refresh Projects to match resume's current 4 projects (dropped an outdated project, added GenAI RAG Knowledge Lab and Graph-Based Fraud Detection System)
- Update Hero/About copy to reflect current Graduate Assistant role and full tool set (added R, Neo4j; mentioned HubSpot/Jira)
- Add R and Neo4j to the Skills ticker
- Remove orphaned ruva.html draft
- Compress profile photo (5.3MB -> under 1MB) for faster page load

No visual/layout redesign — existing dark theme and glowing-card style preserved throughout.

## Test plan
- [x] Opened index.html locally and clicked every nav link (desktop + mobile) to confirm it scrolls to the right section
- [x] Confirmed Education and Experience sections render all resume content correctly
- [x] Confirmed Projects section shows exactly the 4 current resume projects
- [x] Confirmed Skills ticker scrolls smoothly with R and Neo4j included
- [x] Confirmed profile photo still displays correctly after compression
EOF
)"
```

- [ ] **Step 3: Verify the PR was created**

```bash
gh pr view --repo ruvacynthia/Cynthia --web=false
```
Expected: shows the PR with branch `resume-content-update` targeting `main`, status OPEN.

- [ ] **Step 4: Report the PR URL to the user**

No commit needed for this step — just confirm the PR URL is visible in the previous command's output and share it with the user for review/merge.
