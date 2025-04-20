# Add Emojis to Parrot OS

Adding emoji support to **Parrot OS** involves enabling the correct fonts and ensuring your system has the necessary packages installed. Follow these steps to add emoji characters:

## Steps to Add

### 1. Install Emoji Fonts
Ensure you have fonts that include emojis. A popular choice is the `noto-fonts-emoji` package.

- Open your terminal.
- Install the emoji font with the following command:
  ```bash
  sudo apt update
  sudo apt install fonts-noto-color-emoji
  ```

### 2. Set Emoji Font as Default
To ensure emojis display properly in applications:

- Edit or create a font configuration file in your home directory:
  ```bash
  mkdir -p ~/.config/fontconfig/conf.d/
  nano ~/.config/fontconfig/conf.d/emoji.conf
  ```
- Add the following configuration to prioritize emoji fonts:
  ```xml
  <?xml version="1.0"?>
  <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
  <fontconfig>
    <alias>
      <family>sans-serif</family>
      <prefer>
        <family>Noto Color Emoji</family>
      </prefer>
    </alias>
    <alias>
      <family>serif</family>
      <prefer>
        <family>Noto Color Emoji</family>
      </prefer>
    </alias>
    <alias>
      <family>monospace</family>
      <prefer>
        <family>Noto Color Emoji</family>
      </prefer>
    </alias>
  </fontconfig>
  ```
- Save the file and exit.

### 3. Rebuild Font Cache
Rebuild the font cache to apply changes:
```bash
fc-cache -fv
```

### 4. Enable Emoji Support in Terminal (Optional)
If you want emojis to render in your terminal (e.g., in bash, zsh, or other terminal apps):

- Make sure your terminal supports Unicode. Most modern terminals do.
- Use a font in your terminal emulator that includes emoji support, such as `Noto Color Emoji` or `Hack Nerd Font`.

### 5. Test Emoji Rendering
You can test emoji support by running the following command in your terminal:
```bash
echo -e "\xF0\x9F\x98\x81"
```
This should render a smiling face emoji (😁).

### 6. Install Additional Emoji Tools (Optional)
For extended emoji usage, consider installing emoji-picker tools:
```bash
sudo apt install rofi-emoji
```
You can then invoke the emoji picker using `rofi-emoji`.

By following these steps, you should be able to fully enjoy emoji support across your system.

---
