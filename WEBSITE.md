# WEBSITE.md: Membangun Website Portofolio dari Desain FigCraft

**Tugas:** ubah desain di FigCraft (frame `Desktop` dan `Mobile`) menjadi website portofolio satu halaman di project Next.js ini, lengkap dengan interaksi dan animasi scroll. Website diekspor sebagai HTML statis: tidak ada database dan tidak ada server khusus.

**Urutan kerja (kerjakan semuanya tanpa berhenti di tiap fase):**
1. Persiapan dan membaca desain (bagian 1).
2. Fondasi: konfigurasi statis, token, font, struktur folder, konstanta (bagian 2 dan 3).
3. Bangun semua seksi sesuai desain (bagian 4).
4. Interaksi dan fitur (bagian 5).
5. Animasi dan efek scroll (bagian 6).
6. Aksesibilitas, SEO, performa (bagian 7).
7. Pengecekan akhir dan laporan (bagian 8).

**Cara kerja (wajib):**
- Baca frame `Desktop` dan `Mobile` di FigCraft satu kali di awal, catat hasilnya secara ringkas (teks, warna, ukuran, posisi tiap seksi), lalu bekerja dari catatan itu. Jangan membaca ulang canvas berulang kali. Koneksi ke FigCraft punya batas 60 detik per request, jadi baca per frame atau per seksi, bukan seluruh dokumen sekaligus kalau dokumennya besar.
- Tulis banyak file sekaligus dalam satu tool call (misalnya semua hook dan konstanta dalam satu langkah, lalu semua komponen satu seksi dalam satu langkah). Jangan membaca ulang file yang baru ditulis kecuali ada error.
- Jalankan `npm run lint` dan `npm run build` satu kali di akhir. Ulangi hanya kalau ada error.
- Jangan menjalankan dev server atau mengambil screenshot kecuali perlu untuk memperbaiki masalah.

## 1. Persiapan dan Acuan Desain
- Baca `AGENTS.md` di root project dan ikuti aturannya. Cek `package.json` untuk versi Next.js, React, dan Tailwind yang terpasang, lalu pakai cara penulisan yang sesuai versi itu.
- **Acuan utama adalah desain di canvas FigCraft.** Pakai tool `figcraft-app` untuk membaca frame `Desktop` dan `Mobile` beserta isinya: urutan elemen, teks, warna, font, ukuran, jarak, radius, posisi, dan gambar yang dipakai. Kalau tool punya fitur export kode atau inspect, pakai sebagai referensi, tetapi tulis ulang kodenya dengan rapi, jangan menempel mentah.
- **Acuan pelengkap adalah `DESIGN.md`** (token di bagian 1 dan tata letak di bagian 2). Kalau canvas dan `DESIGN.md` berbeda, canvas yang menang, karena itu hasil akhir yang sudah disetujui. Teks yang tampil di website harus sama dengan teks di canvas (termasuk nama pemilik yang tertulis di desain). Satu pengecualian: tinggi Hero mengikuti aturan "Layar pertama" di bagian 4, bukan angka tinggi di canvas.
- Kalau tool `figcraft-app` gagal atau tidak bisa membaca frame, tampilkan error aslinya, lalu lanjut memakai `DESIGN.md` sebagai spesifikasi dan sebutkan hal itu di laporan akhir.
- Jangan mengubah, memindahkan, atau menghapus: `DESIGN.md`, folder `referensi/`, folder `public/`, dan `AGENTS.md`.
- Jangan menjalankan `git commit` atau `git push`.

## 2. Struktur Folder dan Aturan Kode
**Konfigurasi ekspor statis (kerjakan lebih dulu):**
- Di file config Next.js (`next.config.ts`, `.mjs`, atau `.js`, sesuai yang ada di project), set `output: 'export'` dan `images: { unoptimized: true }`. Hasil build statis ada di folder `out/`.
- Jangan memakai fitur yang butuh server Node: Server Actions (`'use server'`), API route, middleware, `cookies()` atau `headers()`, rewrites dan redirects di config, serta rute dinamis tanpa `generateStaticParams`. Semua logika (validasi, pengiriman form) berjalan di browser.
- `next/image` tetap dipakai untuk markup, `alt`, `sizes`, dan `priority`, tetapi optimasi otomatis tidak aktif, jadi file di `public/` disajikan apa adanya. Jangan mengubah file di `public/`; kalau ada gambar yang sangat besar, catat di laporan akhir.

