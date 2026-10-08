---
name: clarity-first-ui
description: Design, refactor, or critique application and website interfaces using visual hierarchy, layout, typography, color, and component composition. Use for new screens, improving existing UI, screenshot reviews, and design tokens while respecting the product's branding and existing design system.
license: MIT
---

# Clarity First UI

Make the user's task, key information, and next action clear. Apply the decisions relevant to the request using the host project's tools and conventions. This file contains the complete skill.

## Workflow

1. Identify the audience, concrete task, real content, primary action, and requested deliverable. Inspect the relevant interface, implementation, tokens, fonts, and components. Preserve valid behavior and established branding.
2. Diagnose the largest obstacle to understanding or completing the task. Work through structure, grouping, dimensions, typography, color, and finishing details as needed. For new work, design a representative feature before committing to its navigation shell. Grayscale or a rough sketch can expose weak hierarchy when useful.
3. Produce the requested result. A refactor should make the smallest coherent improvement within scope; a critique should return prioritized findings without editing the project. Token guidance should map choices to meaningful roles. A narrow styling request should stay narrow.
4. Inspect the result with realistic content and relevant states. Report concrete changes or findings, why they help, what was checked, and any limits on verification.

## Hierarchy

- Rank information as primary, supporting, or tertiary according to the task. Use weight, contrast, placement, spacing, and visual area as well as size. If increasing the main element's emphasis stops helping, quiet its competitors.
- Give the next useful action the strongest treatment within its task area. Use quieter treatments for secondary and tertiary actions. Communicate destructive consequences through wording and behavior; destructive meaning alone does not make an action primary.
- Make important values easy to scan. Subordinate, combine, or omit displayed-data labels only when context, format, units, and meaning stay unambiguous. Specifications may need prominent labels. Keep persistent form labels.
- Maintain semantic heading levels independently of visible size. An application title can be modest while the working content leads; an article headline may appropriately dominate.
- Balance dense icons, dark borders, and large colored surfaces against nearby text. Reduce their weight when they become unintended focal points.
- Choose supporting text for its actual surface. A related opaque tint can work better than generic grey on a colored panel; measure its contrast rather than assuming the tint is readable.

## Layout and spacing

- Make related items closer than separate groups. Attach headings to following content and labels to their fields; keep wrapped list items distinct. Try spacing and alignment before adding separators or containers.
- Explore enough whitespace to reveal structure, then choose density deliberately. Compact dashboards can support frequent comparisons; reading pages need comfortable rhythm. Extra whitespace is useful only when it improves the task.
- Let content determine width. Constrain simple forms and prose; allocate room to tables and comparisons. Use intrinsic, fixed, flexible, and maximum widths purposefully. CSS Grid is suitable when its relationships fit the content; indiscriminate percentage widths can waste or crush space.
- Adjust headings, body text, controls, and padding independently as the viewport changes. Preserve reading order and essential information when columns stack. Inspect intermediate widths as well as narrow and wide layouts.
- Reuse a finite spacing and sizing system. Compare neighboring sensible tokens instead of making arbitrary pixel tweaks. Introduce a token when it serves a repeatable distinction.
- Optional source spacing example, in px: `4, 8, 12, 16, 24, 32, 48, 64, 96, 128, 192, 256, 384, 512, 640, 768`. Map useful relationships into the host system rather than replacing its scale.

## Typography

- Select fonts at their intended size. Small UI text needs distinguishable characters and clear labels and numerals; display character should serve headlines. Keep existing brand fonts when they suit the task.
- Use a restrained scale and assign sizes to roles. Optional source example, in px: `12, 14, 16, 18, 20, 24, 30, 36, 48, 60, 72`. Font metrics and readability determine whether a size is useful.
- Give sustained prose its own maximum measure. Approximately 45–75 characters per line is a starting heuristic for the source's Latin-script examples. Tune line-height with width, size, font, and script; longer lines and smaller text often need more leading than large headings.
- Align prose to the writing direction's start; reserve centering for short blocks. Try baseline alignment for related mixed-size text. Align comparable numbers consistently, preserving units and meaningful precision; use tabular numerals where suitable.
- Keep inline links discoverable without requiring hover. Start with the font's designed tracking; adjust it for a specific optical need, such as a large heading or short uppercase label.
- Check required scripts, actual weights, fallback behavior, current availability, and licensing when introducing a font. Historical specimen labels and weight counts are suggestions to verify, not production specifications.

