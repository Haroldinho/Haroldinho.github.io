High-Level Architecture: Client-Server (Updated 2026-01-10)
The system has evolved from a local CLI into a distributed application. The Orchestrator (main.py) has been wrapped in a REST API, allowing a native iOS client to drive the experience while the heavy lifting (AI Agents) remains on the server.

1. Deployment & Infrastructure
Containerization: Dockerfile builds a lightweight Python 3.11 image.
Server: A FastAPI application (backend/main.py) runs on port 8000.
Persistence: The container uses a file-based cache (.coin_cache) which is now partitioned by user_id to support multiple users.
Client: A native iOS app (SwiftUI) acts as the frontend interface.

2. The Agents (Unchanged Logic)
The backend instantiates the same stateless agents from the CLI version (GoalAgent, DiagnosticAgent, OptimizerAgent, ExaminerAgent) as singletons. They perform the same logical tasks but are now triggered by HTTP requests instead of user input in a terminal.

3. Data & Communication
External Protocol (Client <-> Server):
Transport: REST API (JSON) over HTTP/HTTPS.
Auth: X-User-ID header identifies the user/device, ensuring data isolation on the server.
Network Layer: APIService.swift manages all network calls, decoding JSON responses into Swift models (Project, Flashcard, Question).
Internal Protocol (Server <-> Agents):
The FastAPI endpoints call agent methods directly, passing Pydantic models.

4. Storage (Dual-State) - ENHANCED
Server-Side (Source of Truth):
MemoryManager: Extended to handle paths dynamically: .coin_cache/{user_id}/{project_id}/.
Files: Stores learning_goal.json, user_profile.json, etc., on the server's filesystem.
Flashcard Caching: New caching layer stores generated flashcards per milestone (flashcards_{milestone_title}.json) to avoid wasteful regeneration.
Remediation Cache: Separate cache for remediation flashcards (flashcards_remediation.json).

Client-Side (Local Cache) - ENHANCED:
Identity: UserPersistence stores a UUID in UserDefaults to maintain session identity.
Project Persistence: currentProject is now persisted to UserDefaults and auto-restored on app launch.
Offline Capability: The app caches flashcards locally using SwiftData with deduplication logic and supports syncing (sync_flashcard_progress endpoint), allowing users to review offline and sync later.

5. Tools & Content Delivery - OPTIMIZED
Flashcards:
Generation: The OptimizerAgent generates Anki-compatible structures.
Server-Side Caching: Flashcards are generated once per milestone and cached. Subsequent requests load from cache.
Delivery: Backend extracts raw card data (Front/Back/Tags) and sends it as a JSON list (FlashcardResponse).
Rendering: The iOS app renders these cards natively in its own UI (FlashcardStudyView), replacing the need for the Anki app.
Remediation: New endpoint (/projects/{id}/flashcards/remediation) provides targeted cards for users who fail exams.

6. Cross-View Synchronization - NEW
All views (FlashcardStudyView, DiagnosticView, ExamView) use .onChange(of: currentProject) modifiers to reactively update when the selected project changes.
Project selection now fetches full milestone details from the server, ensuring complete data availability.

7. Key Improvements Since Initial Analysis
Backend: Implemented flashcard caching reducing API calls and ensuring consistency.
iOS: Added project persistence preventing state loss on app restart.
iOS: Implemented flashcard deduplication preventing duplicate entries.
iOS: Added cross-view reactive sync ensuring data consistency across tabs.
API: Enhanced /projects/{id} endpoint to return full milestone details.
API: Added /projects/{id}/flashcards/remediation for adaptive learning.