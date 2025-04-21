# **LAPORAN WORKSHOP ADMINISTRASI JARINGAN**

## **Konfigurasi NAT**

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

# **Konfigurasi Jaringan: Menghubungkan VM2 ke Internet Melalui VM1**

Panduan ini menjelaskan langkah-langkah untuk mengatur agar **VM2 dapat terhubung ke internet melalui VM1**. VM1 berperan sebagai gateway dan NAT untuk VM2.

---

## 🖥️ **Langkah-Langkah di VM1 (Sebagai Gateway dan DNS)**

### **1. Konfigurasi IP Forwarding**

Aktifkan IP forwarding secara sementara:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Agar permanen, tambahkan baris berikut ke `/etc/sysctl.conf`:

```bash
net.ipv4.ip_forward=1
```

Kemudian jalankan:

```bash
sysctl -p
```

---

### **2. Konfigurasi NAT (iptables)**

Asumsikan:
- `enp0s3` = interface ke internet
- `enp0s8` = interface ke VM2

Jalankan:

```bash
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

Agar iptables tetap berlaku setelah reboot:

```bash
apt install iptables-persistent
netfilter-persistent save
netfilter-persistent reload
```

---

### **3. (Opsional) Konfigurasi BIND9 Sebagai DNS Server**

Edit file `/etc/bind/named.conf.options`:

```bash
options {
    directory "/var/cache/bind";

    recursion yes;
    allow-query { any; };
    forwarders {
        8.8.8.8;
        1.1.1.1;
    };
};
```

Restart layanan BIND:

```bash
systemctl restart bind9
```

---

## 💻 **Langkah-Langkah di VM2 (Client)**

### **4. Konfigurasi Default Gateway**

Arahkan default gateway ke IP VM1:

```bash
ip route add default via 192.168.200.1
```

Pastikan konfigurasi gateway juga ada di:
- `/etc/network/interfaces` (jika pakai interfaces)
- atau `/etc/netplan/*.yaml` (jika pakai Netplan)

---

### **5. Konfigurasi DNS**

Edit file `/etc/resolv.conf`:

```bash
nameserver 192.168.200.1
```

Jika menggunakan `systemd-resolved`, edit `/etc/systemd/resolved.conf`:

```ini
[Resolve]
DNS=192.168.200.1
FallbackDNS=8.8.8.8
```

Restart service:

```bash
systemctl restart systemd-resolved
```

---

## ✅ **Uji Koneksi dari VM2**

Cek koneksi ke internet:

```bash
ping 8.8.8.8
```

Cek apakah DNS berfungsi:

```bash
ping google.com
```

---

Dengan mengikuti langkah-langkah ini, VM2 seharusnya sudah bisa mengakses internet melalui VM1 yang bertindak sebagai router dan DNS forwarder. Pastikan semua konfigurasi disesuaikan dengan interface dan IP yang digunakan di masing-masing VM.

