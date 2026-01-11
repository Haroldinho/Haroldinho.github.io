High-Level Architecture: Orchestrator-Worker Pattern
The system creates a Centralized Orchestration model where main.py acts as the controller, managing state and delegating specific tasks to specialized, stateless agents. There is no direct communication between agents; they pass data via the orchestrator.

1. The Agents (Workers)
Located in src/agents/, these classes are designed as stateless functional units.

Common Design:
Use google.genai with gemini-2.0-flash-lite.
Implement tenacity for robust retry logic.
Input: Pydantic models (LearningGoal, UserProfile) or strings.
Output: Pydantic models (Structured Output).
Specific Roles:
GoalAgent: Transforms user input into a structured LearningGoal (SMART goals, milestones).
DiagnosticAgent: Creates a baseline Quiz based on the learning goal.
OptimizerAgent: Generates study materials (FlashcardList) and compiles them into Anki decks (.apkg) via the tools module. Handles both new curriculum and remediation.
ExaminerAgent: Generates active assessments (Quiz) and evaluates user answers into an AssessmentResult.
2. Data & Communication (The Protocol)
Data Models (src/models.py):
Strict Pydantic models define the "language" of the system (e.g., LearningGoal, Flashcard, AssessmentResult).
Ensures type safety and consistent schemas across all agent interactions.
Communication Flow:
Agents do not talk to each other.
flow: Main reads data → calls Agent A → gets Result → calls Agent B with Result.
3. Storage (The State)
MemoryManager (src/memory.py):
Handles all persistence logic.
Format: Local JSON files stored in .coin_cache/.
Buckets:
learning_goal.json: The static plan.
user_profile.json: Dynamic state (mastery, history, current progress).
diagnostic_quiz.json / exam_quiz.json: Transient state for consistency during grading.
4. Tools
Anki Connection (src/tools/anki_connection.py):
A utility used by the OptimizerAgent to convert raw data (Strings/Dicts) into binary Anki Package files (.apkg).