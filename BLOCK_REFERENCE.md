# Blocraft Block Reference

This reference documents the blocks currently available in the toolbox for Blocraft 0.5.0 development. Hover over a block in Blockly’s flyout for a shorter version of its description. Runtime support is checked before a workflow starts; visual-only custom blocks are rejected with a clear explanation.

## How execution works

Every workflow begins at **when I press Run**. Connect statement blocks below the start block. Value blocks (rounded reporters) plug into compatible inputs. A missing required value, invalid conversion, invalid list index, selector timeout, or unsupported block is reported in the execution log. Browser workflows use Playwright; desktop workflows use the Windows desktop adapter. Browser and desktop action blocks cannot be mixed in one workflow.

Pure data, value, comparison, and loop operations are intended to have the same behavior in both interpreters. File reading is performed in the main process. A run gets an in-memory variable map; variables are not automatically saved as project data.

## Triggers and browser actions

| Block | What it does | Notes |
|---|---|---|
| When I press Run | Starts the connected statement chain when the user presses Run. | At least one start block is required. |
| Open URL | Navigates the active browser page. | Requires a complete `http://` or `https://` address. |
| Open a new tab | Creates a browser tab and makes it active for subsequent actions. | Browser runtime only. |
| Click element | Clicks the first selector match. | CSS is the default; supports `xpath=` and `text=` prefixes. |
| Type text into element | Fills the first matching editable element. | Uses the same selector rules as Click. |
| Press key | Sends a key press to the active browser page. | Uses Playwright key names such as `Enter`. |
| Scroll page | Scrolls vertically by a pixel amount. | Positive is down; negative is up. |
| Wait | Pauses for a number of seconds. | Cancellable; capped at 300 seconds. |
| Close current tab | Closes the active page. | A replacement page is created if it was the last tab. |
| Read text from page element | Reads an element’s visible text into a variable. | An empty selector reads the page body. |
| Write to log | Writes a value into the execution log. | Useful for inspecting intermediate results. |

## Desktop actions

Desktop blocks control the currently active Windows session. Keep the target application focused and review coordinates and text before running. The Coordinate Picker reports coordinates in the same native screen coordinate space used by these blocks.

| Blocks | Behavior and limits |
|---|---|
| Click, double-click, right-click | Sends the corresponding mouse click at absolute X/Y coordinates. |
| Click and hold | Holds the left button for a duration and releases it afterward. |
| Mouse down / mouse up | Presses or releases a selected mouse button. Runtime cleanup releases held inputs after stop or error. |
| Move pointer | Moves without clicking. |
| Drag | Holds the left button while moving between two coordinate pairs. |
| Desktop scroll | Scrolls at a coordinate by a bounded integer amount. |
| Desktop type | Types text into the focused application, optionally with an inter-character delay. |
| Desktop press key, key down, key up | Sends supported keyboard input to the focused application. |
| Desktop shortcut | Sends a key with modifiers, for example `control+shift` plus `s`. |
| Desktop wait | Waits for up to 300 seconds and can be stopped. |

Desktop actions run only in the desktop workflow engine. Coordinate calibration across every Windows DPI/scaling configuration has not been certified.

## Variables and data

| Block | Behavior |
|---|---|
| Set variable | Creates or replaces a named in-memory value. |
| Get variable | Reads a named value; an unset name reads as empty text. |
| Delete variable / clear all variables | Removes one or all runtime values. |
| Increment / decrement | Adds or subtracts a finite number. An unset variable starts at zero; a nonnumeric existing value is an error. |
| Append to variable | Appends to an existing list or concatenates onto existing text. |
| Variable exists? / type of variable | Reports whether a value is initialized and its type label. |
| Rename variable | Moves a runtime value to another name if the source exists and the target is unused. It does not rename block references elsewhere in the workspace. |
| Read file | Reads UTF-8 text, JSON, or CSV into a named variable. Input is capped at 10 MB; CSV cells are returned as strings. |
| To text | Converts scalar values to text and serializes objects/lists as JSON. |
| To number / to integer / to Boolean | Strict conversions; invalid numeric, fractional integer, or unrecognized Boolean inputs produce errors. Boolean text accepts true/false, yes/no, and 1/0. |
| Type of value / is type | Reports or checks runtime types. The established object label is `object`; the type predicate displays it as “dictionary.” |
| Is empty / is null | Checks empty text/list/object or null/undefined respectively. Zero and false are not empty. |

Text and number literal blocks support `{variableName}` interpolation. An unknown interpolation name is an error.

## Lists and collections

Create Empty List initializes a named variable with `[]`. Create List Variable initializes a two-item list. List Variable returns a named list, and Add to List Variable appends directly to it. The generic list reporter creates a two-item temporary list.

| Block | Behavior and limits |
|---|---|
| Add item | Appends to a connected list in place. Use a List Variable reporter when the result must persist. |
| Get item | Reads a zero-based index; 0 is the first item. Out-of-range and negative indexes are errors. |
| Set item | Replaces an existing zero-based index. |
| Remove item at index | Removes one item at an existing zero-based index. |
| Contains item | Checks membership; nested lists and objects are compared structurally. |
| Clear list | Removes all items from the connected list in place. |
| Copy list | Returns a shallow copy; nested values remain shared references during the run. |
| List length | Returns the number of items. |

List mutation blocks operate on the connected list object. A list built temporarily inside an expression may not be available to later blocks; use a named list variable for persistent state.

## Dictionaries, nested data, and JSON

Create Empty Dictionary returns a new object. Store it in a variable to reuse it. Dictionary get/set/delete/has-key/keys/length operate on object values; mutation blocks update the connected object. Nested Get/Set accepts paths such as `users[0].name` and `settings["theme-name"]`. Parent containers must already exist for Set, and list indexes cannot skip past the current list length.

Parse JSON converts valid JSON text to its runtime value; malformed JSON reports an error. Is Valid JSON returns a Boolean instead. Objects/lists are ordinary serializable runtime values. Full JSON key/path manipulation, advanced dictionary iterators, and richer collection operations are still in development.

## Conditions and loops

| Block | Behavior and limits |
|---|---|
| If / Else If / Else | Executes the body for the first true branch, otherwise the optional ELSE body. Blockly’s mutation controls add/remove branches. |
| Compare | Equality is strict. Ordered comparisons require both values to be numbers or both to be text. |
| AND / OR / NOT / XOR | Boolean operators require Boolean inputs; conversion is explicit. |
| Repeat | Runs its body a requested number of times, capped at 1,000. |
| For | Inclusive numeric range with a nonzero step; maximum 10,000 iterations. |
| While / repeat until | Checks a Boolean condition and stops at a 10,000-iteration safety limit. |
| For each | Iterates a list snapshot and provides scoped item/index variables. Prior values are restored when the loop exits. |
| Break / continue | Controls the nearest enclosing loop. |

## Custom blocks and unavailable features

Custom block definitions are safe declarative statement blocks with limited string inputs. They have independent IDs and can be stored with a project, but they do **not** execute user code. The runtime rejects them before execution. Excel, broad text/math/date utilities, general file writing, sub-workflows, step debugging, and full run history are not implemented yet. See [FEATURES.md](FEATURES.md) for current status.
