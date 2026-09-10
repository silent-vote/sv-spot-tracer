# CONTEXT

Glossary for SpotTracer. Outputs that name these concepts use them as defined here.

## Language

- **Spot** — one violation event, researched and recorded as a single evidence document in `spots/`. The event name (事件名称) the user supplies is the handle for the spot.
- **Spot id** — `s{xxxx}-{violation-type}-{event-slug}`, doubling as the filename `spots/<spot id>.md`. `{xxxx}` is the 4-digit zero-padded order number (highest existing + 1).
- **Violation-type** — the kebab-case category of a spot, derived by the agent from the event (`employment`, `wage-theft`, `privacy`…). Free-form; there is no fixed taxonomy.
- **Event-slug** — 2–4 kebab-case words pinning one event within its violation-type, so same-type spots stay distinguishable.
- **Critical site** — a spot's primary sources: official statements, regulator/police announcements, the originating post or first report, major investigative articles. At most 5 per spot. Only critical sites get archived and count as evidence.
- **Evidence source** — an archived critical site, numbered `[S1]`, `[S2]`… in the spot record. A page without a snapshot URL is not evidence.
- **Snapshot URL** — the timestamped Wayback Machine address (`https://web.archive.org/web/<YYYYMMDDhhmmss>/<original>`) returned by the archive APIs. Never hand-written.
- **Verified** — a spot whose core facts are confirmed by ≥2 mutually independent credible sources; otherwise its banner carries the `⚠️ 未经核实` flag.
- **Spread** — the set of companies a spot's harm reaches, mapped from the hub company along sourced customer/supplier/affiliate relations and recorded as one company list in `spreads/`. One spread per spot; the filename mirrors the spot id. Companies only — regulators, exchanges, and media are never entries; the hub itself is not an entry.
- **Company id** — global `c{xxxx}`, 4-digit zero-padded, numbering shared across all of `spreads/` (highest existing + 1). A company appearing in several spreads keeps one id; ids are never renumbered or reused. The shared id is the only cross-spread link — spreads carry no cross-references.
- **Relation (edge)** — why a company is in a spread: 客户 (customer), 供应商 (supplier), or 关联 (parent/subsidiary/sibling), each established by at least one source. Customer edges are followed onward until the end/brand customer is reached; nothing else expands past the listed companies.
- **Spread entry** — one company's record in a spread: id, 名称, 别名, 关系, 概述, and 1–3 evidence sources. A company with zero archived evidence is dropped — no snapshot, no listing.
- **Evidence source [E#]** — an archived source inside a spread entry, numbered per entry. Its relation edge must be sourced; impact evidence is preferred. An evidence line may cite a snapshot the spot already archived, tagged `借用 s{xxxx} [S#]`.
