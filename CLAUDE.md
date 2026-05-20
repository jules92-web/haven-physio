# haven-physio — Claude Project Brief

## What this is
A set of self-contained HTML physiotherapy briefing files for dogs at Haven, a dog sanctuary run by Paws in Wayanad (India). Each file summarises one dog's injury history, physio protocol, current status, and key handling considerations — extracted from WhatsApp group chat exports. There is also an `index.html` that serves as a landing page linking to all individual briefings.

The site is hosted on GitHub Pages at: https://jules92-web.github.io/haven-physio/

## Folder structure
```
haven-physio/
├── index.html                        # Landing page with dog cards + dropdown
├── [dogname]-physio-briefing.html    # One file per dog
├── CLAUDE.md                         # This file
└── chat-exports/                     # WhatsApp .txt exports (local only, not in git)
```

## Dogs with briefings
| Dog | File | Status | Condition |
|-----|------|--------|-----------|
| Mooben | mooben-physio-briefing.html | Plateau | Spinal fracture + decompression surgery (Dec 2025) |
| Kanji | kanji-physio-briefing.html | Improving | Spinal luxation T10–T11 (Dec 2025), no surgery |
| Gunni | gunni-physio-briefing.html | Post-op | Right hind femur fracture, multiple surgeries |
| Lumbi | lumbi-physio-briefing.html | Recovering | FHO surgery + partial Achilles rupture |
| Paappi | paappi-physio-briefing.html | Recovering | Spinal cord injury + surgery (Nov 2025) |
| Plumby | plumby-physio-briefing.html | Physio | Bilateral hind fractures → Grade 2 luxating patella |
| Tamu | tamu-physio-briefing.html | Complex Recovery | Spinal cord injury, knuckling, hyperextension, UTI history |
| Teety | teety-physio-briefing.html | Improving | Bilateral left leg fractures, now walking |
| Timmy | timmy-physio-briefing.html | Improving | Severe spinal fracture + 1/3 cord tear |
| Veebukka | veebukka-physio-briefing.html | Improving | Femoral head fracture → two FHO surgeries |
| Zuumba | zuumba-physio-briefing.html | Early Recovery | Spinal compression, no surgery |

## Design system

### Colour variables (used in all briefing files)
```css
--bg            /* page background */
--card          /* card background */
--ink           /* primary text */
--ink-light     /* secondary text */
--ink-faint     /* muted text / labels */
--accent        /* dog's unique accent colour */
--accent-soft   /* light version of accent for icon backgrounds */
--ashram        /* ashram banner text colour (deep blue/brown) */
--ashram-bg     /* ashram banner background */
--ashram-border /* ashram banner border */
--green / --green-bg   /* positive status */
--amber / --amber-bg   /* caution status */
--border        /* card borders */
--shadow        /* card box shadow */
```

