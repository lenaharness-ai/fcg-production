# FCG AI Readiness Diagnostic - Lite Version (LIVE)
**Status:** Live in production
**URL path:** /lite
**Source:** `diagnostic-tool/src/app/lite/page.tsx`
**Last verified:** April 1, 2026

---

## What It Is

A 14-question, 5-7 minute "signal" version of the full 56-question AI Readiness Diagnostic. Designed to:
- Surface enough risk to create urgency without giving away the full picture
- Load fast and feel executive-appropriate (not a survey)
- Feed collected answers into the full diagnostic (progressive profiling - no repeat questions)
- Generate a maturity score and red flag alerts as a lead qualification tool for FCG

The lite tool is the top-of-funnel entry point. The full diagnostic is the paid/engaged deliverable.

---

## User Flow

1. **Firm Info Step** - Collects: firm name (text), role (dropdown), firm size by AUM (dropdown)
2. **Question Steps** - 7 domains shown one at a time. 2 questions per domain. Animated transitions.
3. **Results** - Score, maturity level, domain breakdown bar chart, red flag alerts if triggered

No email gate on lite results (email is gated on the full diagnostic results page).

---

## Firm Info Fields

**Role (dropdown - 6 options):**
- C-Suite Executive (CEO, CTO, CIO, CFO, CCO)
- Chief Risk / Compliance Officer
- Head of Technology / IT Director
- Head of Operations
- Portfolio / Investment Leader
- Other Senior Leader

**Firm Size by AUM (dropdown - 5 options):**
- Under $100M AUM
- $100M - $500M AUM
- $500M - $2B AUM
- $2B - $10B AUM
- Over $10B AUM

---

## Answer Scale

All questions use a 1-5 scale:

| Score | Label |
|---|---|
| 1 | No / Not at all |
| 2 | Rarely / Minimally |
| 3 | Partially / In progress |
| 4 | Mostly / Substantially |
| 5 | Yes / Fully |

---

## The 14 Questions (Live - Approved)

### Domain 1: Data Governance (Weight: 25%)
*CXO Outcome: We have clear visibility into our data, the lineage behind it, and whether that data is fit for AI use.*

**DG-01:** Does your organization maintain a documented data inventory that maps data sources used by AI systems?
- Rationale: Without a data inventory, the firm cannot know what data feeds its AI systems, creating hidden liability.
- Red Flag: No

**DG-02:** Is sensitive client data (PII, financial records) formally tagged and excluded from general AI training or input pipelines?
- Rationale: PII exposure in AI inputs is a direct regulatory violation under GLBA, GDPR, and CCPA for financial services firms.
- Red Flag: No

---

### Domain 2: Model Risk Readiness (Weight: 25%)
*CXO Outcome: We know which AI models are in production, who owns them, and how we validate their outputs before they touch client decisions.*

**MR-01:** Does your organization maintain a formal AI model inventory that includes all production, pilot, and vendor-supplied models?
- Rationale: You cannot govern what you cannot see. An inventory is the minimum baseline for model risk management.
- Red Flag: No

**MR-02:** Is independent model validation required before any AI model is deployed in a client-facing or revenue-impacting context?
- Rationale: Independent validation is a core requirement under SR 11-7 guidance for model risk in financial services.
- Red Flag: No

---

### Domain 3: Strategic Alignment & ROI (Weight: 15%)
*CXO Outcome: We can demonstrate which AI initiatives are creating measurable value and which are consuming capital without return.*

**SA-01:** Is your organization's AI strategy formally documented and explicitly connected to 2-3 measurable business outcomes?
- Rationale: AI without a strategy document is experimentation. A documented strategy forces prioritization and accountability.
- Red Flag: No

**SA-02:** Does leadership have a clear view of total AI spending (licenses, infrastructure, personnel, pilots) vs. quantified business return?
- Rationale: Capital discipline requires a real cost-benefit view. Without it, AI spend grows silently and unchecked.
- Red Flag: No

**ROI Gate trigger:** If Strategic Alignment domain average < 2.5, the ROI Gate flag is triggered.

---

### Domain 4: Cyber, Security & Infrastructure (Weight: 10%)
*CXO Outcome: We have the minimum security conditions required to allow AI use at scale without creating catastrophic data exposure.*

**CS-01:** Does your organization enforce Multi-Factor Authentication (MFA) or Zero Trust access controls for all employees accessing AI-powered systems?
- Rationale: MFA is the minimum security baseline for AI access in financial services.
- **RED FLAG if scored 1:** "Score of 1 triggers RED FLAG - no MFA is a critical infrastructure failure."

**CS-05:** Does your IT team have the capability to detect if employees are using unapproved AI tools (e.g., ChatGPT, Gemini) on company devices with company data?
- Rationale: Shadow AI is the leading source of data breach risk in financial services. Detection capability is the prerequisite to control.
- **RED FLAG if scored 1:** "Score of 1 triggers RED FLAG - no shadow AI detection means client data may already be exposed."

---

### Domain 5: Cultural Readiness (Weight: 10%)
*CXO Outcome: Our organization can absorb AI into existing workflows without eroding trust, accountability, or psychological safety.*

