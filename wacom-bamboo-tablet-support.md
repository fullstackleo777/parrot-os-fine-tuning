# Wacom Bamboo Tablet Support

Wacom tablets are well-supported in Linux through the **input-wacom** driver, which is included in the kernel. Parrot OS should work out of the box for most Wacom Bamboo models.

## Steps to Check
### 1. Plug in the tablet
### 2. Run `lsusb` and look for "Wacom"
### 3. Check if the Wacom driver is loaded
     ```bash
     xsetwacom --list devices
     ```
### 4. Install **xinput** and **xsetwacom** for configuration if not already installed
     ```bash
     sudo apt install xinput
     sudo apt install xserver-xorg-input-wacom
     ```

## Troubleshooting
  - If not working, update the Linux kernel or install the latest Wacom drivers:
    ```bash
    sudo apt install linux-wacom
    ```
  - Use **dmesg** to look for errors:
    ```bash
    dmesg | grep wacom
    ```

---
