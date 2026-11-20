# Shazly Clinic AI: first-phase setup

This is a development branch, not a deployed or clinically validated service.

## Scope
1. Arabic colorectal/proctology medical report drafts from clinician dictation or typed notes.
2. Clinician review and explicit approval before exporting or sharing.
3. Later: separate WhatsApp reception workflow without changing existing Google Sheets/Apps Script appointment rules.

## Existing repository findings
- Default mixed-mode note generation uses Anthropic; OPENAI_API_KEY alone does NOT switch note generation to GPT.
- OpenAI Whisper API can be selected with TRANSCRIPTION_PROVIDER=whisper_openai.
- The default local tiny.en Whisper model is English-only; choose a multilingual model for Arabic.
- The application already supports Custom Note Template in Settings.

## First local test (synthetic patient data only)
1. Install Node.js and pnpm according to the upstream README.
2. Clone this repository and run `pnpm install`.
3. Run `pnpm run setup`.
4. In the local (git-ignored) `apps/web/.env.local`, configure ONE audio backend:
   - Local Arabic: `TRANSCRIPTION_PROVIDER=whisper_local`, `WHISPER_LOCAL_MODEL=base`, `WHISPER_LANGUAGE=ar`. Note: upstream startup scripts may hardcode tiny.en and require modification.
   - OpenAI audio: `TRANSCRIPTION_PROVIDER=whisper_openai`, `WHISPER_LANGUAGE=ar`, and `OPENAI_API_KEY=<your local secret>`.
5. Until GPT note-generation integration is implemented, the web app still needs an Anthropic key for note generation.
6. In Settings > Note Template > Custom, paste `clinic/templates/arabic-colorectal-report.md` as the note structure.
7. Run the app following upstream instructions, test with fictional clinical dictations, and check every field.

## Planned GPT integration (not yet implemented)
- Add an OpenAI-compatible note-generation provider on the server side using the OpenAI API; keep existing Claude as an optional provider.
- Introduce `NOTE_GENERATION_PROVIDER=openai` and a separately configurable OpenAI model after validating supported model IDs and endpoints.
- Keep API keys server-side; never expose them in public repositories, client environment variables, transcripts, logs, or chat.
- Add tests for Arabic RTL output, missing data, clinically unsupported assertions, and clinician approval.
- Ensure secure handling of protected health data, data retention and patient consent/legal compliance before any real patient rollout.

## Report-writing rules
- Draft in professional Arabic, preserve English technical terms only where clinically useful.
- Use only supplied facts. Never invent examination findings, investigations, age, treatment, outcomes, or claims about medical error.
- If a field is unknown, omit it or explicitly say "غير مذكور".
- Do not make a diagnosis or treatment decision independently. Route for clinician review.
- No automatic sending to patients.
