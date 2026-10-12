# DubAI – competition prototype scope

**Tagline (Igor's):** Your AI. Your UAE. Your World.

**One line:** a voice-first AI concierge for visitors to the UAE. You talk to it in your own language and it plans, books, translates, navigates and reminds, end to end, so a tourist never needs the "gazillion apps".

This document scopes what we build for the competition pitch (finals believed to be May 2027). It is deliberately a demo-grade prototype, not a product. Igor will keep adding ideas; section 9 is where they go so the build line stays fixed.

---

## 1. What we are actually pitching

Judges will not remember a feature list. They will remember one tourist, one phone, one conversation, and the moment a real WhatsApp reservation lands on a second phone on stage.

So the prototype is built around **three hero flows** that run live, plus a curated layer of local knowledge that only Igor can supply. Everything else is simulated behind a realistic-looking tool call, and we say so honestly in the pitch ("integrations are mocked today; the agent layer is real").

The pitch claim, framed truthfully:

> One conversation replaces ten apps. The AI understands you in your language, knows the UAE better than a resident, and does the work for you.

Not: "we have integrated every service in Dubai." We haven't, and judges with any technical background will test that claim.

---

## 2. Hero flows (P0, must work live)

| # | Flow | What the judge sees | Real or simulated |
|---|------|---------------------|-------------------|
| 1 | **Arrival in a foreign language** | Igor speaks Serbian: "I just landed at DXB, I need to get to Jumeirah Beach Hotel." The agent replies in spoken Serbian with taxi vs Careem vs metro, price and time for each, picks one on request, and sets a reminder. | Agent, speech in/out, translation: **real**. Ride booking: **simulated** (returns a plausible booking id, fare, ETA). Fares and metro timings: **static data table** we maintain. |
| 2 | **Point the camera at anything** | Photo of an Arabic/English menu, parking sign or metro map. The agent translates it into the user's language, explains what it means, and offers the next action ("want me to order this?" / "this zone needs a Salik tag"). | **Real** (vision model). |
| 3 | **Dinner for two, booked for you** | "Book me a nice Lebanese place near Dubai Marina for two tonight at 8." Agent shortlists from our curated places, user picks, agent drafts a reservation message in English and sends it over WhatsApp to the restaurant. A second phone on stage (the "restaurant") receives it. Agent adds the booking to the itinerary and a reminder. | Shortlist: **real** over curated data. WhatsApp send: **real** via WhatsApp Business Cloud API to a number we control. Restaurant availability: **simulated**. |

Supporting pieces the hero flows depend on:

- **Itinerary and reminders.** Every booking lands in a trip timeline. Reminders fire as push notifications ("driver pickup at 13:00 for your 15:00 safari"). Real.
- **Hidden gems.** 30 to 50 places curated by Igor with photo, GPS, entry cost, drive time from Dubai, best time of day, why it's special. This is the moat. Judges can ask "what's something no tourist knows about?" and get a real answer with a real photo. Real.
- **Multilingual by default.** Serbian, Russian, Hindi, Mandarin, Arabic, English as demo languages. The model handles translation natively; we only need speech-to-text and text-to-speech coverage for those languages (see risk R3).

## 3. Stretch (P1, build only if P0 is stable by the freeze date)

- **eSIM purchase flow.** "I need internet for 10 days." Agent quotes a plan and "activates" it. Simulated; a real partner API (e.g. Airalo partner programme) needs a commercial agreement we won't have in time. Verify before promising.
- **Attraction tickets.** Museum of the Future, Global Village, desert safari. Simulated checkout with a QR code so it looks real on stage.
- **Price comparison for rides.** Live quotes would need Careem/Uber partner APIs (not public, verify). Static fare tables are enough for the demo.
- **Trip planner.** "I have three days, I like nature and food, no malls." Agent produces a day-by-day plan from the curated data. Cheap to build once the data exists, high pitch value.

## 4. Explicitly out of scope for the competition

- Real payments, wallets, or storing card data.
- Real ride-hail, airline, hotel, or attraction booking integrations.
- User accounts, social features, reviews.
- App store release. We demo from a dev build / TestFlight / web.
- Operating outside the UAE. "International later" is a pitch slide, not code.

---

## 5. Architecture

Keep it boring. Two people, part-time, one demo.

```
phone (Expo / React Native, TypeScript)
  - chat + push-to-talk + camera
  - speech-to-text and text-to-speech via cloud providers
  - local notifications for reminders
        |
        v  https
backend (Node, TypeScript, single service)
  - agent loop: Anthropic SDK tool runner, model claude-opus-5-5,
    adaptive thinking, effort "low" or "medium" for latency
  - tools (see table below)
  - Postgres (Supabase or Neon): places, trips, bookings, reminders
  - WhatsApp Business Cloud API client
        |
        v
curated data (seeded from a spreadsheet Igor owns)
```

### Tool surface the agent sees

| Tool | Backing | Status |
|------|---------|--------|
| `search_places(query, area, category, budget)` | Postgres full-text over curated data, optional Google Places fallback | real |
| `get_transport_options(from, to, when)` | static fare/time table + metro timetable | real data, hand-maintained |
| `book_ride(provider, pickup, dropoff, when)` | mock, returns booking id + ETA | simulated |
| `send_whatsapp_reservation(restaurant_phone, message)` | WhatsApp Business Cloud API | real |
| `check_restaurant_availability(place_id, when, party)` | mock, deterministic | simulated |
| `buy_esim(days, data_gb)` | mock | simulated (P1) |
| `add_to_itinerary(item)` / `get_itinerary()` | Postgres | real |
| `set_reminder(item_id, minutes_before)` | Postgres + push | real |
| `translate_image` | not a tool, image goes straight to the model as a vision input | real |

Design notes:

- One model. Multi-model cascades save pennies and cost cache hits and debugging time. `claude-opus-5-5` with `effort: "low"` for chatty turns, `"medium"` when the turn calls tools. Thinking cannot be disabled on this model, so latency is managed by effort, not by switching it off.
- Enable the server-side refusal fallback (`fallbacks: "default"`) so a safety classifier decline never shows up on stage as a blank reply.
- Stream everything. Tool inputs stream with `eager_input_streaming`, text streams to the phone, TTS starts on the first sentence. Perceived latency is the whole demo.
- Prompt caching on the system prompt and tool list. They never change per request, so put them first and cache them. Verify with `cache_read_input_tokens` > 0 in logs.
- Session memory is just the conversation plus the itinerary rows. No vector store. Not needed at this size.
- Secrets: API keys live in the backend's environment only. The phone never holds a key. WhatsApp token and Anthropic key in the hosting provider's secret store, not in the repo.

### Speech

- Speech-to-text: cloud provider with Serbian support. Candidates: Azure Speech, Google Cloud Speech-to-Text, Deepgram. Pick by Serbian accuracy on a 10-clip test of Igor's voice.
- Text-to-speech: same test. Azure and Google both list Serbian voices; ElevenLabs quality is better for English but Serbian coverage needs checking. Low confidence until tested.
- Fallback: iOS and Android on-device speech recognition for English only.

### Hosting

- Backend on Fly.io, Railway or Render. One region (Frankfurt or Bahrain for latency to Dubai). Container image built in CI, signed with cosign if we have a spare afternoon, otherwise skip for the prototype.
- Database on Supabase free tier.
- Total running cost for the demo period: under 50 USD a month plus API usage, which is trivial at demo volume.

---

## 6. Integration reality check

Facts, assumptions and things to verify before we promise them in a pitch.

| Service | Public API? | What we do |
|---------|-------------|------------|
| WhatsApp Business Cloud API | Yes (Meta). Needs a Meta Business account, a verified business, and a phone number. Test numbers work without verification for up to 5 recipients. | Real send to a number we control. Start the Meta business setup in week 1 because verification can take weeks. |
| Careem | No public consumer API that I am aware of. Low confidence, verify. | Simulate. |
| Uber | Ride request API exists but is partner-gated. | Simulate. |
| RTA metro timetable / fares | Dubai Pulse open data has some RTA datasets. Verify what is current. | Static table we maintain by hand. |
| Museum of the Future, Global Village | No public booking API. | Simulate with deep link to the real site. |
| eSIM (Airalo etc.) | Partner API exists, commercial agreement needed. | Simulate (P1). |
| Google Places | Yes, paid. | Optional fallback for places outside the curated set. Keep it off by default so the demo stays on Igor's data. |
| Restaurant availability | No universal API in the UAE. | Simulate. Pitch slide: "today via WhatsApp, which 80% of restaurants already accept; integrations as partnerships land". |

---

## 7. Build plan and effort

Assumes Joseph builds, Igor curates content and drives the pitch. Part-time for both. Weeks are calendar weeks, not effort.

| Phase | Weeks | Deliverable | Exit criterion |
|-------|-------|-------------|----------------|
| 0. Decide | 1 to 2 | Competition rules read, submission date known, written partnership agreement signed (see section 8), Meta business verification started, places spreadsheet template given to Igor. | We know what the judges score and when the real deadline is. |
| 1. Agent core | 3 | Backend with all tools (mocked where listed), text-only chat on web. Curated data seeded (first 20 places). | Flow 3 works end to end in text, WhatsApp message arrives on a second phone. |
| 2. Voice and vision | 3 | Expo app with push-to-talk, TTS, camera. Serbian round trip. | Flows 1 and 2 work live on a phone. |
| 3. Polish and pitch | 2 | Itinerary screen, reminders, demo script rehearsed 10 times, deck. Feature freeze at the start of this phase. | Three dry runs with no crash. Backup video recorded in case the venue wifi dies. |

Rough total: 9 to 10 calendar weeks. Say 100 to 140 hours of engineering. Finals in May leave slack, but **the submission or qualifying round is almost certainly months earlier**. That is the date that matters and we don't know it yet.

Effort tags: phase 1 is known-to-work territory (standard tool-use agent). Phase 2 is should-work-per-docs (Serbian speech quality is the unknown). Phase 3 is rehearsal, not engineering.

---

## 8. Risks and the strongest counter-arguments

**R1. The aggregator idea has been tried.** Travel super-apps exist and most died on partnership costs, not technology. Counter-argument to our own pitch: Careem itself is already a "super app" in the UAE. Our answer has to be the conversational layer plus local knowledge, not "we integrate everything". If Igor pitches integrations as done, a judge who works at Careem or RTA will take it apart.

**R2. Partnership before product.** Prize money of 2.5 million split 50/50 is a verbal agreement in a voice note. Before any code is written there should be a one-page written agreement: ownership of the code and the brand, split of any prize or investment, what happens if one of us stops. This is a 30-minute task and it protects both of us. Reversible if we never need it, expensive if we do.

**R3. Serbian speech quality.** The whole emotional hook of flow 1 is Igor speaking his own language. If STT mangles it or TTS sounds robotic, the demo falls flat. Test in week 1 with real audio, before building anything on top of it. If Serbian is weak, demo in Russian or another well-supported language instead and keep Serbian as a text demo.

**R4. Live demo dependencies.** Venue wifi, WhatsApp delivery delay, API outage, phone battery. Mitigations: hotspot from a second phone, pre-recorded backup video, a "demo mode" flag that short-circuits every external call with canned responses if we flip it. Build demo mode in phase 1, not the night before.

**R5. Name and brand.** "Dubai" as part of a trademark in the UAE may need approval from the relevant authority. Low confidence, worth 20 minutes of checking before printing it on a deck. Also check the domain and app store name are free.

**R6. Scope creep.** Igor's voice notes already contain about fifteen features. He will add more. The rule: anything not in sections 2 or 3 goes into section 9 and is revisited only after the phase 3 freeze. Both of us sign off on changes to section 2.

**R7. Judging format unknown.** If judges want a deck only, half of phase 2 is wasted. If they want a working product with users, this scope is too small. Resolve in phase 0.

---

## 9. Parking lot (ideas from Igor, not in the prototype)

Everything he mentioned that is not above, kept so nothing is lost:

- flight tickets and full trip booking
- car rental
- taxi vs Uber live price comparison
- metro station guidance and timings in the app UI (we cover this conversationally)
- SIM card purchase (P1 if time)
- "whole country on the app", every venue with photos and travel time (this is the curated dataset grown over years, not a feature)
- international expansion beyond the UAE
- locals investing / monetisation model
- verbal translation for the person you're talking to (two-way interpreter mode)

---

## 10. Open questions for Igor

Answer these before phase 1 starts. Each one changes the build.

1. Which competition exactly, and what is the submission or qualifying deadline (not the finals date)?
2. Is the 2.5 million a cash prize, investment for equity, or something else? Which currency?
3. What do the judges expect on the day: live demo, video, deck, or all three? How long is the slot?
4. Will you curate the first 30 places into a spreadsheet (name, GPS, photo, cost, drive time, why special) in the first two weeks?
5. Are you willing to sign a short written agreement on ownership and split before we start?
6. Which second language do we demo if Serbian speech turns out weak in testing?

---

## 11. Decisions already taken in this scope

- Prototype, not product. Simulated integrations, stated honestly.
- Three hero flows, nothing else is a must.
- TypeScript end to end, Expo client, single Node backend, Postgres.
- One model, `claude-opus-5-5`, streaming, caching, refusal fallback on.
- WhatsApp is the one real outbound integration because it is the one judges can see happen.
- Feature freeze two weeks before the submission date, whatever that date turns out to be.
