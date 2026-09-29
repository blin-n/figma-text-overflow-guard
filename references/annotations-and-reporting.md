# Annotations and Reporting

## Annotation system

Create or reuse a component set named `Annotation / Text Overflow Guard`. Annotations in `03_Annotated` must be component instances, not ad hoc rectangles, text, arrows, lines, or groups.

Provide the necessary component types:

- `Annotation / Spacing / Horizontal`
- `Annotation / Spacing / Vertical`
- `Annotation / Size / Width`
- `Annotation / Size / Height`
- `Annotation / Rule`
- `Annotation / Change`
- `Annotation / Warning`
- `Annotation / Info`

Use orange as the primary annotation color and white label text. Prefer a 2px measurement line, 8px label radius, 8px/16px label padding, auto layout, Hug Contents, and spacing of 8px, 16px, or 24px. Existing design-system annotation components and tokens take precedence when they provide an equivalent standardized system.

## Measurement annotation visual specification

When annotating component spacing and margins, use dimension lines rather than width-like filled overlays. The purpose is to show the empty distance between two relevant edges.

- Draw a 2px orange measurement line spanning the exact measured distance.
- Add short perpendicular end caps at both measured edges so the endpoints are unambiguous.
- Place the measurement component flush with, or visually attached to, the relevant component edges. Do not leave a disconnected floating measurement.
- Keep the annotation outside important content whenever space permits. A connector may cross empty UI space but must not obscure text, controls, icons, or images.
- The orange value badge contains only the pixel value, for example `24px`, `40px`, or `56px`. Do not add labels such as `gap`, `padding`, component names, explanations, or arrows inside the badge.
- Center the value badge on the measurement line's axis: horizontal measurements share the same X center; vertical measurements share the same Y center.
- Calculate centering from actual rendered bounds after resizing an instance, not from the component master's default dimensions. Validate the badge center against the line center; an unavoidable 0.5px difference caused by odd pixel dimensions is acceptable when visually centered.
- Prefer an orange fill with white text, 8px corner radius, and compact padding. Keep the badge readable without making it visually dominant.

Use horizontal measurement components for left/right margins, horizontal padding, column or card gaps, and inline-item gaps. Use vertical measurement components for top/bottom margins, vertical padding, section spacing, title-to-content spacing, and row gaps.

Do not annotate a component's width merely because a component is present. Add width or height measurements only when size is directly related to the overflow finding or the user explicitly requests component dimensions.

Before completion, inspect every measurement instance and verify:

- its endpoints touch the two edges whose distance is being reported;
- its pixel value equals that edge-to-edge distance;
- its badge is centered on the line axis;
- it represents a margin, padding, or gap rather than the component width, unless size annotation was requested;
- it does not cover important UI content.

## What to annotate

Annotate only:

- parameters changed by this audit;
- dimensions and constraints directly related to overflow;
- critical layout rules and new responsive behavior;
- minimum/maximum constraints;
- implementation rules developers must preserve.

Do not redraw a full engineering specification or repeat unrelated existing dimensions.

Use:

- **Spacing** for padding, gap, margin, and component distance;
- **Size** for width, height, and min/max dimensions;
- **Rule** for implementation constraints such as maximum two lines or auto height;
- **Change** for before → after plus reason;
- **Warning** for current failure, high risk, or manual review;
- **Info** for non-blocking guidance.

## Placement

Never cover text, controls, icons, images, or key information. Place labels in this order of preference: outside the frame, right, above, below, then an internal empty area. Connectors may enter the UI. Prefer 8px, 16px, or 24px distance from the target.

## Finding record schema

Every finding must contain:

```text
ID: TOG-NNN
Component: human-readable node/component name
Issue: issue class and concise symptom
Risk Level: Critical | Warning | Info
Before: relevant original values/behavior
After: relevant repaired values/behavior, or N/A
Constraint: max/min/product/component rule
Strategy: approved strategy label
Grid: 8pt aligned | Existing specification preserved | N/A
Status: Fixed | Potential Risk | Manual Review Required | No Action Needed
Reason: evidence-based explanation
```

Do not represent predicted expansion as a measured failure. Include whether evidence came from actual localized text or expansion-risk analysis.

## Page summary

At the top of the guard page, create a concise summary that includes:

```text
Figma Text Overflow Guard

Scanned Text Nodes       <count>
Confirmed Overflow       <count>
Potential Risks          <count>
Auto Fixed               <count>
Manual Review            <count>

Critical                 <count>
Warning                  <count>
Info                     <count>
```

Counts must reflect the final validated records, not intermediate attempts.

## Final response

Tell the user:

- the exact new page name;
- scope and number of text nodes scanned;
- confirmed, predicted, fixed, and manual-review counts;
- whether original safety and second-scan validation passed;
- the most important unresolved constraints.

Never claim a Figma write or validation occurred when the required tool was unavailable.
