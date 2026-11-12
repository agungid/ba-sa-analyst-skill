# Backlog dan Sprint Plan: Aplikasi Pemesanan Laundry "BersihKilat"

> Contoh terisi untuk metode Agile. Acuannya PRD v0.1, SRS v0.1, dan tech spec v0.1. Velocity dan kapasitas masih berupa `[ASUMSI]` sampai tim memberi data.

| Field | Isi |
|---|---|
| Versi | 0.1 |
| Tanggal | 12 November 2026 |
| Status | Draft |
| Pemilik | [Nama Scrum Master atau Project Planner] |
| Product Owner | [Nama PO] |
| Acuan | PRD v0.1 (F, FR, NFR), SRS v0.1 (UC), tech spec v0.1 (TASK) |
| Durasi sprint | 2 minggu (14 hari kalender, 10 hari kerja) `[ASUMSI]` |
| Tanggal target go-live | 30 Juni 2027 (BRD, constraint) |

## Ringkasan

Rencana ini memecah fitur versi pertama BersihKilat menjadi 11 story dan 4 enabler, lalu menyusunnya ke dalam sprint 0 sampai sprint 7. Total 50 poin fitur ditambah pekerjaan enabler. Dengan velocity awal 20 poin `[ASUMSI]`, pekerjaan fitur membutuhkan sekitar tiga sprint penuh, sehingga sisa sprint dipakai untuk enabler, integrasi notifikasi, UAT, dan buffer.

## 1. Kapasitas Tim

| Peran | Jumlah | Hari kerja efektif per sprint | Catatan |
|---|---|---|---|
| Backend developer | 1 | 9 | Satu hari untuk rapat dan review `[ASUMSI]` |
| Mobile dan web developer | 1 | 9 | `[ASUMSI]` |
| QA | 1 | 9 | Mulai efektif dari sprint 1 |
| Product Owner | 1 | Paruh waktu | Tidak dihitung dalam kapasitas |

Velocity awal: 20 poin per sprint `[ASUMSI]`. Setelah sprint 2, angka ini diganti dengan rata-rata dua sprint terakhir.

## 2. Epic

| Epic | Nama | Acuan PRD | Prioritas |
|---|---|---|---|
| E-01 | Akun dan autentikasi | F-01 | Must |
| E-02 | Pemesanan dan pembatalan | F-02, F-05, F-07 | Must, Should |
| E-03 | Status dan notifikasi | F-03, F-04, F-06 | Must |
| E-04 | Dashboard admin | F-06 | Must |
| E-05 | Biaya simpan | F-10 | Could |
| E-06 | Riwayat dan pesan ulang | F-08, F-09 | Should, Could |

E-06 belum punya FR di PRD. Story di dalamnya ditandai `Ditahan` sampai FR-010 dan FR-011 ditulis.

## 3. Enabler (Sprint 0)

Enabler adalah pekerjaan teknis yang menjadi prasyarat fitur. Mengacu ke TASK di tech spec.

| ID | Pekerjaan | Acuan | Estimasi | Dependensi |
|---|---|---|---|---|
| TASK-001 | Setup repo, CI, dan konfigurasi linter | Tech spec bagian 5 | 3 | Tidak ada |
| TASK-002 | Setup PostgreSQL dan migration awal | ADR-002, ERD | 5 | TASK-001 |
| TASK-003 | Kerangka modul auth dan middleware peran | Tech spec bagian 6, NFR-004 | 5 | TASK-002 |
| TASK-004 | Backup harian dengan retensi 14 hari | Tech spec bagian 7 | 3 | TASK-002, keputusan hosting (ADR-005) |

Dependensi keputusan: TASK-004 dan seluruh deploy menunggu ADR-005 (hosting) setelah NFR-005 dikonfirmasi. Ini ditandai sebagai `Blocked (keputusan)`.

## 4. User Story

Format: *Sebagai [peran], saya ingin [aksi], supaya [manfaat].* Acceptance criteria diambil dari kriteria uji di SRS.

### E-01: Akun dan autentikasi

**US-001: Pendaftaran pelanggan** (Must, 5 poin)
Sebagai pelanggan, saya ingin mendaftar dengan nomor HP dan kata sandi, supaya saya bisa memesan laundry.
- Acuan: F-01, FR-001, UC-001
- Given nomor HP belum terdaftar, When pelanggan mengisi form dengan benar, Then akun dibuat dan pelanggan masuk
- Given nomor HP sudah terdaftar, When pelanggan mendaftar, Then sistem menolak dan menampilkan pesan yang jelas
- Dependensi: TASK-003
- Definition of Ready: terpenuhi

