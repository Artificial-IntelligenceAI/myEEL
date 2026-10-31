# EEL

Design repository for **EEL**, an esoteric programming language (owner, 2026-10-31: "Let's design/make EEL"). Started
from nothing: no concept, syntax or semantics is settled until `DECISIONS.md` says so.

## Ground rules

- **Design authority is the owner's** (owner, 2026-10-31: "Design choices are mine to make, you could suggest").
  Sessions advise and recommend; none decide syntax, naming, semantics or architecture, and none fill in a default
  for an open decision. Ask in plain text and wait.
- **No chat popups** (owner, 2026-10-31: "Use whatever's needed, except chat popups"): never the AskUserQuestion
  tool, nor a card or box asking the owner to pick.
- **One question at a time** (owner, 2026-10-31): put a single question to the owner, wait for the answer, then the
  next. A design file may list many; the conversation asks them one by one.
- Record decisions in `DECISIONS.md` as they are made, noting what the owner chose against what was proposed.
- Proposals live in `design/` until the owner picks; a pitch there is not a decision.

## Sessions

- **Design session** ("design claude", this repo's default): decisions, `design/`, `DECISIONS.md`. Commits carry
  `Co-Authored-By: design claude <noreply@anthropic.com>`.

## Layout

- `DECISIONS.md` — the running log of settled decisions, and the open questions at the bottom.
- `design/` — design documents, one topic per file. `design/pitches.md` holds the first concepts on offer.

## Repository

**`Artificial-IntelligenceAI/myEEL`**, public (owner, 2026-10-31), checked out at
`/Users/ts/EEL - Extraoridinary Esotric Language`.

Licences (owner, 2026-10-31): `LICENSE`, Apache-2.0 WITH LLVM-exception, for the code and everything else;
`LICENSE-CC-BY-4.0`, CC BY 4.0, for the writing (the Markdown files) and any logos.

## Git

- Branch: `main`. Author: `Tankun Sriket <tankunsriket63741ecff056.invalid>` (repo-local config, already set).
- Commit style is CyborgPL's (owner, 2026-10-31: "commit style: Soclets/CyborgPL"):
  - Subjects are `kind.folder.narrowing: sentence`. Kinds: `decision`, `design`, `feature`, `fix`, `test`, `house`.
    The second word is the folder, or the topic for `decision` / `design` / `house` (`house.license`,
    `house.readme`). The third word is optional, and there is never a fourth.
  - The sentence says what is now true, in plain words; who chose it goes in brackets at the end, the owner's own
    words quoted where they settle it: `decision.concept.body: … (owner: "yes")`.
  - A body, when needed, says what changed and why, wrapped at about 72 columns.
  - Every commit ends with the session's house name: `Co-Authored-By: design claude <noreply@anthropic.com>`.
