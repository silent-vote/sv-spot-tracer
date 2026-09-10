---
name: spot
description: Trace an event (事件名称) into an archived evidence record in spots/.
disable-model-invocation: true
---

Take the event name (spot 名称 / 事件名称) the user passes. Research it, archive its critical sites on archive.org, and record the result as `spots/s{xxxx}-{violation-type}-{event-slug}.md`. Follow the steps in order.

## 1. Research

Search the internet for the event. `webfetch` static search endpoints first (e.g. `https://cn.bing.com/search?q=<事件名称>`, Bing); escalate to the `chrome-devtools` MCP for JS-heavy or blocked pages (weibo, zhihu, newsroom pages).

Dig until the event's shape is clear: what happened, to whom, which violation, every milestone in real chronological order with its date where recoverable, and the current status. Cross-read at least two independent outlets for each core fact.

Done when each core fact traces to at least one source URL and the chronology is assembled.

## 2. Duplicate check

Grep `spots/` for the event name and known aliases. If an existing spot is clearly the same event, **stop and ask**: update that spot, or create a new one.

Update means append-only: add newly found timeline entries and evidence sources, refresh the banner's 检索日期. Existing lines stay untouched.

## 3. Select critical sites

From the sources found, keep only primary sources, at most 5: official company statements, regulator / police announcements, the originating post or first report, major investigative articles. They become the evidence set.

## 4. Archive each critical site

Per URL, in order:

1. `GET https://archive.org/wayback/available?url=<url>` — if a snapshot exists, reuse its timestamped URL.
2. Otherwise `GET https://web.archive.org/save/<url>` (public Save Page Now, no auth, takes ~20–60s); wait for completion and read the capture URL from the response / status endpoint.
3. Cite the timestamped form: `https://web.archive.org/web/<YYYYMMDDhhmmss>/<original-url>`.
4. A page that cannot be archived (rate limit, login-wall, error) is dropped from the evidence set — no snapshot, no evidence.

Write down only URLs the APIs actually returned; every snapshot URL in the file must come from a real response.

## 5. Name the spot

- `{violation-type}` — kebab-case category derived from the event (`employment`, `wage-theft`, `privacy`…); free-form.
- `{event-slug}` — 2–4 kebab-case words pinning this specific event.
- `{xxxx}` — existing highest number in `spots/` plus one, 4-digit zero-padded.

Create the file even if step 4 archived nothing; step 6 flags it.

## 6. Write the record

Write the file **in Chinese** (AGENTS.md exempts `spots/` files from the English rule), on this template:

```markdown
> s{xxxx} · {violation-type} · 事件时期：<YYYY-MM[ 至今] / 日期不详> · 检索日期：<YYYY-MM-DD>
> ⚠️ 未经核实——独立可信信源不足两个

# <事件名称>

## 总结

2–5 段：事件是什么、涉及谁、属于何种违规、目前进展与结果。

## 时间线

- 2026-03-02 —— <发生了什么> [S1]
- 约 2026-04 —— <发生了什么> [S2]
- 日期不详 —— <发生了什么> [S3]

## 证据来源

- [S1] <来源名 / 标题> — 原文：<url> — 快照：<timestamped archive.org url>
```

- Second banner line only when the spot fails verification: verified = ≥2 mutually independent credible sources agree on the core facts (an empty evidence set always fails).
- 时间线 strictly ordered by when things actually happened; approximate dates led with 约, unrecoverable ones with 日期不详.
- Number evidence sources [S1], [S2]… and tag every timeline bullet with the sources supporting it.

## 7. Report

Output the file path, the snapshot URLs, and the verification status. Leave the file uncommitted for human review.
