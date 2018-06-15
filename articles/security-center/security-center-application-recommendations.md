---
title: Uygulamalarınızı Azure Güvenlik Merkezi'nde koruma | Microsoft Docs
description: Yardımcı olacak öneriler Azure Güvenlik Merkezi'nde Azure uygulamalarınızı korumaya ve güvenlik ilkeleriyle uyumlu olmak bu belge adresleri.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: b5fc7a9e-24b1-415f-b3b5-62a53f5dd424
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: terrylan
ms.openlocfilehash: b5bc02082fa8c2681aa67910729870921fec329d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866271"
---
# <a name="protecting-your-applications-in-azure-security-center"></a>Uygulamalarınızı Azure Güvenlik Merkezi'nde koruma
Azure Güvenlik Merkezi, ayrıca Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması sürecinde size rehberlik öneriler oluşturur.  Önerileri Azure kaynak türleri için geçerlidir: sanal ağ, SQL ve uygulamaları makineleri (VM'ler).

Bu makalede, uygulamalar için geçerli önerileri giderir.  Uygulama önerileri merkezi bir web uygulaması güvenlik duvarı dağıtımını etrafında.  Aşağıdaki tabloda, kullanılabilir uygulama önerileri ve onu uygularsanız ne her biri yaptığını anlamanıza yardımcı olması için bir başvuru olarak kullanın.

## <a name="available-application-recommendations"></a>Kullanılabilir uygulama önerileri
| Öneri | Açıklama |
| --- | --- |
| [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md) |Web uç noktaları için web uygulaması Güvenlik Duvarı (WAF) dağıtmak önerir. WAF öneri açık gelen web bağlantı noktaları (80,443) içeren bir ilişkili ağ güvenlik grubu olan tüm genel kullanıma yönelik IP'si için (örnek düzeyinde IP veya yük dengeli IP) gösterilir.</br></br>Güvenlik Merkezi, uygulama hizmeti ortamı ve sanal makineler üzerinde web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir WAF sağlamasını önerir. Uygulama hizmeti ortamı (ana) olan bir [Premium](https://azure.microsoft.com/pricing/details/app-service/) hizmet planı seçeneği Azure App Service, güvenli bir şekilde Azure App Service uygulamalarını çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar. Ana hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı belgeleri](../app-service/environment/intro.md).</br></br>Varolan WAF dağıtımlarınız için bu uygulamaları ekleyerek, birden çok web uygulamasına Güvenlik Merkezi'nde koruyabilirsiniz. |
| [Uygulama korumasını sonlandırma](security-center-add-web-application-firewall.md#finalize-application-protection) |Bir WAF yapılandırmasını tamamlamak için trafiği WAF Gereci yönlendirilmesi gerekir. Bu öneri aşağıdaki gerekli Kurulum değişiklikleri tamamlar. |

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türleri için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde, sanal makineleri koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi, ağınızda koruma](security-center-network-recommendations.md)
* [Azure SQL hizmetinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
