# Portal Layanan Akademik – Fakultas Sains dan Teknologi, Universitas Pattimura

Tugas Pertemuan 5: *Responsive & Accessible Web Client*. Satu halaman statis (HTML + CSS, tanpa JavaScript), tema oranye FST, berjalan di Google Chrome dan Microsoft Edge.

## Cara membuka

1. Ekstrak zip (klik kanan → **Extract All**). Jangan buka dari dalam zip.
2. Buka folder `portal-layanan-akademik`, lalu klik dua kali **`index.html`** (atau klik kanan → Open with → Chrome / Edge).

Tidak perlu server atau `localhost`. Jika folder `css/` atau `img/` terpisah dari `index.html`, tampilan akan rusak, jadi biarkan struktur folder utuh. Cadangan satu-file yang tidak butuh folder lain ada di `alternatif/index-satu-file.html`.

## Struktur folder

```
portal-layanan-akademik/
├── index.html                 halaman utama
├── css/
│   └── styles.css             seluruh gaya (mobile-first)
├── img/
│   ├── gedung-unpatti.jpg     foto gedung (halaman depan)
│   └── logo-unpatti.png       logo Universitas Pattimura
├── evidence/                  bukti pengujian DevTools
│   ├── mobile-360-full.png / mobile-360-atas.png
│   ├── tablet-768-full.png / tablet-768-atas.png
│   ├── desktop-1280-full.png / desktop-1280-atas.png
│   ├── fokus-keyboard-nav.png / fokus-keyboard-form.png
│   ├── form-validasi-error.png / form-terkirim.png
│   └── hasil-audit.json       data mentah pengujian
├── alternatif/
│   └── index-satu-file.html   versi mandiri (CSS + gambar tertanam)
└── README.md
```

## Keputusan desain

| Keputusan | Alasan |
|---|---|
| Tema gelap bergaya "lab/blueprint": latar kertas milimeter, label monospace | Sesuai karakter fakultas sains dan teknologi, dan tidak terlihat seperti template umum. |
| Palet: cokelat gelap `#120D09`, oranye `#FF8A3D` (oranye FST), krem `#FFF4E8` | Oranye di atas gelap berkontras ≈ 8:1; teks gelap di atas tombol oranye ≈ 8:1; form berlatar krem agar kontrol terbaca jelas. |
| Foto gedung menyatu dengan latar lewat fade (desktop: sisi kanan hero; mobile: di atas judul) | Foto menjadi pusat halaman depan tanpa menutupi teks. |
| Font sistem, tanpa webfont/CDN | Tanpa request eksternal; cepat dan tidak ada resource gagal. |
| Mobile-first, 2 breakpoint: `40em` (640 px) dan `62em` (992 px) | Dipilih saat konten mulai lapang (form 2 kolom; hero & form berdampingan), bukan berdasarkan nama perangkat. |
| Grid untuk kartu/form/alur, Flexbox untuk header, nav, chip, tombol | Sesuai kebutuhan layout tiap bagian. |
| Radio dalam `fieldset/legend` untuk jenis layanan | Pilihan tunggal 4 opsi, ter-grouping jelas bagi pembaca layar. |
| Submit via `method="get"` ke `#terkirim` | Tanpa JS: validasi bawaan browser tetap berjalan, pesan sukses tampil lewat `:target`. Halaman ini demo; data tidak dikirim ke server. |

## Memenuhi spesifikasi tugas

| No | Komponen | Implementasi |
|---|---|---|
| 1 | Struktur semantik | `header`, `nav` (berlabel), `main`, `section` (berlabel), `article` (kartu), `footer` |
| 2 | Kartu layanan | 4 kartu; masing-masing `h3` + link bernama jelas |
| 3 | Form | 7 kolom: nama, NIM, email, program studi, jenis layanan, keperluan, persetujuan; label terasosiasi; tipe input tepat; validasi HTML; `<button type="submit">` |
| 4 | Responsive | Mobile-first; diuji 360, 768, 1280 px |
| 5 | Tanpa overflow | Lihat tabel pengujian |
| 6 | Keyboard | Semua kontrol terjangkau Tab; fokus terlihat (outline 3px) |
| 7 | Gambar | Logo dan foto gedung informatif dengan `alt` bermakna; ornamen (garis, pola, panah `→`, label `LAYANAN / 0x`) bukan `<img>` dan disembunyikan dari pembaca layar (`aria-hidden`/CSS) |
| 8 | CSS layout | Grid dan Flexbox |
| 9 | DevTools | Lihat bagian pengujian |

