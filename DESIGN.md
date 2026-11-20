# DESIGN.md: Website Portofolio Pribadi

**Instruksi untuk agent:** Baca seluruh file ini di awal setiap sesi desain, lalu kerjakan sesuai urutan di bawah. Semua angka di file ini adalah ukuran dalam piksel pada kanvas 1440 px (Desktop) atau 390 px (Mobile). Ikuti angka itu persis. Jangan mengecilkan, menggeser, atau menyederhanakan.

**Urutan kerja (kerjakan semuanya tanpa berhenti di tiap langkah, pakai tool figcraft-app):**
0. Persiapan:
   - Kalau sudah ada frame `Desktop` atau `Mobile` lama di canvas, hapus dulu keduanya, lalu bangun ulang dari awal sesuai file ini.
   - Cek font Plus Jakarta Sans sudah terpasang. Kalau belum, download dari sumber resmi dan pasang ke folder font pengguna: macOS `~/Library/Fonts`, Windows `%LOCALAPPDATA%\Microsoft\Windows\Fonts`. Jelaskan tiap command sebelum dijalankan, jangan pakai sudo atau hak admin.
   - Cek koneksi ke FigCraft memakai tool figcraft-app. Kalau sudah terhubung, jangan tutup atau buka ulang FigCraft. Kalau belum terhubung, tutup FigCraft kalau sedang terbuka, lalu buka lagi dengan control channel: macOS `open -a FigCraft --args --control 7979`, Windows `Start-Process -FilePath "<path absolut figcraft.exe>" -ArgumentList "--control 7979"`. Cek channel nyala: macOS `lsof -nP -iTCP:7979 -sTCP:LISTEN`, Windows `netstat -ano | findstr 7979`. Lalu berhenti dan beri tahu saya untuk restart OpenCode dari folder ini.
1. Buat design token di FigCraft sesuai bagian 1. Jangan membangun seksi sebelum token selesai.
2. Buat frame `Desktop` dan `Mobile` sesuai bagian 4.
3. Bangun seksi Desktop satu per satu: Navbar, Hero, Proyek, Tentang, Kontak, Footer. Gunakan posisi (x, y), lebar, tinggi, dan ukuran font persis seperti di bagian 2.
4. Bangun versi Mobile (390 px) sesuai bagian 2 (sub-bagian Mobile).
5. Beri laporan akhir: apa yang berhasil, apa yang pakai placeholder, apa yang tidak sesuai DESIGN.md.

**Cara kerja (wajib):**
- Kirim beberapa elemen sekaligus dalam satu tool call (atau `batch`) kalau tool mendukung, tetapi pecah per sub-bagian seksi (misalnya Hero: foto, kolom kiri, kolom kanan, empat keahlian) supaya tiap call selesai kurang dari 60 detik. Satu seksi cukup beberapa call, bukan satu call per elemen.
- Koneksi ke FigCraft punya batas 60 detik per request dan tidak diulang otomatis. Kalau satu call timeout atau error dengan pesan bahwa operasi "mungkin sudah selesai", cek dulu isi dokumen (inspect_document) sebelum mengulang, supaya elemen tidak dobel.
- Jangan membaca ulang node atau mengambil screenshot setelah tiap elemen atau tiap seksi.
- Buka tiap gambar referensi satu kali, tepat sebelum seksinya dibangun.
- Periksa hasil di canvas hanya di tiga titik: (1) setelah Hero Desktop selesai, karena itu seksi paling rumit; (2) setelah seluruh Desktop selesai; (3) setelah Mobile selesai. Di tiap titik, kumpulkan semua yang meleset lalu perbaiki sekaligus.
- Jangan membangun ulang seksi yang sudah benar.

