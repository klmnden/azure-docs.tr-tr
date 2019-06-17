---
title: 'ARP tabloları - ExpressRoute sorun giderme Al: Klasik: Azure | Microsoft Docs'
description: Bu sayfa, bir ExpressRoute bağlantı hattı için - Klasik dağıtım modelinde ARP tablolarını alma yönelik yönergeler sağlar.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 01/30/2017
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 3e49a1da0e8ea83faf5fc5a10d4c01a41d62fa88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60883105"
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a>ARP tablolarını Klasik dağıtım modelinde alma
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klasik](expressroute-troubleshooting-arp-classic.md)
> 
> 

Bu makalede, Azure ExpressRoute bağlantı hattı için Adres Çözümleme Protokolü (ARP) tabloları alma adımlarında size kılavuzluk eder.

> [!IMPORTANT]
> Bu belge, basit sorunları tanılayın ve giderin yardımcı olması için yöneliktir. Microsoft destek için bir değişiklik olacak şekilde tasarlanmamıştır. Aşağıdaki yönergeleri kullanarak sorunu çözemezseniz, bir destek isteği açın [Microsoft Azure Yardım + Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres Çözümleme Protokolü (ARP) ve ARP tabloları
ARP protokolüdür tanımlanan bir katman 2 [RFC 826](https://tools.ietf.org/html/rfc826). ARP bir Ethernet adresi (MAC adresi) bir IP adresine eşlemek için kullanılır.

ARP tablosu, IPv4 adresi ve MAC adresi, belirli bir eşleme için bir eşleme sağlar. Bir ExpressRoute bağlantı hattı eşlemesi için ARP tablosu (birincil ve ikincil) her arabirim için aşağıdaki bilgileri sağlar:

1. Bir şirket içi yönlendirici arabirimi IP adresinin bir MAC adresine eşleme
2. Bir ExpressRoute yönlendirici arabirimi IP adresinin bir MAC adresine eşleme
3. Eşleme yaşı

ARP tablolarını, Katman 2 yapılandırmasını doğrulama ve temel Katman 2 bağlantısı sorunlarını giderme ile yardımcı olabilir.

ARP tablosu, bir örneği verilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Aşağıdaki bölümde, ExpressRoute uç yönlendiricileri tarafından görülen ARP tabloları görüntüleme hakkında bilgi sağlar.

## <a name="prerequisites-for-using-arp-tables"></a>ARP tablolarını kullanma önkoşulları
Devam etmeden önce aşağıdakiler olduğunuzdan emin olun:

* En az bir eşleme ile yapılandırılmış geçerli bir ExpressRoute devresi. Bağlantı hattı, bağlantı sağlayıcı tarafından tam olarak yapılandırılmalıdır. Siz (veya bağlantı sağlayıcınızdan) eşlemeleri (Azure özel, Azure genel veya Microsoft) en az biri bu bağlantı hattındaki yapılandırmanız gerekir.
* (Azure özel, Azure genel ve Microsoft) eşlikleri yapılandırmak için kullanılan IP adresi aralığı. IP adresi ataması örneklerde gözden [ExpressRoute yönlendirme gereksinimleri sayfasındaki](expressroute-routing.md) arabirimlerine, tarafında ve ExpressRoute tarafında IP adreslerinin nasıl eşlendiğini bir anlamak için. Gözden geçirerek eşleme yapılandırması hakkında daha fazla bilgi edinebilirsiniz [ExpressRoute eşlemesi yapılandırma sayfası](expressroute-howto-routing-classic.md).
* Bu IP adresi ile kullanılan arabirimlerin MAC adresleri hakkında bilgi ağ ekip veya bağlantı sağlayıcınızdan.
* Azure (sürüm 1,50 veya sonrası) için en son Windows PowerShell modülü.

## <a name="arp-tables-for-your-expressroute-circuit"></a>ExpressRoute devreniz ARP tablolarını
Bu bölümde her PowerShell kullanarak eşleme türü için ARP tabloları görüntüleme hakkında yönergeler açıklanmaktadır. Devam etmeden önce eşlemesini yapılandırmak üzere, veya bağlantı sağlayıcınızdan gerekir. Her bağlantı hattı (birincil ve ikincil) iki yolu vardır. Her yol için ARP tablosu bağımsız olarak kontrol edebilirsiniz.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tabloları, Azure özel eşleme
Aşağıdaki cmdlet, Azure özel eşleme için ARP tabloları sağlar:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Aşağıdaki yollardan biri örnek çıktısı şöyledir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tabloları, Azure genel eşdüzey hizmet sağlama için:
Aşağıdaki cmdlet, Azure ortak eşleme için tablolar ARP sağlar:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Aşağıdaki yollardan biri örnek çıktısı şöyledir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Aşağıdaki yollardan biri örnek çıktısı şöyledir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft eşlemesi için ARP tabloları
Aşağıdaki cmdlet, Microsoft eşlemesi için ARP tabloları sağlar:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Örnek çıktı aşağıdaki yollardan biri gösterilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Bu bilgileri kullanma
Bir eşleme ARP tablosu, Katman 2 yapılandırma ve bağlantıyla doğrulamak için kullanılabilir. Bu bölümde, ARP tablolarını farklı senaryolarda nasıl görüneceğini genel bir bakış sağlar.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Bir devreyi bir işletimsel (Beklenen) durumda olduğunda ARP tablosu
* ARP tablosu, şirket içi yan geçerli bir IP ve MAC adresi için bir giriş ve Microsoft tarafında için benzer bir giriş vardır.
* Şirket içi IP adresinin son sekizli her zaman tek bir sayıdır.
* Her zaman Microsoft IP adresinin son sekizli bir çift sayı ' dir.
* Aynı MAC adresi üç eşlemenin tamamını (birincil/ikincil) için Microsoft tarafında görünür.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Şirket içi ya da bağlantı sağlayıcısı tarafı sorunları olduğunda olduğunda ARP tablosu
 Yalnızca bir giriş ARP tabloda görüntülenir. Bu, MAC adresini ve Microsoft tarafında kullanılan IP adresi arasındaki eşlemeyi gösterir.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Böyle bir sorunla karşılaşırsanız, bu sorunu çözmek için bağlantı sağlayıcınız ile bir destek isteği açın.
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Microsoft tarafında sorunu yaşadığı zamanları ARP tablosu
* Microsoft tarafında sorunları varsa bir eşdüzey hizmet sağlama için gösterilen bir ARP tablosu görmezsiniz.
* Bir destek isteği açın [Microsoft Azure Yardım + Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Katman 2 bağlantısı ile ilgili bir sorun olduğunu belirtin.

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantı hattı için Katman 3 yapılandırmaları doğrulama:
  * Bir rota BGP oturumları durumunu belirlemek için özetini alın.
  * ExpressRoute hangi ön eklerin tanıtılıp belirlemek için bir rota tablosunu alın.
* Veri aktarımı giriş ve çıkış bayt gözden geçirerek doğrulayın.
* Bir destek isteği açın [Microsoft Azure Yardım + Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.

