# LAPORAN ADMINISTRASI JARINGAN  

**Dosen Pengampu:**  
Dr Ferry Astika Saputra ST, M.Sc  

**Dikerjakan Oleh:**  
Shalsabilla Wahyu Arifhana - 3123600014  

**DEPARTEMEN TEKNIK INFORMATIKA**  
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**  
2025  

---

## 1. Konfigurasi Internal Network  

### a. Instalasi bind9utils  
![Instalasi bind9utils](path/to/image.jpg)  
![Proses Instalasi](path/to/image.jpg)  

### b. Menambahkan internal-network dalam `/etc/bind/named.conf`  
![Menambahkan internal network](path/to/image.jpg)  

### c. Atur ACL untuk internal network  
![Atur ACL internal network](path/to/image.jpg)  

### d. Atur zona internal  
![Zona internal](path/to/image.jpg)  

### e. Mengatur `/etc/default/named` ketika perangkat mendukung IPv4  
![Mengatur /etc/default/named](path/to/image.jpg)  

---

## 2. Konfigurasi External Network  

### a. Menambahkan external-network dalam `/etc/bind/named.conf`  
![Menambahkan external network](path/to/image.jpg)  

### b. Atur ACL untuk internal network  
![Atur ACL external network](path/to/image.jpg)  

### c. Atur zona external  
![Zona external](path/to/image.jpg)  

---

## 3. Konfigurasi File Zona  

### a. Buat file zona yang digunakan oleh server untuk menerjemahkan alamat IP dari nama domain  
![File zona forward](path/to/image.jpg)  

### b. Buat file zona yang memungkinkan server menyelesaikan (menentukan) nama domain dari alamat IP  
![File zona reverse](path/to/image.jpg)  

---

## 4. Verifikasi  

### a. Restart BIND untuk menerapkan perubahan  
![Restart BIND](path/to/image.jpg)  

### b. Jalankan perintah:  
```bash
dig dlp.sasa.home
