## **rsrc**
> https://www.happyassassin.net/2014/01/25/uefi-boot-how-does-that-actually-work-then/

> https://askubuntu.com/questions/927924/how-to-install-ubuntu-in-uefi-mode

> https://linuxmint-installation-guide.readthedocs.io/en/latest/efi.html

> grub-pc

> grub-efi

> https://en.wikipedia.org/wiki/EFI_system_partition

> https://uefi.org/specsandtesttools

> efibootmgr -v

> https://docs.microsoft.com/en-us/previous-versions/windows/hardware/cert-program/windows-hardware-certification-requirements-for-client-and-server-systems

> https://launchpad.net/ubuntu/+source/shim-signed

> shim-signed: Secure Boot chain-loading bootloader (Microsoft-signed binary)

## **Info**

- `UEFI` dimaksudkan untuk menggantikan `BIOS` yang merupakan standar lama
- BIOS booting melalui `bootloader` yang ditulis pada disk
- UEFI booting melalui file `efi` yang disimpan pada partisi `FAT/FAT32` pada disk 
- UEFI dapat booting `bootloader` milik BIOS melalui `CSM (compatibility support module)`
- Untuk dapat install OS melalui mode UEFI, install media (USB/CD) **HARUS** boot melalui mode native UEFI
- Untuk mengecek apakah anda boot melalui CSM atau UEFI dapat dengan `efibootmgr -v` jika output serupa `EFI variables are not supported on this system.` berarti tidak boot melalui native UEFI
- Jika anda ingin mode CSM, gunakan `MBR`
- Jika anda ingin native UEFI, gunakan `GPT`
- Selain itu sangat tidak direkomendasikan
- Jika anda ingin dualboot (windows 8 atau lebih baru) maka kemungkinan besar anda sudah menggunakan mode native UEFI, maka sebaiknya install linux melalui mode yang sama
- Jika melakukan mode installasi linux melalu `Something else`, buat partisi efi dengan ukuran minimal 200MB, (disarankan 500MB). Lalu mount ke `/boot/efi`
- Jika sudah ada partisi efi yang dibuat oleh ehem.. windows maka partisi tersebut dapat di mount ke `/boot/efi`
- Secure boot adalah mekanisme keamanan dimana sistem dapat menolak untuk booting bila file efi tidak memiliki signature kriptografi atau memiliki signature kriptografi yang salah
- Spesifikasi secure boot dari ehem.. microsoft memungkinkan 
  - Device ship with Secure Boot turned on (except for servers)
  - Have Microsoft’s key in the list of keys they trust
  - Disable BIOS compatibility mode when Secure Boot is enabled (actually the UEFI spec requires this too, if I read it correctly)
  - Support signature blacklisting
- Khusus device berbasis `ARM` user tak diberi akses untuk disable secure boot. 

## **Rekomendasi**

* Sebisa mungkin satu OS per satu komputer. Jika butuh OS lain, beli komputer lagi atau lakukan `virtualisasi`
* Jika memang harus dual-boot, sebisa mungkin satu OS per satu disk. Misal 2 SSD, satu linux satu windows.
* Jika memang harus dual-boot dan hanya satu disk, pastikan **TIDAK** mencampur native UEFI dengan CSM
* Sebisa mungkin cegah native UEFI dengan MBR dan CSM/BIOS dengan GPT

## **Log Ujicoba**

