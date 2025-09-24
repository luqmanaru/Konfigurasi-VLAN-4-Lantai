# Konfigurasi-VLAN-4-Lantai

Implementasi jaringan VLAN untuk SMK Maju Jaya yang terdiri dari 4 lantai dengan berbagai ruangan dan konfigurasi DHCP.

## Deskripsi

Tugas ini bertujuan untuk mengimplementasikan jaringan VLAN pada SMK Maju Jaya yang memiliki 4 lantai dengan berbagai ruangan. Setiap lantai memiliki kebutuhan jaringan yang berbeda dengan pembagian VLAN sebagai berikut:
- VLAN 10: Ruang ICT, Ruang Guru, dan Ruang Teknisi
- VLAN 20: Ruang Resepsionis dan Ruang Lab 2
- VLAN 30: Ruang TU dan Ruang Lab 1

## Topologi Jaringan

```
                     +--------+
                     | Router |
                     +--------+
                         |
                         |
+-----------+     +--------------+     +-----------+
| Lantai 1  |-----| Lantai 3 (SW3)|-----| Lantai 4  |
+-----------+     +--------------+     +-----------+
                         |
                         |
                     +--------------+
                     |   Lantai 2   |
                     | (SW1 & SW2)  |
                     +--------------+
                         |
                         |
                     +--------------+
                     |   Lantai 3   |
                     | (SW1 & SW2)  |
                     +--------------+
```

## Pembagian VLAN

| VLAN ID | Nama VLAN    | Ruangan           | Jumlah PC | IP Network       | DHCP |
|---------|--------------|-------------------|-----------|------------------|------|
| 10      | ICT/Guru/Tek | ICT, Guru, Teknisi| 29        | 192.168.10.0/24  | No   |
| 20      | Resep/Lab2   | Resepsionis, Lab2 | 41        | 192.168.20.0/24  | Yes  |
| 30      | TU/Lab1      | TU, Lab1          | 40        | 192.168.30.0/24  | Yes  |

## Konfigurasi Router

```
R-SMK#show running-config

Building configuration...

Current configuration : 1234 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R-SMK
!
ip dhcp excluded-address 192.168.20.1
ip dhcp excluded-address 192.168.30.1
!
ip dhcp pool VLAN20
   network 192.168.20.0 255.255.255.0
   default-router 192.168.20.1
   dns-server 8.8.8.8
!
ip dhcp pool VLAN30
   network 192.168.30.0 255.255.255.0
   default-router 192.168.30.1
   dns-server 8.8.8.8
!
interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
!
ip classless
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
end
```

## Konfigurasi Switch Lantai 1

```
SW-LANTAI1#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23
10   ICT                              active    Fa0/1, Fa0/2, Fa0/3
20   Resepsionis                      active    Fa0/4
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Konfigurasi Switch Lantai 2A (TU)

```
SW-LANTAI2A#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23
30   TU                               active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Konfigurasi Switch Lantai 2B (Guru)

```
SW-LANTAI2B#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
10   Guru                             active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Konfigurasi Switch Lantai 3A (Lab1)

```
SW-LANTAI3A#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
30   Lab1                             active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Konfigurasi Switch Lantai 3B (Lab2)

```
SW-LANTAI3B#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
20   Lab2                             active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Konfigurasi Switch Lantai 3C (Interkoneksi)

```
SW-LANTAI3C#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23
10   Interkoneksi                     active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Konfigurasi Switch Lantai 4 (Teknisi)

```
SW-LANTAI4#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23
10   Teknisi                          active    Fa0/1, Fa0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Hasil Testing

### Testing Antar VLAN yang Sama

| Dari VLAN | Ke VLAN | Dari Device       | Ke Device         | Hasil |
|-----------|---------|-------------------|-------------------|-------|
| 10        | 10      | PC di ICT         | PC di Guru        | Success |
| 10        | 10      | PC di Guru        | PC di Teknisi     | Success |
| 20        | 20      | PC di Resepsionis | PC di Lab2        | Success |
| 30        | 30      | PC di TU          | PC di Lab1        | Success |

### Testing Antar VLAN yang Berbeda

| Dari VLAN | Ke VLAN | Dari Device       | Ke Device         | Hasil |
|-----------|---------|-------------------|-------------------|-------|
| 10        | 20      | PC di ICT         | PC di Resepsionis | Failed |
| 10        | 30      | PC di Guru        | PC di TU          | Failed |
| 20        | 30      | PC di Lab2        | PC di Lab1        | Failed |

### Testing DHCP

| VLAN     | Device       | Hasil IP          | Gateway          | DNS Server       | Status |
|----------|--------------|-------------------|------------------|------------------|--------|
| VLAN 20  | PC Resepsionis| 192.168.20.2     | 192.168.20.1     | 8.8.8.8          | Success|
| VLAN 20  | PC Lab2      | 192.168.20.3     | 192.168.20.1     | 8.8.8.8          | Success|
| VLAN 30  | PC TU        | 192.168.30.2     | 192.168.30.1     | 8.8.8.8          | Success|
| VLAN 30  | PC Lab1      | 192.168.30.3     | 192.168.30.1     | 8.8.8.8          | Success|

## Kesimpulan

Dari hasil pengujian, dapat disimpulkan bahwa:
1. Konfigurasi VLAN telah berhasil diimplementasikan pada semua lantai di SMK Maju Jaya.
2. Perangkat dalam satu VLAN yang sama dapat saling berkomunikasi meskipun berada pada lantai yang berbeda.
3. Perangkat dalam VLAN yang berbeda tidak dapat saling berkomunikasi sesuai dengan konsep VLAN.
4. Konfigurasi DHCP untuk VLAN 20 dan VLAN 30 berfungsi dengan baik dan memberikan IP address secara otomatis.
5. Jaringan telah tersegmentasi dengan baik sesuai dengan fungsi masing-masing ruangan.

---
**luqmanaru**
