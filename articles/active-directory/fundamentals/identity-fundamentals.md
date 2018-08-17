---
title: Azure kimlik yönetimi ile ilgili temel bilgiler | Microsoft Docs
description: Bulut tabanlı kimlikler, kullanıcıların kuruluş uygulamalarına ve verilerine erişim şekli ve zamanı hakkında denetim ve görünürlük sağlamanın en iyi yoludur.
keywords: ''
author: eross-msft
manager: mtillman
ms.reviewer: jsnow
ms.author: lizross
ms.date: 08/07/2018
ms.topic: overview
ms.prod: ''
ms.service: active-directory
ms.component: fundamentals
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 797c35bad03c063203e3616740af633b71835a9c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39640284"
---
# <a name="fundamentals-of-azure-identity-management"></a>Azure kimlik yönetimi ile ilgili temel bilgiler

Bulutta ve cihazlarda olmak üzere şirket ağının dışında bulunan dijital şirket kaynaklarının sayısı arttıkça etkili bir bulut tabanlı kimlik ve erişim yönetimi çözümü gerekli hale gelmektedir. Bulut tabanlı kimlikler, kullanıcıların kuruluş uygulamalarına ve verilerine erişim şekli ve zamanı hakkında denetim ve görünürlük sağlamanın en iyi yoludur.

Microsoft, on yılı aşkın süredir bulut tabanlı kimliklerin güvenliğini sağlamakta ve şimdi [Azure Active Directory (AD)](active-directory-whatis.md) ile bu koruma sistemlerini sizin kullanımınıza sunmaktadır. Azure AD ile kuruluş yöneticileri her zamankinden daha iyi güvenli ve yönetim özellikleriyle kullanıcı ve yönetici sorumluluğunu kolayca mümkün kılabilir.