### Each dog has a unique dark header colour
- Mooben: near-black charcoal (#1a1e1a)
- Kanji: dark forest green (#1a2e1a)
- Gunni: dark brown (#3d1a08)
- Lumbi: dark teal (#0a2830)
- Paappi: dark navy (#0f1d3d)
- Plumby: dark indigo (#1d0a3d)
- Tamu: dark slate (#0d2230)
- Teety: dark olive (#1a2e0a)
- Timmy: dark burgundy (#2d0a12)
- Veebukka: forest green (#0a2518)
- Zuumba: chocolate brown (#271810)

### Fonts
Google Fonts: `Playfair Display` (headings) + `DM Sans` (body)

### Page structure (in order)
1. `.header` — dark background, dog name in Playfair Display, meta subtitle
2. `.stats-row` — same dark background, grid of key stats (sex, age, weight, injury, status)
3. `.content` (max-width 860px, centred) containing:
   - `.ashram-banner` — amber/brown banner for Tom/Uma guidance (see below)
   - Diagnosis & Condition section
   - Key History timeline
   - Current Status grid
   - Physio Protocol table
   - Key Considerations cards
   - Open Questions
4. `.footer`

### Component patterns

**Ashram banner** — amber background (`--ashram-bg`), brown border (`--ashram-border`), 🪔 icon. Used exclusively for messages from Tom or Uma, which carry Bhagavan's guidance and take precedence over vet opinions.

**Timeline dots:**
- Default (blue/accent) — regular events
- `.milestone` (green) — positive milestones
- `.warning` (amber) — setbacks or concerns

**Status grid badges:**
- `.badge-green` — positive / improving
- `.badge-amber` — caution / ongoing
- `.badge-red` — critical / mandatory / not resolved

**Protocol table** — dark header matching the dog's header colour, alternating row shading.

**Open questions** — amber boxes with `?` prefix. Use for genuinely unresolved clinical questions.

## Authority hierarchy
**Tom and Uma** speak on behalf of Bhagavan. Their messages always go in the Ashram banner and override vet recommendations. Flag these clearly.

All other contributors (Helis, Tieme, Elske, Anne, Rhea, Ava, Nick, etc.) are staff/volunteers — their observations and vet consultations are important but sit below Ashram guidance in authority.

## Chat export naming convention
Chat exports follow this naming pattern:
```
chat-exports/[DogName]_chat_YYYY-MM-DD.txt
```
Examples:
```
chat-exports/Tamu_chat_2026-05-20.txt
chat-exports/Plumby_chat_2026-05-20.txt
```
The date is the export date. Using YYYY-MM-DD ensures files sort chronologically in Finder. When multiple exports exist for the same dog, always use the most recent one (latest date).

## Weekly update workflow
1. Export active chats from WhatsApp (only dogs where something happened that week)
2. Drop the `.txt` files into `chat-exports/` following the naming convention above
3. Tell Claude Code which dogs have new exports, e.g. "Tamu and Plumby have new chat exports, update their briefings"
4. Claude Code reads the new chat file, compares with the existing HTML, and updates only what has changed
5. Review changes by opening the HTML files directly in your browser (`open index.html`)
6. When happy: `git add . && git commit -m "update briefings week of YYYY-MM-DD" && git push`

## How to update a briefing from a new chat export
1. Find the latest export in `chat-exports/[DogName]_chat_YYYY-MM-DD.txt`
2. Read the file (in chunks if large — files can be 1000+ lines)
3. Extract only physio-relevant content: new diagnoses, protocol changes, Tom/Uma messages, status changes, key events
4. Update the relevant sections: Ashram banner, Timeline, Current Status grid, Physio Protocol table, Open Questions
5. Update the header subtitle with the new date range and "Updated DD Mon YYYY"
6. Update the footer "Last updated" date
7. Update the stat for Status if it has changed

## index.html structure
- Dropdown `<select id="dogSelect">` with `<option value="[filename]">[Name]</option>` for each dog
- Dog cards: `<a class="dog-card" href="[filename]">` containing `.dog-info` (name + meta) and `.dog-badge` (status)
- Badge classes on index: `badge-active` (green), `badge-watch` (amber), `badge-red` (red)
- When adding a new dog: add to both the dropdown AND the card list

## Key physio concepts referenced in these files
- **Knuckling** — inability to correct paw placement; paws fold under during walking
- **PROM** — passive range of motion exercises
- **FHO** — femoral head ostectomy (hip surgery)
- **Hyperextension** — joints overextending beyond normal range (seen in Tamu)
- **Durga** — physiotherapist based in Chennai who has consulted on several cases
- **Spinal decompression** — surgery to relieve pressure on spinal cord
- **Bladder expression** — manual technique to empty bladder in dogs with nerve damage
- **Luxating patella** — kneecap that slips out of position (Grade 1–4)
- **Hydrotherapy** — pool-based physio, used for Tamu and others
- **Ashram** — the spiritual community that runs Haven; Tom and Uma are senior members
