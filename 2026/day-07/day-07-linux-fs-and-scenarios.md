# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Part 1: Linux File System Hierarchy (30 minutes)

**Core Directories (Must Know):**
- `/` (root) - The starting point of everything

After running, `ls -l /`, we get:
```bash
total 1048656
lrwxrwxrwx   1 root root          7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root       4096 Feb 26  2024 bin.usr-is-merged
drwxr-xr-x   5 root root       4096 May 16 19:58 boot
drwxr-xr-x  17 root root       3820 May 30 14:27 dev
drwxr-xr-x 118 root root       4096 May 30 14:27 etc
drwxr-xr-x   3 root root       4096 Feb 10  2025 home
lrwxrwxrwx   1 root root          7 Apr 22  2024 lib -> usr/lib
drwxr-xr-x   2 root root       4096 Apr  8  2024 lib.usr-is-merged
lrwxrwxrwx   1 root root          9 Apr 22  2024 lib64 -> usr/lib64
drwx------   2 root root      16384 Jan 22  2025 lost+found
drwxr-xr-x   2 root root       4096 Jan 22  2025 media
drwxr-xr-x   2 root root       4096 Jan 22  2025 mnt
drwxr-xr-x   5 root root       4096 May 30 14:27 opt
dr-xr-xr-x 184 root root          0 May 30 14:27 proc
drwx------   4 root root       4096 May 16 20:00 root
drwxr-xr-x  37 root root       1000 May 30 15:05 run
lrwxrwxrwx   1 root root          8 Apr 22  2024 sbin -> usr/sbin
drwxr-xr-x   2 root root       4096 Mar 31  2024 sbin.usr-is-merged
drwxr-xr-x   2 root root       4096 Feb 10  2025 snap
drwxr-xr-x   2 root root       4096 Jan 22  2025 srv
-rw-------   1 root root 1073741824 May 30 14:27 swapfile
dr-xr-xr-x  13 root root          0 May 30 15:07 sys
drwxrwxrwt  14 root root       4096 May 30 15:10 tmp
drwxr-xr-x  12 root root       4096 Jan 22  2025 usr
drwxr-xr-x  12 root root       4096 May 16 19:58 var
```
I would use this when I want to switch from directories.

---

- `/home` - User home directories
```bash
ubuntu
```
After running, `ls -l /home/`, we get:
```bash
total 4
drwxr-x--- 3 ubuntu ubuntu 4096 Feb 10  2025 ubuntu
```
I would use this when creating new users and managing them.

---

- `/root` - Root user's home directory
```bash
filesystem
```
After running, `ls -l /root/`, we get:
```bash
total 0
lrwxrwxrwx 1 root root 1 May 16 19:59 filesystem -> /
```
I would use this when switching user.

--

