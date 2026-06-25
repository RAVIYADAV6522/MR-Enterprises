# Short-Screen Tab Layout Design

## Goal

Fix the current short-screen and mobile behavior so that content cards visible in desktop tab views remain reachable and visually well-packed on smaller or shorter screens. Remove the large blank areas that appear below section content while preserving the current desktop interaction model.

## Current Problem

The site uses a horizontal full-height panel shell. On desktop this works because each panel has enough viewport height to display its intended composition. On short-height or mobile screens, several panels show only part of the content while still leaving unused vertical space. The `Products` tab is the clearest example: desktop shows three cards at once, while mobile shows a single card with a large empty region below it.

## Scope

In scope:

- Responsive behavior for all main tabs: `home`, `about`, `products`, `industries`, `pricing`, and `contact`
- Height-based responsive adjustments in addition to existing width-based mobile rules
- Grid and spacing changes that let section content stack naturally on smaller screens
- Preserving the fixed header, bottom tab strip, and desktop horizontal tab experience

Out of scope:

- Rewriting the site into a framework
- Changing business content or section order
- Redesigning desktop layout behavior
- Removing the horizontal panel model on desktop

## Recommended Approach

Use a short-screen responsive mode driven by viewport height, with support from existing width-based mobile rules. In this mode, each panel should favor natural content flow over desktop-like composition density.

The core idea is:

- Keep the horizontal shell and tab navigation intact
- Keep each panel vertically scrollable
- Reduce dead vertical space caused by desktop spacing assumptions
- Collapse multi-column grids and card groups into single-column stacks where needed
- Ensure bottom padding accounts for the fixed tab strip and floating WhatsApp button so the last visible card is not obstructed

This keeps the current design language and navigation structure while making each tab practical on short screens.

## Layout Behavior By Area

### Global panel behavior

For short screens:

- Panels remain horizontally addressable by tabs and arrows where enabled
- Panel content should scroll vertically without clipping
- Top and bottom spacing should be reduced where current padding creates unused visual gaps
- Section containers should avoid trying to visually center content inside the viewport height

### Home

- Keep the hero readable, but reduce excess vertical padding and decorative spacing on short screens
- Preserve CTA visibility without forcing the hero to consume unnecessary height
- Allow trust content to sit closer to the hero content so blank space is reduced

### About

- Convert any side-by-side presentation to a stacked flow earlier on short screens
- Keep text and stat cards in one vertical reading order
- Reduce gaps between heading, body copy, badges, and stat boxes

### Products

- Collapse the products grid to a single-column stack on short screens
- Keep all product cards in natural document order
- Preserve the specifications table, but ensure its spacing follows immediately after the cards instead of leaving a large gap

### Industries

- Keep the counter section visible near the top of the panel
- Stack industry cards in a way that uses available height efficiently
- Reduce the gap between counters and industry cards on short screens

### Pricing and Gallery

- Stack pricing cards vertically where required by viewport size
- Keep the gallery directly below pricing with tighter section spacing
- Avoid large blank areas between the pricing section and gallery section

### Contact

- Stack contact information and action buttons vertically
- Ensure the final CTA and footer remain reachable above the fixed bottom controls

## Responsive Triggers

Use a new height-based breakpoint for “short screen” behavior. The implementation may tune the exact threshold, but the logic should be based on viewport height rather than width alone. Width-based breakpoints should remain in place and continue to handle narrow mobile screens.

The intent is:

- Width rules handle narrow layouts
- Height rules handle short layouts
- The combination determines when sections need tighter spacing and vertical stacking

## Component-Level Changes

- Reduce section padding for short screens
- Reduce card padding where necessary, without harming readability
- Reduce heading-to-content spacing
- Collapse multi-column grids to one column sooner when height is constrained
- Ensure fixed UI overlays are accounted for in panel bottom padding
- Preserve card order and content hierarchy

## Error Handling and Edge Cases

- No section should rely on a specific minimum viewport height to remain usable
- The last item in a panel must not be hidden behind the bottom tab strip
- Floating WhatsApp UI must not cover critical content near the panel bottom
- Hash navigation and tab switching must continue to land the user in the correct panel
- Decorative effects must not force content clipping or create extra non-scrollable space

## Testing Strategy

Verify behavior manually across representative viewport classes:

- Desktop with normal height
- Tablet portrait
- Narrow mobile with moderate height
- Narrow mobile with short height
- Short laptop viewport

For each tab, confirm:

- All major cards/blocks are reachable
- No large empty vertical gap remains below the main content when additional content should still be visible
- The last card or CTA is not hidden by fixed controls
- Tab navigation still works as before
- Desktop layout remains unchanged

## Implementation Notes

This change should be implemented as targeted CSS adjustments, with minimal JavaScript impact. The existing panel navigation model is already correct; the main issue is responsive layout behavior and spacing inside panels.

## Success Criteria

- Every main tab uses short mobile screens efficiently
- Content that appears as multiple cards on desktop becomes a readable vertical stack on short screens
- Large blank zones below content are removed or materially reduced
- Users can scroll naturally within each panel to reach all content
- Desktop behavior remains visually unchanged
