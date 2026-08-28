# 🛠️ MacBook Storage Cleanup Cheatsheet for Developers

Panduan ringkas untuk membebaskan ruang penyimpanan (*storage*) pada macOS yang penuh akibat *cache development*, *build artifacts*, Docker images, dan `node_modules`.

---

## 🔍 1. Diagnosis & Pindaian Storage

Gunakan **`ncdu`** untuk melihat direktori/folder mana di *Home directory* (`~`) yang paling menyita ruang:

```bash
# Install ncdu via Homebrew (jika belum ada)
brew install ncdu

# Pindai direktori Home
ncdu ~
```

---

## ⚡ 2. Perintah Pembersihan Cepat (Instant Clean)

Jalankan perintah ini untuk menghapus *cache* sementara yang **aman untuk dihapus** tanpa mengganggu project aktif:

```bash
# Hapus cache NPM (Sangat sering membengkak puluhan GB)
rm -rf ~/.npm

# Hapus cache sistem lokal & Gradle
rm -rf ~/.cache/*
rm -rf ~/.gradle/caches/

# Hapus cache kompilasi & modul Golang
go clean -cache -modcache -testcache

# Hapus sisa installer Homebrew lama
brew cleanup -s && rm -rf $(brew --cache)
```

---

## 🧰 3. Pembersihan Spesifik Ecosystem & Tools

### 📦 A. Node.js (`node_modules`)
Gunakan `npkill` secara interaktif untuk mencari dan menghapus folder `node_modules` dari project-project lama/pasif:

```bash
npx npkill
```
> **Tips:** Navigasi dengan tombol panah `↑` `↓`, dan tekan `Space` untuk menghapus folder `node_modules` yang dipilih.

---

### 🐳 B. Docker Desktop
Docker Virtual Machine (`Docker.raw`) menyita ruang besar di kategori *System Data*.

1. **Prune via Terminal:**
   ```bash
   docker system prune -a --volumes
   ```
2. **Kecilkan Batas Disk (GUI):**
   - Buka **Docker Desktop** $\rightarrow$ **Settings (Ikon Roda Gigi)** $\rightarrow$ **Resources**.
   - Batasi **Disk usage limit** ke angka secukupnya (misal: **16 GB** atau **32 GB**).
   - Klik **Apply & restart**.
3. **Purge Data (Reset VM jika disk tidak berkurang):**
   - Klik ikon **Troubleshoot (Gambar Bug)** di pojok kanan atas Docker Desktop.
   - Pilih **Clean / Purge data** $\rightarrow$ Centang opsi data $\rightarrow$ **Delete**.

---

### 📱 C. Android Studio & Emulator
Jika sedang tidak aktif mengembangkan aplikasi Android/Flutter:

```bash
# Hapus AVD (Emulator) & Android SDK Cache (~8 GB+)
rm -rf ~/.android
```

---

### 🤖 D. Model AI Lokal (LM Studio / Ollama)
Model Large Language Model (LLM) lokal sangat menyita memori (3–10 GB per model).

```bash
# Hapus model cache LM Studio
rm -rf ~/.lmstudio
```

---

### 🛠️ E. Xcode & iOS Simulator
Jika pernah menginstal Xcode atau Flutter iOS tools:

```bash
# Hapus DerivedData
rm -rf ~/Library/Developer/Xcode/DerivedData/*

# Erase data simulator
xcrun simctl erase all
```

---

## 🚀 4. One-Liner Command (Pembersihan Sekali Jalan)

Salin dan jalankan satu baris perintah ini di Terminal untuk mengosongkan **20 GB – 40 GB** secara otomatis:

```bash
rm -rf ~/.npm ~/.cache/* ~/.gradle/caches/ && go clean -cache -modcache -testcache && brew cleanup -s && docker system prune -f
```

---

*Disimpan untuk kebutuhan pemeliharaan berkala MacBook*
