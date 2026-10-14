# UTS_Daspro_KelompokSukaHati
# Asisten Keuangan Mini

Asisten Keuangan Mini adalah program Pascal berbasis teks untuk membantu mahasiswa mengatur uang saku. Lewat program ini pengguna bisa mencatat pengeluaran, memantau sisa saldo, menabung harian sampai target tercapai, dan melihat status keuangannya. Semua data disimpan ke file CSV, jadi saat program dibuka lagi, saldo dan progres tabungan terakhir langsung dimuat dan bisa dilanjutkan.

Proyek ini dibuat untuk UTS Dasar Pemrograman (DP5) Semester Ganjil T.A. 2026/2027, Program Studi Ilmu Komputer, Fasilkom-TI Universitas Sumatera Utara.

---

## Anggota Kelompok

1. Nama: Dafa Dwi Andika (261401057), KOM : C, Lab : 2
2. Nama: Adzra Bariqah (261401012), KOM : C, Lab : 2
3. Nama: Muhammad Syukur (261401015), KOM : C, Lab : 2

---

## Fitur Utama

### 1. Muat data keuangan otomatis

Saat program dibuka, program mengecek apakah file `riwayat_keuangan.csv` sudah ada.

- Jika sudah ada, program membaca file sampai baris terakhir, lalu mengambil nomor transaksi terakhir dan sisa saldo terakhir. Dengan begitu pengguna bisa langsung melanjutkan pencatatan.
- Jika belum ada, program membuat file baru dan meminta pengguna mengisi uang saku awal. Angka itu menjadi saldo awal.

### 2. Muat data tabungan otomatis

Cara kerjanya sama dengan fitur pertama, tapi untuk file `simulasi_tabungan.csv`. Dari baris terakhir, program mengambil hari ke berapa tabungan terakhir dilakukan, total yang sudah terkumpul, dan sisa target. Target tabungan dihitung ulang dari total terkumpul ditambah sisa target.

### 3. Catat pengeluaran (menu 1)

- Pengguna memilih kategori: Makan & Minum, Transportasi, atau Kebutuhan Kuliah. Pilihan di luar 1 sampai 3 otomatis menjadi "Lainnya".
- Pengguna mengisi nominal pengeluaran. Jika nominalnya 0 atau negatif, program menolak dan menampilkan pesan error.
- Jika nominal valid, saldo dikurangi, nomor transaksi bertambah satu, lalu transaksi disimpan ke `riwayat_keuangan.csv`.

### 4. Tabungan harian (menu 2)

- Jika target belum diatur, pengguna diminta mengisi target tabungan. Jika sudah, program menampilkan target dan sisa yang harus dikumpulkan.
- Pengguna mengisi setoran hari ini (harus lebih dari 0).
- Program menambah hitungan hari dan menjumlahkan setoran ke total terkumpul.
- Jika total sudah mencapai atau melebihi target, statusnya "TERCAPAI!" dan muncul ucapan selamat. Jika belum, statusnya "Belum Tercapai" dan sisa target dihitung ulang.
- Hasilnya disimpan ke `simulasi_tabungan.csv`.

### 5. Cek status keuangan (menu 3)

Program menilai kondisi uang saku dari sisa saldo:

- Lebih dari Rp 200.000: status AMAN
- Di atas 0 sampai Rp 200.000: status WASPADA
- 0 atau minus: status DEFISIT

### 6. Keluar (menu 4)

Program berhenti. Pengguna tidak perlu menyimpan manual karena setiap transaksi sudah langsung ditulis ke file CSV saat dicatat.

### 7. Fungsi AmbilKolom

Data CSV dipisahkan tanda titik koma. Fungsi ini dipakai untuk mengambil isi satu kolom dari satu baris, misalnya kolom ke-4 (sisa saldo) dari baris terakhir. Hasilnya berupa teks, lalu diubah menjadi angka dengan perintah `Val`.

---

## Penjelasan Perulangan

### repeat-until (menu utama)

```pascal
repeat
  ... tampilkan menu, baca pilihan, proses pilihan ...
until pilihan = 4;
```

Menu ditampilkan berulang-ulang. Perulangan ini baru berhenti ketika pengguna memilih 4 (Keluar). Dipakai repeat-until karena menu harus tampil minimal satu kali, baru setelah itu dicek apakah pengguna ingin keluar.

### while-do (membaca file CSV)

```pascal
while not eof(fileCSV) do
  readln(fileCSV, baris);
```

