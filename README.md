<h1> Jarkom-Modul-2-E13-2023 </h1>

| No | Nama | NRP |
|----------|----------|----------|
| 1 | Nadya Permata Sari | 5025201015 |
| 2 | Najma Ulya Agustina | 5025211239 |

<h2>Daftar Isi</h2>

| Soal | Solusi | Testing |
|----------|----------|----------|
| [Soal 1](#soal-1) | [Solusi](#solusi1) | [Testing](#testing1) |
| [Soal 2](#soal-2) | [Solusi](#solusi2) | [Testing](#testing2) |
| [Soal 3](#soal-3) | [Solusi](#solusi3) | [Testing](#testing3) |
| [Soal 4](#soal-4) | [Solusi](#solusi4) | [Testing](#testing4) |
| [Soal 5](#soal-5) | [Solusi](#solusi5) | [Testing](#testing5) |
| [Soal 6](#soal-6) | [Solusi](#solusi6) | [Testing](#testing6) |
| [Soal 7](#soal-7) | [Solusi](#solusi7) | [Testing](#testing7) |
| [Soal 8](#soal-8) | [Solusi](#solusi8) | [Testing](#testing8) |
| [Soal 9](#soal-9) | [Solusi](#solusi9) | [Testing](#testing9) |
| [Soal 10](#soal-10) | [Solusi](#solusi10) | [Testing](#testing10) |
| [Soal 11](#soal-11) | [Solusi](#solusi11) | [Testing](#testing11) |
| [Soal 12](#soal-12) | [Solusi](#solusi12) | [Testing](#testing12) |
| [Soal 13](#soal-13) | [Solusi](#solusi13) | [Testing](#testing13) |
| [Soal 14](#soal-14) | [Solusi](#solusi14) | [Testing](#testing14) |
| [Soal 15](#soal-15) | [Solusi](#solusi15) | [Testing](#testing15) |
| [Soal 16](#soal-16) | [Solusi](#solusi16) | [Testing](#testing16) |
| [Soal 17](#soal-17) | [Solusi](#solusi17) | [Testing](#testing17) |
| [Soal 18](#soal-18) | [Solusi](#solusi18) | [Testing](#testing18) |
| [Soal 19](#soal-19) | [Solusi](#solusi19) | [Testing](#testing19) |
| [Soal 20](#soal-20) | [Solusi](#solusi20) | [Testing](#testing20) |


Prefix IP Kelompok E13: 10.43

# <h3>Soal 1</h3>
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut.

## <h4>Solusi</h4> <a name="solusi1"></a>
    Hasil topologi yang telah dibuat adalah:
    <img width="470" alt="soal1" src="img/1.png">

## - Konfigurasi network:

**Router**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.43.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.43.2.1
	netmask 255.255.255.0
```
**NakulaClient**
```
auto eth0
iface eth0 inet static
	address 10.43.1.2
	netmask 255.255.255.0
	gateway 10.43.1.1
```
**SadewaClient**
```
auto eth0
iface eth0 inet static
	address 10.43.1.3
	netmask 255.255.255.0
	gateway 10.43.1.1
```
**AbimanyuWebServer**
```auto eth0
iface eth0 inet static
	address 10.43.1.4
	netmask 255.255.255.0
	gateway 10.43.1.1
```
**PrabukusumaWebServer**
```
auto eth0
iface eth0 inet static
	address 10.43.1.5
	netmask 255.255.255.0
	gateway 10.43.1.1
```
**WisanggeniWebServer**
```
auto eth0
iface eth0 inet static
	address 10.43.1.6
	netmask 255.255.255.0
	gateway 10.43.1.1
```
**YudhistiraDNSMaster**
```
auto eth0
iface eth0 inet static
	address 10.43.2.2
	netmask 255.255.255.0
	gateway 10.43.2.1
```
**WerkudaraDNSSlave**
```
auto eth0
iface eth0 inet static
	address 10.43.2.3
	netmask 255.255.255.0
	gateway 10.43.2.1
```
**ArjunaLoadBalancer**
```
auto eth0
iface eth0 inet static
	address 10.43.2.4
	netmask 255.255.255.0
	gateway 10.43.2.1
```

- Melakukan pengeditan pada /root/.bashrc
  
**Router**
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.43.0.0/16
```
**YudhistiraDNSMaster**
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
service bind9 restart  
mkdir /etc/bind/arjuna    
mkdir /etc/bind/abimanyu
```
**WerkudaraDNSSlave**
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
service bind9 restart 
```
**NakulaClient & SadewaClient**
```
echo -e '
nameserver 10.43.2.2 # IP Yudhistira
nameserver 10.43.2.3 # IP Werkudara
nameserver 192.168.122.1
' > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
apt-get install lynx -y
```
**ArjunaLoadBalancer**
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
apt-get install lynx -y
apt-get install bind9 nginx -y
service nginx start
```
**AbimanyuWebServer, PrabukusumaWebserver, WisanggeniWebServer**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt install nginx php php-fpm -y
php -v
```

<h4>Testing</h4> <a name="testing1"></a>
Pada Node lakukan:

```
ping google.com -c 5
```

<h3>Soal 2</h3>
Buatlah website utama dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.
<h4>Solusi</h4> <a name="solusi2"></a>
Pada `YudhistiraDNSMaster`, lakukan pengeditan `nano /etc/bind/named.conf.local`

```
zone "arjuna.e13.com" {
	type master;
	file "/etc/bind/jarkom/arjuna.e13.com";
};
```

<h4>Testing</h4>  <a name="testing2"></a>

<h3>Soal 3</h3>
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.
<h4>Solusi</h4> <a name="solusi3"></a>
<h4>Testing</h4>  <a name="testing3"></a>

<h3>Soal 4</h3>
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
<h4>Solusi</h4> <a name="solusi4"></a>
<h4>Testing</h4>  <a name="testing4"></a>

<h3>Soal 5</h3>
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)
<h4>Solusi</h4> <a name="solusi5"></a>
<h4>Testing</h4>  <a name="testing5"></a>

<h3>Soal 6</h3>
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.
<h4>Solusi</h4> <a name="solusi6"></a>
<h4>Testing</h4>  <a name="testing6"></a>

<h3>Soal 7</h3>
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
<h4>Solusi</h4> <a name="solusi7"></a>
<h4>Testing</h4>  <a name="testing7"></a>

<h3>Soal 8</h3>
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.
<h4>Solusi</h4> <a name="solusi8"></a>
<h4>Testing</h4>  <a name="testing8"></a>

<h3>Soal 9</h3>
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.
<h4>Solusi</h4> <a name="solusi9"></a>
<h4>Testing</h4>  <a name="testing9"></a>

<h3>Soal 10</h3>
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
<h4>Solusi</h4> <a name="solusi10"></a>
<h4>Testing</h4>  <a name="testing10"></a>

<h3>Soal 11</h3>
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy
<h4>Solusi</h4> <a name="solusi11"></a>
<h4>Testing</h4>  <a name="testing11"></a>

<h3>Soal 12</h3>
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.
<h4>Solusi</h4> <a name="solusi12"></a>
<h4>Testing</h4>  <a name="testing12"></a>

<h3>Soal 13</h3>
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
<h4>Solusi</h4> <a name="solusi13"></a>
<h4>Testing</h4>  <a name="testing13"></a>

<h3>Soal 14</h3>
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).
<h4>Solusi</h4> <a name="solusi14"></a>
<h4>Testing</h4>  <a name="testing14"></a>

<h3>Soal 15</h3>
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
<h4>Solusi</h4> <a name="solusi15"></a>
<h4>Testing</h4>  <a name="testing15"></a>

<h3>Soal 16</h3>
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi www.parikesit.abimanyu.yyy.com/js
<h4>Solusi</h4> <a name="solusi16"></a>
<h4>Testing</h4>  <a name="testing16"></a>

<h3>Soal 17</h3>
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
<h4>Solusi</h4> <a name="solusi17"></a>
<h4>Testing</h4>  <a name="testing17"></a>

<h3>Soal 18</h3
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
<h4>Solusi</h4> <a name="solusi18"></a>
<h4>Testing</h4>  <a name="testing18"></a>

<h3>Soal 19</h3>
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)
<h4>Solusi</h4> <a name="solusi19"></a>
<h4>Testing</h4>  <a name="testing19"></a>

<h3>Soal 20</h3>
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
<h4>Solusi</h4> <a name="solusi20"></a>
<h4>Testing</h4>  <a name="testing20"></a>
