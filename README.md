# Switching and VLANs
Welcome to our exciting journey through the world of computer networking. We're not just learning about networking; we're diving deep into its core with two incredibly hands-on projects (Router on Stick && Switch Virtual Interfaces). It's like embarking on a thrilling adventure, where each step is a discovery in the vast universe of networks

- ### Router on Stick
  Here, we're not just reading about concepts; we're actually creating VLANs, configuring switches, and watching how data zips through our network. It's a bit like being a network conductor, orchestrating the flow of data with precision and skill. But that's just the warm-up

- ### Switch Virtual Interfaces
  The Switch Virtual Interfaces, is where things get really interesting. Now that you've got the basics down, we're moving onto something bigger and more complex. Imagine combining the power of a router and a switch into one that's what we're tackling here. You'll learn how to manage network traffic like a pro, understanding the INs and OUTs of multi-layer switching. This part is like solving a complex puzzle, where each piece is a different part of the network, and you're the one putting it all together.

## Procedure
### Part 1: Router on Stick 
  We will begin by exploring the
  initial setup, where our focus will be on configuring a Cisco router and dividing its interface into subinterfaces. Each step in this procedure is crafted to enhance our practical skills in network segmentation,    emphasizing the application of VLANs in efficiently managing traffic and ensuring secure, organized network communication.

- #### Building the topology
  There are two routers (Router 0 and Router 1) that serve as gateways for the network. Router 0 has three subinterfaces (Fa0/0.10, Fa0/0.20, Fa0/0.30), and Router 1 has three (Fa0/0.40, Fa0/0.50, Fa0/0.60). These subinterfaces correspond to different VLANs. The use of subinterfaces indicates that the routers are employing router-on-a-stick VLAN configurations, which allow a single router interface to route traffic between multiple VLANs. And there are six PCs (PC0 to PC5), each assigned to a specific VLAN.
