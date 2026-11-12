# Template dan Contoh ADR

Setiap keputusan teknis penting ditulis sebagai satu file ADR di folder `docs/decisions/`. Nama file memakai pola `ADR-NNN-judul-singkat.md`. Nomor tidak boleh dipakai ulang, dan ADR yang sudah diganti tetap disimpan.

## Format

```markdown
# ADR-NNN: Judul Keputusan

| Field | Isi |
|---|---|
| Status | Proposed / Accepted / Superseded by ADR-XXX / Deprecated |
| Tanggal | YYYY-MM-DD |
| Pengambil keputusan | Nama atau peran |
| Acuan | FR, NFR, BR, atau ADR lain yang terkait |

## Konteks
Masalah atau kebutuhan yang memaksa keputusan ini diambil. Sebut ID requirement yang relevan.

## Opsi yang Dipertimbangkan
Minimal dua opsi. Untuk setiap opsi, tulis kelebihan dan kekurangannya.

## Keputusan
Opsi yang dipilih dan alasan utamanya, dalam satu sampai dua paragraf.

## Konsekuensi
- Hal yang menjadi lebih mudah
- Hal yang menjadi lebih sulit
- Pekerjaan lanjutan (misalnya task baru)
- Dampak biaya operasional, kalau relevan

## Catatan
Asumsi, versi yang perlu diverifikasi, dan kapan keputusan ini perlu ditinjau ulang.
```

## Contoh Terisi: ADR-002

# ADR-002: Database Memakai PostgreSQL

| Field | Isi |
|---|---|
| Status | Accepted |
| Tanggal | 12 November 2026 |
| Pengambil keputusan | Solution Architect, disetujui Tech Lead |
| Acuan | NFR-001, NFR-002, NFR-003, NFR-007, FR-001, FR-006, FR-008, ERD di SRS bagian 7 |

## Konteks

Data BersihKilat bersifat relasional. Pelanggan membuat pesanan, pesanan punya riwayat status, dan pesanan terhubung ke outlet. Aturan integritasnya cukup ketat: nomor HP harus unik (FR-001), dan status tidak boleh melompat (FR-006). Dashboard juga harus tampil cepat untuk 500 pesanan aktif (NFR-001).

## Opsi yang Dipertimbangkan

**Opsi A: PostgreSQL**
- Kelebihan: mendukung foreign key (penanda relasi antar tabel), constraint (aturan nilai), dan transaksi dengan baik. Indeks parsial bisa dipakai untuk mempercepat dashboard. Didukung luas oleh penyedia hosting.
- Kekurangan: perlu pengaturan backup dan pemeliharaan yang lebih rinci dibanding layanan sederhana.

**Opsi B: MongoDB**
- Kelebihan: skema fleksibel, cocok untuk data yang sering berubah bentuk.
- Kekurangan: integritas relasi, misalnya pesanan harus punya pelanggan yang valid, harus dijaga di kode. Ini menambah risiko bug. ERD yang sudah dibuat dirancang sebagai tabel relasional.

**Opsi C: SQLite**
- Kelebihan: sederhana dan tidak memerlukan server database terpisah.
- Kekurangan: tidak cocok untuk dua instance backend yang menulis bersamaan (NFR-002).

## Keputusan

PostgreSQL dipilih. ERD di SRS sudah berbentuk relasi, dan NFR-002 mensyaratkan dua instance backend yang berjalan bersamaan. Opsi B dan C baru bisa memenuhi kebutuhan itu setelah banyak pekerjaan tambahan.

## Konsekuensi

- Integritas data dijaga oleh database, bukan hanya oleh kode aplikasi.
- Perlu task khusus untuk backup dan pemulihan (TASK-004).
- Migration dikelola lewat Prisma sesuai ADR-002.
- Biaya hosting database terkelola bergantung pada ukuran data. Estimasinya ada di BRD bagian 3.

## Catatan

- Versi PostgreSQL 16 perlu dicek di dokumentasi resmi sebelum status berubah menjadi Accepted.
- Keputusan ini ditinjau ulang kalau volume melebihi 10 kali target NFR-007.
