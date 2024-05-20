## Network > Network Interface > Console User Guide


#### Create network interface

* Name: Enter a name for the network interface.

* VPC: Select the VPC in which to create the network interface.

* Subnet: Select the subnet in which to create the network interface.

* Virtual IP: Create a network interface to use for a virtual IP for redundancy.<br>You can preempt an IP to be used as a virtual IP so that it is not assigned to resources such as other instances and load balancers.<br>The network interface created with a virtual IP has a device name of "VIRTUAL_IP" and can be set as the gateway of the route in the route setting of the routing table.<br>A network interface created with a virtual IP cannot be directly attached to an instance for use when creating an instance.

* Security: This feature lets you prevent spoofing and set up a security group on a network interface.<br>Enabling the security feature allows you to prevent spoofing by blocking packets that do not have the IP/MAC address of a network interface as a source from being sent through the network interface, and to control traffic using security groups. <br>Disabling the security feature turns off all of such anti-spoofing features and security group settings. This will allow all packets, so if you do not have your own security features (firewall, etc.), it is strongly recommended to use this feature.

* Select security group: Select the security group to use on the network interface. Multiple selections can be made.

Click **OK** to create the network interface.

> [Note] Difference between creating a network interface when creating an instance and assigning an existing network interface
>
> * When creating a new network interface
> 	If you delete the instance, the automatically created network interface is also deleted.
> * When creating an instance by assigning an existing network interface and deleting the connected instance
>	The network interfaces used by the instance will not be deleted together. The remaining network interfaces can be attached to other instances for use in the future.


#### Change network interface
Among the properties of the network interface, you can change the name, IP, and security group.
Changes can be made only when the network interface is not associated with a floating IP.
To reflect the IP change, it takes time until the instance is rebooted and the DHCP is renewed.

#### Delete network interface
Delete the selected network interface.
To delete a network interface, it must not be attached to a device.

#### Change source/target check
You can enable or disable source/target check to determine if it is the source or target of the traffic that the instance receives.
For network interfaces of NAT instances, source/target check should not be enabled, so manage the enable/disable features through this menu.

#### Use virtual IPs
The network interface you create for a virtual IP is for IP preemption and for designation as a route gateway in the routing table, and is not directly associated with a specific instance.
To use an IP that you specify as a virtual IP, the following procedure is required.

1. Change Security Settings
The **Security** feature on a network interface enables anti-spoofing, which means that packets originating from an instance can only be sent if they have an IP assigned by the cloud system as a source.
Therefore, to use a virtual IP, you must change the security settings to bypass the anti-spoofing feature.
    * Disable security feature<br>
        Disabling the **Security** feature on a network interface turns off anti-spoofing, allowing unrestricted use of virtual IPs.<br>
        However, you can't apply security groups in this case, so it's not recommended unless you have your own security features in place.
    * Register additional allowed addresses<br>
        When registering the virtual IP as an "additional allowed address" on the network interfaces of the instances for which the virtual IP is used, you allow packets with this IP as a source.<br>
        In limited cases, you can bypass anti-spoofing and apply security groups without turning off security features.<br>
        However, this feature is not currently available in the console, to use this feature, you need to create a virtual IP and contact [1:1 Inquiry](https://www.nhncloud.com/en/support/inquiry) using the form below (it will be available in the console in the future).
        ```
        1. Request to register additional allowed addresses
            * Organization ID/Project ID:
            * Region: (Public/Public, KR1/KR2)
        2. Virtual IP
            * IP Address:
            * VPC ID:
        3. Register additional allowed addresses for
            * Instance Name:
            * Instance UUID:
            * The IP of the network interface to which the virtual IP will be attached:
        ```
        
2. Configure instance operating system
After you've changed the security settings, configure a virtual IP for your instance. The cloud system does not provide a separate feature for this, and you must either add the IP within the instance using the appropriate method for the instance's operating system or distribution, or configure it yourself using an application.

> [Note] When using virtual IPs, please note the following.
> * The virtual IP and the network interfaces of the instances that will use the IP must be located within the same subnet.
> * When communicating using a virtual IP, instances using it are subject to existing security groups.Therefore, there are no separate security group rules that apply to virtual IPs.
> * Instances that must receive packets with a virtual IP as a source must check their security groups for rules that allow them to receive virtual IPs remotely.
>     Virtual IPs do not belong to any security groups, so if you specify "security groups" in your receipt rules remotely, you may not be able to communicate with them because they are not included in that security group.
> * Instances that use virtual IPs may need to manually adjust their internal routing rules depending on their network configuration.

