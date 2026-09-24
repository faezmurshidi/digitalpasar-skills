---
name: digitalpasar
description: Operate a DigitalPasar shop through its MCP tools and understand how the platform works — pricing, payouts, delivery, page authoring. Use when the user has a DigitalPasar shop (digitalpasar.xyz) and wants their assistant to manage, improve or explain it.
---

## Sambungan

Kedai DigitalPasar disambung melalui MCP. Penjual cipta kunci di dashboard
(Settings), kemudian:

```
claude mcp add --transport http digitalpasar https://www.digitalpasar.xyz/api/mcp --header "Authorization: Bearer dp_live_..."
```

Kunci berskop: ia boleh baca kedai, produk dan analytics, dan tulis halaman.
Ia TIDAK boleh baca email atau pesanan pembeli — jangan minta, memang tiada.

## Tujuh tools

- `dp_get_template_contract` — vocabulary halaman yang sah. **Panggil ini
  dulu setiap sesi menulis.** Jangan tulis dari ingatan; contract berubah.
- `dp_get_store` — nama, tagline, bahasa kedai, dan konteks onboarding
  (about, audience, tone). Guna ini, jangan tanya penjual benda yang
  platform dah tahu.
- `dp_list_products` — produk dengan harga dan stok semasa, serta flag
  `available` / `soldOut` yang renderer sendiri guna.
- `dp_get_analytics` — pelawat, checkout, jualan, conversion. Ada senarai
  variant A/B yang pernah dihidang; panggil semula dengan satu nama variant
  untuk banding.
- `dp_validate_page` — semak halaman. Ralat datang dengan baris dan lajur.
- `dp_preview_page` — render dengan produk sebenar, tanpa terbit.
- `dp_publish_page` — simpan. Server semak semula dan MENOLAK halaman yang
  gagal, walau apa pun validate kata sebelum ini.

## Peraturan yang server kuatkuasa

- Halaman jalan bawah `script-src 'none'` — tiada JavaScript, tiada
  `<script>`, borang biasa sahaja. Checkout ialah form POST platform.
- `publish: true` menjadikan kedai boleh dilihat pembeli. Tanya penjual
  dulu; itu keputusan mereka.
- Menukar nama variant ujian A/B yang sedang berjalan DITOLAK melainkan
  `allowVariantChange: true` — kerana attribution jualan menunggang nama
  itu. Tanya penjual sebelum guna.
- Kunci test (`dp_test_`) boleh validate dan preview, tak boleh publish.

## Cara platform berfungsi (fakta untuk menjawab soalan penjual)

- **Harga:** RM10 sebulan, 10% setiap jualan. Contoh: jualan RM49 — yuran RM 4.90,
  penjual terima RM 44.10.
- **Harga produk minimum RM5** — bawah itu kos pemprosesan makan
  semua margin.
- **Payout:** penjual minta bila-bila dari dashboard; sampai dalam 48 jam.
  Minimum RM10. Baki yang dipaparkan sudah bersih selepas yuran.
- **Checkout:** DuitNow QR di desktop; telefon terus ke halaman bayaran
  hosted (FPX, kad, e-wallet). Pembeli tak perlu akaun.
- **Penghantaran automatik:** fail, link atau kunci lesen sampai ke email
  pembeli sejurus bayaran disahkan. Tukar fail — pembeli lama pun dapat
  versi baru melalui link mereka.
- **Refund:** platform yang urus, bukan penjual. Tempoh refund 7 hari dari
  jualan.
- **Bahasa kedai** (`ms` atau `en`) menentukan bahasa checkout. Tulis
  halaman dalam bahasa kedai.

## Menulis halaman yang bagus

Baca jawapan onboarding penjual dari `dp_get_store` dan tulis dengan suara
mereka, bukan suara generik. Guna nama produk dan harga sebenar dari
`dp_list_products` — halaman yang dihidang ialah apa yang anda tulis,
bukan placeholder. Selepas terbit, baca `dp_get_analytics` seminggu
kemudian dan baiki berdasarkan angka: ramai pelawat tapi tiada checkout
bermakna tawaran tak jelas; checkout tanpa jualan bermakna harga atau
kepercayaan.
