Comparison: CLI vs. Distributed Architecture (Updated 2026-01-10)

The project has successfully transitioned from a Monolithic CLI Utility to a Distributed Client-Server Application. While the core intelligence (Agents) remains identical, the surrounding infrastructure, data management, and user interaction layers have been fundamentally decoupled. Recent improvements have further enhanced the distributed architecture's robustness.

1. Architectural Pattern
Feature	CLI Implementation (Standalone)	Distributed App (Client-Server)
Pattern	Monolithic / Controller Script	Service-Oriented / REST API
Controller	src/main.py (Script)	backend/main.py (FastAPI)
Execution	Synchronous/Blocking. The script runs in a loop, waiting for user input to proceed to the next step.	Asynchronous/Event-Driven. The server waits for HTTP requests. The User (Client) drives the flow.
Coupling	High. logic, UI (print statements), and state are strictly bound to the running process.	Low. The Backend (Intelligence) is completely decoupled from the Frontend (Presentation).
Caching	None (regenerates on every run)	✅ Server-side flashcard caching per milestone (NEW)

2. User Interface & Interaction
Feature	CLI Implementation	Distributed App
Interface	Terminal (Text-based).	Native iOS App (SwiftUI).
Input	Raw text via input().	Structured UI (Buttons, Forms, Taps).
Feedback	print() statements.	Visual feedback, Animations, Native Alerts.
Dependency	Requires Python installed locally on the user's machine.	Zero dependencies for the end-user (Server is hosted remote).
State Persistence	Session-based (lost on exit)	✅ Persisted locally and remotely (NEW)

3. Data & State Management
Feature	CLI Implementation	Distributed App (ENHANCED)
Tenancy	Single Tenant. Assumes one user per machine.	Multi-Tenant. Uses X-User-ID to partition data (.coin_cache/{user_id}/).
Session	Ephemeral. State often lives in Python variables (current_milestone) during execution.	✅ Stateless API but persistent storage. State saved to disk (JSON) and iOS UserDefaults (NEW)
Identity	None.	UUID generation per device (UserPersistence).
Project Selection	Lost on restart	✅ Auto-restored on app launch (NEW)
Flashcard Cache	None	✅ Server-side per-milestone caching (NEW)

4. Content Delivery (The Mechanics)
Feature	CLI Implementation	Distributed App (IMPROVED)
Flashcards	External Dependency. Generates an .apkg file that the user must manually import into the Anki Desktop app.	✅ Integrated Experience with caching. Cards generated once, cached on server, sent as JSON. iOS renders natively with deduplication. (IMPROVED)
Exams	Text-based Q&A loop in the terminal.	Interactive Form in the iOS App.
Remediation	Generated but not easily accessible	✅ Dedicated endpoint with iOS support (NEW)
Efficiency	Regenerates content on every run	✅ Caching eliminates redundant generation (NEW)

5. Core Logic (The shared "Brain")
Feature	CLI & Distributed App
Agents	Identical. Both use src/agents/. This is the strongest point of the architecture; the "business logic" (Agentic Workflow) was portable because it was designed as stateless functional units returning Pydantic models.
Models	Identical. src/models.py serves as the shared language.

6. Cross-Component Communication
Feature	CLI	Distributed App (ENHANCED)
View Sync	N/A (single view)	✅ Reactive with .onChange modifiers (NEW)
State Propagation	Direct variable access	✅ @Published + SwiftUI reactive bindings (IMPROVED)
Data Freshness	Always fresh (in-memory)	✅ Cache-aware with automatic invalidation (NEW)

Summary Table (UPDATED)
Aspect	CLI	Distributed App (Post-Fixes)
Scalability	Low (1 user per install)	High (Server handles N users)
Deployment	git clone + pip install	Docker Container + App Store
Data Flow	Input → Function → Print	Request → Endpoint (Cache-Check) → JSON → View
Offline	Full Offline (Local)	Hybrid (Syncs progress, caches locally, intelligent offline mode)
State Persistence	None	✅ Full (Server + iOS)
Efficiency	Low (regenerates everything)	✅ High (caching + deduplication)
UX Quality	Basic	✅ Premium (native UI + reactive sync)

Conclusion: The refactor honors the Hexagonal Architecture (or Ports and Adapters). The "Core" (Agents/Models) remained untouched, while the "Adapter" changed from a Terminal Interface (main.py) to a Web API (FastAPI + iOS). Recent enhancements have addressed all critical architectural shortcomings, resulting in a production-ready distributed system that maintains the CLI's simplicity while adding robustness, efficiency, and superior UX.