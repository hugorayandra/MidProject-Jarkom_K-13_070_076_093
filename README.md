# MID-PROJECT
# Implementasi Jaringan Smart City Kota Surabaya

**Mata Kuliah:** Komunikasi Data dan Jaringan Komputer  
**Semester:** Gasal 2026/2027  
**Kelompok:** 13  
**Skema Pengalamatan:** VLSM  
**Simulator:** Cisco Packet Tracer  

---

## 1. Identitas Kelompok

| No. | Nama Anggota | NRP |
|---|---|---|
| 1 | Muhammad Hugo Rayandra Esmid | 5027251076 |
| 2 | Donnavie Aulia | 093 |
| 3 | Sultan Ahmad | 070 |

---

# 2. Deskripsi Proyek

Proyek ini bertujuan merancang dan membangun infrastruktur jaringan komputer untuk mendukung kebutuhan Smart City Kota Surabaya menggunakan Cisco Packet Tracer.

Jaringan dirancang dengan menghubungkan beberapa wilayah melalui router pusat (R-PUSAT) dan router wilayah. Setiap wilayah memiliki subnet yang disesuaikan dengan kebutuhan host berdasarkan perhitungan VLSM.

Pada bagian pengerjaan ini, fokus implementasi meliputi:

1. Perancangan topologi jaringan.
2. Perhitungan dan pembagian alamat IP menggunakan VLSM.
3. Konfigurasi interface router.
4. Konfigurasi static routing antarwilayah.
5. Konfigurasi default route menuju jaringan di atasnya atau jaringan luar.
6. Pengujian konektivitas antarrouter dan antarwilayah.

---

# 3. Perancangan Topologi Jaringan

## 3.1 Gambaran Topologi

Topologi jaringan menggunakan pendekatan hierarki dengan R-PUSAT sebagai router utama yang menghubungkan seluruh router wilayah.

Router yang digunakan:

1. R-PUSAT
2. R-DC
3. R-UTARA
4. R-TIMUR
5. R-SELATAN
6. R-BARAT
7. R-PELABUHAN
8. R-ISP

Setiap router wilayah terhubung dengan switch yang menghubungkan perangkat pengguna, server, dan perangkat pendukung.

## 3.2 Pembagian Wilayah

| Router | Wilayah | Jaringan/Layanan |
|---|---|---|
| R-PUSAT | Balai Kota | Pegawai dan Server Balai Kota |
| R-DC | Data Center | CCTV Kota dan Server & Storage DC |
| R-UTARA | Surabaya Utara | WiFi Utara dan Sensor Lalu Lintas |
| R-TIMUR | Surabaya Timur | WiFi Timur dan Sensor Banjir |
| R-SELATAN | Surabaya Selatan | WiFi Selatan dan PJU Pintar |
| R-BARAT | Surabaya Barat | WiFi Barat dan Puskesmas |
| R-PELABUHAN | Pelabuhan | SCADA dan Kantor Pelabuhan |
| R-ISP | Internet | Server internet 8.8.8.8 |

## 3.3 Perangkat yang Digunakan

| Perangkat | Fungsi |
|---|---|
| Router Cisco 2911 | Menghubungkan jaringan antarwilayah |
| Switch Cisco 2960 | Menghubungkan perangkat dalam LAN |
| PC-PT | Perangkat pengguna dan pengujian |
| Server-PT | Server pada jaringan Data Center, Balai Kota, dan ISP |
| Kabel Serial | Menghubungkan router melalui backbone |
| Copper Straight-Through | Menghubungkan router, switch, dan end device |

## 3.4 Alasan Pemilihan Topologi

Topologi menggunakan pendekatan hierarki dengan R-PUSAT sebagai pusat distribusi jaringan.

Pemilihan topologi ini didasarkan pada beberapa pertimbangan:

- Memudahkan pengelolaan komunikasi antarwilayah.
- Memisahkan jaringan berdasarkan lokasi dan kebutuhan layanan.
- Mempermudah proses troubleshooting.
- Memudahkan penerapan static routing.
- Memungkinkan pengembangan jaringan apabila terdapat penambahan wilayah atau perangkat.
- Memusatkan pengelolaan jalur komunikasi antarwilayah melalui R-PUSAT.

