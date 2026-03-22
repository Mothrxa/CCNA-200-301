# IPv4 Addressing
---

```
- /31 is used for point-to-point connection 
- A subnet is needed in each VLAN and each point-to-point WAN connection.
```


| Class | First Octet | First Octet | Values    | Purpose                          |
| ----- | ----------- | ----------- | --------- | -------------------------------- |
| A     | 0xxxxxxx    | 1–126       | Unicast   | Large networks                   |
| B     | 10xxxxxx    | 128–191     | Unicast   | Medium-sized networks            |
| C     | 110xxxxx    | 192–223     | Unicast   | Small networks                   |
| D     | 1110xxxx    | 224–239     | Multicast | Multicast                        |
| E     | 1111xxxx    | 240–255     | Reserved  | Reserved (formerly experimental) |

| Property               | Class A              | Class B                 | Class C                   |
| ---------------------- | -------------------- | ----------------------- | ------------------------- |
| First Octet Range      | 1–126                | 128–191                 | 192–223                   |
| Valid Network Numbers  | 1.0.0.0 – 126.0.0.0  | 128.0.0.0 – 191.255.0.0 | 192.0.0.0 – 223.255.255.0 |
| Total Networks         | 2⁷ − 2 = 126         | 2¹⁴ = 16,384            | 2²¹ = 2,097,152           |
| Hosts per Network      | 2²⁴ − 2 = 16,777,214 | 2¹⁶ − 2 = 65,534        | 2⁸ − 2 = 254              |
| Octets in Network Part | 1 (8 bits)           | 2 (16 bits)             | 3 (24 bits)               |
| Octets in Host Part    | 3 (24 bits)          | 2 (16 bits)             | 1 (1 bit)                 |
| Default mask           | 255.0.0.0            | 255.255.0.0             | 255.255.255.0             |
