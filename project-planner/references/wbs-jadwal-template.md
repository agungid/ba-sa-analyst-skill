# WBS dan Jadwal: Aplikasi Pemesanan Laundry "BersihKilat" (Waterfall)

> Contoh terisi untuk metode Waterfall. Dipakai jika sponsor memilih Waterfall untuk seluruh proyek, atau untuk fase build pada hybrid. Durasi memakai hari kerja dan masih berupa `[ASUMSI]`.

| Field | Isi |
|---|---|
| Versi | 0.1 |
| Tanggal | 12 November 2026 |
| Status | Draft, menunggu baseline |
| Pemilik | [Nama Project Planner] |
| Acuan | BRD v0.1, SRS v0.1, tech spec v0.1 |
| Kalender kerja | Senin sampai Jumat, tidak termasuk hari libur nasional `[ASUMSI]` |
| Tanggal go-live | Paling lambat 30 Juni 2027 (BRD, constraint) |

## Ringkasan

Rencana ini memecah proyek menjadi enam fase berurutan. Setiap fase berakhir dengan milestone yang harus disetujui sebelum fase berikutnya dimulai. Jalur kritis berjalan dari desain database sampai UAT, dan menentukan tanggal go-live. Total durasi sekitar 110 hari kerja `[ASUMSI]`.

## 1. Struktur Pekerjaan (WBS)

| WBS | Pekerjaan | Deliverable | Durasi (hari kerja) | Dependensi | PIC |
|---|---|---|---|---|---|
| 1 | Analisis | | | | |
| 1.1 | BRD, PRD, SRS, ERD | Dokumen yang disetujui | Selesai | Tidak ada | BA |
| 1.2 | Keputusan NFR-005 dan hosting | ADR-005 Accepted | 10 | Konfirmasi lokasi server | Solution Architect |
| 2 | Desain | | | | |
| 2.1 | Tech spec dan ADR | Tech spec v1.0 disetujui | 10 | 1.1, 1.2 | Solution Architect |
| 2.2 | Desain UI dan alur | Prototipe disetujui | 10 | 1.1 | UI Designer |
| 2.3 | Migration database awal | Schema siap dijalankan | 4 | 2.1 | Backend |
| 3 | Build | | | | |
| 3.1 | Modul auth (TASK-003) | Login dan registrasi lulus uji | 8 | 2.3 | Backend |
| 3.2 | Modul pesanan dan status | Pembuatan, pembatalan, dan perubahan status lulus uji | 15 | 3.1 | Backend |
| 3.3 | Aplikasi pelanggan | Layar pesanan dan status | 15 | 2.2, 3.1 | Mobile dan web |
| 3.4 | Dashboard admin | Daftar pesanan dan input berat | 10 | 3.2 | Web |
| 3.5 | Notifikasi | Pengiriman status lewat antrean | 8 | 3.2, IF-01 | Backend |
| 3.6 | Biaya simpan | Penjadwal berjalan | 5 | 3.2, keputusan BR-003 | Backend |
| 4 | Pengujian sistem | | | | |
| 4.1 | Test case dari SRS bagian 8 | Test otomatis lulus | 10 | 3.3, 3.4, 3.5 | QA |
| 4.2 | Uji beban (NFR-001, NFR-007) | Laporan load test | 3 | 4.1 | QA |
| 4.3 | Perbaikan bug | Bug kritis nol | 7 | 4.1, 4.2 | Tim build |
| 5 | UAT | | | | |
| 5.1 | Persiapan UAT dan skenario | Rencana UAT disetujui | 3 | 4.3 | QA, PO |
| 5.2 | Pengujian oleh admin outlet | Berita acara UAT | 10 | 5.1 | Admin outlet |
| 5.3 | Perbaikan dari UAT | Bug UAT tertutup | 5 | 5.2 | Tim build |
| 6 | Go-live | | | | |
| 6.1 | Deploy produksi dan pelatihan | Aplikasi berjalan | 3 | 5.3 | DevOps, Admin |
| 6.2 | Periode paralel dengan WhatsApp | Laporan 1 bulan | 20 | 6.1 | Operasional |

## 2. Milestone