## 3.5 Dokumentasi Topologi

Lampirkan screenshot topologi Cisco Packet Tracer pada bagian ini.

Nama file yang disarankan:

`docs/topologi-smart-city.png`

![Topologi Smart City Surabaya](docs/topologi-smart-city.png)

---

# 4. Perancangan Pengalamatan IP (VLSM)

## 4.1 Dasar Pengalamatan

Pengalamatan jaringan menggunakan metode VLSM (Variable Length Subnet Mask).

VLSM memungkinkan setiap subnet menggunakan prefix yang berbeda sesuai kebutuhan jumlah host. Dengan metode ini, penggunaan alamat IP menjadi lebih efisien dibandingkan penggunaan satu subnet mask untuk seluruh jaringan.

Gateway setiap LAN menggunakan alamat host pertama dari subnet yang bersangkutan.

## 4.2 Tabel Pengalamatan LAN

Berikut pengalamatan LAN yang diterapkan pada topologi:

| No. | Nama Subnet | Network ID | Prefix | Subnet Mask | Gateway |
|---|---|---|---|---|---|
| 1 | WiFi Utara | 10.104.0.0 | /18 | 255.255.192.0 | 10.104.0.1 |
| 2 | WiFi Selatan | 10.104.64.0 | /19 | 255.255.224.0 | 10.104.64.1 |
| 3 | CCTV Kota | 10.104.96.0 | /19 | 255.255.224.0 | 10.104.96.1 |
| 4 | WiFi Timur | 10.104.128.0 | /19 | 255.255.224.0 | 10.104.128.1 |
| 5 | SCADA Pelabuhan | 10.104.160.0 | /19 | 255.255.224.0 | 10.104.160.1 |
| 6 | WiFi Barat | 10.104.192.0 | /19 | 255.255.224.0 | 10.104.192.1 |
| 7 | Pegawai Balai Kota | 10.104.224.0 | /21 | 255.255.248.0 | 10.104.224.1 |
| 8 | Sensor Lalu Lintas | 10.104.236.0 | /23 | 255.255.254.0 | 10.104.236.1 |
| 9 | Sensor Banjir Timur | 10.104.232.0 | /22 | 255.255.252.0 | 10.104.232.1 |
| 10 | Puskesmas Barat | 10.104.238.0 | /24 | 255.255.255.0 | 10.104.238.1 |
| 11 | Server & Storage DC | 10.104.239.0 | /24 | 255.255.255.0 | 10.104.239.1 |
| 12 | Kantor Pelabuhan | 10.104.240.0 | /25 | 255.255.255.128 | 10.104.240.1 |
| 13 | PJU Pintar Selatan | 10.104.240.128 | /25 | 255.255.255.128 | 10.104.240.129 |
| 14 | Server Balai Kota | 10.104.241.0 | /27 | 255.255.255.224 | 10.104.241.1 |

**Catatan:** Tabel ini merupakan ringkasan pengalamatan yang digunakan dalam konfigurasi topologi. Jumlah host yang digunakan untuk perhitungan VLSM harus mengikuti kebutuhan host penuh dari hasil UTS kelompok.

## 4.3 Tabel Rentang Host dan Broadcast

