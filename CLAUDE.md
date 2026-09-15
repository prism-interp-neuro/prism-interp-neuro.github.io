# PRISM website — context for Claude

Static single-page site for the PRISM reading club (Harvard SEAS / Kempner Institute),
served by GitHub Pages at <https://prism-interp-neuro.github.io>.

## Repo layout

- `index.html` — **the entire site** (inline `<style>`, four "pages", inline `<script>`). All edits happen here.
- `photos/` — speaker headshots (`<firstname>_photo.jpg`) and the two footer logos.
- Remote `origin` = `prism-interp-neuro/prism-interp-neuro.github.io`, branch `main`. **Pushing to `main` publishes the live site.**

The four pages are `div#page-home`, `div#page-past`, `div#page-about`, `div#page-organizers`,
toggled by `showPage(name)`; only one has class `active` at a time. Each page carries its own copy of the `<footer>`.

## The weekly job: adding a new session

The trigger is an announcement (title, speaker + affiliation, abstract, date/time/room, sign-up form, Zoom link).
Five steps, all in `index.html` unless noted:

**1. New card → Home, "Upcoming Event" section.**
Session number = highest existing `S<N>` + 1. Label format:
`<Type> · S<N> · <Month D, YYYY>`, where type is `Guest Talk`, `Paper Presentation`,
`Paper Discussion`, or `Panel Discussion`. Include the sign-up and Zoom buttons.

**2. Demote the previous upcoming event → Home, "Most Recent Event" section.**
Drop its sign-up/Zoom buttons and switch the abstract to past tense
("Yoav will discuss…" → "Yoav discussed…").

**3. Past Events page.** Add the **demoted** event's card at the top of the current semester's
section (cards run newest-first), if it isn't already listed. Do **not** list the new upcoming
session here — Past Events only holds sessions that have already happened, so a session lands
there the following week, when it is demoted. Past Events cards carry `data-tags` and **no**
sign-up/Zoom buttons.

**4. Speaker photo.** Search the web for a headshot — the speaker's lab, department, or
personal page is the usual source. Save as `photos/<firstname>_photo.jpg`, roughly square,
~400px (it renders as a 160px circle). Say where the photo came from in the final message.
If none can be found, use the photoless card layout below.

**5. Commit and push to `main`**, e.g.
`Add S24: Shivam Gandhi (September 15); move Yoav to most recent and past events`.

## Card markup

With a photo (the normal case):

```html
<div class="event-card" data-tags="ml">
  <div class="event-label">Guest Talk · S24 · September 15, 2026</div>
  <div class="event-card-inner">
    <img class="event-speaker-photo" src="photos/shivam_photo.jpg" alt="Shivam Gandhi">
    <div class="event-card-content">
      <span class="tag tag-ml">ML Interp</span><span class="tag tag-sci">AI for Science</span>
      <h3>Talk title</h3>
      <div class="meta"><strong>Speaker Name</strong> (PhD Student, Institution · Program)<br/>
        Tuesday, September 15, 2026 · 3:00–4:00 PM ET · SEC 6.242, Kempner Institute
      </div>
      <p>Abstract, lightly edited for flow.</p>
      <a class="btn btn-outline" href="SIGNUP_URL" target="_blank">Sign Up to Attend In Person →</a>
      <a class="btn btn-outline" href="ZOOM_URL" target="_blank">Join on Zoom →</a>
    </div>
  </div>
</div>
```

Without a photo (and for panels): drop `event-card-inner` and the `<img>`, putting
`div.event-card-content` directly inside `div.event-card`. Panel cards add a
`div.panelists-grid` of `panelist-card` entries (`panelist-photo` / `panelist-name` /
`panelist-role`, role being `Neuro` or `AI`) after the `<p>`.

Buttons: `.btn` is the filled style, used for `Read Paper →` links; `.btn btn-outline` is
the outline style, used for sign-up and Zoom. `data-tags` is only read on the Past Events
page (the Home cards don't need it, but it's harmless).

## Tags

| Span | Label | `data-tags` key | Use for |
|---|---|---|---|
| `tag tag-ml` | ML Interp | `ml` | interpretability of ML models |
| `tag tag-neuro` | Neuroscience | `neuro` | biological neural systems, brain–model alignment |
| `tag tag-sci` | AI for Science | `sci` | interpretability of models trained on scientific data (proteins, physics, biomarkers) |

Tags combine (`data-tags="ml neuro sci"`). Each has a colour block in the CSS and a matching
`.filter-btn.active-*` rule; the Past Events filter bar has one button per key and
`filterEvents()` switches on the key, so **adding a new tag means touching four places**:
the `.tag-*` colour rule, the `.filter-btn.active-*` rule, the filter-bar button, and the
two branches in `filterEvents()`.

## Semester rollover

When a new semester starts, add an `<h2>` heading on Past Events (inline style copied from
the existing headings, `margin:2rem 0 0.5rem`) plus a one-sentence grey summary paragraph
below it with rough session statistics — e.g. "17 sessions — 8 guest talks, 7 paper
presentations and discussions, and 2 panel discussions — taking a synthesis-oriented view…".
Sessions that run through the summer stay grouped under the spring semester.

Then update the About page: rename `This Semester · <term>` to the new term with its focus
areas, and demote the outgoing term to a `<term> Overview` section carrying the same session
statistics. About-page copy stays concise and professional — no methodology prose, no citations.

## House style

- Session times are Tuesdays 3:00–4:00 PM ET, usually SEC 6.242 at the Kempner Institute.
- Abstracts are copied from the announcement with light edits; keep them one paragraph.
- Use `·` as the separator in labels and meta lines, and en/em dashes as in existing copy.
- Match the surrounding indentation (the file is 2-space indented, deeply nested).
- Sanity check after editing: `<div>`/`</div>` and `<span>`/`</span>` counts should balance.