**Aturan wajib:**
- Sebelum membangun tiap seksi, buka gambar referensi seksi itu di folder `referensi/` (bagian 3) dan bandingkan proporsinya: ukuran judul terhadap lebar halaman, posisi teks, jarak antar elemen. Tiru tata letak dan proporsinya. Jangan menyalin teks, foto, nama brand, atau logo dari gambar referensi.
- Semua warna, font, ukuran teks, jarak, dan radius harus memakai token di bagian 1. Jangan membuat nilai baru di luar token.
- Judul besar (Hero, Proyek, Kontak) harus benar-benar besar sesuai angka di file ini. Teks yang terlihat kecil dibanding referensi berarti salah.
- Hero dan Footer membentang penuh selebar frame (1440 px), tanpa margin kiri-kanan. Seksi lain memakai padding kiri-kanan 48 px (lebar isi 1344 px).
- Teks tidak boleh menempel ke tepi atau terpotong. Teks tidak boleh menimpa wajah subjek di foto (rambut atau baju yang gelap boleh ditimpa teks putih asalkan tetap terbaca). Antar elemen tidak boleh saling tumpuk, kecuali yang memang ditentukan (Navbar di atas Hero, teks di atas foto Hero).
- Setiap kotak teks diberi lebar tetap sesuai file ini supaya tidak pecah baris sembarangan.
- Pasang gambar ke desain memakai path absolut (gabungkan folder project dengan path relatif berikut): `public/foto/hero.png`, `public/foto/tentang.png`, `public/proyek/github-radar.webp`, `public/proyek/company-profile.webp`, `public/proyek/landing-page.webp`.
- Kalau FigCraft tidak bisa memuat .webp, buat salinan PNG di folder sementara di luar project dan pakai itu. Jangan ubah atau tambah file di `public/`.
- Kalau gambar tetap tidak bisa dimuat, pakai kotak placeholder `#141414` dengan label nama file dan catat di laporan akhir.
- Di tiga titik pemeriksaan (lihat "Cara kerja"), bandingkan hasil di canvas dengan referensi, baris "Cek" tiap seksi, dan angka di file ini: tidak ada teks menempel ke tepi, menimpa wajah, atau terlalu kecil. Perbaiki sendiri kalau meleset.
- Kalau ada tool yang gagal, tampilkan error aslinya. Jangan menebak.
- Kalau font Plus Jakarta Sans tidak tersedia di FigCraft setelah dipasang, beri tahu saya dan pakai font sans-serif geometris yang paling mirip sebagai pengganti sementara.

## 1. Design Token
### Warna
- **Background utama:** `#0C0C0C`
- **Background panel/kartu:** `#141414`
- **Teks utama:** `#FFFFFF`
- **Teks sekunder:** `#A1A1AA`
- **Aksen (oranye):** `#F26A21`
- **Border halus:** `#FFFFFF1A` (putih 10%)
- **Border tegas:** `#FFFFFF40` (putih 25%), untuk garis pemisah dan garis bawah input
- **Gradasi hero:** `#F26A21` (kiri atas) → `#8A2E17` (tengah) → `#1A0F0D` (kanan bawah)

### Aturan pemakaian warna
- **Background halaman:** `#0C0C0C`. Panel dan kartu yang perlu terpisah dari latar memakai `#141414`.
- **Teks:** judul dan teks penting `#FFFFFF`. Deskripsi, label kecil, dan keterangan `#A1A1AA`.
- **Aksen oranye `#F26A21`:** hanya untuk elemen penekanan: tanda `#` pada nomor keahlian, lingkaran panah pada tombol, label kecil "TENTANG SAYA", tombol "Kirim", dan ikon yang di-hover. Jangan dipakai untuk blok besar atau paragraf.
- **Gradasi hero:** hanya dipakai di seksi Hero.
- **Di atas gradasi oranye** semua teks memakai putih (`#FFFFFF`, atau putih 70% untuk deskripsi).

### Font
Semua teks memakai `Plus Jakarta Sans`.
- **Light** (300): sebagian kalimat besar di Tentang
- **Regular** (400): isi paragraf, label input
- **Medium** (500): menu, tombol, label huruf renggang
- **SemiBold** (600): sapaan, tagline, nama keahlian
- **Bold** (700): judul besar, logo, nama proyek, email dan nomor

### Ukuran teks (px)
- **xs** `12`: nomor kecil di Mobile
- **sm** `14`: label input, info kecil, kategori, teks hak cipta
- **base** `16`: menu, tombol, deskripsi, bio, nomor keahlian
- **lg** `18`: nama keahlian, nama skill
- **xl** `24`: logo, nama proyek
- **2xl** `32`: sapaan Hero, tagline Hero, subjudul Proyek
- **3xl** `48`: kalimat besar di Tentang, email dan nomor di Kontak
- **display** `144`: judul Hero ("Software Engineer")
- **display-xl** `208`: judul besar "Proyek" dan "Hubungi Saya"
- **display-m** `72`: judul Hero dan Proyek di Mobile

