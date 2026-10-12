---
starter_id: expo
package_manager: npm
project_name: japanese-learn-app
hints:
  language_family: js
  team_size: solo
  deployment_target: expo-go
  ci_provider: github-actions
  ci_default_flow: manual-promotion
  bootstrapper_confidence: verified
  path_taken: standard
  quality_override: false
  self_check_answers: null
  has_auth: false
  has_payments: false
  has_realtime: false
  has_ai: false
  has_background_jobs: false
---

## Why this stack

A solo developer building an 8-week, after-hours MVP of a Japanese-learning app for iOS and Android from one codebase, in TypeScript. Expo (React Native) is the recommended default for mobile in the JS family, clears all four agent-friendly gates, and its scaffolding is verified end-to-end. The app runs through Expo Go on the author's own phone, so no store accounts or paid builds are needed for the MVP. There is no auth (local profile only), no payments and no realtime. AI was dropped from the MVP after research showed that, as of 2026, third-party apps cannot use a consumer Claude subscription; AI would require a user-supplied API key and is deferred to v2. CI runs on GitHub Actions with manual promotion, which keeps the limited free build quota for deliberate releases.
