---
title: "ARP tabloları alınıyor: Resource Manager: Azure expressroute bağlantı sorunlarını giderme | Microsoft Docs"
description: "Bu sayfa, ARP tablolar expressroute bağlantı hattı için alma yönergeleri sağlar"
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: a65b1ba2998eae33b3e73bd2492fbbf025eb5946
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>ARP tabloları Resource Manager dağıtım modelinde alınıyor
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klasik](expressroute-troubleshooting-arp-classic.md)
> 
> 

Bu makalede, ARP tablolar expressroute bağlantı hattı için öğrenmek için adım adım anlatılmaktadır. 

> [!IMPORTANT]
> Bu belge, tanılamak ve basit sorunları gidermenize yardımcı olmak için tasarlanmıştır. Microsoft destek için yenileme olması amaçlanmamıştır. İle bir destek bileti açmanız gerekir [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) aşağıda açıklanan yönergeleri kullanarak sorunu çözmeyi erişemiyorsanız.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres Çözümleme Protokolü (ARP) ve ARP tabloları
Adres Çözümleme Protokolü (ARP) tanımlanan bir protokolüdür Katman 2 [RFC 826](https://tools.ietf.org/html/rfc826). ARP Ethernet adresi (MAC adresi) bir IP adresi ile eşleştirmek için kullanılır.

ARP tablosu IPv4 adresi ve belirli bir eşleme için MAC adresi eşlenmesini sağlar. Bir ExpressRoute bağlantı hattı eşlemesi için ARP tablosu (birincil ve ikincil) her arabirim için aşağıdaki bilgileri sağlar

1. Şirket içi yönlendirici arabiriminin IP adresini MAC adresi için eşleme
2. ExpressRoute yönlendirici arabiriminin IP adresini MAC adresi için eşleme
3. Eşleme yaşı

Temel sorun gidermeyi Katman 2 bağlantı sorunları ve Katman 2 yapılandırmasını doğrula ARP tabloları yardımcı olabilir. 

Örnek ARP tablosu: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


Aşağıdaki bölümde ExpressRoute sınır yönlendiricileri tarafından görülen ARP tabloları nasıl görüntüleyebileceğiniz hakkında bilgi sağlar. 

## <a name="prerequisites-for-learning-arp-tables"></a>ARP tabloları öğrenme için Önkoşullar
Daha fazla ilerleme önce aşağıdakilerin yüklenmiş olduğundan emin olun

* En az bir eşleme ile yapılandırılmış bir geçerli expressroute bağlantı hattı. Bağlantı hattı bağlantı sağlayıcı tarafından tam olarak yapılandırılması gerekir. Siz (veya bağlantı sağlayıcınız) eşlemeleri (Azure özel, Azure genel ve Microsoft) en az biri bu bağlantı hattındaki yapılandırmış olmanız gerekir.
* IP adresi aralığı (Azure özel, Azure genel ve Microsoft) eşlemeleri yapılandırmak için kullanılır. IP adresi ataması örneklerde gözden [ExpressRoute yönlendirme gereksinimleri sayfa](expressroute-routing.md) arabirimlerine, tarafında ve ExpressRoute tarafında IP adreslerinin nasıl eşlendiğini bir anlamak için. Gözden geçirerek eşleme yapılandırması hakkında bilgi alabilirsiniz [ExpressRoute eşleme yapılandırma sayfası](expressroute-howto-routing-arm.md).
* Ağ ekibinizin bilgilerinden / bağlantı sağlayıcı arabirimleri MAC adreslerini üzerinde kullanılan bu IP adreslerine.
* En son PowerShell modülü Azure (1,50 ya da daha yeni sürümü) olmalıdır.

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>ARP tablolar expressroute bağlantı hattı için alınıyor
Bu bölüm, PowerShell kullanarak eşlemesi başına ARP tabloları nasıl görüntüleyebileceğiniz hakkında yönergeler sağlar. Daha fazla İleri aşamalara önce eşliği, siz veya bağlantı sağlayıcınız yapılandırmış olmanız gerekir. Her bağlantı hattı (birincil ve ikincil) iki yolu vardır. Her yol ARP tablosu bağımsız olarak kontrol edebilirsiniz.

### <a name="arp-tables-for-azure-private-peering"></a>Azure özel eşleme için ARP tabloları
Azure özel eşleme için aşağıdaki cmdlet'i ARP tablolar sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Örnek çıktı yollarından biri, için aşağıda verilmiştir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure genel eşliği ARP tabloları
Azure ortak eşleme için aşağıdaki cmdlet'i ARP tablolar sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Örnek çıktı yollarından biri, için aşağıda verilmiştir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft eşlemesi için ARP tabloları
Microsoft eşlemesi için aşağıdaki cmdlet'i ARP tablolar sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Örnek çıktı yollarından biri, için aşağıda verilmiştir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Bu bilgileri kullanma
Bir eşlemenin ARP tablosu belirlemek için kullanılan katman 2 bağlantı ve doğrulayın. Bu bölümde, ARP tabloları farklı senaryolarda nasıl görüneceğine genel bir bakış sağlar.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Bir bağlantı hattı işlemsel durum (beklenen durumu) olduğunda ARP tablosu
* ARP tablosu, şirket içi yan geçerli bir IP adresi ve MAC adresi için bir giriş ve Microsoft tarafı için benzer bir giriş sahip olur. 
* Şirket içi IP adresinin son sekizlisi her zaman tek sayı olacaktır.
* Microsoft IP adresi son sekizlisi her zaman bir çift sayı olacaktır.
* Aynı MAC adresi tüm 3 eşlemeleri (birincil / ikincil) için Microsoft tarafında görünür. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP tablosu şirket içi / bağlantı sağlayıcısı yan sorunlar var
Şirket içi sorunları vardır veya bağlantı sağlayıcı ARP tablosu veya şirket içi MAC adresi ya da yalnızca bir giriş görünür görebilirsiniz tamamlanmamış gösterecektir varsa. Bu Microsoft tarafta kullanılan IP adresi ve MAC adresi arasındaki eşlemeyi gösterir. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

or
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Bu tür sorunları hata ayıklamak için bağlantı sağlayıcınız ile bir destek isteği açın. ARP tablosu MAC adreslerinin eşlenen arabirimlerin IP adreslerini yoksa, aşağıdaki bilgileri gözden geçirin:
> 
> 1. Varsa/30 ilk IP adresini MSEE PR ve MSEE arasındaki bağlantıyı MSEE PR. arabirimde kullanılır atanan alt ağ Azure, ikinci IP adresi Msee için her zaman kullanır.
> 2. Hizmet (S-Tag) VLAN etiketlerini ve müşteri (C-Tag) hem de MSEE PR ve MSEE çifti eşleşip eşleşmediğini denetleyin.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>Microsoft yan sorunları olduğunda ARP tablosu
* Microsoft tarafında sorunları varsa bir eşleme için gösterilen bir ARP tablosu görmezsiniz. 
* Bir destek bileti ile açmak [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Katman 2 bağlantı ile ilgili bir sorun olduğunu belirtin. 

## <a name="next-steps"></a>Sonraki Adımlar
* Expressroute bağlantı hattı için Katman 3 yapılandırmaları doğrula
  * Rota BGP oturumlarının durumunu belirlemek için Özet Al 
  * ExpressRoute hangi ön eklerin tanıtılıp belirlemek için yönlendirme tablosunu Al
* Veri aktarımı bayt giriş / çıkış gözden geçirerek doğrula
* Bir destek bileti ile açmak [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.

