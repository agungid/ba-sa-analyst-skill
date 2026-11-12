# Panduan Gaya Bahasa

## Contoh Perbaikan

| Versi AI (kaku) | Versi yang diharapkan |
|---|---|
| Berdasarkan hasil analisis, dapat disimpulkan bahwa proses pemesanan saat ini masih dilakukan secara manual. | Saat ini pemesanan masih lewat WhatsApp. Admin harus mencatat ulang setiap pesan ke buku, dan di jam sibuk catatan sering terlewat. |
| Sistem akan memberikan notifikasi kepada pengguna setiap kali terjadi perubahan status. | Setiap kali status cucian berubah, pelanggan menerima satu notifikasi. |
| Fitur ini bertujuan untuk meningkatkan efisiensi operasional secara menyeluruh. | Admin tidak perlu lagi mengetik ulang pesanan dari chat. Waktu yang dihemat sekitar 4 jam per hari. `[ASUMSI]` |
| Keamanan data pengguna akan dijaga dengan menerapkan standar terbaik. | Kata sandi disimpan sebagai hash, sehingga tim IT tidak bisa membaca kata sandi asli pelanggan. |

## Cara Menulis Penjelasan Teknis

Salah: "Sistem menggunakan JWT untuk autentikasi."

Benar: "Setelah login, server memberi token JWT (JSON Web Token, semacam tiket digital yang dibawa aplikasi di setiap permintaan). Dengan tiket ini, pelanggan tidak perlu login ulang setiap pindah halaman."

Pola yang dipakai:
1. Sebut istilah teknisnya apa adanya.
2. Beri penjelasan singkat dalam kurung atau kalimat berikutnya.
3. Jelaskan apa yang dirasakan pengguna atau bisnis dari hal itu.

## Contoh Istilah yang Tidak Diterjemahkan

| Istilah | Keterangan saat pertama muncul |
|---|---|
| endpoint | alamat tujuan yang dipanggil aplikasi untuk meminta atau mengirim data |
| cache | penyimpanan sementara supaya data tidak perlu diambil ulang dari sumber |
| deploy | memasang aplikasi versi baru ke server supaya bisa dipakai |
| migration | catatan perubahan struktur database, supaya perubahan bisa diulang di mana pun |
| rollback | mengembalikan sistem ke kondisi sebelumnya jika perubahan bermasalah |

## Contoh Paragraf Bagus

> Pelanggan membuka aplikasi dan memilih outlet terdekat. Mereka mengisi estimasi berat dalam kilogram, lalu sistem langsung menghitung perkiraan harga. Angka ini belum final, karena berat asli baru diketahui setelah admin menimbang di outlet. Karena itu, aplikasi menampilkan keterangan "perkiraan" di setiap harga.