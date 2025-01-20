# VPC


## VPC Sizes

- AWS VPCs can range from a minimum of /28 (16 IP addresses) to a maximum of /16 (65,536 IP addresses).
- Avoiding common ranges, such as x.y.z.0 - x.y.z.10, is recommended to avoid conflicts with other AWS resources.
- Reserving 2 networks per account per region is recommended to ensure future growth. For example, if there are 3 US regions, 1 European region, and 1 Australian region, and 4 accounts per region, then 5 * 4 * 2 = 40 networks should be reserved.

AWS defines the following VPC sizes for convenience:
- Micro: /24 netmask, /27 subnet size, 27 hosts/subnet, 8 subnets per VPC
- Small: /21 netmask, /24 subnet size, 251 hosts/subnet, 8 subnets per VPC
- Medium: /19 netmask, /22 subnet size, 1019 hosts/subnet, 8 subnets per VPC
- Large: /18 netmask, /21 subnet size, 2043 hosts/subnet, 8 subnets per VPC
- Huge: /16 netmask, /19 subnet size, 4091 hosts/subnet, 16 subnets per VPC

As an example, the breakdown of addresses using the Micro size would be:
- /24 bits reserved for the netmask
- /3 bits reserved for the number of subnets -> 2^3 = 8 subnets -> /27 subnet size
- 5 hosts reserved for AWS network and broadcast addresses
- Leaving 2^5 = 32 hosts - 5 reserved hosts = 27 hosts per subnet

