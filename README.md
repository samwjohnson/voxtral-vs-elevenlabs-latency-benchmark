# Voxtral vs ElevenLabs: Voxtral wins in speech-to-text, ElevenLabs wins in text-to-speech but Voxtral fastest overall in streaming across both STT and TTS

Measuring speech-to-text and text-to-speech latency for Voxtral (Mistral) and ElevenLabs under one methodology.

**What we timed**
- STT: time to first text
- TTS: time to first audio

**Setup**
- Sample: ~10 seconds of speech, played at real-time pace
- 10 timed trials after warmup, medians reported
- Two STT modes: streaming in 100 ms chunks, and batch dump with no pacing

**Results (ms, medians)**

| Leg | 10s (n=10) | 5s (n=3) |
|-----|----------:|---------:|
| STT stream 100 ms — Voxtral | **1643** | **1500** |
| STT stream 100 ms — ElevenLabs | **2241** | **2328** |
| STT batch — Voxtral | **1112** | **1051** |
| STT batch — ElevenLabs | **334** | **361** |
| TTS HTTP — Voxtral | **681** | **705** |
| TTS HTTP — ElevenLabs Flash | **234** | **230** |
| TTS WS text-input — ElevenLabs | **251** | **243** |

**Notes**
- Under live pacing, Voxtral STT is faster: 1643 ms vs ElevenLabs 2241 ms. Batch dump favors ElevenLabs only when audio is not streamed in real time (Voxtral 1112 vs ElevenLabs 334).
- TTS: ElevenLabs Flash HTTP 234 ms vs Voxtral 681 ms. ElevenLabs WebSocket text-input 251 ms, within about 20 ms of HTTP Flash.
- Live-agent pick: Voxtral STT + ElevenLabs TTS for one-provider continuity and built-in barge-in. Voxtral offline TTS has no text-chunk input path (full input only; audio may stream out via SSE).
- Caveat: our clocks (time-to-first under live pacing) differ from other published benches (e.g., TTFS = speech-end to final transcript), so do not compare directly.
