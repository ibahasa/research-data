# Aturan klasifikasi sebab kegagalan pekerjaan AI

**Tanggal:** 2026-08-05 · **Sumber:** salinan basis data produksi 2026-08-02
**Menyertai:** `taksonomi-kegagalan-job-ai.csv` dan `ringkasan-pekerjaan-ai.csv`
**Dipakai temuan:** [`03 — Ketika AI gagal di sistem nyata`](../docs/riset/03-ketika-ai-gagal-biasanya-bukan-jawabannya-yang-salah.md)

Berkas ini terbit bersama angkanya, bukan sesudahnya. Klasifikasi dari pesan galat
selalu punya kasus yang bisa jatuh ke dua golongan; tanpa aturannya, pembaca tidak
punya cara menilai apakah "36% bentuk jawaban" masuk akal.

## Populasi

Seluruh 50 pekerjaan berstatus gagal dari 3.670 pekerjaan. Bukan sampel, tidak ada
yang dibuang.

## Aturan, diterapkan berurutan

Urutan menentukan hasil: sebuah pesan digolongkan oleh aturan **pertama** yang
cocok. Pesan diperiksa tanpa memandang huruf besar-kecil.

| Urut | Golongan | Cocok bila pesan memuat |
|---:|---|---|
| 1 | **bentuk jawaban tak terbaca mesin** | `parse …json`, `parse AI response`, `unmarshal`, `invalid character` |
| 2 | **penyaring mutu kami menolak hasilnya** | `no entries generated`, `filtered` |
| 3 | **sisi penyedia tidak menjawab** | `429`, `no choices`, `Post "http`, `timeout`, `EOF` |
| 4 | **permintaan kami ditolak** | `400`, `404`, `405`, `invalid_request`, `beta-limitations` |
| 5 | belum tergolong | — |

Setelah aturan ini, golongan kelima **kosong**: ke-50 pekerjaan tergolong.

## Kenapa urutannya begitu

Aturan 1 didahulukan karena pesan tentang penguraian sering **juga** memuat kode
status, sehingga kalau aturan 4 lebih dulu, kegagalan bentuk akan tersamar jadi
permintaan ditolak.

Aturan 2 didahulukan atas 3 dan 4 karena penolakan oleh penyaring kami sendiri
adalah peristiwa yang berbeda jenis: model menjawab, jawabannya terbaca, lalu
**kami** yang menolaknya.

## Batas yang harus ikut dibaca

- **Golongan bisa bertumpuk.** Permintaan yang ditolak penyedia dapat dibaca
  sebagai masalah penyedia maupun masalah permintaan. Kami menggolongkannya
  sebagai permintaan ditolak bila pesannya memuat kode status yang menunjuk
  permintaan (400/404/405), dan sebagai sisi penyedia bila menunjuk ketersediaan
  (429, tanpa jawaban, waktu habis).
- **Pesan galat ditulis oleh sistem yang gagal**, dan bentuknya berubah antar
  versi kode. Aturan ini cocok untuk potret 2026-08-02; potret berikutnya bisa
  menuntut pola tambahan.
- **Tiga pola pada aturan 1 dan 4 ditambahkan setelah memeriksa sisa.** Putaran
  pertama menyisakan enam pekerjaan tak tergolong. Setelah dibaca, tiga di
  antaranya jelas milik golongan yang sudah ada — satu kegagalan penguraian yang
  memakai kata berbeda, dan dua penolakan permintaan (satu kode 405, satu
  penolakan parameter). Pola `parse …json`, `405`, dan `beta-limitations`
  ditambahkan karena itu. Langkah ini diungkapkan karena ia **penyesuaian aturan
  setelah melihat data**, dan pembaca berhak tahu.
- **50 kejadian adalah dasar yang tipis untuk empat golongan.** Yang kokoh adalah
  urutan dan selisih besar; selisih antara 36% dan 30% **tidak** bermakna pada
  jumlah ini.

## Catatan penting: sebagian "kegagalan" adalah keberhasilan

Tiga dari 50 kegagalan berbunyi bahwa keluaran model **ditolak karena duplikat**.
Model menjawab, jawabannya terbaca utuh, lalu penyaring kami menolaknya. Itu pagar
yang bekerja, bukan kegagalan.

Artinya angka gagal 1,36% **sedikit melebihkan** kegagalan yang sesungguhnya.
Setelah ketiganya dikeluarkan: **1,28%**.

Selisihnya kecil dan tidak mengubah kesimpulan apa pun. Kami menyebutkannya karena
angka yang melebihkan kegagalan sendiri lebih mudah dipercaya daripada angka yang
mengecilkannya, dan pembaca tidak punya cara mengetahuinya kecuali diberi tahu.

## Cara memeriksa ulang

Kueri lengkapnya ada di berkas kerja internal
(`apps/system-audit/docs/tasks/task-05-*`). Ia tidak disalin ke sini karena memuat
nama tabel dan kolom.

**Baris per pekerjaan sengaja tidak diterbitkan.** Muatan permintaan dan pesan
galat memuat potongan prompt dan nama model yang tidak dimaksudkan terbit, dan
agregat ini sudah membuktikan seluruh klaim di artikelnya.

## Lisensi

CC BY 4.0, sama dengan katalog `research-data`. Data milik sendiri; tidak ada
sumber pihak ketiga yang menyeret syarat tambahan.
