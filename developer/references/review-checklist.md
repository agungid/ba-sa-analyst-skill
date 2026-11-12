# Checklist Review Kode dan Laporan Developer

## 0. Penelusuran Mutlak (dicek pertama)

1. Setiap fungsi, field, kolom, dan library punya ID acuan yang ada di dokumen.
2. Tidak ada helper atau abstraksi yang tidak disebut di tech spec.
3. Respons API sama persis dengan API spec, tanpa field tambahan.
4. Tidak ada kasus error atau aturan bisnis yang tidak tertulis.
5. Tidak ada refactor atau perubahan di luar task.

Gunakan checklist ini sebelum task diserahkan ke review. Setiap poin dijawab ya atau tidak. Kalau tidak, tulis tindakan yang diperlukan.

## 1. Keterlacakan

1. Setiap fungsi, endpoint, tabel, dan migration punya acuan ke FR, NFR, TASK, atau ADR.
2. Setiap kriteria selesai di task punya test yang menyebut ID-nya.
3. Commit menyebut ID task atau requirement di awal pesan.
4. Tidak ada kode yang tidak punya acuan dan tidak ada di task.

## 2. Test

1. Test ditulis sebelum kode, dan pernah gagal dengan alasan yang benar.
2. Semua test untuk task ini lulus.
3. Test lama tidak rusak. Tidak ada test yang dihapus atau diubah ekspektasinya tanpa alasan tertulis.
4. Tidak ada `skip` atau `only` yang tertinggal tanpa ID task.
5. Logika bisnis tidak di-mock.

## 3. Keamanan

1. Kata sandi di-hash dengan bcrypt atau algoritma yang disebut di ADR.
2. Otorisasi dicek di server, bukan hanya di tampilan.
3. Input divalidasi di setiap titik masuk.
4. Tidak ada rahasia (kunci API, kata sandi, token) di kode, konfigurasi, atau log.
5. Data pribadi tidak masuk ke log.
6. Query database memakai parameter, tidak menyambung string dari input pengguna.

## 4. Data dan Migration

1. Setiap perubahan schema ada di migration.
2. Migration punya nama yang jelas dan jalur rollback atau sudah diuji ulang.
3. ERD dan kamus data ikut diperbarui dalam commit yang sama.
4. Tipe uang integer, berat decimal(5,2), waktu disimpan UTC.

## 5. Konvensi dan Kualitas

1. Lint dan type check lulus.
2. Tidak ada `console.log`, `TODO` tanpa ID, atau kode yang dikomentari tertinggal.
3. Nilai bisnis disimpan sebagai konstanta bernama dengan acuan.
4. Error memakai kelas dari `shared/errors`, dengan kode yang sama dengan API spec.
5. Modul lain diakses hanya lewat `index.ts`.
6. Library yang dipakai ada di daftar boleh.

## 6. Laporan

1. Laporan menyebut ID task, requirement yang dipenuhi, file yang berubah, dan hasil test.
2. Keputusan kecil dan alasannya tercatat.
3. Semua `[ASUMSI]` tercatat dengan pemilik untuk validasi.
4. Hal yang belum selesai atau gagal ditulis jujur, tidak disembunyikan.
5. Tidak ada pola khas AI di laporan, seperti pembuka basa-basi, penutup berlebihan, atau kata penguat kosong.

## Contoh Temuan Review

| Temuan | Tindakan |
|---|---|
| Test registrasi di-skip karena gagal di CI | Perbaiki penyebabnya, lalu hapus `skip`. Kalau belum bisa, tulis ID task dan alasan. |
| Harga dihitung dengan `float` | Ganti ke integer rupiah sesuai ERD dan konvensi bagian 7 |
| Log menampilkan nomor HP lengkap | Samarkan nomor di logger dan tambahkan ke daftar field terlarang |
| Commit berisi perbaikan bug di modul lain | Pisahkan jadi task baru dan commit terpisah |
| Migration mengubah tipe kolom tanpa catatan risiko | Tambahkan catatan risiko di ERD dan tanyakan ke solution architect |