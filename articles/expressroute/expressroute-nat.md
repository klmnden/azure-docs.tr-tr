---
title: "ExpressRoute devreleri için NAT gereksinimleri | Microsoft Azure"
description: "Bu sayfada, ExpressRoute bağlantı hatları için NAT’yi yapılandırma ve yönetmeye yönelik ayrıntılı gereksinimler sağlanmıştır."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 867bf936-c851-485f-84c8-d8d6e33fee9f
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: d3de566ff2825ef0c41d88d4a86157dc23d9f46b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="expressroute-nat-requirements"></a>ExpressRoute NAT gereksinimleri
ExpressRoute kullanarak Microsoft bulut hizmetlerine bağlanmak için, NAT’leri ayarlamanız ve yönetmeniz gerekir. Bazı bağlantı sağlayıcıları NAT ayarlama ve yönetimini yönetilen bir hizmet olarak sunar. Bu tür bir hizmet sunulup sunulmadığını öğrenmek için bağlantı sağlayıcınıza başvurun. Aksi durumda, aşağıda açıklanan gereksinimlere uymalısınız. 

Çeşitli yönlendirme etki alanlarına genel bir bakış için [ExpressRoute bağlantı hatları ve yönlendirme etki alanları](expressroute-circuit-peerings.md) sayfasını gözden geçirin. Azure ortak ve Microsoft eşlemesi için ortak IP adresi gereksinimlerini karşılamak için, NAT’nizi ağınız ve Microsoft arasında ayarlamanızı öneririz. Bu bölümde ayarlamanız gereken NAT altyapısının ayrıntılı bir açıklaması sağlanmıştır.

## <a name="nat-requirements-for-azure-public-peering"></a>Azure ortak eşleme için NAT gereksinimleri
Azure ortak eşleme yolu, Azure’da barındırılan tüm hizmetlere ortak IP adresleri üzerinden bağlanmanıza olanak sağlar. Bunlar [ExpessRoute hakkında SSS](expressroute-faqs.md)’de listelenen tüm hizmetleri ve ISV’ler tarafından Microsoft Azure üzerinde barındırılan hizmetleri içerir. 

> [!IMPORTANT]
> Ortak eşleme üzerinden Microsoft Azure hizmetlerine bağlama, her zaman sizin ağınızdan Microsoft ağına doğru başlatılır. Bu nedenle, oturumlar ExpressRoute üzerinden ağınıza Microsoft Azure hizmetlerinden gönderilemez. Bu işlem denenirse, tanıtılan bu IP'lere gönderilen paketler ExpressRoute yerine İnternet kullanır.
> 

Ortak eşlemede Microsoft Azure’u hedefleyen trafiğe, trafik Microsoft ağına girmeden önce geçerli bir ortak IPv4 adresi ile SNAT uygulanmalıdır. Aşağıdaki şekilde NAT’nin yukarıdaki gereksinimi karşılamak için nasıl ayarlanabileceğini gösteren yüksek düzey bir resim sağlanmıştır.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### <a name="nat-ip-pool-and-route-advertisements"></a>NAT IP havuzu ve rota tanıtma
Trafiğin geçerli bir ortak IPv4 adresiyle Azure ortak eşleme yoluna girdiğinden emin olmalısınız. Microsoft’un IPv4 NAT adres havuzu sahipliğini bölgesel bir yönlendirme İnternet kaydı (RIR) veya bir İnternet yönlendirme kaydıyla (IRR) karşılaştırarak doğrulayabilmesi gerekir. Eşlenen AS numarası ve NAT için kullanılan IP adreslerine göre bir denetim gerçekleştirilir. Kayıt defterlerini yönlendirme hakkında bilgi için [ExpressRoute yönlendirme gereksinimleri](expressroute-routing.md) sayfasına bakın.

Bu eşleme aracılığıyla tanıtılan NAT IP öneki için bir uzunluk sınırlaması yoktur. NAT havuzunu izlemeniz ve NAT oturumlarına gerek duyulmadığından emin olmanız gerekir.

> [!IMPORTANT]
> Microsoft'a tanıtılan NAT IP havuzu İnternet'e tanıtılmamalıdır. Bu, diğer Microsoft hizmetlerine bağlantıyı keser.
> 
> 

