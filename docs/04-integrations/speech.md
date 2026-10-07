# Speech (Voice Commands)

## Speech-to-text
Adapter `speech/stt` with a provider chosen for pt-BR accuracy and latency (evaluate 2–3 providers on recorded operator commands containing client names, campaign names and amounts). Custom vocabulary/hints from client and campaign names improve recognition.

## Text-to-speech (optional)
For the spoken daily digest and voice read-back of plans. Adapter `speech/tts`.

## Data handling
Audio stored only if retention is enabled (default: keep transcript, delete audio after N days). Transcripts are linked to `commands` for audit.

## Phase 1 alternative
In M1 the operator uses the Claude app's voice mode connected to the Growth OS MCP server, which needs no speech infrastructure in Growth OS.
