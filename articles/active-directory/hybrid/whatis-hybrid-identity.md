---
title: Azure Active Directory ile Active Directory’yi bağlayın. | Microsoft Docs
description: Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 40ac3ca92c65607df056b883608dde60d816143e
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181796"
---
# <a name="hybrid-identity-and-microsofts-identity-solutions"></a>Karma kimlik ve Microsoft'un kimlik çözümleri
Günümüzde işletmeler ve kuruluşlar giderek daha fazla şirket içi ve bulut uygulamalarının bir karışımı haline geliyor.  Gerek şirket içinde gerekse bulutta uygulamaları ve bu uygulamalara erişmesi gereken kullanıcıları olmak, güçlüklerle dolu bir senaryo haline geldi.

Microsoft’un kimlik çözümleri şirket içi ve bulut tabanlı çözümleri birleştirerek konumdan bağımsız olarak tüm kaynaklara kimlik doğrulaması ve yetkilendirme sağlamak üzere tek bir kullanıcı kimliği oluşturur. Buna hibrit kimlik denir.

## <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?

Azure AD Connect, Microsoft'un karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek için tasarladığı araçtır.  Bu sayede kullanıcılarınıza Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik sağlayabilirsiniz.  Aşağıdaki özellikleri sağlar:
    
- [Eşitleme](how-to-connect-sync-whatis.md) - Bu bileşen; kullanıcı, grup ve diğer nesnelerin oluşturulmasından sorumludur. Aynı zamanda şirket içi kullanıcılarınıza ve gruplarınıza ilişkin kimlik bilgilerinin bulut ile eşleşmesini sağlamaktan da sorumludur.  Azure AD ile parola karmalarını eşitlemekten sorumludur.
-   [AD FS ve federasyon tümleştirmesi](how-to-connect-fed-whatis.md) - Federasyon, Azure AD Connect'in isteğe bağlı bir parçası olup şirket içi bir AD FS altyapısını kullanarak karma bir ortamı yapılandırabilir. Ayrıca sertifika yenileme ve ek AD FS sunucusu dağıtımları gibi AD FS yönetim özellikleri de sunar.
-   [Doğrudan Kimlik Doğrulaması](how-to-connect-pta.md) - kullanıcıların aynı parolayı şirket içinde ve bulutta kullanmasına izin veren ancak federe bir ortamın ek altyapısını gerektirmeyen başka bir isteğe bağlı bileşen
-   [Sistem Durumu İzleme](whatis-hybrid-identity-health.md) - Azure AD Connect Health, güçlü izleme ve bu etkinliği görüntülemek için Azure portalda merkezi bir konum sağlayabilir. 


