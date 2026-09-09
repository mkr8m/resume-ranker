# 📄 Resume Ranker

An AI-powered resume screening and candidate ranking automation built with **n8n**.

Resume Ranker helps recruiters quickly evaluate multiple resumes against a specific job description, identify strengths and gaps, rank candidates, and receive a clear recruitment report directly through Telegram.

---

## 🎯 Project Overview

Resume screening can be repetitive and time-consuming, especially when recruiters need to compare several resumes against different job requirements.

Resume Ranker automates this process by:

1. Receiving candidate resumes through Telegram.
2. Extracting the resume text.
3. Analyzing the job description to identify hiring criteria.
4. Evaluating each resume against those criteria.
5. Extracting evidence from the resume instead of relying on generic assumptions.
6. Validating and normalizing the AI output.
7. Calculating a transparent fit score.
8. Ranking all candidates.
9. Generating a recruiter-friendly screening report.
10. Creating an exact visual candidate-ranking summary.
11. Sending the report and summary image through Telegram.

---

## ⚙️ How It Works

```text
Telegram
   ↓
Resume Intake
   ↓
Resume Text Extraction
   ↓
Job Description Analysis
   ↓
Candidate Preparation
   ↓
┌─────────────────────────────┐
│ Analyze Each Resume         │
│          ↓                  │
│ Extract Evidence            │
│          ↓                  │
│ Validate Evaluation         │
│          ↓                  │
│ Calculate Fit Score         │
└──────────────┬──────────────┘
               ↓
        Rank Candidates
               ↓
      Recruiter Report
          ↙         ↘
   Telegram Report   Summary Image
```

---

## 🧠 AI Architecture

The project intentionally separates AI reasoning from deterministic logic.

### 1. Job Description Analyzer

The AI extracts criteria directly from the provided job description, including:

* Job title
* Seniority
* Required experience
* Must-have requirements
* Nice-to-have requirements
* Industry
* Soft skills
* Education
* Certifications
* Location / timezone requirements

The job description is treated as the **source of truth**.

### 2. Resume Analyzer

Each resume is evaluated independently against the extracted job criteria.

The AI looks for explicit evidence such as:

* Relevant employment history
* Years of experience
* Required skills
* CRM experience
* Calling platforms
* Prospecting experience
* Industry experience
* Soft-skill evidence
* Location/timezone information

The system avoids treating generic statements as proof when stronger evidence is required.

For example, a requirement such as:

> 130–150+ calls per day

cannot be satisfied simply because a candidate worked as an SDR.

The resume must contain explicit call-volume evidence.

---

## 🛡️ Validation Layer

The AI output is not used directly for scoring.

A deterministic validation layer checks and normalizes the AI response before scoring.

It verifies that:

* Required criteria are preserved.
* Criteria are not invented or altered.
* Evidence comes from the resume.
* Missing evidence is not guessed.
* Numeric requirements are handled strictly.
* Compound requirements are not incorrectly marked as fully supported.
* Each criterion appears exactly once in its category.

This creates a separation between:

**AI evidence extraction → deterministic validation → deterministic scoring**

---

## 📊 Scoring System

The final fit score is calculated using deterministic JavaScript logic rather than asking the AI to decide the final score.

Current scoring weights:

| Category              | Weight |
| --------------------- | -----: |
| Must-have criteria    |    50% |
| Experience            |    20% |
| Seniority             |    10% |
| Industry              |     5% |
| Location / Timezone   |     5% |
| Nice-to-have criteria |     5% |
| Soft skills           |     5% |

For criteria-based groups:

* **Supported** = 100%
* **Partial** = 50%
* **Not mentioned** = 0%

The final score is normalized to a percentage based on the applicable criteria.

> These weights are design choices for this project and are not prescribed by the original exercise.

---

## 🏆 Candidate Ranking

After scoring, candidates are sorted by their calculated fit score.

Each candidate receives:

* Rank
* Candidate name
* Fit score
* Experience summary
* Key strengths
* Partial matches
* Key gaps
* Screening question

