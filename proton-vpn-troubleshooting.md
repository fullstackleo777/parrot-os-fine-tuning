# Proton VPN Troubleshooting

There's a classic ProtonVPN trap. Even **without the Kill Switch** featured turned on, ProtonVPN can modify routing tables and DNS settings in a way that doesn’t get properly reset when you disconnect, especially on **Linux systems like Parrot OS**. Here's how you can fix that and make sure your internet works even after you turn off ProtonVPN.

## Steps to Fix

### 1. Check Your Default Route

When ProtonVPN disconnects, sometimes it **removes your default gateway**, leaving your system with no idea how to reach the internet.

Run this to see if there's a default route:

```bash
ip route
```

If there's **no `default via ...` route**, then your system can’t reach out to the internet.

To restore it (replace with your actual interface, usually `eth0`, `wlan0`, or `enpXsY`):

```bash
sudo ip route add default via <your-gateway-ip> dev <your-interface>
```

You can find your gateway IP with:

```bash
ip route | grep default
```

or check your interface's details:

```bash
ip addr show
```

### 2. Fix DNS Resolution

ProtonVPN often uses **custom DNS servers**, which may stay configured after disconnect. If those DNS servers are no longer reachable, resolution will break.

Edit your `resolv.conf` to use a public DNS:

```bash
sudo nano /etc/resolv.conf
```

Replace the contents with:

```bash
nameserver 1.1.1.1
nameserver 8.8.8.8
```

Save and exit with `Ctrl + O`, then `Ctrl + X`.

> If you're using `resolvconf` or `systemd-resolved`, let me know so I can guide you differently.

### 3. Flush and Restart NetworkManager (optional but helpful)

```bash
sudo systemctl restart NetworkManager
```

Also, check your interface state:

```bash
nmcli device status
```

You can reconnect the interface if needed:

```bash
nmcli device disconnect <interface>
nmcli device connect <interface>
```

## Restore Defaults Easily

If this happens often, make a script like this:

```bash
#!/bin/bash
# fix-internet.sh

# Replace these with your actual values
GATEWAY="192.168.1.1"
INTERFACE="wlan0"

sudo ip route add default via $GATEWAY dev $INTERFACE
echo "nameserver 1.1.1.1" | sudo tee /etc/resolv.conf
echo "nameserver 8.8.8.8" | sudo tee -a /etc/resolv.conf
sudo systemctl restart NetworkManager
```

Make it executable:

```bash
chmod +x fix-internet.sh
```

Run it any time your internet dies post-VPN.

---