Source font examples by role:

- **Headlines:** Proxima Nova, Freight Sans, Futura, Harmonia Sans, Graphik, FF Meta Serif, Roboto, Jubilat, Interstate, Neue Plak, Adelle.
- **Articles:** Freight Text, Source Sans, Open Sans, Merriweather, Proxima Nova, Franklin Gothic, Camphor.
- **Applications:** Proxima Nova, Inter UI, Roboto, Graphik, Source Sans, Lato, Avenir Next, Open Sans, Aktiv Grotesk, Benton Sans, Soleil, Camphor, Neue Plak Text, Effra.

These are historical examples to compare in context, not automatic font replacements.

## Color

- Establish roles before choosing hues: neutrals for surfaces, text, and borders; a primary family for actions and emphasis; supporting colors for status or categories that need distinction.
- Use a finite ramp with pale surface shades, functional middle shades, and dark text shades. Test a new ramp in actual components before retaining it. Shade numbers identify positions within a family; they do not certify contrast or semantic meaning.
- Tune hue and saturation as well as lightness when a ramp looks washed out or disconnected. Keep neutral temperature coherent instead of unintentionally mixing warm and cool greys. HSL is a useful teaching model; its lightness is neither perceived brightness nor a contrast calculation.
- Use restrained accents and readable text/surface pairs. A quiet status badge can pair dark colored text with a pale tint. Communicate status, selection, and categories through labels, symbols, or other suitable cues alongside hue.
- Distinguish brand colors from semantic roles even when they share a hue. Keep charts and category colors from competing with ordinary interface actions.
- Design dark-theme roles directly and check actual pairs. Mechanical inversion of a light palette can lose hierarchy and contrast.

## Component choices

Choose a pattern for its task and content. Alignment and whitespace may make containment unnecessary. Reuse host components and preserve their interaction contracts when changing appearance.

| Components | Decision guidance |
| --- | --- |
| Buttons, inputs, input groups | Rank actions; retain visible labels and instructions. Combine a field and action when they form one operation, stacking them when space is insufficient. |
| Validation, alerts, badges | Keep errors close to affected fields with a specific correction; use a summary when needed. Preserve entered work. Use alerts for consequential feedback and badges for concise status. |
| Breadcrumbs, pagination | Breadcrumbs express ancestry, while step indicators express process. Choose pagination for deliberate jumping or returning; incremental loading for discovery. Preserve position and handle loading failure. |
| Horizontal, vertical, and header navigation | Distinguish destinations from local tab panels. Make current location or selection clear independently of keyboard focus; maintain usable overflow or collapse. |
| Tables | Use clear headers and comparable values. Compose related supporting facts within a cell when useful, preserving separate columns for independent sorting or comparison. Local horizontal scrolling can preserve comparison better than converting every row to a card. |
| Sign-in, multi-section forms | Keep the entry task clear and group fields by meaning. Place explanations beside controls when space supports it, above them when stacked. Preserve authentication choices, save scope, and expected behavior. |
| Modals | Reserve interruption for a focused task. Establish a clear title, content, primary action, and dismissal. Preserve accessible focus handling and background isolation. |
| Pricing, checkout | Compare meaningful plan differences with cards or a comparison table. Keep order details, totals, terms, and commitment clear in checkout. Use actual product terms and supported actions. |
| Marketing heroes, sections, testimonials | Lead with one clear promise and action; organize supporting explanation and proof. Retain substantive testimonial evidence and attribution. |
| Preview cards, profile cards | Let the decisive cue lead: identity, title, image, price, or metadata. Provide predictable interactions and accommodate missing images and long names. |
| Application layouts, footers, activity feeds | Arrange navigation, working content, and contextual panes by task relationships. Group footer destinations. Make feed actor, event, and time scannable without oversized metadata. |

