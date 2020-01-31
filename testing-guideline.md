### Testing Guideline
Terdapat tiga iso yang ditest (dapat berubah sesuai kebijakan). Pada hal ini disebut sebagai level, sekaligus menentukan akses tester.

* ISO Level 1 (Akses Level 1)
Pada iso ini, pengetest hanya berupa Packager (Orang yang membuat ISO), PM (Project Manager), Researcher (Software riset), dan Pembuat App khas (jika ada).
Yang ditest pada iso ini adalah fitur yang baru diimplementasi (experimental), Kompabilitas aplikasi khas, dan Kestabilan sistem secara mendasar. Jika dirasa 
cukup stabil maka dapat lanjut ke level 2.

* ISO Level 2 (Akses Level 2)
Pada iso ini, pengetest berupa Beta / Alpha Tester yang ditunjuk (Bisa saat rapat perdana). Pada iso ini, Packager memberitahu Aplikasi &/ Fitur apa yang telah 
diupdate kemudian dapat ditest secara intensif baik menggunakan VM atau Baremetal. Hasil dari testing sebaiknya didokumentasikan (Misal dengan Google Form). 
Untuk pengembangan dan perbaikan lebih lanjut.

* ISO Level 3 (Akses Level 3)
Pada iso ini, pengetest dapat berupa siapa saja mulai dari sesepuh hingga calon anggota. Diharapkan semua anggota aktif ikut serta melakukan testing 
untuk mengumpulkan data yang beragam. Data yang dihasilkan dapat didokumentasikan (Misal dengan Google Form).

Testing dapat berupa virtual dan baremetal.

Untuk testing UEFI di VM dapat menggunakan <https://github.com/catzy007/log-packaging-tealinuxos11/blob/master/test-uefi-iso-di-kvm.md>

Untuk testing baremetal dapat melihat `Keberagaman Hardware & Software`

### Pengumpulan Data
Untuk data yang dikumpulkan dapat berupa :
1. Nama / Nickname
2. Veri ISO yang digunakan (Misal Alpha V0.1)
3. Hardware yang digunakan. Untuk mempermudah dapat dengan ATU <https://github.com/Yukinessa/TealinuxATU>
4. Keterangan (dapat berupa masalah yang dialami, error code yang muncul)

### Keberagaman Hardware & Software
Untuk firmware yang ditest dapat berupa 
* UEFI
* BIOS/Legacy/CSM

Arsitektur dapat berupa
* x86 / i686 / 32Bit
* x86_x64 / x64 / 64Bit
* ARM (Jika Perlu)

Mikroarsitektur dapat berupa
* Intel
    * 2006 Intel Core (core2duo, core2quad)
    * 2007 Penryn (core2duo, core2quad)
    * 2008 Nehalem, Bonnell (core i 1st gen)
    * 2010 Westmere (xeon, i7 extreme)
    * 2011 Saltwell, Sandy Bridge (core i 2nd gen)
    * 2012 Ivy Bridge (core i 3rd gen)
    * 2013 Silvermont, Haswell (core i 4th gen)
    * 2014 Broadwell (core i 5th gen)
    * 2015 Airmont, Skylake (core i 6th & 7th gen)
    * 2016 Goldmont, Kaby Lake (core i 7th & 8th gen)
    * 2017 Coffee Lake, Goldmont Plus
    * 2018 Cannon Lake, Whiskey Lake, Amber Lake
    * 2019 Cascade Lake, Comet Lake, Sunny Cove (Ice Lake, Lakefield)
* AMD
    * 2011 Bobcat, Bulldozer (AMD FX series)
    * 2012 Piledriver (AMD A & FX series)
    * 2013 Jaguar (AMD A Series)
    * 2014 Steamroller, Puma (AMD E & A series)
    * 2015 Excavator (AMD APU A series & Athlon)
    * 2017 Zen (ryzen 1000 series)
    * 2018 Zen+ (ryzen 2000 series)
    * 2019 Zen2 (ryzen 3000 series)

Graphic
* Nvidia 400, 500, 600, 700, 900, 1000, 2000 series and mobile
* Intel Inside, HD, UHD
* AMD Radeon HD, R5, R7, R9, RX series

Add On
* Wifi/Ethernet Intel, Atheros, Realtek