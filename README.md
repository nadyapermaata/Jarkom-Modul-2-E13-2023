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

<h3>Soal 1</h3>
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut.

<h4>Solusi</h4> <a name="solusi1"></a>
    Hasil topologi yang telah dibuat adalah:
    <img width="470" alt="soal1" src="img/1.png">

- Konfigurasi network:

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

- YudhistiraDNSMaster

lakukan pengeditan `nano /etc/bind/named.conf.local`

```
zone "arjuna.e13.com" {
	type master;
	file "/etc/bind/jarkom/arjuna.e13.com";
};
```
Lalu lanjutkan
```
mkdir /etc/bind/jarkom
```
```
cp /etc/bind/db.local /etc/bind/jarkom/arjuna.e13.com
```
lakukan pengeditan `nano /etc/bind/jarkom/arjuna.e13.com`

<img width="470" alt="soal1" src="img/2.png">

```
service bind9 restart
```

<h4>Testing</h4>  <a name="testing2"></a>

- NakulaClient

```
ping arjuna.e13.com -c  5
ping www.arjuna.e13.com -c 5
host -t CNAME www.arjuna.e13.com
```

<img width="470" alt="soal1" src="img/2.png">

<h3>Soal 3</h3>
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.
<h4>Solusi</h4> <a name="solusi3"></a>

- YudhistiraDNSMaster

lakukan pengeditan `nano /etc/bind/named.conf.local`

```
zone "abimanyu.e13.com" {
	type master;
	file "/etc/bind/jarkom/abimanyu.e13.com";
};
```
Lalu lanjutkan
```
cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.e13.com
```
lakukan pengeditan `nano /etc/bind/jarkom/abimanyu.e13.com`

<img width="470" alt="soal1" src="img/3.png">

```
service bind9 restart
```
<h4>Testing</h4>  <a name="testing3"></a>

- NakulaClient

```ping abimanyu.e13.com -c  5
ping www.abimanyu.e13.com -c 5
host -t CNAME www.abimanyu.e13.com
```

<img width="470" alt="soal1" src="img/3a.png">


<h3>Soal 4</h3>
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
<h4>Solusi</h4> <a name="solusi4"></a>

- YudhistiraDNSMaster

lakukan pengeditan `nano /etc/bind/jarkom/abimanyu.e13.com`

<img width="470" alt="soal1" src="img/4.png">

service bind9 restart

<h4>Testing</h4>  <a name="testing4"></a>

- NakulaClient

```
ping parikesit.abimanyu.e13.com -c 5
```
<img width="470" alt="soal1" src="img/4a.png">


<h3>Soal 5</h3>
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)
<h4>Solusi</h4> <a name="solusi5"></a>

- YudhistiraDNSMaster

lakukan pengeditan `nano /etc/bind/named.conf.local`

```
zone "1.43.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/1.43.10.in-addr.arpa";
};
```

Kemudian lanjutkan
```
cp /etc/bind/db.local /etc/bind/jarkom/1.43.10.in-addr.arpa
```
Lalu, lakukan pengeditan `nano /etc/bind/jarkom/1.43.10.in-addr.arpa`

<img width="470" alt="soal1" src="img/5.png">

service bind9 restart

<h4>Testing</h4>  <a name="testing5"></a>

- NakulaClient
  
```
host -t PTR 10.43.1.4
```

<img width="470" alt="soal1" src="img/5a.png">


<h3>Soal 6</h3>
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.
<h4>Solusi</h4> <a name="solusi6"></a>

- YudhistiraDNSMaster

lakukan pengeditan `nano /etc/bind/named.conf.local`

```
zone "arjuna.e13.com" {
    type master;
    notify yes;
    also-notify { 10.43.2.3; }; //IP Werkudara
    allow-transfer { 10.43.2.3; }; //IP Werkudara
    file "/etc/bind/jarkom/arjuna.e13.com";
};
zone "abimanyu.e13.com" {
    type master;
    notify yes;
    also-notify { 10.43.2.3; }; //IP Werkudara
    allow-transfer { 10.43.2.3; }; //IP Werkudara
    file "/etc/bind/jarkom/abimanyu.e13.com";
};
```

