# First Steps After A Fresh Parrot OS Install

## To Fully Upgrade Your Fresh Install of Parrot OS, Follow These Simple Steps:

### Step 1: Open a Terminal
- Press `Ctrl + Alt + T` to open the Terminal, or find it in the Applications menu.

### Step 2: Update the Package List
First, you need to update the package list to make sure you’re getting the latest versions from the repositories. Type the following command and press `Enter`:

```bash
sudo apt update
```

- The system will ask for your password. Type it in (you won’t see the characters appear, but that’s normal) and press `Enter`.

### Step 3: Upgrade Installed Packages
Now, to upgrade all installed packages to their latest versions, run the following command:

```bash
sudo apt full-upgrade
```

- The `full-upgrade` command will upgrade the packages and handle any changes in dependencies.

### Step 4: Confirm the Upgrade
- If it asks for confirmation (like "Do you want to continue? [Y/n]"), just press `Y` and `Enter`.

### Step 5: Clean Up (Optional)
Once the upgrade is done, you can clean up old packages and free up space by running:

```bash
sudo apt autoremove
```

That’s it! Your system should now be fully upgraded.

## Steps After Full Upgrade

Great job on upgrading your system! Now that your Parrot OS is up to date, here are a few important configurations you might want to consider next:

### 1. Set Up Firewall (UFW - Uncomplicated Firewall)
Security is one of the main focuses of Parrot OS, and setting up the firewall is a good next step.

- **Install UFW (if it's not installed):**

  ```bash
  sudo apt install ufw
  ```

- **Enable UFW:**

  ```bash
  sudo ufw enable
  ```

- **Allow basic services (like SSH, if needed):**
  For example, if you need SSH access:

  ```bash
  sudo ufw allow ssh
  ```

- **Check the firewall status:**

  ```bash
  sudo ufw status
  ```

### 2. Configure the Appearance
You can tweak the desktop environment to your liking:

- **Change the theme, icons, and background**: Go to `Settings` → `Appearance` to explore available options.
- **Dark Mode**: If you prefer dark themes, you can enable it under `Appearance` as well.

### 3. Set Up Privacy Tools
Parrot OS comes with privacy tools by default, but it’s good to configure them properly:
- **Anonsurf**: Anonsurf routes all traffic through Tor for privacy.

  To enable Anonsurf:

  ```bash
  sudo anonsurf start
  ```

  You can stop it with:

  ```bash
  sudo anonsurf stop
  ```

- **Tor Browser**: If you need the Tor Browser for anonymous browsing, you can launch it from the Applications menu or install it if it's missing:

  ```bash
  sudo apt install torbrowser-launcher
  ```

### 4. Configure Software Repositories
By default, Parrot OS should have the correct repositories, but it’s good to double-check in case you want to enable more sources.

- **To check or modify repositories:**

  Open the file with a text editor:

  ```bash
  sudo nano /etc/apt/sources.list.d/parrot.list
  ```

  Make sure the entries look correct (the default Parrot ones should already be there). If you want to add custom ones, this is where you can do it.

### 5. Install Additional Tools
Depending on your usage, you may want to install specific tools:
- **Development tools**: Install programming languages like Python, Java, or other development environments.

  For Python:

  ```bash
  sudo apt install python3-pip
  ```

  For Java:

  ```bash
  sudo apt install default-jdk
  ```

- **VirtualBox or VMware**: If you want to run virtual machines, you might want to install VirtualBox.

  ```bash
  sudo apt install virtualbox
  ```

### 6. Update Your System Regularly
Security updates are important, so you might want to schedule regular updates. You can automate this, but for now, you can run:

```bash
sudo apt update && sudo apt full-upgrade
```

### 7. Backup Configuration
Setting up a backup system is crucial in case something goes wrong:

- **Install Timeshift** (a system backup tool):

  ```bash
  sudo apt install timeshift
  ```

- Configure it to take regular snapshots of your system.

---
