---
name: preflightchecklist
description: Before a flight plan can be created you always need to run this pre-flight checklist
---
When this skill is activated:
1. Make sure you always get a flight date AND time.
2. Find out what weekday the flight date is and compare it to the 'In-service days' column of the aircraft (fighter jet table) to confirm the fighter jet is available on that day (Monday, Tuesday, Wednesday, etc.)
3. Check for scheduling conflicts:
   a. Calculate the new flight's End Time = Flight Date/Time + Duration (Duration is stored in minutes).
   b. Query the flight plan table using the Dataverse MCP server, filtered to:
      - Same Aircraft as the requested flight plan
      - Same Flight Date
   c. For each flight plan returned, calculate its End Time = its Flight Date/Time + its Duration (minutes).
   d. A conflict exists if: (new Start Time < existing End Time) AND (existing Start Time < new End Time).
      This catches any overlap, not just exact time matches. For example, a 10:00 to 11:00 flight
      conflicts with a request for 10:30, even though the start times don't match.
   e. If any returned flight plan conflicts under this rule, the flight plan cannot be created.

## Guidelines
- If the flight date is on a day that the fighter jet is not in service, a flight plan cannot be created.
- If the date provided is in the past, a flight plan cannot be created.
- If the new flight's time range overlaps with any existing flight plan for the same aircraft (per the Step 3 conflict rule), a flight plan cannot be created.
- Duration values are always in minutes. Do not treat them as hours when calculating End Time.
- Always filter the flight plan query by aircraft and date before checking for overlaps, rather than scanning all flight plans.

## Examples
**Example 1: Aircraft not in service**
- User request: "I want to file a new flight plan for 11/25/2026 at 11AM. Pilot: Amelia Johnson, Mission Type: Patrol Mission, Aircraft: A-10C Thunderbolt II"
- Expected behavior: Use the Dataverse MCP server to get the fighter jet details from the fighter jet table. Tell the user that since 11/25/2026 is a Wednesday and the A-10C Thunderbolt II is only in service on Monday, Tuesday, and Sunday, a new flight plan cannot be created.

**Example 2: Missing time**
- User request: "I want to file a new flight plan for 11/23/2026. Pilot: Amelia Johnson, Mission Type: Patrol Mission, Aircraft: A-10C Thunderbolt II"
- Expected behavior: Ask the user to provide a time, then run the skill again.

**Example 3: Successful creation**
- User request: "I want to file a new flight plan for 11/23/2026 at 10AM. Pilot: Amelia Johnson, Mission Type: Patrol Mission, Aircraft: A-10C Thunderbolt II"
- Expected behavior: Use the Dataverse MCP server to create the new flight plan in the flight plan table. Tell the user a new flight plan has been created and provide the user with the flight ID.

**Example 4: Scheduling conflict (overlap, not exact match)**
- Existing flight plan: Aircraft A-10C Thunderbolt II, 11/30/2026 at 10:00 AM, Duration 60 minutes (ends 11:00 AM).
- User request: "I want to file a new flight plan for 11/30/2026 at 10:30 AM. Pilot: Amelia Johnson, Mission Type: Patrol Mission, Aircraft: A-10C Thunderbolt II"
- Expected behavior: Calculate new End Time as 11:30 AM (10:30 AM + 60 min duration assumed if not provided, or as given). Query existing flight plans for A-10C Thunderbolt II on 11/30/2026. Detect that 10:30 AM < 11:00 AM (existing end) AND 10:00 AM (existing start) < 11:30 AM (new end), so this is a conflict. Tell the user a flight plan cannot be created due to a scheduling conflict with an existing flight plan from 10:00 to 11:00 AM.

## Notes
The two Dataverse tables that have the relevant information are the fighter jet table and the flight plan table.
