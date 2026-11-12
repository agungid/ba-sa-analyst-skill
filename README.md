# Skill untuk AI Agent: Analisis, Desain, dan Perencanaan Software

Kumpulan skill untuk AI agent (Antigravity, Claude Code, Claude.ai, atau platform lain yang mendukung instruksi berbasis file). Repo ini berisi tiga skill yang saling berurutan, dari kebutuhan bisnis sampai rencana kerja tim:

- **`ba-sa-analyst`** membantu membuat dokumen analisis kebutuhan: BRD, PRD, SRS, use case, NFR, dan ERD.
- **`solution-architect`** menerjemahkan dokumen analisis menjadi keputusan teknis: ADR, tech spec, diagram C4, kontrak API, dan pola arsitektur.
- **`project-planner`** memecah PRD, SRS, dan tech spec menjadi backlog, sprint plan (Agile), atau WBS dan jadwal (Waterfall).

Tujuannya satu: AI tidak bekerja liar. AI bertanya dulu sebelum menulis, menandai asumsi dengan `[ASUMSI]` dan versi yang belum dicek dengan `[VERIFIKASI]`, berhenti saat ada konflik antar dokumen, tidak menambah fitur di luar spesifikasi, dan tidak menulis tanggal pasti tanpa dasar kapasitas.

## Isi Repo

```
ba-sa-analyst-skill/
├── README.md
├── CHANGELOG.md
├── LICENSE
├── ba-sa-analyst/                ← Skill 1: analisis kebutuhan
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
├── solution-architect/           ← Skill 2: desain teknis
│   ├── SKILL.md
│   └── references/
│       ├── tech-spec-template.md
│       ├── adr-template.md
│       └── review-checklist.md
└── project-planner/              ← Skill 3: perencanaan kerja
    ├── SKILL.md
    └── references/
        ├── backlog-sprint-template.md
        ├── wbs-jadwal-template.md
        └── review-checklist.md
```

Urutan kerja antar skill:

```
ba-sa-analyst → solution-architect → project-planner → (developer, qa-tester: belum dibuat)
```

Setiap folder skill harus berdiri sendiri. Jangan menaruh README atau contoh hasil di dalam folder skill. Setiap skill tetap berfungsi kalau dipasang sendiri, karena ringkasan gaya bahasa sudah disalin ke masing-masing `SKILL.md`. Skill yang dipasang bersama akan membaca `gaya-bahasa.md` yang lebih lengkap.

## Instalasi

### Antigravity (per proyek)

Salin folder skill yang dibutuhkan ke dalam proyek. Untuk alur lengkap, pasang ketiganya:

```bash
mkdir -p .agent/skills
cp -r /path/ke/ba-sa-analyst-skill/ba-sa-analyst .agent/skills/
cp -r /path/ke/ba-sa-analyst-skill/solution-architect .agent/skills/
cp -r /path/ke/ba-sa-analyst-skill/project-planner .agent/skills/
```

Struktur akhir di proyek:

```
nama-proyek/
└── .agent/
    └── skills/
        ├── ba-sa-analyst/
        ├── solution-architect/
        └── project-planner/
```

Kalau versi Antigravity kamu belum mendukung skills, salin `SKILL.md` ke `.agent/rules/` dengan nama yang sesuai, lalu minta agent membuka file di `references/` saat dibutuhkan. Nama menu dan lokasi folder bisa berbeda antar versi, jadi cek dokumentasi resmi jika ada yang tidak cocok.

### Claude Code

```bash
# Untuk satu user
cp -r ba-sa-analyst solution-architect project-planner ~/.claude/skills/

# Untuk satu proyek
cp -r ba-sa-analyst solution-architect project-planner ./.claude/skills/
```

### Claude.ai

Buat ZIP per skill, lalu upload lewat pengaturan Skills:

```bash
zip -r ba-sa-analyst.zip ba-sa-analyst/
zip -r solution-architect.zip solution-architect/
zip -r project-planner.zip project-planner/
```

### Platform lain

Salin isi `SKILL.md` ke system prompt atau custom instructions, lalu lampirkan folder `references/` sebagai dokumen pendukung.

## Cara Pakai

### Aturan umum penulisan perintah

1. **Aktifkan skill di awal sesi.** Satu perintah cukup, lalu lanjutkan pekerjaan.
2. **Sebut proyeknya.** "Aplikasi pemesanan laundry", bukan "aplikasi".
3. **Sebut metodenya.** Agile, Waterfall, atau hybrid. Kalau tidak disebut, AI akan bertanya.
4. **Berikan data yang sudah pasti.** Angka, harga, tenggat, dan kapasitas tim mengurangi `[ASUMSI]`.
5. **Ikuti urutan dokumen.** BRD, lalu PRD, SRS, dan ERD, lalu tech spec dan ADR, lalu backlog dan sprint plan. Dokumen hilir tidak dibuat tanpa hulunya.
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

**Langkah 8: Susun backlog dan sprint plan**

```
Pakai skill project-planner. PRD, SRS, dan tech spec ada di folder docs/.
Baca dulu semuanya, lalu tanyakan kapasitas tim dan metode kerja sebelum menyusun sprint.
```

```
Kapasitas tim: 1 backend, 1 mobile/web, 1 QA. Velocity belum diketahui.
Tanggal go-live 30 Juni 2027 bersifat tetap. Susun sprint plan di docs/sprint-plan.md.
```

### Contoh perintah untuk perubahan

```
Tambahkan pembayaran QRIS ke SRS. Buat Change Request-nya dulu, jangan langsung ubah dokumen.
```

```
Ubah keputusan database di ADR-002. Buat ADR baru yang menggantikan ADR-002, jangan hapus yang lama.
```

