---
title: "Azure Active Directory'de uygulama yönetimi için makale dizini | Microsoft Azure"
description: "Federasyon sertifikalarınızı sona erme tarihini özelleştirmeyi ve süresi yakında dolacak sertifikaları yenilemek nasıl öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 5321b8e4-2afa-4dfe-8d53-4add7abb5ec8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: d8ed395abb31a1cb41e35456ab5892a2e7c3a750
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Azure Active Directory'de Uygulama Yönetimi için Makale Dizini
Bu sayfa, Azure Active Directory (Azure AD) çeşitli uygulama ile ilgili özellikler hakkında yazılan her belge kapsamlı bir listesini sağlar.

Her ana özellik alanı yanı sıra hangi bilgilere bağlı olarak, aradığınız okumak için hangi makaleleri hakkında yönergeler için kısa bir giriş yok.

## <a name="overview-articles"></a>Genel Bakış makaleleri
Aşağıdaki makaleleri iyi yalnızca Azure AD uygulama yönetimi özelliklerinden kısa bir açıklama istediğiniz olanlar için başlangıç noktaları.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure AD çözdü uygulama yönetimi sorunları giriş |[Azure Active Directory (AD) ile uygulamaları yönetme](active-directory-enable-sso-scenario.md) |
| Uygulamalara kimlerin erişebileceğini ve kullanıcıların uygulamaları nasıl başlatma tanımlama Azure ad çoklu oturum açma, etkinleştirme ile ilgili çeşitli özelliklere genel bakış |[Uygulama erişimi ve Azure Active Directory'de çoklu oturum açma](active-directory-appssoaccess-whatis.md) |
| Farklı adımlar uygulamaları Azure AD ile tümleştirdiğinizde bakma |[Azure Active Directory uygulamaları ile tümleştirme](active-directory-integrating-applications-getting-started.md)<br /><br />[Çoklu oturum açma SaaS uygulamaları için etkinleştirme](active-directory-enterprise-apps-manage-sso.md)<br /><br />[Uygulamalara erişimi yönetme](active-directory-managing-access-to-apps.md) |
| Uygulamaları Azure AD'de nasıl temsil edildiğini bir teknik açıklama |[Neden ve nasıl uygulamaları için Azure AD eklenir](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Sorun giderme makaleleri
Bu bölümde, ilgili sorun giderme kılavuzları hızlı erişim sağlar. Bu sayfaya geri kalanı her özellik alanı hakkında daha fazla bilgi bulunabilir.

| Özellik alanı |  |
|:---:| --- |
| Federasyon çoklu oturum açma |[Sorun giderme SAML tabanlı çoklu oturum açma](active-directory-saml-debugging.md) |
| Parola tabanlı çoklu oturum açma |[Erişim paneli uzantısı Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md) |
| Uygulama Proxy'si |[Uygulama Proxy sorun giderme kılavuzu](active-directory-application-proxy-troubleshoot.md) |
| Çoklu oturum açma şirket içi arasında AD ve Azure AD |[Parola eşitleme sorunlarını giderme](connect/active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)<br /><br />[Parola geri yazma sorunlarını giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Dinamik grup üyelikleri |[Dinamik grup üyeliklerini sorunlarını giderme](active-directory-accessmanagement-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Çoklu Oturum Açma (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Federasyon çoklu oturum açma: Oturum bir kimlik kullanarak birçok uygulamaları açın
Çoklu oturum açma kullanıcıların çeşitli uygulamaları ve Hizmetleri yalnızca bir kimlik bilgileri kümesi kullanarak erişmesine izin verir. Federasyon, çoklu oturum açma etkinleştirebilirsiniz bir yöntemdir. Birleştirilmiş uygulamalarda oturum açmak kullanıcılar denediğinizde, bunlar Azure Active Directory tarafından işlenen kuruluşun resmi oturum açma sayfasına yönlendirilirsiniz ve sonra geri başarılı kimlik doğrulama sırasında uygulama yönlendirilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Federasyon ve diğer türleri oturum açma giriş |[Azure AD ile çoklu oturum açma](active-directory-appssoaccess-whatis.md) |
| SaaS uygulamaları ile Azure AD ile önceden tümleştirilmiş binlerce Basitleştirilmiş tek oturum açma yapılandırma adımları |[Azure AD uygulama Galerisi'ni kullanmaya başlama](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Federasyon desteği önceden tümleştirilmiş uygulamaların tam listesi](http://aka.ms/aadfederatedapps)<br /><br />[Azure AD uygulama galerisinde uygulamanızı ekleme](active-directory-app-gallery-listing.md) |
| 150'den fazla uygulama öğreticileri nasıl yapılandırılacağı hakkında çoklu oturum açmayı uygulamalar gibi [Salesforce](active-directory-saas-salesforce-tutorial.md), [ServiceNow](active-directory-saas-servicenow-tutorial.md), [Google Apps](active-directory-saas-google-apps-tutorial.md), [Workday](active-directory-saas-workday-tutorial.md)ve çok daha fazlası |[Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md) |
| El ile ayarlamak ve çoklu oturum açma yapılandırmanızı özelleştirmek nasıl |[Nasıl için yapılandırma federe çoklu oturum açma, Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar](application-config-sso-how-to-configure-federated-sso-non-gallery.md)<br /><br />[Önceden tümleştirilen uygulamalar için SAML belirtecinde verilen talepler özelleştirme](active-directory-saml-claims-customization.md) |
| SAML protokolünü kullanan Federasyon uygulamaları için sorun giderme kılavuzu |[Sorun giderme SAML tabanlı çoklu oturum açma](active-directory-saml-debugging.md) |
| Uygulamanızın sertifikanın sona erme tarihini yapılandırma ve sertifikalarınızı yenilemek için |[Federasyon tek oturum açma için Azure Active Directory'de sertifikaları yönetme](active-directory-sso-certs.md) |

Federasyon çoklu oturum açma için kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümlerinde kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamaları destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), yapabilecekleriniz sonra [Federasyon uygulamalarına erişim atamak için grupları kullanma](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Parola tabanlı çoklu oturum açma: hesap paylaşımı ve SSO Federasyon olmayan uygulamalar için
Federasyon desteklemeyen uygulamalar için çoklu oturum açmayı etkinleştirmek için Azure AD parolalarını SaaS uygulamaları için güvenli bir şekilde depolamak ve otomatik olarak kullanıcılar konusu uygulamalarda oturum parola yönetimi özellikleri sunar. Kolayca yeni oluşturulan hesaplar için kimlik bilgilerini dağıtmak ve takım hesapları birden çok kişiyle paylaşabilirsiniz. Gerekmeyen kullanıcılar, erişimi verilir hesapları için kimlik bilgilerini bilmeniz gerek yoktur.

| Makale Kılavuzu |  |
|:---:| --- |
| SSO works nasıl parola tabanlı bir giriş ve kısa bir teknik genel bakış |[Parola tabanlı çoklu oturum açma Azure AD ile](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| Hesap paylaşımı ve bu sorunları Azure AD tarafından nasıl çözülür ilgili senaryoları özeti |[Azure AD ile hesapları paylaşma](active-directory-sharing-accounts.md) |
| Belirli uygulamalar için parola düzenli aralıklarla otomatik olarak değiştir |[Otomatik parola Rollover (Önizleme)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Dağıtım ve Azure AD parola yönetimi uzantısı Internet Explorer sürümüne yönelik Kılavuzlar sorun giderme |[Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtma](active-directory-saas-ie-group-policy.md)<br /><br />[Erişim paneli uzantısı Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md) |

Parola tabanlı çoklu oturum açma için kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümlerinde kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamaları destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), yapabilecekleriniz sonra [uygulamalara erişim atama için grupları kullanma](#managing-access-to-applications). Otomatik parola geçiş bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

### <a name="app-proxy-single-sign-on-and-remote-access-to-on-premises-applications"></a>Uygulama Ara sunucusu: Şirket içi uygulamalar için çoklu oturum açma ve Uzaktan erişim
Özel ağınızdaki kullanıcılara ve cihazlara Ağ dışından tarafından erişilmesi gereken uygulamalarınız varsa, bu uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulama proxy'si kullanabilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure AD uygulama proxy'si ve nasıl çalıştığı genel bakış |[Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md) |
| Uygulama proxy'si nasıl yapılandırılır ve ilk uygulamanızı yayımlamak nasıl öğreticileri |[Azure AD uygulama ara sunucusu kurma](active-directory-application-proxy-enable.md)<br /><br />[Uygulama Ara sunucusu Bağlayıcısı sessiz yükleme](active-directory-application-proxy-silent-installation.md)<br /><br />[Nasıl yapılır uygulama ara sunucusu kullanarak uygulama yayımlama](active-directory-application-proxy-publish.md)<br /><br />[Kendi etki alanı adınızı kullanma](active-directory-application-proxy-custom-domains.md) |
| Uygulamalar için çoklu oturum açma ve koşullu erişimi etkinleştirmek nasıl uygulama ara sunucusu ile yayımlanan |[Çoklu oturum açma uygulama proxy'si ile uygulama](active-directory-application-proxy-sso-using-kcd.md)<br /><br />[Koşullu erişim ve uygulama proxy'si](application-proxy-enable-remote-access-sharepoint.md) |
| Aşağıdaki senaryolar için uygulama proxy'si kullanma hakkında yönergeler |[Yerel istemci uygulamaları destekleme](active-directory-application-proxy-native-client.md)<br /><br />[Talep kullanan uygulama destekleme](active-directory-application-proxy-claims-aware-apps.md)<br /><br />[Ayrı ağlar ve konumları yayımlanan uygulamalar destekleme](active-directory-application-proxy-connectors-azure-portal.md) |
| Uygulama proxy'si için sorun giderme kılavuzu |[Uygulama Proxy sorun giderme kılavuzu](active-directory-application-proxy-troubleshoot.md) |

Uygulama proxy'si, kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümleri için kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamaları destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), yapabilecekleriniz sonra [uygulamalara erişim atama için grupları kullanma](#managing-access-to-applications).

De ilginizi çekebilir [Azure AD etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-overview.md), yine söz konusu uygulamaların kimlik gereksinimlerini çağıran çalışırken, şirket içi uygulamalarınızı Azure geçirmek sağlar.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Çoklu oturum açma Azure AD arasında etkinleştirme ve şirket içi AD
Kuruluşunuzun şirket içi bulut Azure Active Directory'yi birlikte Windows Server Active Directory koruyorsa, sonra büyük olasılıkla bu iki sistem arasında çoklu oturum açmayı etkinleştirmek istediğiniz. Azure AD Connect (Bu iki sistemleri birlikte tümleştirir aracı), çoklu oturum açmayı ayarlama için birden çok seçenek sunar: ADFS veya başka bir Federasyon sağlayıcı Federasyon kurmak veya parola eşitlemeyi etkinleştir.

| Makale Kılavuzu |  |
|:---:| --- |
| Karma ortamlar yönetme hakkında bilgi yanı sıra, Azure AD Connect tek oturum açma seçenekleri hakkında genel bir bakış sunulur |[Kullanıcı oturum açma seçenekleri Azure AD'de Bağlan](active-directory-aadconnect-user-signin.md) |
| Her ikisi de ortamlarıyla yönetmek için genel rehberlik Active Directory ve Azure Active Directory şirket içi |[Azure AD karma kimlik tasarımı hakkında dikkat edilecek noktalar](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) |
| SSO'yu etkinleştirmek için parola eşitleme kullanarak Kılavuzu |[Azure AD ile parola eşitlemeyi uygulama Bağlan](active-directory-aadconnectsync-implement-password-synchronization.md)<br /><br />[Parola eşitleme sorunlarını giderme](https://support.microsoft.com/en-us/kb/2855271) |
| SSO'yu etkinleştirmek için parola geri yazma özelliğini kullanma konusunda yönergeler |[Azure AD'de parola yönetimine Başlarken](active-directory-passwords-getting-started.md)<br /><br />[Parola Geri Yazma Sorunlarını Giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| SSO'yu etkinleştirmek için üçüncü taraf kimlik sağlayıcıları kullanma konusunda yönergeler |[Çoklu oturum açmayı etkinleştirmek için kullanılan uyumlu üçüncü taraf kimlik sağlayıcıları listesi](https://aka.ms/ssoproviders) |
| Windows 10 kullanıcıları Azure AD katılım aracılığıyla çoklu oturum açmaya yararları nasıl keyfini çıkarabilirsiniz |[10 cihaz Azure Active Directory üzerinden katılmak Windows bulut özelliklerini genişletme](active-directory-azureadjoin-overview.md) |

Azure AD Connect için kullanılabilir [Azure Active Directory'nin tüm sürümlerinde](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD Self Servis parola sıfırlama için kullanılabilir [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Parola geri yazma AD şirket içi için olan bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Koşullu erişim: yüksek riskli uygulamalar için ek güvenlik gereksinimleri uygulama
Çoklu oturum açma uygulamalar ve kaynaklar ayarladıktan sonra ardından daha duyarlı uygulamalar her oturum açma bu uygulama için belirli güvenlik gereksinimlerine zorlayarak güvenliğini sağlayabilirsiniz. Örneğin, belirli bir uygulamanın tüm erişim her zaman bu uygulama bu işlevselliği innately destekleyip desteklemediğini bağımsız olarak çok faktörlü kimlik doğrulaması gerektiren isteğe bağlı Azure AD'ye kullanabilirsiniz. Başka bir ortak koşullu erişim özellikle duyarlı bir uygulamaya erişmek için kullanıcıların kuruluşun güvenilen ağa bağlı gerektirecek şekilde örnektir.

| Makale Kılavuzu |  |
|:---:| --- |
| Koşullu erişim yetenekleri giriş sunulan Azure AD arasında Office365 ve Intune |[Koşullu erişim ile risk yönetme](active-directory-conditional-access-azure-portal.md) |
| Aşağıdaki türdeki kaynakları için koşullu erişimi etkinleştirme |[SaaS uygulamaları için koşullu erişim](active-directory-conditional-access-azure-portal-get-started.md)<br /><br />[Office 365 Hizmetleri için koşullu erişim](active-directory-conditional-access-device-policies.md)<br /><br />[Şirket içi uygulamalar için koşullu erişim](active-directory-conditional-access-azure-portal.md)<br /><br />[Azure AD uygulama proxy'si şirket içi uygulamalar için koşullu erişim yayımlanan](application-proxy-enable-remote-access-sharepoint.md) |
| Cihaz temelli koşullu erişim ilkelerini etkinleştirmek için Azure Active Directory ile cihazları kaydetme |[Azure Active Directory cihaz kaydı genel bakış](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Etki alanı için otomatik cihaz kaydını etkinleştirme katılmış Windows cihazlarda](active-directory-conditional-access-automatic-device-registration.md)<br />— [Adımları için Windows 8.1 cihazları](active-directory-conditional-access-automatic-device-registration-setup.md)<br />— [Adımları için Windows 7 aygıtları](active-directory-conditional-access-automatic-device-registration-setup.md) |

| İki aşamalı doğrulama için Microsoft Authenticator uygulamasını kullanma | [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

Koşullu erişim bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

## <a name="apps--azure-ad"></a>Uygulamalar ve Azure AD
### <a name="cloud-app-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud App Discovery: kuruluşunuzda hangi SaaS uygulamaları kullanılan Bul
Cloud App Discovery hangi SaaS uygulamaları kuruluş genelinde kullanılan öğrenin BT departmanları yardımcı olur. Uygulama kullanımı ölçebilirsiniz ve BT denetim ve Azure AD ile tümleşik altında böylece hangi uygulamaların engeller en yararlı olacaktır, BT'nin belirleyebilir popülerliği getirildi.

| Makale Kılavuzu |  |
|:---:| --- |
| Nasıl çalıştığına ilişkin genel bir bakış |[Cloud App Discovery ile bulut uygulamaları tasdik bulma](active-directory-cloudappdiscovery-whatis.md) |
| Derin Dalış nasıl içine, gizlilik hakkında sorular için yanıtlar ile çalışır |[Güvenlik ve gizlilik konuları](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) |
| Sık Sorulan Sorular |[Cloud App Discovery hakkında SSS](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |
| Cloud App Discovery dağıtmak için öğreticileri |[Grup İlkesi Dağıtım Kılavuzu](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)<br /><br />[System Center Dağıtım Kılavuzu](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)<br /><br />[Proxy sunucuları özel bağlantı noktaları ile yükleme](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| Cloud App Discovery Aracısı güncelleştirmeleri için değişiklik günlüğü |[Değişiklik günlüğü](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx) |

Cloud App Discovery bir [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özelliği.

### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Otomatik olarak sağlamak ve kullanıcı hesapları SaaS uygulamalarında yetkisini kaldırma
Oluşturulması, Bakım ve kullanıcı kimlikleri Dropbox, Salesforce, ServiceNow ve daha fazlası gibi SaaS uygulamalarında kaldırılmasını otomatikleştirin. Eşleşen ve Azure AD arasında varolan kimlik eşitleme ve SaaS uygulamalarınıza ve kullanıcılar kuruluş ayrıldığında bırakılarak otomatik olarak hesaplar erişimi denetleme.

| Makale Kılavuzu |  |
|:---:| --- |
| Sık sorulan soruların yanıtlarını bulun ve nasıl çalıştığı hakkında bilgi edinin |[Sağlama & SaaS uygulamaları için sağlama kaldırmayı kullanıcı otomatikleştirme](active-directory-saas-app-provisioning.md) |
| Bilgileri Azure AD arasında nasıl eşlendi yapılandırmak ve SaaS uygulamanız |[Öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Otomatik sağlama SCIM'yi protokolünü destekleyen herhangi bir uygulama için etkinleştirme |[Otomatik kullanıcı hazırlama hiçbir SCIM-Enabled uygulamaya ayarlama](active-directory-scim-provisioning.md) |
| Raporlama ve kullanıcı hazırlama ilgili sorunları giderme |[Otomatik kullanıcı sağlamayı raporlama](active-directory-saas-provisioning-reporting.md)<br><br>[Sağlama bildirimleri](active-directory-saas-account-provisioning-notifications.md)<br><br>[Kullanıcı sağlama sorunlarını giderme](active-directory-application-provisioning-content-map.md) |
| Öznitelik değerlerine göre bir uygulamaya sağlanan sınırı |[Kapsam belirleme filtreleri](active-directory-saas-scoping-filters.md) |

Otomatik kullanıcı hazırlama, kullanıcı başına en fazla on uygulamaları için Azure AD'in tüm sürümleri için kullanılabilir. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) sınırsız uygulamaları destekler. Kuruluşunuzun varsa [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) veya [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), yapabilecekleriniz sonra [hangi kullanıcıların sağlanan yönetmek için grupları kullanma](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Azure AD ile tümleştirmek uygulamaları oluşturma
Kuruluşunuz geliştirme veya satır iş kolu (LoB) uygulamaları koruma ya da bir uygulama geliştiricisi Azure Active Directory kullanan müşteriler sahip değilseniz, aşağıdaki öğreticiler yardımcı olur, uygulamalarınızın Azure AD ile tümleştirin.

| Makale Kılavuzu |  |
|:---:| --- |
| BT uzmanları ve uygulamaları Azure AD ile tümleştirme uygulama geliştiricileri için yönergeler |[BT uzmanları kılavuzu için Azure AD uygulamaları geliştirmek için](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[Azure Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md) |
| Uygulama satıcıları uygulamalarını Azure AD uygulama galerisinde eklemek için ne |[Uygulamanız Azure Active Directory Uygulama galerisinde listeleme](active-directory-app-gallery-listing.md) |
| Azure Active Directory'yi kullanarak geliştirilmiş uygulamalara erişimi yönetme |[Kullanıcı Ataması geliştirilen uygulamaları için etkinleştirme](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Kullanıcılar uygulamanıza atama](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Uygulamanıza grup atama](active-directory-applications-guiding-developers-assigning-groups.md) |

Tüketiciye yönelik uygulamalar geliştiriyorsanız, kullanarak ilgilenebilirsiniz [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) böylece kullanıcılarınızın yönetmek için kendi kimlik sistemi geliştirmek gerekmez. [Daha fazla bilgi edinin](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-to-applications"></a>Uygulamalara erişimi yönetme
### <a name="using-groups-and-self-service-to-manage-who-has-access-to-which-apps"></a>Gruplar ve kimin hangi uygulamalara erişimi yönetmek için Self Servis kullanarak
Kimlerin hangi kaynaklara erişimi olması gereken yönetmenize yardımcı olmak için Azure Active Directory gruplarını kullanarak ölçekte atamaları ve izinleri ayarlamanıza olanak sağlar. BT, gereksinim duyan kullanıcılar yalnızca izni istemesini Self Servis özelliklerini etkinleştirmek tercih edebilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure AD erişim yönetim özelliklerine genel bakış |[Uygulamalara erişimi yönetme giriş](active-directory-managing-access-to-apps.md)<br /><br />[Azure AD'de erişim yönetimi nasıl çalışır?](active-directory-manage-groups.md)<br /><br />[SaaS uygulamalarına erişimi yönetmek için grupları kullanma](active-directory-accessmanagement-group-saasapps.md) |
| Self Servis yönetimine, uygulamaların ve grupları izin ver |[Self Servis uygulama yönetimi](active-directory-self-service-application-access.md)<br /><br />[Self Servis Grup Yönetimi](active-directory-accessmanagement-self-service-group-management.md) |
| Azure AD'de, grupları ayarlama yönergeleri |[Güvenlik grupları oluşturma](active-directory-groups-create-azure-portal.md)<br /><br />[Bir grubun sahiplerini atama](active-directory-accessmanagement-managing-group-owners.md)<br /><br />["Tüm kullanıcıları" grubu kullanma](active-directory-accessmanagement-dedicated-groups.md) |
| Öznitelik tabanlı üyelik kurallarını kullanarak grup üyeliğini otomatik olarak doldurmak için dinamik grupları kullanma |[Dinamik grup üyeliğini: Gelişmiş kurallar](active-directory-groups-dynamic-membership-azure-portal.md)<br /><br />[Dinamik grup üyeliklerini sorunlarını giderme](active-directory-accessmanagement-troubleshooting.md) |

Grup tabanlı bir uygulamaya erişim yönetimi için kullanılabilir [Azure AD temel](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Self Servis Grup Yönetimi, uygulama kendi kendine yönetimi ve dinamik grupların [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) özellikleri.

### <a name="b2b-collaboration-enable-partner-access-to-applications"></a>B2B işbirliği: uygulamaları için ortak erişimi etkinleştir
İşinizin diğer şirketlerle işbirliği değilse, şirket uygulamalarınız için ortak erişimi yönetmek üzere ihtiyacınız olasıdır. Azure Active Directory B2B işbirliği uygulamalarınızı iş ortakları ile paylaşmak için kolay ve güvenli bir yol sağlar.

| Makale Kılavuzu |  |
|:---:| --- |
| Farklı Azure AD genel bir bakış dış kullanıcılar gibi yönetmenize yardımcı ortakları olduğunu, müşteriler, vb. içerir. |[Dış kimlikler Azure AD'de yönetmek için özellikleri karşılaştırma](active-directory-b2b-compare-external-identities.md) |
| B2B işbirliği ve nasıl başlayacağınızı giriş |[Basit, güvenli, Azure AD ile bulut iş ortağı tümleştirme](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Azure Active Directory B2B işbirliği](active-directory-b2b-collaboration-overview.md) |
| Azure AD B2B işbirliği ve nasıl kullanılacağını daha derin Dalış |[B2B işbirliği: Nasıl çalışır?](active-directory-b2b-how-it-works.md)<br /><br />[Azure AD B2B işbirliği geçerli sınırlamaları](active-directory-b2b-current-limitations.md)<br /><br />[Azure AD B2B işbirliği kullanımının ayrıntılı kılavuz](active-directory-b2b-detailed-walkthrough.md) |
| Azure AD B2B işbirliği nasıl çalıştığı hakkında teknik bilgi makalelerle başvurusu |[İş ortağı kullanıcılar eklemek için CSV dosya biçimi](active-directory-b2b-references-csv-file-format.md)<br /><br />[Azure AD B2B işbirliği tarafından etkilenen kullanıcı öznitelikleri](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[İş ortağı kullanıcılar için kullanıcı belirteci biçimi](active-directory-b2b-references-external-user-token-format.md) |

B2B işbirliği için şu anda kullanılabilir [Azure Active Directory'nin tüm sürümlerinde](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Erişim paneli: uygulamaları ve Self Servis özelliklerine erişmek için bir portal
Azure AD erişim paneli burada son kullanıcılar uygulamalarını başlatmak ve bunları kendi uygulamaları ve grup üyeliklerini yönetmek izin Self Servis özelliklerine erişmek ' dir. Erişim paneli ek olarak, aşağıdaki listede SSO özellikli uygulamalar erişmek için diğer seçenekleri dahil edilir.

| Makale Kılavuzu |  |
|:---:| --- |
| Kullanıcılar için çoklu oturum açma uygulamalarını dağıtmak için kullanılabilir farklı seçenekler karşılaştırması |[Tümleşik uygulamalarını kullanıcılara Azure AD dağıtma](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) |
| Erişim paneli ve kendi mobil eşdeğer MyApps genel bakış |[Erişim paneli ve MyApps giriş](active-directory-saas-access-panel-introduction.md)<br />— [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Azure AD uygulamalarınız Office 365 Web sitesinden erişme |[Office 365 uygulama Başlatıcı kullanma](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Azure AD uygulamaları Intune Managed Browser mobil uygulamasından erişmek nasıl |[Intune yönetilen tarayıcı](https://technet.microsoft.com/en-us/library/dn878029.aspx)<br />— [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Çoklu oturum açma başlatmak için ayrıntılı bağlantılar kullanarak Azure AD uygulamaları erişme |[Oturum açma doğrudan bağlantılar uygulamalarınızı alma](active-directory-appssoaccess-whatis.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Erişim paneli yüklenebilir [Azure Active Directory'nin tüm sürümlerinde](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-to-apps"></a>Raporları: Kolayca uygulama erişim değişikliklerini denetleme ve oturum açma işlemlerine uygulamaları izleme
Azure Active Directory, çeşitli raporlar ve kuruluşunuzun uygulamalarına erişmesine izlemenize yardımcı olması için uyarılar sağlar. Anormal oturum açma işlemleri için uyarılar uygulamalarınıza alabilir ve ne zaman ve neden bir uygulamaya bir kullanıcıların erişim değişti izleyebilirsiniz.

| Makale Kılavuzu |  |
|:---:| --- |
| Azure Active Directory'de raporlama özelliklerine genel bakış |[Başlarken Azure AD raporlama](active-directory-reporting-getting-started.md) |
| Oturum açma işlemleri ve kullanıcılarınızın uygulama kullanımını izleme |[Erişim ve kullanım raporları görüntüleyin](active-directory-view-access-usage-reports.md) |
| Belirli bir uygulama erişebilecek yapılan değişiklikleri izleyin |[Azure Active Directory Denetim Raporu olayları](active-directory-reporting-audit-events.md) |
| Bu raporların verileri raporlama API'sini kullanarak, tercih edilen araçlarınızı dışarı aktarma |[Azure AD raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) |

Hangi raporların Azure Active Directory, farklı sürümlerine dahil olduğunu görmek için [burayı](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Ayrıca bkz.
[Azure Active Directory nedir?](active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure Active Directory etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/)

[Azure çok faktörlü kimlik doğrulaması](https://azure.microsoft.com/services/multi-factor-authentication/)
