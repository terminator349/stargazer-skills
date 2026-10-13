---
name: sky-tour-guide
description: Gives a live, guided tour of what is in the sky right now (or at a chosen time tonight), with directions anyone can follow, like "look southeast, 3 fists above the horizon". It combines the astro_sky_today and astro_positions tools, filters to objects that are actually up, and orders the stops in a sweep across the sky. Use when the user is outside or about to go out and asks "what can I see right now?", "what's that bright star?", "give me a tour of the sky", or wants to know where to look for a planet or star tonight.
license: MIT
compatibility: Requires the CycleCalcs astronomy MCP server (https://www.cyclecalcs.com/mcp), which provides the astro_sky_today and astro_positions tools.
metadata:
  author: Jake Croft
  version: "1.1"
---

# Sky Tour Guide

Astronomy tools answer in altitude and azimuth degrees, which is useless to
someone standing in a dark backyard. This skill teaches the agent how to
combine the CycleCalcs tools and translate their numbers into directions a
person can follow with their hands, like a planetarium guide would.

## Workflow

1. **Location and time.** Use the place the user named, or their saved home
   location if your agent keeps one. If you don't know where they are, ask.
   The time is **now**, unless the user names a time ("at 10 pm").
   Work out the IANA time zone and pass it as `tz`.

2. **Snapshot:** call `astro_sky_today` with the location and `tz`.
   - If the Sun is up or it is still twilight, say so in one line and give the tour
     for the time darkness starts tonight (nautical dusk or later).
   - Note the Moon's phase and which planets are up.

3. **Read `references/bright-targets.md`** from this skill (in Agno:
   `get_skill_reference("sky-tour-guide", "bright-targets.md")`).

4. **Positions in one call:** call `astro_positions` with the location, the
   chosen `date` (omit it for "now") and `bodies` = `moon` + the naked-eye planets
   that are up + about 14 `radec:` targets from the table (20 bodies maximum).

5. **Filter and order:**
   - Keep objects with altitude **≥ 15°**. Drop the Moon if it is below 15°.
   - Only use planets you can see without a telescope: Mercury, Venus, Mars, Jupiter and Saturn.
     **Never include Uranus, Neptune or Pluto** unless the user asks for them by name.
   - If fewer than 5 objects pass, add more bright stars from `bright-targets.md`
     instead of faint objects.
   - Choose **5 to 7 stops**, preferring the brightest objects.
   - Order them by **azimuth, smallest first** (a simple sort). The user then turns
     smoothly from north through east, south and west instead of spinning back and forth.
     Don't do any other reordering.
   - Use at least one binocular target if any are up, and mention Polaris if the
     user is in the Northern Hemisphere and has never done this before.

6. **Translate every position** with the rules below, then write the tour in
   the output format.

## Translating numbers into directions

**Direction** (azimuth → 16-point compass):

| Azimuth | Say | Azimuth | Say |
|---|---|---|---|
| 349–11° | north | 169–191° | south |
| 11–34° | north-northeast | 191–214° | south-southwest |
| 34–56° | northeast | 214–236° | southwest |
| 56–79° | east-northeast | 236–259° | west-southwest |
| 79–101° | east | 259–281° | west |
| 101–124° | east-southeast | 281–304° | west-northwest |
| 124–146° | southeast | 304–326° | northwest |
| 146–169° | south-southeast | 326–349° | north-northwest |

**Height** (altitude → hand measures at arm's length):
- A closed **fist** ≈ 10°. Say "N fists up", with N = altitude ÷ 10, rounded to the nearest half fist.
- Above 70°: say "almost straight overhead".
- Always also give the degrees in brackets, e.g. "3 fists up (31°)".

**Brightness clues:** planets don't twinkle and stars do. Say which way to tell
them apart when a planet is near a bright star.

## Output format

```
## 🌠 Sky Tour · <Place> · <time, local>
<one line: darkness status + Moon phase emoji and %>

**1. <Object>** · look **<direction>**, **<N> fists up** (<alt>°)
<one sentence: how to recognize it> <one sentence: a fun fact>
🔭 <only if useful: what binoculars or a small telescope show>

**2. <Object>** · ...

...

👉 Next: <one suggestion, e.g. "Want to know what rises later tonight?">
```

Rules:
- Use plain words and no jargon. Never show RA/Dec or azimuth numbers, only
  directions and fists (with altitude degrees in brackets).
- Local 12-hour time only.
- If fewer than 3 objects are ≥ 15° up (for example, during a bright twilight), say so
  and suggest a better time tonight.
