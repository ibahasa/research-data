# Provenance — Gazetteer Nama Diri Indonesia

| | |
|---|---|
| Sumber utama | Wikidata SPARQL (`query.wikidata.org`) |
| **Lisensi** | **CC0** — data Wikidata bebas hak cipta; tidak menyeret kewajiban share-alike ke produk |
| Metode | traversal hierarkis `Q252 --wdt:P150--> provinsi -> kabupaten/kota -> kecamatan`; nama orang lewat `P27=Q252` + `P735`/`P734`, di-GROUP BY nama |
| Diambil pada | 2026-07-29T14:20:36Z |
| Jumlah permintaan | 5 query SPARQL, jeda 2 detik — bukan crawling |
| User-Agent | `ibahasa-research/1.0 (halo@ibahasa.com)` |

## Hasil mentah

| Berkas | Isi |
|---|---|
| `vendor/provinsi.json` | 42 |
| `vendor/kabupaten.json` | 601 kabupaten/kota |
| `vendor/kecamatan.json` | 9.619 kecamatan |
| `vendor/nama_depan.json` | 3.079 nama depan + jumlah pemakai |
| `vendor/nama_keluarga.json` | 2.077 nama keluarga + jumlah pemakai |
| `vendor/kbbi_pos.psv` | 71.829 lema KBBI + POS (DB lokal, read-only) — untuk deteksi homograf |

**Catatan metode:** query agregat global (`?item wdt:P17 wd:Q252` + GROUP BY) **timeout** di
endpoint publik. Traversal hierarkis per tingkat berjalan lancar dan jauh lebih ringan bagi server.
