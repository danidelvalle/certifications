# Virtual Network
### General
* Azure reserves first 3 IPs and last IPs
* Apply DNS to NIC or VNET
* Up to 50 virtual networks per subscription per region, although you can increase this limit to 500 by contacting Azure support
### Powershell
```powershell
# create vnet with subnet in 1 step

$subnet = New-AzVirtualNetworkSubnetConfig -Name subnet02 -AddressPrefix 10.1.0.0/24
New-AzVirtualNetwork -ResourceGroupName myResourceGroup -Location EastUS -Name myVNet2 -AddressPrefix 10.1.0.0/16 -Subnet $subnet

# add subnet to existing Vnet
$myVNet2 = New-AzVirtualNetwork -ResourceGroupName myResourceGroup -Location EastUS -Name myVNet2 -AddressPrefix 10.0.0.0/16
$mySubnet2 = Add-AzVirtualNetworkSubnetConfig -Name mySubnet2 -AddressPrefix 10.0.0.0/24 -VirtualNetwork $myVNet2
$mySubnet2 | Set-AzVirtualNetwork
```
### Network Security Group
* Rules evaluated by priority 100-4096 with lowest being evaluated first
* Be cautious when you want to apply NSG to both VM (NIC) and subnet level at the same time. NSGs are evaluated independently, and an “allow” rule must exist at both levels otherwise traffic will not be admitted.
* Service Tags of VirtualNetwork, AzureLB, Internet
* 100 NSGs per Sub (raise to 400)
* 200 NSGs rules per NSG (raise to 500)
* Vnets per Sub 50 (raise to 500)
* PIP dynamic 60 PIP reserved 20
### Public Ips
* Associated with VMs, LoadBalancer, AppGw, VpnGW
* Assignment: Dynamic (all) or Static (only VMs and LoadBalancer)
* Basic SKU: can be dynamic or static, open by default and no redundancy
* Standard SKU: is only static, supports AZ, and are closed by default (only VMs and Public Standard LBs)
* Load Balancer SKU and Public IP SKU must match
### Private IPs
* Associated with VMs, internal LBs and AppGws.
* Dyn (default) or Static in all cases
```powershell
$nic=Get-AzNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzNetworkInterface -NetworkInterface $nic
```
### Service Endpoints
* Azure Storage and Data Lake Store Gen 1
* Azure SQL Database and Azure SQL Data Warehouse
* Azure Database for PostgreSQL server and MySQL
* Azure Cosmos DB
* Azure Key Vault
* Azure Service Bus and Azure Event Hubs
### Routing
* Priority: 1userdefined, 2bgp and 3system
* Always most specific

## Inter-site connectivity
### Topologies
* Point2Site (P2S) - single device to azure
* Site2Site (S2S) - azure vnet to onPrem
* Vnet-to-Vnet - azure to azure, when you cannot use VNet peering. Connection between two Vnets in same or different subscriptions that is encrypted with IPSec (Azure2Azure when Encryption specific scenarios that are not latency sensitive and do not need high throughout).
### Types
* Route-Based (most types)
* Policy-Based - IPSec policies + address prefixes.
  * **Only S2S**
  * Only 1 tunnel
  * Only **Basic SKU**
### SKUs
|SKU|S2S/VNet-to-VNet Tunnels|P2S SSTP Connections|P2S IKEv2 Connections|Aggregate Throughput Benchmark|
|--- |--- |--- |--- |--- |
|Basic|Max. 10|Max. 128|Not Supported|100 Mbps|
|VpnGw1|Max. 30|Max. 128|Max. 250|650 Mbps|
|VpnGw2|Max. 30|Max. 128|Max. 500|1 Gbps|
|VpnGw3|Max. 30|Max. 128|Max. 1000|1.25 Gbps|
## VPN Gateways
* Require a dedicated subnet
* Basic SKU
* Route-based VPN doesn't support RADIUS or IKEv2 for P2S and supports 10 tunnels
### VpnGw1-3
* Supports route-based only
* Up to 30 tunnels, P2S, BGP, active-active, custom IPSec/IKE and ExpressRoute coexistence
### P2S VPN
* Supports SSTP, IKEv2, SSL/TLS
* Authenticate with certificate or RADIUS
* Basic SKU only support SSTP while VpnGw1+ supports IKEv2
* Clients download a config file
### Vnet Peering
* Regional VNet peering connects Azure virtual networks in the same region.
* Global VNet peering connects Azure virtual networks in different regions.
* Options:
  * **Forwarded Traffic** - allow VNA in another Vnet to forward traffic to this Vnet over the peering
  * **Allow gateway transit**. Allows the peer virtual network to use your virtual network gateway. The peer cannot already have a gateway configured.
  * **Use remote gateways**. Use your peer’s virtual gateway. Only one virtual network can have this enabled.
* You must configure peering on each virtual network. If you select ‘allow gateway transit’ on one virtual network; then you should select ‘use remote gateways’ on the other virtual network.

* VNet Peering is **non-transitive**. This means that if you establish VNet Peering between VNet1 and VNet2 and between VNet2 and VNet3, VNet Peering capabilities do not apply between VNet1 and VNet3. However, you can leverage user-defined routes and service chaining to implement custom routing that will provide transitivity. (A Virtual Machine Appliance or a VPN Virtual GW could make the routing to create a Hub and Spoke topology.)