### Letter-spacing dan line-height
- display dan display-xl: letter-spacing -3%, line-height 1.0
- 2xl dan 3xl: letter-spacing -1%, line-height 1.1 (3xl), 1.35 (2xl)
- Label huruf renggang: huruf kapital, letter-spacing +30%
- Paragraf (base, sm): letter-spacing 0, line-height 1.75

### Jarak
Kelipatan 8 px: 8, 16, 24, 32, 48, 64, 96. Jangan pakai angka di luar ini kecuali angka yang tertulis eksplisit di bagian 2.

### Radius
- **Kartu dan gambar:** `24`
- **Panel Tentang:** `32`
- **Tombol pil:** `999`
- **Hero:** `0` di semua sudut. Hero berbentuk persegi panjang lurus, tanpa sudut melengkung (tepi bawah Hero lurus)

### Border
- `1 px` default. `2 px` hanya untuk emphasis (hover atau fokus).

## 2. Tata Letak, Konten, dan Warna per Seksi
Koordinat (x, y) dihitung dari sudut kiri atas seksi masing-masing. Semua isi seksi (kecuali Hero dan Footer) berada di dalam lebar 1344 px, mulai x = 48 sampai x = 1392.
### Navbar (menumpuk di atas Hero)
- **Acuan:** `referensi/navbar-01.png` (susunan: menu kiri, logo tengah, tombol pil kanan) dan bagian atas `referensi/hero-01.png` (navbar menumpuk di atas gradasi).
- **Posisi:** y = 32, tinggi 56, tanpa latar (transparan). Semua item sejajar vertikal di tengah.
- **Menu (kiri):** mulai x = 48. Tiga link berurutan dengan jarak 40: "Beranda", "Tentang", "Proyek". Ukuran `base` (16), Medium, putih.
- **Logo (tengah):** teks "Nama Kamu", rata tengah di x = 720. Ukuran `xl` (24), Bold, putih.
- **Tombol CTA (kanan):** ujung kanan di x = 1392. Lebar 192, tinggi 56. Bentuk pil, border 1 px putih `#FFFFFF`, latar transparan. Teks "Hubungi Saya" ukuran `base` (16), Medium, putih, padding kiri 24. Di dalam pil, sisi kanan: lingkaran diameter 48 berwarna `#F26A21` dengan ikon panah putih 20 px. Lingkaran menyatu di dalam pil (jarak 4 dari tepi pil), bukan terpisah.
- **Tindakan klik:** CTA menuju seksi Kontak.

### Hero
- **Acuan:** `referensi/hero-01.png`. Tiru komposisinya persis: foto subjek berukuran besar di tengah, judul raksasa dua baris di kiri, tagline dan deskripsi di kanan, dan empat keahlian bernomor di baris bawah yang membentang selebar halaman. Pengecualian: sudut bawah Hero di referensi membulat, tetapi di desain kita sudut bawah Hero lurus tanpa lengkungan.
- **Ukuran:** lebar 1440 (penuh, tanpa margin), tinggi 960. Semua sudut Hero radius 0: tepi bawah Hero lurus (garis horizontal lurus dari kiri ke kanan), tidak ada ujung melengkung. Di bawah Hero latar `#0C0C0C`.
- **Lapisan (dari bawah ke atas):**
  1. Foto `public/foto/hero.png`. Foto bukan sekadar cover: skala dan posisinya harus diatur agar komposisinya sama dengan orang di `referensi/hero-01.png` (foto close-up setengah badan, kepala besar):
     - Subjek berada di tengah Hero. Titik tengah kepala di x ≈ 800 (±40), sedikit di kanan tengah halaman seperti di referensi.
     - Ukuran kepala sama dengan referensi: puncak rambut di y ≈ 96 (±8% dari tepi atas Hero), dagu di y ≈ 700. Tinggi kepala dari puncak rambut sampai dagu ±600 px (±62% tinggi Hero).
     - Bahu dan dada memanjang sampai tepi bawah Hero. Lengan dan sisa badan terpotong di bawah.
     - Foto harus menutupi seluruh lebar Hero tanpa celah kosong. Kalau perlu, perbesar sedikit atau pakai frame foto yang lebih besar dari Hero dan dipotong (clip) mengikuti Hero. Kalau FigCraft tidak bisa mengatur skala dan posisi isi gambar, catat di laporan akhir.
  2. Gradasi hero (bagian 1) di atas foto, opasitas 50%.
  3. Gradasi gelap di bagian bawah: tinggi 360 dari tepi bawah, dari transparan ke `#0C0C0C` opasitas 85%. Fungsinya agar empat keahlian di bawah terbaca.
