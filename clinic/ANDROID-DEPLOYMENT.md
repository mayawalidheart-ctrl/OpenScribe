# Android access plan: Shazly Clinic OpenScribe

**Status:** deployment instructions only; no hosted site is live and GPT note generation has not been implemented. Do NOT use with patient-identifiable data until security and clinical validation are complete.

## Recommended topology

Android Chrome (HTTPS + microphone permission) -> authenticated hosted Next.js app -> OpenAI transcription API -> GPT note-generation provider -> clinician-review screen -> approved report export.

**Important**: The existing app defaults to Anthropic Claude for note generation. Configuring OPENAI_API_KEY currently affects transcription only. Implement an OpenAI note provider before stating that the system is GPT-powered end to end.

## Android deployment prerequisites
1. Create a Vercel account and connect GitHub repository `mayawalidheart-ctrl/OpenScribe`, preferably a private repository for deployment work.
2. Select the `feature/shazly-clinic-ai` branch for initial preview testing. This is not a clinically secure rollout.
3. Confirm Next.js build/runtime compatibility and microphone capture in Android Chrome. Vercel deployment is not yet verified for this repo.
4. On Vercel configure server-side secrets, e.g. `OPENAI_API_KEY`. Never prefix secrets with NEXT_PUBLIC_.
5. Use cloud transcription; local `whisper_local` expects a local whisper server and will not work as-is from a serverless Vercel deployment.
6. Current transcription provider is `whisper_openai` using `whisper-1`. The OpenAI docs announce retirement of older speech-to-text models; migrate the adapter to current `gpt-transcribe` after verifying API compatibility.
7. Add authentication, authorization, session expiry, secure backend storage, clinician approval, audit trail, deletion/retention policy, security review, backups, and compliance checks BEFORE admitting any identifiable patient data.
8. Then expose the HTTPS site on Android Chrome and use Chrome menu > Add to Home screen or Install app, if supported.

## Functional scope for first pilot
- Dictate fictional clinical findings in Arabic.
- Generate an Arabic structured draft using `clinic/templates/arabic-colorectal-report.md`.
- Display unsupported/missing inputs clearly; never fabricate clinical findings.
- Require explicit doctor approval before exporting.
- Keep WhatsApp booking/triage on a separate integration, preserving existing Google Sheets and Apps Script rules.

## Phased checklist
- [x] Source fork identified in connected GitHub.
- [x] Arabic clinical report template committed to feature branch.
- [x] Android rollout plan documented.
- [ ] OpenAI GPT note generation provider implemented.
- [ ] Speech model adapter migrated/tested.
- [ ] Authentication and server privacy/security implemented.
- [ ] Verified mobile microphone capture, Arabic output and exports.
- [ ] Hosted HTTPS site deployed.
- [ ] Compliance/security approval and clinician acceptance tests.
