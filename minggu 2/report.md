# Laporan Praktikum Sistem Operasi Jobsheet 2

<h4>Nama : Surya Sadikin Firdaus<h4>
<h4>NIM : 254107020105<h4>
<h4>Kelas : TI-1H<h4>

## Praktikum 2.1 -- Identifikasi CPU dan Memori
Langkah-langkah:
1. Tampilkan Informasi CPU:
```
lscpu
```
![langkah1](img/prak1lang1.png "Langkah No. 1")

2. Tampilkan ringkasan memori:
```
free -h
```
![langkah2](img/prak1lang2.png "Langkah No. 2")

3. (Opsional) cek informasi hardware dari DMI/BIOS (butuh sudo):
```
sudo dmidecode -t system
```
![langkah3](img/prak1lang3.png "Langkah No. 3")


### Pertanyaan Latihan 2.1
Catat:
* 1. Jumlah CPU(s), core/thread
* 2. Total RAM
* 3. Total swap
Jelaskan perbedaan RAM vs swap dalam 2-3 kalimat

### Jawaban Latihan 2.1
* 1. 6 CPU, 1 Core, 1 Thread
* 2. 1,9 GB
* 3. 2 GB
RAM adalah perangkat keras yang digunakan untuk menyimpan data sementara. Sedangkan, swap adalah bentuk lain dari RAM yang menggunakan sebagian storage dari hdd atau ssd untuk dijadikan sebagai RAM. Swap digunakan ketika RAM sudah habis digunakan.


## Praktikum 2.2 -- Identifikasi Perangkat PCI/USB dan Driver

Langkah-langkah:
1. Lihat daftar perangkat PCI:
```
lspci
```
![langkah1](img/prak2lang1.png "Langkah 1")

2. Lihat perangkat PCI beserta driver kernel yang digunakan:
```
lspci -nnk
```
![langkah2](img/prak2lang2.png "Langkah 2")

3. Fokus pada NIC (Ethernet) untuk mencari modul driver:
```
lspci -nnk | grep -A3 -i ethernet
```
![langkah3](img/prak2lang3.png "Langkah 3")

4. Lihat perangkat USB:
```
lsusb
```
![langkah4](img/prak2lang4.png "Langkah 4")

5. lihat topologi USB (tree):
```
lsusb -t
```
![langkah5](img/prak2lang5.png "Langkah 5")

### Pertanyaan Latihan 2.2
Tentukan 1 perangkat PCI (misal NIC) dan tuliskan:
* Vendor: Device ID (angka heksadesimal)
* nama driver/modul kernel
* deskripsi singkat fungsinya

### Jawaban Latihan 2.2
* Ethernet controller [0200]: Intel Corporation 82540EM Gigabit Ethernet Controller [8086:100e]
* Kernel Driver: e100
* Digunakan untuk menyambungkan internet dengan perangkat


## Praktikum 2.3 -- Identifikasi Storage dan Filesystem

Langkah-langkah:
1. Lihat daftar disk/partisi:
```
lsblk -f
```
![langkah1](img/prak3lang1.png "Langkah 1")

2. Tampilkan UUID dan tipe filesystem:
```
sudo blkid
```
![langkah2](img/prak3lang2.png "Langkah 2")

3. Lihat mount point untuk root filesystem
```
findmnt /
```
![langkah3](img/prak3lang3.png "Langkah 3")


## Praktikum 2.4 -- Melihat Modul Aktif dan Informasinya

Langkah-langkah:
1. Cek versi kernel:
```
uname -r
```
![langkah1](img/prak4lang1.png "Langkah 1")

2. Tampilkan daftar modul aktif:
```
lsmod | head
```
![langkah2](img/prak4lang2.png "Langkah 2")

3. Pilih salah satu modul (contoh aman: loop) dan lihat detailnya:
```
modinfo loop
```
![langkah3](img/prak4lang3.png "Langkah 3")

4. Muat modul (jika belum akitf), lalu verifikasi:
```
sudo modprobe loop
lsmod | grep -i loop
```
![langkah4](img/prak4lang4.png "Langkah 4")

