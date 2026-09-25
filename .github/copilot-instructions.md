# Copilot instructions for this repository

This repository is a fork of Azure-Samples/call-center-voice-agent-accelerator, adapted to a single purpose: an outbound restaurant-reservation voice agent acting on behalf of Sven Ortmann. Read this file before any task.

## Non-negotiable product rules

1. Disclosure first. The agent's very first sentence must state that it is a digital assistant ("digitaler Assistent"). When the personal voice is active it must additionally say it speaks with a replicated voice ("nachgebildete Stimme"). Never remove, shorten or make this optional. Any prompt change must keep a test that asserts the disclosure in the first sentence for both voice variants.
2. Never impersonate a human. If asked whether a machine is speaking, the agent says yes. Do not add code paths, prompts or options that hide the synthetic nature of the call.
3. No commitments beyond the order. The agent must not confirm menus, deposits, allergies or special requests. Such items go to offene_punkte.
4. Personal data stays local. Transcripts and results contain names and phone numbers: write them only to server/transcripts/ and server/results/, both gitignored. Never log secrets or the full .env.

## Scope

- In scope: server/ application code, prompts under server/prompts/, server/reservation.json, tests, docs/, README-reservierung.md, scripts/.
- Out of scope unless an issue explicitly says otherwise: telephony provider handlers (ACS, Twilio, Infobip, Sinch, Genesys, Bandwidth), infra/, azure.yaml, GitHub Actions deployment workflows, the ambient audio mixer.

## Engineering conventions

- Python, dependency management with uv and pyproject.toml. Run the app with uv run server.py, tests with uv run pytest.
- Configuration comes from environment variables (server/.env, template server/.env.sample) and server/reservation.json. No hard-coded prompts, endpoints, keys or phone numbers.
- Prompt files are plain text with {{PLACEHOLDER}} tokens. Rendering must fail fast if any placeholder is left unresolved.
- Voice Live session configuration follows Microsoft Learn (How to use the Voice Live API, How to customize Voice Live input and output). Voice block by VOICE_TYPE:
  - azure-standard: {"type": "azure-standard", "name": VOICE_NAME}
  - azure-personal: {"type": "azure-personal", "model": VOICE_MODEL, "name": VOICE_NAME, "temperature": 0.8} with VOICE_MODEL in {DragonLatestNeural, DragonHDOmniLatestNeural}
- German for user-facing text (README, operator error messages, prompts). English for identifiers, code comments and commit messages.
- Small, reviewable pull requests. Every behaviour change ships with a test. Do not reformat unrelated files.
- Fail fast with actionable messages: name the missing field or variable and where to set it.

## Testing expectations

- Unit tests for prompt rendering, configuration validation and report_result serialisation.
- docs/testkatalog.md lists the ten conversational scenarios; when you change prompt logic, state in the PR which scenarios you re-checked manually or why not.
