~~~
openSUSE Leap 15.1
4.12.14-lp151.28.32-default (4.12.14-lp151.28.32.1)
~~~

## Dev

Create a live memory dump with virtualbox:

~~~
$ VBoxManage debugvm 'openSUSE Leap 15.1' dumpvmcore --filename=chall.raw
~~~

## Solution

Download ISO:

- [Direct download][3]
- [Torrent][4]

Install the VM.

Install the right kernel version.

~~~
$ zypper info kernel-default
# zypper in -f kernel-default-4.12.14-lp151.28.32.1
~~~

Install `dwarfdump`, `gcc`/`make` and kernel headers as required in the [Volatility wiki][1].
Also install `zip` to be able to create the profiel archive.

~~~
# zypper update
# zypper in libdwarf-tools kernel-default-devel zip
# zypper install -t pattern devel_basis
# reboot
~~~

[Creating vtypes][2]

~~~
$ cd /tmp
$ git clone https://github.com/volatilityfoundation/volatility.git
$ cd volatility/tools/linux
$ make
$ head module.dwarf
~~~

[Making the profile][5]

~~~
$ cd /tmp
$ sudo zip openSUSELeap151.zip volatility/tools/linux/module.dwarf /boot/System.map-4.12.14-lp151.28.32-default
~~~

Use a bridge adapter or something and share the profile zip with a HTTP server:

~~~
$ python3 -m http.server 1337
~~~

And retrieve it on your CTF machine:

~~~
$ wget http://192.168.1.94:1337/openSUSELeap151.zip
~~~


[1]:https://github.com/volatilityfoundation/volatility/wiki/Linux#creating-a-new-profile
[2]:https://github.com/volatilityfoundation/volatility/wiki/Linux#creating-vtypes
[3]:https://download.opensuse.org/distribution/leap/15.1/iso/openSUSE-Leap-15.1-DVD-x86_64.iso
[4]:https://download.opensuse.org/distribution/leap/15.1/iso/openSUSE-Leap-15.1-DVD-x86_64.iso.torrent
[5]:https://github.com/volatilityfoundation/volatility/wiki/Linux#making-the-profile
