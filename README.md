Raspberry Pi Configuration
==========================

## `raspi-config` (root)
- Change `pi` user password
- Set time zone
- Set hostname
- Enable SSH

## Configure Bluetooth (root)
    export DEBIAN_FRONTEND=noninteractive
    /usr/bin/apt-get --quiet update
    /usr/bin/apt-get --quiet --yes --no-install-recommends install bluez    
    
## Install Collectd Librato plugin (root)
    pushd /usr/local/src/
    /usr/bin/git clone git://github.com/librato/collectd-librato.git
    cd collectd-librato/
    /usr/bin/make install
    popd

    pushd /etc/collectd.d/
    /bin/cat <<\EOF> librato.conf
    <LoadPlugin "python">
        Globals true
    </LoadPlugin>
    <Plugin "python">
        # collectd-librato.py is at /opt/collectd-librato-0.0.10/lib/collectd-librato.py
        ModulePath "/opt/collectd-librato-0.0.10/lib"
        Import "collectd-librato"
        <Module "collectd-librato">
            Email "<email>"
            APIToken "<token>"
            LowercaseMetricNames true
            MetricPrefix ""
            IncludeRegex "^cpu\\..+$,^df\\.mnt.+$,^df\\.root\\..+$,^memory\\.memory\\..+$,^ping\\.ping_droprate\\..+$,^swap\\.swap\\..+$"
        </Module>
    </Plugin>
    EOF
    popd

    /etc/init.d/collectd restart

## Install No-IP Dynamic Update Client (root)
    pushd /usr/local/src/
    /usr/bin/wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
    /bin/tar zxvf noip-duc-linux.tar.gz
    cd noip-2.1.9-1/
    /usr/bin/make
    /usr/bin/make install
    popd

    /bin/cat <<\EOF> /etc/cron.d/noip2
    # m h dom mon dow user command
    @reboot root /usr/local/bin/noip2
    EOF

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

## Install JDK7 GoDaddy Class 2 Certification Authority Root Certificate - G2 (root)
    /usr/bin/wget --output-document=/tmp/gdroot-g2.crt 'https://certs.godaddy.com/anonymous/repository.pki?streamfilename=gdroot-g2.crt&actionMethod=anonymous%2Frepository.xhtml%3Arepository.streamFile%28%27%27%29&cid=849433'

    pushd /usr/lib/jvm/jdk-7-oracle-armhf/jre/lib/security/
    [ -f cacerts.DEFAULT ] || /bin/cp cacerts cacerts.DEFAULT
    /usr/bin/keytool -import -noprompt -file /tmp/gdroot-g2.crt -alias gdroot-g2 -trustcacerts -keystore cacerts -storepass changeit
    popd
