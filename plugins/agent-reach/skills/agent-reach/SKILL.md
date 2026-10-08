---
name: agent-reach
description: Read or search the internet via the Agent Reach CLI and its upstream tools — any web page, YouTube transcripts and search, GitHub repos/issues, RSS feeds, Exa web search, Twitter/X, Reddit, Bilibili, Facebook, Instagram, LinkedIn. Use when the user asks to read a link, summarize a video, search a platform, or install/fix Agent Reach.
---

# Agent Reach

Agent Reach is a selector, installer and health checker. It does **not** wrap the platforms — after setup, call the upstream tools directly.

## 1. Check it is installed

```bash
agent-reach doctor
```

If the command is missing, install it (prefer pipx; use a venv if pip hits PEP 668):

```bash
pipx install https://github.com/Panniantong/agent-reach/archive/main.zip
agent-reach install --env=auto            # read-only check (safe default)
```

Only after the user explicitly approves system changes:

```bash
agent-reach install --env=auto --system
agent-reach install --env=auto --system --channels=twitter,reddit   # optional channels the user named
```

Do not install the PyPI package called `agent-reach` — it is a different project.

## 2. Use the upstream tools

| Need | Command |
|------|---------|
| Read any web page | `curl -s "https://r.jina.ai/<URL>"` |
| YouTube info / subtitles | `yt-dlp --dump-json <URL>` · `yt-dlp --write-auto-subs --skip-download <URL>` |
| GitHub | `gh repo view owner/repo` · `gh search repos "query"` · `gh issue list -R owner/repo` |
| RSS | `python3 -c "import feedparser; f=feedparser.parse('<FEED>'); [print(e.title, e.link) for e in f.entries[:10]]"` |
| Web search (Exa) | `mcporter call exa.web_search_exa query="..." numResults=5` |
| Twitter/X | set `TWITTER_AUTH_TOKEN` and `TWITTER_CT0`, then `twitter search "query" -n 10` |
| Reddit | `opencli reddit search "query" -f yaml` (fallback: `rdt read POST_ID`) |
| Bilibili | `bili search "query" --type video` · subtitles: `opencli bilibili subtitle BVxxx` |
| Facebook / Instagram | `opencli facebook search "query" -f yaml` · `opencli instagram user nasa -f yaml` |
| LinkedIn | `mcporter call linkedin.get_person_profile linkedin_username="..."` |

For multi-backend platforms, `agent-reach doctor --json` shows the `active_backend` to use.

## 3. Credentials and limits

- Twitter, Reddit, Xiaohongshu, Facebook, Instagram need a logged-in session or cookies. Ask the user; recommend a **secondary account** (ban risk). Cookies are stored only in `~/.agent-reach/`.
- Configure with `agent-reach configure twitter-cookies`, `agent-reach configure proxy`, `agent-reach configure groq-key`.
- Never run `sudo`, never write files into the user's project folder, never log in on the user's behalf.
- If a channel shows ❌ in `agent-reach doctor`, follow the fix it prints; ask the user before anything needing elevated permissions.
