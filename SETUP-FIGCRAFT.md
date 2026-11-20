description: Setup FigCraft MCP untuk OpenCode (macOS/Windows)
agent: build

Tugas: setup FigCraft MCP untuk OpenCode di mesin ini, lengkap sampai siap dipakai.

Aturan:
- Jelaskan singkat setiap command sebelum dijalankan. Jangan pakai sudo atau hak admin kecuali aku setuju.
- Jangan menimpa opencode.json yang sudah ada. Kalau sudah ada, gabungkan (merge) dan tampilkan isi akhirnya.
- Kalau ada langkah gagal, berhenti, tampilkan error aslinya, dan usulkan perbaikan. Jangan menebak path.

Langkah:
1. Deteksi OS dan arsitektur (macOS universal / Windows x64 / Windows arm64).
2. Ambil rilis terbaru dari https://github.com/storytold/figcraft/releases. Pakai nama file asli dari halaman rilis, jangan hardcode versi.
   - macOS: download figcraft-<versi>-macos-universal.dmg dan figcraft-cli-<versi>-macos-universal.zip. Pasang app ke /Applications, ekstrak CLI ke ~/.local/bin/figcraft-cli, chmod +x, hapus atribut quarantine (xattr -d com.apple.quarantine).
   - Windows: download file .msi sesuai arsitektur dan pasang. figcraft-cli.exe ikut terpasang bersama app. Cari lokasi figcraft.exe dan figcraft-cli.exe dengan Get-ChildItem (misalnya di folder %LOCALAPPDATA%\Programs), lalu catat path absolutnya.
3. Verifikasi: jalankan figcraft-cli --version (pakai path absolut hasil langkah 2) dan tampilkan hasilnya.
4. Tambahkan MCP server ke opencode.json di folder kerja ini memakai format V2 OpenCode: nama server diletakkan di bawah "mcp" lalu "servers" (mcp.servers.figcraft-app), BUKAN langsung di bawah "mcp".
   Isi server: nama "figcraft-app", type "local", command [<path absolut figcraft-cli>, "mcp", "--attach", "7979"].
   Gunakan path absolut hasil deteksi, bukan placeholder. Di Windows, tulis path di JSON dengan "/" atau "\\".
5. Buka FigCraft dengan control channel:
   - macOS: open -a FigCraft --args --control 7979
   - Windows: Start-Process -FilePath "<path absolut figcraft.exe hasil langkah 2>" -ArgumentList "--control 7979"
   Kalau FigCraft sudah terbuka, minta aku menutupnya dulu. Flag --control hanya untuk app (figcraft.exe), bukan figcraft-cli.
6. Cek channel: macOS lsof -nP -iTCP:7979 -sTCP:LISTEN, Windows netstat -ano | findstr 7979. Harus ada 127.0.0.1:7979 LISTEN.
7. Selesai. Beritahu aku untuk restart OpenCode dari folder ini, lalu cek /mcps sampai figcraft-app Connected, lalu tes dengan prompt: "Pakai tool figcraft-app, buat frame 390x844 bernama Home".