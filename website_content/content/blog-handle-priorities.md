# How’d you handle conflicting priorities from stakeholders?


[![Handle Conflict](https://img.youtube.com/vi/Kvdgmd_P78Q/0.jpg)](https://www.youtube.com/watch?v=Kvdgmd_P78Q)


## Introduction

Building software is rarely a solo act. Product managers, designers, engineers, marketing leads, compliance officers, and executives all bring *requests* they believe are critical. Inevitably those requests collide—the sales team needs a one-off feature to close a deal, the CTO wants time for technical debt, the PM is chasing a North-Star metric, and legal just discovered a looming regulation. **Handling conflicting stakeholder priorities** is the discipline of turning that chaotic wish-list into a coherent, value-led roadmap. Mastering it keeps teams focused on business outcomes, prevents “loudest-voice wins” politics, and—most importantly—delivers the right value to users at the right time.

---

## Explanation

### 1. Frame the Problem

1. **Surface every ask.** Create a single backlog (spreadsheet, Jira board, Notion database) where *all* requests live.
2. **Attach context.** For each item capture: owner, user problem, expected impact, deadline, and risk of inaction.
3. **Anchor on a North Star.** Agree on 1–3 top-level objectives (OKRs, KPIs). Everything funnels through these.

### 2. Quantify with a Scoring Model

A simple—but powerful—pattern is **Value ÷ Effort**:

```text
Priority Score = (Business Value + User Value + Strategic Fit) / Effort
```

Common variants:

| Model      | Main Inputs                              | Strengths                   | Watch-outs              |
| ---------- | ---------------------------------------- | --------------------------- | ----------------------- |
| **RICE**   | Reach, Impact, Confidence, Effort        | Balances optimism with risk | Subjective “confidence” |
| **WSJF**   | (Business Value + Urgency + Risk) / Size | Highlights cost of delay    | Needs good numbers      |
| **MoSCoW** | Must, Should, Could, Won’t               | Fast, non-numerical         | Easy to game terms      |

Choose one model and stick with it; consistency builds trust.

### 3. Facilitate a Trade-Off Workshop

1. Pre-work: Each stakeholder privately scores their items.
2. Workshop: Reveal scores, discuss deltas, re-score if facts change.
3. Decision: Rank by score, then slice by capacity (e.g., top N fit the next sprint or quarter).

### 4. Document & Broadcast

Maintain a **Decision Log**:

```text
2025-Q3 Prioritization
-------------------------------------------------
# | Feature             | Score | Decision | Rationale
1 | Custom SSO          | 8.5   | ✅ Now    | $2 M ARR at risk
2 | Tech-Debt Cleanup   | 7.2   | 🟡 Buffer | Prevents scale issues
3 | Self-Serve Onboard  | 6.1   | ❌ Next   | Higher effort, lower COD
```

Share the summary in Slack, Confluence, or email. Transparency reduces “you ignored my request” drama.

### 5. Inspect & Adapt

Measure **leading indicators** (cycle time, stakeholder satisfaction) and **lagging outcomes** (ARR, NPS). Re-prioritize every sprint or quarter—markets change.

---

## Real-World Use Cases

1. **E-Commerce Platform**

   * *Conflict*: Sales needs a “Buy Now, Pay Later” option to close holiday deals; Operations demands a returns-workflow overhaul before refund-fraud spikes.
   * *Resolution*: Use WSJF. BNPL scores higher on **business value but lower on risk**, returns workflow wins because cost of delay is massive in peak season.

2. **Banking App**

   * *Conflict*: Product wants social-sharing of savings goals; Compliance must meet a new KYC mandate.
   * *Resolution*: Compliance items tagged **“Must-have”** in MoSCoW with a legal deadline, bumping social features to the next release.

---

## Best Practices

| ✅ Do                                                       | ❌ Avoid                            |
| ---------------------------------------------------------- | ---------------------------------- |
| Use one clear scoring model and educate stakeholders on it | Switching models every cycle       |
| Time-box discussions; let data, not volume, win            | Letting the loudest voice dominate |
| Keep a 10-15 % buffer for unplanned work                   | Filling roadmap to 100 % capacity  |
| Revisit priorities at a fixed cadence                      | Treating the roadmap as immovable  |
| Document dissenting opinions for context                   | Erasing minority viewpoints        |

---

## Key Takeaways

* **Centralize requests** and capture impact, effort, and risk up front.
* **Quantify** every item with an agreed-upon model (RICE, WSJF, etc.).
* **Facilitate, don’t dictate**—structured workshops build alignment.
* **Log decisions** publicly; transparency equals trust.
* **Review often**; new data can (and should) reorder priorities.

---

Done right, prioritization becomes a repeatable, data-driven habit that turns stakeholder conflict from a headache into a healthy competitive dialogue—and your roadmap into a value-delivery machine.

---
