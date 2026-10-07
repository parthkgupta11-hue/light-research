# light-research

A Claude skill: deep-research-style answers on a token budget.

It asks how much you want to spend (a token count or a share of your plan, e.g. "10% of Pro plan"), picks a tier (Quick ~15k, Standard ~40k, Thorough ~100k tokens), runs a capped search → read → synthesize loop, and returns a short cited report with source grades (A/B/C), a "where sources disagree" section, a confidence level and an honest list of gaps.

Token figures are rough targets, not measured guarantees. In small tests it produced better-structured, better-graded reports than unaided Claude, but did not measurably use fewer tokens than plain Claude; savings versus a full deep-research run have not been measured.

## Install

**Claude Code:** copy the `light-research` folder to `~/.claude/skills/` (Windows: `%USERPROFILE%\.claude\skills\`).

**claude.ai:** download `light-research.skill` (or zip the `light-research` folder so `SKILL.md` is inside it), then Settings -> Capabilities -> Skills -> upload. Menu names may vary by plan and release. Web search must be enabled in the chat.

## Use

Say "light research: <your question>" and answer the budget question.

## License

MIT
