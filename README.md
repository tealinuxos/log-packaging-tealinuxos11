# log-packaging-tealinuxos11

-----------------------------------
## :octocat: Mencari tools remaster
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

------------------------------------------
## :octocat: Langkah - Langkah remastering
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

## :octocat: Langkah Pack (setelah update kernel & ganti plymouth)
1. masuk `chroot`
1. lihat kernel yang terinstall `ls /lib/modules/`
1. buat ramdisk `mkinitramfs <KERNEL-AKTIF> -o /tmp/initrd`
1. kemudian simpan `<PROJECT-FOLDER>/squashfs-root/tmp/initrd` di folder baru
1. lalu copy iso tealinux yang hampir jadi **(ISO INI NANTI AKAN MATI JADI BUAT BACKUP)**
1. kemudian instal `file-roller` dan buka iso copy-an pada `langkah 5` dengan file roller
1. selanjutnya replace `casper/initrd` pada iso dengan `initrd` yang disimpan pada `langkah 4`
1. lalu buat project baru pada `cubic` dengan iso yang telah dimodif pada `langkah 7`
1. generate iso, dan iso yang baru akan menggunakan plymouth dan kernel baru
> langkah ini sedikit tricky jadi harap hati-hati

-------------------------------------
## :octocat: Mulai Menghapus Aplikasi
* pastikan list app yang dihapus telah diselesaikan oleh `software research`
* masuk `chroot`
* buka list app yang dihapus
* **misal** akan menghapus `libreoffice`

```
$ sudo dpkg -l | grep libreoffice
```
maka akan muncul seperti
```
ii  libreoffice                                 1:6.0.7-0ubuntu0.18.04.5                   amd64        office productivity suite (metapackage)
ii  libreoffice-avmedia-backend-gstreamer       1:6.0.7-0ubuntu0.18.04.5                   amd64        GStreamer backend for LibreOffice
ii  libreoffice-base                            1:6.0.7-0ubuntu0.18.04.5                   amd64        office productivity suite -- database
ii  libreoffice-base-core                       1:6.0.7-0ubuntu0.18.04.5                   amd64        office productivity suite -- shared library
ii  libreoffice-base-drivers                    1:6.0.7-0ubuntu0.18.04.5                   amd64        Database connectivity drivers for LibreOffice
ii  libreoffice-calc                            1:6.0.7-0ubuntu0.18.04.5                   amd64        office productivity suite -- spreadsheet
ii  libreoffice-common                          1:6.0.7-0ubuntu0.18.04.5                   all          office productivity suite
... masih banyak lagi
```
kemudian hapus satu persatu dari daftar 
```
$ sudo apt remove libreoffice libreoffice-avmedia-backend-gstreamer libreoffice-base libreoffice-base-core libreoffice-base-drivers libreoffice-calc libreoffice-common -y
```

* kalau belum bersih tinggal `purge` app yang ada pada `dpkg -l | grep libreoffice`
* selesaikan list apps yang dihapus
* kalau sudah maka tinggal `sudo apt autoremove && sudo apt autoclean`

-----------------------------------------------
## :octocat: Mulai Menginstall Aplikasi Default
* pastikan list app default telah diselesaikan oleh `software research`
* masuk `chroot`
* update dan upgrade dulu `sudo apt update && sudo apt upgrade -y`
* buka list default apps
* misal akan menginstall `vlc`

```
$ sudo apt update
$ sudo apt install vlc -y
```
* selesaikan list app default
* kalau ada app yang tidak masuk apt atau ada yang error, coba tanya `software research`

--------------------------------------------------------
## :octocat: Mulai Mengubah Isolinux, GRUB dan Disk info
ubah kata `xubuntu` menjadi `TealinuxOS 11` pada 

* isolinux config file `<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg`
* grub config file `<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg`
* disk info `<PROJECT-FOLDER>/custom-live-iso/.disk/info`

