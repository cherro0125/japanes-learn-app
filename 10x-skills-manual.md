# 10x skills – manual: flow tworzenia aplikacji krok po kroku

Instrukcja oparta na zawartości `.claude/skills/10x-*` (pakiet `@przeprogramowani/10x-cli` 1.6.1, kurs 10xdevs3). Każdy skill wywołujesz w Claude Code jako slash-komendę, np. `/10x-shape`.

## 1. Idea ogólna

Skille tworzą łańcuch, w którym **każdy krok zapisuje plik w `context/`, a następny krok go czyta**. Dzięki temu kontekst nie żyje w głowie rozmowy, tylko w repo.

```
context/
├── foundation/   # dokumenty "stałe" projektu: prd.md, tech-stack.md, roadmap.md, infrastructure.md, lessons.md ...
├── changes/      # aktywne zmiany: <change-id>/{change.md, research.md, frame.md, plan.md, plan-brief.md, reviews/}
└── archive/      # zakończone zmiany (po /10x-archive)
```

Flow dzieli się na trzy fazy:

1. **Fundament** (raz na projekt): pomysł → PRD → stack → scaffold → roadmapa.
2. **Pętla zmiany** (powtarzana dla każdego slice'a z roadmapy): new → research/frame → plan → review → implement → review → archive.
3. **Higiena**: AGENTS.md, lessons, przegląd reguł, CI review.

---

## 2. Faza 1 – Fundament (greenfield)

| # | Skill | Co robi | Wejście → wyjście |
|---|-------|---------|-------------------|
| 0 | `/10x-init` | Tworzy szkielet `context/{changes,archive,foundation}/` i README. Idempotentny (tylko brakujące). | – → `context/` |
| 1 | `/10x-mom-test` *(opcjonalnie)* | Waliduje, czy problem jest realny (zasady The Mom Test): krytyka, scenariusz wywiadu, ankieta, kryteria go/no-go. | notatki/pomysł → raport walidacji |
| 2 | `/10x-opportunity-map` *(opcjonalnie)* | Klasyfikuje pomysł: build / buy / complement / wait, wskazuje jednego kandydata. | pomysł → opportunity map |
| 3 | `/10x-shape [pomysł]` | Rozmowa discovery. Sam wykrywa greenfield/brownfield z cwd. | pomysł → `shape-notes.md` |
| 4 | `/10x-prd [shape-notes]` | Generuje PRD zgodne ze schematem (greenfield: 10 sekcji, brownfield: 11). | `shape-notes.md` → `context/foundation/prd.md` |
| 5 | `/10x-tech-stack-selector [prd]` | Dobiera starter i stack wg 4 bramek "agent-friendly" (typowany, oparty na konwencjach, popularny, dobrze udokumentowany). | `prd.md` → `context/foundation/tech-stack.md` |
| 6 | `/10x-bootstrapper [tech-stack]` | Uruchamia CLI wybranego startera w bieżącym katalogu. Zawsze zachowuje `context/`. Zapisuje log weryfikacji. | `tech-stack.md` → scaffold aplikacji |
| 7 | `/10x-infra-research [tech-stack/prd]` | Wywiad + równoległy research webowy: wybór platformy deploymentu, scoring i rejestr ryzyk. | → `context/foundation/infrastructure.md` |
| 8 | `/10x-roadmap [prd]` | Zamienia PRD w uporządkowaną listę pionowych slice'ów (end-to-end, widocznych dla użytkownika). | `prd.md` → `context/foundation/roadmap.md` |
| 9 | `/10x-agents-md` | Generuje `AGENTS.md` ("Repository Guidelines") – krótki onboarding dla agentów. | repo → `AGENTS.md` |
| 10 | `/10x-rule-review` | Ocenia plik z regułami AI (CLAUDE.md, AGENTS.md, .cursor/rules…) w skali 5 punktów z konkretnymi poprawkami. | plik reguł → scorecard |

Kolejność z opisów skilli: `prd` → `tech-stack-selector` → `bootstrapper`; `infra-research` po `prd` lub `tech-stack-selector`, **przed** `10x-implement`; `roadmap` po `prd` (i po wyborze stacku/bootstrapie, jeśli dotyczy).

### Wariant brownfield (istniejący projekt)

Zamiast selektora stacku i bootstrappera:

1. `/10x-init`
2. `/10x-shape` (wykryje brownfield) → `/10x-prd` (szablon 11 sekcji)
3. `/10x-stack-assess [prd]` → `context/foundation/stack-assessment.md` (oceny komponentów, luki, gotowe wpisy do CLAUDE.md/AGENTS.md)
4. `/10x-health-check [stack-assessment]` → `context/foundation/health-check.md` (audyt zależności, security, testy, CI/CD, werdykt "agent-readiness")
5. `/10x-agents-md`, `/10x-rule-review`
6. `/10x-roadmap`
7. `/10x-test-plan` *(tylko brownfield)* → `context/foundation/test-plan.md`. Stanowy orkiestrator etapowego rolloutu testów; każdą fazę przepuszcza przez `new → research → plan → implement`. Ponowne uruchomienie wznawia od następnej oczekującej fazy (`--status`, `--refresh`).

---

## 3. Faza 2 – Pętla zmiany (jeden slice z roadmapy)

Każda zmiana ma własny folder `context/changes/<change-id>/` i plik `change.md` ze statusem.

```
/10x-new → [/10x-research] [/10x-frame] → /10x-plan → [/10x-plan-review]
        → /10x-implement | /10x-tdd | /10x-e2e | /10x-goal-implement
        → [/10x-impl-review] → /10x-archive
```

### Krok po kroku

1. **`/10x-new <change-id> [intent]`**: tworzy folder zmiany i `change.md` (status `new`). `change-id` w kebab-case, np. `oauth-login`.
2. **`/10x-research`** *(opcjonalnie)*: bada codebase równoległymi sub-agentami, zapisuje `research.md` (status `preparing`).
3. **`/10x-frame`** *(opcjonalnie)*: używaj, gdy dostajesz "bug + proponowana poprawka" albo pytanie o zakres. Kwestionuje założenia (CO budować) **przed** planowaniem (JAK). Zapisuje `frame.md`.
4. **`/10x-plan`**: tworzy szczegółowy plan z fazami i kryteriami sukcesu → `plan.md` (+ `plan-brief.md`), status `planned`. Jeśli jest `research.md`, zadaje mniej pytań. Na dole planu jest sekcja `## Progress` (patrz niżej).
5. **`/10x-plan-review`** *(zalecane)*: ocena planu pod kątem treści, wykonalności i dopasowania architektury (status `plan_reviewed`).
6. **Implementacja** – wybierz jeden tryb:
   - `/10x-implement`: ręcznie, fazami, z weryfikacją po każdej.
   - `/10x-tdd`: test-first (red → green → refactor), tylko dla faz "TDD-owalnych"; resztę przekazuje do `/10x-implement`.
   - `/10x-e2e`: fazy przeglądarkowe (Playwright MCP): plan → generate → review → verify, po jednym ryzyku. Tylko gdy funkcja jest już zbudowana.
   - `/10x-goal-implement`: autonomicznie, bez interakcji, pod `/goal` lub headless (`claude -p`). Commituje każdą fazę na zielono (Conventional Commits), a ręczne punkty zostawia jako checklistę.
7. **`/10x-impl-review <plan-path> [phase N]`**: sprawdza implementację względem planu (drift, ryzykowne decyzje, zgodność ze wzorcami). Status `impl_reviewed`.
8. **`/10x-archive <change-id>`**: przenosi folder do `context/archive/` i oznacza `change.md` jako `archived`.

### Statusy w `change.md`

`new → preparing → planned → plan_reviewed → implementing → implemented → impl_reviewed → archived` (plus `blocked`, ustawiany ręcznie). Kroki opcjonalne można pominąć (np. `new → planned`, `planned → implementing`).

### Sekcja `## Progress` w `plan.md`

Jedyne źródło prawdy o stanie wykonania. Ma fazy z podziałem na `#### Automated` (komendy, testy, typy) i `#### Manual` (do obejrzenia przez człowieka). Zasady: `- [ ]` oczekuje, `- [x]` zrobione, po wykonaniu dopisz ` — <sha commita>`, nie zmieniaj nazw kroków. Szczegóły: `.claude/skills/10x-plan/references/progress-format.md`.

---

## 4. Faza 3 – Higiena i automatyzacja

- **`/10x-lesson`**: zapisuje powtarzalną regułę lub pułapkę do `context/foundation/lessons.md`, żeby przyszłe review i implementacje ją uwzględniały. Używaj po znalezieniu klasy błędu.
- **`/10x-impl-review-ci`**: wersja nieinteraktywna dla CI (GitHub Actions / Claude Code Action). Sama znajduje plan i PR, zapisuje `reviews/impl-review.md`, commituje go do gałęzi PR i dodaje komentarz. Szablon workflow: `.claude/skills/10x-impl-review-ci/references/workflow-template.yml`.
- **`/10x-agents-md`** i **`/10x-rule-review`**: odświeżaj, gdy repo mocno się zmieni.

---

## 5. Ściąga: pełny przebieg nowej aplikacji

```
/10x-init
/10x-mom-test            (opcjonalnie)
/10x-opportunity-map     (opcjonalnie)
/10x-shape "mój pomysł"
/10x-prd
/10x-tech-stack-selector
/10x-bootstrapper
/10x-infra-research
/10x-roadmap
/10x-agents-md
/10x-rule-review
-- dla każdego slice'a z roadmapy --
/10x-new <change-id> "co chcę zrobić"
/10x-research            (opcjonalnie)
/10x-plan
/10x-plan-review
/10x-implement           (albo /10x-tdd, /10x-e2e, /10x-goal-implement)
/10x-impl-review
/10x-lesson              (gdy wyszła powtarzalna pułapka)
/10x-archive <change-id>
```

---

## 6. Prompty pomocnicze (`.claude/prompts/`)

- `skill-explainer.md`: prompt, który czyta wskazany skill (SKILL.md + `references/`) i tłumaczy jego mechanikę oraz sposób zbudowania podobnego. Użycie: podaj nazwę, np. `10x-plan`.
- `m1l5-1…4-*.md`: sekwencja z lekcji o planowaniu wdrożenia (M1L5). Plan integracji i deploymentu na podstawie `context/foundation/infrastructure.md` → ograniczenie, że auto-deploy z `main` robi platforma, a nie zewnętrzny CI/CD → dopisanie wymagań wstępnych (konfiguracja CLI) → zapis planu w `context/changes/deployment/deployment-plan.md`. Zmienna `{{platform_name}}` podstawiasz sam.

## 7. Uwagi

- Plik `.claude/.10x-cli-manifest.json` to ślad instalacji przez `10x-cli` (wersja, lekcja `m2l2`, lista plików). Nie edytuj go ręcznie.
- Opis powstał z nagłówków (frontmatter) skilli, referencji `progress-format.md` i `change-md.md` oraz promptów. Treści wszystkich `SKILL.md` nie czytałem w całości, więc szczegóły zachowania poszczególnych kroków (pytania, formaty plików) sprawdzaj w danym `SKILL.md`.
- `progress-format.md` wspomina o skillu `/10x-status`, którego **nie ma** w tym zestawie plików.
