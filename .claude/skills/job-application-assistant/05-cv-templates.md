---
framework_version: 2.0.0
---

# CV Templates and Tailoring Guide

<!-- SETUP: Summary bullets and section content are personalized by running /setup -->

## Base content source & selection rules (read FIRST)

**Every tailored CV is built from one base file: `documents/cv/master_resume_consolidated.md`.** It holds the full, canonical resume content (summary framings, skills, experience bullets). This `05` file governs *style/layout*; the base file governs *what content exists*. Do not invent content absent from the base file or `01-candidate-profile.md`.

### The base file's annotations are drafter directives - never rendered

The base file tags items with parentheticals and category markers meant for **you, the drafter**. **Strip all of them from the generated CV** - no `(Optional...)`, `(mandatory)`, `[General]`, or `[Data]` marker may ever appear in the output.

- **Untagged item/bullet = MANDATORY.** Always carry it into every tailored CV (e.g. a core-responsibility or flagship-project bullet, earlier-role bullets, Education, and any untagged core skills).
- **`(Optional...)` item/bullet = CONDITIONAL.** Include it when the JD supports it:
  - the JD **names** the skill/tech (JD requires Django or Go -> include it), OR
  - your **analysis of the JD's focus** supports it (a data-engineering / data-infrastructure-leaning role -> pull in `Spark`/`PySpark`, `Hadoop`, `Great Expectations`, `Go`; a backend role -> the backend-framed bullets). Honor any usage hint inside the parenthesis ("Use it for Data oriented role").
- **`[General]` / `[Data]` are preliminary categories, not fixed lanes.** A real posting can lean both - pick the framing matching the role, or blend/restructure across them.

### Building the content from the base file

1. **Summary:** choose ONE framing (backend vs data) matching the role's lean, or blend for a hybrid role. Output exactly 3 summary bullets.
2. **Skills:** all mandatory items + the optional items the JD supports. Keep the base file's category structure (Languages / Frameworks & Libraries / Platforms & Tools / Databases & Storage / AI coding tools / Certifications). Reorder items so the posting's core stack leads. Never add any credential the profile marks as CV-excluded.
3. **Experience:** all mandatory bullets + optional bullets selected for the JD. Where an achievement has `[General]`/`[Data]` variants, pick the best or **merge into one bullet**. You are **encouraged to restructure and reword optional bullets** to match the JD's language and priorities - as long as every fact still traces to the base file / `01-candidate-profile.md` (reframe emphasis, never fabricate).
4. **Education:** mandatory, unchanged. Never add a degree the profile marks as CV-excluded.

### Output

- A **2-page PDF** in the template style below.
- The **deliverable** is written to the target position's application directory as **`documents/applications/<company>_<role>/[YOUR_NAME]_CV.pdf`** (`/apply` compiles the working `.tex` in `cv/`, then delivers the final PDF to the app dir named for the candidate).

## Template: Clean single-column resume (Carlito / Calibri-style)

CVs use a lightweight `article`-based LaTeX template in a clean single-column style: a centered blue name, a plain pipe-separated contact line, blue ALL-CAPS section headings (no rules), and single-column body text. Single-column plain text is also the most ATS-friendly layout.

**Output file:** `cv/main_<company>_<role>.tex`
**Compile with:** **lualatex** (the template uses the `carlito` font package; lualatex handles it cleanly and matches the rest of the toolchain).
**Master reference:** `cv/main_example.tex` (comprehensive CV with all competencies, experience, and achievements - use as the source when building targeted CVs).

### Compile command

```bash
cd cv && lualatex -interaction=nonstopmode main_<company>_<role>.tex
```

Expected output: `Output written on main_<company>_<role>.pdf (2 pages, ...)`. Any page count other than 2 is a failure that must be fixed before presenting to the user.

**Font/package dependencies:** `carlito`, `enumitem`, `titlesec`, `needspace` (all installed via `tlmgr` in this fork). If `carlito` is ever unavailable, the closest fallbacks are `lato` or, worst case, `\usepackage{helvet}\renewcommand\familydefault{\sfdefault}` - never fall back to the default Computer Modern serif, which breaks the look.

## Document Structure

