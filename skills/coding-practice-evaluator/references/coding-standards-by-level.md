# Coding Standards & Principles, by Level

Reference for calibrating what to flag and how deep to go during an
evaluation. Match the level(s) below to the learner's current
`profile.md` reading — don't reach into a higher level's concerns for a
learner who hasn't solidified the level below it.

## 1. Beginner Level
Basic coding conventions, clean syntax habits, and fundamental program logic rules.

- **Meaningful and Descriptive Naming** — variable, function, and class names should clearly reveal intent, purpose, and usage without needing extra comments.
- **Avoid Magic Numbers and Strings** — use named constants, enumerations, or config files instead of hardcoding numeric/string literals into logic.
- **Keep Functions Short and Single-Purpose** — concise functions that do exactly one job well, for readability and easy unit testing.
- **DRY (Don't Repeat Yourself)** — eliminate duplication by encapsulating repeated logic into reusable functions, classes, or modules.
- **KISS (Keep It Simple, Stupid)** — clear, simple, straightforward code; avoid unnecessary complexity.
- **Use Comments Sparingly and Meaningfully** — omit comments for self-explanatory code; use them mainly to document "why," not "what."
- **Follow Established Coding Standards** — adhere to team conventions and language style guides for formatting, indentation, structure.
- **Loop Termination and Boundary Checks** — clear and achievable loop termination conditions; test array indexes against bounds; explicitly test divisors against zero.
- **Version Control Practices** — track changes with a revision control system (e.g. Git) to maintain history and collaborate safely.

## 2. Intermediate Level
Object-oriented design principles, defensive programming, refactoring techniques, and structured peer reviews.

- **Single Responsibility Principle (SRP)** — each class or module should have only one reason to change.
- **Open/Closed Principle (OCP)** — open for extension (e.g. via interfaces/inheritance), closed for modification.
- **Liskov Substitution Principle (LSP)** — derived classes/subtypes must be fully substitutable for their base classes without breaking correctness.
- **Interface Segregation Principle (ISP)** — split large, general-purpose interfaces into smaller, specific ones so clients only depend on what they use.
- **Dependency Inversion Principle (DIP)** — high- and low-level modules should depend on abstractions (interfaces), not concrete implementations.
- **YAGNI (You Ain't Gonna Need It)** — build functionality only when it's currently required; avoid speculative development.
- **Separation of Concerns** — divide software into distinct layers (UI, business/domain, data storage), each handling one concern.
- **Fail Fast** — detect and report parameter errors or invalid states immediately, at the start of a function, to prevent downstream errors.
- **Encapsulate Nested Conditionals** — extract deeply nested conditionals into dedicated, descriptively-named helper functions.
- **Continuous Refactoring** — regularly improve internal structure and maintainability without changing external behavior.
- **Defensive Programming & Input Validation** — validate imported data, input parameters, and pointers; ensure proper deallocation of memory/resources.
- **Unit Testing & TDD** — automated tests covering expected behavior, invalid parameters, and error conditions.
- **Peer Code Review Rules** — review under 200–400 LOC at a time, keep inspection speed under 300–500 LOC/hour, cap sessions at 60–90 minutes, require author preparation/annotation.

## 3. Beginning of the Industry Level (Junior / Professional Production Level)
Production security hygiene, API design, cloud application standards, observability, and technical debt management.

- **The Twelve-Factor App Methodology** — standard cloud-native practices: one tracked codebase, explicitly declared dependencies, config stored in the environment, stateless processes, dev/prod parity.
- **Coding for Observability** — structured logging (e.g. JSON with contextual IDs), request metrics, distributed tracing — while avoiding logging sensitive data.
- **Injection Attack Prevention** — parameterized queries, prepared statements, whitelist validation to prevent SQL/code injection.
- **API Security & Design** — protect endpoints with strong token-based auth (OAuth, JWT), enforce rate limiting/throttling, use standard HTTP status codes.
- **Secure Communication & Security Headers** — enforce HTTPS/TLS everywhere; configure headers like CSP, HSTS, X-Content-Type-Options, X-Frame-Options.
- **Container & Build Security** — minimal, trusted base images; static security scans in CI/CD; keep third-party dependencies patched.
- **Managing Technical Debt** — a strict "Definition of Done"; track deliberate vs. accidental debt; fold refactoring into regular sprint cycles.

## 4. Top Industry Level (Senior / Enterprise Architecture / Systems Level)
Enterprise application architecture, complex distribution strategies, concurrency patterns, and system design laws.

- **First Law of Distributed Object Design** — avoid distributing objects across network/process boundaries when possible; keep fine-grained objects local within a single process, put coarse-grained facades at distribution boundaries, or cluster instead.
- **Enterprise Domain Logic Patterns** — pick the domain structure by complexity: Transaction Script (procedural), Domain Model (rich OO business logic), Table Module (tabular/record-set workflows), or Service Layer (application boundaries).
- **Data Source Architectural Patterns** — decouple database interactions with Table/Row Data Gateways, Active Record (matches domain to table structure), or Data Mapper (isolates complex domain models from relational schemas).
- **Object-Relational Behavioral & Structural Patterns** — manage DB state with Unit of Work (atomic change tracking), Identity Map & Lazy Load (in-memory identity / deferred loading), and Inheritance Mapping (Single/Class/Concrete Table).
- **Offline Concurrency Control** — support multi-system-transaction business transactions with Optimistic Offline Lock, Pessimistic Offline Lock, Coarse-Grained Lock, or Implicit Lock.
- **Web Presentation & Distribution Patterns** — structure enterprise web presentation with MVC, Front Controller, Template/Transform View; use Remote Facade and DTOs to optimize calls across remote boundaries.

## How to use this during evaluation

- A learner whose `profile.md` says "beginner" should mainly be flagged on
  **Level 1** items, plus obvious outright bugs. Don't bring up SRP or
  Twelve-Factor to someone still shaky on naming and loop bounds.
- Move to **Level 2** concerns once Level 1 habits look solid across
  multiple sessions (per `mistakes-log.md`).
- **Levels 3–4** are mostly out of scope for a learner still building
  fundamentals — keep them in mind for later, more advanced sessions, or
  for the future Practice/Guide skills, rather than raising them here.
