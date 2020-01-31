### Testing Guideline
Terdapat tiga iso yang ditest (dapat berubah sesuai kebijakan). Pada hal ini disebut sebagai level, sekaligus menentukan akses tester.

* ISO Level 1 (Akses Level 1)
Pada iso ini, pengetest hanya berupa Packager (Orang yang membuat ISO), PM (Project Manager), Researcher (Software riset), dan Pembuat App khas (jika ada).
Yang ditest pada iso ini adalah fitur yang baru diimplementasi (experimental), Kompabilitas aplikasi khas, dan Kestabilan sistem secara mendasar. Jika dirasa 
cukup stabil maka dapat lanjut ke level 2.

* ISO Level 2 (Akses Level 2)
Pada iso ini, pengetest berupa Beta / Alpha Tester yang ditunjuk (Bisa saat rapat perdana). Pada iso ini, Packager memberitahu Aplikasi &/ Fitur apa yang telah 
diupdate kemudian dapat ditest secara intensif baik menggunakan VM atau Baremetal. Hasil dari testing sebaiknya didokumentasikan (Misal dengan Google Form). 
Untuk pengembangan lebih lanjut.

* ISO Level 3 (Akses Level 3)
Pada iso ini, pengetest dapat berupa siapa saja mulai dari sesepuh hingga calon anggota. Diharapkan semua anggota aktif ikut serta melakukan testing 
untuk mengumpulkan data yang beragam. Data yang dihasilkan dapat didokumentasikan (Misal dengan GOogle Form).

### Pengumpulan Data
Untuk data yang dikumpulkan dapat berupa :
1. Nama / Nickname
2. Veri ISO yang digunakan (Misal Alpha V0.1)
3. Hardware yang digunakan. Untuk mempermudah dapat dengan ATU <https://github.com/Yukinessa/TealinuxATU>
4. Keterangan (dapat berupa masalah yang dialami, error code yang muncul)