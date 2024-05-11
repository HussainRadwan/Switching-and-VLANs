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