**US-002: Login pelanggan** (Must, 3 poin)
Sebagai pelanggan, saya ingin masuk ke akun saya, supaya saya bisa melihat dan membuat pesanan.
- Acuan: F-01, UC-002, NFR-003
- Given kata sandi benar, When login, Then token akses dibuat dan pelanggan masuk
- Dependensi: US-001

**US-003: Batas percobaan login** (Must, 2 poin)
Sebagai pemilik sistem, saya ingin login dibatasi, supaya akun tidak mudah ditebak.
- Acuan: Tech spec bagian 6 `[ASUMSI: 5 percobaan per 15 menit]`
- Dependensi: US-002

### E-02: Pemesanan dan pembatalan

**US-004: Buat pesanan dengan estimasi harga** (Must, 8 poin)
Sebagai pelanggan, saya ingin memilih outlet dan mengisi estimasi berat, supaya saya tahu perkiraan biayanya sebelum mengantar cucian.
- Acuan: F-02, FR-002, FR-003, UC-003
- Given berat 3,5 kg, Then perkiraan harga Rp 24.500 dan tertulis "perkiraan"
- Given berat 1,5 kg, Then sistem menampilkan "minimum pesanan 2 kg"
- Dependensi: US-002, TASK-002

**US-005: Batalkan pesanan** (Should, 3 poin)
Sebagai pelanggan, saya ingin membatalkan pesanan, supaya saya tidak perlu datang ke outlet jika berubah pikiran.
- Acuan: F-05, FR-004, UC-005
- Given status "diterima", When dibatalkan, Then status berubah menjadi dibatalkan
- Given status "dicuci", When dibatalkan, Then pembatalan ditolak
- Dependensi: US-004

### E-03: Status dan notifikasi

**US-006: Admin mengubah status sesuai urutan** (Must, 5 poin)
Sebagai admin outlet, saya ingin mengubah status cucian satu per satu, supaya pelanggan tahu prosesnya.
- Acuan: F-06, FR-006, FR-008, UC-009
- Given status "diterima", When admin memilih "siap diambil", Then sistem menolak
- Setiap perubahan tercatat dengan timestamp
- Dependensi: US-004, TASK-003

**US-007: Input berat aktual** (Must, 3 poin)
Sebagai admin outlet, saya ingin memasukkan berat hasil timbangan, supaya tagihan sesuai berat asli.
- Acuan: F-07, FR-002, UC-008
- Dependensi: US-006

**US-008: Notifikasi status ke pelanggan** (Must, 8 poin)
Sebagai pelanggan, saya ingin menerima pesan setiap status cucian berubah, supaya saya tidak perlu menelepon outlet.
- Acuan: F-04, FR-005, UC-010, IF-01
- Given status berubah, Then tepat satu notifikasi terkirim
- Given pengiriman gagal, Then sistem mencoba ulang sampai 3 kali
- Dependensi: US-006, keputusan kanal notifikasi (WhatsApp atau SMS, pertanyaan terbuka SRS bagian 9)

**US-009: Pelanggan melihat status** (Must, 3 poin)
Sebagai pelanggan, saya ingin melihat status cucian saya, supaya saya tahu kapan bisa mengambilnya.
- Acuan: F-03, UC-004
- Dependensi: US-006

### E-04: Dashboard admin

**US-010: Daftar pesanan hari ini** (Must, 5 poin)
Sebagai admin outlet, saya ingin melihat pesanan hari ini dari yang paling lama, supaya saya bisa mengerjakan yang paling mendesak dulu.
- Acuan: F-06, FR-007, NFR-001, UC-007
- Given 500 pesanan aktif, Then daftar tampil di bawah 2 detik pada 4G
- Dependensi: US-006

### E-05: Biaya simpan

**US-011: Hitung biaya simpan harian** (Could, 5 poin)
Sebagai pemilik usaha, saya ingin biaya simpan dihitung otomatis, supaya tidak perlu menghitung manual.
- Acuan: F-10, FR-009, BR-003
- Status: `Blocked (keputusan)`. BR-003 belum disetujui manajemen. UC-011 belum ada di SRS.
- Dependensi: keputusan manajemen (BRD bagian 6), UC-011

