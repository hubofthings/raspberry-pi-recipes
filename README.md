Raspberry Pi Configuration
==========================

## `raspi-config` (root)
- Set time zone
- Set hostname
- Enable SSH

## Install Overcast

    $ npm install --global overcast

## Import your Pi into Overcast

    $ overcast instance import my-pi 1.2.3.4 --user pi

## Customize configuration

    $ cp config/pi.template config/my-pi

## `./recipes/mkpi my-pi config/my-pi --password raspberry`