* Mencoba install xubuntu 18.04 native UEFI pada laptop Acer E5-471 dengan mode UEFI only dan secureboot enable. laptop ini sudah ada os windows versi ? dan linux. Namun tidak dapat booting. Kemungkinan HDD menggunakan MBR.
* Hasilnya sistem tidak dapat booting. Bahkan tak terdaftar di boot efi list kemungkinan karena HDD menggunakan MBR, mencoba format penuh lalu ubah menjadi GPT. 
* Mencoba boot-repair juga tidak membuahkan hasil yang sesuai, masih tidak dapat booting dan tidak terdaftar di boot efi list
* Mencoba install Xubuntu 18.04 single boot native UEFI pada laptop Acer E5-471 dengan HDD GPT. hasilnya sistem dapat booting secara normal. UEFI pada linux berhasil!
```
efi@efi-Aspire-E5-471:~$ efibootmgr -v
BootCurrent: 0001
Timeout: 0 seconds
BootOrder: 0001,2001,2002,2003
Boot0000* USB HDD: TOSHIBA TransMemory	PciRoot(0x0)/Pci(0x14,0x0)/USB(0,0)/HD(1,MBR,0x40c10,0x800,0x39c9100)RC
Boot0001* ubuntu	HD(1,GPT,6a6232e0-d02a-453e-8a9f-3a66a9e1a2d9,0x800,0xf3800)/File(\EFI\ubuntu\shimx64.efi)
Boot2001* EFI USB Device	RC
Boot2002* EFI DVD/CDROM	RC
Boot2003* EFI Network	RC
```
* Akan mencoba dengan TealinuxOS 11.
* Mencoba install TealinusOS 11 singleboot dengan mode native UEFI dan Secureboot enabled pada laptop Acer Aspire E5-471. Hasilnya terjadi error pada saat installasi
```
The 'grub-efi-amd64-signed' package failed to install into /target/.
Without the GRUB boot loader, the installed system will not boot.
```
* Mencoba install kembai TealinuxOS 11 dengan konfigurasi yang sama, namun dengan koneksi internet nyala dan hasilnya installasi selesai, namun saat booting, hanya nampak efi terminal. 
* Kesimpulan : tealinuxos 11 tidak dapat di install pada mode UEFI. Namun Xubuntu dapat install pada mode UEFI.
* Melanjutkan test dengan Xubuntu 18.04 dengan mode dualboot dengan Windows 10 native UEFI.
* Hasilnya sistem dapat boot dan kedua OS terdaftar di EFI list
```
efi4@efi4-Aspire-E5-471:~$ efibootmgr -v
BootCurrent: 0001
Timeout: 0 seconds
BootOrder: 0001,0003,2001,2002,2003
Boot0001* ubuntu	HD(2,GPT,3d0833e5-01f5-4874-a4f8-2baa743f364e,0xe1800,0x32000)/File(\EFI\ubuntu\shimx64.efi)
Boot0003* Windows Boot Manager	HD(2,GPT,3d0833e5-01f5-4874-a4f8-2baa743f364e,0xe1800,0x32000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)WINDOWS.........x...B.C.D.O.B.J.E.C.T.=.{.9.d.e.a.8.6.2.c.-.5.c.d.d.-.4.e.7.0.-.a.c.c.1.-.f.3.2.b.3.4.4.d.4.7.9.5.}...M................
Boot2001* EFI USB Device	RC
Boot2002* EFI DVD/CDROM	RC
Boot2003* EFI Network	RC
```

## **Rekomendasi (dari hasil ujicoba)**

* Bagi packaging, gunakan laptop/PC yang mendukung UEFI dan menjalankan OS dengan mode `Native UEFI` agar iso yang dihasilkan mendukung mode tersebut
>  As far as UEFI support goes, Cubic uses files from your host machine to create the EFI ISO. Therefore, you need to create the custom Ubuntu or Linux Mint ISO while using a host system that uses EFI to be able to create an EFI-enabled custom ISO. See [Launchpad](https://answers.launchpad.net/cubic/+question/387566) for details.

* Untuk testing, disarankan mengetest ISO yang dibuat menggunakan laptop/PC yang mendukung UEFI (biasanya intel core gen2 keatas sudah mendukung) juga mengetest di laptop yang menggunakan mode BIOS (biasanya core 2 duo kebawah)

### Hasil riset dari lepi safira
```
sudo dpkg --configure -a
sudo apt-get install -fy
sudo apt-get purge -y grub*-common grub-common:i386 shim-signed
```
lalu
```
sudo apt-get install -y grub-efi-amd64-signed os-prober shim-signed linux-headers-generic linux-signed-generic
```
## log packaging tealinuxos 11.1
mengembalikan lsb_release, menghapus TealinuxOS.info dan reinstall grub 
akan membuat grub efi tealinux bekerja

*dengan catatan legacy support akan drop atau iso hanya dapat install uefi.
untuk legacy dapat install dengan syarat lakukan boot-repair*

### Todo
##### app yang diinstall
```
sudo apt update
sudo apt install gnome-disk-utility htop catfish
```
#### Reinstall grub-efi
```
sudo apt install grub-pc-bin grub-efi-amd64-signed shim-signed
```
#### Restore lsb-release
```
sudo nano /etc/lsb-release
```
lalu edit menjadi
```
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.2 LTS"
```
hapus info lama
```
rm /usr/share/python-apt/templates/TealinuxOS.info
```
#### Reinstall grub lagi dengan lsb ubuntu
```
sudo apt install grub-pc-bin grub-efi-amd64-signed shim-signed grub*-common --reinstall
```
#### Removed somehow
```
ubiquity* ubiquity-frontend-gtk*
```
