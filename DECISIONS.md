# EEL — Design decisions

Running log. Each entry: date, the decision, who proposed it, and who chose it.
Nothing here is final until the owner says so; open questions live at the bottom.

## Decided

### 2026-10-31 — The owner decides; sessions suggest, in plain text, never by popup (owner)

The owner: "Let's design/make EEL. Design choices are mine to make, you could suggest. Use whatever's needed, except
chat popups." So every design choice is the owner's, and sessions ask in plain text.

### 2026-10-31 — The repository is `Artificial-IntelligenceAI/myEEL`, public (owner)

The owner: "repo: public, <myacc>/myEEL"; the account is the owner's GitHub login, `Artificial-IntelligenceAI`.

### 2026-10-31 — Apache-2.0 WITH LLVM-exception for the code, CC BY 4.0 for the writing (owner)

The owner: "Legal: Apache 2.0 with LLVM exception, CC BY 4.0". Split as in CyborgPL: `LICENSE` for the code and
everything else, `LICENSE-CC-BY-4.0` for the Markdown files and any logos.

### 2026-10-31 — Commits are written in CyborgPL's style (owner)

The owner: "commit style: Soclets/CyborgPL". `kind.folder.narrowing: sentence`, kinds `decision`, `design`,
`feature`, `fix`, `test`, `house`, and the session's house name as the trailer; `CLAUDE.md`, "Git".

## Open

- **The hosts, first** (owner, 2026-10-31: "Let's do hosts first (tools, langs, backends, etc)"). Design claude's
  proposals are in `design/hosts.md`, H1–H6: the host language, backends, platforms, tools, dependencies, hosting.
  Design claude recommends Rust, a step interpreter as the executable spec, macOS and the browser first, a CLI and a
  browser stepper that is also the playground, no crates in the interpreter, GitHub Pages and releases.
- **The concept.** Design claude's five pitches are in `design/pitches.md` (A Body, B Life cycle, C Charge,
  D Slippery, E Knot); design claude recommends A. The owner's, or something else entirely.
- What follows the concept: how hard it is to write, Turing completeness, the file extension, input and output, what "EEL" stands for (`design/pitches.md`, "What comes after the concept").
