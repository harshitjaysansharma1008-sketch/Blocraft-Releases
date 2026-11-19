# Blocraft

Blocraft is a Windows-first, local desktop visual automation studio. This repository is the **working foundation**, not a claim that every item in the long-term product brief is already finished.

**Author:** Harshit Jaysan Sharma<br>
**Project version:** 0.5.0 (development)
<img width="1278" height="761" alt="image" src="https://github.com/user-attachments/assets/8d65ed0d-8633-493f-b1db-0de36df71715" />
<img width="2558" height="1526" alt="image" src="https://github.com/user-attachments/assets/3dfaa762-2077-4c0a-a5bf-2b24ec72bd7b" />
<img width="2558" height="1526" alt="image" src="https://github.com/user-attachments/assets/8c1e19a4-3743-4cba-9017-5372a2a3840b" />
<img width="2556" height="1521" alt="image" src="https://github.com/user-attachments/assets/560594dd-7d2b-4698-9b55-b8cb63255b15" />

**Visual automation made simple.**

Blocraft is a desktop automation application that lets you create workflows visually without writing traditional code.

## Download

Visit the [Releases](../../releases) page to download the latest Windows installer.

## Installation

1. Download `Blocraft-Setup.exe`.
2. Double-click the downloaded file.
3. Follow the installation instructions.
4. Open Blocraft and start automating!

## First workflow

1. Click **Add start**.
2. Open the Actions category in Blockly's toolbox.
3. Drag **open URL** beneath the Start block.
4. Add a Text shadow/value and enter `https://example.com`.
5. Press **Run**. A visible Chromium window will open and navigate.
6. Press **Stop** to cancel a running workflow.

## Selector Support

For a description of each available block and its execution engine, see the [Block Reference](docs/BLOCK_REFERENCE.md). Hover over a block in the toolbox flyout to see its short explanation and important constraints.


## Requirements

- Windows 10 or Windows 11 (64-bit).

## Author

Blocraft is authored by **Harshit Jaysan Sharma**. The project metadata and application About panel use this author credit.

Blocraft supports three selector types:

- CSS selectors: `#search`, `.button`
- XPath selectors: `//input[@name='q']`
- Text selectors: `text=Search`

XPath is detected automatically when no explicit
selector prefix is provided.

Explicit prefixes are also supported:

- `css=`
- `xpath=`
- `text=`

Click and Type Text share the selector resolver.
Text selectors used with Type Text must target
an editable element.

## Desktop automation

Desktop blocks run in a distinct native runtime and control the currently active Windows session. They use RobotJS 0.9.1 (MIT) through a platform adapter; Electron Builder rebuilds and packages its N-API binary. Coordinate Picker runs in the main process and captures using the same native coordinate space as desktop actions. The picker displays monitor positions and sizes. Windows display scaling can make native coordinate units differ from physical panel pixels; use the picker result directly with desktop blocks and verify a target before running a workflow.

Desktop and browser blocks currently run as separate workflow types. Mixed workflows are rejected with a clear error. Desktop automation is user initiated and the Stop control releases held keys and mouse buttons.

## Important scope and honest limitations

This development build does not execute arbitrary user code. Custom blocks are declarative statement blocks and are saved with the project; right-click editing changes only supported presentation settings. View Block Code shows the registered Blockly initializer and current block configuration; it does not generate JavaScript. Desktop automation is Windows-first; future operating systems need their own adapter and permission handling. Element-picker accessible names are useful DOM heuristics rather than a complete accessibility-tree query. Scheduled/event-driven triggers, spreadsheet operations, file writing, broad data utilities, functions, advanced error handling, and general HTTP/API blocks are not yet implemented.

The browser runtime launches a fresh Chromium session for each run. The Element Picker launches its own visible Chromium page and exits when stopped or when Blocraft quits. Persistent authenticated browser profiles are intentionally not enabled by default. Workbook `.xlsx` access, actual Excel desktop interaction, file writing, functions, advanced error handling, and general HTTP/API blocks are not yet implemented.


## Copyright

Copyright © 2026 Harshit Sharma.

Blocraft is proprietary software. Free personal use is permitted. Redistribution or resale requires permission from the copyright owner.
