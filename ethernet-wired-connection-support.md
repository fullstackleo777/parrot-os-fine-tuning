# Troubleshoot and Fix Wired Ethernet Connection

## Troubleshooting

### Step 1: Check if Your System Recognizes the Wired Connection
We’ll first check if your Parrot OS detects the LAN connection. Open a terminal (press `Ctrl` + `Alt` + `T` or search for "Terminal" in the application menu), then type the following command:

```
ip a
```

This will display a list of network interfaces. Look for something like `eth0`, `enp3s0`, or `eno1`—these are typical names for wired interfaces. Check if it says `UP` or `DOWN`. If it says `DOWN`, it means the interface is not active.

### Step 2: Bring the Interface Up
If the wired interface is down, you can try bringing it up manually. In the terminal, type:

```
sudo ip link set <interface-name> up
```

Replace `<interface-name>` with the name of your interface (e.g., `eth0`). For example, if your interface is `eth0`, the command would be:

```
sudo ip link set eth0 up
```

After running the command, check if the internet is working.

### Step 3: Restart the NetworkManager
Sometimes restarting the network services can solve the issue. You can restart the NetworkManager service like this:

```
sudo systemctl restart NetworkManager
```

Wait a few moments, and then see if the wired connection is working.

### Step 4: Check if the Network Connection is Set to Auto-Connect
You can verify whether the connection is set to auto-connect:

#### 1. Go to **System Settings** or **Settings**.
#### 2. Navigate to **Network** or **Network Connections**.
#### 3. Look for your wired connection profile.
#### 4. Ensure the checkbox "Connect automatically" is enabled.

If it wasn't checked, try enabling it and reconnecting.

### Step 5: Reboot Your System
If none of the above steps worked, it might be a simple issue that can be fixed with a reboot. Reboot your Parrot OS system and see if the LAN connection is restored.

### Step 6: Check for Missing Drivers or Firmware
If you still don't have a connection, it could be a driver issue. To check if your Ethernet adapter has a missing driver, type:

```
lspci | grep Ethernet
```

If the Ethernet adapter shows up but isn't working, you might need to install drivers or firmware.

## Troubleshooting

After running `lspci | grep Ethernet` and got the message `0a:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 0c)`

It looks like your system has a Realtek RTL8111/8168/8411 Ethernet controller. This particular controller sometimes needs an additional or updated driver for proper functionality on Linux systems like Parrot OS.

Here’s how we can address this issue:

### Step 1: Check if the Driver is Installed
First, let’s check if the correct driver for the Realtek Ethernet card is installed and loaded. In your terminal, run this command:

```
lsmod | grep r8169
```

This command checks if the `r8169` driver, which is typically used for this Ethernet card, is loaded.

- If you see `r8169` in the output, the driver is loaded.
- If you don’t see any output, it means the driver is not loaded.

### Step 2: Install or Update the Realtek Driver
If the `r8169` driver is loaded but you’re still facing issues, the default `r8169` driver might not work well with your specific Realtek chip version. We can install the **r8168** driver, which is more compatible with this chip.

Here are the steps to install the `r8168` driver:

#### 1. **Update your package list:**

   ```
   sudo apt update
   ```

#### 2. **Install the `r8168-dkms` package**, which provides a better driver for Realtek network cards:

   ```
   sudo apt install r8168-dkms
   ```

#### 3. **Unload the old driver and load the new one:**

   First, remove the current `r8169` driver:

   ```
   sudo modprobe -r r8169
   ```

   Then, load the `r8168` driver:

   ```
   sudo modprobe r8168
   ```

#### 4. **Reboot your system:**
   
   After the above steps, reboot your machine:

   ```
   sudo reboot
   ```

Once your system has rebooted, check if your LAN connection is working properly.

### Step 3: Verify the Network Connection
After rebooting, you can verify the connection status by running:

```
ip a
```

Check if your Ethernet interface is now up and has an IP address assigned to it. You should see something like this:

- The `eth0`, `enp3s0`, or similar interface should say `UP`.
- It should have an IP address listed under it (something like `inet 192.168.x.x`).

---
