# Peta Keputusan: Rule LanguageTool → iBahasa

Sumber: `github.com/languagetool-org/languagetool` (sparse checkout, bukan scraping HTML).  
Unit analisis: **2909** entri (satu `<rulegroup>` = satu entri; 6277 rule mentah sebelum dedupe varian infleksi).

Silang-periksa: halaman `community.languagetool.org/rule/list?lang=en` melaporkan **6.150** rule Inggris; kita mengekstrak **6277** varian mentah dari repo. Selisihnya wajar (repo lebih baru + rule Java + berkas dialek), jadi ekstraksi dianggap lengkap.

> Untuk tim `apps/editor` / `apps/editor-rust`: baca **`HANDOFF-EDITOR.md`** lebih dulu — dokumen itu memetakan temuan di sini ke kode kita (apa yang sudah ada, apa yang belum, dan di lapis mana tiap rule seharusnya ditaruh). Dokumen ini adalah datanya.

## 1. Lima temuan yang mengubah rencana

**1. Rule tipografi universal TIDAK ada di `grammar.xml`.** Yang paling kita butuhkan untuk Lapis-1 — spasi ganda, tanda baca ganda, kurung tak berpasangan, kata berulang, kapital awal kalimat — ditulis sebagai kelas **Java** di `languagetool-core`, bukan XML. Kalau hanya XML yang ditambang, kesimpulannya akan salah: seolah LanguageTool nyaris tidak punya rule bebas-bahasa. Karena itu sumber kedua (78 kelas Java core + en) ikut ditambang.

**2. XML Inggris hampir seluruhnya leksikal.** Dari 2831 entri XML, hanya 14 yang murni regex — sisanya menempel pada kata/POS Inggris. Nilai XML bagi kita bukan isinya, melainkan **cetakan mekanismenya**: pasangan homofon, kata majemuk, pleonasme, kapitalisasi.

**3. Sepertiga rule tidak bisa dipakai sama sekali** (954 entri, 33%): artikel a/an/the, kala (tense), kesesuaian subjek-verba, posesif apostrof, ejaan Britania vs Amerika. Bahasa Indonesia tidak punya kategori-kategori itu.

**4. Tidak ada modul Indonesia di LanguageTool.** Modul yang tersedia: ar, ast, be, br, ca, crh, da, de, el, en, eo, es, fa, fr, ga, gl, is, it, ja, km, lt, ml, nl, pl, pt, ro, ru, sk, sl, sr, sv, ta, **tl**, uk, zh. Tidak ada `id`/`ms`. Yang paling dekat secara tipologis adalah **Tagalog (tl)** — sesama Austronesia. Ke-44 rule-nya ditambang terpisah ke `rules_tl.jsonl` (lihat bagian 6).

**5. Beberapa rule harus DIBALIK, bukan disalin.** Pemisah desimal/ribuan, format tanggal, dan penulisan mata uang punya konvensi Indonesia yang berlawanan dengan Inggris. Mengadopsinya mentah-mentah justru akan menandai teks yang sudah benar.

## 2. Agregat

### Portabilitas
| Portabilitas | Jumlah | % |
|---|---:|---:|
| adapt | 1870 | 64.3% |
| skip | 954 | 32.8% |
| direct | 85 | 2.9% |

### Mekanisme deteksi
| Mekanisme | Jumlah | % |
|---|---:|---:|
| needs-lexicon | 1185 | 40.7% |
| needs-morphology | 867 | 29.8% |
| needs-POS | 811 | 27.9% |
| regex-only | 46 | 1.6% |

### Lapis sasaran
| Lapis | Jumlah | % |
|---|---:|---:|
| rust | 2851 | 98.0% |
| FE | 41 | 1.4% |
| either | 17 | 0.6% |

> Catatan: `rust` mendominasi karena XML Inggris berbasis leksikon/POS. Untuk kita, yang menentukan bukan angka ini melainkan kolom `portability` — pekerjaan FE yang nyata ada pada 85 entri `direct`.