- **Kolom kiri (rata kiri di x = 48, teks maksimal sampai x = 660):**
  - Sapaan: "Halo, saya". y = 240, `2xl` (32), SemiBold, putih.
  - Judul: "Software" (baris 1) dan "Engineer" (baris 2). y = 296, ukuran `display` (144), Bold, putih, line-height 1.05 (tinggi tiap baris ±151, blok judul y = 296 sampai 598). Judul harus terlihat sangat besar: lebar kata "Software" ±610 px. Ujung huruf judul boleh sedikit menimpa rambut subjek, tetapi tidak boleh menimpa wajah.
- **Kolom kanan (rata kiri di x = 976, lebar 416, berakhir di x = 1392):**
  - Tagline: "Software yang baik terasa mudah dan andal." y = 456, lebar kotak 416, `2xl` (32), SemiBold, putih, line-height 1.35. Pecah menjadi dua baris: "Software yang baik terasa" dan "mudah dan andal."
  - Deskripsi: "Dari database hingga antarmuka, saya membangun solusi digital yang rapi, skalabel, dan mudah dirawat." y = 566, lebar kotak 416, `base` (16), Regular, putih 70%, line-height 1.75 (tiga baris).
  - Kolom kanan berada di sebelah kanan wajah subjek, sejajar vertikal dengan bagian tengah Hero seperti di referensi. Teks boleh menimpa rambut atau bahu yang gelap, tetapi tepi kiri teks (x = 976) harus berada di kanan wajah.
- **Empat keahlian bernomor (baris bawah, selebar halaman dan di tengah):**
  - Baris ini adalah satu auto layout horizontal selebar 1344 (x = 48 sampai 1392) dengan distribusi "space between". Jangan atur posisi tiap item secara manual dan jangan condong ke kiri.
  - Item 1 rata kiri di x = 48. Item 4 berakhir tepat di x = 1392. Jarak antar item sama besar. Perkiraan x awal tiap item: 48, 481, 904, 1277. Barisnya simetris terhadap tengah Hero (x = 720).
  - Nomor di y = 824: tanda `#` berwarna `#F26A21`, angka putih, `base` (16), Bold. Label di y = 856: `lg` (18), Regular, putih.
  - Isi: `#01` Pengembangan Frontend, `#02` Pengembangan Backend, `#03` Desain Database, `#04` Integrasi API.
- **Cek:** (1) kepala subjek berukuran besar dan berada di tengah seperti di referensi; (2) judul 144 px di kiri, tagline dan deskripsi di kanan, dan tidak ada teks yang menimpa wajah; (3) empat keahlian tersebar rata dari x = 48 sampai x = 1392 sehingga barisnya terlihat di tengah, bukan di sisi kiri; (4) tidak ada teks menempel ke tepi.

