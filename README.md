![Parrot OS Fine Tuning Cover Image](https://raw.githubusercontent.com/fullstackleo777/covers/refs/heads/main/covers/parrot-os-fine-tuning/cover_parrot-os-fine-tuning.png)

# parrot-os-fine-tuning

Fine Tuning Details for Parrot OS - FullStackLeo

A practical collection of Parrot OS setup notes, fixes, and quality-of-life tweaks.

This repo is a personal Parrot OS tuning cookbook covering post-install setup, hardware fixes, development tools, desktop polish, networking issues, storage support, and common Linux workflow improvements.

___

## What is included

- Fresh install checklist
- Bluetooth and audio fixes
- Webcam, microphone, Wacom tablet, and Ethernet support notes
- Brave, GitHub CLI, and VSCodium setup
- Fonts, emoji, and Unicode input improvements
- exFAT and bootable USB notes
- Proton VPN troubleshooting
- Useful regular expressions and daily Linux references

___

## Before you start

> [!WARNING]
> Some guides include commands that can modify packages, drivers, disks, network routes, DNS, or removable drives. Read each guide fully before running commands.

Update your Parrot OS system before applying major changes:

```bash
sudo parrot-upgrade
```

Parrot OS is rolling-release based, so keeping the system current helps avoid outdated package and dependency issues.

___

## Requirements

Most guides assume:

- Parrot OS installed
- `sudo` access
- Basic terminal knowledge
- An internet connection
- A backup of important files before changing disks, drivers, or system services

___

## Quick start

Clone the repo:

```bash
git clone https://github.com/fullstackleo777/parrot-os-fine-tuning.git
cd parrot-os-fine-tuning
```

Start with:

1. [First Steps After Fresh OS Install](first-steps-after-fresh-os-install.md)
2. [Useful Software Not Included](useful-software-not-included.md)
3. Any hardware or tool-specific guide that matches your setup

___

## Guide index

### Start here

| Guide | Description | Risk |
|___|___|___|
| [First Steps After Fresh OS Install](first-steps-after-fresh-os-install.md) | Initial setup checklist after installing Parrot OS. | Medium |
| [Useful Software Not Included](useful-software-not-included.md) | Useful apps and tools to install after setup. | Low |
| [Update Brave Browser](update-brave-browser.md) | Notes for keeping Brave Browser updated. | Low |

### Hardware and peripherals

| Guide | Description | Risk |
|___|___|___|
| [Blue Yeti Mic Support](blue-yeti-mic-support.md) | Notes for getting a Blue Yeti microphone working properly. | Low |
| [Bluetooth Issues](fix-bluetooth-issues.md) | Troubleshooting Bluetooth services and audio stack issues. | Medium |
| [Bluetooth Paired but No Audio](fix-bluetooth-paired-but-no-audio.md) | Fixes for paired Bluetooth devices with no sound output. | Medium |
| [Ethernet Wired Connection Support](ethernet-wired-connection-support.md) | Troubleshooting wired network connection issues. | Medium |
| [Logitech Webcam Support](logitech-webcam-support.md) | Notes for Logitech webcam compatibility. | Low |
| [Wacom Bamboo Tablet Support](wacom-bamboo-tablet-support.md) | Setup notes for Wacom Bamboo tablets. | Medium |

### Storage and boot media

| Guide | Description | Risk |
|___|___|___|
| [Add exFAT to GParted](add-exfat-to-gparted.md) | Enable exFAT support for external drives and partition tools. | Medium |
| [Make Bootable USB](make-bootable-usb.md) | Create bootable USB media from an ISO file. | High |

> [!CAUTION]
> Be extra careful with bootable USB and disk-formatting commands. Choosing the wrong drive can permanently erase data.

### Development tools

| Guide | Description | Risk |
|___|___|___|
| [Install GitHub CLI](install-github-cli.md) | Install and configure the GitHub command-line tool. | Low |
| [VSCodium General Settings](vscodium-general-settings.md) | Recommended VSCodium settings. | Low |
| [Stop VSCodium Trust Prompt on Folder Open](stop-vscodium-trust-prompt-on-folder-open.md) | Disable or reduce VSCodium workspace trust prompts. | Low |
| [Common Regular Expressions I Use](common-regular-expressions-I-use.md) | Handy regex examples for daily development. | Low |

### Desktop experience

| Guide | Description | Risk |
|___|___|___|
| [Emoji Support](emoji-support.md) | Improve emoji rendering and input support. | Low |
| [Install Multiple Custom Fonts](install-multiple-custom-fonts.md) | Install custom fonts system-wide or per-user. | Low |
| [Type Unicode Special Characters](type-unicode-special-characters.md) | Reference for typing Unicode characters on Linux. | Low |

### Privacy and networking

| Guide | Description | Risk |
|___|___|___|
| [Proton VPN Troubleshooting](proton-vpn-troubleshooting.md) | Fix common Proton VPN networking, DNS, and route issues. | Medium |

___

## Recommended setup order

For a clean Parrot OS install, use this order:

```text
1. Update system
2. Run first install checklist
3. Install useful software
4. Configure browser and developer tools
5. Apply hardware-specific fixes
6. Add fonts, emoji, and desktop polish
7. Troubleshoot VPN or networking only if needed
```

___

## Safety checklist

Before running commands from any guide:

- Read the full guide first.
- Confirm the command applies to your exact issue.
- Check device names with `lsblk` before formatting or writing to drives.
- Avoid copying destructive commands blindly.
- Keep notes of what you changed.
- Create backups before editing system files.

Useful commands:

```bash
# Check Parrot/Debian version info
cat /etc/os-release

# List disks and partitions
lsblk

# Check active audio services
systemctl --user status pipewire wireplumber

# Check Bluetooth service
systemctl status bluetooth

# Check network routes
ip route

# Check DNS configuration
resolvectl status
```

___

## Documentation style

Each guide should ideally follow this structure:

```md
# Guide Title

## Problem

What issue this solves.

## Tested On

Parrot OS version, hardware, package versions, or desktop environment.

## Symptoms

How to recognize the problem.

## Fix

Commands and steps.

## Verify

How to confirm the fix worked.

## Rollback

How to undo the change.

## Notes

Extra context, alternatives, or warnings.
```

___

## Contributing

Improvements are welcome.

Good contributions include:

- Fixing outdated commands
- Adding rollback steps
- Adding tested Parrot OS versions
- Improving Markdown formatting
- Adding screenshots where useful
- Adding safer alternatives for risky commands
- Clarifying whether a fix applies to Home, Security, or other Parrot editions

Before opening a pull request, please keep guides:

- Clear
- Reproducible
- Safe by default
- Focused on one issue per file

___

## Disclaimer

These are personal configuration notes and troubleshooting guides. They are not official Parrot OS documentation.

Use commands at your own risk, especially commands involving disks, partitions, drivers, package removal, VPN routing, DNS, or system services.

For official operating system documentation, refer to the Parrot OS documentation.

___

## License

This project is licensed under the [GPL-3.0 License](LICENSE).

___