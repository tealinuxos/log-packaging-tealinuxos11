# log-packaging-tealinuxos11
===============================

-------------------------------------------

referensi `https://help.ubuntu.com/community/LiveCDCustomization`

## Panduan unpack iso pertama kali ##

```shell
$ sudo uck-remaster-unpack-iso namafile.iso
$ sudo uck-remaster-unpack-rootfs
$ sudo uck-remaster-chroot-rootfs
```

Jika sudah pernah melakukan unpack maka hanya perlu `sudo uck-remaster-chroot-rootfs`

------------------------------------

menemui kendala berupa
```
Creating apt cache...
Creating root home...
Mounting /proc
Mounting /sys
Mounting /dev/pts
Mounting /tmp
Mounting /home/catzy/tmp/remaster-root-home
Mounting /home/catzy/tmp/remaster-apt-cache
Mounting /run
Copying fstab/mtab...
cp: '/etc/mtab' and '/home/catzy/tmp/remaster-root/etc/mtab' are the same file
Unmounting /home/catzy/tmp/remaster-root/var/cache/apt...
Unmounting /home/catzy/tmp/remaster-root/tmp...
Unmounting /home/catzy/tmp/remaster-root/sys...
Unmounting /home/catzy/tmp/remaster-root/run...
Unmounting /home/catzy/tmp/remaster-root/root...
Unmounting /home/catzy/tmp/remaster-root/proc...
Unmounting /home/catzy/tmp/remaster-root/dev/pts...
Failed to copy mtab, error=1
```

mencari fix atau pindah tools lain `https://github.com/kamilion/customizer`

mencoba customizer
```
cd ~
wget https://github.com/kamilion/customizer/releases/download/4.2.0-0/customizer_4.2.0-0+20180825_all.deb
dpkg -i customizer*.deb
sudo apt --fix-broken install -y
```

mendapat error yang gajelas saat menggunakan customizer

mencoba cubic `https://launchpad.net/cubic`

------------------------------------
