# Visual language

The style is independent of subject matter: a pale canvas, white cards, softly
colored groups, crisp type, and precise relationships. Reuse these visual choices
across architecture, flow, class, sequence, and state diagrams. Preserve each
diagram's notation rather than reusing one architecture's topology.

## Palette

| Visual role | Fill | Stroke / accent |
|---|---|---|
| Canvas | `#f7f9fc` | — |
| Main text | — | `#172b43` |
| Secondary text | — | `#62758a` |
| Blue group | `#eef4fd` | `#c6d6f0` |
| Blue card | `#ffffff` | `#b4cbed` |
| Blue connector / highlight | — | `#7997c6` / `#2563eb` |
| Green group | `#eef8f5` | `#b8ddcf` |
| Green card | `#ffffff` | `#b2dacc` |
| Green connector / highlight | — | `#149d83` / `#087f68` |
| Violet accent | `#f9f6ff` | `#c8b7e8` |
| Amber accent | `#fff9ea` | `#e5cd8e` |
| Neutral note | `#ffffff` | `#dce5ee` |

Assign colors consistently within a drawing: ownership, stage, or category. Add
a compact legend if color carries meaning. Blue can denote control and green data
in an architecture diagram; those roles are not mandatory for other diagram types.
Labels and shapes carry meaning even without color. Preserve product-logo colors.

## Layout and notation

| Diagram | Layout and semantics to preserve | Suitable animation |
|---|---|---|
| Architecture | Group components by ownership; show external actors and dependencies; expand a representative replica when helpful | Tokens on real control/data routes |
| Flowchart | Process rectangles, decision diamonds, start/end terminals; label every decision branch and make loop destinations clear | Follow one valid path per pass; distinguish alternate paths |
| Class | Compartmented type/name/member cards; solid hollow-triangle inheritance, dashed realization, associations with role/cardinality, diamonds at the owning end | Sequential relationship emphasis, explicitly a reading guide rather than object activity |
| Sequence | Participants across the top; time downwards; dashed lifelines, appropriate call/return arrowheads, activation bars and `alt`/`opt`/`loop`/`par` frames where relevant | Highlight messages in order; preserve concurrent/alternative semantics |
| State | Initial/final symbols, named states, event/guard/action labels on transitions | Trace a valid transition path; do not highlight mutually exclusive states as one state |

Use rounded cards (roughly 10–12 units) and group panels (14–16 units) where that
does not change notation. A decision remains a diamond, and an inheritance arrow
remains a hollow triangle. A subtle shadow is optional; keep edges and text sharp.

Use comfortable outer margins and regular spacing. Group only meaningful ownership
or categories; avoid deeply nested decorative frames. Leave room for edge labels.
Adapt the canvas to the content rather than forcing a fixed width/height. Related
detail views can live inside collapsible sections of the HTML, keeping the main
SVG readable. Add explanatory notes only when the shapes alone omit a key fact.

## Typography and connectors

- Use local sans-serif fonts with CJK fallbacks, such as
  `Inter, Noto Sans CJK SC, Microsoft YaHei, system-ui, sans-serif`.
- Around a 1200–1400 unit canvas width, use a 24–28 unit title, 16–18 unit primary
  labels, and 12–14 unit supporting labels. Check the intended display width.
- Keep technical names exact; class members can use a local monospace font.
- Use straight or gently rounded orthogonal connectors around 2 units thick.
  The target endpoint must land on the intended shape, compartment, or lifeline.
- Put labels beside connectors or on a small background mask. Draw moving overlays
  before node/label layers so motion never obscures text.
- Define routes once under `defs`. Reference the same geometry for both base
  connectors and motion. Preserve semantic arrowheads while highlighting a route.

## Product marks

Product logos are optional, not a requirement for every drawing. Use them when
the diagram represents actual products; generic classes or decisions need none.

1. Prefer verified repository artwork, an official product site, vendor icon pack,
   or official project repository. A company logo is not necessarily a product icon.
2. Keep proportions and recognizable colors; include the product name as text.
3. Embed SVG artwork as a data URI or sanitized inline paths. Remove scripts and
   remote resources; namespace inline IDs to avoid gradient/clip-path collisions.
4. Record product, owner, source URL, exact asset/symbol, content SHA-256, and any
   transformation inside SVG `metadata`. The HTML embeds that same SVG, so the
   provenance remains available without a separate manifest. A hash is not a
   license grant; retain applicable attribution and do not imply endorsement.
5. Prefer a neutral vector glyph to an unverified logo. Disclose any bitmap used;
   making it larger does not make it vector artwork.

Do not emit separate logo files or catalogs into the output directory.

## Motion and portability

Use small tokens for directional flow and restrained outlines for structural
emphasis. A 12–20 second loop is often comfortable; adapt it to the diagram.
Highlight meaningful stages only when the subject has stages. Class diagrams
should not gain a fictional runtime lifecycle just to support animation.

Keep every node, edge, and label visible in the static base layer. Do not animate
box positions, resize text, or continuously flash. Motion must not imply that
submitting a request guarantees its completion, or that independent paths are
sequential. Use `prefers-reduced-motion` and print styles to suppress only motion.

The HTML is a small offline viewer, not a new application. Embed the final SVG
bytes and keep styles/scripts inline. Simple zoom and fit controls are enough.
Avoid remote fonts, CDNs, runtime fetches, or a generated frontend dependency tree.
