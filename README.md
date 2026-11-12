# Skill Analisis dan Arsitektur Software untuk AI Agent

Kumpulan skill untuk AI agent (Antigravity, Claude Code, Claude.ai, atau platform lain yang mendukung instruksi berbasis file) agar bisa bekerja seperti Business Analyst (BA), System Analyst (SA), dan Solution Architect. Skill ini membantu membuat dokumen BRD, PRD, SRS, use case, NFR, ERD, tech spec, dan ADR.

Tujuannya satu: AI tidak bekerja liar. AI bertanya dulu sebelum menulis, menandai asumsi dengan `[ASUMSI]`, menandai versi yang belum dicek dengan `[VERIFIKASI]`, berhenti saat ada konflik antar dokumen, dan tidak menambah fitur di luar spesifikasi.

## Isi Repo

```
ba-sa-analyst-skill/
├── README.md
├── CHANGELOG.md
├── LICENSE
├── ba-sa-analyst/              ← Skill 1: analisis kebutuhan
│   ├── SKILL.md
│   ├── references/
│   │   ├── brd-template.md
│   │   ├── prd-template.md
│   │   ├── srs-template.md
│   │   ├── erd-guidelines.md
│   │   ├── nfr-checklist.md
│   │   └── gaya-bahasa.md
│   └── scripts/
│       └── test_skill.sh
└── solution-architect/         ← Skill 2: desain teknis
    ├── SKILL.md
    └── references/
        ├── tech-spec-template.md
        ├── adr-template.md
        └── review-checklist.md
```

Urutan kerja antar skill:

```
ba-sa-analyst  →  solution-architect  →  (developer, qa-tester: belum dibuat)
```

Setiap folder skill harus berdiri sendiri. Jangan menaruh README atau contoh hasil di dalam folder skill.

## Instalasi

### Antigravity (per proyek)

Salin kedua folder ke dalam proyek:

```bash
mkdir -p .agent/skills
cp -r /path/ke/ba-sa-analyst-skill/ba-sa-analyst .agent/skills/
cp -r /path/ke/ba-sa-analyst-skill/solution-architect .agent/skills/
```

Struktur akhir di proyek:

```
nama-proyek/
└── .agent/
    └── skills/
        ├── ba-sa-analyst/
        └── solution-architect/
```

Kalau versi Antigravity kamu belum mendukung skills, salin `SKILL.md` ke `.agent/rules/` dengan nama yang sesuai, lalu minta agent membuka file di `references/` saat dibutuhkan. Nama menu dan lokasi folder bisa berbeda antar versi, jadi cek dokumentasi resmi jika ada yang tidak cocok.

### Claude Code

```bash
# Untuk satu user
cp -r ba-sa-analyst solution-architect ~/.claude/skills/

# Untuk satu proyek
cp -r ba-sa-analyst solution-architect ./.claude/skills/
```

### Claude.ai

Buat ZIP per skill, lalu upload lewat pengaturan Skills:

```bash
zip -r ba-sa-analyst.zip ba-sa-analyst/
zip -r solution-architect.zip solution-architect/
```

### Platform lain

Salin isi `SKILL.md` ke system prompt atau custom instructions, lalu lampirkan folder `references/` sebagai dokumen pendukung.

## Cara Pakai

### Aturan umum penulisan perintah

1. **Aktifkan skill di awal sesi.** Satu perintah cukup, lalu lanjutkan pekerjaan.
2. **Sebut proyeknya.** "Aplikasi pemesanan laundry", bukan "aplikasi".
3. **Sebut metodenya.** Agile, Waterfall, atau hybrid. Kalau tidak disebut, AI akan bertanya.
4. **Berikan data yang sudah pasti.** Angka, harga, dan tenggat mengurangi jumlah `[ASUMSI]`.
5. **Ikuti urutan dokumen.** BRD, lalu PRD, lalu SRS dan ERD, lalu tech spec dan ADR. Dokumen hilir tidak dibuat tanpa hulunya.
6. **Simpan dokumen di folder `docs/`** supaya AI bisa membacanya di sesi berikutnya.

### Contoh alur lengkap

**Langkah 1: Aktifkan skill analisis**

```
Saya menambahkan skill di folder project ini beserta referencenya.
Apa kamu bisa membaca dan menerapkan aturannya?
```

**Langkah 2: Mulai proyek**

```
Saya ingin membuat aplikasi pemesanan laundry. Metode Agile.
```

Agent seharusnya bertanya, misalnya masalah bisnis, pengguna utama, dan batasan. Belum menulis dokumen.

**Langkah 3: Buat BRD dengan data lengkap**

```
Buatkan BRD untuk aplikasi pemesanan laundry, metode Agile.
Masalahnya: admin sering salah catat berat cucian dari WhatsApp, sekitar 30% pesanan.
Tenggat go-live 30 Juni 2027, anggaran maksimal Rp 180 juta.
Simpan di docs/brd.md.
```

**Langkah 4: Buat PRD dan SRS setelah BRD disetujui**

```
BRD sudah disetujui. Buatkan PRD berdasarkan BO-001 sampai BO-003 dan BR-001 sampai BR-003.
Simpan di docs/prd.md.
```

```
Buatkan SRS dari PRD di docs/prd.md. Sertakan use case untuk setiap FR.
Simpan di docs/srs.md.
```

**Langkah 5: Buat ERD dan periksa NFR**

```
Buatkan ERD dan kamus data dari SRS di docs/srs.md, ikuti erd-guidelines.md.
```

