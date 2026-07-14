# Stop VSCodium Trust Prompt on Folder Open

In order to keep VSCodium sandboxed (restricted mode) without prompting to "Trust this folder?" whenever you open a folder, edit your VSCodium settings.json file.

## Step-by-Step: Keep Restricted Mode, Disable Trust Prompt

1. **Open VSCodium Settings (JSON view)**
   In VSCodium, press:

   ```
   Ctrl + Shift + P → "Preferences: Open Settings (JSON)"
   ```

2. **Add These Settings**
   Add the following snippet to your `settings.json`:

   ```json
   {
     "security.workspace.trust.enabled": true,
     "security.workspace.trust.startupPrompt": "never",
     "security.workspace.trust.emptyWindow": "never",
     "security.workspace.trust.untrustedFiles": "open",
     "security.workspace.trust.banner": false
   }
   ```

   | Setting                    | Effect                                     |
   | -------------------------- | ------------------------------------------ |
   | `"enabled": true`          | Keeps Restricted Mode functionality active |
   | `"startupPrompt": "never"` | Never prompt on startup                    |
   | `"emptyWindow": "never"`   | No trust prompt in empty windows           |
   | `"untrustedFiles": "open"` | Just open the files, don’t prompt          |
   | `"banner": false`          | Hides the "Restricted Mode" banner         |

##  Notes for Parrot OS Users

* **Parrot OS runs AppArmor/SELinux and other hardening**. VSCodium’s Restricted Mode is additive, but not a replacement.
* If you want to double down on security:

  * Don't install any extensions you don't trust.
  * Disable workspace shell access (`terminal.integrated.allowWorkspaceShell`).
  * Use Flatpak or AppImage versions for better sandboxing.

## Verify The Settings Work

1. Open any random folder.
2. VSCodium should open it in **Restricted Mode** silently.
3. You won’t be prompted to "trust" it anymore.

___