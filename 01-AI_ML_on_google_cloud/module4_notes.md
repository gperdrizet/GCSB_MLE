# 4. Generative AI

## 4.1. Introduction

- Generative AI and workflow
- Gemini multimodal
- Prompt design
- Model tuning
- Model garden
- AI solutions
- Lab: Vertex AI studio

## 4.2. Generative AI and workflow

Generative AI = transforming the way we interact with technology! It generates content for you!

### Google generative models

- Gemini: multimodal
- Gemma: language generation
- Codey: code generation
- Imagen: image processing

Pre-trained vs fine-tuned.

### Generative AI workflow

Via Vertex AI Studio

- Prompt
- Prompt screening
- Foundation model (with optional customization via training/fine-tuning)
- Grounding/citation checking/safety checking
- Response

## 4.3. Gemini multimodal

Works with text, image and video.

### Prompt components

1. Input: question, task, entity, completion
2. Context: instructions of information
3. Examples: zero-shot, one-shot, few-shot

## 4.4. Prompt design

Zero-shot: describe the task with no other input
One-shot: provide one example with input
Few-shot: provide multiple examples with input

Free-form prompting - good for just typing in a question. Structured prompting, can add context and examples to be injected with each user input.

Also has options to modify temperature, top k and top p. Pretty basic, manual/subjective tuning via GUI.

## 4.5. Model tuning

Full-scale fine-tuning is resource intensive.

Parameter efficient tuning techniques

- Adapter tuning
- Reinforcement tuning
- Distillation

Vertex AI studio - model tuning GUI.

## 4.6. Model garden

Access to other models outside of Google. Integrated with Vertex AI studio. Has many models organized by capabilities in the following categories:

1. Foundation
2. Task specific models
3. Fine-tunable models (open source models)

## 4.7. AI solutions

Vertical solutions: specific problem in specific industry
Horizontal solutions: similar problem across industries

EX: contact center AI - make it easy for business to set up AI for customer service.

## 4.8. Lab introduction

Gemini multimodal on Vertex AI studio.

- Analyze images with Gemini
- Explore Vertex AI Studio Freeform mode
- Design text prompts for zero-shot, one-shot, and few-shot prompting
- Generate conversations with chat prompts

## 4.9 Lab: Get Started with Vertex AI Studio

See [Vertex AI lab](https://github.com/gperdrizet/GCSB_MLE/blob/main/01-AI_ML_on_google_cloud/module4_lab_notes.md) notes.

## 4.10. Summary

## 4.11. Quiz

Score: 100% (7 multiple choice questions)

## 4.12. Reading

See [module 4 reading list](https://github.com/gperdrizet/GCSB_MLE/main/course_01/01-AI_ML_on_google_cloud/module4_reading_list.pdf) PDF.
