---
title: "ExpressRoute müşteri yönlendirici yapılandırma örnekleri | Microsoft Docs"
description: "Bu sayfa için Cisco ve Juniper yönlendirici yönlendirici yapılandırma örnekleri sağlar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 564826bc-017a-4683-a385-37c9fa814948
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 032e584dc5abf59e9e3e8d80673b402f1fbf721b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a>Yönlendirme ayarlamanıza için yönlendirici yapılandırması örnekleri
Bu sayfa için Cisco IOS-XE ve Juniper MX series yönlendirici arabirimi ve yönlendirme yapılandırma örnekleri sağlar. Bu yönergeler yalnızca için örnek olacak şekilde tasarlanmıştır ve olarak kullanılmamalıdır. Ağınız için uygun yapılandırmalarla gündeme için satıcınıza çalışabilir. 

> [!IMPORTANT]
> Bu sayfayı örneklerinde zamanıyla ilgili yönergeler için olacak şekilde tasarlanmıştır. Satıcınızın satış / teknik ekip ve ağ ekibinizin gereksinimlerinizi karşılamak için uygun yapılandırmaları gündeme birlikte çalışmalısınız. Microsoft bu sayfada listelenen yapılandırmaları ile ilgili sorunlar desteklemez. Destek sorunları için aygıt satıcınıza başvurmanız gerekir.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Yönlendirici arabirimleri MTU ve TCP MSS ayarları
* ExpressRoute arabirimi MTU 1500, bir yönlendiricideki Ethernet arabirimi için tipik varsayılan MTU olduğu ' dir. Yönlendiriciniz varsayılan olarak farklı bir MTU olmadıkça yönlendirici arabirimi bir değer belirtmek için gerek yoktur.
* Bir Azure VPN ağ geçidi, bir expressroute bağlantı hattı için TCP MSS belirtilmesi gerekmez.

Yönlendirici yapılandırma örnekleri aşağıdaki tüm eşlemeleri için geçerlidir. Gözden geçirme [ExpressRoute eşlemeler](expressroute-circuit-peerings.md) ve [ExpressRoute yönlendirme gereksinimleri](expressroute-routing.md) yönlendirme hakkında daha fazla bilgi.


## <a name="cisco-ios-xe-based-routers"></a>Yönlendiriciler Cisco IOS-XE tabanlı
Bu bölümdeki örnekler IOS XE işletim sistemi ailesi çalıştıran herhangi bir yönlendirici için geçerlidir.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Arabirimleri ve alt arabirimleri yapılandırma
Bir alt arabirim Microsoft'a bağlanmak her yönlendirici eşliği başına gerektirir. Bir alt arabirimi, bir VLAN kimliği veya yığın çiftinin VLAN kimlikleri ve bir IP adresi ile tanımlanabilir.

**Dot1Q arabirim tanımı**

Bu örnek bir alt arabiriminin tek bir VLAN kimliği ile alt arabirim tanımı sağlar VLAN kimliği eşliği başına benzersizdir. IPv4 adresinizin son sekizli her zaman tek sayı olacaktır.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

**QinQ arabirim tanımı**

Bu örnek bir alt arabiriminin iki VLAN kimliği ile alt arabirim tanımı sağlar. Dış VLAN kimliği (s-kullandıysanız etiketi), aynı tüm eşlemeler kalır. İç VLAN kimliği (c-etiketi), eşlemeyi başına benzersizdir. IPv4 adresinizin son sekizli her zaman tek sayı olacaktır.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a>2. EBGP oturumları ayarlama
Her eşleme için Microsoft ile bir BGP oturumu kurmanız gerekir. Aşağıdaki örnek, Microsoft ile bir BGP oturumu oluşturmak sağlar. Sub arabiriminiz için kullanılan IPv4 adresi a.b.c.d olduysa, BGP komşu (Microsoft) IP adresini a.b.c.d+1 olacaktır. BGP komşu'nın IPv4 adresi son sekizlisi her zaman bir çift sayı olacaktır.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. BGP oturumu üzerinden tanıtılan önekler ayarlama
Select öneklerini Microsoft'a yönlendiricinizin yapılandırabilirsiniz. Aşağıdaki örneği kullanarak bunu yapabilirsiniz.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. Rotayı eşler
Rota haritaları kullanabilirsiniz ve ağınıza yayılan filtre önekler için önek listeler. Görevi gerçekleştirmek için aşağıdaki örneği kullanın. Uygun önek listeleri Kurulum olduğundan emin olun.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Juniper MX series yönlendirici
Bu bölümdeki örnekler Juniper MX serisi yönlendiricilerden için geçerlidir.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Arabirimleri ve alt arabirimleri yapılandırma

**Dot1Q arabirim tanımı**

Bu örnek bir alt arabiriminin tek bir VLAN kimliği ile alt arabirim tanımı sağlar VLAN kimliği eşliği başına benzersizdir. IPv4 adresinizin son sekizli her zaman tek sayı olacaktır.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


**QinQ arabirim tanımı**

Bu örnek bir alt arabiriminin iki VLAN kimliği ile alt arabirim tanımı sağlar. Dış VLAN kimliği (s-kullandıysanız etiketi), aynı tüm eşlemeler kalır. İç VLAN kimliği (c-etiketi), eşlemeyi başına benzersizdir. IPv4 adresinizin son sekizli her zaman tek sayı olacaktır.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. EBGP oturumları ayarlama
Her eşleme için Microsoft ile bir BGP oturumu kurmanız gerekir. Aşağıdaki örnek, Microsoft ile bir BGP oturumu oluşturmak sağlar. Sub arabiriminiz için kullanılan IPv4 adresi a.b.c.d olduysa, BGP komşu (Microsoft) IP adresini a.b.c.d+1 olacaktır. BGP komşu'nın IPv4 adresi son sekizlisi her zaman bir çift sayı olacaktır.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. BGP oturumu üzerinden tanıtılan önekler ayarlama
Select öneklerini Microsoft'a yönlendiricinizin yapılandırabilirsiniz. Aşağıdaki örneği kullanarak bunu yapabilirsiniz.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. Rotayı eşler
Rota haritaları kullanabilirsiniz ve ağınıza yayılan filtre önekler için önek listeler. Görevi gerçekleştirmek için aşağıdaki örneği kullanın. Uygun önek listeleri Kurulum olduğundan emin olun.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).

