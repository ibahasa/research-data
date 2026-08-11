# Arti 43 kata Jawa langka: empat model, kunci jamak yang ditinjau penutur

**Tanggal:** 2026-08-11 · **Penulis:** Benchmark Team · **Lisensi:** CC BY 4.0
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
  2026-08-11 hanya 2 sampai 93 persen, selisih itu mengukur seberapa cepat data
  yang diterbitkan masuk ke data latih.

Itu sebabnya berkas ini diterbitkan penuh alih-alih dipangkas. Kadaluwarsanya
datang cepat atau lambat, sebab halaman kamusnya sudah terindeks; menerbitkan
kuncinya membuat **tanggalnya diketahui**, dan tanggal yang diketahui dapat
diukur.

## Perubahan sejak terbitan pertama (2026-08-10)

Terbitan ini menggantikan versi 2026-08-10. **Tidak satu pun jawaban model
berubah**; yang berubah kunci jawaban kami, setelah model keempat akhirnya dibaca
penutur.

| model | 2026-08-10 | sekarang |
|---|---:|---:|
| `mistral-small-3` | 2 dari 43 | **1 dari 43** |
| `deepseek-v4-flash` | 23 dari 43 | **22 dari 43** |
| `gpt-5.6-luna` | 35 dari 43 | 35 dari 43 |
| `gemini-3.5-flash` | 39 dari 43 | **40 dari 43** |

Empat sunting kunci menyebabkannya:

- **`gremet gremet` dibuang dari lema `Gremet`.** Kunci itu lemanya sendiri,
  diulang. Model selalu mengulang kata yang ditanyakan, jadi kunci itu meloloskan
  jawaban apa pun. Dua model lolos lewatnya dengan jawaban yang salah total.
- **`bunyi` dibuang dari lema `klotekan`.** Ia menggambarkan jenis makna, bukan
  maknanya.
- **`slang` dan `slang jawa` dibuang** dari `Coeg`, `Gondes`, `Kupret`, `Ndlogok`,
  dan `sompret`. Setiap kata Jawa gaul adalah "slang", jadi kunci itu meloloskan
  jawaban yang tidak menyatakan apa pun tentang maknanya.
- **Padanan ditambahkan** pada `Gendakan`, `Leksanani`, `Pimen`, dan `Gondes`.

`Gondes` kasus tersendiri dan ia menyingkap batas rancangan ini. Kamus kami
merekamnya sebagai sapaan akrab; asal-usulnya "gondrong ndeso", dan makna itu
masih dipakai. Satu model menjawab dengan asal-usulnya, tepat sampai ke
etimologinya, lalu dinilai salah karena kami hanya merekam satu makna. Padanan
`gondrong` dan `ndeso` ditambahkan. **Ini persoalan rujukan tunggal**, kelas yang
sama yang dulu menggugurkan rancangan terjemahan kami: di sana satu kalimat punya
banyak terjemahan sah, di sini satu lema punya lebih dari satu makna sah.

Satu usulan **ditolak** setelah ditimbang: `bertanya` untuk `Pimen`. Kata itu
fungsi gramatikal `pimen`, bukan artinya, sekelas dengan `bunyi` yang dibuang di
atas.

Lantai kebetulan penilai turun dari **1,3 ke 0,7 persen**. Untuk pertama kalinya
sebuah putaran tinjau membuat alat ukur ini lebih ketat, bukan lebih longgar.

## Apa yang diukur

Tiap model ditanya hal yang sama, pada `temperature` 0, satu ulangan per lema:

```
Apa arti kata Jawa berikut dalam bahasa Indonesia?
Jawab singkat, maksimal satu kalimat.

Kata: <lema>
```

Jawaban dinilai benar bila memuat **salah satu** padanan yang terdaftar di
`kunci.csv`. Pencocokannya membandingkan akar kata.

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
| `kunci_hadir` | 1 bila salah satu padanan di `kunci.csv` muncul di jawaban. **Inilah vonis benchmark** |
| `sim_definisi` | kedekatan makna jawaban terhadap definisi lemanya, 0 sampai 1 |
| `persentil_vs_lantai` | posisi `sim_definisi` di antara 7.224 pasang yang dijamin tidak berhubungan |
| `sim_kunci_maks` | kedekatan makna tertinggi terhadap salah satu padanan di `kunci.csv` |
| `peringkat_dalam_lema` | 1 sampai 4, urutan keempat model pada lema itu menurut `sim_definisi` |
| `vonis_penutur` | 1 atau 0, vonis manusia atas jawaban ini. Kosong bila belum ditinjau |
| `latency_ms` | waktu satu panggilan, dari kirim sampai balasan lengkap |
| `biaya_usd` | biaya yang **dilaporkan penyedia**, bukan dihitung dari tabel harga |
| `tokens_in`, `tokens_out` | token masukan dan keluaran menurut penyedia |

