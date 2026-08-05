# CTO Slide-by-Slide Prep — LEAP4 Call

**For:** Kavipriyan (CTO). Per-slide: what an investor asks off that slide, and your
follow-up answer. Grounded in verified code + ADRs (`docs/pitch/tool-map-and-lifecycle.md`,
`docs/pitch/2026-08-04-cto-technical-prep.md`).

**Who probes what:**
- **Fares Agua** (Principal, TUM/CDTM) → architecture, latency, LangGraph, voice pipeline. Your main technical sparring partner.
- **Matthias Kempf** (GP, ex-MOIA/VW) → is the mobility/orchestration claim technically real; rail/OEM credibility.
- **Daniel Willenbrink** (IM) → unit economics, cost-per-session, burn.
- **Jan / Tobias / Theresia** → GTM, equity story, team — not your lane, defer to Harish.

**Two golden rules:**
1. Separate **live today** from **roadmap** in every technical answer. The deck now labels the Demo as "vision" — hold that line verbally everywhere.
2. Never overclaim. "Ready to switch on" ≠ "running." A technical VC will test the seam.

---

## ⚠️ Deliberate bait-gaps you left (so he must ask) — be READY, not surprised

You left these incomplete on purpose so the investor engages. Make sure the answer is sharper than the slide:

| Where | The gap you left | The answer you must nail |
|---|---|---|
| Slide 2 / Demo | "sub-500ms voice pipeline" + "real-time transit graph" stated, not shown | Voice latency is real & measurable; transit graph is **roadmap** — see Q&A below. Do NOT let him think the transit graph is live. |
| Demo slide | "Vision preview — not yet live" label | Invites "so what IS live?" → pivot to the real Deliver screens (itinerary/audio/PDF). Have that transition rehearsed. |
| Roadmap slide | 4 things "switch on next" (vector, eval, booking, self-host) with no dates/triggers | He WILL ask "when / what unblocks each." Triggers + sequence below. |
| Self-host card | "Own the voice + inference layer as volume grows" | The self-hosting Q — full answer in §Roadmap. This is the one you flagged you need. |
| Business slide | Phase 2 take-rate "Amadeus or Duffel" | "which, when, is it integrated?" → not yet; post-launch; keys-not-wired. |

---

## Slide-by-slide

### S1 Cover — "real-time orchestration layer for disrupted mobility"
**He asks:** "Is the disruption product live, or is it planning today?"
**You:** "Today the live product plans complete trips end-to-end — voice or chat. The real-time
disruption re-routing is the vision this raise funds; the orchestration engine underneath it is
already built. I'll show you what's live in a minute." *(Sets the honest frame up front.)*

### S2 Team — "built the sub-500ms voice pipeline and proprietary real-time transit graph"
**Fares asks:** "Walk me through the sub-500ms pipeline. And what's the transit graph?"
**You (voice — real):** "The voice pipeline is LiveKit for WebRTC transport plus Deepgram —
Nova-3 STT in, Aura-2 TTS out — wrapping our LangGraph agent. The sub-500ms figure is the
voice round-trip feel: STT partial + first TTS token; the full plan takes longer because it
fans out to travel APIs."
**You (transit graph — honest):** "The 'real-time transit graph' is roadmap language — today
we orchestrate live travel APIs into one itinerary. A dedicated transit graph for live rail
re-routing is what we build next; I won't claim it's running."
> ⚠️ Biggest landmine in the deck. If you claim the transit graph is live and Kempf (ex-MOIA)
> probes rail data feeds, you're exposed. Call it roadmap.

### S3 Problem / S4 Solution — disruption re-routing
**Kempf asks:** "Live rail disruption data across EU operators is brutal — GTFS-RT coverage is
patchy, DB/SNCF/Trenitalia all differ. How do you get it?"
**You:** "Exactly why it's the roadmap, not the launch product. Launch = trip planning on APIs
we already integrate. Disruption re-routing needs the real-time feeds you're describing —
that's a data-partnership and integration effort the raise funds, and it's where your LP network
(DB-adjacent operators) is uniquely valuable to us." *(Turns the hard question into an ask.)*

### S5 Why Now — "model is commodity, edge is orchestration"
**Fares asks:** "If the model's a commodity, isn't the orchestration just prompt-chaining?"
**You:** "No — it's a stateful graph. LangGraph with Postgres checkpointing: the plan is a
multi-step agent that fans out to ~8 workflow nodes in parallel, merges, and can pause/resume a
conversation. Prompt-chaining is stateless and linear; this is stateful and parallel, which is
what makes it reliable enough to book a real trip."

