# Jarkom-Modul-2-E13-2023

E13
5025211239
Najma Ulya Agustina

5025201015
NADYA PERMATA SARI


Prefix IP Kelompok E13: 10.43

Soal:
1. Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 
Kelompok E13 mendapatkan Topologi 08 sebagai berikut:

Hasil topologi yang telah dibuat adalah:

Konfigurasi network:

>> Router:

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
 

NakulaClient

auto eth0
iface eth0 inet static
	address 10.43.1.2
	netmask 255.255.255.0
	gateway 10.43.1.1
 

SadewaClient

auto eth0
iface eth0 inet static
	address 10.43.1.3
	netmask 255.255.255.0
	gateway 10.43.1.1


AbimanyuWebServer

auto eth0
iface eth0 inet static
	address 10.43.1.4
	netmask 255.255.255.0
	gateway 10.43.1.1
 

PrabukusumaWebServer

auto eth0
iface eth0 inet static
	address 10.43.1.5
	netmask 255.255.255.0
	gateway 10.43.1.1


WisanggeniWebServer

auto eth0
iface eth0 inet static
	address 10.43.1.6
	netmask 255.255.255.0
	gateway 10.43.1.1


YudhistiraDNSMaster

auto eth0
iface eth0 inet static
	address 10.43.2.2
	netmask 255.255.255.0
	gateway 10.43.2.1
 

WerkudaraDNSSlave

auto eth0
iface eth0 inet static
	address 10.43.2.3
	netmask 255.255.255.0
	gateway 10.43.2.1
 

ArjunaLoadBalancer

auto eth0
iface eth0 inet static
	address 10.43.2.4
	netmask 255.255.255.0
	gateway 10.43.2.1
 

Restart semua Node

Di Router, nano /root/.bashrc terus kasih paling bawah:

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.43.0.0/16

Kalau butuh echo nameserver 192.168.122.1 > /etc/resolv.conf


2. Buatlah website utama dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.
   

Di DNS Master yudhis:

 apt-get update
 apt-get install bind9 -y

Cd root/ dulu kemudian nano makedomain.sh

echo ‘zone "arjuna.e13.com" {
	type master;
	file "/etc/bind/jarkom/arjuna.e13.com";
};’ >> /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/arjuna.e13.com

nano /etc/bind/jarkom/arjuna.e13.com

service bind9 restart


Di NakulaClient:

Kalau belom → echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update  
apt-get install dnsutils -y  
echo "nameserver 10.43.2.2" > /etc/resolv.conf 
ping arjuna.e13.com
ping www.arjuna.e13.com
host -t CNAME www.arjuna.e13.com


3. Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.
   

Di DNS Master yudhis:

apt update
apt install bind9 -y

makedomain.sh
echo ‘zone "abimanyu.e13.com" {
	type master;
	file "/etc/bind/jarkom/abimanyu.e13.com";
};’ >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.e13.com

nano /etc/bind/jarkom/abimanyu.e13.com

service bind9 restart


Di NakulaClient:

apt-get update  
apt-get install dnsutils -y  
echo "nameserver 10.43.2.2" > /etc/resolv.conf 
ping abimanyu.e13.com
ping www.abimanyu.e13.com
host -t CNAME www.abimanyu.e13.com


4. Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.


nano /etc/bind/jarkom/jarko

Pada Yudhistira, edit file /etc/bind/jarkom/arjuna.e13.com lalu tambahkan subdomain untuk arjuna.e13.com yang mengarah ke IP Abimanyu

Restart service bind

Ping ke subdomain dari PrabukusumaWebServer


5. Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)


6. Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Yudhistira:

nano /etc/bind/named.conf.local

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

service bind9 restart


Werkudara:

echo nameserver 192.168.122.1 > /etc/resolv.conf 
apt-get update
apt-get install bind9 -y

nano /etc/bind/named.conf.local

zone "arjuna.e13.com" {
    type slave;
    masters { 10.43.2.2; }; // IP Yudhistira
    file "/var/lib/bind/abimanyu.e13.com";
};

zone "abimanyu.e13.com" {
    type slave;
    masters { 10.43.2.2; }; // IP Yudhistira
    file "/var/lib/bind/abimanyu.e13.com";
};

service bind9 restart

Yudhistira:

service bind9 stop

NakulaClient:

Cek nameserver
ping web webnya


7. Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
   

Yudhistira:

nano /etc/bind/jarkom/abimanyu.e13.com

nano /etc/bind/named.conf.options
comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options

nano /etc/bind/named.conf.local
zone "abimanyu.e13.com" {
        type master;
        //notify yes;
        //also-notify {10.45.2.3;};  
        allow-transfer {10.43.2.3;}; // IP Werkudara
        file "/etc/bind/jarkom/abimanyu.e13.com";
};


service bind9 restart

Werkudara:

nano /etc/bind/named.conf.options
comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options

nano /etc/bind/named.conf.local
zone "baratayuda.abimanyu.e13.com"{  
        type master;
        file "/etc/bind/Baratayuda/baratayuda.abimanyu.e13.com";
};  

mkdir /etc/bind/Baratayuda
cp /etc/bind/db.local /etc/bind/Baratayuda/baratayuda.abimanyu.e13.com
nano /etc/bind/Baratayuda/baratayuda.abimanyu.e13.com

service bind9 restart

NakulaClient:

ping baratayuda.abimanyu.e13.com
ping www.baratayuda.abimanyu.e13.com


8. Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

Werkudara:

nano /etc/bind/Baratayuda/baratayuda.abimanyu.e13.com

service bind9 restart

NakulaClient:

host -t CNAME www.rjp.baratayuda.abimanyu.e13.com
ping rjp.baratayuda.abimanyu.e13.com -c 5
ping www.rjp.baratayuda.abimanyu.e13.com -c 5
host -t A rjp.baratayuda.abimanyu.e13.com


9. Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.
    

>>LB ARJUNA
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 nginx -y

nano /etc/nginx/sites-available/jarkom
upstream myweb {
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

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart

nginx -t

>>WORKER

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt install nginx php php-fpm -y
php -v
>>>>mkdir; index.php
mkdir /var/www/jarkom

nano /var/www/jarkom/index.php
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

service php7.2-fpm start
nano /etc/nginx/sites-available/jarkom
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

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart
nginx -t

Nakula:

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
GAUSA apt-get install php -y
nano /etc/resolv.conf
nameserver 10.43.2.2 # IP Yudhistira
nameserver 10.43.2.3 # IP Werkudara

lynx http://10.43.1.4

lynx http://10.43.1.5

lynx http://10.43.1.6


10. Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

NOMER 10 – sebenernya nomer 9 tp ganti beberapa dikit

DII LB ARJUNA
nano /etc/nginx/sites-available/jarkom
 # Default menggunakan Round Robin
 upstream myweb  {
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

service nginx restart
nginx -t

DIII WORKERR
nano /etc/nginx/sites-available/jarkom
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

service nginx restart
nginx -t

Nakula:

echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
GAUSA apt-get install php -y
nano /etc/resolv.conf
nameserver 10.43.2.2 # IP Yudhistira
nameserver 10.43.2.3 # IP Werkudara

lynx http://10.43.1.4
lynx http://10.43.1.5
lynx http://10.43.1.6
lynx http://10.43.2.4
lynx http://arjuna.e13.com



