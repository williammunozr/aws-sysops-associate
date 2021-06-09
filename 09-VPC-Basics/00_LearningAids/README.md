# Amazon VPC

## Table of Contents

1. [VPC Sizing and Structure](#vpc-sizing-n-structure)
2. [Network Access Control List - NACL](#nacl)
3. [Network Access Control List - NACL - Ephemeral](#nacl-ephemeral)
4. [NAT Architecture](#nat-architecture)
5. [VPC Design - NATGW Full Resilience](#natgw-full-resilience)
6. [Using an IGW](#ig-architecture)
7. [IPv4 Addresses with a IGW](#igw-static-nat)
8. [Security Group](#security-group)

## VPC Sizing and Structure <a name = "vpc-sizing-n-structure"></a>

### VPC Considerations

- What `size` should the VPC be..
- Are there any Networks we can't use..
- VPC's, Cloud, On-premises, Partners & Vendors
- Try to predict the future..
- VPC `Structure`- Tiers & Resiliency (Availability) Zones

### Example Global Architecture

#### Major Offices (Service Consumption)

- London
- New York
- Seattle

#### Field Workers

- Laptop
- 3G, 4G, Satellite Connection
- Data u/l & d/l
- Email, File Access
- Chat & Planning
- Research datasets

#### Head Office

- On-premise `192.168.10.0/24`
- AWS Pilot `10.0.0.0/16`
- Azure Pilot `172.31.0.0/16`

#### IP Ranges to Avoid in this example

| Net             | Location   | IP Range                      |
| --------------- | :--------- | :---------------------------: |
| 192.168.10.0/24 | On-Premise | 192.168.10.0 - 192.168.10.255 |
| 10.0.0.0/16     | AWS        | 10.0.0.0 - 10.0.255.255       |
| 172.31.0.0/16   | Azure      | 172.31.0.0 - 172.31.255.255   |
| 192.168.15.0/24 | London     | 192.168.15.0 - 192.168.15.255 |
| 192.168.20.0/24 | New York   | 192.168.20.0 - 192.168.20.255 |
| 192.168.25.0    | Seattle    | 192.168.25.0 - 192.168.25.255 |
| 10.128.0.0/9    | Google     | 10.128.0.0 - 10.255.255.255   |

#### More Considerations

- VPC minimum `/28` (16 IP), maximum `/16` (65456 IPs)
- Personal preference for the 10.x.y.z range
- Avoid common ranges - avoid future issues
- Reserve 2+ networks per region being used per account
- 3 US, Europe, Australia (5) x2 - Assume 4 Accounts
- Total 40 ranges (ideally)

#### VPC Sizing

AWS have some recommendations for sizing a VPC:

| VPC Size        | Netmask | Subnet Size | Hosts/Subnet* | Subnets/VPC | Total IPs* |
| --------------- | :-----: | :---------: | :-----------: | :---------: | :--------: |
| **Micro**       | /24     | /27         | 27            | 8           | 216        |
| **Small**       | /21     | /24         | 251           | 8           | 2008       |
| **Medium**      | /19     | /22         | 1019          | 8           | 8152       |
| **Large**       | /18     | /21         | 2043          | 8           | 16344      |
| **Extra Large** | /16     | /20         | 4091          | 16          | 65456      |

[AWS Single VPC Design](https://aws.amazon.com/answers/networking/aws-single-vpc-design/)

## Network Access Control List - NACL <a name="nacl"></a>

![Network Access Control List - NACL](NACL.png)

- Network Access Control Lists (NACLs) are a type of security filter (like firewalls) which can filter traffic as it enters or leaves a subnet.
- NACLs are attached to subnets and only filter data as it crosses the subnet boundary.
- NACLs are stateless and so see initiation and response phases of a connection and 1 inbound and 1 outbound stream requiring two roles (one IN one OUT).
- Stateless - INITIATION and RESPONSE seen as different.
- Outbound ports are ephemeral (1024-65535).
- Only impacts data crossing subnet border.
- Can EXPLICITLY ALLOW and DENY
- IPs/Networks, Ports & Protocols - no logical resources.
- NACLs cannot be assigned TO AWS resources.. only subnets.
- Use WITH SG's to add explicit DENY (Bad IPs/Nets).
- One subnet = One NACL at a time.
- It is ordered processing if it finds a rule that matches, it just stops.

## Network Access Control List - NACL - Ephemeral <a name="nacl-ephemeral"></a>

![Network Access Control List - NACL - Ephemeral](NACL_ephemeral.png)

## NAT Architecture <a name="nat-architecture"></a>

![NAT Architecture](NATArchitecture.png)

## VPC Design - NATGW Full Resilience <a name="natgw-full-resilience"></a>

![VPC Design - NATGW Full Resilience](NATHA.png)

## Using an IGW - Architecture <a name="ig-architecture"></a>

![Using an IGW - Architecture](InternetGatewayArchitectiure.png)

## IPv4 Addresses with a IGW <a name="igw-static-nat"></a>

![IPv4 Addresses with a IGW](IGW_Static_NAT.png)

## Security Group <a name="security-group"></a>

![Security Group](SecurityGroup.png)

