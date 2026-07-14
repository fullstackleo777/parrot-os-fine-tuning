# Install QEMU/KVM With Virt-Manager on Parrot OS 7.3

QEMU/KVM and Virt-Manager setup for Parrot OS 7.3 - FullStackLeo

This guide installs a complete local virtualization stack using the packages already available through the Parrot OS repositories.

The Parrot way is simple:

- Keep the official Parrot repositories.
- Upgrade with `parrot-upgrade`.
- Install QEMU, KVM, libvirt, and Virt-Manager with `apt`.
- Add your user to the correct groups.
- Verify everything before creating a virtual machine.

___

## Problem

You want to run virtual machines on Parrot OS using Linux-native virtualization instead of adding VirtualBox, VMware, or third-party package repositories.

The recommended stack is:

- QEMU for machine emulation
- KVM for hardware acceleration
- libvirt for virtual machine management
- Virt-Manager for the graphical interface
- OVMF for UEFI virtual machines

___

## Target System

This guide is written for:

- Parrot OS 7.3
- A physical `amd64` or `x86-64` computer
- An Intel or AMD processor with hardware virtualization support
- A user account with `sudo` access
- An active internet connection

This guide can work on both Parrot Home and Parrot Security editions because they use the same Parrot repositories.

> **Note**
>
> If Parrot OS is already running inside another virtual machine, the host hypervisor must support and enable nested virtualization.

___

## Before You Start

Read the full guide before running the commands.

Do not add Ubuntu, Debian, random PPA, or third-party virtualization repositories. Parrot already provides the packages needed for this setup.

It is also a good idea to back up important files before making major system changes.

___

## Step 1: Confirm Your Parrot OS Version and Architecture

Open a Terminal with `Ctrl + Alt + T`.

Check the operating system information:

```bash
cat /etc/os-release
```

Check the system architecture:

```bash
dpkg --print-architecture
```

For this guide, the expected architecture is:

```text
amd64
```

You can also check the kernel architecture:

```bash
uname -m
```

The expected result is:

```text
x86_64
```

___

## Step 2: Check Hardware Virtualization Support

Run:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

A result of `1` or higher means the processor exposes Intel VT-x or AMD-V virtualization support.

You can also check with:

```bash
lscpu | grep -E 'Virtualization|Hypervisor vendor'
```

Common results include:

```text
Virtualization: VT-x
```

or:

```text
Virtualization: AMD-V
```

If the first command returns `0`, check your BIOS or UEFI settings and enable one of the following:

- Intel Virtualization Technology
- Intel VT-x
- AMD-V
- SVM Mode

The exact setting name depends on the motherboard or laptop manufacturer.

___

## Step 3: Upgrade Parrot OS Properly

Parrot OS provides its own upgrade command for safely handling rolling-release package updates.

Run:

```bash
sudo parrot-upgrade
```

Enter your password when asked.

Let the upgrade finish before installing the virtualization packages.

> **Important**
>
> Do not interrupt the system while packages are being upgraded.

___

## Step 4: Install QEMU, KVM, libvirt, and Virt-Manager

Install the complete virtualization stack:

```bash
sudo apt install -y --install-recommends \
  qemu-system-x86 \
  qemu-utils \
  libvirt-daemon-system \
  libvirt-clients \
  virt-manager \
  bridge-utils \
  ovmf
```

This installs:

- `qemu-system-x86` - QEMU system emulation for x86 and x86-64 virtual machines
- `qemu-utils` - Disk image tools such as `qemu-img`
- `libvirt-daemon-system` - The system-level libvirt virtualization service
- `libvirt-clients` - Commands such as `virsh`
- `virt-manager` - The graphical Virtual Machine Manager
- `bridge-utils` - Traditional Linux bridge management utilities
- `ovmf` - UEFI firmware for virtual machines

### Why This Guide Does Not Use `qemu-kvm`

The official Parrot virtualization page currently shows an installation command containing `qemu-kvm`.

On the Debian 13-era package base used by Parrot 7, `qemu-kvm` may not exist as a separately installable package. The modern `qemu-system-x86` package provides the x86 QEMU binaries and the `/usr/bin/kvm` command on `amd64`.

This guide uses the current package name to avoid an unnecessary `Package qemu-kvm has no installation candidate` error.

___

## Step 5: Add Your User to the libvirt and KVM Groups

Run:

```bash
sudo usermod -aG libvirt,kvm "$USER"
```

Confirm the group records exist:

```bash
getent group libvirt
getent group kvm
```

The group change does not fully apply to your current desktop session.

___

## Step 6: Reboot the Computer

Reboot Parrot OS:

```bash
sudo reboot
```