---------------------------------------
## :octocat: Mulai Mengubah lsb-release
* masuk `chroot`
* edit config `sudo nano /etc/lsb-release`
* ubah dari 
```
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.2 LTS"` 
```
* menjadi 
```
DISTRIB_ID=TealinuxOS
DISTRIB_RELEASE=11   
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="TealinuxOS 11 Stevia"
```
* kemudian `cd /usr/share/python-apt/templates/`
* lalu `sudo ln -s Ubuntu.info TealinuxOS.info`

-----------------------------------------------------
## :octocat: Mulai Menambahkan Asset Artwork Tealinux
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
* Copy asset wallpaper ke `<PROJECT-FOLDER>/squashfs-root/usr/share/tealinux/wallpaper` (kalau tidak ada buat dulu)
* Ubah default wallpaper pada `<PROJECT-FOLDER>/squashfs-root/etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml`
* dari `<property name="image-path" type="string" value="/usr/share/xfce4/backdrops/xubuntu-wallpaper.png"/>`
* menjadi `<property name="image-path" type="string" value="/usr/share/tealinux/wallpaper/defaults.jpg"/>`

### Menambahkan Ubiquity background (bg saat installasi)
* masuk `chroot`
* edit config `nano /usr/bin/ubiquity-dm`
* dari
```
for background in (
                    '/usr/share/xfce4/backdrops/xubuntu-wallpaper.png',
```
* menjadi
```
for background in (
                    '/usr/share/tealinux/wallpaper/defaults.jpg',
```

### Menambahkan lightdm-greeter background (bg saat login)
* masuk `chroot`
* lalu edit config `nano /etc/lightdm/lightdm-gtk-greeter.conf`
* ubah menjadi `background=/usr/share/tealinux/wallpaper/defaults.jpg`

### Mengedit lightdm-greeter themes
* masuk `chroot`
* lalu edit config `nano /etc/lightdm/lightdm-gtk-greeter.conf`
* tambahkan
```
  theme-name = Tea-Light
  icon-theme-name = Papirus-Dark
```

### Menambahkan whisker (start menu icon)
* copy `wisker-tea.png` ke `<PROJECT-FOLDER>/squashfs-root/usr/share/pixmaps/`
* masuk `chroot`
* edit confg `nano /etc/xdg/xdg-xubuntu/xfce4/whiskermenu/defaults.rc`
* lalu ubah menjadi `button-icon=wisker-tea`

### Menambahkan Tema
* `Themes` masuk ke `<PROJECT-FOLDER>/squashfs-root/usr/share/themes`
* masuk chroot
* edit config pertama `nano /etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfwm4.xml`
* *dari `<property name="theme" type="string" value="Greybird"/>`*
* *menjadi `<property name="theme" type="string" value="Tea-Dark"/>`*
* lalu edit config kedua `nano /etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xsettings.xml`
* *dari `<property name="ThemeName" type="string" value="Greybird"/>`*
* *menjadi `<property name="ThemeName" type="string" value="Tea-Dark"/>`*

### Menambahkan plymouth (bootanimation)
* copy plymouth folder `stevia-tea` yang berisi `gambar plmouth, stevia-tea.script, stevia-text.plymouth, dan stevia-tea.plymouth` ke `<PROJECT-FOLDER>/squashfs-root/usr/share/plymouth/themes`
* lalu masuk ke `chroot`
* kemudian install plymouth 
```
update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth /usr/share/plymouth/themes/stevia-tea/stevia-tea.plymouth 200
update-alternatives --install /usr/share/plymouth/themes/text.plymouth text.plymouth /usr/share/plymouth/themes/stevia-tea/stevia-text.plymouth 200
```
* selanjutnya buat plymouth menjadi default 
```
update-alternatives --config default.plymouth #pilih stevia-tea
update-alternatives --config text.plymouth #pilih stevia-text
```
* kemudian update initrams `update-initramfs -u`
* *kalau ada installer tinggal copy folder plymouth `stevia-tea` ke `<PROJECT-FOLDER>/squashfs-root/tmp` lalu masuk ke `chroot` kemudian `cd /tmp/stevia-tea` lalu ikuti langkah2 pada installer*
* lalu lakukan [PACK-ULANG](https://github.com/catzy007/log-packaging-tealinuxos11#octocat-langkah-pack-setelah-update-kernel--ganti-plymouth)

### Menambahkan Cursor
* copy `Tea-Cursor-Light` dan `Tea-Cursor-Dark` ke `<PROJECT-FOLDER>/squashfs-root/usr/share/icons/`
* masuk ke `chroot` lalu
* install cursor
```
sudo update-alternatives --install /usr/share/icons/default/index.theme x-cursor-theme /usr/share/icons/Tea-Cursor-Light/cursor.theme 65
sudo update-alternatives --install /usr/share/icons/default/index.theme x-cursor-theme /usr/share/icons/Tea-Cursor-Dark/cursor.theme 65
```
* set cursor menjadi default (disini dark yang jadi default)
```
gsettings set org.gnome.desktop.interface cursor-theme "Tea-Cursor-Dark" & sudo ln -fs /usr/share/icons/Tea-Cursor-Dark/cursor.theme /etc/alternatives/x-cursor-theme
```
* kemudian `nano /etc/alternatives/x-cursor-theme` ubah menjadi `Inherits=Tea-Cursor-Light`

### Menambahkan Ubiquity Icon (icon saat installasi)
* persiapkan `cd_in_tray.png` dan `ubuntu_installed.png`
* replace `cd_in_tray.png` dan `ubuntu_installed.png` pada `<PROJECT-FOLDER>squashfs-root/usr/share/ubiquity/pixmaps/`

### Menambahkan Icon Default TealinuxOS 11
* buka cubic lalu masuk ke `chroot`
* tambah ppa [papirus icon](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme) `sudo add-apt-repository ppa:papirus/papirus`
* lalu install `sudo apt-get install papirus-icon-theme`
* lalu set config agar menjadi default `nano /etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xsettings.xml`
* dari `<property name="IconThemeName" type="string" value="elementary-xfce-darker"/>`
* menjadi `<property name="IconThemeName" type="string" value="Papirus"/>`

---------------------------------------------------
## :octocat: Menambahkan Asset Dokumentasi Tealinux
### Menambahkan ubiquity-slideshow (slideshow saat proses installasi)
* persiapkan `ubiquity-slideshow` tealinuxOS. isinya `slideshow.conf` dan folder `slides` (kalau tidak ada buat dulu)
* masuk ke `<PROJECT-FOLDER>/squashfs-root/usr/share/ubiquity-slideshow`
* hapus `slideshow.conf` dan folder `slides` yang lama
* copy `slideshow.conf` dan folder `slides` ke `<PROJECT-FOLDER>/squashfs-root/usr/share/ubiquity-slideshow`

--------------------------------------------------------
## :octocat: Menambahkan Keyboard Shortcut dan Ksuperkey
* masuk `chroot`
* install ksuperkey `sudo apt install ksuperkey`
* tambahkan config pada autostart `sudo nano /etc/xdg/autostart`
```
[Desktop Entry]
Name=Super Key
Comment=Tealinux Super Key
Icon=wisker-tea
Exec=ksuperkey -e 'Super_L=Control_L|Escape'
Terminal=false
Type=Application
Categories=
OnlyShowIn=XFCE;
```
* tambah dan edit sesuai kebutuhan pada `nano /etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfce4-keyboard-shortcuts.xml`

contoh
```
<?xml version="1.0" encoding="UTF-8"?>

<channel name="xfce4-keyboard-shortcuts" version="1.0">
  <property name="commands" type="empty">
    <property name="default" type="empty">
      <property name="&lt;Alt&gt;F2" type="string" value="xfrun4"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Delete" type="string" value="xflock4"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Escape" type="string" value="xkill"/>
      <property name="&lt;Super&gt;p" type="string" value="xfce4-display-settings --minimal"/>
      <property name="&lt;Primary&gt;Escape" type="string" value="xfce4-popup-whiskermenu"/>
      <property name="Print" type="string" value="xfce4-screenshooter -f"/>
      <property name="&lt;Alt&gt;Print" type="string" value="xfce4-screenshooter -w"/>
      <property name="&lt;Super&gt;w" type="string" value="exo-open --launch WebBrowser"/>
      <property name="&lt;Super&gt;m" type="string" value="exo-open --launch MailReader"/>
      <property name="&lt;Super&gt;f" type="string" value="exo-open --launch FileManager"/>
      <property name="&lt;Super&gt;F1" type="string" value="xfce4-find-cursor"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;t" type="string" value="exo-open --launch TerminalEmulator"/>
      <property name="&lt;Super&gt;t" type="string" value="exo-open --launch TerminalEmulator"/>
      <property name="&lt;Super&gt;r" type="string" value="xfce4-appfinder"/>
      <property name="&lt;Super&gt;e" type="string" value="thunar"/>
      <property name="F12" type="string" value="xfce4-terminal --drop-down"/>
    </property>
  </property>
  <property name="xfwm4" type="empty">
    <property name="default" type="empty">
      <property name="&lt;Alt&gt;Insert" type="string" value="add_workspace_key"/>
      <property name="Escape" type="string" value="cancel_key"/>
      <property name="Left" type="string" value="left_key"/>
      <property name="Right" type="string" value="right_key"/>
      <property name="Up" type="string" value="up_key"/>
      <property name="Down" type="string" value="down_key"/>
      <property name="&lt;Alt&gt;Tab" type="string" value="cycle_windows_key"/>
      <property name="&lt;Alt&gt;&lt;Shift&gt;Tab" type="string" value="cycle_reverse_windows_key"/>
      <property name="&lt;Alt&gt;Delete" type="string" value="del_workspace_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Down" type="string" value="down_workspace_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Left" type="string" value="left_workspace_key"/>
      <property name="&lt;Shift&gt;&lt;Alt&gt;Page_Down" type="string" value="lower_window_key"/>
      <property name="&lt;Alt&gt;F4" type="string" value="close_window_key"/>
      <property name="&lt;Alt&gt;F5" type="string" value="maximize_horiz_key"/>
      <property name="&lt;Alt&gt;F6" type="string" value="maximize_vert_key"/>
      <property name="&lt;Alt&gt;F7" type="string" value="maximize_window_key"/>
      <property name="&lt;Alt&gt;F8" type="string" value="stick_window_key"/>
      <property name="&lt;Alt&gt;F9" type="string" value="hide_window_key"/>
      <property name="&lt;Alt&gt;F10" type="empty"/>
      <property name="&lt;Alt&gt;F11" type="string" value="fullscreen_key"/>
      <property name="&lt;Alt&gt;F12" type="string" value="above_key"/>
      <property name="&lt;Primary&gt;&lt;Shift&gt;&lt;Alt&gt;Left" type="string" value="move_window_left_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;End" type="string" value="move_window_next_workspace_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Home" type="string" value="move_window_prev_workspace_key"/>
      <property name="&lt;Primary&gt;&lt;Shift&gt;&lt;Alt&gt;Right" type="string" value="move_window_right_key"/>
      <property name="&lt;Primary&gt;&lt;Shift&gt;&lt;Alt&gt;Up" type="string" value="move_window_up_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_1" type="string" value="move_window_workspace_1_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_2" type="string" value="move_window_workspace_2_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_3" type="string" value="move_window_workspace_3_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_4" type="string" value="move_window_workspace_4_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_5" type="string" value="move_window_workspace_5_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_6" type="string" value="move_window_workspace_6_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_7" type="string" value="move_window_workspace_7_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_8" type="string" value="move_window_workspace_8_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;KP_9" type="string" value="move_window_workspace_9_key"/>
      <property name="&lt;Super&gt;KP_1" type="string" value="tile_down_left_key"/>
      <property name="&lt;Super&gt;Down" type="string" value="tile_down_key"/>
      <property name="&lt;Super&gt;KP_3" type="string" value="tile_down_right_key"/>
      <property name="&lt;Super&gt;Left" type="string" value="tile_left_key"/>
      <property name="&lt;Super&gt;Right" type="string" value="tile_right_key"/>
      <property name="&lt;Super&gt;KP_7" type="string" value="tile_up_left_key"/>
      <property name="&lt;Super&gt;Up" type="string" value="tile_up_key"/>
      <property name="&lt;Super&gt;KP_9" type="string" value="tile_up_right_key"/>
      <property name="&lt;Alt&gt;space" type="string" value="popup_menu_key"/>
      <property name="&lt;Shift&gt;&lt;Alt&gt;Page_Up" type="string" value="raise_window_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Right" type="string" value="right_workspace_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;d" type="string" value="show_desktop_key"/>
      <property name="&lt;Primary&gt;&lt;Alt&gt;Up" type="string" value="up_workspace_key"/>
      <property name="&lt;Super&gt;Tab" type="string" value="switch_window_key"/>
      <property name="&lt;Primary&gt;F1" type="string" value="workspace_1_key"/>
      <property name="&lt;Primary&gt;F2" type="string" value="workspace_2_key"/>
      <property name="&lt;Primary&gt;F3" type="string" value="workspace_3_key"/>
      <property name="&lt;Primary&gt;F4" type="string" value="workspace_4_key"/>
      <property name="&lt;Primary&gt;F5" type="string" value="workspace_5_key"/>
      <property name="&lt;Primary&gt;F6" type="string" value="workspace_6_key"/>
      <property name="&lt;Primary&gt;F7" type="string" value="workspace_7_key"/>
      <property name="&lt;Primary&gt;F8" type="string" value="workspace_8_key"/>
      <property name="&lt;Primary&gt;F9" type="string" value="workspace_9_key"/>
      <property name="&lt;Primary&gt;F10" type="string" value="workspace_10_key"/>
      <property name="&lt;Primary&gt;F11" type="string" value="workspace_11_key"/>
      <property name="&lt;Primary&gt;F12" type="string" value="workspace_12_key"/>
    </property>
  </property>
</channel>
```

------------------------------------------
## :octocat: Menambahkan default filetypes
* masuk `chroot`
* tambah dan edt sesuai kebutuhan pada `nano /usr/share/applications/defaults.list`

-----------------------------------
## :octocat: Menambahkan item panel
### menambahkan workspaces
* edit `<PROJECT-FOLDER>/squashfs-root/etc/xdg/xdg-xubuntu/xfce4/panel/default.xml`
```
    <value type="int" value="12"/>
```
```
    <property name="plugin-12" type="string" value="windowmenu"/>
```
* masuk `chroot`
* set config `xfconf-query -c xfwm4 -p /general/workspace_count -s 2`

---------------------------------------
## :octocat: Menambahkan Theme-Switcher
* copy `theme-switcher.desktop` ke `<PROJECT-FOLDER>/squashfs-root/etc/xdg/autostart`
* copy `theme-switcher.sh` ke `<PROJECT-FOLDER>/squashfs-root/usr/share/tealinux/ThemeSwitcher/` (kalau tidak ada buat dulu)
* masuk `chroot`
* install yad `sudo apt install yad`
* add permission `chmod +x /usr/share/tealinux/ThemeSwitcher/theme-switcher.sh`

--------------------------------------------
## :octocat: Disable Zoom (alt + mousewheel)
* masuk ke `chroot`
* edit config `nano /etc/xdg/xdg-xubuntu/xfce4/xfconf/xfce-perchannel-xml/xfwm4.xml`
* ubah dari 
```
    <property name="wrap_resistance" type="int" value="10"/>
    <property name="wrap_windows" type="bool" value="true"/>
    <property name="wrap_workspaces" type="bool" value="false"/>
  </property>
</channel>
```
* menjadi
```
    <property name="wrap_resistance" type="int" value="10"/>
    <property name="wrap_windows" type="bool" value="true"/>
    <property name="wrap_workspaces" type="bool" value="false"/>
    <property name="zoom_desktop" type="bool" value="false"/>
  </property>
</channel>
```

---------------------------------------
## :octocat: Mengedit Terminal Emulator
* masuk ke `chroot`
* edit config `sudo nano /etc/xdg/xdg-xubuntu/xfce4/terminal/terminalrc`
* tambahkan pada bagian bawah
```
BackgroundMode=TERMINAL_BACKGROUND_TRANSPARENT
BackgroundDarkness=0.850000
```

----------------------------------------------------------------
## :octocat: (OPTIONAL) Menambahkan patch untuk nvidia pci error
patch ini berlaku untuk laptop intel core generasi 6,7 yang menggunakan vga diskrit nvidia.
> contoh laptop asus ROG, X550V, 15-ab549tx

cara memasang patch. tambahkan `pci=noaer` setelah `quiet splash` pada
```
<PROJECT-FOLDER>/custom-live-iso/isolinux/txt.cfg
<PROJECT-FOLDER>/custom-live-iso/boot/grub/grub.cfg
<PROJECT-FOLDER>/squashfs-root/etc/default/grub
```

--------------------------------------------------------------
## :octocat: (NOT-FIXED) Jika ada error di installasi GRUB EFI
##### Patch ini berlaku untuk laptop yang menggunakan UEFI. Penyebab error
```
Broken grub-efi-amd64-signed:amd64 Depends on grub-efi-amd64
Considering grub-efi-amd64:amd64 1 as a solution to grub-efi-amd64-signed:amd64
grub-efi-amd64-signed : Depends: grub-efi-amd64 (= 2.02~beta2-36ubuntu3.7) but it is not going \
 to be installed
Investigating () shim-signed
Broken shim-signed:amd64 Depends on grub-efi-amd64-bin
```
#### Cara Mengatasi
* masuk chroot
* lalu
```
sudo apt update
sudo apt install grub-efi-amd64-signed 
```

## Menambahkan lightm session select icon
* Persiapkan icon `svg` dengan ukuran `16 x 16px`
* copy icon ke `<PROJECT FOLDER>/squashfs-root/usr/share/icons/hicolor/scalable/places/`
* lalu masuk ke `chroot`
* lakukan `sudo gtk-update-icon-cache /usr/share/icons/hicolor`
* lalu `cd /usr/share/xsessions`
* kemudian `nano tealinuxos.desktop` dan isikan 
```
[Desktop Entry]
Version=1.0
Name=TealinuxOS Session
Comment=Use this session to run TealinuxOS as your desktop environment
Exec=startxfce4
Icon=
Type=Application
DesktopNames=XFCE
```

--------------------
## Install LaporHama
`sudo apt -y install libgconf2-4`

----
## Setting Favorites Menu
`nano /etc/xdg/xdg-xubuntu/menus/xfce-applications.menu`

---
