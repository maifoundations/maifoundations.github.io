---
title: 'Grounded Chain-of-Thought Makes Multimodal LLMs More Data-Efficient'
author: 'Jiaer Xia'
publishDate: 2025-07-04
tags:
  - VLM
  - Reasoning
  - Bootstrapping
  - Data-Efficiency
---

*Bootstrapping transforms expensive annotation into smart solutions.*


In the rapidly advancing field of artificial intelligence, Large Language Models (LLMs) have become pivotal in reshaping how we approach natural language processing and computer vision. These models, when combined with vision encoders, form Multimodal Large Language Models (MLLMs) that are capable of interpreting images through natural language prompts. This innovative fusion allows MLLMs to seamlessly handle both text and image data, opening up vast possibilities for applications across various domains.

However, the journey to harnessing the full potential of MLLMs is not without its challenges. A significant hurdle lies in the model's need for constant learning to tackle new, specialized tasks. While MLLMs excel at general image interpretation, they often stumble when faced with niche tasks like chart understanding. This difficulty arises from a mismatch between the datasets used for pre-training and those needed for specific tasks. MLLMs are primarily trained on object-centric images from the Internet, which limits their effectiveness with specialized non-object images such as charts and tables.

Moreover, the process of data annotation required for these specialized tasks is an arduous and resource-intensive endeavor. Creating large-scale, task-specific datasets demands significant time and effort, making it a bottleneck in the continuous learning process. As a result, ensuring that MLLMs can adapt and excel in specialized applications remains a complex challenge for researchers and practitioners alike.

To address these limitations, we propose the Grounded Chain-of-Thought (GCoT) approach. This simple yet effective strategy aims to inject grounding information into CoT data, enhancing the fidelity of reasoning steps to input images. By doing so, models trained with GCoT data can potentially achieve better generalization with limited training samples. Given the challenges in collecting grounded CoT data, we introduce a straightforward bootstrapping method: iteratively using an MLLM to generate grounding labels and refining them through self-verification. This work has been published in [ICCV 2025](https://iccv.thecvf.com/Conferences/2025).

## Enhance Grounding Ability with Bootstrapping Loop

Grounding is a very important capability, but the bounding box annotations used for training grounding capabilities are costly. To more efficiently enhance the model's grounding ability, we adopt a bootstrapping approach, leveraging the model's basic capabilities to automatically obtain accurate bounding box data for training.

Let's walk through the process:

1. **Creating Sub-Questions**: For each target, we ask a simple question like, "Where is the target?" This helps guide the computer to focus on finding that specific object.

2. **Generating Bounding Boxes**: The computer then tries to draw a box around the target in the image. This box is called a "bounding box," and it helps the computer understand where the object is.

3. **Checking Its Work**: The computer doesn't just stop there. It checks its work by looking at the object inside the box and comparing it to the original target. If it matches, great! This will be regarded as a correct bounding box.

4. **Learning and Improving**: After model generate and select the correct bounding boxes, it learns from it. This information is used to make the computer better at finding objects in future loop.

Here is a demo for the bootstrapping process:

<video src="/videos/gcot/Bootstrapping.mp4" width="720" controls></video>

## Inject Grounding information to CoT

Grounded Chain-of-Thought (GCoT) enhances models' understanding of visual data by ensuring they provide correct answers with clear, evidence-backed reasoning. After the initial bootstrapping learning phase, models integrate their thought process with precise bounding boxes as visual cues, to form GCoT. This refinement ensures models reason before answering, supported by verifiable visual evidence.

![](Framework.png 'Method')

During fine-tuning, the model generates GCoTs for the same questions, verifying both answer accuracy and visual evidence from bounding boxes. Only verified GCoTs are selected, with three chosen per question to diversify and enhance the training set. A crucial self-verification step ensures consistency between visual evidence and text, eliminating inconsistencies and enhancing reasoning reliability.

An example of the self-verification process is shown below:

![](Verification.png 'Verification')

Finally, the model is retrained with this refined data, improving performance and expanding the training dataset's quality. GCoT enables models to reason more like humans by using visual evidence, boosting accuracy and providing a transparent mechanism for verifying thought processes, making it a valuable tool in machine learning.

Below we show an application of GCoT in robotic manipulation where the model correctly understands the user intent and generates a grounded reasoning process to achieve the goal.

<video src="/videos/gcot/GCoT_Robot.mp4" width="720" controls></video>


## Summary
In summary, the paper presents an innovative approach to enhancing the adaptability and reasoning capabilities of Multimodal Large Language Models (MLLMs) through the Grounded Chain-of-Thought (GCoT) methodology. By addressing the challenges of data annotation and domain-specific task adaptation, the GCoT strategy injects grounding information into the reasoning process, allowing MLLMs to generalize better with fewer training samples. The bootstrapping loop facilitates efficient bounding box generation, leveraging the model's existing capabilities to self-improve. This iterative process not only enhances the model's grounding abilities but also refines its reasoning by ensuring answers are supported by verifiable visual evidence. Through self-verification and careful selection of GCoTs, the model achieves improved performance, offering a transparent and efficient means to tackle specialized tasks. This work highlights the potential of GCoT as a powerful tool for data-efficient model adaptation in multimodal AI systems.

## Resources
- Paper: https://arxiv.org/pdf/2507.02859.
- Code: https://github.com/maifoundations/GCoT.