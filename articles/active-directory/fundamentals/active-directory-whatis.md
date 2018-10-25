---
title: Azure Active Directory (Azure AD) nedir? | Microsoft Docs
description: Azure Active Directory’yi kullanarak mevcut şirket içi kimliklerinizi buluta taşıma veya Azure AD ile tümleşik uygulamalar geliştirme hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
ms.date: 09/13/2018
ms.custom: it-pro
ms.openlocfilehash: 406baeac60c7c0cdf5f74876e5fc29ea23d3d6f6
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957556"
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?
Azure Active Directory (Azure AD), Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD, temel dizin hizmetlerini, uygulama erişimi yönetimini ve kimlik korumasını tek bir çözümde birleştirerek geliştiricilerin merkezi ilke ve kurallara göre uygulamalarına yönelik erişim denetimi sunmasına yardımcı olan standartlara dayalı bir platform sunar.

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="benefits-of-azure-ad"></a>Azure AD’nin avantajları
Azure AD aşağıdakileri yapmanıza yardımcı olur:

-   Kuruluşunuzdaki kullanıcılar için tek bir kimlik oluşturup yönetebilir; kullanıcıları, grupları ve cihazları [Azure AD Connect](../connect/active-directory-aadconnect.md) ile eşitleyebilirsiniz.

-   Binlerce önceden tümleştirilmiş SaaS uygulaması dahil olmak üzere uygulamalarınızda çoklu oturum açma özelliğini kullanabilir ve [Azure AD Uygulama Ara Sunucusu](../manage-apps/application-proxy.md) ile şirket içi SaaS uygulamalarına güvenli uzaktan erişim sağlayabilirsiniz.

-   Hem şirket içi hem de bulut uygulamaları için kural tabanlı [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) ilkeleri uygulayarak uygulama erişimi güvenliğini sağlayabilirsiniz.

-   [Self servis parola sıfırlama](../user-help/user-help-reset-password.md) ile kullanıcı üretkenliğini, [MyApps portalını](../user-help/active-directory-saas-access-panel-introduction.md) kullanarak grup ve uygulama erişimini geliştirebilirsiniz.

