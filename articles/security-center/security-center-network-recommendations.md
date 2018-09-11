---
title: Azure Güvenlik Merkezi'nde ağınızı koruma | Microsoft Docs
description: Bu belge adresleri yardımcı önerilerini Azure Güvenlik Merkezi'ne Azure ağınızı korumanıza ve güvenlik ilkelerine uygun haberdar olun.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 12c00d6dfac6c9c2a377a8c142118ff6fd0af751
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302281"
---
# <a name="protecting-your-network-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde ağınızı koruma
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur.  Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler), ağ, SQL ve uygulamaları.

Bu makalede ağınıza uygulama önerileri ele alır.  Ağ önerileri merkezi sonraki generation güvenlik duvarları, ağ güvenlik grupları, gelen trafik kuralları yapılandırma ve daha fazla.  Aşağıdaki tabloda, kullanılabilir ağ önerileri ve uygulamanız durumunda her birinin yaptığı anlamanıza yardımcı olması için bir başvuru olarak kullanın.

## <a name="available-network-recommendations"></a>Kullanılabilir ağ önerileri
| Öneri | Açıklama |
| --- | --- |
| [Yeni Nesil Güvenlik Duvarı ekleme](security-center-add-next-generation-firewall.md) |Bir Microsoft iş ortağından, güvenlik korumaları artırmak için bir sonraki Generation Firewall (NGFW) eklemenizi önerir. |
| [Trafiği yalnızca NGFW aracılığıyla yönlendirme](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Önerir, NGFW aracılığıyla sanal makinenize gelen trafiği zorlamak ağ güvenlik grubu (NSG) kurallarını yapılandırın. |
| [Alt ağlar veya sanal makinelerde ağ güvenlik gruplarını etkinleştirme](security-center-enable-network-security-groups.md) |Nsg'leri alt ağlara veya sanal makineleri üzerinde etkinleştirmenizi önerir. |
| [Internet'e yönelik uç noktalarla erişimi sınırlama](security-center-restrict-access-through-internet-facing-endpoints.md) |Nsg'ler için gelen trafik kuralları yapılandırma önerir. |

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde sanal makinelerinizi koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
