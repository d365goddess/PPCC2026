---
name: weatherforecast
description: Gets tomorrow's weather forecast for a location and checks it against the weather maximums for the mission type before a flight plan is created or confirmed.
---
When this skill is activated:
1. Make sure you have a location and a mission type. If the flight plan already has them, use those values instead of asking again.
2. The forecast tool only covers tomorrow. If the flight is not tomorrow, tell the pilot a forecast isn't available for that date and stop.
3. Use the 'Get the forecast for tomorrow' tool for the location to get:
   - Rain chance (%)
   - Wind speed
   - Temperature high
   - The forecast conditions (for example: thunderstorms, snow)
4. Look up the mission type in the Weather Maximums table (Flight Ops Manual, Section 4.1).
5. Compare the forecast against that mission type's maximums. Each value must be at or below the maximum:
   a. Rain chance
   b. Wind speed
   c. Temperature high
6. Check the no-go conditions (Section 4.2): thunderstorms, icing, snowfall, or wind shear advisories.
7. Report a clear GO or NO-GO:
   - GO: every value is at or below the maximums and there are no no-go conditions.
   - NO-GO: say exactly which value failed and by how much (for example: "forecast wind 18 is over the 15 maximum for a Patrol Mission").

## Guidelines
- Never create or confirm a flight plan when the forecast fails any maximum for its mission type. Flag it as NO-GO and let the pilot decide.
- Always say which mission type's maximums you used, so the pilot can check it.
- If the mission type isn't given and isn't on the flight plan, ask for it. The maximums differ a lot by mission type.
- If the tool returns no forecast for the location, tell the pilot. Never assume the weather is fine.

## Examples
**Example 1: GO**
- Pilot: "Check tomorrow's weather in Las Vegas for a Patrol Mission."
- Expected behavior: Get tomorrow's forecast for Las Vegas. Suppose rain chance 10%, wind 8, high 72. Patrol Mission maximums are 75% / 15 / 80. Report GO and list each value against its maximum.

**Example 2: NO-GO on temperature**
- Pilot: "Can we fly a Combat Simulation out of Phoenix tomorrow?"
- Expected behavior: Get tomorrow's forecast for Phoenix. Suppose the high is 88. The Combat Simulation maximum is 32. Report NO-GO: the forecast high of 88 is 56 over the maximum.

**Example 3: Flight isn't tomorrow**
- Pilot: "What's the weather for a Training Exercise next Friday?"
- Expected behavior: Explain that the forecast tool only covers tomorrow, so the weather can't be checked yet.

**Example 4: Missing mission type**
- Pilot: "What's the weather at the base tomorrow?"
- Expected behavior: Ask which mission type the flight is for before comparing, because the maximums differ by mission type.

## Notes
- The maximums are in the Flight Ops Manual, Section 4.1 (Mission Sortie Maximums) and Section 4.2 (No-Go Conditions).
- This skill checks weather only. It doesn't create or update flight plans. Pair it with the preflightchecklist skill when filing a new flight plan.
