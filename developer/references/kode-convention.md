# Konvensi Kode dan Komentar

Dokumen ini melengkapi tech spec bagian 4 dan 5. Kalau ada konflik, tech spec yang menang.

## 1. Struktur dan Penamaan

- Folder memakai `kebab-case`. File memakai pola `nama.jenis.ts`, misalnya `auth.service.ts`.
- Nama variabel, fungsi, dan kelas memakai bahasa Inggris. Istilah bisnis yang ada di glosarium boleh memakai bahasa Indonesia, misalnya `pesanan`, `status`, `outlet`.
- Konstanta bernilai tetap memakai `UPPER_SNAKE_CASE`, misalnya `MINIMUM_BERAT_KG`.
- Nama fungsi memakai kata kerja, misalnya `hitungHarga`, `ubahStatus`.
- Nama boolean memakai awalan `is`, `has`, atau `can`, misalnya `isAktif`.

## 2. Nilai Bisnis Tidak Boleh Ditulis Langsung di Kode

Angka dan aturan bisnis disimpan dalam konstanta yang punya nama dan acuan.

```typescript
// Benar
// BR-001: harga per kilogram laundry kiloan
export const HARGA_PER_KG_RUPIAH = 7000;

// Salah: angka 7000 tanpa nama dan acuan
const total = berat * 7000;
```

Setiap konstanta bisnis punya komentar acuan (BR, FR, atau NFR). Konstanta tanpa acuan tidak boleh masuk.

## 3. Komentar

Komentar menjelaskan kenapa, bukan apa. Kode yang sudah jelas tidak perlu dikomentari.

- Komentar wajib untuk aturan bisnis yang tidak jelas dari kodenya, dengan ID acuan.
- Komentar wajib untuk keputusan teknis yang tidak biasa, dengan nomor ADR.
- Komentar tidak boleh berisi kalimat pembuka basa-basi atau penjelasan yang diulang dari nama fungsi.
- `TODO` wajib menyebut ID task, misalnya `// TODO(TASK-012): ganti dengan layanan notifikasi`.
- `FIXME` dan `HACK` tidak boleh masuk ke cabang utama tanpa ID task dan alasan.

Contoh:

```typescript
// Benar
// FR-003: pesanan di bawah 2 kg ditolak sebelum disimpan.
if (beratKg < MINIMUM_BERAT_KG) {
  throw new ErrorValidasi('minimum pesanan 2 kg');
}

// Salah
// Fungsi ini digunakan untuk menghitung harga pesanan dengan benar.
function hitungHarga(beratKg: number): number { ... }
```

## 4. Penanganan Error

- Semua error dilempar dari kelas di `shared/errors`. Tidak boleh melempar `Error` biasa dari modul.
- Setiap kelas error punya kode yang sama dengan API spec, misalnya `NOMOR_TERDAFTAR` dengan status 409.
- Pesan error untuk pengguna ditulis dalam bahasa Indonesia yang jelas. Pesan teknis (stack trace) hanya masuk ke log.
- Error yang tidak dikenal ditangkap di middleware global, dicatat ke log tanpa data pribadi, dan dikembalikan sebagai error umum tanpa detail internal.

## 5. Validasi Input

- Setiap input dari luar (body, query, parameter, pesan antrean) divalidasi dengan Zod sebelum masuk ke fungsi bisnis.
- Skema validasi diletakkan di `*.schema.ts` dalam modul yang sama.
- Validasi tidak boleh hanya di frontend. Server selalu memvalidasi ulang.

## 6. Logging

- Gunakan logger dari `shared/logger`. Dilarang `console.log`.
- Log memakai format JSON dengan request ID.
- Field berikut tidak boleh masuk ke log: `kata_sandi`, `token`, `nomor_hp`, `alamat`, dan `email`. Kalau perlu untuk debugging, gunakan nilai yang sudah disamarkan, misalnya `08****1234`.

## 7. Waktu dan Uang

- Waktu disimpan dalam UTC. Konversi ke WIB dilakukan di lapisan tampilan atau di respons API sesuai API spec.
- Uang disimpan sebagai integer rupiah. Dilarang memakai `float` atau `number` desimal untuk harga.
- Berat disimpan sebagai `decimal(5,2)`. Di kode, pembulatan memakai fungsi tunggal yang disebut di tech spec, supaya hasilnya konsisten.

## 8. Test

- Nama test menyebut ID acuan, misalnya `TC-010: harga 3,5 kg menghasilkan Rp 24.500`.
- Satu test menguji satu hal. Nama test menjelaskan hasil yang diharapkan.
- Dilarang memakai `skip` atau `only` di cabang utama tanpa ID task dan alasan.
- Mock hanya dipakai untuk dependensi luar (layanan pihak ketiga, waktu sistem). Logika bisnis tidak boleh di-mock.

## 9. Dependensi dan Import

- Modul lain diakses hanya lewat `index.ts`. Import langsung ke file internal modul lain dilarang. Aturan ini dicek lewat lint.
- Library yang dipakai harus ada di daftar boleh di tech spec bagian 11.
- Import yang tidak dipakai dihapus sebelum commit.

## 10. Format

- Format memakai Prettier, lint memakai ESLint. Keduanya dijalankan sebelum commit.
- Konfigurasi tidak diubah tanpa ADR atau persetujuan tech lead.