# log-packaging-tealinuxos11

-------------------------
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

--------------------------------
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

## Langkah Pack (setelah update kernel & ganti plymouth)
1. masuk chroot
1. lihat kernel yang terinstall `ls /lib/modules/`
1. buat ramdisk `mkinitramfs <KERNEL-AKTIF> -o /tmp/initrd`
1. kemudian simpan `<PROJECT-FOLDER>/squashfs-root/tmp/initrd` di folder baru
1. lalu copy iso tealinux yang hampir jadi **(ISO INI NANTI AKAN MATI JADI BUAT BACKUP)**
1. kemudian instal `file-roller` dan buka iso copy-an pada `langkah 5` dengan file roller
1. selanjutnya replace `casper/initrd` pada iso dengan `initrd` yang disimpan pada `langkah 4`
1. lalu buat project baru pada `cubic` dengan iso yang telah dimodif pada `langkah 7`
1. generate iso, dan iso yang baru akan menggunakan plymouth dan kernel baru

----------------------------------------------
## Mulai Mengubah Isolinux, GRUB dan Disk info
ubah kata xubuntu menjadi TealinuxOS 11 pada 

* isolinux config file `<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg`
* grub config file `<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg`
* disk info `<PROJECT-FOLDER>/custom-live-iso/.disk/info`

-------------------------------------------
## Mulai Menambahkan Asset Artwork Tealinux
### Menambahkan Splash Isolinux (gambar saat memilih mode installasi)
- persiapkan `splash.png` tealinux dengan ukuran `640 x 480px` lalu buat `splash.pcx`
```
cara membuat splash.pcx
* buka splash.png dengan gimp
* pada menu gimp, pilih "Image > Mode > Indexed..."
* pilih "Generate optimum palette"
* pada "Maximum number of colors" isikan angka 14
* pada "Color Dithering" pilih "Floyd-Steinburg (Reduced color bleeding" lalu gambar akan menjadi agak aneh
* lalu convert dan export sebagai splash.pcx
```
- ubah splash xubuntu menjadi splash TealinuxOS pada `<PROJECT-FOLDER>/custom-live-iso/isolinux/splash.png` dan `<PROJECT-FOLDER>/custom-live-iso/isolinux/splash.pcx`
- lalu copy `<PROJECT-FOLDER>/custom-live-iso/isolinux/bootlogo` ke directory baru
- unpack bootlogo dan hapus bootlogo lama
``` console
$ cpio -i -F bootlogo
$ rm -rf bootlogo
```
- kemudian replace `splash.pcx` xubuntu dengan `splash.pcx` tealinuxos
- lalu pack kembali
```console
$ ls -w1 | cpio -o -F bootlogo
$ ls | grep -v bootlogo | xargs rm
```
- kemudian kembalikan bootlogo ke isolinux

### Menambahkan wallpaper
- Copy asset wallpaper ke `/usr/share/tealinux/wallpaper` (kalau tidak ada buat dulu)
- Ubah default wallpaper pada `/etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml`
- dari

```   <property name="image-path" type="string" value="/usr/share/xfce4/backdrops/xubuntu-wallpaper.png"/>```
- menjadi

```    <property name="image-path" type="string" value="/usr/share/tealinux/wallpaper/defaults.jpg"/>```

### Menambahkan whisker (*bahasa windowsnya start menu icon)
* `whisker/*.png` masuk ke `/usr/share/pixmaps/`
* `whisker/*.rc` masuk ke `/etc/xdg/xdg-xubuntu/xfce4/whiskermenu/`

### Menambahkan Custom Themes
* `Themes` masuk ke `/usr/share/themes`

### Menambahkan plymouth (bootanimation)
* `Plymouth` masuk ke `/usr/share/plymouth/themes`

### Menambahkan background saat installasi
* default ubiquity wallpaper config `/usr/bin/ubiquity-dm`

-----------------------------------------------

## Menambahkan Asset Dokumentasi Tealinux
### Menambahkan ubiquity-slideshow (slideshow saat proses installasi)
*`ubiquity-slideshow` masuk ke `/usr/share/ubiquity-slideshow`

------------------------------------------------------
## (OPTIONAL) Menambahkan patch untuk nvidia pci error
patch ini berlaku untuk laptop intel core generasi 6,7 yang menggunakan vga diskrit nvidia. contoh laptop asus ROG, X550V, 15-ab549tx

cara memasang patch. tambahkan `pci=noaer` setelah `quiet splash` pada
```
<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg
<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg
/etc/default/grub
```

---
