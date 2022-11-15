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

1. Step: To get into the privileged mode.
2. Step: To get into the global configuration mode.
3. Step: To assign the router/switch a name of our choice. Here, when we want to name a device, it is better to follow a simple (convenient) functional naming convention. For example, a name could be containing three identifires as the following:

`[LOCATION]-[DEVICE_TYPE / DEVICE_FUNCTION]-[THE_NUMBER_OF_THE_DEVICE]`

As a location identifier I used the following:

`IS` for istanbul.
`AN` for ankara.
`IZ` for Izmir.

As a device type/function identifier I used the following:

`ER` for the edge routers.
`SW` for the switches.

4. To Specify a hashed password to prevent unauthorized access to the router/switch. Here, we need to bare in mind that we had better not enable the `password`. It is not encrypted in any way and it is stored as a plain-text in the running configuration. No one use the `enable password` it is only there for backward compatibility.

5. Step: To specify a virtual terminal for remote console access (Telnet/SSH). The numbers 0-4 mean that the device can allow five simultaneous virtual connections. And as I know, we may have up to 16 (1-15) simultaneous virtual connections.

6. Step: To copy the running configurations into the startup configurations. We need to do this step because immediately after we type a command in the global configuration mode, it will be stored in the running configurations which reside in a device’s RAM, so if a device loses power, all the configurations will be erased. Here, I want to point out that we need to precede the command with `do` if we are still in the configuration mode. 

So far, we have done all the basic configurations that need to be done whenever powering up a new Cisco router/switch. To display all the configurations currently running on the terminal, we use the `show running-config` or if we are still in the configuration mode we need to type `do show running-config`.

By doing all the above steps on all the network devices (switches and routers), we have gotten them ready for all the advanced configuration. And now, we can move on to the next phase.

### Phase 2: Router interfaces configuration with IPv4.


