# Provenance — Leipzig Corpora Collection (Indonesian)

| | |
|---|---|
| Sumber | `https://downloads.wortschatz-leipzig.de/corpora/` (host unduhan langsung — **beda** dari `wortschatz.uni-leipzig.de` yang dilindungi Anubis anti-bot; index HTML diambil manual via browser oleh pemilik repo, lalu link `.tar.gz` di dalamnya diverifikasi bisa diunduh otomatis) |
| Berkas | `ind_mixed_2013_1M.tar.gz` (bangun 2021-12-16, campuran web+berita+wiki, 1 juta kalimat) + `ind_news_2020_1M.tar.gz` (bangun ~2021-03-31, berita 2020, 1 juta kalimat) |
| Diambil pada | 2026-07-30 |
| Isi dipakai | `*-words.txt` saja (format `rank\tword\tfrequency`, tab-separated) — bukan `*-sentences.txt`/`*-co_*.txt` (ko-okurensi, tak dipakai fase ini) |
| **Lisensi** | **CC BY 4.0** (dikonfirmasi di `docs/tasks/task-07-frequency-corpus.md`). Atribusi wajib bila daftar turunan (`out/word_frequencies.json`) ikut didistribusikan ke luar. |

## Kenapa host beda dari yang tercatat di task-07 awal

Halaman index (`wortschatz.uni-leipzig.de/en/download/Indonesian`) sekarang di belakang **Anubis**
(proof-of-work anti-bot). Tak dicoba dibobol. Solusinya: pemilik repo membuka halaman itu di
browser biasa (Anubis lolos otomatis untuk browser sungguhan), lalu link unduhan `.tar.gz` di
dalam HTML itu ternyata menunjuk ke subdomain **`downloads.wortschatz-leipzig.de`** yang
**TIDAK** dilindungi Anubis — bisa diunduh otomatis biasa. HTML index tersimpan di
`scrape/downloads/Downloads – Indonesian – Wortschatz Leipzig.html` kalau perlu dicek ulang link
korpus lain (mis. tahun/domain lain, ukuran berbeda).

## Kenapa 1M dipilih, bukan 100K

Diuji dua-duanya (lihat riwayat kerja) — cakupan lema kita (72.454 kata) yang dapat frekuensi:

| Ukuran korpus | Cakupan |
|---|---|
| 100K kalimat (mixed+news) | 22.845 kata (31.5%) |
| **1M kalimat (mixed+news)** | **33.895 kata (46.8%)** |

1M jelas lebih baik tanpa biaya berarti (file ~10-20MB, wajar). 100K dihapus dari `vendor/`
(redundan, subset dari 1M).
