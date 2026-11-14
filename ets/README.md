# UTS: Subnetting VLSM dan CIDR

Soal: rancang pengalamatan IP jaringan Smart City Surabaya dengan VLSM dan CIDR (7 router, 14 LAN, link L1–L6). Blok kelompok 4: `10.32.0.0/13`. Lembar soal ada di [`problems/`](problems/).

## Dokumen

| File                                       | Isi                                                                                                                    |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| [`kebutuhan.md`](kebutuhan.md)             | Blok alamat, tabel kebutuhan tiap subnet (host, blok, prefix, mask)                                                    |
| [`topologi.md`](topologi.md)               | Skema topologi, link antar-router, LAN per router                                                                      |
| [`subnetting-vlsm.md`](subnetting-vlsm.md) | Alokasi VLSM dalam `10.32.0.0/16` (pemakaian 94,23%)                                                                   |
| [`subnetting-cidr.md`](subnetting-cidr.md) | Agregat per router, pohon penggabungan, alokasi CIDR dalam `10.32.0.0/15` (pemakaian 47,11%), perbandingan dengan VLSM |

## File Packet Tracer

| File                               | Skema |
| ---------------------------------- | ----- |
| [`ets-vlsm.pkt`](ets-vlsm.pkt)     | VLSM  |
| [`ets-cidr.pkt`](ets-cidr_fix.pkt) | CIDR  |

Keduanya memakai topologi yang sama: 7 router 2911 (link serial `HWIC-2T` antar-router), 14 switch 2960, dan satu PC atau server perwakilan tiap LAN.

Isi konfigurasi:

- IP interface router, `clock rate` sisi DCE, dan hostname.
- Static route dasar (default route ke atas, rute ke bawah di R-PUSAT dan R-UTARA).
- IP statis host `.2` (di LAN `10.32.240.128/25` pada VLSM host `.130`), gateway = host pertama.
- Ping lintas wilayah sudah diuji.

Nama perangkat di file ini masih format UTS (`SW-Pegawai`, `PC-Pegawai`). Format huruf besar dipakai di folder [`project/`](../project/).

---

# Laporan Subnetting Jaringan Smart City Surabaya (No. 2)

---

### File Tabel VLSM & CIDR

| File                                    | Keterangan                               |
| :-------------------------------------- | :--------------------------------------- |
| [VLSM & CIDR](TABEL_VLSM_DAN_CIDR.xlsx) | Tabel Alokasi VLSM, CIDR, dan Revisi UTS |

---

## 1. Tabel Alokasi VLSM (Murni Terbesar ke Terkecil)

- **IP Induk Base:** `10.32.0.0/16`
- **Metode:** Pengurutan alokasi subnet dari ukuran blok terbesar ke terkecil secara linier tanpa celah.

| No  | Subnet / LAN               | Ukuran Blok | Prefix | Network ID    | Subnet Mask     | Rentang Host Valid            | Broadcast     | Gateway (IP Router) |
| :-: | :------------------------- | :---------: | :----: | :------------ | :-------------- | :---------------------------- | :------------ | :-----------------: |
|  1  | WiFi Publik Utara          |   16.384    |  /18   | 10.32.0.0     | 255.255.192.0   | 10.32.0.2 – 10.32.63.254      | 10.32.63.255  |      10.32.0.1      |
|  2  | WiFi Publik Selatan        |    8.192    |  /19   | 10.32.64.0    | 255.255.224.0   | 10.32.64.2 – 10.32.95.254     | 10.32.95.255  |     10.32.64.1      |
|  3  | CCTV Kota                  |    8.192    |  /19   | 10.32.96.0    | 255.255.224.0   | 10.32.96.2 – 10.32.127.254    | 10.32.127.255 |     10.32.96.1      |
|  4  | WiFi Publik Timur          |    8.192    |  /19   | 10.32.128.0   | 255.255.224.0   | 10.32.128.2 – 10.32.159.254   | 10.32.159.255 |     10.32.128.1     |
|  5  | Sensor & SCADA Pelabuhan   |    8.192    |  /19   | 10.32.160.0   | 255.255.224.0   | 10.32.160.2 – 10.32.191.254   | 10.32.191.255 |     10.32.160.1     |
|  6  | WiFi Publik Barat          |    8.192    |  /19   | 10.32.192.0   | 255.255.224.0   | 10.32.192.2 – 10.32.223.254   | 10.32.223.255 |     10.32.192.1     |
|  7  | Pegawai Balai Kota         |    2.048    |  /21   | 10.32.224.0   | 255.255.248.0   | 10.32.224.2 – 10.32.231.254   | 10.32.231.255 |     10.32.224.1     |
|  8  | Sensor Banjir Timur        |    1.024    |  /22   | 10.32.232.0   | 255.255.252.0   | 10.32.232.2 – 10.32.235.254   | 10.32.235.255 |     10.32.232.1     |
|  9  | Sensor Lalu Lintas Utara   |     512     |  /23   | 10.32.236.0   | 255.255.254.0   | 10.32.236.2 – 10.32.237.254   | 10.32.237.255 |     10.32.236.1     |
| 10  | Puskesmas & Sekolah Barat  |     256     |  /24   | 10.32.238.0   | 255.255.255.0   | 10.32.238.2 – 10.32.238.254   | 10.32.238.255 |     10.32.238.1     |
| 11  | Server & Storage DC        |     256     |  /24   | 10.32.239.0   | 255.255.255.0   | 10.32.239.2 – 10.32.239.254   | 10.32.239.255 |     10.32.239.1     |
| 12  | Kantor Pelabuhan           |     128     |  /25   | 10.32.240.0   | 255.255.255.128 | 10.32.240.2 – 10.32.240.126   | 10.32.240.127 |     10.32.240.1     |
| 13  | PJU Pintar Selatan         |     128     |  /25   | 10.32.240.128 | 255.255.255.128 | 10.32.240.130 – 10.32.240.254 | 10.32.240.255 |    10.32.240.129    |
| 14  | Server Balai Kota          |     32      |  /27   | 10.32.241.0   | 255.255.255.224 | 10.32.241.2 – 10.32.241.30    | 10.32.241.31  |     10.32.241.1     |
| 15  | L1 (R-PUSAT – R-DC)        |      4      |  /30   | 10.32.241.32  | 255.255.255.252 | 10.32.241.33 – 10.32.241.34   | 10.32.241.35  |          -          |
| 16  | L2 (R-PUSAT – R-UTARA)     |      4      |  /30   | 10.32.241.36  | 255.255.255.252 | 10.32.241.37 – 10.32.241.38   | 10.32.241.39  |          -          |
| 17  | L3 (R-PUSAT – R-SELATAN)   |      4      |  /30   | 10.32.241.40  | 255.255.255.252 | 10.32.241.41 – 10.32.241.42   | 10.32.241.43  |          -          |
| 18  | L4 (R-PUSAT – R-TIMUR)     |      4      |  /30   | 10.32.241.44  | 255.255.255.252 | 10.32.241.45 – 10.32.241.46   | 10.32.241.47  |          -          |
| 19  | L5 (R-PUSAT – R-BARAT)     |      4      |  /30   | 10.32.241.48  | 255.255.255.252 | 10.32.241.49 – 10.32.241.50   | 10.32.241.51  |          -          |
| 20  | L6 (R-UTARA – R-PELABUHAN) |      4      |  /30   | 10.32.241.52  | 255.255.255.252 | 10.32.241.53 – 10.32.241.54   | 10.32.241.55  |          -          |