### E-06: Riwayat dan pesan ulang

**US-012: Riwayat pesanan** (Should) dan **US-013: Pesan ulang** (Could)
Status: `Ditahan`. Keduanya belum punya FR. Tidak dijadwalkan sampai FR-010 dan FR-011 ditulis dan disetujui.

## 5. Sprint Plan

| Sprint | Tanggal (perkiraan) | Sprint goal | Story atau enabler | Poin |
|---|---|---|---|---|
| Sprint 0 | 1 sampai 14 Desember 2026 | Fondasi teknis siap dipakai | TASK-001, TASK-002, TASK-003 | 13 |
| Sprint 1 | 15 sampai 28 Desember 2026 | Pelanggan bisa daftar dan masuk | US-001, US-002, US-003 | 10 |
| Sprint 2 | 29 Desember 2026 sampai 11 Januari 2027 | Pelanggan bisa membuat pesanan | US-004, US-005 | 11 |
| Sprint 3 | 12 sampai 25 Januari 2027 | Admin bisa mengelola status | US-006, US-007, US-009 | 11 |
| Sprint 4 | 26 Januari sampai 8 Februari 2027 | Notifikasi status berjalan | US-008, TASK-004 (jika ADR-005 sudah Accepted) | 11 |
| Sprint 5 | 9 sampai 22 Februari 2027 | Dashboard admin dan perbaikan | US-010, buffer | 5 + buffer |
| Sprint 6 | 23 Februari sampai 8 Maret 2027 | Buffer dan perbaikan dari uji pertama | Bug dan sisa story | Buffer |
| Sprint 7 | 9 sampai 22 Maret 2027 | Pilot dengan satu outlet | UAT, perbaikan | Buffer |

Catatan jadwal:
- Tanggal ini dihitung dari velocity 20 poin `[ASUMSI]`. Setelah sprint 2, tanggal dihitung ulang.
- Libur Natal dan Tahun Baru tidak dikurangi dari kapasitas di sprint 1 dan 2 `[ASUMSI]`. Jika tim libur, kapasitas sprint tersebut dikurangi.
- US-011 dan E-06 tidak masuk jadwal sampai keputusan dan FR tersedia.

## 6. Jadwal Ringkas Menuju Go-live

| Milestone | Target | Dasar |
|---|---|---|
| Fondasi siap | Akhir sprint 0 | TASK-001 sampai TASK-003 selesai |
| Pesanan bisa dibuat | Akhir sprint 2 | US-004 dan US-005 lulus acceptance criteria |
| Operasional outlet berjalan | Akhir sprint 4 | US-006 sampai US-009 dan US-008 lulus |
| UAT selesai | Akhir sprint 6 | Semua story Must lulus |
| Pilot satu outlet | Akhir sprint 7 | Persetujuan sponsor |
| Go-live | Sebelum 30 Juni 2027 | BRD, constraint tenggat |

Jadwal ini menyisakan lebih dari tiga bulan sebelum tenggat. Sisa waktu tidak dianggap sebagai buffer resmi sampai sponsor menyetujui.

## 7. Risiko Jadwal

| ID | Risiko | Dampak | Mitigasi |
|---|---|---|---|
| RJ-001 | Keputusan hosting (ADR-005) tertunda karena NFR-005 belum dikonfirmasi | Tinggi | TASK-004 dan deploy dipindah ke sprint berikutnya |
| RJ-002 | Velocity lebih rendah dari 20 poin | Sedang | Hitung ulang setelah sprint 2, kurangi scope Should |
| RJ-003 | Integrasi WhatsApp Business API memakan waktu lebih dari perkiraan | Tinggi | Spike (investigasi singkat) di sprint 3 untuk menguji API |
| RJ-004 | Libur akhir tahun mengurangi kapasitas | Sedang | Kapasitas sprint 1 dan 2 dikurangi, dicatat sebagai revisi |

## 8. Pertanyaan Terbuka

1. Apakah kapasitas tim seperti di bagian 1 sudah benar?
2. Apakah tanggal 30 Juni 2027 bisa digeser jika scope Should belum selesai?
3. Siapa yang menjadi Product Owner dan memutuskan prioritas Should dan Could?

## 9. Riwayat Revisi

| Versi | Tanggal | Perubahan | Pemohon |
|---|---|---|---|
| 0.1 | 12 November 2026 | Draft pertama dari PRD v0.1 dan tech spec v0.1 | Project Planner |