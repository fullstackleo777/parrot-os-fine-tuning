# VSCodium General Settings

Settings for VSCodium that instantly make my dev flow better.

In VSCodium, open the command palette (Ctrl+Shift+P or Cmd+Shift+P) and type `Preferences: Open Settings (JSON)` and select it. Add the following in the `settings.json` file.

## JSON Settings

```
{
  "workbench.startupEditor": "none"
  "security.workspace.trust.enabled": true,
  "security.workspace.trust.startupPrompt": "never",
  "security.workspace.trust.emptyWindow": "never",
  "security.workspace.trust.untrustedFiles": "open",
  "security.workspace.trust.banner": false,
  "editor.fontSize": 11,
  "editor.wordWrap": "on"
}
```

## What Each Setting Does

| Setting                                   | Value     | What it does                                                                                                                                                                                               |
| ----------------------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `workbench.startupEditor`                 | `"none"`  | Don’t show the Welcome page or a new untitled file at startup. If you reopen a folder/workspace, it just restores your last session; if it’s an empty window, you’ll see the Explorer with no editor open. |
| `security.workspace.trust.enabled`        | `true`    | Enables the Workspace Trust system. Untrusted folders open in **Restricted Mode**, which limits code execution features until you mark the folder as trusted.                                              |
| `security.workspace.trust.startupPrompt`  | `"never"` | Suppresses the “Do you trust this folder?” prompt at startup. Untrusted workspaces will go straight into Restricted Mode without a modal prompt.                                                           |
| `security.workspace.trust.emptyWindow`    | `"never"` | Treats **empty windows** (no folder open) as **untrusted** by default, so they run in Restricted Mode.                                                                                                     |
| `security.workspace.trust.untrustedFiles` | `"open"`  | When you open a single file from an untrusted location, just open it in the current window (in Restricted Mode) without prompting or spawning a new window.                                                |
| `security.workspace.trust.banner`         | `false`   | Hides the yellow “Restricted Mode” banner. You’ll still see status bar indicators and can trust the workspace manually.                                                                                    |
| `editor.fontSize`                         | `11`      | Sets the editor font size to 11 px.                                                                                                                                                                        |
| `editor.wordWrap`                         | `"on"`    | Wraps long lines at the viewport width (no horizontal scrolling for long lines).                                                                                                                           |

## A Note About This Setup

Restricted Mode limits things like running tasks, debugging, some extension activations, and scripts that could execute code from the workspace. This config is tuned for a quiet, low-nag experience while still keeping protections on.

___