### Ringkasan Alokasi Alamat IP (VLSM)

- **Total alamat terpakai:** `61.752`
- **Blok induk:** `10.32.0.0/16`
- **Pemakaian:** **`94,23%`**

---

## 2. Skema Penggabungan CIDR (Supernetting & Alokasi Per Router)

### A. Agregat per Router (Supernetting)

| No  | Router      | Blok Induk  | Prefix Agregat | Rentang IP Teralokasi       |
| :-: | :---------- | :---------- | :------------: | :-------------------------- |
|  1  | R-UTARA     | 10.32.0.0   |      /17       | 10.32.0.0 – 10.32.127.255   |
|  2  | R-DC        | 10.32.128.0 |      /18       | 10.32.128.0 – 10.32.191.255 |
|  3  | R-PELABUHAN | 10.32.192.0 |      /18       | 10.32.192.0 – 10.32.255.255 |
|  4  | R-SELATAN   | 10.33.0.0   |      /18       | 10.33.0.0 – 10.33.63.255    |
|  5  | R-TIMUR     | 10.33.64.0  |      /18       | 10.33.64.0 – 10.33.127.255  |
|  6  | R-BARAT     | 10.33.128.0 |      /18       | 10.33.128.0 – 10.33.191.255 |
|  7  | R-PUSAT     | 10.33.192.0 |      /20       | 10.33.192.0 – 10.33.207.255 |

### B. Tabel Alokasi LAN & Link CIDR

| Router                | Subnet / LAN                | Prefix | Network ID   | Rentang Host Valid          | Broadcast     | Gateway / Keterangan |
| :-------------------- | :-------------------------- | :----: | :----------- | :-------------------------- | :------------ | :------------------- |
| R-UTARA               | WiFi Publik Utara           |  /18   | 10.32.0.0    | 10.32.0.1 – 10.32.63.254    | 10.32.63.255  | 10.32.0.1            |
| R-UTARA               | Sensor Lalu Lintas Utara    |  /23   | 10.32.64.0   | 10.32.64.1 – 10.32.65.254   | 10.32.65.255  | 10.32.64.1           |
| R-DC                  | CCTV Kota                   |  /19   | 10.32.128.0  | 10.32.128.1 – 10.32.159.254 | 10.32.159.255 | 10.32.128.1          |
| R-DC                  | Server dan Storage DC       |  /24   | 10.32.160.0  | 10.32.160.1 – 10.32.160.254 | 10.32.160.255 | 10.32.160.1          |
| R-PELABUHAN           | Sensor dan SCADA Pelabuhan  |  /19   | 10.32.192.0  | 10.32.192.1 – 10.32.223.254 | 10.32.223.255 | 10.32.192.1          |
| R-PELABUHAN           | Kantor Pelabuhan            |  /25   | 10.32.224.0  | 10.32.224.1 – 10.32.224.126 | 10.32.224.127 | 10.32.224.1          |
| R-SELATAN             | WiFi Publik Selatan         |  /19   | 10.33.0.0    | 10.33.0.1 – 10.33.31.254    | 10.33.31.255  | 10.33.0.1            |
| R-SELATAN             | PJU Pintar Selatan          |  /25   | 10.33.32.0   | 10.33.32.1 – 10.33.32.126   | 10.33.32.127  | 10.33.32.1           |
| R-TIMUR               | WiFi Publik Timur           |  /19   | 10.33.64.0   | 10.33.64.1 – 10.33.95.254   | 10.33.95.255  | 10.33.64.1           |
| R-TIMUR               | Sensor Banjir Timur         |  /22   | 10.33.96.0   | 10.33.96.1 – 10.33.99.254   | 10.33.99.255  | 10.33.96.1           |
| R-BARAT               | WiFi Publik Barat           |  /19   | 10.33.128.0  | 10.33.128.1 – 10.33.159.254 | 10.33.159.255 | 10.33.128.1          |
| R-BARAT               | Puskesmas dan Sekolah Barat |  /24   | 10.33.160.0  | 10.33.160.1 – 10.33.160.254 | 10.33.160.255 | 10.33.160.1          |
| R-PUSAT               | Pegawai Balai Kota          |  /21   | 10.33.192.0  | 10.33.192.1 – 10.33.199.254 | 10.33.199.255 | 10.33.192.1          |
| R-PUSAT               | Server Balai Kota           |  /27   | 10.33.200.0  | 10.33.200.1 – 10.33.200.30  | 10.33.200.31  | 10.33.200.1          |
| R-PUSAT / R-DC        | L1 R-PUSAT – R-DC           |  /30   | 10.33.200.32 | 10.33.200.33 – 10.33.200.34 | 10.33.200.35  | -                    |
| R-PUSAT / R-UTARA     | L2 R-PUSAT – R-UTARA        |  /30   | 10.33.200.36 | 10.33.200.37 – 10.33.200.38 | 10.33.200.39  | -                    |
| R-PUSAT / R-SELATAN   | L3 R-PUSAT – R-SELATAN      |  /30   | 10.33.200.40 | 10.33.200.41 – 10.33.200.42 | 10.33.200.43  | -                    |
| R-PUSAT / R-TIMUR     | L4 R-PUSAT – R-TIMUR        |  /30   | 10.33.200.44 | 10.33.200.45 – 10.33.200.46 | 10.33.200.47  | -                    |
| R-PUSAT / R-BARAT     | L5 R-PUSAT – R-BARAT        |  /30   | 10.33.200.48 | 10.33.200.49 – 10.33.200.50 | 10.33.200.51  | -                    |
| R-UTARA / R-PELABUHAN | L6 R-UTARA – R-PELABUHAN    |  /30   | 10.32.66.0   | 10.32.66.1 – 10.32.66.2     | 10.32.66.3    | -                    |
| R-PUSAT / R-ISP       | Link R-PUSAT ke R-ISP       |  /30   | 203.0.113.16 | 203.0.113.17 – 203.0.113.18 | 203.0.113.19  | 203.0.113.18 (R-ISP) |
| R-PUSAT / R-PELABUHAN | Tunnel GRE                  |  /30   | 192.168.4.0  | 192.168.4.1 – 192.168.4.2   | 192.168.4.3   | Tunnel Interface     |
| R-ISP                 | Server Internet             |  /24   | 8.8.8.0      | 8.8.8.1 – 8.8.8.254         | 8.8.8.255     | 8.8.8.1 (R-ISP)      |

### Ringkasan Alokasi Alamat IP (CIDR)

- **Total alamat terpakai:** `61.752`
- **Blok induk:** `10.32.0.0/15`
- **Pemakaian:** **`47,11%`**

---

## 3. Catatan Perbaikan & Revisi Hasil UTS

