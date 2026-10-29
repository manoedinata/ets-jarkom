# Subnetting CIDR (soal 4 dan 5)

Kelompok 4, blok `10.32.0.0/13`. Setiap router mendapat satu blok yang berisi seluruh subnet miliknya, lalu blok-blok itu digabung (supernetting) menjadi satu agregat.

Link L1–L5 dimiliki R-PUSAT, link L6 dimiliki R-UTARA.

## 4a. Kebutuhan dan prefix per router

| Router | Anggota | Jumlah alamat | Blok | Prefix |
|---|---|---|---|---|
| R-UTARA | WiFi Publik Utara 16.384 + Sensor Lalu Lintas 512 + L6 4 | 16.900 | 32.768 | /17 |
| R-DC | CCTV 8.192 + Server dan Storage DC 256 | 8.448 | 16.384 | /18 |
| R-PELABUHAN | SCADA 8.192 + Kantor Pelabuhan 128 | 8.320 | 16.384 | /18 |
| R-SELATAN | WiFi Publik Selatan 8.192 + PJU 128 | 8.320 | 16.384 | /18 |
| R-TIMUR | WiFi Publik Timur 8.192 + Sensor Banjir 1.024 | 9.216 | 16.384 | /18 |
| R-BARAT | WiFi Publik Barat 8.192 + Puskesmas dan Sekolah 256 | 8.448 | 16.384 | /18 |
| R-PUSAT | Pegawai 2.048 + Server Balai Kota 32 + L1–L5 20 | 2.100 | 4.096 | /20 |

Total blok:

- /17: 32.768 (R-UTARA)
- /18: 5 × 16.384 = 81.920 (R-DC, R-PELABUHAN, R-SELATAN, R-TIMUR, R-BARAT)
- /20: 4.096 (R-PUSAT)
- **Total 118.784 alamat**, sehingga butuh agregat **/15** (131.072).

## Pohon penggabungan

```
10.32.0.0/15                                   131.072
├── 10.32.0.0/16                                65.536
│   ├── 10.32.0.0/17    R-UTARA                 32.768
│   └── 10.32.128.0/17                          32.768
│       ├── 10.32.128.0/18  R-DC                16.384
│       └── 10.32.192.0/18  R-PELABUHAN         16.384
└── 10.33.0.0/16                                65.536
    ├── 10.33.0.0/17                            32.768
    │   ├── 10.33.0.0/18    R-SELATAN           16.384
    │   └── 10.33.64.0/18   R-TIMUR             16.384
    └── 10.33.128.0/17                          32.768
        ├── 10.33.128.0/18  R-BARAT             16.384
        └── 10.33.192.0/18                      16.384
            ├── 10.33.192.0/20  R-PUSAT          4.096
            └── 10.33.208.0 – 10.33.255.255 bebas 12.288
```

## 4b. Tabel alokasi

| Router | Subnet | Prefix | Network ID | Rentang host | Broadcast |
|---|---|---|---|---|---|
| R-UTARA | WiFi Publik Utara | /18 | 10.32.0.0 | 10.32.0.1 – 10.32.63.254 | 10.32.63.255 |
| R-UTARA | Sensor Lalu Lintas Utara | /23 | 10.32.64.0 | 10.32.64.1 – 10.32.65.254 | 10.32.65.255 |
| R-UTARA | L6 (R-UTARA – R-PELABUHAN) | /30 | 10.32.66.0 | 10.32.66.1 – 10.32.66.2 | 10.32.66.3 |
| R-DC | CCTV Kota | /19 | 10.32.128.0 | 10.32.128.1 – 10.32.159.254 | 10.32.159.255 |
| R-DC | Server dan Storage DC | /24 | 10.32.160.0 | 10.32.160.1 – 10.32.160.254 | 10.32.160.255 |
| R-PELABUHAN | Sensor dan SCADA Pelabuhan | /19 | 10.32.192.0 | 10.32.192.1 – 10.32.223.254 | 10.32.223.255 |
| R-PELABUHAN | Kantor Pelabuhan | /25 | 10.32.224.0 | 10.32.224.1 – 10.32.224.126 | 10.32.224.127 |
| R-SELATAN | WiFi Publik Selatan | /19 | 10.33.0.0 | 10.33.0.1 – 10.33.31.254 | 10.33.31.255 |
| R-SELATAN | PJU Pintar Selatan | /25 | 10.33.32.0 | 10.33.32.1 – 10.33.32.126 | 10.33.32.127 |
| R-TIMUR | WiFi Publik Timur | /19 | 10.33.64.0 | 10.33.64.1 – 10.33.95.254 | 10.33.95.255 |
| R-TIMUR | Sensor Banjir Timur | /22 | 10.33.96.0 | 10.33.96.1 – 10.33.99.254 | 10.33.99.255 |
| R-BARAT | WiFi Publik Barat | /19 | 10.33.128.0 | 10.33.128.1 – 10.33.159.254 | 10.33.159.255 |
| R-BARAT | Puskesmas dan Sekolah Barat | /24 | 10.33.160.0 | 10.33.160.1 – 10.33.160.254 | 10.33.160.255 |
| R-PUSAT | Pegawai Balai Kota | /21 | 10.33.192.0 | 10.33.192.1 – 10.33.199.254 | 10.33.199.255 |
| R-PUSAT | Server Balai Kota | /27 | 10.33.200.0 | 10.33.200.1 – 10.33.200.30 | 10.33.200.31 |
| R-PUSAT | L1 (R-PUSAT – R-DC) | /30 | 10.33.200.32 | 10.33.200.33 – 10.33.200.34 | 10.33.200.35 |
| R-PUSAT | L2 (R-PUSAT – R-UTARA) | /30 | 10.33.200.36 | 10.33.200.37 – 10.33.200.38 | 10.33.200.39 |
| R-PUSAT | L3 (R-PUSAT – R-SELATAN) | /30 | 10.33.200.40 | 10.33.200.41 – 10.33.200.42 | 10.33.200.43 |
| R-PUSAT | L4 (R-PUSAT – R-TIMUR) | /30 | 10.33.200.44 | 10.33.200.45 – 10.33.200.46 | 10.33.200.47 |
| R-PUSAT | L5 (R-PUSAT – R-BARAT) | /30 | 10.33.200.48 | 10.33.200.49 – 10.33.200.50 | 10.33.200.51 |

## 5. Perbandingan VLSM dan CIDR

Jumlah alamat yang dibutuhkan subnet: 61.752.

| Metode | Prefix terkecil | Alamat | Pemakaian | Alamat terbuang |
|---|---|---|---|---|
| VLSM | /16 | 65.536 | 94,23% | 3.784 |
| CIDR | /15 | 131.072 | 47,11% | 69.320 |

**5a.** VLSM lebih efisien, selisih pemakaian **47,12 poin persentase** (94,23% − 47,11%).

**5b.** Alasan:
- VLSM menempatkan blok dari terbesar ke terkecil secara rapat, tanpa celah, sehingga sisa hanya di ujung blok.
- CIDR membulatkan kebutuhan tiap router ke pangkat dua (misalnya R-UTARA 16.900 menjadi 32.768, dan R-DC 8.448 menjadi 16.384), lalu agregat gabungannya membulat lagi ke `/15`.
- Imbalannya: CIDR menghasilkan tabel routing yang jauh lebih ringkas, karena tiap router cukup diumumkan sebagai satu prefix.
