# portofolio-cybersecurity
Markdown
# Menggunakan Perintah Linux untuk Mengelola Izin File

## Deskripsi Proyek
Dalam proyek ini, saya bertindak sebagai profesional keamanan untuk sebuah organisasi besar yang mengelola sistem untuk tim riset internal. Tugas utama saya adalah memeriksa dan memperbarui izin file dan direktori pada sistem berbasis Linux. Melalui penggunaan antarmuka baris perintah (CLI) Linux, saya mengidentifikasi kerentanan perizinan dan memodifikasi otorisasi untuk memastikan hanya pengguna yang sah yang memiliki akses, sehingga keamanan sistem dan kerahasiaan data tetap terjaga.

## Periksa Detail File dan Direktori
Untuk memeriksa perizinan saat ini pada direktori `projects`, saya menggunakan perintah berikut:

```bash
ls -la
```
Penjelasan:
Perintah ls digunakan untuk menampilkan isi direktori. Argumen -l digunakan untuk menampilkan format panjang (long format) yang mencakup detail izin (permissions), pemilik, grup, ukuran file, dan waktu modifikasi. Argumen -a (all) ditambahkan untuk memastikan semua file ditampilkan, termasuk file tersembunyi (hidden files) yang namanya diawali dengan tanda titik (seperti .project_x.txt).

Output yang dihasilkan (sebelum dimodifikasi):

Plaintext
total 32

drwxr-xr-x 3 researcher2 research_team 4096 Oct 18 13:18 .
drwxr-xr-x 3 researcher2 research_team 4096 Oct 18 13:56 ..
-r--r----- 1 researcher2 research_team   46 Oct 18 13:18 .project_x.txt
drwx------ 2 researcher2 research_team 4096 Oct 18 13:18 drafts
-rw-rw-rw- 1 researcher2 research_team   46 Oct 18 13:18 project_k.txt
-rw-r----- 1 researcher2 research_team   46 Oct 18 13:18 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Oct 18 13:18 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Oct 18 13:18 project_t.txt
Jelaskan String Perizinan
Saya memilih contoh string perizinan dari file project_k.txt, yaitu: -rw-rw-rw-
String 10 karakter ini merepresentasikan hak akses untuk file tersebut:

Karakter ke-1 (-): Menunjukkan bahwa ini adalah file reguler (bukan direktori).

Karakter ke-2 hingga ke-4 (rw-): Mewakili izin untuk Pemilik (User). Pemilik memiliki izin untuk membaca (read) dan menulis (write).

Karakter ke-5 hingga ke-7 (rw-): Mewakili izin untuk Grup (Group). Anggota grup juga memiliki izin untuk membaca dan menulis.

Karakter ke-8 hingga ke-10 (rw-): Mewakili izin untuk Pengguna Lain (Other). Pengguna lain di luar pemilik dan grup memiliki izin membaca dan menulis. (Izin inilah yang menjadi celah keamanan).

Ubah Izin File
Organisasi tidak mengizinkan pengguna lain (other) memiliki akses tulis ke file apa pun. Berdasarkan pemeriksaan, file project_k.txt memiliki izin tulis untuk other. Saya mengubahnya dengan perintah:

Bash
chmod o-w project_k.txt
Penjelasan:
Perintah chmod (change mode) digunakan untuk mengubah izin file. Argumen o-w berarti saya menghapus (-) izin menulis (w) untuk pengguna lain (o). Setelah perintah ini dijalankan, izin file berubah menjadi -rw-rw-r--.

Ubah Izin File pada File Tersembunyi
File .project_x.txt adalah file arsip tersembunyi yang seharusnya tidak memiliki hak akses tulis untuk siapa pun, namun pengguna dan grup harus tetap dapat membacanya. Saya memodifikasinya dengan perintah:

Bash
chmod ug-w .project_x.txt
Penjelasan:
Karena file ini diawali dengan titik (.), ini adalah file tersembunyi. Perintah chmod ug-w secara spesifik menghapus (-) izin menulis (w) untuk pemilik / user (u) dan grup (g). Perintah ini membuat file tersebut bersifat read-only agar arsip aman dari modifikasi.

Ubah Perizinan Direktori
Direktori drafts hanya boleh diakses oleh pemiliknya yaitu researcher2. Grup tidak boleh memiliki akses masuk. Saya memodifikasinya dengan perintah:

Bash
chmod g-x drafts
Penjelasan:
Dalam Linux, untuk dapat masuk dan mengakses isi sebuah direktori, pengguna memerlukan izin eksekusi (x). Dengan menggunakan perintah g-x, saya mencabut izin eksekusi dari grup (g). Hal ini mengunci direktori drafts sehingga hanya pengguna researcher2 yang memiliki akses penuh (izinnya berubah menjadi drwx------).

Ringkasan
Melalui aktivitas ini, saya berhasil mengidentifikasi dan memitigasi risiko keamanan akses pada sistem file Linux tim riset. Dengan memanfaatkan perintah ls -la untuk mengaudit perizinan dan perintah chmod untuk mengubah hak akses, saya memastikan sistem beroperasi sesuai dengan prinsip least privilege. Modifikasi pada file reguler, file tersembunyi, dan direktori ini memastikan bahwa data sensitif organisasi terlindungi dari akses dan modifikasi yang tidak sah.