Pencatatan resmi perbaikan kesalahan perhitungan dari lembar kerja UTS untuk memastikan efisiensi dan konsistensi laporan:

| No  | Subnet / Komponen         | Jawaban di UTS (Sebelum Revisi)                                | Perbaikan (Hasil Benar)                                                                           | Alasan Kesalahan / Analisis Koreksi                                                                                                                                                                             |
| :-: | :------------------------ | :------------------------------------------------------------- | :------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  1  | **R-DC**                  | Offset: `28675`<br>Network: `10.32.128.0`                      | Offset: `28672`<br>Network: `10.32.112.0`                                                         | Salah dalam melakukan akumulasi penjumlahan offset dari subnet sebelumnya ($24576 + 4096 = 28672$) serta salah konversi ke oktet ke-3 ($28672 / 256 = 112$).                                                    |
|  2  | **R-Pelabuhan**           | Offset: `30720`<br>Network: `10.32.136.0`                      | Offset: `30720`<br>Network: `10.32.120.0`                                                         | Offset dasar sudah benar ($28672 + 2048 = 30720$), namun terjadi kesalahan pembagian oktet ke-3 ($30720 / 256 = 120$, bukan 136).                                                                               |
|  3  | **R-Barat**               | Offset: `31744`<br>Network: `10.32.140.0`                      | Offset: `31744`<br>Network: `10.32.124.0`                                                         | Terjadi kesalahan perambatan (_propagation error_) konversi IP dari kesalahan pembagian oktet ke-3 pada baris sebelumnya ($31744 / 256 = 124$).                                                                 |
|  4  | **R-Pusat**               | Offset: `32256`<br>Network: `10.32.142.0`                      | Offset: `32256`<br>Network: `10.32.126.0`                                                         | Terjadi kesalahan perambatan konversi IP dari kesalahan pembagian oktet ke-3 ($32256 / 256 = 126$).                                                                                                             |
|  5  | **L1 – L6 (Link Router)** | Offset: `32515` dst.<br>Network: `10.32.193.0 – 10.32.199.25`  | Offset: `32512` dst.<br>Network: `10.32.127.0 – 10.32.127.52`                                     | Typo penulisan offset akumulasi ($32256 + 256 = 32512$) dan kesalahan perambatan oktet ke-3 ($32512 / 256 = 127$).                                                                                              |
|  6  | **Seluruh Subnet LAN**    | Range Host mencantumkan `.2` sebagai IP awal di beberapa baris | Range Host diseragamkan mencantumkan `.1` sampai `.254` (Host Valid), dengan `.1` sebagai Gateway | Pada UTS, Gateway diambil pada IP pertama (`.1`), sehingga IP host valid diawali dari IP kedua (`.2`). Format penulisan disempurnakan agar mencakup seluruh Usable Host Range dan kolom Gateway dipisah khusus. |

---

## Konfigurasi NAT Overload (PAT) dan Skema Routing Topologi Jaringan (Soal 5)

---

### **1. Tujuan Praktikum**

- Mengonfigurasi Network Address Translation (NAT) Overload / Port Address Translation (PAT) pada Router Pusat (`R-PUSAT`).
- Menerapkan akses kontrol (_access-list_) untuk mengizinkan seluruh subnet jaringan internal (`10.0.0.0/8`) mengakses jaringan luar/internet.
- Mengonfigurasi Router ISP (`R-ISP`) dan Server Internet (`8.8.8.8`) untuk memvalidasi keberhasilan translasi IP privat menjadi IP publik.
- Membuktikan keberhasilan koneksi end-to-end melalui pengujian `ping` dan pemantauan tabel translasi NAT.

---

### **2. Topologi dan Alamat IP**

- **IP Publik R-PUSAT ke R-ISP (`Serial0/2/1`):** `203.0.113.17 /30`
- **IP Publik R-ISP ke R-PUSAT (`Serial0/3/0`):** `203.0.113.18 /30`
- **IP Server Internet (`8.8.8.8`):** `8.8.8.1 /24` (Gateway pada `R-ISP` interface `GigabitEthernet0/0`)
- **Jaringan Internal:** Seluruh perangkat client internal menggunakan segmen IP privat `10.0.0.0/8`.

---

### **3. Langkah-Langkah Konfigurasi**

#### **A. Konfigurasi pada R-PUSAT (NAT Gateway)**

Akses CLI `R-PUSAT`, lalu jalankan perintah berikut untuk mendefinisikan _access-list_, menandai interface _inside_ dan _outside_, serta mengaktifkan NAT Overload dan _default route_:

```text
enable
configure terminal

! 1. Buat Access List untuk mencakup seluruh jaringan internal 10.0.0.0/8
access-list 1 permit 10.0.0.0 0.255.255.255

! 2. Tandai interface internal (LAN dan link serial cabang) sebagai "ip nat inside"
interface GigabitEthernet0/0
 ip nat inside
exit

interface GigabitEthernet0/1
 ip nat inside
exit

interface Serial0/0/0
 ip nat inside
exit

interface Serial0/0/1
 ip nat inside
exit

interface Serial0/1/0
 ip nat inside
exit

interface Serial0/1/1
 ip nat inside
exit

interface Serial0/2/0
 ip nat inside
exit

! 3. Tandai interface yang mengarah ke R-ISP sebagai "ip nat outside"
interface Serial0/2/1
 ip address 203.0.113.17 255.255.255.252
 ip nat outside
 no shutdown
exit

! 4. Terapkan NAT Overload (PAT) menggunakan interface publik
ip nat inside source list 1 interface Serial0/2/1 overload

! 5. Konfigurasi Default Route mengarah ke R-ISP
ip route 0.0.0.0 0.0.0.0 203.0.113.18

end
write
```

#### **B. Konfigurasi pada R-ISP**

Akses CLI R-ISP, lalu pasang modul serial (misalnya HWIC-2T) dan jalankan konfigurasi interface berikut:

```text
enable
configure terminal

hostname R-ISP

! Interface Serial yang terhubung ke R-PUSAT
interface Serial0/3/0
 ip address 203.0.113.18 255.255.255.252
 clock rate 64000
 no shutdown
exit

! Interface yang mengarah ke Server Internet (8.8.8.8)
interface GigabitEthernet0/0
 ip address 8.8.8.1 255.255.255.0
 no shutdown
exit

end
write
```

#### **C. Konfigurasi pada Server Internet (Server0 / Server-Internet)**

1. Buka tab Desktop
2. Masukkan parameter berikut:

- IPv4 Address: 8.8.8.8
- Subnet Mask: 255.255.255.0
- Default Gateway: 8.8.8.1

### **4. Hasil Pengujian dan Verifikasi**

#### **A. Pengujian Konektivitas (Ping Test)**

Dilakukan pengujian dari PC internal (contoh: PC-Pegawai dengan IP 10.33.192.2) menuju Server Internet (8.8.8.8):

```bash
C:\>ping 8.8.8.8

Pinging 8.8.8.8 with 32 bytes of data:

Reply from 8.8.8.8: bytes=32 time=6ms TTL=126
Reply from 8.8.8.8: bytes=32 time=2ms TTL=126
Reply from 8.8.8.8: bytes=32 time=29ms TTL=126
Reply from 8.8.8.8: bytes=32 time=26ms TTL=126

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
```