Struktur folder yang harus dibuat:
- **`app/`**
  - `layout.tsx`: font, metadata, bahasa, struktur dasar
  - `page.tsx`: menyusun semua seksi
  - `globals.css`: design token dan gaya global
- **`components/`**
  - `layout/`: `Navbar`, `MobileMenu`, `Footer`, `ScrollProgress`, `BackToTop`
  - `sections/`: `Hero`, `Projects`, `About`, `Contact`
  - `ui/`: `Reveal`, `MaskedText`, `ProjectCard`, `ProjectModal`, `ContactForm`, `SocialLinks`, `Toast`, `icons` (SVG inline)
- **`hooks/`**: `useInView`, `useScrollSpy`, `useScrollDirection`, `useParallax`, `useReducedMotion`
- **`constants/`**: `siteConfig`, `navigation`, `projects`, `skills`, `animation`
- **`utils/`**: `buildMailtoUrl`, `formatPhoneForWhatsapp` (fungsi murni, tanpa UI)
- **`validations/`**: `contactSchema` (Zod)
- **`actions/`**: `submitContact` (fungsi biasa yang berjalan di browser, bukan Server Action)

Aturan kode:
- Satu komponen satu tanggung jawab. Jangan campur tampilan, validasi, dan logika kirim dalam satu komponen.
- Nama: camelCase untuk fungsi dan variabel, PascalCase untuk komponen dan file komponen.
- Semua nilai tetap (nama, email, nomor, tautan sosmed, daftar menu, daftar proyek, daftar skill, durasi animasi) ditaruh di `constants/`. Jangan menulis ulang di dalam komponen.
- TypeScript strict. Hindari `any`. Beri komentar singkat (1 sampai 2 baris) pada fungsi dan logika penting.
- Fungsi yang bisa gagal mengembalikan bentuk yang konsisten: `{ data, error }`.
- Semua `async` memakai try/catch. Setiap proses kirim punya state loading, success, dan error.
- Hapus kode mati (import, variabel, dan komponen yang tidak dipakai), termasuk sisa template bawaan `create-next-app`.
- Jangan menambah library besar untuk fungsi kecil. Library yang boleh ditambah: `zod`. Ikon dibuat sebagai SVG inline sendiri. Animasi dibuat dengan CSS dan hook sendiri (IntersectionObserver dan `requestAnimationFrame`), tanpa library animasi.

## 3. Token, Font, dan Gaya Global
- Pindahkan semua token dari `DESIGN.md` bagian 1 ke `app/globals.css` (di blok `@theme`, atau cara yang sesuai versi Tailwind yang terpasang): warna, ukuran teks, radius, dan font. Komponen memakai token, tidak menulis hex atau ukuran piksel baru secara langsung.
- Satu-satunya token tambahan yang boleh dibuat: `error` (misalnya `#F87171`), hanya untuk pesan kesalahan form.
- Font: `Plus Jakarta Sans` lewat `next/font/google` (weight 300, 400, 500, 600, 700). Tidak perlu download manual.
- Gaya global: `color-scheme: dark`, background `#0C0C0C`, teks putih, `scroll-behavior: smooth`, `overflow-x: clip` pada body (tidak boleh ada scroll horizontal), `scroll-margin-top` pada tiap seksi agar tidak tertutup navbar.
- **Ukuran teks responsif:** tiap ukuran besar memakai `clamp()` yang berinterpolasi linear dari nilai Mobile (lebar 390) ke nilai Desktop (lebar 1440), dengan batas atas nilai Desktop:
  - Judul Hero: 72 px menjadi 144 px
  - Judul "Proyek": 72 px menjadi 208 px
  - Judul "Hubungi Saya": 48 px menjadi 208 px (harus tetap satu baris dan tidak melebar keluar layar)
  - Kalimat besar Tentang: 40 px menjadi 48 px
  - Email dan nomor di Kontak: 24 px menjadi 48 px
