---
title: 'Önkoşullar - ExpressRoute: Azure | Microsoft Docs'
description: Bu sayfada bir Azure ExpressRoute bağlantı hattı sipariş etmeden önce karşılanması gereken gereksinimlerin listesi bulunmaktadır. Bu, bir denetim listesi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: afe8d3971a51d57498e3e32b7e1cf5bf5a3263d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60883293"
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute önkoşulları ve denetim listesi
ExpressRoute kullanarak Microsoft bulut hizmetlerine bağlanmak için, aşağıdaki bölümlerde listelenen gereksinimlerin karşılanmış olduğunu doğrulamanız gerekir.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure hesabı
* Geçerli ve etkin bir Microsoft Azure hesabı. ExpressRoute bağlantı hattı kurmak için bu hesap gereklidir. ExpressRoute bağlantı hatları Azure aboneliklerinin içindeki kaynaklardır. Bağlantı, Office 365 hizmetleri ve Dynamics 365 gibi Azure olmayan Microsoft bulut hizmetleriyle sınırlı olsa da bir Azure aboneliği gereklidir.
* Etkin bir Office 365 aboneliği (Office 365 hizmetlerini kullanıyorsanız) Daha fazla bilgi için bu makalede Office 365 belirli gereksinimler bölümüne bakın.

## <a name="connectivity-provider"></a>Bağlantı sağlayıcısı

* Microsoft bulut’a bağlanmak için [ExpressRoute bağlantı ortağı](expressroute-locations.md#partners) ile çalışabilirsiniz. Şirket içi ağınız ve Microsoft arasında bağlantıyı [üç şekilde](expressroute-introduction.md) kurabilirsiniz.
* Sağlayıcınız ExpressRoute bağlantı ortağı değilse bile [bulut değişim sağlayıcısı](expressroute-locations.md#connectivity-through-exchange-providers) yoluyla Microsoft bulut’a bağlanabilirsiniz.

## <a name="network-requirements"></a>Ağ gereksinimleri
* **Her eşleme konumunda yedeklilik**: Microsoft, yedekli BGP oturumlarının Microsoft yönlendiricileri ve her ExpressRoute bağlantı hattı üzerinde eşleme yönlendiricileri arasında kurulu olmasını gerektirir (hatta yalnızca olduğunda [bulut değişimine tek bir fiziksel bağlantı](expressroute-faqs.md#onep2plink)).
* **Yedeklilik, olağanüstü durum kurtarma için**: Microsoft, bir tek hata noktasını önlemek için en az iki ExpressRoute bağlantı hatları eşleme farklı konumlarda ayarlayabilirsiniz kesinlikle önerir.
* **Yönlendirme**: Microsoft Cloud için nasıl bağlandığınıza bağlı olarak, siz veya sağlayıcınız ayarlamak ve yönetmek için BGP oturumları gereken [Yönlendirme etki alanları](expressroute-circuit-peerings.md). Bazı Ethernet bağlantı sağlayıcıları veya Bulut exchange sağlayıcıları olarak BGP Yönetimi sunabilir bir değer ekleme hizmeti.
* **NAT**: Microsoft, yalnızca genel IP adresleri Microsoft eşleme yoluyla kabul eder. Şirket içi ağınızda özel IP adresleri kullanıyorsanız, özel IP adreslerini genel IP adreslerine çevirmek siz veya sağlayıcınız gereken [NAT kullanarak](expressroute-nat.md).
* **QoS**: İşletme için Skype kurumsal'da farklı QoS davranışları gerektiren çeşitli hizmetler (örneğin; ses, video, mesaj) vardır. [Qos gereksinimleri](expressroute-qos.md)ne siz ve sağlayıcınız tarafından uyulmalıdır.
* **Ağ Güvenliği**: ExpressRoute ile Microsoft Bulut'a bağlanırken [ağ güvenliğine](../best-practices-network-security.md) dikkat edin.

## <a name="office-365"></a>Office 365
ExpressRoute'ta Office 365'i etkinleştirmeyi planlıyorsanız, Office 365 gereksinimleri hakkında daha fazla bilgi içeren aşağıdaki belgelere göz atın.

* [Office 365 için ExpressRoute’a Genel Bakış](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Office 365 için ExpressRoute ile Yönlendirme](https://support.office.com/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [ExpressRoute ile yüksek kullanılabilirlik ve yük devretme](https://aka.ms/erhighavailability)
* [Office 365 URL’leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Office 365 için ağ planlama ve performans ayarı](https://support.office.com/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Ağ bant genişliği hesaplayıcıları ve araçları](https://support.office.com/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Office 365’i şirket içi ortamlarla tümleştirme](https://support.office.com/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [Office 365'te ExpressRoute ileri düzey eğitim videoları](https://channel9.msdn.com/series/aer/)

## <a name="dynamics-365"></a>Dynamics 365
ExpressRoute üzerinde Dynamics 365'i etkinleştirmeyi planlıyorsanız Dynamics 365 hakkında daha fazla bilgi için aşağıdaki belgelere göz atın

* [Dynamics 365 ve ExpressRoute teknik incelemesi](https://download.microsoft.com/download/B/2/8/B2896B38-9832-417B-9836-9EF240C0A212/Microsoft%20Dynamics%20365%20and%20ExpressRoute.pdf)
* [Dynamics 365 URL'leri](https://support.microsoft.com/kb/2655102) ve [IP adresi aralıkları](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* ExpressRoute bağlantı sağlayıcısı bulun. Bkz. [ExpressRoute ortakları ve eşleme konumları](expressroute-locations.md).
* [Yönlendirme](expressroute-routing.md), [NAT](expressroute-nat.md) ve [QoS](expressroute-qos.md) için gereksinimlere bakın.
* ExpressRoute bağlantınızı yapılandırın.
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
