# Laporan Praktikum Sistem Operasi Jobsheet 6

<h4>Nama : Surya Sadikin Firdaus<h4>
<h4>NIM : 254107020105<h4>
<h4>Kelas : TI-1H<h4>

## Tugas Praktikum 1 -  Script Pertama: Laporan Sistem
1. Buat workspace praktikum:
```
mkdir -p ~/praktikum-os/week09/{scripts, logs, data}
cd ~/praktikum-os/week09/scripts
```
![steps](img/prak1lang1.png "Langkah ke-1")

2. Buat script dengan nano:
```
nano laporan-sistem.sh
```
![steps](img/prak1lang2.png "Langkah ke-2")

3. Ketik isi berikut, simpan (Ctrl+O Enter), lalu keluar (Ctrl+X):
```
#!/bin/bash
# Script: laporan-sistem.sh

echo "================================"
echo " LAPORAN SISTEM"
echo "================================"
echo "Tanggal : $(date '+%A, %d %B %Y')"
echo "Jam : $(date '+%H:%M:%S')"
echo "Hostname : $(hostname)"
echo "User : $(whoami)"
echo "CPU core : $(nproc)"
echo "RAM bebas: $(free -h | awk '/^Mem/ {print $4}')"
echo "Disk / : $(df -h / | awk 'NR==2 {print $5}')
terpakai"
echo "================================"
```
![steps](img/prak1lang3.png "Langkah ke-3")

4. Beri izin dan jelaskan:
```
chmod +x laporan-sistem.sh
./laporan-sistem.sh
```
![steps](img/prak1lang4.png "Langkah ke-4")

### Pertanyaan Latihan 9.1
Modifikasi laporan-sistem.sh agar menyimpan output ke file laporan-YYYY-MM-DD.txt sekaligus menampilkannya di terminal. Petunjuk:gunakan tee yang sudah dipelajari di bab sebelumnya.

### Jawaban Latihan 9.1
![jawaban](img/lat1.png "Jawaban")


## Praktikum 2 - Script Info Sistem dengan Argumen
1. Buat script:
```
nano ~/praktikum-os/week09/scripts/info-sistem.sh
```
![steps](img/prak2lang1.png "Langkah ke-1")

2. Ketik isi berikut:
```
#!/bin/bash
# Penggunaan: ./info-sistem.sh [nama-admin] [batas-
disk-persen]

ADMIN=${1:-"Tidak dikenal"}
BATAS=${2:-80}
TANGGAL=$(date '+%F %T')
DISK_PERSEN=$(df / | awk 'NR==2 {print $5}' | tr -d '%')

echo "Admin : $ADMIN"
echo "Tanggal : $TANGGAL"
echo "Disk / : ${DISK_PERSEN}% terpakai"
echo "Batas : ${BATAS}%"

if [ "$DISK_PERSEN" -gt "$BATAS" ]; then
    echo "STATUS : PERINGATAN - disk melebihi batas!"
else
    SISA=$((BATAS - DISK_PERSEN))
    echo "STATUS : Normal (sisa toleransi ${SISA}%)"
fi
```
![steps](img/prak2lang2.png "Langkah ke-2")

3. Simpan, beri izin, uji dengan berbagai kombinasi argumen
```
chmod +x ~/praktikum-os/week09/scripts/info-sistem.sh
./info-sistem.sh
./info-sistem.sh "Dian" 50
./info-sistem.sh "Dian" 10
```
![steps](img/prak2lang3.png "Langkah ke-3")

### Pertanyaan Latihan 9.2
Buat script kalkulator.sh yang menerima tiga argumen: <angka1> <operator> <angka2> dengan operator +, -, *, atau /. Contoh:./kalkulator.sh 20 + 5 menghasilkan 25. Gunakan case untuk memilih operasi, dan validasi jika argumen tidak lengkap.

### Jawaban Latihan 9.2
![jawaban](img/codekalkulator.png "Code")
![jawaban](img/hasilkalkulator.png "Hasil")


## Praktikum 3 - Script Grading dan Menu Interaktif
1. Buat script grading (menggunakan if dan for):
```
nano ~/praktikum-os/week09/scripts/grading-batch.sh
```
![steps](img/prak3lang1.png "Langkah ke-1")

