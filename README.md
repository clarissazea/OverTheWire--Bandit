# Write Up 
# OverTheWire - Bandit

## Level 11 --> 12
- Username: bandit11
- Password (dari level sebelumnya): Diperoleh dari level 10 `(dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr)`.
- Tujuan: Mencari password untuk login ke user bandit12
- Lokasi file: ~/data.txt


### Langkah pengerjaan

Kata sandi untuk level berikutnya disimpan dalam file data.txt, di mana semua huruf kecil (az) dan huruf besar (AZ) telah diputar sebanyak 13 posisi.

File bernama data.txt dapat ditemukan di direktori home. Jika kita melihat isi file tersebut, kita akan menemukan serangkaian karakter/string kalimat acak. Pada petunjuk bandit disebutkan bahwa file data.txt berisi password untuk level berikutnya, tetapi terenkripsi menggunakan ROT13. ROT13 adalah bentuk sandi substitusi sederhana yang menggantikan setiap huruf dengan huruf ke-13 dalam alfabet (misalnya, A menjadi N, B menjadi O, dan seterusnya). Jadi, kita perlu mendekripsi isi file tersebut menggunakan metode ROT13.

1. Masuk ke server sebagai `bandit11`:
```bash
   ssh bandit11@bandit.labs.overthewire.org -p 2220
```
2. Lihat isi file data.txt
```bash
cat data.txt
```
3. Gunakan perintah `tr` untuk mendekripsi ROT13:
```bash
cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
```
4. Password untuk `bandit12`:
```bash
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

![image](https://github.com/user-attachments/assets/fe95e8bc-2305-4671-b74f-2bbab0051623)

## Level 12 --> 13
- Username: bandit12
- Password (dari level sebelumnya): Diperoleh dari level 11 `(7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4)`.
- Tujuan: Mencari password untuk login ke user bandit13
- Lokasi file: ~/data.txt


File data.txt berisi hexdump dari file asli yang telah dikompresi berulang kali. Tugas kita adalah memulihkan file asli tersebut, mengekstrak isinya, dan menemukan password untuk level selanjutnya. Disarankan untuk membuat direktori sementara di /tmp menggunakan mktemp -d, lalu bekerja di dalamnya.

1. Masuk sebagai bandit12
```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

2. Membuat folder dan menyalin `data.txt` ke dalamnya

Kita bisa menggunakan perintah `mktemp -d` untuk membuat folder dengan nama acak di /tmp, lalu masuk ke folder tersebut. Setelah itu, salin file data.txt dari home directory (~) ke folder tersebut menggunakan `cp ~ /data.txt .` Titik (.) berarti file disalin ke direktori saat ini. Terakhir, kita rename file tersebut menggunakan mv agar lebih sesuai untuk proses berikutnya.