service bind9 restart

- WerkudaraDNSSlave
  
lakukan pengeditan `nano /etc/bind/named.conf.local`

```
zone "arjuna.e13.com" {
    type slave;
    masters { 10.43.2.2; }; // IP Yudhistira
    file "/var/lib/bind/arjuna.e13.com";
};

zone "abimanyu.e13.com" {
    type slave;
    masters { 10.43.2.2; }; // IP Yudhistira
    file "/var/lib/bind/abimanyu.e13.com";
};
```
service bind9 restart

- YudhistiraDNSMaster

service bind9 stop

<h4>Testing</h4>  <a name="testing6"></a>

- NakulaClient
  
```
ping arjuna.e13.com -c 5
ping www.abimanyu.e13.com -c 5
ping parikesit.abimanyu.e13.com -c 5
```

<img width="470" alt="soal1" src="img/6.png">



<h3>Soal 7</h3>
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
<h4>Solusi</h4> <a name="solusi7"></a>

- YudhistiraDNSMaster
  
Lakukan Pengeditan  `nano /etc/bind/jarkom/abimanyu.e13.com`

<img width="470" alt="soal1" src="img/7.png">

Lakukan pengeditan ` nano /etc/bind/named.conf.options`

```
comment dnssec-validation auto; dan tambahkan allow-query{any;};
```

Lakukan Pengeditan  `nano /etc/bind/named.conf.local`

```
zone "abimanyu.e13.com" {
        type master;
        //notify yes;
        //also-notify {10.45.2.3;};  
        allow-transfer {10.43.2.3;}; // IP Werkudara
        file "/etc/bind/jarkom/abimanyu.e13.com";
};
```

service bind9 restart

- WerkudaraDNSSlave
  
Lakukan Pengeditan  `nano /etc/bind/named.conf.options`  
```
comment dnssec-validation auto; dan tambahkan baris allow-query{any;};
```

Lakukan Pengeditan  `nano /etc/bind/named.conf.local`  

```
zone "baratayuda.abimanyu.e13.com"{  
        type master;
        file "/etc/bind/Baratayuda/baratayuda.abimanyu.e13.com";
};  
```
Kemudian lanjutkan,
```
mkdir /etc/bind/Baratayuda
cp /etc/bind/db.local /etc/bind/Baratayuda/baratayuda.abimanyu.e13.com
```

Lakukan Pengeditan  `nano /etc/bind/Baratayuda/baratayuda.abimanyu.e13.com`  

<img width="470" alt="soal1" src="img/7a.png">

<h4>Testing</h4>  <a name="testing7"></a>

- NakulaClient
  
```
ping baratayuda.abimanyu.e13.com -c 5
ping www.baratayuda.abimanyu.e13.com -c 5
```

<img width="470" alt="soal1" src="img/7b.png">



<h3>Soal 8</h3>
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.
<h4>Solusi</h4> <a name="solusi8"></a>

- WerkudaraDNSSlave
  
Lakukan Pengeditan  `nano /etc/bind/Baratayuda/baratayuda.abimanyu.e13.com`   

<img width="470" alt="soal1" src="img/8.png">

service bind9 restart

<h4>Testing</h4>  <a name="testing8"></a>

- NakulaClient
  
```
host -t CNAME www.rjp.baratayuda.abimanyu.e13.com
ping rjp.baratayuda.abimanyu.e13.com -c 5
ping www.rjp.baratayuda.abimanyu.e13.com -c 5
host -t A rjp.baratayuda.abimanyu.e13.com
```

<img width="470" alt="soal1" src="img/8a.png">

<h3>Soal 9</h3>
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.
<h4>Solusi</h4> <a name="solusi9"></a>

- Workers (AbimanyuWebServer, PrabukusumaWebServer, WisanggeniWebServer)
  
Lakukan Pengeditan  `nano /var/www/jarkom/index.php`   

