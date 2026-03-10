# Laporan Praktikum Sistem Operasi Jobsheet 4

<h4>Nama : Surya Sadikin Firdaus<h4>
<h4>NIM : 254107020105<h4>
<h4>Kelas : TI-1H<h4>

## Percobaan 1: Direktory
Langkah-langkah:
1. Melihat direktori HOME
```
pwd
echo $HOME
```
![langkah1](img/prak1lang1.png "Langkah No. 1")

2. Melihat direktori aktual dan parent
```
pwd
cd .
pwd
cd ..
pwd
cd
```
![langkah2](img/prak1lang2.png "Langkah No. 2")

3. Membuat satu direktori, lebih dari satu direktori atau sub direktori
```
pwd
mkdir A B C A/D A/E B/F A/D/A
ls -l
ls -l A
ls -l A/D
```
![langkah3](img/prak1lang3.png "Langkah No. 3")

4. Menghapus satu atau lebih direktori hanya dapat dilakukan pada direktori kosong dan hanya dapat dihapus oleh pemiliknya kecuali bila diberikan ijin aksesnya
```
rmdir B
ls -l B
rmdir B/F B
ls -l B
```
![langkah4](img/prak1lang4.png "Langkah No. 4")
* Terjadi error saat rmdir B karena direktori B tidak kosong sehingga tidak bisa dihapus
* Terjadi error saat ls -l B karena direktori B sudah dihapus sehingga tidak bisa di-list

5. Navigasi direktori dengan instruksi cd untuk pindah dari satu direktori ke direktori lai
```
pwd
ls -l
cd A
pwd
cd ..
pwd
cd /home/laut/C
pwd
cd /laut/C
pwd
```
![langkah5](img/prak1lang5.png "Langkah No. 5")
* Terjadi error karena alamat yang ditulis tidak lengkap, jika ingin menulis alamat tanpa harus menulis /home/username/ maka kita bisa menggunakan ~ sebagai pengganti dan bukannya langsung menulis /username/


## Percobaan 2: Manipulasi File
1. Perintah cp untuk mengkopi file atau seluruh direktori
```
cat > contoh
Membuat sebuah file
[Cntrl-d]
ls -l
cp contoh A
ls -l A
cp contoh contoh1 A/D
ls -l A/D
```
![langkah1](img/prak2lang1.png "Langkah No. 1")

2. Perintah mv untuk memindah file
```
mv contoh contoh2
ls -l
mv contoh1 contoh2 A/D
ls -l A/D
mv contoh contoh1 C
ls -l C
```
![langkah2](img/prak2lang2.png "Langkah No. 2")

3. Perintah rm untuk menghapus file
```
rm contoh2
ls -l
rm -i contoh
rm -rf A C
ls -l
```
![langkah3](img/prak2lang3.png "Langkah No. 3")


## Percobaan 3: Symbolic Link
```
echo "Hello apa khabar" > halo.txt
ls -l
ln halo.txt z
ls -l
cat z
mkdir mydir
ln z mydir/halo.juga
cat mydir/halo.juga
ln -s z bye.txt
ls -l bye.txt
cat bye.txt
```
![langkah1](img/prak3lang1.png "Langkah No. 1")


## Percobaan 4: Melihat Isi File
```
ls -l
file halo.txt
file bye.txt
```
![langkah1](img/prak4lang1.png "Langkah No. 1")


## Percobaan 5: Mencari File
1. Perintah find
```
find /home -name "*.txt" -print > myerror.txt
cat myerror.txt
find . -name "*.txt" -exec wc -l '{}' ';'
```
![langkah1](img/prak5lang1.png "Langkah No. ")

2. Perintah which
```
which ls
```
![langkah2](img/prak5lang2.png "Langkah No. 2")

3. Perintah locate
```
locate "*.txt"
```
![langkah3](img/prak5lang3.png "Langkah No. 3")


## Percobaan 6: Mencari text pada file
```
grep Hallo *.txt
```
![langkah1](img/prak6lang1.png "Langkah No. 1")


