# Microsoft Style Practice Exam

A fully self-contained, single-file HTML tool that emulates the Microsoft certification exam experience. No server, no dependencies, no install — just open the file in a browser and load your question bank.

---

## Features

- **6 question types** — Single choice, Multiple choice, Fill/Dropdown, Yes/No, Order (drag), Matching
- **Timed exam** — Configurable duration with a countdown bar
- **Microsoft-style scoring** — Scaled score (0–1000), configurable pass threshold
- **Review mode** — After the exam, the side panel shows correct/incorrect per question and clicking any dot scrolls directly to that question's feedback
- **Persistent history** — Attempts saved in browser `localStorage`, grouped by exam title
- **CSV export** — Export any attempt or the full history for external analysis
- **No dependencies** — Pure HTML + CSS + vanilla JS, one file

---

## Usage

1. Download `ms-practice-exam.html`
2. Open it in any modern browser
3. Load a question bank file (`.txt` or `.pipe` format)
4. Configure number of questions, time, max score and pass threshold
5. Click **Start Exam**

---

## Question bank format

One question per line, pipe-delimited (`|`). A `TITLE:` line at the top sets the exam name in the UI and in exported filenames.

```
TITLE:"AZ-900 Azure Fundamentals"
ID:01|TYPE:"SINGLE"|QUESTION:"..."|OPTIONS:"A","B","C","D"|ANSWER:"B"|EXPLANATION:"..."|POINTS:1
```

### Fields

| Field | Required | Notes |
|---|---|---|
| `ID` | No | Unique identifier; auto-generated if omitted |
| `TYPE` | Yes | See question types below |
| `QUESTION` | Yes | Question text |
| `OPTIONS` | Depends | Comma-separated quoted strings |
| `ANSWER` | Yes | Correct option(s) |
| `EXPLANATION` | No | Shown in review mode |
| `POINTS` | No | Defaults to `1` |

Lines starting with `#` are treated as comments and ignored.

---

## Question types

### SINGLE
One correct answer from four options.
```
ID:01|TYPE:"SINGLE"|QUESTION:"What is Azure?"|OPTIONS:"A cloud platform","An OS","A browser","A database"|ANSWER:"A"|EXPLANATION:"Azure is Microsoft's cloud computing platform."|POINTS:1
```

### MULTI (or MULTIPLE)
Multiple correct answers. State the count in the question text.
```
ID:02|TYPE:"MULTI"|QUESTION:"Which are Azure compute services? (Choose 2)"|OPTIONS:"Azure VMs","Azure Blob","Azure Functions","Azure DNS"|ANSWER:"A","C"|EXPLANATION:"VMs and Functions are compute services."|POINTS:1
```

### FILL (also: DROPDOWN, CLOZE, COMPLETE)
The question must contain `{{BLANK}}` where the dropdown appears.
```
ID:03|TYPE:"FILL"|QUESTION:"Azure {{BLANK}} provides scalable object storage."|OPTIONS:"Blob Storage","SQL Database","Cosmos DB","Table Storage"|ANSWER:"A"|EXPLANATION:"Blob Storage is Azure's object storage solution."|POINTS:1
```
> If `{{BLANK}}` is missing, the question is automatically converted to SINGLE or MULTI.

### YESNO (also: SINO, TRUEFALSE, TF)
Three statements, each answered Yes or No. All-or-nothing scoring.
```
ID:04|TYPE:"YESNO"|QUESTION:"Regarding Azure SLAs:"|STATEMENTS:"Free tier includes SLA","99.9% SLA requires redundancy","SLAs are the same for all services"|ANSWER:"No","Yes","No"|EXPLANATION:"SLAs vary by service and tier."|POINTS:1
```

### DRAG (also: ORDER, SORT, ORDENAR)
Items are presented shuffled; the user must place them in the correct order. All-or-nothing scoring.
```
ID:05|TYPE:"DRAG"|QUESTION:"Order the steps to deploy an Azure VM:"|ITEMS:"Choose a region","Select VM size","Configure networking","Review and create"|EXPLANATION:"This is the correct deployment sequence."|POINTS:1
```

### MATCH (also: MATCHING)
Match each statement on the left to an option on the right.
```
ID:06|TYPE:"MATCH"|QUESTION:"Match each service to its category:"|STATEMENTS:"Azure Blob","Azure SQL","Azure VMs","Azure DNS"|OPTIONS:"Compute","Storage","Database","Networking"|ANSWER:"B","C","A","D"|EXPLANATION:"Each service belongs to a different category."|POINTS:1
```

---

## Scoring

The raw score (earned points / total points) is scaled linearly to the configured maximum:

```
Scaled Score = round( (earned / total) × maxScore )
```

Default configuration mirrors Microsoft exams: **45 questions · 45 minutes · 1000 max · 700 pass threshold**.

---

## History & export

- All attempts are stored in `localStorage` under the key `msexam_attempts_v1`
- The history panel groups attempts by exam title
- **Export attempt** — downloads a CSV with per-question detail (question, type, user answer, correct answer, result, points, explanation)
- **Export history** — downloads a summary CSV for all stored attempts

---

## Browser compatibility

Any modern browser (Chrome, Edge, Firefox, Safari). No internet connection required after the file is downloaded.

---

## License

MIT — free to use, modify and distribute.
