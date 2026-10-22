# Verify the HTML and animated SVG

Keep checks proportional to a documentation asset. Use available browser tools;
do not add application tests or install a frontend toolchain into the target
project just to inspect a diagram. Keep temporary captures, scripts, and validation
reports outside the repository. The deliverable directory contains only the named
HTML/SVG pair unless the user explicitly requests more.

## Structural checks

Parse the final SVG as XML, not just as HTML:

```bash
python3 -c 'import sys, xml.etree.ElementTree as E; E.parse(sys.argv[1]); print("SVG XML valid")' path/to/diagram.svg
```

Check these properties in the final artifact:

- Positive `viewBox` dimensions; matching canvas aspect ratio.
- Unique IDs and resolved local `href` / `url(#...)` references.
- A meaningful `title` and `desc`, with `aria-labelledby` on the root.
- No runtime scripts or remote resource loads inside the SVG. Inspect logo SVGs too;
  a data URI can still contain references to external resources.
- Text and routes remain vectors. Check image MIME types before describing the
  whole artifact as vector; an embedded PNG is still raster.
- Motion is absent in static/reduced/print modes without removing static routes.
- The HTML embeds the final SVG bytes, not an outdated copy; decode its data URI
  and compare it with the standalone SVG. Related embedded detail views may differ.
- HTML styles and controls are inline. Open each file offline independently;
  neither file requires sibling assets, JSON, remote fonts, or a rendering service.
- Logo provenance, if needed, lives in SVG `metadata`; no separate manifest.
- Existing README links point to the retained files or HTML section anchors.

These checks do not prove layout quality or playback.

The template's root `data-motion="on"` enables its animation. Set it to `off`
for static delivery; keep the same routes, labels, and relationships.

## Browser checks

Open the final SVG, using a local file or a local server appropriate to the tools.
Wait for fonts before taking measurements or screenshots.

| Check | Evidence |
|---|---|
| Main composition | Read the actual image: no cropped labels, crossings through cards, or ambiguous arrow endpoints |
| Normal reading size | Inspect around the README's displayed width, not just a large desktop canvas |
| 4× zoom | Inspect type, connector tips, and logos after changing render scale; do not enlarge a screenshot |
| Actual playback | Capture two different times; confirm moving overlays change while static content remains |
| Phase ownership | Inspect a frame in every phase; active edges and highlighted chip describe the same action |
| Image embedding | Load the SVG through an HTML `img`, as a README does; confirm it still animates |
| HTML viewer | Open the delivered HTML offline; check zoom/fit and any detail-view anchors |
| Reduced motion | Enable the preference and confirm a complete, static diagram |
| Print | Confirm a complete diagram with moving overlays suppressed |
| Offline portability | Block network requests; marks, labels, and geometry still render |

For deterministic phase inspection with a browser console or Playwright, pause
CSS animations and seek them without editing the SVG file:

```javascript
for (const animation of document.getAnimations()) {
  animation.pause();
  animation.currentTime = 8000;
}
```

The template's useful sample times are 0.5, 5, 9, and 13 seconds. Adapt them if
the authored phase schedule changes. Also verify normal playback after resuming;
seeking alone does not demonstrate that the image animates on its own.

In some browser automation environments, a page-level reduced-motion emulation
does not reach the isolated SVG document behind an `img`. Check with the actual
OS/browser preference or a separate browser launched with that preference before
diagnosing the SVG. In Chromium, a browser-wide check can use
`--force-prefers-reduced-motion`. Record which mode was exercised; do not claim
cross-browser support based on one engine.

Check the host platform separately when available: some Markdown viewers sanitize
styles or present a static thumbnail. A working browser `img` is useful evidence,
not a guarantee about every repository viewer. Link the downloadable SVG for
direct browser playback. Preserve the requested format.

## Review and handoff

Read the finished diagram visually; a successful parser or geometry validator
cannot establish aesthetic quality. Make focused corrections to actual problems,
then recheck the affected views. Do not regenerate unrelated diagrams or run a
backend build when only the SVG changed.

Report both HTML and SVG paths, what was verified, and any rendering limitation.
Use the repository's diff workflow. For new files, `git diff --no-index -- /dev/null
path/to/file` produces real hunks without staging; exit code 1 means differences.
