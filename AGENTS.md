
# Blocraft - AI Development Instructions

## 1. Project Identity

Blocraft is a local-first visual desktop automation
studio built with Electron, React, TypeScript,
Blockly and Playwright.

It allows users to create visual automation
workflows using drag-and-drop programming blocks.

The long-term goal is to build an extensible
automation platform with browser automation,
desktop automation, custom blocks and extensions.

## 2. Technology Stack

- Electron: Desktop application framework
- React: User interface
- TypeScript: Application language
- Blockly: Visual programming workspace
- Playwright: Browser automation engine
- Vite: Renderer development and bundling
- Zustand: UI state management
- Lucide React: Icons
- Electron Builder: Windows installer packaging
- Vitest: Automated testing

## 3. Project Structure

src/main/
- Electron main process.
- Owns application lifecycle and IPC handlers.

src/main/index.ts
- Creates the Electron window.
- Registers IPC handlers.
- Initializes automation runtime.

src/main/runtime/
- Contains browser automation execution logic.
- automation-runtime.ts interprets Blockly workspace
  data and executes actions using Playwright.

src/main/storage/
- Handles local project persistence.

src/main/extensions/
- Reserved for the future extension system.

src/preload/
- Exposes the secure IPC bridge to the renderer.
- Keep exposed APIs explicit and restricted.

src/renderer/
- React renderer application.

src/renderer/blocks/
- Blockly block definitions and toolbox registration.
- register.ts contains native block definitions.

src/renderer/ui/
- Main interface components.
- App.tsx is the primary application UI.
- CodePanel.tsx displays workspace information.
- LogPanel.tsx displays automation execution logs.
- store.ts manages UI state.

docs/
- Technical documentation.

## 4. Important Architecture Rules

1. Keep Electron main and renderer processes separate.
2. Never enable nodeIntegration in the renderer.
3. Keep contextIsolation enabled.
4. Use preload for controlled IPC communication.
5. Browser automation belongs in the main process.
6. Keep Blockly definitions separate from execution logic.
7. Avoid unnecessary dependencies.
8. Preserve existing application functionality.
9. Do not rewrite entire files when a small change is sufficient.
10. Do not introduce breaking changes without explaining them.

## 5. Adding New Blockly Blocks

When adding a native block:

1. Define the block in:
   src/renderer/blocks/register.ts

2. Register it in the appropriate toolbox category.

3. Add its execution handler in:
   src/main/runtime/automation-runtime.ts

4. Add or update automated tests.

5. Update:
   docs/ADDING_BLOCKS.md

6. Update:
   docs/FEATURES.md

A block must not be considered implemented merely
because it appears in the Blockly toolbox.
It needs a working runtime implementation.

## 6. Automation Runtime

The runtime interprets serialized Blockly workspace
data and executes supported blocks.

Existing actions include:

- Open URL
- New tab
- Click element
- Type text
- Press key
- Scroll page
- Wait
- Close tab
- Set variable
- Log output

Existing logic includes:

- If condition
- Repeat loop
- Value reporters
- Basic comparisons
- Text and number conversion

The selector resolver should support:

- CSS selectors
- XPath selectors
- Text selectors

Automatic selector detection should be preserved.

Do not silently remove existing actions or logic.

## 7. Development Commands

Install dependencies:

npm install

Install Playwright Chromium:

npx playwright install chromium

Start development:

npm run dev

TypeScript check:

npm run typecheck

Run tests:

npm test

Build Windows installer:

npm run build

Build unpacked Windows application:

npm run build:dir

## 8. Development Rules

Before making changes:

1. Inspect the existing implementation.
2. Read relevant documentation.
3. Identify all affected files.
4. Preserve existing functionality.
5. Explain the proposed changes.

After making changes:

1. Run TypeScript checks.
2. Run relevant tests.
3. Build the application when appropriate.
4. Report modified files.
5. Explain any known limitations.

Never claim a feature works without verifying
its implementation or test results.

## 9. Documentation Rules

Whenever functionality changes:

- Update README.md if user-facing behavior changes.
- Update ARCHITECTURE.md if architecture changes.
- Update ADDING_BLOCKS.md if block development changes.
- Update FEATURES.md when implementation status changes.
- Update CHANGELOG.md for meaningful releases.

Keep documentation consistent with actual source code.

Do not document planned features as completed.

## 10. Product Development Philosophy

Blocraft should be:

- Modular
- Extensible
- Maintainable
- Local-first
- Secure
- User-friendly
- Reliable

Prioritize stable architecture over quick hacks.

Avoid unnecessary rewrites.

Preserve backwards compatibility with existing
Blocraft projects wherever possible.
