# Template Task dan Contoh Pengerjaan

Template ini dipakai untuk setiap task yang dikerjakan developer. Contoh di bawah mengambil TASK-003 dari tech spec BersihKilat (kerangka modul auth dan middleware peran).

## Format Task

```markdown
# TASK-NNN: Judul Singkat

| Field | Isi |
|---|---|
| Acuan | FR, NFR, TASK, atau ADR yang dipenuhi |
| Dependensi | TASK lain yang harus selesai dulu |
| Modul | Folder atau modul yang disentuh |
| Status | Todo / In Progress / Review / Done / Blocked |
| Pengerjaan | Nama developer |

## Tujuan
Satu sampai dua kalimat tentang apa yang harus bisa dilakukan setelah task ini selesai.

## Kriteria Selesai
- [ ] Kriteria yang bisa diuji, diambil dari SRS atau tech spec
- [ ] Test untuk setiap kriteria lulus

## File yang Diubah
- Daftar file, dengan keterangan singkat

## Rencana Test
- Test unit: ...
- Test integrasi: ...

## Asumsi
- [ASUMSI] ... (pemilik untuk memvalidasi)

## Catatan Pengerjaan
Ditulis saat task berjalan. Berisi keputusan kecil dan alasannya.
```

## Contoh Terisi: TASK-003

# TASK-003: Kerangka Modul Auth dan Middleware Peran

| Field | Isi |
|---|---|
| Acuan | NFR-003 (hash kata sandi), NFR-004 (akses admin dicek di server), FR-001, UC-001, UC-002 |
| Dependensi | TASK-002 (PostgreSQL dan migration awal) |
| Modul | `src/modules/auth/`, `src/shared/middleware/` |
| Status | Todo |
| Pengerjaan | [Nama developer] |

## Tujuan

Modul auth bisa mendaftarkan pelanggan, memeriksa kata sandi lewat hash, dan membuat token akses. Middleware peran memeriksa hak akses sebelum endpoint admin dijalankan.

## Kriteria Selesai

- [ ] Registrasi dengan nomor HP baru berhasil. Nomor yang sudah terdaftar ditolak dengan kode error yang sama dengan API spec. (TC-001, TC-002)
- [ ] Kata sandi tersimpan sebagai hash bcrypt. Tidak ada kolom atau log yang berisi teks biasa. (NFR-003)
- [ ] Endpoint admin yang dipanggil akun pelanggan mengembalikan status 403. (NFR-004)
- [ ] Login dengan kata sandi salah tidak membuat token. (UC-002)
- [ ] Batas percobaan login diterapkan sesuai US-003. `[ASUMSI: 5 percobaan per 15 menit]`

## File yang Diubah

- `src/modules/auth/auth.routes.ts`: endpoint `/auth/daftar` dan `/auth/masuk`
- `src/modules/auth/auth.service.ts`: logika registrasi, login, dan hash
- `src/modules/auth/auth.schema.ts`: skema validasi input dengan Zod
- `src/modules/auth/index.ts`: ekspor fungsi publik modul
- `src/shared/middleware/peran.ts`: pengecekan peran
- `tests/unit/auth.service.test.ts`
- `tests/integrasi/auth.routes.test.ts`

## Rencana Test

- Unit: hash dan verifikasi kata sandi, validasi format nomor HP, pembuatan token.
- Integrasi: registrasi berhasil, registrasi ganda ditolak, login salah ditolak, akses admin oleh pelanggan mendapat 403.

## Asumsi

- [ASUMSI] Format nomor HP `08xxxxxxxxxx` saja. Format `+62` belum diputuskan. Pemilik: Product Owner.
- [ASUMSI] Batas login 5 kali per 15 menit per nomor HP. Pemilik: Tech Lead.

## Catatan Pengerjaan

- Tidak ada catatan. Task belum dikerjakan.