- **Breakpoint:** layout Desktop dipakai mulai lebar 1024 px (ukuran skala mengikuti lebar layar). Di bawah 1024 px memakai layout Mobile (satu kolom). Isi dibatasi lebar maksimal 1344 px dan dipusatkan; latar Hero dan Footer tetap penuh selebar layar.

## 4. Seksi (ikuti desain di canvas)
ID seksi untuk navigasi: `beranda` (Hero), `proyek`, `tentang`, `kontak`. Urutan di halaman: Navbar, Hero, Proyek, Tentang, Kontak, Footer.
**Layar pertama (wajib, mengalahkan tinggi Hero di canvas):** di semua ukuran layar (HP, tablet, laptop, monitor besar), yang terlihat saat halaman pertama dibuka hanyalah Navbar dan Hero. Seksi Proyek tidak boleh terlihat sedikit pun di tepi bawah layar sebelum pengunjung scroll.
- Tinggi Hero sama dengan tinggi layar: `min-height: 100vh` sebagai cadangan, lalu `min-height: 100svh` (dan `100dvh` kalau didukung) supaya tidak terpengaruh bilah alamat browser HP. Jangan memakai tinggi tetap 960 px, persentase lebar layar, atau `max-height`. Angka 960 px di canvas hanyalah contoh untuk lebar 1440, bukan aturan untuk website.
- Navbar tidak menambah tinggi: ia menumpuk di atas Hero (`position: absolute` atau `fixed`), jadi Hero tetap setinggi satu layar, bukan satu layar ditambah navbar.
- Isi Hero disusun dengan flexbox kolom: teks dan foto mengikuti tinggi layar, dan empat keahlian menempel di dasar Hero (padding bawah 48 px, 32 px di Mobile). Kalau layar sangat pendek sehingga isi Hero lebih tinggi dari layar (misalnya HP posisi landscape), Hero boleh tumbuh mengikuti isi (pakai `min-height`, bukan `height`) supaya tidak ada teks terpotong atau saling tumpuk.
- Ukuran judul Hero juga mempertimbangkan tinggi layar: `clamp(4.5rem, min(10vw, 18vh), 9rem)`, agar judul dua baris dan isi lainnya tetap muat di layar yang pendek.
- Foto Hero memakai `object-fit: cover` dengan `object-position` di atas-tengah (kira-kira 55% horizontal dan 10% sampai 20% vertikal), sehingga seluruh wajah tetap terlihat di layar sangat lebar maupun sempit. Teks tidak boleh menimpa wajah di ukuran layar apa pun. Wadah foto dibuat sedikit lebih tinggi dari Hero agar efek parallax tidak membuka celah kosong.
- Seksi `#proyek` dan seterusnya dimulai tepat di bawah tepi bawah Hero, yaitu di bawah layar pertama. Klik menu "Proyek" melakukan scroll ke sana.

- **Navbar:** menu kiri (Beranda, Tentang, Proyek), logo teks di tengah, tombol pil "Hubungi Saya" dengan lingkaran panah oranye di kanan. Di Mobile: logo di kiri dan ikon menu di kanan.
- **Hero:** foto `public/foto/hero.png` sebagai latar dengan gradasi hero dan gradasi gelap bawah, tepi bawah lurus tanpa sudut melengkung, sapaan, judul dua baris, tagline, deskripsi, dan empat keahlian bernomor.
- **Proyek:** judul besar digeser ke kanan, subjudul, garis pemisah, dua kartu di atas dan satu kartu selebar penuh di bawah. Gambar: `public/proyek/github-radar.webp`, `company-profile.webp`, `landing-page.webp`. Gambar landing page punya pita ungu di sisi kiri dan kanan: tampilkan dengan `object-fit: cover` dan perbesar/posisikan agar bagian tengah yang dominan, seperti di canvas.
- **Tentang:** satu kartu tiga kolom (kalimat besar, foto `public/foto/tentang.png`, panel berisi label, bio, dan tiga skill).
- **Kontak:** judul raksasa, kolom info di kiri, form di kanan, email dan nomor besar di baris bawah.
- **Footer:** tiga ikon sosmed dalam lingkaran dan teks hak cipta.

