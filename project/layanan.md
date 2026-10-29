# Layanan Aplikasi Smart City (soal 8)

Domain internal: `k4.smartcity.lab`. Alamat server dan router memakai IP statis.

| Layanan | Server | Alamat | Konfigurasi |
|---|---|---|---|
| DNS | SERVER-DC | 10.32.160.2 | `www` ke 10.33.200.2, `ftp` ke 10.32.160.2, `mail` ke 10.33.200.2 |
| Web | SERVER-BALAIKOTA | 10.33.200.2 | Halaman "Portal Smart City Surabaya" berisi "Kelompok 4" |
| FTP | SERVER-DC | 10.32.160.2 | Akun `cctv` (password `cctv123`), izin RWDNL |
| Email | SERVER-BALAIKOTA | 10.33.200.2 | Akun `pegawai` dan `pelabuhan` |

Klien email:
- PC-PEGAWAI (Balai Kota): `pegawai@k4.smartcity.lab`
- PC-KANTOR-PELABUHAN (Pelabuhan): `pelabuhan@k4.smartcity.lab`
- SMTP dan POP3: `mail.k4.smartcity.lab`

## Hasil uji

| Uji | Dari | Hasil |
|---|---|---|
| `ping 10.32.160.2` | PC-PEGAWAI | berhasil (2 paket pertama hilang karena ARP) |
| `ping 10.33.200.2` | PC-KANTOR-PELABUHAN | berhasil |
| `nslookup www.k4.smartcity.lab` | LAPTOP-WIFI-SELATAN | 10.33.200.2 |
| `http://www.k4.smartcity.lab` | LAPTOP-WIFI-TIMUR | halaman "Portal Smart City Surabaya, Kelompok 4" tampil |
| `ftp ftp.k4.smartcity.lab` | PC-CCTV-KOTA | login `cctv` berhasil, `dir` menampilkan direktori |
| Kirim email dua arah | PC-PEGAWAI dan PC-KANTOR-PELABUHAN | terkirim, isi mailbox belum diverifikasi |

## Belum selesai

- Domain email (`k4.smartcity.lab`) pada layanan EMAIL di SERVER-BALAIKOTA perlu diisi lewat jendela server. API tidak menyediakannya.
- Penerimaan email belum terbukti. Isi mailbox tidak bisa dibaca lewat API, jadi perlu screenshot manual dari Desktop > Email.
- Upload file FTP dari PC-CCTV-KOTA (perintah `put`) belum diuji.
- Screenshot bukti per uji belum ada.
- Alamat PC dan laptop saat ini statis (sementara). Soal 4 menggantinya dengan DHCP, termasuk DHCP CCTV dari SERVER-DC.
- Static route dasar sudah dipasang agar layanan bisa diuji lintas wilayah. Soal 3 (static routing lengkap), 5 (NAT), 6 (GRE), dan 7 (OSPF) masih dikerjakan anggota lain.