2. Ketik isi berikut: 
```
#!/bin/bash
# Script: grading-batch.sh
# Proses daftar nilai mahasiswa

MAHASISWA=("Andi:92" "Budi:73" "Citra:55" "Deni:80" Eka:45")

echo "=== HASIL GRADING ==="
for ENTRI in "${MAHASISWA[@]}"; do
NAMA=$(echo "$ENTRI" | cut -d: -f1)
NILAI=$(echo "$ENTRI" | cut -d: -f2)

if [ "$NILAI" -ge 85 ]; then
GRADE="A"
elif [ "$NILAI" -ge 75 ]; then
GRADE="B"
elif [ "$NILAI" -ge 65 ]; then
GRADE="C"
elif [ "$NILAI" -ge 55 ]; then
GRADE="D"
else
GRADE="E"
fi

printf "%-10s %3d %s\n" "$NAMA" "$NILAI" "$GRADE"

done
echo "====================="
```
![steps](img/prak3lang2.png "Langkah ke-2")

3. Simpan, beri izin, dan jalankan:
```
chmod +x ~/praktikum-os/week09/scripts/grading-batch.sh
./grading-batch.sh
```
![steps](img/prak3lang3.png "Langkah ke-3")

4. Buat script menu interaktif (while + case): 
```
nano ~/praktikum-os/week09/scripts/menu-sistem.sh
```
![steps](img/prak3lang4.png "Langkah ke-4")

5. Ketik isi berikut:
```
#!/bin/bash
# Menu interaktif pemantauan sistem

while true; do
echo ""
echo "===== MENU MONITOR ====="
echo "1) Info disk"
echo "2) Info memori"
echo "3) Proses teratas"
echo "4) Keluar"
echo -n "Pilih [1-4]: "
read PILIHAN
case $PILIHAN in
1) df -h ;;
2) free -h ;;
3) ps aux --sort=-%cpu | head -6 ;;
4) echo "Sampai jumpa!"; exit 0 ;;
*) echo "Pilihan tidak valid." ;;
esac

done
```
![steps](img/prak3lang5.png "Langkah ke-5")

6. Beri izin dan jalankan, coba setiap opsi:
```
chmod +x ~/praktikum-os/week09/scripts/menu-sistem.sh
./menu-sistem.sh
```
![steps](img/prak3lang6.png "Langkah ke-6")

### Pertanyaan Latihan 9.3
Tambahkan ke script grading-batch.sh sebuah ringkasan di bagian bawah yang menampilkan: jumlah mahasiswa per grade (A, B, C, D, E) menggunakan perulangan for kedua yang mengiterasi array MAHASISWA.

### Jawaban Latihan 9.3
![answer](img/modgrading.png "Modifikasi grading-batch.sh")


## Praktikum 4 - Library Fungsi Validasi
1. Buat file library:
```
nano ~/praktikum-os/week09/scripts/lib-validasi.sh
```
![steps](img/prak4lang1.png "Langkah ke-1")

2. Ketik isi berikut:
```
#!/bin/bash
# lib-validasi.sh - Library fungsi validasi

adalah_angka() {
    [[ "$1" =~ ^[0-9]+$ ]]
}

file_bisa_dibaca() {
    [ -f "$1" ] && [ -r "$1" ]
}

error_exit() {
    echo "ERROR: $1" >&2
    exit 1
}

info() { echo "[INFO] $1"; }
sukses() { echo "[OK] $1"; }
```
![steps](img/prak4lang2.png "Langkah ke-2")

3. Buat script yang menggunakan library:
```
nano ~/praktikum-os/week09/scripts/pakai-library.sh
```
![steps](img/prak4lang3.png "Langkah ke-3")

4. Ketik isi berikut: 
```
#!/bin/bash
# Muat library (seperti import di Java)
source ~/praktikum-os/week09/scripts/lib-validasi.sh

ANGKA=$1
FILE=$2

[ -z "$ANGKA" ] || [ -z "$FILE" ] && \error_exit "Penggunaan: $0 <angka> <path-file>"

if adalah_angka "$ANGKA"; then
    sukses "Input '$ANGKA' adalah angka valid"
else
    error_exit "'$ANGKA' bukan angka"
fi

if file_bisa_dibaca "$FILE"; then
    sukses "File '$FILE' bisa dibaca"
    info "Jumlah baris: $(wc -l < "$FILE")"
else
    error_exit "File '$FILE' tidak ada atau tidak bisa dibaca"
fi
```
![steps](img/prak4lang4.png "Langkah ke-4")