Pencocokan dengan desain: saat menulis tiap seksi, cocokkan dengan catatan dari canvas (ukuran teks, jarak, proporsi, urutan). Teks tidak boleh menempel ke tepi, menimpa wajah di foto, atau terpotong.

## 5. Interaksi dan Fitur
**Navbar**
- Di posisi paling atas: transparan di atas Hero.
- Setelah scroll lebih dari 80 px: menjadi `fixed` dengan latar `#0C0C0C` opasitas 80% dan efek blur, tinggi mengecil menjadi sekitar 72 px, dengan transisi halus 0,4 detik.
- Saat scroll ke bawah (setelah melewati Hero) navbar bersembunyi ke atas. Saat scroll ke atas navbar muncul lagi.
- Klik menu "Beranda", "Tentang", "Proyek" melakukan smooth scroll ke seksi masing-masing. Klik logo scroll ke paling atas. Klik tombol "Hubungi Saya" scroll ke `#kontak`.
- Scroll-spy: link menu yang sesuai seksi yang sedang terlihat diberi garis bawah oranye 2 px (pakai IntersectionObserver).
- Mobile: ikon menu membuka menu layar penuh berlatar `#0C0C0C` yang berisi tiga link dan tombol "Hubungi Saya", dengan animasi muncul. Menu menutup saat link diklik, saat tombol Esc ditekan, atau saat tombol tutup diklik. Scroll halaman dikunci saat menu terbuka. Gunakan `aria-expanded` dan `aria-controls`, dan fokus keyboard tetap di dalam menu selama terbuka.

**Proyek**
- Klik kartu proyek membuka modal pratinjau: gambar besar, nama, dan kategori. Modal menutup lewat tombol tutup, klik latar gelap, atau tombol Esc. Fokus terkunci di modal dan kembali ke kartu saat modal ditutup. Scroll halaman dikunci saat modal terbuka.
- Data proyek di `constants/projects.ts` punya field `url` (kosong untuk sekarang). Kalau `url` terisi, di modal muncul tombol "Kunjungi Proyek" yang membuka tab baru dengan `rel="noopener noreferrer"`. Kalau kosong, tombol tidak ditampilkan.
- Kartu bisa dioperasikan dengan keyboard (Tab dan Enter).

**Kontak**
- Form: Nama Depan, Nama Belakang, Keperluan (pilihan: Proyek Freelance, Kerja Penuh Waktu, Lainnya), Email, Pesan.
- Validasi memakai Zod di `validations/contactSchema.ts`, pesan kesalahan berbahasa Indonesia tepat di bawah field yang salah, tampil saat field kehilangan fokus dan saat tombol dikirim. Aturan: nama depan wajib, email wajib dan valid, keperluan wajib dipilih, pesan minimal 10 karakter dan maksimal 1000 karakter.
- Anti-spam: satu field jebakan (honeypot) yang tersembunyi, dan jeda 30 detik antar pengiriman (tombol dinonaktifkan dengan hitung mundur).
- Pengiriman: karena website ini statis tanpa server, saat form valid website membuka aplikasi email pengunjung lewat tautan `mailto:` ke alamat email di `siteConfig`, dengan subjek dan isi pesan yang sudah terisi dari form (`utils/buildMailtoUrl.ts`, fungsi murni). Pisahkan logika pengiriman di `actions/submitContact.ts` sebagai fungsi biasa yang berjalan di browser (tanpa `'use server'` dan tanpa Server Action, karena website diekspor statis), sehingga nanti mudah diganti ke layanan lain (misalnya Formspree atau layanan form eksternal lewat `fetch`) tanpa mengubah komponen.
- Setelah dikirim: tampilkan pesan sukses di dekat tombol, kosongkan form, dan sediakan cadangan "Kalau aplikasi email tidak terbuka, kirim langsung ke [email]" dengan tombol salin.
- Fokus input: garis bawah berubah menjadi oranye dengan animasi tumbuh dari kiri ke kanan. Label dan placeholder tetap terbaca.
- Email besar di baris bawah menjadi tautan `mailto:` dan nomor besar menjadi tautan WhatsApp (`https://wa.me/<nomor tanpa spasi dan tanpa +>`, buka tab baru). Di samping masing-masing ada tombol kecil "Salin" yang menyalin ke clipboard dan memunculkan toast singkat ("Email disalin" atau "Nomor disalin"). Toast hilang otomatis setelah 2 detik.

