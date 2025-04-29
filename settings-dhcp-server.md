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

---

Dari hasil konfigurasi yang dilakukan, server berhasil diatur menggunakan BIND9 sebagai DNS server. Alamat domain kelompok5.home dapat di-resolve dengan benar ke IP 192.168.5.10. Semua tahapan mulai dari pengaturan IP, instalasi BIND9, konfigurasi file zona, hingga pengujian dengan dig berjalan sukses tanpa error.

