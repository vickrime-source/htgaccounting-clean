# HTG Accounting — AI Studio Cleanup

Tujuan cleanup ini adalah mengurangi beban context/crawler coding agent tanpa mengubah flow bisnis aplikasi.

## Perubahan utama

- Menghapus `inspect_report.html` yang memiliki baris Base64 sangat besar.
- Menghapus folder hasil inspeksi/ekstraksi/intermediate: `inspect_*`, `extracted_assets`, `optimized_assets`, `final_assets`.
- Menghapus DOCX root yang tidak direferensikan source (`temp_prohe.docx`, `docx_*.docx`).
- Menghapus asset PNG/JPG legacy/duplikat di `public/logos` dan asset PROHE lama yang tidak direferensikan.
- Menghapus `src/lib/proheAssets.ts` yang menyimpan image Base64 inline.
- Memindahkan seluruh logo/stempel/tanda tangan yang sebelumnya Base64 inline menjadi file binary normal di `public/store-assets/`.
- Mengubah `StoreProfile` dari field `*Base64` menjadi `*Src` dan mengupdate seluruh consumer yang ditemukan:
  - `src/lib/htmlInvoicePdf.ts`
  - `src/components/InvoiceModal.tsx`
- Menambahkan generated/temporary paths ke `.gitignore`.

## Asset runtime baru

Semua asset yang sebelumnya embedded tetap dipertahankan byte-for-byte, hanya media penyimpanannya yang berubah ke file terpisah:

- `/store-assets/htg-logo.png`
- `/store-assets/common-signature.png`
- `/store-assets/htg-stamp.png`
- `/store-assets/luweng-boga-logo.png`
- `/store-assets/luweng-boga-stamp.png`
- `/store-assets/lumbung-adifruta-logo.png`
- `/store-assets/lumbung-adifruta-signature.png`
- `/store-assets/lumbung-adifruta-stamp.png`
- `/store-assets/prohe-logo.png`
- `/store-assets/prohe-stamp-signature.png`

## Validasi

- Tidak ada lagi `data:image/...;base64` di source project.
- Tidak ada lagi referensi source ke field lama `logoBase64`, `signatureBase64`, `stampBase64`, atau `stampSignatureCombinedBase64`.
- Tiga file yang dimodifikasi (`storeProfiles.ts`, `htmlInvoicePdf.ts`, `InvoiceModal.tsx`) lolos syntax parse TypeScript/TSX.
- Full `tsc --noEmit` belum dapat dijadikan validasi final di environment cleanup karena dependency project belum ter-install. Error yang muncul adalah missing modules/types, bukan syntax error dari perubahan cleanup.

## Catatan

Jika project dijalankan setelah clone/upload baru, install dependency sesuai `package.json` terlebih dahulu. `bun.lock` dari ZIP asli memang kosong dan tidak diubah oleh cleanup ini.