```
Velocity sprint 1 ternyata 14 poin. Hitung ulang jadwal sprint 2 sampai 7, dan tulis sebagai revisi rencana.
```

## Hasil yang Diharapkan

Agent yang sudah membaca skill biasanya:

1. Menyebut aturan dari skill saat ditanya, atau langsung berperilaku sesuai aturan.
2. Mengajukan maksimal tiga pertanyaan sebelum membuat dokumen besar.
3. Menandai angka yang tidak diberikan pengguna dengan `[ASUMSI]`.
4. Menandai versi library yang belum diverifikasi dengan `[VERIFIKASI]`.
5. Menulis rentang tanggal, bukan tanggal pasti, kalau kapasitas belum diketahui.
6. Tidak memasukkan story yang diblokir ke dalam sprint.
7. Menghindari pembuka basa-basi dan kata penguat kosong.

Kalau agent langsung membuat dokumen penuh tanpa bertanya dan tanpa penanda, skill belum terbaca. Periksa lokasi folder, lalu ulangi perintah aktivasi.

## Menguji Skill

### Uji struktur (otomatis)

```bash
bash .agent/skills/ba-sa-analyst/scripts/test_skill.sh
```

Script ini memeriksa keberadaan file, kecocokan `name` di frontmatter dengan nama folder, penanda `[ASUMSI]`, dan pola kata khas AI. Hasilnya berupa daftar `[LULUS]` dan `[GAGAL]`.

Catatan: pemeriksaan pola kata memakai grep, jadi bisa ada false positive. Cek baris yang disebut sebelum memperbaiki. Script ini belum memeriksa `solution-architect` dan `project-planner`. Menambahkannya termasuk dalam roadmap.

### Uji perilaku (manual)

| Uji | Perintah | Hasil yang diharapkan |
|---|---|---|
| Bertanya dulu | "Buatkan BRD untuk aplikasi pesan makanan." | AI bertanya, belum menulis dokumen |
| Tidak mengarang | "Waterfall. Anggap datanya lengkap, langsung buat BRD." | Angka yang tidak diberikan ditandai `[ASUMSI]` |
| Konflik berhenti | "FR-002 bilang Rp 7.000, BR-001 bilang Rp 8.000. Lanjut buat SRS." | AI menandai konflik dan bertanya |
| Out of scope | "Tambahkan pembayaran QRIS ke SRS." | AI mengingatkan bahwa pembayaran online di luar scope |
| Versi library | "Buat tech spec dengan stack Node.js dan PostgreSQL." | Versi ditandai `[VERIFIKASI]` atau dicek lewat pencarian |
| Story diblokir | "Masukkan semua story ke sprint 1, termasuk yang belum ada FR-nya." | AI menahan story yang diblokir dan menandainya `Ditahan` |
| Jadwal tanpa kapasitas | "Kasih tanggal pasti go-live sprint planning-nya." | AI meminta kapasitas dulu, atau memberi rentang dengan `[ASUMSI]` |
| Gaya bahasa | Lihat hasil dokumen apa pun | Tidak ada pembuka basa-basi, istilah teknis tetap bahasa asli dengan keterangan singkat |

## Pemecahan Masalah

| Gejala | Penyebab yang mungkin | Solusi |
|---|---|---|
| AI tidak menyebut aturan skill | Skill belum aktif | Cek lokasi folder, lalu jalankan perintah aktivasi di awal sesi |
| AI menulis dokumen tanpa bertanya | Perintah dianggap sudah lengkap | Tambahkan "tanya dulu sebelum menulis" |
| Istilah teknis diterjemahkan | Aturan gaya belum terbaca | Minta: "Ikuti aturan istilah teknis di gaya-bahasa.md" |
| Template tidak diikuti | File di `references/` belum dibaca | Minta: "Baca references/brd-template.md dulu, lalu ikuti strukturnya" |
| Tech spec menulis versi tanpa `[VERIFIKASI]` | Verifikasi versi belum dijalankan | Minta: "Cek versi terbaru dari dokumentasi resmi lewat pencarian" |
| Sprint plan memberi tanggal pasti | Kapasitas belum diberikan | Minta kapasitas tim, atau minta rentang tanggal dengan `[ASUMSI]` |
| Script gagal di Windows | Format baris berbeda | Jalankan lewat Git Bash atau WSL |

Kalau aturan terbaca tapi tetap dilanggar, tambahkan pola pelanggarannya ke bagian "Batasan Perilaku" di `SKILL.md` skill yang bersangkutan, lalu uji ulang.

## Batasan

- Skill memandu AI, bukan pengganti keputusan bisnis, teknis, atau manajemen proyek. Keputusan tetap ada di tangan stakeholder dan tim.
- Contoh di `references/` memakai proyek fiktif BersihKilat. Ganti dengan data proyekmu.
- Hasil tetap perlu ditinjau manusia, terutama angka, aturan bisnis, versi library, estimasi, dan jadwal.
- Skill developer dan qa-tester belum tersedia.

## Roadmap

- [ ] `developer`: menulis kode sesuai tech spec dan ADR
- [ ] `qa-tester`: membuat test otomatis dari kriteria uji di SRS
- [ ] `code-reviewer`: meninjau diff terhadap tech spec
- [ ] Perluas `scripts/test_skill.sh` untuk memeriksa `solution-architect` dan `project-planner`
- [ ] Contoh hasil lengkap di folder `examples/`

## Kontribusi

Buka issue atau pull request. Untuk perubahan besar, jelaskan dulu kasusnya di issue. Setiap perubahan pada `SKILL.md` atau template dicatat di `CHANGELOG.md`.

## Lisensi

MIT. Lihat `LICENSE`.