## Aksesibilitas

- Heading logis: 1× `h1`, `h2` per bagian, `h3` untuk kartu, langkah, dan tips.
- Skip link "Lewati ke konten utama" sebagai fokus pertama.
- Fokus terlihat dan kontras sengaja dirancang per latar (oranye di latar gelap; garis gelap + halo oranye di kartu form).
- Link kartu bernama jelas ("Ajukan Surat Aktif Kuliah"), bukan "Selengkapnya".
- Setiap kontrol form punya `<label for>`; hint dihubungkan lewat `aria-describedby`; `autocomplete` untuk nama dan email; `inputmode="numeric"` untuk NIM.
- Elemen native lebih dulu daripada ARIA.
- Menghormati `prefers-reduced-motion`; ada dukungan `forced-colors` dan gaya cetak.

## Bukti pengujian (Browser DevTools)

Pengujian dijalankan dengan Chromium otomatis (Playwright), mesin yang sama dengan Chrome dan Edge; data mentah di `evidence/hasil-audit.json`. Disarankan mengulang langkah yang sama secara manual dan melampirkan capture sendiri.

### 1. Responsive Mode (F12 → Ctrl+Shift+M)

| Viewport | scrollWidth / clientWidth | Elemen di luar tepi | Hasil |
|---|---|---|---|
| 360 px | 360 / 360 | 0 | Lulus |
| 768 px | 768 / 768 | 0 | Lulus |
| 1280 px | 1280 / 1280 | 0 | Lulus |

### 2. Elements / Inspector
DOM berisi landmark semantik, tidak ada `id` ganda. Panel *Computed* dipakai memeriksa box model (`box-sizing: border-box`), lebar maksimum konten 72rem, dan breakpoint aktif di 640 px dan 992 px.

### 3. Network (reload, Disable cache)

| Resource | Status |
|---|---|
| `index.html` | 200 |
| `css/styles.css` | 200 |
| `img/logo-unpatti.png` | 200 |
| `img/gedung-unpatti.jpg` | 200 |

Tidak ada request gagal dan tidak ada error/warning di Console.

### 4. Accessibility
- 10 kontrol form, masing-masing tepat 1 label terasosiasi.
- Urutan Tab: skip link → brand → nav → CTA → 4 link kartu → nama → NIM → email → prodi → jenis layanan → keperluan → persetujuan → kirim → footer.
- Screenshot: `evidence/fokus-keyboard-nav.png`, `evidence/fokus-keyboard-form.png`.

### 5. Validasi form

| Skenario | Hasil |
|---|---|
| Nama 2 huruf, email `salah-email`, NIM `12ab` | `valid=false`; email `typeMismatch` dan NIM `patternMismatch` terdeteksi (`evidence/form-validasi-error.png`) |
| Semua kolom benar | `checkValidity()=true`; pesan sukses tampil (`evidence/form-terkirim.png`) |

## Temuan debugging

1. **Paragraf hero menabrak foto di desktop.** Penyebab: foto dimulai di 34% lebar dan teks setebal 36rem. Perbaikan: foto dimulai di 50%, lebar teks dibatasi 31rem, judul 34rem.
2. **Label "Fakultas Sains…" menimpa foto di mobile.** Penyebab: tumpang tindih negatif terlalu besar (-3.5rem). Perbaikan: -1.25rem.
3. **Menu mobile turun ke baris kedua.** Penyebab: padding tautan terlalu lebar untuk 360 px. Perbaikan: padding dikecilkan di mobile dan dikembalikan di ≥ 40em.

## Catatan konten

Isi layanan, estimasi waktu, persyaratan, daftar program studi, dan jam loket adalah **contoh**; sesuaikan dengan ketentuan resmi fakultas. Papan nama pada foto gedung bertuliskan fakultas lain; ganti `img/gedung-unpatti.jpg` jika ingin foto gedung FST.
