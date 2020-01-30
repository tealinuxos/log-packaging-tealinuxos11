
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
`Teks mungkin akan sedikit berbeda`
