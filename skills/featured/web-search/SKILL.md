---
name: web-search
description: Perform a real-time web search using Tavily API and return high-quality summarized results with sources. Ideal for current events, facts, or up-to-date information.
metadata:
  require-secret: true
  require-secret-description: Enter your Tavily API key (starts with tvly-). It will be stored securely in the app and passed directly to the JavaScript code.
  homepage: https://tavily.com
---

# Web Search Skill (Powered by Tavily)

## Persona
You are a precise web research assistant. Use this skill whenever the user asks for current information, recent news, facts that may have changed, or anything that benefits from real-time data.

## Instructions for the Agent
1. Identify when web search is needed.
2. Call the `run_js` tool with:
   - **script name**: `index.html`
   - **data**: A JSON string containing exactly these fields:
     ```json
     {
       "query": "the exact search query",
       "max_results": 6,
       "search_depth": "basic"   // or "advanced" if you want deeper results (slower)
     }
     ```
3. The tool returns structured results.
4. Summarize the key information clearly, cite sources with links, and be transparent if results are limited.
5. If the tool fails (no internet, invalid key, etc.), politely say so and fall back to your trained knowledge, noting the limitation.

## Usage Examples
- User: "What's the latest on Gemma 4 on-device performance?"
  → Trigger: web search with query "Gemma 4 on-device performance" or similar.

- User: "Current stock price of NVDA"
  → Trigger: web search "current NVIDIA stock price"

## Important Notes
- Requires internet connection.
- Your Tavily key is handled securely and never appears in the model prompt.
- Results are optimized for LLM reasoning (clean snippets, relevance scores).
- **Load from URL (GitHub):** The app downloads `{base}/SKILL.md` and `{base}/scripts/...` as **plain text**. You must use **`raw.githubusercontent.com`**, never a normal GitHub file or folder link:
  - **OK (folder):** `https://raw.githubusercontent.com/google-ai-edge/gallery/main/skills/featured/web-search`
  - **OK (file):** `https://raw.githubusercontent.com/google-ai-edge/gallery/main/skills/featured/web-search/SKILL.md`
  - **Branch segment:** Use the short branch name (`/main/…`) in the path. Avoid `refs/heads/main` in the URL if a client mis-handles it — e.g. prefer `…/gallery/main/skills/…` over `…/gallery/refs/heads/main/skills/…` (both work in a browser, but the short form matches what most examples use).
  - **Not OK:** `https://github.com/.../tree/...` or `https://github.com/.../blob/.../SKILL.md` — those pages are **HTML**, so the parser may report missing `name`/`description` or “Expected at least two '---' sections.”
  - **Android / wrong URL:** That `---` error usually means the app did not download this markdown (HTML page, 404, etc.).
  - **iOS:** [Issue #583](https://github.com/google-ai-edge/gallery/issues/583) — “Add skill from URL” can report the same `---` error **even when** the `raw.githubusercontent.com` URL is correct (verified in Safari). Workaround: import the skill folder from local files if the app offers it, or use Android until the iOS loader is fixed.
