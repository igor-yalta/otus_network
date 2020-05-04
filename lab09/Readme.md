## Различные виды фильтрации в протоколе OSPF ##

Домашнее задание

OSPF

Цель: Настроить OSPF офисе Москва Разделить сеть на зоны Настроить фильтрацию между зонами

1. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone
2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по-умолчанию
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101
5. План работы и изменения зафиксированы в документации 

Схема AREA OSPF филиала Москва

![](https://github.com/svasornd/otus_network/blob/master/lab09/lab_09_area_ospf.png)

| OSPF | Area | Type Area | Router ID  |
|------|------|-----------|------------|
| R12  | 10   | Standard  | 10.10.0.12 |
| R13  | 10   | Standard  | 10.10.0.13 |
| R14  | 0    | Backbone  | 10.10.0.14 |
| R14  | 10   | Standard  | 10.10.0.14 |
| R14  | 101  | TotalStub | 10.10.0.14 |
| R15  | 0    | Backbone  | 10.10.0.15 |
| R15  | 10   | Standard  | 10.10.0.15 |
| R15  | 101  | TotalStub | 10.10.0.15 |
| R19  | 101  | Stub      | 10.10.0.19 |
| R20  | 102  | Standard  | 10.10.0.19 |


Схама и адресация филиала Москва IPv4

![](https://github.com/svasornd/otus_network/blob/master/lab09/lab_09_Moscow_IPv4.png)

| Филиал       | Москва     |               |                 |            |
|--------------|------------|---------------|-----------------|------------|
| IPv4         |            |               |                 |            |
| 10.10.0.0/20 | 10.10.0.1  | 10.10.15.254  |                 |            |
| VLAN         |            |               |                 |            |
| 30           | Management | 10.10.10.0/25 |                 |            |
| 111          | vpc1       | 10.10.2.0/24  |                 |            |
| 121          | vpc7       | 10.10.3.0/24  |                 |            |
| IP           |            |               |                 |            |
| Устройство   | Интерфейс  | Адрес IPv4    | Маска           | Шлюз       |
| R14          | Lo1        | 10.10.0.14    | 255.255.255.255 |            |
| R14          | e0/2       | 50.50.23.145  | 255.255.255.0   |            |
| R14          | e0/0       | 10.10.15.5    | 255.255.255.252 |            |
| R14          | e0/1       | 10.10.15.9    | 255.255.255.252 |            |
| R14          | e0/3       | 10.10.15.2    | 255.255.255.252 |            |
| R15          | Lo1        | 10.10.0.15    | 255.255.255.255 |            |
| R15          | e0/2       | 60.60.154.75  | 255.255.255.0   |            |
| R15          | e0/0       | 10.10.15.17   | 255.255.255.252 |            |
| R15          | e0/1       | 10.10.15.13   | 255.255.255.252 |            |
| R15          | e0/3       | 10.10.15.21   | 255.255.255.252 |            |
| R12          | Lo1        | 10.10.0.12    | 255.255.255.255 |            |
| R12          | e0/2       | 10.10.15.6    | 255.255.255.252 |            |
| R12          | e0/3       | 10.10.15.14   | 255.255.255.252 |            |
| R12          | VLAN111    | 10.10.11.254  | 255.255.255.0   |            |
| R12          | VLAN121    | 10.10.12.254  | 255.255.255.0   |            |
| R13          | VLAN30     | 10.10.10.126  | 255.255.255.128 |            |
| R13          | Lo1        | 10.10.0.13    | 255.255.255.255 |            |
| R13          | e0/2       | 10.10.15.18   | 255.255.255.252 |            |
| R13          | e0/3       | 10.10.15.10   | 255.255.255.252 |            |
| R13          | VLAN111    | 10.10.11.253  | 255.255.255.0   |            |
| R13          | VLAN121    | 10.10.12.253  | 255.255.255.0   |            |
| R13          | VLAN30     | 10.10.10.125  | 255.255.255.128 |            |
| R19          | Lo1        | 10.10.0.19    | 255.255.255.255 |            |
| R19          | e0/0       | 10.10.15.1    | 255.255.255.252 |            |
| R20          | Lo1        | 10.10.0.20    | 255.255.255.255 |            |
| R20          | e0/0       | 10.10.15.22   | 255.255.255.252 |            |
| SW3          | vlan30     | 10.10.10.3    | 255.255.255.128 |            |
| SW2          | vlan30     | 10.10.10.2    | 255.255.255.128 |            |
| SW4          | vlan30     | 10.10.10.4    | 255.255.255.128 |            |
| SW5          | vlan30     | 10.10.10.5    | 255.255.255.128 |            |
| VPC1         | eth0       | 10.10.11.2    | 255.255.255.0   | 10.10.11.1 |
| VPC7         | eth0       | 10.10.12.2    | 255.255.255.0   | 10.10.12.1 |

Схема HSRP

| HSRP   |        |         |               |
|--------|--------|---------|---------------|
| Филиал | Москва |         |               |
| Vlan   | Active | Standby | IP            |
| 30     | R12    | R13     | 10.10.10.10.1 |
| 111    | R12    | R13     | 10.10.10.11.1 |
| 121    | R13    | R12     | 10.10.10.12.1 |

Схема STP

![](https://github.com/svasornd/otus_network/blob/master/lab09/lab_09_Moscow_STP.png)

| STP | Priority  |
|-----|-----------|
| SW2 | 32769     |
| SW3 | 32769     |
| SW4 | 24577     |
| SW5 | 28673     |

[Конфигурация R12](https://github.com/svasornd/otus_network/blob/master/lab09/config/R12.md)

[Конфигурация R13](https://github.com/svasornd/otus_network/blob/master/lab09/config/R13.md)

[Конфигурация R14](https://github.com/svasornd/otus_network/blob/master/lab09/config/R14.md)

[Конфигурация R15](https://github.com/svasornd/otus_network/blob/master/lab09/config/R15.md)

[Конфигурация R19](https://github.com/svasornd/otus_network/blob/master/lab09/config/R19.md)

[Конфигурация R20](https://github.com/svasornd/otus_network/blob/master/lab09/config/R20.md)

[Конфигурация SW2](https://github.com/svasornd/otus_network/blob/master/lab09/config/SW2.md)

[Конфигурация SW3](https://github.com/svasornd/otus_network/blob/master/lab09/config/SW3.md)

[Конфигурация SW4](https://github.com/svasornd/otus_network/blob/master/lab09/config/SW4.md)

[Конфигурация SW5](https://github.com/svasornd/otus_network/blob/master/lab09/config/SW5.md)


Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию

        Gateway of last resort is 10.10.15.2 to network 0.0.0.0

        O*IA  0.0.0.0/0 [110/11] via 10.10.15.2, 00:24:29, Ethernet0/0


Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101

        Gateway of last resort is 10.10.15.21 to network 0.0.0.0

        O*E2  0.0.0.0/0 [110/1] via 10.10.15.21, 00:10:44, Ethernet0/0
              10.0.0.0/8 is variably subnetted, 13 subnets, 4 masks
        O IA     10.10.0.12/32 [110/21] via 10.10.15.21, 00:05:34, Ethernet0/0
        O IA     10.10.0.13/32 [110/21] via 10.10.15.21, 00:03:53, Ethernet0/0
        O IA     10.10.0.15/32 [110/11] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.10.0/25 [110/30] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.11.0/24 [110/30] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.12.0/24 [110/30] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.15.4/30 [110/30] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.15.8/30 [110/30] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.15.12/30 [110/20] via 10.10.15.21, 1d14h, Ethernet0/0
        O IA     10.10.15.16/30 [110/20] via 10.10.15.21, 1d14h, Ethernet0/0