![Ping](assets/CekPing.png)

#### **B. Verifikasi Tabel Translasi NAT**

Dilakukan pengecekan pada CLI R-PUSAT menggunakan perintah:

```bash
R-PUSAT#show ip nat translations
Contoh Output Tabel Translasi:

Plaintext
Pro Inside global      Inside local       Outside local      Outside global
icmp 203.0.113.17:5    10.33.192.2:5      8.8.8.8:5          8.8.8.8:5
icmp 203.0.113.17:6    10.33.192.2:6      8.8.8.8:6          8.8.8.8:6
icmp 203.0.113.17:7    10.33.192.2:7      8.8.8.8:7          8.8.8.8:7
icmp 203.0.113.17:8    10.33.192.2:8      8.8.8.8:8          8.8.8.8:8
```

![Show IP](assets/ShowIP.png)

- Analisis: IP privat internal 10.33.192.2 terbukti berhasil ditranslasikan secara dinamis menggunakan port berbeda ke satu alamat IP publik 203.0.113.17 milik R-PUSAT.

---

## Konfigurasi DHCP (Soal 4)

---

### **1. Tujuan**

- Seluruh perangkat pengguna (PC, laptop, dan sensor) mendapat alamat IP, gateway, dan DNS server secara otomatis.
- Interface router dan seluruh server tetap memakai IP statis, tanpa bentrok dengan alamat yang dibagikan DHCP.
- Khusus subnet **CCTV Kota**, alamat dibagikan oleh server DHCP di LAN Server dan Storage DC (`SERVER-DC`), bukan oleh router.

File: [`project/K4_CIDR.pkt`](../project/K4_CIDR.pkt).

---

### **2. Rancangan**

| Keputusan | Alasan |
|---|---|
| Satu pool DHCP per LAN pengguna di router masing-masing | Router adalah gateway LAN, sehingga broadcast DHCP dari klien langsung diterima tanpa relay. |
| Alamat `.1` sampai `.10` di setiap LAN dikecualikan (`ip dhcp excluded-address`) | `.1` adalah gateway (host pertama). Sisanya dicadangkan untuk perangkat statis, sehingga alamat DHCP dimulai dari `.11` dan tidak pernah bentrok dengan IP statis. |
| DNS server `10.32.160.2` dan domain `k4.smartcity.lab` | DNS soal 8 berada di `SERVER-DC`. Klien langsung bisa memakai nama `www`, `ftp`, dan `mail`. |
| CCTV: tanpa pool di router, R-DC memakai `ip helper-address 10.32.160.2` di `Gi0/0` | Soal mewajibkan CCTV dilayani server DHCP di DC. Router meneruskan broadcast DHCP menjadi unicast ke `SERVER-DC` dan mengisi alamat gateway (`10.32.128.1`) di paket, sehingga server memilih pool `CCTV` yang gateway-nya cocok. |
| `serverPool` bawaan di `SERVER-DC` dipersempit ke `10.32.160.11` sampai `.254` | Pool bawaan Packet Tracer mulai dari `.0` dan mencakup 512 alamat (melebihi /24). Dipersempit agar tidak mungkin membagikan alamat gateway atau server. |

---

### **3. Langkah Konfigurasi**

#### **A. Pool DHCP di router**

```text
! R-PUSAT (LAN Pegawai Balai Kota, Gi0/0)
ip dhcp excluded-address 10.33.192.1 10.33.192.10
ip dhcp pool PEGAWAI
 network 10.33.192.0 255.255.248.0
 default-router 10.33.192.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab

! R-UTARA
ip dhcp excluded-address 10.32.0.1 10.32.0.10
ip dhcp excluded-address 10.32.64.1 10.32.64.10
ip dhcp pool WIFI-UTARA
 network 10.32.0.0 255.255.192.0
 default-router 10.32.0.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab
ip dhcp pool SENSOR-LALIN
 network 10.32.64.0 255.255.254.0
 default-router 10.32.64.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab

! R-PELABUHAN
ip dhcp excluded-address 10.32.192.1 10.32.192.10
ip dhcp excluded-address 10.32.224.1 10.32.224.10
ip dhcp pool SCADA
 network 10.32.192.0 255.255.224.0
 default-router 10.32.192.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab
ip dhcp pool KANTOR-PELABUHAN
 network 10.32.224.0 255.255.255.128
 default-router 10.32.224.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab

! R-SELATAN
ip dhcp excluded-address 10.33.0.1 10.33.0.10
ip dhcp excluded-address 10.33.32.1 10.33.32.10
ip dhcp pool WIFI-SELATAN
 network 10.33.0.0 255.255.224.0
 default-router 10.33.0.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab
ip dhcp pool PJU-SELATAN
 network 10.33.32.0 255.255.255.128
 default-router 10.33.32.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab

! R-TIMUR
ip dhcp excluded-address 10.33.64.1 10.33.64.10
ip dhcp excluded-address 10.33.96.1 10.33.96.10
ip dhcp pool WIFI-TIMUR
 network 10.33.64.0 255.255.224.0
 default-router 10.33.64.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab
ip dhcp pool SENSOR-BANJIR
 network 10.33.96.0 255.255.252.0
 default-router 10.33.96.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab

! R-BARAT
ip dhcp excluded-address 10.33.128.1 10.33.128.10
ip dhcp excluded-address 10.33.160.1 10.33.160.10
ip dhcp pool WIFI-BARAT
 network 10.33.128.0 255.255.224.0
 default-router 10.33.128.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab
ip dhcp pool PUSKESMAS
 network 10.33.160.0 255.255.255.0
 default-router 10.33.160.1
 dns-server 10.32.160.2
 domain-name k4.smartcity.lab
```

Setiap router diakhiri dengan `end` dan `write memory`.

#### **B. Relay DHCP untuk CCTV di R-DC**

```text
interface GigabitEthernet0/0
 ip helper-address 10.32.160.2
```

#### **C. Server DHCP di SERVER-DC (Services > DHCP, Service On)**

| Pool | Default Gateway | DNS Server | Start IP | Subnet Mask | Max Users |
|---|---|---|---|---|---|
| CCTV | 10.32.128.1 | 10.32.160.2 | 10.32.128.11 | 255.255.224.0 | 6000 |
| serverPool | 10.32.160.1 | 10.32.160.2 | 10.32.160.11 | 255.255.255.0 | 244 |

#### **D. Klien**

Desktop > IP Configuration > **DHCP** pada PC dan laptop. Sensor (`WiredEndDevice-PT`) tidak punya tab Desktop, sehingga DHCP dipilih di Config > FastEthernet0.

---

### **4. Hasil Pengujian**

#### **A. Alamat yang diterima klien**

