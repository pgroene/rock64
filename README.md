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
  -e TZ="Europe/Amsterdam" \
  -v homeassistant_config:/config \
  --network=host \
  --device /dev/zigbee0:/dev/ttyUSB0 \
  --label com.centurylinklabs.watchtower.enable=true \
   homeassistant/home-assistant

docker run -d \
  --name home-assistant \
  --privileged \
  --restart=unless-stopped \
  -p 8123:8123 \
  -e TZ="Europe/Amsterdam" \
  -v homeassistant_config:/config \
  --network=host \
  --label com.centurylinklabs.watchtower.enable=true \
   homeassistant/home-assistant

--device /dev/modbus_current:/dev/modbus_current \

  
# mqtt broker
https://smarthomecircle.com/how-to-setup-mqtt-docker-container-with-home-assistant#running-mqtt-broker-as-a-docker-container-using-docker-compose
docker run -d \
  --name mosquitto \
  --restart=unless-stopped \
  -e TZ="America/Seattle" \
  -v ./mosquitto:/mosquitto   \
  -v ./mosquitto/data:/mosquitto/data \
  -v ./mosquitto/log:/mosquitto/log \
  -p 1883:1883 \
  -p 9001:9001 \
  --label com.centurylinklabs.watchtower.enable=true \
  eclipse-mosquitto  
  
# zigbee2mqtt
https://smarthomecircle.com/install-zigbee2mqtt-with-home-assistant#running-zigbee2mqtt-as-a-docker-container

docker run -d \
  --name zigbee2mqtt \
  --restart=unless-stopped \
  -e TZ="America/Seattle" \
  -p 8080:8080 \
  -v ./zigbee2mqtt/data:/app/data   \
  -v /run/udev:/run/udev:ro \
  --device /dev/zigbee1:/dev/ttyUSB1 \
  --label com.centurylinklabs.watchtower.enable=true \
   koenkk/zigbee2mqtt

docker run -d \
  --name zigbee2mqtt_presence \
  --restart=unless-stopped \
  -e TZ="America/Seattle" \
  -p 8081:8080 \
  -v ./zigbee2mqtt_presence/data:/app/data   \
  -v /run/udev:/run/udev:ro \
  --device /dev/zigbee0:/dev/ttyUSB1 \
  --label com.centurylinklabs.watchtower.enable=true \
   koenkk/zigbee2mqtt


   
#update

docker stop home-assistant

docker rm home-assistant

docker pull homeassistant/home-assistant



#Wispher

docker run -d  --name whisper --restart=unless-stopped --label com.centurylinklabs.watchtower.enable=true -p 10300:10300 -v /path/to/local/data:/data rhasspy/wyoming-whisper  --model base-int8 --language en

tiny-int8 (43 MB)
tiny (152 MB)
base-int8 (80 MB)
base (291 MB)
small-int8 (255 MB)
small (968 MB)
medium-int8 (786 MB)
medium (3.1 GB)

#Piper

docker run -d --name piper --restart=unless-stopped --label com.centurylinklabs.watchtower.enable=true -p 10200:10200 -v /path/to/local/data:/data rhasspy/wyoming-piper --voice en_US-lessac-medium

#openwakeword

docker run -d --name openwakeword --label com.centurylinklabs.watchtower.enable=true --restart=unless-stopped  -p 10400:10400 rhasspy/wyoming-openwakeword --preload-model 'ok_nabu'

#snowboy

docker run -d --name showboy --restart=unless-stopped  -p 10400:10400 rhasspy/wyoming-snowboy

#watchtower

docker run -d \
  --name watchtower \
  --restart=unless-stopped \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower:latest

#p1

docker run -d -p 8090:80\
       	-h p1mon\
        --name=p1monitor\
       	--tmpfs /tmp\
       	--tmpfs /run\
       	-v /docker/volumes/p1mon/data:/p1mon/data\
       	-v /docker/volumes/p1mon/usbdisk:/p1mon/mnt/usb\
       	-v /docker/volumes/p1mon/mnt/ramdisk:/p1mon/mnt/ramdisk\
       	--device /dev/ttyUSB2:/dev/ttyUSB0\
       	--restart=unless-stopped\
       	mclaassen/p1mon
        

Firmware update using docker:

docker run --rm --device /dev/zigbee1:/dev/ttyUSB1 -e FIRMWARE_URL=https://github.com/Koenkk/Z-Stack-firmware/raw/master/coordinator/Z-Stack_3.x.0/bin/CC1352P2_CC2652P_launchpad_coordinator_20240710.zip ckware/ti-cc-tool -ewv -p /dev/ttyUSB1 --bootloader-sonoff-usb

   