| Nama Subnet | Network ID | Prefix | Host Pertama | Host Terakhir | Broadcast |
|---|---|---|---|---|---|
| WiFi Utara | 10.104.0.0 | /18 | 10.104.0.1 | 10.104.63.254 | 10.104.63.255 |
| WiFi Selatan | 10.104.64.0 | /19 | 10.104.64.1 | 10.104.95.254 | 10.104.95.255 |
| CCTV Kota | 10.104.96.0 | /19 | 10.104.96.1 | 10.104.127.254 | 10.104.127.255 |
| WiFi Timur | 10.104.128.0 | /19 | 10.104.128.1 | 10.104.159.254 | 10.104.159.255 |
| SCADA Pelabuhan | 10.104.160.0 | /19 | 10.104.160.1 | 10.104.191.254 | 10.104.191.255 |
| WiFi Barat | 10.104.192.0 | /19 | 10.104.192.1 | 10.104.223.254 | 10.104.223.255 |
| Pegawai Balai Kota | 10.104.224.0 | /21 | 10.104.224.1 | 10.104.231.254 | 10.104.231.255 |
| Sensor Banjir Timur | 10.104.232.0 | /22 | 10.104.232.1 | 10.104.235.254 | 10.104.235.255 |
| Sensor Lalu Lintas | 10.104.236.0 | /23 | 10.104.236.1 | 10.104.237.254 | 10.104.237.255 |
| Puskesmas Barat | 10.104.238.0 | /24 | 10.104.238.1 | 10.104.238.254 | 10.104.238.255 |
| Server & Storage DC | 10.104.239.0 | /24 | 10.104.239.1 | 10.104.239.254 | 10.104.239.255 |
| Kantor Pelabuhan | 10.104.240.0 | /25 | 10.104.240.1 | 10.104.240.126 | 10.104.240.127 |
| PJU Pintar Selatan | 10.104.240.128 | /25 | 10.104.240.129 | 10.104.240.254 | 10.104.240.255 |
| Server Balai Kota | 10.104.241.0 | /27 | 10.104.241.1 | 10.104.241.30 | 10.104.241.31 |

## 4.4 Pengalamatan Backbone Antarrouter

Backbone antarrouter menggunakan subnet /30 karena setiap link point-to-point membutuhkan dua alamat host.

| Link | Router 1 | IP Router 1 | Router 2 | IP Router 2 | Network |
|---|---|---|---|---|---|
| L1 | R-PUSAT | 10.104.241.33 | R-DC | 10.104.241.34 | 10.104.241.32/30 |
| L2 | R-PUSAT | 10.104.241.37 | R-UTARA | 10.104.241.38 | 10.104.241.36/30 |
| L3 | R-PUSAT | 10.104.241.41 | R-SELATAN | 10.104.241.42 | 10.104.241.40/30 |
| L4 | R-PUSAT | 10.104.241.45 | R-TIMUR | 10.104.241.46 | 10.104.241.44/30 |
| L5 | R-PUSAT | 10.104.241.49 | R-BARAT | 10.104.241.50 | 10.104.241.48/30 |
| L6 | R-UTARA | 10.104.241.53 | R-PELABUHAN | 10.104.241.54 | 10.104.241.52/30 |

## 4.5 Pengalamatan Link ISP

Sesuai instruksi soal, link R-PUSAT ke R-ISP menggunakan:

`203.0.113.(4 × K)/30`

Dengan nomor kelompok K = 13:

| Parameter | Nilai |
|---|---|
| Network | 203.0.113.52/30 |
| R-PUSAT | 203.0.113.53 |
| R-ISP | 203.0.113.54 |
| Subnet Mask | 255.255.255.252 |

Server internet:

| Parameter | Nilai |
|---|---|
| Network | 8.8.8.0/24 |
| IP Server | 8.8.8.8 |
| Gateway | 8.8.8.1 |
| Subnet Mask | 255.255.255.0 |

---

# 5. Konfigurasi Interface Router

## 5.1 Konfigurasi R-PUSAT

R-PUSAT berfungsi sebagai router pusat yang menghubungkan seluruh wilayah.

### Interface LAN

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.224.1 255.255.248.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.104.241.1 255.255.255.224
no shutdown
exit
```

### Interface Backbone

```cisco
interface serial 0/2/0
ip address 10.104.241.33 255.255.255.252
no shutdown
exit

interface serial 0/2/1
ip address 10.104.241.37 255.255.255.252
no shutdown
exit

interface serial 0/3/0
ip address 10.104.241.41 255.255.255.252
no shutdown
exit

interface serial 0/3/1
ip address 10.104.241.45 255.255.255.252
no shutdown
exit

interface serial 0/1/0
ip address 10.104.241.49 255.255.255.252
no shutdown
exit
```

### Pemeriksaan Interface

```cisco
show ip interface brief
```

## 5.2 Konfigurasi R-DC

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.96.1 255.255.224.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.104.239.1 255.255.255.0
no shutdown
exit

interface serial 0/3/0
ip address 10.104.241.34 255.255.255.252
no shutdown
exit

end
```

## 5.3 Konfigurasi R-UTARA

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.0.1 255.255.192.0
no shutdown
exit

