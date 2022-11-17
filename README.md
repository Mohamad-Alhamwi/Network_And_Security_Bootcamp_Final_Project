# Network And Security Bootcamp Final Project

## Tasks To Be Done:

- [ ] The network devices will be working as a dual-stack. So each networking device will be configured with both IPv4 and IPv6 connectivity capabilities.
- [ ] The routing functionality will be done by using OSPF protocol.
- [ ] On both IPv4 and IPv6 backbones everything will be served from only the specified ports and every port except those ports will be closed. Apply that with the Access Control List (ACL).

## Project Phases:
The project will be done into 6 separate phases of work as the following:

**1. Phase:** Creating the base configuration for the devices.<br>
**2. Phase:** Configuring the backbone with IPV4.<br>
**3. Phase:** Doing the OSPF configuration on the IPV4 backbone.<br>
**4. Phase:** Configuring the backbone with IPV6.<br>
**5. Phase:** Doing the OSPF configuration on the IPV6 backbone.<br>
**6. Phase:** Restricting access to the only specified ports.<br>

### Phase 1: Basic Cisco Router/Switch Configuration

SUMMARY STEPS:

1. enable (en)

2. configure terminal (conf t)

3. hostname [PICK_WHATEVER_NAME_YOU_WANT]

4. enable secret [PICK_WHATEVER_PASSWORD_YOU_WANT]

5. line vty 0 4
   - password [PICK_A_STRONG_TELNET_PASSWORD] 
   - login

6. do copy running-config startup-config (do wr)

STEPS EXPLANATION:

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

So far, we have done all the basic configurations that need to be done whenever powering up a new Cisco router/switch. To display all the configurations currently running on the terminal, we use the `show running-config` or if we are still in the configuration mode we need to type `do show running-config`.

By doing all the above steps on all the network devices (switches and routers), we have gotten them ready for all the advanced configuration. And now, we can move on to the next phase.

### Phase 2: Router interfaces configuration with IPv4.

SUMMARY STEPS:

1. enable (en)

2. configure terminal (conf t)

3. interface fastethernet slot/port

4. ip address ip-address mask

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

![scenario 1 ping test](/Assets/Images/ping-test-1.png)

2. The Host A wants to ping the Host B which is on a different subnet that does not have a direct connection with the Host A's subnet router.

![scenario 2 ping test](/Assets/Images/ping-test-2.png)

3. The Host A wants to ping the IS-ER-01 router which is on a different subnet that has direct connection with the Host A's subnet router.

![scenario 3 ping test](/Assets/Images/ping-test-3.png)
