---
title: Azure ExpressRoute bağlantı hatları ve Yönlendirme etki alanları | Microsoft Docs
description: Bu sayfa ExpressRoute bağlantı hatları ve Yönlendirme etki alanları genel bir bakış sağlar.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: ganesr,cherylmc
ms.openlocfilehash: 0e060e67d615f0d6aa8ca6cbe305670956ac3faf
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="expressroute-circuits-and-routing-domains"></a>ExpressRoute bağlantı hatları ve Yönlendirme etki alanları
 Sipariş gerekir bir *expressroute bağlantı hattı* şirket içi altyapınızı bağlantı sağlayıcı Microsoft'a bağlanmak için. Aşağıdaki şekilde bir mantıksal temsilini WAN ve Microsoft arasında bağlantı gösterilmektedir.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>ExpressRoute bağlantı hatları
Bir *expressroute bağlantı hattı* şirket içi altyapınızı ve bağlantı sağlayıcı üzerinden Microsoft bulut hizmetleri arasında mantıksal bağlantıyı temsil eder. Birden çok ExpressRoute bağlantı hatları sıralayabilirsiniz. Her bağlantı hattı, aynı veya farklı bölgelerde olabilir ve şirketinizde farklı bağlantı sağlayıcıları üzerinden bağlanır. 

ExpressRoute bağlantı hatları için herhangi bir fiziksel varlık eşleme yok. Bir bağlantı hattı benzersiz GUID bir hizmet anahtarı (s-anahtarı) adlı bir standart olarak tanımlanır. Hizmet anahtarı yalnızca Microsoft, bağlantı sağlayıcısı ve sizin arasında alınıp bilgidir. S anahtarı güvenlik amacıyla bir gizlilik değil. Bir expressroute bağlantı hattı ve s anahtar arasındaki 1:1 eşleme yoktur.

Bir expressroute bağlantı hattı en fazla üç bağımsız eşlemenin sahip olabilir: Azure genel, Azure özel ve Microsoft. Her eşleme bağımsız BGP çiftinin her bunların nedenle yüksek kullanılabilirlik için yapılandırılmış oturumdur. 1: n yoktur (1 < = N < = 3) arasında bir expressroute bağlantı hattı eşlemesi ve Yönlendirme etki alanları. Bir expressroute bağlantı hattı herhangi biri, iki veya üç eşlemenin expressroute bağlantı hattı etkin tamamını olabilir.

Her bağlantı hattı (50 MB/sn, 100 MB/sn, 200 MB/sn, 500 MB/sn, 1 GB/sn, 10 GB/sn) sabit bir bant genişliğine sahiptir ve bağlantı sağlayıcı ve eşleme konumu eşlendi. Seçtiğiniz bant genişliği bağlantı hattı için tüm eşlemeler arasında paylaşılır. 

### <a name="quotas-limits-and-limitations"></a>Kotalar, sınırlar ve sınırlamalar
Varsayılan kotalar ve sınırlar her expressroute bağlantı hattı için geçerlidir. Başvurmak [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md) Kotalar hakkında güncel bilgi sayfası.

