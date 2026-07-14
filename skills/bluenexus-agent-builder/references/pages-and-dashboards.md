# Pages and dashboards on BlueNexus

BlueNexus pages (dashboards) render inside a **restricted sandboxed iframe**. A page can look perfect in the HTML and still not behave in the render if you ignore these rules. This is the single biggest source of wasted build time — read it fully before building any page.

> Note (platform v1.10.0, Jul 2026): page artifacts can now be deployed as **live per-visitor mini-apps** with a richer runtime and live data collections, and a page can request consent to perform writes. The full-page-write + repaint model below still works and remains the safe default; if you use the newer live-collection features, verify behaviour against the current platform docs, as the sandbox specifics are evolving.

## The output rules (non-negotiable)

1. **Full-page writes only. Never a partial patch.** Updating a fragment of an existing page can mis-render — markup may be escaped so your `<table>` shows as the literal text `&lt;table&gt;` and the canvas collapses into plain text. Always rewrite the entire page in one write (mode = replace).
2. **Inline CSS with hex colours.** Do **not** use Tailwind or other utility class names — the sandbox has no compiled stylesheet, so those classes do nothing. No external CDN, web font, image, or `<script src>`; only same-origin platform libraries are available.
3. **Do not rely on in-page click JavaScript.** Scripts may not execute in the rendered iframe even when present and correct in the saved HTML. So: don't build tab-switching, expanders, or buttons that depend on the page's own JS. Instead treat the page as a **rendered snapshot you repaint** whenever state changes (see "progressive dashboard" below).
4. **Everything on one screen; the embedded view may not scroll** and can truncate content below the fold. Use fixed, compact element heights so all key content fits in the initial viewport. If content grows when populated (e.g. filled tiles get taller), pin their heights so the layout doesn't overflow.
5. **Design tokens.** The platform exposes CSS variables (e.g. `--bn-*`) and a small set of per-page brand colours; use them so the page matches the agent's brand, and always pair a fill with a readable foreground.

If you follow 1–4, a page renders reliably. If a page comes back as escaped text, you patched instead of full-writing. If controls are dead, you depended on in-page JS.

## The screenshot-recreate method (how to nail a specific look)

This is the most reliable way to reproduce a designed dashboard:
1. The user pastes **reference screenshots** of the target design into the agent chat.
2. The agent **maps the design to its own data and skills** — "recreate this layout, but wire each element to what I actually have (this connector, this field, this skill output)."
3. The agent builds the page with a **single full-page write** following the output rules above.
4. Iterate in chat: the user points at what's off, the agent full-writes again. Repeat until it's right.
5. **Save the finished build as one of the agent's own skills** (a dashboard-builder skill) so the agent can reproduce that exact template — personalized — for every future user, instead of sharing one static page.

## Progressive / personalized dashboards (the "fills in as you connect data" pattern)

A dashboard that appears to fill in step by step as the user connects data or answers questions is achieved **entirely through repaints**, not interactivity — which is why it works despite rule 3:
- On the trigger, the agent builds the empty/Phase-0 board (full-page write).
- As each new piece of context arrives (a connector linked, a question answered, a document read), the agent recomputes the page state and does **another full-page write** reflecting the new state — a filled tile, an advanced progress ring, a status change.
- Because each update is a full write (not a patch) and a static render (not in-page JS), it renders cleanly every time.
- Keep tile/element heights fixed so the board stays on one screen as it fills.

Model the visual states explicitly in the skill: e.g. empty / in-progress / done / needs-review / gap, each with its own colour treatment, so repaints are consistent.

## Downloads / final artifacts

If the workflow ends in a downloadable output, produce it as a **separate document artifact** the user can open/copy. Note that direct file downloads from the page sandbox may be limited — a document artifact is the reliable vehicle; don't rely on `window.print()` or blob-download inside a page.