- `/etc` - Configuration files
```bash
ModemManager            cloud                 debian_version  gnutls           iproute2       locale.gen      multipath               passwd-     rc6.d          ssh                ucf.conf
NetworkManager          cni                   default         gprofng.rc       iscsi          localtime       multipath.conf          perl        rcS.d          ssl                udev
PackageKit              console-setup         deluser.conf    groff            issue          logcheck        nanorc                  php         resolv.conf    subgid             udisks2
X11                     containerd            depmod.d        group            issue.net      login.defs      needrestart             pki         rmt            subgid-            ufw
adduser.conf            containers            dhcp            group-           kernel         logrotate.conf  netconfig               plymouth    rpc            subuid             update-manager
alternatives            credstore             dhcpcd.conf     grub.d           killercoda     logrotate.d     netplan                 pm          rsyslog.conf   subuid-            update-motd.d
apache2                 credstore.encrypted   dkms            gshadow          landscape      lsb-release     network                 polkit-1    rsyslog.d      sudo.conf          update-notifier
apparmor                crictl.yaml           dnsmasq.d       gshadow-         ld.so.cache    lvm             networkd-dispatcher     pollinate   screenrc       sudo_logsrvd.conf  usb_modeswitch.conf
apparmor.d              cron.d                docker          gss              ld.so.conf     machine-id      networks                profile     security       sudoers            usb_modeswitch.d
apport                  cron.daily            dpkg            hdparm.conf      ld.so.conf.d   magic           newt                    profile.d   selinux        sudoers.d          vconsole.conf
apt                     cron.hourly           e2scrub.conf    host.conf        ldap           magic.mime      nftables.conf           protocols   sensors.d      supercat           vim
bash.bashrc             cron.monthly          ec2_version     hostname         legal          manpath.config  nsswitch.conf           python3     sensors3.conf  sysctl.conf        vmware-tools
bash_completion         cron.weekly           environment     hosts            libaudit.conf  mdadm           opt                     python3.12  services       sysctl.d           vtrgb
bash_completion.d       cron.yearly           ethertypes      hosts.allow      libblockdev    mime.types      os-release              rc0.d       sgml           sysstat            wgetrc
bindresvport.blacklist  crontab               fonts           hosts.deny       libibverbs.d   mke2fs.conf     overlayroot.conf        rc1.d       shadow         systemd            xattr.conf
binfmt.d                cryptsetup-initramfs  fstab           init             libnl-3        modprobe.d      overlayroot.local.conf  rc2.d       shadow-        terminfo           xdg
byobu                   crypttab              fuse.conf       init.d           lighttpd       modules         pam.conf                rc3.d       shells         timezone           xml
ca-certificates         dbus-1                fwupd           initramfs-tools  locale.alias   modules-load.d  pam.d                   rc4.d       skel           tmpfiles.d         zsh_command_not_found
ca-certificates.conf    debconf.conf          gai.conf        inputrc          locale.conf    mtab            passwd                  rc5.d       sos            ubuntu-advantage
```
After running, `ls -l /etc/`, we get:
```bash
total 988
drwxr-xr-x 4 root root       4096 Jan 22  2025 ModemManager
drwxr-xr-x 3 root root       4096 May 16 19:58 NetworkManager
drwxr-xr-x 2 root root       4096 May 16 19:58 PackageKit
drwxr-xr-x 4 root root       4096 Jan 22  2025 X11
-rw-r--r-- 1 root root       3444 Jul  5  2023 adduser.conf
drwxr-xr-x 2 root root       4096 May 16 19:59 alternatives
drwxr-xr-x 3 root root       4096 May 16 19:59 apache2
drwxr-xr-x 2 root root       4096 May 16 19:58 apparmor
drwxr-xr-x 9 root root      12288 May 16 19:59 apparmor.d
drwxr-xr-x 3 root root       4096 May 16 19:58 apport
drwxr-xr-x 8 root root       4096 Jan 22  2025 apt
-rw-r--r-- 1 root root       2319 Mar 31  2024 bash.bashrc
-rw-r--r-- 1 root root         45 Jan 24  2020 bash_completion
drwxr-xr-x 2 root root       4096 May 16 19:58 bash_completion.d
-rw-r--r-- 1 root root        367 Aug  2  2022 bindresvport.blacklist
drwxr-xr-x 2 root root       4096 Apr 19  2024 binfmt.d
drwxr-xr-x 2 root root       4096 Jan 22  2025 byobu
drwxr-xr-x 3 root root       4096 Jan 22  2025 ca-certificates
-rw-r--r-- 1 root root       6288 Jan 22  2025 ca-certificates.conf
drwxr-xr-x 5 root root       4096 May 16 19:58 cloud
drwxr-xr-x 3 root root       4096 May 16 19:58 cni
drwxr-xr-x 2 root root       4096 Jan 22  2025 console-setup
drwxr-xr-x 3 root root       4096 May 16 19:59 containerd
drwxr-xr-x 5 root root       4096 May 16 19:59 containers
drwx------ 2 root root       4096 Apr 19  2024 credstore
drwx------ 2 root root       4096 Apr 19  2024 credstore.encrypted
-rw-r--r-- 1 root root         57 May 16 19:59 crictl.yaml
drwxr-xr-x 2 root root       4096 May 16 19:59 cron.d
drwxr-xr-x 2 root root       4096 May 16 19:58 cron.daily
drwxr-xr-x 2 root root       4096 Jan 22  2025 cron.hourly
drwxr-xr-x 2 root root       4096 Jan 22  2025 cron.monthly
drwxr-xr-x 2 root root       4096 Jan 22  2025 cron.weekly
drwxr-xr-x 2 root root       4096 Jan 22  2025 cron.yearly
-rw-r--r-- 1 root root       1188 May 16 20:00 crontab
drwxr-xr-x 2 root root       4096 May 16 19:58 cryptsetup-initramfs
-rw-r--r-- 1 root root         54 Jan 22  2025 crypttab
drwxr-xr-x 4 root root       4096 Jan 22  2025 dbus-1
-rw-r--r-- 1 root root       2967 Apr 12  2024 debconf.conf
-rw-r--r-- 1 root root         11 Apr 22  2024 debian_version
drwxr-xr-x 3 root root       4096 May 16 19:58 default
-rw-r--r-- 1 root root       1706 Jul  5  2023 deluser.conf
drwxr-xr-x 2 root root       4096 May 16 19:57 depmod.d
drwxr-xr-x 3 root root       4096 Jan 22  2025 dhcp
-rw-r--r-- 1 root root       1429 Mar 31  2024 dhcpcd.conf
drwxr-xr-x 3 root root       4096 May 16 19:59 dkms
drwxr-xr-x 2 root root       4096 May 16 19:58 dnsmasq.d
drwxr-xr-x 2 root root       4096 May 16 19:59 docker
drwxr-xr-x 4 root root       4096 May 16 19:59 dpkg
-rw-r--r-- 1 root root        685 Apr  8  2024 e2scrub.conf
-rw-r--r-- 1 root root         34 Jan 22  2025 ec2_version
-rw-r--r-- 1 root root        106 Jan 22  2025 environment
-rw-r--r-- 1 root root       1853 Oct 17  2022 ethertypes
drwxr-xr-x 4 root root       4096 Jan 22  2025 fonts
-rw-r--r-- 1 root root        146 Jan 22  2025 fstab
-rw-r--r-- 1 root root        694 Apr  8  2024 fuse.conf
drwxr-xr-x 4 root root       4096 May 16 19:58 fwupd
-rw-r--r-- 1 root root       2584 Jan 31  2024 gai.conf
drwxr-xr-x 2 root root       4096 May 16 19:57 gnutls
-rw-r--r-- 1 root root       3986 Feb  9 21:03 gprofng.rc
drwxr-xr-x 2 root root       4096 Jan 22  2025 groff
-rw-r--r-- 1 root root        843 May 16 19:58 group
-rw-r--r-- 1 root root        829 Feb 10  2025 group-
drwxr-xr-x 2 root root       4096 May 16 19:58 grub.d
-rw-r----- 1 root shadow      711 May 16 19:58 gshadow
-rw-r----- 1 root shadow      700 Feb 10  2025 gshadow-
drwxr-xr-x 3 root root       4096 Jan 22  2025 gss
-rw-r--r-- 1 root root       4436 Oct  6  2022 hdparm.conf
-rw-r--r-- 1 root root         92 Apr 22  2024 host.conf
-rw-r--r-- 1 root root          7 May 16 19:58 hostname
-rw-r--r-- 1 root root        255 May 16 19:58 hosts
-rw-r--r-- 1 root root        411 Jan 22  2025 hosts.allow
-rw-r--r-- 1 root root        711 Jan 22  2025 hosts.deny
drwxr-xr-x 2 root root       4096 May 16 19:58 init
drwxr-xr-x 2 root root       4096 May 16 19:58 init.d
drwxr-xr-x 5 root root       4096 May 16 19:58 initramfs-tools
-rw-r--r-- 1 root root       1875 Mar 31  2024 inputrc
drwxr-xr-x 4 root root       4096 May 16 19:57 iproute2
drwxr-xr-x 2 root root       4096 May 16 19:57 iscsi
-rw-r--r-- 1 root root         26 Feb  6 07:23 issue
-rw-r--r-- 1 root root         19 Feb  6 07:23 issue.net
drwxr-xr-x 7 root root       4096 May 16 19:59 kernel
drwxr-xr-x 2 root root       4096 May 30 14:27 killercoda
drwxrwxr-x 2 root landscape  4096 May 28  2024 landscape
-rw-r--r-- 1 root root      25971 May 16 19:59 ld.so.cache
-rw-r--r-- 1 root root         34 Aug  2  2022 ld.so.conf
drwxr-xr-x 2 root root       4096 May 16 19:59 ld.so.conf.d
drwxr-xr-x 2 root root       4096 May 16 19:57 ldap
-rw-r--r-- 1 root root        267 Apr 22  2024 legal
-rw-r--r-- 1 root root        191 Mar 31  2024 libaudit.conf
drwxr-xr-x 3 root root       4096 Jan 22  2025 libblockdev
drwxr-xr-x 2 root root       4096 May 16 19:58 libibverbs.d
drwxr-xr-x 2 root root       4096 May 16 19:57 libnl-3
drwxr-xr-x 4 root root       4096 May 16 19:59 lighttpd
-rw-r--r-- 1 root root       2996 Mar 30  2024 locale.alias
-rw-r--r-- 1 root root         13 Jan 22  2025 locale.conf
-rw-r--r-- 1 root root       9563 May 16 19:57 locale.gen
lrwxrwxrwx 1 root root         27 Jan 22  2025 localtime -> /usr/share/zoneinfo/Etc/UTC
drwxr-xr-x 4 root root       4096 Jan 22  2025 logcheck
-rw-r--r-- 1 root root      12345 Feb 22  2024 login.defs
-rw-r--r-- 1 root root        586 Apr  8  2024 logrotate.conf
drwxr-xr-x 2 root root       4096 May 16 19:58 logrotate.d
-rw-r--r-- 1 root root        104 Feb  6 07:22 lsb-release
drwxr-xr-x 3 root root       4096 May 16 19:58 lvm
-r--r--r-- 1 root root         33 Feb 10  2025 machine-id
-rw-r--r-- 1 root root        111 Mar 31  2024 magic
-rw-r--r-- 1 root root        111 Mar 31  2024 magic.mime
-rw-r--r-- 1 root root       5230 Apr  8  2024 manpath.config
drwxr-xr-x 2 root root       4096 Jan 22  2025 mdadm
-rw-r--r-- 1 root root      75113 Jul 12  2023 mime.types
-rw-r--r-- 1 root root        744 Apr  8  2024 mke2fs.conf
drwxr-xr-x 2 root root       4096 May 16 19:59 modprobe.d
-rw-r--r-- 1 root root        212 Jan 22  2025 modules
drwxr-xr-x 2 root root       4096 May 16 19:59 modules-load.d
lrwxrwxrwx 1 root root         19 Jan 22  2025 mtab -> ../proc/self/mounts
drwx------ 2 root root       4096 Feb 10  2025 multipath
-rw-r--r-- 1 root root         41 Apr  7  2024 multipath.conf
-rw-r--r-- 1 root root      11424 May 23  2023 nanorc
drwxr-xr-x 6 root root       4096 Jan 22  2025 needrestart
-rw-r--r-- 1 root root        767 Mar 31  2024 netconfig
drwxr-xr-x 2 root root       4096 Feb 10  2025 netplan
drwxr-xr-x 6 root root       4096 May 16 19:58 network
drwxr-xr-x 8 root root       4096 Jan 22  2025 networkd-dispatcher
-rw-r--r-- 1 root root         91 Apr 22  2024 networks
drwxr-xr-x 2 root root       4096 Jan 22  2025 newt
-rwxr-xr-x 1 root root        243 Oct 19  2023 nftables.conf
-rw-r--r-- 1 root root        526 Jan 22  2025 nsswitch.conf
drwxr-xr-x 2 root root       4096 Jan 22  2025 opt
lrwxrwxrwx 1 root root         21 Feb  6 07:23 os-release -> ../usr/lib/os-release
-rw-r--r-- 1 root root       6920 Jul  3  2024 overlayroot.conf
-rw-r--r-- 1 root root        112 Jan 22  2025 overlayroot.local.conf
-rw-r--r-- 1 root root        552 Oct 13  2022 pam.conf
drwxr-xr-x 2 root root       4096 May 16 19:58 pam.d
-rw-r--r-- 1 root root       1811 May 16 19:58 passwd
-rw-r--r-- 1 root root       1751 Feb 10  2025 passwd-
drwxr-xr-x 3 root root       4096 Jan 22  2025 perl
drwxr-xr-x 3 root root       4096 May 16 19:59 php
drwxr-xr-x 4 root root       4096 Jan 22  2025 pki
drwxr-xr-x 2 root root       4096 Mar 31  2024 plymouth
drwxr-xr-x 3 root root       4096 Jan 22  2025 pm
drwxr-xr-x 3 root root       4096 Jan 22  2025 polkit-1
drwxr-xr-x 2 root root       4096 May 16 19:58 pollinate
-rw-r--r-- 1 root root        582 Apr 22  2024 profile
drwxr-xr-x 2 root root       4096 May 16 19:58 profile.d
-rw-r--r-- 1 root root       3144 Oct 17  2022 protocols
drwxr-xr-x 2 root root       4096 Jan 22  2025 python3
drwxr-xr-x 2 root root       4096 May 16 19:57 python3.12
drwxr-xr-x 2 root root       4096 May 16 19:58 rc0.d
drwxr-xr-x 2 root root       4096 May 16 19:58 rc1.d
drwxr-xr-x 2 root root       4096 May 16 19:58 rc2.d
drwxr-xr-x 2 root root       4096 May 16 19:58 rc3.d
drwxr-xr-x 2 root root       4096 May 16 19:58 rc4.d
drwxr-xr-x 2 root root       4096 May 16 19:58 rc5.d
drwxr-xr-x 2 root root       4096 May 16 19:58 rc6.d
drwxr-xr-x 2 root root       4096 Jan 22  2025 rcS.d
lrwxrwxrwx 1 root root         32 May 30 14:27 resolv.conf -> /run/systemd/resolve/resolv.conf
lrwxrwxrwx 1 root root         13 Apr  8  2024 rmt -> /usr/sbin/rmt
-rw-r--r-- 1 root root        911 Oct 17  2022 rpc
-rw-r--r-- 1 root root       1213 Mar 22  2024 rsyslog.conf
drwxr-xr-x 2 root root       4096 May 16 19:58 rsyslog.d
-rw-r--r-- 1 root root       3663 Jun 20  2016 screenrc
drwxr-xr-x 4 root root       4096 May 16 19:57 security
drwxr-xr-x 2 root root       4096 Jan 22  2025 selinux
drwxr-xr-x 2 root root       4096 Jan 22  2025 sensors.d
-rw-r--r-- 1 root root      10593 Mar 31  2024 sensors3.conf
-rw-r--r-- 1 root root      12813 Mar 27  2021 services
drwxr-xr-x 2 root root       4096 Jan 22  2025 sgml
-rw-r----- 1 root shadow     1002 May 16 19:58 shadow
-rw-r----- 1 root shadow      980 Feb 10  2025 shadow-
-rw-r--r-- 1 root root        148 May 16 19:58 shells
drwxr-xr-x 2 root root       4096 Jan 22  2025 skel
drwxr-xr-x 6 root root       4096 May 16 19:58 sos
drwxr-xr-x 4 root root       4096 May 16 19:58 ssh
drwxr-xr-x 4 root root       4096 May 16 19:57 ssl
-rw-r--r-- 1 root root         45 Feb 10  2025 subgid
-rw-r--r-- 1 root root         20 Feb 10  2025 subgid-
-rw-r--r-- 1 root root         45 Feb 10  2025 subuid
-rw-r--r-- 1 root root         20 Feb 10  2025 subuid-
-rw-r--r-- 1 root root       4343 Apr  8  2024 sudo.conf
-rw-r--r-- 1 root root       9804 Apr  8  2024 sudo_logsrvd.conf
-r--r----- 1 root root       1800 Jan 29  2024 sudoers
drwxr-x--- 2 root root       4096 May 16 19:57 sudoers.d
drwxr-xr-x 2 root root       4096 Jan 22  2025 supercat
-rw-r--r-- 1 root root       2244 May 16 19:59 sysctl.conf
drwxr-xr-x 2 root root       4096 May 16 19:59 sysctl.d
drwxr-xr-x 2 root root       4096 Jan 22  2025 sysstat
drwxr-xr-x 6 root root       4096 May 30 14:27 systemd
drwxr-xr-x 2 root root       4096 Jan 22  2025 terminfo
-rw-r--r-- 1 root root          8 May 16 19:57 timezone
drwxr-xr-x 2 root root       4096 Jan 22  2025 tmpfiles.d
drwxr-xr-x 2 root root       4096 May 16 19:58 ubuntu-advantage
-rw-r--r-- 1 root root       1260 Jan 27  2023 ucf.conf
drwxr-xr-x 4 root root       4096 May 16 19:57 udev
drwxr-xr-x 2 root root       4096 May 16 19:58 udisks2
drwxr-xr-x 3 root root       4096 Jan 22  2025 ufw
drwxr-xr-x 3 root root       4096 May 16 19:58 update-manager
drwxr-xr-x 2 root root       4096 May 16 19:58 update-motd.d
drwxr-xr-x 2 root root       4096 Apr  8  2024 update-notifier
-rw-r--r-- 1 root root       1523 Apr  8  2024 usb_modeswitch.conf
drwxr-xr-x 2 root root       4096 Dec 16  2023 usb_modeswitch.d
lrwxrwxrwx 1 root root         16 Jan 22  2025 vconsole.conf -> default/keyboard
drwxr-xr-x 2 root root       4096 May 16 19:57 vim
drwxr-xr-x 4 root root       4096 May 16 19:58 vmware-tools
lrwxrwxrwx 1 root root         23 Feb 26  2024 vtrgb -> /etc/alternatives/vtrgb
-rw-r--r-- 1 root root       4942 Jun 19  2024 wgetrc
-rw-r--r-- 1 root root        681 Apr  8  2024 xattr.conf
drwxr-xr-x 4 root root       4096 Jan 22  2025 xdg
drwxr-xr-x 2 root root       4096 May 16 19:58 xml
-rw-r--r-- 1 root root        460 Jan 20  2023 zsh_command_not_found
```
I would use this when managing users, system state and booting, downloading software packages, and networking.

