# Perubahan UTS ke Mid-Project

Kelompok 4, skema CIDR, blok `10.32.0.0/13`. Dokumen UTS ada di folder `ets/`.

## Topologi

| Perubahan | UTS | Mid-Project |
|---|---|---|
| R-ISP | tidak ada | ada, terhubung ke R-PUSAT |
| Server internet | tidak ada | `8.8.8.8` di belakang R-ISP |
| Tunnel GRE | tidak ada | R-PUSAT ke R-PELABUHAN (soal 6, dikerjakan anggota lain) |
| Link L7 | tidak ada | R-DC ke R-UTARA, cadangan (soal 7, dikerjakan anggota lain) |
| Backbone L1–L6 | sama | sama (L1 R-DC, L2 R-UTARA, L3 R-SELATAN, L4 R-TIMUR, L5 R-BARAT, L6 R-UTARA ke R-PELABUHAN) |
| Perangkat | tidak diminta | ada nama perangkat, label LAN, dan perwakilan per LAN |

## Subnetting

Seluruh subnet LAN dan link L1–L6 memakai hasil UTS tanpa perubahan. Belum ada kesalahan hitung UTS yang ditemukan, jadi tabel revisi kosong (lihat `subnetting-cidr.md`).

Tambahan alamat baru (K = 4):

| Keperluan | Alamat |
|---|---|
| Link R-PUSAT ke R-ISP | `203.0.113.16/30` |
| Server internet | `8.8.8.0/24` |
| Tunnel GRE | `192.168.4.0/30` |
| Link L7 | belum dialokasikan (soal 7), ambil dari sisa blok `10.33.200.52` ke atas |

Tabel alokasi sekarang juga memuat kolom gateway (host pertama tiap LAN).