The highest-ranked candidate becomes the **Top Recommendation**.

---

## 📱 Telegram Output

The final result is delivered through Telegram in two formats.

### Recruiter Report

A concise, mobile-friendly report containing:

* Job title
* Number of candidates
* Top recommendation
* Candidate rankings
* Fit scores
* Strengths
* Gaps
* Screening questions

### Visual Summary

The workflow also generates an exact summary image containing the candidate rankings and fit scores.

The image is generated from structured workflow data rather than asking an image model to reproduce the numbers.

This ensures that names and scores remain accurate.

---

## 🛠️ Technologies Used

* **n8n** — Workflow automation
* **Rooya AI / GPT-4o-mini** — AI analysis and report formatting
* **Telegram Bot API** — Resume intake and result delivery
* **JavaScript** — Data processing, validation, scoring, and ranking
* **Data Tables** — Temporary resume storage
* **SVG** — Deterministic visual summary generation
* **PDF / PNG conversion** — Summary image generation

---

## 🔑 Key Design Decisions

### Evidence-based evaluation

The system does not simply ask an LLM:

> "Is this candidate good?"

Instead, it asks the AI to extract evidence for each specific requirement.

### Deterministic scoring

The LLM extracts evidence, while JavaScript calculates the final score.

This makes the scoring process more predictable and explainable.

### Job-specific evaluation

Candidates are evaluated against the actual job description rather than a fixed generic resume checklist.

### Conservative handling of missing information

If something is not explicitly supported by the resume, it is treated as unverified instead of being assumed.

### Exact visual reporting

The summary image is generated from structured data so candidate names and scores are not altered by an image-generation model.

---

## 📌 Example Use Case

A recruiter receives applications for an **SDR position**.

Instead of manually comparing five resumes:

1. The recruiter sends the resumes to the Telegram bot.
2. The system extracts the resumes.
3. The job requirements are analyzed.
4. Each candidate is evaluated independently.
5. Candidates receive calculated fit scores.
6. The system ranks them automatically.
7. The recruiter receives a complete report and visual summary.

---

## 📈 Example Output

```text
📊 RESUME SCREENING REPORT

🎯 Position
Sales Development Representative

👥 Candidates Analyzed: 5

━━━━━━━━━━━━━━━━━━━━

🏆 TOP RECOMMENDATION

🥇 ELSHADAY ABRAHAM
Fit Score: 68.7%
🟡 Potential Match

The resume provides explicit evidence for the
required criteria...

⚠️ Main Gaps
• Required call volume was not verified

🎯 Screening Question
Can you provide specific evidence of your
daily outbound call volume?

━━━━━━━━━━━━━━━━━━━━

🥇 #1 — ELSHADAY ABRAHAM
Fit Score: 68.7%

💼 Experience
Relevant SDR experience was identified.

✅ Key Strengths
• SDR experience
• CRM experience

🟡 Partial Matches
• High-volume outbound calling — partially supported

⚠️ Key Gaps
• 130–150+ calls per day — not verified

🎯 Screening Question
...
```

*Example output is illustrative.*

---

## 🚀 Project Status

**MVP — Completed and Tested**

The workflow has been tested end-to-end, including:

* Telegram resume intake
* Resume text extraction
* Job description analysis
* Multi-candidate evaluation
* Evidence validation
* Deterministic scoring
* Candidate ranking
* Recruiter report generation
* Visual summary generation
* Telegram delivery

---

## 💡 What This Project Demonstrates

This project demonstrates practical experience with:

* AI-powered workflow automation
* LLM prompt engineering
* Structured JSON extraction
* Evidence-based AI evaluation
* Deterministic business logic
* JavaScript in n8n
* Data validation
* Multi-item processing and loops
* Candidate ranking
* Telegram integrations
* Automated reporting
* Combining AI with traditional automation logic

---

## 👤 Author

Built as an AI automation portfolio project using **n8n**.

The project focuses on building practical AI systems where the LLM handles language understanding while deterministic automation handles validation, scoring, and business logic.
