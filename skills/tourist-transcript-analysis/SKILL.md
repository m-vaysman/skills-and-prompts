---
name: tourist-transcript-analysis
version: 3
status: open
---

# 🌆 Tourist Transcript Analysis Skill (v3)

**Purpose:** A reusable prompt template for deep analysis of a tourist's travel transcript. It moves beyond the itinerary to extract sensory, emotional, and cultural material while adhering strictly to the source text.

**Changes from v2:**
- Evidence rules that enforce "invent nothing" instead of just requesting it: three-tier tagging (stated / strong inference / weak inference), text anchoring, no unpacked labels, emotion as behavioral evidence, speaker provenance, silence as data, length caps.
- A scene spine (chronological table) that every section refers back to.
- New lenses: route & movement, daily rhythm, the body, food & drink, money & friction, language, encounters, other tourists, expectation vs. reality, change & decay, a proper-noun index, the narrator as filter.
- Closing sections: Gaps, and What to Steal for Fiction.

---

## 📝 The Prompt Template

Copy and paste the text below along with your transcript into any AI to run the analysis.

---

Act as an expert travel psychologist, cultural anthropologist, and literary analyst. I am going to provide you with a transcript of a tourist's experience in a city. Analyze the text through the lenses below.

**CRITICAL RULE: Add and invent nothing.** Your analysis must be based strictly and solely on the provided transcript. Do not fabricate details, make up events, or hallucinate sensory inputs that are not present in the text. Do not supply outside knowledge about the city — its history, geography, landmarks, customs, or reputation — even if you are certain it is true. If the tourist did not say it, it is out of scope.

### EVIDENCE RULES (apply to every section)

1. **Tag every claim with one of three tiers.**
   - STATED — the tourist says or does this in the transcript. Quote the words or give the line/timestamp.
   - STRONG INFERENCE — supported by several moments; cite them.
   - WEAK INFERENCE — plausible but thinly supported; say so.
   "Not determinable from the transcript" is a valid and required answer whenever the text is silent. Never fill a section for the sake of filling it.

2. **Anchor every claim to the text** with a short quote or a line/timestamp reference.

3. **No unpacked labels.** Do not use words such as vibrant, charming, bustling, authentic, chaotic, magical, gritty, laid-back, or friendly unless you immediately cash them out into what was specifically seen, heard, smelled, said, or done.

4. **Emotion as evidence.** Whenever you name a feeling, give the behavioral sign next to it: they lingered, doubled back, left early, went quiet, laughed, complained, took photos, refused something, sat down. A feeling with no sign is at most a weak inference.