### S6 Intake — 8 signals, one canonical payload, either channel
**Fares asks:** "Same schema for voice and chat? How do you keep them in sync?"
**You:** "One canonical intake payload — Yatra (voice) and Orion (chat) are two front doors to
the identical schema and the identical LangGraph engine. No duplicated planning logic; only the
I/O channel differs. That's a deliberate architecture choice so quality improvements land on
both at once."

### S6b System — LangGraph + Postgres checkpoint  ⭐ YOUR STRONGEST SLIDE
**Fares asks:** "Why checkpoint in Postgres? Why not just hold state in memory?"
**You:** "Because a planning conversation is long-lived and serverless is stateless. Postgres
checkpointing lets a session pause and resume — a user can drop off and come back, and the agent
resumes mid-graph. It's also how we get durability and observability per step. Dwell here — this
is the real engineering."

### S7 Demo — "resolve a disruption in sub-500ms" (labeled vision)
**He asks:** "So this specific flow isn't live — what IS?"
**You:** "Correct, this is the target. What's live today is the full planning loop — let me show
you." → **cut to the Deliver screens (S8+).** Rehearse this transition; it's the pivot from
vision to proof.

### S8 Workflows — 14 parallel workflows
**Fares asks:** "Are all 14 actually wired?"
**You (honest):** "The planning workflows are live — weather, events, transit, currency,
activities, packing, outdoors, Viator — about eight nodes running in parallel in the graph today.
The disruption-specific ones (rail rebooking, live alerts) are roadmap. The grid shows the full
vision; I'll be precise about which are running now."
> ⚠️ Don't claim Rail rebooking / Air tracking / Alerts are live. Map the 14 to the real 8 +
> roadmap. (See tool-map: the real nodes are events·transit·currency·outdoors·packing·trips·Viator·weather.)

### S9–S11 Deliver (Itinerary / Audio Studio / PDF) — THE REAL PROOF
**He asks:** "Is this real output or mockup?"
**You (Itinerary):** "Real. Viator activities, Google Places cafés, Open-Meteo weather —
rendered as live interactive cards through Tambo, our generative-UI layer. Tap Going/Skip and the
plan re-composes."
**Audio Studio:** "Every plan becomes a narrated audio briefing — three modes shipped, an in-trip
live companion on the roadmap." *(Don't claim the in-trip companion is live — it's tagged 'In-trip'.)*
**PDF:** "This is a real generated PDF, not a comp — those are actual exported pages."
> These slides are your credibility anchor. Spend time here, not on the disruption vision.

### S12 4D Smart Map — labeled "Coming next · the moat"
**He asks:** "What makes this a moat vs. just Google Maps with pins?"
**You:** "Google Maps routes A→B. This routes a *whole day* — stay, transport, stops, food, timed
and sequenced — as one optimized plan, and re-optimizes when you change a pick. It's roadmap; the
planning data that feeds it is already live."

### S13 Roadmap — vector memory / eval loop / booking / self-host  ⚠️ (your flagged one)
**He asks (each card):** "When does each switch on, and what unblocks it?"

**Vector memory (Chroma):** "Provisioned — credits and keys in place, not yet imported into the
product path. It's cross-trip personalization: Yatra remembers you between trips. Switches on
post-launch; it's additive, not a dependency for the core loop."

**Eval loop (LangWatch):** "Simulation + scoring of the voice agent against real scenarios —
eval, not observability (Langfuse is our tracer). Cloud free tier covers our volume. Near-term:
scenario tests in CI as a regression gate. It's how the agent gets measurably better at *this*
task — part of the moat."

**Booking (Amadeus/Duffel):** "Post-launch. Turns planning into revenue via take-rate. Keys
staged, not wired — we're finishing the planning loop first, then make it transactional."

**Self-hosted stack (THE self-hosting answer):**
> "We deliberately run managed today — Cloudflare Workers for web, LiveKit Cloud for voice — and
> self-host nothing pre-launch. We evaluated it and chose not to: self-hosting for near-zero
> traffic burns engineering and adds ops load for no gain. Our rule is **build on a trigger, not
> because we hold the credits.** The triggers are concrete: self-host the **voice server** when
> the concurrent-agent cap blocks real sessions or Cloud minute-cost gets material at volume — and
> the LiveKit API is identical Cloud vs self-hosted, so that migration is a **config swap, not a
> rewrite**. Self-host **GPU inference** (we hold a Lambda credit) only when a fine-tune/inference
> workload beats API cost at our volume. So self-hosting is a cost-and-control lever we pull at
> scale, not a pre-launch project."