After logging back in, open a new Terminal.

___

## Step 7: Verify Your Group Membership

Run:

```bash
groups
```

The output should include:

```text
libvirt kvm
```

You can check only the relevant groups with:

```bash
id -nG "$USER" | tr ' ' '\n' | grep -E '^(libvirt|kvm)$'
```

___

## Step 8: Verify the KVM Device and Kernel Modules

Check the KVM device:

```bash
ls -l /dev/kvm
```

A working device normally belongs to the `kvm` group.

Check the loaded KVM kernel modules:

```bash
lsmod | grep '^kvm'
```

An Intel system normally shows:

```text
kvm_intel
kvm
```

An AMD system normally shows:

```text
kvm_amd
kvm
```

Confirm the QEMU and KVM commands are installed:

```bash
command -v qemu-system-x86_64
command -v kvm
```

Expected paths include:

```text
/usr/bin/qemu-system-x86_64
/usr/bin/kvm
```

___

## Step 9: Verify the libvirt System Connection

Run:

```bash
virsh -c qemu:///system list --all
```

A working installation should display a table, even when no virtual machines exist:

```text
 Id   Name   State
--------------------
```

You can run a more complete host validation with:

```bash
sudo virt-host-validate qemu
```

Review any warnings or failures before creating a virtual machine.

___

## Step 10: Check the Default Virtual Network

List the libvirt networks:

```bash
virsh -c qemu:///system net-list --all
```

A normal installation should include a network named `default`.

If `default` exists but is inactive, start it:

```bash
virsh -c qemu:///system net-start default
```

Make it start automatically:

```bash
virsh -c qemu:///system net-autostart default
```

Check it again:

```bash
virsh -c qemu:///system net-list --all
```

The expected state is similar to:

```text
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes
```

The default network normally gives virtual machines NAT-based internet access through the Parrot host.

___

## Step 11: Start Virt-Manager

Launch Virt-Manager from the Terminal:

```bash
virt-manager
```

You can also open it from the Parrot OS application menu.

Virt-Manager should show a connection similar to:

```text
QEMU/KVM
```

The preferred connection URI is:

```text
qemu:///system
```

If the connection does not appear:

1. Open `File`.
2. Select `Add Connection`.
3. Select `QEMU/KVM`.
4. Select the system connection.
5. Connect.

Use the system connection instead of `qemu:///session` when you want the standard libvirt storage pools, networks, permissions, and system-managed virtual machines.

___

## Step 12: Create Your First Virtual Machine

Inside Virt-Manager:

1. Click `Create a new virtual machine`.
2. Select `Local install media`.
3. Browse to your ISO file.
4. Select the operating system manually if automatic detection is wrong.
5. Choose the RAM and CPU allocation.
6. Create a virtual disk.
7. Review the settings.
8. Start the installation.

For modern Linux and Windows guests, enable UEFI firmware when needed. The `ovmf` package installed earlier provides the UEFI firmware.

Do not allocate every CPU core or all available memory to one virtual machine. Leave enough resources for the Parrot OS host.

___

## Complete Installation Commands

For an `amd64` Parrot OS 7.3 system, the main installation sequence is:

```bash
sudo parrot-upgrade

sudo apt install -y --install-recommends \
  qemu-system-x86 \
  qemu-utils \
  libvirt-daemon-system \
  libvirt-clients \
  virt-manager \
  bridge-utils \
  ovmf

sudo usermod -aG libvirt,kvm "$USER"

sudo reboot
```

After rebooting, verify the installation:

```bash
groups
ls -l /dev/kvm
lsmod | grep '^kvm'
virsh -c qemu:///system list --all
virsh -c qemu:///system net-list --all
virt-manager
```

___

## Troubleshooting

### Virt-Manager Says It Cannot Connect to libvirt

First, test the system connection directly:

```bash
virsh -c qemu:///system list --all
```

Check the traditional libvirt service and socket:

```bash
systemctl status libvirtd.service libvirtd.socket --no-pager
```

Some newer libvirt installations use modular daemons. Check the QEMU socket:

```bash
systemctl status virtqemud.socket --no-pager
```

If `libvirtd.socket` exists but is inactive, enable it:

```bash
sudo systemctl enable --now libvirtd.socket
```

If the system uses modular libvirt daemons instead, enable the QEMU socket:

```bash
sudo systemctl enable --now virtqemud.socket
```

For virtual networking with modular daemons, also check:

```bash
systemctl status virtnetworkd.socket --no-pager
```

Enable it if the unit exists but is inactive:

```bash
sudo systemctl enable --now virtnetworkd.socket
```

Then test again:

```bash
virsh -c qemu:///system list --all
```

