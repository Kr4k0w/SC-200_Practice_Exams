# SC-200 Exam Simulator

A single-file, zero-dependency practice exam simulator for the **Microsoft SC-200: Security Operations Analyst** certification, plus six ready-to-import question banks (360 questions in total).

Open the HTML file in any modern browser and start practicing. There is no build step, no server and no installation.

> **Disclaimer:** This is an unofficial, community study tool. It is not affiliated with, endorsed by, or sponsored by Microsoft. The questions are original practice material and are **not** real exam questions. Microsoft, Defender, Sentinel, Entra and Azure are trademarks of Microsoft Corporation.
>
> **AI disclosure:** The simulator code, the questions, the answer keys and the explanations in this repository were generated with the help of AI (large language models). They have not been verified by Microsoft or by a certified exam author and **may contain errors or outdated information**. Always confirm important details on [Microsoft Learn](https://learn.microsoft.com/) and do not rely on this project as your only study source. Corrections are very welcome (see [Content accuracy](#content-accuracy-and-ai-disclosure)).

---

## Table of contents

- [Features](#features)
- [Quick start](#quick-start)
- [Practice exams](#practice-exams)
- [Question banks](#question-banks)
- [Importing questions](#importing-questions)
- [JSON format](#json-format)
- [Scoring rules](#scoring-rules)
- [Score report and answer review](#score-report-and-answer-review)
- [Project structure](#project-structure)
- [Browser support and privacy](#browser-support-and-privacy)
- [Content accuracy and AI disclosure](#content-accuracy-and-ai-disclosure)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Realistic exam flow:** 120-minute countdown timer, question navigation, *Mark for Review*, a review screen with answered / unanswered / marked overview, and an *End Exam* confirmation.
- **Single-choice and multiple-response questions:** multiple-response items ask you to select two or three options and are rendered with checkboxes.
- **Code-aware questions:** KQL and log snippets are shown in a formatted code block with light syntax highlighting.
- **Detailed score report:**
  - Overall score out of 1000 with a pass / fail badge.
  - Performance breakdown by SC-200 skill domain.
  - **Answer review** for every question, correct and incorrect, with filters and a color-coded question map (see [below](#score-report-and-answer-review)).
- **Explanations:** every question includes an explanation of the correct answer and, in most banks, why the distractors are wrong.
- **AI study coach prompt:** one click copies a structured diagnostic prompt (scores, domain results and your incorrect answers) that you can paste into an AI assistant to get a personalized study plan.
- **Print / Save as PDF:** print-friendly report styling.
- **Bring your own questions:** import any bank from a JSON file or by pasting JSON, with no code changes.
- **Auto-submit:** the exam is submitted automatically when the timer reaches zero, and the browser warns you before you accidentally close the tab mid-exam.
- **Responsive layout:** works on desktop, tablet and mobile.

## Quick start

1. Clone or download this repository.
2. Open `sc200-simulator.html` in your browser (double-click it, or serve the folder with any static server).
3. Pick an exam in the lobby and click **Start Exam**.

```bash
git clone https://github.com/<your-user>/<your-repo>.git
cd <your-repo>
# open sc200-simulator.html in your browser
```

The simulator ships with some questions embedded. To load the full set of six banks, see [Importing questions](#importing-questions).

## Practice exams

| # | Exam | Level | Focus |
|---|------|-------|-------|
| 1 | Foundational & Core Concepts | Beginner | Core Defender XDR, Defender for Cloud and Sentinel concepts |
| 2 | Intermediate KQL & Defender XDR | Intermediate | KQL, advanced hunting, Defender XDR, Defender for Cloud Apps |
| 3 | Microsoft Sentinel Architecture & Ingestion | Intermediate | Data connectors, DCRs, AMA, retention, RBAC, cost |
| 4 | Advanced Threat Hunting & Automation (SOAR) | Advanced | Hunting queries, playbooks, automation rules, Logic Apps |
| 5 | Complex Case Studies & Incident Response | Advanced | Multi-stage incidents, containment and eviction scenarios |
| 6 | Master Boss Level (Exam Readiness) | Expert | Mixed, exam-level scenarios |

Each exam contains **60 questions** distributed across the three SC-200 skill domains:

| Domain | Questions per exam |
|--------|-------------------|
| Mitigate threats using Microsoft Defender XDR | 27 |
| Mitigate threats using Microsoft Sentinel | 21 |
| Mitigate threats using Microsoft Defender for Cloud | 12 |

## Question banks

Each bank is a standalone JSON file, so you can use them individually.

| File | Single choice | Select 2 | Select 3 | Total |
|------|:---:|:---:|:---:|:---:|
| `exam1-Foundational_Core_Concepts.json` | 52 | 8 | 0 | 60 |
| `exam2-Intermediate-KQL-Defender-XDR.json` | 39 | 15 | 6 | 60 |
| `exam3-Microsoft_Sentinel_Architecture_Ingestion.json` | 45 | 11 | 4 | 60 |
| `exam4-Advanced_Threat_Hunting_Automation__SOAR_.json` | 46 | 9 | 5 | 60 |
| `exam5-Complex_Case_Studies_Incident_Response.json` | 46 | 10 | 4 | 60 |
| `exam6-Master_Boss_Level__Exam_Readiness_.json` | 52 | 7 | 1 | 60 |

Correct answers are spread across all option positions, so the position of an option is never a hint.

## Importing questions

1. In the lobby, scroll to **Import questions (JSON)**.
2. Choose the target exam slot (Exam 1 to Exam 6).
3. Either select a `.json` file or paste the JSON array into the text box.
4. Click **Load JSON**.

The slot is replaced with the imported questions and the lobby updates immediately. Imported banks are also saved in your browser's `localStorage` (when available), so they are restored the next time you open the file in the same browser.

Match each JSON file to its slot (for example, `exam3-...json` goes into **Exam 3**).

## JSON format

The simulator accepts two formats and converts them automatically.

### Bank format (used by the files in this repository)

```json
[
  {
    "id": 1,
    "domain": "Microsoft Defender XDR",
    "type": "single",
    "question": "Which response action cuts network connectivity but keeps the Defender for Endpoint channel?",
    "options": [
      "A. Collect investigation package",
      "B. Run antivirus scan",
      "C. Isolate device",
      "D. Offboard device"
    ],
    "correctAnswers": [2],
    "explanation": "Isolate device blocks general network traffic while preserving the cloud channel."
  }
]
```

| Field | Description |
|-------|-------------|
| `id` | Question number (1 to 60). |
| `domain` | One of `"Microsoft Defender XDR"`, `"Microsoft Defender for Cloud"`, `"Microsoft Sentinel"`. |
| `type` | `"single"` or `"multiple"`. |
| `question` | Question text. A query or log snippet can be embedded as `<pre><code>...</code></pre>` and will be rendered as a code block. |
| `options` | Array of 2 to 6 strings. A leading `"A. "` prefix is stripped and re-applied by the UI. |
| `correctAnswers` | **Zero-based** indexes of the correct options. One index for single choice; two or three for multiple response. |
| `explanation` | Explanation shown in the answer review. |

### Native simulator format

```js
{
  domain: 3,                 // 1 = Defender XDR, 2 = Defender for Cloud, 3 = Sentinel
  q: "Question text",
  code: { lang: "KQL", text: "SigninLogs | take 10" },   // optional
  options: ["Option A", "Option B", "Option C", "Option D"],
  answer: [1],               // zero-based indexes
  explanation: "Why this is correct."
}
```

Whether a question is single choice or multiple response is determined by the number of entries in `answer` / `correctAnswers`. For multiple-response questions, state the number of selections in the question text, for example *"(Select two.)"*.

## Scoring rules

- **Score** = correct answers ÷ total questions × 1000.
- **Passing score in this simulator:** 850 / 1000. This is intentionally stricter than the official Microsoft passing score of 700, to build a safety margin.
- **Multiple-response questions** require *all* correct options and *no* incorrect ones. There is no partial credit.
- Unanswered questions count as incorrect.
- Time limit: 120 minutes. The timer turns red during the last 10 minutes.

You can change the passing score, exam length or domain names by editing the constants `PASS_SCORE`, `EXAM_SECONDS` and `DOMAINS` near the top of the script section in `sc200-simulator.html`.

## Score report and answer review

When the exam ends, the report shows:

- Your score, pass / fail result, number of correct answers and time spent.
- A per-domain performance table with progress bars.
- An **Answer review** section covering **all** questions:
  - **Question map:** one tile per question, green for correct and red for incorrect or unanswered. Click a tile to jump to that question.
  - **Filters:** *All*, *Incorrect*, *Correct* and *Marked for review*, each with a counter.
  - **Per-question detail:** status, domain, question type, code snippet, every option with its result, and the explanation.
  - **Option markers** (not color alone):
    - `✓` on a green row: the correct option you selected.
    - `✓` on a dashed green row: a correct option you missed (useful for multiple-response questions).
    - `✗` on a red row: an option you selected that was wrong.

Use **Print / Save as PDF** to export the report (the active filter is what gets printed), or **Copy AI Diagnostic Prompt** to get a ready-made prompt for an AI study coach.

## Project structure

A suggested repository layout:

```
.
├── sc200-simulator.html          # The simulator (HTML + CSS + JS in one file)
├── questions/
│   ├── exam1-Foundational_Core_Concepts.json
│   ├── exam2-Intermediate-KQL-Defender-XDR.json
│   ├── exam3-Microsoft_Sentinel_Architecture_Ingestion.json
│   ├── exam4-Advanced_Threat_Hunting_Automation__SOAR_.json
│   ├── exam5-Complex_Case_Studies_Incident_Response.json
│   └── exam6-Master_Boss_Level__Exam_Readiness_.json
├── README.md
└── LICENSE
```

## Browser support and privacy

- Works in current versions of Chrome, Edge, Firefox and Safari.
- The simulator runs **entirely in your browser**. It makes no network requests, has no analytics and sends no data anywhere.
- Exam progress lives only in the open tab and is lost on refresh. Only imported question banks are stored in `localStorage`.
- The *Copy AI Diagnostic Prompt* button copies text to your clipboard; nothing is transmitted unless you paste it into a tool yourself.

## Content accuracy and AI disclosure

All content in this repository, including the simulator code, the 360 questions, the answer keys and the explanations, was generated with the help of AI tools. Because of that:

- Some questions may have a wrong or debatable answer key, ambiguous wording, or distractors that are partially correct.
- Microsoft products, feature names, licensing and limits change often. A question that was accurate when written may be outdated today.
- The explanations in some banks are short and do not cover every distractor.
- The questions follow the public SC-200 skills outline but are **not** copied from the real exam, and passing this simulator does not guarantee passing the certification.

If you find a mistake, please [open an issue](../../issues) with the exam number, the question `id`, what you believe is wrong, and a link to the relevant Microsoft Learn page. Pull requests with fixes are even better.

## Contributing

Contributions are welcome, especially:

- Corrections to questions, answers or explanations.
- Updates when Microsoft changes product names, features or the SC-200 skills outline.
- New questions in the [bank format](#bank-format-used-by-the-files-in-this-repository).

When opening an issue or pull request about a question, please include the exam number, the question `id` and a link to the relevant [Microsoft Learn](https://learn.microsoft.com/) documentation. Each pull request that adds questions should keep the format valid: `correctAnswers` indexes must be in range, and the number of correct answers must match "Select two / three" in the question text.

## License

This project is released under the [MIT License](LICENSE).