5. (Opsional) lihat pesan kernel terbaru:
```
dmesg -T | tail -n 20
```
![langkah4](img/prak4lang5.png "Langkah 5")


## Praktikum 2.5 -- Konfigurasi Auto-load dan Blacklist

Langkah-langkah:
1. Buat file auto-load:
```
echo "loop" | sudo tee /etc/modules-load.d/loop.conf
```
![langkah1](img/prak5lang1.png "Langkah 1")

2. Simulasikan verifikasi (tanpa reboot) dengan memastikan modul sudah aktif
```
lsmod | grep -i loop
```
![langkah2](img/prak5lang2.png "Langkah 2")

3. (Opsional, konsep) blacklist modul:
```
# echo "blacklist loop" | sudo tee /etc/modprobe.d/blacklist-loop.conf
```
![langkah3](img/prak5lang3.png "Langkah 3")


## Praktikum 2.6 -- Mengenali Block vs Character Device

Langkah-langkah:
1. Lihat detail salah satu disk (sesuaikan dengan perangkat Anda, misal sda):
```
ls -l /dev/sda
```
![langkah1](img/prak6lang1.png "Langkah 1")

2. Lihat detail device terminal:
```
ls -l /dev/tty
```
![langkah2](img/prak6lang2.png "Langkah 2")

3. Lihat disk dan partisi untuk mengaitkan dengan /dev:
```
lsblk
```
![langkah3](img/prak6lang3.png "Langkah 3")

### Pertanyaan Latihan 2.3
Dari output ls -l jelaskan perbedaan penanda file untuk block device dan character device. (Hint: karakter pertama pada permission string)

### Jawaban Latihan 2.3
Karakter pertama pada permission string menunjukkan tipe filenya. B untuk Block device, sedangkan C untuk Character device


## Praktikum 2.7 -- Melihat Informasi udev

Langkah-langkah:
1. Cek atribut udev untuk disk:
```
udevadm info --query=all --name=/dev/sda | head -n 30
```
![langkah1](img/prak7lang1.png "Langkah 1")

2. (Opsional) monitor event udev (jalanakan, lalu colok/lepas USB pada mesin fisik):
```
sudo eudevadm monitor
```
![langkah2](img/prak7lang2.png "Langkah 2")


## Praktikum 2.8 -- Membuat Workspace Praktikum

Langkah-langkah:
1. Buat direktori praktikum dan masuk ke dalamnya:
```
mkdir -p ~/praktikum-os/week02
cd ~/praktikum-os/week02
pwd
```
![langkah1](img/prak8lang1.png "Langkah 1")

2. Buat beberapa file contoh:
```
touch notes.txt data.log config.txt
ls -lah
```
![langkah2](img/prak8lang2.png "Langkah 2")

3. Isi file log contoh (simulasi):
```
echo "INFO: service started" >> data.log
echo "WARN: disk usage high" >> data.log
echo "ERROR: failed to connect" >> data.log
cat data.log
```
![langkah3](img/prak8lang3.png "Langkah 3")

4. Baca file dengan less:
```
less data.log
```
![langkah4](img/prak8lang4.png "Langkah 4")


## Praktikum 2.9 -- Pencarian Pola dengan grep

Langkah-langkah:
1. Cari baris yang mengandung ERROR pada data.log:
```
grep "ERROR" data.log
```
![langkah1](img/prak9lang1.png "Langkah 1")

2. Cari tanpa memperhatikan huruf besar/kecil:
```
grep -i "error" data.log
```
![langkah2](img/prak9lang2.png "Langkah 2")

3. Tampilkan nomor baris:
```
grep -n "WARN" data.log
```
![langkah3](img/prak9lang3.png "Langkah 3")

4. Tampilkan baris yang tidak cocok (invert match):
```
grep -v "INFO" data.log
```
![langkah4](img/prak9lang4.png "Langkah 4")

### Pertanyaan Latihan 2.4
Gunakan grep untuk menampilkan hanya baris yang mengandung INFO atau WARN dari data.log (Hint: gunakan grep -E dengan pola alternatif)

