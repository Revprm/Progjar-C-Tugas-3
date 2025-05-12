# Tugas 3 Pemrograman Jaringan C

| Nama             | NRP            |
|:----------------:|:--------------:|
| Revy Pramana     | 5025221252     |

## Arsitektur

Proyek ini dibagi menjadi beberapa modul utama:

* `file_server.py`: Menjalankan server TCP dan menerima koneksi dari client.
* `file_protocol.py`: Menerjemahkan perintah dari client dan meneruskan ke handler.
* `file_interface.py`: Melakukan operasi file di sisi server (seperti upload dan delete).
* `file_client_cli.py`: Client sederhana untuk mengirim perintah ke server.

### Upload

1. Client memanggil fungsi `remote_upload("nama_file")` di `file_client_cli.py`.
2. File dibaca dan dikodekan menjadi Base64, kemudian dikirim ke server dalam format:

   ```
   UPLOAD nama_file.ext base64_data
   ```
3. Server menerima perintah tersebut, lalu:

   * Mendeteksi perintah `UPLOAD` di `file_protocol.py`
   * Memanggil method `upload()` pada `file_interface.py`
   * File disimpan ke direktori `files/` dengan isi hasil decoding dari Base64
4. Jika berhasil, server mengembalikan respons:

   ```json
   {"status": "OK", "data": "File uploaded"}
   ```

Jika terjadi kesalahan, misalnya format Base64 tidak valid, maka respons akan:

```json
{"status": "ERROR", "data": "penjelasan kesalahan"}
```

### Delete

1. Client memanggil fungsi `remote_delete("nama_file")` di `file_client_cli.py`.
2. Perintah dikirim ke server dalam format:

   ```
   DELETE nama_file.ext
   ```
3. Server memproses perintah tersebut dengan:

   * Mendeteksi perintah `DELETE` di `file_protocol.py`
   * Memanggil method `delete()` pada `file_interface.py`
   * File dihapus dari direktori `files/`
4. Jika berhasil, server mengembalikan:

   ```json
   {"status": "OK", "data": "File deleted"}
   ```

Jika file tidak ditemukan atau tidak bisa dihapus, respons akan:

```json
{"status": "ERROR", "data": "penjelasan kesalahan"}
```

### Struktur Folder

```
.
├── file_server.py
├── file_protocol.py
├── file_interface.py
├── file_client_cli.py
├── PROTOKOL.txt
└── files/                # Folder penyimpanan file server
```

### Catatan

* Semua komunikasi antara client dan server dilakukan dalam format string dengan akhiran `\r\n\r\n`.
* File dikirim dalam bentuk Base64 agar bisa diserialisasi dalam teks.