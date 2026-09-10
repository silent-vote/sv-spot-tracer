---
name: spread
description: Trace a spot's company spread — customers, suppliers, affiliates — into an evidenced company list in spreads/.
disable-model-invocation: true
---

Take the spot filename (or spot id) the user passes. Map every company the spot's harm spreads to, archive the evidence, and record the result as `spreads/<spot id>.md` — the filename mirrors the spot's. Follow the steps in order.

## 1. Read the seed

Resolve the input to a file in `spots/` (by exact filename, spot id, or unique match on either; ask the user when ambiguous). Extract: the hub company (the violator — full name and abbreviations), the event name, 事件时期, and the spot's evidence sources `[S1]…[Sn]` with their snapshot URLs. The hub itself is never a spread entry; it heads the file as 源事件.

## 2. Map the network

Seed from companies the spot record already names, then research fresh — same stack as the spot skill: `webfetch` static search endpoints first (e.g. `https://cn.bing.com/search?q=<公司名> 客户 供应商`), escalate to `chrome-devtools` for JS-heavy or blocked pages. Dig for the hub's:

- direct customers (下游客户)
- upstream suppliers (供应商)
- parent / subsidiary / sibling companies (关联公司)

Rules of the map:

- **Companies only.** Regulators, stock exchanges, media are not entries — even when central to the event.
- Follow **customer edges** onward (violator → its customer → that customer's customer…) until the recognizable end/brand customer is reached; every hop must be sourced. Stop there: do not expand a customer's own suppliers or unrelated subsidiaries.
- Affiliates list one hop from any already-listed company only.
- Every edge must trace to a source URL: annual report / prospectus 主要客户・供应商 disclosures, official statements, corporate registries, credible investigative articles. Cross-read two independent outlets where a single source seems doubtful.
- No entry cap; the evidence gates below are the limit. A documented-but-dropped company (step 5 fails) may be mentioned in another company's 概述 instead of getting an id.

## 3. Assign company ids

Ids are global across `spreads/`: `c{xxxx}`, 4-digit zero-padded. Grep every spread file for each company's name and known aliases (Chinese, English, 曾用名) — a company already numbered keeps its existing id; otherwise assign highest existing + 1. Never renumber or reuse an existing id. No cross-references between spreads: the shared id is the link.

## 4. Select evidence per company

1–3 sources per company. The relation edge must be established by a source; where a source shows the company reacting to or being affected by this event, prefer it as [E1]. Same bar as the spot's critical sites: primary sources — filings, official statements, major investigative articles.

## 5. Archive each evidence source

Per URL, in order — identical to the spot skill's procedure:

1. `GET https://archive.org/wayback/available?url=<url>` — if a snapshot exists, reuse its timestamped URL.
2. Otherwise `GET https://web.archive.org/save/<url>` (public Save Page Now, no auth, takes ~20–60s); wait for completion and read the capture URL from the response / status endpoint.
3. Cite the timestamped form: `https://web.archive.org/web/<YYYYMMDDhhmmss>/<original-url>`.
4. A page that cannot be archived (rate limit, login-wall, error) is dropped — no snapshot, no evidence.

Snapshots already archived by the spot itself may be reused instead of re-archiving: quote the spot's timestamped URL and tag the evidence line `借用 s{xxxx} [S#]`.

A company left with zero archived sources is dropped from the list entirely — no snapshot, no listing.

Write down only URLs the APIs actually returned; every snapshot URL in the file must come from a real response.

## 6. Write the record

If `spreads/<spot id>.md` exists, update append-only: existing entries and lines stay untouched; add newly found companies with fresh ids, append evidence to existing entries, refresh the banner's 检索日期.

Otherwise create the file **in Chinese** (AGENTS.md exempts `spreads/` records from the English rule), on this template — flat entries in id order, no overview section:

```markdown
> 源事件：s0001 <事件名称> · 检索日期：<YYYY-MM-DD>

# s0001 公司波及清单

## c0001 · <公司名称（全称＋常用简称）>

- 别名：<中英文/曾用名，逗号分隔>
- 关系：<客户 / 供应商 / 关联>（相对谁、什么角色）
- 概述：1–3 段：它在链条中的位置、如何被本事件波及、目前的动作或表态。
- 证据：
  - [E1] <来源名 / 标题> — 原文：<url> — 快照：<timestamped archive.org url>
  - [E2] <来源名 / 标题> — 借用 s0001 [S5] — 快照：<timestamped archive.org url>

## c0002 · …
```

## 7. Report

Output the file path, every company with its id and snapshot URLs, the companies dropped for lack of archivable evidence, and — on a re-run — what was appended. Leave the file uncommitted for human review.
