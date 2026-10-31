# EEL — first pitches

> **Superseded, 2026-10-31.** The owner gave EEL's own concept instead (`DECISIONS.md`, "EEL is the owner's fictional
> language, made real"). Kept as history.

Design claude's proposals, 2026-10-31, for the owner to pick from, mix, or throw out. **Nothing here is decided.**
The sketches only show the idea; every symbol in them is a placeholder.

Each pitch takes something an eel really is or does and makes it the one rule the language is built around, the way
Befunge is built around a moving pointer and Brainfuck around a tape.

## A. Body — the eel's body is the stack

The program is a 2D grid of characters. An eel swims through it: its head reads the instruction under it, and its
body trails behind, lying on the cells it just crossed. **Each segment of the body holds one value**, so the stack
is not hidden somewhere: it is lying on the program.

- Pushing a value grows the eel by a segment (the tail stays where it is for a step).
- Popping shrinks it (the tail pulls in).
- **The head cannot swim into its own body.** When the next cell is body, it turns.

That last rule is the point: how much data the program holds changes where it goes. A loop needs no counter and no
test: an eel that grows a segment every lap is eventually long enough to block its own way back in, and turns out of
the loop. Long data is a wall; short data is an open road.

```
>  1  v      the eel goes round, one segment longer each lap,
^     <      until its tail blocks the turn and it leaves
```

- Writing it feels like: Befunge, but your data is a physical obstacle you have to steer around.
- Cost: easy to build (a grid, a queue of segments); hard to write anything big, which is the fun.

## B. Life cycle — one text, read five ways

A real eel lives as five different animals: leptocephalus (a clear leaf of a larva), glass eel, elver, yellow eel,
silver eel. In EEL **the same source text is read under a different instruction set at each stage**. The program
starts as a larva; an instruction (`molt`) moves it to the next stage, where every character means something else.
The data carries over; the meaning of the text does not.

At the last stage the silver eel goes back to the sea to spawn: **a program ends by writing its offspring**, a new
EEL program, which can be run in turn.

```
stage 1 (larva):   a..z are digits 0..25, the rest is noise
stage 2 (glass):   a..z are operations, digits are jumps
…
stage 5 (silver):  what is printed is the next program
```

- Writing it feels like: writing one text that is a correct program in five languages at once, each stage handing
  the next a useful state.
- Cost: five small instruction sets to design; the most "extraordinary" of the five, and the hardest to write in.

## C. Charge — every value is leaking

Electric eels store charge and let it go. In EEL **every stored value loses 1 towards 0 on every step**, as DRAM
would if nobody refreshed it. A program keeps its data alive by recharging it, and an instruction that discharges a
cell adds its value to the cells around it, so data spreads like a shock through water.

- Writing it feels like: computing against a clock; timing is part of every algorithm.
- Cost: simple to build; the risk is that it reads as a gimmick on top of an ordinary machine unless the discharge
  rule is the way computation is done.

## D. Slippery — nothing stays where you put it

**Every line, after it runs, slides one place down the program** (trading places with the line below; the last
line slides to the top). The program reorders itself as it runs, so a loop is not written, it falls out of the
slide: a line comes round again once everything else has passed it.

- Writing it feels like: choreographing a conveyor belt; each line has to be right in every position it will pass
  through.
- Cost: simple rule, very hard to reason about; the order of a run is a puzzle in itself.

## E. Knot — programs are braids

Moray eels tie themselves in knots to tear their food. In EEL **a program is a braid drawn in ASCII**: vertical
strands each carry a value down the page, and where two strands cross they act on each other, the kind of crossing
saying how (one over the other adds the left to the right; the other way round subtracts). A strand closed into a loop
runs again.

```
| | |
 X  |      the first two strands cross
| | |
|  X       then the last two
```

- Writing it feels like: drawing. A program is a picture you can read.
- Cost: needs a careful parser for the drawing; closer to a real piece of mathematics (braid groups) than the others.

## Design claude's recommendation

**A, Body.** It ties the name to the one rule most tightly (the eel *is* the data structure), it is small enough to
build an interpreter for quickly, and it makes ordinary things (a loop, a stack) strange in a way you can see on the
page. B is the boldest and could later be how an A program ends (spawning), but it is the owner's call.

## What comes after the concept

Once a concept is picked, these follow, each the owner's:

1. **How hard it should be to write**: hard but usable, as Befunge is, or deliberately hostile, as Malbolge is.
2. **Turing completeness**: a goal to prove, or not a concern.
3. **The interpreter's language**: now in `design/hosts.md`, H1.
4. **The file extension**: `.eel`, or another.
5. **Input and output**: numbers, characters, both.
6. **What "EEL" stands for**: this folder says "Extraoridinary Esotric Language"; whether that spelling is on
   purpose.