5. Beri izin dan uji semua skenario:
```
chmod +x ~/praktikum-os/week09/scripts/pakai-library.sh
./pakai-library.sh 42 /etc/hostname
./pakai-library.sh abc /etc/hostname
./pakai-library.sh 42 /tidak-ada.txt
./pakai-library.sh
```
![steps](img/prak4lang5.png "Langkah ke-5")

### Pertanyaan Latihan 9.4
Tambahkan fungsi konfirmasi() ke lib-validasi.sh. Fungsi ini menampilkan pertanyaan, membaca input Y/N dari user, mengembalikan 0 jika Y dan 1 jika N. Buat script demo yang memanggil fungsi ini sebelum menghapus sebuah file.

### Jawaban Latihan 9.4
![answer](img/fungConfirm.png "Fungsi Konfirmasi")
![answer](img/scriptDemo.png "Script Demo")
![answer](img/implementasi.png "Percobaan pengunaan script dan fungsi")

## Praktikum 5 - Script Backup dengan Opsi
1. Buat script:
```
nano ~/praktikum-os/week09/scripts/backup-data.sh
```
![steps](img/prak5lang1.png "Langkah ke-1")

2. Ketik isi berikut:
```
#!/bin/bash
# Penggunaan: ./backup-data.sh [-v] [-c] [-l logfile] <sumber> <tujuan>

VERBOSE=false
COMPRESS=false
LOG_FILE=""

while getopts "vcl:" OPSI; do
case $OPSI in
    v) VERBOSE=true ;;
    c) COMPRESS=true ;;
    l) LOG_FILE="$OPTARG" ;;
    *) echo "Penggunaan: $0 [-v] [-c] [-l logfile] <sumber> <tujuan>"
    exit 1 ;;
esac
done
shift $((OPTIND - 1))

SUMBER=$1
TUJUAN=$2

log() {
    local MSG="[$(date '+%T')] $1"
    echo "$MSG"
    [ -n "$LOG_FILE" ] && echo "$MSG" >> "$LOG_FILE"
}

[ -z "$SUMBER" ] || [ -z "$TUJUAN" ] && { echo "ERROR: sumber dan tujuan wajib diisi"; exit 1; }

[ ! -d "$SUMBER" ] && { log "ERROR: $SUMBER tidak ada"; exit 2; }

mkdir -p "$TUJUAN"
TANGGAL=$(date '+%F-%H%M%S')
[ "$VERBOSE" = true ] && log "Sumber: $SUMBER | Tujuan: $TUJUAN"

if [ "$COMPRESS" = true ]; then
    ARSIP="$TUJUAN/backup-$(basename "$SUMBER")-$TANGGAL.tar.gz"
    tar -czf "$ARSIP" -C "$(dirname "$SUMBER")" "$(basename "$SUMBER")"
    log "Arsip: $ARSIP ($(du -sh "$ARSIP" | cut -f1))"
else
    cp -r "$SUMBER" "$TUJUAN/backup-$(basename "$SUMBER")-$TANGGAL"
    log "Backup selesai."
fi
```
![steps](img/prak5lang2.png "Langkah ke-2")

3. Beri izin dan uji:
```
chmod +x ~/praktikum-os/week09/scripts/backup-data.sh
cd ~/praktikum-os/week09/scripts

# Tanpa opsi
./backup-data.sh ~/praktikum-os/week09/data ~/praktikum-os/week09/logs

# Dengan verbose dan kompresi + log ke file
./backup-data.sh -v -c -l ../logs/backup.log \~/praktikum-os/week09/data ~/praktikum-os/week09/logs

cat ../logs/backup.log
```
![steps](img/prak5lang3.png "Langkah ke-3")

## Praktikum 6: Debugging Script

1. Buat script untuk dianalisi:
```
nano ~/praktikum-os/week09/scripts/debug-latihan.sh
```
![steps](img/prak6lang1.png "Langkah ke-1")