### Proyek
- **Acuan:** `referensi/proyek-01.png`. Tiru: judul raksasa yang digeser ke kanan (tidak rata kiri dan tidak rata kanan penuh), subjudul kecil rata kiri di bawahnya, garis pemisah tipis selebar isi, gambar besar dengan teks dua baris di bawahnya (nama dan kategori), tanpa kotak panel di sekeliling gambar.
- **Ukuran:** lebar 1440, tinggi 1640, latar `#0C0C0C`.
- **Judul:** "Proyek". Kotak mulai di x = 624, y = 96. `display-xl` (208), Bold, putih, line-height 1.0 (y = 96 sampai 304).
- **Subjudul:** "Proyek Terbaru". x = 48, y = 328, `2xl` (32), Regular, putih.
- **Garis pemisah:** x = 48 sampai 1392, y = 392, tebal 1 px, warna `#FFFFFF40`.
- **Baris atas (2 kartu), y = 424:**
  - Kartu kiri x = 48, kartu kanan x = 732. Jarak antar kartu 24. Lebar tiap kartu 660.
  - Gambar: 660 × 368, radius 24, fit cover, anchor atas.
  - Teks di bawah gambar (jarak 16 dari gambar), rata kiri, tanpa panel dan tanpa border: nama proyek `xl` (24) Bold putih (y = 808), kategori `base` (16) Regular `#A1A1AA` (y = 840).
  - Kiri: `public/proyek/github-radar.webp`, nama "GitHub Radar", kategori "Web App".
  - Kanan: `public/proyek/company-profile.webp`, nama "Company Profile", kategori "Website".
- **Baris bawah (1 kartu penuh), y = 912:**
  - x = 48, lebar 1344. Gambar 1344 × 560, radius 24, fit cover, anchor atas.
  - `public/proyek/landing-page.webp` punya pita ungu di kiri dan kanan. Skalakan gambar agar area konten tengah memenuhi lebar kartu sehingga pita ungu terpotong. Kalau FigCraft tidak bisa mengatur skala dan crop gambar, pakai fit cover saja dan catat di laporan akhir.
  - Nama "Landing Page" (y = 1488) dan kategori "Website" (y = 1520), gaya sama seperti kartu atas.
- **Cek:** judul "Proyek" harus sangat besar (208 px), kartu atas sejajar rata, tidak ada ruang kosong besar antara elemen, ruang bawah terakhir 96 px.

### Tentang
- **Acuan:** `referensi/tentang-01.png`. Tiru: tiga kolom dalam satu kartu besar bersudut membulat. Kolom kiri berisi kalimat besar dengan variasi tipis-tebal, kolom tengah foto potret tinggi penuh, kolom kanan berisi label huruf renggang, paragraf pendek, dan daftar tiga item bergaris pemisah. Warna mengikuti token gelap kita, bukan putih seperti referensi.
- **Ukuran seksi:** lebar 1440, tinggi 976, latar `#0C0C0C`.
- **Kartu:** x = 48, y = 96, lebar 1344, tinggi 784, radius 32, isi dipotong mengikuti sudut kartu. Tiga kolom tanpa jarak: kiri 416, tengah 544, kanan 384.
- **Kolom kiri** (x = 0 sampai 416 di dalam kartu): latar `#141414`, padding 32. Kalimat dipusatkan vertikal dan rata kiri, ukuran `3xl` (48), line-height 1.1, empat baris:
  - "Membangun" (Light 300, putih)
  - "Software yang" (Light 300, putih)
  - "Berguna," (Light 300, putih)
  - "Selalu." (Bold 700, putih)
- **Kolom tengah** (x = 416 sampai 960): foto `public/foto/tentang.png`, 544 × 784, fit cover, anchor tengah-atas, tanpa radius sendiri (mengikuti kartu).
- **Kolom kanan** (x = 960 sampai 1344): latar `#141414`, padding 32 (lebar isi 320). Isi dipusatkan vertikal, rata kiri, dari atas ke bawah:
  - Label: "TENTANG SAYA", `sm` (14), Medium, huruf kapital, letter-spacing +30%, warna `#F26A21`.
  - Jarak 24.
  - Bio: "Saya seorang Software Engineer yang suka membangun solusi digital yang rapi dan mudah dirawat. Saya senang belajar teknologi baru dan bekerja sama dengan tim. Saat ini saya terbuka untuk proyek freelance maupun kerja penuh waktu." `base` (16), Regular, `#A1A1AA`, line-height 1.6.
  - Jarak 32.
  - Daftar skill, 3 baris, tiap baris tinggi 72 dengan garis atas 1 px `#FFFFFF1A`. Di kiri nomor (`#01`, dst: `#` oranye `#F26A21`, angka putih, `sm` 14, Bold). Di kanannya (mulai 48 dari tepi kiri baris) nama skill `lg` (18) SemiBold putih, dan di bawahnya teknologi `sm` (14) Regular `#A1A1AA`:
    - `#01` Frontend, "React, Next.js"
    - `#02` Backend, "Node.js, API"
    - `#03` Database, "PostgreSQL, Supabase"
