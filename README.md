# Mid-Project-Jarkom-5

# Mid-Project Jarkom - Smart City Surabaya (Kelompok 5)

| Nama | NRP |
|---|---|
| Aura Syahzanani A. | 5027251123 |
| Danish Faeyza | 5027251028 |
| Naoval James O. | 5027251092 |

---

## 1. Topologi dan Pemilihan Skema

<img width="616" height="283" alt="image" src="https://github.com/user-attachments/assets/8f8560d4-b393-4b45-8088-578f39569db8" />


**Skema yang dipilih: VLSM** 

Alasan:
- Ukuran LAN sangat bervariasi (20 sampai 11.000 host), sehingga VLSM menyesuaikan ukuran subnet dengan kebutuhan dan tidak membuang alamat.
- Pemakaian ruang alamat VLSM 94,23% (muat di satu /16), dibanding CIDR 23,56% (butuh /14).
- Hasil VLSM sama dengan yang dipakai saat UTS, sehingga mudah ditelusuri dan direvisi.
- Kekurangan: tidak ada route summarization, jadi static route di R-PUSAT lebih banyak. Ini dibahas di bagian konfigurasi.

### Alamat dasar
- Alamat dasar kelompok: **10.40.0.0/13** (oktet ke-2 = 8 x 5).
- Alamat tambahan: link R-PUSAT ke R-ISP `203.0.113.20/30`, server internet `8.8.8.0/24`, tunnel GRE `192.168.5.0/30`.

## 2. Penjelasan Desain Topologi

Backbone mengikuti Gambar 1 (L1-L6). Desain tiap lokasi:

