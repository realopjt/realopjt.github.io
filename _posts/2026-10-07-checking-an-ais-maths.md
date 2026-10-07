---
layout: post
title: "Checking an AI's maths"
date: 2026-10-07 09:00:00 +0000
---

I independently checked one of the results in OpenAI's new maths release, and it holds up: all 61 cases confirmed, by code that shares nothing with OpenAI's.

On 6 October 2026 OpenAI published 722 papers written by an unreleased model, claiming progress on problems mathematicians have worked on for decades. Most have not been peer reviewed, and only about a third have any computer-checked proof. The hard part is no longer producing proofs. It is checking them. I am not a mathematician, so I picked the kind of claim an outsider can check properly: one that comes down to a finite computation.

## The result I checked

One of the papers, [Universal Tensor Squares for Symmetric Groups](https://github.com/openai/math/blob/main/preprints/Universal-Tensor-Squares-for-Symmetric-Groups-September-24-2026/main.pdf), claims to prove the tensor square conjecture of Pak, Panova and Vallejo, which has been open since 2012.

In plain terms: the ways of shuffling n objects have a set of basic building blocks, its irreducible representations. The conjecture says that for almost every n, one of those building blocks, combined with itself, contains every building block at once. The only exceptions are n = 2, 4 and 9.

The paper proves this with a general argument for large n. For n up to 64 it relies on a computer search instead. That search is what I checked.

## What I found

The claim holds in all 61 degrees from n = 1 to 64 (excluding 2, 4 and 9).

- **OpenAI's own scripts pass.** I re-ran them unchanged on separate hardware.
- **The missing witnesses are now public.** OpenAI's log only records "success"; it never says which building block works for each n. I recovered and published all 61, for example (13, 10, 8, 7, 6, 6, 4, 3, 2, 2, 1, 1, 1) at n = 64.
- **An independent program agrees.** I wrote a new checker in a different language, using different arithmetic and a different method for each step. It confirms every case: at n = 64 that is all 1,741,630 building blocks, each one shown to be present.
- **Exact whole numbers agree for small n.** A third method, with no shortcuts in the arithmetic, matches the checker exactly up to n = 22 and confirms nothing works for 2, 4 and 9.

The largest case took under six minutes on an ordinary two-core cloud machine.

## What this does not prove

This covers the finite part of the paper only. The general argument for n above 64 is a written proof, and it still needs review by mathematicians in the field. My checker is independent in its code and arithmetic, but it rests on the same standard formulas any such check would use. And the witnesses came from OpenAI's search; my program confirms them rather than finding them.

## Why it matters

AI can now produce research mathematics faster than people can check it. Independent replication is how the field decides what stands, and much of it does not need a professor: any claim that reduces to a computation can be rechecked by someone careful, with ordinary hardware. There are plenty more in OpenAI's release.

Everything is public, with instructions to reproduce it in about 25 minutes: [github.com/realopjt/tensor-square-finite-check](https://github.com/realopjt/tensor-square-finite-check). I did this work with Claude (Anthropic). The mathematics and the original computation are OpenAI's.
