# Logitech Webcam Support

Parrot OS is a Debian-based Linux distribution, so device compatibility often depends on the Linux kernel and available drivers. Here’s how you can ensure support for your devices:

## 1. Logitech Webcam
Most Logitech webcams are plug-and-play on Linux systems, including Parrot OS. They use the **UVC (USB Video Class)** standard, which is natively supported by the Linux kernel. To verify:

- Steps to Check:
  1. Plug in the webcam.
  2. Run `lsusb` to confirm the system detects it. Look for "Logitech" in the output.
  3. Test the webcam using a tool like **Cheese** or **VLC**:
     ```bash
     sudo apt install cheese
     cheese
     ```
  4. If it works in Cheese, it’s fully functional.

## Troubleshooting:
  - If not detected, ensure the **uvcvideo** module is loaded:
    ```bash
    sudo modprobe uvcvideo
    ```
  - Make sure the audio settings are picking up the right input mic!

---
