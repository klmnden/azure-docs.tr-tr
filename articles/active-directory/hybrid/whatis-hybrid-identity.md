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
ms.date: 11/02/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2aca42c23cc213d5d7e451105052d5d5d697b77d
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979480"
---
# <a name="hybrid-identity-and-microsoft-identity-solutions"></a>Karma kimlik ve Microsoft kimlik çözümleri
[Microsoft Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) karma kimlik çözümleri, şirket içi dizin nesnelerini Azure AD ile eşitlerken kullanıcı yönetimini şirket içi ortamda gerçekleştirmeye devam etmenizi sağlar. Şirket içi Windows Server Active Directory ortamınızı Azure AD ile eşitleme planlaması yaparken vermeniz gereken ilk karar, yönetilen kimlikler veya federasyon kimliği kullanımı konusunda olacaktır. 

- **Yönetilen kimlikler** - Şirket içi bir Active Directory’den eşitlenen ve kullanıcının kimlik doğrulamasının Azure tarafından yönetildiği kullanıcı hesapları ve grupları.   
- **Federasyon kimlikleri**, kullanıcı kimlik doğrulamasını Azure'dan ayırıp güvenilen ve şirket içi ortamda bulunan kimlik sağlayıcısını yetkilendirerek kullanıcılar üzerinde daha fazla denetim sahibi olmanızı sağlar. 

Karma kimlik yapılandırması için kullanılabilecek birçok seçenek mevcuttur. Kuruluşunuzun ihtiyaçlarına en uygun olan kimlik modeline karar verirken zaman, var olan altyapı, karmaşıklık düzeyi ve maliyetler üzerine de düşünmeniz gerekir. Bu faktörler her kuruluş için farklıdır ve zaman içinde değişiklik gösterebilir. Ancak gereksinimleriniz değiştikçe farklı bir kimlik modeline geçiş yapma esnekliğine de sahip olursunuz.

## <a name="managed-identity"></a>Yönetilen kimlik 

Yönetilen kimlik, şirket içi dizin nesnelerini (kullanıcılar ve gruplar) Azure AD ile eşitlemenin en kolay yoludur. 

![Eşitlenen karma kimlik](./media/whatis-hybrid-identity/managed.png)

Yönetilen kimlik en kolay ve en hızlı yöntem olsa da kullanıcılarınızın bulut tabanlı kaynaklar için ayrı parola kullanmasını gerektirir. Bunu önlemek için (isteğe bağlı olarak) [kullanıcı parolalarının karmasını](how-to-connect-password-hash-synchronization.md) Azure AD dizininizle eşitleyebilirsiniz. Parola karmalarını eşitlemek, kullanıcıların bulut tabanlı kuruluş kaynaklarında şirket içi ortamda kullandıkları parolayla oturum açmasını sağlar. Azure AD Connect, şirket içi dizininizdeki değişiklikleri düzenli olarak denetler ve Azure AD dizininizin eşitlenmesini sağlar. Şirket içi Active Directory ortamında bir kullanıcı özniteliği veya parolası değiştiğinde ilgili veriler Azure AD'de de otomatik olarak güncelleştirilir. 

Kullanıcılarının Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklarda oturum açmasına ihtiyaç duyan çoğu kuruluş için varsayılan parola karması eşitleme seçeneği önerilir. Bu kullanım size uygun değilse doğrudan kimlik doğrulama ile AD FS arasında bir seçim yapmanız gerekir.

> [!TIP]
> Kullanıcı parolaları, şirket içi Windows Server Active Directory ortamında gerçek kullanıcı parolasını temsil eden bir karma kod biçiminde depolanır. Karma değeri, tek yönlü bir matematiksel işlev (karma algoritması) sonucunda elde edilir. Tek yönlü işlevin sonucunu, parolanın düz metin sürümüne döndürmek mümkün değildir. Parola karması kullanarak şirket içi ağınızda oturum açamazsınız. Parolaları eşitleme seçeneğini kullandığınızda Azure AD Connect şirket içi Active Directory ortamındaki parola karmalarını ayıklar ve bu karmaları Azure AD ile eşitlemeden önce ek güvenlik işlemlerine tabi tutar. Parola karması eşitlemeyi, parola geri yazma özelliğiyle birlikte kullanarak Azure AD'de self servis parola sıfırlama işlevinin kullanılmasını sağlayabilirsiniz. Ayrıca kuruluş ağına bağlanmış ve etki alanına katılmış olan bilgisayarlardaki kullanıcılar için çoklu oturum açma (SSO) özelliğini etkinleştirebilirsiniz. Çoklu oturum açma özelliği etkinleştirildiğinde kullanıcılar yalnızca bir kullanıcı adı ile bulut kaynaklarına güvenle erişim sağlayabilir. 
>