## <a name="nat-requirements-for-microsoft-peering"></a>Microsoft eşlemesi için NAT gereksinimleri
Microsoft eşleme yolu, Azure ortak eşleme yolu üzerinden desteklenmeyen Microsoft bulut hizmetlerine bağlanmanızı sağlar. Bunlara Exchange Online, SharePoint Online, Skype Kurumsal ve Dynamics 365 gibi Office 365 hizmetleri dahildir. Microsoft, Microsoft eşlemesi üzerinde çift yönlü bağlantıyı desteklemeyi bekliyor. Microsoft bulut hizmetlerini hedefleyen trafiğe, trafik Microsoft ağına girmeden önce geçerli bir ortak IPv4 adresi ile SNAT uygulanmalıdır. [Asimetrik yönlendirmeyi](expressroute-asymmetric-routing.md) önlemek için, Microsoft bulut hizmetlerinden ağınızı hedefleyen trafiğe İnternet ucunuzda SNAT uygulanmalıdır. Aşağıdaki şekilde NAT’nin Microsoft eşlemesi için nasıl ayarlanacağı gösteren yüksek düzey bir resim sağlanmıştır.

![](./media/expressroute-nat/expressroute-nat-microsoft.png) 

### <a name="traffic-originating-from-your-network-destined-to-microsoft"></a>Ağınızdan kaynaklanan ve Microsoft’u hedefleyen trafik
* Trafiğin geçerli bir ortak IPv4 adresiyle Microsoft eşleme yoluna girdiğinden emin olmalısınız. Microsoft’un IPv4 NAT adres havuzu sahibini bölgesel yönlendirme İnternet kaydı (RIR) veya bir İnternet yönlendirme kaydıyla (IRR) karşılaştırarak doğrulayabilmesi gerekir. Eşlenen AS numarası ve NAT için kullanılan IP adreslerine göre bir denetim gerçekleştirilir. Kayıt defterlerini yönlendirme hakkında bilgi için [ExpressRoute yönlendirme gereksinimleri](expressroute-routing.md) sayfasına bakın.
* Azure ortak eşleme kurulumu ve diğer ExpressRoute bağlantı hatları için kullanılan IP adresleri BGP oturumu aracılığıyla Microsoft’a tanıtılmamalıdır. Bu eşleme aracılığıyla tanıtılan NAT IP öneki için bir uzunluk sınırlaması yoktur.
  
  > [!IMPORTANT]
  > Microsoft'a tanıtılan NAT IP havuzu İnternet'e tanıtılmamalıdır. Bu, diğer Microsoft hizmetlerine bağlantıyı keser.
  > 
  > 

### <a name="traffic-originating-from-microsoft-destined-to-your-network"></a>Microsoft’tan kaynaklanan ve ağınızı hedefleyen trafik
* Belirli senaryolar Microsoft’un ağınız içinde barındırılan hizmet uç noktalarına bağlantı başlatmasını gerektirir. Tipik bir senaryo örneği, Office 365’ten ağınızda barındırılan ADFS sunucularına bağlanmaktır. Bu gibi durumlarda, ağınızdan uygun önekleri Microsoft eşlemesine sızdırmanız gerekir. 
* [Asimetrik yönlendirmeyi](expressroute-asymmetric-routing.md) önlemek için ağınızdaki hizmet uç noktalarında, Microsoft trafiğine İnternet ucunuzda SNAT uygulamanız gerekir. ExpressRoute üzerinden alınan bir yol ile eşleşen hedef IP’si olan istekler **ve yanıtlar** her zaman ExpressRoute üzerinden gönderilir. İstek İnternet üzerinden alınıp yanıt ExpressRoute üzerinden gönderiliyorsa asimetrik yönlendirme mevcuttur. Gelen Microsoft trafiğine İnternet ucunda SNAT uygulanması, yanıt trafiğini İnternet ucuna geri dönmeye zorlayarak sorunu çözer.

![ExpressRoute ile asimetrik yönlendirme](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Yönlendirme](expressroute-routing.md) ve [QoS](expressroute-qos.md) için gereksinimlere bakın.
* İş akışı bilgileri için bkz. [ExpressRoute bağlantı hattı sağlama iş akışları ve devre durumları](expressroute-workflows.md).
* ExpressRoute bağlantınızı yapılandırın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-classic.md)