```
Periksa NFR di docs/prd.md dengan nfr-checklist.md. Tunjukkan NFR yang belum punya target terukur.
```

**Langkah 6: Pindah ke solution architect**

```
Pakai skill solution-architect. Dokumen BRD, PRD, SRS, dan ERD ada di folder docs/.
Baca dulu semuanya, lalu tanyakan hal yang belum jelas sebelum menulis tech spec.
```

**Langkah 7: Buat ADR dan tech spec setelah pertanyaan dijawab**

```
Tulis ADR untuk pemilihan database dan backend. Status Proposed.
```

```
Tulis tech spec berdasarkan ADR yang sudah Accepted. Tandai versi library yang belum dicek dengan [VERIFIKASI].
```

### Contoh perintah untuk perubahan

```
Tambahkan pembayaran QRIS ke SRS. Buat Change Request-nya dulu, jangan langsung ubah dokumen.
```

```
Ubah keputusan database di ADR-002. Buat ADR baru yang menggantikan ADR-002, jangan hapus yang lama.
```

## Hasil yang Diharapkan

Agent yang sudah membaca skill biasanya:

1. Menyebut aturan dari skill saat ditanya, atau langsung berperilaku sesuai aturan.
2. Mengajukan maksimal tiga pertanyaan sebelum membuat dokumen besar.
3. Menandai angka yang tidak diberikan pengguna dengan `[ASUMSI]`.
4. Menandai versi library yang belum diverifikasi dengan `[VERIFIKASI]`.
5. Menghindari pembuka basa-basi dan kata penguat kosong.

Kalau agent langsung membuat dokumen penuh tanpa bertanya dan tanpa penanda, skill belum terbaca. Periksa lokasi folder, lalu ulangi perintah aktivasi.

## Menguji Skill

### Uji struktur (otomatis)

```bash
bash .agent/skills/ba-sa-analyst/scripts/test_skill.sh
```

Script ini memeriksa keberadaan file, kecocokan `name` di frontmatter dengan nama folder, penanda `[ASUMSI]`, dan pola kata khas AI. Hasilnya berupa daftar `[LULUS]` dan `[GAGAL]`.

Catatan: pemeriksaan pola kata memakai grep, jadi bisa ada false positive. Cek baris yang disebut sebelum memperbaiki.

### Uji perilaku (manual)

| Uji | Perintah | Hasil yang diharapkan |
|---|---|---|
| Bertanya dulu | "Buatkan BRD untuk aplikasi pesan makanan." | AI bertanya, belum menulis dokumen |
| Tidak mengarang | "Waterfall. Anggap datanya lengkap, langsung buat BRD." | Angka yang tidak diberikan ditandai `[ASUMSI]` |
| Konflik berhenti | "FR-002 bilang Rp 7.000, BR-001 bilang Rp 8.000. Lanjut buat SRS." | AI menandai konflik dan bertanya |
| Out of scope | "Tambahkan pembayaran QRIS ke SRS." | AI mengingatkan bahwa pembayaran online di luar scope |
| Versi library | "Buat tech spec dengan stack Node.js dan PostgreSQL." | Versi ditandai `[VERIFIKASI]` atau dicek lewat pencarian |
| Gaya bahasa | Lihat hasil dokumen apa pun | Tidak ada pembuka basa-basi, istilah teknis tetap bahasa asli dengan keterangan singkat |

## Pemecahan Masalah

| Gejala | Penyebab yang mungkin | Solusi |
|---|---|---|
| AI tidak menyebut aturan skill | Skill belum aktif | Cek lokasi folder, lalu jalankan perintah aktivasi di awal sesi |
| AI menulis dokumen tanpa bertanya | Perintah dianggap sudah lengkap | Tambahkan "tanya dulu sebelum menulis" |
| Istilah teknis diterjemahkan | Aturan gaya belum terbaca | Minta: "Ikuti aturan istilah teknis di gaya-bahasa.md" |
| Template tidak diikuti | File di `references/` belum dibaca | Minta: "Baca references/brd-template.md dulu, lalu ikuti strukturnya" |
| Tech spec menulis versi tanpa `[VERIFIKASI]` | Verifikasi versi belum dijalankan | Minta: "Cek versi terbaru dari dokumentasi resmi lewat pencarian" |
| Script gagal di Windows | Format baris berbeda | Jalankan lewat Git Bash atau WSL |

Kalau aturan terbaca tapi tetap dilanggar, tambahkan pola pelanggarannya ke bagian "Batasan Perilaku" di `SKILL.md` skill yang bersangkutan, lalu uji ulang.

## Batasan

- Skill memandu AI, bukan pengganti keputusan bisnis atau teknis. Keputusan tetap ada di tangan stakeholder dan tim.
- Contoh di `references/` memakai proyek fiktif BersihKilat. Ganti dengan data proyekmu.
- Hasil tetap perlu ditinjau manusia, terutama angka, aturan bisnis, versi library, dan persetujuan.
- Skill developer dan qa-tester belum tersedia.

## Roadmap

- [ ] `developer`: menulis kode sesuai tech spec dan ADR
- [ ] `qa-tester`: membuat test otomatis dari kriteria uji di SRS
- [ ] `code-reviewer`: meninjau diff terhadap tech spec
- [ ] Contoh hasil lengkap di folder `examples/`

## Kontribusi

Buka issue atau pull request. Untuk perubahan besar, jelaskan dulu kasusnya di issue. Setiap perubahan pada `SKILL.md` atau template dicatat di `CHANGELOG.md`.

## Lisensi

MIT. Lihat `LICENSE`.
