# EEL — Design decisions

Running log. Each entry: date, the decision, who proposed it, and who chose it.
Nothing here is final until the owner says so; open questions live at the bottom.

## Decided

### 2026-10-31 — EEL compiles for macOS on Apple Silicon and Linux on x86-64 and AArch64 (owner: "1, 2")

Inline binary is one CPU's machine code, so each target is a CPU its programs' inline binary must be written for.
Design claude offered (1) macOS AArch64 alone, (2) adding Linux x86-64 and AArch64, (3) adding Windows x86-64, (4)
adding WebAssembly, all built into Homebrew's LLVM 23, and recommended (1) for now. The owner: "1, 2". So **EEL's
targets are macOS AArch64, Linux x86-64 and Linux AArch64**: two CPUs' inline binary, two executable formats (Mach-O,
ELF). Windows and WebAssembly are not targets.

### 2026-10-31 — CMake and Ninja build EEL's compiler (owner: "yup")

Design claude offered CMake + Ninja (installed, 4.4.3 and 1.13.2; LLVM ships `LLVMConfig.cmake`, so
`find_package(LLVM 23 CONFIG)` brings its headers, libraries and flags), Meson, a plain Makefile, or another, and
recommended CMake + Ninja. The owner: "yup".

### 2026-10-31 — EEL's compiler is built by clang 23, from `llvm@23` (owner: "yes")

Asked which C++ compiler builds EEL's compiler, the owner: "Clang? We don't have C syntax, nor is it another C++".
Design claude explained that clang never reads EEL: it compiles EEL's compiler, itself a C++23 program, and `eel`
then reads EEL and hands LLVM IR to LLVM, as clang does for C++. "Oh, why not the same compiler for that C++?": clang
23 is LLVM 23's own C++ front end, in the same package. The owner: "yes". So **clang 23 from `llvm@23` builds EEL's
compiler**, not Apple's clang 21 or GCC 16 (which cannot use LLVM's C++ API). Self-hosting, EEL's compiler written in
EEL, stays possible later.

### 2026-10-31 — C++23 (owner: "C++23")

Strictly, the latest standard clang 23 fully supports is C++17, and only its language: libc++ 23 has finished no
standard's library. Design claude offered C++17, strict, or C++23, full for what EEL's compiler uses (none of the gaps
touch a compiler: parallel algorithms, special maths, time zones, `std::generator`, `<stacktrace>`), and recommended
C++23. The owner: "C++23". So **EEL's implementation is C++23**, staying clear of the parts clang 23 and libc++ 23 lack.

### 2026-10-31 — LLVM stays; EEL's C++ is the latest standard with full support (owner: "LLVM, C++ latest full support")

Shown clang 23's C++26 lacking reflection, the owner asked whether to switch to GCC, then "No, I meant switching to
GCC entirely". Design claude laid out GCC as the whole toolchain (libgccjit, EEL emitted as C for `gcc-16`, a GCC
front end), tested EEL-as-C with inline binary under `gcc-16`, and GCC 16's reflection through LLVM's C API. Asked
"Jeez, GCC is not like LLVM?", design claude explained GCC's monolithic design, libgccjit as the bolt-on library, and
that libgccjit is GPLv3, so a compiler linking it would ship under GPLv3, not this repo's licence. The owner: "LLVM,
C++ latest full support". So **LLVM 23 stays the backend**, and **the C++ standard is the latest one fully supported**;
which standard that is, on clang 23 and libc++ 23, is open below.

### 2026-10-31 — LLVM 23, stable, pinned as `llvm@23` (owner: "The one you picked")

Asked "Any unstable ones?", design claude listed LLVM's `main` (24.0.0git, never released, its C++ API moving every
week; through `brew install llvm --HEAD` or a checkout pinned to a commit) and the `release/23.x` tip, and still
recommended stable 23 (23.1.4, 2026-10-20; 23.1.2 is installed), pinned so a Homebrew move to 24 does not break the
build. The owner: "The one you picked". So **EEL builds on stable LLVM 23**, `llvm@23`.

### 2026-10-31 — The backend is LLVM, and EEL's implementation is written in C++ (owner: "LLVM, and C++")

Design claude laid out Cranelift, LLVM and GCC (`design/hosts.md`, H2): first recommending Cranelift, then, with the
owner's concept, LLVM, the one with C's speed, inline binary in the middle of a function (inline assembly) and
WebAssembly. For the host language design claude had recommended Rust (H1). The owner: "LLVM, and C++". So **EEL
compiles through LLVM, and its implementation is C++**, LLVM's own language. Not yet chosen: the LLVM version, the C++
standard, the build system, whether a step interpreter stands beside the compiler.

### 2026-10-31 — EEL is the owner's fictional language, made real: everything is imported from the internet (owner)

The owner: "EEL context: it was a fictional language that imports from the internet, has no stdout (it uses an import
for each output, say to a file, to a GUI, to a fucking printer), it can write inline binary, and in lore it was C
speed, painful to write & obviously painful to debug (even err msgs are an import)". So EEL:

- **imports from the internet**;
- **has no stdout**: every output is an import, one for each place it goes (a file, a GUI, a printer);
- **can write inline binary**;
- **was C speed**, in the lore;
- is **painful to write and to debug**, and **even its error messages are an import**.

This replaces design claude's five pitches (`design/pitches.md`), none of which the owner took. Not yet designed: the
syntax, what an import is and how it is fetched, what inline binary means exactly.

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
  Settled: H1 C++23 built by clang 23 with CMake and Ninja, H2 LLVM 23, H3 macOS AArch64 + Linux x86-64 and AArch64. Design claude recommends, for the rest, a CLI and a
  browser stepper that is also the playground, no crates in the interpreter, GitHub Pages and releases.
- Still open from `design/pitches.md`'s list: Turing completeness, the file extension, what "EEL" stands for.
