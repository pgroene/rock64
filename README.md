# rock64
Rock64 setup


# eMMC Flash rock64 instructions
https://wiki.pine64.org/wiki/Getting_started

Follow: Flashing Using the USB-to-eMMC Adapter (Preferred Way)

# Use Etcher https://www.balena.io/etcher

# Software releases: https://wiki.pine64.org/wiki/ROCK64_Software_Releases#Debian

use https://deb.debian.org/debian/dists/bookworm/main/installer-arm64/current/images/netboot/SD-card-images/

# Concat the images: zcat firmware.rock64-rk3328.img.gz partition.img.gz > debian-installer.img

On Windows systems, you have to first decompress the two parts separately,
which can be done e.g. by using 7-Zip, and then concatenate the decompressed
parts together by running the command

  copy /b firmware.<board_name>.img + partition.img complete_image.img



#run home assistant docker with normal volume

docker run -d \
  --name home-assistant \
  --privileged \
  --restart=unless-stopped \
  -p 8123:8123 \
  -e TZ="America/Seattle" \
  -v homeassistant_config:/config \
  --network=host \
  --device /dev/ttyUSB0:/dev/ttyUSB0 \
   homeassistant/home-assistant

#run home assistant docker with on different mounted volume
docker run -d   --name home-assistant   --privileged   --restart=unless-stopped   -p 8123:8123   -e TZ="America/Seattle"   --mount type=volume,dst=/config,volume-driver=local,volume-opt=type=none,volume-opt=o=bind,volume-opt=device=/mount/docker/volumes/homeassistant   --network=host   --device /dev/ttyUSB0:/dev/ttyUSB0    homeassistant/home-assistant

#update

docker stop home-assistant

docker rm home-assistant

docker pull homeassistant/home-assistant