**If pushed "why not self-host now to save on credits?":** "Credits mean infra is effectively
free through launch — self-hosting now would *cost* us engineering time to save money we're not
spending. Wrong trade pre-launch."

### S14 Moat — "patent-pending conflict resolution"  ⚠️
**He asks:** "You have a patent filed? On what exactly?"
**You:** "Let me be precise — 'patent-pending' overstates it; we haven't filed. The real,
defensible moat is three things: **stateful orchestration** (checkpointed multi-step agent),
**integration depth** (a dozen travel APIs stitched into one canonical flow), and the **eval
loop** that makes the agent better at this task over time. The model is swappable commodity."
> ⚠️ Do NOT claim a patent to a partner who's filed real ones. Drop the phrase; own the real moat.
> Consider editing this off the slide before the call.

### S15 LEAP4 Alignment — "vector routing engine using Chroma", "sub-500ms edge", "Claude (Bedrock)"
**Fares asks:** "Vector routing engine — is that live? And is your LLM Claude or Azure OpenAI?"
**You (Chroma):** "Vector layer is provisioned, switching on post-launch — 'engine' is
forward-looking on that row." **(LLM):** "Reasoning runs on Azure OpenAI GPT-4o-mini today; the
layer is provider-swappable, so Claude/Bedrock is available to us — the row names the option, not
a hard dependency."
> ⚠️ Deck says Claude(Bedrock) here but Azure OpenAI is what's wired. Reconcile verbally or fix
> the slide — a technical VC will catch the mismatch if you say Azure in the demo and Claude here.

### S16 Business — Phase 2 take-rate Amadeus/Duffel
**Daniel asks:** "Is booking integrated? What's the take-rate assumption based on?"
**You / Harish:** "Booking is post-launch — keys staged, not wired. Take-rate of 2-5% is the
standard OTA affiliate/GDS range; we'll validate exact rates when Amadeus/Duffel ships ~6 months
post-launch." *(Mostly Harish's lane; you confirm the integration status honestly.)*

### S17 Market / S18 Traction — "£1.5M+ credits", "real-time transit graph" (traction card)
**Daniel asks:** "£1.5M credits — is that cash?"
**You:** "No — infrastructure credits, ~$189K across Google, AWS, Azure, Cloudflare, NVIDIA,
Chroma, etc. It means our compute is covered well past launch before we spend a dollar. Precisely:
credits, not raised cash."
> ⚠️ Traction card still says "Real-time transit graph" under Sept MVP. Same landmine as S2 —
> that's roadmap, not the MVP. Consider editing to "real-time trip planning."
> Also credits number is inconsistent: deck says £1.5M+, your brief says ~$189K. Pick ONE and
> use it everywhere. (£1.5M+ likely counts partner/marketplace value; $189K is cloud/AI compute.)

### S19 Ask — £1M @ £8M
Harish's lane. Your only job if asked "what does 60% engineering buy technically": "Hardening the
core loop to launch, the eval loop, and booking integration — not servers; infra's on credits."

### S20 Partners — logo wall
**He asks:** "Are all these in the product, or org tools?"
**You:** "Mix — the cloud/AI/travel logos (AWS, Azure, Google, NVIDIA, Cloudflare, Lambda, Viator)
power the product; Datadog/Sentry/PostHog are observability; some (Atlassian, GitHub, Miro) run
the company, not the product." *(Be ready to separate — a sharp VC asks.)*
> Note: deck shows Datadog logo but tool-map says Sentry+PostHog+Langfuse. If Datadog isn't
> actually wired, drop the logo.

---

## The 3 answers you MUST have cold (you flagged these)
1. **Self-hosting** → S13 answer above (trigger-based, config-swap migration, credits make now the wrong time).
2. **"What's actually live vs roadmap"** → live: voice+chat planning, 8 workflow nodes, Tambo UI, PDF, audio (3 modes), Langfuse. Roadmap: disruption re-routing, transit graph, vector memory, eval loop in CI, booking, in-trip companion, 4D map.
3. **Latency / "sub-500ms"** → it's the voice round-trip feel (STT partial + first TTS token), not full-plan time; full plan is API-bound. Be precise or Fares catches it.

## Pre-call: 4 slide edits worth making (reduce live landmines)
- S2 + S18: "real-time transit graph" → "real-time trip planning" (or label roadmap).
- S14: remove "patent-pending."
- S15: "Claude (Bedrock)" → "Azure OpenAI (model-swappable)"; "vector routing engine" → note roadmap.
- Pick ONE credits number (£1.5M+ vs $189K) and make S18 + verbal match.
