---
title: "Azure Güvenlik Merkezi'nde, ağ korumaya | Microsoft Docs"
description: "Yardımcı olacak öneriler Azure Güvenlik Merkezi'nde Azure ağınızı korumak ve güvenlik ilkeleriyle uyumlu olmak bu belge adresleri."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 00b715507a7c3a4d784b800e7bf0c700f6ea6ff1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a>Azure Güvenlik Merkezi, ağınızda koruma
Azure Güvenlik Merkezi, ayrıca Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması sürecinde size rehberlik öneriler oluşturur.  Önerileri Azure kaynak türleri için geçerlidir: sanal ağ, SQL ve uygulamaları makineleri (VM'ler).

Bu makalede, ağınıza olanların önerileri giderir.  Ağ önerileri merkezi İleri nesil güvenlik duvarları, ağ güvenlik grupları, yapılandırma gelen trafiği kuralları ve daha fazla etrafında.  Aşağıdaki tabloda, kullanılabilir ağ önerileri ve onu uygularsanız ne her biri yaptığını anlamanıza yardımcı olması için bir başvuru olarak kullanın.

## <a name="available-network-recommendations"></a>Kullanılabilir ağ önerileri
| Öneri | Açıklama |
| --- | --- |
| [Yeni Nesil Güvenlik Duvarı ekleme](security-center-add-next-generation-firewall.md) |Bir Microsoft iş ortağı güvenlik korumaları artırmak için İleri nesil Güvenlik Duvarı (NGFW) eklemek önerir. |
| [Trafiği yalnızca NGFW aracılığıyla yönlendirme](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Gelen trafik, VM, NGFW üzerinden zorla ağ güvenlik grubu (NSG) kurallarını yapılandırmak önerir. |
| [Ağ güvenlik grupları alt ağları veya sanal makinelerde etkinleştir](security-center-enable-network-security-groups.md) |Nsg'ler alt ağları veya VM'ler etkinleştirmenizi önerir. |
| [Uç nokta Internet'e aracılığıyla erişimi kısıtlama](security-center-restrict-access-through-internet-facing-endpoints.md) |İçin Nsg'ler gelen trafik kurallarını yapılandırmak önerir. |

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türleri için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde, sanal makineleri koruma](security-center-virtual-machine-recommendations.md)
* [Uygulamalarınızı Azure Güvenlik Merkezi'nde koruma](security-center-application-recommendations.md)
* [Azure SQL hizmetinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
