# Rancangan Topologi (Mid-Project)

File Packet Tracer: `K4_CIDR.pkt`. Skema: CIDR.

## Backbone

| Link | Ujung A | Port A | Ujung B | Port B |
|---|---|---|---|---|
| L1 | R-PUSAT | Serial0/0/0 | R-DC | Serial0/0/0 |
| L2 | R-PUSAT | Serial0/1/0 | R-UTARA | Serial0/0/0 |
| L3 | R-PUSAT | Serial0/2/0 | R-SELATAN | Serial0/0/0 |
| L4 | R-PUSAT | Serial0/1/1 | R-TIMUR | Serial0/0/0 |
| L5 | R-PUSAT | Serial0/0/1 | R-BARAT | Serial0/0/0 |
| L6 | R-UTARA | Serial0/0/1 | R-PELABUHAN | Serial0/0/0 |
| ISP | R-PUSAT | GigabitEthernet0/2 | R-ISP | GigabitEthernet0/0 |
| Internet | R-ISP | GigabitEthernet0/1 | SERVER-INTERNET | FastEthernet0 |

Semua router memakai Cisco 2911.

## Alasan penambahan port router

- Router 2911 hanya punya 3 port Gigabit. R-PUSAT butuh 8 koneksi (2 LAN, 5 link router, 1 ISP), jadi link antar-router memakai serial.
- Serial memakai modul `HWIC-2T` (2 port per modul). R-PUSAT memakai 3 modul, router lain 1 modul.
- R-UTARA memakai 2 modul. Modul kedua (`Serial0/1/0`, `Serial0/1/1`) disiapkan untuk link cadangan L7 ke R-DC (soal 7). R-DC masih punya `Serial0/0/1` kosong.
- Sisi DCE memakai `clock rate 64000`: R-PUSAT di semua link L1–L5, R-UTARA di L6.
- Port Gigabit dipakai untuk LAN (`Gi0/0` dan `Gi0/1`) dan R-ISP.

## Desain per LAN

Tiap LAN: router, satu switch 2960-24TT, satu perangkat perwakilan. Kabel straight dari router ke switch dan dari switch ke host. Jumlah host penuh tetap dihitung di subnet.

| Router | LAN | Switch | Perwakilan | Alasan |
|---|---|---|---|---|
| R-PUSAT | Pegawai Balai Kota | SW-PEGAWAI | PC-PEGAWAI (PC) | Pegawai memakai komputer kantor. |
| R-PUSAT | Server Balai Kota | SW-SERVER-BALAIKOTA | SERVER-BALAIKOTA (Server) | Menjalankan web dan email. |
| R-DC | CCTV Kota | SW-CCTV-KOTA | PC-CCTV-KOTA (PC) | Pengunggah rekaman ke FTP. |
| R-DC | Server dan Storage DC | SW-SERVER-DC | SERVER-DC (Server) | Menjalankan DNS, DHCP, dan FTP. |
| R-BARAT | WiFi Publik Barat | SW-WIFI-BARAT | LAPTOP-WIFI-BARAT (Laptop) | Pengguna publik, perangkat mobile. |
| R-BARAT | Puskesmas dan Sekolah | SW-PUSKESMAS | PC-PUSKESMAS (PC) | Komputer kantor. |
| R-UTARA | WiFi Publik Utara | SW-WIFI-UTARA | LAPTOP-WIFI-UTARA (Laptop) | Pengguna publik. |
| R-UTARA | Sensor Lalu Lintas | SW-SENSOR-LALIN | SENSOR-LALIN (Wired End Device) | Perangkat sensor. |
| R-TIMUR | WiFi Publik Timur | SW-WIFI-TIMUR | LAPTOP-WIFI-TIMUR (Laptop) | Pengguna publik. |
| R-TIMUR | Sensor Banjir | SW-SENSOR-BANJIR | SENSOR-BANJIR (Wired End Device) | Perangkat sensor. |
| R-SELATAN | WiFi Publik Selatan | SW-WIFI-SELATAN | LAPTOP-WIFI-SELATAN (Laptop) | Pengguna publik. |
| R-SELATAN | PJU Pintar | SW-PJU | PJU-SELATAN (Wired End Device) | Perangkat lampu jalan. |
| R-PELABUHAN | Sensor dan SCADA | SW-SCADA | SENSOR-SCADA (Wired End Device) | Perangkat sensor industri. |
| R-PELABUHAN | Kantor Pelabuhan | SW-KANTOR-PELABUHAN | PC-KANTOR-PELABUHAN (PC) | Komputer kantor. |

Pertimbangan media dan jenis perangkat:
- Semua perwakilan memakai kabel, bukan wireless. Packet Tracer tidak punya API SSID. Dengan beberapa AP berSSID bawaan, laptop bisa terhubung ke AP LAN lain dan menerima alamat DHCP yang salah, sehingga mengacaukan alamat dan pengujian DHCP (soal 4).
- Sensor memakai `WiredEndDevice-PT`. Perangkat `SBC-PT` ternyata hanya punya port wireless sehingga tidak bisa dikabel.
- WiFi Publik memakai laptop sebagai perwakilan perangkat mobile pengguna.

## Perangkat tambahan Mid-Project

- R-ISP (2911), `Gi0/0` ke R-PUSAT (`203.0.113.18/30`), `Gi0/1` ke server (`8.8.8.1/24`).
- SERVER-INTERNET `8.8.8.8/24`, gateway `8.8.8.1`.
- Tunnel GRE R-PUSAT ke R-PELABUHAN dan link L7 R-DC ke R-UTARA: belum dibangun (soal 6 dan 7).

## Penamaan dan label

Perangkat memakai format huruf besar dengan tanda hubung, misalnya `R-PUSAT`, `SW-CCTV-KOTA`, `PC-CCTV-KOTA`. Setiap LAN punya catatan di kanvas berisi nama subnet, network ID/prefix, dan jumlah host. Hostname IOS router juga diganti sesuai nama perangkat.
