# **Pengelolaan Proses di Unix/Linux**

## **Apa Itu Proses?**
Di sistem operasi Unix/Linux, **proses** adalah program yang sedang berjalan. Setiap aplikasi yang kita jalankan, seperti terminal, browser, atau server, adalah proses yang bekerja di dalam sistem.

Proses memiliki berbagai atribut seperti **ID unik (PID)**, status, prioritas, serta sumber daya yang digunakannya (CPU, memori, dan file). Sistem operasi mengelola proses agar dapat berjalan secara bersamaan (multitasking) tanpa mengganggu satu sama lain.

## **Bagaimana Proses Dikelola di Linux?**
Linux menyediakan berbagai alat dan perintah untuk melihat, mengontrol, dan mengelola proses. Dalam sistem, ada **dua jenis proses utama**:
1. **Foreground Process** → Berjalan di depan layar dan langsung berinteraksi dengan pengguna. Contohnya: mengetik perintah di terminal.
2. **Background Process** → Berjalan di belakang layar tanpa campur tangan pengguna langsung, seperti daemon atau service sistem.

---

## **1. Melihat Daftar Proses dengan `ps`**
Untuk mengetahui proses apa saja yang sedang berjalan, kita bisa menggunakan perintah **`ps`**:
- **`ps aux`** → Menampilkan semua proses dengan detail seperti pengguna, PID, dan status.
- **`ps lax`** → Menampilkan informasi teknis tambahan.
- **`ps aux | grep [nama_proses]`** → Mencari proses tertentu berdasarkan nama.

---

## **2. Siklus Hidup Proses**
Setiap proses melewati beberapa tahap dalam siklus hidupnya:
1. **Dibuat (`fork`)** → Proses baru dibuat dengan menyalin proses induk.
2. **Berjalan (`Running`)** → Proses sedang dieksekusi oleh CPU.
3. **Menunggu (`Sleeping`)** → Proses menunggu event tertentu (misalnya input dari pengguna).
4. **Dihentikan (`Stopped`)** → Proses berhenti sementara.
5. **Dihapus (`Terminated`)** → Proses berakhir dan dihapus dari memori.

Proses pertama di Linux adalah **`init` atau `systemd` (PID 1)** yang mengatur proses lain saat sistem dinyalakan.

---

## **3. Mengontrol Proses dengan Sinyal**
Linux menggunakan **sinyal (signals)** untuk berkomunikasi dengan proses. Beberapa sinyal yang sering digunakan:
- **`SIGKILL` (`kill -9 PID`)** → Menghentikan proses secara paksa.
- **`SIGINT` (`Ctrl+C`)** → Menghentikan proses dari terminal.
- **`SIGTERM` (`kill PID`)** → Meminta proses berhenti dengan baik.
- **`SIGHUP`** → Digunakan untuk me-restart daemon atau proses background.

Selain sinyal di atas, terdapat berbagai sinyal lain yang digunakan dalam sistem Linux. Berikut adalah daftar lengkap 31 sinyal yang tersedia dalam sistem operasi Linux beserta deskripsinya:

