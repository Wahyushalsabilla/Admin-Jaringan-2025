# **LAPORAN WORKSHOP ADMINISTRASI JARINGAN**

## **Rangkuman Protokol Mail, Mail Server, dan Alur Diagram**

### **Dosen Pengampu:**
Dr Ferry Astika Saputra ST, M.Sc

### **Dikerjakan Oleh:**
- **Nama:** Shalsabilla Wahyu Arifhana  
- **Kelas:** 2 D4 Teknik Informatika A  
- **NRP:** 3123600014  

**DEPARTEMEN TEKNIK INFORMATIKA**  
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**  
2024

---

## 1. Protokol Mail (SMTP, POP3, IMAP, POP3S)

Dalam sistem email, terdapat beberapa protokol utama yang digunakan:

* **SMTP (Simple Mail Transfer Protocol)**
  SMTP berfungsi untuk **mengirim email** dari pengirim ke mail server, atau dari satu mail server ke mail server lain. Protokol ini digunakan selama proses pengiriman email.

* **POP3 (Post Office Protocol version 3)**
  POP3 digunakan untuk **mengambil email dari server ke perangkat pengguna**. Biasanya email akan langsung diunduh ke perangkat dan dihapus dari server.

* **IMAP (Internet Message Access Protocol)**
  IMAP juga digunakan untuk mengambil email, tetapi email **tetap tersimpan di server**. Protokol ini cocok digunakan jika email diakses dari beberapa perangkat karena status email tetap sinkron.

* **POP3S (POP3 Secure)**
  POP3S adalah versi **aman** dari POP3, menggunakan **enkripsi SSL/TLS** untuk melindungi proses pengambilan email.

## 2. Informasi Mail Server dalam Domain

Setiap domain yang mendukung layanan email memiliki **mail server**. Informasi mengenai mail server disimpan dalam **DNS melalui MX record (Mail Exchange record)**.

Fungsi MX record:

* Menentukan server mana yang akan menerima email untuk domain tersebut.
* Jika terdapat lebih dari satu server, prioritas diatur melalui nilai preference (semakin kecil nilainya, semakin prioritas).

Contoh:
Jika domain `example.com` memiliki MX record yang mengarah ke `mail.example.com`, maka semua email yang dikirim ke `nama@example.com` akan diteruskan ke server `mail.example.com`.

## 3. Penjelasan Alur Diagram

Diagram menggambarkan alur pengiriman dan penerimaan email dari **User A** ke **User B** melalui internet. Berikut penjelasan detail alurnya:

1. **User A** menulis email menggunakan **UA (User Agent)**, yaitu aplikasi client email seperti Outlook, Thunderbird, atau webmail.
2. Email yang diketik oleh User A diteruskan ke **Spool**, yaitu area penyimpanan sementara yang menampung email sebelum dikirim.
3. Dari Spool, email dikirim ke **Alias Expander**. Komponen ini berfungsi untuk mengecek apakah alamat tujuan menggunakan alias atau tidak. Jika ditemukan alias, sistem akan mencari alamat sebenarnya melalui **Database**.
4. Setelah proses ekspansi alias selesai, email diteruskan ke **MTA (Mail Transfer Agent)** pada sisi client. MTA bertugas mengirim email ke server tujuan.
5. Email dikirim dari MTA client menuju **MTA server** User A.
6. **MTA server User A** kemudian mengirim email melalui **Internet** ke **MTA server User B**.
7. Saat email diterima oleh MTA server User B, email kembali diperiksa oleh **Alias Expander** untuk memastikan apakah alamat penerima adalah alias. Jika alias, sistem akan mengakses **Database** untuk menemukan alamat sebenarnya.
8. Setelah alamat diverifikasi, email disimpan di **Mailboxes** milik User B.
9. **Spool** di sisi User B juga digunakan untuk menyimpan email yang masih dalam antrean pemrosesan.
10. **User B** kemudian mengakses email melalui **UA** dengan mengambil email dari Mailboxes. Proses pengambilan email ini dapat menggunakan protokol **POP3**, **IMAP**, atau **POP3S**, tergantung konfigurasi yang digunakan.

Secara berurutan, alur prosesnya adalah:

`User A → UA → Spool → Alias Expander → Database (jika perlu) → MTA Client → MTA Server (User A) → Internet → MTA Server (User B) → Alias Expander → Database (jika perlu) → Mailboxes → Spool → UA (User B)`

Setiap komponen berperan untuk memastikan email dikirim, diteruskan, diverifikasi, dan diterima dengan benar oleh penerima sesuai dengan alamat yang dituju.


Setiap komponen berperan penting untuk memastikan email dikirim dan diterima dengan baik sesuai tujuan.
