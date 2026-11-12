# Checklist NFR (Non-Functional Requirements)

Dokumen ini dipakai AI saat menyusun, meninjau, atau memverifikasi NFR. Acuannya PRD bagian 5, SRS bagian 5, dan constraint di BRD bagian 7.

## 1. Prinsip Dasar

1. **Terukur.** Setiap NFR punya angka atau kriteria yang bisa diuji. Kata seperti "cepat" atau "aman" tanpa angka tidak boleh masuk dokumen.
2. **Punya asal.** Setiap NFR bisa ditelusuri ke BO, BR, atau constraint. Kalau tidak ada, tandai `[ASUMSI]`.
3. **Punya cara uji.** Setiap NFR punya cara verifikasi, misalnya load test, code review, audit, atau uji perangkat.
4. **Sesuai biaya.** Target yang tinggi tanpa dasar bisnis perlu ditanyakan ke stakeholder, karena berpengaruh ke biaya.

## 2. Kategori yang Diperiksa

Tidak semua kategori dipakai di setiap proyek. Tetapi AI wajib memeriksa semuanya dan mencatat alasan untuk kategori yang tidak dipakai.

| Kategori | Pertanyaan Pemeriksaan | Status |
|---|---|---|
| Performa | Seberapa cepat respons yang diharapkan, dan berapa pengguna yang akses bersamaan? | Dipakai |
| Ketersediaan | Kapan sistem harus hidup, dan berapa uptime yang diterima? | Dipakai |
| Keamanan | Bagaimana login, pembatasan akses, dan perlindungan kata sandi? | Dipakai |
| Privasi | Data pribadi apa yang disimpan, di mana, dan berapa lama? | Dipakai |
| Kompatibilitas | Perangkat, OS, dan browser apa yang didukung? | Dipakai |
| Skalabilitas | Berapa pertumbuhan volume dalam 1 sampai 3 tahun? | Dipakai |
| Kegunaan (usability) | Apakah pengguna lansia atau non-teknis perlu perhatian khusus? | Dipakai |
| Pemeliharaan (maintainability) | Siapa yang merawat sistem, dan perlu logging atau monitoring? | Dipakai |
| Observabilitas | Apa yang dicatat: log, metrik, dan alert? | Dipakai |
| Backup dan pemulihan | Berapa lama data boleh hilang (RPO) dan sistem boleh mati (RTO)? | Belum jelas, tanyakan |
| Aksesibilitas | Perlu memenuhi standar aksesibilitas tertentu? | Belum jelas, tanyakan |
| Lokalisasi | Perlu lebih dari satu bahasa atau mata uang? | Tidak dipakai. Hanya Bahasa Indonesia dan Rupiah. |
| Regulasi | Ada aturan hukum atau standar industri yang wajib dipenuhi? | Dipakai (lihat NFR-005) |

Kategori yang "belum jelas" masuk ke daftar pertanyaan terbuka. Jangan dilewati diam-diam.

## 3. Format Penulisan

| Kolom | Wajib | Contoh |
|---|---|---|
| ID | Ya | NFR-001 |
| Kategori | Ya | Performa |
| Kebutuhan | Ya | Daftar pesanan tampil cepat |
| Target terukur | Ya | Di bawah 2 detik pada 4G untuk 500 pesanan aktif |
| Kondisi | Dianjurkan | Koneksi 4G, perangkat kelas menengah |
| Cara verifikasi | Ya | Load test |
| Acuan | Ya | BO-001 |
| Prioritas | Dianjurkan | Must, Should, atau Could |

Kondisi itu penting. "Di bawah 2 detik" tanpa kondisi tidak bisa diuji dengan adil, karena hasilnya beda di jaringan dan perangkat yang berbeda.

## 4. Contoh Target yang Benar dan Salah

