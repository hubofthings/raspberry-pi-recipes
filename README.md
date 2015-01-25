Raspberry Pi Configuration
==========================

## `raspi-config` (root)
- Set time zone
- Set hostname
- Enable SSH

## `npm --global install overcast`

## `./recipes/mkpi [instance|cluster|all] [config]`

## Configure Bluetooth (root)
    export DEBIAN_FRONTEND=noninteractive
    /usr/bin/apt-get --quiet update
    /usr/bin/apt-get --quiet --yes --no-install-recommends install bluez    
