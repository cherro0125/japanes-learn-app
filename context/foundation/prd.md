---
project: "japanese-learn-app"  # working name
version: 1
status: draft
created: 2026-10-11
context_type: greenfield
product_type: mobile
target_scale:
  users: small
  qps: low
  data_volume: small
timeline_budget:
  mvp_weeks: 8
  hard_deadline: null
  after_hours_only: true
---

<!-- Lint override (2026-10-11): the AI provider names (Claude, ChatGPT) in User & Persona, FR-018, FR-024 and Open Question 1 were kept on purpose by the user. Connecting the learner's own subscription is treated as a product-scope decision, not a technology choice. -->

<!-- Revision (2026-10-12, after tech-stack selection): third-party apps cannot use a consumer Claude subscription (API keys only). The user decided: no AI in the MVP. FR-017 and FR-018 moved to nice-to-have, FR-018 and FR-024 reworded to a personal API key, Open Questions 1 and 2 resolved, a Non-Goal added. -->

# PRD: japanese-learn-app

## Vision & Problem Statement

A beginner self-learner of Japanese who studies by chatting with a general-purpose AI assistant drops the habit after a few days. The chat turns into a mess: they don't know where they stopped, the conversation context runs out, and it is hard to find their way around. Every session starts from zero, they must decide on their own what to learn and how to ask for it, the material arrives as a wall of text with no exercises or visual presentation, and nothing tracks progress or brings material back for review. What they want instead is a systematic, orderly path with continuity and repetition.