![Azure AD Connect nedir?](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>Azure AD Connect Health nedir?

Azure Active Directory (Azure AD) Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur. Active Directory Federasyon Hizmetleri (AD FS) sunucuları, Azure AD Connect sunucuları (diğer adıyla eşitleme altyapısı), Active Directory etki alanı denetleyicileri gibi anahtar kimlik bileşenlerinizi izleme işlevleri sağlayarak Office 365 ve Microsoft Online Services ile güvenilir bir bağlantı sürdürmenizi sağlar. Aynı zamanda, bu bileşenlere ait önemli veri noktalarını kolayca erişilebilir hale getirir. Böylece, bilgiye dayalı kararlar almak için kullanım bilgilerini ve diğer önemli öngörüleri almayı kolaylaştırır.

Bilgiler, [Azure AD Connect Health Portalı](https://aka.ms/aadconnecthealth)'nda sunulur. Azure AD Connect Health portalında uyarıları, performans izlemeyi, kullanım analizini ve daha fazla bilgiyi görüntüleyebilirsiniz. Azure AD Connect Health, anahtar kimlik bileşenleriniz için aynı yerde tek sistem durumu odağı sağlar.

![Azure AD Connect Health nedir?](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)


Azure AD Connect Health özellikleri arttıkça portal, kimlik odaklı tek bir pano sunar. Kullanıcılarınızın iş yapma olanaklarını artırmanızı sağlayacak daha sağlam, iyi durumda ve tümleşik bir ortam sağlar.


## <a name="why-use-azure-ad-connect"></a>Azure AD Connect neden kullanılır?
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Kullanıcılar ve kuruluşlar aşağıdaki avantajlardan faydalanabilir:

* Kullanıcılar, Office 365 gibi bulut hizmetlerine ve şirket içi uygulamalara erişmek için tek bir kimlik kullanabilir.
* Eşitleme ve oturum açmaya yönelik kolay bir dağıtım deneyimi sağlamak için tek araç.
* Senaryolarınız için en yeni işlevleri sağlar. Azure AD Connect; DirSync ve Azure AD Eşitleme gibi kimlik tümleştirme araçlarının eski sürümlerinin yerine kullanılmaktadır. Daha fazla bilgi için bkz. [Karma Kimlik dizini tümleştirme araçları karşılaştırması](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Azure AD Connect Health neden kullanılır?
Şirket içi dizinlerinizi Azure AD ile tümleştirdiğinizde, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturulur ve kullanıcıların daha üretken olmaları sağlanır. Ancak kullanıcıların herhangi bir cihazdan şirket içi ve buluttaki kaynaklara güvenilir bir şekilde erişmeleri için bu ortamın sağlıklı olup olmadığından emin olma testleri de bu tümleştirmeyle oluşturulur. Azure AD Connect Health, Office 365 veya diğer Azure AD uygulamalarına erişmek için kullanılan şirket içi kimlik altyapınızı izlemeniz ve altyapınıza ilişkin öngörüler elde etmenize yardımcı olur. Her bir şirket içi kimlik sunucunuza bir aracı yüklemek kadar basittir.

### <a name="azure-ad-connect-health-for-ad-fshow-to-connect-health-adfsmd"></a>[AD FS için Azure AD Connect Health](how-to-connect-health-adfs.md)
AD FS'ye ilişkin Azure AD Connect Health Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 üzerinde AD FS 2.0'ı destekler. Ayrıca, extranet erişimi için kimlik doğrulaması desteği sağlayan AD FS ve web uygulaması ara sunucularının izlenmesini de destekler. AD FS için Azure AD Connect Health, çabucak ve kolayca kurulan Sistem Durumu Aracısı ile bir dizi başlıca özellik sunar.

#### <a name="key-benefits-and-best-practices"></a>Başlıca yararları ve en iyi yöntemler

- *Geliştirilmiş güvenlik*
  - [Extranet kilitlemede trendler](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)
  - [Başarısız oturum açma raporu](how-to-connect-health-adfs.md#risky-ip-report-public-preview) 
  - [Gizlilik uyumlu](reference-connect-health-user-privacy.md) olarak    
- *Tüm [kritik ADFS sistem sorunlarında](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services) uyarı alma*
    - Sunucu yapılandırması ve kullanılabilirlik 
    - [Performans ve bağlantı](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs) 
  - Düzenli bakım    
- *Dağıtımı ve yönetimi kolaydır*
  - Hızlı [aracı yükleme](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs) 
  - Aracıyı en sün sürüme otomatik yükseltme 
  - Dakikalar içinde portalda veri    
- *Zengin [kullanım ölçümleri](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)* 
  - En çok kullanılan uygulamalar
  - Ağ konumları ve TCP bağlantısı
  - Sunucu başına belirteç istekleri    
- *Harika kullanıcı deneyimi* 
  - Azure portaldan bir pano tarzında
  - [E-postayla uyarılar](how-to-connect-health-adfs.md#alerts-for-ad-fs)    

#### <a name="feature-highlight"></a>Öne çıkan özellik

*   AD FS ve AD FS ara sunucularının sağlıklı olup olmadıklarını öğrenmek için uyarılarla izleme
*   Kritik uyarılar için e-posta bildirimleri
*   AD FS kapasite planlaması için yararlı olan performans verilerindeki eğilimler
*   Pivotlarla (uygulama, kullanıcı, ağ konumu vb.) AD FS oturum açmaları için kullanım analizi
*   Son IP adresi ile hatalı kullanıcı adı/parola denemesi yapan ilk 50 kullanıcı gibi AD FS raporları
*   Başarısız AD FS oturum açma işlemleri için riskli IP raporu

Daha fazla bilgi için bkz. [Azure AD Connect Health'i AD FS ile kullanma](how-to-connect-health-adfs.md)

### <a name="azure-ad-connect-health-for-synchow-to-connect-health-syncmd"></a>[Eşitleme için Azure AD Connect Health](how-to-connect-health-sync.md)
Eşitleme için Azure AD Connect Health, şirket içi Active Directory'niz ve Azure AD'niz arasında oluşan eşitlemeleri izler ve haklarında bilgi verir. Eşitleme için Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

* Azure AD Connect sunucularının (diğer adıyla Eşitleme Altyapısının) sağlıklı olup olmadığını öğrenmek için uyarılarla izleme
* Kritik uyarılar için e-posta bildirimleri
* Ekleme, güncelleştirme, silme gibi farklı işlemlerdeki eğilimlere ve eşitleme işlemlerine ilişkin gecikme süresi grafikleri de dahil olmak üzere, eşitlemeyle ilgili operasyonel İçgörüler
* Eşitleme özelliklerine hızlı bakış bilgileri ve Azure AD'ye son başarılı aktarma
* Nesne düzeyinde eşitleme hatalarıyla ilgili raporlar için \(Azure AD Premium gerekli değildir\)

Daha fazla bilgi için bkz. [Eşitleme için Azure AD Connect Health'i kullanma](how-to-connect-health-sync.md)

### <a name="azure-ad-connect-health-for-ad-dshow-to-connect-health-addsmd"></a>[AD DS için Azure AD Connect Health](how-to-connect-health-adds.md)
Active Directory Domain Services (AD DS) için Azure AD Connect Health, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016'da yüklü etki alanı denetleyicileri için izleme olanağı sağlar. Durum Aracısı kurulumu, size şirket içi AD DS ortamınızı buluttan izleme olanağı sağlar. AD DS için Azure AD Connect Health aşağıdaki temel işlevler kümesini sağlar:

* Etki alanı denetleyicilerinin iyi durum olmadığı zamanları algılayan, kritik uyarılara ilişkin e-posta bildirimleri ve izleme uyarıları
* Etki alanı denetleyicilerinizin durumunu ve işlem durumunu hızlıca görüntülemenizi sağlayan Etki Alanı Denetleyicileri panosu
* Hatalar algılandığında sorun giderme kılavuzlarının bağlantılarını ve en son çoğaltma bilgilerini içeren Çoğaltma Durumu panosu
* Sorun giderme ve izleme için gerekli olan popüler performans sayaçlarının performans veri grafiklerine her yerden hızlı erişim

Daha fazla bilgi için bkz. [Azure AD Connect Health'i AD DS ile kullanma](how-to-connect-health-adds.md)

## <a name="next-steps"></a>Sonraki Adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)
- [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md)|
- [Doğrudan kimlik doğrulama](how-to-connect-pta.md)
- [Azure AD Connect ve federasyon](how-to-connect-fed-whatis.md)
- [Azure AD Connect Health aracılarını yükleme](how-to-connect-health-agent-install.md) 
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
- [Sürüm geçmişi](reference-connect-version-history.md)
- [Dizin tümleştirmesi araçlarının karşılaştırılması](plan-hybrid-identity-design-considerations-tools-comparison.md)
- [Azure AD Connect ile ilgili SSS](reference-connect-faq.md)









