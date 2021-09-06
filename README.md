# Audio-Relay

### HFP server with ffpeg tunneling

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://github.com/scripting-drafts/Audio-Relay/)

Runs on [Raspbian 10](https://downloads.raspberrypi.org/raspbian/images/raspbian-2020-02-14/2020-02-13-raspbian-buster.zip) (32bit Debian Buster)

sudo apt-get purge chromium
sudo apt-get autoremove
sudo apt-get autoclean

In `/etc/dbus-1/system.d/ofono.conf` modify:

    <policy context="default">
        <deny send_destination="org.ofono"/>
    </policy>

to:

    <policy context="default">
        <allow send_destination="org.ofono"/>
    </policy>


Run:
```sh
git clone -b stable-14.x https://gitlab.freedesktop.org/pulseaudio/pulseaudio.git
```

In `src/modules/bluetooth/backend-native.c` and `src/modules/bluetooth/backend-ofono.c` change:
    
    *imtu = 48;
    
to:
    
    *imtu = 60;

Run:
```sh
sudo apt-get build-dep pulseaudio
cd pulseaudio
./bootstrap.sh && make && sudo make install && ldconfig
```

make services for:
systemctl enable --now ofono
pulseaudio --start

Limit profiles to HFP only in `/usr/local/etc/pulse/default.pa` by adding:

    .ifexists module-bluetooth-discover.so
    load-module module-bluetooth-discover autodetect_mtu=yes headset=ofono
    .endif


### Pending:
 - Lower output volume programatically
 - Tunnel audio through ssh
 - "logo.nologo" to the line in /boot/cmdline.txt
