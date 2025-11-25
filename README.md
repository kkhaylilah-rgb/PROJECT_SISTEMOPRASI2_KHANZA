# PROJECT_SISTEMOPRASI2_KHANZA

# SISTEM BACKUP OTOMATIS
# Perintah nano
memasukkan nano agar muncul taskbar nano
[Deskripsi gambar] ( https://drive.google.com/file/d/1FIzF-f9pBQKdVc8OjE8YuwSDoB-Qmu7a/view?usp=drivesdk )

~~~~
cd
nano
~~~~
penjelasan :
cd berpindah atau keluar dri direktori
nano = membuka taskbar nano untuk membuat script

# Menentukan folder asal dan folder tujuan untuk backup
kita sudah mulai membuat script dengan menentukan folder asal dan tujuan
[Deskripsi gambar] : ( https://drive.google.com/file/d/1fLrkxuQ4NBaMMgx0szIQm69JMhRwU926/view?usp=drivesdk )

~~~~
#!/bin/bash

SOURCE_DIR="/home/khanzak/project_manajemen_file"
BACKUP_DIR=" /home/khanzak/bacup 1

~~~~

penjelasan :
#!/bin/bash : menandakan bahwa script di jalankan menggunakan bash
SOURCE_DIR : Folder sumber yang mau dibackup
BACKUP_DIR : Folder tempat menyimpan backup

# Membuat folder backup, timestamp dan nama file yang di hasilkan
folder backup akan di buat jika belum ada, menggunakan timestamp untuk nama file dan membuat nama file yang akan di hasilkan
[Deskripsi gambar] ( https://drive.google.com/file/d/1ORAIRdtbk6MFwMj7hnVVuUE4Q-VbDYP1/view?usp=drivesdk )

~~~~
mkdir -p "$BACKUP_DIR"
TIMESTAMP=$(date +'%Y%m%d_%H%M%S') BACKUP_FILE="backup_${TIMESTAMP}.tar.gz"
~~~~

penjelasan : 

mkdir → membuat directory (folder)

-p → memastikan tidak error jika folder sudah ada

"$BACKUP_DIR" → memakai nilai variabel BACKUP_DIR, yaitu /home/khanzak/backup1

timestamp = penanda waktu lengkap untuk nama file, supaya nama file backup unik dan berdasarkan waktu pembuatan.

backup_ → awalan nama file

${TIMESTAMP} → nilai timestamp tadi

.tar.gz → format file hasil kompresi

# Membuat file log dan mencari file sesuai kriteria
file log berguna untuk mencatat aktivitas backup, mencari file sesuai kriteria dan di modifikasi 7 hri terakhir
[Deskripsi gambar] : ( https://drive.google.com/file/d/1X7591KVXzP6_DyezcahkwnRSRRTN5rhp/view?usp=drivesdk )

~~~~
LOG_FILE="$BACKUP_DIR/backup.log"
FILES_TO_BACKUP=$(find "$SOURCE_DIR" -type f \( -name "*.pdf" -o -name "*.txt" \) -mtime -7)
~~~~
penjelasan : 
LOG_FILE : membuat folder untuk mencatat aktivitas backup

find "$SOURCE_DIR" : Mencari file di dalam folder sumber
-type f : Hanya mencari file, bukan folder.
-name "*.pdf" → cari file PDF
-name "*.txt" → cari file teks
-o → OR → artinya salah satu
-mtime -7 : file yang dimodifikasi dalam 7 hari terakhir.

# Mengecek file dan mengkompresinya
Mengecek apakah ada file yang layak di-backup. Jika tidak ada tulis log + beri pesan error + hentikan script. Jika ada kompres file-file tersebut ke file .tar.gz
[Deskripsi gambar] : ( https://drive.google.com/file/d/1NVgL2NRLlWBe-pzimZtxEJd2a69S5Owv/view?usp=drivesdk )

~~~~
if [ -z "$FILES_TO_BACKUP" ]; then
    echo "$TIMESTAMP - Tidak ditemukan file yang memenuhi kriteria backup." >> "$LOG_FILE"
    echo "Backup gagal: tidak ada file yang sesuai kriteria."
    exit 1
fi
 tar -czf "$BACKUP_DIR/$BACKUP_FILE" $FILES_TO_BACKUP
~~~~
penjelasan : 
if [ -z "$FILES_TO_BACKUP" ]; then : Bagian ini mengecek apakah variabel FILES_TO_BACKUP kosong.

echo "$TIMESTAMP - Tidak ditemukan file yang memenuhi kriteria backup." >> "$LOG_FILE" : Baris ini mencatat ke log file bahwa tidak ada file yang memenuhi kriteria.

echo "Backup gagal: tidak ada file yang sesuai kriteria." : Memberi pesan ke terminal bahwa backup gagal.

exit 1 : Keluar dari script langsung dan memberi status 1 (gagal/error).

fi : Menutup blok if.

tar -czf "$BACKUP_DIR/$BACKUP_FILE" $FILES_TO_BACKUP : Jika tadi FILES_TO_BACKUP tidak kosong, maka perintah ini membuat file backup terkompresi

# notifikasi backup

Bagian ini tugasnya mengecek apakah proses kompresi backup sukses atau gagal.

 Jika berhasil → catat ke log + tampilkan pesan sukses
 Jika gagal → catat ke log + tampilkan pesan gagal + hentikan script
 
[Deskripsi gambar] : ( https://drive.google.com/file/d/18Xs5evJ09A_FLKkFrwFh3P9wjkZVB1ZX/view?usp=drivesdk )

~~~~
if [ $? -eq 0 ]; then
    echo "$TIMESTAMP - Backup berhasil: $BACKUP_FILE dibuat." >> "$LOG_FILE"
    echo "Backup berhasil: $BACKUP_FILE"
else
    echo "$TIMESTAMP - Backup gagal." >> "$LOG_FILE"
    echo "Backup gagal."
    exit 1
fi
~~~~
penjelasan : 
$? : digunakan untuk mengecek exit status dari perintah sebelumnya.

echo "$TIMESTAMP - Backup berhasil: $BACKUP_FILE dibuat." >> "$LOG_FILE" → Menulis catatan keberhasilan ke log file.

echo "Backup berhasil: $BACKUP_FILE" → Menampilkan pesan sukses ke terminal untuk user.

else : Masuk ke blok ELSE jika perintah tar gagal.

echo "$TIMESTAMP - Backup gagal." >> "$LOG_FILE" → Mencatat ke dalam log bahwa proses backup gagal.

echo "Backup gagal." → Menampilkan pesan gagal ke terminal.

exit 1 → Menghentikan script dan memberi status error (1).

fi : Menutup blok if.

# Memberi izin akses dan menjalankan
setelah script selesai, berikan izin akses dan menjalankan nya
[Deskripsi gambar] : ( https://drive.google.com/file/d/1ZLeKVd6-RaLWKckx4e7GDNOB9d0bgEgD/view?usp=drivesdk )

~~~~
chmod +× backup.sh
./backup.sh
~~~~
penjelasan :
chmod +x : memberi izin akses
./namafile : menjalankannya

# Mengecek apakah file backup tersimpan
[Deskripsi gambar] : ( https://drive.google.com/file/d/1TwZV85KV9BBeKQTHlkAlHf01NqOj47YI/view?usp=drivesdk )

~~~~
ls
cd backup1
ls
~~~~
penjelasan :
ls : menampilkan
cd namafile : masuk ke direktori atau folder
