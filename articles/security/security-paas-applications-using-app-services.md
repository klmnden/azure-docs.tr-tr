---
title: PaaS web ve mobil uygulamaları Azure uygulama hizmeti kullanarak güvenli hale getirme | Microsoft Docs
description: " Azure uygulama hizmeti güvenliği hakkında bilgi edinme PaaS web ve mobil uygulamaların güvenliğini sağlamaya yönelik en iyi uygulamalar. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: d2e606fe23a3a6eb9d2310b0932ccec8fcfc518c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-service"></a>PaaS web ve mobil uygulamaları Azure uygulama hizmeti kullanarak güvenli hale getirme

Bu makalede, bir koleksiyonu aşağıdakiler ele [Azure App Service](https://azure.microsoft.com/services/app-service/) PaaS web ve mobil uygulamaların güvenliğini sağlamaya yönelik en iyi güvenlik uygulamaları. Bu en iyi uygulamaları Azure ile deneyimi bizim ve kendiniz gibi müşterilerin deneyimleri türetilir.

## <a name="azure-app-service"></a>Azure App Service
[Azure uygulama hizmeti](../app-service/app-service-web-overview.md) teklifini bir PaaS sağlar, web ve tüm platform veya cihazlar için mobil uygulamalar oluşturun ve her yerden, veri Bulut veya şirket içi bağlanmak değil. Uygulama hizmeti web ve daha önce ayrı olarak Azure Web siteleri ve Azure Mobile Services teslim mobil özelliklerini içerir. Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir tümleşik hizmet olarak App Service özellikleri web, mobil ve tümleştirme senaryolarına zengin kümesi getirir.

## <a name="best-practices"></a>En iyi uygulamalar

Uygulama hizmeti kullanırken, bu en iyi uygulamaları izleyin:

- [Azure Active Directory (AD) aracılığıyla kimlik doğrulaması](../app-service/app-service-authentication-overview.md). App Service kimlik sağlayıcınız için OAuth 2.0 hizmeti sağlar. OAuth 2.0 istemci Geliştirici Basitlik, Web uygulamaları, Masaüstü uygulamaları ve cep telefonları için özel yetkilendirme akışları sağlarken odaklanır. Azure AD, web uygulamaları ve mobil erişim yetkisi vermek etkinleştirmek için OAuth 2.0 kullanır.
- Bilmeniz gereken ve en az ayrıcalık güvenlik ilkelerine göre erişimi kısıtlayın. Erişimi kısıtlamak, veri erişimi için güvenlik ilkelerini zorlamak istiyorsanız kuruluşlar için zorunludur. Rol tabanlı erişim denetimi (RBAC), kullanıcılar, gruplar ve uygulamalar belirli bir kapsamda izinleri atamak için kullanılabilir. Kullanıcılar uygulamalara erişim izni verme hakkında daha fazla bilgi için bkz: [access management ile çalışmaya başlama](../role-based-access-control/overview.md).
- Anahtarlarınızı koruyun. Güvenlik ne kadar iyi olduğu önemli değildir, abonelik anahtarlarınızı kaybetmeniz durumunda. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları (kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .PFX dosyaları ve parolalar gibi), donanım güvenlik modülleri tarafından korunan anahtarları kullanarak şifreleyebilirsiniz. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. Bkz: [Azure anahtar kasası](../key-vault/key-vault-whatis.md) daha fazla bilgi için. Anahtar kasası, TLS sertifikalarınızı otomatik yenileme ile yönetmek için de kullanabilirsiniz.
- Gelen kaynak IP adreslerini kısıtlayın. [Uygulama hizmeti ortamı](../app-service/environment/intro.md) ağ güvenlik grupları (Nsg'ler) üzerinden gelen kaynak IP adresleri kısıtlamanıza yardımcı olan bir sanal ağ tümleştirme özelliği vardır. Azure sanal ağlar (Vnet'ler) ile bilginiz yoksa, bu Azure kaynaklarınızı çoğunu erişimini denetleyen bir dışındaki Internet yönlendirilebilir ağı yerleştirmek izin veren bir yetenektir. Daha fazla bilgi için bkz: [uygulamanız bir Azure sanal ağı ile tümleştirmek](../app-service/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede bir koleksiyonuna PaaS web ve mobil uygulamaların güvenliğini sağlamak için uygulama hizmeti en iyi güvenlik uygulamalarını kullanıma sunuldu. PaaS dağıtımları güvenliğini sağlama hakkında daha fazla bilgi için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](security-paas-deployments.md)
- [PaaS web ve mobil uygulamaları Azure SQL Database ve SQL Data Warehouse kullanarak güvenli hale getirme](security-paas-applications-using-sql.md)
