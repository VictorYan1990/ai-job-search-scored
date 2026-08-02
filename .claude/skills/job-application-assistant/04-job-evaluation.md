---
framework_version: 2.0.0
---

# Job Evaluation Framework

<!-- SETUP: The weights, salary anchors, per-category scores, and hard-skip rules below are
     PLACEHOLDERS / neutral defaults. Run /setup to set them to your own preferences, and
     review this file before your first /rank or /apply. Anything in [SQUARE_BRACKETS] or a
     `[SET ...]` tag is yours to fill in. -->

Every posting is scored on **two independent 0–100 axes**, then blended:

- **FIT** — how well the job matches *your background* (can you do it; does it match your skills/experience).
- **TARGET** — how much *you want* the job (desirability: pay, industry, role type, prestige, size, work-life balance).

**Overall = (Fit weight)·Fit + (Target weight)·Target.** Set the blend in /setup — a **50/50** split is a reasonable starting point. Always present Fit, Target, and Overall together, and flag a large imbalance (high Fit / low Target = "can do it, won't love it"; high Target / low Fit = "want it, would be a stretch").

Two things run **before** any scoring: the hard-skip gate (below) and, for `/scrape`/`/rank`, the `suppression.yaml` dedup. A posting that fails the gate is not scored and not drafted.

---

## Hard-Skip Gate — run FIRST
<!-- SETUP: fill each [SET_...] with your own hard deal-breakers. Leave a row's rule blank to disable it. -->

If a posting hits any rule below, **skip it** (do not score, do not draft). Quote the triggering wording back to the user rather than silently dropping it.

| Gate | Skip when… |
|------|-----------|
| **Work authorization** | The posting's sponsorship/eligibility cannot meet `[SET_YOUR_WORK_AUTHORIZATION_NEEDS]` (e.g. needs visa sponsorship the employer won't give, or requires citizenship / permanent residency / clearance you don't hold). **Silence ≠ permission** — verify on the employer's own site at `/apply`; flag as unverified in `/rank`. |
| **Location** | Outside `[SET_YOUR_LOCATION_CONSTRAINTS]` (e.g. requires relocation, or onsite/hybrid outside your area and not remote). Onsite-day count is *scored* under Work-Life Balance, not skipped here. |
| **Salary** | Posting states an **hourly wage**, or estimated total comp `< [SET_YOUR_MINIMUM_TOTAL_COMP]` (see Salary scoring for the estimate). |
| **Industry** | Any sector in `[SET_YOUR_EXCLUDED_INDUSTRIES]` (industries you refuse outright). |
| **Company size** | Smaller than `[SET_YOUR_MINIMUM_COMPANY_SIZE]` (e.g. very early-stage / pre-funding-milestone startups). |

Notes:
- A posting with **no salary range** is **not** skipped — it is scored at your current level (see Salary).
- `suppression.yaml` handles dedup/cooldowns separately (see `/scrape`, `/rank`).

---

# FIT (0–100)

## Fit Weighting
<!-- SETUP: set these to your priorities; they must sum to 100. Suggested ranges in parens. -->
- Technical Skills: `[SET %]`  (e.g. 40–50)
- Experience Match: `[SET %]`  (e.g. 25–30)
- Behavioral Fit: `[SET %]`  (e.g. 10–15)
- Career Alignment: `[SET %]`  (e.g. 10–15)

`Fit = weighted average of the four dimensions using the weights above.`

### 1. Technical Skills Match (0–100)
How well do the required/preferred skills align with your capabilities?

| Score | Meaning |
|-------|---------|
| 80–100 | Core requirements are primary skills |
| 60–79 | Most requirements match, 1–2 learnable gaps |
| 40–59 | Partial match, significant upskilling needed |
| 0–39 | Fundamental mismatch |

**Strong match areas:** [YOUR_PRIMARY_SKILLS]
**Moderate match areas:** [YOUR_SECONDARY_SKILLS]
**Weak match areas:** [SKILLS_YOU_LACK]

### 2. Experience Match (0–100)
Does your work history align with the role?

| Score | Meaning |
|-------|---------|
| 80–100 | Direct experience in the same domain and role type |
| 60–79 | Related experience, transferable skills clear |
| 40–59 | Adjacent experience, would need to make the case |
| 0–39 | Unrelated experience |

**Strong:** [YOUR_DIRECT_EXPERIENCE_DOMAINS]
**Moderate:** [YOUR_ADJACENT_EXPERIENCE]
**Entry-level / limited:** [ROLES_WITH_LIMITED_EXPERIENCE]

### 3. Behavioral / Culture Fit (0–100)
Does the role/company culture match your behavioral profile (see `02-behavioral-profile.md`)?

| Score | Meaning |
|-------|---------|
| 80–100 | Culture strongly matches your preferences |
| 60–79 | Mixed signals but mostly compatible |
| 40–59 | Some friction areas |
| 0–39 | Significant mismatch |

### 4. Career Alignment (0–100)
Does this role advance your direction and contain energizing work? **(Scope note: pay and role-type/industry preference live in TARGET — do not double-count them here. Career Alignment is only about career direction, growth path, and task energy.)**

