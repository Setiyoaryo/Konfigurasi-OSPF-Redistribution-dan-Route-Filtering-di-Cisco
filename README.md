# Konfigurasi-OSPF-Redistribution-dan-Route-Filtering-di-Cisco

### Topology

![Topologi](image/Topologi.png)

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