## Latihan
1. Cobalah urutan perintah berikut:
```
cd
pwd
ls -al
cd .
pwd
cd ..
pwd
ls -al
cd ..
pwd
ls -al
cd /etc
ls -al | more
cat passwd
cd -
pwd
```
![langkah1](img/lat1lang1a.png "Langkah No. 1")
![langkah1](img/lat1lang1b.png "Langkah No. 1")
![langkah1](img/lat1lang1c.png "Langkah No. 1")
![langkah1](img/lat1lang1d.png "Langkah No. 1")

2. Lanjutkan penelusuran pohon pada sistem file menggunakan cd, ls, owd, dan cat. Telusuri direktory /bin, /usr/bin, /sbin, /tmp, dan /boot
* /bin
![langkah2](img/lat1lang2bin.png "Langkah No. 2")
* /usr/bin
![langkah2](img/lat1lang2usrbin.png "Langkah No. 2")
* /sbin
![langkah2](img/lat1lang2sbin.png "Langkah No. 2")
* /tmp
![langkah2](img/lat1lang2tmp.png "Langkah No. 2")
* /boot
![langkah2](img/lat1lang2boot.png "Langkah No. 2")

3. Telusuri direkoty /dev. Identifikasi perangkat yang tersedia. Identifikasi tty (terminal) Anda (ketik who am i); siapa pemilik tty Anda (gunakan ls -l)
![langkah3](img/lat1lang3.png "Langkah No. 3")

4. Telusuri directory /proc. Tampilkan isi file interrupts, devices, cpuinfo, meminfo dan uptime menggunakan perintah cat. Dapatkah anda melihat mengapa directory /proc disebut pseudo-filesystem yang memungkinkan akses ke struktu data kernel?
* interrupts
![langkah4](img/lat1lang4interrupts.png "Langkah No. 4")
* devices
![langkah4](img/lat1lang4devices.png "Langkah No. 4")
* cpuinfo
![langkah4](img/lat1lang4cpuinfo.png "Langkah No. 4")
* meminfo
![langkah4](img/lat1lang4meminfo.png "Langkah No. 4")
* uptime
![langkah4](img/lat1lang4uptime.png "Langkah No. 4")
* Kenapa /proc disebut sebagai pseudo-filesystem, hal ini dikarenakan direktori /proc tidak memiliki file nyata, melainkan /proc memiliki file yang memuat informasi mengenai segala sesuatu yang sedang berjalan di mesin saat ini. Hal ini relevan dengan penjelasan mengenai pseudo-filesystem yang memiliki arti sebaga filesystem yang memuat informasi mengenai system yang berjalan saat ini

5. Ubahlah direktory home ke user lain secara langsung menggunakan cd ~username
![langkah5](img/lat1lang5.png "Langkah No. 5")

6. Ubah kembali ke directory home anda
![langkah6](img/lat1lang6.png "Langkah No. 6")

7. Buat subdirectory work dan play
![langkah7](img/lat1lang7.png "Langkah No. 7")

8. Hapus directory work
![langkah8](img/lat1lang8.png "Langkah No. 8")

9. copy file /etc/passwd ke directory home anda
![langkah9](img/lat1lang9.png "Langkah No. 9")

10. Pindahkan ke subdirectory play
![langkah10](img/lat1lang10.png "Langkah No. 10")

11. Ubahlah ke direktory play dan buat symbolic link dengan nama terminal yang menunjuk ke perangkat tty. Apa yang terjadi jika melakukan hard link ke perangkat tty?
![langkah11](img/lat1lang11.png "Langkah No. 11")
* Akan terjadi error "Invalid cross-device link" jika melakukan hard link ke perangkat tty

12. Buatlah file bernama hello.txt yang berisi kata "hello world". Dapatkah anda gunakan "cp" menggunakan "terminal" sebagai file asal untuk menghasilkan efek yang sama
![langkah12](img/lat1lang12.png "Langkah No. 12")

13. Copy hello.txt e terminal. Apa yang terjadi?
![langkah13](img/lat1lang13.png "Langkah No. 13")

14. Masih direktory home, copy keseluruhan direktory ke direktory bernama menggunakan symbolic link.
![langkah14](img/lat1lang14.png "Langkah No. 14")

15. Hapus direktory work dan isinya dengan satu perintah
![langkah15](img/lat1lang15.png "Langkah No. 15")

