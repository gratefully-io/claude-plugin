---
name: lapsed-donor-win-back
description: Win back lapsing and lapsed donors - find who is at risk from Gratefully, prioritise them, and draft a personal re-engagement email for each, saved for review. Use when the user asks about lapsing, lapsed, at-risk or LYBUNT/SYBUNT donors, donor retention, or "who should I re-engage".
license: MIT
compatibility: Requires the Gratefully connector (https://app.gratefully.io/mcp) to be connected.
---

# Lapsed-donor win-back

Turn Gratefully's lapse-risk signals into a short, prioritised win-back list with drafted outreach the user reviews before anything is sent.

## Ground rules

- **Only Gratefully facts.** Names, amounts, dates and reasons must come from Gratefully tool results in this conversation. Never invent donor facts or personal details. Cite the source link the tool returned for each donor.
- **No invented organization facts.** Programs, projects, impact numbers, events and their status (built, finished, launched) may only be stated if a tool result says so, for example the brand voice or relationship notes from `get_outreach_context`. Otherwise write a bracketed placeholder such as `[one concrete impact from this year]` for the user to fill in. Never make tax, legal or matching-gift claims.
- **Only state counts you can see.** Numbers like "N donors", "N gifts" or "checked N donors" must come straight from tool results; don't summarize what you didn't fetch.
- **Nothing is sent.** Drafts are saved to the user's Ready to Send queue in Gratefully, where they review and send. Never claim an email was sent.
- **Ask first, then save.** Show drafts to the user and only call `save_outreach_draft` for the ones they approve.
- **Respect opt-outs.** Skip any donor where `get_outreach_context` returns `can_email: false`, and say why.
- **Sample data.** If any result has `data_state.state = "demo"`, say up front: "This is Gratefully's sample data, not your donors," and share the `connect_url` once. If `data_state.state = "importing"`, say the list may be incomplete until the import finishes. If `data_state.state = "empty"`, say there's no donor data in Gratefully yet and share the `connect_url`; stop there.
- **Plan limit.** If a result has `limit_reached: true`, stop and show its `message` and `upgrade_url`.

## Steps

1. **Find candidates.** Call `get_at_risk_donors` (default 20). If the user asked about donors who have already lapsed, also call `query_donors` with `filters: ["cohort:lapsed"]` and `sort: "total_giving"`. Use `list_segments` to find other relevant tokens (for example `cohort:lybunt`) if the user names them.
2. **Prioritise.** Rank by tier (high, medium, worth a look), then lifetime giving, then how recently they last gave. Pick the top 5 unless the user asked for a different number. For each, give one line on why they're on the list, quoting the tool's headline or rationale.
3. **Confirm the list** with the user before drafting. Let them add or drop donors.
4. **Draft.** For each donor, call `get_outreach_context` with `donor_id` and a purpose such as "re-engage lapsed donor". Write a short, warm email (subject plus 80 to 150 words) that:
   - thanks them for specific past support (use the giving summary and relationship notes),
   - shares one concrete reason to reconnect (an update, impact, invitation), in the organization's brand voice,
   - ends with a soft call to action, not a hard ask, unless the user asked for an ask,
   - leaves the signature out; Gratefully adds it at send time.
5. **Review.** Show all drafts together, each with the facts it relied on and their source links.
6. **Save approved drafts** with `save_outreach_draft` (`donor_id`, `subject`, `body`). Report the Ready to Send link from the result.
7. **Optional follow-up.** Offer to set a reminder with `create_reminder` to check for replies in two weeks.

## Example

> "Which of my lapsed donors should I try to win back this month? Draft emails for the top five."
