# Audio-Relay

## HFP server with ffpeg tunneling

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://github.com/scripting-drafts/Audio-Relay/)

Runs on [Raspbian 10](https://downloads.raspberrypi.org/raspbian/images/raspbian-2020-02-14/2020-02-13-raspbian-buster.zip) (32bit Debian Buster)

(Enable developer sources in the ``/etc/apt/sources.list`` file by commenting out the lines starting with deb-src.)
```sh
sudo apt-get install pi-bluetooth ofono ninja (doxygen?)
(sudo -i
sudo pip3 install meson
exit
//or just
pip3 install meson)
```
In `/etc/dbus-1/system.d/ofono.conf` modify:
    ``<policy context="default">``
        ``<deny send_destination="org.ofono"/>``
    ``</policy>``
To:
  ``<policy context="default">``
        ``<allow send_destination="org.ofono"/>``
  ``</policy>``
 
```sh
git clone -b stable-14.x https://gitlab.freedesktop.org/pulseaudio/pulseaudio.git
```

In `src/modules/bluetooth/backend-native.c` and `src/modules/bluetooth/backend-ofono.c` change ``*imtu = 48;`` to ``*imtu = 60;``

```sh
sudo apt-get build-dep pulseaudio
(sudo reboot)
cd pulseaudio
./bootstrap.sh && make && sudo make install && ldconfig
```

Optionally limit profiles to HFP only in `/etc/pulse/default.pa` by adding:
`.ifexists module-bluetooth-discover.so
load-module module-bluetooth-discover headset=ofono
.endif`
