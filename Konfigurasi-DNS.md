# Laporan Konfigurasi DNS Server di Debian 12

## **Konfigurasi DNS Server untuk Kelompok5**

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

## (01) Konfigurasi untuk Jaringan Internal
Langkah-langkah:
1. Install paket DNS server (bind9):
   ```bash
   sudo apt update
   sudo apt install bind9
   ```
2. Konfigurasi file `/etc/bind/named.conf.options` untuk mendukung jaringan internal:
   ```bash
   options {
       directory "/var/cache/bind";

       recursion yes;
       allow-query { 192.168.1.0/24; }; # ganti dengan IP jaringan internal

       forwarders {
           8.8.8.8;
           8.8.4.4;
       };

       dnssec-validation auto;

       listen-on-v6 { any; };
   };
   ```
3. Restart layanan:
   ```bash
   sudo systemctl restart bind9
   ```

## (02) Konfigurasi untuk Jaringan Eksternal
Langkah-langkah:
1. Tambahkan zona domain publik di `/etc/bind/named.conf.local`:
   ```bash
   zone "example.com" {
       type master;
       file "/etc/bind/db.example.com";
   };
   ```
2. Pastikan firewall dan NAT memperbolehkan port 53 (TCP/UDP) untuk DNS.
3. Sesuaikan setting DNS publik sesuai dengan kebutuhan dan keamanan.

## (03) Konfigurasi Berkas Zona
Langkah-langkah:
1. Duplikat file zona dari template:
   ```bash
   sudo cp /etc/bind/db.local /etc/bind/db.example.com
   ```
2. Edit `/etc/bind/db.example.com`:
   ```bash
   \$TTL    604800
   @       IN      SOA     ns1.example.com. admin.example.com. (
                             2         ; Serial
                        604800         ; Refresh
                         86400         ; Retry
                       2419200         ; Expire
                        604800 )       ; Negative Cache TTL
   ;
   @       IN      NS      ns1.example.com.
   ns1     IN      A       192.168.1.10
   www     IN      A       192.168.1.20
   ```
3. Restart bind:
   ```bash
   sudo systemctl restart bind9
   ```

## (04) Verifikasi Resolusi
Langkah-langkah:
1. Gunakan `dig` untuk memverifikasi:
   ```bash
   dig @localhost example.com
   dig @localhost www.example.com
   ```
2. Gunakan `nslookup`:
   ```bash
   nslookup www.example.com 127.0.0.1
   ```
3. Jika hasil sesuai, berarti konfigurasi berhasil.

---
**Catatan:** Gantilah `example.com`, `192.168.1.10`, dan `192.168.1.20` sesuai kebutuhan jaringan masing-masing.

