---
layout: post
title: "Checking an AI's maths"
date: 2026-10-07 09:00:00 +0000
---

**Short version:** OpenAI's AI claims to have proved a maths puzzle that's been open since 2012. Part of that proof is a huge computer calculation. I checked the calculation with my own program, built from scratch. It came out right.

## Why bother?

On 6 October 2026, OpenAI released 722 maths papers written by an AI. Some claim to solve problems that experts have been stuck on for decades.

The catch: almost none of it has been checked by humans yet. Writing proofs is now fast. Checking them is slow. That's the bottleneck.

I'm not a mathematician. But some of these proofs lean on a big computer calculation, and a calculation is something anyone careful can re-check. So I picked one.

## The puzzle, in plain English

Think of a deck of n cards. There are lots of ways to shuffle it, and mathematicians break all those shuffles down into a set of basic "ingredients".

The puzzle asks: for almost every deck size, is there one ingredient that, when you combine it with itself, produces every ingredient at least once?

It's a bit like asking whether one paint colour, mixed with itself the right way, can produce every colour on the chart.

The paper says yes, for every deck size except 2, 4 and 9. For big decks it gives a written argument. For decks up to 64 cards it relies on the computer to check each case. That computer part is what I checked.

## What I found

**It holds up.** Every one of the 61 deck sizes checks out.

- **I re-ran OpenAI's own code.** Same answers.
- **I wrote my own program from scratch,** in a different language and using a different method. Same answers. For a 64-card deck, that meant confirming all 1,741,630 ingredients appear.
- **I filled a gap.** OpenAI's records said "it works" for each deck size, but never said *which* ingredient does the job. I found and published all 61.
- **A third check** using plain whole-number arithmetic agreed for smaller decks.

## What this doesn't prove

I checked the computer part, not the written argument for big decks. That still needs real mathematicians to read it.

## Why it matters

AI can now produce maths faster than people can check it. Re-checking is how science decides what's true, and some of it doesn't need a PhD. There are plenty more results in OpenAI's release that could be checked the same way.

All the code and results are public: [github.com/realopjt/tensor-square-finite-check](https://github.com/realopjt/tensor-square-finite-check). I did this with help from Claude (Anthropic). The maths itself is OpenAI's.

---

## For mathematicians

**Claim checked.** The finite-range proposition of [Universal Tensor Squares for Symmetric Groups](https://github.com/openai/math/blob/main/preprints/Universal-Tensor-Squares-for-Symmetric-Groups-September-24-2026/main.pdf): for every 1 ≤ n ≤ 64 with n ∉ {2, 4, 9} there is a self-conjugate λ ⊢ n with g(λ, λ, ν) > 0 for every ν ⊢ n (the tensor square conjecture of Pak, Panova and Vallejo in that range).

**Witnesses.** OpenAI's log records only exit statuses. I recovered the witness λ for each n by adding a print statement to a copy of their search (no change to its logic), e.g. λ = (13, 10, 8, 7, 6, 6, 4, 3, 2, 2, 1, 1, 1) at n = 64. Full list in the repo.

**Independent checker.** Rust, computing g(λ, λ, ν) = Σ_ρ χ^λ(ρ)² χ^ν(ρ) / z_ρ for all ν at once, modulo 2⁶¹ − 1 and 998244353 (OpenAI used 10⁹+7 and 10⁹+9). Differences from their implementation: rank-based partition indexing instead of hashed beta-set masks; border strips found from cells of hook length k and removed with the rim-hook formula instead of abacus bead moves; equal parts of ρ grouped by multiplicity with weight 1/(k^m m!) and a Horner scheme; dimensions from the hook length formula. Every target is nonzero modulo each prime separately. Built-in checks: Σ_ν g·dim ν = (dim λ)², g(λ, λ, (n)) = 1, g(λ, λ, (1ⁿ)) = 1.

**Exact cross-check.** A separate Python implementation (Murnaghan–Nakayama on cell sets, exact rationals) matches the checker coefficient for coefficient for n ≤ 22, and confirms no covering square exists for n = 2, 4, 9.

**Scope.** Finite range only. The large-n argument is not checked.
