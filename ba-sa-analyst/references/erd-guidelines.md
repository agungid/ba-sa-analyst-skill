# Panduan ERD dan Kamus Data

Dokumen ini jadi aturan saat AI membuat, mengubah, atau memeriksa ERD (Entity Relationship Diagram, gambar hubungan antar tabel) dan kamus data. Acuannya SRS bagian 7 dan BRD bagian 6.

## 1. Prinsip Dasar

1. **Satu sumber kebenaran.** ERD dan kamus data harus sama dengan schema database (file migration atau schema). Kalau berbeda, schema yang dianggap benar, lalu dokumen diperbarui.
2. **Mulai dari konsep.** Pertama tentukan entitas bisnis, baru turunkan ke tabel, kolom, dan tipe data.
3. **Setiap angka punya asal.** Batas panjang, presisi desimal, dan nilai yang diizinkan harus bisa ditelusuri ke FR, NFR, atau BR.
4. **Jangan menambah tanpa dasar.** Tabel, kolom, atau index yang tidak punya acuan tidak boleh dibuat. Tanyakan dulu.

## 2. Tahap Pembuatan

| Tahap | Hasil | Contoh BersihKilat |
|---|---|---|
| Logis | Entitas, atribut inti, relasi, kardinalitas (jumlah pasangan yang boleh terjadi) | Pelanggan, Pesanan, Outlet, Riwayat Status |
| Fisik | Tabel, kolom, tipe data, PK, FK, UK, index | `pesanan`, `berat_estimasi_kg decimal(5,2)` |
| Dokumentasi | Kamus data dan diagram ER | Tabel di SRS bagian 7.2 |

PK (primary key) adalah kolom unik pengenal setiap baris. FK (foreign key) adalah kolom yang menunjuk ke PK di tabel lain. UK (unique key) adalah kolom yang nilainya tidak boleh sama antar baris.

## 3. Penamaan

- Nama tabel dan kolom memakai `snake_case` (huruf kecil dengan garis bawah) dan bahasa Indonesia sesuai glosarium. Istilah teknis baku tetap dipakai.
- Tabel untuk kumpulan data memakai bentuk jamak (`pesanan`, `pelanggan`).
- Primary key selalu bernama `id`.
- Foreign key memakai pola `<nama_tabel_tunggal>_id`, misalnya `pesanan_id` atau `pelanggan_id`.
- Kolom waktu memakai akhiran `_pada` atau `waktu`, misalnya `dibuat_pada`.
- Kolom boolean memakai awalan `is_` atau `sudah_`, misalnya `is_aktif`.
- Hindari singkatan yang tidak ada di glosarium.

## 4. Kunci dan Relasi

- Setiap tabel wajib punya primary key.
- Setiap relasi wajib punya foreign key di sisi "banyak". Contoh: `pesanan.pelanggan_id` menunjuk `pelanggan.id`.
- Kardinalitas wajib ditulis. Notasi yang dipakai: `||--o{` untuk satu ke banyak, `||--||` untuk satu ke satu, dan `}o--o{` untuk banyak ke banyak.
- Relasi banyak ke banyak tidak boleh langsung. Buat tabel penghubung dengan composite key (kunci gabungan dari dua kolom) atau PK sendiri.
- Unique constraint dipakai untuk aturan yang harus unik. Contoh: `pelanggan.nomor_hp` (FR-001).

Contoh dari BersihKilat:

| Relasi | Kardinalitas | Foreign Key | Acuan |
|---|---|---|---|
| Pelanggan membuat pesanan | Satu ke banyak | `pesanan.pelanggan_id` | UC-003 |
| Outlet menerima pesanan | Satu ke banyak | `pesanan.outlet_id` | UC-003 |
| Pesanan punya riwayat status | Satu ke banyak | `riwayat_status.pesanan_id` | FR-008 |

## 5. Tipe Data

