# BA/SA Analyst Skill

Skill untuk AI agent agar bisa bekerja seperti Business Analyst (BA) dan System Analyst (SA). Skill ini membantu membuat dokumen analisis kebutuhan software: BRD, PRD, SRS, use case, user story, NFR, ERD, kamus data, dan test plan. Skill mendukung metode Agile, Waterfall, atau hybrid.

Skill ini dibuat supaya AI tidak "liar". AI akan bertanya dulu sebelum menulis, menandai asumsi dengan `[ASUMSI]`, berhenti saat ada konflik, dan tidak menambah fitur di luar dokumen.

## Isi Repo

```
ba-sa-analyst-skill/
├── README.md                  ← dokumen ini
├── CHANGELOG.md
├── LICENSE
└── ba-sa-analyst/             ← folder skill, yang di-copy ke proyek
    ├── SKILL.md               ← aturan utama dan alur kerja
    ├── references/
    │   ├── brd-template.md    ← contoh BRD terisi (proyek BersihKilat)
    │   ├── prd-template.md    ← contoh PRD terisi
    │   ├── srs-template.md    ← contoh SRS terisi
    │   ├── erd-guidelines.md  ← aturan ERD dan kamus data
    │   ├── nfr-checklist.md   ← checklist NFR
    │   └── gaya-bahasa.md     ← panduan gaya penulisan
    └── scripts/
        └── test_skill.sh      ← validasi struktur skill
```

Folder `ba-sa-analyst/` harus berdiri sendiri. Jangan menaruh README atau contoh hasil di dalamnya.

## Instalasi

### Antigravity (per proyek)

Salin folder skill ke dalam proyek:

```bash
mkdir -p .agent/skills
cp -r /path/ke/ba-sa-analyst-skill/ba-sa-analyst .agent/skills/
```

Struktur akhirnya harus seperti ini:

```
nama-proyek/
└── .agent/
    └── skills/
        └── ba-sa-analyst/
            ├── SKILL.md
            └── references/
```

Kalau Antigravity versimu belum mendukung skills, salin `SKILL.md` ke `.agent/rules/ba-sa-analyst.md`. Dalam mode ini, minta agent membuka file di `references/` secara manual.

### Claude Code

```bash
# Untuk satu user
cp -r ba-sa-analyst ~/.claude/skills/

# Untuk satu proyek
cp -r ba-sa-analyst ./.claude/skills/
```

### Claude.ai

Jalankan `zip -r ba-sa-analyst.zip ba-sa-analyst/`, lalu upload lewat pengaturan Skills.

### Platform lain

Salin isi `SKILL.md` ke system prompt atau custom instructions, lalu lampirkan folder `references/` sebagai dokumen pendukung.

## Cara Pakai

Skill ini aktif otomatis saat kamu meminta pekerjaan analisis. Kamu tidak perlu menyebut nama file satu per satu, tapi menyebut nama skill membuat hasilnya lebih pasti.

### Aturan penulisan perintah

1. **Sebut proyeknya.** Contoh: "aplikasi pemesanan laundry", bukan "aplikasi".
2. **Sebut metodenya.** Agile, Waterfall, atau hybrid. Kalau tidak disebut, AI akan menanyakannya.
3. **Sebut tujuannya.** Masalah apa yang ingin diselesaikan, kalau sudah tahu.
4. **Sebut data yang sudah ada.** Angka, harga, atau aturan yang sudah pasti. Ini mengurangi `[ASUMSI]`.
5. **Satu dokumen per permintaan.** Minta BRD dulu, lalu PRD, lalu SRS. Urutan ini mengikuti dependensi dokumen.

### Contoh perintah

**Mengaktifkan skill (dijalankan sekali di awal sesi):**
```
Saya menambahkan skill di folder project ini beserta referencenya.
Apa kamu bisa membaca dan menerapkan aturannya?
```

**Memulai proyek baru:**
```
Saya ingin membuat aplikasi pemesanan laundry. Metode Agile.
```

**Membuat BRD dengan data lengkap:**
```
Buatkan BRD untuk aplikasi pemesanan laundry, metode Waterfall.
Masalahnya: admin sering salah catat berat cucian dari WhatsApp, sekitar 30% pesanan.
Tenggat go-live 30 Juni 2027, anggaran maksimal Rp 180 juta.
```

**Lanjut ke PRD setelah BRD disetujui:**
```
BRD sudah disetujui. Buatkan PRD berdasarkan BO-001 sampai BO-003 dan BR-001 sampai BR-003.
```