```latex
\documentclass[11pt,letterpaper]{article}

\usepackage[T1]{fontenc}
\usepackage[utf8]{inputenc}
\usepackage{carlito}                 % Calibri-metric font
\renewcommand{\familydefault}{\sfdefault}

\usepackage[letterpaper,top=0.7in,bottom=0.7in,left=0.8in,right=0.8in]{geometry}
\usepackage[dvipsnames]{xcolor}
\definecolor{cvblue}{HTML}{2E5496}
\usepackage{titlesec}
\usepackage{enumitem}
\usepackage{needspace}
\usepackage{hyperref}
\hypersetup{colorlinks=true, urlcolor=cvblue, linkcolor=cvblue, pdftitle={[YOUR_NAME] - CV}}

% --- Vertical rhythm: TUNE these per version so the finalized content fills
% --- two full pages (see "Two-page balance" below). More content -> smaller
% --- values (linespread ~1.05, parskip ~4pt); less content -> larger values
% --- (linespread up to ~1.18, parskip up to ~7pt, itemsep ~4.5pt, wider margins).
\linespread{1.08}
\titleformat{\section}{\color{cvblue}\bfseries\large}{}{0pt}{}
\titlespacing*{\section}{0pt}{11pt}{5pt}
\setlist[itemize]{leftmargin=1.5em, topsep=3pt, itemsep=2.5pt, parsep=0pt, label=\textbullet}
\setlength{\parindent}{0pt}
\setlength{\parskip}{5pt}
\pagestyle{empty}

\newcommand{\cvname}[1]{\begin{center}{\color{cvblue}\bfseries\fontsize{20}{24}\selectfont #1}\end{center}}
\newcommand{\cvcontact}[1]{\begin{center}#1\end{center}\vspace{-2pt}}

\begin{document}

\cvname{[YOUR_NAME]}
\cvcontact{[Address] | [Phone] | [Email] \\
\href{[LinkedIn URL]}{[linkedin.com/in/handle]} | \href{[GitHub URL]}{[github.com/handle]} | \href{[Website URL]}{[website]}}

\section*{SUMMARY}
\begin{itemize}
\item [Summary bullet 1 - tailored to the role]
\item [Summary bullet 2]
\item [Summary bullet 3]
\end{itemize}

\section*{TECHNICAL SKILLS}
\begin{itemize}
\item Languages: ...
\item Frameworks \& Libraries: ...
\item Platforms \& Tools: ...
\item Databases \& Storage: ...
\item AI coding tools: ...
\item Certifications: ...
\end{itemize}

\section*{PROFESSIONAL EXPERIENCE}

\needspace{5\baselineskip}
\textbf{[Company],} [Division] --- [City, State] | [YYYY.MM -- Present/YYYY.MM]

[Role Title] | [YYYY.MM -- Present/YYYY.MM]
\begin{itemize}
\item [Achievement/responsibility, specific, with numbers where possible]
\end{itemize}

% A second role at the SAME company: another role line + its own bullets,
% under the same company header (no repeated company line).

\section*{EDUCATION}

[Institution] --- [Degree], [GPA/details] | [Month Year]

\end{document}
```

### Formatting conventions (match these exactly)

- **Name:** centered, blue (`cvblue`), bold, ~20pt.
- **Contact block:** two centered lines. Line 1 = `Address | Phone | Email` (plain text, no icons — email/phone stay literal for ATS). Line 2 = LinkedIn, GitHub, and personal website as **short clickable text** (drop the `https://www.` prefix, keep each hyperlinked in `cvblue`), pipe-separated. The short link text is literal (ATS-readable) and clickable.
- **Section headings:** `\section*{...}` in ALL CAPS, rendered blue/bold by the `\titleformat` above. No horizontal rule. The four standard sections are **SUMMARY, TECHNICAL SKILLS, PROFESSIONAL EXPERIENCE, EDUCATION** - no Languages or References section.
- **Separators:** company/education lines use ` --- ` (em dash) before the location, and ` | ` before the date range. Date ranges use ` -- ` (en dash), e.g. `2022.10 -- Present`. These structural separators are the one place em/en dashes are correct; the no-em-dash rule in `03-writing-style.md` governs prose sentences, not these header separators.
- **Company header:** bold company short name with a trailing comma inside the bold (`\textbf{Acme Inc.,}`), then the rest of the line in regular weight.
- **Role sub-line:** regular weight, its own date range. Multiple roles at one company each get their own role line + bullets under a single company header (see the master example).

### Section language must match the CV's language

