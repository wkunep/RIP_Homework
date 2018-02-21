# Практика. RIP

Борзенко Кирилл  (КН-202). Номер в журнале 5  (**n = 5**)

Создаем сеть, как показано в практике . В каждый **Router (1-3)** добавляем физический модуль **NM-1FE-TX**.

## Настройка Роутеров

#### Router 1

```
Router>enable
Router#conf t
Router(config)#int fa0/0
Router(config-if)#ip addres 192.168.10.1 255.255.255.252
Router(cofig-if)#no shutdown
Router(config-if)#exit

Router(config)#int fa0/1
Router(config-if)#ip addres 10.5.1.1  255.255.255.0
Router(cofig-if)#no shutdown
Router(config-if)#exit

Router(config)#int fa1/0
Router(config-if)#ip addres 192.168.1.1 255.255.255.0
Router(cofig-if)#no shutdown
Router(config-if)#exit
Router(config)#exit
Router#write memory
Router#exit

```

#### Router 2

```
Router>enable
Router#conf t
Router(config)#int fa0/0
Router(config-if)#ip addres 192.168.10.2 255.255.255.252
Router(cofig-if)#no shutdown
Router(config-if)#exit

Router(config)#int fa0/1
Router(config-if)#ip addres 192.168.10.6  255.255.255.252
Router(cofig-if)#no shutdown
Router(config-if)#exit

Router(config)#int fa1/0
Router(config-if)#ip addres 192.168.2.1 255.255.255.0
Router(cofig-if)#no shutdown
Router(config-if)#exit
Router(config)#exit
Router#write memory
Router#exit

```

#### Router 3

```
Router>enable
Router#conf t
Router(config)#int fa0/0
Router(config-if)#ip addres 10.5.1.2 255.255.255.0
Router(cofig-if)#no shutdown
Router(config-if)#exit

Router(config)#int fa0/1
Router(config-if)#ip addres 192.168.10.5  255.255.255.252
Router(cofig-if)#no shutdown
Router(config-if)#exit

Router(config)#int fa1/0
Router(config-if)#ip addres 192.168.3.1 255.255.255.0
Router(cofig-if)#no shutdown
Router(config-if)#exit
Router(config)#exit
Router#write memory
Router#exit

```

## Назначение IP

#### Server 1:

```
IP Adress:     192.168.1.15
Subnet Mask:   255.255.255.0

```

#### Server 2:

```
IP Adress:     192.168.2.15
Subnet Mask:   255.255.255.0

```

#### Server 3:

```
IP Adress:     192.168.3.15
Subnet Mask:   255.255.255.0

```

#### PC 0:

```
IP Adress:       10.5.1.15
Subnet Mask:     255.255.255.0
Default gateway: 10.5.1.2`

```

#### PC 1:

```
IP Adress:       10.5.1.16
Subnet Mask:     255.255.255.0
Default gateway: 10.5.1.2

```

## Запуск протокола RIP

#### Router 1

```
Router>enable
Router#conf t
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.5.1.0
Router(config-router)#network 192.168.1.0
Router(config-router)#exit
Router(config)#exit
Router#write memory
Router#exit

```

#### Router 2

```
Router>enable
Router#conf t
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 192.168.10.0
Router(config-router)#network 192.168.10.4
Router(config-router)#network 192.168.2.0
Router(config-router)#exit
Router(config)#exit
Router#write memory
Router#exit

```

#### Router 3

```
Router>enable
Router#conf t
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 10.5.1.0
Router(config-router)#network 192.168.10.4
Router(config-router)#network 192.168.3.0
Router(config-router)#exit
Router(config)#exit
Router#write memory
Router#exit

```

## Результаты

#### Router 1

```
Router>enable
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
R       10.0.0.0/8 is possibly down, routing via 192.168.10.2, FastEthernet0/0
C       10.5.1.0/24 is directly connected, FastEthernet0/1
C    192.168.1.0/24 is directly connected, FastEthernet1/0
R    192.168.2.0/24 [120/1] via 192.168.10.2, 00:00:01, FastEthernet0/0
R    192.168.3.0/24 [120/1] via 10.5.1.2, 00:00:08, FastEthernet0/1
     192.168.10.0/24 is variably subnetted, 3 subnets, 2 masks
R       192.168.10.0/24 is possibly down, routing via 10.5.1.2, FastEthernet0/1
C       192.168.10.0/30 is directly connected, FastEthernet0/0
R       192.168.10.4/30 [120/1] via 192.168.10.2, 00:00:01, FastEthernet0/0

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.1    YES manual up                    up 
FastEthernet0/1        10.5.1.1        YES manual up                    up 
FastEthernet1/0        192.168.1.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
Router#exit
```

#### Router 2

```
Router>enable
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

R    10.0.0.0/8 [120/1] via 192.168.10.1, 00:00:20, FastEthernet0/0
                [120/1] via 192.168.10.5, 00:00:08, FastEthernet0/1
R    192.168.1.0/24 [120/1] via 192.168.10.1, 00:00:20, FastEthernet0/0
C    192.168.2.0/24 is directly connected, FastEthernet1/0
R    192.168.3.0/24 [120/1] via 192.168.10.5, 00:00:08, FastEthernet0/1
     192.168.10.0/24 is variably subnetted, 3 subnets, 2 masks
R       192.168.10.0/24 is possibly down, routing via 192.168.10.1, FastEthernet0/0
C       192.168.10.0/30 is directly connected, FastEthernet0/0
C       192.168.10.4/30 is directly connected, FastEthernet0/1

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.2    YES manual up                    up 
FastEthernet0/1        192.168.10.6    YES manual up                    up 
FastEthernet1/0        192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
Router#exit
```

#### Router 3

```
Router>enable
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

R    10.0.0.0/8 [120/1] via 192.168.10.1, 00:00:20, FastEthernet0/0
                [120/1] via 192.168.10.5, 00:00:08, FastEthernet0/1
R    192.168.1.0/24 [120/1] via 192.168.10.1, 00:00:20, FastEthernet0/0
C    192.168.2.0/24 is directly connected, FastEthernet1/0
R    192.168.3.0/24 [120/1] via 192.168.10.5, 00:00:08, FastEthernet0/1
     192.168.10.0/24 is variably subnetted, 3 subnets, 2 masks
R       192.168.10.0/24 is possibly down, routing via 192.168.10.1, FastEthernet0/0
C       192.168.10.0/30 is directly connected, FastEthernet0/0
C       192.168.10.4/30 is directly connected, FastEthernet0/1

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.2    YES manual up                    up 
FastEthernet0/1        192.168.10.6    YES manual up                    up 
FastEthernet1/0        192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
Router#exit
```