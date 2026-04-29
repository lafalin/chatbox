# Chatbox

A chat UI for any LLM, in one HTML file. Bring your own key — or run a local model.

- **No backend.** The whole app is one static page. Nothing you type leaves your browser except the request you make to the model provider you chose.
- **No telemetry, no analytics, no third-party scripts** beyond Google Fonts (which loads the typeface — you can self-host if you'd rather not).
- **Your key stays on your device.** It's read directly from the form into `localStorage` (or kept in memory only for the tab if you uncheck "Save on this device"). The author of this repo cannot see your key or your conversations.

## Try it

Open `index.html` in any modern browser. That's it.

To deploy: it's a single file, so any static host works (GitHub Pages, Netlify, Cloudflare Pages, Vercel, an S3 bucket, your laptop). No build step.

## Supported providers

| Provider | Endpoint | Notes |
|---|---|---|
| Anthropic | `https://api.anthropic.com/v1/messages` | Uses `anthropic-dangerous-direct-browser-access` to allow direct browser calls. |
| OpenAI | `https://api.openai.com/v1/chat/completions` | Standard streaming chat completions. |
| Google | `https://generativelanguage.googleapis.com/v1beta/...` | Gemini streaming via SSE. |
| Ollama | `http://localhost:11434/api/chat` | Local. Set `OLLAMA_ORIGINS=*` (or this page's origin) before running `ollama serve`, otherwise the browser CORS check blocks you. |
| Custom | Anything OpenAI-compatible | Provide a Base URL ending before `/chat/completions`. |

## Features

- Multi-provider, BYOK (Anthropic, OpenAI, Google, Ollama, custom)
- Streaming responses
- Threading: highlight any phrase in an assistant reply → branch a sub-thread that quotes it
- Side Quests: highlight → save with a note for later
- Recent chats with a tree of threads, all stored in `localStorage`
- System prompt configurable in Settings
- Cmd/Ctrl + `,` opens Settings
- Stop / regenerate
- "Forget everything" button that wipes all local data including the key

## Privacy

Open DevTools → Network. The only outbound calls go to the provider's domain (or `localhost` for Ollama). There is no proxy in between.

If you don't trust that claim:

1. Read `index.html` — there is one `<script>` block, all logic is in there.
2. Search for `fetch(` — every call site is to a provider URL.
3. Check `localStorage` in DevTools to see exactly what's stored, in plain JSON.

Risks worth knowing:
- Anyone with access to your browser profile can read `localStorage`. Use a key with the smallest scope you need.
- Browser extensions can read this page like any other site. Only trust extensions you'd trust with a password.
- The provider you connect to (Anthropic, OpenAI, Google, etc.) sees your prompts under their privacy policy.

## License

MIT. See [LICENSE](LICENSE).

## Credits

Design + frontend: yours. Built without a server, on purpose.
