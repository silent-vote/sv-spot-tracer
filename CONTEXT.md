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
