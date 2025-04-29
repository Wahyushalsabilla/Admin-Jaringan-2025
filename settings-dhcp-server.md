# Konfigurasi Jaringan dan DNS Server

## 1. Pengaturan Wired Settings
Set IP address secara manual melalui pengaturan Wired Settings sesuai dengan gambar berikut:

![Gambar Pengaturan Wired](img/wired-settings.png)

## 2. Verifikasi IP Address
Cek IP address menggunakan perintah `ip a`, hasil yang relevan:

```
2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.5.10/24 brd 192.168.5.255 scope global noprefixroute enp2s0
```

## 3. Uji Konektivitas
Ping ke alamat IP lain:

```
# ping 10.252.108.55
64 bytes from 10.252.108.55: icmp_seq=44 ttl=64 time=0.285 ms
```

## 4. Instalasi BIND9
Instalasi paket DNS server:

```
sudo apt install bind9 bind9-utils
```

Jika sudah terinstal, akan muncul keterangan seperti:
```
bind9 is already the newest version...
```

## 5. Konfigurasi BIND
Edit file konfigurasi:

```
nano /etc/bind/named.conf.options
```

Setelah itu, lakukan pengecekan konfigurasi:

```
named-checkconf
```

## 6. Pengujian DNS Menggunakan `dig`
Lakukan pengujian menggunakan `dig`:

```
dig kelompok5.home

;; ANSWER SECTION:
kelompok5.home. 86400 IN A 192.168.5.10
```

## 7. Konfigurasi Zona `kelompok5.home`
Berikut isi file zona untuk domain `kelompok5.home`:

```
$TTL 86400
@   IN  SOA     kelompok5.home. root.kelompok5.home. (
        2025042401  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
        IN  NS      ns.kelompok5.home
        IN  A       192.168.5.10
        IN  MX 10   ns.kelompok5.home

dlp     IN  A       192.168.5.10
www     IN  CNAME   ns
```

## 8. Pengujian DNS Setelah Konfigurasi Zona
Setelah file zona disimpan dan service BIND9 direstart, lDari hasil konfigurasi yang dilakukan, server berhasil diatur menggunakan BIND9 sebagai DNS server. Alamat domain kelompok5.home dapat di-resolve dengan benar ke IP 192.168.5.10. Semua tahapan mulai dari pengaturan IP, instalasi BIND9, konfigurasi file zona, hingga pengujian dengan dig berjalan sukses tanpa error.akukan kembali pengujian:

```
dig kelompok5.home

;; ANSWER SECTION:
kelompok5.home. 86400 IN A 192.168.5.10
```
*Pengujian DNS ke Domain Kelompok Lain*

Saya melakukan pengujian DNS menggunakan perintah `dig` untuk menguji apakah domain dari kelompok lain bisa di-resolve dengan baik. Berikut hasil pengujian terhadap domain kelompok 1 dan kelompok 3:

---
Konfigurasi File /etc/resolv.conf

Sebelum melakukan pengecekan DNS dengan dig, saya melakukan pengecekan dan pengaturan pada file /etc/resolv.conf. File ini digunakan oleh sistem untuk menentukan alamat server DNS yang akan digunakan untuk melakukan resolusi nama domain.

Isi dari file tersebut:

img

Berikut adalah tampilan dari file tersebut saat dibuka menggunakan nano editor:
### 🔎 Pengujian Domain Kelompok 1
**Domain:** `kelompok1.home`

📄 **Perintah:**
```bash
dig kelompok1.home
```

🖼️ **Hasil:**
![kelompok1 dig result](attachment:/mnt/data/ae602f03-da4f-425e-9629-747ceb0ad9bb.png)

📌 **Penjelasan:**
Hasil `dig` menunjukkan bahwa domain `kelompok1.home` berhasil di-resolve dan mendapatkan jawaban dari DNS. Ini menandakan bahwa domain tersebut sudah dikonfigurasi dengan benar.

---

### 🔎 Pengujian Domain Kelompok 3
**Domain:** `kelompok3.home`

📄 **Perintah:**
```bash
dig kelompok3.home
```

🖼️ **Hasil:**
![kelompok3 dig result](attachment:/mnt/data/63c43218-6283-4c85-bcc1-37ce35b85adf.png)

📌 **Penjelasan:**
Domain `kelompok3.home` juga berhasil di-resolve dengan sukses. Hal ini menunjukkan konfigurasi DNS mereka berjalan baik.

---

### 🌐 Pengujian Akses Server Kelompok 2
**Domain:** `kelompok2.home`

📌 **Penjelasan:**
Saya berhasil mengakses web server milik kelompok 2 melalui browser menggunakan alamat `http://kelompok2.home`. Website mereka dapat diakses dan menampilkan halaman dengan informasi anggota kelompok secara lengkap.

🖼️ **Hasil Tampilan:**
![kelompok2 homepage](attachment:/mnt/data/211e4c89-435c-4a27-8553-75686958636c.png)

Hal ini menunjukkan bahwa DNS dan web server dari kelompok 2 telah dikonfigurasi dengan benar dan berjalan normal.




---

Dari hasil konfigurasi yang dilakukan, server berhasil diatur menggunakan BIND9 sebagai DNS server. Alamat domain kelompok5.home dapat di-resolve dengan benar ke IP 192.168.5.10. Semua tahapan mulai dari pengaturan IP, instalasi BIND9, konfigurasi file zona, hingga pengujian dengan dig berjalan sukses tanpa error.

