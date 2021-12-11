# Jarkom-Modul-5-A13-2021

|Nama|NRP|
|----|-----|
|Afifah Nur Sabrina Syamsudin|05111940000022|
|James Rafferty Lee|05111940000055|
|Zulfiqar Fauzul Akbar|05111940000101|

### Topologi

### Subnetting (...)
**Pembagian Subnet**

**Perhitungan Subnet**
|Subnet|Jumlah IP|Netmask|
|------|---------|-------|

**VLSM Tree**

**Pembagian IP**
|Subnet|Network ID|Netmask|
|------|----------|-------|

### Setting GNS3
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

## Setting DHCP Relay
**Foosha**
- Install aplikasi isc-dhcp-relay.

  ```
  apt-get install isc-dhcp-relay -y
  ```
- Edit file `/etc/default/isc-dhcp-relay` seperti gambar berikut:

  ![ss]
- Restart isc-dhcp-relay.

  ```
  service isc-dhcp-relay restart
  ```

**Water7**
- Install aplikasi isc-dhcp-relay.

  ```
  apt-get install isc-dhcp-relay -y
  ```
- Edit file `/etc/default/isc-dhcp-relay` seperti gambar berikut:

  ![ss]
- Restart isc-dhcp-relay.

  ```
  service isc-dhcp-relay restart
  ```

**Guanhao**
- Install aplikasi isc-dhcp-relay.

  ```
  apt-get install isc-dhcp-relay -y
  ```
- Edit file `/etc/default/isc-dhcp-relay` seperti gambar berikut:

  ![ss]
- Restart isc-dhcp-relay.

  ```
  service isc-dhcp-relay restart
  ```

## Setting DHCP Server
**Jipangu**
- Install aplikasi isc-dhcp-server.

  ```
  apt-get install isc-dhcp-server -y
  ```
- Edit file `/etc/default/isc-dhcp-server` seperti gambar berikut:

  ![ss]
- Edit file `/etc/dhcp/dhcpd.conf` untuk menambahkan subnet sebagai berikut:

- Restart isc-dhcp-server.

  ```
  service isc-dhcp-server restart
  ```

## Setting DNS Server
**Doriki**
- Install aplikasi bind9.

  ```
  apt-get install bind9 -y
  ```
- Edit file `/etc/bind/named.conf.options` seperti gambar berikut:

  ![ss]
- Restart bind9.

  ```
  service bind9 restart
  ```

### Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

### Penjelasan
**Foosha**

Catatan: ip eth0 didapatkan dengan menjalankan command `ip a` pada Foosha.

**Blueno**

![ss]

### Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

### Penjelasan
**Foosha**

**Jipangu dan Doriki**

Install aplikasi netcat: `apt-get install netcat`.

**Testing**
- Pada JIPANGU dan DORIKI ketikkan: `nc -l -p 80`.

  ![ss]
- Pada FOOSHA ketikkan: `nmap -p 80 ` atau `nmap -p 80 `.

  ![ss]

### Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

### Penyelesaian
**Jipangu dan Doriki**
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

**Testing**

Lakukan ping ke Jipangu atau Doriki dari 4 client secara bersamaan.

![ss]

### Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

### Penyelesaian
**Doriki**
```
```

**Testing**
- Pada Blueno ubah waktu: `date -s "3 DEC 2021 13:00:00"` dan ping Doriki.

  ![ss]
- Pada Cipher ubah waktu: `date -s "8 DEC 2021 13:00:00"` dan ping Doriki.

  ![ss]

### Soal 5
Akses dari subnet Elena dan Fukurou hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

### Penyelesaian
**Doriki**
```
iptables -A INPUT -s 192.181.2.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
iptables -A INPUT -s 192.181.1.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```

**Testing**
- Pada Elena ubah waktu: `date -s "8 DEC 2021 13:00:00"` dan ping Doriki.

  ![ss]
- Pada Fukorou ubah waktu: `date -s "8 DEC 2021 18:00:00"` dan ping Doriki.

  ![ss]

### Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.

### Penyelesaian
**Guanhao**
```
```

**Testing**
- Pada Jorge, Maingate, Elena, dan Fukuroa install aplikasi netcat: `apt-get install netcat`.
- Pada Jorge dan Maingate ketikkan: `nc -l -p 80`.
- Pada Elena dan Fukurou ketikkan: `nc 192.181.0.18 80`
- Ketikkan sembarang kata pada Elena atau Fukurou, nanti akan muncul pada Jorge atau Maingate.

![ss]

### Kendala
