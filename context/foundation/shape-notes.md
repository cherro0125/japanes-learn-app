---
project: "japanese-learn-app"  # working name
context_type: greenfield
created: 2026-10-05
updated: 2026-10-10
timeline_budget:
  mvp_weeks: 8
  hard_deadline: null
  after_hours_only: true   # side project: after hours or free moments during the workday
product_type: mobile
target_scale:
  users: small
  qps: low
  data_volume: small
checkpoint:
  current_phase: 8
  phases_completed: [1, 2, 3, 4, 5, 6, 7]
  gray_areas_resolved:
    - topic: "context type"
      decision: "greenfield — repo holds only workflow tooling, no app code"
    - topic: "target language"
      decision: "Japanese (an earlier mention of English was a typo)"
    - topic: "pain category"
      decision: "no structure/continuity, decision paralysis, poor UX/visuals, no progress tracking"
    - topic: "primary persona scope"
      decision: "MVP serves only the author"
    - topic: "access model"
      decision: "local profile, no login; manual export/import of progress"
    - topic: "MVP scope"
      decision: "full scope kept; 8-week timeline accepted; hiragana in MVP, katakana nice-to-have (revised in Socrates round)"
    - topic: "AI provider in MVP"
      decision: "Claude only; ChatGPT nice-to-have"
    - topic: "teacher tone"
      decision: "one polished 'annoyed sensei' tone in MVP; tone choice nice-to-have"
    - topic: "domain rule"
      decision: "next material + difficulty chosen from interview answers and mistakes; respects path order and >2-day break; mastery gates advancement"
  frs_drafted: 24
  quality_check_status: accepted
---

# Shape notes

## Seed idea (verbatim)

> aplikacja do nauki języka japońskiego dla osoby początkującej. Ma sama dobierać poziom trudności i proponować czego teraz się uczyć. Na początku raczej skupienie na nauce gramatyki i słownictwie. Aplikacja na ios i androida w jednym. Możliwe z użyciem Ai. Ale raczej w kierunku możliwości podpięcia swojego konta claude bądź chat gpt niż rozliczaniu na zasadzie api.

## Vision & Problem Statement

A beginner self-learner of Japanese who studies by chatting with a general-purpose AI assistant drops the habit after a few days. The chat turns into a mess: they don't know where they stopped, the conversation context runs out, and it is hard to find their way around. Every session starts from zero, they must decide on their own what to learn and how to ask for it, the material arrives as a wall of text with no exercises or visual presentation, and nothing tracks progress or brings material back for review. What they want instead is a systematic, orderly path with continuity and repetition.

Insight (user's hypothesis, not yet verified against existing products): existing dedicated apps teach through English and are paid; their courses follow rigid paths that do not adapt to the learner; and grammar, vocabulary and explanations live in separate tools. Learning through Polish would make progress faster, the learner wants to shape the app's style to their own taste, and AI can assemble an adaptive path in one place while being paid for through AI subscriptions the learner already has.

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

## MVP flow

First session:
1. The app interviews the learner: what they want to learn first, what to skip, what to pay attention to.
2. A short placement test across categories (e.g. word translation in both directions PL↔JP, sentence translation).
3. The app evaluates the learner's level.
4. Learning — e.g. a set of vocabulary.
5. Repetition and tests that catch mistakes.
6. On a mistake, the app responds mockingly, like an annoyed Japanese person — e.g. a funny line from anime.
7. Level and progress within the session are mapped to ranks inspired by anime (e.g. ninja ranks from Naruto, shinigami ranks from Bleach).

Next day:
8. The app checks knowledge from the last lesson.
9. The app proposes what to learn next.

Throughout: the product is strongly themed around Japanese anime, manga and mythology — e.g. learning through quotes from anime or manga.

Learner starts from zero (no kana), so hiragana is part of the MVP; katakana was moved to nice-to-have during the Socrates round. Grammar and vocabulary are the initial focus after kana.

## Timeline budget

- mvp_weeks: 8

## Timeline acknowledgment

Acknowledged on 2026-10-05: 8-week MVP requires sustained dedication; user accepted. Scope-cost surfaced: AI-driven interview/adaptation (depends on Open Question 1), anime/manga quote content sourcing and Polish translation, grammar + vocabulary + kana together, multi-category placement test. Also noted: quotes and rank names from existing anime are copyrighted — fine for personal use, a concern if the app is ever published.

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
- FR-017: Learner can choose the learning mode — AI-assisted or without AI. Priority: must-have
  > Socrates: Counter-arguments offered ("two modes = two products", "the non-AI mode may be thin"). Resolution: no counter-argument; stands as written.
- FR-018: Learner can connect their own Claude subscription. Priority: must-have
  > Socrates: Counter-argument considered: "two providers = double integration." Resolution: Claude only in MVP; ChatGPT moved to FR-024 (nice-to-have).
- FR-024: Learner can connect their own ChatGPT subscription. Priority: nice-to-have
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
- No full accessibility (WCAG) guarantee in the MVP.

## Open Questions

1. **Can a third-party mobile app use the learner's existing Claude / ChatGPT consumer subscription?** — The insight relies on paying for AI through a subscription the learner already has. Consumer subscriptions may not be usable from a third-party app at all, in which case AI would need a user-supplied API key (pay-per-use), a local model, or no AI in v1. Owner: user, to verify during tech-stack selection. Block: partial (affects how AI enters the product, not the core learning path).
2. **Source of Polish vocabulary translations** — via the learner's connected AI subscription if Open Question 1 resolves positively; otherwise a starter word list prepared up front. Size and origin of that list TBD. Owner: user.
3. **What counts as "mastered"?** — the accuracy threshold that unlocks new material and the next rank (Business Logic). Owner: user; resolve during planning.
4. **Which OS is the author's phone (iOS or Android)?** — the MVP targets only that device (NFR). Owner: user.

## Forward: tech-stack

Informational — not part of the PRD schema. Volunteered in the seed idea:

- One codebase targeting both iOS and Android.
- AI is optional ("możliwe"); preference is for the user to connect their own Claude account (ChatGPT later) rather than the product paying per-call API billing.
