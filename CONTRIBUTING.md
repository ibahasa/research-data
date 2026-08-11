# Alur kerja repo ini

Dokumen ini untuk tim ibahasa yang mengelola repo ini. Kalau Anda cuma mau
mengutip atau memverifikasi data, baca `README.md` saja.

## Dari riset sampai artikel terbit

1. Riset selesai, ekspor datanya lewat `scripts/publish-research-data.sh`.
   Skrip ini yang mendorong `MANIFEST.json`, `PROVENANCE.md`, dan
   memperbarui `INDEX.md` — tidak perlu ditulis manual.
2. Ambil commit SHA hasil push tadi, tempelkan sebagai tautan provenance di
   artikel riset (di admin ibahasa.com, bukan di repo ini).
3. Artikel ditinjau ulang dan direvisi sebelum naik produksi.
4. Artikel terbit.

## Langkah yang sering kelewat: setelah artikel terbit

Begitu artikel yang memakai dataset di sini sudah live, balik ke
`MANIFEST.json` dataset yang bersangkutan dan tambahkan field `cited_by`:

```json
{
  "slug": "...",
  "title": "...",
  "cited_by": ["https://ibahasa.com/id/riset/slug-artikelnya"]
}
```

Array, karena satu dataset bisa dipakai lebih dari satu artikel. Field ini
**opsional** dan **aditif** — jangan mengubah atau menghapus field yang
sudah ada (`sha256`, `source_commit`, `fingerprint`, dll.), karena itu yang
sudah dikutip dari luar.

Kenapa ini langkah terpisah, bukan bagian dari langkah 1: saat data
diekspor, artikelnya belum ada dan slug-nya belum ditentukan. Tautan
baliknya baru bisa diisi setelah artikel benar-benar live.

Tanpa langkah ini, orang yang mengunduh data langsung dari repo (tanpa
lewat artikelnya) tidak tahu artikel mana yang memakainya — cuma bisa
menebak dari nama dataset.

## Kapan bikin GitHub Release baru (versi Zenodo baru)

**Tidak per artikel.** Tiap Release baru mencetak DOI versi baru di
Zenodo, dan kalau itu terjadi tiap artikel terbit (rutin tiap beberapa
hari berdasarkan `INDEX.md`), version DOI-nya jadi terlalu banyak dan
jarang beda berarti dari versi sebelumnya.

Rilis versi baru pada titik yang benar-benar berarti — misalnya setelah
beberapa artikel terkumpul, atau saat memang ada pihak luar yang mau
mengutip arsip ini secara formal. Reproduksibilitas per-artikel sudah
terjamin lewat tautan commit SHA (lihat "Citing a file" di `README.md`),
jadi tidak ada tekanan untuk merilis versi baru tiap kali.
