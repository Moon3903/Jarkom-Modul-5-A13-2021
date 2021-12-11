# Jarkom-Modul-5-A13-2021

|Nama|NRP|
|----|-----|
|Afifah Nur Sabrina Syamsudin|05111940000022|
|James Rafferty Lee|05111940000055|
|Zulfiqar Fauzul Akbar|05111940000101|

### Topologi
![image](https://user-images.githubusercontent.com/62832487/145673676-5eb00726-d041-4fa6-903a-4bbdb528ec5c.png)

### Subnetting (...)
![ok](https://user-images.githubusercontent.com/62832487/145672203-f560ac98-d8ba-464b-8b1b-a213c3915a98.jpg)

**VLSM Tree**
![treee](https://user-images.githubusercontent.com/62832487/145672197-e058cc6c-a065-4f28-864e-2b67214389dc.jpg)

**Pembagian IP**
| Subnet | Jumlah IP | Netmask | subnetmask      | nid           |
| ------ | --------- | ------- | --------------- | ------------- |
| A1     | 2         | /30     | 255.255.255.252 | 192.175.0.0   |
| A2     | 2         | /30     | 255.255.255.252 | 192.175.0.4   |
| A3     | 4         | /29     | 255.255.255.248 | 192.175.0.16  |
| A4     | 4         | /29     | 255.255.255.248 | 192.175.0.24  |
| A5     | 101       | /25     | 255.255.255.128 | 192.175.0.128 |
| A6     | 201       | /24     | 255.255.255.0   | 192.175.1.0   |
| A7     | 301       | /23     | 255.255.254.0   | 192.175.2.0   |
| A8     | 701       | /22     | 255.255.252.0   | 192.175.4.0   |
| Jumlah | 1316      | /21     | 255.255.248.0   | \-            |

### Setting GNS3
(ip disesuaikan dengan gambar diatas)
**Foosha (sebagai Router / DHCP Relay)**

**Water7 (sebagai Router / DHCP Relay)**

**Guanhao (sebagai Router / DHCP Relay)**

**Doriki (sebagai DNS Server)**

**Jipangu (sebagai DHCP Server)**

**Jorge (sebagai Web Server)**

**Maingate (sebagai Web Server)**

**Blueno (sebagai Client)**

**Cipher (sebagai Client)**

**Elena (sebagai Client)**

**Fukuruo (sebagai Client)**

## Routing
**Foosha**
```
route add -net 192.175.1.0 netmask 255.255.255.0 gw 192.175.0.6
route add -net 192.175.2.0 netmask 255.255.254.0 gw 192.175.0.6
route add -net 192.175.0.24 netmask 255.255.255.248 gw 192.175.0.6
route add -net 192.175.4.0 netmask 255.255.252.0 gw 192.175.0.2
route add -net 192.175.0.128 netmask 255.255.255.128 gw 192.175.0.2
route add -net 192.175.0.16 netmask 255.255.255.248 gw 192.175.0.2
```
## Setting DHCP Relay
**Foosha, Guanhao, dan Water7**
- Install aplikasi isc-dhcp-relay.

  ```
  apt-get install isc-dhcp-relay -y
  ```
- Edit file `/etc/default/isc-dhcp-relay` seperti berikut:

  ```
  #Defaults for isc-dhcp-relay initscript
  # sourced by /etc/init.d/isc-dhcp-relay
  # installed at /etc/default/isc-dhcp-relay by the maintainer scripts

  #
  # This is a POSIX shell fragment
  #

  # What servers should the DHCP relay forward requests to?
  SERVERS="192.175.0.19"

  # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
  INTERFACES=""

  # Additional options that are passed to the DHCP relay daemon?
  OPTIONS=""
  ```
- edit file `/etc/sysctl.conf` seperti berikut:
  ```
  net.ipv4.ip_forward=1
  net.ipv4.conf.all.accept_source_route = 1
  ```
- Restart isc-dhcp-relay.

  ```
  service isc-dhcp-relay restart
  ```

## Setting DHCP Server
**JIPANGU**
- Install aplikasi isc-dhcp-server.

  ```
  apt-get install isc-dhcp-server -y
  ```
- edit file `/etc/default/isc-dhcp-server` seperti berikut
  ```
  INTERFACE="eth0"
  ```
- edit file `/etc/dhcp/dhcpd.conf` seperti berikut

  ```
  subnet 192.175.1.0 netmask 255.255.255.0 {
        range 192.175.1.2 192.175.1.254;
        option routers 192.175.1.1;
        option broadcast-address 192.175.1.255;
        option domain-name-servers 192.175.0.18;
        default-lease-time 600;
        max-lease-time 7200;
  }
  subnet 192.175.0.128 netmask 255.255.255.128 {
          range 192.175.0.130 192.175.0.254;
          option routers 192.175.0.129;
          option broadcast-address 192.175.0.255;
          option domain-name-servers 192.175.0.18;
          default-lease-time 600;
          max-lease-time 7200;
  }
  subnet 192.175.4.0 netmask 255.255.252.0 {
          range 192.175.4.2 192.175.4.254;
          option routers 192.175.4.1;
          option broadcast-address 192.175.4.255;
          option domain-name-servers 192.175.0.18;
          default-lease-time 600;
          max-lease-time 7200;
  }
  subnet 192.175.2.0 netmask 255.255.254.0 {
          range 192.175.2.2 192.175.2.254;
          option routers 192.175.2.1;
          option broadcast-address 192.175.2.255;
          option domain-name-servers 192.175.0.18;
          default-lease-time 600;
          max-lease-time 7200;
  }
  subnet 192.175.0.16 netmask 255.255.255.248{
  }
  ```
- Restart DHCP server.

  ```
  service isc-dhcp-server restart
  ```
**chiper**
mencoba DHCP <br>
![image](https://user-images.githubusercontent.com/62832487/145672500-a14bee5c-1713-4760-863f-1696805e9002.png)

### Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

### Penjelasan
**Foosha**

`iptables -t nat -A POSTROUTING -s 192.175.0.0/16 -o eth0 -j SNAT --to-s <ip eth0>`
Catatan: ip eth0 didapatkan dengan menjalankan command `ip a` pada Foosha.

- `-t nat`: Menggunakan tabel NAT karena akan mengubah alamat asal dari paket
- `-A POSTROUTING`: Menggunakan chain POSTROUTING karena mengubah asal paket setelah routing
- `-s 192.175.0.0/16`: Mendifinisikan alamat asal dari paket yaitu semua alamat IP dari subnet 192.175.0.0/16
- `-o eth0`: Paket keluar dari eth0 Foosha
- `-j SNAT`: Menggunakan target SNAT untuk mengubah source atau alamat asal dari paket
- `--to-s` (ip eth0): Mendefinisikan IP source

**Semua node selain Foosha**
- edit file `/etc/resolv.conf` seperti berikut
  ```
  echo nameserver 192.168.122.1
  ```
  
**Blueno**

![image](https://user-images.githubusercontent.com/62832487/145672791-5714df3f-5825-40c3-b2e4-eb1538f2f081.png)


### Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

### Penjelasan
**Foosha**
- dengan command
  ```
  iptables -A FORWARD -p tcp --dport 80 -d 192.175.0.16/29 -i eth0 -j DROP
  ```
**Jipangu dan Doriki**

Install aplikasi netcat: `apt-get install netcat`.

**Testing**
- Pada JIPANGU dan DORIKI ketikkan: `nc -l -p 80`.

- Pada FOOSHA ketikkan: `nmap -p 80 192.175.0.19` dan `nmap -p 80 192.175.0.18 ` untuk mengecek.

  ![image](https://user-images.githubusercontent.com/62832487/145673045-ffae9539-67fa-4461-85fb-67592b102078.png)


### Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

### Penyelesaian
**Jipangu dan Doriki**
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

**Testing**

Lakukan ping ke Jipangu atau Doriki dari 4 client secara bersamaan, ping ke 4 akan di block.

![image](https://user-images.githubusercontent.com/62832487/145673100-57ef59e7-0c18-4f5a-85b8-737e49a082e6.png)
![image](https://user-images.githubusercontent.com/62832487/145673110-20733f08-b80d-4ce5-ab8c-cecc87e54602.png)
![image](https://user-images.githubusercontent.com/62832487/145673120-55fdbea3-e7c6-4a47-a615-653a61fb6914.png)
![image](https://user-images.githubusercontent.com/62832487/145673121-6d026f19-3f1e-4810-bc42-b352e83dbf08.png)


### Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

### Penyelesaian
**Doriki**
- install `bind9` pada doriki
```
apt-get install bind9 -y
```
menggunakan command
```
#blueno
iptables -A INPUT -s 192.175.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.175.0.128/25 -j REJECT
#chiper
iptables -A INPUT -s 192.175.4.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.175.4.0/22 -j REJECT
```
- `-A INPUT` : Menggunakan chain INPUT
- `-s 192.175.0.128/25` : Mendifinisikan alamat asal dari paket yaitu IP dari subnet Blueno
- `-s 192.175.4.0/22` : Mendifinisikan alamat asal dari paket yaitu IP dari subnet Chiper
- `-m time` : Menggunakan rule time
- `--timestart 07:00` : Mendefinisikan waktu mulai yaitu 07:00
- `--timestop 15:00`: Mendefinisikan waktu berhenti yaitu 15:00
- `--weekdays Mon,Tue,Wed,Thu` : Mendefinisikan hari yaitu Senin hingga Kamis
- `-j ACCEPT` : Paket di-accept
- `-j REJECT` : Paket ditolak

**Testing**
- Blueno.

  ![image](https://user-images.githubusercontent.com/62832487/145673274-53c4465e-183c-4c5d-9ace-b8aa164e5258.png)


### Soal 5
Akses dari subnet Elena dan Fukurou hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

### Penyelesaian
**Doriki**
dengan command
```
#elena
iptables -A INPUT -s 192.175.2.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
#fukurou
iptables -A INPUT -s 192.175.1.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```
- `-A INPUT` : Menggunakan chain INPUT
- `-s 192.175.2.0/23` : Mendifinisikan alamat asal dari paket yaitu IP dari subnet Elena
- `-s 192.175.1.0/24` : Mendifinisikan alamat asal dari paket yaitu IP dari subnet Fukurou
- `-m time` : Menggunakan rule time
- `--timestart 07:00` : Mendefinisikan waktu mulai yaitu 07:00
- `--timestop 15:00` : Mendefinisikan waktu berhenti yaitu 15:00
- `-j REJECT` : Paket ditolak
**Testing**
- Pada Elena <br>
  ![image](https://user-images.githubusercontent.com/62832487/145673355-51220efd-c35b-4f7e-8418-38e373b361f8.png)


### Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.

### Penyelesaian
**Doriki**
- membuat domain (DNS) pada file `/etc/bind/named.conf`
```
zone "jarkomA13.com" {
        type master;
        file "/etc/bind/jarkom/jarkomA13.com";
};
```
- Lalu copy `db.local` ke file `/etc/bind/jarkom/jarkomA13.com`
  ```
  mkdir /etc/bind/jarkom
  cp /etc/bind/db.local /etc/bind/jarkom/jarkomC05.com
  ```
- Lalu edit file `/etc/bind/jarkom/jarkomC05.com`
  ```
  $TTL    604800
  @       IN      SOA     jarkomA13.com. root.jarkomA13.com. (
                          2021120705      ; Serial
                           604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                           604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      jarkomA13.com.
  @       IN      A       192.175.8.1
  ```
**Guanhao**
```
iptables -A PREROUTING -t nat -p tcp -d 192.175.8.1 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.175.0.26:80
iptables -A PREROUTING -t nat -p tcp -d 192.175.8.1 --dport 80 -j DNAT --to-destination 192.175.0.27:80
iptables -t nat -A POSTROUTING -p tcp -d 192.175.0.26 --dport 80 -j SNAT --to-source 192.175.8.1:80
iptables -t nat -A POSTROUTING -p tcp -d 192.175.0.27 --dport 80 -j SNAT --to-source 192.175.8.1:80
```

**Testing**
- Pada Jorge, Maingate, Elena, dan Fukuroa install aplikasi netcat: `apt-get install netcat`.
- Pada Jorge dan Maingate ketikkan: `nc -l -p 80`.
- Pada Elena dan Fukurou ketikkan: `nc 192.175.8.1 80`
- Ketikkan sembarang kata pada Elena atau Fukurou, nanti akan muncul pada Jorge atau Maingate.

- fukurou<br>
  ![image](https://user-images.githubusercontent.com/62832487/145673600-2f773210-de81-4074-bef4-d9ee52ac0c83.png)
- elena<br>
  ![image](https://user-images.githubusercontent.com/62832487/145673606-9eb3460d-290a-429a-8115-7f0588b8131b.png)
- jorge<br>
  ![image](https://user-images.githubusercontent.com/62832487/145673609-316e5e0a-d5eb-4a5f-9ec6-0d34130c316b.png)
- maingate<br>
  ![image](https://user-images.githubusercontent.com/62832487/145673615-3ce3a79b-0171-4c91-bea1-36b4bc94e966.png)



### Kendala
- pernah debugging lama karena pernah tidak sengaja ada 2 iptables(untuk internet) pada foosha
- susah 
