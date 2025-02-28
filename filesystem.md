# Chapter 5: The Filesystem

## Pengantar
Filesystem bertujuan untuk merepresentasikan dan mengorganisir sumber daya penyimpanan sistem. Komponen utama filesystem:

1. **Namespace** - Penamaan dan hierarki organisasi.
2. **API** - Sistem panggilan untuk navigasi dan manipulasi.
3. **Model Keamanan** - Perlindungan, penyembunyian, dan berbagi data.
4. **Implementasi** - Perangkat lunak yang menghubungkan model logis dengan perangkat keras.

## Jenis Filesystem
Filesystem terbagi menjadi beberapa kategori utama:
- **Filesystem Berbasis Disk**:
  - **ext4**: Standar filesystem Linux modern.
  - **XFS**: Cocok untuk sistem skala besar dengan performa tinggi.
  - **UFS**: Digunakan pada BSD.
  - **ZFS**: Mendukung snapshot dan kompresi otomatis.
  - **Btrfs**: Filesystem dengan fitur copy-on-write dan deduplikasi.
- **Filesystem Jaringan**:
  - **NFS**: Digunakan untuk berbagi file antar sistem Unix/Linux.
  - **SMB/CIFS**: Digunakan untuk berbagi file dengan Windows.
- **Filesystem Virtual**:
  - **procfs** (`/proc`): Menyediakan akses ke informasi kernel.
  - **sysfs** (`/sys`): Menyediakan informasi tentang perangkat dan driver.
- **Filesystem Arsip**:
  - **ISO 9660**: Digunakan pada CD/DVD.
  - **SquashFS**: Filesystem read-only yang digunakan untuk distribusi perangkat lunak.

## Melihat Informasi Filesystem
Untuk melihat informasi terkait filesystem yang terpasang di sistem, gunakan perintah:
```bash
df -h   # Menampilkan penggunaan disk dalam format yang mudah dibaca
lsblk   # Melihat struktur partisi dan filesystem pada perangkat
mount   # Menampilkan semua filesystem yang saat ini ter-mount
```

## Mounting dan Unmounting
Filesystem dapat dipasang menggunakan perintah `mount`:
```bash
sudo mount /dev/sda4 /mnt
ls /mnt  # Melihat isi direktori setelah dipasang
```
Unmounting dilakukan dengan `umount`:
```bash
sudo umount /mnt
```
Jika filesystem sibuk, gunakan `lsof` atau `fuser` untuk menemukan proses yang menggunakannya:
```bash
lsof /mnt
```

## Struktur Direktori
- **/**: Root filesystem.
- **/boot**: Kernel dan file bootloader.
- **/etc**: Konfigurasi sistem.
- **/sbin, /bin**: Program utilitas penting.
- **/tmp**: File sementara.
- **/dev**: Perangkat virtual.
- **/mnt, /media**: Titik mounting untuk perangkat eksternal.
- **/usr**: Aplikasi dan library pengguna.
- **/var**: File log dan data yang sering berubah.

## Jenis File
Filesystem UNIX mendukung beberapa jenis file:
1. **Regular files**
2. **Directories**
3. **Character device files**
4. **Block device files**
5. **Local domain sockets**
6. **Named pipes (FIFOs)**
7. **Symbolic links**

Menentukan jenis file dengan perintah:
```bash
file /bin/bash
```

## Hak Akses File
File memiliki **mode** yang menentukan hak akses:
- **r** (read), **w** (write), **x** (execute).
- Tiga kelompok: **owner**, **group**, **others**.
- Mengubah hak akses:
```bash
chmod 755 file.txt
```
- Mengubah kepemilikan:
```bash
chown user:group file.txt
```

## Access Control Lists (ACLs)
ACL memungkinkan lebih banyak kontrol terhadap hak akses.
- Melihat ACL:
```bash
getfacl file.txt
```
- Mengubah ACL:
```bash
setfacl -m u:user:rw file.txt
```

## Metadata dan Atribut File
- **Timestamp**: Setiap file memiliki **atime** (access time), **mtime** (modification time), dan **ctime** (change time).
- **Inode**: Setiap file memiliki inode yang menyimpan metadata.
- Melihat informasi inode:
```bash
ls -i file.txt
stat file.txt
```

## Journaling Filesystem
Journaling membantu mencegah kerusakan data saat sistem crash.
- **Ext3/Ext4, XFS, Btrfs, ZFS** mendukung journaling.
- Mengecek status journaling:
```bash
tune2fs -l /dev/sda1 | grep has_journal
```

Filesystem modern terus berkembang dengan fitur tambahan seperti snapshot, deduplikasi, dan enkripsi.
