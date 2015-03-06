Raspberry Pi Recipes
====================

## What's this repo all about?

This project contains [Overcast](http://andrewchilds.github.io/overcast/) scripts to automatically configure your Raspberry Pis _from your local machine_, including:

- Adding your public key to `authorized_keys`
- Updating installed packages to the latest and greatest
- Installing the [GoDaddy Class 2 Certification Authority Root Certificate - G2](https://certs.godaddy.com/repository) into the Java CA Certificates Store
- Installing [Collectd](https://collectd.org/)
- Configuring Collectd metrics to forward to [Librato](https://www.librato.com/)
- Installing the [No-IP](http://www.noip.com/) Dynamic Update Client
- Configuring [WPA Supplicant](http://w1.fi/wpa_supplicant/) to connect to your WiFi SSID (requires USB WiFi adapter)
- Installing [BlueZ 5](http://www.bluez.org/) (requires USB Bluetooth adapter)
- Installing [Node.js](http://nodejs.org/)
- Installing [Node-RED](http://nodered.org/), with helpful nodes
- Disabling password-based authentication for the `pi` user account

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
