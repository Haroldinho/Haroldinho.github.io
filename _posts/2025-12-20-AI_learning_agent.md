---
layout: post
title: AI Learning Coach
date: 2025-12-19 20:55:11 -0500
categories: [LLM, Agentic AI]
tags: [llm, gemini, ]
---

## Motivation

After taking [Kaggle 5 days of AI](https://www.kaggle.com/learn-guide/5-day-agents), I became quite determined to create my own agentic solutions.
This is the first of these projects.
The project is an effort to create an agentic workflow that demonstrates the use of persistent memory and being able to adapt the recomendations to the user's actions.

As a self-proclaimed continuous learner, one of the toughest challenge is to test myself often and early to achieve continuous progress. 
The motivation for the AI learning Coach was to create a learning coach to help me be more effective in my self-imposed studies ... and maybe it could help others too.

## Objectives

The main objective is to create a group of agents that work together to support the learner every step of the way from deciding what to learn, to creating a learning plan, to assessing the user and providing flashcards for offline active recall.
This mixed of online and off-line learning is often used by other continuous learners such as medical students.

A secondary objective was to start using Google Antigravity.
Most of the code was built in the Antigravity IDE with Gemini 3.0. Pro as the main LLM.

## Description

The project is an automated, full-cycle learning assistant. It starts by taking a topic you want to learn, creating a custom curriculum with SMART goals, generating study materials (Anki flashcards), and continuously testing your knowledge to ensure you are ready to advance to the next level.

## 🤖 The Agents

The system is powered by four specialized AI agents, each handling a specific phase of the learning lifecycle:

1.  **GoalAgent** (`src/agents/goal_agent.py`):
    *   **Role**: The Planner.
    *   **Function**: Takes your broad topic (e.g., "Quantum Physics") and converts it into a concrete, 30-day SMART learning plan.
    *   **Output**: A structured schedule with milestones and estimated timeframes.

2.  **DiagnosticAgent** (`src/agents/diagnostic_agent.py`):
    *   **Role**: The Assessor.
    *   **Function**: Before you start, it generates a diagnostic quiz to check your current knowledge level.
    *   **Utility**: Helps establish a baseline so you know where you stand.

3.  **OptimizerAgent** (`src/agents/optimizer_agent.py`):
    *   **Role**: The content Creator.
    *   **Function**: For each milestone, it generates high-quality study materials, specifically formatted for *Spaced Repetition*.
    *   **Output**: Creates ready-to-import Anki Flashcard packages (`.apkg`).

4.  **ExaminerAgent** (`src/agents/examiner_agent.py`):
    *   **Role**: The Teacher.
    *   **Function**: After your study period, it creates a challenging exam for the current milestone. It grades your answers, provides feedback, and decides if you have passed or need to review.
    *   **Feature**: Includes *Active Recall* questions from previous milestones to ensure long-term retention.

## Prompts

There are two types of prompts:
a. the prompts to generate the code
b. the prompts for the agents

### Agent prompts
. Each agent uses specialized system instructions to enforce roles:
*   **GoalAgent**: "You are an expert Learning Coach..."
*   **DiagnosticAgent**: "You are a specialized diagnostic assessment bot..."
*   **OptimizerAgent**: "You are an expert educational content creator..."
*   **ExaminerAgent**: "You are a strict but fair academic examiner..."


## AI Architecture and Workflow

The system is designed as a multi-agent pipeline
with a clear evaluator and optimizer:

1.  **Initialization**: `GoldAgent` analyzes the user's request and builds a `LearningGoal` with milestones.
2.  **Assessment Loop**:
    *   **Diagnostic**: `DiagnosticAgent` creates a baseline quiz to tailor the experience.
    *   **Study Phase**: `OptimizerAgent` generates an Anki deck (`.apkg`) for the current milestone, tailored to the user's current level. (optimizer)
    *   **Examination**: After a study period (simulated or real-time), `ExaminerAgent` generates a test based on the milestone content and previous knowledge (Active Recall). (evaluator)
    *   **Progression**: If the score is > 80%, the user advances to the next milestone. Otherwise, remediation material is generated to address weak points. 

![Architecture of the AI agents]({{"/assets/img/architecture_diagram.png" | relative_url}})
*Figure 1: AI Learning Coach Architecture *


## 💾 Memory
The project uses a file-based JSON caching system located in `.coin_cache/`.
-   **`learning_goal.json`**: Stores the structured plan and milestones.
-   **`user_profile.json`**: Tracks progress, completed milestones, and assessment history.
This allows the state to persist between sessions.


## Single User Experiment


## Conclusion



## Further Work and Extensions

There are a few next steps for this project:

- Create a website or an application that would make it easier for others to test the application and provide feedback.
- Test and extend the application to more demanding use cases such as the medical fields or even more advanced engineering application where Retrieval Augmented Generation may be needed to guarantee the integrity of the information provided.
- Make everything more adaptive. The ultimate is that your learning coach should be unique to you, what you want to achieve and the way you learn best. 

There are a few next steps after this project:

- Use Google-adk or OpenAI SDK to create a more ambitious project that would solicit the use of more tools and more sophisticated architecture.
- Create my own tools and functions that can be used by others.

## A note on Learning

This section is a meta-reflection on learning based on {% cite brown_make_2014 %}.


The agentic system adresses some of the lowest levels of learning such as retrieval practice and paced practice.
It is the intent of the author to augment the agents so they can prompt the user to go a bit deeper by being able to reflect on their learning and their responses, as well as by encouraging the user of the project to go out and apply their knowledge to new fields, make parallels and extensions to what they already know.


With respect to Bloom's Taxonomy of knowledge, the current tool addresses level 1: acquiring knowledge, level 2: gaining understanding and level 3: apply knowledge to solve problems.
It should be possible to extend the tool for level 4: analyzing an idea to make inferences and level 5: synthetizing knowledge and understanding to create new ideas. That would transform the tool from a learning tool to an education tool and achieve the true end goal of generating deep knowledge.
The last level, level 6: Evaluating opinions and data in light of the knowledge gained, may be hard to evaluate with current agents technology.


## Github project:

Check out the code in [github](https://github.com/Haroldinho/ai_learning_coach/tree/master)

## References

{% bibliography --cited %}