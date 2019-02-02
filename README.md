# log-packaging-tealinuxos11

-------------------------------------------

## Mencari tools remaster
referensi utama <https://help.ubuntu.com/community/LiveCDCustomization>

* #### mencoba ubuntu customization kit (uck)
```console
$ sudo apt install uck
```
> menemui kendala berupa `Failed to copy mtab, error=1` juga perkembangan uck telah dihentikan jadi tidak ada support

* #### mencoba customizer <https://github.com/kamilion/customizer>
```console
$ cd ~
$ wget https://github.com/kamilion/customizer/releases/download/4.2.0-0/customizer_4.2.0-0+20180825_all.deb
$ dpkg -i customizer*.deb
$ sudo apt --fix-broken install -y
```
> mendapat error yang gajelas saat menggunakan customizer

* #### mencoba cubic <https://launchpad.net/cubic>
```console
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 081525E2B4F1283B
$ sudo apt-add-repository ppa:cubic-wizard/release
$ sudo apt update
$ sudo apt install cubic
```
> memilih cubic sebagai tools remaster tealinuxos 11

------------------------------------

default wallpaper config dir `/etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml`

default ubiquity wallpaper config `/usr/bin/ubiquity-dm`

---
## Mulai Mengubah Isolinux, GRUB dan Disk info
ubah kata xubuntu menjadi TealinuxOS 11 pada 

* isolinux config file `<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg`
* grub config file `<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg`
* disk info `<PROJECT-FOLDER>/custom-live-iso/.disk/info`

---
## Mulai Mengubah Splash pada Isolinux
ubah splash xubuntu menjadi splash TealinuxOS pada `<PROJECT-FOLDER>/custom-live-iso/isolinux/splash.png` dan `<PROJECT-FOLDER>/custom-live-iso/isolinux/splash.pcx`

> edit splash.pcx dengan gimp

lalu copy `<PROJECT-FOLDER>/custom-live-iso/isolinux/bootlogo` ke directory baru

unpack bootlogo dan hapus bootlogo lama
``` console
$ cpio -i -F bootlogo
$ rm -rf bootlogo
```
kemudian replace `splash.pcx` xubuntu dengan `splash.pcx` tealinuxos

lalu pack kembali
```console
$ ls -w1 | cpio -o -F bootlogo
```
