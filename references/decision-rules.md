# Detection and Repair Decision Rules

## Issue classes and severity

- **Hard Overflow — Critical:** rendered text extends beyond its container.
- **Clip Overflow — Critical:** clip content, a frame, mask, or hidden overflow conceals text.
- **Truncation:** ellipsis or system truncation. Escalate when meaning, an action, a number, or a critical state is lost.
- **Wrap Overflow:** actual lines exceed the permitted maximum even if auto-height prevents clipping.
- **Compression — Warning:** content technically fits but safe padding or spacing has collapsed.
- **Layout Chain Overflow:** text expands a child or ancestor and causes sibling squeeze, parent overflow, overlap, edge collision, or icon compression. Use Critical when layout or meaning is already broken.

Use `Info` for non-blocking recommendations, `Warning` for likely or emerging failure, and `Critical` for actual overflow, lost critical meaning, or broken layout.

## Semantic defaults

| Context | Default behavior |
|---|---|
| Button / primary action | Show fully; avoid truncation and wrapping by default |
| Tab / navigation / status | Prefer one line |
| Menu item | One line or safe width increase |
| Card or dialog title | One to two lines |
| Body / description / tooltip / error | Prefer complete content and auto height |
| Table cell | Ellipsis only with access to the full value |
| Tag | Hug contents |
| Input placeholder | Truncation may be acceptable |
| Device/file/long name | Truncation only when product semantics allow it |
| Person name | Truncate cautiously |
| Number | Never truncate |
| Number + unit | Never split the pair across lines |
| Time | Normally keep intact |
| Date | Prefer complete display |

Treat explicit product rules as authoritative over these defaults.

## Repair ladder

Stop at the first safe solution:

1. **No external size change:** correct Hug/Fill, auto layout, text resize, width/height mode, wrap, parent sizing, alignment, or constraints.
2. **Minimal component resize:** increase width or height only as much as necessary and within max constraints.
3. **Padding or gap adjustment:** preserve touch target, hierarchy, component specifications, and comfortable spacing; never crush padding merely to force a fit.
4. **Wrap:** only for semantics that allow multiple lines. Avoid by default for buttons, tabs, status, tags, time, and numbers.
5. **Truncate:** only when meaning remains accessible and product rules permit it. Avoid for primary/destructive actions, error messages, and critical status.
6. **Rewrite:** recommend `Copy review recommended`; do not change meaning without explicit authorization.
7. **Responsive restructuring:** consider only after lower-impact options fail. Mark substantial layout or interaction changes `Manual Review Recommended` / `Manual Review Required` rather than applying automatically.

Use one strategy label per action: `Auto Layout`, `Resize`, `Wrap`, `Truncate`, `Rewrite`, `Responsive`, `Padding Adjustment`, `Gap Adjustment`, `Constraint Update`, or `Manual Review`.

## 8pt grid and precedence

For newly introduced layout dimensions, prefer multiples of 8 for width, height, padding, gap, margin, card spacing, button sizing, and container spacing. Round a measured 193px need to a safe 200px only when no higher-priority rule conflicts.

Never force font size, line height, stroke width, existing radius, existing icon size, tokens, or explicit component/product dimensions onto the grid. Precedence is:

1. Explicit product specification
2. Existing design token
3. Existing component rule
4. Existing layout rule
5. 8pt grid

## Automatic-repair stop conditions

Report without modifying when any of these applies:

- font is unavailable or precise sizing requires missing translations;
- shared main component, global library, variable, token, or multiple pages must change;
- product meaning, information architecture, or interaction logic must change;
- nested component behavior cannot be changed safely;
- a button or parent has reached its maximum or the screen edge;
- the repair causes new overflow, overlap, sibling compression, or major visual restructuring;
- product/component constraints conflict and no safe local override exists.

Use the exact status `Manual Review Required` and explain the blocking constraint.
