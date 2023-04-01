# rock64
Rock64 setup


# eMMC Flash rock64 instructions
https://wiki.pine64.org/wiki/Getting_started

Follow: Flashing Using the USB-to-eMMC Adapter (Preferred Way)

# Use Etcher https://www.balena.io/etcher

# Software releases: https://wiki.pine64.org/wiki/ROCK64_Software_Releases#Debian

# Concat the images: zcat firmware.rock64-rk3328.img.gz partition.img.gz > debian-installer.img

On Windows systems, you have to first decompress the two parts separately,
which can be done e.g. by using 7-Zip, and then concatenate the decompressed
parts together by running the command

  copy /b firmware.<board_name>.img + partition.img complete_image.img
