# VSCodium General Settings

Settings for VSCodium that instantly make my dev flow better.

In VSCodium, open the command palette (Ctrl+Shift+P or Cmd+Shift+P) and type `Preferences: Open Settings (JSON)` and select it. Add the following in the `settings.json` file.

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

___