| Perangkat | LAN | IP (DHCP) | Gateway | DNS | Server DHCP |
|---|---|---|---|---|---|
| PC-PEGAWAI | Pegawai Balai Kota | 10.33.192.12/21 | 10.33.192.1 | 10.32.160.2 | 10.33.192.1 (R-PUSAT) |
| PC-CCTV-KOTA | CCTV Kota | 10.32.128.11/19 | 10.32.128.1 | 10.32.160.2 | **10.32.160.2 (SERVER-DC)** |
| LAPTOP-WIFI-UTARA | WiFi Publik Utara | 10.32.0.12/18 | 10.32.0.1 | 10.32.160.2 | 10.32.0.1 (R-UTARA) |
| SENSOR-LALIN | Sensor Lalu Lintas | 10.32.64.11/23 | 10.32.64.1 | 10.32.160.2 | 10.32.64.1 (R-UTARA) |
| SENSOR-SCADA | Sensor dan SCADA | 10.32.192.12/19 | 10.32.192.1 | 10.32.160.2 | 10.32.192.1 (R-PELABUHAN) |
| PC-KANTOR-PELABUHAN | Kantor Pelabuhan | 10.32.224.12/25 | 10.32.224.1 | 10.32.160.2 | 10.32.224.1 (R-PELABUHAN) |
| LAPTOP-WIFI-SELATAN | WiFi Publik Selatan | 10.33.0.12/19 | 10.33.0.1 | 10.32.160.2 | 10.33.0.1 (R-SELATAN) |
| PJU-SELATAN | PJU Pintar | 10.33.32.12/25 | 10.33.32.1 | 10.32.160.2 | 10.33.32.1 (R-SELATAN) |
| LAPTOP-WIFI-TIMUR | WiFi Publik Timur | 10.33.64.11/19 | 10.33.64.1 | 10.32.160.2 | 10.33.64.1 (R-TIMUR) |
| SENSOR-BANJIR | Sensor Banjir | 10.33.96.12/22 | 10.33.96.1 | 10.32.160.2 | 10.33.96.1 (R-TIMUR) |
| LAPTOP-WIFI-BARAT | WiFi Publik Barat | 10.33.128.12/19 | 10.33.128.1 | 10.32.160.2 | 10.33.128.1 (R-BARAT) |
| PC-PUSKESMAS | Puskesmas dan Sekolah | 10.33.160.11/24 | 10.33.160.1 | 10.32.160.2 | 10.33.160.1 (R-BARAT) |

- Analisis: seluruh alamat berada di luar rentang `.1` sampai `.10`, sehingga tidak bentrok dengan gateway maupun IP statis. Hanya PC-CCTV-KOTA yang mencatat `10.32.160.2` sebagai server DHCP, membuktikan CCTV dilayani SERVER-DC melalui relay, bukan oleh router.

#### **B. Bukti CCTV dilayani SERVER-DC**

![Relay di R-DC](assets/Soal4_R-DC_helper.png)

![Pool DHCP SERVER-DC](assets/Soal4_SERVER-DC_pool.png)

![ipconfig PC-CCTV-KOTA](assets/Soal4_PC-CCTV-KOTA_ipconfig.png)

- Analisis: `Helper address is 10.32.160.2` di `Gi0/0` R-DC meneruskan permintaan DHCP. PC-CCTV-KOTA menerima `10.32.128.11` dengan `DHCP Servers: 10.32.160.2`.

#### **C. Pool dan lease di router**

![DHCP R-PUSAT](assets/Soal4_R-PUSAT_dhcp.png)

![Binding R-UTARA](assets/Soal4_R-UTARA_binding.png)
![Binding R-PELABUHAN](assets/Soal4_R-PELABUHAN_binding.png)
![Binding R-SELATAN](assets/Soal4_R-SELATAN_binding.png)
![Binding R-TIMUR](assets/Soal4_R-TIMUR_binding.png)
![Binding R-BARAT](assets/Soal4_R-BARAT_binding.png)

- Analisis: setiap `show ip dhcp binding` berisi lease dengan MAC yang sama dengan klien di tabel A.

#### **D. Konfigurasi IP klien**

![PC-PEGAWAI](assets/Soal4_PC-PEGAWAI_ipconfig.png)
![LAPTOP-WIFI-UTARA](assets/Soal4_LAPTOP-WIFI-UTARA_ipconfig.png)
![LAPTOP-WIFI-BARAT](assets/Soal4_LAPTOP-WIFI-BARAT_ipconfig.png)
![LAPTOP-WIFI-TIMUR](assets/Soal4_LAPTOP-WIFI-TIMUR_ipconfig.png)
![LAPTOP-WIFI-SELATAN](assets/Soal4_LAPTOP-WIFI-SELATAN_ipconfig.png)
![PC-PUSKESMAS](assets/Soal4_PC-PUSKESMAS_ipconfig.png)
![PC-KANTOR-PELABUHAN](assets/Soal4_PC-KANTOR-PELABUHAN_ipconfig.png)
![SENSOR-LALIN](assets/Soal4_SENSOR-LALIN_config.png)
![SENSOR-BANJIR](assets/Soal4_SENSOR-BANJIR_config.png)
![PJU-SELATAN](assets/Soal4_PJU-SELATAN_config.png)
![SENSOR-SCADA](assets/Soal4_SENSOR-SCADA_config.png)

#### **E. Server tetap statis dan DNS dari DHCP berfungsi**

![SERVER-DC statis](assets/Soal4_SERVER-DC_static.png)

![nslookup dari klien DHCP](assets/Soal4_dns-test.png)

- Analisis: `nslookup www.k4.smartcity.lab` dari LAPTOP-WIFI-SELATAN dijawab server `10.32.160.2` dengan alamat `10.33.200.2`, jadi opsi DNS dari DHCP bekerja. Layanan soal 8 tetap berjalan: login FTP akun CCTV dari PC-CCTV-KOTA berhasil dengan alamat DHCP.

---

## Static Routing (Soal 3)

---

### **1. Tujuan**

- Seluruh LAN dapat saling berkomunikasi hanya dengan static route, dengan default route untuk arah keluar.
- Default route hanya mengarah ke router di atasnya (arah keluar), tidak ke router di bawahnya.
- R-PUSAT dan R-UTARA memiliki static route ke jaringan di bawahnya: R-PUSAT ke semua LAN R-DC, R-UTARA, R-PELABUHAN, R-SELATAN, R-TIMUR, dan R-BARAT; R-UTARA ke semua LAN R-PELABUHAN.

File: [`project/K4_CIDR.pkt`](../project/K4_CIDR.pkt).

---

### **2. Rancangan**

| Keputusan | Alasan |
|---|---|
| R-PUSAT memakai satu rute ringkas (agregat) per router tujuan, bukan satu rute per LAN | Skema CIDR sudah menyediakan satu blok agregat per router (tabel agregat di Soal 2), sehingga tabel routing R-PUSAT hanya berisi 6 rute ringkas ditambah 1 default route. |
| Default route R-DC, R-BARAT, R-TIMUR, R-SELATAN, dan R-UTARA mengarah ke R-PUSAT | R-PUSAT adalah router di atas mereka dan satu-satunya yang terhubung ke R-ISP. |
| Default route R-PELABUHAN mengarah ke R-UTARA | Satu-satunya link R-PELABUHAN adalah L6 ke R-UTARA, yang berada di atasnya. |
| R-UTARA memiliki rute `10.32.192.0/18` ke R-PELABUHAN | Blok agregat R-PELABUHAN mencakup seluruh LAN Sensor dan SCADA serta Kantor Pelabuhan. |
| R-ISP tidak memiliki rute ke alamat privat internal | Syarat Soal 5: R-ISP tidak boleh mengenal blok privat internal. |
| Rute R-PUSAT ke R-PELABUHAN diarahkan ke tunnel | Lalu lintas Balai Kota dan Pelabuhan wajib lewat tunnel GRE (lihat Soal 6). |