```
<?php
$hostname = gethostname();
$date = date('Y-m-d H:i:s');
$php_version = phpversion();
$username = get_current_user();


echo "Hello World!<br>";
echo "Saya adalah: $username<br>";
echo "Saat ini berada di: $hostname<br>";
echo "Versi PHP yang saya gunakan: $php_version<br>";
echo "Tanggal saat ini: $date<br>";
?>
```

service php7.2-fpm start

Lakukan Pengeditan  `nano /etc/nginx/sites-available/jarkom`   
```
server {
        listen 80; 

        root /var/www/jarkom;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.2-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}
```

Lalu lanjutkan dengan pembuatan symlink dan penghapusan file `/etc/nginx/sites-enabled/default`    
```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm /etc/nginx/sites-enabled/default
```

service nginx restart

Periksa Konfigurasi
```
nginx -t
```

- ArjunaLoadBalancer
  
Lakukan Pengeditan  `nano /etc/nginx/sites-available/jarkom`   

```
upstream myweb{
  server 10.43.1.4; # IP Abimanyu
  server 10.43.1.5; # IP Prabukusuma
  server 10.43.1.6; # IP Wisanggeni
}

server {
  listen 80;
  server_name arjuna.e13.com www.arjuna.e13.com;

  location / {
    proxy_pass http://myweb;
  }
}
```

Lalu lanjutkan dengan pembuatan symlink dan penghapusan file `/etc/nginx/sites-enabled/default`    

```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm /etc/nginx/sites-enabled/default
```

service nginx restart

Periksa Konfigurasi

```
nginx -t
```

<h4>Testing</h4>  <a name="testing9"></a>

- SadewaClient
```
lynx http://10.43.1.4
lynx http://10.43.1.5
lynx http://10.43.1.6
```

lynx http://10.43.1.4

<img width="470" alt="soal1" src="img/9a.png">

lynx http://10.43.1.5

<img width="470" alt="soal1" src="img/9b.png">

lynx http://10.43.1.6

<img width="470" alt="soal1" src="img/9c.png">

<h3>Soal 10</h3>
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
<h4>Solusi</h4> <a name="solusi10"></a>

- Workers (AbimanyuWebServer, PrabukusumaWebServer, WisanggeniWebServer)

Lakukan Pengeditan  `nano /etc/nginx/sites-available/jarkom`   

Mengganti 80 menjadi 8001/8002/8003. Sebagai contoh:

```
listen 8001; //Abimanyu
```

service nginx restart

Periksa Konfigurasi
```
nginx -t
```

- ArjunaLoadBalancer
  
Lakukan Pengeditan  `nano /etc/nginx/sites-available/jarkom`   
```
upstream myweb{
 	server 10.43.1.4:8001; #IP Abimanyu
 	server 10.43.1.5:8002; #IP Prabukusuma
	server 10.43.1.6:8003; #IP Wisanggeni
}

server {
  listen 80;
  server_name arjuna.e13.com www.arjuna.e13.com;

  location / {
    proxy_pass http://myweb;
  }
}
```

service nginx restart

Periksa Konfigurasi
```
nginx -t
```

<h4>Testing</h4>  <a name="testing10"></a>

- SadewaClient
```
lynx http://10.43.1.4:8001
lynx http://10.43.1.5:8002
lynx http://10.43.1.6:8003
```

lynx http://10.43.1.4

<img width="470" alt="soal1" src="img/9a.png">

lynx http://10.43.1.5

<img width="470" alt="soal1" src="img/9b.png">

lynx http://10.43.1.6

<img width="470" alt="soal1" src="img/9c.png">


<h3>Soal 11</h3>
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy
<h4>Solusi</h4> <a name="solusi11"></a>

- AbimanyuWebServer

Lakukan langkah-langkah berikut untuk penginstalan apache2, php dan juga wget

```
apt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y
service apache2 start
apt-get install wget -y
apt-get install unzip -y
```

Download dan unzip file dalam google drive

```
wget -O '/var/www/abimanyu.e13.com' 'https://drive.usercontent.google.com/download?id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc'
unzip -o /var/www/abimanyu.e13.com -d /var/www/
```  

