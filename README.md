# Case Rehearsal

A small offline web app (PWA) for rehearsing a psychiatry case presentation and the Q&A that follows.

**Live:** https://isaymotion.github.io/102526/

## What it does

- **Flashcards:** likely questions by category. Tap to reveal the answer, tip and reference, then mark *Got it* or *Review again* (simple Leitner boxes with New / Review / Got it counts).
- **Grill mode:** random questions from the categories you pick, with a countdown (30/45/60/90 s). When time runs out the screen flashes, the phone vibrates (if supported) and a short beep plays. You keep a session score.
- **Presentation timer:** per-section minute budgets and the overall total. Colors show progress: green, then amber in the last 20%, then red when over. The current section's speaker notes are shown large, like a teleprompter. The screen stays awake while the timer runs, where the browser supports it.
- **Notes / script:** every section with its notes. Use A− / A+ to change the text size.
- **Key numbers:** labs, doses, dates and scores as compact cards.
- **Search:** searches questions, answers, tips, speaker notes and key numbers.
- Light, dark or automatic theme. Back and forward buttons. Works offline after the first visit.

Add it to the iPhone or iPad Home Screen with Safari's **Share → Add to Home Screen**.

## Loading your case

The app ships only with a **fictional demo case** ("Juan"). To rehearse your own case:

1. Write your case as a `.json` file using the schema below. Start from [`demo-case.json`](demo-case.json).
2. Open the app, go to **Case**, tap **Open case file**, and choose your file.
3. Tap **Clear case** when you are done.

### Privacy

Your case file is read **on your device only**. It is stored in this browser's local storage and is **never uploaded** to any server, including this repository or GitHub Pages. Do not commit real case files to this public repository. Use initials or a pseudonym in your case file, as your consultants require.

## Case file schema (v1)

```json
{
  "v": 1,
  "title": "string",
  "presenter": "string",
  "date": "string",
  "sections": [
    { "id": "s1", "title": "Identifying data", "minutes": 1.5, "notes": ["speaker note bullet", "..."] }
  ],
  "qa": [
    { "id": "q1", "cat": "Diagnosis", "q": "question", "a": ["answer bullet", "..."], "tip": "optional delivery tip", "ref": "optional reference" }
  ],
  "numbers": [
    { "group": "Labs", "items": [ { "label": "Potassium", "value": "2.6 → 3.38 mmol/L", "note": "optional" } ] }
  ]
}
```

| Field | Rules |
| --- | --- |
| `v` | Must be `1`. |
| `title` | Required, non-empty. `presenter` and `date` are optional strings. |
| `sections[]` | Each needs a unique `id`, a `title`, `minutes` (a number of 0 or more, decimals allowed) and `notes` (a list of strings). The total of all `minutes` is the presentation budget. |
| `qa[]` | Each needs a unique `id`, `cat` (category, used for the filters), `q` and `a` (a list of strings). `tip` and `ref` are optional. |
| `numbers[]` | Optional. Groups with a `group` name and `items`, each with `label` and `value` and an optional `note`. |

When a file doesn't match the schema, the app lists what to fix.

## Storage keys

`rehearse:v1:case` (case), `rehearse:v1:progress` (flashcard boxes and timer times), `rehearse:v1:theme`, `rehearse:v1:prefs` (text size and grill seconds).

## Development

Everything is static: `index.html` (inline CSS and JS, no external requests), `sw.js` (cache-first service worker), `manifest.webmanifest`, icons and `demo-case.json`. When you change any file, bump `VERSION` in **both** `index.html` and `sw.js` so installed copies update.

---

Web app developed in 2026 by Isabella Navarro, MD · isaymotion@gmail.com
