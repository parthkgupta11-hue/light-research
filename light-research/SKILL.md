---
name: light-research
description: Token-budgeted alternative to deep research. Asks the user how much to spend (e.g. "10% of Pro plan" or a token count), then runs a capped search-read-synthesize loop and returns a concise, cited answer. Use whenever the user says "light research", "quick research", "cheap research", "research this but don't burn tokens", "look into X without going deep", or asks for sourced research on a topic where a full deep-research run would be overkill. Also use when the user wants research and mentions cost, usage limits, or their plan quota, even if they never say "light".
---

# Light Research

Deep research is thorough but expensive: dozens of searches, full-page reads, many subagents. This skill gives most of the value at a fraction of the cost by fixing the budget up front and sizing the effort to it.

## Step 1: Ask for the budget (always, before any searching)

If the user has not already stated a budget in this conversation, ask with AskUserQuestion (or, where that tool is unavailable such as in claude.ai, ask in plain chat with the same options and wait for the reply). Accept either a quantity or a share of their plan. Offer these options (the user can also type their own via "Other"):

- **~5% of Pro plan** (~15k tokens) - Quick: a fast sourced answer
- **~10% of Pro plan** (~40k tokens) - Standard (recommended default)
- **~25% of Pro plan** (~100k tokens) - Thorough: still far below deep research

Also confirm the research question if it is ambiguous, in the same question set. Do not ask more than that; every extra round trip costs tokens too.

Percent-of-plan figures are rough, since real plan limits vary and are not visible from inside a session. Treat them as the token targets above, say so in one line, and map a custom number to the nearest tier (interpolate if it falls between).

## Step 2: Size the effort to the tier

| Tier | Searches | Full page fetches | Subagents | Notes |
|------|----------|-------------------|-----------|-------|
| Quick (~15k) | 2-3 | 0-1 | none | Rely on search snippets; fetch only if snippets conflict |
| Standard (~40k) | 4-6 | 2-3 | none | Fetch the best 2-3 sources |
| Thorough (~100k) | 8-12 | 4-6 | up to 2, only for independent sub-questions | Reconcile conflicts between sources |

These are ceilings, not targets. Stop early once the question is answered with confidence; spending the whole budget is not the goal.

## Step 3: Run the loop cheaply

Token cost comes from three places: search breadth, page reads, and the model's own re-reading of accumulated context. Control each:

1. **Plan in a few lines.** Break the question into 2-4 sub-questions. Write them down briefly and skip any long planning prose.
2. **Search with the standard mode first.** Use the cheap search mode and escalate to the extended mode only if results come back thin or stale, and only on Standard/Thorough tiers. Issue independent searches in the same turn so they run in parallel.
3. **Prefer snippets over pages.** Read a full page only when a snippet cannot settle a claim. When you fetch, ask for the specific facts you need rather than the whole page.
4. **Prefer primary and high-quality sources** (official docs, papers, filings, reputable outlets). One good source beats three blog rewrites of it, and it saves reads.
5. **Keep running notes terse.** After each step, keep only claim + source URL + confidence. Drop the rest. This keeps later turns cheap.
6. **Avoid subagents unless the tier allows them.** Each one starts cold and re-derives context, which is the expensive path. When used, give them a narrow question and ask for a short summary back.
7. **Do a rough budget check at the midpoint.** If you are near the ceiling with gaps remaining, stop searching and report the gaps rather than overspending.

## Grade sources as you go (one word each, no extra searches)

Tag every source A, B or C while taking notes; it costs nothing and lets the reader judge the answer at a glance.

- **A** - governments, international bodies, peer-reviewed papers
- **B** - industry bodies, company filings/docs, a vendor's own pages (authoritative for its own price/features only), major research firms
- **C** - news, trade media, blogs, forums

A key figure should never rest on grade C alone; if it does, say "single weak source" next to it. For key figures, keep a short quote (25 words max) from the page when you fetched it, so claims are checkable. Count sources by origin: reprints of one press release are one source.

**Freshness.** Write the publication year (month if known) next to every source; use "undated" rather than guessing. Today's date is in your context, so judge recency against it. For anything fast-moving (AI, markets, regulation, medicine guidelines, product launches), make one of your searches target the newest evidence explicitly (add the current year, "latest", or "2026 review"), and prefer the most recent systematic review or primary filing over older papers. If the best sources you found are more than a year or two old on a fast-moving topic, say so under gaps.

## Step 4: Deliver

Write the answer in this shape, kept short:

```
## Answer
[2-5 sentence direct answer to the question]

## Key findings
- Finding (source link, grade A/B/C; short quote for key figures)
- ...

## Where sources disagree
[Both values and the likely reason (definition, period, population). Omit this section if they agree.]

## Confidence and gaps
[Level: high / medium / low. High = two or more independent A/B sources agree and are current. Medium = sources agree but are thin, dated or partly C. Low = one source, or unresolved conflict. Then: what is uncertain, and what you did not check because of the budget.]

## Sources
[Numbered links]
```

Rules for the report:
- Every non-obvious factual claim carries a source. If something is your inference, label it as such.
- Report disagreement between sources instead of quietly picking one.
- End with one line: approximate budget used (e.g. "Used roughly 30k tokens, Standard tier") and an offer to go deeper on a specific gap. Offering a targeted follow-up is cheaper than defaulting to a broad one.

## Why this design

The user chose this skill because they want research quality that respects a quota. So the budget question comes first, effort is capped by tier, and the report is honest about what the cap left unexamined. Silently exceeding the budget, or silently cutting corners without saying so, would each defeat the point.
