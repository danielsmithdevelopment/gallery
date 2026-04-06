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