interface gigabitEthernet 0/2
ip address 10.104.241.53 255.255.255.252
no shutdown
exit

interface serial 0/3/0
ip address 10.104.241.38 255.255.255.252
no shutdown
exit

end
```

## 5.4 Konfigurasi R-TIMUR

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.128.1 255.255.224.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.104.232.1 255.255.252.0
no shutdown
exit

interface serial 0/3/0
ip address 10.104.241.46 255.255.255.252
no shutdown
exit

end
```

## 5.5 Konfigurasi R-SELATAN

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.240.129 255.255.255.128
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.104.64.1 255.255.224.0
no shutdown
exit

interface serial 0/3/0
ip address 10.104.241.42 255.255.255.252
no shutdown
exit

end
```

## 5.6 Konfigurasi R-BARAT

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.192.1 255.255.224.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.104.238.1 255.255.255.0
no shutdown
exit

interface serial 0/3/0
ip address 10.104.241.50 255.255.255.252
no shutdown
exit

end
```

## 5.7 Konfigurasi R-PELABUHAN

```cisco
enable
configure terminal

interface gigabitEthernet 0/0
ip address 10.104.160.1 255.255.224.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.104.241.54 255.255.255.252
no shutdown
exit

interface gigabitEthernet 0/2
ip address 10.104.240.1 255.255.255.128
no shutdown
exit

end
```

## 5.8 Pemeriksaan Interface

Pada setiap router, jalankan:

```cisco
show ip interface brief
```

Interface yang telah dikonfigurasi harus menunjukkan status `up/up`.

---

# 6. Konfigurasi Static Routing

## 6.1 Tujuan Static Routing

Static routing digunakan untuk menentukan jalur komunikasi antarjaringan secara manual.

Sesuai ketentuan soal:

- Seluruh LAN harus dapat saling berkomunikasi menggunakan static route.
- Default route hanya boleh mengarah ke router di atasnya atau ke luar.
- R-PUSAT harus memiliki static route menuju seluruh LAN milik R-DC, R-UTARA, R-PELABUHAN, R-SELATAN, R-TIMUR, dan R-BARAT.
- R-UTARA harus memiliki static route menuju seluruh LAN milik R-PELABUHAN.

## 6.2 Static Route R-PUSAT

R-PUSAT perlu mengetahui jaringan yang berada di bawah setiap router wilayah.

```cisco
enable
configure terminal

! Jaringan R-DC
ip route 10.104.96.0 255.255.224.0 10.104.241.34
ip route 10.104.239.0 255.255.255.0 10.104.241.34

! Jaringan R-UTARA dan R-PELABUHAN
ip route 10.104.0.0 255.255.192.0 10.104.241.38
ip route 10.104.236.0 255.255.254.0 10.104.241.38
ip route 10.104.160.0 255.255.224.0 10.104.241.38
ip route 10.104.240.0 255.255.255.128 10.104.241.38

! Jaringan R-SELATAN
ip route 10.104.64.0 255.255.224.0 10.104.241.42
ip route 10.104.240.128 255.255.255.128 10.104.241.42

! Jaringan R-TIMUR
ip route 10.104.128.0 255.255.224.0 10.104.241.46
ip route 10.104.232.0 255.255.252.0 10.104.241.46

! Jaringan R-BARAT
ip route 10.104.192.0 255.255.224.0 10.104.241.50
ip route 10.104.238.0 255.255.255.0 10.104.241.50

end
```

## 6.3 Default Route R-DC

R-DC meneruskan trafik menuju R-PUSAT.

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 10.104.241.33

end
```

## 6.4 Default Route R-UTARA

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 10.104.241.37

end
```

### Static Route R-UTARA menuju R-PELABUHAN

Sesuai instruksi soal, R-UTARA harus memiliki static route menuju seluruh LAN milik R-PELABUHAN.

```cisco
ip route 10.104.160.0 255.255.224.0 10.104.241.54
ip route 10.104.240.0 255.255.255.128 10.104.241.54
```

## 6.5 Default Route R-TIMUR

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 10.104.241.45

end
```

## 6.6 Default Route R-SELATAN

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 10.104.241.41

end
```