Dipakai dua kali, yaitu untuk file keuangan dan file tabungan. Program membaca file baris demi baris sampai habis (eof berarti end of file). Karena variabel `baris` terus ditimpa, setelah perulangan selesai isinya adalah baris paling terakhir, yaitu data terbaru yang kita butuhkan.

### for-do (fungsi AmbilKolom)

```pascal
for i := 1 to Length(teks) do
```

Program menelusuri satu baris teks karakter demi karakter dari awal sampai akhir. Setiap karakter yang bukan titik koma dikumpulkan ke variabel sementara. Setiap ketemu titik koma, berarti satu kolom sudah selesai, penghitung kolom bertambah, dan variabel sementara dikosongkan untuk kolom berikutnya.

### break (menghentikan loop lebih awal)

Di dalam for-do pada AmbilKolom, ketika kolom yang dicari sudah selesai dibaca, `break` dipakai untuk langsung keluar dari perulangan. Ini menghemat proses karena kolom setelahnya tidak perlu dibaca lagi.

---

## Penjelasan Percabangan

### if-then-else saat memuat data

1. `if FileExists('riwayat_keuangan.csv')`: jika file keuangan ada, baca datanya. Jika tidak, buat file baru dan minta saldo awal.
2. `if (baris <> '') and (baris <> 'sep=;') and (AmbilKolom(baris, 1) <> 'No')`: memeriksa apakah baris terakhir benar-benar berisi data, bukan baris kosong, bukan baris `sep=;`, dan bukan judul kolom. Jika ya, saldo dan nomor transaksi diambil. Jika tidak, program meminta pengguna mengisi saldo awal.
3. `if FileExists('simulasi_tabungan.csv')`: sama dengan nomor 1, tapi untuk file tabungan. Jika belum ada, file dibuat beserta judul kolomnya.
4. `if (baris <> '') and (baris <> 'sep=;') and (AmbilKolom(baris, 1) <> 'Hari Ke')`: sama dengan nomor 2, tapi untuk data tabungan.

### if pada tampilan menu

5. `if targettabungan > 0`: target dan total terkumpul hanya ditampilkan di menu kalau target tabungan sudah diatur.

### case-of

6. `case pilihan of 1..4`: menentukan menu mana yang dijalankan. Bagian `else` menangani angka di luar 1 sampai 4 dengan pesan "Pilihan menu tidak valid".
7. `case kategori of 1..3`: menentukan nama kategori pengeluaran. Bagian `else` memberi nilai "Lainnya" untuk angka di luar 1 sampai 3.

### if pada menu 1 (catat pengeluaran)

8. `if pengeluaran <= 0`: jika nominal tidak valid, tampilkan error dan jangan simpan apa pun. Jika valid, kurangi saldo dan simpan transaksi.

### if pada menu 2 (tabungan)

9. `if targettabungan = 0`: jika target belum ada, minta pengguna mengisinya. Jika sudah ada, cukup tampilkan target dan sisa yang harus dikumpulkan.
10. `if setoranhariini <= 0`: jika setoran tidak valid, tampilkan error. Jika valid, proses setoran.
11. `if totalterkumpul >= targettabungan`: jika target tercapai, sisa target menjadi 0 dan status "TERCAPAI!". Jika belum, sisa target dihitung dan status "Belum Tercapai".
12. `if statustabungan = 'TERCAPAI!'`: pesan selamat hanya muncul ketika target baru saja tercapai.

### if bertingkat pada menu 3 (status keuangan)

13. Pengecekan dilakukan berurutan dari atas: `if sisa > 200000` (AMAN), kalau bukan lanjut `else if sisa > 0` (WASPADA), kalau bukan juga masuk `else` (DEFISIT). Karena berurutan, setiap kondisi hanya dicek jika kondisi sebelumnya salah.

### if di dalam AmbilKolom

14. `if teks[i] = ';'`: jika karakter saat ini adalah titik koma, berarti satu kolom selesai. Di dalamnya ada `if count = kolomke` untuk mengecek apakah ini kolom yang dicari. Jika ya, loop dihentikan dengan `break`. Jika bukan, penghitung kolom bertambah dan variabel sementara dikosongkan. Jika karakternya bukan titik koma (bagian `else`), karakter itu ditambahkan ke variabel sementara.

---

## Materi Praktikum yang Dipakai

- Input dan output: write, writeln, readln, clrscr
- Variabel dan tipe data: integer, longint, string, dan text untuk file
- Percabangan: if-then-else, nested if, case-of
- Perulangan: repeat-until, while-do, for-do
- Kontrol perulangan: break
- Function: AmbilKolom
- Pengolahan file: assign, reset, rewrite, append, close, FileExists

---

