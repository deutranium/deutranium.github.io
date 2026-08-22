---
title: Don't use big models for specific tasks
date: 2026-08-22
description: Best case, we discover a new optimized method. Worst case, we learn about the current optimizations. Either way, sounds like a win to me.
tags: ["programming"]
draft: False

---

As someone who works in inference, I'm not a fan of how every user needs GBs and GBs of KV cache. I still remember the days when one 8GB Sandisk pen drive was the maximum I had for portable storage, and seeing how one session of Gemma 4 31B needs two such pen drives for cache is just ....sad.

Towards our unified goal of optimizing compute, I'll now try to figure out how easy/difficult it can really be to finetune a model. I'll have a series of blog posts over the next few weeks covering the following: 

Part 1
1. Defining the task, getting (high quality) data. Spoiler alert: it's [train tracks puzzles](https://puzzlemadness.co.uk/traintracks/medium).
2. Building verifiers
3. Benchmarking and evaluation system
4. [side quest] What representation of the task is most understandable by LLMs?

Part 2
1. How much do we hillclimb with SFT?
2. How much do we hillclimb with DPO?
3. How much do we hillclimb with RLVR (RL w/ verifiable rewards)
4. How useful is prompt optimization (DSPy)?
5. What do we learn from the three flavours of finetuning?

Some priorities and assumptions:
1. My focus is to make a small LLM really good at a specific task. This probably means that it can reason less about string theory, but that's okay. Our focus is a specific task and that's all we care about
2. We assume that we have unlimited great quality data. The focus is on post-training techniques instead of data collection ones. I'll have an in-depth verifier design section, but that's still more about post-training than data.
3. We have limited compute and hence can't go past a model of certain size

In an ideal world, I would move on to efficient inference after Part 2, but there are too many unknowns for me to plan. Hopefully Part 3 would be efficient inference for this finetuned model, but we'll find out what happens together.