## 6.7 Default Route R-BARAT

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 10.104.241.49

end
```

## 6.8 Default Route R-PELABUHAN

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 10.104.241.53

end
```

## 6.9 Default Route R-PUSAT menuju ISP

Sesuai instruksi soal, R-PUSAT harus meneruskan trafik ke jaringan luar melalui R-ISP.

Untuk kelompok 13, gunakan:

```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 203.0.113.54

end
```

Link ISP menggunakan:
- R-PUSAT: `203.0.113.53/30`
- R-ISP: `203.0.113.54/30`

Pastikan interface ISP dan R-PUSAT telah menggunakan alamat tersebut sebelum mengaktifkan default route ini.

## 6.10 Pemeriksaan Tabel Routing

Untuk memeriksa seluruh tabel routing:

```cisco
show ip route
```

Untuk melihat konfigurasi static route:

```cisco
show running-config | include ip route
```

---

# 7. Pengujian Konektivitas

Pengujian dilakukan untuk memastikan setiap router dapat berkomunikasi dengan router tetangga maupun jaringan lain.

## 7.1 Pengujian Backbone L1–L6

| Link | Koneksi | Hasil Pengujian |
|---|---|---|
| L1 | R-PUSAT ↔ R-DC | 100% |
| L2 | R-PUSAT ↔ R-UTARA | 100% |
| L3 | R-PUSAT ↔ R-SELATAN | 100% |
| L4 | R-PUSAT ↔ R-TIMUR | 100% |
| L5 | R-PUSAT ↔ R-BARAT | 100% |
| L6 | R-UTARA ↔ R-PELABUHAN | 100% |

Perintah pengujian:

```cisco
ping <alamat-IP-router-tujuan>
```

## 7.2 Pengujian R-DC

```cisco
ping 10.104.241.33
ping 10.104.224.1
ping 10.104.239.1
```

Hasil pengujian: seluruh ping berhasil dengan tingkat keberhasilan 100%.

## 7.3 Pengujian R-SELATAN

```cisco
ping 10.104.241.41
ping 10.104.224.1
ping 10.104.239.1
```

Hasil pengujian: seluruh ping berhasil dengan tingkat keberhasilan 100%.

## 7.4 Pengujian R-TIMUR

```cisco
ping 10.104.241.45
ping 10.104.224.1
ping 10.104.239.1
```

Hasil pengujian: seluruh ping berhasil dengan tingkat keberhasilan 100%.

## 7.5 Pengujian R-BARAT

```cisco
ping 10.104.241.49
ping 10.104.224.1
ping 10.104.239.1
ping 10.104.241.34
```

Hasil pengujian: seluruh ping berhasil dengan tingkat keberhasilan 100%.

## 7.6 Pengujian R-PELABUHAN

```cisco
ping 10.104.241.53
ping 10.104.224.1
ping 10.104.239.1
ping 10.104.236.1
```

Pada pengujian awal, terdapat kendala komunikasi menuju jaringan Balai Kota dan Data Center. Setelah penambahan static route menuju subnet backbone L6 pada R-PUSAT, konektivitas berhasil mencapai 100%.

Perintah perbaikan yang diterapkan:

```cisco
ip route 10.104.241.52 255.255.255.252 10.104.241.38
```

## 7.7 Pengujian ISP

Pengujian koneksi ISP dilakukan dari R-ISP dan R-PUSAT.

Dari R-ISP:

```cisco
ping 203.0.113.53
ping 8.8.8.8
```

Dari R-PUSAT:

```cisco
ping 203.0.113.54
ping 8.8.8.1
ping 8.8.8.8
```

**Catatan:** Pengujian koneksi server internet sebelumnya berhasil menggunakan konfigurasi sementara. Konfigurasi transit harus disesuaikan terlebih dahulu dengan alamat resmi soal, yaitu 203.0.113.52/30, sebelum pengujian final.

---

# 8. Troubleshooting

Selama pengerjaan, terdapat beberapa kendala yang ditemukan dan diperbaiki.

## 8.1 Interface Router Down

**Masalah:** Interface router menunjukkan status `administratively down`.

**Penyebab:** Interface belum diaktifkan.

**Solusi:**