| Score | Meaning |
|-------|---------|
| 80–100 | Strongly advances your direction, clear growth path |
| 60–79 | Good role, partially aligned with long-term direction |
| 40–59 | Doesn't build toward the direction |
| 0–39 | Dead end or backward step |

- **Energizing tasks:** [YOUR_ENERGIZING_TASKS]
- **Draining tasks:** [YOUR_DRAINING_TASKS]

---

# TARGET (0–100)

## Target Weighting
<!-- SETUP: set these to your priorities; they must sum to 100. Suggested ranges in parens. -->
- Salary: `[SET %]`  (e.g. 20–25)
- Industry: `[SET %]`  (e.g. 15–20)
- Job Subcategory: `[SET %]`  (e.g. 15–20)
- Company Reputation: `[SET %]`  (e.g. 10–15)
- Company Stage/Size: `[SET %]`  (e.g. 5–10)
- Work-Life Balance Culture: `[SET %]`  (e.g. 5–10)

`Target = weighted average of the six dimensions using the weights above.`

### 1. Salary (0–100)
<!-- SETUP: set [YOUR_CURRENT_TOTAL_COMP], [COMP_FOR_TOP_SCORE], [YOUR_MINIMUM_TOTAL_COMP]. -->
Baseline: your current total comp is `[YOUR_CURRENT_TOTAL_COMP]` — the neutral point, which scores **60**.

**Estimate expected total comp:**
1. **Base:** midpoint of the posted base-salary range.
2. **Multiplier → total comp** (default heuristic; adjust if you like):
   - total-comp figure stated directly → use as-is.
   - equity / RSU / options mentioned → base × **1.4**.
   - bonus mentioned (financial/traditional) → base × **1.25**.
   - neither stated → infer by industry (tech/startup ×1.4; financial/traditional ×1.25).
   - both mentioned → ×1.4.
3. **No salary range posted:** if a well-known firm with abundant public comp data (Levels.fyi, Glassdoor, H1B disclosures), estimate expected total comp from it and score; otherwise default to your current level → **60**.

**Score (linear, higher is better):**
`score = 60 + (est_total − [YOUR_CURRENT_TOTAL_COMP]) / ([COMP_FOR_TOP_SCORE] − [YOUR_CURRENT_TOTAL_COMP]) × 40`, clamped to a max of 100.

- est_total ≥ `[COMP_FOR_TOP_SCORE]` → 100
- est_total = `[YOUR_CURRENT_TOTAL_COMP]` → 60
- est_total ≤ `[YOUR_MINIMUM_TOTAL_COMP]` → **SKIP** (hard gate)
- hourly wage → **SKIP**
- no range, insufficient data → 60

### 2. Industry (0–100)
<!-- SETUP: set the tier scores and fill [YOUR_..._INDUSTRIES] with your own preferences. -->
Score by how much you want to work in the sector. Suggested default tiering (edit to taste):

| Score | Industry |
|---|---|
| 100 | `[YOUR_TOP_INDUSTRIES]` — e.g. software / SaaS / internet / fintech, including **vertical SaaS** (a software product counts as tech regardless of its end-domain, e.g. health-tech, insurtech) |
| ~80 | `[YOUR_SECOND_TIER_INDUSTRIES]` — e.g. asset management, AI labs |
| ~70 | other / unlisted |
| ~60 | `[YOUR_LOWER_TIER_INDUSTRIES]` — e.g. traditional (non-tech) industries |

**Crypto:** treat as fintech (not auto-skipped). If the company's **major business is crypto** (exchange, token/stablecoin issuer), subtract **5–10** from Industry (pure-crypto → −10; crypto as one product of a broader fintech → −5). Strong general reputation still counts on the Reputation dimension.
Industries you rule out entirely → put them in the hard-skip gate (`[SET_YOUR_EXCLUDED_INDUSTRIES]`).

### 3. Job Subcategory (0–100)
<!-- SETUP: the role-type taxonomy is generic; the SCORE you give each type is YOUR preference. Assign scores + reorder to taste. -->
Classify the role into the nearest role type, then apply the optional language modifier. Assign each type a score (0–100) reflecting how much you want it. Common engineering role types:

- **Product-oriented / developer-platform backend** (API development; FastAPI, AsyncIO; SDK/platform for internal or external developers) → `[SET score]`
- **Full-stack / DevOps / platform engineer**, or **data-heavy backend** (not titled Data Engineer) → `[SET score]`
- **Data Engineering / Data Engineer** → `[SET score]`
- **Infra-heavy platform / SRE** (Kubernetes/K8s, Terraform, on-call, "keep the lights on," reliability/ops focus) → `[SET score]`

(Add, remove, or reorder role types to match your own preferences.)

**Language modifier (optional; apply after base, clamp 0–100):** a small +/- for languages or frameworks you prefer or avoid — e.g. `[SET: +N for languages you like (and their frameworks), −N for ones you avoid]`.

