# Write Up OverTheWire--Bandit

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

Kita bisa menggunakan perintah mktemp -d untuk membuat folder dengan nama acak di /tmp, lalu masuk ke folder tersebut. Setelah itu, salin file data.txt dari home directory (~) ke folder tersebut menggunakan `cp ~ /data.txt .` Titik (.) berarti file disalin ke direktori saat ini. Terakhir, kita rename file tersebut menggunakan mv agar lebih sesuai untuk proses berikutnya.

![image](https://github.com/user-attachments/assets/8920ba5f-5141-402b-9b94-27fd59fea8f3)

3. Rename dan lihat file

Kita rename file tersebut menggunakan mv agar lebih sesuai untuk proses berikutnya. Melihat file tersebut, kita melihat format datanya. Seperti yang dinyatakan, ini adalah hexdump. Ini terlihat seperti ini:
![image](https://github.com/user-attachments/assets/555dfca0-1991-4b16-aec9-78eafb383fe5)

4. Mengembalikan file hasil hexdump
   
Namun, kami ingin beroperasi pada data yang sebenarnya. Oleh karena itu, kami kembali ke hexdump dan mendapatkan data yang sebenarnya.
![image](https://github.com/user-attachments/assets/e0aabfa0-a632-4672-b7ea-5ecc1c02c68f)
- xxd: tool untuk membuat atau membalikkan hexdump.
- -r: reverse, artinya mengubah dari hexdump ke bentuk biner.
- hexdump_data: file input berisi data dalam format hexdump (hasil representasi heksadesimal dari file asli).
- compressed_data: nama file output hasil konversi ke bentuk biner (asli).
- Hasilnya adalah file compressed_data, yaitu file asli dalam bentuk biner (yang telah dikompresi berulang kali) dan siap untuk tahap dekompresi.

![image](https://github.com/user-attachments/assets/e4653ab0-6103-421e-9f89-26c3c7d5519a)

5. Ketika dilihat file data aslinya

![image](https://github.com/user-attachments/assets/cc0765a4-6647-4292-a367-a54500c34f6c)

6. 
![image](https://github.com/user-attachments/assets/1437269b-e94e-4bf5-817e-ab57fb335a11)

![image](https://github.com/user-attachments/assets/f0ae242d-2a91-4696-9a21-5af324971b84)


![image](https://github.com/user-attachments/assets/17dd5d6e-20bd-4455-b0bf-3bee278dffd1)
![image](https://github.com/user-attachments/assets/b7f286a3-8106-4289-92b7-8ac6c30d109c)

![image](https://github.com/user-attachments/assets/a2345821-8e73-4fcf-9660-9558af50078d)
![image](https://github.com/user-attachments/assets/90222ece-ce83-49e9-a272-caabb64d0c51)


![image](https://github.com/user-attachments/assets/95db42e6-ef24-4664-bc43-0074345a464f)
![image](https://github.com/user-attachments/assets/55fb1246-f404-4b7c-b5be-62883940e43e)


password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
## Level 13 --> 14
password: sshkey

![image](https://github.com/user-attachments/assets/60ec7971-f76f-4b2f-91b3-ee95cd378e52)
![image](https://github.com/user-attachments/assets/1ae5eb2d-e508-4ad6-b1b8-4956960d299d)


## Level 14 --> 15

password: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

![image](https://github.com/user-attachments/assets/a20a1a89-b0f3-40c0-b1c1-72a8db595834)




## Level 15 --> 16
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
![image](https://github.com/user-attachments/assets/86e5c02e-3292-4be3-8822-efd2ac253c6c)
![image](https://github.com/user-attachments/assets/e0ae7633-1546-4632-9d73-197b77c54b71)