## Screen patterns

- **Dashboard:** identify important comparisons → rank summary values and recent items → choose table facts and density → clarify status and actions → adapt narrow layouts while preserving units, essential facts, and comparison needs.
- **Content:** establish message and reading order → constrain prose measure and paragraph rhythm → distinguish headline, facts, and body → refine lists, quotes, and attribution → clarify the action → adjust typography and stacking.
- **Complex settings:** group semantic sections → relate explanations to controls → size fields for expected values → make choices comparable → group summaries and update actions → preserve save/cancel scope and verify stacked layouts. Single-choice cards retain radio behavior and explicit selected state.

## Depth, imagery, and finishing

- Use elevation to explain layering and affordances. Keep a coherent light direction and a small shadow system. Broad cast shadows suggest lift; tighter contact shadows anchor an edge. Surface contrast or flat offsets can express depth when realistic shadows do not suit the product.
- Overlap elements when it clarifies a relationship; preserve reading order, target access, and focus visibility. Use surface-colored boundaries to separate overlapping images.
- Design with representative images early. Use intentional ratios and crops for unpredictable uploads while preserving the meaningful subject. Check text over actual images and responsive crops; use an overlay or controlled surface when necessary.
- Inspect icons, logos, and screenshots at displayed size. Vector sharpness does not ensure appropriate optical weight. Choose simplified small-size marks; crop or simplify screenshots rather than shrinking meaningful text until it is unreadable.
- Prefer the host's icon system. If using supplied two-tone SVGs, scope their primary/secondary fill classes to avoid collisions. Give meaningful icons appropriate names and keep decorative icons out of redundant announcements. Use authorized assets under their actual terms.
- Polish existing structure with purposeful icons, small accents, or restrained surface variation after hierarchy works. Explain empty states and give a useful next action. Observe the mechanism behind a strong design reference and test an original adaptation in context.

## Verification

The following production checks supplement the source's visual design advice. Apply them to the changed feature, using [WCAG 2.2](https://www.w3.org/TR/WCAG22/) and relevant [ARIA interaction patterns](https://www.w3.org/WAI/ARIA/apg/patterns/) when precise requirements are needed.

- Inspect rendered output at representative widths and with long, missing, numerous, and zero-result content. Preserve essential information, grouping, readable text, useful targets, and local overflow where appropriate.
- Check relevant hover, focus, selected, disabled/read-only, loading/submitting, success, and error states. Distinguish first use, no search results, failed loading, and restricted access so the explanation and next step match the cause.
- Verify meaningful headings, associated labels, link/button semantics, keyboard operation, and visible focus. Keep selection distinct from focus. A modal needs suitable initial focus, a contained tab sequence, dismissal, inert background content, and appropriate focus restoration.
- Measure actual text/surface and necessary control contrast. Check image-backed text across crops. Test relevant text resizing, reflow, localization, writing direction, and reduced-motion behavior using the host's conventions.
- Run appropriate project checks for implementation changes. A screenshot supports visual findings; it does not establish keyboard behavior, semantics, or persistence. State unavailable checks rather than claiming they passed.

For a critique, give each finding an observable symptom, effect on the task, and concrete correction. Finish when the requested deliverable is complete and the relevant checks have run or their limits are explicit.

## Source basis

Original synthesis of the supplied `Refactoring UI v1.0.2.pdf` (foundations/hierarchy/layout pp. 6–86; typography pp. 87–117; color pp. 118–148; depth/imagery/finishing pp. 149–214; observation and experimentation pp. 215–218), `Component Gallery v1.1.0.pdf`, `Font Recommendations v1.0.1.pdf`, `Color Palettes v1.1.0.pdf`, and icon conventions. Font and palette add-ons inform selection guidance; exhaustive specimen annotations and hexadecimal catalogs are omitted to keep this a single practical skill.

Dashboard, content, and complex-form workflows reflect sampled visual sequences from the supplied videos; narration was not reviewed. This is not an official Refactoring UI product. The skill operates without the source package and includes no purchased PDFs, videos, fonts, or icon geometry.
