# Gratefully for Claude

Gratefully is a donor intelligence platform for nonprofit fundraisers. This plugin brings it into Claude: ask which donors matter today and why, get answers cited back to your Gratefully records, and run ready-made workflows for the jobs fundraisers do most.

It contains:

- **The Gratefully connector** (`https://app.gratefully.io/mcp`). Claude can read your donors, gifts, scores and Action Center priorities, research organizations, log notes, set reminders and save email drafts for your review. Nothing is ever sent from Claude.
- **Three skills**, listed below.

## Install

**Claude Code**

```
/plugin marketplace add gratefully-io/claude-plugin
/plugin install gratefully@gratefully
```

**Claude (web and desktop):** go to Customize → Plugins → Add → Add marketplace and enter `gratefully-io/claude-plugin`. Then open the plugin's **Connectors** tab and connect Gratefully.

The first time Claude uses Gratefully, you'll sign in to Gratefully, or create a free account, and choose which organization to connect. Connect your CRM or upload a donor CSV in Gratefully. Until you do, Claude answers from Gratefully's sample data and says so.

## Skills

| Skill | What it does | Try |
|---|---|---|
| `donor-meeting-prep` | A one-page, cited brief before you meet a donor: giving history, scores, recent signals, talking points and a suggested ask. | "Prep me for coffee with Dorothy Hale tomorrow." |
| `lapsed-donor-win-back` | Finds lapsing and lapsed donors, prioritises them, and drafts a personal re-engagement email for each, saved to your Ready to Send queue for review. | "Which of my lapsed donors should I try to win back this month? Draft emails for the top five." |
| `year-end-appeal` | Segments your donors, sizes each segment, picks who deserves a personal note, and drafts appeal copy for each segment. | "Help me plan our year-end appeal: who should we target, and draft the emails for each group." |

Every skill uses only facts from your Gratefully data, links each fact back to its source, and never sends anything on its own.

## Your data and access

- Claude sees the same donor data you can see in Gratefully, including contact details, for the organization you connected.
- Usage counts toward your Gratefully plan.
- You can disconnect Claude at any time from your Profile page in Gratefully.

See https://gratefully.io/privacy-policy for how data is handled.

## License

MIT
