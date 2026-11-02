# UTS: Subnetting VLSM dan CIDR

Soal: rancang pengalamatan IP jaringan Smart City Surabaya dengan VLSM dan CIDR (7 router, 14 LAN, link L1–L6). Blok kelompok 4: `10.32.0.0/13`. Lembar soal ada di [`problems/`](problems/).

## Dokumen

| File | Isi |
|---|---|
| [`kebutuhan.md`](kebutuhan.md) | Blok alamat, tabel kebutuhan tiap subnet (host, blok, prefix, mask) |
| [`topologi.md`](topologi.md) | Skema topologi, link antar-router, LAN per router |
| [`subnetting-vlsm.md`](subnetting-vlsm.md) | Alokasi VLSM dalam `10.32.0.0/16` (pemakaian 94,23%) |
| [`subnetting-cidr.md`](subnetting-cidr.md) | Agregat per router, pohon penggabungan, alokasi CIDR dalam `10.32.0.0/15` (pemakaian 47,11%), perbandingan dengan VLSM |

## File Packet Tracer

| File | Skema |
|---|---|
| [`ets-vlsm.pkt`](ets-vlsm.pkt) | VLSM |
| [`ets-cidr.pkt`](ets-cidr.pkt) | CIDR |

Keduanya memakai topologi yang sama: 7 router 2911 (link serial `HWIC-2T` antar-router), 14 switch 2960, dan satu PC atau server perwakilan tiap LAN.

Isi konfigurasi:
- IP interface router, `clock rate` sisi DCE, dan hostname.
- Static route dasar (default route ke atas, rute ke bawah di R-PUSAT dan R-UTARA).
- IP statis host `.2` (di LAN `10.32.240.128/25` pada VLSM host `.130`), gateway = host pertama.
- Ping lintas wilayah sudah diuji.

Nama perangkat di file ini masih format UTS (`SW-Pegawai`, `PC-Pegawai`). Format huruf besar dipakai di folder [`project/`](../project/).

---

## Soal 2: Perancangan Subnetting (VLSM & CIDR)

---

### 1. Kebutuhan Blok (Soal 1a & 1b)

#### 1a. Blok Alamat
* **Kelompok:** 4
* **Oktet ke-2:** $8 \times 4 = 32 \implies \mathbf{10.32.0.0/13}$ (Rentang IP: `10.32.0.0` sampai `10.39.255.255`)
* **Alasan Pemilihan Rentang `10.0.0.0/8`:**
  * Total kebutuhan jaringan adalah **61.752 alamat IP**.
  * Rentang `192.168.0.0/16` (65.536 alamat) nyaris tidak muat dan tidak menyisakan ruang yang cukup untuk skema agregasi CIDR (membutuhkan blok `/15`).
  * Rentang `172.16.0.0/12` cukup besar, tetapi aturan *"oktet ke-2 = 8 × nomor kelompok"* hanya dapat diterapkan secara konsisten pada kelas `10.0.0.0/8`.
  * Rentang `10.0.0.0/8` (16.777.216 alamat) memberikan alokasi `/13` (524.288 alamat) untuk kelompok, sangat mencukupi untuk subnetting VLSM (`/16`) maupun CIDR (`/15`).

#### 1b. Tabel Kebutuhan Host dan Ukuran Blok
> **Aturan:** Ukuran blok terkecil kelipatan $2^n$ dengan $2^n \ge \text{host} + 2$ (alamat Network & Broadcast). Link *point-to-point* membutuhkan 2 host, sehingga dialokasikan blok 4 IP (`/30`).

| No | Router | Subnet / LAN | Host | Kebutuhan (Host + 2) | Ukuran Blok | Prefix | Subnet Mask | Host Usable |
| :-: | :--- | :--- | :-: | :-: | :-: | :-: | :--- | :-: |
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

* **Total Ukuran Blok:**
  * 14 LAN: 61.728 alamat
  * 6 Link: 24 alamat
  * **Total: 61.752 alamat** (Prefix minimum `/16` = 65.536 alamat, cukup).
  * *Catatan: Uplink R-ISP tidak termasuk dalam perhitungan kebutuhan internal.*

---

### 2. Subnetting VLSM (Soal 3)

* **IP Induk Base:** `10.32.0.0/16` (bagian awal dari `10.32.0.0/13`)
* **Metode:** Mengurutkan subnet dari jumlah host terbesar ke terkecil, lalu dialokasikan secara berurutan tanpa celah.

| No | Subnet / LAN | Ukuran Blok | Prefix | Network ID | Subnet Mask | Rentang Host Valid | Broadcast | Gateway (IP Router) |
| :-: | :--- | :-: | :-: | :