If the CV language (see `CV language` in CLAUDE.md's Identity section) is not English, translate the four section headings too (e.g. Spanish: `RESUMEN`, `COMPETENCIAS TÉCNICAS`, `EXPERIENCIA PROFESIONAL`, `EDUCACIÓN`), not just the body prose. A localized body under English headings reads as sloppy. Check this in the verification pass.

## Section-by-Section Tailoring

### Summary (3 bullets - the most important section to tailor)

Three bullets, each 1-2 lines, that function as an elevator pitch for *this specific role*. Lead the first bullet with the identity that matches the posting (e.g. "Senior Backend Engineer", "Senior Full-Stack Engineer"). Focus on what the employer gains. When the role sits outside the home domain, lead with the domain-transfer argument in the first bullet.

<!-- SETUP: /setup populates one or more role-type summary templates from the candidate's background. -->
Keep 2-3 summary templates for your main role types. Each is 3 bullets; fill from your profile:

**For [YOUR_PRIMARY_ROLE_TYPE] roles:**
> - [Identity + years of experience + domain, tailored to this role type]
> - [Core strengths: the architectures / systems / methods you build]
> - [Signature expertise + an end-to-end ownership statement]

**For [YOUR_SECONDARY_ROLE_TYPE] roles:**
> - [Alternate framing for a different role lean]
> - [Core strengths for that lean]
> - [Signature expertise for that lean]

Statements labeled *[Used for: <company>_<role>]* were extracted from archived drafts by `/setup` Path A. They are **phrasing references, never fact sources**: every factual claim still comes from `01-candidate-profile.md`.

### Technical Skills (category bullets)

One bullet per category, label then colon then comma-separated items (labels are **not** bold, matching the candidate's style). Reorder categories and items so the posting's core stack appears first. Use the posting's own term over a synonym when it is truthfully applicable (ATS matches literally). Standard categories: `Languages`, `Frameworks & Libraries`, `Platforms & Tools`, `Databases & Storage`, `AI coding tools`, `Certifications`. Include only certifications the profile lists for CV use; some credentials are kept **profile-only** and must never appear on the CV.

### Professional Experience

- Rewrite bullets to emphasize aspects most relevant to the target role.
- Use 4-6 bullets for the most recent role, 3-4 for previous, 2-3 for older.
- **Emphasize measurable results:** "reduced processing time by X%", "adopted by 40%+ of engineers".
- Group by company; give each role its own role line + bullets under one company header.
- Any mention of agentic coding / AI tooling must reference **Claude Code** by name.

#### Check tenure against visible output

Before finalizing, look at each role the way a stranger will: date span versus how much work is shown. A multi-year role represented by one project reads as low output. Fixes, in order: surface more real work; make the phases within the role explicit; name what made the cycle long. **Never** pad with invented projects or quietly shorten dates.

### In-progress qualifications must say so explicitly

A bare year range on a degree or certification, seen partway through, looks finished. State completion inside the entry itself: `[Degree], [Field], expected [Month Year]`. Claiming a credential not yet held is a factual misstatement discovered at reference check. The same applies to in-progress certifications.

### Education

One line per degree: `Institution --- Degree, GPA/details | Month Year`. Keep the highest degrees. Omit any degree the profile marks as **CV-excluded** (some are kept profile-only).

### Evidence Links

Where the CV names a verifiable public artifact (a project, a publication), carry its link with `\href{...}{...}` so a reader can verify in one click.

## Compile-and-Inspect Loop (MANDATORY)

After writing the CV and before presenting, always compile and visually inspect the PDF. Iterate until clean:

1. Run `lualatex -interaction=nonstopmode main_<company>_<role>.tex`
2. Check the output page count: must be exactly 2.
3. Read the PDF via the Read tool and inspect both pages.
4. Check for **orphaned company/role headers**: a company or role title line must never sit alone at the bottom of a page with its bullets on the next page.

### Fixing common page-break problems

- **Orphaned company header** (company/role line at the bottom of page 1, bullets on page 2): add `\needspace{5\baselineskip}` immediately before the company line (the master already does this before each company). For a role sub-line that orphans, add `\needspace{4\baselineskip}` before it.
- **CV spills a few lines onto page 3:** tighten with `\enlargethispage{2\baselineskip}` on page 2, or cut the lowest-relevance line (see below). Do **not** shrink the font or margins.
- **Substantial content on page 3:** cut content using relevance-weighted cutting (below).
- **Content finishes early on page 2 (feels thin):** restore the highest-relevance bullet previously cut.

Do not shrink geometry, font size, or `\parskip` to force a fit - a cramped CV reads worse than a cut one.

## ATS Parseability

Most employers run CVs through an ATS that reads the PDF's embedded **text layer**. This single-column template extracts cleanly, but still verify after the layout passes:

```bash
cd cv && pdftotext -layout main_<company>_<role>.pdf main_<company>_<role>.txt
```

`pdftotext` (poppler) is optional; if missing, skip the mechanical check with a warning and check keyword coverage from the visual PDF read.

- **Contact details as literal text.** The contact line is plain text (no icons), so address, phone, and email all extract verbatim - this is a key advantage of this template over icon-based ones. Verify they appear.
- **No garbled output** (`(cid:*)` markers or `�`) - none expected with carlito under lualatex.
- **Reading order** matches the visual order - guaranteed here because the layout is single-column.
- **Keyword coverage.** Match the posting's required/preferred terms against the extracted text. Prefer the posting's exact term over a synonym when truthfully applicable. Never add a keyword the profile does not support.

## Page Budget - Hard 2-Page Limit

The CV **must** fit exactly 2 pages. Guide limits:

| Section | Max budget |
|---------|-----------|
| Summary | 3 bullets, 1-2 lines each |
| Technical Skills | 6 category bullets, 1 line each |
| Most recent role | 4-6 bullets |
| Previous role | 3-4 bullets |
| Older roles | 2-3 bullets |
| Education | 2-3 one-line entries |

**If in doubt, cut rather than squeeze.**

## Two-page balance (fill page 2)

The hard rule is exactly 2 pages; the quality rule is that **page 2 should look close to full**, not trail off with a large blank area. After the content is finalized (mandatory + JD-selected optional bullets), tune the vertical rhythm so the content fills roughly two full pages:

- Adjust in the preamble: `\linespread` (~1.05-1.18), `\parskip` (4-7pt), list `itemsep` (1.5-4.5pt), section `\titlespacing` before-space (9-15pt), and margins (0.6-0.9in).
- **More content -> smaller values; less content -> larger values.** A leaner CV needs more spacing to fill two pages; a denser one needs less. (E.g. a leaner CV might use `\linespread{1.15}`, `parskip 7pt`, `itemsep 4.5pt`, `0.9in` margins; a fuller one `\linespread{1.08}`, `parskip 5pt`, `0.8in`.)
- Compile, read the PDF, and iterate until page 2 is ~85-95% full **without** spilling to a 3rd page and **without** looking padded or sparse.
- If spacing alone can't balance it (content is only ~1.3 pages), rebalance by letting a role block flow to page 2, or add a JD-relevant optional bullet the base file offers.

Never leave page 2 mostly blank, and never cut below the mandatory set to hit the limit — tune spacing instead.

## Relevance-weighted cutting (the right way to shrink a CV)

**Cut by signal, not by section.** For every candidate line, score three things:

1. **Relevance to THIS posting** - does the line hit a named tool, keyword, or stated responsibility?
2. **Uniqueness** - is it the only place this claim appears?
3. **Narrative load** - does the cover letter depend on it?

Cut the lowest-total-score line first, regardless of section.

### Practical order of cuts (easiest -> last resort)

1. **Redundancy** - a claim duplicated between Summary and an experience bullet: cut the Summary version (the bullet is more concrete).
2. **Summary fluff** - a bullet that just restates what Skills or Experience already shows.
3. **Low-relevance experience bullets** - a bullet that does not touch posting keywords, wherever it sits.
4. **Low-relevance skills** - trim a category's less-relevant items, or drop a category the posting never mentions.
5. **Last-resort structural cuts** - tighten an older role to 2 bullets, or drop the oldest education line.

### Pitfalls

- Do not mechanically cut the oldest role first if it speaks directly to the posting.
- Do not cut the one concrete example the cover letter leans on.
- Do not cut for a 2.05-page near-miss - use `\needspace`/`\enlargethispage` or trim one redundant clause instead.

## Recommended Section Order

1. Summary (3 bullets)
2. Technical Skills (category bullets)
3. Professional Experience (reverse chronological, grouped by company)
4. Education (reverse chronological)

For roles where credentials are the key qualifier, Education may move above Professional Experience.
