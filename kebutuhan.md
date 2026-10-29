# Kebutuhan Blok

## Blok alamat (soal 1a)

Kelompok 4, jadi oktet ke-2 = 8 × 4 = 32. Blok kelompok: `10.32.0.0/13` (`10.32.0.0` sampai `10.39.255.255`).

Alasan memilih rentang `10.0.0.0/8`:
- Total kebutuhan 61.752 alamat. `192.168.0.0/16` (65.536 alamat) nyaris tidak muat dan tidak menyisakan ruang untuk CIDR, yang butuh `/15`.
- `172.16.0.0/12` cukup besar, tetapi aturan "oktet ke-2 = 8 × nomor kelompok" hanya bisa diterapkan di `10.0.0.0/8`.
- `10.0.0.0/8` (16.777.216 alamat) memberi tiap kelompok `/13` (524.288 alamat), cukup untuk VLSM (`/16`) maupun CIDR (`/15`).

## Tabel kebutuhan (soal 1b)

Aturan: blok terkecil 2^n dengan 2^n ≥ host + 2 (alamat network dan broadcast). Link point-to-point butuh 2 host, jadi blok 4 (`/30`).

| No | Router | Subnet | Host | Kebutuhan (host+2) | Ukuran blok | Prefix | Subnet mask | Host usable |
|---|---|---|---|---|---|---|---|---|
| 1 | R-UTARA | WiFi Publik Utara | 11.000 | 11.002 | 16.384 | /18 | 255.255.192.0 | 16.382 |
| 2 | R-SELATAN | WiFi Publik Selatan | 6.500 | 6.502 | 8.192 | /19 | 255.255.224.0 | 8.190 |
| 3 | R-DC | CCTV Kota | 6.000 | 6.002 | 8.192 | /19 | 255.255.224.0 | 8.190 |
| 4 | R-TIMUR | WiFi Publik Timur | 5.500 | 5.502 | 8.192 | /19 | 255.255.224.0 | 8.190 |
| 5 | R-PELABUHAN | Sensor dan SCADA Pelabuhan | 5.000 | 5.002 | 8.192 | /19 | 255.255.224.0 | 8.190 |
| 6 | R-BARAT | WiFi Publik Barat | 4.500 | 4.502 | 8.192 | /19 | 255.255.224.0 | 8.190 |
| 7 | R-PUSAT | Pegawai Balai Kota | 1.800 | 1.802 | 2.048 | /21 | 255.255.248.0 | 2.046 |
| 8 | R-TIMUR | Sensor Banjir Timur | 700 | 702 | 1.024 | /22 | 255.255.252.0 | 1.022 |
| 9 | R-UTARA | Sensor Lalu Lintas Utara | 450 | 452 | 512 | /23 | 255.255.254.0 | 510 |
| 10 | R-BARAT | Puskesmas dan Sekolah Barat | 220 | 222 | 256 | /24 | 255.255.255.0 | 254 |
| 11 | R-DC | Server dan Storage DC | 180 | 182 | 256 | /24 | 255.255.255.0 | 254 |
| 12 | R-PELABUHAN | Kantor Pelabuhan | 120 | 122 | 128 | /25 | 255.255.255.128 | 126 |
| 13 | R-SELATAN | PJU Pintar Selatan | 90 | 92 | 128 | /25 | 255.255.255.128 | 126 |
| 14 | R-PUSAT | Server Balai Kota | 20 | 22 | 32 | /27 | 255.255.255.224 | 30 |
| 15 | R-PUSAT – R-DC | L1 | 2 | 4 | 4 | /30 | 255.255.255.252 | 2 |
| 16 | R-PUSAT – R-UTARA | L2 | 2 | 4 | 4 | /30 | 255.255.255.252 | 2 |
| 17 | R-PUSAT – R-SELATAN | L3 | 2 | 4 | 4 | /30 | 255.255.255.252 | 2 |
| 18 | R-PUSAT – R-TIMUR | L4 | 2 | 4 | 4 | /30 | 255.255.255.252 | 2 |
| 19 | R-PUSAT – R-BARAT | L5 | 2 | 4 | 4 | /30 | 255.255.255.252 | 2 |
| 20 | R-UTARA – R-PELABUHAN | L6 | 2 | 4 | 4 | /30 | 255.255.255.252 | 2 |

Total ukuran blok:
- 14 LAN: 61.728 alamat
- 6 link: 24 alamat
- **Total: 61.752 alamat** (`/16` = 65.536 alamat, cukup)

Uplink R-ISP tidak termasuk dalam kebutuhan.
