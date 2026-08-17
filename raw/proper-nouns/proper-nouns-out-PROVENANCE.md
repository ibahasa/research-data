# Provenance — Gazetteer Nama Diri Indonesia

| | |
|---|---|
| Sumber utama | Wikidata SPARQL (`query.wikidata.org`) |
| **Lisensi** | **CC0** — data Wikidata bebas hak cipta; tidak menyeret kewajiban share-alike ke produk |
| Metode (wilayah/orang) | traversal hierarkis `Q252 --wdt:P150--> provinsi -> kabupaten/kota -> kecamatan`; nama orang lewat `P27=Q252` + `P735`/`P734`, di-GROUP BY nama |
| Metode (perusahaan, baru 2026-08-13) | `P17=Q252` (negara=Indonesia) + `P31` langsung ke satu dari 8 QID tipe usaha — lihat §Perusahaan di bawah |
| Diambil pada | 2026-07-29T14:20:36Z (wilayah/orang) · 2026-08-13T14:40:00Z (perusahaan) |
| Jumlah permintaan | 5 query SPARQL (2026-07-29) + 8 query SPARQL (2026-08-13), jeda 2 detik — bukan crawling |
| User-Agent | `ibahasa-research/1.0 (halo@ibahasa.com)` |

## Hasil mentah

| Berkas | Isi |
|---|---|
| `vendor/provinsi.json` | 42 |
| `vendor/kabupaten.json` | 601 kabupaten/kota (baris mentah; 500 nama unik setelah dedup 2026-08-13, lihat §Dedup) |
| `vendor/kecamatan.json` | 9.619 kecamatan (baris mentah; 6.918 nama unik setelah dedup — banyak nama kecamatan berulang lintas kabupaten, mis. "Tebing Tinggi" muncul di 6 kabupaten berbeda) |
| `vendor/nama_depan.json` | 3.079 nama depan + jumlah pemakai |
| `vendor/nama_keluarga.json` | 2.077 nama keluarga + jumlah pemakai |
| `vendor/perusahaan_*.json` (8 berkas, baru) | 2.076 baris mentah lintas 8 tipe → 1.686 nama unik setelah dedup |
| `vendor/kbbi_pos.psv` | 71.829 lema KBBI + POS (DB lokal, read-only) — untuk deteksi homograf |

**Catatan metode (wilayah, 2026-07-29):** query agregat global (`?item wdt:P17 wd:Q252` + GROUP BY)
**timeout** di endpoint publik. Traversal hierarkis per tingkat berjalan lancar dan jauh lebih
ringan bagi server.

## Dedup entri (baru, 2026-08-13)

`build_gazetteer.py` sekarang men-dedup `entries` per `(nama, kelas)` sebelum menulis
`gazetteer.jsonl` — sebelumnya tidak ada dedup di tingkat ini. Ini **retroaktif memperbaiki**
`gazetteer.jsonl`, bukan cuma menambah data baru: nama kecamatan yang berulang lintas kabupaten
(mis. "Tebing Tinggi", "Galang", "Parigi" — masing-masing muncul di 6 kabupaten berbeda) tadinya
tercatat sebagai baris terpisah per kemunculan geografis, sekarang satu baris per nama. Token-level
(`token_aman.txt`/`token_homograf.tsv`) **tidak terpengaruh** — itu sudah pakai `set()` sejak awal.
Efeknya: **entri gazetteer turun dari 15.315 jadi 14.160** meski 1.686 entri perusahaan baru
ditambahkan (dedup wilayah lama membuang lebih banyak daripada yang ditambahkan perusahaan baru).
Perlu diketahui siapa pun yang membandingkan angka before/after publikasi.

## Perusahaan/merek Indonesia (dimensi baru, 2026-08-13)

**Kenapa ini dimensi terpisah, bukan cuma tambahan token:** diminta eksplisit oleh tim HF
(`HANDOFF-HF-2026-08-13.md`) sebagai celah yang sudah ditandai kosong sejak `HANDOFF-EDITOR.md`
§5 ("belum ada: nama merek/perusahaan Indonesia"). Kelas barunya: `organisasi:perusahaan`.

**Metode — kenapa 8 query terpisah, bukan satu:**
- `?item wdt:P17 wd:Q252 . ?item wdt:P31/wdt:P279* wd:Q4830453` (semua yang secara transitif
  termasuk "badan usaha") **timeout**, bahkan setelah difilter negara — sama seperti pelajaran
  wilayah 2026-07-29.
- `VALUES ?type { 7 QID }` (gabungan beberapa tipe eksplisit tanpa wildcard subclass) **juga
  timeout**.
- Tipe TUNGGAL langsung (`P31=QID`, tanpa wildcard, tanpa VALUES gabungan) selesai 3–30 detik.
  Jadi: **8 query terpisah**, satu per tipe, sama semangatnya dengan traversal hierarkis yang
  sudah dipakai untuk wilayah.