![image](https://github.com/HussainRadwan/Switching-and-VLANs/assets/161932786/3b79f9a1-bcf8-4cd3-af9e-3770b5ed0270)

 For switch 2, we add an extra interface physically using PT-SWITCH-NM-1CFE Module, as shown in the Figure below:
![image](https://github.com/HussainRadwan/Switching-and-VLANs/assets/161932786/f611295b-bf87-48ab-9e3b-42efa9c1ea4a)

To configure the IP addresses we used the IPs as shown in the table below:
![image](https://github.com/HussainRadwan/Switching-and-VLANs/assets/161932786/40fe8b78-9294-4c88-8da1-a7fd200d059f)

- #### Configuring Routers
  - For router 0:
   ```
    Router>en
    Router#configure terminal
    Router(config)#interface FastEthernet0/0.10
    Router(config-subif)#encapsulation dot1Q 10
    Router(config-subif)#ip address 192.75.10.1 255.255.255.0
    Router(config-subif)#exit
    Router#
    Router(config)#interface FastEthernet0/0.20
    Router(config-subif)#encapsulation dot1Q 20
    Router(config-subif)#ip address 192.75.20.1 255.255.255.0
    Router(config-subif)#exit
    Router#
    Router(config)#interface FastEthernet0/0.30
    Router(config-subif)#encapsulation dot1Q 30
    Router(config-subif)#ip address 192.75.30.1 255.255.255.0
    Router(config-subif)#exit
   ```
  - For router 1:
   ```
    Router>en
    Router#configure terminal
    Router(config)#interface FastEthernet0/0.10
    Router(config-subif)#encapsulation dot1Q 10
    Router(config-subif)#ip address 192.75.10.1 255.255.255.0
    Router(config-subif)#exit
    Router#
    Router(config)#interface FastEthernet0/0.20
    Router(config-subif)#encapsulation dot1Q 20
    Router(config-subif)#ip address 192.75.20.1 255.255.255.0
    Router(config-subif)#exit
    Router#
    Router(config)#interface FastEthernet0/0.30
    Router(config-subif)#encapsulation dot1Q 30
    Router(config-subif)#ip address 192.75.30.1 255.255.255.0
    Router(config-subif)#exit
   ```
- #### Configuring OSPF Routing
  In OSPF, each sub-interface will be treated as a separate network. We need to add each sub-interface's network to the OSPF configuration. We used OSPF network commands to include the IP address ranges of my VLANs in the OSPF process. This will allow OSPF to advertise these networks to other OSPF routers, enabling routing between different VLANs.
  - For router 0:
    ```
      router ospf 1
      log-adjacency-changes
      network 192.75.10.0 0.0.0.255 area 0
      network 192.75.20.0 0.0.0.255 area 0
      network 192.75.30.0 0.0.0.255 area 0
      network 192.75.0.0 0.0.0.3 area 0
    ```
  - For router 1:
    ```
      router ospf 1
      log-adjacency-changes
      network 192.75.40.0 0.0.0.255 area 0
      network 192.75.50.0 0.0.0.255 area 0
      network 192.75.0.0 0.0.0.3 area 0
    ```

- #### Configuring Switches
  - ##### Creating a VLAN
    To create a VLAN, you typically need to access the switch’s configuration mode. This is usually done through a command-line interface. The commands you mentioned are typical for Cisco switches or other similar network equipment.
      
       For switch 0&1&2:
      ```
        Switch(config)# VLAN 10
        Switch(config-vlan)# exit
        Switch(config)# VLAN 20
        Switch(config-vlan)# exit
        Switch(config)# VLAN 30
        Switch(config-vlan)# exit
        Switch(config)# VLAN 40
        Switch(config-vlan)# exit
        Switch(config)# VLAN 50
        Switch(config-vlan)# exit
      ```
  - ##### Configuring Switch Access & Trunk
    Think of a building with different rooms, each for a specific purpose. Assigning a network port to a VLAN is like designating a door to lead to one of these rooms. For example, connecting a port to VLAN 10 is like setting a door to open only into the 'Room 10'. Devices connected to this port are like people entering through that door, finding themselves in 'Room 10', and can only interact with others in the same room.
Commands that we used to configure access and trunks:
  ##### For switch 0:
  ```
    Switch(config)#interface Fa0/1
    Switch(config-if)#switchport mode trunk 
    Switch(config-if)#ex
    Switch(config)#interface Fa1/1
    Switch(config-if)#switchport mode trunk 
    Switch(config-if)#ex
    Switch(config)#interface fa2/1
    Switch(config-if)#switchport access vlan 10
    Switch(config-if)#ex
    Switch(config)#interface fa3/1
    Switch(config-if)#switchport access vlan 20
  ```
  
  ##### For switch 1
  ```
    Switch(config)#interface fa1/1
    Switch(config-if)#switchport access vlan 30
    Switch(config-if)#ex
    Switch(config)#interface fa2/1
    Switch(config-if)#switchport access vlan 10
    Switch(config-if)#ex
    Switch(config)#interface fa3/1
    Switch(config-if)#switchport mode trunk
    For switch 2
    Switch(config)#interface fa6/1
    Switch(config-if)#switchport mode trunk 
    Switch(config-if)#ex
    Switch(config)#interface fa1/1
    Switch(config-if)#switchport access vlan 40
    Switch(config-if)#ex
    Switch(config)#interface fa2/1
    Switch(config-if)#switchport access vlan 50
    Switch(config-if)#ex
    Switch(config)#interface fa3/1
    Switch(config-if)#switchport access vlan 20
  ```

### Part 2: Switch Virtual Interfaces
In this session, we'll focus on configuring Switch Virtual Interfaces (SVIs) on Cisco IOS Switches, a crucial aspect of network management. We'll start by setting up a Cisco switch and delve into creating and managing SVIs, which are essential for providing gateways to VLANs. This hands-on approach is aimed at enhancing our understanding of how SVIs function and their role in facilitating efficient network architecture and performance. By the end of this exploration, we aim to be adept in using SVIs for advanced network configurations and functionalities.

As we will use the Topology in Part 1 to continue to build a Switch Virtual Interfaces (SVI) 

- #### Building the topology
  On the right side of the Topology we have the Topology that we covered on part one as (ROS) but now we are adding the left side to the ROS (multilayer switch 0) and this multilayer switch are connected to end devices (hosts) PC7, PC8, and PC9, each assigned to VLAN 70, 60, and 50 respectively. This multilayer switch has multiple FastEthernet interfaces (Fa0/1 - Fa0/6), suggesting that it handles inter-VLAN routing with subinterfaces for different VLANs, likely using Switch Virtual Interfaces (SVIs) for routing between VLANs.
  ![image](https://github.com/HussainRadwan/Switching-and-VLANs/assets/161932786/ac1bcbf9-5b79-49e5-9056-642a7dec8580)
  Use the following IPs shown in Table (Newly IPs) for the configuration addition Topology network:
  ![image](https://github.com/HussainRadwan/Switching-and-VLANs/assets/161932786/1b0c3a5b-382a-4945-ab85-18f4ebe36e82)

- #### Configuration Multi-Layer Switch to Router link
  To establish communication between a multi-layer switch and a router, we need to configure one of the switch ports to function as a routed port, essentially allowing it to behave like a port on a router. done it by adding an IP address (192.75.0.6) to the third layer (multi-layer) switch on fa0/1. This is done with the commands:
  ```
    Switch(config)#interface FastEthernet0/1
    Switch(config-if)#no switchport 
    Switch(config-if)#ip address 192.75.0.6 255.255.255.252
  ```

- #### Multi-Layer Switch Configuring VLAN Interfaces IPs
  To configuring Switch Virtual Interfaces (SVIs) on a multi-layer switch involves assigning IP addresses to VLAN interfaces. We have to Assigning an IP Address for each VLAN interface 60&70, it is done by commands on multilayer switch0:
  ```
    Switch(config)#interface vlan 60
    Switch(config-if)#ip address 192.75.60.1 255.255.255.0
    Switch(config-if)#ex
    Switch(config)#
    Switch(config)#interface vlan 70
    Switch(config-if)#ip address 192.75.70.1 255.255.255.0
    Switch(config-if)#
  ```

- #### Enabling routing on Multi-Layer Switch and configuring OSPF
  To configuration OSPF, first we have to add new network 192.75.0.4/30 to OSPF 1 in router 0, it is done by commands:
  ```
    Router(config)#router ospf 1
    Router(config-router)#network 192.75.0.4 0.0.0.3 area 0 
  ```
  
  Second, we have to add OSPF to the multi-layer switch 0 and adding the networks to it, and it is done by commands:
  ```
    Switch(config)#ip routing 
    Switch(config)#router ospf 1
    Switch(config-router)#network 192.75.0.4 0.0.0.3 area 0
    Switch(config-router)#network 192.75.60.0 0.0.0.255 area 0
    Switch(config-router)#network 192.75.70.0 0.0.0.255 area 0
    Switch(config-router)#network 192.75.50.0 0.0.0.255 area 0
    Switch(config-router)#network 192.75.10.0 0.0.0.255 area 0
  ```
  ##### The command (IP routing) is used to enable the layer 3 routing capability of the switch
	
- #### Configuring Access and Trunk Ports on Multi-Layer Switch
  Think of a building with different rooms, each for a specific purpose. Assigning a network port to a VLAN is like designating a door to lead to one of these rooms. For example, connecting a port to VLAN 10 is like setting a door to open only into the 'Room 10'. Devices connected to this port are like people entering through that door, finding themselves in 'Room 10', and can only interact with others in the same room.
  Commands that we used to configure access and trunks:

  - ##### Access ports:
     ```
      Switch(config)#interface fa0/3
      Switch(config-if)#switchport access vlan 70
      Switch(config-if)#ex
      
      Switch(config)#interface fa0/4
      Switch(config-if)#switchport access vlan 60
      Switch(config-if)#ex
      
      Switch(config-if)#interface fa0/5
      Switch(config-if)#switchport access vlan 50
      Switch(config-if)#ex
      
      Switch(config)#interface fa0/6
      Switch(config-if)#switchport access vlan 10
     ```
     
  - ##### Trunk:
    ```
      switch(config)#interface FastEthernet0/2
      Switch(config-if)#switchport trunk encapsulation dot1q 
      Switch(config-if)#switchport mode trunk
    ```

### Conclusion
In conclusion, both Router on a Stick and Switch Virtual Interfaces (SVIs) are techniques used for managing traffic across VLANs in a network.
Router on a Stick is a method that involves a single physical interface on a router configured with multiple sub-interfaces, each associated with a distinct VLAN. It allows a router to route traffic between VLANs using a single physical interface connected to a switch.
Switch Virtual Interfaces, on the other hand, are configured on multi-layer switches. SVIs enable a switch to perform routing between VLANs without the need for an external router. Each SVI acts as the default gateway for its respective VLAN, allowing devices within that VLAN to communicate with other VLANs and networks.
Both methods serve the purpose of facilitating communication between different VLANs, thereby segmenting and organizing network traffic efficiently. The choice between using Router on a Stick and SVIs often depends on the network requirements, hardware capabilities, and scalability needs.