**CR-01:** Does senior leadership (C-suite and Board) have a clearly articulated, communicated vision for how AI will change the way this firm operates?
- Rationale: Without a visible leadership vision, employees interpret AI adoption as a threat to their roles, creating resistance.
- Red Flag: No

**CR-02:** Do employees feel psychologically safe to report AI errors, hallucinations, or unexpected outputs without fear of blame or reprisal?
- Rationale: If employees hide AI errors to avoid penalties, the firm accumulates invisible risk that surfaces catastrophically.
- Red Flag: No

---

### Domain 6: AI Skills & Talent (Weight: 10%)
*CXO Outcome: Our people understand their accountability for AI outputs and can exercise sound human judgment in AI-assisted workflows.*

**AT-02:** Do licensed professionals and advisors understand that they are legally accountable for AI-assisted recommendations and cannot delegate fiduciary responsibility to a model?
- Rationale: Fiduciary accountability cannot be transferred to an AI. Human sign-off and understanding is a legal requirement.
- Red Flag: No

**AT-04:** Can employees at all levels identify an AI hallucination or a factually incorrect AI output in the context of their specific job function?
- Rationale: Hallucination detection is the minimum human-in-the-loop capability for any firm using AI in client-impacting work.
- Red Flag: No

---

### Domain 7: Ethical & Responsible AI (Weight: 5%)
*CXO Outcome: Every AI decision made by or for our organization is explainable to a client, regulator, or auditor - and we can defend it.*

**ER-01:** Has your organization defined explicit "No-Go" zones - specific AI use cases that will not be pursued due to ethical, legal, or reputational risk?
- Rationale: No-Go zones demonstrate intentional ethical governance and protect the firm from well-meaning but harmful innovation.
- Red Flag: No

**ER-03:** Can your organization provide a plain-English explanation for any AI-driven recommendation that a client, regulator, or auditor requests?
- Rationale: Explainability (XAI) is increasingly required by regulation. "The algorithm decided" is not a defensible answer.
- Red Flag: No

---

## Scoring Logic

### Method: `calculateScoresPartial()`
Used instead of `calculateScores()` to handle unanswered questions gracefully. For each domain:
- Average only the questions that have answers
- If a domain has no answers, defaults to neutral score of 3
- Weighted contribution = (domain average / 5) x domain weight x 100
- Total score = sum of all weighted contributions, rounded to integer (0-100)

### Domain Weights

| Domain | Code | Weight |
|---|---|---|
| Data Governance | DG | 25% |
| Model Risk Readiness | MR | 25% |
| Strategic Alignment & ROI | SA | 15% |
| Cyber, Security & Infrastructure | CS | 10% |
| Cultural Readiness | CR | 10% |
| AI Skills & Talent | AT | 10% |
| Ethical & Responsible AI | ER | 5% |

### Maturity Levels

| Score | Label | Color |
|---|---|---|
| 0-20 | Initial / Ad-hoc | Red (#DC2626) |
| 21-40 | Reactive | Orange (#EA580C) |
| 41-60 | Defined | Amber (#CA8A04) |
| 61-80 | Managed | Green (#16A34A) |
| 81-100 | Optimized / AI-Native | Navy (#0A1628) |

### Red Flag Triggers
A red flag banner appears on the results screen when any of the following questions is scored 1:
- **CS-01** (MFA/Zero Trust) - "Critical infrastructure failure"
- **CS-05** (Shadow AI detection) - "Client data may already be exposed"

### ROI Gate
Triggers when Strategic Alignment domain average < 2.5. Signals that the firm lacks the strategic foundation to realize value from AI investment.

---

## Technical Details

- **Framework:** Next.js (App Router), TypeScript, Tailwind CSS v4
- **Animation:** Framer Motion - slide transitions between domains (left/right direction-aware)
- **Scoring function:** `calculateScoresPartial()` from `src/lib/scoring.ts`
- **Question set:** `LITE_QUESTION_IDS` from `src/lib/questions.ts`
- **Routing:** On completion, results display inline (no separate results page for lite)
- **No email gate** on lite results (email gate is on full diagnostic only)
- **Progressive profiling:** Lite answers carry forward to full diagnostic; users do not repeat answered questions

---

## Relationship to Full Diagnostic

| | Lite | Full |
|---|---|---|
| Questions | 14 (2 per domain) | 56 (8 per domain) |
| Time | 5-7 minutes | 15-20 minutes |
| Email gate | No | Yes |
| Results detail | Score + maturity + red flags | Radar chart + full domain breakdown + AI-generated executive report emailed |
| Purpose | Lead capture / urgency signal | Paid engagement / FCG deliverable |
| URL | /lite | /diagnostic (assessment) + /results |

---

## File Locations

| Asset | Path |
|---|---|
| Lite page source | `diagnostic-tool/src/app/lite/page.tsx` |
| Question definitions | `diagnostic-tool/src/lib/questions.ts` |
| Scoring logic | `diagnostic-tool/src/lib/scoring.ts` |
| This reference doc | `context/tech_dev/diagnostic_lite_LIVE.md` |
| HTML version | `output/fcg/diagnostic/diagnostic_lite_LIVE.html` |
| Pre-build design spec | `context/tech_dev/diagnostic_lite_and_website_handoff.md` |