Kemudian lanjutkan dengan command berikut:

```
mv /var/www/abimanyu.yyy.com /var/www/abimanyu.e13
rm /var/www/abimanyu.e13.com
rm -rf /var/www/abimanyu.yyy.com
```

Copy file  `/etc/apache2/sites-available/000-default.conf`   ke  `/etc/apache2/sites-available/abimanyu.e13.com.conf`   

```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/abimanyu.e13.com.conf
```

Hapus file 000-default.conf

```
rm /etc/apache2/sites-available/000-default.conf
```

Lakukan pengeditan  `nano /etc/apache2/sites-available/abimanyu.e13.com.conf`   

```
<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/abimanyu.e13

  ServerName abimanyu.e13.com
  ServerAlias www.abimanyu.e13.com

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Aktifkan Konfigurasi website yang telah dibuat

```
a2ensite abimanyu.e13.com.conf
```

service apache2 restart

<h4>Testing</h4>  <a name="testing11"></a>

- NakulaClient
```
lynx abimanyu.e13.com
```

<img width="470" alt="soal1" src="img/11.png">

<h3>Soal 12</h3>
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.
<h4>Solusi</h4> <a name="solusi12"></a>

- AbimanyuWebServer

Lakukan pengeditan  `nano /etc/apache2/sites-available/abimanyu.e13.com.conf`
lalu tambahkan:

```
<Directory /var/www/abimanyu.e13/index.php/home>
  Options +Indexes
</Directory>

Alias "/home" "/var/www/abimanyu.e13/index.php/home"
```

service apache2 restart

<h4>Testing</h4>  <a name="testing12"></a>

- NakulaClient
```
lynx abimanyu.e13.com/home
```
<img width="470" alt="soal1" src="img/12.png">

<h3>Soal 13</h3>
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
<h4>Solusi</h4> <a name="solusi13"></a>

- AbimanyuWebServer
  
Download dan unzip file gdrive

```
wget -O '/var/www/parikesit.abimanyu.e13.com' 'https://drive.usercontent.google.com/download?id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS'
unzip -o /var/www/parikesit.abimanyu.e13.com -d /var/www/
```

Kemudian, lanjutkan dengan command berikut

```
mv /var/www/parikesit.abimanyu.yyy.com /var/www/parikesit.abimanyu.e13
rm /var/www/parikesit.abimanyu.e13.com
rm -rf /var/www/parikesit.abimanyu.yyy.com
```
Buat file secret dalam direktori /var/www

```
mkdir /var/www/parikesit.abimanyu.e13/secret
```

Lakukan pengeditan  `nano /etc/apache2/sites-available/parikesit.abimanyu.e13.com.conf`

```
<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/parikesit.abimanyu.e13
  ServerName parikesit.abimanyu.e13.com
  ServerAlias www.parikesit.abimanyu.e13.com

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>'
```

Aktifkan File Konfigurasi

```
a2ensite parikesit.abimanyu.e13.com.conf
```

service apache2 restart

<h4>Testing</h4>  <a name="testing13"></a>

- NakulaClient
  
```
lynx parikesit.abimanyu.e13.com
```
<img width="470" alt="soal1" src="img/13.png">

<h3>Soal 14</h3>
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).
<h4>Solusi</h4> <a name="solusi14"></a>

- AbimanyuWebServer

Lakukan pengeditan  `nano /etc/apache2/sites-available/parikesit.abimanyu.e13.com.conf`
lalu tambahkan:

```
  <Directory /var/www/parikesit.abimanyu.e13/public>
          Options +Indexes
  </Directory>

  <Directory /var/www/parikesit.abimanyu.e13/secret>
          Options -Indexes
  </Directory>

  Alias "/public" "/var/www/parikesit.abimanyu.e13/public"
  Alias "/secret" "/var/www/parikesit.abimanyu.e13/secret"