## <a name="expressroute-routing-domains"></a>ExpressRoute yönlendirme etki alanları
Bir expressroute bağlantı hattı kendisiyle ilişkili birden fazla Yönlendirme etki alanları sahiptir: Azure genel, Azure özel ve Microsoft. Her bir yönlendirme etki alanları aynı yönlendiriciler çifti üzerinde yapılandırılmış (etkin-etkin ya da yük paylaşma yapılandırma) yüksek kullanılabilirlik için. Azure Hizmetleri olarak kategorilere *Azure genel* ve *Azure özel* IP adresleme şemasını temsil etmek için.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="azure-private-peering"></a>Azure özel eşleme
Azure işlem Hizmetleri, yani sanal makineler (Iaas) ve bir sanal ağ içindeki dağıtılan bulut hizmetlerini (PaaS) üzerinden özel eşleme etki alanına bağlanabilir. Özel Eşleme etki alanı çekirdek ağınıza Microsoft Azure için güvenilen bir uzantısı olarak kabul edilir. Azure sanal ağlar (Vnet'ler) ve çekirdek ağınızı arasında çift yönlü bağlantı ayarlayabilirsiniz. Bu eşleme, sanal makinelere bağlanmak ve özel IP adresleri doğrudan Services'de bulut sağlar.  

Özel Eşleme etki alanı için birden fazla sanal ağ bağlanabilir. Gözden geçirme [SSS sayfasını](expressroute-faqs.md) sınırlar ve sınırlamalar hakkında bilgi için. Ziyaret ettiğiniz [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md) sınırları hakkında güncel bilgi sayfası.  Başvurmak [yönlendirme](expressroute-routing.md) yönlendirme yapılandırması hakkında ayrıntılı bilgi için sayfa.

### <a name="azure-public-peering"></a>Azure ortak eşleme

> [!IMPORTANT]
> Tüm Azure PaaS hizmetlerine Microsoft eşlemesi üzerinden de erişilebilir. Microsoft eşlemesi oluşturmanızı ve Azure PaaS hizmetlerine Microsoft eşlemesi üzerinden bağlanmanızı öneririz.  
>   


Azure Storage, SQL veritabanları ve Web siteleri gibi hizmetleri ortak IP adreslerini sunulur. Özel olarak, bulut hizmetleriyle ortak eşleme Yönlendirme etki alanı VIP'ler dahil olmak üzere, genel IP adresleri barındırılan hizmetlere bağlanabilir. Ortak eşleme etki alanı için çevre ağınız bağlanmak ve ortak IP adresleri üzerindeki tüm Azure hizmetlerine WAN'ınız Internet üzerinden bağlanmak zorunda kalmadan bağlanın. 

Bağlantı, her zaman Microsoft Azure hizmetlerini WAN'ınız başlatılır. Microsoft Azure Hizmetleri, ağınızda bu yönlendirme etki alanı aracılığıyla bağlantılar başlatamaz olmaz. Ortak eşleme etkinleştirildikten sonra tüm Azure hizmetlerine bağlanabilir. Biz, seçmeli olarak biz yolları tanıtma Hizmetleri seçmenize olanak izin vermez.

Yalnızca gereksinim duyduğunuz yolların tüketmeye ağınızın içinde özel yol filtreleri tanımlayabilirsiniz. Başvurmak [yönlendirme](expressroute-routing.md) yönlendirme yapılandırması hakkında ayrıntılı bilgi için sayfa. 

Desteklenen ortak eşleme Yönlendirme etki alanı hizmetleri hakkında daha fazla bilgi için bkz: [SSS](expressroute-faqs.md).

### <a name="microsoft-peering"></a>Microsoft eşlemesi
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Microsoft eşleme aracılığıyla Microsoft Çevrimiçi Hizmetleri (Office 365, Dynamics 365 ve Azure PaaS hizmetler) için bağlantı olur. Biz, WAN ve Microsoft bulut hizmetleriyle Microsoft eşliği Yönlendirme etki alanı arasında çift yönlü bağlantı etkinleştirin. Yalnızca siz veya bağlantı sağlayıcınız tarafından sahip olunan genel IP adresleri üzerinden Microsoft bulut hizmetlerine bağlanmak ve tüm tanımlı kurallara uymalıdır. Daha fazla bilgi için bkz: [ExpressRoute önkoşulları](expressroute-prerequisites.md) sayfası.

Bkz: [SSS sayfasını](expressroute-faqs.md) daha fazla bilgi desteklenen Hizmetleri, maliyetleri ve yapılandırma ayrıntıları. Bkz: [ExpressRoute konumları](expressroute-locations.md) Microsoft eşleme desteği sunan bağlantı sağlayıcıları listesi hakkında bilgi için sayfa.

## <a name="routing-domain-comparison"></a>Yönlendirme etki alanı karşılaştırma
Aşağıdaki tabloda, üç yönlendirme etki alanları karşılaştırılır:

|  | **Özel eşleme** | **Ortak eşleme** (yeni oluşturmaları için kullanım dışı) | **Microsoft eşlemesi** |
| --- | --- | --- | --- |
| **Maks. eşleme başına desteklenen # önekleri** |Varsayılan olarak, ExpressRoute Premium 10.000 4000 |200 |200 |
| **Desteklenen IP adres aralıkları** |WAN'ınız dahilinde geçerli bir IP adresi. |Siz veya bağlantı sağlayıcınız tarafından sahip olunan genel IP adresleri. |Siz veya bağlantı sağlayıcınız tarafından sahip olunan genel IP adresleri. |
| **Gereksinim sayısı olarak** |Özel ve ortak AS numaraları. Ortak sahibi kullanmayı seçerseniz bir sayı olarak. |Özel ve ortak AS numaraları. Ancak, genel IP adresleri sahipliğini kanıtlamanız gerekir. |Özel ve ortak AS numaraları. Ancak, genel IP adresleri sahipliğini kanıtlamanız gerekir. |
| **Desteklenen IP protokolleri**| IPv4 | IPv4 | IPv4, IPv6 |
| **Yönlendirme arabirimi IP adresleri** |RFC1918 ve genel IP adresleri |Genel IP adresleri kayıt defterlerini yönlendirme kaydettiniz. |Genel IP adresleri kayıt defterlerini yönlendirme kaydettiniz. |
| **MD5 karma değeri desteği** |Evet |Evet |Evet |



Bir veya daha fazla Yönlendirme etki alanları, expressroute bağlantı hattı bir parçası olarak etkinleştirmeyi seçebilirsiniz. Tek bir yönlendirme etki alanına birleştirmek istiyorsanız aynı VPN put tüm Yönlendirme etki alanları sahip olmayı seçebilirsiniz. Ayrıca bunları farklı yönlendirme etki alanları, diyagrama benzer koyabilirsiniz. Önerilen özel eşleme çekirdek ağı'na doğrudan bağlı ve genel ve Microsoft eşleme bağlantıları çevre ağınız bağlı yapılandırmadır.

Tüm üç eşliği oturumlarını tercih ediyorsanız, BGP oturumları (her eşleme türü için bir çift) üç çiftleri bulunmalıdır. BGP oturumu çiftleri yüksek oranda kullanılabilir bir bağlantı sağlayın. Katman 2 bağlantı sağlayıcıları bağlanıyorsanız, yapılandırma ve yönlendirme yönetmekten sorumlu. İnceleyerek daha fazla bilgi edinebilirsiniz [iş akışları](expressroute-workflows.md) ExpressRoute ayarlama.

## <a name="next-steps"></a>Sonraki adımlar
* Bir hizmet sağlayıcı bulun. Bkz: [ExpressRoute hizmet sağlayıcıları ve konumları](expressroute-locations.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).
* ExpressRoute bağlantınızı yapılandırın.
  * [ExpressRoute devreleri oluşturup yönetme](expressroute-howto-circuit-portal-resource-manager.md)
  * [ExpressRoute işlem hatları için yönlendirmeyi (eşleme) yapılandırma](expressroute-howto-routing-portal-resource-manager.md)

