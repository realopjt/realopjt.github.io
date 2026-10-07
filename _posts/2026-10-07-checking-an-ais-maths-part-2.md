---
layout: post
title: "Checking an AI's maths, part 2"
date: 2026-10-07 12:00:00 +0000
---

A second result from OpenAI's maths release checks out: the computer-verified core of its proof of Foulkes' conjecture for sixth powers holds for every case I have run so far, b = 6 to 20, with no exceptions.

In [part 1]({% post_url 2026-10-07-checking-an-ais-maths %}) I checked a result about symmetric groups. This time I picked something harder: a paper whose proof leans on about 1.2 million exact comparisons of very large whole numbers.

## What the conjecture says

Take polynomials of degree b, and then form all products of 6 of them. Now do it the other way round: polynomials of degree 6, multiplied in groups of b. Both give the same total degree, 6b. Foulkes asked in 1950 whether the first collection always fits inside the second, with room to spare, once b is at least 6.

In practice that means splitting each collection into its basic building blocks and counting how many copies of each block appear. The conjecture says the second count is never smaller than the first, for every block. It was known for groups of up to 5. OpenAI's paper, [Foulkes' Conjecture for the Sixth Symmetric Power](https://github.com/openai/math/blob/main/preprints/Foulkes-Conjecture-for-the-Sixth-Symmetric-Power-September-25-2026/main.pdf), claims the sixth case for every b ≥ 6.

The proof has three layers. For b from 6 to 25 it compares every count directly by computer. For b up to 149 it uses computer-generated bounds. Beyond that it is a written argument.

## What I checked and found

I checked the first layer, b = 6 to 25, in two ways.

- **OpenAI's own code, re-run.** Their programs ran unchanged on separate hardware and matched all 62 published output lines exactly.
- **A new program, built from scratch.** It counts the building blocks by a different route: it first tallies every individual "weight" of each collection, then converts those tallies into block counts in one final step. OpenAI's code works with the blocks directly and uses shortcut bounds to skip most cases. Mine skips nothing and computes every count exactly.

Result so far: for every b from 6 to 20, the second count is at least the first for every block, with no exceptions. At b = 20 that is 436,140 blocks compared, with individual counts as large as 4 quintillion (4 × 10¹⁸). b = 21 to 25 are still running and take several hours each.

The program also passed four classical test cases with known answers, and at every b its block counts add back up to the exact size of each collection. Where the two programs report comparable numbers, they agree.

## What a different program means

A different program does not mean a different solution. The mathematical claim is fixed: these counts, compared this way. Two independent programs that compute the same counts by different routes and agree make it very unlikely that a bug produced the answer. If the programs had disagreed, that would have pointed to an error in one of them, or in the paper.

This checks the computational base of the proof only. The bounds for b up to 149 and the written argument beyond that still need review by mathematicians.

## Code and results

Everything is public, with instructions to reproduce it: [github.com/realopjt/foulkes-sixth-power-check](https://github.com/realopjt/foulkes-sixth-power-check). This work was done with Claude (Anthropic). The mathematics and the original computation are OpenAI's.