```

service apache2 restart

<h4>Testing</h4>  <a name="testing14"></a>

- NakulaClient

```
lynx parikesit.abimanyu.e13.com/public
lynx parikesit.abimanyu.e13.com/secret
```
lynx parikesit.abimanyu.e13.com/public

<img width="470" alt="soal1" src="img/14a.png">

lynx parikesit.abimanyu.e13.com/secret

<img width="470" alt="soal1" src="img/14b.png">

<h3>Soal 15</h3>
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
<h4>Solusi</h4> <a name="solusi15"></a>

- AbimanyuWebServer

Lakukan pengeditan  `nano /etc/apache2/sites-available/parikesit.abimanyu.e13.com.conf`
latu tambahkan:

```
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
```

service apache2 restart

<h4>Testing</h4>  <a name="testing15"></a>

- NakulaClient
```
lynx parikesit.abimanyu.e13.com/testerror
lynx parikesit.abimanyu.e13.com/secret
```
lynx parikesit.abimanyu.e13.com/testerror

<img width="470" alt="soal1" src="img/15a.png">

lynx parikesit.abimanyu.e13.com/secret

<img width="470" alt="soal1" src="img/15b.png">

<h3>Soal 16</h3>
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi www.parikesit.abimanyu.yyy.com/js
<h4>Solusi</h4> <a name="solusi16"></a>

- AbimanyuWebServer

Lakukan pengeditan  `nano /etc/apache2/sites-available/parikesit.abimanyu.e13.com.conf`
lalu tambahkan:

```
 Alias "/js" "/var/www/parikesit.abimanyu.e13/public/js"
```

service apache2 restart

<h4>Testing</h4>  <a name="testing16"></a>

- NakulaClient
  
```
lynx parikesit.abimanyu.e13.com/public/js
lynx parikesit.abimanyu.e13.com/js
```

Hasil Sama:

<img width="470" alt="soal1" src="img/16.png">


<h3>Soal 17</h3>
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
<h4>Solusi</h4> <a name="solusi17"></a>

- AbimanyuWebServer
  
Buat file direktori baratayuda.abimanyu.e13
  
```
mkdir /var/www/rjp.baratayuda.abimanyu.e13
```
Download file gdrive

```
wget --no-check-certificate "https://drive.google.com/uc?export=download&id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6" -O /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.e13.com.zip
```
Unzip file
  
```
unzip /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.e13.com.zip -d /var/www/rjp.baratayuda.abimanyu.e13/
```

Kemudian lanjutkan dengan baris perintah berikut:
```
mv /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.yyy.com/anya-bond.webp /var/www/rjp.baratayuda.abimanyu.e13/
mv /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.yyy.com/loid.png /var/www/rjp.baratayuda.abimanyu.e13/
mv /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.yyy.com/waku.mp3 /var/www/rjp.baratayuda.abimanyu.e13/
mv /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.yyy.com/yor.jpg /var/www/rjp.baratayuda.abimanyu.e13/

rm -r /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.e13.com.zip
rm -r /var/www/rjp.baratayuda.abimanyu.e13/rjp.baratayuda.abimanyu.yyy.com/
```

Lakukan pengeditan  `nano /etc/apache2/sites-available/rjp.baratayuda.abimanyu.e13.com.conf`
lalu tambahkan:

```
<VirtualHost *:14000 *:14400>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/rjp.baratayuda.abimanyu.e13
  ServerName rjp.baratayuda.abimanyu.e13.com
  ServerAlias www.rjp.baratayuda.abimanyu.e13.com

  ErrorDocument 404 /error/404.html
  ErrorDocument 403 /error/403.html

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Lakukan pengeditan  `nano /etc/apache2/ports.conf`
tambahkan:
  
```
Listen 14000
Listen 14400
```

Aktifkan file konfigurasi

```
a2ensite rjp.baratayuda.abimanyu.e13.com.conf
```

service apache2 restart

<h4>Testing</h4>  <a name="testing17"></a>

- NakulaClient
  
```
lynx rjp.baratayuda.abimanyu.e13.com:14000
lynx rjp.baratayuda.abimanyu.e13.com:14400
lynx rjp.baratayuda.abimanyu.e13.com:88000
```