| Lokasi | Switch | Perangkat | Media | Tambahan port router | Alasan |
|---|---|---|---|---|---|
| Balai Kota | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |
| Command Center (DC) | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |
| Surabaya Barat | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |
| Surabaya Utara | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |
| Surabaya Timur | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |
| Surabaya Selatan | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |
| Pelabuhan | `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |

Penamaan perangkat: `R-PUSAT`, `SW-BALAIKOTA`, `PC-CCTV-KOTA`, dst. Setiap LAN diberi label berisi nama subnet, Network ID/prefix, dan jumlah host.

## 3. Tabel Subnetting

### 3.1 Kebutuhan subnet

| LAN | Router | Host | Prefix | Blok | Subnet mask |
|---|---|---|---|---|---|
| WiFi Publik Utara | R-UTARA | 11.000 | /18 | 16.384 | 255.255.192.0 |
| WiFi Publik Selatan | R-SELATAN | 6.500 | /19 | 8.192 | 255.255.224.0 |
| CCTV Kota | R-DC | 6.000 | /19 | 8.192 | 255.255.224.0 |
| WiFi Publik Timur | R-TIMUR | 5.500 | /19 | 8.192 | 255.255.224.0 |
| Sensor dan SCADA Pelabuhan | R-PELABUHAN | 5.000 | /19 | 8.192 | 255.255.224.0 |
| WiFi Publik Barat | R-BARAT | 4.500 | /19 | 8.192 | 255.255.224.0 |
| Pegawai Balai Kota | R-PUSAT | 1.800 | /21 | 2.048 | 255.255.248.0 |
| Sensor Banjir Timur | R-TIMUR | 700 | /22 | 1.024 | 255.255.252.0 |
| Sensor Lalu Lintas Utara | R-UTARA | 450 | /23 | 512 | 255.255.254.0 |
| Puskesmas dan Sekolah Barat | R-BARAT | 220 | /24 | 256 | 255.255.255.0 |
| Server dan Storage DC | R-DC | 180 | /24 | 256 | 255.255.255.0 |
| Kantor Pelabuhan | R-PELABUHAN | 120 | /25 | 128 | 255.255.255.128 |
| PJU Pintar Selatan | R-SELATAN | 90 | /25 | 128 | 255.255.255.128 |
| Server Balai Kota | R-PUSAT | 20 | /27 | 32 | 255.255.255.224 |

### 3.2 Tabel VLSM (10.40.0.0/16)

| Subnet | Network ID/Prefix | Rentang host | Broadcast | Gateway |
|---|---|---|---|---|
| WiFi Publik Utara | 10.40.0.0/18 | 10.40.0.1 - 10.40.63.254 | 10.40.63.255 | 10.40.0.1 |
| WiFi Publik Selatan | 10.40.64.0/19 | 10.40.64.1 - 10.40.95.254 | 10.40.95.255 | 10.40.64.1 |
| CCTV Kota | 10.40.96.0/19 | 10.40.96.1 - 10.40.127.254 | 10.40.127.255 | 10.40.96.1 |
| WiFi Publik Timur | 10.40.128.0/19 | 10.40.128.1 - 10.40.159.254 | 10.40.159.255 | 10.40.128.1 |
| Sensor dan SCADA Pelabuhan | 10.40.160.0/19 | 10.40.160.1 - 10.40.191.254 | 10.40.191.255 | 10.40.160.1 |
| WiFi Publik Barat | 10.40.192.0/19 | 10.40.192.1 - 10.40.223.254 | 10.40.223.255 | 10.40.192.1 |
| Pegawai Balai Kota | 10.40.224.0/21 | 10.40.224.1 - 10.40.231.254 | 10.40.231.255 | 10.40.224.1 |
| Sensor Banjir Timur | 10.40.232.0/22 | 10.40.232.1 - 10.40.235.254 | 10.40.235.255 | 10.40.232.1 |
| Sensor Lalu Lintas Utara | 10.40.236.0/23 | 10.40.236.1 - 10.40.237.254 | 10.40.237.255 | 10.40.236.1 |
| Puskesmas dan Sekolah Barat | 10.40.238.0/24 | 10.40.238.1 - 10.40.238.254 | 10.40.238.255 | 10.40.238.1 |
| Server dan Storage DC | 10.40.239.0/24 | 10.40.239.1 - 10.40.239.254 | 10.40.239.255 | 10.40.239.1 |
| Kantor Pelabuhan | 10.40.240.0/25 | 10.40.240.1 - 10.40.240.126 | 10.40.240.127 | 10.40.240.1 |
| PJU Pintar Selatan | 10.40.240.128/25 | 10.40.240.129 - 10.40.240.254 | 10.40.240.255 | 10.40.240.129 |
| Server Balai Kota | 10.40.241.0/27 | 10.40.241.1 - 10.40.241.30 | 10.40.241.31 | 10.40.241.1 |
| L1 (PUSAT - DC) | 10.40.241.32/30 | .33 - .34 | 10.40.241.35 | - |
| L2 (PUSAT - UTARA) | 10.40.241.36/30 | .37 - .38 | 10.40.241.39 | - |
| L3 (PUSAT - SELATAN) | 10.40.241.40/30 | .41 - .42 | 10.40.241.43 | - |
| L4 (PUSAT - TIMUR) | 10.40.241.44/30 | .45 - .46 | 10.40.241.47 | - |
| L5 (PUSAT - BARAT) | 10.40.241.48/30 | .49 - .50 | 10.40.241.51 | - |
| L6 (UTARA - PELABUHAN) | 10.40.241.52/30 | .53 - .54 | 10.40.241.55 | - |
| L7 (DC - UTARA, OSPF) | 10.40.241.56/30 | .57 - .58 | 10.40.241.59 | - |

### 3.3 Pohon penggabungan CIDR

| Router | Anggota | Jumlah alamat | Blok | Supernet |
|---|---|---|---|---|
| R-PUSAT | Pegawai 2048 + Server BK 32 + L1-L5 20 | 2.100 | 4.096 (/20) | 10.42.0.0/20 |
| R-DC | CCTV 8192 + Server DC 256 | 8.448 | 16.384 (/18) | 10.41.0.0/18 |
| R-BARAT | WiFi 8192 + Puskesmas 256 | 8.448 | 16.384 (/18) | 10.41.64.0/18 |
| R-TIMUR | WiFi 8192 + Banjir 1024 | 9.216 | 16.384 (/18) | 10.41.128.0/18 |
| R-SELATAN | WiFi 8192 + PJU 128 | 8.320 | 16.384 (/18) | 10.41.192.0/18 |
| R-PELABUHAN | SCADA 8192 + Kantor 128 | 8.320 | 16.384 (/18) | 10.40.64.0/18 |
| R-UTARA | WiFi 16384 + Sensor LL 512 + L6 4 + R-PELABUHAN 16384 | 33.284 | 65.536 (/16) | 10.40.0.0/16 |
| **Root** | R-UTARA + 4 x 16384 + R-PUSAT 4096 | 135.168 | 262.144 (/14) | 10.40.0.0/14 |

### 3.4 Tabel alokasi CIDR

| Subnet | Router | Network ID/Prefix | Rentang host | Broadcast |
|---|---|---|---|---|
| WiFi Publik Utara | R-UTARA | 10.40.0.0/18 | 10.40.0.1 - 10.40.63.254 | 10.40.63.255 |
| Sensor dan SCADA Pelabuhan | R-PELABUHAN | 10.40.64.0/19 | 10.40.64.1 - 10.40.95.254 | 10.40.95.255 |
| Kantor Pelabuhan | R-PELABUHAN | 10.40.96.0/25 | 10.40.96.1 - 10.40.96.126 | 10.40.96.127 |
| Sensor Lalu Lintas Utara | R-UTARA | 10.40.128.0/23 | 10.40.128.1 - 10.40.129.254 | 10.40.129.255 |
| L6 | R-UTARA | 10.40.130.0/30 | .1 - .2 | 10.40.130.3 |
| CCTV Kota | R-DC | 10.41.0.0/19 | 10.41.0.1 - 10.41.31.254 | 10.41.31.255 |
| Server dan Storage DC | R-DC | 10.41.32.0/24 | 10.41.32.1 - 10.41.32.254 | 10.41.32.255 |
| WiFi Publik Barat | R-BARAT | 10.41.64.0/19 | 10.41.64.1 - 10.41.95.254 | 10.41.95.255 |
| Puskesmas dan Sekolah Barat | R-BARAT | 10.41.96.0/24 | 10.41.96.1 - 10.41.96.254 | 10.41.96.255 |
| WiFi Publik Timur | R-TIMUR | 10.41.128.0/19 | 10.41.128.1 - 10.41.159.254 | 10.41.159.255 |
| Sensor Banjir Timur | R-TIMUR | 10.41.160.0/22 | 10.41.160.1 - 10.41.163.254 | 10.41.163.255 |
| WiFi Publik Selatan | R-SELATAN | 10.41.192.0/19 | 10.41.192.1 - 10.41.223.254 | 10.41.223.255 |
| PJU Pintar Selatan | R-SELATAN | 10.41.224.0/25 | 10.41.224.1 - 10.41.224.126 | 10.41.224.127 |
| Pegawai Balai Kota | R-PUSAT | 10.42.0.0/21 | 10.42.0.1 - 10.42.7.254 | 10.42.7.255 |
| Server Balai Kota | R-PUSAT | 10.42.8.0/27 | 10.42.8.1 - 10.42.8.30 | 10.42.8.31 |
| L1 - L5 | R-PUSAT | 10.42.8.32/30, .36, .40, .44, .48 | - | - |

### 3.5 Efisiensi

| | VLSM | CIDR |
|---|---|---|
| Prefix terkecil | /16 | /14 |
| Total alamat | 65.536 | 262.144 |
| Terpakai | 61.752 | 61.752 |
| Pemakaian | 94,23% | 23,56% |
| Terbuang | 3.784 | 200.392 |

VLSM lebih efisien dengan selisih 70,67 poin persen. CIDR mengimbangi dengan route summarization per router.

### 3.6 Tabel revisi (UTS vs perbaikan)

| No | Subnet / bagian | Jawaban UTS | Perbaikan | Alasan |
|---|---|---|---|---|
| 1 | VLSM WiFi Barat (broadcast) | 10.40.233.255 | 10.40.223.255 | 192 + 32 = 224, dikurangi 1 |
| 2 | VLSM WiFi Timur (akhir host) | 10.46.159.254 | 10.40.159.254 | Salah tulis oktet ke-2 |
| 3 | VLSM Sensor Banjir (awal host) | 10.40.231.1 | 10.40.232.1 | Network ID 232.0 + 1 |
| 4 | VLSM link L1-L6 | Tidak ada | 6 x /30 di 10.40.241.32 - .52 | Tiap link butuh /30 |
| 5 | CIDR R-BARAT | Puskesmas 216 | 256 | Blok /24 = 256 |
| 6 | CIDR R-SELATAN | PJU tidak tertulis | WiFi 8192 + PJU 128 | Anggota tidak lengkap |
| 7 | CIDR Sensor LL Utara | Di R-PELABUHAN, 10.40.236.0/23 | R-UTARA, 10.40.128.0/23 | Terhubung ke R-UTARA |
| 8 | CIDR SCADA dan Kantor | 10.40.160.0/19 dan 10.40.240.0/25 | 10.40.64.0/19 dan 10.40.96.0/25 | Harus satu blok /18 agar bisa digabung |
| 9 | CIDR Kantor (broadcast) | 10.40.239.255 | 10.40.96.127 | /25: network + 127 |
| 10 | CIDR PJU (broadcast) | `TODO: sesuaikan` | 10.41.224.127 | /25: network + 127 |
| 11 | CIDR link L1-L6 | Hanya jumlah alamat | Network ID nyata | Dipakai di Packet Tracer |
| 12 | Selisih efisiensi | 71% | 70,67 poin persen | 94,23% - 23,56% |

## 4. Penjelasan Konfigurasi

> Salin potongan konfigurasi utama. Konfigurasi lengkap per router ada di folder [`config/`](config/).

### 4.1 Static routing
`TODO`: jelaskan default route (R-DC, R-BARAT, R-TIMUR, R-SELATAN ke R-PUSAT; R-PELABUHAN ke R-UTARA; R-PUSAT ke R-ISP) dan static route R-PUSAT dan R-UTARA ke jaringan di bawahnya.

### 4.2 DHCP
`TODO`: pool per router, excluded-address, DNS server, dan CCTV Kota via `ip helper-address` ke Server DC.

### 4.3 NAT
`TODO`: PAT di R-PUSAT, `ip nat inside/outside`, dan alasan R-ISP tidak punya rute ke jaringan privat.

### 4.4 Tunnel GRE
`TODO`: konfigurasi Tunnel0, rute ke tunnel, alasan router memilih tunnel, dan alasan GRE tidak mengenkripsi (hanya encapsulation). Bonus: IPsec.

### 4.5 OSPF
`TODO`: link L7, `passive-interface`, `default-information originate` di R-PUSAT, uji failover.

### 4.6 Layanan Smart City (domain `k5.smartcity.lab`)
`TODO`: DNS (www, ftp, mail), Web "Portal Smart City Surabaya", FTP, dan Email.

## 5. Bukti Pengujian

| Soal | Pengujian | Screenshot |
|---|---|---|
| 1 | Topologi, nama, label LAN | `![](images/01-topologi.png)` |
| 3 | `ping` antar LAN, `show ip route` | `![](images/03-routing.png)` |
| 4 | DHCP lease di tiap PC, CCTV dari Server DC | `![](images/04-dhcp.png)` |
| 5 | `ping 8.8.8.8`, `show ip nat translations` | `![](images/05-nat.png)` |
| 6 | `tracert` Balai Kota ke Pelabuhan lewat tunnel | `![](images/06-gre.png)` |
| 7 | `show ip route ospf`, uji link putus | `![](images/07-ospf.png)` |
| 8 | DNS, Web, FTP, Email | `![](images/08-layanan.png)` |
