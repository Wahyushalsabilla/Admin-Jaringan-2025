# LAPORAN WORKSHOP ADMINISTRASI JARINGAN

**Membuat Container System Sederhana**

**Dosen Pengampu:**
Dr. Ferry Astika Saputra ST, M.Sc

**Dikerjakan Oleh:**
Shalsabilla Wahyu Arifhana - 3123600014

**DEPARTEMEN TEKNIK INFORMATIKA**
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**
**2024**

---

## Instalasi Aplikasi

### Power BI

**Analisis:**
Proses instalasi Power BI Desktop cukup sederhana dan dapat dilakukan langsung melalui situs resmi Microsoft atau Microsoft Store. Power BI menyediakan antarmuka yang user-friendly dan mendukung berbagai sumber data seperti Excel, SQL Server, maupun layanan cloud seperti Google Analytics dan Azure.

![powerbi-install.png](path/to/your/image.png)

### Connector MySQL

**Analisis:**
Power BI tidak bisa langsung mengakses database MySQL tanpa adanya konektor (driver) tambahan yang disebut MySQL Connector/NET. Instalasi ini memungkinkan Power BI untuk mengenali MySQL sebagai salah satu sumber data eksternal. Setelah terinstal, pengguna dapat membuka Power BI Desktop, memilih opsi “Get Data”, lalu memilih “MySQL Database”. Jika konektor belum terdeteksi, Power BI akan menampilkan peringatan bahwa driver belum tersedia.

![mysql-connector.png](path/to/your/image.png)

### Docker Desktop

**Analisis:**
Docker memungkinkan pengembang untuk membangun, menjalankan, dan mengelola aplikasi dalam wadah (container) yang ringan dan portabel. Proses instalasi Docker Desktop cukup mudah dilakukan pada sistem operasi Windows, macOS, maupun Linux, asalkan sistem memenuhi persyaratan minimum, seperti dukungan untuk virtualisasi.

![docker-install.png](path/to/your/image.png)

---

## Buat Folder untuk File SQL yang Disediakan

**Analisis:**
File SQL dimasukkan ke dalam folder yang ada.

![sql-folder.png](path/to/your/image.png)

---

## Menjalankan Container MySQL dengan Docker

**Analisis:**
Perintah `docker run` digunakan untuk menjalankan container MySQL versi 5.7 dengan nama `axon-mysql`, password root `123456`, dan pemetaan port `3306`. Karena image `mysql:5.7` belum tersedia secara lokal, Docker secara otomatis mengunduh image tersebut dari Docker Hub.

![docker-run-container.png](path/to/your/image.png)

---

## Salin File dari Komputer Lokal ke Dalam Container Docker

**Analisis:**
Proses pemindahan file SQL ke dalam container Docker menggunakan perintah `docker cp` bergantung pada path dan nama file yang tepat. Percobaan pertama gagal karena file tidak ditemukan, namun berhasil setelah menggunakan nama file yang benar yaitu `Axon sales - Mysql Database.sql` dan disalin ke dalam container `axon-mysql` sebagai `axon.sql`.

![docker-cp.png](path/to/your/image.png)

---

## Eksekusi Shell dalam Container dan Import Database MySQL

**Analisis:**
Masuk ke container `axon-mysql` menggunakan perintah:

```bash
docker exec -it axon-mysql bash
```

Kemudian mencoba impor file `axon.sql` ke database `classicmodels`, namun gagal karena perintah dijalankan tanpa password. Setelah itu berhasil login dengan:

```bash
mysql -u root -p
```

dan memasukkan password yang sesuai. MySQL siap digunakan untuk proses selanjutnya, termasuk impor data.

![mysql-import.png](path/to/your/image.png)

---

## Membuat Database `classicmodels` dan Menampilkan Daftar Seluruh Database

**Analisis:**
Setelah login ke MySQL, gunakan:

```sql
SHOW databases;
```

Database `classicmodels` muncul bersama dengan database default lainnya seperti `information_schema`, `mysql`, `performance_schema`, dan `sys`.

![show-databases.png](path/to/your/image.png)

---

## Import Model Database

**Analisis:**
Gunakan perintah:

```bash
mysql -u root -p classicmodels < /axon.sql
```

untuk mengimpor data ke database `classicmodels`.

![import-model.png](path/to/your/image.png)

---

## Menampilkan Tabel yang Ada di Database

**Analisis:**
Setelah memilih database `classicmodels`, gunakan perintah:

```sql
SHOW tables;
```

Hasilnya menunjukkan ada 8 tabel: `customers`, `employees`, `offices`, `orderdetails`, `orders`, `payments`, `productlines`, dan `products`.

![show-tables.png](path/to/your/image.png)

---

## Menampilkan Tabel untuk 5 Customer

**Analisis:**
Analisis data pelanggan dilakukan dengan pendekatan klasik, menggunakan query SQL sederhana untuk menghitung jumlah pelanggan per negara atau rata-rata batas kredit. Hasil berupa laporan statis.

![top-customers.png](path/to/your/image.png)

---

## Import Data BI ke Power BI untuk Visualisasi

**Analisis:**
Dashboard Power BI menampilkan analisis penjualan AXON Classical Cars secara visual. Total penjualan: 9,60 juta, profit: 3,83 juta, 122 pelanggan, dan 23 karyawan. Penjualan terbesar dari Amerika Serikat (3,3 juta), dengan tahun terbaik 2004 (4,5 juta). Pelanggan terbanyak dari Eropa (52,46%). Visualisasi ini membantu pengambilan keputusan strategis berbasis data.

![powerbi-dashboard.png](path/to/your/image.png)