--

- `/var/log` - Log files (very important for DevOps!)
```bash
README            apport.log  auth.log    btmp                   cloud-init.log  dmesg    dmesg.1.gz  dpkg.log  kern.log    killercoda  lastlog  syslog    sysstat              wtmp
alternatives.log  apt         auth.log.1  cloud-init-output.log  dist-upgrade    dmesg.0  dmesg.2.gz  journal   kern.log.1  landscape   private  syslog.1  unattended-upgrades
```
After running, `ls -l /var/log/`, we get:
```bash
total 1360
lrwxrwxrwx  1 root      root                39 Jan 22  2025 README -> ../../usr/share/doc/systemd/README.logs
-rw-r--r--  1 root      root             14881 May 16 19:59 alternatives.log
-rw-r-----  1 root      adm                  0 Feb 10  2025 apport.log
drwxr-xr-x  2 root      root              4096 May 16 20:00 apt
-rw-r-----  1 syslog    adm               4806 May 30 15:39 auth.log
-rw-r-----  1 syslog    adm              15514 May 16 19:59 auth.log.1
-rw-rw----  1 root      utmp               384 Feb 10  2025 btmp
-rw-r-----  1 root      adm               5536 Feb 10  2025 cloud-init-output.log
-rw-r-----  1 syslog    adm              93479 Feb 10  2025 cloud-init.log
drwxr-xr-x  2 root      root              4096 Sep  5  2024 dist-upgrade
-rw-r-----  1 root      adm              59124 May 30 14:27 dmesg
-rw-r-----  1 root      adm              59193 May 16 19:57 dmesg.0
-rw-r-----  1 root      adm              15202 May 16 19:56 dmesg.1.gz
-rw-r-----  1 root      adm              15317 Feb 10  2025 dmesg.2.gz
-rw-r--r--  1 root      root            282113 May 16 20:00 dpkg.log
drwxr-sr-x+ 3 root      systemd-journal   4096 Feb 10  2025 journal
-rw-r-----  1 syslog    adm              66327 May 30 14:27 kern.log
-rw-r-----  1 syslog    adm             143066 May 16 19:59 kern.log.1
drwxr-xr-x  2 root      root              4096 May 30 14:49 killercoda
drwxr-xr-x  2 landscape landscape         4096 Jan 22  2025 landscape
-rw-rw-r--  1 root      utmp               292 Feb 10  2025 lastlog
drwx------  2 root      root              4096 Feb 10  2025 private
-rw-r-----  1 syslog    adm             150958 May 30 15:40 syslog
-rw-r-----  1 syslog    adm             387880 May 16 20:00 syslog.1
drwxr-xr-x  2 root      root              4096 May 30 14:27 sysstat
drwxr-x---  2 root      adm               4096 Feb 10  2025 unattended-upgrades
-rw-rw-r--  1 root      utmp              9600 May 30 14:27 wtmp
```
I would use this when system troubleshooting, infrastructure monitoring, automated alerting, and security auditing.

