# Provenance — AWB RegExTypoFix id.wikipedia

| | |
|---|---|
| Sumber | `https://id.wikipedia.org/wiki/Wikipedia:AutoWikiBrowser/Typos` |
| Metode | MediaWiki `action=raw` (bukan scraping HTML) |
| Revisi (`oldid`) | **25645328** |
| Timestamp revisi | 2024-04-30T01:58:25Z |
| Diambil pada | 2026-07-29T05:42:11Z |
| Ukuran |   232905 byte, 2952 entri `<Typo>` |
| User-Agent | `ibahasa-research/1.0 (halo@ibahasa.com)` |
| **Lisensi** | **CC BY-SA** — bila daftar turunan ikut didistribusikan (mis. masuk paket produk), atribusi + share-alike berlaku. Untuk pemakaian internal sebagai sumber kurasi, cukup catatan ini. |

## Aset lokal yang dipakai untuk penyaringan

| Berkas | Isi | Sumber |
|---|---|---|
| `vendor/kbbi_map.psv` |    71746 lema + rujukan "Lihat X" | `kbbi_entries` (DB lokal, read-only) |
| `vendor/kbbi_proper.txt` |      859 lema `semantic_class='proper'` | idem |
| `vendor/db_misspellings.psv` |      100 `wrong_word` yang sudah jadi artikel /ejaan | `misspelling_articles` |

Halaman sumber hidup (terus disunting komunitas). Untuk memutakhirkan: jalankan `fetch.sh`
lagi, bandingkan `oldid`, lalu diff `out/typos.jsonl` — entri baru dari komunitas datang gratis.
