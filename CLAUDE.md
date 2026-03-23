# Redpoint — CLAUDE.md

## Who this is for and why
Boulderers (rock climbers who specialize in bouldering) want to 
track attempts and sessions on difficult climbs called "projects." 
Existing tools like Kaya are social-first and expensive (~$60/year). 
This is a lightweight tool to test whether climbers want a simpler, 
cheaper alternative focused on intentional training. The app has 
been demoed to real climbers at a gym — it needs to feel credible, 
not like a rough prototype.

## What this is
A single-page app for tracking bouldering projects and sessions. 
No backend, no auth, no persistence — data only lasts for a single 
browser session. Built as a V0 proof of concept.

## Tech stack
- Plain HTML, CSS, and JavaScript
- Single file (index.html) — no frameworks, no build tools
- Inter font via Google Fonts

## Visual style
- Modern and clean, inspired by Notion
- Lots of white space, subtle borders, simple sans-serif typography
- Notion-like color palette: off-white backgrounds, muted grays, 
  subtle semantic colors (orange for Projecting, green for Sent, 
  blue for Flashed)
- Use climbing terminology throughout: grades are "V0–V17", 
  completing a climb is "sending" it, a hard climb is a "project",
  sending on the first attempt is a "flash"
- App name is Redpoint, displayed with a 🪨 emoji and "Red" in red

## Architecture
The app uses a simple in-memory state object:
- state.boulders — array of project objects
- state.session — array of session entry objects
- state.settings — max attempts and rest time settings
Changes to session data must update both state.session and 
state.boulders to keep the two tabs in sync.

## Features

### Projects tab (default view)
- Displays a table of projects with columns: Name, Grade, 
  Attempts, Sessions, Status
- A "+ Add project" button opens a dialog to add a new project
  - Name: free text input (autocomplete, autocorrect, and 
    autocapitalize are all off to prevent iOS AutoFill)
  - Grade: dropdown with values V0 through V17
- Status is not user-editable, derived from session data:
  - "Projecting" (default, orange badge)
  - "Sent" (green badge)
  - "Flashed" (blue badge) — automatically applied when a 
    boulder is sent on the first attempt
- On mobile, the table becomes a card layout (one card per 
  project) showing all five data points stacked vertically

### Sessions tab
- Header shows "Today's session" with a pencil/gear icon that 
  opens the Session settings dialog
- A "+ Add project" button lets the user add a project to 
  the current session
- Each session card shows:
  - Header row: project name and grade badge on the left, 
    "Sent" checkbox on the right
  - Divider line
  - Actions row: "ATTEMPTS" label with counter (n/max) on 
    the left, "Add attempt" button on the right
  - Card background is light grey; actions section is white 
    to create visual hierarchy
  - Cards highlight green when sent, blue when flashed
- "Add attempt" is disabled when:
  - Max attempts reached
  - Boulder is marked as sent
  - Rest timer is still in the red (not enough rest)
- "Sent" checkbox is disabled until at least one attempt 
  has been logged

### Rest timer
- Displayed in a dedicated row above the session cards
- Shows total session time and split time since last attempt
- Split time displays in red with 😴 emoji when rest period 
  is not complete, green with 🧗 emoji when ready
- Timer starts automatically on first attempt
- Resets split timer on each new attempt

### Session settings dialog
- Accessible via the pencil/gear icon next to session header
- Max attempts: configurable 1–9 (default 5)
- Rest time: configurable 1–5 minutes (default 3)

## Mobile design decisions
- Breakpoint: 600px and below is treated as mobile
- Projects table switches to a card layout on mobile — 
  no horizontal scrolling
- Session cards use a two-section layout on mobile:
  - Header section (grey background): name, grade, sent checkbox
  - Actions section (white background): attempts counter and 
    add attempt button
- Dialogs are full-width on mobile (max-width: calc(100vw - 32px))
- All input and select font sizes are 16px on mobile to prevent 
  iOS auto-zoom
- Dialog focus is set to the container (not inputs or selects) 
  on open to prevent iOS from auto-expanding dropdowns
- window.scrollTo(0, 0) is called via requestAnimationFrame 
  when dialogs close to prevent iOS scroll position drift
- outline: none on dialog element to prevent focus ring on open
- Tab nav is sticky below the header (top: 48px) so it stays 
  visible while scrolling

## Out of scope (V0)
- Login or authentication
- Saving data beyond a single browser session
- Sorting or filtering the projects table
- Editing or deleting a project once added
- Tracking more than one session
- Custom domains or deployment beyond GitHub Pages