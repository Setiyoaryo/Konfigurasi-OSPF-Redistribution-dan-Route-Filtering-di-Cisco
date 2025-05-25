# Konfigurasi-OSPF-Redistribution-dan-Route-Filtering-di-Cisco

### Overview :
Di repository ini, saya mendokumentasikan konfigurasi OSPF di router Cisco, lengkap dengan fitur redistribution dan route filtering. Tujuannya simpel: jadi panduan praktis dan mudah dipahami untuk siapa pun yang ingin belajar atau mengimplementasikan OSPF yang lebih kompleks di jaringan Cisco.

### TOPOLOGY

![TOPOLOGI](Image/Topologi.png)

### IP ADDRESS TABLE

|Hostname           |Interface          |IP Adrress        |Description      |
|-------------------|-------------------|------------------|-----------------|
|R-1                | G0/0              | DHCP Client      | WAN             |
|                   | G0/1              | 192.0.1.1/30     | R1 <--> R2      |  
|                   | Loopback 0        | 1.1.1.1/32       | Loopback        |
|R-2                | G0/0              | 192.0.1.2/30     | R2 <--> R1      |
|                   | G0/1              | 192.0.2.1/30     | R2 <--> R3      |
|                   | Loopback 0        | 2.2.2.2/32       | Loopback        |
|R-3                | G0/0              | 192.0.2.2/30     | R3 <--> R2      |
|                   | G0/1              | 192.0.3.1/30     | R3 <--> R4      |
|                   | Loopback 0        | 3.3.3.3/32       | Loopback        |
|R-4                | G0/0              | 192.0.3.2/30     | R4 <--> R3      |
|                   | G0/1              | 192.0.4.1/30     | R4 <--> R5      |
|                   | Loopback 0        | 4.4.4.4/32       | Loopback        |
|                   | Loopback 1        | 10.0.0.1/32      | Loopback        |
|                   | Loopback 2        | 10.0.0.2/32      | Loopback        |
|                   | Loopback 3        | 10.0.0.3/32      | Loopback        |
|                   | Loopback 4        | 10.0.0.4/32      | Loopback        |
|R-5                | G0/0              | 192.0.4.2/30     | R5 <--> R4      |
|                   | G0/1              | 172.16.1.1/24    | LAN             |
|                   | Loopback 0        | 5.5.5.5/32       | Loopback        |


### PER-DEVICE IP CONFIGURATION 
Sebelum mulai konfigurasi OSPF, langkah pertama yang saya lakukan adalah mengatur IP address di masing-masing perangkat. Ini penting supaya semua device bisa saling terhubung dengan benar.
#### Router 1 (R-1)
```Bash
R-1(config) interface gigabitEthernet 0/0
R-1(config-if) ip address dhcp
R-1(config-if) description WAN
R-1(config-if) duplex full
R-1(config-if) bandwidth 1024000
R-1(config-if) ip nat outside
R-1(config-if) no shutdown
R-1(config-if) exit
R-1(config) interface gigabitEthernet 0/1
R-1(config-if) ip address 192.0.1.1 255.255.255.252
R-1(config-if) description R1<-->R2
R-1(config-if) duplex full
R-1(config-if) bandwidth 10240000
R-1(config-if) ip nat inside
R-1(config-if) no shutdown
R-1(config-if) exit
R-1(config) interface lo0
R-1(config-if) ip address 1.1.1.1 255.255.255.255
R-1(config-if) exit
R-1(config)
```
#### Router 2 (R-2)
```Bash
R-2(config) interface gigabitEthernet 0/0
R-2(config-if) ip address 192.0.1.2 255.255.255.252
R-2(config-if) description R2<-->R1
R-2(config-if) duplex full
R-2(config-if) bandwidth 10240000
R-2(config-if) no shutdown
R-2(config-if) exit
R-2(config) interface gigabitEthernet 0/1
R-2(config-if) ip address 192.0.2.1 255.255.255.252
R-2(config-if) description R2<-->R3
R-2(config-if) duplex full
R-2(config-if) bandwidth 1024000
R-2(config-if) no shutdown
R-2(config-if) exit
R-2(config) interface lo0
R-2(config-if) ip address 2.2.2.2 255.255.255.255
R-2(config-if) exit
R-2(config)
```
#### Router 3 (R-3)
```Bash
R-3(config) interface gigabitEthernet 0/0
R-3(config-if) ip address 192.0.2.2 255.255.255.252
R-3(config-if) description R3<-->R2
R-3(config-if) duplex full
R-3(config-if) bandwidth 10240000
R-3(config-if) no shutdown
R-3(config-if) exit
R-3(config) interface gigabitEthernet 0/1
R-3(config-if) ip address 192.0.3.1 255.255.255.252
R-3(config-if) description R3<-->R4
R-3(config-if) duplex full
R-3(config-if) bandwidth 1024000
R-3(config-if) no shutdown
R-3(config-if) exit
R-3(config) interface lo0
R-3(config-if) ip address 3.3.3.3 255.255.255.255
R-3(config-if) exit
R-3(config)
```

#### Router 4 (R-4)
```Bash
R-4(config) interface gigabitEthernet 0/0
R-4(config-if) ip address 192.0.3.2 255.255.255.252
R-4(config-if) description R4<-->R3
R-4(config-if) duplex full
R-4(config-if) bandwidth 10240000
R-4(config-if) no shutdown
R-4(config-if) exit
R-4(config) interface gigabitEthernet 0/1
R-4(config-if) ip address 192.0.4.1 255.255.255.252
R-4(config-if) description R4<-->R5
R-4(config-if) duplex full
R-4(config-if) bandwidth 1024000
R-4(config-if) no shutdown
R-4(config-if) exit
R-4(config) interface lo0
R-4(config-if) ip address 4.4.4.4 255.255.255.255
R-4(config-if) interface lo1
R-4(config-if) 
R-4(config-if) 
R-4(config-if) exit
R-4(config)
```
#### Router 5 (R-5)
```Bash
R-5(config) interface gigabitEthernet 0/0
R-5(config-if) ip address 192.0.4.2 255.255.255.252
R-5(config-if) description R5<-->R4
R-5(config-if) duplex full
R-5(config-if) bandwidth 10240000
R-5(config-if) no shutdown
R-5(config-if) exit
R-5(config) interface gigabitEthernet 0/1
R-5(config-if) ip address 172.16.1.1 255.255.255.0
R-5(config-if) description LAN
R-5(config-if) duplex full
R-5(config-if) bandwidth 1024000
R-5(config-if) no shutdown
R-5(config-if) exit
R-5(config) interface lo0
R-5(config-if) ip address 3.3.3.3 255.255.255.255
R-5(config-if) exit
R-5(config)