### Ongkos implementasi
| Effort | Jumlah | % |
|---|---:|---:|
| L | 1683 | 57.9% |
| S | 639 | 22.0% |
| M | 587 | 20.2% |

### Kategori (semua)

| Kategori | n | direct | adapt | skip | S | M | L |
|---|---:|---:|---:|---:|---:|---:|---:|
| Grammar | 953 | 1 | 332 | 620 | 95 | 141 | 717 |
| Possible Typo | 539 | 0 | 488 | 51 | 195 | 146 | 198 |
| Commonly Confused Words | 513 | 0 | 479 | 34 | 95 | 71 | 347 |
| Proper Nouns | 131 | 0 | 115 | 16 | 78 | 13 | 40 |
| Redundant Phrases | 117 | 0 | 117 | 0 | 35 | 41 | 41 |
| Compounding | 112 | 0 | 107 | 5 | 16 | 28 | 68 |
| Style | 87 | 1 | 36 | 50 | 25 | 14 | 48 |
| Plain English | 81 | 0 | 8 | 73 | 18 | 39 | 24 |
| Punctuation | 78 | 16 | 49 | 13 | 2 | 14 | 62 |
| Java rule (core) | 47 | 27 | 12 | 8 | 27 | 7 | 13 |
| Upper/Lowercase | 41 | 0 | 41 | 0 | 14 | 5 | 22 |
| Collocations | 37 | 0 | 36 | 1 | 5 | 9 | 23 |
| Typography | 37 | 30 | 4 | 3 | 7 | 8 | 22 |
| Java rule (en) | 31 | 9 | 20 | 2 | 5 | 17 | 9 |
| Semantics | 27 | 0 | 19 | 8 | 2 | 9 | 16 |
| Nonstandard Phrases | 26 | 0 | 2 | 24 | 4 | 15 | 7 |
| American English phrases | 19 | 0 | 0 | 19 | 8 | 1 | 10 |
| British English phrases | 9 | 0 | 0 | 9 | 3 | 2 | 4 |
| American English Style | 9 | 0 | 0 | 9 | 0 | 6 | 3 |
| Wikipedia | 5 | 1 | 0 | 4 | 5 | 0 | 0 |
| Orthographic errors | 3 | 0 | 3 | 0 | 0 | 0 | 3 |
| British English, Oxford spelling (~iz~ not ~is~) | 2 | 0 | 0 | 2 | 0 | 0 | 2 |
| Repetitions (Style) | 2 | 0 | 2 | 0 | 0 | 0 | 2 |
| Creative Writing | 2 | 0 | 0 | 2 | 0 | 0 | 2 |
| Academic Writing | 1 | 0 | 0 | 1 | 0 | 1 | 0 |

## 3. Shortlist prioritas

Baris = **pola**, bukan leksem Inggris. Contoh diambil apa adanya dari `<example>` (atau javadoc untuk rule Java). Kolom relevansi Indonesia memakai `perlu-verifikasi` bila kaidahnya belum dipastikan — tidak ada kaidah Indonesia yang dikarang di sini.

Tanda `*` pada kolom Lapis/Effort = penilaian kami untuk konteks Indonesia, berbeda dari nilai mentah LanguageTool di `rules.jsonl` (biasanya karena POS di LT hanya berperan sebagai antipattern, bukan pemicu).

**P0 — Lapis-1 (FE): regex murni, tanpa kamus dan tanpa POS. Tidak butuh data linguistik baru.**

