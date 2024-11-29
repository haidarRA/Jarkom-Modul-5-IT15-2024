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