---

### **3. Langkah Konfigurasi**

```text
! R-PUSAT
ip route 10.32.0.0 255.255.128.0 10.33.200.38    ! semua LAN R-UTARA
ip route 10.32.128.0 255.255.192.0 10.33.200.34  ! semua LAN R-DC
ip route 10.33.0.0 255.255.192.0 10.33.200.42    ! semua LAN R-SELATAN
ip route 10.33.64.0 255.255.192.0 10.33.200.46   ! semua LAN R-TIMUR
ip route 10.33.128.0 255.255.192.0 10.33.200.50  ! semua LAN R-BARAT
ip route 10.32.192.0 255.255.192.0 10.33.200.38  ! semua LAN R-PELABUHAN (kemudian diganti lewat tunnel, lihat Soal 6)
ip route 0.0.0.0 0.0.0.0 203.0.113.18            ! default ke R-ISP

! R-UTARA
ip route 10.32.192.0 255.255.192.0 10.32.66.2    ! semua LAN R-PELABUHAN
ip route 0.0.0.0 0.0.0.0 10.33.200.37            ! default ke R-PUSAT

! R-DC
ip route 0.0.0.0 0.0.0.0 10.33.200.33

! R-BARAT
ip route 0.0.0.0 0.0.0.0 10.33.200.49

! R-TIMUR
ip route 0.0.0.0 0.0.0.0 10.33.200.45

! R-SELATAN
ip route 0.0.0.0 0.0.0.0 10.33.200.41

! R-PELABUHAN
ip route 0.0.0.0 0.0.0.0 10.32.66.1
```

Setiap router diakhiri dengan `end` dan `write memory`.

---

### **4. Hasil Pengujian**

#### **A. Tabel static route**

![Static route R-PUSAT](assets/Soal3_R-PUSAT_route.png)

![Static route R-UTARA](assets/Soal3_R-UTARA_route.png)

- Analisis: R-PUSAT memiliki rute ringkas ke semua router di bawahnya dan default route ke R-ISP. Rute ke `10.32.192.0/18` (R-PELABUHAN) sudah menunjuk ke `192.168.4.2` karena rute lama diganti lewat tunnel pada Soal 6. R-UTARA memiliki rute ke `10.32.192.0/18` lewat `10.32.66.2` dan default route lewat `10.33.200.37` (R-PUSAT).

---

## Tunnel GRE (Soal 6)

---

### **1. Tujuan**

- Membangun tunnel GRE antara R-PUSAT dan R-PELABUHAN.
- Lalu lintas antara subnet Balai Kota dan subnet Pelabuhan wajib melewati tunnel.
- Menjelaskan alasan router memilih jalur tunnel dibanding jalur lain ke Pelabuhan, serta alasan GRE saja tidak mengenkripsi data.

File: [`project/K4_CIDR.pkt`](../project/K4_CIDR.pkt).

---

### **2. Rancangan**

Tunnel dibangun di atas jalur fisik yang sudah ada: R-PUSAT, link L2, R-UTARA, link L6, R-PELABUHAN. Subnet tunnel mengikuti tabel soal, yaitu `192.168.K.0/30` dengan K = 4, sehingga `192.168.4.0/30`.

| Item | R-PUSAT | R-PELABUHAN |
|---|---|---|
| Interface | Tunnel0 | Tunnel0 |
| IP tunnel | 192.168.4.1/30 | 192.168.4.2/30 |
| Tunnel source | Serial0/1/0 (10.33.200.37, link L2) | Serial0/0/0 (10.32.66.2, link L6) |
| Tunnel destination | 10.32.66.2 | 10.33.200.37 |
| Mode | GRE over IPv4 | GRE over IPv4 |

| Keputusan | Alasan |
|---|---|
| Tunnel source dan destination memakai IP serial yang sudah ada | Keduanya saling terjangkau lewat R-UTARA: R-PUSAT menjangkau `10.32.66.2` lewat rute `10.32.0.0/17`, dan R-PELABUHAN menjangkau `10.33.200.37` lewat default route ke R-UTARA. |
| Rute lama R-PUSAT ke Pelabuhan lewat R-UTARA dihapus, bukan hanya ditambah rute baru | Jika dua rute dengan prefix dan administrative distance sama (1) dibiarkan, router membagi beban dan sebagian paket tidak lewat tunnel. |
| Rute LAN Balai Kota di R-PELABUHAN dibuat dua rute spesifik (`/21` dan `/27`), tidak diringkas menjadi `10.33.192.0/20` | Blok `/20` mencakup `10.33.200.37` (tunnel source R-PUSAT). Paket GRE menuju alamat itu akan diarahkan ke tunnel itu sendiri (recursive routing) dan tunnel menjadi tidak stabil. |

---

### **3. Langkah Konfigurasi**

```text
! R-PUSAT
interface Tunnel0
 ip address 192.168.4.1 255.255.255.252
 tunnel source Serial0/1/0
 tunnel destination 10.32.66.2
 tunnel mode gre ip
 no shutdown
 exit
no ip route 10.32.192.0 255.255.192.0 10.33.200.38
ip route 10.32.192.0 255.255.192.0 192.168.4.2

! R-PELABUHAN
interface Tunnel0
 ip address 192.168.4.2 255.255.255.252
 tunnel source Serial0/0/0
 tunnel destination 10.33.200.37
 tunnel mode gre ip
 no shutdown
 exit
ip route 10.33.192.0 255.255.248.0 192.168.4.1
ip route 10.33.200.0 255.255.255.224 192.168.4.1
```

Setiap router diakhiri dengan `end` dan `write memory`.

Rute lalu lintas Balai Kota dan Pelabuhan:

| Router | Tujuan | Next hop | Keterangan |
|---|---|---|---|
| R-PUSAT | `10.32.192.0/18` | `192.168.4.2` | LAN Sensor dan SCADA serta Kantor Pelabuhan |
| R-PELABUHAN | `10.33.192.0/21` | `192.168.4.1` | LAN Pegawai Balai Kota |
| R-PELABUHAN | `10.33.200.0/27` | `192.168.4.1` | LAN Server Balai Kota |

---

### **4. Penjelasan**

#### **A. Mengapa router memilih jalur tunnel dibanding jalur lain ke Pelabuhan**

