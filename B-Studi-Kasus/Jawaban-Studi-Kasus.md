### **Bagian B**

#### **1. Mengirim dan Menerima File ke Suatu Server**

Jika diminta untuk mengirim dan menerima file ke suatu server, beberapa protokol yang umum digunakan adalah File Transfer Protocol (FTP) dan SSH File Transfer Protocol (SFTP). Berikut langkah-langkah setiap protokolnya:

**FTP:**
1. **Pembukaan Koneksi:** Client membuka koneksi dengan server FTP menggunakan port 21.
2. **Otentikasi:** Client menyediakan kredensial (username dan password) untuk otentikasi ke server.
3. **Navigasi ke Direktori:** Client menjelajahi struktur direktori server untuk mencapai direktori tujuan.
4. **Transfer File:** Client mengirimkan perintah untuk mengunggah (STOR) atau mengunduh (RETR) file ke atau dari server.
5. **Penutupan Koneksi:** Setelah selesai, koneksi FTP ditutup.

**SFTP:**
1. **Pembukaan Koneksi:** Client membuka koneksi dengan server SFTP menggunakan port 22 yang sama dengan SSH.
2. **Otentikasi:** Client menyediakan kredensial (username dan password atau kunci publik) untuk otentikasi ke server melalui SSH.
3. **Navigasi ke Direktori:** Client menjelajahi struktur direktori server untuk mencapai direktori tujuan.
4. **Transfer File:** Client menggunakan perintah seperti `put` untuk mengunggah file ke server atau `get` untuk mengunduh file dari server.
5. **Penutupan Koneksi:** Setelah selesai, koneksi SFTP ditutup.

#### **2. Menulis Hasil Perintah Ping ke Sebuah File Text** 

Tahapan untuk menulis hasil perintah `ping` ke sebuah file teks adalah:

1. **Buka Terminal:**
   - Buka terminal seperti CMD/PowerShell atau terminal sistem operasi yang digunakan.
   
2. **Mengetik Perintah Ping:**
   - Ketik perintah `ping` diikuti dengan alamat IP atau nama host yang ingin diping. Contoh: `ping www.google.com`.

3. **Tambahkan Perintah Direksi:**
   - Tambahkan perintah direksi untuk mengarahkan hasil ping ke file teks. Ada dua opsi perintah direksi:
     - `>` : Mengarahkan output ping ke file teks. Jika file teks sudah ada, isi sebelumnya akan ditimpa.
     - `>>` : Menambahkan output ping ke file teks tanpa menghapus isi sebelumnya (jika ada).
  
   - Contoh perintah:
     ```bash
     ping www.google.com > hasil_ping.txt
     ```
     atau
     ```bash
     ping www.google.com >> hasil_ping.txt
     ```

4. **Menentukan Nama File:**
   - Tentukan nama file yang akan digunakan untuk menyimpan hasil ping. Dalam contoh di atas, nama file adalah `hasil_ping.txt`.
   - Tekan `Enter` untuk menjalankan perintah tersebut.

**Kesimpulan:**
Hasil ping akan tersimpan dalam file teks yang dapat kita buka dan baca menggunakan editor teks atau program lainnya.
