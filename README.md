
# OSPF-SUBNETTING

## Description
The project is a Simulation of Designed Network Topology to demonstrate ospf,subnetting,dhcp relay,dns relay,communication between different subnets with routers acting as default gateway and use of serial links:

### Features:
- Used the ip 192.168.1.0/24 and the task was to divide into 4 subnets.
- 2 to the power of n equals 4 so n=2, means borrow(steal!) 2 bits from the host portion making it /26 .
- So 4 subnets 192.168.1.0, 192.168.1.64, 192.168.1.128, 192.168.1.192.
- The first and the third subnet are assign at DHCP pool for the machines that are at behind the  Layer 2 switches
- The second is assign at serial link connections between default gateway routers
- The last one is asign at DNS server
- The DHCP server is in the same subnet, the 192.168.1.0
- The DNS serves a very simple html 


---

## **1. Initial Setup**

- **Selection of routers**:
  - use 2 `4321` routers and connect them with serial link.
  - Serial link here represent a P2P connection in order to simulate real-world WAN links.
  - Put modules `NIM-2T` and `NIM-ES2-4` that provides 2x Serial Ports and the 2 provides four Gigabit Ethernet switching ports, enabling integrated switching capabilities within the router.Bring first the router down, put them and
    bring them up again.
  - Bring the serial port up. First we should assign the corresponding ip's.For router0, `enable`, `config t`, `int Serial0/1/0`, `ip address 192.168.1.65 255.255.225.192`, `no shut`. The same for the router1.
  - In Packet Tracer (and real Cisco routers), when using a serial cable, one router acts as the DCE (Data Communication Equipment) and the other as the DTE (Data Terminal Equipment).
  - The DCE side must set a clock rate, or the link will remain down.The clock rate controls the speed of data transmission over the link.If there is no clock rate, the routers don't know at what speed to transmit data, 
    so the interface stays down.
  - The router0 is the DCE and the router1 the DTE. For the DCE part we go to config mode, choose the interface and put the command `clock rate <rate number in bits per second (bps)>`. For the DTE part we don't do nothing.
  - Use the `show running-config | section interface Serial` command in config mode in order to check the configuration of the DCE,DTE.

---

- **Selection of switches**:
  - use 2 `pt-switch`.
  - connect them with Copper-Straight-Through cable at routers, with Fast ethernet port at switch side and Gigabit ethernet at router side.
  - Connect to the connected Gigabit interface from router side and bring it up, `en`, `config t`, `int <interface name>`, `no shut`. Do the same for th switch port.
  

---

- **Selection of machines**:

  - Connect with Copper-Straight-Through cable the pc,laptops with the switches at Fast ethernet from both sides.
  


- **Selection of Servers**:
  - Connect to the switch the DHCP server and at the first router the DNS Server.
    

## **2. ip configuration, subnetting, DHCP configuration,DNS configuration,OSPF, DHCP relay, DNS relay**
- **Subneting and DHCP pool**:
  - We have already ssign the ip for the serial link connection between the routers. Let's to the same for the Gigabit part that resides in both subnets and will act as default gateway.
  - We connect to the interface `GigabitEthernet0/0/0` for the subnet 192.168.1.0 and assign the last ip of the subnet 192.168.1.62
  - Do the same for the `GigabitEthernet0/0/0` and assign the 192.168.1.129 the first ip of the third subnet 192.168.1.128.
  - Assign a static ip 192.168.1.194 to the DNS server that is the second ip of the last subnet 192.168.1.192 and 192.168.1.194 at `GigabitEthernet0/0/1` at router 0

  Now we have gathered all the Information required to configure DHCP Server. So follow the steps
  1. In DHCP Tab you will find Pool Name, enter the desired pool name
  2. Next is Default Gateway, fill 192.168.1.62 for the pool for the first subnet 
  3. Next is DNS Server, it is IP of server itself 192.168.1.194
  4. Next is starting IP address, fill 192.168.1.0
  5. Next is Subnet Mask, fill 255.255.255.192 
  6. Next is Maximum users, fill by your requirements, example 30
  7. Click on add to add.
  8. Do the same for the 3 subnet for the IT department.
  9. Default Gateway, fill  192.168.1.129.
  10. DNS Server, it is IP of server itself 192.168.1.194.
  11. Starting IP address, fill 192.168.1.133.
  12. Next is Subnet Mask, fill 255.255.255.192.
  13. Next is Maximum users, fill by your requirements, example 30.
  14. To test the working of DHCP Server goto each PCs config > interface (Fastethernet0) and change Static option to DHCP in IP Configuration block.
  15. Put the simulation mode on in order to see the packet flow.

---




## Getting Started

### Dependencies



### Installing



## Help

For common issues, check the logs or run the following command for help:


## Authors

Contributors names and contact info

ex. Vasilhs S.  


## Version History



## License


## Acknowledgments


