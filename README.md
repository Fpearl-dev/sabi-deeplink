# Sabi

A quiet social app. Feed, private journal, circles, ephemeral updates, messages.

## Running it

```bash
npm install
npm run dev
```

Requires Node 18+. Opens on http://localhost:5173.

```bash
npm run build     # production bundle
npm run lint      # eslint
npm run format    # prettier
```

## Structure

```
src/
  main.jsx            entry point
  App.jsx             root state and routing
  theme.js            palettes; every colour resolves through CSS variables
  constants.js        nav items, layout/visibility options, camera filters
  data/seed.js        placeholder content — replaced by the API
  lib/                formatting helpers, SVG placeholder media
  hooks/              timers, object URLs, scroll lock, double tap
  components/         reusable pieces (FeedCard, sheets, nav, updates)
  views/              full screens (Home, Journal, Circles, Profile, ...)
```

Two rules worth keeping:

1. **No component is defined inside another component.** Doing so creates a new
   type on every render, which unmounts inputs and drops the keyboard. There is
   an eslint rule (`react/no-unstable-nested-components`) set to error for this.
2. **No hardcoded colours.** Everything goes through the CSS variables in
   `theme.js`, which is what lets a theme repaint the whole app.

## Database

`supabase/schema.sql` is the full schema: tables, row-level security, and the
triggers that keep denormalised counts honest.

Visibility is enforced in the database, not the client. A journal entry is
private because RLS says only its author can read it, not because the UI hides
it.

## State of things

Working: the whole interface, four themes, camera capture with filters,
local persistence.

Not built: the backend. Auth accepts anything, every other person is a constant
in `data/seed.js`, and reporting shows a toast instead of filing a report.
That is the next piece of work.