**Footer dan global**
- Ikon GitHub, LinkedIn, dan Instagram adalah tautan ke URL di `constants/siteConfig.ts` (isi dengan URL placeholder yang jelas seperti `https://github.com/`), buka tab baru dengan `rel="noopener noreferrer"` dan `aria-label` yang jelas. Hover: border dan ikon menjadi oranye dan naik 4 px.
- Bar progres scroll tipis (3 px, warna oranye) di paling atas halaman, panjangnya mengikuti persentase scroll.
- Tombol "Kembali ke atas" mengambang di kanan bawah, muncul setelah scroll lebih dari 800 px (animasi fade dan skala), klik memicu smooth scroll ke atas.
- Link lewat keyboard: sediakan tautan "Lompat ke konten" di awal halaman, dan outline fokus yang jelas (`focus-visible`) berwarna oranye.

## 6. Animasi dan Efek Scroll
Aturan umum: pakai CSS transform dan opacity saja (hindari animasi properti yang memicu layout). Easing utama `cubic-bezier(0.16, 1, 0.3, 1)`. Semua durasi dan delay disimpan di `constants/animation.ts`. Elemen baru disembunyikan lewat class yang ditambahkan di client setelah komponen terpasang, sehingga konten tetap terlihat kalau JavaScript gagal dan tidak terjadi hydration mismatch atau lompatan layout. Efek scroll-reveal berjalan satu kali (tidak berulang saat scroll naik-turun).

**Animasi saat halaman dibuka (Hero)**
1. Navbar: muncul dari atas (fade dan geser 16 px), 0,6 detik.
2. Sapaan: fade dan naik 24 px, delay 0,1 detik.
3. Judul: tiap baris ("Software" dan "Engineer") naik dari bawah di dalam pembungkus `overflow: hidden` (efek mask reveal), 0,9 detik, jeda antar baris 0,12 detik.
4. Tagline, lalu deskripsi: fade dan naik, delay 0,5 dan 0,6 detik.
5. Empat keahlian: muncul berurutan (stagger 0,08 detik), mulai delay 0,7 detik.
6. Foto latar: skala 1,1 menjadi 1 selama 1,6 detik sambil fade in.

**Efek saat scroll**
- **Parallax foto Hero:** foto bergeser lebih lambat dari halaman (maksimal 60 px) saat scroll. Pakai `requestAnimationFrame` dan listener scroll pasif, hanya aktif di layar 1024 px ke atas.
- **Reveal umum:** elemen fade dan naik 40 px dengan durasi 0,8 detik, mulai saat 15 persen elemen terlihat di layar.
- **Proyek:** judul "Proyek" memakai mask reveal; subjudul fade; garis pemisah tumbuh dari kiri ke kanan (skala X 0 ke 1, 1 detik); kartu muncul berurutan (stagger 0,12 detik).
- **Hover kartu proyek:** gambar membesar 1,06 kali (0,6 detik), kartu naik 4 px, dan ikon panah kecil muncul di samping nama proyek.
- **Tentang:** empat baris kalimat besar muncul satu per satu dengan mask reveal; foto terbuka dengan `clip-path` dari atas ke bawah (1,1 detik) sementara gambar di dalamnya skala 1,15 menjadi 1; label dan bio fade naik; tiga baris skill muncul berurutan (stagger 0,1 detik) dan garis pemisahnya tumbuh dari kiri ke kanan.
- **Kontak:** judul "Hubungi Saya" memakai mask reveal; field form muncul berurutan; email dan nomor besar naik dari bawah. Hover email atau nomor memunculkan garis bawah yang tumbuh dari kiri.
- **Footer:** tiga ikon muncul dengan skala 0,8 menjadi 1 secara berurutan.
- **Menu Mobile dan modal:** animasi masuk dan keluar halus (fade dan geser, 0,3 detik).