--

- `/tmp` - Temporary files
```bash
github-remote                                                                   systemd-private-a070a4eedb75412e859516cff594bda5-systemd-resolved.service-vmmvWZ
http-remote                                                                     systemd-private-a070a4eedb75412e859516cff594bda5-systemd-timesyncd.service-UB3NZA
systemd-private-a070a4eedb75412e859516cff594bda5-ModemManager.service-METbm8    theia-install-run
systemd-private-a070a4eedb75412e859516cff594bda5-polkit.service-ZyPJ0W          theia_upload
systemd-private-a070a4eedb75412e859516cff594bda5-systemd-logind.service-JLUyDu
```
After running, `ls -l /tmp/`, we get:
```bash
total 36
drwxr-xr-x 2 root root 4096 May 30 14:27 github-remote
drwxr-xr-x 2 root root 4096 May 30 14:27 http-remote
drwx------ 3 root root 4096 May 30 14:27 systemd-private-a070a4eedb75412e859516cff594bda5-ModemManager.service-METbm8
drwx------ 3 root root 4096 May 30 14:27 systemd-private-a070a4eedb75412e859516cff594bda5-polkit.service-ZyPJ0W
drwx------ 3 root root 4096 May 30 14:27 systemd-private-a070a4eedb75412e859516cff594bda5-systemd-logind.service-JLUyDu
drwx------ 3 root root 4096 May 30 14:27 systemd-private-a070a4eedb75412e859516cff594bda5-systemd-resolved.service-vmmvWZ
drwx------ 3 root root 4096 May 30 14:27 systemd-private-a070a4eedb75412e859516cff594bda5-systemd-timesyncd.service-UB3NZA
-rw-r--r-- 1 root root    6 May 30 14:27 theia-install-run
drwxr-xr-x 2 root root 4096 May 30 14:27 theia_upload
```
I would use this when storing temporary, transient files.

--

**Additional Directories (Good to Know):**
- `/bin` - Essential command binaries
- `/usr/bin` - User command binaries
- `/opt` - Optional/third-party applications
