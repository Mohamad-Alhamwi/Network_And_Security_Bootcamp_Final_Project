# Network And Security Bootcamp Final Project

## Tasks To Be Done:

- [X] The network devices will be working as a dual-stack. So each networking device will be configured with both IPv4 and IPv6 connectivity capabilities.
- [X] The routing functionality will be done by using OSPF protocol.
- [X] On both IPv4 and IPv6 backbones everything will be served from only the specified ports and every port except those ports will be closed. Apply that with the Access Control List (ACL).

## Project Phases:
The project will be done into 6 separate phases of work as the following:

**1. Phase:** Creating the base configuration for the devices.<br>
**2. Phase:** Configuring the backbone with IPV4.<br>
**3. Phase:** Doing the OSPF configuration on the IPV4 backbone.<br>
**4. Phase:** Configuring the backbone with IPV6.<br>
**5. Phase:** Doing the OSPF configuration on the IPV6 backbone.<br>
**6. Phase:** Restricting access to the only specified ports.<br>

### Phase 1: Basic Cisco Router/Switch Configuration

==SUMMARY STEPS:==

1. enable (en)

2. configure terminal (conf t)

3. hostname [PICK_WHATEVER_NAME_YOU_WANT]

4. enable secret [PICK_WHATEVER_PASSWORD_YOU_WANT]

5. line vty 0 4
   - password [PICK_A_STRONG_TELNET_PASSWORD] 
   - login

6. do copy running-config startup-config (do wr)

==STEPS EXPLANATION:==

**1. Step:** To get into the privileged mode.

**2. Step:** To get into the global configuration mode.

**3. Step:** To assign the router/switch a name of our choice. Here, when we want to name a device, it is better to follow a simple (convenient) functional naming convention. For example, a name could be containing three identifires as the following:

`[LOCATION]-[DEVICE_TYPE / DEVICE_FUNCTION]-[THE_NUMBER_OF_THE_DEVICE]`

As a location identifier I used the following:

`IS` for Istanbul.

`AN` for Ankara.

`IZ` for Izmir.

As a device type/function identifier I used the following:

`ER` for edge routers.

`SW` for switches.

`MLSW` for Layer 3 switches.

**4. Step:** To Specify a hashed password to prevent unauthorized access to the router/switch. Here, we need to bare in mind that we had better not enable the `password`. It is not encrypted in any way and it is stored as a plain-text in the running configuration. No one use the `enable password` it is only there for backward compatibility.

**5. Step:** To specify a virtual terminal for remote console access (Telnet/SSH). The numbers 0-4 mean that the device can allow five simultaneous virtual connections. And as I know, we may have up to 16 (1-15) simultaneous virtual connections.

**6. Step:** To copy the running configurations into the startup configurations. We need to do this step because immediately after we type a command in the global configuration mode, it will be stored in the running configurations which reside in a device’s RAM, so if a device loses power, all the configurations will be erased. Here, I want to point out that we need to precede the command with `do` if we are still in the configuration mode.

On Ankara router, the configuration steps would be like the following:

```
en

conf t

hostname AN-ER-01

enable secret cisco

line vty 0 4

password cisco

login

do wr
```

So far, we have done all the basic configurations that need to be done whenever powering up a new Cisco router/switch. To display all the configurations currently running on the terminal, we use the `show running-config` or if we are still in the configuration mode we need to type `do show running-config`.

By doing all the above steps on all the network devices (switches and routers), we have gotten them ready for all the advanced configuration. And now, we can move on to the next phase.

### Phase 2: Router interfaces configuration with IPv4

SUMMARY STEPS:

1. enable (en)

2. configure terminal (conf t)

3. interface fastethernet slot/port

4. ip address [IP_ADDRESS] [MASK]

5. no shutdown (no sh)

6. exit

7. do copy running-config startup-config (do wr)

STEPS EXPLANATION:

I have already explained the first two steps. So let us jump into the step number 3.

**3. Step:**  To enter the configuration mode for a serial or fastethernet interface on the router. 

For example, on the AN-ER-01 router, for the serial interface the command would be `interface serial 0/0/0`, and for the fastethernet interface the command would be `interface fastEthernet 0/0`

**4. Step:** To set the IP address and subnet mask for the specified interface.

