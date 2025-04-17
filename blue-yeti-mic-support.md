# Blue Yeti Mic Support

The Blue Yeti microphone is also plug-and-play on Linux systems. It uses the **standard USB Audio Class (UAC)** driver, which is built into the Linux kernel.

## Steps to Check:
  1. Plug in the microphone.
  2. Run `lsusb` to confirm detection. You should see "Blue Microphones" or similar.
  3. Check if it’s listed as an audio input device:
     ```bash
     arecord -l
     ```
  4. Use tools like **Audacity** or **PulseAudio Volume Control (pavucontrol)** to test:
     ```bash
     sudo apt install pavucontrol
     pavucontrol
     ```

## Troubleshooting:
  - If audio is not detected, make sure **ALSA** and **PulseAudio** are properly installed and configured.
  - Use `dmesg | grep audio` to look for driver issues.

---
