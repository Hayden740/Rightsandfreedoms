# Rightsandfreedoms

A static, self-contained HTML unit for Seaford Secondary College HASS: "Indigenous
Australians Civil Rights Movement" (5 sequential lessons). No build tooling —
plain HTML/CSS/vanilla JS, opened directly in a browser or served as static files.
There is no package.json, bundler, or framework; don't introduce one.

## Files

- `index.html` — landing page: Acknowledgement of Country, content warning, links
  to all 5 lessons.
- `lesson1.html` … `lesson5.html` — the lessons, in sequence (see below).
- `page-01.jpg` … `page-34.jpg` — scanned booklet pages, embedded as source
  material inline within lessons.
- `*.mp4` — local video assets. Lessons currently embed video via **YouTube
  iframes**, not these local files directly.

## Design conventions — read before touching CSS or colours

**The colour palette is deliberately the Aboriginal flag: black, red, yellow**
(`--black:#1a1008`, `--red:#c0392b`, `--yellow:#e8b84b`), plus `--ochre`,
`--cream`, `--warm-grey` as supporting earth tones. This is intentional, not
incidental — the design centres Aboriginal identity and culture, not the
school. **Never introduce Seaford Secondary College's institutional
branding/colours into this site.** The only place the school is named is a
plain-text credit line ("Seaford Secondary College · HASS"); it does not
otherwise brand the design.

Each lesson also has its own `--accent-color` (set once in `:root` at the top
of the file) used to visually distinguish the 5 lessons, matching the
colour-coded lesson cards on `index.html`:

| Lesson | Accent | Hex |
|---|---|---|
| 1 | red (flag colour) | `#c0392b` |
| 2 | maroon | `#8b3a3a` |
| 3 | ochre (flag-adjacent) | `#b5651d` |
| 4 | green | `#4a7a4a` |
| 5 | purple | `#6b4c8a` |

These accents stay within warm/earthy tones and must never be a school-brand
colour (e.g. blue). Set the accent **once**, via `:root{--accent-color:...}`,
and let the CSS (`.sec-head`, `.li-box`, `.hero`, `.quote`, `.voc strong`,
`.q-num`, etc.) reference `var(--accent-color, var(--red))`. Do **not** repeat
the colour as an inline `style="border-color:#..."` or `style="--accent-color:#..."`
on individual elements — that duplication was a bug fixed on 2026-07-13 and
should not be reintroduced.

## Content/cultural protocols

- Every lesson includes a content warning (deceased persons in film/images,
  outdated historical language) — keep this on any lesson that includes
  archival photos, film, or first-hand accounts.
- Preferred terminology: Aboriginal · Torres Strait Islander · First Nations
  Peoples · Indigenous Australian (per the disclaimer on `index.html`).
- Acknowledgement of Country (Kaurna Land) stays on `index.html`.

## Lesson template (all 5 lessons follow this structure)

1. Student name field (`id="sName"`)
2. 💭 Thinking Question (hook)
3. 🎯 Learning Intention (`.li-box`)
4. ⚠️ Content warning (`.warn`)
5. Booklet page images (`page-NN.jpg`) as primary source material
6. Embedded YouTube video(s) via iframe, labelled "📹 Watch"
7. Question items (`.q-item`) with `<textarea>`/`<input>` answer fields
8. 🪞 Lesson Reflection (`ref1`, `ref2`, `ref3` — always these three IDs)
9. "Download answers as Word doc" button, wired to `doSave()`

## Naming conventions

- **Field IDs**: sequential `q1, q2, q3, …` in document order within each
  lesson (standardized 2026-07-13 — previously each lesson invented its own
  abbreviation scheme; don't go back to that). Paired fields (e.g. lesson 5's
  Indigenous-vs-general-population stats table) use a shared number with an
  `_i`/`_g` suffix, e.g. `q12_i` / `q12_g`. `ref1`, `ref2`, `ref3`, and `sName`
  are constant across all lessons — don't renumber them.
- **`localStorage` keys**: `L<lesson-number>_<field-id>`, e.g. `L3_q7`. Every
  autosaved field is registered in a single `[...].forEach(id => ...)` array
  near the bottom of each lesson's script — when adding a field, add its ID to
  both the HTML and this array, or it won't persist.
- **Exported Word doc filename**: `Lesson_<N>_<Full_Title_With_Underscores>_<StudentName>.doc`,
  built in `doSave()`. Keep the full lesson title in the filename (a lesson 5
  regression to a truncated filename was fixed 2026-07-13).

## Lesson sequence

1. The White Australia Policy
2. Assimilation Policy & The Stolen Generations
3. The Freedom Rides & Wave Hill Walk-Off
4. The 1967 Referendum & Land Rights
5. The Apology & Contemporary Issues

Each lesson's `nav-links` point to the previous/next lesson, except lesson 1
(no previous — points to `index.html`) and lesson 5 (no next — points to
`index.html`).

## Known accepted inconsistencies (not bugs, do not "fix" without asking)

- Section count/depth varies a lot between lessons (lesson 1 has 1 major
  section, lesson 5 has 5+) — this reflects genuine differences in how much
  content each lesson covers, not an oversight.
- Lesson 5 contains a much larger activity than the others: a 28-field,
  paired Indigenous/general-population "Closing the Gap" statistics table.
  This is intentional content design, not a structural bug to normalize away.
