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
## Langkah - Langkah remastering
* buka cubic, masukkan password user
* pilih project directory atau buat baru lalu next
* pada "original iso", select, lalu pilih iso yang digunakan
* pada custom iso isikan dengan format dibawah lalu next
``` 
filename : TealinuxOS-11-amd64.iso
volume id : TealinuxOS 11 amd64
release name : Stevia
disk name : TealinuxOS 11 "Stevia" amd64
```
* kemudian akan masuk ke chroot. disini sudah bisa ngoprek. kalau sudah next
* lalu masuk ke package manifest "ganti bila perlu" lalu next
* lalu cubic akan generate squashfs, iso, md5 dll. bisa ditunggu
* selesai, iso dapat ditest maupun di distribusikan

---
## Mulai Mengubah Isolinux, GRUB dan Disk info
ubah kata xubuntu menjadi TealinuxOS 11 pada 

* isolinux config file `<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg`
* grub config file `<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg`
* disk info `<PROJECT-FOLDER>/custom-live-iso/.disk/info`

---
## Mulai Mengubah Splash pada Isolinux
persiapkan splash.png dengan ukuran 640 x 480px lalu buat splash.pcx
```
cara membuat splash.pcx
* buka splash.png dengan gimp
* pada menu gimp, pilih "Image > Mode > Indexed..."
* pilih "Generate optimum palette"
* pada "Maximum number of colors" isikan angka 14
* pada "Color Dithering" pilih "Floyd-Steinburg (Reduced color bleeding" lalu gambar akan menjadi agak aneh
* lalu convert dan export sebagai splash.pcx
```

ubah splash xubuntu menjadi splash TealinuxOS pada `<PROJECT-FOLDER>/custom-live-iso/isolinux/splash.png` dan `<PROJECT-FOLDER>/custom-live-iso/isolinux/splash.pcx`

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
kemudian kembalikan bootlogo ke isolinux

---
## Mulai Menambahkan Wallpaper Tealinux
Copy asset wallpaper ke `/usr/share/tealinux/wallpaper` (kalau tidak ada buat dulu)

Ubah default wallpaper pada `/etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml`

dari

```   <property name="image-path" type="string" value="/usr/share/xfce4/backdrops/xubuntu-wallpaper.png"/>```

menjadi

```    <property name="image-path" type="string" value="/usr/share/tealinux/wallpaper/defaults.jpg"/>```

---
## (OPTIONAL) Menambahkan patch untuk nvidia pci error
patch ini berlaku untuk laptop intel core generasi 6,7 yang menggunakan vga diskrit nvidia. contoh laptop asus ROG, X550V, 15-ab549tx

cara memasang patch. tambahkan `pci=noaer` setelah `quiet splash` pada
```
<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg
<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg
/etc/default/grub
```

---

default ubiquity wallpaper config `/usr/bin/ubiquity-dm`

---