| # | ID LanguageTool | Pola | Lapis | Effort | Contoh (dari repo) | Relevansi Indonesia |
|---|---|---|---|---|---|---|
| 1 | `WHITESPACE_RULE` | Spasi berulang di dalam kalimat | FE | S (regex saja) | _Whitespace repetition (bad formatting) Check if there is duplicated whitespace i_ | Ada padanan: konvensi tipografi Indonesia sama (satu spasi antarkata). |
| 2 | `COMMA_PARENTHESIS_WHITESPACE` | Spasi sebelum koma / di dalam kurung | FE | S (regex saja) | _Use of whitespace before comma and before/after parentheses A rule that matches _ | Ada padanan: PUEBI — tanda koma rapat ke kata sebelumnya, diikuti satu spasi. |
| 3 | `WHITESPACE_PUNCTUATION` | Spasi sebelum titik dua, titik koma, persen | FE | S (regex saja) | _Use of whitespace before colon, semicolon and percentage._ | Ada padanan: sama persis untuk titik dua/titik koma/persen dalam tipografi Indonesia. |
| 4 | `DOUBLE_PUNCTUATION` | Dua titik/koma berturut-turut ('..', ',,') | FE | S (regex saja) | _Use of two consecutive dots or commas A rule that matches ".." (but not "..." et_ | Ada padanan: galat ketik universal; hati-hati jangan menandai elipsis '...' sebagai galat. |
| 5 | `SENTENCE_WHITESPACE` | Spasi hilang antara dua kalimat ('...selesai.Kalimat') | FE | S (regex saja) | _Missing space between sentences Checks that there's whitespace between sentences_ | Ada padanan: sama; deteksi butuh pemenggal kalimat yang sadar singkatan (dll., yth.). |
| 6 | `WORD_REPEAT_RULE` | Kata sama berulang berturut-turut ('yang yang') | FE | S (regex saja) | ✗ will will | Galat ketik universal. WAJIB kecualikan reduplikasi sah Indonesia: 'anak-anak', 'lama-lama', dan kata ulang tanpa hubung yang memang berpasangan. |
| 7 | `UNPAIRED_BRACKETS` | Kurung/kurawal tak berpasangan | FE\* | S\* (butuh POS) | _Unpaired braces, brackets, quotation marks and similar symbols Rule that finds u_ | Ada padanan: tanda kurung dalam PUEBI selalu berpasangan buka-tutup. Kelas generiknya bebas bahasa; POS hanya dipakai subclass Inggris untuk apostrof. |
| 8 | `UNPAIRED_QUOTES` | Tanda petik tak berpasangan | FE | S (regex saja) | _Unpaired quotation marks Rule that finds unpaired quotes @author Fred Kruse @sin_ | Ada padanan: tanda petik selalu berpasangan; termasuk petik lengkung “ ”. |
| 9 | `ELLIPSIS` | '...' tiga titik → karakter elipsis '…' | FE | S (regex saja) | ✗ This is important... as far as I know.<br>✓ This is important… as far as I know. | Ada padanan: elipsis tiga titik, dan menjadi EMPAT titik bila di akhir kalimat. perlu-verifikasi gaya kita: '...' rapat, '. . .' berspasi, atau dinormalisasi ke '…'. |
| 10 | `TWO_HYPHENS` | '--' → tanda pisah em dash '—' | FE | M (regex saja) | ✗ This is great -- is it not?<br>✓ This is great — is it not? | Ada padanan: PUEBI membedakan tanda hubung (-) dan tanda pisah (—). |
| 11 | `HYPHEN_TO_EN` | Rentang angka '40-70' → tanda pisah en dash '40–70' | FE\* | S\* (butuh POS) | ✗ Participants in the study were 40-70 years old.<br>✓ Participants in the study were 40–70 years old. | Ada padanan: PUEBI memakai tanda pisah (–) untuk rentang, mis. '2020–2024', 'Jakarta–Bandung'. |
| 12 | `CURRENCY_SPACE` | Spasi menempel salah pada lambang mata uang ('$ 100') | FE | S (regex saja) | ✗ You owe me $ 100.<br>✓ You owe me $100. | PERLU DIBALIK: penulisan rupiah Indonesia adalah 'Rp100.000' (tanpa spasi, tanpa titik setelah Rp) — mekanismenya sama, polanya berbeda. |
| 13 | `COMMA_PERIOD_CONFUSION` | Pemisah desimal/ribuan tertukar ('12,5%' vs '12.5%') | FE\* | M\* (butuh kamus/daftar) | ✗ The price rose by 12,5%<br>✓ The price rose by 12.5% | PERLU DIBALIK: Indonesia memakai KOMA sebagai desimal dan TITIK sebagai pemisah ribuan. Rule ini bernilai tinggi karena teks Indonesia sering tercampur konvensi Inggris. |
| 14 | `SPACE_BETWEEN_NUMBER_AND_WORD` | Spasi hilang antara angka dan satuan ('1min') | FE\* | S\* (butuh POS) | ✗ It took me 1min.<br>✓ It took me 1 min. | perlu-verifikasi format satuan, tetapi kebutuhan spasi angka+satuan ('5 kg') sama. |
| 15 | `COMMA_AND_NUMBERS` | Koma menempel di antara angka ('16,1963') | FE | M (regex saja) | ✗ On June 16,1963 Valentina Tereshkova was launched into space aboard Vo<br>✓ On June 16, 1963 Valentina Tereshkova was launched into space aboard V | Ada padanan: koma diikuti spasi; berguna untuk tanggal & daftar angka. |
| 16 | `PLUS_MINUS` | '+-' → simbol '±' | FE | S (regex saja) | ✗ m = 75,5 +- 0,1 g<br>✓ m = 75,5 ± 0,1 g | Ada padanan: simbol matematis bersifat universal. |
| 17 | `ARROWS` | '->' → '→' | FE | M (regex saja) | ✗ -> Point 1<br>✓ → Point 1 | Ada padanan: universal (tipografi, bukan kaidah bahasa). |
| 18 | `PUNCTUATION_GENERIC_CHECK` | Kombinasi tanda baca tak lazim ('?!.,') | FE | S (regex saja) | _Use of unusual combination of punctuation characters A rule that matches "..", "_ | Ada padanan: tanda baca berulang/bertumpuk tidak baku dalam ragam formal Indonesia. |
| 19 | `PUNCTUATION_PARAGRAPH_END` | Paragraf berakhir tanpa tanda baca | FE | S (regex saja) | _No punctuation mark at the end of paragraph A rule that checks for a punctuation_ | Ada padanan: kalimat berita diakhiri titik. |
| 20 | `WHITESPACE_PARAGRAPH` | Spasi di awal/akhir paragraf | FE | S (regex saja) | _Space character at the end of paragraph A rule that checks for a whitespace at t_ | Ada padanan: murni kebersihan tipografi. |
| 21 | `TOO_LONG_SENTENCE` | Kalimat melebihi N kata (ambang dapat diatur) | FE | S (regex saja) | _A rule that warns on long sentences_ | perlu-verifikasi ambangnya untuk bahasa Indonesia; konsepnya (keterbacaan) universal. |
| 22 | `NO_SPACE_CLOSING_QUOTE` | Tidak ada spasi setelah tanda petik penutup | FE | M (regex saja) | ✗ “Good morning, Frank,”said Hal.<br>✓ “Good morning, Frank,” said Hal. | Ada padanan: sama dalam tipografi Indonesia. |
| 23 | `COMMA_CLOSING_PARENTHESIS` | Koma sebelum kurung tutup | FE\* | S\* (butuh POS) | ✗ Its villages include Dreibelbis (also in Greenwich Township,) Edenburg<br>✓ Its villages include Dreibelbis (also in Greenwich Township), Edenburg | Ada padanan: penempatan koma terhadap kurung sama; POS di LT hanya antipattern. |
| 24 | `UNLIKELY_OPENING_PUNCTUATION` | Tanda baca menggantung di awal kalimat | FE\* | S\* (butuh POS) | _Loose punctuation mark._ | Ada padanan: universal; POS di LT hanya antipattern. |
| 25 | `SPACE_BEFORE_PARENTHESIS` | Spasi hilang sebelum kurung buka | FE\* | M\* (butuh POS) | ✗ I'm from San Diego(California).<br>✓ I'm from San Diego(California). | Ada padanan: universal; LT memakai POS hanya untuk menekan positif palsu. |

**P1 — Lapis-2 (editor-rust), mekanisme portabel dengan data Indonesia.**

| # | ID LanguageTool | Pola | Lapis | Effort | Contoh (dari repo) | Relevansi Indonesia |
|---|---|---|---|---|---|---|
| 1 | `ABSTRACT:AbstractWordCoherencyRule` | Konsistensi ejaan dalam satu dokumen (dua varian ejaan kata yang sama) | rust\* | M\* (regex saja) | _A rule that matches words for which two different spellings are used throughout _ | SANGAT relevan: 'praktik/praktek', 'analisis/analisa', 'nasihat/nasehat', 'izin/ijin'. Bisa dijalankan TANPA memutuskan mana yang baku (cukup konsistensi), lalu ditingkatkan ke saran bentuk baku dari KBBI. Kandidat terkuat untuk editor-rust. |
| 2 | `UPPERCASE_SENTENCE_START` | Huruf kapital pada awal kalimat | either\* | M\* (butuh POS) | _Checks that a sentence starts with an uppercase letter Checks that a sentence st_ | Ada padanan: PUEBI mewajibkan kapital di awal kalimat. Versi LT memakai POS untuk mengecualikan nama diri; versi kita bisa memakai daftar singkatan + KBBI dulu. |
| 3 | `HALLOWEEN` | Kapitalisasi nama diri/peristiwa/merek (kelas rule, ~156 entri) | rust\* | M\* (butuh kamus/daftar) | ✗ It's trick or treat on halloween.<br>✓ It's trick or treat on Halloween. | Ada padanan: PUEBI mewajibkan kapital untuk nama orang, tempat, bangsa, bahasa, hari, bulan, hari besar. Datanya harus daftar Indonesia (Idulfitri, Nyepi, Jawa, Senin, Agustus). |
| 4 | `TIK_TOK` | Ejaan resmi nama diri/merek ('Tik tok' → 'TikTok') | rust | S (butuh kamus/daftar) | ✗ Tik tok is a video-sharing social networking service owned by ByteDanc<br>✓ TikTok is a video-sharing social networking service owned by ByteDance | perlu-verifikasi: mekanismenya sama; perlu daftar nama Indonesia (Gojek, Tokopedia, Jakarta Selatan) dan nama geografis KBBI. |
| 5 | `LOOKS_LIKES` | Bentuk salah ketik yang mirip kata sah (kelas rule, ~488 entri) | rust | S (butuh kamus/daftar) | ✗ So far it looks likes this.<br>✓ So far it looks like this. | perlu-verifikasi per lema: konsep bentuk baku ada (KBBI); daftar Inggris tidak terpakai. Analog Indonesia: 'silahkan'→'silakan', 'hutang'→'utang', 'praktek'→'praktik'. |
| 6 | `HEATH_HEALTH` | Pasangan kata mudah tertukar / homofon (kelas rule, ~479 entri) | rust | S (butuh kamus/daftar) | ✗ He works for the heath department.<br>✓ He works for the health department. | perlu-verifikasi: konsepnya ada. Analog paling bernilai untuk Indonesia adalah 'di' preposisi vs 'di-' awalan ('di rumah' vs 'dimakan'), 'ke'/'ke-', dan 'pun'. PUEBI memang memisahkan kata depan dan merangkaikan awalan. |
| 7 | `DENIAL_OF_SERVICE_ATTACK` | Kata majemuk: dipisah vs digabung vs berhubung (~107 entri) | rust | M (butuh kamus/daftar) | ✗ Our website got hit by a denial of service attack.<br>✓ Our website got hit by a denial-of-service attack. | perlu-verifikasi per lema: kaidah PUEBI tentang gabungan kata ada ('terima kasih' terpisah, 'daripada' padu). Ini masalah nyata pengguna Indonesia ('di mana'/'dimana'). |
| 8 | `PIN_NUMBER` | Pleonasme/frasa mubazir (~117 entri) | rust | S (butuh kamus/daftar) | ✗ Please input your PIN number to access.<br>✓ Please input your PIN to access. | perlu-verifikasi daftar: konsep pleonasme berlaku dalam bahasa Indonesia ('agar supaya', 'naik ke atas', 'para hadirin'); daftar harus disusun dari korpus sendiri. |
| 9 | `IN_CHARGE_FOR` | Kolokasi preposisi salah (~36 entri) | rust | M (butuh kamus/daftar) | ✗ Who is in charge for this shop?<br>✓ Who is in charge of this shop? | perlu-verifikasi: analog Indonesia ada ('berbeda dengan' vs 'berbeda dari', 'terdiri atas' vs 'terdiri dari') — butuh keputusan editorial, bukan salin-tempel. |
| 10 | `E_G` | Penulisan singkatan bertitik ('eg.' → 'e.g.') | rust | M (butuh kamus/daftar) | ✗ He has watched many action movies (eg. Fast & Furious)<br>✓ He has watched many action movies (e.g. Fast & Furious) | Ada padanan: PUEBI mengatur singkatan bertitik (dll., dsb., dst., yth., a.n.); pola deteksinya identik. |
| 11 | `MISSING_COMMA_BETWEEN_DAY_AND_YEAR` | Koma pada format tanggal | rust | M (butuh kamus/daftar) | ✗ My birthday is October 18 1983.<br>✓ My birthday is October 18, 1983. | PERLU DIBALIK: format Indonesia '18 Oktober 1983' TIDAK memakai koma. Rule ini berguna sebagai contoh bahwa format tanggal harus ditulis ulang, bukan diadopsi. |
| 12 | `OK_OK_COMMA` | Koma di antara unsur perincian/pengulangan | rust | S (butuh kamus/daftar) | ✗ Ok ok, you are right.<br>✓ Ok, ok, you are right. | Ada padanan: PUEBI mewajibkan koma di antara unsur perincian. |
| 13 | `FILLER_WORDS` | Kata pengisi/tak bermakna (ambang kepadatan per teks) | FE | S (regex saja) | _Filler words A rule that gives hints about the use of filler words_ | perlu-verifikasi daftarnya untuk Indonesia; mekanisme (hitung kepadatan kata pengisi) portabel. |
| 14 | `WORD_REPEAT_BEGINNING_RULE` | Beberapa kalimat berturut diawali kata yang sama | FE | S (regex saja) | _Successive sentences beginning with the same word Check if three successive sent_ | perlu-verifikasi: ini kaidah gaya, bukan PUEBI; tetapi mekanismenya bebas bahasa. |
| 15 | `STYLE_REPEATED_WORD_RULE` | Kata (lema) yang sama berulang di kalimat berdekatan | rust | L (butuh morfologi) | _Repeated words in consecutive sentences An abstract rule checks the appearance o_ | perlu-verifikasi: butuh lematisasi; di Indonesia berarti melucuti afiks (me-, ber-, -kan) dulu. |
| 16 | `SPACE_BEFORE_CONJUNCTION` | Spasi hilang sebelum konjungsi | FE | S (regex saja) | _Checks for missing space before some conjunctions An abstract rule that checks i_ | perlu-verifikasi daftar konjungsinya (dan, atau, tetapi, karena) — mekanismenya regex. |
| 17 | `EN_CONSISTENT_APOS` | Konsistensi jenis apostrof/petik dalam satu dokumen | FE | S (regex saja) | _Checks if the two types of apostrophes (' and ’) are used consistently in a text_ | Ada padanan: campuran petik lurus " dan petik lengkung “ ” lazim terjadi pada teks Indonesia. |

**P2 — bernilai tetapi bergantung POS (POS kita ~63% coverage & noisy → positif palsu).**

| # | ID LanguageTool | Pola | Lapis | Effort | Contoh (dari repo) | Relevansi Indonesia |
|---|---|---|---|---|---|---|
| 1 | `EN_QUOTES` | Petik lurus → petik tipografis | rust | L (butuh POS) | ✗ This is a "test".<br>✓ This is a “test”. | perlu-verifikasi kebijakan gaya kita (petik lengkung vs lurus); kaidah PUEBI memakai "...". |
| 2 | `THIRD_WORLD` | Kapitalisasi istilah majemuk yang bergantung konteks | rust | S (butuh POS) | ✗ Children living in third world countries deserve better healthcare.<br>✓ Children living in Third World countries deserve better healthcare. | perlu-verifikasi; contoh kelas rule yang butuh POS — tandai berisiko dengan POS kita. |

## 4. Yang sengaja TIDAK diusulkan

954 entri (33%) ditandai `skip`. Sebaran kategori terbesarnya:

| Kategori (skip) | Jumlah | % |
|---|---:|---:|
| Grammar | 620 | 65.0% |
| Plain English | 73 | 7.7% |
| Possible Typo | 51 | 5.3% |
| Style | 50 | 5.2% |
| Commonly Confused Words | 34 | 3.6% |
| Nonstandard Phrases | 24 | 2.5% |
| American English phrases | 19 | 2.0% |
| Proper Nouns | 16 | 1.7% |
| Punctuation | 13 | 1.4% |
| British English phrases | 9 | 0.9% |
| American English Style | 9 | 0.9% |
| Semantics | 8 | 0.8% |
| Java rule (core) | 8 | 0.8% |
| Compounding | 5 | 0.5% |
| Wikipedia | 4 | 0.4% |
| Typography | 3 | 0.3% |
| British English, Oxford spelling (~iz~ not ~is~) | 2 | 0.2% |
| Creative Writing | 2 | 0.2% |
| Java rule (en) | 2 | 0.2% |
| Collocations | 1 | 0.1% |
| Academic Writing | 1 | 0.1% |

Alasan tersering: artikel/determiner, kala & aspek verba, kesesuaian jumlah (singular/plural), posesif apostrof, dan perbedaan ejaan Britania–Amerika. Angka ini berguna sebagai kalibrasi harapan: **kalau ada yang mengusulkan "port saja rule LanguageTool", sepertiga isinya memang tidak punya padanan.**

## 5. Peringatan ketergantungan POS

1365 entri bergantung POS/morfologi; 784 di antaranya berada di kelompok direct/adapt. Dengan POS kita (~63% coverage, sebagian salah), rule seperti ini akan menghasilkan positif palsu. Dua siasat yang terbaca dari kode LanguageTool sendiri: (a) banyak rule memakai POS hanya sebagai **antipattern** — penekan positif palsu, bukan pemicu; (b) rule `AbstractWordCoherency` membuktikan bahwa **konsistensi** bisa diperiksa tanpa tahu bentuk mana yang benar. Keduanya jalan aman untuk kita.

## 6. Tagalog — modul serumpun (`out/rules_tl.jsonl`)

Tidak ada modul Indonesia, tetapi modul Tagalog (44 rule) menunjukkan kategori galat apa yang muncul pada bahasa Austronesia — jenis galat yang **tidak pernah muncul** di modul Inggris. Semua ditandai `adapt` (heuristik `skip` bahasa Inggris tidak berlaku di sini: 'plural' dan 'determiner' pada Tagalog berarti reduplikasi dan partikel).

| Kategori TL | n | Contoh dari repo | Kemungkinan analog Indonesia |
|---|---:|---|---|
| Adjective Plurality | 7 | ✗ Magaganda ako. → ✓ Magaganda kami. | perlu-verifikasi — jamak lewat reduplikasi, bukan sufiks. |
| Loan Words | 6 | ✗ Kumain kami ng cake kagabi. → ✓ Kumain kami ng keyk kagabi. | perlu-verifikasi — kata serapan; sangat relevan (KBBI punya bentuk serapan baku). |
| Alternation of D and R | 4 | ✗ Baka ang tingin mo ay talo ka din. → ✓ Baka ang tingin mo ay talo ka rin. | perlu-verifikasi — sejajar dengan morfofonemik peluluhan meN- (mem-, men-, meng-, meny-) yang sering salah ditulis. |
| Missing Lexical Marker | 4 | ✗ Si Maria maganda. → ✓ Si Maria ay maganda. | perlu-verifikasi — penanda wajib yang terlewat. |
| Ligature Usage | 3 | ✗ Baka ang tingin mo ay asul mata ang meron si → ✓ Baka ang tingin mo ay asul na mata ang meron | perlu-verifikasi — partikel penghubung; padanan terdekatnya 'yang'. |
| Ng and Nang | 3 | ✗ Kumain ng maayos. → ✓ Kumain nang maayos. | perlu-verifikasi — sepadan dengan pasangan fungsional homofon kita: 'di' preposisi vs 'di-' awalan, 'ke'/'ke-'. Ini kelas galat #1 penutur Indonesia. |
| Wrong Determiner | 3 | ✗ Pinalakad ni Maria si abogado. → ✓ Pinalakad ni Maria ang abogado. | perlu-verifikasi. |
| Word Repetition | 3 | ✗ Maganda ang ang tanawin. → ✓ Maganda ang mga tanawin. | Galat ketik universal + harus sadar reduplikasi sah. |
| Missing Last Word | 3 | — | perlu-verifikasi — kalimat terpotong. |
| Affix Usage | 2 | ✗ Ang dentista ay nagbunot ng ngipin. → ✓ Ang dentista ay bumunot ng ngipin. | perlu-verifikasi — pemilihan afiks (me-/ber-/ter-) adalah galat khas Indonesia. |
| False Friend | 2 | ✗ Kapag lagging kapiling kita, sumasaya ako. → ✓ Kapag laging kapiling kita, sumasaya ako. | perlu-verifikasi — 'teman palsu' Inggris–Indonesia. |
| Code-switching | 2 | ✗ Ano ba, where na kayo? → ✓ Ano ba, nasaan na kayo? | perlu-verifikasi — campur kode Inggris–Indonesia, gejala nyata pada teks kita. |
| Missing Determiner | 1 | ✗ Matalino Maria. → ✓ Matalino si Maria. | perlu-verifikasi. |
| Exchange Word Positions | 1 | ✗ Maganda Maria si. → ✓ Maganda si Maria. | perlu-verifikasi — urutan kata. |

> Nilainya bukan rule-nya (Tagalog ≠ Indonesia), melainkan **daftar kategori galat**: afiksasi, morfofonemik, partikel, reduplikasi, serapan, campur kode. Tidak satu pun kategori ini ada di 2.800+ rule Inggris. Kalau kita membangun taksonomi galat sendiri, ini titik awal yang jauh lebih tepat daripada modul Inggris.

## 7. Cara membaca `rules.jsonl`

Satu baris = satu entri. Kolom skema: `id`, `name`, `category`, `what_it_catches`, `example_wrong`, `example_correct`, `mechanism`, `portability`, `target_layer`, `id_relevance`, `effort`. Kolom tambahan untuk audit: `why` (alasan klasifikasi), `pos_risk`, `n_variants` (berapa `<rule>` digabung), `n_tokens`, `message`, `source_file`.

```bash
# semua rule yang langsung bisa dikerjakan FE
jq -c 'select(.portability=="direct" and .target_layer=="FE")' out/rules.jsonl

# cetakan mekanisme untuk editor-rust, ongkos kecil
jq -c 'select(.portability=="adapt" and .effort!="L")' out/rules.jsonl

# lihat alasan klasifikasi sebuah rule
jq -c 'select(.id=="ELLIPSIS")' out/rules.jsonl
```

Reproduksi: `bash fetch.sh && python3 lt_mine.py build && python3 make_summary.py` (tanpa dependensi eksternal; entity DTD LanguageTool diperluas manual di `lt_mine.py`).

### Batas yang perlu diketahui

- Klasifikasi `portability`/`effort` dihasilkan heuristik (kata kunci + analisis atribut `<token>`), bukan pembacaan manual 2.909 rule. Kolom `why` mencatat alasannya agar bisa dibantah per baris.
- Rule Java tidak punya `<example>`; contohnya diambil dari javadoc bila ada, sehingga sebagian kosong.
- `remote-rule-filters.xml` (filter rule berbasis model jarak jauh) sengaja dilewati.
- Relevansi Indonesia hanya dinyatakan tegas untuk kaidah yang memang diatur PUEBI (kapital, koma perincian, tanda hubung/pisah, elipsis, angka); selebihnya `perlu-verifikasi` dan harus dicek ke PUEBI/KBBI sebelum diimplementasi.
