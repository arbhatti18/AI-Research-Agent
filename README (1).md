# AI Research Agent

AI Research Agent is a chat-based assistant that researches any topic for you. It searches the web with Brave Search, reads the best sources in full, cross-checks key facts, and runs calculations when needed. You get a clean, well-organized report with summary, tables, inline citations and source links, in whatever language you ask in.

Built with [n8n](https://n8n.io) and Claude.

---

## Features

- **Deep research:** runs several searches from different angles, then reads the most credible pages in full instead of relying only on snippets.
- **Cross-checking:** verifies important numbers and claims against a second independent source.
- **Clean reports:** summary, headings, tables for comparisons, inline citations like `[1]`, and a numbered Sources list with links.
- **Any language:** replies in the language you write in (English, Urdu, Roman Urdu, Arabic, and more).
- **Conversation memory:** remembers the last 8 messages, so you can ask follow-up questions.
- **Safe by design:** treats web content as untrusted data and ignores instructions found inside pages.

---

## How it works

```
Research Chat  -->  Research Agent
                       |-- Claude Model       (reasoning and writing)
                       |-- Chat Memory        (last 8 messages)
                       |-- Brave Web Search   (find sources)
                       |-- Fetch Web Page     (read full pages)
                       `-- Calculator         (accurate math)
```

| Node | Type | Purpose |
|------|------|---------|
| Research Chat | Chat Trigger | Chat window where you ask questions |
| Research Agent | AI Agent | Plans the research, calls tools, writes the report |
| Claude Model | Anthropic Chat Model | Claude Sonnet 5, up to 8,000 output tokens |
| Chat Memory | Window Buffer Memory | Keeps the last 8 messages per session |
| Brave Web Search | Brave Search Tool | Returns up to 8 results per query |
| Fetch Web Page | HTTP Request Tool | Reads a page (up to 12,000 characters, scripts and menus removed) |
| Calculator | Calculator Tool | Growth rates, totals, percentages |

Tools are used only when needed. For example, a simple fact may need only a search, so Fetch Web Page and Calculator stay idle.

---

## Setup

1. **Create or import the workflow** in your n8n instance (name: `AI Research Agent`).
2. **Add credentials:**
   - **Anthropic API** for the Claude Model node.
   - **Brave Search API** for the Brave Web Search node.
   - On n8n Cloud, Gateway credits can cover both, so no separate keys are needed.
3. **Fetch Web Page** needs no credentials. It reads public pages only.
4. **Publish** the workflow.
5. Click **Open chat** (or use the hosted chat page) and ask a question.

---

## Example questions

- `Who is the current CEO of Netflix? Give the source URL.`
- `Summarize this page in 5 bullets: https://about.netflix.com/en/leadership`
- `What is the size and growth of the electric bike market in Pakistan? Include top companies, risks, and a comparison table.`
- `A market is worth 2 billion dollars and grows 18% per year. What will it be in 5 years?`

---

## Output format

For research questions, the agent writes:

1. **Summary** (3-5 bullets answering the question)
2. **Sections** with headings and short paragraphs
3. **Tables** for comparisons
4. **Inline citations** such as `[1]`, `[2]`
5. **Sources** list with titles, URLs, and dates
6. **Limitations** and a **Recommendation** when relevant

Simple questions get short, direct answers.

---

## Customization

| What to change | Where |
|----------------|-------|
| Behavior, tone, report format | Research Agent > Options > System Message |
| Model | Claude Model node |
| Memory length | Chat Memory > Context Window Length |
| Number of search results | Brave Web Search > Count |
| Page reading length | Fetch Web Page > Max Length |
| Maximum agent steps | Research Agent > Options > Max Iterations (currently 25) |

---

## Limitations

- Fetch Web Page cannot read pages that need JavaScript to load, require a login, or sit behind a paywall. In that case the agent relies on search snippets.
- Brave Search results default to English, so very local topics in other languages may return fewer results.
- Deep research can take 1 to 3 minutes and uses more tokens than a simple question.
- Answers depend on what the web sources say. Check important decisions against the cited sources.
- n8n's debug Output panel shows raw text with `\n` markers. Read the answer in the Chat panel, where markdown renders properly.

---

## License

Use and modify freely for your own projects.
