---
title: "Learning to Think Fast and Slow for Visual Language Models"
author: "Chenyu Lin"
publishDate: 2025-11-27
tags:
  - VLM
  - reasoning
  - rl
  - dual-mode thinking
---

*Teaching Visual Languege Models to Think Fast and Slow*

Humans don't approach every problem with the same thinking level of effort. When the answer is obvious, our brain reacts instantly — recognizing a face, reading a sign, noticing an emotion. But when the problem becomes abstract or ambiguous, like solving geometry or interpreting a tricky chart, we slow down and begin to reason carefully. Psychologists describe this difference using **two complementary thinking systems**:

- **System 1 — fast, intuitive, effortless.** It works automatically, based on perception and experience.
- **System 2 — slow, analytical, deliberate.** It activates only when we need deeper reasoning.

Crucially, **the human mind does not choose these systems randomly**. We switch between them depending on the complexity of the task, saving effort where we can and investing it where we must. This simple efficiency principle allows humans to think both *quickly* and *carefully* without conscious control. If visual language models are meant to reason like humans, they should adopt the same principle—thinking fast on simple problems and slowing down only when deeper reasoning is truly needed.

# What we do?

To bring this dual-thinking principle into multimodal reasoning, we introduce **DualMindVLM**, a visual language model designed to think fast and slow depending on the difficulty of the task. Unlike existing reasoning-oriented VLMs—most of which rely solely on System-2-style chain-of-thought generation—DualMindVLM does not treat every question as a complex problem. Instead, it automatically chooses whether to answer concisely or reason step by step, just as humans do. The example below illustrates this contrast clearly: 

![](example.png 'example')

For a simple emoji recognition question, the System-2-only model writes a long reasoning chain to justify an obvious answer, while DualMindVLM responds directly and efficiently. On the other hand, when facing a challenging geometry task, DualMindVLM automatically activates slow, structured reasoning to solve the problem correctly.

# How it works?
