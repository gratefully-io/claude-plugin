---
name: donor-meeting-prep
description: Prepare a one-page brief before meeting, calling or visiting a donor, from their Gratefully record - giving history, scores, recent signals, talking points and a suggested ask, every fact cited. Use when the user mentions an upcoming donor meeting, call, visit, lunch, coffee or "prep me on <donor>".
license: MIT
compatibility: Requires the Gratefully connector (https://app.gratefully.io/mcp) to be connected.
---

# Donor meeting prep

Build a short, cited brief the fundraiser can read in two minutes before talking to one donor.

## Ground rules

- **Only Gratefully facts.** Every fact about the donor (gifts, amounts, dates, notes, scores, signals, contact details) must come from a Gratefully tool result in this conversation. Never invent or estimate donor facts. If something isn't in the data, say "not in Gratefully".
- **No assumptions.** Don't state or imply anything the data doesn't show, such as whether a project was finished, how a gift was used, or why a donor stopped giving. Phrase open questions as questions for the meeting.
- **Cite as you go.** After each fact, link the source the tool returned, e.g. `([donor record](url))`. Put a "Sources" list at the end.
- **Sample data.** If any tool result has `data_state.state = "demo"`, start the brief with: "This is Gratefully's sample data, not your donors." and include the `connect_url` once at the end. If `data_state.state = "importing"`, say the import is still running and the brief may be incomplete. If `data_state.state = "empty"`, say there's no donor data in Gratefully yet and share the `connect_url`; stop there.
- **Plan limit.** If a result has `limit_reached: true`, stop and show the user its `message` and `upgrade_url`.

## Steps

1. **Find the donor.** Call `search_donors` with the name the user gave. If several match, ask which one (show name + summary). If none match, say so and stop.
2. **Get the record.** Call `get_donor_profile` with the `donor_id`. If `gifts_pagination.has_next` is true and the user asked about long-term history, fetch one more page; otherwise one page is enough.
3. **Check current signals.** Call `get_todays_priorities` and keep any card for this donor. Optionally call `get_stewardship_moments` and `get_donor_news` and keep only cards whose `donor_id` matches.
4. **Outside context (only if relevant).** If the donor is a foundation, company or other organization, you may call `research_organization` with its name. Never research an individual person.
5. **Write the brief** in this order:
   - **Who they are**: name, relationship owner, cultivation stage, segment and flags, contact details on file.
   - **Giving history**: lifetime total, first, largest and last gift (amount, date, fund), number of gifts, preferred fund. Note gaps (e.g. "no gift in 14 months").
   - **Scores**: RFM composite and percentile if present; explain in plain words what they mean.
   - **What's happening now**: open priorities, recent signals and news, with why they matter.
   - **Notes from the team**: staff notes and the donor narrative, briefly.
   - **Talking points**: 3 to 5 specific, warm points grounded in the facts above (thank them for X, ask about Y).
   - **Suggested ask**: an amount and purpose with one line of reasoning based only on their giving pattern (for example "1.2x their largest gift of $5,000 to the same fund"). If an open priority or stewardship moment suggests this is a thank-you or relationship meeting, recommend no ask and say why.
   - **Sources**: every link used.
6. **Offer next steps**: logging the meeting afterwards with `log_interaction`, or setting a follow-up with `create_reminder`. Only call those tools if the user agrees.

## Example

> "Prep me for coffee with Dorothy Hale tomorrow."
