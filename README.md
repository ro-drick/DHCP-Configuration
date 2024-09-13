
# DHCP Configuration Lab

## Lab Overview
This lab involves configuring multiple DHCP pools on **Router R2**, setting **R1** as a DHCP client and relay agent, and verifying DHCP IP address assignments for **PC1** and **PC2**. The goal is to gain hands-on experience with DHCP server, client, and relay configurations.


<img src= "https://github.com/ro-drick/DHCP-Configuration/blob/main/dhcp.PNG">


## Lab Tasks

### Task 1: Configure DHCP Pools on R2
1. Create the following DHCP pools on **R2**:
    - **POOL1 (192.168.1.0/24)**:
      - Reserve **192.168.1.1** to **192.168.1.10**.
      - Set **DNS**: 8.8.8.8.
      - Set **Domain**: jeremysitlab.com.
      - Set the **Default Gateway** to **R1**.

    - **POOL2 (192.168.2.0/24)**:
      - Reserve **192.168.2.1** to **192.168.2.10**.
      - Set **DNS**: 8.8.8.8.
      - Set **Domain**: jeremysitlab.com.
      - Set the **Default Gateway** to **R2**.

    - **POOL3 (203.0.113.0/30)**:
      - Reserve **203.0.113.1**.

#### Commands:
```bash
# On R2
R2(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
R2(config)# ip dhcp pool POOL1
R2(dhcp-config)# network 192.168.1.0 255.255.255.0
R2(dhcp-config)# default-router 192.168.1.1
R2(dhcp-config)# dns-server 8.8.8.8
R2(dhcp-config)# domain-name jeremysitlab.com

R2(config)# ip dhcp excluded-address 192.168.2.1 192.168.2.10
R2(config)# ip dhcp pool POOL2
R2(dhcp-config)# network 192.168.2.0 255.255.255.0
R2(dhcp-config)# default-router 192.168.2.1
R2(dhcp-config)# dns-server 8.8.8.8
R2(dhcp-config)# domain-name jeremysitlab.com

R2(config)# ip dhcp excluded-address 203.0.113.1
R2(config)# ip dhcp pool POOL3
R2(dhcp-config)# network 203.0.113.0 255.255.255.252
```

### Task 2: Configure R1’s G0/0 Interface as a DHCP Client
1. Set **R1’s G0/0** interface to acquire its IP address via DHCP.

#### Commands:
```bash
# On R1
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address dhcp
R1(config-if)# no shutdown

# Verify the IP address
R1# show ip interface brief
```
Check the IP address assigned by DHCP to **R1**.

### Task 3: Configure R1 as a DHCP Relay Agent
1. Configure **R1** to act as a **DHCP relay agent** for the **192.168.1.0/24** subnet, relaying DHCP requests to **R2**.

#### Commands:
```bash
# On R1
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip helper-address 203.0.113.1
```

### Task 4: Request IP Addresses on PC1 and PC2
1. Use the CLI of **PC1** and **PC2** to request IP addresses from their respective DHCP servers.

#### Commands:
```bash
# On PC1 and PC2 (in CLI)
PC> ipconfig /renew

# Verify the IP address received
PC> ipconfig /all
```

## Summary of Configuration
- **R2** is configured with three DHCP pools for different subnets.
- **R1** is configured as a DHCP client and as a DHCP relay agent for the **192.168.1.0/24** subnet.
- **PC1** and **PC2** receive their IP addresses dynamically from their respective DHCP pools.

## Verification and Testing
- Verify the IP addresses assigned to **R1**, **PC1**, and **PC2** using `show ip interface brief` and `ipconfig` commands.
- Check the DHCP pool usage on **R2** using the command:
```bash
R2# show ip dhcp binding
```

### Conclusion

This lab provided practical experience in configuring DHCP pools, setting up a router as a DHCP client, and configuring a DHCP relay agent to forward requests between subnets.
 Through configuring DHCP on **R2**, setting **R1** to dynamically receive an IP address, and successfully enabling **PC1** and **PC2** to acquire their IPs via DHCP,
 you gained a deeper understanding of dynamic IP management and inter-router communication.
Mastering these configurations is essential for managing larger, scalable networks where efficient IP address allocation is crucial.


## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
