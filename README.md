# Jarkom-Modul-5-IT15-2024

## ***KELOMPOK IT15***
| Nama      | NRP         |
|-----------|-------------|
| M. Abhinaya Al Faruqi | 5027231011  |  
| Haidar Rafi Aqyla | 5027231029   |

# Misi 1: Memetakan Kota New Eridu

## No. 1
Soal:
> Sebuah topologi sederhana menggambarkan jaringan New Eridu:
> ![image](https://github.com/user-attachments/assets/d0fe6455-76eb-432e-90ea-c25c39b37252)
>
> Keterangan:
> * HDD: Berfungsi sebagai DNS Server.
> * Fairy: Berfungsi sebagai DHCP Server.
> * Web Servers: HIA, HollowZero.
> * Client:
>   * Burnice: Memiliki 5 host.
>   * Lycaon: Memiliki 20 host.
>   * Policeboo: Memiliki 30 host.
>   * Caesar: Memiliki 50 host
>   * Ellen: Memiliki 100 host.
>   * Jane: Memiliki 200 host.

Buatlah topologi jaringan pada GNS3 seperti yang diberikan soal beserta dengan.
![topo gns3 modul 5 uncircled](https://github.com/user-attachments/assets/14f93320-744a-4432-851d-9bb781cf46fc)

## No. 2
Soal:
> Setelah membagi alamat IP menggunakan VLSM, gambarkan pohon subnet yang menunjukkan hierarki pembagian IP di jaringan New Eridu. Lingkari subnet-subnet yang akan dilewati dalam jaringan.

Berikut adalah tabel routing, tabel pembagian IP VLSM, dan tree VLSM.

### Tabel Routing:
![image](https://github.com/user-attachments/assets/cc17ea76-626f-408b-9132-253a13cb1e60)

### Tabel Pembagian IP VLSM:
![image](https://github.com/user-attachments/assets/72b88c08-59ba-4f2c-84c1-321517990b75)

### Tree VLSM:
![VLSM modul 5](https://github.com/user-attachments/assets/cbb5cfd5-88b9-4feb-834b-a74708429355)

Untuk topologi jaringan pada GNS3 dengan lingkaran pembagian IP VLSM adalah sebagai berikut.
![topo gns3 modul 5](https://github.com/user-attachments/assets/ff99c83b-8166-404d-9b51-c1bd06fd10f0)

## No. 3
Soal:
> Setelah pembagian IP selesai, buatlah konfigurasi rute untuk menghubungkan semua subnet dengan benar di jaringan New Eridu. Pastikan perangkat dapat saling terhubung.

Sebelum melakukan konfigurasi routing, konfigurasikan network interface tiap node yang ada pada jaringan dengan konfigurasi berikut.

## Network configuration

## Router 
1. NewEridu:
```
auto eth0 #DawgkerNAT
iface eth0 inet dhcp

auto eth1 #SixStreet
iface eth1 inet static
    address 10.71.1.22
    netmask 255.255.255.252

auto eth2 #LuminaSquare
iface eth2 inet static
    address 10.71.1.34
    netmask 255.255.255.252
```

2. ScootOutpost:
```
auto eth0 #A3
iface eth0 inet static
    address 10.71.1.1
    netmask 255.255.255.248
    gateway 10.71.1.3

auto eth1 #A1
iface eth1 inet static
    address 10.71.1.18
    netmask 255.255.255.252
```

## DHCP Relay
1. SixStreet:
```
auto eth0 #NewEridu
iface eth0 inet static
    address 10.71.1.21
    netmask 255.255.255.252
    gateway 10.71.1.22

auto eth1 #A3
iface eth1 inet static
    address 10.71.1.3
    netmask 255.255.255.248

auto eth2 #A4
iface eth2 inet static
    address 10.71.1.14
    netmask 255.255.255.248
```

2. OuterRing:
```
auto eth0 #A3
iface eth0 inet static
    address 10.71.1.2
    netmask 255.255.255.248
    gateway 10.71.1.3

auto eth1 #A2
iface eth1 inet static
    address 10.71.1.126
    netmask 255.255.255.192
```

3. LuminaSquare:
```
auto eth0 #NewEridu
iface eth0 inet static
    address 10.71.1.33
    netmask 255.255.255.252
    gateway 10.71.1.34

auto eth1 #A7
iface eth1 inet static
    address 10.71.1.27
    netmask 255.255.255.248

auto eth2 #A8
iface eth2 inet static
    address 10.71.0.254
    netmask 255.255.255.0
```

4. BalletTwins:
```
auto eth0 #A7
iface eth0 inet static
    address 10.71.1.25
    netmask 255.255.255.248
    gateway 10.71.1.27

auto eth1 #A6
iface eth1 inet static
    address 10.71.1.254
    netmask 255.255.255.128
```

## DNS Server
HDD:
```
auto eth0
iface eth0 inet static
    address 10.71.1.9
    netmask 255.255.255.248
    gateway 10.71.1.14
```

## DHCP Server
Fairy:
```
auto eth0
iface eth0 inet static
    address 10.71.1.10
    netmask 255.255.255.248
    gateway 10.71.1.14
```

## Web Server
1. HIA:
```
auto eth0
iface eth0 inet static
    address 10.71.1.26
    netmask 255.255.255.248
    gateway 10.71.1.27
```

2. HollowZero:
```
auto eth0
iface eth0 inet static
    address 10.71.1.17
    netmask 255.255.255.252
    gateway 10.71.1.18
```

## Client
Caesar, Burnice, Jane, Policeboo, Ellen, Lycaon:
```
auto eth0
iface eth0 inet dhcp
```

## Routing configuration

Setelah melakukan konfigurasi network, jalankan command berikut pada tiap router dan DHCP relay agar antar subnet dapat terhubung.

1. NewEridu:
```
#from SixStreet
route add -net 10.71.1.16 netmask 255.255.255.252 gw 10.71.1.21
route add -net 10.71.1.64 netmask 255.255.255.192 gw 10.71.1.21
route add -net 10.71.1.0 netmask 255.255.255.248 gw 10.71.1.21
route add -net 10.71.1.8 netmask 255.255.255.248 gw 10.71.1.21

#from LuminaSquare
route add -net 10.71.1.128 netmask 255.255.255.128 gw 10.71.1.33
route add -net 10.71.1.24 netmask 255.255.255.248 gw 10.71.1.33
route add -net 10.71.0.0 netmask 255.255.255.0 gw 10.71.1.33
```

2. SixStreet:
```
route add -net 10.71.1.16 netmask 255.255.255.252 gw 10.71.1.1
route add -net 10.71.1.64 netmask 255.255.255.192 gw 10.71.1.2
```

3. ScootOutpost:
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.71.1.3
```

4. OuterRing:
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.71.1.3
```

5. LuminaSquare:
```
route add -net 10.71.1.128 netmask 255.255.255.128 gw 10.71.1.25 #A6
```

6. BalletTwins:
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.71.1.27
```

### Test Routing
![image](https://github.com/user-attachments/assets/feb43e23-ca73-4a76-aa6f-a8ea5892ce46)

## No. 4
Soal:
> Konfigurasi → dikerjakan setelah misi 2 nomor 1
> * Fairy sebagai DHCP Server agar perangkat yang berada dalam Burnice, Caesar, Ellen, Jane, Lycaon, dan Policeboo dapat menerima alamat IP secara otomatis.
> * OuterRing, BalletTwins, Sixstreet dan LuminaSquare Sebagai DHCP Relay
> * HDD sebagai DNS server
> * HIA dan HollowZero Sebagai Web server (gunakan apache)
> 
> index.html
> ```
> Welcome to {hostname}
> ```

Sebelum melakukan konfigurasi pada DHCP relay, DHCP server, DNS server, dan DHCP server, jalankan script berikut untuk menambahkan ```nameserver 192.168.122.1``` agar dapat terhubung dengan internet.
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
```

### Script Konfigurasi
1. DHCP Relay (OuterRing, SixStreet, LuminaSquare, BalletTwins):
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

echo 'SERVERS="10.71.1.10"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay restart
```

2. DHCP Server (Fairy):
```
apt-get update
apt-get install isc-dhcp-server

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo '
#Burnice & Caesar
subnet 10.71.1.64 netmask 255.255.255.192 {
    range 10.71.1.65 10.71.1.125;
    option routers 10.71.1.126;
    option broadcast-address 10.71.1.63;
    option domain-name-servers 10.71.1.9;
}

#Jane & Policeboo
subnet 10.71.0.0 netmask 255.255.255.0 {
    range 10.71.0.1 10.71.0.253;
    option routers 10.71.0.254;
    option broadcast-address 10.71.0.255;
    option domain-name-servers 10.71.1.9;
}

#Ellen & Lycaon
subnet 10.71.1.128 netmask 255.255.255.128 {
    range 10.71.1.129 10.71.1.253;
    option routers 10.71.1.254;
    option broadcast-address 10.71.1.255;
    option domain-name-servers 10.71.1.9;
}

subnet 10.71.1.8 netmask 255.255.255.248 {
}'> /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

3. DNS Server (HDD):
```
apt-get update
apt-get install bind9 -y

echo 'options {
        directory "/var/cache/bind";

        forwarders {
            192.168.122.1;
        };

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

4. Web Server (HIA, HollowZero):
```
apt-get update
apt-get install apache2 -y

HOST=$(hostname)
echo "Welcome to $HOST" > /var/www/html/index.html

service apache2 restart
```

### Testing
1. DHCP client yang sudah mendapatkan IP address dari DHCP server
![image](https://github.com/user-attachments/assets/f6c17f72-bc75-4245-8e8f-3346bdbe954d)

2. DHCP client yang dapat terhubung dengan internet melalui DNS server
![image](https://github.com/user-attachments/assets/2f527099-45d7-4ea4-82a5-650d2d220674)

3. Akses ke web server HIA dan HollowZero (note: Install Lynx terlebih dahulu sebelum mengakses web server)
![image](https://github.com/user-attachments/assets/94de83b7-276f-4380-b9ab-e249bfe0fab1)
![image](https://github.com/user-attachments/assets/84c2044c-ea0e-4f5b-a8b6-c56ab888de2e)

# Misi 2: Menemukan Jejak Sang Peretas

## No. 1
Soal:
> Agar jaringan di New Eridu bisa terhubung ke luar (internet), kalian perlu mengkonfigurasi routing menggunakan iptables. Namun, kalian tidak diperbolehkan menggunakan MASQUERADE.

Jalankan script berikut pada router NewEridu agar dapat mengakses internet.
```
ETH0_IP=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $ETH0_IP
```

### Testing
![image](https://github.com/user-attachments/assets/8fc6f53e-13b2-487f-8ecd-eba122fcd596)

## No. 2
Soal:
> Karena Fairy adalah AI yang sangat berharga, kalian perlu memastikan bahwa tidak ada perangkat lain yang bisa melakukan ping ke Fairy. Tapi Fairy tetap dapat mengakses seluruh perangkat.

Jalankan script berikut pada DNS server Fairy agar perangkat lain tidak dapat melakukan ping ke Fairy dan masih bisa akses node lainnya dari Fairy.
```
iptables -A OUTPUT -j ACCEPT
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -j DROP
```

### Testing
1. Test ping dari perangkat/node lain ke Fairy
![image](https://github.com/user-attachments/assets/54e4e914-0817-4d2d-a1bd-9cebd0344e02)
![image](https://github.com/user-attachments/assets/ad2332f2-0e66-48a4-81f5-2e7cd5406a91)

2. Test ping dari Fairy ke perangkat/node lain
![image](https://github.com/user-attachments/assets/613ec9a0-f036-4d9c-8fca-62feb89231d4)

## No. 3
Soal:
> Selain itu, agar kejadian sebelumnya tidak terulang, hanya Fairy yang dapat mengakses HDD. Gunakan nc (netcat) untuk memastikan akses ini. [hapus aturan iptables setelah pengujian selesai agar internet tetap dapat diakses.]

Jalankan script berikut pada DHCP server HDD agar hanya Fairy yang dapat mengakses HDD.
```
iptables -A INPUT -s 10.71.1.10 -j ACCEPT
iptables -A INPUT -j DROP
```

### Testing
1. Test netcat dari Fairy ke HDD
![image](https://github.com/user-attachments/assets/022fda8c-1079-49e7-ac63-bf59ef49f38a)

2. Test netcat dari perangkat/node lain ke HDD
![image](https://github.com/user-attachments/assets/7f980eb5-7c23-43ed-aea6-0eec7dbba374)

## No. 4
Soal:
> Fairy mendeteksi aktivitas mencurigakan di server Hollow. Namun, berdasarkan peraturan polisi New Eridu, Hollow hanya boleh diakses pada hari Senin hingga Jumat dan hanya oleh faksi SoC (Burnice & Caesar) dan PubSec (Jane & Policeboo). Karena hari ini hari Sabtu, mereka harus menunggu hingga hari Senin. Gunakan curl untuk memastikan akses ini.

Jalankan script berikut pada web server HollowZero.
```
iptables -A INPUT -s 10.71.1.64/26 -m time --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 10.71.0.0/24 -m time --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```

### Testing
1. curl dari client Burnice (testing hari Jumat dan Sabtu)
![image](https://github.com/user-attachments/assets/e70e35a3-55a9-4e19-87fc-636bef57a0de)

2. Curl dari client selain Burnice, Caesar, Jane, dan Policeboo
![image](https://github.com/user-attachments/assets/feaaae48-7357-4a8c-aca6-811bdd2c9de7)

## No. 5
Soal:
> Sembari menunggu, Fairy menyarankan Phaethon untuk berlatih di server HIA dan meminta bantuan dari faksi Victoria (Ellen & Lycaon) dan PubSec. Akses HIA hanya diperbolehkan untuk a. Ellen dan Lycaon pada jam 08.00-21.00. b. Jane dan Policeboo pada jam 03.00-23.00 (hak kepolisian). Gunakan Curl untuk memastikan akses ini.

Script iptables di web server HIA:
```
iptables -A INPUT -s 10.71.0.0/24 -m time --timestart 03:00 --timestop 23:00 -j ACCEPT
iptables -A INPUT -s 10.71.1.128/25 -m time --timestart 08:00 --timestop 21:00 -j ACCEPT
iptables -A INPUT -j REJECT
```

### Testing
1. curl dari Ellen ke HIA (jam akses)

    ![image](https://github.com/user-attachments/assets/0a56ecc3-bc5a-4922-aaf2-e8027f0775be)

2. curl dari Ellen ke HIA (bukan jam akses)
![image](https://github.com/user-attachments/assets/989bf96b-7fa2-4e4f-933d-9ee17981469d)

3. curl dari Jane ke HIA (jam akses)

    ![image](https://github.com/user-attachments/assets/c3efd8ed-2583-4115-89e2-9af764e23b49)

4. curl dari Jane ke HIA (bukan jam akses)
![image](https://github.com/user-attachments/assets/3b12988f-6bf4-4a52-87b1-766dc2872d0d)

5. curl dari client selain faksi Victoria (Ellen & Lycaon) & PubSec (Jane & Policeboo)
![image](https://github.com/user-attachments/assets/c7338166-9d2b-4c8f-9d60-345cfdd8fe96)

## No. 6
Soal:
> Sebagai bagian dari pelatihan, PubSec diminta memperketat keamanan jaringan di server HIA. Jane dan Policeboo melakukan simulasi port scan menggunakan nmap pada rentang port 1–100.
> 
> a. Web server harus memblokir aktivitas scan port yang melebihi 25 port secara otomatis dalam rentang waktu 10 detik.
> 
> b. Penyerang yang terblokir tidak dapat melakukan ping, nc, atau curl ke HIA.
> 
> c. Catat log dari iptables untuk keperluan analisis dan dokumentasikan dalam format PDF.

Script untuk melakukan pemblokiran terhadap serangan nmap
```
iptables -N PORTSCAN

# Mendeteksi aktivitas baru dan menandai IP
iptables -A INPUT -p tcp -m state --state NEW -m recent --set --name portscan

# Blokir IP yang melakukan lebih dari 25 koneksi dalam 10 detik
iptables -A INPUT -p tcp -m state --state NEW -m recent --update --seconds 10 --hitcount 25 --name portscan -j DROP

# untuk melakukan logging
iptables -A INPUT -p tcp -m state --state NEW -m recent --update --seconds 10 --hitcount 25 --name portscan -j LOG --log-prefix "Port Scan Detected: " --log-level 7

#  Blokir ICMP (ping)
iptables -A INPUT -p icmp -m recent --name portscan --rcheck -j DROP

# Blokir TCP dan UDP
iptables -A INPUT -p tcp -m recent --name portscan --rcheck -j DROP
iptables -A INPUT -p udp -m recent --name portscan --rcheck -j DROP

#  Konfigurasi Forward Chain
iptables -A FORWARD -m recent --name portscan --rcheck -j DROP
```
pengujian nmap
![Screenshot 2024-12-01 050935](https://github.com/user-attachments/assets/8dd7ec6c-ae5d-4208-a016-e2143ab69b1c)

## No. 7
> Buatlah agar akses HollowZero hanya boleh berasal dari 2 koneksi aktif dari 2 ip yang berbeda dalam waktu yang bersamaan pengujian dilakukan di burnice, caeser, jane dan policeboo lakukan pengujian dengan curl

script untuk membuat akses hanya boleh berasal dari 2 koneksi aktif dari 2 ip yang berbeda dalam waktu yang bersamaan di web server HollowZero
```
iptables -I INPUT -p tcp --dport 80 -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
iptables -I INPUT -p tcp --dport 443 -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP

iptables -I INPUT -p tcp --dport 80 -m hashlimit --hashlimit-name ip_limit --hashlimit-above 2/sec --hashlimit-mode srcip --hashlimit-srcmask 32 -j DROP
iptables -I INPUT -p tcp --dport 443 -m hashlimit --hashlimit-name ip_limit --hashlimit-above 2/sec --hashlimit-mode srcip --hashlimit-srcmask 32 -j DROP

iptables -I INPUT -p tcp --dport 80 -m connlimit --connlimit-above 2 --connlimit-mask 32 -j DROP
iptables -I INPUT -p tcp --dport 443 -m connlimit --connlimit-above 2 --connlimit-mask 32 -j DROP

iptables -I INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

lakukan pengujian di 3 client yang berbeda dengan perintah berikut
```
for i in {1..100}; do curl 10.71.1.17 & done
```
![Screenshot 2024-12-01 090750](https://github.com/user-attachments/assets/c37d0cea-e4e3-488e-80dd-d93edd848609)

## No. 8
> selama ujicoba fairy memdeteksi aktivitas mencurigakan dari burnice, setiap kali fairy mengirimkan paket ke burnice paket tersebut akan di alihkan ke HollowZero. Gunakan nc untuk memastikan pengalihan tersebut

jalankan perintah - perintah tersebut sesuai dengan gambar di fairy, burnice dan HollowZero
![Screenshot 2024-12-01 093311](https://github.com/user-attachments/assets/585c3b92-94d5-411d-b7ac-cc12e120ef74)
![Screenshot 2024-12-01 093324](https://github.com/user-attachments/assets/f660e9bf-36ad-4c8d-886d-1286b9988193)
![Screenshot 2024-12-01 093336](https://github.com/user-attachments/assets/549f347a-a8ca-49f1-8e1b-d1ced390b4dc)

dapat dilihat burnice tidak menerima paket apapun namun di HollowZero mendapatkan paket yang dirimkan dari fairy


# Misi 3: Menangkap Burnice
## No. 1
> Mengetahui hal tersebut Wise dan Belle mengambil langkah drastis: memblokir semua lalu lintas masuk dan keluar dari Burnice, gunakan  nc dan ping. Burnice ya bukan Caesar.