### Kolom semantik bukan vonis

Empat kolom kemiripan dihitung dengan
[IndoBERT versi ONNX](https://huggingface.co/asmud/LazarusNLP-indobert-onnx),
turunan [`LazarusNLP/congen-indobert-lite-base`](https://huggingface.co/LazarusNLP/congen-indobert-lite-base),
dijalankan lokal. Ia deterministik; dua jalan berturut-turut menghasilkan berkas
identik bita per bita.

**Kolom ini tidak pernah mengubah `kunci_hadir`.** Ia dilaporkan berdampingan
supaya peringkat model dapat diperiksa dengan alat ukur yang tidak memakai kunci
sama sekali.

Cosine tidak punya makna absolut, jadi `persentil_vs_lantai` adalah kolom yang
layak dikutip, bukan `sim_definisi` mentahnya. Lantainya diukur dengan
menyilangkan jawaban tiap lema ke definisi 42 lema lain:

| | rerata pasangan tak berhubungan | rerata pasangan sebenarnya |
|---|---:|---:|
| teks apa adanya | 0,389 | 0,527 |
| lema & 49 kata pembingkai dibuang | 0,143 | 0,344 |

Artinya **63 persen kemiripan pada teks apa adanya bukan makna**, melainkan
bingkai kalimat yang dipakai jawaban dan definisi sama-sama. Angka di berkas ini
dihitung setelah pembersihan.

`sim_kunci_maks` punya lantainya sendiri, diukur dengan cara yang sama tetapi
menyilangkan jawaban ke kunci 42 lema lain:

| | lantai | pasangan sebenarnya | selisih |
|---|---:|---:|---:|
| `sim_definisi` | 0,143 | 0,344 | 0,202 |
| `sim_kunci_maks` | 0,168 | 0,458 | **0,290** |

Jaraknya lebih lebar, tetapi kolom itu **memakai kunci**, sehingga ia tidak dapat
dipakai memeriksa apakah kunci kami bias. Untuk keperluan itu hanya `sim_definisi`
yang berdiri sendiri.

Daya pisah kolom semantik terhadap vonis kunci, atas seluruh 172 baris: **0,937**.
Terhadap vonis penutur: 0,73 sampai 0,94 bergantung model. Cukup untuk mengurutkan
antrean baca, tidak cukup untuk memvonis.

## Kolom `kunci.csv`

Satu baris per lema. 43 baris.

| Kolom | Arti |
|---|---|
| `lema`, `definisi_id` | sebagaimana terbit di halaman kamus kami |
| `kunci` | padanan yang diterima, dipisah ` \| ` |
| `jumlah_kunci` | banyaknya padanan; rata-rata 3,1 |
| `kunci_asal` | `penutur` bila sudah ditinjau manusia, `mesin` bila belum. 39 dari 43 sudah |
| `nusax_leksikon`, `nusax_nusawrites_teks`, `jv_wiktionary` | kemunculan lema di tiap sumber. Nol di ketiganya adalah syarat masuk set ini |

## Batas

**Keempat model kini sudah dibaca penutur satu per satu.** Perlakuan tak sama yang
menjadi batasan terbesar terbitan pertama sudah tertutup. `vonis_penutur` terisi
pada 171 dari 172 baris; satu baris dikosongkan karena penilainya menandainya
ambigu alih-alih benar atau salah.

**Kunci ditulis satu penutur**, yaitu penyusun kamusnya sendiri, tanpa angka
kesepakatan antar-penilai.

**Satu ulangan per lema per model.** Ragam antar-ulangan tidak terukur, sehingga
selisih beberapa lema antara dua model yang berdekatan tidak dapat dipisahkan dari
derau. Dengan 43 lema, satu lema bernilai 2,3 poin.

**Satu keterbatasan sengaja tidak diperbaiki.** Pencocokan kunci tidak mengupas
akhiran `-lah`, sehingga jawaban "laksanakanlah" tidak dianggap memuat kunci
"melaksanakan". Menambalnya akan menaikkan dua model di tengah pengukuran, jadi ia
dicatat sebagai batas alat dan angka di sini adalah angka dengan batas itu
terpasang.

**Model embeddingnya terkuantisasi 8 bit** dan dilatih untuk bahasa Indonesia
umum, sementara teks di sini memuat kata Jawa dan ragam kamus. Pengaruh keduanya
tidak diukur terpisah.

**Biaya diambil dari yang dilaporkan penyedia** per panggilan. Yang benar-benar
terpotong dari saldo dapat berbeda bila perantara menerapkan biaya di tingkat
akun.
