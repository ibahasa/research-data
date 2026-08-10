# Arti 43 kata Jawa langka: empat model, kunci jamak yang ditinjau penutur

**Tanggal:** 2026-08-10 · **Penulis:** Benchmark Team · **Lisensi:** CC BY 4.0
**Berkas:** `hasil-model.csv`, `kunci.csv`
**Sumber:** lema berbahasa Jawa dari kamus kami, seluruhnya berstatus terverifikasi
manusia dan sudah terbit di halaman kamus publik

Pengukuran kami sendiri atas empat model yang dipanggil lewat satu perantara.
Lema, definisi, dan kunci jawabannya milik kami. Korpus pembanding yang dipakai
menentukan kelangkaan berasal dari pihak ketiga dan disebut namanya di bawah.

---

## ⚠️ Set ini SEKALI PAKAI

Bagian terpenting di berkas ini, dan ia dinyatakan di depan supaya tidak terlewat.

43 lema di sini terpilih justru **karena tidak ada di sumber Jawa terbuka mana
pun**. Menerbitkan `kunci.csv` membuat sebuah berkas rapi berisi 43 kata Jawa
langka lengkap dengan artinya, yaitu persis lubang data latih yang sedang diukur.

Konsekuensinya mengikat:

- **Sesudah tanggal terbit ini, skor tinggi pada 43 lema yang sama tidak dapat
  dibaca sebagai pemahaman.** Model yang menjawab benar mungkin memahaminya, atau
  mungkin membaca berkas ini.
- **Pengukuran berikutnya wajib memakai lema yang belum pernah diterbitkan.**
- Set ini beralih fungsi jadi **kanari kontaminasi**: bila model yang terbit
  belakangan tiba-tiba mengenal keempat puluh tiga lema ini sementara model
  2026-08-10 hanya 5 sampai 91 persen, selisih itu mengukur seberapa cepat data
  yang diterbitkan masuk ke data latih.

Itu sebabnya berkas ini diterbitkan penuh alih-alih dipangkas. Kadaluwarsanya
datang cepat atau lambat, sebab halaman kamusnya sudah terindeks; menerbitkan
kuncinya membuat **tanggalnya diketahui**, dan tanggal yang diketahui dapat
diukur.

## Apa yang diukur

Tiap model ditanya hal yang sama, pada `temperature` 0, satu ulangan per lema:

```
Apa arti kata Jawa berikut dalam bahasa Indonesia?
Jawab singkat, maksimal satu kalimat.

Kata: <lema>
```

Jawaban dinilai benar bila memuat **salah satu** padanan yang terdaftar di
`kunci.csv`. Pencocokannya membandingkan kata utuh dengan pemuatan, sehingga
`lihat` cocok dengan `melihat` dan `selingkuh` dengan `perselingkuhan`.

Rancangan ini menggantikan rancangan sebelumnya yang menilai terjemahan kalimat
terhadap satu glos, dan gugur karena satu kalimat dapat diterjemahkan dengan
banyak cara yang sama benarnya. Soal arti kata tidak memikul keberatan itu:
jawabannya biner dan tidak dapat berakhir seri.

## Bagaimana "langka" ditetapkan

Bukan frekuensi pemakaian, melainkan **ketidakhadiran di sumber Jawa terbuka yang
paling mungkin masuk data latih model**:

| Sumber | Pemilik |
|---|---|
| NusaX, leksikon dan korpus | IndoNLP |
| NusaWrites, `nusa_kalimat` dan `nusa_alinea` | IndoNLP |
| Wiktionary Jawa, seluruh judul entri | Wikimedia |

Dari 386 lema Jawa terverifikasi, 48 tidak muncul di satu pun ketiganya. Lima
dikeluarkan karena contoh kalimatnya berbahasa Indonesia, menyisakan 43.

**Ketidakhadiran di tiga sumber ini bukan bukti** sebuah kata tidak ada di data
latih model. Web jauh lebih luas, dan sebagian besar isinya tidak dapat kami
periksa.

