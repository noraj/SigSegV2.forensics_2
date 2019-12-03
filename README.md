# Volatility profile creation + 10 basic questions

## Version

Date        | Author                  | Contact               | Version | Comment
---         | ---                     | ---                   | ---     | ---
20/11/2019  | noraj (Alexandre ZANNI) | noraj#0833 on discord | 1.0     | Document creation

Information displayed for CTF players:

+ **Name of the challenge** / **Nom du challenge**: `10 questions about my system`
+ **Category** / **Catégorie**: `Forensics`
+ **Internet**: not needed
+ **Difficulty** / **Difficulté**: Medium / Moyen

### Description

Attach the memory dump.

```
Information about the memory dump:

openSUSE Leap 15.1
4.12.14-lp151.28.32-default (4.12.14-lp151.28.32.1)

Question n°X <here>

Flag format: sigsegv{sha1(flag)}

author: [noraj](https://pwn.by/noraj/)
```

### Hints

- Hint1: create volatility profile
- Hint2: share the volatility profile

## Integration

Attach the memory dump.

## Solving

### Author solution

I used [ctf-party](https://rubygems.org/gems/ctf-party) to generate flag quicker.

Here is the config used:

```ruby
$ ctf_party_console
irb(main):001:0> String.flag = {prefix: 'sigsegv', digest: 'sha1'}
```

**Questions**

#### 1 - command time

What is the command that noraj used at `2019-11-19 22:57:38 UTC+0000`?

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_bash | grep '2019-11-19 22:57:38 UTC+0000'
...
    1498 bash                 2019-11-19 22:57:38 UTC+0000   python3 -m http.server 1337
...
~~~

```ruby
irb(main):002:0> 'python3 -m http.server 1337'.flag
=> "sigsegv{1d4893cc25cc5453be125227fb8ac34988c29ad0}"
```

#### 2 - gcc version

What is the gcc version used to compile the kernel? (full string)

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_banner
Volatility Foundation Volatility Framework 2.6.1
Linux version 4.12.14-lp151.28.32-default (geeko@buildhost) (gcc version 7.4.1 20190905 [gcc-7-branch revision 275407] (SUSE Linux) ) #1 SMP Wed Nov 13 07:50:15 UTC 2019 (6e1aaad)
~~~

```ruby
irb(main):002:0> 'gcc version 7.4.1 20190905 [gcc-7-branch revision 275407] (SUSE Linux)'.flag
=> "sigsegv{524993e05b0ded8e112a134c68d04b319de13423}"
```

#### 3 - debug message

What is the debug message at `1105416124.1`?

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_dmesg | grep '1105416124.1'
Volatility Foundation Volatility Framework 2.6.1
[1105416124.1]   Magic number: 11:134:907
~~~

```ruby
irb(main):003:0> 'Magic number: 11:134:907'.flag
=> "sigsegv{77038af8d03d8b4cdb28e6e592b87a2c3195d1fe}"
```

#### 4 - IP / MAC

What is the IP address of eth0 and its MAC address? (concatenate the answer)

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_ifconfig
Volatility Foundation Volatility Framework 2.6.1
Interface        IP Address           MAC Address        Promiscous Mode
---------------- -------------------- ------------------ ---------------
lo               127.0.0.1            00:00:00:00:00:00  False
eth0             192.168.1.94         08:00:27:93:fe:6c  False
lo               127.0.0.1            00:00:00:00:00:00  False
~~~

```ruby
'192.168.1.9408:00:27:93:fe:6c'.flag
=> "sigsegv{cba34462a24124ada4e4882a4db5104b254343f8}"
```

#### 5 - libraries

What is the 3rd library loaded by sshd? (full path)

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_library_list | grep sshd
Volatility Foundation Volatility Framework 2.6.1
sshd                 1271 0x00007f0e8ec8a000 /usr/lib64/libgpg-error.so.0
sshd                 1271 0x00007f0e8eeaa000 /lib64/libresolv.so.2
sshd                 1271 0x00007f0e8f0c1000 /usr/lib64/libkeyutils.so.1
sshd                 1271 0x00007f0e8f2c5000 /usr/lib64/libkrb5support.so.0
sshd                 1271 0x00007f0e8f4d2000 /usr/lib64/libk5crypto.so.3
sshd                 1271 0x00007f0e8f704000 /lib64/libpthread.so.0
sshd                 1271 0x00007f0e8f922000 /usr/lib64/libgcrypt.so.20
sshd                 1271 0x00007f0e8fc3e000 /usr/lib64/liblz4.so.1
sshd                 1271 0x00007f0e8fe53000 /usr/lib64/liblzma.so.5
sshd                 1271 0x00007f0e9008d000 /lib64/librt.so.1
sshd                 1271 0x00007f0e90295000 /usr/lib64/libcap.so.2
sshd                 1271 0x00007f0e927e7000 /lib64/ld-linux-x86-64.so.2
sshd                 1271 0x00007f0e9049a000 /usr/lib64/libpcre.so.1
sshd                 1271 0x00007f0e90727000 /lib64/libdl.so.2
sshd                 1271 0x00007f0e9092b000 /lib64/libc.so.6
sshd                 1271 0x00007f0e90ce5000 /lib64/libcom_err.so.2
sshd                 1271 0x00007f0e90ee9000 /usr/lib64/libkrb5.so.3
sshd                 1271 0x00007f0e911c5000 /usr/lib64/libgssapi_krb5.so.2
sshd                 1271 0x00007f0e91411000 /lib64/libcrypt.so.1
sshd                 1271 0x00007f0e9164c000 /lib64/libz.so.1
sshd                 1271 0x00007f0e91863000 /lib64/libutil.so.1
sshd                 1271 0x00007f0e91a66000 /usr/lib64/libcrypto.so.1.1
sshd                 1271 0x00007f0e91ef4000 /usr/lib64/libsystemd.so.0
sshd                 1271 0x00007f0e92189000 /lib64/libselinux.so.1
sshd                 1271 0x00007f0e923b2000 /lib64/libpam.so.0
sshd                 1271 0x00007f0e925c1000 /usr/lib64/libaudit.so.1
sshd                 1271 0x00007ffda9814000 linux-vdso.so.1
~~~

```ruby
irb(main):005:0> '/usr/lib64/libkeyutils.so.1'.flag
=> "sigsegv{bcf5ae7945ce4e711d11861ec2cc1f75efbe758b}"
```

#### 6 - FS and mount

What is the file system and mount options of `/tmp`? (concatenate the answer)

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_mount
Volatility Foundation Volatility Framework 2.6.1
tmpfs                     /@/sys/fs/cgroup                    tmpfs        ro,nosuid,nodev,noexec
proc                      /@/proc                             proc         rw,relatime,nosuid,nodev,noexec
devtmpfs                  /@/dev                              devtmpfs     rw,nosuid
/dev/sda2                 /@/@/tmp/tmp                        btrfs        rw,relatime
sysfs                     /@/sys                              sysfs        rw,relatime,nosuid,nodev,noexec
cgroup                    /@/sys/fs/cgroup/perf_event         cgroup       rw,relatime,nosuid,nodev,noexec
systemd-1                 /@/proc/sys/fs/binfmt_misc          autofs       rw,relatime
...
~~~

```ruby
irb(main):006:0> 'btrfsrw,relatime'.flag
=> "sigsegv{d27802b77f4a14b3745ab47aaa86cfdc3c231394}"
```

#### 7 - UNIX socket

What is the service name/pid of the process using UNIX socket 18707?

```
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_netstat | grep 18707
Volatility Foundation Volatility Framework 2.6.1
UNIX 18707        wickedd-nanny/866
```

```ruby
irb(main):007:0> 'wickedd-nanny/866'.flag
=> "sigsegv{6b95288247a023c860fe0848f3990cc25ce7d697}"
```

#### 8 - ps

What is the full command of pid 364?

~~~
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_psaux -p 364
Volatility Foundation Volatility Framework 2.6.1
Pid    Uid    Gid    Arguments                                                       
364    0      0      /usr/sbin/haveged -w 1024 -v 0 -F 
~~~

```ruby
irb(main):012:0> '/usr/sbin/haveged -w 1024 -v 0 -F'.flag
=> "sigsegv{4c0bed3d6381e2014d77a35e6931d604e4bd8ec1}"
```

#### 9 - INVOCATION_ID

What is the invocation id of bash?

```
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_psenv -p 1498
Volatility Foundation Volatility Framework 2.6.1
Name   Pid    Environment 
bash              1498   LANG= PATH=/usr/local/bin:/bin:/usr/bin INVOCATION_ID=d6dd8e717833428bac595d565958fdf4 TERM=linux JOURNAL_STREAM=9:21427 LANGUAGE= LC_CTYPE= LC_NUMERIC= LC_TIME= LC_COLLATE= LC_MONETARY= LC_MESSAGES= LC_PAPER= LC_NAME= LC_ADDRESS= LC_TELEPHONE= LC_MEASUREMENT= LC_IDENTIFICATION= HOME=/home/noraj USER=noraj SHELL=/bin/bash MAIL=/var/mail/noraj LOGNAME=noraj XDG_SESSION_ID=1 XDG_RUNTIME_DIR=/run/user/1000 XDG_SEAT=seat0 XDG_VTNR=1
```

```ruby
irb(main):013:0> 'd6dd8e717833428bac595d565958fdf4'.flag
=> "sigsegv{a64603f7b14cdd948ab9f065befc350854e0a25d}"
```

#### 10 - PPID

What is the PPID of qmgr process?

```
$ volatility --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 -f chall.raw linux_pstree
Volatility Foundation Volatility Framework 2.6.1
Name                 Pid             Uid            
systemd              1                              
.systemd-journal     360                            
.haveged             364                            
.lvmetad             369                            
.systemd-udevd       376                            
.auditd              465                            
.dbus-daemon         668             499            
.wickedd-auto4       675                            
.wickedd-dhcp4       676                            
.wickedd-dhcp6       678                            
.rsyslogd            722                            
.nscd                690             479            
.systemd-logind      801                            
.wickedd             824                            
.wickedd-nanny       866                            
.iscsid              1236                           
.sshd                1271                           
.login               1314                           
..bash               1498            1000           
.master              1368                           
..pickup             1370            51             
..qmgr               1371            51             
.cron                1381                           
.systemd             1492            1000           
..(sd-pam)           1493            1000           
[kthreadd]           2                              
.[kworker/0:0H]      4
```

```ruby
irb(main):014:0> '1368'.flag
=> "sigsegv{570722b44ec7003126d686b70703051e72ff7408}"
```

---

**Profile**

~~~
openSUSE Leap 15.1
4.12.14-lp151.28.32-default (4.12.14-lp151.28.32.1)
~~~

#### (Dev)

Create a live memory dump with virtualbox:

~~~
$ VBoxManage debugvm 'openSUSE Leap 15.1' dumpvmcore --filename=chall.raw
~~~

#### Solution (create profile for volatility)

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

Put the profile in any directory.
Then search the profile name:

~~~
$ mkdir plugins
$ mv openSUSELeap151.zip plugins
$ volatility --plugins=plugins/ --info | grep openSUSELeap151
Volatility Foundation Volatility Framework 2.6.1
LinuxopenSUSELeap151x64 - A Profile for Linux openSUSELeap151 x64
~~~

Specify it in volatility options for each module you want to run, for example:

~~~
$ volatility -f chall.raw --plugins=plugins/ --profile=LinuxopenSUSELeap151x64 pslist
~~~


[1]:https://github.com/volatilityfoundation/volatility/wiki/Linux#creating-a-new-profile
[2]:https://github.com/volatilityfoundation/volatility/wiki/Linux#creating-vtypes
[3]:https://download.opensuse.org/distribution/leap/15.1/iso/openSUSE-Leap-15.1-DVD-x86_64.iso
[4]:https://download.opensuse.org/distribution/leap/15.1/iso/openSUSE-Leap-15.1-DVD-x86_64.iso.torrent
[5]:https://github.com/volatilityfoundation/volatility/wiki/Linux#making-the-profile

## SigSegV2

A forensics challenge that was available during SigSegV2 CTF (2019).

- Q1: 15 teams on 36 flaged this question.
- Q2: 19 teams on 36 flaged this question.
- Q3: 14 teams on 36 flaged this question.
- Q4: 16 teams on 36 flaged this question.
- Q5: 15 teams on 36 flaged this question.
- Q6: 15 teams on 36 flaged this question.
- Q7: 15 teams on 36 flaged this question.
- Q8: 15 teams on 36 flaged this question.
- Q9: 11 teams on 36 flaged this question.
- Q10: 15 teams on 36 flaged this question.

---

**This part is not used, just archived.**

~~~
openSUSE Leap 42.1
4.1.39-56-default (4.1.39-56.1)
~~~

## Dev

Create a live memory dump with virtualbox:

~~~
$ VBoxManage debugvm 'openSUSE Leap 42.1' dumpvmcore --filename=chall.raw
~~~

## Solution

Download ISO:

- [Direct download][3b]
- [Torrent][4b]

Install the VM.

Install the right kernel version.

~~~
$ zypper info kernel-default
# zypper in -f kernel-default-4.1.39-56.1
~~~

Install `dwarfdump`, `gcc`/`make` and kernel headers as required in the [Volatility wiki][1b].
Also install `zip` to be able to create the profiel archive.

~~~
# zypper update
# zypper in libdwarf-tools kernel-default-devel zip git
# zypper install -t pattern devel_basis
# reboot
~~~

[Creating vtypes][2b]

~~~
$ cd /tmp
$ git clone https://github.com/volatilityfoundation/volatility.git
$ cd volatility/tools/linux
$ make
$ head module.dwarf
~~~

[Making the profile][5b]

~~~
$ cd /tmp
$ zip openSUSELeap421.zip volatility/tools/linux/module.dwarf /boot/System.map-4.1.39-56-default
~~~

Use a bridge adapter or something and share the profile zip with a HTTP server:

~~~
$ python3 -m http.server 1337
~~~

And retrieve it on your CTF machine:

~~~
$ wget http://192.168.1.94:1337/openSUSELeap421.zip
~~~

Put the profile in any directory.
Then search the profile name:

~~~
$ mkdir plugins
$ mv openSUSELeap151.zip plugins
$ volatility --plugins=plugins/ --info | grep openSUSELeap421
Volatility Foundation Volatility Framework 2.6.1
LinuxopenSUSELeap421x64 - A Profile for Linux openSUSELeap421 x64
~~~

Specify it in volatility options for each module you want to run, for example:

~~~
$ volatility -f chall421.raw --plugins=plugins/ --profile=LinuxopenSUSELeap421x64 pslist
~~~


[1b]:https://github.com/volatilityfoundation/volatility/wiki/Linux#creating-a-new-profile
[2b]:https://github.com/volatilityfoundation/volatility/wiki/Linux#creating-vtypes
[3b]:http://ftp5.gwdg.de/pub/opensuse/discontinued/distribution/leap/42.1/iso/openSUSE-Leap-42.1-DVD-x86_64.iso
[4b]:http://ftp5.gwdg.de/pub/opensuse/discontinued/distribution/leap/42.1/iso/openSUSE-Leap-42.1-DVD-x86_64.iso.torrent
[5b]:https://github.com/volatilityfoundation/volatility/wiki/Linux#making-the-profile