| #  | Signal Name | Default Action | Comment | POSIX |
|----|------------|---------------|---------|-------|
| 1  | SIGHUP    | Terminate     | Hang up controlling terminal or process | Yes |
| 2  | SIGINT    | Terminate     | Interrupt from keyboard | Yes |
| 3  | SIGQUIT   | Dump          | Quit from keyboard | Yes |
| 4  | SIGILL    | Dump          | Illegal instruction | Yes |
| 5  | SIGTRAP   | Dump          | Breakpoint for debugging | No |
| 6  | SIGABRT   | Dump          | Abnormal termination | Yes |
| 7  | SIGIOT    | Dump          | Equivalent to SIGABRT | No |
| 8  | SIGBUS    | Dump          | Bus error | No |
| 9  | SIGFPE    | Dump          | Floating-point exception | Yes |
| 10 | SIGKILL   | Terminate     | Forced-process termination | Yes |
| 11 | SIGUSR1   | Terminate     | Available to processes | Yes |
| 12 | SIGSEGV   | Dump          | Invalid memory reference | Yes |
| 13 | SIGUSR2   | Terminate     | Available to processes | Yes |
| 14 | SIGPIPE   | Terminate     | Write to pipe with no readers | Yes |
| 15 | SIGALRM   | Terminate     | Real-timeclock | Yes |
| 16 | SIGTERM   | Terminate     | Process termination | Yes |
| 17 | SIGSTKFLT | Terminate     | Coprocessor stack error | No |
| 18 | SIGCHLD   | Ignore        | Child process stopped or terminated | Yes |
| 19 | SIGCONT   | Continue      | Resume execution, if stopped | Yes |
| 20 | SIGSTOP   | Stop          | Stop process execution | Yes |
| 21 | SIGTSTP   | Stop          | Stop process issued from tty | Yes |
| 22 | SIGTTIN   | Stop          | Background process requires input | Yes |
| 23 | SIGTTOU   | Stop          | Background process requires output | Yes |
| 24 | SIGURG    | Ignore        | Urgent condition on socket | No |
| 25 | SIGXCPU   | Dump          | CPU time limit exceeded | No |
| 26 | SIGXFSZ   | Dump          | File size limit exceeded | No |
| 27 | SIGVTALRM | Terminate     | Virtual timer clock | No |
| 28 | SIGPROF   | Terminate     | Profile timer clock | No |
| 29 | SIGWINCH  | Ignore        | Window resizing | No |
| 30 | SIGIO     | Terminate     | I/O now possible | No |
| 31 | SIGPOLL   | Terminate     | Equivalent to SIGIO | No |
| 32 | SIGPWR    | Terminate     | Power supply failure | No |


---

## **4. Mengirim Sinyal dengan `kill`**
- **`kill -9 [PID]`** → Mematikan proses secara paksa.
- **`killall [nama_proses]`** → Menghentikan semua proses dengan nama tertentu.
- **`pkill [nama_proses]`** → Mirip `killall`, tapi bisa mencari proses berdasarkan pola nama.

---

## **5. Menjalankan Proses Secara Otomatis**
Ada dua metode utama untuk menjadwalkan proses berjalan otomatis:
- **`cron`** → Menjalankan perintah atau skrip pada jadwal tertentu.
- **`systemd timers`** → Alternatif modern untuk `cron`, digunakan untuk menjadwalkan layanan sistem.

Contoh entri di crontab untuk menjalankan skrip setiap hari pukul 12:
```sh
0 12 * * * /home/user/script.sh
```

---

## **6. `/proc` – Tempat Informasi Proses**
Linux memiliki **filesystem virtual** bernama `/proc`, yang menyimpan informasi tentang semua proses yang berjalan.
- **`/proc/PID/cmdline`** → Menampilkan perintah yang digunakan untuk menjalankan proses.
- **`/proc/PID/status`** → Menampilkan status detail dari proses.
- **`/proc/PID/maps`** → Menunjukkan penggunaan memori proses.

---

## **7. Debugging Proses dengan `strace`**
- **`strace`** → Melacak sistem panggilan (syscalls) dari proses di Linux.
- **`truss`** → Alternatif `strace` untuk FreeBSD.
- Contoh penggunaan `strace`:
  ```sh
  strace -p [PID]
  ```
  Ini akan menunjukkan sistem panggilan apa saja yang dilakukan proses tertentu.

---

## **8. Menghentikan Proses yang Tidak Merespons**
Kadang, proses bisa mengalami hang atau menggunakan terlalu banyak CPU.
- **Cek dengan `top` atau `htop`**
- **Bunuh dengan `kill -9 PID`**
- **Gunakan `strace` untuk melihat masalahnya**

---

## **9. Memantau Proses Secara Real-Time**
- **`top`** → Menampilkan proses dengan detail pemakaian CPU dan memori.
- **`htop`** → Alternatif dengan tampilan lebih interaktif.

---

## **10. Mengatur Prioritas Proses (`nice` & `renice`)**
Setiap proses memiliki prioritas yang menentukan seberapa sering mendapat waktu CPU.
- **`nice`** → Menjalankan proses dengan prioritas tertentu.
  ```sh
  nice -n 10 myprogram
  ```
- **`renice`** → Mengubah prioritas proses yang sudah berjalan.
  ```sh
  renice -n 5 -p 1234  # Mengubah prioritas proses dengan PID 1234
  ```
- **Nilai `nice` berkisar dari -20 (prioritas tinggi) hingga 19 (prioritas rendah).**

---

