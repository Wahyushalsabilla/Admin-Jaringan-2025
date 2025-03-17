# LAPORAN ADMINISTRASI JARINGAN

**Dosen Pengampu:**  
Dr. Ferry Astika Saputra, S.T., M.Sc  

**Dikerjakan Oleh:**  
Shalsabilla Wahyu Arifhana - 3123600014  

**DEPARTEMEN TEKNIK INFORMATIKA**  
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**  
2025  

---

## 1. Konfigurasi Internal Network  

### a. Instalasi bind9utils  
![Instalasi bind9utils](images/config-bind/instalasi-bind9utils.png)  

### b. Menambahkan Internal Network dan External Network dalam `/etc/bind/named.conf`  
![Menambahkan internal dan external network](images/config-bind/named-conf.png)  

### c. Atur ACL untuk Internal Network  
![Atur ACL internal network](images/config-bind/config-internal-zones.png)  

### d. Atur Zona Internal  
![Zona internal](images/config-bind/config-zones-internal.png)  

### e. Mengatur `/etc/default/named` ketika perangkat mendukung IPv4  
![Mengatur /etc/default/named](images/config-bind/ipv4.png)  

---

## 2. Konfigurasi External Network  

### a. Menambahkan External Network dalam `/etc/bind/named.conf`  
![Menambahkan external network](images/config-bind/named-conf.png)  

### b. Atur ACL untuk External Network  
![Atur ACL external network](images/config-bind/config-external-zones.png)  

### c. Atur Zona External  
![Zona external](images/config-bind/config-zones-external.png)  

---

## 3. Konfigurasi File Zona  

### a. Buat file zona yang digunakan oleh server untuk menerjemahkan alamat IP dari nama domain dan sebaliknya  
1. Setting file `/etc/bind/sasa.home.lan`

   ![](images/config-bind/config-zone-file1.png)  

2. Setting file `/etc/bind/example.com.zones`

   ![](images/config-bind/config-file-zone2.png)  

### b. Buat file zona yang memungkinkan server menyelesaikan (menentukan) nama domain dari alamat IP dan sebaliknya  
1. Setting file `/etc/bind/0.0.10.db` untuk sasa.home

   ![](images/config-bind/config-file-zone11.png)  

2. Setting file `/etc/bind/2.0.10.db` untuk example.com

   ![](images/config-bind/config-file-zone22.png)  

---

## 4. Verifikasi  

### a. Restart BIND untuk menerapkan perubahan  

```bash
sudo systemctl restart bind9
```

### b. Jalankan perintah:  

Kemudian jalankan perintah : 
```bash
dig dlp.sasa.home
```
Perintah ini untuk mengetes sambungan dengan sasa.home

```bash
dig www.example.com @10.0.2.15
```

Perintah ini untuk menjalankan sambungan tes dengan external network
