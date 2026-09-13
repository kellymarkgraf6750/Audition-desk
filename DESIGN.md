# Audition Desk — WebApp Design

## Purpose
A focused, austere web application for faculty audition evaluation. The live workflow is append-only: enter judgment once, never reorder rows, and let the application derive role rankings and final-voting suggestions.

## V1 screens
1. **Live Entry** — singer name/voice, 3–9 score, qualitative decision, Role 1/2/3 in casting-precedence order, notes, previous/next, audition queue.
2. **Role Board** — one role at a time; candidates ranked by score with priority (P1/P2/P3), decision, and notes.
3. **Final Ballot** — each role with suggested 1/2/3 plus independent editable faculty choices.
4. **Season Setup** — show/role administration; later becomes the import/configuration area.

## Proposed production stack
- Next.js + React + TypeScript
- Tailwind CSS, with a custom restrained visual system rather than a large component library
- Supabase: PostgreSQL, Auth, Row Level Security, and optional Realtime
- PWA shell for tablet/laptop use
- IndexedDB/local-first queue for instant saves during auditions; sync to Supabase when connected
- Vercel (or equivalent) for deployment

## Core data model
- `seasons`: id, name, status, created_at
- `productions`: id, season_id, title
- `roles`: id, production_id, name, voice_type, sort_order
- `singers`: id, season_id, external_id, name, voice_type, audition_date, audition_time, sort_order
- `evaluations`: id, season_id, singer_id, score, decision, notes, updated_at
- `role_considerations`: id, evaluation_id, role_id, priority (1..3)
- `faculty_votes`: id, season_id, role_id, faculty_id, rank (1..3), singer_id

The crucial separation is between an **evaluation** (what the faculty member thought of the singer) and a **role consideration** (which role(s) that singer is being considered for, and in what precedence). This maps directly to the logic that made the spreadsheet useful.

## Import/export roadmap
Not required for V1, but the schema should support:
- CSV/XLSX import of singer schedules
- CSV/XLSX import of productions and roles
- CSV/XLSX export of evaluations and final ballot
- optional full-session JSON backup

## UX principles
- No drag-and-drop ranking during auditions.
- Large, fast score controls.
- Autosave every edit locally before attempting cloud sync.
- One-screen live workflow with minimal navigation.
- Role suggestions are recommendations, never automatic final votes.
- The system should surface conflicts but not make casting decisions for the faculty.
- The default view on launch during an audition is the current singer, not a dashboard.

## Offline/sync behavior
1. Input changes immediately update local state.
2. UI never waits on the network to acknowledge an audition note.
3. A background sync writes changes to Supabase when available.
4. Each record carries `updated_at` and a client version/sequence so conflicts can be detected rather than silently overwritten.
5. The app should visibly show a small `Saved locally` / `Synced` status, not an intrusive alert.

## Prototype
`audition-casting-prototype.html` is a dependency-free first-pass interaction prototype. It uses localStorage and a small seed dataset based on the current audition repertoire, so the interaction can be evaluated before building authentication and the Supabase layer.
