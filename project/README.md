# Mid-Project: Implementasi Jaringan Smart City

Skema yang dipilih: **CIDR** (blok `10.32.0.0/13`, agregat `10.32.0.0/15`). Lembar soal ada di [`problems/`](problems/). Deadline: Minggu, 1 November 2026, pukul 23.59 WIB.

## File

| File | Isi |
|---|---|
| [`K4_CIDR.pkt`](K4_CIDR.pkt) | Topologi Packet Tracer (R-ISP, server internet, 7 router, 14 LAN) dengan alamat statis dan layanan |
| [`perubahan.md`](perubahan.md) | Perbedaan topologi dan subnetting dibanding UTS |
| [`topologi.md`](topologi.md) | Rancangan topologi, penambahan port router, desain per LAN, penamaan |
| [`subnetting-cidr.md`](subnetting-cidr.md) | Tabel alokasi CIDR dengan gateway, link, alamat tambahan, dan tabel revisi |
| [`layanan.md`](layanan.md) | Konfigurasi dan hasil uji DNS, web, FTP, dan email (soal 8) |

## Status per soal

| Soal | Topik | Status |
|---|---|---|
| 1 | Perancangan topologi | selesai |
| 2 | Tabel subnetting | selesai, tabel revisi kosong |
| 3 | Static routing | rute dasar saja (dipasang agar layanan bisa diuji) |
| 4 | DHCP | belum, host masih IP statis sementara |
| 5 | NAT | belum |
| 6 | Tunnel GRE | belum |
| 7 | OSPF dan link L7 | belum (R-UTARA sudah punya modul serial cadangan) |
| 8 | Layanan aplikasi | selesai, ada catatan di bawah |

Catatan soal 8:
- Domain email `k4.smartcity.lab` perlu diisi manual di layanan EMAIL pada SERVER-BALAIKOTA.
- Penerimaan email dan upload FTP (`put`) belum terbukti lewat bukti uji.
- Screenshot bukti per soal belum ada.

Nama file kumpulan sesuai ketentuan soal: `K4_CIDR.pkt`.
