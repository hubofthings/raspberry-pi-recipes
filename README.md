Raspberry Pi Recipes
====================

## What's this repo all about?

This project contains [Overcast](http://andrewchilds.github.io/overcast/) scripts to automatically configure your Raspberry Pis _remotely_, including:

- Add your public key to `authorized_keys`
- Update installed packages to the latest and greatest
- Add the [GoDaddy Class 2 Certification Authority Root Certificate - G2](https://certs.godaddy.com/repository) to your Java CA Certificates Store
- Install [Collectd](https://collectd.org/)
- Configure Collectd metrics to forward to [Librato](https://www.librato.com/)
- Install the [No-IP](http://www.noip.com/) Dynamic Update Client
- Configure [WPA Supplicant](http://w1.fi/wpa_supplicant/) to connect to your WiFi SSID (requires USB WiFi adapter)
- Install [BlueZ 5](http://www.bluez.org/) (requires USB Bluetooth adapter)
- Install [Bluepy](https://github.com/IanHarvey/bluepy) (requires USB Bluetooth adapter)
- Install [Node.js](http://nodejs.org/)
- Install [Node-RED](http://nodered.org/) with basic nodes
- Install [Mosquitto](http://mosquitto.org/)
- Disable password-based authentication for the `pi` user account

Note that each of these tools is _**optional**_ and controlled by a flag (see [config/pi.template](https://github.com/garnold/raspberry-pi-recipes/blob/master/config/pi.template)).  

Also note that 99.999% of the recipe is _not_ Pi-specific, and as such it can be used to configure any [Debian GNU/Linux](https://www.debian.org/)-based distribution.  We've used it to configure a Debian guest running under [VirtualBox](https://www.virtualbox.org/) on OSX.

# Using

<!-- ## Configure your Pi

    $ sudo raspi-config

- Set time zone
- Set hostname
- Enable SSH -->

## Install Overcast on your local machine

    $ npm install --global overcast

## Import your Pi into Overcast

    $ overcast instance import my-pi 1.2.3.4 --user pi

## Customize the configuration

    $ cp config/pi.template config/my-pi
    $ vi config/my-pi

## Make Pi

    $ ./recipes/mkpi my-pi config/my-pi --password raspberry
