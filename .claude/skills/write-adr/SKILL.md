---
name: write-adr
description: Turn a decision made in the conversation into a numbered Architecture Decision Record in `docs/adr/`, using this project's ADR format (Nygard style — Status, Date, Context, Decision, Consequences). Handles numbering, file naming, and superseding older ADRs. Use this whenever the user asks to record, document, write up, or "ADR" a decision, says "let's capture why we did this", or has just settled a significant choice (a library, an architecture pattern, a tooling or workflow change, a trade-off) that a future contributor would otherwise wonder about — even if they don't say "ADR".
---

# Write an ADR

Records why a decision was made, so a newcomer (or a future you) doesn't have to dig through
chats and PR threads. The ADR is written from what was actually discussed: the real context,
the options that were weighed, and the trade-offs accepted. Never invent rationale. A
plausible-sounding reason nobody gave is worse than no ADR, because people will trust it.

## 1. Read the format and the existing records

- Read `docs/adr/0001-record-architecture-decisions.md`. It defines the rules and contains the
  template, between its `## Template` heading and the end of the file. Use the template as it
  is today; don't rely on a remembered version.
- List `docs/adr/` and skim the titles, to find:
  - **the next number:** highest existing number + 1, zero-padded to four digits
  - **related ADRs:** if this decision reverses or replaces one, it supersedes it (see step 4)

## 2. Pull the decision out of the conversation

Find these in what was said:

- **Context:** the problem or need, and the forces at play: constraints, requirements,
  incidents, things that went wrong. Include concrete facts from the conversation, such as
  version numbers, error messages and advisory IDs.
- **Options considered:** what else was on the table, and why each one lost.
- **Decision:** what was chosen, stated as "We will …".
- **Consequences:** what gets easier, what gets harder, and the downsides knowingly
  accepted, plus any follow-up work it creates.

If the conversation doesn't say why, or doesn't name the alternatives, ask one short question
for the missing piece instead of filling it in. If the decision hasn't actually been made yet,
offer to write it with status **Proposed**.

If several decisions were made, write one ADR per decision (the format's rule), and confirm the
list with the user first.

## 3. Write the file

- **Path:** `docs/adr/NNNN-short-title.md`, with a kebab-case title of a few words.
- **Heading:** `# N. Title`, without leading zeros, matching 0001's style.
- **Status:** `Accepted`, unless the user says it's still a proposal.
- **Date:** today.
- **Options:** the template has no separate section for them. Put them in **Context**, or in
  **Decision** as "We considered X and Y; …", whichever reads better. Keep them, because
  they're often the most useful part later.
- **Length:** a page at most. Write full sentences, not bullet fragments, and link to the PR,
  issue, release notes or advisory when the conversation mentions one.

Run `npm run format` (or `prettier --write` on the file) afterwards so it passes
`format:check`.

## 4. Superseding an older ADR

ADRs are never rewritten after they're accepted, except for their status. If the new decision
replaces an existing one:

- In the old ADR, change only the status line to `Superseded by ADR-NNNN`.
- In the new ADR's Context, mention which ADR it supersedes and what changed since then.

## 5. Hand off

Show the ADR, or a short summary and the path if it's long, and point out anything you weren't
sure about. Don't commit unless the user asks. They may want it in the same PR as the change
it describes, which is usually the right place.