### Permission Denied for `/dev/kvm`

Check the device:

```bash
ls -l /dev/kvm
```

Check your groups:

```bash
groups
```

If `kvm` or `libvirt` is missing, add the groups again:

```bash
sudo usermod -aG libvirt,kvm "$USER"
```

Then log out and back in, or reboot:

```bash
sudo reboot
```

### The Virtualization Check Returns `0`

Run:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

If the result is still `0`:

- Enable VT-x, AMD-V, or SVM in BIOS/UEFI.
- Confirm the processor supports hardware virtualization.
- If Parrot is inside another VM, enable nested virtualization on the physical host.
- Confirm another hypervisor is not preventing access to KVM.

### The Default Network Is Inactive

Check the network:

```bash
virsh -c qemu:///system net-list --all
```

Start and enable it:

```bash
virsh -c qemu:///system net-start default
virsh -c qemu:///system net-autostart default
```

### The Default Network Does Not Exist

Check whether the default network configuration package is installed:

```bash
dpkg -l | grep libvirt-daemon-config-network
```

Install it when missing:

```bash
sudo apt install libvirt-daemon-config-network
```

Then restart the libvirt connection by closing and reopening Virt-Manager.

Check again:

```bash
virsh -c qemu:///system net-list --all
```

### Check libvirt Logs

For the traditional daemon:

```bash
sudo journalctl -u libvirtd.service -b --no-pager | tail -100
```

For the modular QEMU daemon:

```bash
sudo journalctl -u virtqemud.service -b --no-pager | tail -100
```

For virtual networking:

```bash
sudo journalctl -u virtnetworkd.service -b --no-pager | tail -100
```

### Virt-Manager Opens a User Session Instead of the System Connection

Check the available connections:

```bash
virsh uri
```

Explicitly test the system connection:

```bash
virsh -c qemu:///system list --all
```

Inside Virt-Manager, add a new `QEMU/KVM` system connection and use:

```text
qemu:///system
```

___

## Optional Guest Integration

After installing a Linux guest, you can install the QEMU guest agent inside that virtual machine:

```bash
sudo apt update
sudo apt install qemu-guest-agent
sudo systemctl enable --now qemu-guest-agent
```

The guest agent can improve communication between libvirt and the guest, including shutdown handling and guest information reporting.

This command belongs inside the virtual machine, not on the Parrot host unless the Parrot system itself is a guest.

___

## Rollback

Before removing the virtualization stack, check which virtual machines exist:

```bash
virsh -c qemu:///system list --all
```

Virtual disk images are commonly stored under:

```text
/var/lib/libvirt/images/
```

Back up any virtual machines you want to keep.

Remove the main packages:

```bash
sudo apt remove \
  virt-manager \
  qemu-system-x86 \
  qemu-utils \
  libvirt-daemon-system \
  libvirt-clients \
  bridge-utils \
  ovmf
```

Remove your user from the groups:

```bash
sudo gpasswd -d "$USER" libvirt
sudo gpasswd -d "$USER" kvm
```

Optionally remove unused dependencies:

```bash
sudo apt autoremove
```

Do not manually delete `/var/lib/libvirt/` unless you understand what is stored there and have backed up anything important.

___

## Notes

- Use `qemu:///system` for the normal system-wide Virt-Manager setup.
- Use Parrot repositories instead of adding unrelated Debian or Ubuntu repositories.
- Run `sudo parrot-upgrade` regularly to keep the Parrot rolling-release system current.
- KVM acceleration requires hardware virtualization support.
- OVMF provides UEFI firmware but does not automatically enable Secure Boot for every guest.
- VM performance depends on available CPU, memory, storage, and host workload.
- The `libvirt` group has powerful access to system virtualization. Only add trusted users.

___

## References

- [Parrot OS: Installing QEMU/KVM, Virt-Manager, and GNOME Boxes](https://parrotsec.org/docs/virtualization/qemu-kvm-configuration/)
- [Parrot OS Software Management](https://parrotsec.org/docs/configuration/parrot-software-management/)
- [Debian Package: qemu-system-x86](https://packages.debian.org/trixie/qemu-system-x86)
- [Debian Package: virt-manager](https://packages.debian.org/trixie/virt-manager)
- [Debian Package: libvirt-daemon-system](https://packages.debian.org/trixie/libvirt-daemon-system)
- [libvirt Daemons](https://libvirt.org/daemons.html)
- [virtqemud Manual](https://libvirt.org/manpages/virtqemud.html)

___

## Disclaimer

These are personal configuration notes and troubleshooting steps. They are not official Parrot OS documentation.

Read commands before running them and keep backups of important files and virtual machine images.