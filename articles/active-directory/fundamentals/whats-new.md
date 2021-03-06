---
title: Yenilikler Sürüm Notları - Azure Active Directory | Microsoft Docs
description: Azure Active Directory'ye yeni gibi bilinen sorunları, hata düzeltmeleri, en son sürüm notları, işlevler de kullanım dışı ve gelecek öğrenin değişiklikler.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: af215c80148010d526c951e7a5128d6e4622b1cd
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625559"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'deki yenilikler nelerdir?

>Ne zaman kopyalayıp yapıştırarak bu URL'yi güncelleştirmeler için bu sayfayı yeniden ziyaret etmeniz hakkında bilgi edinin: `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` içine, ![bir RSS Okuyucu simgesi](./media/whats-new/feed-icon-16x16.png) okuyucu akış.

Azure AD iyileştirmeleri düzenli olarak alır. İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

Bu sayfaya ay güncelleştirilir, böylece bunu düzenli olarak tekrar ziyaret. Altı aydan eski olan öğeleri arıyorsanız, bunları bulabilirsiniz [Azure Active Directory'deki yenilikler için arşiv](whats-new-archive.md).

---

## <a name="june-2019"></a>Haziran 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Microsoft Graph (genel Önizleme) için yeni riskDetections API

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Microsoft Graph için yeni riskDetections API'si artık genel önizlemeye sunuldu duyurmaktan Mutluluk duyuyoruz. Kuruluşunuzun kimlik koruması ile ilgili kullanıcı ve oturum açma riski algılamalar listesini görüntülemek için bu yeni API'yi kullanabilirsiniz. Bu API, saptama türü, durumu, düzey ve diğer ilgili ayrıntılar dahil olmak üzere, risk algılamaların daha verimli bir şekilde sorgulamak için de kullanabilirsiniz.

Daha fazla bilgi için [Risk algılama API başvuru belgeleri](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Haziran 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Haziran 2019 ' federasyon ile 22 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Azure AD SAML Araç Seti](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (大塚商会)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN istemcisi](https://portal.azure.com/), [ExpenseIn](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [ Yardımcısı Yardımcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial), [Costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [araç Mercedes Benz Office](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), [ Oracle bulut altyapısı konsol](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [CyberArk SAML kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial), [Scrible Edu](https://www.scrible.com/sign-in/#/create-account), [PandaDoc](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial), [Perceptyx ](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial), [Proptimise işletim sistemi](https://proptimise.co.uk/software/), [Vtiger CRM (SAML)](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial), Oracle için Oracle perakende satış, Oracle E-Business Suite, IDCS Oracle için Oracle Erişim Yöneticisi Erişim Yöneticisi için E-Business Suite, Oracle PeopleSoft, Oracle için azure'da JD Edwards IDCS için IDCS

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni destekli SaaS uygulamaları için hesap kullanıcı sağlamayı otomatikleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** İzleme ve Raporlama

Oluşturma, güncelleştirme ve silme bu yeni tümleşik uygulamalar için kullanıcı hesapları artık otomatikleştirebilirsiniz:

- [Yakınlaştırma](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Envoy](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Daha iyi otomatik kullanıcı hesabı sağlama kullanarak, kuruluşunuzun güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Azure AD sağlama hizmeti, gerçek zamanlı ilerlemeyi görüntüleyebilirsiniz

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün özelliği:** Kimlik yaşam döngüsü yönetimi

Ne kadar sağlama işlemi kullanıcı olduğunu gösteren yeni bir ilerleme çubuğu içerecek şekilde sağlama deneyimini Azure AD'ye güncelleştirdik. Çok sayıda kullanıcı tarih için sağlanan nasıl bu güncelleştirilmiş deneyim ayrıca geçerli döngüsü sırasında sağlanan kullanıcı sayısı hakkında bilgi de sağlar.

Daha fazla bilgi için [kullanıcı sağlama durumunu kontrol](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user).

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Oturumu kapatın ve hata ekranları şirket markası artık görünür

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Şirket markanızı şimdi Oturumu Kapat ve hata iletilerini yanı üzerinde oturum açma sayfasında görünmesi, Azure AD güncelleştirdik. Bu özelliği açmak için herhangi bir şey yapmanız gerekmez, Azure AD, yalnızca zaten ayarladığınız içinde varlıkları kullanan **şirket markası ekleme** Azure portal'ın alan.

Şirketinizin markasını ayarlama hakkında daha fazla bilgi için bkz. [ekleyin, kuruluşunuzun Azure Active Directory sayfalara marka](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure multi-Factor Authentication (MFA) sunucusu artık yeni dağıtımlar için kullanılabilir

**Türü:** Kullanım Dışı  
**Hizmet kategorisi:** MFA  
**Ürün özelliği:** Kimlik güvenliği ve koruması

1 Temmuz 2019'dan itibaren Microsoft artık yeni dağıtımlar için MFA sunucusu sunacaktır. Çok faktörlü kimlik doğrulaması, kuruluş içindeki gerekli olmasını istediğiniz yeni müşteriler artık Azure multi-Factor Authentication'ı bulut tabanlı kullanmanız gerekir. MFA sunucusu 1 Temmuz'dan önce etkinleştiren müşteriler, bir değişikliği göremezsiniz. Yine de etkinleştirme kimlik bilgileri oluştur en son sürümünü indirin ve gelecek güncelleştirmeleri almak mümkün olacaktır.

Daha fazla bilgi için [Azure multi-Factor Authentication Sunucusu'nu kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy). Bulut tabanlı Azure multi-Factor Authentication hakkında daha fazla bilgi için bkz: [bulut tabanlı bir Azure multi-Factor Authentication dağıtım planlaması](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Hizmet Değiştir: Yalnızca TLS 1.2 protokolleri uygulama ara Sunucusu hizmetine yönelik gelecek destek

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Ara sunucusu  
**Ürün özelliği:** Erişim Denetimi

Müşterilerimiz için sınıfının en iyisi şifreleme sağlamak için biz yalnızca TLS 1.2 protokolleri uygulama ara Sunucusu hizmetine erişimi sınırlandırma. Bu değişiklik, değişiklikleri görmeyeceksiniz için zaten yalnızca TLS 1.2 protokolleri kullanan müşteriler için kademeli olarak sunulacaktır.

31 Ağustos 2019, TLS 1.0 ve TLS 1.1 kullanımdan kaldırma gerçekleşir ancak bu değişikliğe hazırlanmak için zamanınız şekilde ek Gelişmiş bildirimi sağlarız. Bu değişiklik hazırlamak için kullanıcılarınızın uygulama proxy'si aracılığıyla yayımlanan uygulamalara erişmek için kullandığı tüm istemciler dahil olmak üzere, istemci-sunucu ve tarayıcı sunucu birleşimlerini emin olun, uygulamaya bir bağlantı sürdürmenizi TLS 1.2 protokolünü kullanacak şekilde güncelleştirildi Proxy hizmeti. Daha fazla bilgi için [Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Kullanım ve öngörüleri raporunu uygulamayla ilgili oturum açma verilerinizi görüntülemek için kullanın

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** İzleme ve Raporlama

Artık bulunan kullanım ve öngörüleri raporu kullanabilirsiniz **kurumsal uygulamalar** hakkında bilgileri de dahil olmak üzere, oturum açma verilerini, uygulama odaklı bir görünümünü elde etmek için Azure Portalı'nın alan:

- Kuruluşunuz için üst kullanılan uygulamalar

- Uygulamalarla en çok başarısız oturum açma işlemleri

- En sık karşılaşılan hatalar her uygulama için oturum açma

Bu özellik hakkında daha fazla bilgi için bkz. [kullanım bilgilerini ve öngörüleri Azure Active Directory Portalı'nda rapor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report)

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Azure AD kullanarak uygulamaları bulut sağlama, kullanıcı otomatikleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** İzleme ve Raporlama

Oluşturma, silme, otomatik hale getirmek için Azure AD sağlama hizmeti kullanmak için bu yeni öğreticileri izleyin ve aşağıdaki bulut tabanlı uygulamalar için kullanıcı hesapları güncelleştiriliyor:

- [Comeet](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [KeeperSecurity](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

Ayrıca bu yeni izleyebilirsiniz [Dropbox öğretici](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial), Grup nesnelerini sağlamasını yapma hakkında bilgi sağlar.

Daha iyi otomatik kullanıcı hesabı sağlama üzerinden kuruluşunuzun güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Güvenli kimlik puanı Azure AD'de (Genel kullanım) kullanıma sunuldu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Yok  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Artık izleme ve kimlik bilgilerinizi artırmak güvenlik duruşunu kimliğini kullanarak Azure AD'de puanı özellik güvenli. Kimlik güvenli yardımcı olması için tek bir Pano puanı özellik kullanır:

- Hatada yavaşlık durumunu nesnel 1 ile 223 arasında bir puan dayalı kimlik güvenlik duruşu ölçün.

- Kimlik güvenlik geliştirmeleri için planlama

- Güvenlik geliştirmelerinizin başarısını gözden geçirin

Güvenlik puanı kimlik özelliği hakkında daha fazla bilgi için bkz. [Azure Active Directory'de kimlik güvenli puanı nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score).

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Yeni uygulama kaydı deneyimini sunulmuştur kullanılabilir (Genel kullanım)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** Bir geliştirici deneyimi

Yeni [uygulama kayıtları](https://aka.ms/appregistrations) deneyimi, artık genel kullanıma sunulmuştur. Bu yeni deneyim, Azure portalı ve uygulama kayıt portalı alışık olduğunuz tüm anahtar özelliklerini içerir ve aracılığıyla bağlı geliştirir:

- **Daha iyi uygulama yönetimi.** Uygulamalarınızı farklı portalları arasında görmenin yerine, artık tüm uygulamalarınızı tek bir konumda görebilirsiniz.

- **Basitleştirilmiş uygulama kaydı.** Yenilenmiş izni seçme deneyimi için geliştirilmiş Gezinti deneyiminden artık kaydetmek ve uygulamaları yönetmek daha kolaydır.

- **Daha ayrıntılı bilgi.** Hızlı Başlangıç kılavuzları ve daha fazlası dahil olmak üzere uygulamanızı hakkında daha fazla ayrıntı bulabilirsiniz.

Daha fazla bilgi için [Microsoft kimlik platformu](https://docs.microsoft.com/azure/active-directory/develop/) ve [uygulama kayıtları deneyimi genel kullanıma sunuldu!](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) blog duyuruları.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Kimlik koruması için riskli kullanıcılar API'sinde kullanılabilen yeni özellikleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Artık riskli kullanıcılar API'si kullanıcıların risk geçmişini alamadık, riskli kullanıcılar kapatmak ve kullanıcılar tehlikede gibi onaylamak için kullanabileceğiniz duyurmaktan Mutluluk duyuyoruz. Bu değişiklik, daha verimli bir şekilde kullanıcılarınızın risk durumu güncelleştirme ve kendi risk geçmişini anlama yardımcı olur.

Daha fazla bilgi için [riskli kullanıcılar API başvuru belgeleri](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Mayıs 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Mayıs 2019 ' federasyon ile 21 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [gerçek bağlantı](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kianda](https://app.kianda.com/sso/OpenID/AzureAD/), [basit oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [Braze](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [Displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [Templafy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo satış ilgisini](https://toutapp.com/login), [ACLP](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [Meta4 genel ik](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [Kuantum çalışma alanına](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Cobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [iki Web yöntemini API bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [RedFlag](https://pocketstop.com/redflag/), [Whatfix](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [Denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [JOBHUB](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [NEOGOV](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [Foodee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial), [MyVR](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Geliştirilmiş grupları oluşturma ve yönetim deneyimleri Azure AD portalında

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** İş Birliği

Azure AD portalında deneyimleri grupları ile ilgili iyileştirmeler yaptık. Bu geliştirmeler yöneticilerin grupları listesi, üyeleri listeler, daha iyi yönetin ve ek oluşturma seçenekleri sağlamak için olanak sağlar.

Geliştirmeler şunlardır:

- Temel üyelik türü ve grup türüne göre filtreleme.

- Ayrıca, kaynak ve e-posta adresi gibi yeni bir sütun.

- Çoklu seçim grupları, üyeleri ve sahibi olanağı kolay silinmek üzere listeler.

- Bir e-posta adresi seçin ve grup oluşturma sırasında sahipleri ekleme yeteneği.

Daha fazla bilgi için [temel bir grup oluşturma ve Azure Active Directory'yi kullanarak üye ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Azure AD portalında (Genel kullanım) Office 365 grupları için bir adlandırma ilkesini yapılandırma

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** İş Birliği

Yöneticiler artık bir adlandırma ilkesini Azure AD portalı kullanarak, Office 365 grupları için yapılandırabilirsiniz. Bu değişiklik veya kuruluşunuzdaki kullanıcılar tarafından düzenlenemiyor Office 365 grupları için tutarlı adlandırma kuralları uygulanmasına yardımcı olur.

Office 365 grupları için adlandırma ilkesi iki farklı yolla yapılandırabilirsiniz:

- Ön ekleri veya bir grup adı için otomatik olarak eklenen son eklerini tanımlayın.

- Özelleştirilmiş bir kelimelerin (örneğin "CEO, bordro, ik") Grup adlarında izin verilmeyen engellenen kuruluşunuz için karşıya yükleyin.

Daha fazla bilgi için [Office 365 grupları için bir adlandırma ilkesini zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft Graph API uç noktaları için Azure AD etkinlik günlüklerini (Genel kullanım) kullanıma sunulmuştur

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Microsoft Graph API uç noktaları Azure AD etkinlik desteği genel kullanıma duyurmaktan Mutluluk duyuyoruz günlükleri. Kullanım sürüm 1.0 hem Azure ad denetim günlükleri, oturum açma günlüklerinin yanı sıra API bu sürümle birlikte, artık.

Daha fazla bilgi için [Azure AD denetim günlüğü API genel bakış](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Yöneticiler artık birleşik kayıt işlemi (genel Önizleme) için koşullu erişim kullanabilirsiniz

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Kimlik güvenliği ve koruması  

Yöneticiler, artık birleşik kayıt sayfasına tarafından kullanmak için koşullu erişim ilkeleri oluşturabilirsiniz. Bu kayıt, izin vermek için ilkeler uygulama içerir:

- Güvenilen bir ağda kullanıcılardır.

- Düşük bir oturum açma riski kullanıcılardır.

- Yönetilen bir cihazda kullanıcılardır.

- Kullanıcılar kuruluşun kullanım koşulları (TOU) için kabul etmiş olursunuz.

Koşullu erişim ve parola sıfırlama hakkında daha fazla bilgi için gördüğünüz [koşullu erişim için Azure AD MFA birleştirilir ve parola sıfırlama kayıt deneyimi blog gönderisi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348). Birleşik kayıt işlemi için koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [birleşik kayıt için koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Kullanım Özelliği Azure AD koşulları hakkında daha fazla bilgi için bkz. [kullanım Özelliği Azure Active Directory koşullarını](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

## <a name="april-2019"></a>Nisan 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-in-refreshed-azure-ad-identity-protection"></a>Yeni Azure AD tehdit zekası algılama yenilenmiş Azure AD kimlik koruması kullanıma sunuldu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Kimlik Koruması  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Azure AD tehdit zekası algılama yenilenmiş Azure AD kimlik koruması kullanıma sunulmuştur. Bu yeni işlevselliği, belirli bir kullanıcı için sıra dışı veya Microsoft'un iç ve dış tehdit zekasına dayalı bilinen saldırı düzenleriyle tutarlı olan kullanıcı etkinliğini gösteren yardımcı olur.

Azure AD kimlik Koruması'nın yenilenmiş sürümü hakkında daha fazla bilgi için bkz. [dört ana Azure AD kimlik koruması geliştirmeleri genel önizlemede kullanıma](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) blog ve [Azure Active Directory nedir Kimlik koruması (yenilenmiş)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) makale. Azure AD tehdit zekası algılama hakkında daha fazla bilgi için bkz: [Azure Active Directory kimlik koruması, risk olayları](https://docs.microsoft.com/azure/active-directory/identity-protection/risk-events-reference#azure-ad-threat-intelligence) makalesi.

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Azure AD hak yönetimi sunulmuştur kullanılabilir (genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Identity Governance  
**Ürün özelliği:** Identity Governance

Azure AD hak yönetimi, artık genel önizlemede nasıl çalışanların ve iş ortaklarına erişim isteğinde bulunabileceği, kimin tarafından onaylanması gerekir ve ne kadar süreyle erişimleri tanımlayan yönetim erişim paketlerin temsilci müşterilere yardımcı olur. Erişim paketleri üyelik Azure AD'de ve Office 365 grupları, kurumsal uygulamalar bölümündeki rol atamaları ve SharePoint Online siteleri için rol atamalarını yönetebilir. Hak Yönetimi hakkında daha fazla bilgiyi [Azure AD Hak Yönetimi'ne genel bakış](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Avantajlarına Privileged Identity Management, erişim gözden geçirmeleri ve kullanım şartları dahil olmak üzere Azure AD kimlik yönetimi özellikleri hakkında daha fazla bilgi için bkz: [Azure AD kimlik yönetimi nedir?](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>(Genel Önizleme) Azure AD portalında Office 365 grupları için bir adlandırma ilkesini yapılandırma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** İş Birliği

Yöneticiler artık bir adlandırma ilkesini Azure AD portalı kullanarak, Office 365 grupları için yapılandırabilirsiniz. Bu değişiklik veya kuruluşunuzdaki kullanıcılar tarafından düzenlenemiyor Office 365 grupları için tutarlı adlandırma kuralları uygulanmasına yardımcı olur.

Office 365 grupları için adlandırma ilkesi iki farklı yolla yapılandırabilirsiniz:

- Ön ekleri veya bir grup adı için otomatik olarak eklenen son eklerini tanımlayın.

- Özelleştirilmiş bir kelimelerin (örneğin "CEO, bordro, ik") Grup adlarında izin verilmeyen engellenen kuruluşunuz için karşıya yükleyin.

Daha fazla bilgi için [Office 365 grupları için bir adlandırma ilkesini zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD etkinlik günlüklerini Azure İzleyici'de (Genel kullanım) kullanıma sunulmuştur

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Azure AD etkinlik günlüklerini görselleştirmelerle hakkındaki görüşlerinizi çözümüne yardımcı olmak için yeni bir öngörü özelliği Log analytics'te sunacağız. Bu özellik, çalışma kitapları olarak adlandırılan etkileşimli şablonlarımızı kullanarak, Azure AD kaynaklarını hakkında Öngörüler edinin yardımcı olur. Bu çalışma kitaplarını önceden oluşturulmuş uygulamalar veya kullanıcılarla için ayrıntıları sağlayın ve şunları içerir:

- **Oturum açma işlemleri.** Uygulamalar ve kullanıcılar, oturum açma konumu, kullanımdaki işletim sistemi veya tarayıcı istemcisi ve sürümü ve başarılı veya başarısız oturum açma sayısı gibi ayrıntıları sağlar.

- **Eski bir kimlik doğrulama ve koşullu erişim.** Uygulamalar ve koşullu erişim ilkeleri kullanarak uygulamaları koşullu erişim ilkeleri tarafından tetiklenen multi-Factor Authentication kullanımı dahil olmak üzere eski kimlik doğrulaması kullanan kullanıcılar için Ayrıntılar sağlar ve benzeri.

- **Oturum açma hatası analizi.** Oturum açma hataları bir kullanıcı eylemi, ilke sorunları veya altyapınızı nedeniyle ortaya çıkan, belirlemenize yardımcı olur.

- **Özel raporlar.** Yeni oluştur veya Yardım öngörüleri özellik kuruluşunuz için özelleştirmek için var olan çalışma kitaplarını düzenleyin.

Daha fazla bilgi için [raporları Azure Active Directory için Azure İzleyici çalışma kitaplarını kullanmak nasıl](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Nisan 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Federasyon ile 21 bu yeni uygulamalar için uygulama Galerisi destek Nisan 2019 ' ekledik:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [HRworks çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [Percolate](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [MobiControl](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [ Shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [Benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [EduBrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [RStudio Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel bağlanma](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba bulut (rol tabanlı SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [Certent sermayesi Yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [Sectigo Sertifika Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [GreenOrbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [Workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [ SurveyMonkey Kurumsal](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [Indiggo](https://indiggolead.com/)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Sıklık seçenek ve birden çok rolü seçimi yeni erişim gözden geçirmeleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Erişim Gözden Geçirmeleri  
**Ürün özelliği:** Identity Governance

Yeni güncelleştirmeler Azure ad erişim gözden geçirmeleriyle olanak sağlar:

- Sıklığı, erişim gözden geçirmeleri için değişiklik **yarı annually**, haftalık, aylık, üç aylık ve yıllık önceden var olan seçenekleri yanı sıra.

- Birden çok Azure AD'yi seçin ve tek bir erişim oluştururken Azure kaynağı rolleri gözden geçirin. Bu durumda, tüm rolleri aynı ayarlarla ayarlanır ve aynı anda tüm gözden geçirenler bildirilir.

Erişim gözden geçirmesi oluşturma hakkında daha fazla bilgi için bkz. [gruplarının bir erişim gözden geçirmesi oluşturma veya uygulamaları Azure ad erişim gözden geçirmeleriyle](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect e-posta uyarı sistemleri bazı müşteriler için yeni bir e-posta gönderen bilgileri gönderme geçiş

**Türü:** Değişen özellik  
**Hizmet kategorisi:** AD eşitleme  
**Ürün özelliği:** Platform

Azure AD Connect, e-posta uyarı sistemleri geçiş sürecinde bazı müşteriler yeni bir e-posta gönderen potansiyel olarak gösteren. Bu adres için eklemelisiniz `azure-noreply@microsoft.com` için kuruluşunuzun izin verilenler listesi veya Office 365, Azure veya eşitleme hizmetlerinizi önemli uyarılar almaya devam etmek mümkün olmayacaktır.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>UPN soneki değişiklikler Azure AD CONNECT'te federe etki alanları arasında başarılı

**Türü:** Sabit  
**Hizmet kategorisi:** AD eşitleme  
**Ürün özelliği:** Platform

Artık başarıyla bir kullanıcının UPN soneki bir federe etki alanından başka bir federe etki alanına Azure AD CONNECT'te değiştirebilirsiniz. Bu düzeltme FederatedDomainChangeError hata iletisi eşitleme döngüsü sırasında deneyimi veya artık bildirim e-posta belirten bir, alma anlamına gelir "olduğundan bu nesne Azure Active Directory'de güncelleştirilemiyor özniteliği [ FederatedUser.UserPrincipalName] geçerli değil. Değeri yerel dizin hizmetlerinizde güncelleştirin:".

Daha fazla bilgi için [eşitleme sırasında hataları giderme](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror).

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Uygulama koruma tabanlı koşullu erişim ilkesi, Azure AD'de (genel Önizleme) kullanarak daha yüksek güvenlik

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Uygulama koruma tabanlı koşullu erişim artık kullanılabilir kullanarak **uygulama koruması gerektiren** ilkesi. Bu yeni ilkenin önlemek için yardımcı olarak, kuruluşunuzun güvenliğini artırmaya yardımcı olur:

- Kullanıcılar Microsoft Intune lisansı olmadan uygulamalara erişim sağlamasını.

- Bir Microsoft Intune uygulama koruma ilkesini almak çözememesi kullanıcılar.

- Yapılandırılmış bir Intune uygulama koruma İlkesi olmadan uygulamalara erişim sağlamasını kullanıcılar.

Daha fazla bilgi için [koşullu erişim ile bulut uygulaması erişimi için uygulama koruma ilkesini zorunlu kılma](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Yeni Azure AD çoklu oturum açma ve Microsoft Edge (genel Önizleme) içinde koşullu erişim desteği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Azure AD çoklu oturum açma ve koşullu erişim için yeni destek sağlamak da dahil, Microsoft Edge için sunduğumuz Azure AD desteğini geliştirdik. Microsoft Intune Managed Browser'ı daha önce kullandıysanız, artık Microsoft Edge yerine kullanabilirsiniz.

Ayarlama ve koşullu erişim kullanarak uygulamaları ve cihazları yönetme hakkında daha fazla bilgi için bkz. [gerektiren yönetilen cihazlar için koşullu erişim ile bulut uygulaması erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) ve [gerektiren onaylı istemci uygulamalarını bulut için Koşullu erişimle uygulama](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Microsoft Edge ile Microsoft Intune ilkeleri kullanarak erişimi yönetme hakkında daha fazla bilgi için bkz. [Intune ilkeyle korunan bir tarayıcı kullanarak Internet erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser).

---

## <a name="march-2019"></a>Mart 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Kimlik deneyimi çerçevesi ve özel ilke Azure Active Directory B2C'de desteği kullanıma (GA) sunuldu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Artık Azure AD B2C'de, aşağıdaki görevler de dahil olmak üzere özel ilkeleri oluşturabilirsiniz, ölçekli desteklenir ve Azure SLA'mız altında:

- Oluşturun ve özel kimlik doğrulama kullanıcı yolculuklarından özel ilkeler kullanarak yükleyin.

- Kullanıcı yolculuklarından değişimleri adım adım talep sağlayıcıları arasında açıklanmaktadır.

- Koşullu dallanmayı içinde kullanıcı yolculuklarından tanımlayın.

- Dönüştürme ve gerçek zamanlı kararlar ve iletişimleri bilgisinde taleplerini eşle.

- REST API özellikli hizmetler, özel kimlik doğrulama kullanıcı yolculuklarından içinde kullanın. Örneğin, e-posta sağlayıcıları, katılımlarını ve özel yetkilendirme sistemleri ile.

- Openıdconnect protokolünü ile uyumlu olan kimlik sağlayıcıları ile federasyona eklemek. Örneğin, çok kiracılı Azure AD, sosyal hesap sağlayıcılarıyla veya iki Faktörlü doğrulama sağlayıcıları ile.

Özel ilkeleri oluşturma hakkında daha fazla bilgi için bkz. [özel ilkeleri Azure Active Directory B2C için Geliştirici Notları](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom) okuyup [Alex Simon'ın blog gönderisi, örnek olay incelemeleri de dahil olmak üzere](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Mart 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Mart 2019 ' federasyon ile 14 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[ISEC7 mobil Exchange temsilci](https://www.isec7.com/english/), [MediusFlow](https://office365.cloudapp.mediusflow.com/), [ePlatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [Fulcrum](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [ExcelityGlobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama tabanlı denetim sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [Powerschool performans sorunları](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [Cinode](https://cinode.com/), [Iris İntranet](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [Empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [GÖR](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Yeni Zscaler ve Azure AD Galerisi - Mart 2019 bağlayıcılar sağlama Atlassian

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme

Oluşturma, güncelleştirme ve silme aşağıdaki uygulamalar için kullanıcı hesapları otomatik hale getirin:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler bir](https://aka.ms/ZscalerOneProvisioning), [Zscaler iki](https://aka.ms/ZscalerTwoProvisioning), [Zscaler üç](https://aka.ms/ZscalerThreeProvisioning), [Zscaler ZSCloud ](https://aka.ms/ZscalerZSCloudProvisioning), [Atlassian bulut](https://aka.ms/atlassianCloudProvisioning)

Daha iyi otomatik kullanıcı hesabı sağlama üzerinden kuruluşunuzun güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin](https://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Geri yükleme ve, silinen bir Office 365 grupları Azure AD portalında yönetin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** İş Birliği

Şimdi, görüntüleyin ve Azure AD Portalı'ndan silinmiş, Office 365 grupları yönetin. Bu değişikliği geri yükleme, kuruluşunuz tarafından artık gerekmeyen tüm grupları kalıcı olarak süreniz birlikte silmek hangi grupların kullanılabildiğini görmek için yardımcı olur.

Daha fazla bilgi için [geri yükleme süresi doldu veya grupları silinmiş](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Çoklu oturum açma artık uygulama ara sunucusu (genel Önizleme) ile Azure AD SAML güvenli şirket içi uygulamalar için kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Ara sunucusu  
**Ürün özelliği:** Erişim Denetimi

Şimdi, şirket içinde uygulama proxy'si aracılığıyla bu uygulamalara uzaktan erişimi birlikte SAML kimlik doğrulaması uygulamaları için çoklu oturum açma (SSO) deneyimi sağlayabilirsiniz. Şirket içi uygulamalarınızı SAML SSO'yu ayarlama hakkında daha fazla bilgi için bkz. [SAML çoklu oturum açma uygulama ara sunucusu (Önizleme) ile şirket içi uygulamalar için](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>Güvenilirlik ve kullanıcı deneyimini geliştirmek için istemci uygulamaları istek Döngülerde kesintiye uğrar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

İstemci uygulamaları, kısa bir süre içinde aynı oturum açma isteklerinin yüzlerce yanlış verebilir. Bunlar, başarılı olun ister bu istekleri tüm bir kullanıcı deneyimi zayıf ve yüksek iş yükleri için tüm kullanıcılar için gecikme süresini artırmak ve IDP kullanılabilirliğini azaltarak IDP için katkıda bulunur.

Bu güncelleştirme gönderen bir `invalid_grant` hata: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` yinelenen istekleri birden çok kez bir kısa süre içinde normal işlem kapsamı dışında sorun istemci uygulamaları için. Bu hatayla karşılaşmaya istemci uygulamaları, kullanıcı yeniden oturum açmanızı gerektiren, etkileşimli bir istemi göstermelidir. Bu hatayla karşılaşırsa, uygulamanızı nasıl ve bu değişiklik hakkında daha fazla bilgi için bkz: [kimlik doğrulaması için yenilikler nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Yeni denetim günlüklerini kullanıcı deneyimini kullanıma sunuldu

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Yeni bir Azure AD oluşturduk **denetim günlükleri** okunabilirlik ve bilgilerinizi nasıl arama geliştirmek için sayfa. Yeni görmek için **denetim günlükleri** sayfasında **denetim günlükleri** içinde **etkinlik** Azure AD'nin bölüm.

![Örnek bilgileri ile yeni denetim günlükleri sayfası](media/whats-new/audit-logs-page.png)

Yeni hakkında daha fazla bilgi için **denetim günlükleri** sayfasında, bkz: [denetim etkinlik raporları Azure Active Directory portalındaki](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Yeni uyarılar ve rehberlik yanlışlıkla yönetici kilitleme yapılandırılmış koşullu erişim ilkelerinin önlemeye yardımcı olmak için

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Yöneticilerin yanlışlıkla kendilerini yapılandırılmış koşullu erişim ilkeleri aracılığıyla kendi kiracılar dışında kilitlemelerini önlemek için yeni uyarılar ve güncelleştirilmiş yönergeler Azure Portalı'nda oluşturduk. Yeni bir kılavuz hakkında daha fazla bilgi için bkz: [Hizmet bağımlılıklarını Azure Active Directory koşullu erişim nedir](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Mobil cihazlarda kullanım deneyimlerini geliştirilmiş son kullanıcı koşulları

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün özelliği:** İdare

Mevcut nasıl gözden geçirin ve bir mobil cihazda kullanım koşullarını kabul edersiniz geliştirmeye yardımcı olmak için kullanım deneyimlerini koşullarımızı güncelleştirdik. Artık yakınlaştırma ve uzaklaştırma, geri dönün bilgilerini indirerek ve köprüler seçin. Güncellenmiş kullanım koşulları hakkında daha fazla bilgi için bkz: [kullanım Özelliği Azure Active Directory koşullarını](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Kullanılabilir deneyimi yeni Azure AD etkinlik günlüklerini indir

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Etkinlik günlükleri büyük miktarlarda doğrudan Azure Portalı'ndan indirebilirsiniz. Bu güncelleştirme şunları yapmanızı sağlar:

- En fazla 250.000 satırları indirin.

- İndirme tamamlandıktan sonra bildirim alın.

- Dosya adınızı özelleştirin.

- Çıkış biçimi, JSON veya CSV belirleyin.

Bu özellik hakkında daha fazla ayrıntı için bkz. [hızlı başlangıç: Azure portalını kullanarak bir denetim raporu indir](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Hataya neden olan değişiklik: Koşul değerlendirme Exchange ActiveSync (EAS) göre güncelleştirmeler

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Erişim Denetimi

Exchange ActiveSync (EAS) aşağıdaki koşulların nasıl değerlendirir güncelleştiriliyor duyuyoruz:

- Ülke, bölge veya IP adresine göre kullanıcı konumu

- Oturum açma riski

- Cihaz platformu

Koşullu erişim ilkelerinizi Bu koşullar daha önce kullanmadıysanız, koşul davranışı değişebilir dikkat edin. Örneğin, ilke kullanıcı konum koşulu daha önce kullandıysanız, artık iki tanesinden ilke kullanıcı konumunu temel alarak fark edebilirsiniz.

---

## <a name="february-2019"></a>Şubat 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Yapılandırılabilir Azure AD SAML belirteci şifreleme (genel Önizleme) 

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** SSO

Şifrelenmiş SAML belirteçlerini almak için desteklenen tüm SAML uygulamasında artık yapılandırabilirsiniz. Azure AD, bir uygulama ile kullanılan ve yapılandırılmış olduğunda, Azure AD'de depolanan bir sertifika alınan bir ortak anahtar kullanarak yayılan SAML onaylamalarını şifreler.

SAML belirteci şifreleme yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma Azure AD SAML belirteci şifreleme](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Grup veya Azure AD erişim gözden geçirmelerini kullanarak uygulamaları için erişim gözden geçirmesi oluştur

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Erişim Gözden Geçirmeleri  
**Ürün özelliği:** İdare

Artık birden çok gruba dahil edebilirsiniz veya tek bir Azure AD uygulamalarında grup üyeliğini veya uygulama ataması için gözden geçirme erişebilirsiniz. Erişim gözden geçirmeleri ile birden çok grup veya aynı ayarları kullanarak uygulamalar ayarlamak ve dahil edilen tüm gözden geçirenler aynı anda bildirilir.

Hakkında daha fazla bilgi için Azure AD erişim gözden geçirmelerini kullanarak bir erişim gözden geçirmesi oluşturma, bkz: [Azure AD erişim gözden geçirmeleri grupları ve uygulamaları, erişim gözden geçirmesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Şubat 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Şubat 2019 ' federasyon ile 27 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT PARMAK](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [ IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), İzin öğesini [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [Sismik ](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [Paylaşımı A Dream](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [iki Web yöntemini tümleştirme bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [herhangi bir Bilgi Bankası LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope veri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [PureCloud Genesystarafından](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp üretkenlik platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Gelişmiş birleşik MFA/SSPR kaydı

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Self Servis parola sıfırlama  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Müşteri geri bildirimine yanıt olarak, birleştirilmiş MFA/SSPR kayıt Önizleme deneyimini daha hızlı bir şekilde MFA ve SSPR için güvenlik bilgilerini kaydetmek için kullanıcıların yardımcı olma geliştirdik. 

**Etkinleştirmek için kullanıcılarınızın için Gelişmiş deneyimine Bugün, şu adımları izleyin:**

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi oturum açın ve gidin **Azure Active Directory > Kullanıcı Ayarları > erişim paneli Önizleme özellikleri için ayarları yönetme**. 

2. İçinde **Önizleme kullanabilen kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için özellikleri – Yenile** seçeneğinde, özelliklerini açmak seçin bir **kullanıcı seçili grup** veya **tüm kullanıcılar** .

Önümüzdeki birkaç hafta boyunca biz bunu açık yoksa kiracılar için eski birleşik MFA/SSPR kayıt Önizleme deneyimini etkinleştirmek için özelliği kaldırılıyor.

**Kiracınız için Denetim kaldırılacaktır görmek için aşağıdaki adımları izleyin:**

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi oturum açın ve gidin **Azure Active Directory > Kullanıcı Ayarları > erişim paneli Önizleme özellikleri için ayarları yönetme**.  

2. Varsa **kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilen kullanıcılar** seçeneği **hiçbiri**, kiracınızdan seçeneği kaldırılır.

Açık olup olmadığı, daha önce eski birleşik MFA/SSPR kayıt bağımsız olarak kullanıcılar için deneyimi veya Önizleme, gelecekteki bir tarihte eski deneyimi kapatılır. Bu nedenle kesin için yeni ve geliştirilmiş deneyimini olabildiğince çabuk taşıma öneririz.

Geliştirilmiş kayıt deneyimi hakkında daha fazla bilgi için bkz. [seyrek erişimli geliştirmeler Azure AD MFA birleştirilir ve parola sıfırlama kayıt deneyimi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Kullanıcı akışları için güncelleştirilmiş ilke yönetim deneyimi

**Türü:** Değişen özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Kullanıcı akışları (daha önce olarak bilinen, yerleşik ilkeleri) daha kolay ilke oluşturma ve yönetme işlemlerinde güncelleştirdik. Bu yeni deneyim artık tüm Azure AD kiracılar için varsayılandır.

Gülümseme Gönder'i kullanarak ek geri bildirim ve öneriler sağlamak ya da simgeleri frown **bize geri bildirim gönderin** portal ekranın üst kısmındaki alan.

Yeni ilke yönetimi deneyimi hakkında daha fazla bilgi için bkz. [Azure AD B2C şimdi olan JavaScript özelleştirme ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blogu.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Azure AD B2C tarafından sağlanan belirli sayfa öğesi sürümlerini seçin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Şimdi, Azure AD B2C tarafından sağlanan sayfa öğeleri belirli bir sürümünü de seçebilirsiniz. Belirli bir sürüm seçerek yaptığınız güncelleştirmeler bir sayfada görünürler ve tahmin edilebilir davranış alabilirsiniz önce sınayabilirsiniz. Ayrıca, artık JavaScript özelleştirmeleri izin vermek için belirli bir sayfaya sürümlerini zorlamak için kabul. Bu özelliği etkinleştirmek için şuraya gidin: **özellikleri** kullanıcı akışlarınızda sayfası.

Sayfa öğelerinin belirli sürümler seçme hakkında daha fazla bilgi için bkz. [Azure AD B2C şimdi olan JavaScript özelleştirme ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blogu.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>Yapılandırılabilir son kullanıcı parola gereksinimleri için B2C (GA)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Yerel kullanmak zorunda olmak yerine, son kullanıcılar için artık kuruluşunuzun parola karmaşıklığını ayarlayabilirsiniz Azure AD parola ilkesi. Gelen **özellikleri** dikey penceresinde, kullanıcı akışları (daha önce yerleşik ilkeleri olarak da bilinir), parola karmaşıklığını seçebilirsiniz **basit** veya **güçlü**, veya oluşturma bir **özel** gereksinimleri bir dizi.

Parola karmaşıklık gereksinimini yapılandırması hakkında daha fazla bilgi için bkz. [parolaların karmaşıklık gereksinimleri Azure Active Directory B2C'de yapılandırma](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Özel markalı kimlik doğrulama deneyimleri için yeni bir varsayılan şablonları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Bulunan yeni varsayılan şablonlarımız, kullanabileceğiniz **sayfa düzenleri** (daha önce yerleşik ilkeleri olarak da bilinir), kullanıcı Akışları'nın dikey penceresini kullanıcılarınız için kimlik doğrulama deneyimi markalı bir özel oluşturmak için.

Şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure AD B2C şimdi olan JavaScript özelleştirme ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595).

---

## <a name="january-2019"></a>Ocak 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Bir kerelik geçiş kodu kimlik doğrulaması (genel Önizleme) kullanarak active Directory B2B işbirliği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

Azure AD gibi başka bir yolla, bir Microsoft hesabı (MSA) veya Google Federasyon kimlik doğrulaması yapılamayan B2B Konuk kullanıcılar için bir kerelik geçiş kodu kimlik doğrulaması (OTP) tanıttık. Bu yeni kimlik doğrulama yöntemi, kullanıcıların yeni bir Microsoft hesabı oluşturmanız gerekmez, Konuk anlamına gelir. Bunun yerine, bir davet kuponumu kullanmakta veya paylaşılan bir kaynağa erişme, Konuk kullanıcı bir e-posta adresine gönderilecek geçici bir kod isteyebilir. Bu geçici bir kod kullanarak Konuk kullanıcı oturum açmak devam edebilirsiniz.

Daha fazla bilgi için [e-posta bir kerelik geçiş kodu kimlik doğrulama (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) ve blog [Azure AD'ye yapar paylaşım ve işbirliği sorunsuz herhangi bir kullanıcı herhangi bir hesap için](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Yeni Azure AD uygulama ara sunucusu tanımlama bilgisi ayarları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Ara sunucusu  
**Ürün özelliği:** Erişim Denetimi

Üç yeni tanımlama bilgisi ayarları, uygulama proxy'si aracılığıyla yayımlandığından uygulamalarınız için kullanılabilir tanıttık:

- **Yalnızca HTTP tanımlama bilgisi kullanın.** Kümeleri **HTTPOnly** , uygulama ara sunucusu erişim ve oturum tanımlama bilgileri bayrakta. Bu ayarın etkinleştirilmesi, kopyalama veya istemci tarafı komut dosyası kullanarak tanımlama bilgilerinin değiştirme önlemeye yardımcı olma gibi ek güvenlik fayda sağlar. Bu bayrağı açmanızı öneririz (seçin **Evet**) avantajlar için.

- **Güvenli bir tanımlama bilgisi kullanın.** Kümeleri **güvenli** , uygulama ara sunucusu erişim ve oturum tanımlama bilgileri bayrakta. Bu ayarın etkinleştirilmesi, tanımlama bilgileri yalnızca HTTPS gibi güvenli TLS Kanallar üzerinden aktarılan vererek, ek güvenlik avantaj sağlar. Bu bayrağı açmanızı öneririz (seçin **Evet**) avantajlar için.

- **Kalıcı bir tanımlama bilgisi kullanın.** Erişim tanımlama bilgileri, web tarayıcı kapatıldığında süresinin dolmasını önler. Bu tanımlama bilgileri, erişim belirtecinin ömrü boyunca son. Ancak, tanımlama bilgisi süre sonu ulaşılırsa veya kullanıcıyı el ile tanımlama bilgisi siler sıfırlanır. Varsayılan ayar tutmanızı öneririz **Hayır**, yalnızca ayar tanımlama bilgilerini işlemler arasında paylaşmayın eski uygulamaları kapatma.

Yeni tanımlama hakkında daha fazla bilgi için bkz: [Azure Active Directory'de şirket içi uygulamalara erişmek için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Ocak 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Ocak 2019 ' federasyon ile 35 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [beceri palet](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco terimdir](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet erişimi Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [süre sonu anımsatıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Görüntüleyicisi](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [fiil](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Kapat Pavaso dijital](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Araç Seti](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [ Bulut hizmeti PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Kurumsal ARES](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [Office 365 için K2](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Ferry ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Azure AD kimlik koruması yenilikleri (genel Önizleme)

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Biz Azure AD kimlik koruması genel önizlemeye sunuldu teklifin aşağıdaki geliştirmeleri ekledik duyurmaktan Mutluluk duyuyoruz dahil olmak üzere:

- Güncelleştirilmiş ve daha tümleşik kullanıcı arabirimi

- Diğer API’ler

- Makine öğrenimi aracılığıyla Gelişmiş risk değerlendirmesi

- Ürün genelinde hizalama riskli kullanıcılar ve riskli oturum açma işlemleri

İyileştirmeler hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması nedir (yenilenebilecek)?](https://aka.ms/IdentityProtectionDocs) Daha fazla bilgi edinin ve fikirlerinizi ürün içindeki istemleri aracılığıyla paylaşın.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>Microsoft Authenticator uygulamasını iOS ve Android cihazları için yeni bir uygulama kilidi özelliği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Microsoft Authenticator uygulaması  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Bir kerelik geçiş kodları, uygulama bilgilerini ve uygulama ayarlarını daha güvenli tutmak için Microsoft Authenticator uygulamasını Uygulama kilidi özelliğini kapatabilirsiniz. Uygulama kilidi açma, Microsoft Authenticator uygulamasını her açışlarında PIN'İNİZİ kullanarak kimlik doğrulaması için sorulan veya biyometrik olacaksınız anlamına gelir.

Daha fazla bilgi için [Microsoft Authenticator uygulaması hakkında SSS](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Gelişmiş Azure AD Privileged Identity Management (PIM) özellikleri Dışarı Aktar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management

Privileged Identity Management (PIM) yöneticileri, artık tüm alt kaynaklar için rol atamalarını içeren belirli bir kaynak için tüm etkin ve uygun rolü atamalarını dışarı aktarabilirsiniz. Daha önce yöneticilerin bir abonelik için rol atamalarını tam bir listesini almak için zordu ve her belirli bir kaynak rolü atamalarını dışarı aktarma gerekiyordu.

Daha fazla bilgi için [PIM Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
