---
layout: post
title: "Checking an AI's maths, part 2"
date: 2026-10-07 12:00:00 +0000
---

**Short version:** I checked a second result from OpenAI's AI maths release. This one rests on over a million number comparisons, some with numbers 25 digits long. Every single comparison comes out the way the paper says.

## The puzzle, in plain English

Imagine two ways of building with Lego.

- **Way 1:** make 6 towers, each b bricks tall, and put them in a box.
- **Way 2:** make b towers, each 6 bricks tall, and put them in a box.

Same number of bricks either way. In 1950 a mathematician called Foulkes asked: if b is at least 6, does everything you can build the first way also fit inside what you can build the second way?

(In the real problem the "bricks" are polynomials, and "fit inside" has a precise meaning, but that's the shape of it.)

It had been proved for 2, 3, 4 and 5 towers. OpenAI's paper claims to prove it for 6.

## How the proof works

The proof has three layers:

1. For b from 6 to 25, a computer compares the two sides directly, piece by piece.
2. For b up to 149, it uses computer-generated shortcuts.
3. Beyond that, it's a written argument.

I checked layer 1.

## What I found

**It holds up.** For every b from 6 to 25, the second way always has at least as much as the first. No exceptions.

- **I re-ran OpenAI's own code.** It matched their published results exactly.
- **I wrote my own program from scratch,** which counts everything a different way and checks every single case. OpenAI's code uses shortcuts to skip most of them. At b = 25 mine compared 1,229,120 pieces, some involving counts 25 digits long.
- **It passed sanity checks** against answers already known from textbooks.

The biggest cases ran on GitHub's own servers, with [public logs](https://github.com/realopjt/foulkes-sixth-power-check/actions/runs/37636354843) anyone can inspect.

## Did I find a different solution?

No. The question and the right answer are fixed. Running a second, independent program is like having a second accountant redo the books from the receipts: if both get the same total, a mistake in the first one is very unlikely. If we'd disagreed, that would have meant an error somewhere.

## What this doesn't prove

Only layer 1. Layers 2 and 3 still need mathematicians to check them.

All code and results: [github.com/realopjt/foulkes-sixth-power-check](https://github.com/realopjt/foulkes-sixth-power-check). Part 1 is [here]({% post_url 2026-10-07-checking-an-ais-maths %}). I did this with help from Claude (Anthropic). The maths is OpenAI's.

---

## For mathematicians

**Claim checked.** The base interval of [Foulkes' Conjecture for the Sixth Symmetric Power](https://github.com/openai/math/blob/main/preprints/Foulkes-Conjecture-for-the-Sixth-Symmetric-Power-September-25-2026/main.pdf): for 6 ≤ b ≤ 25, [s_λ] h_b[h_6] ≥ [s_λ] h_6[h_b] for every λ ⊢ 6b (only ℓ(λ) ≤ 6 matters, since h_6[h_b] has no other constituents). Equivalently Sym⁶(Sym^b V) ↪ Sym^b(Sym⁶ V) in that range.

**Re-run.** `verify_computations.py --run` (unchanged, g++ 13.3, Boost 1.83) passes and matches all 62 reference output rows.

**Independent checker.** Rust, exact 128-bit integers with overflow checks. Unlike the paper's Schur-basis Newton recurrences with signed lookups and an upper bound U to skip most partitions, it works in the weight basis: Newton's identity j·ch Sym^j W = Σ_k ψ^k(W)·ch Sym^{j−k} W on dominant-weight multiplicities, with (ψ^k(W)·χ)(μ) = Σ_{s ∈ wt(W)} χ(sort(μ − k·s)), then Schur multiplicities by the Weyl alternation c(λ) = Σ_{w ∈ S₆} sgn(w) K(λ + δ − wδ). Every partition is computed exactly; no bound is used.

**Checks.** Σ_λ c(λ)·dim₆(λ) equals dim Sym⁶(Sym^b ℂ⁶) and dim Sym^b(Sym⁶ ℂ⁶) at every b; no negative multiplicities; A = B at b = 6. Self-tests: Thrall's h₂[h_q] (q ≤ 12), h₃[h₂], h_q[h₂] (q ≤ 8), h₃[h₃].

**Ties.** Our count of λ with A = B (including A = B = 0) exceeds OpenAI's table by 4 at b = 6, 3 at b = 7, 8 and 2 for every b from 9 to 25. Their table counts ties only among partitions their U-test could not settle, so this is expected.

**Status.** The full base interval b = 6 to 25 is done, zero violations; at b = 25 we check 1,229,120 partitions, matching the paper's count for its largest complete level. The certificate layer (26 ≤ b ≤ 149) and the general argument are not checked.