### Jawaban Latihan 2.4
![Jawaban](img/jawprak24.png "Jawaban")


## Praktikum 2.10 -- Substitusi dengan sed (Aman di File Latihan)

Langkah-langkah:
1. Siapkan file konfigurasi latihan:
```
cat > config.txt << 'EOF'
PORT=8080
MODE=dev
SERVICE_NAME=myserver
EOF
cat config.txt
```
![langkah1](img/prak10lang1.png "Langkah 1")

2. Ganti dev menjadi prod (tanpa mengubah file asli):
```
sed 's/MODE=dev/MODE=prod/' config.txt
```
![langkah2](img/prak10lang2.png "Langkah 2")

3. Terapkan perubahan langsung ke file (-i):
```
sed -i 's/MODE=dev/MODE=prod/' config.txt
cat config.txt
```
![langkah3](img/prak10lang3.png "Langkah 3")

4. Ganti semua kemunculan kata (g untuk global), contoh ubah myserver menjadi node:
```
sed -i 's/myserver/node/g' config.txt
cat config.txt
```
![langkah4](img/prak10lang4.png "Langkah 4")


## Praktikum 2.11 -- Ekstraksi Kolom dengan awk

Langkah-langkah:
1. Lihat output df -h
```
df -h
```
![langkah1](img/prak11lang1.png "Langkah 1")

2. Ambil kolom filesystem dan persentase pemakaian:
```
df -h | awk 'NR==1 {print $1, $5, $6} NR>1 {print $1, $5, $6}'
```
![langkah2](img/prak11lang2.png "Langkah 2")

3. Filter hanya yang pemakaian di atas 80%:
```
df -h | awk 'NR==1 || ($5+0) > 80 {print $1. $5. $6}'
```
![langkah3](img/prak11lang3.png "Langkah 3")


## Praktikum 2.12 -- Melihat Proses dengan ps

Langkah-langkah:
1. Tampilkan semua proses (format BSD):
```
ps aux | head
```
![langkah1](img/prak12lang1.png "Langkah 1")

2. Cari proses tertentu (misal sshd):
```
ps aux | grep -i sshd
```
![langkah2](img/prak12lang2.png "Langkah 2")


## Praktikum 2.13 -- Monitoring Real-time dengan top

Langkah-langkah:
1. Jalankan top:
```
top
```
![langkah1](img/prak13lang1.png "Langkah 1")

2. Amati nilai load average, pemakaian CPU, dan proses teratas. Tekan q untuk keluar


## Praktikum 2.14 -- Menghentikan Proses dengan kill

Langkah-langkah:
1. Jalankan proses dummy di background:
```
sleep 300 &
```
![langkah1](img/prak14lang1.png "Langkah 1")

2. Cari PID proses sleep:
```
ps aux | grep -E "sleep 300" | grep -v grep
```
![langkah2](img/prak14lang2.png "Langkah 2")

3. Hentikan dengan SIGTERM:
```
kill <PID_ANDA>
```
![langkah3](img/prak14lang3.png "Langkah 3")

4. Verifikasi proses berhenti:
```
ps aux | grep -E "sleep 300" | grep -v grep
```
![langkah4](img/prak14lang4.png "Langkah 4")

5. (Opsional) Jika proses sulit untuk diberhentikan dan Anda membutuhkan untuk menghentikan proses tersebut, gunakan SIGKILL:
```
kill -9 <PIP_ANDA>
```


## Praktikum 2.15 -- Cek Disk, Load, dan Service

Langkah-langkah:
1. Cek penggunaan disk:
```
df -h
```
![langkah1](img/prak15lang1.png "Langkah 1")

2. Cari direktori yang besar (contoh pada /var):
```
sudo du -sh /var/* 2>/dev/null | sort -h | tail -n 10
```
![langkah2](img/prak15lang2.png "Langkah 2")

3. Cek load dan uptime:
```
uptime
```
![langkah3](img/prak15lang3.png "Langkah 3")