-   Dünya çapında, kurumsal düzeyde bulut tabanlı kimlik ve erişim yönetimi çözümünün [yüksek düzeyde kullanılabilirlik ve güvenilirlik](https://docs.microsoft.com/azure/architecture/checklist/availability) özelliklerinden faydalanabilirsiniz.

## <a name="who-uses-azure-ad"></a>Kimler Azure AD kullanır?
Azure AD; BT yöneticileri, uygulama geliştiricileri ve Office 365, Azure veya Dynamics CRM Online kullanıcıları için tasarlanmıştır.

- **BT yöneticileri.** Azure AD binlerce [bulut tabanlı SaaS uygulamaları](../saas-apps/tutorial-list.md) ve şirket içi uygulamalar için sunduğu daha güçlü kimlik yönetimi ve çoklu oturum açma (SSO) erişimi sayesinde kuruluşunuz için daha güvenli bir çözüm sunar. Bu uygulamaları kullanarak kullanıcılarınız için bulut tabanlı uygulama güvenliği, sorunsuz erişim, gelişmiş işbirliği ve kimlik yaşam döngüsü otomasyonu özelliklerinden faydalanabilir, bu sayede hem güvenlik hem de uyumluluk alanında gelişme sağlayabilirsiniz.

    Ayrıca [Azure AD Connect](../connect/active-directory-aadconnect-get-started-express.md)’i kullanarak Azure AD'yi mevcut Windows Server Active Directory ile tümleştirebilir, bulut tabanlı SaaS erişimini kuruluşunuzun mevcut şirket içi kimlik yatırımlarını kullanabilirsiniz.

- **Uygulama geliştiricileri için.** Azure AD, dünya üzerinde milyonlarca kuruluş tarafından kullanılan bir kimlik yönetimi çözümü ile tümleştirme sağlayarak uygulamalarınızı derlemeye odaklanmanıza yardımcı olur.

- **Office 365, Azure veya Dynamics CRM Online müşterileri için.** Azure AD'yi zaten kullanıyorsunuz. Office 365, Azure ve Dynamics CRM Online kiracıları aslında birer Azure AD kiracısıdır ve tümleşik bulut uygulamalarınıza kullanıcı erişimini yönetmeye hemen başlayabilirsiniz.

## <a name="how-reliable-is-azure-ad"></a>Azure AD ne kadar güvenilir?
Çok kiracılı, coğrafi olarak dağıtılan ve yüksek kullanılabilirlik özellikli bir tasarıma sahip olan Azure AD'ye en kritik iş gereksinimlerinizde bile güvenebilirsiniz. Azure AD, otomatik yük devretme ile yaklaşık 28 veri merkezini çalıştırır. Başka bir deyişle, bir veri merkezi bozulsa bile dizin verilerinizin kopyaları en az iki başka bölgesel olarak dağınık veri merkezinde canlıdır ve anında erişilebilir.

Hizmet düzeyi sözleşmeleri hakkında daha fazla bilgi için bkz. [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Sürüm seçin
Tüm Microsoft Online iş hizmetlerinde oturum açma ve diğer kimlik gereksinimleri için Azure AD kullanılır. Microsoft Online iş hizmetlerinden herhangi birine (örneğin, Office 365 veya Microsoft Azure) abone olursanız Azure AD’nin tüm Ücretsiz özelliklerine otomatik olarak erişim sahibi olursunuz. Azure Active Directory Ücretsiz sürümünü kullanarak kullanıcıları ve grupları yönetebilir, şirket içi dizinlerle eşitleyebilir, Azure, Office 365 ve Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox gibi binlerce popüler SaaS uygulamasında çoklu oturum açma imkanına sahip olabilirsiniz. 

Azure AD uygulamanızı geliştirmek için Azure Active Directory Temel, Premium P1 veya Premium P2 sürümlerine yükseltme yaparak ücretli özellikler ekleyebilirsiniz. Azure AD’nin ücretli sürümleri, mevcut ücretsiz dizin üzerinde derlenir ve self servis, gelişmiş izleme, güvenlik raporlaması, Multi-Factor Authentication (MFA) ve mobil iş gücünüz için güvenli erişim gibi kurumsal sınıf özellikler sunar.

> [!NOTE]
> Bu sürümlerin fiyatlandırma seçenekleri için bkz. [Azure Active Directory Fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 ve Azure Active Directory Temel şu an için Çin'de desteklenmemektedir. Azure AD fiyatlandırması hakkında daha fazla bilgi için Azure Active Directory Forum sayfasından iletişime geçebilirsiniz.

- **Azure Active Directory Temel.** Buluta yönelik ihtiyaçları öncelikli olan görev çalışanları için tasarlanan bu sürüm, bulut odaklı uygulama erişimi ve self servis kimlik yönetimi çözümleri sağlar. Temel sürümü size grup tabanlı erişim yönetimi, bulut uygulamaları için self servis parola sıfırlama ve Azure Active Directory Uygulama Ara Sunucusu (Azure AD’yi kullanarak şirket içi web uygulamalarında yayımlama için) gibi üretkenliği geliştiren ve maliyeti düşüren özellikler sunar. Bu özelliklerin tamamı, %99,9 oranındaki kurumsal düzeyde çalışma süresi SLA’sı ile desteklenir.

- **Azure Active Directory Premium P1.** Kimlik ve erişim yönetimine yönelik gereksinimleri daha yoğun olan kurumları desteklemek için tasarlanan Azure Active Directory Premium sürümü, özellik açısından zengin, kurumsal düzeyde kimlik yönetimi olanakları eklemenin yanı sıra karma kullanıcıların şirket içindeki ve buluttaki özelliklere sorunsuz bir şekilde erişmesine olanak tanır. Bu sürüm, karma ortamlarda bilgi çalışanı ve kimlik yöneticileri için gerekli olan; bulutta uygulama erişimi, self servis kimlik ve erişim yönetimi (IAM), kimlik koruma ve güvenlik gibi özelliklerin tamamını içerir. Dinamik gruplar ve self servis grup yönetimi gibi gelişmiş kaynak yönetimi ve devretme özelliklerini destekler. Microsoft Identity Manager'ı (şirket içi kimlik ve erişim yönetimi paketi) içerir ve şirket içi kullanıcılarınız için self servis parola sıfırlama gibi çözümler sunan bulut geri yazma özelliklerine sahiptir.

- **Azure Active Directory Premium P2.** Tüm kullanıcılarınız ve yöneticileriniz için gelişmiş koruma özellikleriyle tasarlanmış olan bu yeni teklif, Azure AD Premium P1’in tüm özelliklerinin yanı sıra Identity Protection ve Privileged Identity Management özelliklerini de içerir. Azure Active Directory Kimlik Koruması, uygulamalarınıza ve kritik şirket verilerinize risk tabanlı koşullu erişim sağlamak için milyarlarca sinyali kullanır. Azure Active Directory Privileged Identity Management ile ayrıca ayrıcalıklı hesaplarınızı yönetmek ve korumak için sunulan destek sayesinde yöneticileri ve onların kaynaklara erişimlerini keşfedebilir, kısıtlayabilir ve izleyebilir, gerektiğinde anlık erişim sağlayabilirsiniz.  

> [!NOTE]
> Bazı Azure Active Directory özellikleri "kullandıkça öde" sürümüne de sahiptir:<ul><li>**Azure Active Directory B2C.** Tüketicilere yönelik uygulamalarınız için kullanabileceğiniz kimlik ve erişim yönetimi çözümüdür. Daha fazla bilgi için bkz. [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).</li><li>**Azure Multi-Factor Authentication.** Kullanıcı başına veya kimlik doğrulaması sağlayıcısı başına kullanılır. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)

## <a name="as-an-admin-how-do-i-get-started"></a>Yönetici olarak nasıl kullanmaya başlayabilirim?
Ücretsiz 30 günlük deneme için kaydolmak ve ilk bulut çözümünüzü dağıtmak için bkz [Azure Active Directory Premium denemesi](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="as-a-developer-how-do-i-get-started"></a>Geliştirici olarak nasıl kullanmaya başlarım?
Ücretsiz 30 günlük deneme için kaydolmak ve uygulamalarınızı Azure AD ile tümleştirmeye başlamak için bkz. [Azure Active Directory Premium denemesi](https://azure.microsoft.com/trial/get-started-active-directory/). Daha fazla bilgi için Azure Active Directory’nin [Geliştirici Kılavuzu](../develop/v1-overview.md)’na da bakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure kimlik ve erişim yönetiminin temelleri hakkında daha fazla bilgi edinin](identity-fundamentals.md).

- [Azure AD’yi Windows Server Active Directory ile tümleştirin](../hybrid/how-to-connect-install-express.md).
