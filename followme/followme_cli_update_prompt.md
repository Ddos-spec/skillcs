# Prompt untuk CLI AI — Update Followme CS Knowledge & Workflow

Tugas: update repository dan workflow customer service Followme supaya memakai knowledge terbaru dari klien.

## Context
Repo target:
- Owner: Ddos-spec
- Repo: skillcs
- Folder: `followme/`

File baru yang harus dibuat/diupdate:
1. `followme/followme_product_knowledge_v3.md`
2. `followme/followme_faq_cs_handling.md`

File existing yang sudah ada dan tetap dipakai:
- `followme/followme_brand_info_knowledge.md`
- `followme/followme_classic_series_knowledge.md`
- `followme/followme_occasion_series_knowledge.md`
- `followme/followme_exclusive_series_knowledge.md`
- `followme/followme_price_list_event_jagat_aroma.md`
- `followme/fallback.md`

## Yang Perlu Diupdate

### 1. Tambah knowledge product master
Tambahkan file `followme_product_knowledge_v3.md`.

Isi file ini berisi 18 varian terbaru dari Product Knowledge klien:
Aeros, Basil Breeze, Nutty Whispers, Cartivage, Tonka Dusk, Veldran, L'Ame de L'Ocean, Senso Di Blossom, Sunset Flavor, Glorious Moonlight, Tobacco Noir, Amethyst, Poetic Suede, Stormchaser, Happiness, Love Oud, Brotherhood Story, Amber Vougere.

Aturan penting:
- Mayoritas varian adalah Extrait 12 jam.
- Tobacco Noir adalah EDP 8 jam.
- Jangan klaim ketahanan pasti sama di semua orang.
- Magic Of Nature dan Ombre muncul di FAQ, tapi tidak ada di Product Knowledge Sheet 1 terbaru. Tandai sebagai needs verification jika ditanya detail notes.

### 2. Tambah FAQ CS handling
Tambahkan file `followme_faq_cs_handling.md`.

File ini berisi:
- FAQ ketahanan aroma.
- FAQ aroma berubah/beda di kulit.
- FAQ unisex terasa maskulin/feminin.
- Handling aroma terlalu manis/maskulin/feminin/strong/soft.
- Handling salah varian, produk bocor, spray macet, barang rusak.
- Handling kulit gatal/panas/pusing.
- Handling refund, retur, cancel, barang kurang, resi, alamat salah.
- FAQ rekomendasi parfum berdasarkan gender, aroma, occasion, daily, date, formal, outdoor, blind buy.

### 3. Update workflow n8n
Cari workflow Followme WhatsApp customer service JSON.

Tambahkan 2 GitHub Tool node baru ke AI Agent:

A. Node `Get product_knowledge_v3`
- Type: `n8n-nodes-base.githubTool`
- Owner: `Ddos-spec`
- Repository: `skillcs`
- File path: `followme/followme_product_knowledge_v3.md`
- Tool description: `Get latest Followme product master knowledge: variant, perfume type, aroma character, notes, longevity, occasion, recommendation mapping, and naming aliases.`

B. Node `Get faq_cs_handling`
- Type: `n8n-nodes-base.githubTool`
- Owner: `Ddos-spec`
- Repository: `skillcs`
- File path: `followme/followme_faq_cs_handling.md`
- Tool description: `Get Followme FAQ and customer service handling for complaints, fragrance objections, after-sales issues, recommendation FAQs, and safe response templates.`

Pastikan credential GitHub sama seperti tool lain yang sudah ada.
Jangan mengubah credential existing.
Jangan menghapus tools lama.
Pastikan workflow tetap valid JSON dan importable di n8n.

### 4. Update AI Agent System Message
Tambahkan aturan berikut:

- Untuk notes, jenis parfum, karakter aroma, ketahanan, dan occasion terbaru, prioritaskan `Get product_knowledge_v3`.
- Untuk harga event, gunakan `Get price_list_event`.
- Untuk pertanyaan/komplain/keberatan customer, gunakan `Get faq_cs_handling` dan/atau `Get fallback_handling`.
- Jangan mengarang harga, stok, ongkir, rekening, status pembayaran, atau kebijakan retur yang tidak ada di knowledge.
- Untuk keluhan salah varian/barang tertukar/bocor/rusak/spray macet/barang kurang, selalu minta nomor pesanan + foto/video bukti.
- Untuk aroma tidak cocok, jelaskan bahwa aroma sangat personal dan tawarkan rekomendasi varian lain.
- Untuk kulit panas/gatal/pusing, sarankan hentikan pemakaian dulu; jangan memberi diagnosis medis.
- Untuk ketahanan, pakai bahasa aman: `dirancang sampai`, `bisa berbeda di tiap orang`, bukan klaim pasti.
- Untuk Magic Of Nature dan Ombre, jangan halu detail notes dari Product Knowledge v3. Jika knowledge lama tersedia boleh gunakan, kalau tidak bilang detail lengkapnya perlu dicek kembali.

### 5. Update Build Mode Context
Tambahkan intent keyword:
- `salah varian`, `ketukar`, `barang salah`, `produk salah`
- `bocor`, `pecah`, `rusak`, `lecet`, `spray macet`
- `barang kurang`, `paket kurang`, `belum sampai`, `resi`, `alamat salah`
- `refund`, `retur`, `cancel`, `tukar`
- `gatal`, `panas`, `pusing`, `menyengat`
- `nggak awet`, `cepat hilang`, `tahan lama`
- `nggak cocok`, `zonk`, `kecewa`, `beda dari deskripsi`

Jika keyword ini muncul, set mode/context ke `after_sales_or_faq` dan arahkan AI untuk memakai `Get faq_cs_handling`.

### 6. Update Parse AI JSON Fallback
Jika AI gagal parse JSON dan latest_message mengandung komplain:
- salah varian/ketukar → jawab minta foto produk + nomor pesanan.
- bocor/rusak/spray macet → minta foto/video paket + produk + nomor pesanan.
- aroma tidak cocok → empati + jelaskan aroma personal + tawarkan rekomendasi lain.
- cepat hilang/nggak awet → jelaskan faktor kulit/cuaca/aktivitas/cara pemakaian.
- gatal/panas/pusing → sarankan hentikan pemakaian dulu.

### 7. Commit Message
Gunakan commit message:
`docs: add latest Followme product knowledge and FAQ handling`

## Acceptance Criteria
Workflow dan knowledge dianggap berhasil kalau AI bisa menjawab test case ini:

1. `Tobacco Noir tahan berapa lama?`
Expected: EDP, sekitar 8 jam, bisa berbeda tiap orang.

2. `Saya beli Aeros tapi yang datang Veldran`
Expected: minta maaf, minta foto produk yang diterima + nomor pesanan, bantu cek kesesuaian.

3. `Parfumnya bocor pas diterima`
Expected: minta maaf, minta foto/video paket, botol, resi/nomor pesanan.

4. `Saya suka fresh tapi nggak mau terlalu strong`
Expected: rekomendasi Basil Breeze, Amethyst, Amber Vougere, Veldran, atau Stormchaser dengan alasan singkat.

5. `Magic Of Nature notesnya apa?`
Expected: jawab aman karena Magic Of Nature tidak ada di Product Knowledge v3 terbaru dan perlu cek detail lanjutan, kecuali knowledge lama masih tersedia.