- **Cek:** tiga kolom rata tinggi, foto memenuhi kolom tengah penuh, kalimat besar terbaca dan tidak terpotong.

### Kontak
- **Acuan:** `referensi/kontak-01.png`. Tiru: judul raksasa selebar halaman di atas, dua kolom di bawahnya (info kecil di kiri, form di kanan dengan input bergaya garis bawah), dan satu baris email dan nomor berukuran besar di paling bawah. Warna mengikuti token gelap kita, bukan putih seperti referensi.
- **Ukuran:** lebar 1440, tinggi 1064, latar `#0C0C0C`.
- **Judul:** "Hubungi Saya". x = 48, y = 96. `display-xl` (208), Bold, putih, line-height 1.0, satu baris selebar isi (±1310 px).
- **Kolom kiri** (x = 48, lebar 480, mulai y = 352), teks `sm` (14), Regular, `#A1A1AA`, line-height 1.5:
  - Grup 1: "Kota Kamu, Indonesia" dan "2026".
  - Grup 2 (y = 424): "Jam respons", "Senin - Jumat", "09.00 - 17.00".
- **Kolom kanan** (form, x = 720, lebar 672). Input bergaya garis bawah (tanpa kotak): teks petunjuk `sm` (14) `#A1A1AA` di atas garis, garis bawah 1 px `#FFFFFF40`, teks yang diketik putih:
  - Baris 1, y = 352: label "Nama (wajib)" `sm` (14) Bold putih. Dua input sejajar di y = 384, masing-masing lebar 324 dengan jarak 24 (x = 720 dan x = 1068), tinggi 56, petunjuk "Nama Depan" dan "Nama Belakang".
  - Baris 2, y = 464: "Keperluan", dropdown, lebar penuh 672, tinggi 56, ikon chevron kecil di ujung kanan. Pilihan: Proyek Freelance, Kerja Penuh Waktu, Lainnya.
  - Baris 3, y = 544: "Email (wajib)", lebar penuh, tinggi 56.
  - Baris 4, y = 624: "Pesan", lebar penuh, tinggi 112.
  - Tombol, y = 768: "Kirim", x = 720, lebar 144, tinggi 48, pil, latar `#F26A21`, teks putih `base` (16) Bold.
- **Baris bawah**, y = 912, tinggi 56, `3xl` (48), Bold, putih:
  - Email "contoh@gmail.com", rata kiri di x = 48.
  - Nomor "+62 1234567890", rata kanan, berakhir di x = 1392.
- **Cek:** judul harus sangat besar dan selebar isi, form rapi sejajar di kolom kanan, email dan nomor besar dan terbaca.

### Footer
- **Acuan:** `referensi/footer-01.png`. Tiru: ikon sosmed dalam lingkaran outline di tengah, teks hak cipta di bawahnya, latar gelap, sangat minimalis.
- **Ukuran:** lebar 1440 (penuh), tinggi 192, latar `#0C0C0C` (hitam, sama persis dengan background halaman, bukan `#141414`).
- **Ikon:** tiga lingkaran diameter 48 (GitHub, LinkedIn, Instagram), border 1 px putih `#FFFFFF`, ikon putih 20 px di tengah lingkaran, jarak antar lingkaran 16. Susunan dipusatkan di x = 720, y = 48.
- **Teks:** "© 2026 Nama Kamu. Seluruh hak cipta dilindungi." rata tengah di x = 720, y = 120, `sm` (14), Regular, `#A1A1AA`.