![image](https://github.com/user-attachments/assets/8920ba5f-5141-402b-9b94-27fd59fea8f3)


3. Rename dan lihat file

Kita rename file tersebut menggunakan mv agar lebih sesuai untuk proses berikutnya. Melihat file tersebut, kita melihat format datanya. Seperti yang dinyatakan, ini adalah hexdump. Ini terlihat seperti ini:

![image](https://github.com/user-attachments/assets/555dfca0-1991-4b16-aec9-78eafb383fe5)

4. Mengembalikan file hasil hexdump

Kembali ke hexdump dan mendapatkan data yang sebenarnya (karena ingin beroperasi pada data yang sebenarnya.)

![image](https://github.com/user-attachments/assets/e0aabfa0-a632-4672-b7ea-5ecc1c02c68f)
```bash
- xxd: tool untuk membuat atau membalikkan hexdump.
- -r: reverse, artinya mengubah dari hexdump ke bentuk biner.
- hexdump_data: file input berisi data dalam format hexdump (hasil representasi heksadesimal dari file asli).
- compressed_data: nama file output hasil konversi ke bentuk biner (asli).
- Hasilnya adalah file compressed_data, yaitu file asli dalam bentuk biner (yang telah dikompresi berulang kali) dan siap untuk tahap dekompresi.
```
![image](https://github.com/user-attachments/assets/e4653ab0-6103-421e-9f89-26c3c7d5519a)

5. Proses dekompresi bertahap dari file biner yang sebelumnya dikonversi dari hexdump

- `mv compressed_data compressed_data.gz`: file `compressed_data` di-rename menjadi `compressed_data.gz` agar dikenali sebagai file gzip. Ini penting karena kita tidak tahu urutan kompresi yang dilakukan, jadi kita coba satu per satu. .gz adalah salah satu format umum kompresi.
- `gzip -d compressed_data.gz`: perintah untuk meng-unzip file `.gz`. File `compressed_data.gz` akan didekompres menjadi `compressed_data`.

![image](https://github.com/user-attachments/assets/cc0765a4-6647-4292-a367-a54500c34f6c)

6. Mengecek jenis kompresi file setelah proses dekompresi pertama.


`xxd compressed_data` : xxd: digunakan untuk menampilkan hexdump dari sebuah file (menampilkan isi file dalam format heksadesimal + ASCII).

Tujuannya adalah untuk mengidentifikasi format file, berdasarkan magic number (tanda khusus di awal file yang menunjukkan jenis file).

![image](https://github.com/user-attachments/assets/1437269b-e94e-4bf5-817e-ab57fb335a11)

7. Tahap dekompresi kedua, karena file yang sebelumnya hasil dekompresi `gzip` ternyata berformat `bzip2`, dan kini telah didekompresi menjadi file baru yang siap diperiksa jenis kompresinya selanjutnya.

- `mv compressed_data compressed_data.bz2` : Mengubah nama file compressed_data menjadi compressed_data.bz2. Penambahan ekstensi `.bz2` membantu agar sistem atau perintah seperti `bzip2` mengenali dan memproses file dengan benar.
- `bzip2 -d compressed_data.bz2` : bzip2 -d adalah perintah untuk mendekompresi file berekstensi .bz2 (mengembalikan ke bentuk aslinya).
- Setelah perintah ini dijalankan, file `compressed_data.bz2` akan hilang dan digantikan oleh file bernama `compressed_data` yang telah didekompresi.

![image](https://github.com/user-attachments/assets/f0ae242d-2a91-4696-9a21-5af324971b84)


8. Proses dekompresi ketiga, karena file hasil sebelumnya ternyata masih berupa gzip. Proses rename dan dekompresi diulang agar file bisa diakses lebih lanjut.
![image](https://github.com/user-attachments/assets/17dd5d6e-20bd-4455-b0bf-3bee278dffd1)

9. Menampilkan seluruh isi file `compressed_data`

`xxd compressed_data | head`: xxd mengubah file biner menjadi format heksadesimal dan karakter ASCII agar bisa dibaca. Head membatasi output hanya pada 10 baris pertama, sehingga hasil tidak tertampil isi file panjang.
![image](https://github.com/user-attachments/assets/b7f286a3-8106-4289-92b7-8ac6c30d109c)

10. Menangani file yang ternyata adalah arsip `.tar`. Dengan mengganti nama dan mengekstraknya, kita mendapatkan file baru `(data5.bin)` yang kemungkinan masih harus diproses lebih lanjut.

- `tar -xf compressed_data.tar`: Perintah ini digunakan untuk mengekstrak isi arsip tar.`-x`: extract, `-f`: menggunakan file `(compressed_data.tar)`
-  File `data5.bin` muncul, artinya proses ekstraksi berhasil.
-  `tar -xf data5.bin`: Perintah ini digunakan untuk mengekstrak file `data5.bin` yang ternyata merupakan arsip tar lainnya, meskipun tidak memiliki ekstensi `.tar`.

Dengan mengekstraknya, kita melanjutkan proses membuka lapisan-lapisan kompresi sampai akhirnya menemukan password level berikutnya.

![image](https://github.com/user-attachments/assets/a2345821-8e73-4fcf-9660-9558af50078d)

11. File `data6.bin` tampaknya telah dikompresi `bzip2` lagi.
![image](https://github.com/user-attachments/assets/90222ece-ce83-49e9-a272-caabb64d0c51)

12. `data6.bin.out` menunjukkan nama file lain `data8.bin` lagi. Jadi kita mengekstrak file ini.

![image](https://github.com/user-attachments/assets/95db42e6-ef24-4664-bc43-0074345a464f)

13. Proses dekompresi terakhir

Setelah melalui berbagai tahapan ekstraksi dan dekompresi (gzip, bzip2, tar, dsb.), akhirnya diperoleh file teks `data8` yang berisi password untuk login ke level bandit13.
```bash
- mv data8.bin data8.gz: Mengganti nama file ke ekstensi `.gz` agar formatnya dikenali dengan lebih jelas sebagai gzip.
- gzip -d data8.gz: Menghapus kompresi gzip dari file tersebut. Setelah didekompresi, file baru bernama `data8` akan muncul.
- cat data8: Isi file data8 ternyata adalah password
```

![image](https://github.com/user-attachments/assets/55fb1246-f404-4b7c-b5be-62883940e43e)

14. Password untuk `bandit13`:
```bash
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

## Level 13 --> 14
- Username: bandit13
- Password (dari level sebelumnya): Diperoleh dari level 12 (`FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`)
- Tujuan: Mencari sshkey untuk login ke user `bandit14`
- Lokasi file: sshkey.private (bukan ~/data.txt, tidak ada file tersebut di level ini)


Kata sandi untuk level ini disimpan di `/etc/bandit_pass/bandit14` dan hanya dapat dibaca oleh pengguna bandit14. Untuk level ini, kita tidak mendapatkan kata sandi berikutnya, tetapi mendapatkan kunci SSH pribadi (sshkey) yang dapat digunakan untuk masuk ke level berikutnya. 

1. Login ke level 13
```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

2. Lihat isi directory
```bash
sshkey.private
```
File ini adalah kunci privat SSH untuk mengakses user bandit14.

3. Gunakan kunci privat untuk login ke user bandit14 melalui localhost:
```bash
ssh -i sshkey.private bandit14@localhost -p 2220
```
4. Setelah berhasil login sebagai bandit14, tampilkan isi file password:
```bash
cat /etc/bandit_pass/bandit14
```

5. Password untuk `bandit14`:
```bash
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

![image](https://github.com/user-attachments/assets/60ec7971-f76f-4b2f-91b3-ee95cd378e52)
![image](https://github.com/user-attachments/assets/1ae5eb2d-e508-4ad6-b1b8-4956960d299d)


## Level 14 --> 15
- Username: bandit14
- Password (dari level sebelumnya): Diperoleh dari level 13 (`MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`)
- Tujuan: Mendapatkan password untuk login ke user bandit15 dengan mengirim password level sekarang ke port tertentu
- Lokasi file: Tidak ada file khusus, menggunakan koneksi ke port 30000 di localhost

1. Login ke level 14
```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

2. Gunakan netcat (nc) untuk mengirim password level 14 ke port 30000 di localhost:
```bash
nc localhost 30000
```

3. Masukkan password level 14
```bash
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```
5. Password untuk `bandit15`:
```bash
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

![image](https://github.com/user-attachments/assets/a20a1a89-b0f3-40c0-b1c1-72a8db595834)




## Level 15 --> 16
- Username: bandit15
- Password (dari level sebelumnya): Diperoleh dari level 14 (`8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`)
- Tujuan: Mendapatkan password untuk login ke user bandit16 dengan mengirim password level sekarang ke port 30001 di localhost menggunakan enkripsi SSL
- Lokasi file: Tidak ada file khusus, menggunakan koneksi SSL ke port 30001

1. Login ke level 15
```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

2. Membuat koneksi SSL ke server lokal di port 30001
```bash
openssl s_client -ign_eof -connect localhost:30001
```
Perintah openssl s_client digunakan untuk membuat koneksi SSL (secure socket layer) ke alamat dan port tertentu, dalam hal ini ke localhost:30001.

- `-connect localhost:30001`: Menghubungkan ke localhost pada port 30001
- `-ign_eof`: Mengabaikan EOF (End of File) agar koneksi tetap terbuka meskipun input sudah selesai, mencegah error “HEARTBEATING” atau “Read R BLOCK” yang muncul saat komunikasi SSL.

3. Masukkan password level 15
```bash
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

3. Password untuk `bandit16`:
```bash
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```
![image](https://github.com/user-attachments/assets/a14a59ec-5e24-47b2-8bae-9b87f7c7b07c)

![image](https://github.com/user-attachments/assets/fc99f597-b604-483b-a73c-0632be0b6aa5)


