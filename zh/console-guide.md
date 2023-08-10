## Network > Network Interface > Console User Guide


#### Create network interface

* Name: Enter a name for the network interface.

* VPC: Select the VPC in which to create the network interface.

* Subnet: Select the subnet in which to create the network interface.

* Virtual IP: Create a network interface to use for a virtual IP for redundancy.<br>You can preempt an IP to be used as a virtual IP so that it is not assigned to resources such as other instances and load balancers.<br>The network interface created with a virtual IP has a device name of "VIRTUAL_IP" and can be set as the gateway of the route in the route setting of the routing table.<br>A network interface created with a virtual IP cannot be directly attached to an instance for use when creating an instance.

* Security: This feature lets you prevent spoofing and set up a security group on a network interface.<br>Enabling the security feature allows you to prevent spoofing by blocking packets that do not have the IP/MAC address of a network interface as a source from being sent through the network interface, and to control traffic using security groups. <br>Disabling the security feature turns off all of such anti-spoofing features and security group settings. This will allow all packets, so if you do not have your own security features (firewall, etc.), it is strongly recommended to use this feature.

* Select security group: Select the security group to use on the network interface. Multiple selections can be made.

Click **OK** to create the network interface.

#### Change network interface
Among the properties of the network interface, you can change the name, IP, and security group.
Changes can be made only when the network interface is not associated with a floating IP.
To reflect the IP change, it takes time until the instance is rebooted and the DHCP is renewed.

#### Delete network interface
Delete the selected network interface.
To delete a network interface, it must not be attached to a device.

#### Change source/target check
This feature is only available in the Korea (Pangyo) and Korea (Pyeongchon) regions.
You can enable or disable source/target check to determine if it is the source or target of the traffic that the instance receives.
For network interfaces of NAT instances, source/target check should not be enabled, so manage the enable/disable features through this menu.

> [Note] Difference between creating a network interface when creating an instance and assigning an existing network interface
>
> * When creating a new network interface
> 	If you delete the instance, the automatically created network interface is also deleted.
> * When creating an instance by assigning an existing network interface and deleting the connected instance
>	The network interfaces used by the instance will not be deleted together. The remaining network interfaces can be attached to other instances for use in the future.
