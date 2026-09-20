---
title: "Does 1D text make LLMs structurally bad at 2D?"
date: 2026-09-21
description: "[Garage Door up] Some notes about how exactly LLMs are bad at grids"
tags: ["garage", "llms"]
draft: False

---
I recently came across [Work with the garage door up](https://notes.andymatuschak.org/zCMhncA1iSE74MKKYQS5PBZ), and this is the first post following that philosophy! I'll try to make my posts as accessible as possible for new folks, while making sure they're in-depth enough to tickle the brain of seasoned ones as well. I'll focus on developing intuition, identifying first principles and dicussing the "why" behind choices. A former colleague once said "most of software engineering is just about making choices" - for better or worse, he seems to be right on this.

## Prerequisites

### Simple explanations of some terms you'll see here:
1. **Decode**: Inference stage when LLM generates the output of a prompt sequentially, one token at a time.
2. **Positional encoding**: Every input token comes with a vector representing its position in the current context (input prompt).


### What to expect
This blog would be a journey of evals, experiment design, research methods, LLMs, and most importantly, curiosity. For every question (and follow-ups), I've created a separate section for easy navigation. 
<details>
    <summary>Here's a list of questions we'd like to answer:</summary>
    <ol>
        <li>Does 1D text make LLMs structurally/inherently bad at 2D? (Yes)</li>
        <li>Does the same difference also show up in vertically written languages? (Yes)</li>
        <li>Is the difference same for different "operations" in 2D? (~)</li>
        <li>How do frontier models perform? (Bad, tried with Astra)</li>
        <li>Does switching to image modality help? (No)</li>
        <li>What are the tasks where this difference suddenly vanishes? Do we have any theories about why? (Math, yes)</li>
        <li>How can we fix this? (Positional encodings)</li>
        <li>Did it get fixed?</li>
    </ol>
</details>



My focus is on understanding the behaviour of LLMs without any external appendages like tools, harness etc. I'm using vanilla OpenRouter API for the following models: [!!! ADD MODEL NAMES !!!]. This is not a benchmark of any sorts (could easily be converted into one, feel free to take it up), and **the goal is to understand if the transformer architecture itself, or the parts of it create some structural limitations**.
<details>
    <summary>Why these models specifically?</summary>
Tradeoff between costs and expected ROI. I wanted to evaluate frontier models (Astra) to know if the limitation vanishes with scaling. I used a Qwen one because I'd like to answer question #2 above using Chinese and Qwen might be better at it than others. Gemini was included to add some extra vendor variety, and Sol included was to have a direct comparison point for Astra.

One could add Anthropic models to the mix as well but I don't have a strong reason to believe why they'd be any different, plus they seem too expensive. We have enough models to compare different levels of "intelligence" for now, and token costs shoot up very fast when you let curiosity dictate it.
</details>


## Does 1D text make LLMs structurally bad at 2D?
My hunch is yes, but to validate it, we need to evaluate LLM performance on some inherently 2D tasks. In simple words, we're trying to figure out if LLMs are bad at columns as compared to rows. Here are some reasons why my guess us yes:
1. Most text is 1D, or at least treated like that. We use `\n` to indicate new lines, but it's still represented as a 1D vector, albiet with `\n`s. Please refer to the closing comments section about my thoughts on this.
2. The decode process or one-by-one tokens inherently has a 1D behaviour. There is no reason for me to beleive that it'll output a 2D grid in one go, but would instead follow a what can very simply be called a "[typewriter](https://www.reddit.com/r/singularity/comments/1b7zuq8/the_typewriter_limitation_for_llms_the/)" behaviour.