- Secara fisik hanya ada satu jalur ke R-PELABUHAN. Tunnel GRE adalah jalur logis (overlay) di atas jalur yang sama, sehingga yang dipilih router adalah rute logis tersebut.
- Di R-PUSAT, satu-satunya rute ke prefix `10.32.192.0/18` menunjuk ke next hop `192.168.4.2` (interface Tunnel0). Rute lama via R-UTARA sudah dihapus sehingga tidak ada alternatif yang bersaing.
- Di R-PELABUHAN, rute ke LAN Balai Kota (`/21` dan `/27`) lebih spesifik daripada default route (`/0`). Berdasarkan aturan _longest prefix match_, router memilih tunnel untuk lalu lintas ke Balai Kota, sedangkan tujuan lain tetap memakai default route ke R-UTARA.
- Paket yang masuk tunnel dibungkus header GRE dengan IP luar `10.33.200.37` ke `10.32.66.2`. R-UTARA hanya meneruskan paket luar tersebut dan tidak memproses isi paket asli, sehingga R-UTARA tidak muncul sebagai hop pada traceroute.

#### **B. Mengapa GRE saja tidak mengenkripsi data**

GRE hanya melakukan enkapsulasi: menambahkan header GRE (4 byte) dan header IP luar (20 byte) pada paket asli. GRE tidak menyediakan kerahasiaan, integritas, maupun autentikasi. Isi paket asli tetap berupa teks biasa di sepanjang jalur, sehingga dapat dibaca oleh perangkat yang berada di jalur tunnel, misalnya R-UTARA atau penyadap di link L2 dan L6. Enkripsi memerlukan protokol tambahan seperti IPsec.

---

### **5. Hasil Pengujian**

#### **A. Status tunnel**

![Tunnel R-PUSAT](assets/Soal6_R-PUSAT_tunnel.png)

![Tunnel R-PELABUHAN](assets/Soal6_R-PELABUHAN_tunnel.png)

- Analisis: pada kedua router, `show interfaces tunnel 0` menunjukkan `Tunnel0 is up, line protocol is up` dengan protocol GRE/IP. R-PUSAT memakai `192.168.4.1/30` (source `10.33.200.37`, destination `10.32.66.2`), dan R-PELABUHAN memakai `192.168.4.2/30` (source `10.32.66.2`, destination `10.33.200.37`).

#### **B. Ping antar-ujung tunnel dan antar-LAN lewat tunnel**

Pengujian antar-LAN memakai _extended ping_ dengan source berupa alamat LAN, bukan alamat tunnel, untuk membuktikan lalu lintas dua arah antara LAN Balai Kota dan LAN Pelabuhan.

| Dari | Tujuan | Source | Hasil |
|---|---|---|---|
| R-PUSAT | `192.168.4.2` (ujung tunnel) | otomatis | 5/5 berhasil |
| R-PUSAT | `10.32.192.1` (LAN Sensor dan SCADA) | `10.33.192.1` (LAN Pegawai) | 5/5 berhasil |
| R-PELABUHAN | `10.33.192.1` (LAN Pegawai) | `10.32.192.1` (LAN Sensor dan SCADA) | 5/5 berhasil |

![Ping dari R-PUSAT](assets/Soal6_R-PUSAT_ping.png)

![Ping dari R-PELABUHAN](assets/Soal6_R-PELABUHAN_ping.png)

- Analisis: ketiga pengujian berhasil dengan success rate 100 persen, sehingga lalu lintas antara Balai Kota dan Pelabuhan berjalan dua arah lewat tunnel.

#### **C. Traceroute**

Dari PC-PEGAWAI ke PC-KANTOR-PELABUHAN (`10.32.224.12`):

```bash
C:\>tracert 10.32.224.12
```

![Tracert PC-PEGAWAI ke PC-KANTOR-PELABUHAN](assets/Soal6_tracert.png)

- Analisis: hop pertama adalah `10.33.192.1` (R-PUSAT) dan hop kedua adalah `192.168.4.2` (ujung tunnel di R-PELABUHAN). Hop `10.33.200.38` (R-UTARA) tidak muncul, sehingga terbukti paket Balai Kota menuju Pelabuhan melewati tunnel, bukan hop-by-hop lewat R-UTARA.

---

## Dynamic Routing OSPF (Soal 7)

---

### **1. Tujuan**

- Menambah link cadangan **L7** antara R-DC dan R-UTARA.
- Mengganti seluruh static route antar-router dengan OSPF, tanpa mengirim OSPF ke LAN pengguna.
- Default route ke internet hanya dikonfigurasi di R-PUSAT, router lain menerimanya lewat OSPF.
- Konektivitas pulih otomatis jika salah satu link backbone putus.

File terpisah: [`project/K4_CIDR_OSPF.pkt`](../project/K4_CIDR_OSPF.pkt) (salinan `K4_CIDR.pkt` setelah soal 4).

---

### **2. Link L7 dan Alamat**

| Link | Network | R-DC | R-UTARA | Keterangan |
|---|---|---|---|---|
| L7 | `10.33.200.52/30` | `Serial0/0/1` `10.33.200.53` (DCE, `clock rate 64000`) | `Serial0/1/0` `10.33.200.54` | Blok /30 bebas berikutnya setelah L5 |

Garis L7 di kanvas melintas di belakang ikon R-BARAT karena posisi router sejajar. R-BARAT tidak terhubung ke L7 (lihat label di kanvas).

![Topologi dengan L7](assets/Soal7_topologi-L7.png)

| Router | Router ID | Network OSPF (area 0) | Passive |
|---|---|---|---|
| R-PUSAT | 1.1.1.1 | 10.33.192.0/21, 10.33.200.0/27, L1-L5 | Gi0/0, Gi0/1 |
| R-DC | 2.2.2.2 | 10.32.128.0/19, 10.32.160.0/24, L1, L7 | Gi0/0, Gi0/1 |
| R-UTARA | 3.3.3.3 | 10.32.0.0/18, 10.32.64.0/23, L2, L6, L7 | Gi0/0, Gi0/1 |
| R-SELATAN | 4.4.4.4 | 10.33.0.0/19, 10.33.32.0/25, L3 | Gi0/0, Gi0/1 |
| R-TIMUR | 5.5.5.5 | 10.33.64.0/19, 10.33.96.0/22, L4 | Gi0/0, Gi0/1 |
| R-BARAT | 6.6.6.6 | 10.33.128.0/19, 10.33.160.0/24, L5 | Gi0/0, Gi0/1 |
| R-PELABUHAN | 7.7.7.7 | 10.32.192.0/19, 10.32.224.0/25, L6 | Gi0/0, Gi0/1 |

---

### **3. Langkah Konfigurasi**

#### **A. Link L7**

```text
! R-DC
interface Serial0/0/1
 description L7 cadangan ke R-UTARA
 ip address 10.33.200.53 255.255.255.252
 clock rate 64000
 no shutdown
! R-UTARA
interface Serial0/1/0
 description L7 cadangan ke R-DC
 ip address 10.33.200.54 255.255.255.252
 no shutdown
```

#### **B. Menghapus static route antar-router**

