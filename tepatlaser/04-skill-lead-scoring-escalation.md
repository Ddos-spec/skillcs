# Skill 04 — Lead Scoring dan Escalation Tepat Laser

## Tujuan
Menentukan status lead dan kapan AI harus menyerahkan percakapan ke admin manusia.

## Lead status
### cold
Gunakan bila:
- customer hanya menyapa,
- kebutuhan belum jelas,
- pertanyaan tidak relevan,
- belum ada indikasi order.

### warm
Gunakan bila:
- customer menanyakan layanan atau kisaran harga,
- produk sudah mulai jelas tetapi detail belum cukup,
- material atau ukuran masih belum diketahui,
- customer masih membandingkan atau eksplorasi.

### hot
Gunakan bila salah satu kondisi berikut terpenuhi:
- customer sudah mengirim file/gambar,
- customer meminta quotation final,
- kebutuhan, material, ukuran, dan jumlah cukup jelas,
- customer menyebut siap order atau minta diproses,
- customer meminta survey, produksi, instalasi, atau jadwal pasti.

### not_lead
Gunakan bila:
- spam,
- penawaran vendor yang tidak relevan,
- lowongan kerja,
- percakapan pribadi/nonbisnis,
- kebutuhan jelas di luar scope dan tidak ada alternatif.

## trigger_conversion
- true hanya saat lead_status = hot.
- false untuk cold, warm, dan not_lead.
- Jika gclid tidak tersedia, field tetap false meskipun lead hot bila workflow mensyaratkan gclid untuk conversion.

## Escalate = true
Set true bila:
- file/gambar sudah diterima,
- customer meminta harga final atau penawaran resmi,
- customer meminta analisis gambar/desain,
- pekerjaan kompleks: signage, huruf timbul, facade, pagar, railing, booth, instalasi, finishing, welding, atau assembly,
- material/ketebalan di luar knowledge,
- customer bingung teknis dan perlu rekomendasi manusia,
- customer komplain, marah, atau meminta penanganan khusus,
- customer menanyakan order lama yang tidak tersedia di context,
- butuh kepastian stok, jadwal, ongkir, instalasi, atau kemampuan mesin.

## Jangan escalate terlalu cepat
Jangan eskalasi hanya karena:
- customer baru menyapa,
- customer menanyakan apakah layanan tersedia,
- detail penting masih bisa ditanyakan AI,
- customer hanya meminta kisaran umum yang tersedia di knowledge.

## Data minimum sebelum eskalasi normal
Idealnya tersedia:
- nama atau identitas customer,
- lokasi,
- produk/kebutuhan,
- material,
- ukuran atau file.

Namun attachment, komplain, atau permintaan quotation final boleh langsung dieskalasi meskipun data belum lengkap.

## reason
Reason harus singkat dan konkret.
Contoh:
- `Customer sudah mengirim file dan meminta quotation.`
- `Kebutuhan facade termasuk pekerjaan kompleks dan perlu pengecekan admin.`
- `Customer masih eksplorasi material dan belum siap quotation.`
