# Menginstall-Apache-Server-pada-Ubuntu-20.4-Proxmox-
Ini akan menjelaskan langkah-langkah untuk mengisntall Apache Server 20.4 Proxmox

Teori Dasar
==
Apache adalah salah satu jenis web server yang dapat dijalankan di berbagai sistem operasi, seperti Microsoft Windows, Linux, Unix, Novell Netware serta platform lainnya yang digunakan untuk melayani dan melakukan pengaturan fasilitas web menggunakan sebuah protokol yang dikenal dengan HTTP (Hypertext Transfer Protocol). Nama Apache sendiri dipilih sebagai penghormatan terhadap suku Indian Apache yang menggunakan keterampilan dan strategis yang luar biasa dalam peperangan.

Apache memiliki fungsi yang sama dengan fungsi web server pada umumnya, yaitu memperoleh berkas yang berisi permintaan (request) client melalui web browser, kemudian Apache akan memproses data tersebut dengan menghasilkan keluaran (output) yang diinginkan oleh client. Output didapat berdasarkan data yang tersimpan dalam database website tersebut.

Menginstall Apache
--

Sebelum menginstall Apache pastikan melakukan update package terlebih dahulu:
```
sudo apt update
```
lalu install apache2 package nya:
```
sudo apt install apache2
```
Setelah melakukan command di atas, apache2 akan otomatis terinstall di Ubuntu 20.04.

Menyesuaikan Firewall
--
Sebelum menguji Apache, Anda perlu memodifikasi pengaturan firewall untuk mengizinkan akses dari luar ke porta web asali. Dengan asumsi bahwa Anda telah mengikuti instruksi di prasyarat, Anda seharusnya memiliki firewall UFW yang terkonfigurasi untuk membatasi akses ke server Anda.

Selama instalasi, Apache mendaftarkan dirinya dengan UFW untuk menyediakan beberapa profil aplikasi yang dapat digunakan untuk mengaktifkan atau menonaktifkan akses ke Apache melalui firewall.

Buat daftar profil aplikasi ufw dengan mengetik:
```
sudo ufw app list
```
output:
```
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```
Seperti yang ditunjukkan oleh keluaran, ada tiga profil yang tersedia untuk Apache:

- Apache: Profil ini hanya membuka porta 80 (lalu lintas web normal dan tidak terenkripsi)
- Apache Full: Profil ini membuka baik porta 80 (lalu lintas web normal dan tidak terenkripsi) serta porta 443 (lalu lintas terenkripsi TLS/SSL)
- Apache Secure: Profile ini hanya membuka porta 443 (lalu lintas terenkripsi TLS/SSL)

Selanjutnya disarankan untuk mengaktifkan profil yang paling ketat yang masih akan mengizinkan lalu lintas yang telah konfigurasikan. Karena kita belum mengonfigurasi SSL untuk server dalam panduan ini, lalu hanya perlu mengizinkan lalu lintas pada porta 80:
```
sudo ufw allow 'Apache'
```
Anda dapat memverifikasi perubahan dengan mengetik:
```
sudo ufw status
```
Keluaran akan memberi daftar lalu lintas HTTP yang diizinkan:
```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Apache                     ALLOW       Anywhere                
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Apache (v6)                ALLOW       Anywhere (v6)
```

Memeriksa Server Web
--
Setelah sudah terpasang apache2, web server seharusnya sudah aktif dan berjalan. Periksa dengan system init systemd untuk memastikan layanan berjalan  dengan command di bawah ini:
```
sudo systemctl status apache2
```
output:

![ageng - Proxmox Virtual Environment - Google Chrome 11_22_2022 1_27_25 PM (2)](https://user-images.githubusercontent.com/90621908/203899383-bea36704-4bd6-4eca-b844-6dcabe6029ea.png)

Selanjutnya, Anda dapat mengakses halaman URL arahan Apache default, untuk mengonfirmasikan bahwa software berjalan dengan benar melalui alamat IP Address Anda.

Jika Anda tidak mengetahui alamat IP Server, anda dapat melakukan dengan beberapa command di bawah ini:
```
hostname -I
```
atau
```
curl -4 icanhazip.com
```
output:

![QEMU (vm100) - noVNC - Google Chrome 11_22_2022 1_38_50 PM (3)](https://user-images.githubusercontent.com/90621908/203899795-567d79e5-c8eb-4876-a084-65021819c46e.png)

Jika Anda memiliki IP Server, anda bisa membuka alamat IP Address melalui tab browser:
```
http://your_server_ip
```
Selanjutnya, Anda dapat melihat halaman web default Ubuntu 20.04 Apache:

![Cara Menginstal Server Web Apache pada Ubuntu 20 04 _ DigitalOcean - Google Chrome 11_22_2022 1_39_18 PM](https://user-images.githubusercontent.com/90621908/203899974-a04beac9-1b2e-4ce5-86ce-15a9780c2e60.png)

Mengelola Proses Apache
--
Untuk menghentikan server web:
```
sudo systemctl stop apache2
```
Untuk memulai server web saat berhenti:
```
sudo systemctl start apache2
```
Untuk menghentikan lalu memulai layanan lagi:
```
sudo systemctl restart apache2
```
Jika Anda hanya membuat perubahan konfigurasi, Apache seringkali dapat memuat ulang tanpa memutus koneksi. Untuk melakukan ini, gunakan perintah ini:
```
sudo systemctl reload apache2
```
Secara asali, Apache dikonfigurasikan untuk memulai secara otomatis saat server melakukan boot. Jika ini bukan apa yang Anda inginkan, Anda dapat menonaktifkan perilaku ini dengan mengetik:
```
sudo systemctl disable apache2
```
Untuk mengaktifkan kembali layanan agar memulai saat boot:
```
sudo systemctl enable apache2
```

Menyiapkan Hos Virtual
--
Disini kita akan mencoba mendeit tampilan dari webnya:

Masuk ke direktori:
```
cd var/www/html
```
masuk ke super user, dengan mengetik:
```
sudo su
```
buat direktori baru:
```
mkdir cloud
```
example:

![QEMU (vm100) - noVNC - Google Chrome 11_23_2022 9_52_51 AM (3)](https://user-images.githubusercontent.com/90621908/203901868-0c012787-a431-4cb3-a3b6-5bf0898693f3.png)

lalu masuk ke direktori cloud:
```
cd cloud
```
selanjutnya masuk ke website yang sudah dibuat dengan menambahkan ujungnya dengan /nama_direktori seperti berikut ini:
```
http://192.168.110.139/cloud
```

![Index of _cloud - Google Chrome 11_23_2022 9_51_46 AM](https://user-images.githubusercontent.com/90621908/203902672-9b8ce259-4926-4c77-a54f-c445d4642ed8.png)

selanjutnya bikin file HTML:
```
nano index.html
```
lalu kita bikin code html:
```
<html>
    <head>
        <title>Belajar Membuat Web Server</title>
    </head>
    <body>
        <h1>Saya Cinta Cloud</h1>
    </body>
</html>
```
sudah itu simpan dengan CTRL+X ketik Y dan ENTER

Dan kita refresh halaman web sebelumnya
![192 168 110 139_cloud_ - Google Chrome 11_23_2022 9_57_12 AM](https://user-images.githubusercontent.com/90621908/203903240-59edd06c-135d-464c-9c33-30ad409473bd.png)
