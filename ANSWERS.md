ANSWERS.md
1. How to Run

Open index.html directly in any browser — no installs needed
Or run npx serve . and visit http://localhost:3000
No build step, no dependencies

2. Stack & Design Choices
Stack: HTML, Bootstrap 5 (CDN), and vanilla JavaScript

Chose vanilla JS because it's a single screen with no complex state — a framework wasn't needed
Bootstrap handled the responsive layout and form styling without writing it from scratch

Decision 1 — Preset buttons instead of a dropdown for tip %

My original course code used a <select> dropdown
Replaced it with buttons (10%, 15%, 20%) + a custom input field
The active button highlights in blue so you always know what's selected
A dropdown hides options and doesn't allow custom values easily

Decision 2 — Results panel dims when inputs are invalid

The results card starts at 50% opacity
Only goes fully visible when all inputs are valid
Stops the user from reading a half-calculated result as correct

3. Responsive & Accessibility
Responsive:

On 360px phone: single column, inputs on top, results below
On 1440px laptop: centered in a 520px card so it doesn't stretch on wide screens
Bootstrap's fluid container handles the reflow automatically

Accessibility — what I handled:

All inputs have proper <label> elements linked by for and id
Tip buttons are real <button> elements so Tab and Enter work natively
Error messages appear as text near the field, not as alerts or popups

Accessibility — what I skipped:

Did not add aria-live to the results panel
A screen reader won't announce updates automatically — user has to tab into results manually
Skipped to keep the code simple within the time available

4. AI Usage
Tool 1 — Claude (claude.ai)

Shared my original course assignment code and asked it to compare against the assessment spec
It identified the gaps: missing live updates, inline validation, custom tip input, reset button, rounding policy, and grand total
Used it to figure out where exactly to place new code (like toggleReset())
What I changed: Claude suggested calling toggleReset() with separate oninput listeners on each field. I moved it inside the existing calculate() function instead since that already runs on every input — one place to control it rather than three separate listeners

Tool 2 — Replit AI

Pasted my original course code into Replit and used its AI to help fix the gaps Claude identified
It helped me rewrite the calculate function to support live updates and add the custom tip input logic
What I changed: Replit's AI gave me the custom tip validation with a single error div shared across all tip errors. I split it so each field has its own dedicated error div, which makes errors appear right next to the field that caused them instead of in one generic spot

5. Honest Gap

The custom tip field doesn't handle edge cases like typing just . — parseFloat(".") silently returns NaN and the results go blank with no clear message
With more time I'd add a regex check like /^\d+(\.\d*)?$/ before parsing
Would also show a specific message like "Enter a number like 15 or 12.5" instead of leaving the user confused

Rounding Policy

Per-person amount is always rounded up to 2 decimal places
Formula: Math.ceil(value * 100) / 100
Why ceiling and not nearest: rounding to nearest can cause the group to collectively pay less than the actual bill
Ceiling ensures the group never underpays — maximum difference is Rs 0.01 per person