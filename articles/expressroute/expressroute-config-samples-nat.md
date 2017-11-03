---
title: "ExpressRoute müşteri yönlendirici yapılandırma örnekleri | Microsoft Docs"
description: "Bu sayfa için Cisco ve Juniper yönlendirici yönlendirici yapılandırma örnekleri sağlar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: d6ea716f-d5ee-4a61-92b0-640d6e7d6974
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 83a7da2db537a3c900e90432455d59e8ac56d917
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-nat"></a>NAT ayarlamanıza için yönlendirici yapılandırması örnekleri
Bu sayfa için Cisco ASA ve Juniper SRX series yönlendirici NAT yapılandırma örnekleri sağlar. Bu yönergeler yalnızca için örnek olacak şekilde tasarlanmıştır ve olarak kullanılmamalıdır. Ağınız için uygun yapılandırmalarla gündeme için satıcınıza çalışabilir. 

> [!IMPORTANT]
> Bu sayfayı örneklerinde zamanıyla ilgili yönergeler için olacak şekilde tasarlanmıştır. Satıcınızın satış / teknik ekip ve ağ ekibinizin gereksinimlerinizi karşılamak için uygun yapılandırmaları gündeme birlikte çalışmalısınız. Microsoft bu sayfada listelenen yapılandırmaları ile ilgili sorunlar desteklemez. Destek sorunları için aygıt satıcınıza başvurmanız gerekir.
> 
> 

* Yönlendirici yapılandırma örnekleri aşağıdaki Azure genel ve Microsoft eşlemeleri için geçerlidir. NAT Azure özel eşleme için yapılandırmamalısınız. Gözden geçirme [ExpressRoute eşlemeler](expressroute-circuit-peerings.md) ve [ExpressRoute NAT gereksinimleri](expressroute-nat.md) daha fazla ayrıntı için.

* İnternet'e ve ExpressRoute bağlantı için ayrı NAT IP havuzu kullanmanız gerekir. ExpressRoute ve Internet aynı NAT IP havuzu kullanarak asimetrik Yönlendirme ve bağlantı kaybı neden olur.


## <a name="cisco-asa-firewalls"></a>Cisco ASA güvenlik duvarları
### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Microsoft Müşteri ağ trafiği için PAT yapılandırma
    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>


    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>

    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>

    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>

    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2

    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Microsoft Müşteri ağ trafiği için PAT yapılandırma

**Arabirimleri ve yönü:**

    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

**Yapılandırma:**

NAT havuzu:

    object network outbound-PAT
        host <NAT-IP>

Hedef sunucu:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Müşteri IP adresleri için nesne grubu

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT komutlar:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX series yönlendirici
### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Yedek Ethernet arabirimleri için küme oluşturma
    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. İki güvenlik bölgeleri oluşturma
* Güven iç ağ için hem de Untrust bölgesine sınır yönlendiricileri karşılıklı dış ağ için
* Bölgelere uygun arabirimleri atayın
* Arabirimler Services'de izin ver

    güvenlik {bölgeleri {güvenlik bölgesi güven {konak gelen-trafik {Sistem Hizmetleri {ping;                   } protokolleri {bgp;                   {reth0.100;}} arabirimleri               }} güvenlik bölgesi Untrust {konak gelen-trafik {Sistem Hizmetleri {ping;                   } protokolleri {bgp;                   {reth1.100;}} arabirimleri               }           }       }   }


### <a name="3-create-security-policies-between-zones"></a>3. Bölgeler arasında güvenlik ilkeleri oluşturma
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. NAT ilkelerini yapılandırma
* İki NAT havuzları oluşturun. Bir NAT trafiği giden Microsoft ve diğer Microsoft müşteri için kullanılır.
* Karşılık gelen trafiği NAT kuralları oluşturma
  
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       to routing-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       to routing-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }

### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. Her yönde seçmeli öneklerini BGP yapılandırın
Örnekleri başvurmak [yönlendirme yapılandırma örnekleri ](expressroute-config-samples-routing.md) sayfası.

### <a name="6-create-policies"></a>6. İlke oluşturma
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).

