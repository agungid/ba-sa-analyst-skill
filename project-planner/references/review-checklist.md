# Checklist Review Perencanaan

Gunakan checklist ini sebelum backlog, sprint plan, atau jadwal WBS diserahkan. Setiap poin dijawab ya atau tidak. Kalau tidak, tulis tindakan yang diperlukan.

## 1. Asal dan Keterlacakan

1. Setiap story atau pekerjaan punya acuan ke FR, UC, atau TASK.
2. Setiap FR Must di PRD sudah masuk ke story atau WBS.
3. Setiap story punya acceptance criteria yang berasal dari SRS.
4. Tidak ada task yang tidak punya acuan.
5. Item tanpa FR ditandai `Ditahan`, bukan dijadwalkan.

## 2. Dependensi

1. Setiap dependensi teknis, data, dan keputusan dicatat secara terpisah.
2. Tidak ada story diblokir yang masuk sprint atau jadwal aktif.
3. Dependensi keputusan punya tenggat dan penanggung jawab.
4. Urutan pekerjaan mengikuti dependensi, bukan urutan prioritas bisnis saja.

## 3. Estimasi dan Kapasitas

1. Setiap estimasi punya satuan yang jelas (poin atau hari orang).
2. Story di atas 8 poin sudah dipecah.
3. Kapasitas tim dihitung dari hari kerja efektif, bukan dari jumlah orang.
4. Velocity awal ditandai `[ASUMSI]` dan ada rencana untuk memperbaruinya.
5. Libur dan cuti sudah dikurangi dari kapasitas.

## 4. Sprint dan Jadwal

1. Setiap sprint punya sprint goal yang bisa diuji.
2. Jumlah poin tidak melebihi kapasitas.
3. Sprint sebelum rilis dicadangkan untuk UAT dan perbaikan.
4. Jalur kritis (Waterfall) sudah dihitung dan ditulis.
5. Setiap milestone punya kriteria selesai yang jelas.
6. Buffer ditulis terpisah dan tidak disembunyikan di estimasi.
7. Tanggal rilis punya dasar hitungan dan rentang kalau belum pasti.

## 5. Risiko dan Perubahan

1. Risiko jadwal dicatat dengan dampak dan mitigasi.
2. Keputusan yang berisiko terhadap jalur kritis berada di urutan atas daftar risiko.
3. Perubahan scope dibuat sebagai story baru (Agile) atau Change Request (Waterfall).
4. Sprint atau baseline yang sedang berjalan tidak berubah tanpa catatan revisi.

## 6. Asumsi dan Pertanyaan

1. Semua `[ASUMSI]` masuk daftar asumsi dengan pemilik untuk validasi.
2. Setiap pertanyaan terbuka jelas dan menyebut siapa yang bisa menjawab.
3. Tidak ada pola khas AI yang tersisa, seperti pembuka basa-basi, penutup berlebihan, atau kata penguat kosong.
4. Istilah teknis dan metode tetap dalam bahasa aslinya, dengan keterangan singkat saat pertama muncul.

## Contoh Temuan Review

| Temuan | Tindakan |
|---|---|
| US-011 dijadwalkan di sprint 5, padahal BR-003 belum disetujui | Ubah status ke `Blocked (keputusan)` dan keluarkan dari sprint |
| Sprint 6 berisi story baru | Pindahkan story ke backlog berikutnya. Sprint 6 hanya untuk buffer dan perbaikan. |
| Tanggal go-live dihitung tanpa libur tim | Tambahkan pengurangan kapasitas dan tulis sebagai `[ASUMSI]` |
| Jalur kritis tidak disebut | Hitung dari WBS dan tulis di bagian 3 |
| Velocity 20 ditulis tanpa penanda | Tambahkan `[ASUMSI]` dan jadwalkan perhitungan ulang setelah sprint 2 |