2. Ketik isi berikut:
```
#!/bin/bash
# Script: debug-latihan.sh
# Penggunaan: ./debug-latihan.sh <direktori> <batas-MB>

DIREKTORI=$1
BATAS=$2

if [ $# -ne 2 ]; then
    echo "Penggunaan: $0 <direktori> <batas-MB>"
    exit 1
fi

UKURAN=$(du -sm "$DIREKTORI" | cut -f1)

echo "Direktori : $DIREKTORI"
echo "Ukuran : ${UKURAN} MB"
echo "Batas : ${BATAS} MB"

if [ "$UKURAN" -gt "$BATAS" ]; then
    echo "PERINGATAN: Ukuran melebihi batas!"
    echo "Kelebihan: $((UKURAN - BATAS)) MB"
else
    echo "Status: Normal (sisa: $((BATAS - UKURAN)) MB)"
fi
```
![steps](img/prak6lang2.png "Langkah ke-2")

3. Cek sintaks, lalu jalankan dengan tracing:
```
chmod +x ~/praktikum-os/week09/scripts/debug-latihan.sh
bash -n debug-latihan.sh && echo "Sintaks OK"
bash -x debug-latihan.sh /etc 10
./debug-latihan.sh /var 50
./debug-latihan.sh
```
![steps](img/prak6lang3.png "Langkah ke-3")

### Pertanyaan Latihan 9.5
Script debug-latihan.sh tidak menangani direktori yang tidak ada. Perbaiki dengan menambahkan:
* set -e di baris kedua
* Pengecekan -d "$DIREKTORI" sebelum memanggil du
* Pesan error yang informatif jika direktori tidak ditemukan
Uji dengan direktori yang tidak ada.

### Jawaban Latiham 9.5
![steps](img/modDebug.png "Modifikasi script debug-latihan.sh")


## Tugas Praktikum
1. Tugas 1 Script Absensi Kelas
Konteks: instruktur mencatat kehadiran mahasiswa dari command line
Intruksi:
    a. Buat script absensi.sh yang:
        - Menerima argumen nama mahasiswa dan status (hadir/izin.alpha)
        - Menympan enyti ke absensi-YYYY-MM-DD.txt dengan format [HH:MM] NAMA - STATUS
        - Opsi -r: tampilkan rekapitulasi (jumlah per status)
        - Opsi -h: tampilkan bantuan
    
    b. Rekam minimal 5 entri dan tampilkan rekapitulasinya
Konsep wajib: variabel, parameter posisional, getopts, if, for, fungsi, dan redirection ke file

```
#!/bin/bash
# absensi.sh - Script absensi kelas

FILE="absensi-$(date +%Y-%m-%d).txt"

bantuan() {
    echo "Penggunaan: $0 [opsi] [nama] [status]"
    echo ""
    echo "Status: hadir | izin | alpha"
    echo ""
    echo "Opsi:"
    echo "  -r    Tampilkan rekapitulasi"
    echo "  -h    Tampilkan bantuan ini"
}

rekapitulasi() {
    if [ ! -f "$FILE" ]; then
        echo "Belum ada data absensi hari ini."
        exit 0
    fi

    echo "=== Rekapitulasi: $FILE ==="
    echo "Hadir : $(grep -c "HADIR" "$FILE" 2>/dev/null || echo 0)"
    echo "Izin  : $(grep -c "IZIN" "$FILE" 2>/dev/null || echo 0)"
    echo "Alpha : $(grep -c "ALPHA" "$FILE" 2>/dev/null || echo 0)"
    echo "Total : $(wc -l < "$FILE")"
}

catat_absensi() {
    local nama="$1"
    local status="$2"

    # validasi status
    case "$status" in
        hadir|izin|alpha) ;;
        *)
            echo "ERROR: Status tidak valid. Gunakan: hadir / izin / alpha" >&2
            exit 1
            ;;
    esac

    local waktu=$(date +%H:%M)
    local status_upper=$(echo "$status" | tr '[:lower:]' '[:upper:]')

    echo "[$waktu] $nama - $status_upper" >> "$FILE"
    echo "Tercatat: [$waktu] $nama - $status_upper"
}

# --- parse opsi ---
while getopts "rh" opt; do
    case "$opt" in
        r) rekapitulasi; exit 0 ;;
        h) bantuan; exit 0 ;;
        *) bantuan; exit 1 ;;
    esac
done

shift $((OPTIND - 1))

# --- cek argumen ---
if [ $# -ne 2 ]; then
    bantuan
    exit 1
fi

catat_absensi "$1" "$2"

# --- auto rekap kalau sudah 5 entri ---
total=$(wc -l < "$FILE")
if [ "$total" -ge 5 ]; then
    echo ""
    rekapitulasi
fi
```
![steps](img/penggunaanAbsensi.png "Contoh penggunaan")


