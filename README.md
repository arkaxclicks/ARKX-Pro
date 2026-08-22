# ARKX Backend Upgrade

This is a drop-in replacement for your backend. Your supplied `index.html` was not edited.

## What it adds

- Per-browser, in-memory chat context, bounded to the most recent messages so requests stay reliable.
- Vision preprocessing: orientation correction, a 2,048 px cap, and high-quality WebP compression before a vision request.
- Complete current Unicode emoji catalog fetched daily from Unicode. Google Noto Emoji uses the same Unicode characters; their visual styling is controlled by the browser/device, not the server.
- Live Wikipedia extracts for factual questions.
- Optional live Google Search snippets for fresh topics, memes, trends, and news — using the official Custom Search API, not scraping Google.
- Exact numeric calculations plus basic algebraic equation solving.
- `/health`, `/emojis`, and a future-ready `/chat/new` endpoint.

## Install and Run

Place the supplied frontend unchanged at `frontend/ui.html`, preserve the existing folder layout, then run:

```powershell
npm install
Copy-Item .env.example .env
# Put your GROQ_API_KEY in .env
npm start
```

The current frontend already sends its request to `https://arkx-pro.onrender.com/chat`, so deploy this backend to that same Render service. Its response format remains the same streamed SSE `{ "text": "..." }` format.

## Important Chat-Reset Limitation

The supplied HTML's **New chat** button clears only the visible messages. It makes no network call and sends no chat ID, so a backend cannot know that it was clicked. Therefore, this backend remembers the browser's current chat until its session expires or `/chat/new` is called.

To make **New chat** erase backend context exactly when the button is clicked requires one small frontend call to `POST /chat/new`. That change was not made here, since you explicitly asked not to modify the frontend.

## Google Setup

Google's official Custom Search JSON API requires both an API key and a Programmable Search Engine ID (`cx`). Add them to `.env`; Google retrieval otherwise stays disabled and Wikipedia still works. Do not put either credential in the frontend.

## Memory and Privacy

Chat context is held only in the running server's memory and expires after 24 hours by default. It disappears when the server restarts. For memory that survives deployment restarts or works across multiple server instances, connect a database/Redis store; that needs your choice of provider and credentials.