For example, on the AN-ER-01 router, for the serial interface the command would be `ip address 11.0.0.2 255.255.255.252`, and for the fastethernet interface the command would be `ip address 192.168.2.1 255.255.255.0`

**5. Step:**  To enable the interface to move from adminstration down status to up.

This command comes in very handy when configuring new interfaces or troubleshooting. When we are having trouble with an interface, we might want to try a `shut` and `no shut`.

**6. Step:** To exit configuration mode for an interface and return to global configuration mode.

After configuring the interfaces on our routers, we can display statistics for all interfaces configured on the router by typing in `show interfaces`. This command is useful and frequently used while configuring and monitoring devices.

Now that we have configured all the interfaces on all the routers, let us do a ping test. Here, let us consider the following three scenarios:

1. The Host A wants to ping the AN-ER-01 router which is on the same subnet.

![Scenario 1 ping test](/Assets/Images/ping-test-1.png)

2. The Host A wants to ping the Host B which is on a different subnet that does not have a direct connection with the Host A's subnet router.

![Scenario 2 ping test](/Assets/Images/ping-test-2.png)

3. The Host A wants to ping the IS-ER-01 router which is on a different subnet that has direct connection with the Host A's subnet router.

![Scenario 3 ping test](/Assets/Images/ping-test-3.png)

### Phase 3: Dynamic Routing with OSPF on the IPV4 backbone

SUMMARY STEPS:

1. enable (en)

2. configure terminal (conf t)

3. router ospf 1

4. network [NETWORK_IP] [WILDCARD_MASK] area [AREA_NUMBER]

STEPS EXPLANATION:

**3. Step:** To enable OSPF routing for the specified routing process, and place the router in router configuration mode.

**4. Step:** To advertise the networks on router in the correct OSPF areas.

The OSPF basic operating logic is forming neighbor relationships, called adjacencies, with other routers in the same **area** so that routers can share routing information. Let me explain this step by demonstrating on our topology.

On our topology, we have three routers Istanbul, Ankara and Izmir. We need first to make Ankara and Istanbul routers form neighbor relationship, then the same thing for Istanbul and Izmir routers.

On AN-ER-01, in the CLI we type `network 11.0.0.0 0.0.0.3 area 0`. After typing that, nothing is being presented on the CLI. Now let us switch over to the IS-ER-01 router, in the CLI we type `network 11.0.0.0 0.0.0.3 area 0`. After waiting for seconds, we are going to see something like in the picture below:

![Istanbul and Ankara creating nighbor relationship](/Assets/Images/Is-An-creating-neighbor-relationship.png)

This means that the neighbor relationship between Ankara router and Istanbul router has been established and now they are ready to exchange routing information between eachother. If we want to see what each one has exchanged with the other, we need to look at the routing table for each by typing `show ip route`. And as we see in the picture below, nothing has been exchanged between them:

![Istanbul and Ankara have not exchanged anything yet](/Assets/Images/Is-An-display-ip-route.png)

Now let us have the nighbor relationship established between Izmir router and Istanbul router. On IZ-ER-01, in the CLI we type `network 13.0.0.0 0.0.0.3 area 0`. After typing that, nothing is being presented on the CLI. Now let us switch over to the IS-ER-01 router, in the CLI we type `network 11.0.0.0 0.0.0.3 area 0`. After waiting for seconds, we are going to see something like in the picture below:

![Istanbul and Izmir Forming nighbor relationship](/Assets/Images/Is-Iz-creating-neighbor-relationship.png)

This means that the neighbor relationship between Izmir router and Istanbul router has been established too, and now they are ready to exchange routing information between eachother. Immediately after that, when we look at the routing table, we are going to notice that Istanbul has exchanged the route information of Ankara router with Izmir router as shown in the picture bellow:

![Istanbul and Izmir Forming nighbor relationship](/Assets/Images/Is-Iz-display-ip-route.png)

And exchanged the route information of Izmir router with Ankara router as shown in the picture bellow:

![Istanbul and Izmir Forming nighbor relationship](/Assets/Images/An-display-ip-route.png)

Now that the neighbor relationship has been formed among all the routers, they can share their internal network information by announcing it to their neighbor(s).

