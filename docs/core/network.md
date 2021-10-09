# Category Topic


| Component/Service/Construct| AWS | Azure | GCP | IBM |
|----------------------------|----------------|---------------|------------|-----|
| Cloud Virtual NW/Private NW | VPC | VNet | VPC | VPC|
| Site-to-Site VPN GW| VPN GW, with Virtual Pri GW, Transit GW| VPN | Cloud VPN | VPN|
| Point-to-Site | VPN | VPN | VPN | VPN|
| Dedicated Connect to provider | DirectConnect-Dedicated, DirectConnect GW, Trnsit GW, Virt.Pri. GW | XpressRoute Direct| Dedicated Interconnect | VPN|
| Provider Connect thru partner(when client DC is in a location that can't reach a provider facility) | Direct Connect-Hosted | Xpress Route Partner | Partner Interconnect | VPN|
| L7 Load Balancer - Global| Global Accelerator | FrontDoor | Global HTTP(s) | xyz|
| L7 Load Balancer - Regional| ALB | App Gateway | None | xyz|
| L4 Load Balancer - GLobal| NLB | LB | Global SSL & TCP Proxy | xyz|
| L4 Load Balancer-Regional| NLB | LB | LB-Regional | xyz|
| L4 LB within Vnet/VPC| NLB | LB | LB-Internal| xyz|
| CDN| CloudFront | Azure CDN | LB-Internal| xyz|





   



