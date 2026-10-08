---
name: year-end-appeal
description: Plan a year-end appeal from Gratefully data - segment donors, size each segment, prioritise who gets a personal touch, and draft appeal variants per segment, with figures cited. Use when the user mentions a year-end, end-of-year, December, Giving Tuesday or holiday appeal or campaign.
license: MIT
compatibility: Requires the Gratefully connector (https://app.gratefully.io/mcp) to be connected.
---

# Year-end appeal

Produce a year-end appeal plan grounded in the organization's real giving data: which segments to target, how big each one is, who deserves a personal ask, and draft copy for each segment.

## Ground rules

- **Only Gratefully facts.** Segment sizes, totals, trends and donor names must come from Gratefully tool results in this conversation, with their source links. Don't invent statistics, benchmarks or donor details. If you add general fundraising advice, label it as general advice, not data.
- **No invented organization facts.** Programs, projects, impact numbers, events and their status (built, finished, launched) may only be stated if a tool result says so, for example the brand voice or relationship notes from `get_outreach_context`. Otherwise write a bracketed placeholder such as `[one concrete impact from this year]` for the user to fill in. Never make tax, legal or matching-gift claims.
- **Only state counts you can see.** Numbers like "N donors", "N gifts" or "checked N donors" must come straight from tool results; don't summarize what you didn't fetch.
- **Segment copy is generic.** A segment variant goes to every donor in the segment, so it must not contain any individual donor's facts (gift counts, amounts, dates, notes). Use `[First name]` and segment-level facts only (for example "you gave last year"). Individual facts belong only in a personal draft for that one donor.
- **Nothing is sent.** Segment copy stays in the conversation. Personal drafts are saved to Ready to Send only when the user approves them.
- **Respect opt-outs.** Skip donors where `get_outreach_context` returns `can_email: false`.
- **Sample data.** If any result has `data_state.state = "demo"`, say up front that this is Gratefully's sample data and share the `connect_url` once. If `data_state.state = "importing"`, say the figures may be incomplete. If `data_state.state = "empty"`, say there's no donor data in Gratefully yet and share the `connect_url`; stop there.
- **Plan limit.** If a result has `limit_reached: true`, stop and show its `message` and `upgrade_url`.

## Steps

1. **Context.** Call `get_giving_analytics` with `query_type: "giving_over_time"` and `granularity: "month"` to see last year's December and recent trend. Optionally call `get_portfolio_report` with `period: "last_12_months"` for retention and average gift.
2. **Segments.** Call `list_segments`. For each useful segment, call `query_donors` with that filter token to get its size (`pagination.total_count`) and top donors. Typical year-end segments: current donors, LYBUNT (`cohort:lybunt`), SYBUNT (`cohort:sybunt`), lapsed (`cohort:lapsed`), recurring, and major or champion segments. Use the tokens `list_segments` actually returns.
3. **Plan table.** One row per segment: segment, number of donors, why it matters, channel (personal email, standard email or letter), suggested ask framing. Base ask framing on the segment's giving pattern (for example "renew at last year's amount"); don't invent amounts.
4. **Personal touches.** From the highest-value segments, list up to 10 donors for a personal note. Use `get_todays_priorities` and `get_hidden_revenue` to flag anyone with an open opportunity, quoting the reason.
5. **Draft variants.** Write one appeal variant per segment (subject plus 120 to 200 words) in the organization's voice. Get the voice from `get_outreach_context` for any one donor in the segment, and use the same donor's context to check tone. Each variant should open with impact (from tool results, or a bracketed placeholder), make a clear year-end ask suited to the segment (renew, return, upgrade), and include a deadline (December 31). Before showing a variant, check that every sentence about the donor is true for every donor in that segment.
6. **Optional personal drafts.** If the user wants, call `get_outreach_context` for each personal-touch donor, write a personalised version, show it, and save approved ones with `save_outreach_draft`.
7. **Summary.** End with the plan table, the variants, the personal-touch list, and Sources.

## Example

> "Help me plan our year-end appeal: who should we target, and draft the emails for each group."
