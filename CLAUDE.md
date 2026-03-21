# Boulder Attempt Tracker — CLAUDE.md

## Who this is for and why
Boulderers (rock climbers who specialize in bouldering) want to 
track attempts and sessions on difficult climbs called "projects." 
Existing tools like Kaya are social-first and expensive (~$60/year). 
This is a lightweight demo to test whether climbers want a simpler, 
cheaper alternative focused on intentional training. We're showing 
this to real climbers at a gym — it needs to feel credible, not 
like a rough prototype.

## What this is
A simple, single-page demo app for tracking bouldering attempts. 
No backend, no auth, no persistence required — data only needs 
to last for a single browser session.

## Tech stack
- Plain HTML, CSS, and JavaScript
- Single file (index.html) — no frameworks, no build tools

## Visual style
- Modern and clean, inspired by tools like Notion
- Lots of white space, subtle borders, simple sans-serif typography
- Nothing heavy or dated — this should feel like a real product
- Use climbing terminology: grades are "V0–V17", completing a 
  climb is "sending" it, a hard climb is a "project"

## Core features to build

### Boulders tab (default view)
- A table with columns: Name, Grade, Attempts, Sessions, Status
- A "+" button that opens a dialog to add a new boulder
  - Name: free text input
  - Grade: dropdown with values V0 through V17
- Attempts, Sessions, and Status are not user-editable
- Status defaults to "Projecting"; can become "Sent"

### Sessions tab
- A "+ Add boulder" button that lets the user pick from 
  boulders they've created
- Each added boulder shows:
  - Name and grade
  - An attempt counter (format: n/5, starting at 0/5)
  - An "Add attempt" button that increments the counter
    - Disabled once the counter reaches 5/5
  - A "Mark as sent" checkbox that:
    - Highlights the boulder green
    - Updates its Status to "Sent" in the Boulders tab

### Data sharing between tabs
- Boulders created in the Boulders tab must be available 
  to add in the Sessions tab
- Attempts and sessions logged in the Sessions tab must 
  update the counts in the Boulders tab

## Out of scope
- Login or authentication
- Saving data beyond a single browser session
- Sorting or filtering the table
- Editing or deleting a boulder once added
- Tracking more than one session
- Changing the max attempt limit (hardcoded at 5)