**Aksesibilitas gerak:** saat `prefers-reduced-motion: reduce` aktif, matikan parallax dan semua animasi transform. Konten langsung tampil tanpa animasi (fade singkat boleh).

## 7. Aksesibilitas, SEO, Performa
- `<html lang="id">`. Hanya satu `<h1>` (judul Hero "Software Engineer"), tiap seksi memakai `<h2>`, urutan heading tidak loncat.
- Pakai elemen semantik: `<header>`, `<nav>`, `<main>`, `<section>` dengan `aria-labelledby`, `<footer>`.
- Semua gambar memakai `next/image` (mode `unoptimized`, lihat bagian 2) dengan `alt` yang deskriptif (misalnya "Tampilan halaman utama GitHub Radar"). Foto Hero memakai `priority`. Gambar lain dimuat lazy, dengan `sizes` yang tepat.
- Metadata di `app/layout.tsx`: `title`, `description` berbahasa Indonesia, Open Graph, `themeColor` `#0C0C0C`. Ambil nilainya dari `constants/siteConfig.ts`.
- Kontras teks memenuhi standar keterbacaan. Semua elemen interaktif bisa dipakai lewat keyboard dengan urutan fokus yang logis. Ukuran area sentuh minimal 44 px di Mobile.
- Input yang tampil kembali ke UI (nilai form, pesan) tidak pernah dirender sebagai HTML mentah.
- Hindari layout shift: semua gambar punya ukuran atau rasio yang jelas.

## 8. Pengecekan Akhir dan Laporan
Sebelum melapor:
1. Jalankan `npm run lint` dan `npm run build`. Perbaiki semua error dan peringatan yang muncul sampai bersih. Pastikan build menghasilkan folder `out/` berisi `index.html`.
2. Pastikan tidak ada `any` tanpa alasan, tidak ada kode mati, dan tidak ada nilai hex atau ukuran di komponen yang seharusnya memakai token.
3. Pastikan lewat CSS bahwa tidak ada scroll horizontal di lebar 390, 1024, dan 1920 px.
4. Pastikan lewat satu pencarian (misalnya grep) bahwa tiap komponen, hook, fitur, dan animasi yang disebut di bagian 2, 5, dan 6 ada di kode, dan bahwa tidak ada `'use server'` atau Server Action di project. Jangan membuka semua file satu per satu.
5. Periksa aturan "Layar pertama" (bagian 4) untuk empat ukuran layar yang mewakili HP, laptop kecil, Full HD, dan layar besar (390 × 844, 1366 × 768, 1920 × 1080, 2560 × 1440): tanpa scroll, hanya Navbar dan Hero yang terlihat, tidak ada bagian Proyek di tepi bawah layar, dan wajah di foto tidak tertimpa teks. Kalau tidak bisa menguji langsung di browser, periksa lewat CSS yang dihasilkan dan jelaskan di laporan akhir.

Laporan akhir (singkat) berisi:
- Daftar file yang dibuat dan diubah.
- Fitur dan animasi yang berhasil, dan yang belum atau tidak bisa dibuat beserta alasannya.
- Perbedaan antara website dan desain di canvas (kalau ada).
- Nilai placeholder yang harus saya ganti: URL sosmed, URL proyek, email, nomor telepon, nama kota.
- Cara menjalankan website (`npm run dev`), lokasi hasil build statis (`out/`), dan cara mengganti metode pengiriman form.