---
title: Azure Active Directory'de uygulama yönetimi için makale dizini | Microsoft Azure
description: Federasyon sertifikalarınızı sona erme tarihini özelleştirme ve yakında sona erecek sertifikaları yenile öğrenin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 9d398d810a2d43b3754fd8950376c605d4654f38
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621540"
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Azure Active Directory'de Uygulama Yönetimi için Makale Dizini
Bu sayfa, Azure Active Directory (Azure AD) çeşitli uygulama ile ilgili özellikler hakkında yazılan her belgenin kapsamlı bir listesini sağlar.

Her önemli özellik alanı yanı sıra aradığınız bağlı olarak hangi bilgileri okumak için hangi makaleleri hakkında yönergeler için kısa bir giriş yok.

## <a name="overview-articles"></a>Genel Bakış makalelerini
Aşağıdaki makaleleri iyi başlangıç noktaları isteyenler için yalnızca Azure AD uygulama yönetim özellikleri hakkında kısa bir açıklama.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure AD çözer uygulama yönetimi sorunlarını giriş |[Azure Active Directory (AD) ile uygulamaları yönetme](manage-apps/what-is-application-management.md) |
| Uygulamalara kimlerin erişebileceğini ve kullanıcıların uygulamaları nasıl başlatma tanımlayarak Azure AD'de çoklu oturum açma, etkinleştirme ile ilgili çeşitli özelliklere genel bakış |[Uygulama erişimi ve Azure Active Directory'de çoklu oturum açma](manage-apps/what-is-single-sign-on.md) |
| Uygulamaları, Azure AD ile tümleştirme işleminde kullanılan farklı adımlar atın |[Azure Active Directory ile uygulamaları tümleştirme](manage-apps/plan-an-application-integration.md)<br /><br />[SaaS uygulamalarında Çoklu oturum açma etkinleştiriliyor](manage-apps/configure-single-sign-on-portal.md)<br /><br />[Uygulamalara erişimi yönetme](manage-apps/what-is-access-management.md) |
| Uygulamaları Azure AD'de nasıl temsil edildiğini teknik bir açıklama |[Uygulamaları Azure AD'ye neden ve nasıl eklenir](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Sorun giderme makaleleri
Bu bölümde, ilgili sorun giderme kılavuzları, hızlı erişim sağlar. Bu sayfanın geri kalanını her bir özellik alanı hakkında daha fazla bilgi bulunabilir.

| Özellik alanı |  |
|:---:| --- |
| Federasyon çoklu oturum açma |[SAML tabanlı çoklu oturum açma sorunlarını giderme](develop/howto-v1-debug-saml-sso-issues.md) |
| Parola tabanlı çoklu oturum açma |[Erişim paneli uzantısını Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md) |
| Uygulama Ara Sunucusu |[Uygulama Ara sunucusu sorunlarını giderme kılavuzu](manage-apps/application-proxy-troubleshoot.md) |
| Şirket içi arasında çoklu oturum açma AD ile Azure AD |[Parola Karması eşitleme sorunlarını giderme](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md#troubleshoot-password-hash-synchronization)<br /><br />[Parola geri yazma sorunlarını giderme](authentication/active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Dinamik grup üyeliği |[Dinamik grup üyeliği sorunlarını giderme](users-groups-roles/groups-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Çoklu Oturum Açma (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Federasyon çoklu oturum açma: Tek bir kimlik kullanarak birçok uygulamaları'nda oturum açın
Çoklu oturum açma kimlik bilgileri yalnızca bir kümesi kullanan uygulamalara ve hizmetlere çeşitli erişmesine olanak tanır. Federasyon çoklu oturum açma etkinleştirebilirsiniz bir yöntemdir. Federasyon uygulamalarında oturum açmak kullanıcılar denediğinizde, bunlar Azure Active Directory tarafından işlenen, kuruluşun resmi oturum açma sayfasına yeniden ve sonra geri başarılı kimlik doğrulamadan sonra uygulamayı yönlendirilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Federasyon ve diğer türleri oturum açma giriş |[Azure AD ile çoklu oturum açma](manage-apps/what-is-single-sign-on.md) |
| Binlerce Azure ad ile önceden tümleştirilmiş SaaS uygulamasını tek oturum açma yapılandırma adımları Basitleştirilmiş |[Azure AD uygulama Galerisi'ni kullanmaya başlama](manage-apps/what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Federasyon destekleyen önceden tümleştirilmiş uygulamaların tam listesi](saas-apps/tutorial-list.md)<br /><br />[Bir Azure AD uygulama galerisinde uygulamanızı ekleme](develop/howto-app-gallery-listing.md) |
| 150'den fazla uygulama öğreticileri nasıl yapılandırılacağı hakkında çoklu oturum açma uygulamaları gibi [Salesforce](saas-apps/salesforce-tutorial.md), [ServiceNow](saas-apps/servicenow-tutorial.md), [Google Apps](saas-apps/google-apps-tutorial.md), [Workday](saas-apps/workday-tutorial.md)ve çok daha fazlası |[SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](saas-apps/tutorial-list.md) |
| Nasıl el ile ayarlama ve özelleştirme, çoklu oturum açma yapılandırması |[İçin yapılandırma Federasyon çoklu oturum açma Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için nasıl](application-config-sso-how-to-configure-federated-sso-non-gallery.md)<br /><br />[Önceden tümleştirilmiş uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](active-directory-saml-claims-customization.md) |
| SAML protokolü kullanan Federasyon uygulamaları için sorun giderme kılavuzu |[SAML tabanlı çoklu oturum açma sorunlarını giderme](develop/howto-v1-debug-saml-sso-issues.md) |
| Uygulamanızın sertifikanın sona erme tarihi yapılandırma ve sertifikalarınızı yenileme |[Federasyon çoklu oturum açma için Azure Active Directory'de sertifikaları yönetme](manage-apps/manage-certificates-for-federated-single-sign-on.md) |

Federasyon çoklu oturum açma için kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümlerinde kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamalarını destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), daha sonra [Federasyon uygulamalarına erişim atamak için grupları kullanma](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Parola tabanlı çoklu oturum açma: hesap paylaşımı ve Federasyon olmayan uygulamalar için SSO
Federasyon desteklemeyen uygulamalar için çoklu oturum açmayı etkinleştirmek için Azure AD SaaS uygulamaları için parolaları güvenli bir şekilde saklayın ve otomatik olarak kullanıcılar söz konusu uygulamalarda oturum parola yönetimi özellikleri sunar. Kolayca yeni oluşturulan hesaplar için kimlik bilgilerini dağıtmak ve ekip hesapları birden çok kişilerle paylaşma. Kullanıcıların mutlaka erişim verilir hesaplar için kimlik bilgilerini bilmeniz gerekmez.

| Makale Kılavuzu |  |
|:---:| --- |
| Giriş nasıl parola tabanlı SSO çalışır ve kısa bir teknik genel bakış |[Parola tabanlı çoklu oturum açma Azure AD ile](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on) |
| Hesap paylaşımı ve Azure AD tarafından bu sorunları nasıl çözülür ilgili senaryoları özeti |[Azure AD ile hesapları paylaşma](active-directory-sharing-accounts.md) |
| Belirli uygulamalar için parola düzenli aralıklarla otomatik olarak değiştir |[Otomatik parola geçişi (Önizleme)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Dağıtımı ve sorun giderme kılavuzlarının Azure AD parola yönetimi uzantısı Internet Explorer sürümü |[Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](active-directory-saas-ie-group-policy.md)<br /><br />[Erişim paneli uzantısını Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md) |

Parola tabanlı çoklu oturum açma için kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümlerinde kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamalarını destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), daha sonra [uygulamalara erişim atamak için grupları kullanma](#managing-access-to-applications). Otomatik parola geçişi olduğu bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

### <a name="app-proxy-single-sign-on-and-remote-access-to-on-premises-applications"></a>Uygulama Ara sunucusu: Şirket içi uygulamalar için çoklu oturum açma ve Uzaktan erişim
Özel ağınızdaki kullanıcılara ve cihazlara Ağ dışından tarafından erişilmesi gereken uygulamalarınız varsa, bu uygulamalara güvenli uzaktan erişim'i etkinleştirmek için Azure AD uygulama ara sunucusu kullanabilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure AD uygulama ara sunucusu ve nasıl çalıştığı genel bakış |[Şirket içi uygulamalara güvenli uzaktan erişim sağlama](manage-apps/application-proxy.md) |
| Öğreticiler ve uygulama proxy'sini yapılandırmak nasıl ilk uygulamanızı yayımlama |[Azure AD uygulama ara sunucusu ayarlama](manage-apps/application-proxy-enable.md)<br /><br />[Sessiz bir şekilde uygulama ara sunucusu Bağlayıcısı'nı yükleme](manage-apps/application-proxy-register-connector-powershell.md)<br /><br />[Nasıl yapılır uygulama ara sunucusu kullanarak uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md)<br /><br />[Kendi etki alanı adı kullanma](manage-apps/application-proxy-configure-custom-domain.md) |
| Uygulamalar için çoklu oturum açma ve koşullu erişimi etkinleştirmek nasıl uygulama ara sunucusu ile yayımlanan |[Çoklu oturum açma uygulama ara sunucusu ile](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)<br /><br />[Koşullu erişim ve uygulama proxy'si](manage-apps/application-proxy-integrate-with-sharepoint-server.md) |
| Aşağıdaki senaryolar için uygulama proxy'si kullanma Kılavuzu |[Yerel istemci uygulamaları desteklemek nasıl](manage-apps/application-proxy-configure-native-client-application.md)<br /><br />[Talep kullanan uygulamalar destekleme](manage-apps/application-proxy-configure-for-claims-aware-applications.md)<br /><br />[Ayrı ağlarda ve konumları yayımlanan uygulamaları desteklemek nasıl](manage-apps/application-proxy-connector-groups.md) |
| Uygulama proxy'si için sorun giderme kılavuzu |[Uygulama Ara sunucusu sorunlarını giderme kılavuzu](manage-apps/application-proxy-troubleshoot.md) |

Uygulama Ara sunucusu, kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümlerinde kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamalarını destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), daha sonra [uygulamalara erişim atamak için grupları kullanma](#managing-access-to-applications).

De ilginizi çekebilir [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), yine de bu uygulamaların kimlik ihtiyaçlarını uyarlarken, şirket içi uygulamaları azure'a geçirme sağlar.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Azure AD arasında çoklu oturum açmayı etkinleştirme ve şirket içi AD
Kuruluşunuzda kullanılan şirket içi bulut Azure Active Directory ile birlikte Windows Server Active Directory, ardından, büyük olasılıkla bu iki sistem arasında çoklu oturum açmayı etkinleştirmek isteyebilirsiniz. Azure AD Connect'i (birlikte bu iki sistemleri tümleştiren aracı), çoklu oturum açmayı ayarlamak için birden çok seçenek sunar: AD FS veya başka bir Federasyon sağlayıcı Federasyon kurmak veya parola eşitlemeyi etkinleştirme.

| Makale Kılavuzu |  |
|:---:| --- |
| Hibrit ortamları yönetme hakkında bilgi yanı sıra, Azure AD Connect çoklu oturum açma seçenekleri hakkında genel bir bakış sunulan |[Kullanıcı oturum açma seçenekleri Azure ad Connect](active-directory-aadconnect-user-signin.md) |
| Hem ortamlarını yönetmek için genel kılavuz Active Directory ve Azure Active Directory şirket |[Azure AD karma kimlik tasarım konuları](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) |
| SSO'yu etkinleştirmek için parola eşitlemeyi kullanma yönergeleri |[Azure AD ile parola eşitlemeyi uygulama bağlama](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)<br /><br />[Parola eşitleme sorunlarını giderme](https://support.microsoft.com/en-us/kb/2855271) |
| SSO'yu etkinleştirmek için parola geri yazma özelliğini kullanma yönergeleri |[Azure AD'de parola yönetimine Başlarken](authentication/quickstart-sspr.md)<br /><br />[Parola Geri Yazma Sorunlarını Giderme](authentication/active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| SSO'yu etkinleştirmek için üçüncü taraf kimlik sağlayıcıları kullanma yönergeleri |[Çoklu oturum açmayı etkinleştirmek için kullanılan uyumlu üçüncü taraf kimlik sağlayıcıları listesi](https://aka.ms/ssoproviders) |
| Windows 10 kullanıcıları Azure AD'ye katılım aracılığıyla çoklu oturum açmayı avantajlarından nasıl yararlanabilirsiniz |[10 cihaz Azure Active Directory aracılığıyla bulut işlevlerini Windows genişletme katılın](active-directory-azureadjoin-overview.md) |

Azure AD Connect, kullanılabilir [Azure Active Directory'nin tüm sürümlerinde](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD Self Servis parola sıfırlama, kullanılabilir [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Parola geri yazma şirket için AD, bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Koşullu erişim: yüksek riskli uygulamalar için ek güvenlik gereksinimleri zorla
Çoklu oturum açma uygulamaları ve kaynaklarına ayarladıktan sonra ardından daha duyarlı uygulamalar her oturum açma bu uygulama için belirli güvenlik gereksinimlerine zorlayarak güvenliğini sağlayabilirsiniz. Örneğin, belirli bir uygulamanın tüm erişim her zaman multi-Factor authentication, uygulama işlevselliğini innately destekleyip desteklemediğini gerekli talep Azure AD'ye kullanabilirsiniz. Başka bir yaygın koşullu erişim örneği özellikle duyarlı bir uygulamaya erişmek için kullanıcılar kuruluşun güvenilen bir ağa bağlanması gerektirmektir.

| Makale Kılavuzu |  |
|:---:| --- |
| Koşullu erişim özelliklerini giriş, Azure AD genelinde sunulan Office 365 ve Intune |[Koşullu erişim ile risk yönetme](active-directory-conditional-access-azure-portal.md) |
| Aşağıdaki türdeki kaynakları için koşullu erişimi etkinleştirme |[SaaS uygulamaları için koşullu erişim](conditional-access/app-based-conditional-access.md)<br /><br />[Office 365 Hizmetleri için koşullu erişim](active-directory-conditional-access-device-policies.md)<br /><br />[Şirket içi uygulamalar için koşullu erişim](active-directory-conditional-access-azure-portal.md)<br /><br />[Azure AD uygulama ara sunucusu üzerinden şirket içi uygulamalar için koşullu erişim yayımlandı](manage-apps/application-proxy-integrate-with-sharepoint-server.md) |
| Cihaz tabanlı koşullu erişim ilkelerini etkinleştirmek için Azure Active Directory ile cihazları kaydetme |[Azure Active Directory cihaz kaydına genel bakış](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Etki alanı için otomatik cihaz kaydını etkinleştirme katılmış Windows cihazlar](active-directory-conditional-access-automatic-device-registration.md)<br />— [Adımları için Windows 8.1 cihazları](active-directory-conditional-access-automatic-device-registration-setup.md)<br />— [Adımları Windows 7 cihazları için](active-directory-conditional-access-automatic-device-registration-setup.md) |

| İki aşamalı doğrulama için Microsoft Authenticator uygulamasını kullanma | [Microsoft Authenticator](user-help/microsoft-authenticator-app-how-to.md) |

Koşullu erişim bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

## <a name="apps--azure-ad"></a>Uygulamalar ve Azure AD
### <a name="cloud-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud Discovery'yi: kuruluşunuzda hangi SaaS uygulamalarında kullanılan Bul
Microsoft Cloud App Security bulut uygulaması içeren kataloğuna göre derecelendirilmiş ve puanlanmış uygulamaları 70'ten fazla risk unsurlarına göre 16. 000'den fazla bulut trafik günlüklerinizi cloud Discovery'yi çözümler, sağlamak için bulut sürekli görünürlük kullanın, gölge BT ve riski Gölge BT tümleştirilmesi kuruluşunuz.

| Makale Kılavuzu |  |
|:---:| --- |
| Nasıl çalıştığına ilişkin genel bir bakış |[Cloud discovery'yi ayarlama](/cloud-app-security/set-up-cloud-discovery) |


### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Otomatik olarak sağlama ve sağlamasını kaldırma kullanıcı hesapları, SaaS uygulamaları
Oluşturma, Bakım ve Dropbox, Salesforce, ServiceNow ve diğer SaaS uygulamalarına kullanıcı kimliklerini kaldırılmasını otomatik hale getirin. Eşleşen ve mevcut Azure AD kimlikleri eşitleyin ve SaaS uygulamalarınıza ve hesapları otomatik olarak devre dışı bıraktığınızda kullanıcılar kuruluş tarafından erişimi denetleme.

| Makale Kılavuzu |  |
|:---:| --- |
| Nasıl çalıştığı hakkında bilgi edinin ve sık sorulan soruların yanıtlarını bulun |[Sağlama ve sağlamayı kaldırma SaaS uygulamalarına kullanıcı otomatikleştirin](active-directory-saas-app-provisioning.md) |
| Yapılandırma bilgileri Azure AD arasında nasıl eşlendiğini ve SaaS uygulamanız |[Öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Öznitelik eşlemeleri için ifadeler yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| SCIM protokolünü destekleyen herhangi bir uygulama için otomatik sağlamayı etkinleştirme hakkında |[Otomatik kullanıcı hazırlama hiçbir SCIM-Enabled uygulamaya ayarlama](manage-apps/use-scim-to-provision-users-and-groups.md) |
| Raporlama ve kullanıcı hazırlama sorunlarını giderme |[Otomatik kullanıcı hazırlama raporlama](active-directory-saas-provisioning-reporting.md)<br><br>[Kullanıcı hazırlama sorunlarını giderme](active-directory-application-provisioning-content-map.md) |
| Sağlanan öznitelik değerlerine göre bir uygulama için sınırı |[Kapsam filtreleri](active-directory-saas-scoping-filters.md) |

Otomatik kullanıcı hazırlama, kullanıcı başına en fazla on uygulamaları için Azure AD, tüm sürümleri için kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamalarını destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), daha sonra [hangi kullanıcıların sağlanan ' ı yönetmek için grupları kullanma](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Azure AD ile tümleştirilen uygulamalar oluşturma
Kuruluşunuz geliştirirken veya satır iş kolu (LoB) uygulamalarının bakımını yapma ya da siz Azure Active Directory kullanan müşteriler ile bir uygulama geliştiricisi gibiyseniz, aşağıdaki öğreticilerde yardımcı olacak Azure AD ile uygulamalarınızı tümleştirin.

| Makale Kılavuzu |  |
|:---:| --- |
| BT uzmanları ve uygulamaları Azure AD ile tümleştirme, uygulama geliştiricilere yönelik kılavuz |[BT profesyonelleri kılavuzu için Azure AD uygulamaları geliştirme](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[Azure Active Directory Geliştirici Kılavuzu](develop/azure-ad-developers-guide.md) |
| Nasıl uygulama satıcıları uygulamalarını bir Azure AD uygulama galerisinde ekleyebilirsiniz |[Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](develop/howto-app-gallery-listing.md) |
| Azure Active Directory'yi kullanarak geliştirilen uygulamalara erişimi yönetme |[Kullanıcı Ataması geliştirilen uygulamalar için etkinleştirme](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Uygulamanız için kullanıcı atama](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Uygulamanıza grup atama](active-directory-applications-guiding-developers-assigning-groups.md) |

Tüketiciye yönelik uygulamalar geliştiriyorsanız, kullanarak ilginizi çekebilir [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) , kullanıcılarınızı yönetmek için kendi kimlik sistemi geliştirmek zorunda kalmazsınız. [Daha fazla bilgi edinin](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-to-applications"></a>Uygulamalara erişimi yönetme
### <a name="using-groups-and-self-service-to-manage-who-has-access-to-which-apps"></a>Grupları ve hangi uygulamaların erişimi olan yönetmek için Self Servis kullanma
Kimlerin hangi kaynaklara erişimi olması gereken yönetmenize yardımcı olmak için Azure Active Directory grupları kullanarak uygun ölçekte atamaları ve izinleri ayarlamanızı sağlar. BT Self Servis özellikleri etkinleştirin; böylelikle kullanıcılar yalnızca izin isteyebilir gerektiğinde tercih edebilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure AD erişim yönetim özelliklerine genel bakış |[Uygulamalara erişimi yönetme giriş](manage-apps/what-is-access-management.md)<br /><br />[Azure AD'de erişim yönetimi nasıl çalışır?](fundamentals/active-directory-manage-groups.md)<br /><br />[SaaS uygulamalarına erişimi yönetmek için grupları kullanma](users-groups-roles/groups-saasapps.md) |
| Self Servis Yönetimi grupları ve uygulamaları etkinleştirin |[Self Servis uygulama yönetimi](active-directory-self-service-application-access.md)<br /><br />[Self Servis Grup Yönetimi](users-groups-roles/groups-self-service-management.md) |
| Azure AD'de kendi gruplarını ayarlama yönergeleri |[Güvenlik grupları oluşturma](fundamentals/active-directory-groups-create-azure-portal.md)<br /><br />[Nasıl bir grubun sahiplerini atamak için](fundamentals/active-directory-accessmanagement-managing-group-owners.md)<br /><br />["Tüm kullanıcılar" grubunu kullanma](active-directory-accessmanagement-dedicated-groups.md) |
| Öznitelik tabanlı üyelik kurallarını kullanarak grup üyeliğini otomatik olarak doldurmak için dinamik grupları kullanma |[Dinamik grup üyeliği: Gelişmiş kurallar](active-directory-groups-dynamic-membership-azure-portal.md)<br /><br />[Dinamik grup üyeliği sorunlarını giderme](users-groups-roles/groups-troubleshooting.md) |

Grup tabanlı uygulama erişim yönetimi kullanılabilir [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Self Servis Grup Yönetimi, Self Servis uygulama yönetimi ve dinamik gruplar [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özellikleri.

### <a name="b2b-collaboration-enable-partner-access-to-applications"></a>B2B işbirliği: iş ortağı uygulamalarına erişimini etkinleştir
Diğer şirketlerle iş ortaklığı kurdu, Kurumsal uygulamalarınız için iş ortağı erişimini yönetmek için ihtiyacınız olasıdır. Azure Active Directory B2B işbirliği, iş ortakları ile uygulamalarınızı paylaşma için kolay ve güvenli bir yol sağlar.

| Makale Kılavuzu |  |
|:---:| --- |
| Farklı Azure AD genel bir bakış dış kullanıcılar gibi yönetmenize yardımcı iş ortakları, müşterilerin, vb. özellikler. |[Dış kimlikler Azure AD'de yönetmek için özellikleri karşılaştırma](active-directory-b2b-compare-external-identities.md) |
| B2B işbirliği ve kullanmaya nasıl başlayacağınızı giriş |[Basit, güvenli, Azure AD ile bulut iş ortağı tümleştirmesi](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Azure Active Directory B2B işbirliği](active-directory-b2b-collaboration-overview.md) |
| Azure AD B2B işbirliği ve nasıl kullanılacağını derinlemesine |[B2B işbirliği: Nasıl çalışır?](active-directory-b2b-how-it-works.md)<br /><br />[Azure AD B2B işbirliği, geçerli sınırlamalar](active-directory-b2b-current-limitations.md)<br /><br />[Azure AD B2B işbirliğini kullanmaya yönelik ayrıntılı kılavuz](active-directory-b2b-detailed-walkthrough.md) |
| Başvuru makalelerimize ile Azure AD B2B işbirliği nasıl çalıştığı hakkında teknik ayrıntılar |[İş ortağı kullanıcılar eklemek için CSV dosya biçimi](active-directory-b2b-references-csv-file-format.md)<br /><br />[Azure AD B2B işbirliği tarafından etkilenen kullanıcı öznitelikleri](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[İş ortağı kullanıcılar için kullanıcı belirteci biçimi](active-directory-b2b-references-external-user-token-format.md) |

B2B işbirliği, şu anda kullanılabilir [Azure Active Directory'nin tüm sürümlerinde](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Erişim paneli: uygulamalar ve Self Servis özellikler erişmek için bir portal
Azure AD erişim paneli, burada son kullanıcıların uygulamalarını başlatmak ve kullanıcılar uygulamaları ve grup üyeliklerini yönetmenize olanak tanıyan Self Servis özelliklerine erişimi girebiliriz. Erişim paneli ek olarak, aşağıdaki listede SSO etkin uygulamalara erişim için diğer seçenekleri dahil edilir.

| Makale Kılavuzu |  |
|:---:| --- |
| Kullanıcılara çoklu oturum açma uygulamaları dağıtmak için kullanılabilir farklı seçenekler karşılaştırması |[Dağıtma Azure AD tümleşik uygulamalarını kullanıcılara](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) |
| Erişim paneli ve kendi mobil eşdeğer MyApps genel bakış |[Erişim paneli ve MyApps giriş](user-help/active-directory-saas-access-panel-introduction.md)<br />— [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Azure AD uygulamaları Office 365 Web sitesinden erişme |[Office 365 uygulama Başlatıcısı'nı kullanma](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Azure AD uygulamaları Intune Managed Browser mobil uygulamasından erişme |[Intune yönetilen tarayıcı](https://technet.microsoft.com/library/dn878029.aspx)<br />— [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Çoklu oturum açma başlatmak için ayrıntılı Bağlantılar'ı kullanarak Azure AD uygulamalarına erişmek nasıl |[Oturum açma doğrudan bağlantılar uygulamalarınıza alma](manage-apps/what-is-single-sign-on.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Erişim paneli, kullanılabilir [Azure Active Directory'nin tüm sürümlerinde](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-to-apps"></a>Raporları: Kolayca uygulama erişim değişiklikleri denetleyebilir ve oturum açma uygulamaları izleme
Azure Active Directory, çeşitli raporlar ve uygulamalar, kuruluşunuzun erişimi izlemenize yardımcı olması için uyarılar sağlar. Uygulamalarınıza anormal oturum açma için uyarıları alabilir ve ne zaman ve neden kullanıcıların bir uygulamaya erişim değişti izleyebilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure Active Directory raporlama özelliklerine genel bakış |[Başlarken Azure AD raporlama](active-directory-reporting-getting-started.md) |
| Oturum açma ve kullanıcılarınızın uygulama kullanımını izleme |[Erişim ve kullanım raporlarını görüntüleyin](active-directory-view-access-usage-reports.md) |
| Belirli bir uygulamaya erişebilecek yapılan değişiklikleri izleyin |[Azure Active Directory Denetim Raporu olayları](active-directory-reporting-audit-events.md) |
| Bu raporların verileri raporlama API'sini kullanarak tercih edilen araçlarınıza dışarı aktarma |[Azure AD raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) |

Hangi raporların Azure Active Directory farklı sürümlerine dahil olduğunu görmek için [Buraya](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Ayrıca bkz.
[Azure Active Directory nedir?](fundamentals/active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure Active Directory etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/)

[Azure çok faktörlü kimlik doğrulaması](https://azure.microsoft.com/services/multi-factor-authentication/)
