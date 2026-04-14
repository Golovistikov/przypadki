# Trener Polskiego 🇵🇱

<img src="assets/icon.png" alt="App icon" width="180">

A free, open-source Progressive Web App for practising Polish grammar on the way to the **B1 exam**. No account, no install required — open it in a browser and start drilling.

**Live → [przypadki.com](https://www.przypadki.com)**

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![PWA](https://img.shields.io/badge/PWA-ready-brightgreen)
![Deploy](https://github.com/Golovistikov/przypadki/actions/workflows/deploy.yaml/badge.svg)

---

## Screenshots

| Przypadki | bym / byś / by |
|---|---|
| ![Przypadki – wrong answer](Screenshorts/Trener%20Polskiego%202.png) | ![bym/byś/by – question](Screenshorts/Trener%20Polskiego%203.png) |

| Czas przeszły – ą/ę | Czas przyszły – dokonany |
|---|---|
| ![Czas przeszły – choice](Screenshorts/Trener%20Polskiego%204.png) | ![Czas przyszły – correct answer](Screenshorts/Trener%20Polskiego.png) |

---

## What it covers

| Module | What you practise |
|---|---|
| **Przypadki** | The 6 noun cases — nominative, genitive, dative, accusative, instrumental, locative |
| **bym / byś / by** | Conditional mood — verb conjugation and connector usage |
| **Czas przeszły** | Past tense — masculine/feminine endings, **e→a** verbs (widzieć, musieć, umieć), **ą→ę** verbs (wziąć, zamknąć, zacząć) |
| **Czas przyszły** | Future tense — picking the right form for **dokonany** vs **niedokonany** verbs |

Each module has two interaction modes:

- **Wybór** — pick the correct form from four options
- **Wpisanie** — type the answer yourself

Stats (success rate, correct, errors) are tracked live within the session.

---

## Contributing

Everyone is welcome to open a pull request. The most impactful contribution is **adding new questions** — no coding knowledge needed beyond editing a JSON file.

### Branching strategy

```
main          ← production, always deployable
│
develop       ← integration branch; all features merge here first
│
feature/*     ← one branch per feature or question set, branched from develop
```

| Branch | Deploys to | URL |
|---|---|---|
| `main` | Production | https://www.przypadki.com |
| `develop` | Development | set in `vars.DEV_URL` (GitHub repo variable) |
| `feature/*` | — | no automatic deployment; open a PR → `develop` |

**Workflow for contributors:**

1. Branch off `develop`: `git checkout -b feature/my-new-module develop`
2. Make changes (typically just `data.json`, occasionally `index.html`)
3. Open a PR targeting **`develop`**
4. After review and merge, `develop` auto-deploys to the dev environment for a final check
5. A maintainer merges `develop` → `main` to release to production

### Adding questions

All questions live in [`data.json`](data/data.json). Each entry follows this shape:

```json
{
    "title": "Dopełniacz (kogo? czego?)",
    "number": "sing",
    "context": "Szukam ___.",
    "baseWord": "(zły humor)",
    "answer": "złego humoru",
    "options": ["złego humoru", "zły humor", "złym humorze", "złych humorów"]
}
```

| Field | Description |
|---|---|
| `title` | Grammar concept shown above the question |
| `number` | Shown in the badge (e.g. `sing`, `plur`, `1 os. lp`, `on`, `ja (f)`) |
| `context` | Sentence with `___` marking the blank |
| `baseWord` | Hint shown to the user (usually the dictionary form in parentheses) |
| `answer` | The single correct answer |
| `options` | Exactly 4 options — must include the correct answer |

To add a new training **module**, add a new top-level key to `data.json` and a corresponding tab button in `index.html`.

### Guidelines

- Wrong `options` should be plausible mistakes, not obviously incorrect — that's what makes the drill useful.
- Aim for at least 8 questions per module so the wait-list rotation feels natural.
- Test in a browser served over HTTP (e.g. `npx serve .`) — `file://` won't load `data.json`.

---

## Tech stack

| Layer | Details |
|---|---|
| Frontend | Vanilla HTML / CSS / JavaScript — no framework, no bundler |
| PWA | `manifest.json` — installable on iOS and Android home screens |
| Hosting | Azure Storage static website |
| CI/CD | GitHub Actions → `az storage blob upload-batch` on push to `main` or `develop` |

There is no build step. Edit files, push, done.

---

## Local development

```bash
npx serve .
# open http://localhost:3000
```

Any static file server works. The only requirement is serving over HTTP so that `fetch('data.json')` succeeds.

---

## License

[MIT](LICENSE) — free to use, modify, and distribute.