Azure AD Premium tüm uygulamalar için tek bir güvenli kimlik, kimlik koruması ([Microsoft akıllı güvenlik grafiği](https://www.microsoft.com/en-us/security/intelligence) destekli) ve Privileged Identity Management kullanımını mümkün kılan gelişmiş koruma özelliklerine sahip bulut tabanlı kimlik ve erişim yönetimi çözümüdür. Diğer izleme veya raporlama araçlarından farklı olan Azure AD Premium, kullanıcılarınızın kimliklerini gerçek zamanlı olarak korumanızı ve kuruluş verilerini korumak için risk tabanlı, uyarlanabilir erişim ilkeleri oluşturmanızı sağlayabilir.

Azure AD kimlik yönetimi ve koruma özelliklerini anlatan bu kısa videoyu izleyin:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Microsoft yalnızca tüm uygulamalarda kullanabileceğiniz bir kimlik oluşturmakla kalmaz, aynı zamanda kuruluşunuzun BT altyapısını otomatikleştirmenizi ve yönetmenizi sağlayıp güvenliğini sağlamanıza yardımcı olur. Bulut bilişimdeki ilerlemelere rağmen kullanıcı parolalarını sıfırlamak için yapılan yardım masası çağrıları, kullanıcı grubu yönetimi ve uygulama istekleri gibi BT görevlerini yönetme ve denetleme talepleri devam etmektedir. Durumu daha karmaşık hale getiren de çalışanların işe kendi kişisel cihazlarını getirmesi ve var olan SaaS uygulamalarını kullanmasıdır. Bu da kurumsal veri merkezlerinde ve genel bulut platformlarında uygulama denetimi sağlamayı oldukça zor bir hale getirmektedir.

[!INCLUDE [identity](../../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Şirket içi Active Directory ile Azure AD ve Office 365 arasında bağlantı kurma
Şirket içi Active Directory ortamına büyük yatırımlar yapmış olan kuruluşlar, [karma kimlik yönetimi](https://aka.ms/aadframework) sayesinde şirket içi dizinlerini Azure AD ile tümleştirerek bu yatırımlarını buluta genişletebilir. Bu sayede kullanıcılarınız ortak kimlikle konumdan bağımsız olarak kaynaklara erişim sağlayabilir ve daha üretken hale gelebilir. Bu geçişin ardından kullanıcılar ve kuruluşlar çoklu oturum açma (SSO) özelliklerini kullanarak hem şirket içi kaynaklara hem de Office 365 gibi bulut hizmetlerine erişebilir.

Tümleştirmeyi gerçekleştirmek için tek ihtiyacınız olan [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) aracıdır. Azure AD Connect, kimlik eşitleme ihtiyaçlarınızı desteklemek için gerekli özellikleri sunar. Ayrıca DirSync ve Azure AD Eşitleme gibi eski kimlik tümleştirme araçlarının yerine geçer. Azure AD Connect ile şirket içi ortamla Azure AD arasında kimlik yönetimi ve eşitleme şu şekilde sağlanır:

- Eşitleme - Bu bileşen; kullanıcı, grup ve diğer nesnelerin oluşturulmasından sorumludur. Aynı zamanda şirket içi kullanıcılarınıza ve gruplarınıza ilişkin kimlik bilgilerinin bulut ile eşleşmesini sağlamaktan da sorumludur. Kullanıcılar parolalarını Azure AD'de güncelleştirdiğinde şirket içi dizinlerin eşitlenmesi için parola geri yazma özelliği kullanılabilir.
- Kimlik doğrulaması: Azure AD yeni kontrol düzleminiz haline geldiğinde kimlik doğrulaması, bulut erişiminin temeli olur. Doğru kimlik doğrulama yönteminin seçilmesi Azure AD karma kimlik çözümünün ayarlanması aşamasında oldukça önemli bir karardır. Kuruluşunuz için bulut kimlik doğrulaması (Parola Karması Eşitleme/Doğrudan Kimlik Doğrulama) veya şirket dışı kimlik doğrulaması arasında seçim yapmak için [bu kılavuzu](https://aka.ms/auth-options) inceleyin.
- Sistem Durumu İzleme: [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), iyi bir izleme olanağı sağlayarak bu etkinliğin Azure portalda görüntülenmesi için merkezi bir konum oluşturur.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Self servis ve çoklu oturum açma deneyimleriyle üretkenliği artırın ve yardım masası maliyetlerini düşürün

Çalışanlar tek bir kullanıcı adı ve parola çifti kullandığında ve tüm cihazlarda aynı deneyimi yaşadığında daha üretken olacaktır. Kullanıcılar ayrıca [unutulan parolayı sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) veya yardım masasından destek almadan uygulama erişimi isteme görevleri self servis olarak gerçekleştirdiğinde zaman tasarrufu yapacaktır.

Azure AD, [şirket içi Active Directory ortamını buluta genişleterek](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) kullanıcıların hem etki alanına katılmış cihazlarda, şirket kaynaklarında hem de işlerini yapmak için gerekli olan tüm web ve SaaS uygulamalarında birincil kuruluş hesaplarını kullanmalarını sağlar. Birden fazla kullanıcı adı ve parola hatırlamak zorunda kalmamaya ek olarak kullanıcıların uygulama erişimi kuruluş grubu üyeliklerine ve çalışan durumlarına göre otomatik olarak sağlanabilir (veya kaldırılabilir). Galeri uygulamaları veya geliştirip [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) ile yayımladığınız şirket içi uygulamalar için bu erişimi siz denetleyebilirsiniz.

## <a name="manage-and-control-access-to-corporate-resources"></a>Şirket kaynaklarına erişimi yönetme ve denetleme
Microsoft kimlik ve erişim yönetimi çözümleri BT ekiplerinin şirket veri merkezindeki ve buluttaki uygulamalarla kaynaklara erişimin güvenliğini sağlayarak [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) ve [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) gibi ek doğrulama düzeylerini etkinleştirmesini mümkün kılar. Gelişmiş güvenlik raporlaması, denetim ve uyarı özellikleriyle şüpheli etkinliklerin izlenmesi, olası güvenlik sorunlarının önlenmesine yardımcı olur.

Azure AD Premium'daki koşullu erişim ilkeleri, kuruluş yöneticisi olarak Azure AD bağlantılı tüm uygulamalar (SaaS uygulamaları, bulutta çalışan özel uygulamalar veya şirket içindeki web uygulamaları) için ilke tabanlı erişim kuralları oluşturmanızı sağlar. Azure AD bu ilkeleri gerçek zamanlı olarak değerlendirir ve kullanıcı bir uygulamaya erişmeye çalıştığında bunları uygular. Azure kimlik koruma ilkeleri, şüpheli etkinliklerin tespit edilmesi durumunda otomatik olarak eylem gerçekleştirmenizi sağlar. Bu eylemler yüksek risk düzeyine sahip kullanıcıların erişimini engelleme, çok faktörlü kimlik doğrulaması uygulama ve kimlik bilgilerinin gizliliğinin bozulmuş gibi görünmesi halinde kullanıcı parolalarını sıfırlama gibi işlemleri kapsayabilir.


## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management

Azure Active Directory Premium P2 teklifiyle birlikte sunulan [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), yönetici hesaplarını ve Azure Active Directory ortamınızdaki ve diğer Microsoft çevrimiçi hizmetlerindeki kaynaklara erişimini keşfetmenizi, kısıtlamanızı ve izlemenizi sağlar. Bu özellik ayrıca yalnızca istediğiniz süre boyunca isteğe bağlı yönetici erişimi sunmanızı sağlar.

Privileged Identity Management, istek üzerine yönetici hakları sağlayarak yöneticilerin önceden belirlenen süre boyunca ayrıcalıklarının çok faktörlü kimlik doğrulaması ile ve geçici olarak yükseltilmesini istemesini, bu sürenin sonunda da hesaplarının normal kullanıcı durumuna dönmesini sağlayabilir.

## <a name="benefits-of-azure-identity"></a>Azure Kimliğinin Avantajları

Azure kimlik yönetimiyle aşağıdakileri yapabilirsiniz:

-   Kuruluşunuzdaki kullanıcılar için tek bir kimlik oluşturup yönetebilir; kullanıcıları, grupları ve cihazları [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) ile eşitleyebilirsiniz.

-   Binlerce önceden tümleştirilmiş SaaS uygulaması dahil olmak üzere uygulamalarınızda çoklu oturum açma özelliğini kullanabilir veya [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) ile şirket içi SaaS uygulamalarına güvenli uzaktan erişim sağlayabilirsiniz.

-   Hem şirket içi hem de bulut uygulamaları için [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) tabanlı kurallar kullanarak uygulama erişimi güvenliğini sağlayabilirsiniz.

-   [Self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) ile kullanıcı üretkenliğini, [MyApps portalını](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help) kullanarak grup ve uygulama erişimini geliştirebilirsiniz.

-   Dünya çapında, kurumsal düzeyde bulut tabanlı kimlik ve erişim yönetimi çözümünün [yüksek düzeyde kullanılabilirlik ve güvenilirlik](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) özelliklerinden faydalanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure kimlik çözümleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)