2. Tugas 2 Script Health Check Sistem
Konteks: administrator membuat pemeriksaan kaondisi server sebelum maintenance
Intruksi: 
    a. Buat script healthcheck.sh menggunakan template profesional dari bagian Best Practices.
    b. Script menampilkan: tanggal/waktu, hostname, uptime, penggunaan CPU, memori, dan disk untuk setiap filesystem yang terpasang.
    c. Jika penggunaan disk mana pun melebihi 80%, tampilkan peringatan.
    d. Simpan hasil ke healthcheck-YYYY-MM-DD.log dan tampilkan ke terminal sekaligus menggunakan tee.
    e. Opsi -t <persen> mengubah batas peringatan disk (default 80).
Konsep wajib: set -euo pipefail, trap, getopts, fungsi dengan local, for, if, dan tee
```
#!/bin/bash
# =================================================
# Script: healthcheck.sh
# Deskripsi: Pemeriksaan kondisi sistem sebelum maintenance
# Penggunaan: ./healthcheck.sh [-v] [-h] [-t <persen>]
# =================================================

SCRIPT_NAME=$(basename "$0")
VERBOSE=false
BATAS=80
LOGFILE="healthcheck-$(date +%Y-%m-%d).log"

usage() {
    echo "Penggunaan: $SCRIPT_NAME [-v] [-h] [-t <persen>]"
    echo "  -v  Verbose mode"
    echo "  -h  Tampilkan bantuan"
    echo "  -t  Batas peringatan disk (default: 80)"
    exit 0
}

log() {
    echo "[$SCRIPT_NAME] $*" | tee -a "$LOGFILE"
}

log_verbose() {
    if [ "$VERBOSE" = true ]; then
        log "$*"
    fi
}

error_exit() {
    echo "ERROR: $*" >&2
    exit 1
}

cleanup() {
    log_verbose "Cleanup selesai."
}

trap cleanup EXIT

cek_cpu() {
    local cpu
    cpu=$(awk '/^cpu / {idle=$5; total=0; for(i=2;i<=NF;i++) total+=$i; printf "%.1f", 100-(idle/total*100)}' /proc/stat)
    log "CPU Usage   : ${cpu}%"
}

cek_memori() {
    local total used persen
    total=$(free -m | awk '/Mem:/ {print $2}')
    used=$(free -m | awk '/Mem:/ {print $3}')
    persen=$(( used * 100 / total ))
    log "Memory      : ${used}MB / ${total}MB (${persen}%)"
}

cek_disk() {
    log "Disk Usage  :"
    while read -r persen mount; do
        if [ "$persen" -gt "$BATAS" ]; then
            log "  [PERINGATAN] $mount - ${persen}% (melebihi ${BATAS}%)"
        else
            log "  [OK] $mount - ${persen}%"
        fi
    done < <(df -h | tail -n +2 | awk '{gsub(/%/,"",$5); print $5, $6}')
}

while getopts "vht:" OPSI; do
    case $OPSI in
        v) VERBOSE=true ;;
        h) usage ;;
        t) BATAS="$OPTARG" ;;
        *) error_exit "Opsi tidak dikenal. Gunakan -h." ;;
    esac
done
shift $((OPTIND - 1))

log "Script dimulai"
log_verbose "Batas disk: ${BATAS}%"
log "======================================"
log " Tanggal  : $(date '+%Y-%m-%d %H:%M:%S')"
log " Hostname : $(hostname)"
log " Uptime   : $(uptime -p)"
log "======================================"
cek_cpu
cek_memori
cek_disk
log "======================================"
log "Selesai."
```
![steps](img/healthcheck.png "Contoh penggunaan")