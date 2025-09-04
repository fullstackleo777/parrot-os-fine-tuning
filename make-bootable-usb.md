# Make Bootable USB

When it's time to upgrade to a new version of Parrot OS, it's best to proceed with a fresh install.

## Identify Bootable USB Device Name

Use `lsblk` to list the computer's drives and identify the Bootable USB's device name like `/dev/sdc` or `/dev/sdd` for example.

## Unmount if USB Drive is Mounted

Use `sudo unmount /dev/sdx`. Be sure to replace the device name with the correct drive.

## Verify the ISO File

Validate the integrity of your ISO file by comparing the checksum with the official hash provided by the source.

1. Calculate the SHA256 hash using `sha256sum Parrot-security-6.4_amd64.iso`. Be sure to use the correct file name of the ISO file.

2. Compare the output hash with the signed-hashes.txt file found at `https://deb.parrot.sh/parrot/iso/6.4/signed-hashes.txt`

3. If the corresponding entry in `signed-hashes.txt` matches the output hash, the ISO file is considered valid and unaltered.

## Run the `dd` Command

Make sure you have the correct input and output file paths!

`sudo dd if=/path/to/your-file.iso of=/dev/sdX bs=4M status=progress oflag=sync`

The `bs-4M` option sets the block size to 4 megabytes for better performance.

The `status=progress` shows real-time copying progress in the terminal.

The `oflag=sync` esures data is written synchronously.

## Safely Eject the Drive

Once the `dd` command is done, be sure to safely remove the USB Drive.

## Extra Info

The `dd` command works by copying the ISO file's data byte-by-byte directly onto the USB drive, preserving its bootable structure without requiring manual file system creation.

This method is compatible with most Unix-like systems and is widely available.

For a safer alternative with better progress reporting, consider using `dcfldd`. Graphical tools like Balena Etcher or Win32 Disk Imager are also available for users who prefer a visual interface.

TODO: Must find something faster than `dd` (Balena Etcher worked out just fine)
___