5. **Provenance.** If more than one voice appears (a guide, a companion, a local, a narrator's voice-over), attribute everything. Claims made by a guide are "the guide said," not fact; do not let a guide's history lesson become your history lesson. If the transcript is spoken (fragments, filler, self-corrections), quote it as spoken — do not tidy a fragment into a sentence the tourist never said.

6. **Silence is data.** Subjects the tourist never mentions (food, prices, people, safety, history) tell you about the tourist, not about the city. Record them in the Gaps section rather than filling them in.

7. **Respect each section's length cap.** Padding is where invention creeps in.

### OUTPUT (use these headings, in this order)

**0. Scene Spine**
Before any interpretation, build a chronological table of every scene in the transcript:
Scene # | Place (as the tourist names it) | Time of day (stated, or "unknown") | Who is present | What happens | Mood + its behavioral sign | One concrete detail in the tourist's own words.
Every later section refers back to scene numbers. Cap: one row per scene, no commentary.

**1. The Emotional Itinerary (Where & When)**
- Map the mood across the scene spine: what the tourist felt, where exactly they were, and when.
- Highs and lows: what they loved, what they disliked, and turning points where the mood shifted.
- Anchor each turning point to a threshold if the transcript gives one — crossing a bridge, entering a building, leaving a crowd, nightfall, a meal, an encounter.
Cap: 200 words plus the turning-point list.

**2. Route & Movement**
- Sequence of places in the order visited, and how they moved between them (walking, metro, taxi, bus, boat), only as stated.
- Getting lost, navigating, landmarks used for orientation, distances as felt ("it took forever," "it was right there").
- Where they stopped, sat, waited, turned back.
Cap: 150 words.

**3. Sensory & Atmospheric Details**
- The vibe: through their eyes, what does this city feel like? Unpack every adjective into its source.
- Micro-details: the specific sounds, smells, textures, light, temperatures, and small sights they explicitly noticed that give this city its distinct identity.
Cap: 200 words. Prefer the tourist's exact phrasing.

**4. Time: Season, Weather & Daily Rhythm**
- Time of year: stated, inferred (tag the tier and cite the clue), or not determinable.
- Weather: how it directly shaped the route, pacing, and emotional state.
- Daily rhythm, separate from the calendar: what the city is like at the hours they saw it — early morning, midday, evening, night; opening and closing times, meal hours, quiet hours, nightlife — only as observed.
Cap: 150 words.

**5. The Body**
- Fatigue, heat, cold, hunger, thirst, sore feet, jet lag, sleep, illness, needing a bathroom, hangovers.
- Where a physical state visibly changed what the tourist noticed or how they judged a place.
Cap: 100 words. "Not mentioned" is a fine answer.

**6. Food & Drink**
- What they ate and drank, where, at what hour, how it was served and paid for.
- What they refused, disliked, or were surprised by; rituals around eating they noticed.
Cap: 150 words.

**7. Money & Friction**
- Prices, tipping, bargaining, queues, tickets, reservations, scams or warnings, cash vs. card, closed doors, wrong turns, bureaucratic obstacles.
- How the tourist reacted to friction: patience, irritation, humor, retreat.
Cap: 120 words.

**8. Language**
- Language-barrier moments, phrases they learned or attempted, signage they mention, overheard speech, how locals addressed them (which language, which register).
- Any moment where language changed how they were treated.
Cap: 100 words.

**9. Locals: Observed and Encountered**
- Observed: what the tourist actually stated about local people — dress, behavior, pace, gestures, how they move through the city differently from tourists.
- Encountered: each specific interaction (waiter, driver, vendor, stranger, host), what was said or done, and how the tourist was treated as a tourist — hospitality, indifference, hostility, being sold to, being corrected.
Cap: 200 words. Do not generalize from one encounter to "the locals."

**10. The Outsider Lens**
- Expectation vs. reality: any preconception the tourist states arriving with, and what they found.
- Culture shock: moments of misunderstanding, discomfort, or a shift in perspective from contact with the local environment.
- Other tourists: where the crowds were, and how the narrator positions themself against other tourists — kinship, contempt, avoidance, relief at finding "no tourists."
Cap: 150 words.

**11. Timeless vs. Changing**
- Timeless: elements the tourist themself framed as old, permanent, or deeply rooted in history. Do not add the history; record only what they said about it.
- Changing: construction, chain stores, phones and screens, gentrification, closures, ruin, decay, "it used to be." What is in tension with the timeless layer.
Cap: 150 words.

**12. Proper-Noun Index**
Every named street, district, square, building, landmark, shop, restaurant, dish, drink, brand, transit line, and person, as a simple list, each with what the tourist said about it. If a name is unclear in the transcript (garbled, misspelled), record it as written and flag it. This list is the fabrication check: nothing may appear in any other section that is not traceable to the transcript.

**13. The Narrator as Filter**
The transcript is not a window onto the city; it is a filter. Describe the filter.
- Attention: what does this tourist automatically scan for — food, prices, safety, history, architecture, people, status, comfort, photos, their companion?
- Comparison baseline: what do they compare things to ("like X back home")? What does that reveal about where they are from and how they travel?
- Vocabulary of praise and complaint: their recurring words for good and bad.
- Travel posture: planned or improvised, confident or anxious, alone or accompanied, guidebook or wandering.
- Contradictions: where the narrator contradicts themself (loved it at noon, tired of it by evening). Record; do not resolve.
Then state, in two or three sentences, what in the preceding sections is probably a feature of the city and what is probably a feature of this tourist.
Cap: 250 words.

**14. Gaps**
List every section above that the transcript could not fill, and every ordinary tourist subject the narrator never touched. No speculation about why.

**15. What to Steal for Fiction**
Separate:
- Transferable setting mechanisms — how heat or rain changes pacing, how a street's sound changes at dusk, how money is handled, how a language barrier alters status, how a crowd empties and refills, how a place is entered and exited. These can move to a fictional city without resembling this one.
- The tourist's personal material — their anecdotes, companions, biography, and idiosyncratic reactions — which belongs to them and should not be copied into a character.

Format your response with clear headings for each section so it is easy to read. Use the tourist's own words wherever a quote is shorter and more specific than a paraphrase.

[Insert Transcript Here]