lynx rjp.baratayuda.abimanyu.e13.com:14000

<img width="470" alt="soal1" src="img/17a.png">

lynx rjp.baratayuda.abimanyu.e13.com:14400

<img width="470" alt="soal1" src="img/17b.png">

lynx rjp.baratayuda.abimanyu.e13.com:88000

<img width="470" alt="soal1" src="img/17c.png">

<h3>Soal 18</h3
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
<h4>Solusi</h4> <a name="solusi18"></a>

- AbimanyuWebServer

Lakukan pengeditan  `nano /etc/apache2/sites-available/rjp.baratayuda.abimanyu.e13.com.conf`

Tambahkan:
```
 <Directory /var/www/rjp.baratayuda.abimanyu.e13>
          AuthType Basic
          AuthName "Restricted Content"
          AuthUserFile /etc/apache2/.htsecure
          Require valid-user
  </Directory>
```

Kemudian, lanjutkan dengan baris perintah berikut
```
htpasswd -c -b /etc/apache2/.htsecure Wayang baratayudae13
```

Aktifkan file konfigurasi
```
a2ensite rjp.baratayuda.abimanyu.e13.com.conf
```

service apache2 restart

<h4>Testing</h4>  <a name="testing18"></a>

- NakulaClient
  
```
lynx rjp.baratayuda.abimanyu.e13.com:14000
```

<img width="470" alt="soal1" src="img/18a.png">

<img width="470" alt="soal1" src="img/18b.png">

<img width="470" alt="soal1" src="img/18c.png">


<h3>Soal 19</h3>
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)
<h4>Solusi</h4> <a name="solusi19"></a>

- AbimanyuWebServer

Lakukan pengeditan  `nano /etc/apache2/sites-available/000-default.conf`

```
<VirtualHost *:80>
    ServerAdmin webmaster@abimanyu.e13.com
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    Redirect / http://www.abimanyu.e13.com/
</VirtualHost>
```
```
apache2ctl configtest
```

service apache2 restart


<h4>Testing</h4>  <a name="testing19"></a>

- NakulaClient

```
lynx 10.43.1.4
```
<img width="470" alt="soal1" src="img/19.png">

<h3>Soal 20</h3>
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
<h4>Solusi</h4> <a name="solusi20"></a>

- AbimanyuWebServer

Aktifkan modul rewrite

```
a2enmod rewrite
```
	
Lakukan pengeditan  `nano /var/www/parikesit.abimanyu.e13/.htaccess`	

 ```
RewriteEngine On
RewriteCond %{REQUEST_URI} ^/public/images/(.*)(abimanyu)(.*\.(png|jpg))
RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
RewriteRule abimanyu http://parikesit.abimanyu.e13.com/public/images/abimanyu.png$1 [L,R=301]
```

Lakukan pengeditan  `nano /etc/apache2/sites-available/parikesit.abimanyu.e13.com.conf`
tambahkan:	

```
 <Directory /var/www/parikesit.abimanyu.e13>
          Options +FollowSymLinks -Multiviews
          AllowOverride All
  </Directory>
```
Aktifkan modul rewrite
```
a2enmod rewrite
```

service apache2 restart

<h4>Testing</h4>  <a name="testing20"></a>

- NakulaClient
  
```
lynx parikesit.abimanyu.e13.com/public/images/not-abimanyu.png
lynx parikesit.abimanyu.e13.com/public/images/abimanyu-student.jpg
lynx parikesit.abimanyu.e13.com/public/images/abimanyu.png
lynx parikesit.abimanyu.e13.com/public/images/buddies.jpg
lynx parikesit.abimanyu.e13.com/public/images/elegance-abim.jpg
lynx parikesit.abimanyu.e13.com/public/images/desmondwawklkl.sakdae
lynx parikesit.abimanyu.e13.com/public/images/notabimanyujustmuseum.177013
```
<img width="470" alt="soal1" src="img/20a.png">

<img width="470" alt="soal1" src="img/20b.png">