### Mobile (lebar 390, padding kiri-kanan 24, lebar isi 342)
Semua seksi ditumpuk satu kolom dengan urutan yang sama dengan Desktop. Gaya, warna, dan font sama. Tinggi menyesuaikan isi.
- **Navbar:** y = 24, tinggi 48, transparan di atas Hero. Logo "Nama Kamu" di kiri (x = 24), `xl` (20 di Mobile), Bold. Di kanan ikon menu (dua garis horizontal, lebar 28, putih). Menu dan tombol CTA tidak ditampilkan.
- **Hero:** lebar 390, tinggi 820, semua sudut radius 0 (tepi bawah lurus). Foto `public/foto/hero.png` dengan subjek di tengah (kepala besar, anchor bawah-tengah, bahu sampai tepi bawah Hero), gradasi hero 50%, gradasi gelap bawah tinggi 300. Teks rata kiri di x = 24: sapaan "Halo, saya" (y = 120, 20 px SemiBold), judul "Software Engineer" dua baris (y = 160, `display-m` 72 px Bold, line-height 1.0), tagline (y = 328, 20 px SemiBold, lebar 342), deskripsi (14 px, line-height 1.6, putih 70%, lebar 342). Empat keahlian dalam grid 2 × 2 di dasar Hero (x = 24 dan x = 207, y = 704 dan y = 760): nomor 12 px, label 14 px.
- **Proyek:** padding atas 64. Judul "Proyek" di x = 24 (`display-m` 72, Bold). Subjudul "Proyek Terbaru" (20 px Regular), garis pemisah `#FFFFFF40`. Tiga kartu bertumpuk dengan jarak 32: gambar 342 × 192 (radius 16, cover, anchor atas), nama proyek 20 px Bold, kategori 14 px `#A1A1AA`.
- **Tentang:** padding atas 64. Kalimat besar 40 px, line-height 1.1, empat baris dengan variasi Light dan Bold seperti Desktop. Foto 342 × 456 (radius 24, cover). Lalu label "TENTANG SAYA", bio (14 px, line-height 1.6), dan tiga skill (tiap baris tinggi 72) di bawahnya.
- **Kontak:** padding atas 64. Judul "Hubungi Saya" 48 px Bold satu baris. Info lokasi dan jam respons (14 px). Form selebar 342: tiap input tinggi 56, "Pesan" tinggi 112, tombol "Kirim" selebar 342 tinggi 48. Email dan nomor 24 px Bold, bertumpuk, rata kiri.
- **Footer:** tinggi 192, latar `#0C0C0C`, sama seperti Desktop, dipusatkan di x = 195.

## 3. Referensi
Folder: `referensi/`. Detail apa yang ditiru dari tiap gambar sudah tertulis di baris **Acuan** pada tiap seksi di bagian 2. Ambil gaya dan proporsinya, jangan menyalin layout persis, teks, foto, atau logo.
- `referensi/navbar-01.png`: untuk Navbar
- `referensi/hero-01.png`: untuk Hero
- `referensi/proyek-01.png`: untuk Proyek
- `referensi/tentang-01.png`: untuk Tentang
- `referensi/kontak-01.png`: untuk Kontak
- `referensi/footer-01.png`: untuk Footer
- `referensi/palet-01.png`: untuk palet dan font (palet gelap dengan aksen oranye, gradasi hangat di hero, tombol pil putih dan oranye, font sans-serif geometris yang tebal)

**Mood keseluruhan:** gelap, minimalis, modern, dengan aksen oranye hangat, tipografi besar dan tegas.

Catatan: referensi Tentang dan Kontak berlatar terang. Tata letak dan proporsinya diambil dari referensi, tetapi warna tetap mengikuti token di bagian 1.

## 4. Struktur Frame
- **Desktop:** lebar 1440, layout vertikal, tanpa padding frame (tiap seksi mengatur padding sendiri sesuai bagian 2).
  - `Navbar`: tinggi 120 (y = 0 sampai 120), menumpuk di atas `Hero`
  - `Hero`: tinggi 960
  - `Proyek`: tinggi 1640
  - `Tentang`: tinggi 976
  - `Kontak`: tinggi 1064
  - `Footer`: tinggi 192
  - Total tinggi Desktop: 960 + 1640 + 976 + 1064 + 192 = 4832
- **Mobile:** lebar 390, padding kiri-kanan 24, seksi sama, semua kolom ditumpuk. Tinggi menyesuaikan isi.

Aturan penamaan: `Desktop`, `Mobile`, `Navbar`, `Hero`, `Proyek`, `Tentang`, `Kontak`, `Footer`.