| Salah | Benar |
|---|---|
| Sistem harus cepat | Halaman daftar pesanan tampil di bawah 2 detik pada 4G untuk 500 pesanan aktif |
| Sistem harus aman | Kata sandi disimpan sebagai hash. Tidak ada kata sandi dalam teks biasa. |
| Sistem harus selalu hidup | Uptime 99% per bulan, pukul 07.00 sampai 21.00 WIB |
| Sistem harus bisa berkembang | Kapasitas 300 pesanan per hari selama 2 tahun |
| Aplikasi harus ringan | Ukuran unduhan aplikasi Android di bawah 30 MB `[ASUMSI]` |
| Mudah dipakai | Pelanggan baru bisa membuat pesanan pertama tanpa bantuan admin dalam 5 menit `[ASUMSI: perlu uji pengguna]` |

## 5. Pemeriksaan Keamanan Minimum

- Kata sandi di-hash dengan algoritma yang diakui, misalnya bcrypt atau argon2. Jangan pakai MD5 atau SHA-1 polos.
- Peran (role) pengguna dicek di server. Menyembunyikan tombol di tampilan tidak cukup.
- Komunikasi memakai HTTPS.
- Data pribadi tidak boleh muncul di log.
- Ada batas percobaan login untuk mencegah tebak kata sandi. Angkanya harus tertulis, misalnya 5 percobaan per 15 menit `[ASUMSI]`.
- Akses ke data pelanggan dicatat.

## 6. Pemeriksaan Performa dan Skalabilitas

- Target performa menyebut jumlah data dan kondisi beban.
- Setiap target performa punya skenario load test yang tertulis.
- Asumsi pertumbuhan volume disertai dasarnya. Kalau tidak ada data, tandai `[ASUMSI]`.
- Proyeksi lebih dari 2 tahun ditulis sebagai perkiraan kasar, bukan requirement.

## 7. Pemeriksaan Ketersediaan dan Pemulihan

- Jam operasional ditulis lengkap dengan zona waktu.
- Uptime ditulis dalam persen per periode, misalnya per bulan.
- Uptime 99% juga perlu disertai jumlah downtime yang diizinkan, sekitar 7 jam per bulan.
- Rencana backup dan pemulihan wajib ada, dengan RPO dan RTO. Kalau belum disepakati, tulis sebagai pertanyaan terbuka.

## 8. Checklist Sebelum Menyerahkan NFR

1. Setiap NFR punya ID, kategori, target terukur, dan cara verifikasi.
2. Setiap target punya kondisi: jaringan, perangkat, jumlah data, atau jam.
3. Tidak ada kata samar tanpa angka: cepat, aman, mudah, andal, fleksibel, scalable.
4. Setiap NFR punya acuan ke BO, BR, atau constraint.
5. Setiap kategori sudah diperiksa. Yang tidak dipakai punya alasan.
6. Setiap angka yang tidak berasal dari pengguna ditandai `[ASUMSI]`.
7. Setiap NFR keamanan dan privasi punya langkah verifikasi.
8. Kategori yang belum jelas sudah masuk pertanyaan terbuka.

## 9. Contoh Pemeriksaan pada NFR BersihKilat

| ID | Hasil | Tindakan |
|---|---|---|
| NFR-001 | Target dan kondisi sudah ada (4G, 500 pesanan) | Lolos |
| NFR-002 | Uptime 99% sudah ada, tapi jumlah downtime per bulan belum ditulis | Tambahkan angka 7 jam per bulan |
| NFR-003 | Target jelas, verifikasi lewat code review | Lolos. Tambahkan cek database. |
| NFR-005 | Lokasi server belum dikonfirmasi | Tandai `[ASUMSI]` dan masukkan ke pertanyaan terbuka |
| NFR-007 | Angka 300 pesanan per hari belum punya data pendukung | Tandai `[ASUMSI]` dan minta data historis |
| (belum ada) | Backup dan pemulihan belum punya RPO dan RTO | Tanyakan ke sponsor |

## 10. Pertanyaan Standar untuk Stakeholder

1. Berapa lama sistem boleh mati dalam sebulan tanpa mengganggu operasional?
2. Berapa lama data boleh hilang jika server rusak?
3. Berapa jumlah pengguna dan pesanan di hari paling sibuk?
4. Di mana data pelanggan boleh disimpan, dan siapa yang menetapkan aturannya?
5. Perangkat dan browser apa yang dipakai pelanggan dan admin sekarang?
6. Apakah ada standar keamanan atau regulasi yang wajib diikuti?