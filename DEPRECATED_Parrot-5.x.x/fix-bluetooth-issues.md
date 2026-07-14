# Fix Bluetooth Issues

I have encountered a variety of different bluetooth audio issues on every Parrot OS installation I have deployed. My current conclusion is that pulseaudio absolutely has to be replaced with pipewire.

PipeWire is faster, lower-latency, and cleanly unifies PulseAudio with JACK.

## 1. Replace PulseAudio with PipeWire

The cleanest way to install PipeWire's PulseAudio replacement, disable PulseAudio, and enable WirePlumber for session management.

To avoid mixed repos and apt not allowing mixing, install the bluetooth audio stack from `lory-backports`.

```
sudo apt update

# (Optional) clear any holds that could block resolution
sudo apt-mark unhold 'pipewire*' 'libpipewire-0.3*' 'libspa-0.2*' 'gstreamer1.0-pipewire' 'wireplumber' 2>/dev/null || true
sudo apt --fix-broken install -y

# Pull the ENTIRE stack from lory-backports so versions match your existing libpipewire 1.4.2
sudo apt -t lory-backports install -y \
  pipewire pipewire-bin pipewire-pulse pipewire-alsa pipewire-jack \
  libpipewire-0.3-0 libpipewire-0.3-modules \
  libspa-0.2-modules libspa-0.2-bluetooth libspa-0.2-jack \
  wireplumber gstreamer1.0-pipewire
```

## 2. Stop & Muzzle PulseAudio

Disable the user services and prevent autospawn.

```
systemctl --user --now disable pulseaudio.service pulseaudio.socket || true
systemctl --user mask pulseaudio.service pulseaudio.socket || true
mkdir -p ~/.config/pulse
printf "autospawn = no\ndaemon-binary = /bin/true\n" > ~/.config/pulse/client.conf
```

## 3. Enable PipeWire & WirePlumber

```
# stop & mask PulseAudio so it can’t autospawn
systemctl --user --now disable pulseaudio.service pulseaudio.socket 2>/dev/null || true
systemctl --user mask pulseaudio.service pulseaudio.socket 2>/dev/null || true
mkdir -p ~/.config/pulse
printf "autospawn = no\ndaemon-binary = /bin/true\n" > ~/.config/pulse/client.conf

# enable the new hotness
systemctl --user --now enable pipewire.service
systemctl --user --now enable pipewire-pulse.service
systemctl --user --now enable wireplumber.service

# quick sanity check
systemctl --user --no-pager --full status pipewire pipewire-pulse wireplumber
pactl info | grep "Server Name"   # should say: PulseAudio (on PipeWire X.Y.Z)
```

## 4. Verify It's Live

These should show green and active.

```
systemctl --user --no-pager --full status pipewire pipewire-pulse wireplumber
```

This should say PipeWire is backing the PA server.

```
pactl info | grep "Server Name"
# Expected: "Server Name: PulseAudio (on PipeWire 1.x.x)"
```

### Extra Info

Check more info by running:

```
pw-top         # live graph of audio nodes
pw-cli info 0  # PipeWire core info
```

## 5. Optionally Add GUI Mixer and JACK Apps

GUI Mixer that works with PipeWire

```
sudo apt install -y pavucontrol
```

JACK Apps

```
sudo apt install -y pipewire-jack
```

For screen sharing on Wayland (if you use it)
```
sudo apt install -y xdg-desktop-portal xdg-desktop-portal-gtk
```

## 6. Optional Cleanup for Lean Systems

You don't have to but can remove PulseAudio entirely to clean up space. Keep in mind that pipewire-pulse replaces pulseaudio so there's no need to remove for functionality.

```
sudo apt purge -y pulseaudio
sudo apt autoremove -y
```

## That's It!

You should have everything up and running smoothly now.

### In Case You Messed Something Up and Need A Rollback

DO NOT RUN THIS UNLESS YOU ARE TRYING TO UNDO WHAT YOU DID!!!

```
##### DANGER ZONE: DO NOT RUN UNLESS YOU WANT TO UNDO WHAT YOU HAVE DONE #####

# Unmask + re-enable PulseAudio
systemctl --user unmask pulseaudio.service pulseaudio.socket
systemctl --user --now enable pulseaudio.service pulseaudio.socket

# Disable PipeWire's PA shim
systemctl --user --now disable pipewire-pulse.service

# Remove the autospawn block
rm -f ~/.config/pulse/client.conf

# (Optional) remove pipewire-pulse if you truly want PA back
sudo apt install -y pulseaudio
sudo apt purge -y pipewire-pulse

##### DANGER ZONE: DO NOT RUN UNLESS YOU WANT TO UNDO WHAT YOU HAVE DONE #####
```

___