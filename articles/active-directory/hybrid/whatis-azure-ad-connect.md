---
title: Azure AD Connect ve Connect Health nedir? | Microsoft Docs
description: Eşitleme ve Azure AD ile şirket içi ortamınızı izlemek için kullanılan araçları açıklar.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2fbd6edb02dfc4080d7692e43324a5b3981091f9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109484"
---
# <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?

Azure AD Connect, Microsoft'un karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek için tasarladığı araçtır.  Aşağıdaki özellikleri sağlar:
    
- [Parola Karması eşitleme](whatis-phs.md) -bir kullanıcı bir karma eşitleyen bir oturum açma yöntemi şirket içi Azure AD ile AD parola.
- [Geçişli kimlik doğrulaması](how-to-connect-pta.md) -aynı şirket içi parolayı kullanmasına olanak tanır bir oturum açma yöntemi içindeki ve buluttaki bir Federasyon ortamının ek altyapı gerektirmez, ancak.
- [Federasyon tümleştirme](how-to-connect-fed-whatis.md) -Federasyon, Azure AD Connect, isteğe bağlı bir parçasıdır ve şirket içi kullanarak karma bir ortamda yapılandırmak için kullanılan AD FS altyapısı. Ayrıca, sertifika yenileme ve ek AD FS sunucusu dağıtımları gibi AD FS yönetimi özellikleri sağlar.
- [Eşitleme](how-to-connect-sync-whatis.md) - kullanıcıları, grupları ve diğer nesnelerin oluşturulmasından sorumludur.  Yanı, emin kimlik bilgileri, şirket içi kullanıcılar ve gruplar için yaparak bulut ile eşleşmesini.  Bu eşitleme, parola karmaları da içerir.
-   [Sistem Durumu İzleme](whatis-hybrid-identity-health.md) - Azure AD Connect Health, güçlü izleme ve bu etkinliği görüntülemek için Azure portalda merkezi bir konum sağlayabilir. 


![Azure AD Connect nedir?](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>Azure AD Connect Health nedir?

Azure Active Directory (Azure AD) Connect Health, şirket içi kimlik altyapınızı güçlü izleme sağlar. Office 365 ve Microsoft Online Services ile güvenilir bir bağlantı sürdürmenizi sağlar.  Anahtar kimlik bileşenlerinizi izleme işlevleri sağlayarak bu güvenilirlik elde edilir. Ayrıca, bu bileşenler hakkındaki anahtar veri noktalarını kolayca erişilebilir hale getirir.

Bilgiler, [Azure AD Connect Health Portalı](https://aka.ms/aadconnecthealth)'nda sunulur. Azure AD Connect Health portalında uyarıları, performans izlemeyi, kullanım analizi ve diğer bilgileri görüntülemek için kullanın. Azure AD Connect Health, anahtar kimlik bileşenleriniz için aynı yerde tek sistem durumu odağı sağlar.

![Azure AD Connect Health nedir?](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)

## <a name="why-use-azure-ad-connect"></a>Azure AD Connect neden kullanılır?
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Kullanıcılar ve kuruluşlar avantajlarından yararlanabilirsiniz:

* Kullanıcılar, Office 365 gibi bulut hizmetlerine ve şirket içi uygulamalara erişmek için tek bir kimlik kullanabilir.
* Eşitleme ve oturum açmaya yönelik kolay bir dağıtım deneyimi sağlamak için tek araç.
* Senaryolarınız için en yeni işlevleri sağlar. Azure AD Connect; DirSync ve Azure AD Eşitleme gibi kimlik tümleştirme araçlarının eski sürümlerinin yerine kullanılmaktadır. Daha fazla bilgi için bkz. [Karma Kimlik dizini tümleştirme araçları karşılaştırması](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Azure AD Connect Health neden kullanılır?
Hem bulut hem de şirket içi kaynaklara erişmek için ortak bir kimlik olduğundan, Azure AD ile kullanıcılarınızın daha üretken olmaları sağlanır. Kullanıcıları bu kaynaklara erişebilmesi için güvenilir, ortamıdır sağlamak zor hale gelir.  Azure AD Connect Health, izleme ve bu nedenle bu ortam güvenilirliğini sağlayarak şirket içi kimlik altyapınızı içgörüler edinin yardımcı olur. Her bir şirket içi kimlik sunucunuza bir aracı yüklemek kadar basittir.


## <a name="next-steps"></a>Sonraki adımlar

- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)
- [Azure AD Connect Health aracılarını yükleme](how-to-connect-health-agent-install.md) 
