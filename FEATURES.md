# Blocraft Feature Inventory

Last updated: 2026-11-19

Blocraft 0.5.0 is a development build authored by Harshit Jaysan Sharma. This inventory distinguishes available runtime behavior from the larger roadmap; a toolbox block is not proof that an unfinished feature is implemented.

## Existing features and 0.3.0 development additions

### 0.4.0 development additions
- Inclusive numeric For Loop with configurable start/end/step and a named current-value variable.
- While Loop, Repeat Until, For Each over arrays with item/index variables, Break Loop and Continue Loop. Nested loops are supported; open-ended loops stop at a 10,000 iteration safety limit.
- Shared browser/desktop variables now include set/get, delete, increment/decrement, append, existence/type checks, rename and clear. Text and numeric literal blocks interpolate `{variable}` references.
- Canvas zoom, center and selected-block deletion controls use Lucide icons; the Help/About version reads Electron's application version.
- The unused duplicate left rail was removed; users access blocks through Blockly's categorized toolbox.

### 0.5.0 development additions
- Custom declarative Blockly definitions receive independent permanent type IDs, avoiding collisions when display names repeat.
- Lists & Collections includes named list initialization and actual cross-runtime add/get/set/remove/clear/copy/length/contains operations.
- Shared strict conversion and comparison helpers are used by both runtimes; integer/Boolean conversion, type/empty/null/XOR reporters, nested dictionary access, and JSON validity checks are registered in both engines.
- IF execution selects the first true mutation branch, and numeric/list loop bindings restore prior variables when their scope exits.
- Available Blockly blocks include descriptive hover tooltips, with inputs, behavior, edge cases, and runtime constraints; the detailed [Block Reference](BLOCK_REFERENCE.md) covers the same current subset.
- Unsupported blocks, including declarative-only custom blocks, are rejected before execution with a block-specific error.
- Search displays matching block definitions in a search-results flyout.

### Desktop and Browser Automation
- Separate Playwright browser and native desktop runtimes.
- Windows native mouse move/click/double-click/right-click, drag, scroll, click-hold and mouse button down/up.
- Desktop key press, key down/up, shortcuts, text typing with delay and cancellable waits.
- Stop aborts desktop/browser operations and releases held desktop input state.
- Browser coordinate actions are rejected as mixed-engine workflows rather than silently routed.
- Coordinate Picker global hotkeys, native pointer position, capture, copy and selected-block insertion.
- Browser Element Picker with hover highlighting, tag/attributes/text, accessible-name heuristics, CSS and XPath capture, copy and insertion.

### Visual Programming
- Start, browser action, desktop action, logic, repeat, variable, conversion/type detection, math, string join/contains and list create/add/get/length blocks.
- Read bounded UTF-8 text, JSON and CSV files into variables from either runtime; parse JSON values; delete variables; read browser element/page text into variables.
- Native block context menu plus registered Blockly initializer/configuration inspection.
- Safe block settings editor for description and inline input layout; project-persisted custom declarative definitions.
- Shared two-second category help descriptions.
- Built-in Help pages for workflow, block reference, shortcuts, browser and desktop guides, pickers, custom blocks, troubleshooting and version.

### Security and Projects
- Renderer sandboxing, isolation, disabled Node integration, CSP and main-frame IPC sender checks.
- Workspace count/depth bounds and 10 MB project size cap.
- Custom block schema validation and no arbitrary JavaScript execution.
- Local `.blocraft` project save/open.

### Packaging
- Windows x64 NSIS installer and unpacked app build through electron-builder.
- RobotJS native dependency rebuild and ASAR-unpacked packaging verified for Electron 45.

## Not yet included in the current development work

- Excel workbook (`.xlsx`) operations, actual Excel desktop interaction, file writing/folder operations, generic HTTP/API blocks, functions and structured try/catch blocks.
- Persistent authenticated browser profiles, workflow migration framework, extensions, scheduled/hotkey triggers and application launch triggers.
- Mixed browser-plus-desktop workflows.
- Complete accessibility-tree names in Element Picker (current names are DOM heuristics).
- Scheduled/event-driven triggers, HTTP/API blocks, spreadsheet workbooks, file writing and the broader data manipulation catalog from the 0.4.0 brief.
- Verified coordinate calibration across all Windows per-monitor DPI combinations.
- macOS/Linux adapter implementations.

## Verification rule

Only mark a feature implemented when code and tests/build evidence support it. Windows per-monitor DPI behavior still needs physical-device verification before advertising exact physical-pixel calibration.
