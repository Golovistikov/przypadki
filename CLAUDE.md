# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Przypadki** is a Polish grammar trainer — a static PWA that helps users practice noun declensions (przypadki) and conditional mood (bym/byś/by). Deployed to Azure Storage at https://www.przypadki.com.

## No Build Step

This is a zero-dependency static site: vanilla HTML/CSS/JS with no package manager, bundler, or test runner. Editing `index.html` is all that's needed to change the app.

## Deployment

Push to `main` triggers `.github/workflows/deploy.yaml`, which uploads `*.html`, `*.json`, and `*.png` files to Azure Storage account `przypaki` using OIDC authentication.

## Architecture

The entire application lives in `index.html` (inline CSS + JS). Key internals:

- **`data.json`** — question data, fetched on load. Two sets: `przypadki` (8 questions on the 6 Polish noun cases) and `tryb` (6 questions on conditional forms). Each entry has `title`, `number`, `context` (sentence with `___` blank), `baseWord`, `answer`, and `options` (4 multiple-choice answers).
- **Two tabs** — "Przypadki" and "bym / byś / by", switching sets the active question pool.
- **Two modes** — "Wybór" (multiple choice, renders 4 buttons) and "Wpisanie" (free text input). Mode toggle rebuilds the answer UI without resetting stats.
- **Stats** — `correctAnswers` / `wrongAnswers` counters update after each submission and render as percentage + raw counts.

When adding new questions, follow the existing `database` entry shape exactly — the rendering code keys off those field names.
