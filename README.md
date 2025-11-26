# 🏫 Pemodelan Sistem Antrian Penjadwalan Seminar Proposal, Seminar Hasil, dan Sidang Tugas Akhir Program Studi Sains Data Menggunakan Model M/M/1 dan M/M/c

## 📌 Deskripsi Proyek

Proyek ini merupakan penelitian untuk memodelkan sistem antrian pada tiga jenis proses sidang akademik yang ada di **Program Studi Sains Data Institut Teknologi Sumatera**, yaitu:
- **Seminar Proposal**
- **Seminar Hasil**
- **Sidang Tugas Akhir**

## 🧠 Latar Belakang Singkat

Program Studi Sains Data menghadapi tantangan dalam mengelola penjadwalan sempro, semhas, dan sidang akhir dengan jumlah ruangan terbatas dan permintaan yang bervariasi. Pengamatan awal menunjukkan bahwa utilisasi ruangan pada beberapa hari mencapai 68,75% dengan waktu tunggu rata-rata mencapai 98,59 menit, mengindikasikan adanya ketidakseimbangan antara kapasitas pelayanan dan permintaan. Kondisi ini berpotensi menurunkan kepuasan mahasiswa dan menghambat kelancaran proses penyelesaian studi.

Setiap periode sidang, banyak mahasiswa datang hampir bersamaan untuk mengikuti proses Sempro, Semhas, atau Sidang TA. Jumlah server (ruang sidang/penguji) yang terbatas dapat menimbulkan bottleneck, seperti:
- Jadwal molor
- Penumpukan antrian mahasiswa
- Utilisasi dosen yang tidak optimal

Penelitian ini bertujuan untuk mengukur performa sistem seperti waktu tunggu, utilisasi, dan panjang antrian, serta mengevaluasi apakah sistem saat ini bekerja secara optimal atau memerlukan perbaikan dengan pendekatan model antrian yang digunakan adalah **M/M/1** dan **M/M/c**.

## 🎯 Tujuan Penelitian

- Menentukan model antrian yang sesuai untuk sistem penjadwalan sempro, semhas, dan sidang menggunakan pendekatan **M/M/1** dan **M/M/c**
- Menghitung ukuran kinerja utilisasi **(ρ)**, rata-rata jumlah mahasiswa dalam antrian **(Lq)**, rata-rata jumlah mahasiswa dalam sistem **(Ls)**, rata-rata waktu tunggu dalam antrian **(Wq)**, dan rata-rata waktu tunggu dalam sistem **(Ws)**

---

## 🧮 Model Antrian yang Digunakan

### M/M/1
- 1 server, arrival ~ Poisson
- service ~ Eksponensial.

Digunakan untuk skenario satu ruang

### M/M/c
- c server paralel (beberapa ruang/penguji)
- arrival ~ Poisson
- service ~ Eksponensial
- Menggunakan formula Erlang-C untuk menghitung Lq dan probabilitas antrian.

---

## 🧩 Alur Penelitian 

Berikut adalah langkah-langkah yang dilakukan dalam Pemodelan Sistem Antrian Penjadwalan Seminar Proposal, Seminar Hasil, dan Sidang Tugas Akhir Program Studi Sains Data Menggunakan Model M/M/1 dan M/M/c.

### Pengumpulan Data

Data dikumpulkan dari database Tugas Akhir Program Studi Sains Data dari Januari 2024 sampai Desember 2024.
Data meliputi:
- tanggal, ruangan, tipe kegiatan
- jam mulai–selesai jadwal
- jam mulai–selesai aktual
- waktu check-in mahasiswa
- durasi real pelayanan

### Preprocessing Data
🔹 Model M/M/1 (single-server):

Data dikelompokkan berdasarkan kombinasi (ruangan, tanggal).
Mengurutkan mahasiswa berdasarkan arrival_time (check-in).
Menghitung:
- interarrival time untuk menentukan λ
- service time untuk menentukan μ

**Alasan: antrian hanya valid per ruangan per tanggal.**

🔹 Model M/M/c (multi-server):

Data diagregasi per tanggal, bukan per ruangan.
Mengidentifikasi:
- λ_per_hari = jumlah mahasiswa / 8 jam operasional
- c = jumlah ruangan aktif per hari
- μ = 60 / rata-rata durasi slot

### Penentuan Parameter Model

**Model M/M/1**
- λ = 1 / rata-rata interarrival.
- μ = 1 / rata-rata service time.
- ρ = λ / μ.

**Model M/M/c**
- λ = kedatangan per jam
- μ = pelayanan per jam
- ρ = λ / (c × μ)
P₀ dihitung menggunakan formula Erlang-C.

### Seleksi Ruang–Tanggal yang Valid

Model M/M/1 ditetapkan hanya untuk kombinasi yang memenuhi kriteria:
- hanya satu ruangan aktif pada hari tersebut
- n ≥ 2 mahasiswa
- ρ < 1 (stabil)

**Terpilih 16 kombinasi ruangan–tanggal.**

Model M/M/c:
- c ≥ 2 ruangan aktif
- λ > 0
- ρ < 1

**Terdapat 16 hari yang valid.**

### Analisis Performa Sistem

Menghitung ukuran kinerja:
Untuk M/M/1:
- Lq = ρ² / (1 − ρ)
- Ls = ρ / (1 − ρ)
- Wq = Lq / λ
- Ws = 1 / (μ − λ)

Untuk M/M/c (menggunakan Erlang-C):
- P₀
- Lq = (P₀ × (cρ)ᶜ × ρ) / (c! (1 − ρ)² )
- Wq = Lq / λ
- Ws = Wq + 1/μ
- Ls = λ × Ws

### Simulasi Kejadian Diskrit (DES)

Simulasi dilakukan untuk menggambarkan dinamika antrian nyata.
Event yang dibangkitkan:
- kedatangan → distribusi eksponensial (λ)
- pelayanan → distribusi eksponensial (μ)

Visualisasi:
- arrival vs departure
- Jumlah pelanggan dalam sistem (N(t))

Simulasi menunjukkan titik-titik bottleneck seperti ruang **F206**, **F213**, dan **F214**.
**20 Desember 2024** menjadi hari dengan antrian ppaling sibuk.

### Interpretasi dan Rekomendasi

Ruang dengan ρ tinggi **rawan antrian panjang.**
Ruang dengan ρ < 0.4  **underutilized.**

Rekomendasi:
- Pemerataan jadwal antar-ruangan
- Penyesuaian durasi slot
- Menambah buffer antar jadwal
- Penambahan server (ruangan) pada tanggal padat

## 👥 Tim Penyusun

- Natasya Ega Lina Marbun    (122450024)
- Esteria Ronauli Sidauruk   (122450025)
- Elia Meylani Simanjuntak   (122450026)
- Ukasyah Muntaha		         (122450028) 
