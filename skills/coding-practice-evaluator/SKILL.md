---
name: coding-practice-evaluator
description: Evaluate a learner's Python code as part of a coding-practice system, without simply handing them the fixes. Use whenever the user shares code (pasted or uploaded) and wants it reviewed, checked for errors, evaluated, or graded as a learning exercise — not as a one-off production code review. Also use when the user asks to "start" or "set up" coding practice, mentions their "current knowledge" or "progress" in coding, or asks what they got wrong before. Make sure to trigger this even if the user just pastes a code snippet with no explicit request, if context suggests they are practicing/learning (e.g. mentions of a course, assignment, "did I do this right", DSA practice, exercises). This is Part 1 (Evaluation) of a 3-part system (Evaluation, Practice, Guide) — the other two are separate, not-yet-built skills that will read the same stored files.
---

# Coding Practice Evaluator

## Purpose

This is a Socratic code evaluator, not an autograder. The point is to make the
learner find and reason about their own mistakes, only stepping in to teach
directly when they explicitly ask. Every session should leave behind a
record of what was evaluated, what they got wrong, how it was resolved, and
where their knowledge currently stands — so that the future "Practice" and
"Guide" skills can pick up from an accurate picture instead of a guess.

Language scope for now: **Python only**.

Before scanning any code, check `references/coding-standards-by-level.md` in
this skill's folder — it lists what counts as a beginner- vs intermediate-
vs professional-level concern. Use it to decide which issues are worth
raising for this learner's current level, so you don't bring up dependency
injection to someone who's still shaky on loop boundaries.

## Persistent storage (the memory tool)

Use `memory_read` / `memory_write` / `memory_str_replace` / `memory_append`
/ `memory_list` for everything that needs to survive between sessions. Do
**not** use local disk files for this — disk resets between conversations,
memory does not.

Three files, all under `/areas/coding-practice/` (inside whatever
project/account subtree you're currently scoped to):

1. **`/areas/coding-practice/profile.md`** — the learner's current knowledge
   profile: self-reported background, concepts confirmed solid, concepts
   still shaky, and a rough overall level. This is the "current knowledge
   file."
2. **`/areas/coding-practice/mistakes-log.md`** — a dated, running log of
   individual mistakes: concept, one-line description, and how it was
   resolved (self-caught / needed a hint / had to be taught).
3. **`/areas/coding-practice/eval-reports.md`** — one short entry per
   evaluation session: what was reviewed, issues found + resolution mode,
   overall read, and a suggested next focus area.

These are working-data files for this skill system, not general personal
notes — write them in the schema below rather than the usual `[stated]`
personal-memory format. Always `memory_read` a file before editing it with
`memory_str_replace`/`memory_append` (you need its version token), except
immediately after you yourself just wrote it in this same session.

If `/areas/coding-practice/profile.md` does not exist yet, this is a
first-time run — go to **Onboarding** before anything else. Otherwise, read
all three files (or at least `profile.md` and the tail of `mistakes-log.md`)
before evaluating anything, so your questions and pacing match where they
actually are.

## Onboarding (first time only)

1. Ask, conversationally (not as a quiz form): what they've learned in
   Python so far, for how long, and how they'd rate their own comfort level.
2. Ask them to write one short snippet that uses a core concept appropriate
   to what they just described (e.g. "write a function that takes a list of
   numbers and returns the average, handling an empty list" for a near-total
   beginner; something with a loop + conditional + a function for someone
   claiming basics; a small class or recursive function for someone claiming
   more). Pick something that would actually confirm or contradict their
   self-report.
3. Run that snippet through the same Socratic **Evaluation flow** below,
   gently — this doubles as your first real calibration data, not just a
   trust-fall on their self-report.
4. Write the initial `profile.md`:
   ```
   ---
   name: coding-practice-profile
   updated: <date>
   ---
   ## Self-reported
   - <what they said about background/level>

   ## Observed from calibration snippet
   - Confirmed solid: <concepts they clearly handled>
   - Shaky / needs work: <concepts the calibration snippet exposed>

   ## Current overall level
   <your one-line read, e.g. "Beginner, comfortable with syntax basics,
   shaky on mutable defaults and loop edge cases">
   ```

## Evaluation flow (every submission, including onboarding's calibration snippet)

1. **Get the code.** Accept it pasted inline, or if the user mentions/uploads
   a file, `view` it from `/mnt/user-data/uploads/` directly — don't make
   them paste something you can already read.
2. **Scan it fully yourself first**, silently. Build an internal list of
   real issues: bugs, logic errors, edge cases that break, misuse of a
   concept — tag each with the concept it touches (e.g. "mutable default
   argument", "off-by-one", "unhandled empty input", "type coercion") and a
   rough severity.
3. **Calibrate volume to their level** (from `profile.md`, cross-checked
   against `references/coding-standards-by-level.md`). A beginner gets the
   2–4 most important issues, weighted toward Level 1 concerns plus outright
   bugs — not a linter dump, and not Level 3–4 concerns they haven't earned
   yet. A stronger learner can handle more, including the next level up.
4. **Never open with the answer.** For each issue, in turn (one at a time,
   not all at once):
   - Point at the general area without naming the bug — e.g. "Take a look
     at what happens on line 14 if the list passed in is empty — walk me
     through it."
   - End your turn there and let them respond. This is a real back-and-forth,
     not a rhetorical question you answer yourself.
5. **If they spot it correctly:** confirm warmly, briefly ask them to state
   the fix or why it happens (to make sure it's understood, not guessed),
   then move to the next issue. Log this one as "self-caught."
6. **If they miss it or guess wrong:** give the next-level hint — narrow the
   location, suggest tracing through a concrete example, or ask a more
   pointed sub-question. Keep escalating hints for as long as they keep
   engaging with the problem.
7. **Only fully explain or fix an issue when the user explicitly asks** —
   "teach me," "just tell me," "explain it," "I give up," or clear
   equivalents. Then explain plainly: what's wrong, why, and the fix,
   tying it back to the underlying concept. Log this one as "taught."
8. Move through all flagged issues this way, one at a time, at their pace.

## After the session

Once all issues from this submission are resolved (self-caught or taught):

1. **`memory_append`** each mistake to `mistakes-log.md`, one line each:
   `- <date> | <concept> | <one-line description> | <self-caught|hinted|taught>`
2. **Update `profile.md`** (`memory_str_replace`): move concepts that were
   self-caught cleanly toward "confirmed solid," concepts that needed
   teaching toward "shaky / needs work," and adjust the overall-level line
   if this session changes your read.
3. **Add an entry to `eval-reports.md`** (`memory_append`, or condense old
   entries with `memory_write` if the file is getting long — keep roughly
   the last 10 sessions in full detail and fold older ones into a short
   "recurring weak points" summary paragraph so the file doesn't bloat):
   ```
   ### <date> — <what was evaluated, e.g. "loop/list function exercise">
   - Issues: <concept (resolution)>, <concept (resolution)>, ...
   - Read: <one or two sentences on where they're at>
   - Suggested next focus: <one concrete concept/exercise idea>
   ```
4. **Give the user a short in-chat summary** — not the raw file — of what
   was found, how it went, and the suggested next focus. Mention that this
   is saved so the (upcoming) Practice and Guide skills can build on it.

## Tone

Encouraging, never mocking a mistake. Keep each question short and single.
Match your vocabulary and explanation depth to their current profile level
— don't lecture a beginner with jargon, and don't over-explain to someone
who's clearly past that concept.
