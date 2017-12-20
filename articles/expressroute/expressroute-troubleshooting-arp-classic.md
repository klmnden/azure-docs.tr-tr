---
title: "ARP tabloları alınıyor: Klasik: Azure expressroute bağlantı sorunlarını giderme | Microsoft Docs"
description: "Bu sayfayı ARP tablolar için ExpressRoute devresi almak için yönergeler sağlar."
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: b5856acf-03c2-4933-8111-6ce12998d92a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: fcc847b7e30fd55ca759830e0254ab7542e7663e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a>ARP tabloları Klasik dağıtım modelinde alınıyor
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klasik](expressroute-troubleshooting-arp-classic.md)
> 
> 

Bu makalede, Azure ExpressRoute bağlantı hattınız için Adres Çözümleme Protokolü (ARP) tabloları almak için adım adım anlatılmaktadır.

> [!IMPORTANT]
> Bu belge, tanılamak ve basit sorunları gidermenize yardımcı olmak için tasarlanmıştır. Microsoft destek için yenileme olması amaçlanmamıştır. Aşağıdaki yönergeleri kullanarak sorunu çözemiyorsanız, bir destek isteği açın [Microsoft Azure Yardım + Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres Çözümleme Protokolü (ARP) ve ARP tabloları
ARP protokolüdür tanımlanan bir katman 2 [RFC 826](https://tools.ietf.org/html/rfc826). ARP Ethernet adresi (MAC adresi) bir IP adresine eşlemek için kullanılır.

Bir ARP tablosu IPv4 adresi ve belirli bir eşleme için MAC adresi eşlenmesini sağlar. Bir ExpressRoute bağlantı hattı eşlemesi için ARP tablosu (birincil ve ikincil) her arabirim için aşağıdaki bilgileri sağlar:

1. Bir şirket içi yönlendirici arabirimi IP adresi için bir MAC adresi eşleme
2. Bir ExpressRoute yönlendirici arabirimi IP adresi için bir MAC adresi eşleme
3. Eşleme yaşı

ARP tabloları, Katman 2 yapılandırmasını doğrulama ve temel Katman 2 bağlantı sorunlarını giderme konusunda yardımcı olabilir.

ARP tablosu örneği aşağıdadır:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Aşağıdaki bölümde ExpressRoute sınır yönlendiricileri tarafından görülen ARP tabloları görüntüleme hakkında bilgi sağlar.

## <a name="prerequisites-for-using-arp-tables"></a>ARP tablolarını kullanma önkoşulları
Devam etmeden önce aşağıdakileri yapın:

* En az bir eşleme ile yapılandırılmış geçerli bir expressroute bağlantı hattı. Bağlantı hattı bağlantı sağlayıcı tarafından tam olarak yapılandırılması gerekir. Siz (veya bağlantı sağlayıcınız) eşlemeleri (Azure özel, Azure genel ve Microsoft) en az biri bu bağlantı hattındaki yapılandırmanız gerekir.
* (Azure özel, Azure genel ve Microsoft) eşlemeleri yapılandırmak için kullanılan IP adresi aralığı. IP adresi ataması örneklerde gözden [ExpressRoute yönlendirme gereksinimleri sayfa](expressroute-routing.md) arabirimlerine, aise ve ExpressRoute tarafında IP adreslerinin nasıl eşlendiğini bir anlamak için. Eşleme yapılandırmasını hakkındaki bilgileri gözden geçirerek alabilir [ExpressRoute eşleme yapılandırma sayfası](expressroute-howto-routing-classic.md).
* Bu IP adresi ile kullanılan arabirimlerin MAC adresleri hakkında bilgi ağ ekip veya bağlantı sağlayıcınızdan.
* Azure (sürüm 1,50 veya sonrası) için en son Windows PowerShell modülü.

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP tablolar expressroute bağlantı hattı için
Bu bölümde her tür PowerShell kullanarak eşliği için ARP tabloları görüntüleme hakkında yönergeler sağlar. Devam etmeden önce eşlemesini yapılandırmak üzere siz veya bağlantı sağlayıcınız gerekir. Her bağlantı hattı (birincil ve ikincil) iki yolu vardır. Her yol ARP tablosu bağımsız olarak kontrol edebilirsiniz.

### <a name="arp-tables-for-azure-private-peering"></a>Azure özel eşleme için ARP tabloları
Aşağıdaki cmdlet'i ARP Azure özel eşleme için tablolar sağlar:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Örnek çıktı yollarından biri, için aşağıda verilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure ortak eşleme için ARP tablolar:
Aşağıdaki cmdlet'i ARP Azure ortak eşleme için tablolar sağlar:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Örnek çıktı yollarından biri, için aşağıda verilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Örnek çıktı yollarından biri, için aşağıda verilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft eşlemesi için ARP tabloları
Aşağıdaki cmdlet'i ARP Microsoft eşlemesi için tablolar sağlar:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Örnek çıktı yollarından biri, için aşağıda verilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Bu bilgileri kullanma
Bir eşlemenin ARP tablosu, Katman 2 bağlantı ve doğrulamak için kullanılabilir. Bu bölümde, ARP tabloları farklı senaryolarda nasıl göründüğünü genel bir bakış sağlar.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Bir bağlantı hattı bir işlemsel (Beklenen) durumda olduğunda ARP tablosu
* ARP tablosu, şirket içi yan geçerli bir IP ve MAC adresi için bir giriş ve Microsoft tarafı için benzer bir giriş vardır.
* Şirket içi IP adresinin son sekizlisi her zaman tek bir sayıdır.
* Microsoft IP adresinin son sekizlisi her zaman bir çift sayı olup.
* Aynı MAC adresi (birincil/ikincil) üç eşlemenin tamamını Microsoft tarafında görünür.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Şirket içi veya bağlantı sağlayıcı yan sorunları olduğunda olduğunda ARP tablosu
 Yalnızca bir giriş ARP tabloda görüntülenir. MAC adresi ve Microsoft tarafında kullanılan IP adresi arasındaki eşlemeyi gösterir.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Böyle bir sorunla karşılaşırsanız, bunu çözmek için bağlantı sağlayıcınız ile bir destek isteği açın.
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Microsoft yan sorunları olduğunda ARP tablosu
* Microsoft tarafında sorunları varsa bir eşleme için gösterilen bir ARP tablosu görmezsiniz.
* Bir destek isteği açın [Microsoft Azure Yardım + Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Katman 2 bağlantı ile ilgili bir sorun olduğunu belirtin.

## <a name="next-steps"></a>Sonraki adımlar
* Expressroute bağlantı hattı için Katman 3 yapılandırmaları doğrulama:
  * Bir rota BGP oturumlarının durumunu belirlemek için Özet alın.
  * ExpressRoute hangi ön eklerin tanıtılıp belirlemek için bir yol tablosu alın.
* Veri aktarımı bayt giriş ve çıkış gözden geçirerek doğrulayın.
* Bir destek isteği açın [Microsoft Azure Yardım + Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.