| Jenis Data | Tipe yang Dipakai | Catatan |
|---|---|---|
| Uang (rupiah) | integer | Simpan rupiah utuh. Desimal bisa menimbulkan pembulatan yang meleset. |
| Berat (kg) | decimal(5,2) | Dua angka di belakang koma |
| Teks pendek | varchar dengan batas jelas | Batas panjang harus ditulis di kamus data |
| Teks panjang | text | Tanpa batas tetap |
| Waktu | timestamp dengan zona waktu | Disimpan dalam UTC, ditampilkan dalam WIB (SRS bagian 6.3) |
| ID | uuid atau bigint | Pilih satu dan pakai konsisten |
| Status | enum atau tabel referensi | Nilai yang diizinkan wajib tertulis di kamus data |

Contoh yang salah: harga disimpan sebagai `decimal`, lalu selisih pembulatan muncul di akhir bulan. Contoh yang benar: `total_harga_rupiah integer`.

## 6. Kamus Data

Setiap kolom wajib punya satu baris di kamus data.

| Kolom Dokumen | Isi | Wajib |
|---|---|---|
| Tabel | Nama tabel | Ya |
| Kolom | Nama kolom | Ya |
| Tipe | Tipe data | Ya |
| Nullable | Boleh kosong atau tidak | Ya |
| Aturan | Batas, format, nilai yang diizinkan | Ya |
| Acuan | ID FR, NFR, atau BR | Ya, kecuali kolom teknis seperti `id` dan `dibuat_pada` |
| Keterangan | Arti kolom dalam bahasa sehari-hari | Dianjurkan |

Contoh:

| Tabel | Kolom | Tipe | Nullable | Aturan | Acuan |
|---|---|---|---|---|---|
| pesanan | berat_estimasi_kg | decimal(5,2) | Tidak | Minimal 2,00 | FR-003 |
| pesanan | berat_aktual_kg | decimal(5,2) | Ya | Diisi admin saat menimbang | UC-008 |
| pesanan | status | enum | Tidak | diterima, dicuci, disetrika, siap_diambil, selesai | FR-006 |

## 7. Index

- Index dibuat hanya jika ada query yang membutuhkannya. Query itu harus disebut.
- Setiap foreign key sebaiknya punya index, supaya pencarian relasi tidak lambat.
- Unique constraint sudah otomatis punya index, jadi tidak perlu ditulis dua kali.
- Setiap index dicatat di kamus data beserta alasannya.

Contoh: `pesanan(outlet_id, status)` dipakai dashboard admin (FR-007), karena dashboard memfilter per outlet dan status.

## 8. Perubahan Schema

- Setiap perubahan tabel, kolom, atau relasi dilakukan lewat migration.
- Setiap migration diikuti pembaruan ERD dan kamus data dalam perubahan yang sama.
- Kolom baru wajib punya acuan requirement. Kalau tidak ada, tanyakan dulu.
- Menghapus kolom dilakukan lewat Change Request (Waterfall) atau catatan keputusan (Agile).
- Perubahan tipe data yang bisa menghilangkan data dicatat sebagai risiko.

## 9. Checklist Sebelum Menyerahkan ERD

1. Setiap tabel punya primary key.
2. Setiap relasi punya foreign key dan kardinalitas.
3. Tidak ada relasi banyak ke banyak yang langsung.
4. Setiap kolom ada di kamus data dengan tipe, nullable, dan aturan.
5. Setiap kolom bisnis punya acuan FR, NFR, atau BR.
6. Uang disimpan sebagai integer.
7. Nilai enum tertulis lengkap.
8. Tidak ada tabel atau kolom tanpa acuan.
9. ERD dan kamus data sudah cocok dengan schema.

## 10. Contoh Pelanggaran dan Perbaikannya

| Pelanggaran | Perbaikan |
|---|---|
| Tabel `pesanan` tidak punya foreign key ke `pelanggan` | Tambahkan `pelanggan_id` dengan FK ke `pelanggan.id` |
| Harga disimpan sebagai `float` | Ubah menjadi `total_harga_rupiah integer` |
| Pesanan dan layanan dihubungkan langsung secara banyak ke banyak | Buat tabel `pesanan_layanan` dengan composite key |
| Kolom `status` tanpa daftar nilai | Tulis daftar enum di kamus data dan acuan FR-006 |
| Kolom `catatan_tambahan` tanpa acuan | Tanyakan dulu. Jangan ditambahkan diam-diam. |