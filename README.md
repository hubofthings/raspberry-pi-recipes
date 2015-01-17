Raspberry Pi Configuration
==========================

## `raspi-config` (root)
- Set time zone
- Set hostname
- Enable SSH

## Configure Bluetooth (root)
    export DEBIAN_FRONTEND=noninteractive
    /usr/bin/apt-get --quiet update
    /usr/bin/apt-get --quiet --yes --no-install-recommends install bluez    
    
## Move `pi` home directory to USB drive (root)
    /sbin/mkfs.ext4 /dev/sda1
    /bin/mkdir --parents /mnt/usb/
    pushd /etc/
    [ -f fstab.DEFAULT ] || /bin/cp fstab fstab.DEFAULT
    /bin/cat <<\EOF>> fstab
    /dev/sda1 /mnt/usb ext4 defaults,noatime 0 2
    EOF
    popd
    /bin/mount /mnt/usb
    /bin/cat <<\EOF> /etc/cron.d/pi
    # m h dom mon dow user command
    @reboot root /usr/sbin/usermod --home /mnt/usb/pi --move-home pi && /bin/ln --force --no-dereference --symbolic /mnt/usb/pi /home/pi && /bin/rm --force /etc/cron.d/pi
    EOF

## Install JDK7 (root)
    /bin/cat <<\EOF | /usr/bin/debconf-set-selections
    oracle-java7-installer shared/accepted-oracle-license-v1-1 select true
    EOF

    export DEBIAN_FRONTEND=noninteractive
    /usr/bin/apt-get --quiet update
    /usr/bin/apt-get --quiet --yes --no-install-recommends install oracle-java7-jdk
