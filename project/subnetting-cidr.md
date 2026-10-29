# Subnetting CIDR (Mid-Project)

Kelompok 4, blok `10.32.0.0/13`. Hasil UTS dipakai tanpa perubahan, ditambah kolom gateway dan alamat tambahan dari soal Mid-Project.

## Agregat per router

| Router | Blok | Prefix |
|---|---|---|
| R-UTARA | 10.32.0.0 | /17 |
| R-DC | 10.32.128.0 | /18 |
| R-PELABUHAN | 10.32.192.0 | /18 |
| R-SELATAN | 10.33.0.0 | /18 |
| R-TIMUR | 10.33.64.0 | /18 |
| R-BARAT | 10.33.128.0 | /18 |
| R-PUSAT | 10.33.192.0 | /20 |

Agregat total `10.32.0.0/15`. Sisa bebas: `10.33.208.0` sampai `10.33.255.255`.

## Tabel alokasi LAN

| Router | Subnet | Prefix | Network ID | Rentang host | Broadcast | Gateway |
|---|---|---|---|---|---|---|
| R-UTARA | WiFi Publik Utara | /18 | 10.32.0.0 | 10.32.0.1 – 10.32.63.254 | 10.32.63.255 | 10.32.0.1 |
| R-UTARA | Sensor Lalu Lintas Utara | /23 | 10.32.64.0 | 10.32.64.1 – 10.32.65.254 | 10.32.65.255 | 10.32.64.1 |
| R-DC | CCTV Kota | /19 | 10.32.128.0 | 10.32.128.1 – 10.32.159.254 | 10.32.159.255 | 10.32.128.1 |
| R-DC | Server dan Storage DC | /24 | 10.32.160.0 | 10.32.160.1 – 10.32.160.254 | 10.32.160.255 | 10.32.160.1 |
| R-PELABUHAN | Sensor dan SCADA Pelabuhan | /19 | 10.32.192.0 | 10.32.192.1 – 10.32.223.254 | 10.32.223.255 | 10.32.192.1 |
| R-PELABUHAN | Kantor Pelabuhan | /25 | 10.32.224.0 | 10.32.224.1 – 10.32.224.126 | 10.32.224.127 | 10.32.224.1 |
| R-SELATAN | WiFi Publik Selatan | /19 | 10.33.0.0 | 10.33.0.1 – 10.33.31.254 | 10.33.31.255 | 10.33.0.1 |
| R-SELATAN | PJU Pintar Selatan | /25 | 10.33.32.0 | 10.33.32.1 – 10.33.32.126 | 10.33.32.127 | 10.33.32.1 |
| R-TIMUR | WiFi Publik Timur | /19 | 10.33.64.0 | 10.33.64.1 – 10.33.95.254 | 10.33.95.255 | 10.33.64.1 |
| R-TIMUR | Sensor Banjir Timur | /22 | 10.33.96.0 | 10.33.96.1 – 10.33.99.254 | 10.33.99.255 | 10.33.96.1 |
| R-BARAT | WiFi Publik Barat | /19 | 10.33.128.0 | 10.33.128.1 – 10.33.159.254 | 10.33.159.255 | 10.33.128.1 |
| R-BARAT | Puskesmas dan Sekolah Barat | /24 | 10.33.160.0 | 10.33.160.1 – 10.33.160.254 | 10.33.160.255 | 10.33.160.1 |
| R-PUSAT | Pegawai Balai Kota | /21 | 10.33.192.0 | 10.33.192.1 – 10.33.199.254 | 10.33.199.255 | 10.33.192.1 |
| R-PUSAT | Server Balai Kota | /27 | 10.33.200.0 | 10.33.200.1 – 10.33.200.30 | 10.33.200.31 | 10.33.200.1 |

## Tabel link dan alamat tambahan

Link point-to-point tidak punya gateway. Alamat dipakai langsung di interface kedua router.

| Link | Prefix | Network ID | Rentang host | Broadcast | Sisi A | Sisi B |
|---|---|---|---|---|---|---|
| L1 R-PUSAT – R-DC | /30 | 10.33.200.32 | .33 – .34 | 10.33.200.35 | .33 | .34 |
| L2 R-PUSAT – R-UTARA | /30 | 10.33.200.36 | .37 – .38 | 10.33.200.39 | .37 | .38 |
| L3 R-PUSAT – R-SELATAN | /30 | 10.33.200.40 | .41 – .42 | 10.33.200.43 | .41 | .42 |
| L4 R-PUSAT – R-TIMUR | /30 | 10.33.200.44 | .45 – .46 | 10.33.200.47 | .45 | .46 |
| L5 R-PUSAT – R-BARAT | /30 | 10.33.200.48 | .49 – .50 | 10.33.200.51 | .49 | .50 |
| L6 R-UTARA – R-PELABUHAN | /30 | 10.32.66.0 | .1 – .2 | 10.32.66.3 | .1 | .2 |
| R-PUSAT – R-ISP | /30 | 203.0.113.16 | .17 – .18 | 203.0.113.19 | .17 (R-PUSAT) | .18 (R-ISP) |
| Tunnel GRE | /30 | 192.168.4.0 | .1 – .2 | 192.168.4.3 | .1 (R-PUSAT) | .2 (R-PELABUHAN) |
| Server internet | /24 | 8.8.8.0 | 8.8.8.1 – 8.8.8.254 | 8.8.8.255 | 8.8.8.1 (R-ISP) | 8.8.8.8 (server) |

Catatan: soal tidak menyebut sisi mana dari link R-ISP yang `.17` dan `.18`. Dipilih R-PUSAT `.17`, R-ISP `.18`.

## Tabel revisi

Belum ada kesalahan hitung UTS yang ditemukan.

| Subnet | Jawaban UTS | Perbaikan | Alasan |
|---|---|---|---|
| (kosong) | | | |
