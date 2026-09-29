---
name: figma-text-overflow-guard
description: Audit and safely repair text overflow, clipping, truncation, wrapping, compression, and multilingual expansion risks in selected Figma pages, frames, components, or instances. Use for non-destructive Figma text-fit checks, localization readiness, overflow remediation, and annotated developer handoff; never modify the original design.
---

# Figma Text Overflow Guard

Protect the source design while detecting, evaluating, minimally repairing, and documenting text-fit problems in Figma.

## Non-negotiable invariants

- Treat the original selection and its page as read-only. Never modify, move, rename, delete, detach, flatten, or overwrite source nodes, styles, variables, component properties, constraints, or page structure.
- Before any write, duplicate the selected page, frame, component, or instance into a new page named `Text Overflow Guard`; if it exists, use the next two-digit suffix. Do not overwrite prior results.
- Detect before modifying. Make changes only in the duplicated working areas.
- Prefer the smallest necessary change and preserve component/instance relationships. Never detach an instance unless the user explicitly authorizes it.
- Do not invent precise multilingual measurements without real translations. Do not measure with a fallback font when the required font is unavailable.
- Stop automatic repair when safety cannot be guaranteed; record `Manual Review Required` instead.
- Every meaningful modification must have a unique `TOG-NNN` record and standardized annotation.

## Required Figma support

This workflow requires a Figma-capable write tool that can inspect selections, duplicate nodes, create pages, edit copied nodes, and create component instances. Load and follow the environment's Figma write skill before any Figma mutation. If the required tool or selection is unavailable, explain what is missing and do not simulate a completed audit.

## Workflow

1. Read [references/workflow.md](references/workflow.md) before performing an audit or repair.
2. Read [references/decision-rules.md](references/decision-rules.md) when classifying issues, choosing a repair, checking multilingual risk, or deciding whether to stop.
3. Read [references/annotations-and-reporting.md](references/annotations-and-reporting.md) before creating annotations, change records, or the summary.
4. Inspect the current selection without writing to it.
5. Create the protected duplicate structure, scan all text nodes and layout ancestors, then classify findings.
6. Apply only safe, minimal repairs in `02_Optimized`, annotate the corresponding copy in `03_Annotated`, and rescan the result.
7. Report confirmed findings separately from predicted localization risks and list all manual-review items.

## Authorization boundaries

- Do not rewrite product copy unless the user explicitly authorizes semantic copy changes; otherwise recommend copy review.
- Do not modify shared main components, libraries, variables, or tokens. Mark component-level changes for manual review.
- Do not introduce large responsive or interaction changes automatically. When horizontal-to-vertical restructuring, overflow menus, information architecture, or new interaction logic is required, stop and request review.
