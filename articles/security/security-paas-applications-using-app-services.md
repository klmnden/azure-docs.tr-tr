---
title: PaaS web ve mobil uygulamalarının Azure App Service kullanarak güvenli hale getirme | Microsoft Docs
description: 'Azure App Service güvenliği hakkında PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için en iyi adımları öğrenin. '
services: security
documentationcenter: na
author: techlake
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: terrylan
ms.openlocfilehash: 6e5034d0ff8f14a9fc381f6fd1a214a91ad4d1ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444410"
---
# <a name="best-practices-for-securing-paas-web-and-mobile-applications-using-azure-app-service"></a>PaaS web ve mobil uygulamalarının Azure App Service kullanarak güvenliğini sağlamaya yönelik en iyi yöntemler

Bu makalede ele koleksiyonu [Azure App Service](../app-service/overview.md) PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için en iyi güvenlik uygulamaları. Bu en iyi Azure ile deneyimimizi ve sizin gibi müşteri deneyimleri türetilmiştir.

Azure App Service web uygulamaları ve herhangi bir platform veya cihaz için mobil uygulamalar oluşturmak ve bulutta veya şirket içinde her yerden, veri bağlama sağlayan bir platform-bir hizmet olarak (PaaS) teklifidir. App Service, web ve daha önce ayrı olarak Azure Web siteleri ve Azure mobil hizmetler teslim edilen mobil özelliklerini içerir. Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir tümleşik hizmet olarak App Service özellikleri, Web, mobil ve tümleştirme senaryolarına ilişin zengin.

## <a name="authenticate-through-azure-active-directory-ad"></a>Azure Active Directory (AD) ile kimlik doğrulama
App Service kimlik sağlayıcınız için bir OAuth 2.0 hizmeti sunar. OAuth 2.0 istemci Geliştirici Basitlik, web uygulamaları, Masaüstü uygulamaları ve mobil telefonlar için özel yetkilendirme akışları sağlarken odaklanır. Azure AD OAuth 2.0 web uygulamaları ve mobil erişim yetkisi vermek sağlamak için kullanır. Daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme Azure App Service'te](../app-service/overview-authentication-authorization.md).

## <a name="restrict-access-based-on-role"></a>Rol tabanlı erişimi kısıtlama 
Erişim sınırlama, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunludur. Bilinmesi gerekenler ve en az ayrıcalık güvenlik ilkeleri gibi kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda izinleri atamak için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. Kullanıcılar uygulamalara erişim verme hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi nedir](../role-based-access-control/overview.md).

## <a name="protect-your-keys"></a>Anahtarlarınızı korumak
Güvenlik ne kadar iyi olduğunu fark etmez, abonelik anahtarlarınızın kaybetmeniz durumunda. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Anahtar kasası ile anahtarları ve gizli anahtarları (örneğin, kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları şifrelemenize PFX dosyaları ve parolalar), donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. Key Vault, otomatik olarak yenilenmesi, TLS sertifikalarını yönetmek için de kullanabilirsiniz. Bkz: [Azure anahtar kasası nedir](../key-vault/key-vault-whatis.md) daha fazla bilgi için. 

## <a name="restrict-incoming-source-ip-addresses"></a>Gelen kaynak IP adreslerini kısıtlamak
[App Service ortamları](../app-service/environment/intro.md) ağ güvenlik grupları (Nsg'ler) üzerinden gelen kaynak IP adreslerini kısıtlamak yardımcı olan bir sanal ağ tümleştirme özelliğine sahiptir. Azure sanal ağları (Vnet) ile alışkın değilseniz, birçok Azure kaynaklarınıza erişimi denetleyen internet olmayan, yönlendirilebilir bir ağda yerleştirmenize olanak sağlayan bir özellik budur. Daha fazla bilgi için bkz. [uygulamanızı bir Azure sanal ağ ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için App Service en iyi güvenlik uygulamaları koleksiyonunu kullanıma sunuldu. PaaS dağıtımlarının güvenliğini sağlama hakkında daha fazla bilgi için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](security-paas-deployments.md)
- [Azure PaaS veritabanlarının güvenliğini sağlama](security-paas-applications-using-sql.md)
