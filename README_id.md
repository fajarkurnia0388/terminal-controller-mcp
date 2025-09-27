# Terminal Controller untuk MCP

Server Model Context Protocol (MCP) yang memungkinkan eksekusi perintah terminal yang aman, navigasi direktori, dan operasi sistem file melalui antarmuka yang terstandarisasi.

![](https://badge.mcpx.dev?type=server "MCP Server")
[![smithery badge](https://smithery.ai/badge/@GongRzhe/terminal-controller-mcp)](https://smithery.ai/server/@GongRzhe/terminal-controller-mcp)

## Fitur

- **Eksekusi Perintah**: Menjalankan perintah terminal dengan kontrol timeout dan penangkapan output yang komprehensif
- **Manajemen Direktori**: Navigasi dan daftar isi direktori dengan format yang intuitif
- **Tindakan Keamanan**: Perlindungan bawaan terhadap perintah dan operasi yang berbahaya
- **Riwayat Perintah**: Melacak dan menampilkan eksekusi perintah terbaru
- **Dukungan Cross-Platform**: Bekerja di Windows dan sistem berbasis UNIX
- **Operasi File**: Membaca, menulis, memperbarui, menyisipkan, dan menghapus konten file dengan presisi tingkat baris

## Instalasi

### Instalasi via Smithery

Untuk menginstal Terminal Controller untuk Claude Desktop secara otomatis melalui [Smithery](https://smithery.ai/server/@GongRzhe/terminal-controller-mcp):

```bash
npx -y @smithery/cli install @GongRzhe/terminal-controller-mcp --client claude
```

### Prasyarat

- Python 3.11+
- Klien yang kompatibel dengan MCP (seperti Claude Desktop)
- UV/UVX terinstal (opsional, untuk metode UVX)

### Metode 1: Instalasi PyPI (Direkomendasikan)

Instal paket langsung dari PyPI:

```bash
pip install terminal-controller
```

Atau jika Anda lebih suka menggunakan UV:

```bash
uv pip install terminal-controller
```

### Metode 2: Dari Sumber

Jika Anda lebih suka menginstal dari sumber:

1. Clone repository ini:

   ```bash
   git clone https://github.com/GongRzhe/terminal-controller-mcp.git
   cd terminal-controller-mcp
   ```

2. Jalankan script setup:
   ```bash
   python setup_mcp.py
   ```

## Konfigurasi Klien

### Claude Desktop

Ada dua cara untuk mengkonfigurasi Claude Desktop untuk menggunakan Terminal Controller:

#### Opsi 1: Menggunakan UVX (Direkomendasikan)

Tambahkan ini ke file konfigurasi Claude Desktop Anda:

```json
"terminal-controller": {
  "command": "uvx",
  "args": ["terminal_controller"]
}
```

#### Opsi 2: Menggunakan Python Langsung

```json
"terminal-controller": {
  "command": "python",
  "args": ["-m", "terminal_controller"]
}
```

Path konfigurasi bervariasi berdasarkan sistem operasi:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

### Cursor

Untuk Cursor, gunakan pengaturan konfigurasi yang serupa dengan Claude Desktop.

### Klien MCP Lainnya

Untuk klien lain, lihat dokumentasi mereka tentang cara mengkonfigurasi server MCP eksternal.

## Penggunaan

Setelah dikonfigurasi, Anda dapat menggunakan bahasa alami untuk berinteraksi dengan terminal melalui klien MCP Anda:

- "Jalankan perintah `ls -la` di direktori saat ini"
- "Navigasi ke folder Documents saya"
- "Tunjukkan isi direktori Downloads saya"
- "Tunjukkan riwayat perintah terbaru saya"
- "Baca konten dari config.json"
- "Perbarui baris 5 di file script.py saya dengan 'print(\"Hello World\")'"
- "Hapus baris 10-15 dari file log"
- "Sisipkan baris baru di awal file teks saya"

## Referensi API

Terminal Controller mengekspos alat MCP berikut:

### `execute_command`

Menjalankan perintah terminal dan mengembalikan hasilnya.

**Parameter:**

- `command`: Perintah baris perintah yang akan dieksekusi
- `timeout`: Timeout perintah dalam detik (default: 30)

**Mengembalikan:**

- Output dari eksekusi perintah, termasuk stdout, stderr, dan status eksekusi

### `get_command_history`

Mendapatkan riwayat eksekusi perintah terbaru.

**Parameter:**

- `count`: Jumlah perintah terbaru yang akan dikembalikan (default: 10)

**Mengembalikan:**

- Catatan riwayat perintah yang diformat

### `get_current_directory`

Mendapatkan direktori kerja saat ini.

**Mengembalikan:**

- Path direktori kerja saat ini

### `change_directory`

Mengubah direktori kerja saat ini.

**Parameter:**

- `path`: Path direktori untuk beralih

**Mengembalikan:**

- Informasi hasil operasi

### `list_directory`

Mendaftar file dan subdirektori di direktori yang ditentukan.

**Parameter:**

- `path`: Path direktori untuk mendaftar isi (default: direktori saat ini)

**Mengembalikan:**

- Daftar isi direktori, diformat dengan ikon untuk direktori dan file

### `write_file`

Menulis konten ke file dengan opsi overwrite atau append.

**Parameter:**

- `path`: Path ke file
- `content`: Konten yang akan ditulis
- `mode`: Mode penulisan ('overwrite' atau 'append', default: 'overwrite')

**Mengembalikan:**

- Informasi hasil operasi termasuk verifikasi penulisan yang berhasil

### `read_file`

Membaca konten dari file dengan pemilihan baris opsional.

**Parameter:**

- `path`: Path ke file
- `start_row`: Baris awal untuk membaca (0-based, opsional)
- `end_row`: Baris akhir untuk membaca (0-based, inklusif, opsional)

**Mengembalikan:**

- Konten file atau baris yang dipilih

### `insert_file_content`

Menyisipkan konten di baris tertentu dalam file.

**Parameter:**

- `path`: Path ke file
- `content`: Konten yang akan disisipkan
- `row`: Nomor baris untuk menyisipkan (0-based, opsional)
- `rows`: Daftar nomor baris untuk menyisipkan (0-based, opsional)

**Mengembalikan:**

- Informasi hasil operasi

### `delete_file_content`

Menghapus konten di baris tertentu dari file.

**Parameter:**

- `path`: Path ke file
- `row`: Nomor baris untuk menghapus (0-based, opsional)
- `rows`: Daftar nomor baris untuk menghapus (0-based, opsional)

**Mengembalikan:**

- Informasi hasil operasi

### `update_file_content`

Memperbarui konten di baris tertentu dalam file.

**Parameter:**

- `path`: Path ke file
- `content`: Konten baru untuk ditempatkan di baris yang ditentukan
- `row`: Nomor baris untuk memperbarui (0-based, opsional)
- `rows`: Daftar nomor baris untuk memperbarui (0-based, opsional)

**Mengembalikan:**

- Informasi hasil operasi

## Pertimbangan Keamanan

Terminal Controller mengimplementasikan beberapa tindakan keamanan:

- Kontrol timeout untuk mencegah perintah yang berjalan lama
- Blacklisting perintah berbahaya (rm -rf /, format, mkfs)
- Penanganan error yang tepat dan isolasi eksekusi perintah
- Akses hanya ke perintah dan direktori yang secara khusus diberikan

## Keterbatasan

- Hanya perintah yang selesai dalam periode timeout yang akan mengembalikan hasil
- Secara default, server memiliki akses ke izin sistem file yang sama dengan pengguna yang menjalankannya
- Beberapa perintah interaktif mungkin tidak berfungsi seperti yang diharapkan karena sifat non-interaktif dari antarmuka terminal

## Pemecahan Masalah

Jika Anda mengalami masalah:

1. Periksa bahwa versi Python Anda adalah 3.11 atau lebih tinggi
2. Verifikasi bahwa konfigurasi Claude Desktop Anda benar
3. Coba jalankan terminal controller langsung untuk memeriksa error:
   ```bash
   python -m terminal_controller
   ```
4. Untuk masalah terkait UVX, coba:
   ```bash
   uvx terminal_controller
   ```
5. Tinjau log klien MCP Anda untuk error koneksi

## Berkontribusi

Kontribusi sangat diterima! Silakan kirim Pull Request.

## Lisensi

MIT