Insight (user's hypothesis, not yet verified against existing products): existing dedicated apps teach through English and are paid; their courses follow rigid paths that do not adapt to the learner; and grammar, vocabulary and explanations live in separate tools. Learning through Polish would make progress faster, the learner wants to shape the app's style to their own taste, and AI can assemble an adaptive path in one place while being paid for through AI subscriptions the learner already has. (Update 2026-10-12: the subscription part is disproven — see Open Question 1.)

## User & Persona

**Primary persona — the author.** Polish-speaking beginner learning Japanese on their own. Reaches for the product when sitting down to study, instead of opening a general AI chat. Has tried learning through an AI chat and abandoned it after a few days. Already pays for an AI subscription (Claude or ChatGPT). The MVP serves only this person.

## Success Criteria

### Primary
- Learning continuity: the learner keeps studying regularly for 3–4 weeks without abandoning the habit (today it breaks after a few days).

### Secondary
- The anime atmosphere (mocking sensei comments, quotes) makes the learner want to come back.
- A visible rank promotion.

### Guardrails
- Learning progress never disappears — no session or update loses the learner's state.
- Learning works without AI — lack of AI access does not block kana, vocabulary and reviews.
- Humor never overshadows learning — a mocking comment always comes with the correct answer and an explanation.


## User Stories

### US-01: Learner returns to study the next day

- **Given** a learner who finished a vocabulary lesson yesterday
- **When** they open the app the next day
- **Then** they get a check of the last lesson, followed by a proposal of what to learn next

#### Acceptance Criteria
- The check has 10–15 questions.
- If the break lasted more than 2 days, the app proposes repeating the lesson.
- The learner can skip the check and go straight to what's next.

## Functional Requirements

### Onboarding & level
- FR-001: Learner can go through an interview stating what they want to learn first, what to skip and what to pay attention to. Priority: must-have
  > Socrates: Counter-argument considered: "a beginner starting from zero doesn't know what to skip, so the interview may be an empty ritual." Resolution: kept, with sensible default answers.
- FR-002: Learner can take a short placement test across categories (word translation PL↔JP, sentence translation) that evaluates their level. Priority: must-have
  > Socrates: Counter-arguments offered ("test is meaningless without kana", "a test discourages at the start"). Resolution: no counter-argument; stands as written.
- FR-003: Learner can change their proficiency level during learning. Priority: must-have
  > Socrates: Counter-arguments offered ("conflicts with automatic adaptation", "lets the learner cheat past gaps"). Resolution: no counter-argument; stands as written.
- FR-004: Learner can choose the categories to learn. Priority: must-have
  > Socrates: Counter-arguments offered ("duplicates the interview", "a beginner will pick a bad order"). Resolution: no counter-argument; stands as written.

### Learning
- FR-005: Learner can learn hiragana. Priority: must-have
  > Socrates: Counter-argument considered: "katakana can wait — hiragana is enough for the first weeks of vocabulary and grammar." Resolution: split; hiragana stays must-have, katakana moved to FR-023 (nice-to-have).
- FR-006: Learner can learn vocabulary (translation PL↔JP). Priority: must-have
  > Socrates: Counter-argument considered: "where do the Polish translations come from? Most open dictionaries are JP↔EN." Resolution: kept; translations come through the learner's connected AI subscription if that turns out to be possible, otherwise from a starter word list prepared up front (see Open Questions).
- FR-007: Learner can learn grammar. Priority: must-have
  > Socrates: Counter-argument considered: "grammar doesn't work without vocabulary." Resolution: kept as must-have, but grammar unlocks later in the path — after kana and a first pool of vocabulary.
- FR-008: Learner can learn through quotes from anime or manga. Priority: must-have
  > Socrates: Counter-arguments offered ("anime speech is slang", "copyright and content sourcing", "quotes too hard for a beginner"). Resolution: no counter-argument; stands as written.
- FR-009: Learner can see visualizations, e.g. anime dialogues, for learning purposes. Priority: must-have
  > Socrates: Counter-argument considered: "decoration instead of learning." Resolution: kept, but a visualization always carries learning material (e.g. a dialogue gives sentences to translate), never decoration alone.
- FR-010: Learner can review material through repetition and tests that catch their mistakes. Priority: must-have
  > Socrates: Counter-argument considered: "too many reviews pile up after a break." Resolution: kept; already handled by US-01 (after a break of more than 2 days the app proposes repeating the lesson).
- FR-011: Learner, when returning the next day, gets a check of the last lesson and a proposal of what to learn next. Priority: must-have
  > Socrates: Counter-arguments offered ("a check right at launch discourages", "duplicates FR-010"). Resolution: no counter-argument; stands as written.

- FR-023: Learner can learn katakana. Priority: nice-to-have
  > Socrates: Split out of FR-005 during its challenge; counter-argument already considered there.

### Personality & theme
- FR-012: Learner receives a mocking comment in the style of an annoyed Japanese person / anime line when they make a mistake. Priority: must-have
  > Socrates: Counter-arguments offered ("stops being funny after the 50th time", "discourages on a bad day"). Resolution: no counter-argument; stands as written.
- FR-013: Learner can choose the tone of the "teacher's" comments. Priority: nice-to-have
  > Socrates: Counter-argument considered: "one good tone is enough." Resolution: demoted to nice-to-have; the MVP ships one polished "annoyed sensei" tone.
- FR-014: Learner can select which anime the inserts come from. Priority: must-have
  > Socrates: Counter-arguments offered ("each anime is a separate content pool", "little value for a single user"). Resolution: no counter-argument; stands as written.
- FR-015: Learner can see their progress mapped to ranks, and gets promoted as they progress. Priority: must-have
  > Socrates: Counter-argument offered ("rank ≠ real skill"). Resolution: no counter-argument; stands as written.
- FR-016: Learner can choose the rank style. Priority: must-have
  > Socrates: Counter-arguments offered ("one style is enough in MVP", "anime rank names are copyrighted"). Resolution: no counter-argument; stands as written.

### AI
- FR-017: Learner can choose the learning mode — AI-assisted or without AI. Priority: nice-to-have
  > Socrates: Counter-arguments offered ("two modes = two products", "the non-AI mode may be thin"). Resolution: no counter-argument; stands as written.
- FR-018: Learner can connect their own Claude account using a personal API key. Priority: nice-to-have
  > Socrates: Counter-argument considered: "two providers = double integration." Resolution: Claude only in MVP; ChatGPT moved to FR-024 (nice-to-have).
  > Revision 2026-10-12: consumer subscriptions cannot be used by third-party apps, so connection is via a personal API key (pay-per-use); the whole AI mode is deferred out of the MVP by user decision.
- FR-024: Learner can connect their own ChatGPT account using a personal API key. Priority: nice-to-have
  > Socrates: Split out of FR-018 during its challenge; counter-argument already considered there.

### Progress
- FR-019: Learner can view their progress. Priority: must-have
  > Socrates: Counter-argument offered ("duplicates ranks, FR-015"). Resolution: no counter-argument; stands as written.
- FR-020: Learner can export their progress. Priority: must-have
  > Socrates: Counter-argument offered ("nobody remembers to export"). Resolution: no counter-argument; stands as written.
- FR-021: Learner can import their progress. Priority: must-have
  > Socrates: Counter-argument considered: "an import can overwrite current progress with an older file." Resolution: kept; import warns and asks for confirmation before overwriting.
- FR-022: Learner can reset their progress. Priority: must-have
  > Socrates: Counter-argument considered: "conflicts with the guardrail that progress never disappears." Resolution: kept; reset is a deliberate action with confirmation, and the app offers an export first.

## Non-Functional Requirements

- The learner sees a response to any choice in an exercise within 200 ms; any AI-assisted operation that takes longer than 2 s shows continuous visible progress.
- Japanese learning content (kana, vocabulary, grammar) is verified for correctness; any content generated by AI is visibly marked as such.
- The MVP runs on the author's own phone; wider device support comes later.

## Business Logic

The app chooses the next learning material and its difficulty from the learner's interview answers and the mistakes they make, so that the learner practices what they don't yet know.

Inputs: what the learner declared in the interview (what to learn first, what to skip, what to pay attention to), the mistakes they make in exercises and tests, their position in the learning path (hiragana → vocabulary → grammar, with grammar unlocking after kana and a first pool of vocabulary), and how long it has been since the last session.

Output: the next material to study and its difficulty level. Material with mistakes comes back more often; mastered material less often. Difficulty adjusts to recent accuracy — many mistakes make it easier, few make it harder — and advancing to new material and to the next rank requires mastering the current material. After a break of more than 2 days, the app proposes repeating the last lesson.

The learner encounters the rule at the start of each session (a check of the last lesson and a proposal of what to learn next) and throughout a session, as the order and difficulty of exercises adapt. The learner can override the chosen level manually (FR-003).

## Access Control

Single user; no login. One local profile; learning data lives on-device only. No roles. The learner can export their progress to a file and import it back, so progress survives a phone change or reinstall; no automatic sync.

## Non-Goals

- No kanji — the MVP covers hiragana, vocabulary and grammar; learning kanji comes in v2.
- No speaking or listening — no speech recognition, pronunciation scoring or listening exercises.
- No home-grown review algorithm built from scratch — use a proven spaced-repetition approach instead of inventing one.
- No social features — no friends, leaderboards or shared progress.
- No automatic sync — only manual export/import of progress.
- No AI in the MVP — the AI-assisted mode (FR-017, FR-018, FR-024) comes after the MVP; the MVP must stand on its own without AI, per the guardrail.
- No full accessibility (WCAG) guarantee in the MVP.

## Open Questions

1. **Can a third-party mobile app use the learner's existing Claude / ChatGPT consumer subscription?** — The insight relies on paying for AI through a subscription the learner already has. Consumer subscriptions may not be usable from a third-party app at all, in which case AI would need a user-supplied API key (pay-per-use), a local model, or no AI in v1. Owner: user, to verify during tech-stack selection. Block: partial (affects how AI enters the product, not the core learning path).
   **Resolved 2026-10-12:** no for Claude — since 2026, third-party apps cannot route requests through Free/Pro/Max plans and must use API keys. ChatGPT not verified separately. Decision: no AI in the MVP; a personal API key is the path for later.
2. **Source of Polish vocabulary translations** — via the learner's connected AI subscription if Open Question 1 resolves positively; otherwise a starter word list prepared up front. Size and origin of that list TBD. Owner: user.
   **Partly resolved 2026-10-12:** with no AI in the MVP, translations come from a starter word list prepared up front. Size and origin of the list still TBD.
3. **What counts as "mastered"?** — the accuracy threshold that unlocks new material and the next rank (Business Logic). Owner: user; resolve during planning.
4. **Which OS is the author's phone (iOS or Android)?** — the MVP targets only that device (NFR). Owner: user.