For example, in Istanbul router CLI, after we announce the internal network by typing `network 192.168.1.0 0.0.0.255 area 0`, in Izmir's and Ankara's routing table we will have something like in the image below:

![Istanbul and Izmir Forming nighbor relationship](/Assets/Images/An-Iz-display-ip-route.png)

At the moment, let us have Ankara and Izmir router announced their internal networks so that all the devices on the network can ping each other.

On Ankara router type in `network 192.168.2.0 0.0.0.255 area 0`. And on Izmir router type in `network 192.168.3.0 0.0.0.255 area 0`.

After that, let us open the terminal on the Host A and examine the scenarios 2 and 3 in phase 2 again:

![Scenario 2 ping test after OSPF routing](/Assets/Images/ping-test-2-after-ospf.png)


![Scenario  ping test after OSPF routing](/Assets/Images/ping-test-3-after-ospf.png)


And Voila! everything is working fine as intended.

### Phase 4: Router interfaces configuration with IPv6

SUMMARY STEPS:

1. enable (en)

2. configure terminal (conf t)

3. interface fastethernet slot/port

4. ipv6 address [IPv6_ADDRESS/extension]

5. no shutdown (no sh)

6. exit

7. do copy running-config startup-config (do wr)

STEPS EXPLANATION:

Everything here is same as in phase 2, except for step 4 where the version of IP and the network prefix for the address need to be specified.

For example, the command for the serial 0/0/0 interface on Ankara router would be `ipv6 add 1ef0:abc:bc:c::2/126`.


### Phase 5: Dynamic Routing with OSPF on the IPV6 backbone

SUMMARY STEPS:

1. enable (en)

2. configure terminal (conf t)

3. ipv6 unicast-routing

4. ipv6 router ospf 1

5. router-id [ROUTER_ID_FROM_YOUR_CHOICE]

6. int fa 0/0

7. ipv6 ospf 1 area 0

STEPS EXPLANATION:

Istanbul Router's IPv4 and IPv6 Routing Tables:

![Istanbul Router IPV4/IPV6 Routing Table](/Assets/Images/Is-ipv6-routing-table.png)

Ankara Router's IPv4 and IPv6 Routing Tables:

![Ankara Router IPV4/IPV6 Routing Table](/Assets/Images/An-ipv6-routing-table.png)

Izmir Router's IPv4 and IPv6 Routing Tables:

![Izmir Router IPV4/IPV6 Routing Table](/Assets/Images/Iz-ipv6-routing-table.png)

### Phase 6: Restricting access to the only specified ports

STEPS:

**1. Step:** DNS Server Configuration On Both Host A And Host B:

![DNS Server Conf On Both Hosts](/Assets/Images/Host-A-B-DNS-Config.png)

**2. Step:** Configuring ACL for both IPv4 and IPv6:

Access Control List For IPV6:

```
IPV6  access-list WEBACL6

permit udp any  host 1ef0:111:11:1::2 eq 53

permit tcp any  host 1ef0:111:11:1::3 eq 80

interface fastEthernet 0/0

ipv6 traffic-filter WEBACL6 out 

```

Access Control List For IPV4:

```
ip access-list extended WEBACL4

permit udp 192.168.2.0 0.0.0.255 host 192.168.1.2 eq 53

permit tcp 192.168.2.0 0.0.0.255 host 192.168.1.3 eq 80

permit udp 192.168.3.0 0.0.0.255 host 192.168.1.2 eq 53

permit tcp 192.168.3.0 0.0.0.255 host 192.168.1.3 eq 80

interface fastEthernet  0/0

ip access-group WEBACL4 out

```

![Show Access Lists On Istanbul Router](/Assets/Images/show-access-lists.png)


**3. Step:** Ping Test on both Host A and Host B:

Host A Ping Test:

![Show Access Lists On Istanbul Router](/Assets/Images/Host-A-Ping-Unreachable.png)

Host B Ping Test:

![Show Access Lists On Istanbul Router](/Assets/Images/Host-B-Ping-Unreachable.png)

**4. Step:** Sending request to the Web server on both Host A and Host B:

Request from Host A to the web server:

![Show Access Lists On Istanbul Router](/Assets/Images/Host-A-DNS-Request.png)

Request from Host B to the web server:

![Show Access Lists On Istanbul Router](/Assets/Images/Host-B-DNS-Request.png)


