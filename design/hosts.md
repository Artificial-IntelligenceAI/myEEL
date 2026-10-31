# EEL — hosts: what EEL is built with and runs on

Design claude's proposals, 2026-10-31 (owner: "Let's do hosts first (tools, langs, backends, etc)"). **Nothing here
is decided.** Each question has its options, design claude's recommendation, and whether the concept
(`design/pitches.md`) changes the answer.

What this Mac has, checked 2026-10-31 (Apple M5, 10 cores): Rust 1.96.1 with the `wasm32-unknown-unknown`, Linux,
Windows and Intel macOS targets and `wasm-pack` 0.15; LLVM 21, 22 and 23 (Homebrew); Apple clang 21; Python 3.14.7;
Node 24.16; Swift 6.4; OCaml 5.5; Nim 2.2; Java 26; .NET 10; Julia 1.12; Go, Zig, Lua, LuaJIT; VS Code 1.143.

## H1. The host language: what EEL's implementation is written in

**Decided 2026-10-31: C++** (owner: "LLVM, and C++"), **the latest standard with full support** (owner: "LLVM, C++
latest full support"). Design claude had recommended Rust. Which standard that is: `DECISIONS.md`, "Open".

| Option | For | Against |
|---|---|---|
| **Rust** | fast; one codebase builds the CLI and, through `wasm32` + `wasm-pack` (both installed), the browser; CyborgPL's oracle and `KAYO\n` were Rust | slower to change than a script |
| Python | quickest to try rule changes in | slow for long runs; the browser needs Pyodide |
| TypeScript (Node) | the browser for free | weaker as a CLI; types looser than Rust's |
| Something else (OCaml, Nim, Swift, …) | | |

**Recommendation: Rust.** Concept doesn't change it.

## H2. Backends: how an EEL program runs

**Decided 2026-10-31: LLVM** (owner: "LLVM, and C++"), **stable 23, pinned as `llvm@23`** (owner: "The one you
picked").

1. **A step interpreter**: one step at a time, the whole machine state visible after each. Every concept needs this;
   it is also what a debugger and a visualiser are built on.
2. A bytecode VM: faster than (1), once the instruction set holds still.
3. Compiled to C, built by clang.
4. Compiled to LLVM IR (22 or 23), as `KAYO\n` did.
5. Compiled to WebAssembly.

The concept matters here: a program that reshapes itself as it runs (A Body's eel deciding its own path, D Slippery's
sliding lines, B Life cycle's stages) is hard to compile ahead of time, and an interpreter is the honest backend for
it. C Charge and E Knot compile cleanly.

**Recommendation: (1) only, for now**, written as the executable spec, as CyborgPL's oracle was: it does what
`DECISIONS.md` settles and refuses the rest. A compiled backend later, if the concept allows and the owner wants one.

**Revised with the owner's concept (2026-10-31).** C speed and inline binary need a native, optimising backend.
The owner asked: Cranelift, LLVM or GCC?

| | Cranelift | LLVM | GCC |
|---|---|---|---|
| Code it makes | good, not C's | C's (clang's own optimiser) | C's (gcc's) |
| Inline binary | no inline assembly; raw bytes only as a separate function called through a pointer | inline assembly, so `.byte` lands in the middle of a function | inline assembly, `.byte` the same |
| WebAssembly out | no | yes | no |
| Here | a Cargo crate | 21, 22, 23 (Homebrew) | `gcc-16` (Homebrew), no libgccjit; Apple Silicon support from a fork, not upstream |
| Ties the host to | Rust | any (C API) | C (libgccjit), or C emitted and built by `gcc-16` |

Design claude first recommended Cranelift (fast compiling, a JIT for programs that reshape themselves), before the
concept. **With it: LLVM**, the only one with C's speed, inline binary in place and WebAssembly.

## H3. Platforms: where EEL runs

macOS arm64 (here); Linux x86-64 / arm64 and Windows x86-64 (their Rust targets are installed); the browser (wasm32).

**Recommendation: macOS and the browser first**; Linux and Windows builds when there is a release.

## H4. Tools

| Tool | What it is | Recommendation |
|---|---|---|
| **CLI** | one command (name open; `eel` the obvious one): run a program, show its trace | yes, first |
| **Visual stepper** | watch a program run step by step: the grid and the eel's body for A, the five readings for B, the strands for E | yes, early: an esolang is half its picture, and it is how bugs in programs (and in the interpreter) are found |
| **Playground** | a web page that runs EEL in the browser, the stepper in it | yes, once the stepper exists; it is the same Rust built to wasm |
| **Examples as tests** | `examples/` programs, each with its expected output, run by the interpreter's tests, as in CyborgPL | yes, from the first program |
| **VS Code** | highlighting (a TextMate grammar) at first; a language server only if ever needed | later |
| **Formatter** | | only if the concept has a layout to normalise; a 2D grid (A) or a braid (E) is its own layout |

The stepper could live in the terminal, in the browser, or both. **Recommendation: the browser**, so there is one of
it and the playground and the stepper are the same page.

## H5. Dependencies

- None at all in the interpreter (CyborgPL's oracle used no crates), or
- a few chosen ones (an argument parser for the CLI, `wasm-bindgen` for the browser).

**Recommendation: none in the interpreter; `wasm-bindgen` alone for the browser build**, each one added by the owner's
yes.

## H6. Hosting and distribution

- The playground: **GitHub Pages from this repo** (free, nothing else to run), or Cloudflare as cyborgpl.dev was, or
  a domain of EEL's own.
- The CLI: GitHub releases with built binaries, `cargo install --git`, or a Homebrew tap.

**Recommendation: GitHub Pages for the playground; GitHub releases for the CLI**, when there is something to release.

## In short

Design claude's picks, for the owner to take, change or refuse: **Rust; a step interpreter that is the executable
spec; macOS and the browser first; a CLI, a browser stepper that is also the playground, examples as tests; no crates
in the interpreter; GitHub Pages and releases.**
