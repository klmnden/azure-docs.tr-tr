---
title: Azure AD Connect ve Connect Health nedir? | Microsoft Docs
description: Eşitleme ve Azure AD ile şirket içi ortamınızı izlemek için kullanılan araçları açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48b81d508711f35a75efe1c93fe0a5556c5bb960
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784469"
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

AD FS'ye ilişkin Azure AD Connect Health Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 üzerinde AD FS 2.0'ı destekler. Ayrıca, extranet erişimi için kimlik doğrulaması desteği sağlayan AD FS ve web uygulaması ara sunucularının izlenmesini de destekler. AD FS için Azure AD Connect Health, çabucak ve kolayca kurulan Sistem Durumu Aracısı ile bir dizi başlıca özellik sunar.

Başlıca yararları ve en iyi uygulamalar:

|Önemli Avantajlar|En İyi Uygulamalar|
|-----|-----|
|Gelişmiş güvenlik|[Extranet kilitlemede trendler](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)</br>[Başarısız oturum açma raporu](how-to-connect-health-adfs-risky-ip.md)</br>[Gizlilik uyumlu](reference-connect-health-user-privacy.md)|
|Hakkında uyarı alın [tüm önemli ADFS sistem sorunları](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services)|Sunucu yapılandırması ve kullanılabilirlik</br>[Performans ve bağlantı](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs)</br>Düzenli bakım|
|Kurulumu ve yönetimi kolay|[Hızlı bir aracı yüklemesi](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)</br>Aracıyı en sün sürüme otomatik yükseltme</br>Dakikalar içinde portalda veri|
Zengin [kullanım ölçümleri](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)|En çok kullanılan uygulamalar</br>Ağ konumları ve TCP bağlantısı</br>Sunucu başına belirteç istekleri|
|Harika kullanıcı deneyimini|Azure portaldan bir pano tarzında</br>[E-postayla uyarılar](how-to-connect-health-adfs.md#alerts-for-ad-fs)|


## <a name="license-requirements-for-using-azure-ad-connect"></a>Azure AD Connect kullanarak için lisans gereksinimleri

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]




## <a name="next-steps"></a>Sonraki adımlar

- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)
- [Azure AD Connect Health aracılarını yükleme](how-to-connect-health-agent-install.md) 
