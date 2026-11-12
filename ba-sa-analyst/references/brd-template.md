# BRD: Aplikasi Pemesanan Laundry "BersihKilat"

> Contoh terisi. Ganti isinya dengan data proyekmu. Bagian yang bertanda `[ASUMSI]` harus divalidasi sebelum dokumen disetujui.

| Field | Isi |
|---|---|
| Versi | 0.1 |
| Tanggal | 12 November 2026 |
| Status | Draft |
| Pemilik | [Nama Business Analyst] |
| Sponsor | [Nama Direktur Operasional] |
| Metode | Hybrid: BRD dan arsitektur Waterfall, pengembangan fitur Agile |

## Ringkasan

Pelanggan selama ini pesan laundry lewat telepon dan WhatsApp. Di jam sibuk, admin kewalahan dan sering salah catat berat cucian, sehingga tagihan tidak akurat. Aplikasi ini membuat pesanan tercatat otomatis, pelanggan bisa memantau status cucian sendiri, dan admin punya data yang bisa dipercaya untuk menagih.

## 1. Latar Belakang Masalah

Admin menerima sekitar 120 pesanan per hari lewat WhatsApp. Menurut perkiraan manajer operasional, sekitar 30% pesanan terlewat atau salah catat. `[ASUMSI: angka dari perkiraan, perlu dicek ulang dengan log chat]`

Banyak pelanggan menelepon hanya untuk menanyakan status cucian. Pertanyaan ini memakan waktu admin yang seharusnya dipakai untuk melayani pelanggan di outlet.

Saat ini tidak ada data pesanan yang tersimpan rapi, jadi manajemen sulit melihat pola pelanggan.

## 2. Tujuan Bisnis

| ID | Tujuan | Ukuran | Target | Tenggat |
|---|---|---|---|---|
| BO-001 | Mengurangi pesanan yang salah catat | Persentase pesanan salah catat per bulan | Turun dari 30% menjadi di bawah 5% | 3 bulan setelah go-live |
| BO-002 | Mengurangi pertanyaan status | Jumlah pesan status per hari | Turun 60% | 3 bulan setelah go-live |
| BO-003 | Menambah pesanan berulang | Persentase pelanggan yang memesan lagi dalam 30 hari | Naik dari 25% menjadi 35% `[ASUMSI]` | 6 bulan setelah go-live |

## 3. Business Case

Biaya pengembangan diperkirakan Rp 180 juta, ditambah biaya operasional Rp 3 juta per bulan untuk hosting dan layanan notifikasi. `[ASUMSI: belum ada penawaran vendor]`

Manfaat utamanya adalah penghematan waktu admin sekitar 4 jam per hari dan berkurangnya kesalahan tagihan. Nilai rupiah dari manfaat ini belum dihitung. `[ASUMSI]`

Payback period (lama waktu sampai biaya kembali) belum bisa dihitung sebelum nilai manfaat disepakati.

## 4. Stakeholder

| Stakeholder | Peran | Kepentingan | Pengaruh |
|---|---|---|---|
| Direktur Operasional | Sponsor | Efisiensi dan pertumbuhan omzet | Tinggi |
| Admin Outlet | Pengguna internal utama | Pesanan cepat dan tidak salah catat | Sedang |
| Kurir | Pengguna internal | Daftar pengambilan yang jelas | Sedang |
| Pelanggan | Pengguna eksternal | Bisa pesan cepat dan tahu status cucian | Tinggi |
| Tim IT | Pengelola sistem | Mudah dirawat | Sedang |

## 5. Ruang Lingkup

### Termasuk (In Scope)
- Pemesanan laundry kiloan lewat aplikasi
- Pelacakan status: diterima, dicuci, disetrika, siap diambil, selesai
- Hitung harga otomatis berdasarkan berat
- Notifikasi status ke pelanggan
- Dashboard admin untuk pesanan harian

### Tidak Termasuk (Out of Scope)
- Laundry satuan seperti kemeja, jas, atau sepatu
- Pembayaran online. Pembayaran tetap tunai atau transfer manual.
- Integrasi dengan sistem akuntansi
- Aplikasi untuk kurir. Dibahas di fase berikutnya.
- Program poin atau loyalitas

## 6. Aturan Bisnis

| ID | Aturan | Sumber |
|---|---|---|
| BR-001 | Harga laundry kiloan Rp 7.000 per kg, pesanan minimal 2 kg | Daftar harga outlet |
| BR-002 | Pelanggan hanya bisa membatalkan pesanan sebelum status "dicuci" | Kebijakan operasional `[ASUMSI]` |
| BR-003 | Cucian yang belum diambil lebih dari 7 hari kena biaya simpan Rp 2.000 per hari | `[ASUMSI: belum disetujui manajemen]` |

## 7. Batasan (Constraints)

- Anggaran pengembangan maksimal Rp 180 juta.
- Go-live paling lambat 30 Juni 2027.
- Aplikasi berjalan di Android dan web. Versi iOS tidak termasuk tahap ini.
- Data pelanggan disimpan di server dalam negeri. `[ASUMSI: mengacu kebijakan internal, perlu konfirmasi]`

## 8. Asumsi dan Ketergantungan

| ID | Asumsi atau ketergantungan | Status | PIC |
|---|---|---|---|
| AS-001 | Log WhatsApp bisa diakses untuk mengecek angka 30% | Belum dicek | Manajer Operasional |
| AS-002 | Setiap outlet sudah punya timbangan digital | Belum dicek | Admin Outlet |
| AS-003 | Layanan WhatsApp Business API tersedia untuk notifikasi | Belum dicek | Tim IT |

## 9. Risiko Awal

| ID | Risiko | Dampak | Kemungkinan | Mitigasi |
|---|---|---|---|---|
| RK-001 | Admin enggan pindah dari WhatsApp | Tinggi | Sedang | Pelatihan dan masa paralel 1 bulan |
| RK-002 | Pelanggan lansia kesulitan memakai aplikasi | Sedang | Tinggi | Pesan lewat admin tetap tersedia |
| RK-003 | Biaya notifikasi melebihi perkiraan | Sedang | Sedang | Batasi jumlah notifikasi per pesanan |

## 10. Ukuran Keberhasilan

Proyek dianggap berhasil jika BO-001 dan BO-002 tercapai dalam 3 bulan setelah go-live. BO-003 harus menunjukkan tren naik dalam 6 bulan.

## 11. Pertanyaan Terbuka

1. Apakah log WhatsApp bisa diakses untuk mengecek angka 30%?
2. Siapa yang menyetujui biaya simpan di BR-003?
3. Apakah ada aturan tambahan untuk penyimpanan data pelanggan?

## 12. Persetujuan

| Peran | Nama | Tanggal | Tanda Tangan |
|---|---|---|---|
| Sponsor | | | |
| Business Analyst | | | |
| Perwakilan IT | | | |