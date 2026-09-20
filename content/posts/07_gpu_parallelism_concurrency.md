---
title: "Threads in a GPU - Parallelism and Concurrency"
date: 2026-08-24
description: Why can H100 have over 250,000 concurrent threads and 16,000 parallel threads?
tags: ["gpu", "rabbit-hole"]
draft: True

---

Modal's GPU Glossary has the following lines:
> An H100 SXM GPU draws at most 700 W and has 132 SMs, each of which has four Warp Schedulers that can each issue instructions to 32 threads (aka a warp) in parallel per clock cycle, for a total of 128 × 132 > 16,000 parallel threads running at about 5 cW apiece. Note that this is truly parallel: each of the 16,000 threads can make progress with each clock cycle.

> A single SM on an H100 can concurrently execute up to 2048 threads split across 64 thread groups of 32 threads each. With 132 SMs, that's a total of over 250,000 concurrent threads.

I'd like to understand the back-of-napkin calculation that ended up with these numbers. For that, I'd like to first understand how exactly parallelism and concurrency work in GPUs and develop the intuition for them. Dear reader, welcome to this journey. I hope you learn something the way I will.

### What is NVIDIA [H100](https://www.nvidia.com/en-sg/data-center/h100/)?
NVIDIA's GPU from Hopper series, named after Grace Hopper, launched in 2022. H200s with more memory bandwidth followed shortly after in 2023.

### What is the "SXM" in "H100 SXM GPU"?
(More specifically SXM5 for H100s) SXM is a GPU socket built my NVIDIA to setup the GPUs in places like datacenters. 