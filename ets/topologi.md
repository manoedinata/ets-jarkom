# Topologi Smart City Surabaya

Kelompok 4, blok privat `10.32.0.0/13` (oktet ke-2 = 8 × 4 = 32).

## Skema

```
                          [R-ISP]            (uplink, tidak dialokasikan)
                             |
   [Pegawai Balai Kota]--+   |   +--[Server Balai Kota]
                         |   |   |
                       +-----------+
                       |  R-PUSAT  |
                       +-----------+
        L1 /     L5 |      L2 |       L4 \      L3 \
          /         |         |           \          \
    +-------+  +---------+ +---------+ +---------+ +-----------+
    | R-DC  |  | R-BARAT | | R-UTARA | | R-TIMUR | | R-SELATAN |
    +-------+  +---------+ +---------+ +---------+ +-----------+
     |      |    |      |    |      |     |      |     |       |
   CCTV  Server WiFi  Puskesmas WiFi Sensor WiFi Sensor WiFi    PJU
   Kota  & Stor Publik & Sekolah Publik Lalu Publik Banjir Publik Pintar
         age DC Barat   Barat   Utara Lintas Timur  Timur  Selatan Selatan
                                        Utara
                                  |
                                  | L6
                                  |
                           +-------------+
                           | R-PELABUHAN |
                           +-------------+
                            |           |
                     Sensor dan      Kantor
                     SCADA Pelabuhan Pelabuhan
```

## Link antar-router

| Link | Ujung A | Ujung B |
|---|---|---|
| L1 | R-PUSAT | R-DC |
| L2 | R-PUSAT | R-UTARA |
| L3 | R-PUSAT | R-SELATAN |
| L4 | R-PUSAT | R-TIMUR |
| L5 | R-PUSAT | R-BARAT |
| L6 | R-UTARA | R-PELABUHAN |

## LAN per router

| Router | LAN |
|---|---|
| R-PUSAT | Pegawai Balai Kota, Server Balai Kota |
| R-DC | CCTV Kota, Server dan Storage DC |
| R-UTARA | WiFi Publik Utara, Sensor Lalu Lintas Utara |
| R-PELABUHAN | Sensor dan SCADA Pelabuhan, Kantor Pelabuhan |
| R-SELATAN | WiFi Publik Selatan, PJU Pintar Selatan |
| R-TIMUR | WiFi Publik Timur, Sensor Banjir Timur |
| R-BARAT | WiFi Publik Barat, Puskesmas dan Sekolah Barat |

Total: 7 router, 14 LAN, 6 link point-to-point.
