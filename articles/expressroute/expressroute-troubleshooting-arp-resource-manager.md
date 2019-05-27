---
title: 'ARP tablolarını - ExpressRoute sorunlarıyla ilgili sorun giderme -: Azure | Microsoft Docs'
description: Bu sayfa, ARP tablolarını bir ExpressRoute bağlantı hattı için alma yönergelerini sağlar
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 01/30/2017
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 76e242adb07f4e6176bbdc6c03c75950e3732c2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66151575"
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>ARP tablolarını Resource Manager dağıtım modelinde alma
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klasik](expressroute-troubleshooting-arp-classic.md)
> 
> 

Bu makalede ExpressRoute devreniz ARP tablolarını öğrenmek adımlarında size kılavuzluk eder.

> [!IMPORTANT]
> Bu belge, basit sorunları tanılayın ve giderin yardımcı olması için yöneliktir. Microsoft destek için bir değişiklik olacak şekilde tasarlanmamıştır. Bir destek bileti açmanız gerekir [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) aşağıda açıklanan yönergeleri kullanarak sorunu çözmeyi erişemiyorsanız.
> 
> 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres Çözümleme Protokolü (ARP) ve ARP tabloları
Adres Çözümleme Protokolü (ARP) içinde tanımlanan bir katman 2 protokolü olan [RFC 826](https://tools.ietf.org/html/rfc826). ARP Ethernet adresi (MAC adresi) bir IP adresi ile eşleştirmek için kullanılır.

ARP tablosu, IPv4 adresi ve MAC adresi, belirli bir eşleme için bir eşleme sağlar. Bir ExpressRoute bağlantı hattı eşlemesi için ARP tablosu (birincil ve ikincil) her arabirim için aşağıdaki bilgileri sağlar.

1. Şirket içi yönlendirici arabirimi IP adresinin MAC adresine eşleme
2. ExpressRoute yönlendirici arabirimi IP adresinin MAC adresine eşleme
3. Eşleme yaşı

Katman 2 bağlantısı sorunları temel sorun giderme ve ARP tablolarını Katman 2 yapılandırmasını doğrula yardımcı olabilir. 

Örnek ARP tablosu: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


Aşağıdaki bölümde, ExpressRoute uç yönlendiricileri tarafından görülen ARP tablolarını nasıl görüntüleyebileceğiniz hakkında bilgi sağlar. 

## <a name="prerequisites-for-learning-arp-tables"></a>ARP tablolarını öğrenmek için Önkoşullar
Daha fazla ilerleme önce aşağıdakiler olduğunuzdan emin olun

* En az bir eşleme ile yapılandırılmış geçerli bir ExpressRoute bağlantı hattı. Bağlantı hattı, bağlantı sağlayıcı tarafından tam olarak yapılandırılmalıdır. Siz (veya bağlantı sağlayıcınızdan) eşlemeleri (Azure özel, Azure genel ve Microsoft) en az biri bu bağlantı hattındaki yapılandırmış olmanız gerekir.
* IP adresi aralıklarını (Azure özel, Azure genel ve Microsoft) eşlikleri yapılandırmak için kullanılır. IP adresi ataması örneklerde gözden [ExpressRoute yönlendirme gereksinimleri sayfasındaki](expressroute-routing.md) arabirimlerine, tarafında ve ExpressRoute tarafında IP adreslerinin nasıl eşlendiğini bir anlamak için. Gözden geçirerek eşleme yapılandırması hakkında daha fazla bilgi edinebilirsiniz [ExpressRoute eşlemesi yapılandırma sayfası](expressroute-howto-routing-arm.md).
* Ağ takımınızın bilgileri / MAC adreslerini arabirimleri üzerinde bağlantı sağlayıcısı ile bu IP adresleri kullanılır.
* En son PowerShell modülü için Azure (1,50 veya daha yeni sürümü) olmalıdır.

> [!NOTE]
> Katman 3 hizmet sağlayıcısı tarafından sağlanır ve aşağıdaki portal/çıktıda boş ARP tablolarını, portalı Yenile düğmesini kullanarak bağlantı hattı yapılandırma yenileyin. Bu işlem hattınız üzerinde doğru yönlendirme yapılandırması uygulanır. 
>
>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>ARP tablolarını ExpressRoute devreniz alma
Bu bölüm, PowerShell kullanarak eşleme başına ARP tablolarını nasıl görüntüleyebileceğiniz hakkında yönergeler sağlar. Daha fazla ilerlediğini önce eşdüzey hizmet sağlama, siz veya bağlantı sağlayıcınızdan yapılandırmış olmanız gerekir. Her bağlantı hattı (birincil ve ikincil) iki yolu vardır. Her yol için ARP tablosu bağımsız olarak kontrol edebilirsiniz.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tabloları, Azure özel eşleme
Azure özel eşleme için aşağıdaki cmdlet'i ARP tabloları sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secondary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Örnek çıktı aşağıdaki yollardan biri için gösterilir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure genel eşdüzey hizmet sağlama için ARP tabloları
Azure ortak eşleme için aşağıdaki cmdlet'i ARP tabloları sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secondary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Örnek çıktı aşağıdaki yollardan biri için gösterilir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft eşlemesi için ARP tabloları
Microsoft eşlemesi için aşağıdaki cmdlet'i ARP tabloları sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secondary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Örnek çıktı aşağıdaki yollardan biri için gösterilir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Bu bilgileri kullanma
Bir eşleme ARP tablosu belirlemek için kullanılan katman 2 yapılandırma ve bağlantıyla doğrulayın. Bu bölümde, ARP tablolarını farklı senaryolar altında nasıl görüneceğini genel bir bakış sağlar.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>İşlemsel durum (beklenen durum) bir bağlantı hattı olduğunda ARP tablosu
* ARP tablosu, şirket içi yan geçerli bir IP adresi ve MAC adresi için bir giriş ve Microsoft tarafında için benzer bir giriş bulunur. 
* Şirket içi IP adresinin son sekizli her zaman tek bir sayı olur.
* Microsoft IP adresi son sekizli bir çift sayı her zaman olur.
* Aynı MAC adresi, tüm 3 eşlemeler (birincil / ikincil) için Microsoft tarafında görünür. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP tablosu şirket içi / bağlantı sağlayıcısı yan sorunlar var
Şirket içi ile ilgili sorunları vardır veya bağlantı sağlayıcısı, ARP tablosu veya şirket içi MAC adresi ya da yalnızca bir giriş görünür görebilirsiniz tamamlanmamış Göster olur. Bu in Microsoft tarafında kullanılan IP adresi ve MAC adresi arasındaki eşlemeyi gösterir. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

veya
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Bu sorunların hatalarını ayıklamak için bağlantı sağlayıcınız ile bir destek isteği açın. ARP tablosu MAC adresleriyle eşlenen arabirimlerinin IP adresleri yoksa, aşağıdaki bilgileri gözden geçirin:
> 
> 1. İlk IP adresini/30 alt MSEE çekme isteği MSEE arasındaki bağlantı MSEE PR'yi arabirimde kullanılır atanan Azure, her zaman Msee için ikinci IP adresini kullanır.
> 2. Hizmet (S-Tag) VLAN etiketlerini ve müşteri (C-Tag) hem de MSEE çekme isteği ve MSEE çifti eşleşip eşleşmediğini denetleyin.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>Microsoft tarafında sorunu yaşadığı zamanları ARP tablosu
* Microsoft tarafında sorunları varsa bir eşdüzey hizmet sağlama için gösterilen bir ARP tablosu görmezsiniz. 
* Bir destek bileti açın [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Katman 2 bağlantısı ile ilgili bir sorun olduğunu belirtin. 

## <a name="next-steps"></a>Sonraki Adımlar
* ExpressRoute bağlantı hattı için Katman 3 yapılandırmaları doğrula
  * Rota BGP oturumları durumunu belirlemek için özetini alın 
  * ExpressRoute hangi ön eklerin tanıtılıp belirlemek için rota tablosunu alın
* Veri aktarımı / Giden bayt gözden geçirerek doğrulayın.
* Bir destek bileti açın [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.

