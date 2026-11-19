# Security notes

- Electron renderer uses `contextIsolation: true`, `nodeIntegration: false`, sandboxing and a restrictive Content Security Policy. No Node APIs are exposed to renderer code.
- IPC handlers reject calls from untrusted frames. Workspace payloads have block-count/depth limits; project files are limited to 10 MB and custom definitions accept only declarative string inputs.
- Desktop actions are allow-listed and validate coordinate ranges, keys, modifiers, text length, wait duration and scroll amount. They run in the main process through a dedicated adapter; held keys/buttons are released on cancellation and failure.
- Desktop automation controls the active OS session. Users explicitly start workflows and can stop them. Review coordinates and text before running.
- Coordinate Picker captures with global shortcuts (`Ctrl+Shift+F8`) and exits with (`Ctrl+Shift+F9`) to avoid sending a click to the target application.
- The Element Picker runs in a separate visible Chromium session and returns bounded, plain element metadata. It does not expose Node APIs to the page.
- Browser automation can visit sites as the user and execute workflows against authenticated pages. The normal browser runtime uses a fresh context without persistent profiles.
- Custom blocks cannot contain JavaScript, extensions, mutators or code generators. Editing supports descriptions and inline layout only.
- Main-process pre-run validation rejects block types without an executor, including declarative custom blocks. Tooltips and the block reference describe current behavior but do not grant additional runtime capabilities.
- Do not add arbitrary `eval`, renderer Node access, unrestricted shell execution or downloaded script execution.
- The production dependency audit is clean. The full development-tool audit still reports recent high-severity `picomatch` and moderate `minimatch` advisories in transitive build dependencies; no compatible upstream fix was available during this release check. Recheck before future builds. Windows multi-DPI hardware verification and a formal browser/network permission UX also remain.