| Tipe (QID) | Hasil mentah |
|---|---:|
| business (Q4830453) | 718 |
| public company (Q891723) | 1.068 |
| company (Q783794) | 180 |
| enterprise (Q6881511) | 66 |
| brand (Q431289) | 26 |
| corporation (Q167037) | 7 |
| software company (Q1058914) | 9 |
| startup company (Q129238) | 2 |

**Pembersihan nama** (`bersih_nama()`, `build_gazetteer.py`): awalan `PT`/`PT.`/`CV`/`CV.`
(370 entri) dan akhiran `Tbk`/`Tbk.`/`(Persero)`/`(Perseroda)` (440 entri) dibuang — bagian
badan hukum, bukan bagian nama. Diterapkan berulang karena bisa menumpuk (mis. "PT X (Persero)
Tbk" → "X").

**Hasil:** `out/organisasi_perusahaan.tsv` — 1.686 nama, urut abjad (tak ada bobot frekuensi
seperti nama orang; Wikidata tak mencatat "jumlah pemakai" untuk badan usaha). **11 di antaranya
homograf KBBI**: `Bibit`, `Gajah Tunggal`, `Mandiri`, `Matahari`, `Mekar`, `Multipolar`,
`Pegadaian`, `Prima`, `Tanah Laut`, `Temas`, `Timah` — semuanya kata Indonesia biasa yang
kebetulan juga nama perusahaan (`Pegadaian` secara harfiah berarti "pegadaian"/pawnshop).

**Belum disentuh:** `build_v1_seed.py`/`gen_migration.py` (scope produksi api-go, disepakati
terpisah dengan tim editor di `HANDOFF-SCRAPER.md` §2B) — kalau dijalankan ulang APA ADANYA
sekarang, ia akan otomatis menarik homograf perusahaan juga masuk scope v1 (cuma
`wilayah:kecamatan` yang dikecualikan di skrip itu). Itu keputusan tim editor, bukan saya yang
memutuskan sepihak di sini.

## Wilayah administratif — verifikasi pemekaran (2026-08-13, tak ada perubahan)

Diminta tim HF untuk cek apakah 4 provinsi baru Papua (pemekaran 2022: Papua Selatan, Papua
Tengah, Papua Pegunungan, Papua Barat Daya) sudah tercakup. **Sudah — sejak fetch pertama
2026-07-29**, bukan baru ditambahkan sekarang. Diverifikasi ulang 2026-08-13: query provinsi
masih mengembalikan 42 hasil, termasuk kelima provinsi Papua (Papua, Papua Barat, Papua Barat
Daya, Papua Pegunungan, Papua Selatan, Papua Tengah). **Tidak perlu suplemen Kemendagri** —
Wikidata sudah mutakhir untuk lapisan provinsi.

## Nama bayi terpopuler (Dukcapil/Kemendagri) — DILEWATI, dengan alasan

Dicari 2026-08-13. **Tidak ada rilis publik dalam bentuk berkas/API** yang ditemukan di portal
resmi (`data.go.id`, `dukcapil.kemendagri.go.id`) — yang ada cuma top-3 nama per tahun per
gender, dilaporkan lewat konferensi pers dan disebar ulang media (CNBC Indonesia, Kompas,
IDN Times, dst.), bukan dataset unduh. Gagal syarat "validitas di atas volume" yang diminta
handoff: sumber sekunder (media, bukan Kemendagri langsung), dan volumenya sendiri kecil
(3 nama × beberapa tahun). Dilewati eksplisit, bukan lupa — kalau kelak Kemendagri merilis
dataset resmi berbentuk berkas, ini layak dicoba lagi.

## Daftar calon legislatif (KPU/DCT) — TIDAK DIJALANKAN

Sesuai instruksi eksplisit tim HF: ditahan, butuh lampu hijau terpisah karena menyangkut UU PDP
(data identitas individu, bukan sekadar "sudah dipublikasikan KPU" berarti bebas ditambang).
Tidak ada langkah apa pun diambil untuk item ini.

## Rekomendasi scope publikasi: `full` vs `production-suppression-scope`

Tim HF meminta ini dinilai sendiri, bukan diputuskan sepihak. Rekomendasi: **kedua-duanya**,
sebagai dua split/config terpisah di dataset card HF — datanya **sudah ada** di dua berkas
berbeda, tidak perlu kode baru:

- **`full`** — `gazetteer.jsonl` + `token_homograf.tsv` apa adanya, termasuk 6.918 nama
  kecamatan (banyak berbentuk frasa biasa: `Sukamaju`, `Air Besar`) yang produksi kami buang.
  Untuk peneliti/engineer yang justru ingin tahu "token X juga nama kecamatan DAN kata biasa" —
  bukan derau untuk kebutuhan mereka, meski derau untuk suppression FP kami.
- **`production-suppression-scope`** — `v1_seed.tsv`: 8.456 aman + 890 homograf non-kecamatan +
  328 multi-kata. Ini yang benar-benar dipakai `apps/editor-rust` lewat migrasi 210.

Jangan gabung jadi satu; keduanya punya audiens beda dan mencampurnya diam-diam akan
menyembunyikan alasan kecamatan dibuang di satu sisi.
