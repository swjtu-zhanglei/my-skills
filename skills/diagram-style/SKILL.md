---
name: diagram-style
description: "Draw or restyle architecture diagrams, flowcharts, UML class diagrams, sequence diagrams, and state diagrams in a consistent pastel technical style with crisp vector artwork and restrained animation. Deliver an HTML viewer and an animated SVG. Use for diagram creation, beautification, or requests for this drawing style (架构图、流程图、类图、时序图、状态图、美化、动态高清图)."
license: MIT
---

# Animated Diagram Style

Apply a visual language: pale canvas, soft blue/green panels, white cards, crisp
type, clear connectors, and calm vector animation. Choose the notation and layout
for the requested diagram; the style does not prescribe a system architecture.
Use the user's language while preserving exact identifiers and product names.

## Deliverables

Produce exactly two files with the same basename:

- `<name>.svg`: editable, self-contained animated vector drawing.
- `<name>.html`: self-contained browser viewer embedding that same SVG, with
  simple zoom controls. Both files must open offline independently.

Keep the SVG authoritative and embed its final bytes in the HTML as a data URI.
Do not maintain two independently drawn versions. The HTML can contain related
detail views when requested; keep them embedded instead of emitting extra files.
For a static-diagram request, retain both formats with motion disabled.

Do not generate JSON sidecars, GIF/PNG previews, logo files, source manifests,
rendering scripts, reports, or screenshots in the output directory. Put temporary
generation and verification artifacts outside the repository. Store any logo
provenance inside SVG `metadata`, so it travels with both deliverables. Additional
formats or files require an explicit user request. The skill's own instructions
and templates are resources, not files to copy into the target output directory.

## Resources

- Read [references/style-guide.md](references/style-guide.md) for colors, spacing,
  notation by diagram type, connectors, product marks, and motion semantics.
- [assets/diagram-template.svg](assets/diagram-template.svg) and
  [assets/diagram-template.html](assets/diagram-template.html) demonstrate the
  style and the two-file delivery format using a fictional flowchart. Replace its
  topology and labels; do not turn every requested diagram into this flowchart.
- Read [references/verification.md](references/verification.md) for the final
  structural, playback, embedding, zoom, and delivery checks.

## Draw the requested relationships

Use the user's description, accepted design, or current repository code as the
source of truth. Follow repository exploration instructions. Mark proposals as
proposals rather than presenting them as implemented behavior.

Choose the diagram's organizing principle before placing shapes:

- Architecture: responsibilities, boundaries, dependencies, and control/data flow.
- Flowchart: steps, decisions, labeled branches, loops, and termination.
- Class diagram: types, members, inheritance, associations, and multiplicities.
- Sequence diagram: participants, lifelines, ordered messages, and interaction frames.
- State diagram: states, events/guards, transitions, and initial/final states.

Preserve the corresponding notation. Do not impose Master/Worker, Task/Fragment,
buffers, a blue control plane, or a green execution plane on unrelated diagrams.
For code-backed diagrams, show only the detail needed to explain the requested
relationships. Distinguish accepted requests from completed work where relevant.

## Compose and animate

1. Establish a main reading direction appropriate to the diagram. Use whitespace,
   alignment, and a small number of tinted groups to make the hierarchy clear.
2. Keep names legible at the intended display width. Route connectors around
   shapes and labels. Simplify crowded content before shrinking the type.
3. Preserve semantic shapes and arrowheads. Define route geometry once in `defs`
   and reuse it for static lines and animated overlays.
4. Add verified product artwork only when product identity matters. Neutral vector
   glyphs are appropriate for generic steps, classes, and participants.
5. Animate actual flow or message order with small tokens. For structural diagrams
   such as class diagrams, highlight a relationship for reading focus instead of
   inventing execution flow. Static topology and labels stay visible throughout.
6. Keep animation inside SVG using CSS or SMIL, with no runtime script or external
   resources. Reduced motion and printing must show the complete static drawing.
   HTML-only controls can use a small inline script; the SVG must animate on its own.

High definition comes from vector text, paths, and artwork, not larger bitmap
dimensions. Preserve the `viewBox` and appropriate aspect ratio. Animation timings
illustrate order or reading focus; they do not claim real execution durations.

## Check and deliver

Inspect the actual diagram, including a zoomed region and animation phases.
Confirm playback in the standalone SVG and the HTML viewer. If used in a README,
check SVG image embedding and keep the surrounding prose short. Some hosts show
only a static preview; link the original SVG and HTML for browser playback.

Confirm only the requested HTML/SVG pair was produced, all references resolve,
and temporary artifacts remain outside the repository. When consolidating older
output, update links and retain requested detail views and logo provenance inside
the two deliverables before deleting redundant generated files.

Return both file paths and the checks actually performed. For repository edits,
show actual `git diff` hunks or link the full diff when generated artwork is large.
