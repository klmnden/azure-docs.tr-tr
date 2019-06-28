---
title: Koşullu erişim benimseme Seti - Azure Active Directory
description: Kaynaklara erişim için Azure AD koşullu erişim benimseme
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cc4ff5fb528760be8c910f3da7d5691a6aae0d8
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67387579"
---
#  <a name="adopting-azure-ad-conditional-access"></a>Azure AD koşullu erişim benimseme

Bir mobil öncelikli ve bulut öncelikli dünyada, kullanıcılar, farklı türde cihazları ve uygulamaları her yerde kullanarak kuruluşunuzun kaynaklarına erişebilir. Sonuç olarak, kimin bir kaynağa erişmek için yalnızca odaklanarak artık yeterli değil. Kimlerin eriştiğini ve kullanıcının olduğu ve hangi cihaz kullanılır ve çok daha fazlasını tanımlamak denetleyebilirsiniz.

Bu denetim sağlamak için **Azure Active Directory (AD) koşullu erişim** herhangi bir kullanıcı multi-Factor Authentication (MFA) gibi bir uygulamaya erişmek için karşılaması gerekir koşullar belirtmenizi sağlar. Denetimleri nasıl yetkili kullanıcıların (bir bulut uygulaması için erişim izni verilen kullanıcılar) koşullu erişim ilkelerini kullanarak bulut uygulamaları belirli koşullar altında erişim. Başvurmak [Azure Active Directory'de koşullu erişim nedir](overview.md#conditional-access-policies) daha fazla bilgi için.

## <a name="key-benefits"></a>Önemli avantajlar

Azure AD koşullu erişim kullanmanın avantajları şunlardır:

* **Üretkenliği artırın:** Koşullu erişim (CA) ilkeleri, kullanıcılara sorulur mfa'yı kullanmak için erişimi engellenmiş noktası hedeflemek izin veya güvenilir bir cihaz kullanması gerekir. Örneğin, yalnızca bir uygulamaya kapalı olduğunda şirket ağı kullanıcıları için MFA gerektirme gibi ilkeler ayarlayabilir. MFA istekleri azaltma kullanıcılar oturum açtıklarında her zaman kullanıcılar için mfa'yı varsa daha üretken kalmasını sağlar. Ayrıca, Azure AD koşullu erişim ilkeleri kullanıcı başına esasına belirtmenizi sağlar ve uygulamaya özgü ilkeleri oluşturur.
* **Risk yönetme:** Koşullu erişim ilkelerini etkinleştirme ile bulut ölçeğinde kimlik koruması, risk tabanlı erişim denetimi özellikleri ve yerel çok faktörlü kimlik doğrulama desteği sağlar. Kimlik koruması ile koşullu erişim zorlandığı bir uygulamaya erişim engellendi veya Geçitli tanımlamanızı sağlar.
* **Uyumluluk ve idare adres:** Gerçekleştirilen her uygulama için erişim isteği yerel denetim günlüklerini desteklediğinden, Azure AD ile erişim isteklerini ve uygulama için bir onayları denetim ve genel uygulama kullanımını anlamak kolaydır. Denetim, istek sahibi kimliği, istenen tarih, iş gerekçesi, onay durumunu ve onaylayanın kimliğini içerir. Bu veriler, bu verilerin içe aktarımını bir güvenlik olayı ve tercih ettiğiniz olay izleme (SIEM) sistemine uygulamasına olanak sağlayacak bir API'den de kullanılabilir.
* **Maliyet Yönetimi:** Erişim ilkeleri Azure AD'ye taşınıyor özel güvenme azaltır veya Active Directory Federasyon Hizmetleri (ADFS) gibi çözümleri bu altyapıyı çalıştırmanın maliyeti azaltmak için koşullu erişim, şirket.

## <a name="customer-case-studies"></a>Müşteri örnek olay incelemeleri

Tanımlaması ve otomatik erişim denetimi kararları koşullara göre bulut uygulamalarına erişmek için Azure AD koşullu erişim nasıl çoğu kuruluş kullanacağınızı öğrenin. Aşağıdaki öyküler nasıl bu müşteri ihtiyaçlarının karşılandığından gösterir.

* [**Wipro** müşterilerle yaşadığımız geliştirmek için Microsoft bulut güvenlik araçları ile mobil üretkenliği artıran.](https://customers.microsoft.com/story/wipro-professional-services-enterprise-mobility-security) Azure AD'de koşullu erişim ilkelerini, şirket belgelerin, kaynakları ve uygulamaları ile güvenilir dış varlıklar---paylaşmak kendi kimlik bilgilerini---kendi Kurumsal veriler üzerinde denetim korurken kullanabilen etkinleştirdiniz.
* [**Accenture** Microsoft Cloud App security ile buluta geçiş koruyan](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security) Accenture değerlendirme [koşullu erişim uygulaması denetimi](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad) kullanan Azure Active Cloud App Security özelliğidir Dizin için ağ geçidi uygulama erişimi belirli koşullara göre koşullu erişim. Bu özellik varsayalım, yasaklanması yüklemeleri sırasında dosya salt okunur erişimi etkinleştirmek için yararlı olabileceğini LePenske diyor.
* [**Aramex** teslim - genel Lojistik ve Nakliye şirketi, kimlik ve erişim yönetimi çözümü ile buluta bağlı bir office oluşturur, sınırlı](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en). Güvenli kalmasını sağlama erişim Aramex'ın uzak çalışanlar ile özellikle zordu. Şirket, bu uzak çalışanlar SaaS uygulamalarına Ağ dışından erişmesine izin vermek için koşullu erişim artık uygulanıyor. Koşullu erişim kuralı ve doğru kişiye doğru erişimi verme işlemi, multi-Factor Authentication zorlanıp zorlanmayacağını karar verir.

Azure AD koşullu erişim müşteri ve iş ortağı deneyim hakkında daha fazla bilgi için ziyaret edin - [Azure ile muhteşem şeyler kişiler yapıyor bkz](https://azure.microsoft.com/case-studies/?service=active-directory).

## <a name="announcements"></a>Duyurular

Azure AD iyileştirmeleri düzenli olarak alır. İle en son gelişmeleri güncel kalmak için bkz: [Azure Active Directory'deki yenilikler nelerdir?](../fundamentals/whats-new.md)

Microsoft kimlik bölme ve teknoloji topluluğu tarafından en son Web günlükleri:

* 24 Eylül 2018 [Azure Databricks, Azure Active Directory koşullu erişim](https://azure.microsoft.com/updates/azure-active-directory-conditional-access-in-azure-databricks/)
* 21 Eylül 2018 [özel denetimler, genel Önizleme aşamasında olan Azure AD koşullu erişim](https://azure.microsoft.com/updates/azure-ad-conditional-access-custom-controls-are-in-public-preview/)
* 21 Eylül 2018 [Microsoft Cloud App Security ile sınırlı erişim için Azure AD koşullu erişim desteği kullanıma sunuldu](https://azure.microsoft.com/updates/azure-ad-conditional-access-support-for-limited-access-with-microsoft-cloud-app-security-is-now-available/)
* 21 Eylül 2018 [Azure AD koşullu erişim: Yönetilen iOS/Android platformları şimdi önizlemede için tarayıcı desteği](https://azure.microsoft.com/updates/azure-ad-conditional-access-managed-browser-support-for-ios-android-platforms-now-in-preview/)
* 21 Eylül 2018 [ülke kodları için Azure AD koşullu erişim, genel Önizleme modundadır.](https://azure.microsoft.com/updates/azure-ad-conditional-access-for-country-codes-is-in-public-preview/)
* 21 Eylül 2018 [Azure AD--kullanım koşulları kullanıma sunuldu](https://azure.microsoft.com/updates/azure-ad-terms-of-use-now-available/)

## <a name="learning-resources"></a>Öğrenme kaynakları

Azure AD koşullu erişim işlevleri genel bakışını almak için aşağıdaki bağlantıları izleyin.

* Bilgi edinin "[Azure Active Directory'de koşullu erişim nedir?](overview.md)"
* Bildiğiniz "[Azure Active Directory koşullu erişim koşulları nelerdir?](conditions.md)"
* Bildiğiniz "[konum koşulu, Azure Active Directory koşullu erişim nedir?](location-condition.md)"
* Bildiğiniz "[Azure Active Directory koşullu erişim denetimleri erişim nelerdir?](controls.md)"
* Bul "[ne yenilikler ise aracı Azure Active Directory koşullu erişim?"](what-if-tool.md)
* İzleyin [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md)

Ayrıca, Azure Active Directory ile tümleşik olan tüm hizmetlere erişimi korumak yönergeler için aşağıdaki bağlantılara bakın.

* [Taban çizgisi protection (Önizleme) nedir?](baseline-protection.md) Temel koruma, Azure Active Directory ortamında etkin güvenlik temel düzeyde en az sahip olmasını sağlar.
* [Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations). Enterprise Mobility + Security ürünler üzerinden bulut hizmetlerine güvenli erişim bir önerilen ortam ve yapılandırması, önceden belirlenmiş bir dizi koşullu erişim ilkeleri ve ilgili özellikleri dahil olmak üzere uygulama tarafından yapılandırılması açıklanmaktadır.
* [Azure Active Directory koşullu erişim ayarları başvurusu](technical-reference.md). Bilgi edinin:
   * Hangi uygulamalar koşullu erişim kullanır?
   * Koşullu erişimle hangi hizmetler etkinleştirilir?
* [Azure Active Directory koşullu erişim için güvenli kullanıcı erişimini etkinleştirmek](https://www.youtube.com/watch?v=eLAYBwjCGoA). Nasıl koşullu erişim diğer Kurumsal ve Mobility Suite'nın iş yüklerini bir rol oynar öğrenmek için bu videoyu izleyin.

### <a name="training-videos"></a>Eğitim videoları

**Enterprise Mobility + Security'ye koşullu erişim**
   > [!VIDEO https://www.youtube.com/embed/A7IrxAH87wc]

**Cihaz tabanlı koşullu erişim**
   > [!VIDEO https://www.youtube.com/embed/AdM0zYB-3WQ]

**Azure Active Directory koşullu erişim için güvenli kullanıcı erişimini etkinleştirin.**
   > [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

### <a name="online-courses"></a>Çevrimiçi kurslar

Aşağıdaki koşullu erişim kursları ve daha fazlasını göz atın [Pluralsight.com](https://www.pluralsight.com/):

* Pluralsight.com: [Microsoft Azure'da Kimlik Yönetimi tasarım](https://www.pluralsight.com/courses/microsoft-azure-identity-management-design)
   * "Bu kurs size, Azure AD ile kimlik yönetimi çözümü tasarlamak için bilmeniz gereken temel öğeye yol gösterir." Azure AD koşullu erişim "Kullanarak rolleri ve erişim denetimi Azure AD ile" kapsanan modülü.

* Pluralsight.com: [Tasarım kimlik doğrulaması için Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authentication-design)
   * "Bu kursta Azure AD tüm bulut kimlik doğrulama gereksinimlerini çözümlemek için nasıl kullanılacağını açıklar." Azure AD koşullu erişim, "Kimlik doğrulaması gereksinimleri için farklı senaryolar" modülünde ele alınmıştır.

* Pluralsight.com: [Microsoft Azure için tasarım yetkilendirme](https://www.pluralsight.com/courses/microsoft-azure-authorization-design)
   * "Bu kursta Azure ve Azure ile kullanılabilir yetkilendirme seçenekleri öğretir AD." Azure AD koşullu erişim, "Azure Resource Manager ve Azure AD ile yetkilendirme" modülünde ele alınmıştır.

### <a name="books"></a>Kitaplar

* O'Reilly - [ikinci sürüm Azure çözümleri - uygulama.](https://www.oreilly.com/library/view/implementing-azure-solutions/9781789343045/b7ead3db-eb1c-4ace-897e-86ee25ea86be.xhtml)
   * "Azure hizmetleriyle çalışır duruma alın ve bunları kuruluşunuzda uygulama hakkında bilgi edinin. Azure AD koşullu erişim bölümde ele [dağıtma ve Azure Active Directory eşitleme](https://learning.oreilly.com/library/view/implementing-azure-solutions/9781789343045/02ca8bba-08cf-4691-a7d0-1b96e286e7ea.xhtml). "

* Wiley- [Microsoft Azure altyapı hizmetleri Uzmanlaşma](https://www.wiley.com/Mastering+Microsoft+Azure+Infrastructure+Services-p-9781119003298)
   * "İşte anlamak, değerlendirmek, dağıtmak ve Microsoft Azure'u ortamları sağlamak için ihtiyacınız olan her şey."

## <a name="white-papers"></a>Teknik incelemeler

* 18 Aralık 2018'e, yayımlanan [Azure Active Directory ile esnek erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)
   * Bu belge, bir kuruluş stratejileri hakkında yönergeler beklenmedik kesintileri sırasında kilitleme riskini azaltmak için dayanıklılık sağlamak için benimseyin sağlar.

* 18 Eylül 2018'e, yayımlanan [kaynakları geçirmek için Azure Active Directory uygulamaları](../manage-apps/migration-resources.md)
   * Bu teknik incelemede, uygulama erişimi ve kimlik doğrulaması Azure Active Directory (Azure AD) geçirmenize yardımcı olacak kaynakların bir listesini içerir.

* 12 Temmuz 2018'den yayımlanan [Azure güvenlik ve uyumluluk planı: UK resmi iş yükleri için barındırma PaaS Web uygulaması](../../security/blueprints/ukofficial-paaswa-overview.md)
   * Azure bir Blueprint'i Kılavuzu belgeleri ve akreditasyon veya uyumluluk gereksinimlerine sahip senaryolar için çözümler sunmak için bulut tabanlı mimariler dağıtın Otomasyon şablonları oluşur.

## <a name="guidance-for-it-administrators"></a>BT yöneticileri için yönergeler

Oturum [Azure portalında](https://portal.azure.com/) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak. Başvurmak [Azure Active Directory'de Yönetici rolü izinleri.](../users-groups-roles/directory-assign-admin-roles.md)

Bir BT yöneticisi olarak kullanın [Azure AD koşullu erişim](overview.md) kullanıcıların Azure multi-Factor Authentication kullanarak kimlik doğrulaması gerektirmek için oturum bir güvenilen ağa veya güvenilir bir cihaz.

Başlamanıza yardımcı olan kullanışlı bağlantılar aşağıda verilmiştir:

* [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md)
* [Koşullu erişim ilkeleri'nden dışlanacak kullanıcıları yönetmek için kullanım Azure AD erişim gözden geçirmeleri](../governance/conditional-access-exclusion.md)
* [Nasıl Yapılır: Azure Active Directory'de koşullu erişim dağıtımınızı planlama](plan-conditional-access.md)
* [Hızlı Başlangıç: Azure Active Directory koşullu erişimiyle birlikte belirli uygulamalar için MFA gerektirme](app-based-mfa.md)
* [Hızlı Başlangıç: Bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektirir](require-tou.md)
* [Hızlı Başlangıç: Azure Active Directory koşullu erişim ile bir oturumu risk algılandığında erişimi engelleme](app-sign-in-risk.md)
* [Azure AD koşullu erişim ile ilgili SSS](faqs.md)
   * Başka sorularınız varsa da görüntüleyebilirsiniz [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&sort=relevancedesc&brandIgnore=True&searchTerm=password+reset+azure).
   * Destek ekiplerimiz her zaman bir sorunun yanıtını bulamazsanız, daha fazla yardımcı olmak kullanılabilir. Kullanım [Microsoft desteğine başvurun](../authentication/active-directory-passwords-troubleshoot.md#contact-microsoft-support).

### <a name="tutorials"></a>Öğreticiler

* [**Hızlı Başlangıç: Azure Active Directory koşullu erişimiyle birlikte belirli uygulamalar için MFA gerektirme**](app-based-mfa.md)
   * Bu hızlı başlangıçta, seçili bulut uygulaması ortamınızda için çok faktörlü kimlik doğrulaması gerektiren bir Azure AD koşullu erişim ilkesi yapılandırma gösterilmektedir.

* [**Hızlı Başlangıç: Bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektirir**](require-tou.md)
   * Bu hızlı başlangıçta, seçili bulut uygulaması ortamınızda için bir kullanım koşullarını kabul edilmesini gerektiren bir Azure AD koşullu erişim ilkesi yapılandırma gösterilmektedir.

* [**Hızlı Başlangıç: Azure Active Directory koşullu erişim ile bir oturumu risk algılandığında erişimi engelleme**](app-sign-in-risk.md)
   * Bu hızlı başlangıçta, bir oturum açma yapılandırılmış risk düzeyi algılandığında, bir oturum açma engelleyen bir koşullu erişim ilkesini yapılandırma gösterilmektedir.

* [Öğretici: **Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçişi**](policy-migration-mfa.md)
   * Bu öğreticide, bir bulut uygulaması için çok faktörlü kimlik doğrulaması (MFA) gerektiren bir Klasik ilke geçirme işlemi gösterilmektedir.

## <a name="end-user-readiness-and-communication"></a>Son kullanıcı hazırlığı ve iletişim

Koşullu erişim son kullanıcı deneyimini etkileyebilecek diğer Azure AD özelliklerini kullanır. Örneğin, kullanıcılar için güçlü kimlik doğrulamasını etkinleştirmek için Azure multi-Factor authentication kullanabilirsiniz. Bu durumda, Azure MFA'ın son kullanıcı şablonları kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtımınıza başlamak [koşullu erişim dağıtımını planlama belgelerini](plan-conditional-access.md).