### 4. Company Reputation (0–100)  *(qualitative — judgment)*
<!-- SETUP: describe what "reputation" means to you in [YOUR_REPUTATION_PREFERENCE]. -->
Score the company's reputation per `[YOUR_REPUTATION_PREFERENCE]` (e.g. reward cutting-edge tech stacks, strong engineering brand/ecosystem). Use relative judgment and a consistent set of anchor bands, e.g.:

| Band | Meaning |
|---|---|
| 90–100 | Top-tier engineering prestige & ecosystem |
| 75–89 | Strong / well-regarded |
| 60–74 | Solid but not a prestige eng brand |
| 40–59 | Weak eng brand / dated stack |

`/rank`: estimate from background knowledge. `/apply`: verify/refine with research.

### 5. Company Stage / Size (0–100)
<!-- SETUP: set your size/stage preference in [YOUR_SIZE_PREFERENCE] and adjust the tiers. -->
Score by your size/stage preference (`[YOUR_SIZE_PREFERENCE]`, e.g. larger firms for stability, or earlier-stage for upside). Suggested default tiering by headcount:

| Headcount | Score |
|---|---|
| 2,001–20,000 | 100 |
| 501–2,000 | 90 |
| 20,001–100,000 | 90 |
| > 100,000 | 85 |
| 201–500 | 80 |
| 51–200 | 70 |
| 10–50 | 60 |
| < your minimum | **SKIP** (see gate) |

**Private penalty (optional):** subtract a few points for private companies if stability matters to you (`[SET]`); clamp ≥ 0.

### 6. Work-Life Balance Culture (0–100)  *(qualitative — judgment)*
<!-- SETUP: set how much remote/WLB matters in [YOUR_WLB_PREFERENCE]. -->
Score per `[YOUR_WLB_PREFERENCE]`. A suggested default (remote-weighted): set the band from **remote proportion** first (fully-remote highest → 5-day-onsite lowest), then adjust for WLB reputation (±3–5) and recent layoffs/PIP history (moderate −5 to −10, not a hard floor).

| Band | Remote signal (primary) |
|---|---|
| 90–100 | Fully remote |
| 80–89 | Hybrid ≤3 days onsite |
| 65–79 | Hybrid ~4 days onsite |
| 45–60 | 5 days onsite |

`/rank`: from the posting's remote policy + background knowledge. `/apply`: verify (remote policy, Glassdoor, layoff news).

---

## Output Format

```
## Job Fit Evaluation: [Role] at [Company]

Hard-skip gate: PASS  (or: SKIP — <reason + quoted wording>)

### FIT — <Fit>/100
| Dimension | Score | Wt | Note |
|-----------|-------|----|------|
| Technical Skills | XX | .. % | ... |
| Experience Match | XX | .. % | ... |
| Behavioral Fit | XX | .. % | ... |
| Career Alignment | XX | .. % | ... |

### TARGET — <Target>/100
| Dimension | Score | Wt | Note |
|-----------|-------|----|------|
| Salary | XX | .. % | est. total comp $XXXk (base mid × mult) |
| Industry | XX | .. % | ... |
| Job Subcategory | XX | .. % | base XX <±modifier> |
| Company Reputation | XX | .. % | ... |
| Company Stage/Size | XX | .. % | ~N employees, public/private |
| Work-Life Balance | XX | .. % | remote policy + reputation |

**Overall: <blend of Fit & Target>/100** — Verdict: [Strong/Good/Moderate/Weak/Poor]
Imbalance flag: [none | high Fit / low Target | high Target / low Fit]

### Key strengths / Gaps / Recommendation
- ...
```

## Verdict bands (Overall)
<!-- SETUP: adjust thresholds if you like. -->
- **Strong Fit** (75+): definitely apply, tailor everything.
- **Good Fit** (60–74): apply, address gaps in the cover letter.
- **Moderate Fit** (45–59): consider carefully, discuss with user.
- **Weak Fit** (30–44): probably skip unless strategic.
- **Poor Fit** (<30): skip.

Always report Fit and Target alongside Overall — a middling Overall can hide a strong-one-axis / weak-other-axis split the user should see.

## Company Research Checklist (for `/apply`)
- [ ] Company site (mission, values, recent news), and the specific team if named
- [ ] Work-authorization / sponsorship match, from the employer's own site (gate verification)
- [ ] Comp: confirm posted range / total comp
- [ ] Size, stage (public/private, funding round), headcount
- [ ] Reputation & WLB: Glassdoor/Blind/Levels.fyi, recent layoff/PIP news, remote policy

---

## Pre-Application: Referrals and Warm Outreach

At most tech/fintech firms you apply through LinkedIn or the company career page; postings rarely list a contact to phone, and cold-calling can read as odd. The high-value pre-application move is usually a **warm introduction**, not a call.

- **Get a referral** — often the single strongest lever. Check LinkedIn for 1st/2nd-degree connections at the target company before applying cold.
- **Reach out to a recruiter or someone on the team via LinkedIn** with a short, specific message — only with a genuine question or concrete reason, never just to "be remembered."
- Cold applications to large employers often convert poorly; when a shortlisted role has no warm path, flag it before applying cold.
- If a posting *does* list a contact and invites questions, reach out only with substantive questions and use what you learn to tailor the application.
