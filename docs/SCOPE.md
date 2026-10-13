# DubAI – Create AI Agents Championship scope

**Tagline (Igor's):** Your AI. Your UAE. Your World.

**One line:** a voice-first AI agent for visitors to Dubai. You give it a goal in your own language and it plans, books, translates, navigates and reminds, end to end, so a tourist never needs the "gazillion apps".

This scopes what we build for the **Create AI Agents Championship** run by the Dubai Chamber of Digital Economy, announced by Sheikh Hamdan on 7 October 2026. Igor's voice notes came two days later; "cups" in the transcript is "Create Apps", the predecessor competition. Section 11 is the parking lot for everything he adds later. Section 14 maps his 13 October brief (a 24-area global travel app) against this scope.

---

## 1. The competition, as published

Source: the official page at dubaichamberdigital.com/en/create-ai-agents, cross-checked against The National, Khaleej Times, Emirates 24|7 and tbreak. Where sources disagree, the stricter reading is used.

| Item | Fact |
|------|------|
| Organiser | Dubai Chamber of Digital Economy, under the Create Apps in Dubai initiative |
| Categories | Best Societal, Best Personal, Best Business, Best Youth (lead under 21) |
| Prize per category | AED 550,000 grant "to develop and grow the solution and business" |
| AI Agent of the Year | one of the four category winners, additional AED 1,000,000 |
| Total pool | "more than AED 2.5 million" across all four categories |
| Registration | open now, closes **December 2026** (the page's timeline says January, the FAQ says December; plan for December) |
| Submission | **January 2027**: agent prototype + pitch deck + demo video, all in English |
| Semi-finals | 24 teams announced January, pitch training Feb to Mar, semi-final pitch day March |
| Finals | 12 teams, grand final **first week of May 2027**, must demo a **fully working agent live on stage**, "not an MVP" |
| Team | 1 to 6 people, each registers individually |
| Eligibility | worldwide, any age, no fee, own development costs. Fully commercialised solutions excluded. Must be original, not from an existing repo |
| Hard requirement | the agent "must protect personal, confidential and organisational data and prevent information leakage" |
| What they mean by agent | FAQ: understands a goal, makes decisions, uses tools, takes multi-step actions. Explicitly "beyond a chatbot" |
| What judges validate | "customer need, business potential and measurable impact". No published weightings |
| Support | three live workshops Oct to Nov, Digital Academy videos November, four advanced courses, four practical workshops, mentorship Nov to Dec |

**Two corrections for Igor.** The 2.5 million is the whole pool across four categories, not one prize. A category win is AED 550,000; the realistic ceiling for one team is AED 1.55 million if we also take Agent of the Year. And it is a grant to build the business, not cash to split. That changes the conversation about the partnership (section 8).

**Category choice: Best Personal AI Agent.** Its brief is "help individuals with daily life, productivity, time management, decisions". A visitor's concierge is squarely that. Business is a possible second framing (tourism operators, hotels, DET) but we have no B2B customer and judges will ask for one. One entry, Personal, with the tourism-economy angle in the deck as the scale story. The rules allow multiple submissions; we don't spend the effort.

---

## 2. What we are actually pitching

Judges are Dubai government-adjacent, investors and an "independent jury". They know DubaiNow, Dubai AI on dubai.ae (the 2023 generative concierge), Visit Dubai, RTA's S'hail and Careem's super app by name. Digital Dubai's own 2026 roadmap is "agentic AI across government services". If we pitch "one app for all of Dubai" they will say Dubai already has one. The pitch has to be sharper:

> Dubai built the apps for residents. The agent for the visitor who doesn't speak English, has 72 hours, and wants things done rather than explained, doesn't exist yet. DubAI is that agent: give it a goal in Serbian, Russian, Hindi or Mandarin and it books, translates, navigates and reminds.

Honest framing of integrations: "today the agent completes bookings over WhatsApp, which most Dubai restaurants and tour operators already accept; API partnerships are the roadmap". Never claim an integration we don't have. A judge from Careem or RTA will test it.

The demo must show what the FAQ defines as an agent: one goal in, many decisions and tool calls, actions taken, not a chat transcript. That is the single most important design constraint.

---

## 3. Hero flows (P0, must work in the January demo video)

| # | Flow | What the judge sees | Real or simulated |
|---|------|---------------------|-------------------|
| 1 | **Arrival, in Serbian** | Igor speaks Serbian: "I just landed at DXB, get me to Jumeirah Beach Hotel, cheapest reasonable option, and I want dinner tonight somewhere local." The agent replies in spoken Serbian, compares taxi, Careem and metro, picks one, books it, and carries the dinner request into flow 3 without being asked again. | Agent, speech, translation: **real**. Ride booking: **simulated**. Fares and metro timings: **static table** we maintain. |
| 2 | **Point the camera at anything** | Photo of an Arabic/English menu, parking sign or metro map. Agent translates, explains, and offers the next action ("this zone needs a Salik tag, want me to add a reminder?"). | **Real** (vision input to the model). |
| 3 | **Dinner, booked for you** | Agent shortlists from Igor's curated places, user picks, agent drafts a reservation in English and sends it over WhatsApp to the restaurant. A second phone on screen (the "restaurant") receives it. The booking lands in the itinerary with a reminder. | Shortlist: **real** over curated data. WhatsApp: **real** via WhatsApp Business Cloud API to a number we control. Availability: **simulated**. |

The three flows are one continuous conversation in the video, not three separate clips. That is what makes it an agent rather than a demo of three features.

Supporting pieces:

- **Itinerary and reminders.** Every booking lands in a trip timeline, reminders fire as push notifications. Real.
- **Hidden gems.** 30 to 50 places curated by Igor: photo, GPS, entry cost, drive time from Dubai, best time, why it's special. This is the moat and the "measurable impact" story: places no existing app surfaces. Real.
- **Languages.** Serbian, Russian, Hindi, Mandarin, Arabic, English. The model handles translation natively; we need speech-to-text and text-to-speech coverage for each (risk R3).
- **Destination pack and delete-my-data.** Static Dubai facts (emergency numbers, local rules, currency, official links) loaded into the prompt from data, and a one-tap delete of the trip and conversation. Both from the 13 Oct brief (section 14). Small, real.

## 4. Stretch (P1, only if P0 is stable by the freeze)

- **Trip cost estimate.** "How much will three days cost me?" Sums fares, entry costs and typical meal prices from the curated data and the transport table. No new data, one tool. Igor's idea (12 Oct), good fit.
- **Three-day planner.** "I have three days, I like nature and food, no malls." Day-by-day plan from the curated data. Cheap once the data exists, high pitch value.
- **eSIM.** "I need internet for 10 days." Quote and "activate". Simulated; a real partner API (Airalo partner programme or similar) needs a commercial agreement. Verify before promising.
- **Attraction tickets.** Museum of the Future, Global Village, desert safari. Simulated checkout with a QR so it looks real.

## 5. Finals-only work (March to May 2027, only if we are in the 12)

The grand final needs a fully working agent live on stage. Between semis and finals we harden:

- Hosting and data at rest in a UAE region (section 6).
- At least one real booking partner beyond WhatsApp. Candidates: a desert safari operator or restaurant group that will give us a booking endpoint or a dedicated WhatsApp line. Igor's 11 years of contacts matter here more than code.
- Demo mode flag that short-circuits every external call with canned responses if venue wifi dies. Build this in phase 1, not the night before.
- Backup video of the full flow.

## 6. Out of scope for the championship

- Real payments, wallets, card data.
- Real ride-hail, airline, hotel or attraction APIs (none are public; section 7).
- Accounts, social, reviews, app store release. We demo from a dev build.
- Anything outside the UAE. "International later" is one slide. Section 14 says what "leave space for global" costs now: three data-shape choices, no code.

---

## 7. Architecture

Two people, part-time, twelve weeks to submission. Keep it boring.

```
phone (Expo / React Native, TypeScript)
  - chat + push-to-talk + camera
  - cloud speech-to-text and text-to-speech
  - local notifications for reminders
        |
        v  https
backend (Node, TypeScript, single service)
  - agent loop: Anthropic SDK tool runner, claude-opus-5-5,
    adaptive thinking, effort "low" for chat turns, "medium" with tools
  - tools (table below)
  - Postgres: places, trips, bookings, reminders, destination packs
    (every curated row keyed by country/city; Dubai is the only value until May)
  - WhatsApp Business Cloud API client
  - demo-mode flag
        |
        v
curated data (seeded from a spreadsheet Igor owns)
```

### Tools the agent sees

| Tool | Backing | Status |
|------|---------|--------|
| `search_places(query, area, category, budget)` | Postgres full-text over curated data | real |
| `get_transport_options(from, to, when)` | static fare/time table + metro timetable | real data, hand-maintained |
| `book_ride(provider, pickup, dropoff, when)` | mock, returns booking id + ETA | simulated |
| `send_whatsapp_reservation(restaurant_phone, message)` | WhatsApp Business Cloud API | real |
| `check_restaurant_availability(place_id, when, party)` | mock, deterministic | simulated |
| `add_to_itinerary(item)` / `get_itinerary()` | Postgres | real |
| `set_reminder(item_id, minutes_before)` | Postgres + push | real |
| `buy_esim(days, data_gb)` | mock | simulated (P1) |
| image translation | not a tool, image goes to the model as vision input | real |

### Design notes

- One model, `claude-opus-5-5`. Thinking cannot be disabled on it; latency is managed with `effort`, not by switching models. Multi-model cascades cost cache hits and debugging time for pennies.
- Server-side refusal fallback on (`fallbacks: "default"`) so a safety classifier decline never shows as a blank reply on stage.
- Stream everything. Tool inputs stream, text streams to the phone, TTS starts on the first sentence. Perceived latency is the demo.
- Prompt caching on system prompt and tool list; they never change per request. Verify `cache_read_input_tokens` > 0 in logs.
- Session memory is the conversation plus itinerary rows. No vector store at this size.

### Data protection (a published rule, not a nice-to-have)

The rules say the agent must protect personal and organisational data and prevent leakage. For the deck and the Q&A:

- The phone holds no API keys. All provider keys live in the backend's secret store.
- We store the minimum: a trip id, itinerary items, the conversation for the session. No passport, no card, no home address. Name and phone number only if the user gives them for a reservation, and the reservation message is shown to the user before it is sent.
- Conversation data deleted at trip end by default, and a "delete my data" button in the app that does it now. One query, and the rehearsed answer to "how do I get out".
- Prompt injection: tool outputs (place data, WhatsApp replies, anything from the web) are data, never instructions. The system prompt says so and the agent never sends a message or booking without the user confirming in the UI. This also answers "what stops it booking the wrong thing".
- Data at rest in a UAE region for the finals build: AWS me-central-1 (UAE) or Azure UAE North. Assumption, not verified: whether Claude models are served in a UAE region on Bedrock. If not, inference leaves the region and we say so plainly; the stored data stays. Check before the semi-final pitch, judges may ask. For the January prototype, any region is fine.
- UAE Personal Data Protection Law (Federal Decree-Law 45 of 2021) is the reference to cite. One slide, not a legal review.

### Speech

- STT and TTS: cloud provider with Serbian support. Candidates: Azure Speech, Google Cloud Speech, Deepgram. Pick by a 10-clip test of Igor's voice in week 1. Azure and Google both list Serbian voices; ElevenLabs is better in English but Serbian coverage needs checking. Low confidence until tested.
- Arabic TTS quality matters for the judges' ears. Test the same way.

### Hosting

- January prototype: Fly.io, Railway or Render, nearest region. Supabase or Neon free tier. Under 50 USD a month plus trivial API spend.
- Finals: UAE region as above. Container image built in CI; cosign signing only if it costs nothing extra.

---

## 8. Integration reality check

Facts, assumptions and what to verify before any of it goes on a slide.

| Service | Public API? | What we do |
|---------|-------------|------------|
| WhatsApp Business Cloud API | Yes (Meta). Needs a Meta Business account and a phone number. Test numbers work without business verification for a handful of recipients. | Real sends to a number we control. Start the Meta setup in week 1; verification can take weeks. |
| Careem | No public consumer API that I know of. Low confidence, verify. | Simulate. For the finals, ask Igor's network whether a Careem contact exists. |
| Uber | Ride API is partner-gated. | Simulate. |
| RTA metro timetable and fares | Dubai Pulse open data has RTA datasets; freshness unknown. | Static table maintained by hand. |
| Museum of the Future, Global Village | No public booking API. | Simulate, deep link to the real site. |
| eSIM | Partner APIs exist, commercial agreement needed. | Simulate (P1). |
| Google Places | Yes, paid. | Off by default so the demo stays on Igor's data. |
| Restaurant availability | No universal API in the UAE. | Simulate. Slide: "today via WhatsApp, which most venues already accept". |

---

## 9. Plan and effort

Joseph builds, Igor curates content, works his network and fronts the pitch. Dates are calendar weeks from today, 12 October 2026. Submission is January 2027 with no published day; we aim for the **first week of January** so December holidays can't bite.

| Phase | Dates | Deliverable | Exit criterion |
|-------|-------|-------------|----------------|
| 0. Register and decide | 12 to 18 Oct | Both registered on the official page. Written partnership agreement signed (section 8 of the old doc, now R2). Meta business setup started. Places spreadsheet template to Igor. Serbian STT/TTS test done. First live workshop attended. | We are entered, agreed, and know Serbian speech works. |
| 1. Agent core | 19 Oct to 15 Nov | Backend with every tool (mocked where listed), text chat on web, demo-mode flag, first 20 curated places seeded. | Flow 3 works end to end in text; WhatsApp message arrives on a second phone. |
| 2. Voice and vision | 16 Nov to 13 Dec | Expo app with push-to-talk, TTS, camera. Serbian round trip. Itinerary and reminders. All 30+ places in. | Flows 1 to 3 run as one continuous conversation on a phone. **Feature freeze 13 Dec.** |
| 3. Submission pack | 14 Dec to 8 Jan | Demo video (English narration, the continuous conversation, 3 to 4 minutes), pitch deck (need, impact, business model, data protection, roadmap), prototype packaged. Rehearsed. | Submitted by 8 January. |
| 4. Semis (if in the 24) | Jan to Mar | Pitch training they provide. Tighten the deck. Start one real partner conversation. | Semi-final pitch day, March. |
| 5. Finals (if in the 12) | Mar to first week May | Section 5 work: UAE hosting, one real partner, live demo hardening, backup video. | Live demo, no crash, three dry runs. |

Effort to submission: roughly 120 to 150 hours of engineering over 12 weeks, plus Igor's content and workshop time. Phase 1 is known-to-work territory (standard tool-use agent). Phase 2's unknown is Serbian speech quality. Phase 3 is production, not engineering: the video and deck are what 24 semi-finalists get picked on, so they get two full weeks.

Workshops and mentorship (Oct to Dec) are also where the jury's circle is. Igor attends all of them.

---

## 10. Risks and the strongest counter-arguments

**R1. "Dubai already has this."** DubaiNow, Dubai AI on dubai.ae, Visit Dubai, S'hail, Careem. The counter: all of them are resident-first, English/Arabic, and informational or single-vertical. None takes a goal in Serbian and executes across transport, dining and translation. We put a comparison slide in the deck and say it before a judge does. If Digital Dubai announces a visitor agent before May, our pitch becomes "the one they should license", which is a worse pitch but not a dead one.

**R2. Partnership on a voice note.** Igor's "1.25 each" is based on a misreading of the pool, and the money is a grant to grow the business, not cash. Before code: a one-page written agreement covering ownership of code and brand, how a grant is used, split of any proceeds, and what happens if one of us stops. Thirty minutes, protects both of us. Also required practically: the grant will be paid to an entity or a named lead, so decide who.

**R3. Serbian speech quality.** The emotional hook of flow 1. Test in week 0 with real audio before building on it. If Serbian is weak, demo in Russian (large Dubai visitor segment, well-supported in every speech API) and keep Serbian as a text demo.

**R4. Live demo dependencies.** Venue wifi, WhatsApp delivery delay, API outage, phone battery. Mitigations: hotspot, demo-mode flag, backup video. All built in phase 1.

**R5. Name.** "Dubai" inside a trademark in the UAE may need approval from the relevant authority. Low confidence, 20 minutes to check. Also check the domain and app store name. A rename before the deck is free; after the semi-final it isn't.

**R6. Scope creep.** Igor's notes already hold fifteen features. Rule: anything not in sections 3 or 4 goes into section 11 and is revisited after the 13 December freeze. Changes to section 3 need both of us to agree in writing.

**R7. Originality and "not commercialised".** We are fine on both, but the code must be ours, not a forked travel-agent template. Keep the repo history clean from day one.

**R8. Data protection is scored.** The rule is explicit. If a judge asks "where does my data go" and we don't have a crisp answer, we lose points to a weaker agent that does. Section 7's data protection notes become one slide and one rehearsed answer.

**R9. Registration deadline ambiguity.** December per the FAQ, January per the timeline. Register this week and it doesn't matter.

---

## 11. Parking lot (Igor's ideas not in the prototype)

- flight tickets and full trip booking
- car rental
- live taxi vs Uber price comparison
- metro guidance in a visual UI (we cover it conversationally)
- SIM purchase (P1 if time)
- "whole country on the app" with photos and travel times (this is the curated dataset grown over years, not a feature)
- international expansion
- locals investing / monetisation (goes in the deck's business model slide instead)
- two-way interpreter mode for the person you're talking to
- (12 oct) whole-country coverage: Abu Dhabi, Ras Al Khaimah, all seven emirates, same engine with more curated places
- (12 oct) history and facts section: short history of the UAE, how many emirates, what's good where
- (12 oct) per-emirate pages with hotels and headline attractions (e.g. Jebel Jais zipline)
- (12 oct) parking
- (12 oct) live feeds: exchange rates, fuel prices, taxi fare estimates. cheap, not scored, after the freeze
- (12 oct) flight booking as the start of the journey (already listed above, repeated)
- (13 oct) from the 24-area brief, see section 14: inter-emirate routes and Etihad Rail, hotels, flight status and airport navigation, events feed, in-app visual map, shopping and local services, booking changes and cancellations, email to providers, calendar sync, traffic and weather alerts, loyalty programmes, family and group trips, persistent cross-trip profile

---

## 12. Open questions for Igor

1. Register this week on dubaichamberdigital.com/en/create-ai-agents. Both of us, individually. Done?
2. Agree in writing on ownership and split, given the prize is a grant of AED 550,000 per category (AED 1.55 million ceiling), not 2.5 million. Which of us, or which entity, receives it?
3. First 30 places into the spreadsheet (name, GPS, photo, cost, drive time, why special) by 1 November?
4. Which second language if Serbian speech is weak: Russian?
5. Who in your network runs a restaurant, safari operator or hotel desk and would give us a dedicated WhatsApp line or booking endpoint for the finals?
6. Can you attend the three live workshops and the November to December mentorship sessions?
7. Section 14 is my reading of your 24-area brief: Dubai only until May, global as the roadmap slide, destination as data. Agree? Anything you want moved from parking lot into the prototype needs something of equal size moved out.

---

## 13. Decisions taken

- Enter Best Personal AI Agent. One entry.
- Prototype plus deck plus video by 8 January 2027. Feature freeze 13 December.
- Three hero flows as one continuous conversation. Nothing else is a must.
- TypeScript end to end, Expo client, single Node backend, Postgres.
- One model, `claude-opus-5-5`, streaming, caching, refusal fallback on.
- WhatsApp is the one real outbound integration until the finals.
- User confirms every outbound message and booking in the UI. The agent never sends on its own.
- Demo mode and backup video are phase 1 deliverables.
- (13 Oct) Dubai is the product until May. Global is one roadmap slide. A destination is data, not code: `country`/`city` keys on every curated row, a per-destination prompt pack, no destination names in code. No other generalisation before 8 January.

---

## 14. Igor's 13 Oct brief: the 24-area global app, mapped

Source: `docs/igor-brief-2026-10-13.md` (voice note transcript plus the Word document). Igor's ask in one line: "open in Dubai for sure, but leave a space to create it globally". The document is a 24-area spec for a worldwide travel super app.

**Decision: Dubai is the product until May, global is the roadmap slide, and a destination is data, not code.** The championship scores a working agent, a customer need and measurable impact, all judged in Dubai. Nothing in the 24 areas changes sections 3 or 4. What does change is three cheap architecture choices made now so going global later is adding data rather than rewriting:

- Every curated row (places, transport table, fares, emergency numbers, local rules, currency) carries a `country` and `city` key. Dubai is the only value until May.
- The system prompt is assembled from a per-destination pack (facts, rules, numbers, currency, languages) plus a destination-neutral core. Adding Abu Dhabi, or Belgrade, is a new pack.
- No destination or "Dubai" in package, module or table names. Also covers R5.

That costs a few hours in phase 1 and nothing later. The strongest counter-argument is R6: "designed for global" is how two-person projects grow provider abstractions and plugin systems and miss the freeze. The rule is that generalisation is only ever a data column, never a new layer. If a change needs a new abstraction to support a second country, it waits until after 8 January.

Igor's distribution point is good and goes in the deck: Dubai's visitors are from everywhere, so a visitor who used the agent well in Dubai is the launch channel for their home market. Business model slide, one line.

### The 24 areas against the prototype

| # | Igor's area | Where it lands | Note |
|---|-------------|----------------|------|
| 1 | AI personal assistant | **P0** | This is the agent. |
| 2 | Global travel guide and trip planner | **P1** (three-day planner, Dubai) | Global guide is the roadmap. |
| 3 | Public transport, taxi, route planner | **P0** (flow 1) | Transport table and simulated ride. Inter-emirate routes and Etihad Rail: parking lot. |
| 4 | Flights, airports, international travel | parking lot | No public booking APIs; flight status feeds possible after the freeze. |
| 5 | Hotels and accommodation | parking lot | Section 6: no hotel APIs for the championship. Deep link to the hotel's site at most. |
| 6 | Restaurants, food, reservations | **P0** (flow 3) | The one real booking path, over WhatsApp. |
| 7 | Attractions, tours, activities, events | **P0** curated hidden gems, **P1** simulated tickets | Events feed: parking lot. |
| 8 | Maps and local navigation | **P0** conversational, deep link to Google or Apple Maps | In-app visual map: parking lot. |
| 9 | Real-time translation and communication | **P0** (flow 2 camera, voice in six languages) | Two-way live interpreter mode: parking lot. |
| 10 | Local culture, laws, visas, travel requirements | **P1**, cheap | Static content in the destination pack with links to official sources. Half a day. No legal advice tone. |
| 11 | Weather, currency, local information | parking lot (12 Oct live feeds) | Currency and weather are one free API each, after the freeze. |
| 12 | Shopping, deals, local services | parking lot | Pharmacies could ride on the curated data if Igor adds them. |
| 13 | Emergency, safety, healthcare | **P1**, cheap | UAE emergency numbers and nearest hospital as a static card in the destination pack. One afternoon, and a good answer to a safety question from a judge. |
| 14 | Itinerary and document storage | **P0** itinerary; document storage **declined** | Storing passports and boarding passes contradicts the minimum-data stance in section 7, which is scored. Say so in the deck as a choice. |
| 15 | Booking management and AI actions | **P0** partial | "Approval before consequential actions" is already a decision. Changes and cancellations: parking lot. |
| 16 | Email, messaging, provider communication | **P0** WhatsApp | Email: parking lot. |
| 17 | Reminders, calendar, proactive assistance | **P0** reminders | Calendar sync and traffic or weather-driven alerts: parking lot. |
| 18 | Personalised recommendations, daily planning | **P0** via conversation and curated data, **P1** planner | |
| 19 | Budget, expenses, payments, rewards | **P1** trip cost estimate | Payments and wallets are out (section 6). Loyalty: parking lot. |
| 20 | Family, group, business travel | parking lot | "Party size" already exists on reservations; that is all for now. |
| 21 | User profile, memory, preferences | **P0** minimal (language, trip) | A persistent cross-trip profile is a data protection trade-off; roadmap. |
| 22 | Multilingual app and voice interface | **P0** | Serbian, Russian, Hindi, Mandarin, Arabic, English. UI chrome in English for the demo. |
| 23 | Privacy, security, permissions | **P0** | Section 7. Add a "delete my data" button to the P0 list; it is one query and judges ask. |
| 24 | Global integrations and country expansion | roadmap slide | The three architecture choices above are the whole commitment until May. |

Net new work from the brief: the destination pack (10, 13), the delete-my-data button (23), and the `country`/`city` keys. Roughly one to two days inside phase 1. Everything else was already in, or is in the parking lot.