| Milestone | Tanggal target | Kriteria selesai | Penanggung jawab |
|---|---|---|---|
| M1: Dokumen disetujui | Akhir hari ke-10 | BRD, PRD, SRS, dan ERD ditandatangani | Sponsor |
| M2: Desain disetujui | Akhir hari ke-30 | Tech spec, ADR Accepted, dan prototipe disetujui | Solution Architect dan Sponsor |
| M3: Build selesai | Akhir hari ke-75 | Semua WBS 3.x lulus uji unit dan integrasi | Tech Lead |
| M4: Uji sistem selesai | Akhir hari ke-88 | Semua test case lulus, bug kritis nol | QA |
| M5: UAT disetujui | Akhir hari ke-106 | Berita acara UAT ditandatangani | Sponsor |
| M6: Go-live | Akhir hari ke-110 | Aplikasi berjalan di produksi | Sponsor |

Tanggal dalam hari dihitung dari hari mulai baseline. Setelah baseline disetujui, hari diubah menjadi tanggal kalender.

## 3. Jalur Kritis

Jalur kritis adalah rangkaian tugas yang tidak boleh terlambat tanpa menggeser go-live:

1.1 → 1.2 → 2.1 → 2.3 → 3.1 → 3.2 → 3.4 → 4.1 → 4.3 → 5.1 → 5.2 → 5.3 → 6.1

Tugas di luar jalur kritis punya float (waktu longgar). Contoh: 2.2 desain UI punya float sekitar 20 hari kerja, sedangkan 3.6 biaya simpan punya float sekitar 25 hari kerja selama BR-003 sudah diputuskan sebelum fase build.

Keterlambatan 1.2 (keputusan hosting) menggeser seluruh jalur kritis. Karena itu keputusan ini masuk daftar risiko paling atas.

## 4. Dependensi Keputusan dan Data

| ID | Dependensi | Jenis | Dibutuhkan untuk | Tenggat keputusan |
|---|---|---|---|---|
| DK-001 | Konfirmasi lokasi server (NFR-005) | Keputusan | 1.2, 2.1 | Akhir hari ke-10 |
| DK-002 | Kanal notifikasi: WhatsApp atau SMS | Keputusan | 3.5 | Akhir hari ke-30 |
| DK-003 | Persetujuan biaya simpan (BR-003) | Keputusan | 3.6 | Akhir hari ke-60 |
| DD-001 | Akses log WhatsApp untuk validasi angka 30% | Data | Konfirmasi BRD | Akhir hari ke-10 |
| DT-001 | Penawaran vendor WhatsApp Business API | Teknis | 3.5 | Akhir hari ke-30 |

Dependensi yang tidak terpenuhi tepat waktu membuat pekerjaan terkait ditandai `Blocked`. Kalau dependensi di jalur kritis terlambat, PO dan sponsor diberi tahu pada hari yang sama.

## 5. Estimasi dan Buffer

| Komponen | Hari kerja | Dasar |
|---|---|---|
| Total pekerjaan langsung | 92 | Jumlah WBS di bagian 1 |
| Buffer risiko teknis (sekitar 15 persen) | 14 | Integrasi WhatsApp, deploy pertama |
| Buffer total | 14 | Ditampilkan terpisah, tidak disembunyikan di estimasi |
| Total | 106 | Sampai akhir UAT |
| Periode paralel (tidak masuk jalur build) | 20 | Operasional outlet |

Total sekitar 110 hari kerja dari baseline sampai go-live. Kalau baseline mulai 1 Desember 2026, go-live dihitung sekitar awal Mei 2027 dengan kalender kerja. Tanggal ini masih di bawah tenggat 30 Juni 2027 `[ASUMSI]`. Angka ini dihitung ulang setelah baseline disetujui.

## 6. Change Request yang Mungkin Muncul

| ID | Permintaan | Dampak jadwal | Dampak ke WBS | Status |
|---|---|---|---|---|
| CR-001 | Tambah pembayaran QRIS | Belum dihitung. Di luar scope BRD. | Modul baru, ADR baru | Belum diajukan |

Setiap CR diajukan sebagai dokumen terpisah dengan alasan, dampak hari kerja, dampak biaya, dan dokumen yang perlu diperbarui. Baseline tidak berubah sebelum CR disetujui.

## 7. Pertanyaan Terbuka

1. Apakah kalender kerja 5 hari per minggu sudah sesuai?
2. Apakah periode paralel 20 hari kerja disetujui sponsor?
3. Siapa yang menjadi penanggung jawab keputusan DK-001?

## 8. Persetujuan Baseline

| Peran | Nama | Tanggal | Tanda Tangan |
|---|---|---|---|
| Sponsor | | | |
| Project Planner | | | |
| Tech Lead | | | |
| Product Owner atau BA | | | |