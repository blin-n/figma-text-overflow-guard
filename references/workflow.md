# Audit and Repair Workflow

Use this sequence because writes must never precede source protection.

## 1. Establish scope and source safety

1. Resolve the user's current Page, Frame, Component, or Instance selection.
2. Capture enough source metadata to verify later that the original remains unchanged.
3. Analyze the source read-only. Do not alter any source node or source page property.
4. Create a new page named `Text Overflow Guard`. If occupied, use `Text Overflow Guard 02`, then `03`, and so on.
5. Duplicate the complete target into the new page. Preserve hierarchy, components, instances, overrides, styles, constraints, and auto-layout behavior as far as Figma supports.

## 2. Build the four fixed regions

Create clearly separated, labeled regions in this order:

- `00_Source Copy`: immutable before-state copy. Do not modify it after placement.
- `01_Overflow Check`: copy used to show problem highlights, warning markers, and finding IDs. Do not repair its UI layout.
- `02_Optimized`: clean repaired UI only. No diagnostic marks, dimensions, cards, or guides.
- `03_Annotated`: developer-facing repaired copy with only relevant measurements, rules, changes, reasons, and connectors.

Place a Summary above these regions.

## 3. Scan and classify

Scan every text node in scope and inspect the chain from text through its ancestors to the screen/container boundary. For each node:

1. Record font family, weight, style, size, letter spacing, line height, text resize, width/height mode, max lines, overflow/truncation behavior, and effective bounds.
2. Confirm the required font is available before trusting measurements. If not, skip measurement and record `Font unavailable – measurement skipped` with `Manual Review Required`.
3. Determine semantic component context such as button, tab, title, body, table cell, status, tag, number, unit, date, or error.
4. Check all six issue classes: hard overflow, clip overflow, truncation, wrap overflow, compression, and layout-chain overflow.
5. Inspect parents and siblings for expansion, compression, overlap, icon squeeze, edge collision, and screen overflow.
6. Assign a unique sequential ID (`TOG-001`, `TOG-002`, ...), risk level, strategy candidate, and status.

## 4. Multilingual mode

- If real localized strings exist, measure them with their actual fonts and label failures `Confirmed Overflow`.
- If translations do not exist, perform expansion-risk screening only. Label outcomes `Potential Overflow Risk` with Low, Medium, or High confidence/risk. Expansion factors may prioritize checks but are never final measurements.
- Keep confirmed observations and predicted risks separate in the UI and final report.

## 5. Repair only the optimized copy

Apply the decision ladder in [decision-rules.md](decision-rules.md). Before each change, check parent, sibling, screen-edge, maximum-size, component-rule, overlap, and downstream layout effects. If the change would create a broader problem, do not apply it.

For each safe change:

1. Modify only `02_Optimized` and its corresponding `03_Annotated` design copy.
2. Preserve existing visual styles and component relations.
3. Add or update the finding record.
4. Add standardized annotation instances only in `03_Annotated`.

## 6. Mandatory second scan

Rescan `02_Optimized` and verify:

- original source and `00_Source Copy` are unchanged;
- no new text or ancestor overflow exists;
- no overlaps or broken parent/sibling layout exist;
- no unintended instance detach occurred;
- new layout values prefer the 8pt grid without overriding specifications;
- fonts, weights, colors, radii, icons, images, shadows, and styles did not change without cause;
- annotations cover no important UI content.

If validation fails, revert only the unsafe change in the working copies and mark the issue `Manual Review Required`.

## 7. Completion criteria

Every issue ends in exactly one state:

- `Fixed`
- `Potential Risk`
- `Manual Review Required`
- `No Action Needed`

Do not claim completion if the source-safety check or second scan could not be performed.