## Kolom `hasil-model.csv`

Satu baris per pasangan model dan lema. 172 baris, yaitu 4 model dikali 43 lema.

| Kolom | Arti |
|---|---|
| `model_id` | pengenal model di perantara yang kami pakai |
| `lema` | kata Jawa yang ditanyakan |
| `jawaban_model` | jawaban apa adanya, baris baru diganti spasi |
| `kunci_hadir` | 1 bila salah satu padanan di `kunci.csv` muncul di jawaban |
| `latency_ms` | waktu satu panggilan, dari kirim sampai balasan lengkap |
| `biaya_usd` | biaya yang **dilaporkan penyedia**, bukan dihitung dari tabel harga |
| `tokens_in`, `tokens_out` | token masukan dan keluaran menurut penyedia |

## Kolom `kunci.csv`

Satu baris per lema. 43 baris.

| Kolom | Arti |
|---|---|
| `lema`, `definisi_id` | sebagaimana terbit di halaman kamus kami |
| `kunci` | padanan yang diterima, dipisah ` \| ` |
| `jumlah_kunci` | banyaknya padanan; rata-rata 3,2 |
| `kunci_asal` | `penutur` bila sudah ditinjau manusia, `mesin` bila belum. 39 dari 43 sudah |
| `nusax_leksikon`, `nusax_nusawrites_teks`, `jv_wiktionary` | kemunculan lema di tiap sumber. Nol di ketiganya adalah syarat masuk set ini |

## Perlakuan yang TIDAK sama antarmodel

Ini kekurangan terbesar berkas ini, dan ia tidak dapat diperbaiki tanpa membaca
ulang seluruh jawaban.

**Hanya keluaran GPT-5.6 Luna yang ditinjau penutur satu per satu.** Tinjauan itu
menambahkan padanan yang belum terdaftar, dan skornya naik 14 poin tanpa satu
panggilan pun diulang. Tiga model lain tidak pernah mendapat perlakuan yang sama.

Akibatnya seluruh angka di bawah skor tertinggi adalah **batas bawah**, dan ruang
di atasnya tidak sama besar. Jawaban yang dinilai salah tetapi memuat kata dari
definisi lemanya sendiri, yaitu tanda kuat bahwa kuncinya yang meleset:

| Model | Dinilai salah | Berpotensi salah vonis |
|---|---:|---:|
| `mistral-small-3` | 41 | 4 |
| `deepseek-v4-flash-0731` | 20 | 6 |
| `gpt-5.6-luna` | 8 | 3 |
| `gemini-3.5-flash` | 4 | 1 |

## Lantai kebetulan

Kunci jamak menaikkan peluang mengenainya tanpa memahami. Diukur dengan
menyilangkan kunci tiap lema terhadap jawaban seluruh lema lain: **23 dari 1.806
pasang, atau 1,3 persen**, naik dari 0,8 persen ketika kuncinya masih tunggal.

Angka itu wajib diukur ulang setiap kali kunci diperluas. Bila melewati sekitar 5
persen, skornya mulai mengukur keberuntungan.

Satu usulan padanan ditolak karena pengukuran ini: kata `jawa` muncul di 37 persen
jawaban seluruh lema, sebab pertanyaannya sendiri menyebut "kata Jawa".

## Batas lain

Kunci ditulis satu penutur, yaitu penyusun kamusnya sendiri, tanpa angka
kesepakatan antar-penilai.

Satu ulangan per lema per model. Ragam antar-ulangan tidak terukur, sehingga
selisih beberapa lema antara dua model yang berdekatan tidak dapat dipisahkan
dari derau. Dengan 43 lema, satu lema bernilai 2,3 poin.

Biaya diambil dari yang dilaporkan penyedia per panggilan. Yang benar-benar
terpotong dari saldo dapat berbeda bila perantara menerapkan biaya di tingkat
akun.
