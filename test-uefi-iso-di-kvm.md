### Install VMM dan Configure OVMF
Install [VMM](https://virt-manager.org/) dan [OVMF](http://www.linux-kvm.org/page/OVMF)
```
sudo apt update
sudo apt install virt-manager ovmf -y
```

Enable UEFI support di QEMU
```
sudo nano /etc/libvirt/qemu.conf
```
lakukan `Ctrl + W`, ketik `#nvram`, enter, lalu hapus `#` pada
```
nvram = [
   "/usr/share/OVMF/OVMF_CODE.fd:/usr/share/OVMF/OVMF_VARS.fd",
   "/usr/share/OVMF/OVMF_CODE.secboot.fd:/usr/share/OVMF/OVMF_VARS.fd",
   "/usr/share/AAVMF/AAVMF_CODE.fd:/usr/share/AAVMF/AAVMF_VARS.fd",
   "/usr/share/AAVMF/AAVMF32_CODE.fd:/usr/share/AAVMF/AAVMF32_VARS.fd"
]
```
`Teks mungkin akan sedikit berbeda harap dimengerti`
lalu `Ctrl + X`, ketik `y`, enter


### Buat VM dengan UEFI support
1. Buka VMM dan buat VM baru

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/1.png "Img1")

2. next

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/2.png "Img2")

3. Pilih ISO, next

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/3.png "Img3")

4. set ram + core, next

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/4.png "Img4")

5. set diskimage size jika perlu, next

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/5.png "Img5")

6. ganti nama, Checklist pada `Customize Configuration Before Install`, finish

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/6.png "Img6")

7. ganti firmware ke `UEFI blabla` dan chipset ke `Q35`

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/7.png "Img7")

8. Ubah `IDE Cdrom` dan `IDE Disk` menjadi sata

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/9.png "Img8")

9. Jika muncul error seperti ini, Ada Disk yang belum diubah ke `SATA`

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/8.png "Img9")

10. Klik `Begin Installation` lalu buka terminal dan masukkan `sudo efibootmgr -v` jika tak ada masalah, maka anda telah boot iso dalam mode efi

   ![alt text](https://raw.githubusercontent.com/catzy007/log-packaging-tealinuxos11/master/efi-step/10.png "Img10")
   
> Perlu diingat bahwa penggantian `Firmware` dan `Chipset` hanya dapat dilakukan saat pembuatan VM, Jika perlu balik ke standar BIOS, dapat dilakukan dengan mambuat VM baru