**Memeriksa dokumen yang sudah ada:**
```
Periksa PRD ini dengan checklist di nfr-checklist.md. Tunjukkan NFR yang belum punya target terukur.
```

**Mengajukan perubahan:**
```
Tambahkan fitur pembayaran QRIS ke SRS. Buat Change Request-nya dulu, jangan langsung ubah dokumen.
```

## Hasil yang Diharapkan

Hasil untuk perintah "Saya ingin membuat aplikasi pemesanan laundry. Metode Agile." bukan dokumen langsung. Agent seharusnya:

1. Mengonfirmasi bahwa metode Agile sudah dicatat.
2. Mengajukan maksimal 3 pertanyaan, misalnya masalah bisnis yang ingin diselesaikan, pengguna utama, dan batasan anggaran atau waktu.
3. Belum menulis BRD sebelum pertanyaan itu terjawab, atau melanjutkan dengan asumsi yang diberi label `[ASUMSI]` jika kamu meminta.

Kalau agent langsung membuat dokumen penuh tanpa bertanya dan tanpa `[ASUMSI]`, berarti skill belum terbaca. Kembali ke bagian Instalasi.

## Menguji Skill

### Uji struktur (otomatis)

```bash
bash .agent/skills/ba-sa-analyst/scripts/test_skill.sh
```

Script ini memeriksa keberadaan file, frontmatter `name` yang cocok dengan nama folder, penanda `[ASUMSI]`, dan pola kata khas AI. Hasilnya berupa daftar `[LULUS]` dan `[GAGAL]`.

Catatan: pemeriksaan pola kata memakai grep, jadi bisa ada false positive. Cek baris yang disebut sebelum memperbaiki.

### Uji perilaku (manual)

Script tidak bisa menilai apakah AI mengikuti aturan. Jalankan lima uji ini di panel agent:

| Uji | Perintah | Hasil yang diharapkan |
|---|---|---|
| Bertanya dulu | "Buatkan BRD untuk aplikasi pesan makanan." | AI bertanya, belum menulis dokumen |
| Tidak mengarang | "Waterfall. Anggap datanya lengkap, langsung buat BRD." | Angka yang tidak diberikan ditandai `[ASUMSI]` |
| Konflik berhenti | "FR-002 bilang Rp 7.000, BR-001 bilang Rp 8.000. Lanjut buat SRS." | AI menandai konflik dan bertanya |
| Out of scope | "Tambahkan pembayaran QRIS ke SRS." | AI mengingatkan bahwa pembayaran online di luar scope |
| Gaya bahasa | Lihat hasil dokumen apa pun | Tidak ada pembuka basa-basi, istilah teknis tetap bahasa asli dengan keterangan singkat |

## Pemecahan Masalah

| Gejala | Penyebab yang mungkin | Solusi |
|---|---|---|
| AI tidak menyebut aturan skill | Skill belum aktif | Cek lokasi folder, lalu jalankan perintah aktivasi di awal sesi |
| AI menulis dokumen tanpa bertanya | Informasi di perintah sudah dianggap cukup | Tambahkan "tanya dulu sebelum menulis" di perintah |
| Istilah teknis diterjemahkan | Aturan gaya belum terbaca | Minta: "Ikuti aturan istilah teknis di SKILL.md" |
| Template tidak diikuti | File di `references/` belum dibaca | Minta: "Baca references/brd-template.md dulu, lalu ikuti strukturnya" |
| Script gagal di Windows | Format baris berbeda | Jalankan lewat Git Bash atau WSL |

Kalau aturan terbaca tapi tetap dilanggar, tambahkan pola pelanggarannya ke bagian "Batasan Perilaku" di `SKILL.md`, lalu uji ulang.

## Batasan

- Skill ini memandu AI, bukan pengganti keputusan bisnis. Keputusan tetap ada di tangan stakeholder.
- Contoh di `references/` memakai proyek fiktif BersihKilat. Ganti dengan data proyekmu.
- Hasil tetap perlu ditinjau manusia, terutama angka, aturan bisnis, dan persetujuan.
- Nama menu dan lokasi folder Antigravity bisa berubah antar versi. Cek dokumentasi resmi jika ada yang tidak cocok.

## Kontribusi

Silakan buka issue atau pull request. Untuk perubahan besar, jelaskan dulu kasusnya di issue. Setiap perubahan pada `SKILL.md` atau template dicatat di `CHANGELOG.md`.

## Lisensi

MIT. Lihat `LICENSE`.