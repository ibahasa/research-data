# Data turunan untuk temuan `apps/system-audit`

**Dibuat:** 2026-08-03 · **Sumber:** salinan basis data produksi 2026-08-02
**Untuk:** menjaga janji rubrik `/riset` — *"angka ini bisa kamu periksa sendiri"*

---

## Masalah yang berkas ini coba selesaikan

Katalog riset ini **berbeda dari ketiga katalog lain**, dan perbedaannya bukan
soal selera.

Temuan `apps/editor`, `apps/benchmark`, dan `scrape/` lahir dari berkas: korpus,
keluaran audit, riwayat run. Berkas itu bisa didorong apa adanya ke
`research-data`, dan pembaca bisa mengunduhnya lalu menghitung ulang.

Temuan di sini lahir dari **basis data produksi**, yang memuat seluruh isi kamus,
jejak perbuatan admin, dan riwayat panggilan model. Ia tidak bisa diterbitkan
sekarang maupun nanti — bukan karena besar, melainkan karena menerbitkannya
persis melanggar batas paling keras di [HANDOFF-09
§2.2](../../../discussions/handoff/HANDOFF-09-SYSTEM-AUDIT-ENGINEER.md).

Jadi ada ketegangan nyata: **data yang membuat temuan ini bisa diperiksa adalah
data yang haram diterbitkan.** Berkas di folder ini adalah jalan tengahnya, dan
jalan tengah itu punya batas yang harus dinyatakan, bukan disembunyikan.

## Yang bisa dan tidak bisa diperiksa pembaca luar

| Bisa | Tidak bisa |
|---|---|
| Menghitung ulang **aritmetika** tiap tabel di artikel | Menjalankan ulang **ekstraksinya** dari sumber |
| Memeriksa apakah persentase di artikel cocok dengan hitungannya | Memverifikasi bahwa angka mentahnya benar |
| Menurunkan angka lain dari agregat yang sama | Memeriksa satu baris data mana pun |

Itu **lebih lemah** daripada janji yang dipegang katalog lain, dan tidak boleh
disamarkan. Kalimat yang tepat untuk artikel: *"agregat yang jadi dasar tabel ini
diterbitkan; data mentahnya tidak dapat diterbitkan karena memuat isi kamus dan
jejak perbuatan internal."*

Preseden untuk kejujuran semacam ini sudah ada di repo: katalog `apps/editor`
menandai korpusnya sendiri **⚠ belum bisa direproduksi dari repo oleh pihak
luar**, dan tetap menerbitkan temuannya.

## Berkas

Semuanya agregat. **Tidak ada satu pun lema, definisi, potongan teks, nama tabel,
nama kolom, atau nama berkas kode di dalamnya** — sudah diperiksa dengan uji bocor
mekanis yang sama seperti berkas temuan.

| Berkas | Isi | Baris | Dipakai temuan |
|---|---|---:|---|
| `gerbang-per-desil-frekuensi.csv` | tingkat kelolosan gerbang tinjauan per desil frekuensi korpus | 10 | 01 |
| `cakupan-lapisan-pengayaan.csv` | cakupan tiap lapisan pengayaan terhadap 72.244 entri | 6 | 01 |
| `sebaran-suntingan-per-makna.csv` | berapa banyak makna yang disunting n kali | 20 | 02 |
| `besar-suntingan-per-bagian.csv` | jumlah, kemiripan rata-rata, dan pembagian rapikan/revisi/tulis-ulang per bagian teks | 4 | 02 |
| `ringkasan-pekerjaan-ai.csv` | jumlah pekerjaan AI menurut keadaan akhirnya | 4 | 03 |
| `taksonomi-kegagalan-job-ai.csv` | 50 kegagalan, dikelompokkan menurut sebabnya | 4 | 03 |
| `ATURAN-KLASIFIKASI-KEGAGALAN.md` | aturan penggolongan sebab kegagalan, urutan penerapannya, dan batasnya | — | 03 |

Label di dalamnya sengaja memakai **peran**, bukan nama teknis: "penanda kelas
kata", bukan nama kolomnya. Itu bukan penyamaran kosmetik — nama kolom adalah
cetak biru, perannya bukan.

### Catatan penting soal `sebaran-suntingan-per-makna.csv`

Angka di berkas ini sudah **dibersihkan dari dua artefak** yang membuat versi
pertama temuan 02 menggelembung sekitar sepertiga: medan yang *muncul dan
menghilang* dari rekaman karena bentuk rekamannya berubah antar-versi kode, bukan
karena disunting. Riwayat lengkapnya di temuan 02 §Koreksi.

Siapa pun yang memakai berkas ini untuk membandingkan dengan sistemnya sendiri
perlu tahu itu, karena rekaman berbentuk potret di sistem mana pun akan punya
masalah yang sama.

## Entri katalog

**Sudah ditambahkan** ke `CATALOG` pada `scripts/publish-research-data.sh` sebagai
`system-audit-aggregates`, atas permintaan Riset Clue Engineer (2026-08-05),
lengkap dengan catatan kenapa entri ini memegang janji yang lebih lemah daripada
entri lain. Lisensi CC BY 4.0 — data milik sendiri, tak ada sumber pihak ketiga
yang menyeret syarat.

Seluruh berkas di folder ini di bawah 50 KB, jauh di bawah kedua pagar ukuran.

Satu hal yang tetap perlu diputuskan bersama, bukan oleh saya: apakah agregat
turunan seperti ini perlu **ditandai berbeda di antarmuka** supaya pembaca tidak
menyangka ia bisa diperiksa sedalam dataset lain. Saya condong ke ditandai
berbeda — janji yang dilebihkan lebih merusak daripada janji yang dikecilkan.

## Cara menghasilkan ulang

Keempat berkas dihasilkan dari kueri yang tertulis lengkap di berkas task
internal (`../docs/tasks/task-03-*`, `task-09-*`) terhadap salinan produksi
bertanggal. Kuerinya tidak disalin ke sini karena memuat nama tabel dan kolom.

Salinan produksi berikutnya akan menggeser seluruh angka. Tiap berkas temuan
menyebut tanggal salinannya sendiri, dan berkas di folder ini mengikuti tanggal
yang sama: **2026-08-02**.

## Lisensi

Sama dengan katalog `research-data`: **CC BY 4.0**. Tidak ada data pihak ketiga
di dalamnya, jadi tidak ada kewajiban atribusi tambahan.
