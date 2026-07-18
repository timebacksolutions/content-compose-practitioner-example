# content-compose-practitioner-example

A second worked example of a **composed content project** built with
[throughline-compose](https://github.com/timebacksolutions/throughline-compose) — the
**practitioner-and-neutral** counterpart to
[content-compose-example](https://github.com/timebacksolutions/content-compose-example),
which is general-and-formal.

Same council-housing domain, different reader and register. Where that example is a
tenant-facing **rent-statement help page**, this one is an internal **rent-arrears
procedure note** that a **housing officer** follows to work a case. The officer knows
the housing-management workflow and its everyday vocabulary but not the deep legal
theory — a *practitioner*, not a lay reader and not a specialist. So the note has to be
readable, correctly styled, in the right register, doing the right communicative job
**and** pitched at that reader. Those are independent concerns, so the project adopts
five orthogonal content sources by reference and cites their clauses side by side on the
requirements they satisfy:

| Namespace | Source | Axis |
|---|---|---|
| `plain` | [throughline-plain-language](https://github.com/timebacksolutions/throughline-plain-language) `@v2026-07` | readability |
| `conventions` | [throughline-conventions-uk](https://github.com/timebacksolutions/throughline-conventions-uk) `@v2026-07` | British-English conventions |
| `tone` | [throughline-tone-neutral](https://github.com/timebacksolutions/throughline-tone-neutral) `@v2026-07` | register (neutral) |
| `purpose` | [throughline-purpose-instruct](https://github.com/timebacksolutions/throughline-purpose-instruct) `@v2026-07` | purpose (instruct) |
| `audience` | [throughline-audience-practitioner](https://github.com/timebacksolutions/throughline-audience-practitioner) `@v2026-07` | audience (practitioner) |

## The sibling-swap payoff

Three of the axes are families of **mutually-exclusive sibling sources**: register
(`throughline-tone-formal` / `-neutral` / `-informal`), purpose
(`throughline-purpose-inform` / `-instruct` / `-persuade`) and audience
(`throughline-audience-expert` / `-practitioner` / `-general`).

The tenant help page composes the **formal** register and the **general**-reader
audience. This staff procedure note composes the **neutral** register and the
**practitioner** audience. Moving from that worked example to this one is exactly **two
one-line `ref` swaps** — `tone` and `audience` — while the readability, conventions and
purpose axes are untouched. That is composability: each axis moves independently.

`plain` and `audience` look similar but are independent: `plain` is *universal* clarity
for any reader, while `audience` tunes how much prior knowledge and field vocabulary the
writing may assume for a *specific* reader — here, a housing officer. The note composes
both. Its headline practitioner move is `SR-0006`, which cites `audience:SR-0003`
(use the everyday terms of the trade without defining them) **and** `audience:SR-0004`
(explain the deeper specialist terms) — exactly the practitioner calibration.

Each source numbers its items `SR-0001` upward, yet they never collide because each is
imported under its own namespace (`plain:SR-0026`, `conventions:SR-0008`, `tone:SR-0007`,
`purpose:SR-0005`, `audience:SR-0003`) — the same way a security project composes
`asvs` + `gds` + `wcag`.

## The authored note

The requirements graph is the *spec*; the note it governs is
[`content/rent-arrears-procedure-note.md`](content/rent-arrears-procedure-note.md).
throughline does not lint prose, so the artifact is a plain file — but read it against
the graph and each composed axis bites: the neutral internal register (natural
contractions — "you've", "doesn't" — and plain imperatives), the practitioner
calibration that uses *arrears*, *ledger* and *notice* undefined yet explains the deeper
legal term *Notice of Seeking Possession* in plainer words, the numbered one-action
steps, the condition-before-action branch on whether the tenant responds, and the
finish-the-case verification and what-to-do-if-blocked recovery. Set this note beside the
[tenant help page](https://github.com/timebacksolutions/content-compose-example) and you
can see the two sibling swaps — formal→neutral register, general→practitioner audience —
in the prose itself.

## How it's wired

- The project's own graph lives under `intents/`, `user-requirements/` and
  `system-requirements/`. Each note requirement **grounds** through `implements` →
  `UR-0001` → `derives_from` → `INT-0001` (its own throughline), and **separately**
  `satisfies` the borrowed `plain:`/`conventions:`/`tone:`/`purpose:`/`audience:` clause
  it honours.
- `satisfies` is a traceability link, not a grounding link — so a note requirement still
  justifies itself through its own intent, not through a borrowed standard.

## Running it

```sh
tl-compose check --strict     # fetches all five pinned sources, merges, validates
tl-compose trace SR-0006      # show a requirement's throughline across the axes
```

Drive this project with `tl-compose`, never bare `tl`: bare `tl` fails fast the moment
it meets a namespace-qualified reference (`audience:SR-0003`) it cannot resolve, because
only the composition-aware tool fetches and merges the sources.