## <a name="pass-through-authentication"></a>Doğrudan kimlik doğrulama

[Azure AD doğrudan kimlik doğrulama](how-to-connect-pta.md) özelliği, şirket içi Active Directory ortamınızı kullanarak Azure AD tabanlı hizmetler için basit bir parola doğrulama çözümü sunar. Kuruluşunuzun güvenlik ve uyumluluk ilkeleri, karma biçiminde olsa dahi kullanıcı parolalarının başka bir yere gönderilmesine izin vermiyorsa ve yalnızca etki alanına katılmış olan cihazlar için masaüstü SSO desteği sunmanız gerekiyorsa doğrudan kimlik doğrulama özelliğini değerlendirmeniz önerilir. Doğrudan kimlik doğrulama için DMZ alanında dağıtım yapılması gerekmez. Bu da AD FS ile kıyaslandığında dağıtım altyapısının daha kolay olmasını sağlar. Kullanıcılar Azure AD'de oturum açtığında bu kimlik doğrulama yöntemi parolaları doğrudan şirket için Active Directory dizininizde doğrular.

![Doğrudan kimlik doğrulama](./media/whatis-hybrid-identity/pass-through-authentication.png)

Doğrudan kimlik doğrulama ile karmaşık ağ altyapısı kurmanıza ve şirket içi parolaları bulutta depolamanıza gerek yoktur. Doğrudan kimlik doğrulama özelliği çoklu oturum açma ile birlikte kullanıldığında Azure AD'de ve diğer bulut hizmetlerinde oturum açmak için tamamen tümleşik bir deneyim sunar.

Doğrudan kimlik doğrulama, Azure AD Connect ile yapılandırılır ve bunun için parola doğrulama isteklerini dinleyen tek bir şirket içi aracı kullanılır. Yüksek erişilebilirlik sağlama ve yük dengeleme amacıyla aracı birden fazla makineye kolayca dağıtılabilir. Tüm iletişimler yalnızca dışarı doğru gerçekleştirildiğinden bağlayıcının DMZ ortama yüklenmesi yönünde bir gereksinim yoktur. Bağlayıcı için sunucu bilgisayarı gereksinimleri şu şekildedir:

- Windows Server 2012 R2 veya üzeri
- Bu bilgisayar, kullanıcıların doğrulandığı ormanda bulunan bir etki alanına katılmış olmalıdır

## <a name="federated-identity-ad-fs"></a>Federasyon kimliği (AD FS)

Kullanıcılarınızın Office 365 ve diğer bulut hizmetlerine erişimi üzerinde daha fazla denetim sahibi olmak için [Active Directory Federasyon Hizmetleri (AD FS)](how-to-connect-fed-whatis.md) özelliğini kullanarak çoklu oturum açma (SSO) ile dizin eşitlemesini yapılandırabilirsiniz. Kullanıcılarınızın oturum açma işlemlerini AD FS üzerinden gerçekleştirmek, kimlik doğrulaması yetkisini, kullanıcı kimlik bilgilerini doğrulayan bir şirket içi sunucuya verir. Bu modelde şirket içi Active Directory kimlik bilgileri asla Azure AD'ye geçirilmez.

![Federasyon kimliği](./media/whatis-hybrid-identity/federated-identity.png)

Kimlik federasyonu olarak da bilinen bu oturum açma yöntemi, tüm kullanıcı kimlik doğrulama işlemlerinin şirket içi ortamda denetlenmesini sağlar ve yöneticilerin daha sıkı erişim denetimi düzeyleri belirlemesini mümkün kılar. AD FS ile kimlik federasyonu en karmaşık seçenektir ve şirket içi ortamınızda ek sunucuların dağıtılmasını gerektirir. Ayrıca kimlik federasyonu kullandığınızda Active Directory ve AD FS altyapınız için 7/24 destek sunmanız gerekir. Gerekli destek düzeyinin bu kadar yüksek olmasının nedeni, şirket içi ortamınızdaki İnternet erişimi, etki alanı denetleyicileri veya AD FS sunucularının kullanılamaz duruma gelmesi durumunda kullanıcıların bulut hizmetlerinde oturum açamayacak olmasıdır.

