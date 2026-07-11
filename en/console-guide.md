<!-- pre-align:aligned sig=d38698310a3d -->

<a id="network-network-interface-console-user-guide"></a>
## Network > Network Interface > Console User Guide { #network-network-interface-console-user-guide }


<a id="create"></a>
#### Create network interface

* Name: Enter a name for the network interface.

* VPC: Select the VPC in which to create the network interface.

* Subnet: Select the subnet in which to create the network interface.

* Virtual IP: Create a network interface to use for a virtual IP for redundancy.<br>You can preempt an IP to be used as a virtual IP so that it is not assigned to resources such as other instances and load balancers.<br>The network interface created with a virtual IP has a device name of `VIRTUAL_IP` and can be set as the gateway of the route in the route setting of the routing table.<br>A network interface created with a virtual IP cannot be directly attached to an instance for use when creating an instance.

* Security: This feature lets you prevent spoofing and set up a security group on a network interface.<br>Enabling the security feature allows you to prevent spoofing by blocking packets that do not have the IP/MAC address of a network interface as a source from being sent through the network interface, and to control traffic using security groups. <br>Disabling the security feature turns off all of such anti-spoofing features and security group settings. This will allow all packets, so if you do not have your own security features (firewall, etc.), it is strongly recommended to use this feature.

* Select security group: Select the security group to use on the network interface. Multiple selections can be made.

* Anti-spoofing: A feature that allows you to enable or disable the anti-spoofing security feature.<br>If anti-spoofing is disabled, packets with a source address not assigned to the network interface are also allowed to be sent.<br>You can connect this to instances that have packet forwarding capabilities, such as gateways. In particular, NAT instances must have anti-spoofing disabled, as they need to forward the source address as-is.

* Additional allowed addresses: Use this when you need to send packets with a specific IP as the source beyond the assigned primary IP while anti-spoofing is enabled. Registered IPs are exempted from spoofing blocking and can communicate normally.<br>Use this when you need a redundancy configuration using a predetermined virtual IP.


Click **OK** to create the network interface.

!!! tip "Note"
    Difference between creating a network interface when creating an instance and assigning an existing network interface

    * When creating a new network interface
      If you delete the instance, the automatically created network interface is also deleted.
    * When creating an instance by assigning an existing network interface and deleting the connected instance
      The network interfaces used by the instance will not be deleted together. The remaining network interfaces can be attached to other instances for use in the future.


<a id="change"></a>
#### Change network interface
Among the properties of the network interface, you can change the name, IP, anti-spoofing, additional allowed addresses, and security group.
Changes can be made only when the network interface is not associated with a floating IP.
To reflect the IP change, it takes time until the instance is rebooted and the DHCP is renewed.

<a id="delete"></a>
#### Delete network interface
Delete the selected network interface.
To delete a network interface, it must not be attached to a device.

<a id="virtual-ip-usage"></a>
#### Use virtual IPs
A network interface created for virtual IP use is intended to preempt a specific IP address in high availability (HA) configurations or redundancy scenarios and designate it as a route gateway in the routing table. A network interface created as a virtual IP has the device name `VIRTUAL_IP` and is not directly connected to a specific instance.

To use an IP designated as a virtual IP in an instance, complete the following steps:

1. **Change Security Settings**
The **Security** feature of the network interface restricts the source address to only the IP address assigned to that interface to prevent spoofing. To use a virtual IP, the security settings must be changed to bypass this anti-spoofing feature.
There are three ways to change the security settings, and you can choose the appropriate method based on the security level and environment.

    * **Disable security feature**<br>
    If the **Security** feature of the network interface is disabled, the anti-spoofing feature is also disabled, allowing all IP addresses including virtual IPs to be used without restriction. However, in this case, security groups are also not applied and all traffic is allowed, so this method should only be used in environments with their own security features (such as firewalls).

    * **Disable anti-spoofing only**<br>
    This method disables only the **Anti-spoofing** feature while maintaining other security features such as security groups. It is mainly used in instances that act as NAT instances or gateways. This method balances security and flexibility.

    * **Register additional allowed addresses (recommended)**<br>
    This method maintains all security features including security groups and anti-spoofing, while selectively allowing specific IPs by registering them as **Additional allowed addresses**. If the virtual IP to be used is predetermined, it can be registered in **Additional allowed addresses** for use. This is the most secure method.

2. **Configure instance operating system**
Once the security settings are changed, the operating system must be configured to allow the instance to use the virtual IP. The cloud system does not provide a separate feature for this, and it must be configured directly within the instance.
The virtual IP configuration method can be selected from the following options depending on the environment and requirements. For Linux, a virtual IP can be added as a secondary IP using the `ip addr add` command or by modifying network configuration files. For Windows, it can be added through advanced TCP/IP settings in the network adapter properties. It is also possible to automate the process using high availability solutions such as Keepalived and Pacemaker, or by configuring automatic IP assignment/release scripts for failover scenarios.

!!! tip "Note"
    Note the following when using virtual IPs:
    * The network interfaces of the virtual IP and the instances using it must be in the same subnet.
    * When communicating using a virtual IP, the existing security groups of the instances using it are applied as-is. That is, there are no security group rules applied separately to the virtual IP.
    * Instances that need to receive packets with the virtual IP as the source address must check that their security groups have rules to receive the virtual IP remotely.
      Since the virtual IP does not belong to any security group, if a "security group" is specified remotely in the inbound rules, the virtual IP may not be included in that security group, which may prevent communication.
    * Instances using a virtual IP may need to manually adjust internal routing rules within the instance depending on the network configuration.