```cisco
configure terminal
interface gigabitEthernet 0/1
no shutdown
```

## 8.2 Static Route Belum Tersedia

**Masalah:** R-PELABUHAN belum dapat menjangkau beberapa jaringan di Balai Kota dan Data Center.

**Penyebab:** R-PUSAT belum memiliki jalur balik menuju subnet backbone L6.

**Solusi:**

```cisco
ip route 10.104.241.52 255.255.255.252 10.104.241.38
```

Setelah konfigurasi ditambahkan, pengujian ping berhasil mencapai 100%.

## 8.3 Penyesuaian Port Router

Pada proses konfigurasi, beberapa interface harus disesuaikan dengan koneksi kabel fisik pada topologi Packet Tracer.

Pemeriksaan dilakukan menggunakan:

```cisco
show ip interface brief
```

Selain itu, koneksi kabel diperiksa secara langsung melalui Packet Tracer untuk memastikan IP address dipasang pada interface yang benar.

---

# 9. Dokumentasi dan Bukti Pengujian

Screenshot yang perlu dilampirkan:

1. Topologi keseluruhan Smart City Surabaya.
2. Tabel VLSM.
3. Konfigurasi interface R-PUSAT.
4. Konfigurasi interface R-DC.
5. Konfigurasi interface R-UTARA.
6. Konfigurasi interface R-TIMUR.
7. Konfigurasi interface R-SELATAN.
8. Konfigurasi interface R-BARAT.
9. Konfigurasi interface R-PELABUHAN.
10. Output `show ip route`.
11. Pengujian ping backbone L1–L6.
12. Pengujian komunikasi antarwilayah.
13. Pengujian koneksi R-ISP dan server internet.

Struktur folder dokumentasi:

```text
docs/
├── 01-topologi/
│   └── topologi-smart-city.png
├── 02-vlsm/
│   └── tabel-vlsm.png
├── 03-interface/
│   ├── r-pusat.png
│   ├── r-dc.png
│   ├── r-utara.png
│   ├── r-timur.png
│   ├── r-selatan.png
│   ├── r-barat.png
│   └── r-pelabuhan.png
├── 04-static-routing/
│   ├── routing-table.png
│   └── pengujian-ping.png
└── 05-isp/
    └── pengujian-isp.png
```

---

# 10. Struktur File Pengumpulan

Untuk bagian pengerjaan ini, file yang perlu disiapkan:

```text
MidProject_Jarkom_K13/
│
├── PacketTracer/
│   └── K13_VLSM.pkt
│
├── Spreadsheet/
│   └── Tabel_VLSM.xlsx
│
├── Dokumentasi/
│   ├── README.md
│   └── docs/
│
└── Bukti_Pengujian/
    ├── topologi/
    ├── vlsm/
    ├── interface/
    ├── static-routing/
    └── isp/
```

Nama file ZIP akhir mengikuti format dari dosen:

```text
MidProject_Jarkom_K13_<NRP1>_<NRP2>_<NRP3>.ZIP
```

---

# 11. Kesimpulan

Pada tahap pengerjaan ini, kelompok telah melakukan perancangan topologi jaringan Smart City Kota Surabaya, pembagian alamat IP menggunakan metode VLSM, serta konfigurasi static routing pada router pusat dan router wilayah.

Topologi berhasil menghubungkan Balai Kota, Data Center, Surabaya Utara, Surabaya Timur, Surabaya Selatan, Surabaya Barat, dan Pelabuhan melalui jaringan backbone.

Hasil pengujian menunjukkan bahwa koneksi backbone L1–L6 dan komunikasi antarwilayah yang telah diuji berhasil mencapai tingkat keberhasilan 100%.

Selain itu, proses troubleshooting telah dilakukan untuk memperbaiki masalah interface dan jalur balik static routing.

Dengan demikian, bagian perancangan topologi, pengalamatan jaringan, dan static routing telah dikerjakan sebagai dasar untuk pengembangan layanan jaringan pada tahap berikutnya.

---

**Kelompok 13 — Implementasi Jaringan Smart City Kota Surabaya**  
**Mata Kuliah Komunikasi Data dan Jaringan Komputer**  
**Semester Gasal 2026/2027**