> [!TIP]
> Active Directory Federation Services (AD FS) ile federasyon seçeneğini kullanmaya karar verirseniz AD FS altyapınızın kullanım dışı kalması durumunda yedek çözüm olarak kullanmak üzere parola karması eşitleme seçeneğini de ayarlayabilirsiniz.
>

## <a name="common-scenarios-and-recommendations"></a>Ortak senaryolar ve öneriler

Aşağıda, bazı yaygın karma kimlik ve erişim yönetimi senaryoları, hangi karma kimlik seçeneğinin (veya seçeneklerinin) hangi senaryoya uygun olabileceği hakkında önerilerle birlikte verilmiştir.

|Şunu istiyorum:|PHS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Şirket içi Active Directory ortamında oluşturulan yeni kullanıcı, kişi ve grup hesaplarının otomatik olarak bulutla eşitlenmesi.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Kiracımı Office 365 karma senaryoları için ayarlamak|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Kullanıcılarımın şirket içi parolalarıyla bulut hizmetlerinde oturum açması ve hizmetlere erişim sağlaması|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Kuruluş kimlik bilgileri ile çoklu oturum açmayı uygulamaya alma|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Parola karmalarının bulutta depolanmadığından emin olma| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Bulutta çok faktörlü kimlik doğrulama çözümlerini etkinleştirme| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Şirket içi ortamda çok faktörlü kimlik doğrulama çözümlerini etkinleştirme| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Kullanıcılarım için akıllı kart desteği sunma<sup>4</sup>| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|
|Office Portalı'nda ve Windows 10 masaüstünde parola süre sonu bildirimleri görüntüleme| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|

> <sup>1</sup> Çoklu oturum açma ile parola karması eşitleme.
>
> <sup>2</sup> Doğrudan kimlik doğrulama ve çoklu oturum açma. 
>
> <sup>3</sup> AD FS ile federasyon çoklu oturum açma.
>
> <sup>4</sup> AD FS, kurumsal PKI çözümünüzle tümleştirilerek sertifika ile oturum açma imkanı sunulabilir. Bu sertifikalar MDM veya GPO gibi güvenilen sağlama kanalları aracılığıyla dağıtılan yazılımsal sertifikalar, akıllı kart sertifikaları (PIV/CAC kartları dahil) veya İç için Hello (sertifika güveni) olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için [bu bloga](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) bakın.
>

## <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?

Azure AD Connect, Microsoft'un karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek için tasarladığı araçtır.  Bu sayede kullanıcılarınıza Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik sağlayabilirsiniz.  Aşağıdaki özellikleri sağlar:
    
- [Eşitleme](how-to-connect-sync-whatis.md) - Bu bileşen; kullanıcı, grup ve diğer nesnelerin oluşturulmasından sorumludur. Aynı zamanda şirket içi kullanıcılarınıza ve gruplarınıza ilişkin kimlik bilgilerinin bulut ile eşleşmesini sağlamaktan da sorumludur.  Azure AD ile parola karmalarını eşitlemekten sorumludur.
- [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md): Parola karmalarını Azure AD ile eşitleyerek kullanıcıların şirket içi ortamda ve bulutta aynı parolayı kullanmasını sağlayan isteğe bağlı bir bileşendir.
-   [AD FS ve federasyon tümleştirmesi](how-to-connect-fed-whatis.md) - Federasyon, Azure AD Connect'in isteğe bağlı bir parçası olup şirket içi bir AD FS altyapısını kullanarak karma bir ortamı yapılandırabilir. Ayrıca sertifika yenileme ve ek AD FS sunucusu dağıtımları gibi AD FS yönetim özellikleri de sunar.
-   [Doğrudan Kimlik Doğrulaması](how-to-connect-pta.md) - kullanıcıların aynı parolayı şirket içinde ve bulutta kullanmasına izin veren ancak federe bir ortamın ek altyapısını gerektirmeyen başka bir isteğe bağlı bileşen
-   [PingFederate ve federasyon tümleştirme](how-to-connect-install-custom.md#configuring-federation-with-pingfederate) - Kimlik sağlayıcısı olarak PingFederate kullanmanızı sağlayan farklı bir federasyon seçeneğidir.
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

## <a name="next-steps"></a>Sonraki adımlar


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









