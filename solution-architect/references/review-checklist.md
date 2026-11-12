# Checklist Review Arsitektur

Gunakan checklist ini sebelum tech spec atau ADR diserahkan. Setiap poin dijawab ya atau tidak. Kalau jawabannya tidak, tulis tindakan yang diperlukan.

## 1. Keterlacakan ke Dokumen Analis

1. Setiap FR di PRD punya modul atau endpoint yang mengerjakannya.
2. Setiap NFR punya mekanisme teknis yang disebut, misalnya cache, replika, rate limit, atau hash.
3. Setiap integrasi di SRS bagian 6 punya strategi penanganan gagal.
4. Setiap entitas di ERD punya tabel, atau alasan tertulis kalau tidak ada.
5. Setiap BR di BRD yang memengaruhi logika punya tempat di salah satu modul.

## 2. Keputusan dan ADR

1. Setiap teknologi di stack punya ADR.
2. Setiap ADR punya minimal dua opsi yang dipertimbangkan.
3. Setiap ADR mencatat konsekuensi, termasuk yang negatif.
4. Status setiap ADR jelas: Proposed, Accepted, atau Superseded.
5. Tidak ada ADR yang bertentangan dengan ADR lain yang masih Accepted.

## 3. Versi dan Verifikasi

1. Setiap versi sudah diverifikasi dari sumber resmi, atau diberi penanda `[VERIFIKASI]`.
2. Tidak ada versi yang ditulis sebagai fakta tanpa sumber.
3. Runtime memakai versi LTS, kecuali ada alasan tertulis.

## 4. Kesederhanaan

1. Pola arsitektur sesuai ukuran tim dan volume. Microservices punya alasan tertulis.
2. Tidak ada layanan, antrean, atau cache yang tidak punya NFR atau FR sebagai alasan.
3. Jumlah teknologi baru dibatasi. Setiap teknologi tambahan menambah beban operasional.

## 5. Keamanan

1. Kata sandi di-hash dengan algoritma yang diakui.
2. Otorisasi dicek di server, bukan hanya di tampilan.
3. Token punya masa berlaku dan cara untuk memperbaruinya.
4. Data pribadi tidak masuk ke log.
5. Semua komunikasi memakai HTTPS.
6. Input divalidasi di setiap titik masuk.
7. Rahasia seperti kunci API dan kredensial database tidak ditulis di kode. Simpan di variabel lingkungan atau layanan pengelola rahasia.

## 6. Operasional

1. Ada rencana deploy yang tidak mematikan layanan, atau alasan tertulis kalau tidak ada.
2. Ada health check.
3. Ada backup dengan retensi, dan RPO serta RTO disebut atau masuk pertanyaan terbuka.
4. Log terstruktur dan punya request ID.
5. Metrik minimal dan kondisi alert sudah disebut.

## 7. Kesiapan untuk Developer

1. Struktur folder ditulis lengkap.
2. Konvensi kode, format, dan penanganan error sudah ada.
3. Setiap task punya ID, acuan FR atau NFR, dependensi, dan kriteria selesai.
4. Urutan task mengikuti dependensi.
5. Daftar library yang boleh dan dilarang sudah jelas.

## 8. Asumsi dan Pertanyaan

1. Setiap `[ASUMSI]` masuk daftar asumsi, lengkap dengan pihak yang memvalidasi.
2. Setiap `[VERIFIKASI]` masuk daftar tindak lanjut.
3. Setiap pertanyaan terbuka jelas dan menyebut siapa yang bisa menjawab.
4. Tidak ada pola khas AI yang tersisa, seperti pembuka basa-basi, penutup berlebihan, atau kata penguat kosong.

## Contoh Temuan Review

| Temuan | Tindakan |
|---|---|
| Redis dipakai untuk antrean, tapi tidak ada NFR atau FR yang membutuhkannya | Tulis alasannya di ADR-004, atau hapus dan pakai tabel antrean di PostgreSQL |
| Versi Prisma tertulis "terbaru" | Ganti dengan `[VERIFIKASI]` dan cek dokumentasi resmi |
| NFR-002 disebut terpenuhi, tapi tidak ada health check | Tambahkan health check dan rolling update di bagian 9 |
| Hosting sudah dipilih sebelum NFR-005 dikonfirmasi | Ubah status ADR-005 menjadi Proposed dan tambahkan pertanyaan terbuka |