```text
! R-PUSAT (default route ke R-ISP TIDAK dihapus)
no ip route 10.32.0.0 255.255.128.0 10.33.200.38
no ip route 10.32.192.0 255.255.192.0 10.33.200.38
no ip route 10.32.128.0 255.255.192.0 10.33.200.34
no ip route 10.33.0.0 255.255.192.0 10.33.200.42
no ip route 10.33.64.0 255.255.192.0 10.33.200.46
no ip route 10.33.128.0 255.255.192.0 10.33.200.50
! R-UTARA
no ip route 0.0.0.0 0.0.0.0 10.33.200.37
no ip route 10.32.192.0 255.255.192.0 10.32.66.2
! R-DC, R-SELATAN, R-TIMUR, R-BARAT, R-PELABUHAN: hapus default route ke router atas
no ip route 0.0.0.0 0.0.0.0 10.33.200.33   ! R-DC
no ip route 0.0.0.0 0.0.0.0 10.33.200.41   ! R-SELATAN
no ip route 0.0.0.0 0.0.0.0 10.33.200.45   ! R-TIMUR
no ip route 0.0.0.0 0.0.0.0 10.33.200.49   ! R-BARAT
no ip route 0.0.0.0 0.0.0.0 10.32.66.1     ! R-PELABUHAN
```

#### **C. OSPF**

```text
! R-PUSAT
router ospf 1
 router-id 1.1.1.1
 network 10.33.192.0 0.0.7.255 area 0
 network 10.33.200.0 0.0.0.31 area 0
 network 10.33.200.32 0.0.0.3 area 0
 network 10.33.200.36 0.0.0.3 area 0
 network 10.33.200.40 0.0.0.3 area 0
 network 10.33.200.44 0.0.0.3 area 0
 network 10.33.200.48 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
 default-information originate
! R-DC
router ospf 1
 router-id 2.2.2.2
 network 10.32.128.0 0.0.31.255 area 0
 network 10.32.160.0 0.0.0.255 area 0
 network 10.33.200.32 0.0.0.3 area 0
 network 10.33.200.52 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
! R-UTARA
router ospf 1
 router-id 3.3.3.3
 network 10.32.0.0 0.0.63.255 area 0
 network 10.32.64.0 0.0.1.255 area 0
 network 10.33.200.36 0.0.0.3 area 0
 network 10.32.66.0 0.0.0.3 area 0
 network 10.33.200.52 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
! R-SELATAN
router ospf 1
 router-id 4.4.4.4
 network 10.33.0.0 0.0.31.255 area 0
 network 10.33.32.0 0.0.0.127 area 0
 network 10.33.200.40 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
! R-TIMUR
router ospf 1
 router-id 5.5.5.5
 network 10.33.64.0 0.0.31.255 area 0
 network 10.33.96.0 0.0.3.255 area 0
 network 10.33.200.44 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
! R-BARAT
router ospf 1
 router-id 6.6.6.6
 network 10.33.128.0 0.0.31.255 area 0
 network 10.33.160.0 0.0.0.255 area 0
 network 10.33.200.48 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
! R-PELABUHAN
router ospf 1
 router-id 7.7.7.7
 network 10.32.192.0 0.0.31.255 area 0
 network 10.32.224.0 0.0.0.127 area 0
 network 10.32.66.0 0.0.0.3 area 0
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
```

Penjelasan:
- `passive-interface` pada port LAN: jaringan LAN tetap diiklankan, tetapi hello OSPF tidak dikirim ke pengguna. Ini menghemat trafik dan mencegah perangkat di LAN ikut membentuk adjacency OSPF.
- `default-information originate` hanya di R-PUSAT: R-PUSAT memakai satu-satunya static route `0.0.0.0/0 via 203.0.113.18` dan menyebarkannya sebagai `O*E2`.
- Link `203.0.113.16/30` ke R-ISP sengaja tidak dimasukkan ke OSPF, sehingga R-ISP tidak mengenal blok privat (syarat soal 5).
- Router ID dibuat manual (1.1.1.1 sampai 7.7.7.7) agar mudah dikenali saat membaca tabel neighbor.

---

### **4. Hasil Pengujian**

#### **A. Adjacency OSPF**

![Neighbor R-PUSAT](assets/Soal7_R-PUSAT_neighbor.png)

![Neighbor R-UTARA](assets/Soal7_R-UTARA_neighbor.png)

![Neighbor R-DC](assets/Soal7_R-DC_neighbor.png)

- Analisis: R-PUSAT punya 5 neighbor (R-DC, R-UTARA, R-SELATAN, R-TIMUR, R-BARAT), R-UTARA 3 (R-PUSAT, R-PELABUHAN, R-DC lewat L7), R-DC 2 (R-PUSAT dan R-UTARA lewat L7). Semua `FULL`. Pada R-PUSAT, satu-satunya static route yang tersisa adalah default ke R-ISP.

#### **B. Passive interface dan tabel routing**

![show ip protocols R-PUSAT](assets/Soal7_R-PUSAT_protocols.png)

![Routing R-PELABUHAN](assets/Soal7_R-PELABUHAN_route.png)

- Analisis: `Passive Interface(s): GigabitEthernet0/0, GigabitEthernet0/1`. R-PELABUHAN tidak punya static route (`show running-config | include ip route` kosong) dan menerima seluruh LAN sebagai `O` serta default route `O*E2 0.0.0.0/0 via 10.32.66.1`.

#### **C. Uji link putus (failover)**

Uji dari PC-PEGAWAI (10.33.192.12) ke LAPTOP-WIFI-UTARA (10.32.0.12).

1. Sebelum: jalur lewat L2 (`10.33.200.38`, R-UTARA).

![Sebelum link putus](assets/Soal7_failover-before.png)

2. L2 diputus dengan `shutdown` pada R-PUSAT `Serial0/1/0`. OSPF mencatat `Nbr 3.3.3.3 ... from FULL to DOWN`, lalu rute `10.32.0.0/18` berpindah ke `10.33.200.34` (R-DC) dengan metric 129.

![Rute setelah L2 putus](assets/Soal7_failover-route.png)

3. Sesudah: jalur menjadi R-PUSAT, R-DC (`10.33.200.34`), L7, R-UTARA (`10.33.200.54`), dan ping tetap 0% loss.

![Sesudah link putus](assets/Soal7_failover-after.png)

4. L2 dinyalakan kembali (`no shutdown`): neighbor 3.3.3.3 kembali `FULL` dan rute kembali lewat `10.33.200.38` dengan metric 65.

![Pemulihan L2](assets/Soal7_failover-restore.png)

- Analisis: tanpa perubahan konfigurasi manual, OSPF menghitung ulang jalur terpendek saat L2 putus dan memakai L7 sebagai jalur cadangan, lalu kembali ke L2 (metric lebih kecil) setelah link pulih.

#### **D. Catatan pemeliharaan**

`K4_CIDR_OSPF.pkt` adalah salinan file utama setelah soal 4. Jika konfigurasi soal 3, 5 (NAT), atau 6 (GRE) ditambahkan ke `K4_CIDR.pkt`, konfigurasi yang sama perlu diterapkan juga di file OSPF. Untuk GRE: jaringan tunnel `192.168.4.0/30` tidak boleh dimasukkan ke OSPF, karena endpoint tunnel yang dipelajari lewat tunnel itu sendiri membuat tunnel down (recursive routing).