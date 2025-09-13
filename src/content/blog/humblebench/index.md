---
title: "Measuring Epistemic Humility in Multimodal Large Language Models"
author: "Bingkui Tong"
publishDate: 2025-09-13
tags:
  - VLM
  - Hallucination
  - Benchmark
  - Safety
---


*Measuring when AI should say "none of the answers are correct".*

We build a new benchmark that asks multimodal large language models (a.k.a. visual language models) to abstain when every answer is wrong, an ability we call epistemic humility. The results show that today’s best models—both general-purpose and reasoning models—still struggle to hold back. When a system misreads an image and answers with confidence, the consequences can range from silly to serious: mislabeling lab equipment, misreading a road sign, or summarizing a medical report incorrectly. Most evaluations reward a model for picking the right option. The harder (arguably more important) test is whether it can recognize that none of the options are right and choose not to guess.

This is the gap we set out to fill with **HumbleBench**, a large scale benchmark that measures a model’s willingness to say "none of the answers are correct." Here are some examples of HumbleBench:

![](examples.png 'examples')

## What we build

HumbleBench presents image grounded multiple choice questions with five options, and the last option is set to **"None of the above"** (NOTA). The distractors are plausible rather than absurd. This setup tests not only recognition but also restraint: can a model detect that every specific claim about an image is wrong and choose to abstain.

The benchmark spans tens of thousands of questions drawn from panoptic scene graph annotations and covers three common hallucination risks:

- *Object* (existence and counts)
- *Relation* (how things are positioned relative to each other)
- *Attribute* (colors, textures, motions, and other properties)

By balancing across these three types, we avoid overfitting to a single error mode and surface a model’s general tendency to answer when it should abstain.

## How we assemble it

![](overview.png 'overview')

To build HumbleBench, we first mine fine-grained objects and relations from richly labeled images, which come from the [panoptic scene graph dataset](https://github.com/Jingkang50/OpenPSG). For attributes, we generate concise descriptors for cropped objects with InstructBLIP, then prompt those structured labels into GPT-4-Turbo to produce natural language questions and answers with carefully crafted distractors. Finally, we conduct human audit and filtering, keeping, correcting, or discarding questions to ensure clarity and correctness.

## How we test models

We evaluate a wide range of state-of-the-art multimodal large language models, both general purpose and reasoning tuned, under three complementary settings:

1. **HumbleBench original:** standard images and five choices, including NOTA
2. **Stress test 1 - HumbleBench-E:** we remove the single correct specific option so that only NOTA is correct
3. **Stress test 2 - HumbleBench-GN:** we replace images with Gaussian noise; again, the only correct choice is to abstain

On the main benchmark, top models perform well above the random guess baseline, but they are still far from perfect. The stress tests are more interesting. When only "none of the above" is correct, many models collapse, with some falling below random, which exposes a strong bias to pick something even when there is no clear evidence in the image. In the pure noise setting, several models still choose specific A to D options, which suggests that language priors, for example "the sky is blue", can overwhelm missing or unhelpful visual evidence. Below are some failure cases for Qwen2.5-VL, where correct options appear in green and the model’s chosen wrong options appear in red:

![](error.png 'error')

## What surprises us

- **Abstention is hard.** The NOTA only variants are simple to state but highly diagnostic. Many models that look competent on the standard task revert to guesswork when the safe choice is to refuse.
- **Bigger is not automatically better.** We observe mid-sized models outperform larger peers on humility oriented robustness, which hints that data curation and alignment objectives matter more than raw parameter count for this behavior.
- **Reasoning helps inconsistently.** Some reasoning tuned variants improve over their bases, while others regress. Generic chain-of-thought training does not automatically translate into better rejection of plausible but wrong options. The outcome depends on how the training objective treats uncertainty.
- **Vision and language coupling varies widely.** Noise experiments show clear differences in how tightly models gate answers on visual evidence. Two models with similar headline accuracy on the main task can diverge dramatically when the image signal disappears.

## Why this matters

In the lab, a mistake is a curiosity. In the real world, it can be a safety issue. Hallucinations in language models are well known, but systems that use both vision and language face additional traps because images encourage strong commonsense priors. A robust multimodal assistant should not only excel at picking correct answers, it should also recognize when no answer is correct.

We believe abstention should be a first class capability, not an afterthought. Measuring it directly moves the field from the question "Can the model pick the right option" to the question "Can the model refuse the wrong ones"

## Summary

HumbleBench introduces a large scale and human audited benchmark that tests whether multimodal large language models can abstain when all specific answers are wrong. Across standard and stress settings, top models exceed random chance but still overcommit, especially when the only correct choice is "None of the above" or when the image is pure noise. The benchmark spans objects, relations, and attributes, exposes distinct hallucination risks, and separates language priors from visual grounding. These findings underscore the value of measuring abstention directly and establish a clear baseline for building safer and more reliable vision and language assistants.

## Resources
- Paper: https://arxiv.org/pdf/2509.09658.
- Code: https://github.com/maifoundations/HumbleBench.
- Dataset: https://huggingface.co/datasets/maifoundations/HumbleBench.