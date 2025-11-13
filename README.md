# Subnetting Jaringan Komputer

Oleh : 


Rizqi Akbar Sukirman Putra
5027241044

## Penetuan Prefix
NRP: 5027241044
Prefix:
10.(NRP mod 256).0.0 → 10.84.0.0/16

## Topologi
<img width="1044" height="587" alt="Screenshot 2025-11-13 160139" src="https://github.com/user-attachments/assets/fa552283-564e-4db2-a1de-83ea39af043e" />

## Perhitungan VLSM
VLSM digunakan agar setiap jaringan (unit kerja) mendapatkan ukuran subnet pas sesuai kebutuhan host, sehingga tidak ada pemborosan IP.


Langkah-langkah VLSM:
### Tentukan kebutuhan host tiap subnet
Misalnya:

- Sekretariat – 380 host
- Kurikulum – 220 host
- Guru & Tendik – 95 host
- Sarpras – 45 host
- Pengawas (Cabang) – 18 host
- Server & Admin – 6 host
- Link WAN antar-router – 2 host

### Urutkan dari host terbesar → terkecil
Subnet	Host	Dibulatkan (2^n)	Prefix

- Sekretariat	380	512	/23
- Kurikulum	220	256	/24
- Guru & Tendik	95	128	/25
- Sarpras	45	64	/26
- Pengawas	18	32	/27
- Server & Admin	6	6	/29
- Link WAN	2	4	/30

### Alokasikan subnet berurutan dari base network

Base network = 10.84.0.0/16

|Subnet	|Network	|Prefix	|Host Range	|Broadcast|
|---|---|---|---|---|
|Sekretariat	|10.84.0.0	|/23	|10.84.0.1 – 10.84.1.254	|10.84.1.255|
|Kurikulum	|10.84.2.0	|/24	|10.84.2.1 – 10.84.2.254	|10.84.2.255|
|Guru & Tendik	|10.84.3.0	|/25	|10.84.3.1 – 10.84.3.126	|10.84.3.127|
|Sarpras	|10.84.3.128	|/26	|10.84.3.129 – 10.84.3.190	|10.84.3.191|
|Pengawas	|10.84.3.192	|/27	|10.84.3.193 – 10.84.3.222	|10.84.3.223|
|Server & Admin	|10.84.3.224	|/29	|10.84.3.225 – 10.84.3.238	|10.84.3.239|
|Link WAN (HQ–Cabang)	|10.84.3.232	|/30	|10.84.3.233 – 10.84.3.234	|10.84.3.235|

Catatan: Link antar-router memakai /30 karena point-to-point hanya membutuhkan 2 host.

## Cara Menentukan Gateway

Gateway = alamat host pertama dari subnet tersebut:
gateway = network + 1

Contoh:

- Sekretariat → 10.84.0.1
- Kurikulum → 10.84.2.1
- Guru & Tendik → 10.84.3.1
- Sarpras → 10.84.3.129
- Server Admin → 10.84.3.225
- Pengawas (Cabang) → 10.84.3.193
- WAN HQ → 10.84.3.233
- WAN Cabang → 10.84.3.234

## Perhitungan CIDR (Supernetting)

CIDR digunakan untuk meringkas banyak subnet menjadi satu rute besar, sehingga routing lebih sederhana.

(1) Cari network address paling kecil

→ 10.84.0.0

(2) Cari broadcast address paling besar

→ 10.84.3.255

(3) Tentukan rentang total

10.84.0.0 → 10.84.3.255
Ini mencakup 4 blok /24 (1024 alamat).

(4) Konversi 1024 alamat ke prefix

1024 = 2^10 → Prefix = 32 − 10 = /22


Hasil CIDR Summary:
```10.84.0.0/22```

CIDR ini dipakai untuk routing dari Cabang ke HQ


untuk menyederhanakan banyak rute /23,/24,/25,/26,/27,/28 menjadi satu jalur ringkas