4. Cek service yang gagal:
```
systemctl --failed
```
![langkah4](img/prak15lang4.png "Langkah 4")

5. Ambil log error terbaru (jika ada indikasi masalah):
```
journalctl -xe | tail -n 50
```
![langkah5](img/prak15lang5.png "Langkah 5")


## Praktikum 2.16 -- Monitoring Port dan Koneksi (Network Basic)

Langkahl-langkah:
1. Lihat interface dan IP:
```
ip a
```
![langkah1](img/prak16lang1.png "Langkah 1")

2. Lihat routing table:
```
ip r
```
![langkah2](img/prak16lang2.png "Langkah 2")

3. Lihat port yang sedang listening:
```
sudo ss -tulpn
```
![langkah3](img/prak16lang3.png "Langkah 3")

### Pertanyaan Latihan 2.5
Pilih salah satu port yang listerning dari output ss -tulpn(misal port 22), lalu tuliskan service/proses yang membukanya. Jelaskan kegunaan port tersebut secara singkat

### Jawaban Latihan 2.5
Saya memilih port 22, service yang membukanya adalah sshd. sshd (Secure Shell Daemon) berfungsi untuk digunakan untuk remote login, mengakses dan mengelola server dari jarak jauh secara aman dan terenkripsi


## 1. 9 Latihan

### Pertanyaan 2.A
Jalankan lspci -nnk. Pilih 1 perangkan PCI dan tuliskan: nama perangkat, ID vendor:device, dan kernel driver in use

### Jawaban 2.A
![jawaban](img/lat2a.png "Screenshot jawaban 2A")
Nama perangkat          : Ethernet Controller (Intel 82540EM)
ID vendor:device        : 8086:100e
Kernel driver in use    : e1000

### Pertanyaan 2.B
Tentukan device root filesystem dengan fndmnt /. Lalu cocokkan dengan lsblk -f dan tuliskan tipe filesystem serta UUID-nya

### Jawaban 2.B
![jawaban](img/lat2b.png "Screenshot jawaban 2B")
Device root filysytem berada di sda3 dengan nama ubuntu--vg-ubuntu--lv. Tipe filesystemnya adalah ext4 dan UUIDnya adalah e476159e-ad73-4bee-bb75-bd6aa8972b6d

### Pertanyaan 2.C
Buat file server.log berisi minimal 10 baris dengan variasi kata: INFO, WARN, ERROR. gunakan grep untuk menampilkan hanya baris ERROR.

### Jawaban 2.C
![jawaban](img/lat2c.png "Screenshot jawaban 2C")

### Pertanyaan 2.D
Gunakan sed untuk mengganti semua kata server menjadi node pada file latihan. Tunjukkan sebelum dan sesudah.

### Jawaban 2.D
![jawaban](img/lat2d.png "Screenshot jawaban 2D")

### Pertanyaan 2.E
Gunakan df -h lalu awk untuk menamplkan filesystem yang penggunan disk di atas 70%

### Jawaban 2.E
![jawaban](img/lat2e.png "Screenshot jawaban 2E")

### Pertanyaan 2.F
Jalankan sleep 600 &. Tentukan PID-nya dengan ps. Hentikan dengan SIGTERM. jelaskan beda SIGTERM vs SIGKILL

### Jawaban 2.F
![jawaban](img/lat2f.png "Screenshot jawaban 2F")
Perbedaan antara SIGTERM dan SIGKILL adalah SIGKILL akan langsung dieksekusi. Jika dianalogikan sebagai perintah maka SIGTERM adalah perintah yang sopan yang masih bisa dinego, tetapi jika SIGKILL adalah perintah mutlak yang tidak bisa dibantah

### Pertanyaan 2.G
Gunakan systemctl --failed. Jika tidak ada yang gagal, pilih salah satu service aktif (misal ssh) dan ampikan status serta 30 baris log terkahirnya.

### Jawaban 2.G
![jawaban](img/lat2g1.png "Screenshot jawaban 2G")
![jawaban](img/lat2g2.png "Screenshot jawaban 2G")