Commands:

    az network peering create
    Add-AzVirtualNetworkPeering

# ExpressRoute
### General
* Standard is geopolitical region only and premium is global
* 50Mbps to 10Gbps
* Unlimited inbound traffic but outbound can be unlimited or metered
* Microsoft Peering and Private Peering
* /30 required for BGP peering for each path
* ASN for Azure is 12076
# Network Watcher
### General
* Requires Network Watcher Agent be installed on Linux/Windows (VM)
* Enabled on region by region basis and enabled in Network Watcher -> Overview blade
### Capabilities
* Topology
* Connection Monitor - Continuously monitor connections from VM to URI, IP, FQDN
* IP Flow Verify - Determine why packet allowed or denied and relevant NSG
* Effective Security Rules - see effective NSG cumulative, subnet, NIC
* VPN Troubleshooter
* Packet capture and log to storage account or to VM file system
* View Azure quotas and limits
* View and enable NSG flow logs
* Enable/Disable Diagnostic Logging for networking components
* Enable and view Traffic Analytics
### Network Performance Monitor
* Log Analytics Solution
* Monitor performance across cloud and on-premises
* Monitor network connectivity using HTTP, HTTPS, TCP, ICMP
* Monitor ExpressRoute
# Azure Load Balancer
https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal?tabs=option-1-create-load-balancer-standard

https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-powershell?tabs=option-1-create-load-balancer-standard

ARM
https://azure.microsoft.com/en-us/resources/templates/201-2-vms-loadbalancer-lbrules/

### General
* Layer 4 (TCP/UDP)
* Supports IPv4/IPv6
* Internal or Public
* By default Uses 5 tuple hash to distribute (source IP/port, dest IP/port, protocol) - same user traffic may be handled by different servers
* Supports session affinity (sticky sessions) using 2-tuple or 3-tuple
* Basic VMs cannot be used as targets

Multiple frontends are possible
https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-multivip-overview

### Health Probes
* Basic supports TCP/HTTP and Standard support TCP/HTTP/HTTPS
* Select port, path (HTTP/HTTPS), internal, unhealthy threshold
### Rule
* Port and backend port
* Backend pool
* Health probe
* Session persistence at transport session 5-tuple (src ip, src port, dst ip, dst port, protocol type)
* Idle timeout (minutes)
* Floating IP (SQL AlwaysOn)
### SKUs: Basic vs Standard
* More capacity in standard
* Standard supports HTTPS in addition to HTTP and TCP
* Standard supports AZ
* Standard supports outbound Rules / TCP Reset
* Standard has SLA of 99.99 w/ 2 health VM
* In the Standard SKU you can have up to 1000 instances in the backend pool. In the Basic SKU you can have up to 100 instances.

|-|Standard SKU|Basic SKU|
|--- |--- |--- |
|Backend pool endpoints|Any VM in a single virtual network, including a blend of VMs, availability sets, and VM scale sets.|VMs in a single availability set or VM scale set.|


Info:
* SKUs are not mutable. You may not change the SKU of an existing resource.
* A standalone virtual machine resource, availability set resource, or virtual machine scale set resource can reference one SKU, never both.
* A Load Balancer rule cannot span two virtual networks. Frontends and their related backend instances must be in the same virtual network.
* There is no charge for the Basic load balancer. The Standard load balancer is charged based on number of rules and data processed. Read more at the reference link.
* Load Balancer frontends are not accessible across global virtual network peering.

### Inbound NAT Rule
* Front-end IP
* Service
* Protocol
* Port
* Associations
* Port mapping (def/custom)
# Azure Application Gateway
### General
* Layer 7
* Cookie-based session affinity
* SSL offload
* End to end SSL
* Comes in standard, standardv2, WAF, WAFv2
* V2 supports AZ
* URL-based content filtering
* Requires its own subnet already exists
* Connection draining
### Components
* Front-end IP -> single public/private IP or both
* Listener -> port, protocol, host, IP, which further sends based on request routing rule (1 to 1)
* Request routing rule -> basic or path based
* Backend pool -> NIC, VMSS, Public IP, Private IP, FQDN, multi-tenant backend
# Azure Traffic Manager
### General
* DNS-level
* Supports VM, Cloud Services, Azure Web Apps, External Endpoints
* Internet-facing applications only
* HTTP/HTTPS GET-only

### Routing methods
* Priority - highest available priority
* Performance - latency of dns recursive to the regions
* Geographic - assign geos to endpoints and assign using the ip of the dns resolver
* Weighted - based on the weight of available servers

# Front Door Service
https://docs.microsoft.com/es-es/azure/frontdoor/quickstart-create-front-door
* Accelerate application performance Microsoft's global network for connecting to your application backends from Front Door POPs
* URL-based routing
* Multiple-site hosting
* Session affinity cookie-based session affinity 
* Secure Sockets Layer (SSL) termination
* Custom domains and certificate management
* Application layer security Web Application Firewall (WAF) 
* URL redirection
* URL rewrite
* Protocol support - IPv6 and HTTP/2 traffic
