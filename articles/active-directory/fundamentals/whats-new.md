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
ms.date: 02/28/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5d85d1f211a4cc0307cca6d631a4bf286d3e576
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59271824"
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

## <a name="april-2019"></a>Nisan 2019

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Uygulama koruma tabanlı koşullu erişim ilkesi, Azure AD'de (genel Önizleme) kullanarak daha yüksek güvenlik

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Kimlik Güvenliği ve Koruması

Uygulama koruma tabanlı koşullu erişim artık kullanılabilir kullanarak **uygulama koruması gerektiren** ilkesi. Bu yeni ilkenin önlemek için yardımcı olarak, kuruluşunuzun güvenliğini artırmaya yardımcı olur:

- Kullanıcılar Microsoft Intune lisansı olmadan uygulamalara erişim sağlamasını.

- Bir Microsoft Intune uygulama koruma ilkesini almak çözememesi kullanıcılar.

- Yapılandırılmış bir Intune uygulama koruma İlkesi olmadan uygulamalara erişim sağlamasını kullanıcılar.

Daha fazla bilgi için [koşullu erişim ile bulut uygulaması erişimi için uygulama koruma ilkesini zorunlu kılma](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

## <a name="march-2019"></a>Mart 2019

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Yeni Azure AD çoklu oturum açma ve koşullu erişimi Microsoft edge'de (genel Önizleme) desteği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün özelliği:** Kimlik Güvenliği ve Koruması

Azure AD çoklu oturum açma ve koşullu erişim için yeni destek sağlamak da dahil, Microsoft Edge için sunduğumuz Azure AD desteğini geliştirdik. Microsoft Intune Managed Browser'ı daha önce kullandıysanız, artık Microsoft Edge yerine kullanabilirsiniz.

Ayarlama ve koşullu erişim kullanarak uygulamaları ve cihazları yönetme hakkında daha fazla bilgi için bkz. [gerektiren yönetilen cihazlar için koşullu erişim ile bulut uygulaması erişimi](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/require-managed-devices) ve [gerektiren onaylı istemci uygulamalarını bulut için Koşullu erişimle uygulama](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/app-based-conditional-access). Microsoft Edge ile Microsoft Intune ilkeleri kullanarak erişimi yönetme hakkında daha fazla bilgi için bkz. [Intune ilkeyle korunan bir tarayıcı kullanarak Internet erişimini yönetme](https://docs.microsoft.com/en-us/intune/app-configuration-managed-browser).

---

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Kimlik deneyimi çerçevesi ve özel ilke Azure Active Directory B2C'de desteği kullanıma (GA) sunuldu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
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
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme

Mart 2019 ' federasyon ile 14 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[ISEC7 mobil Exchange temsilci](https://www.isec7.com/english/), [MediusFlow](https://office365.cloudapp.mediusflow.com/), [ePlatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [Fulcrum](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [ExcelityGlobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama tabanlı denetim sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [Powerschool performans sorunları](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [Cinode](https://cinode.com/), [Iris İntranet](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [Empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [GÖR](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Yeni Zscaler ve Azure AD Galerisi - Mart 2019 bağlayıcılar sağlama Atlassian

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün özelliği:** 3. Taraf Tümleştirme

Oluşturma, güncelleştirme ve silme aşağıdaki uygulamalar için kullanıcı hesapları otomatik hale getirin:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](http://aka.ms/ZscalerBetaProvisioning), [Zscaler bir](https://aka.ms/ZscalerOneProvisioning), [Zscaler iki](http://aka.ms/ZscalerTwoProvisioning), [Zscaler üç](http://aka.ms/ZscalerThreeProvisioning), [Zscaler ZSCloud ](http://aka.ms/ZscalerZSCloudProvisioning), [Atlassian bulut](http://aka.ms/atlassianCloudProvisioning)

Daha iyi otomatik kullanıcı hesabı sağlama üzerinden kuruluşunuzun güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin](http://aka.ms/ProvisioningDocumentation).

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
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün özelliği:** Erişim Denetimi

Şimdi, şirket içinde uygulama proxy'si aracılığıyla bu uygulamalara uzaktan erişimi birlikte SAML kimlik doğrulaması uygulamaları için çoklu oturum açma (SSO) deneyimi sağlayabilirsiniz. Şirket içi uygulamalarınızı SAML SSO'yu ayarlama hakkında daha fazla bilgi için bkz. [SAML çoklu oturum açma uygulama ara sunucusu (Önizleme) ile şirket içi uygulamalar için](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>Güvenilirlik ve kullanıcı deneyimini geliştirmek için istemci uygulamaları istek Döngülerde kesintiye uğrar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Doğrulamaları (Oturum Açma İşlemleri)  
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
**Ürün özelliği:** Kimlik Güvenliği ve Koruması

Yöneticilerin yanlışlıkla kendilerini yapılandırılmış koşullu erişim ilkeleri aracılığıyla kendi kiracılar dışında kilitlemelerini önlemek için yeni uyarılar ve güncelleştirilmiş yönergeler Azure Portalı'nda oluşturduk. Yeni bir kılavuz hakkında daha fazla bilgi için bkz: [Hizmet bağımlılıkları Azure Active Directory koşullu erişim nedir](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Mobil cihazlarda kullanım deneyimlerini geliştirilmiş son kullanıcı koşulları

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare

Nasıl gözden geçirin ve bir mobil cihazda kullanım koşullarını kabul edersiniz geliştirmeye yardımcı olmak için mevcut koşullarımızı kullanımı deneyimleri güncelleştirdik. Artık yakınlaştırma ve uzaklaştırma, geri dönün bilgilerini indirerek ve köprüler seçin. Güncellenmiş kullanım koşulları hakkında daha fazla bilgi için bkz: [kullanım özelliği, Azure Active Directory koşulları](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

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

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Yeni değişiklik: Koşul değerlendirme Exchange ActiveSync (EAS) göre güncelleştirmeler

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

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Yapılandırılabilir Azure AD SAML belirteci şifreleme (Genel önizleme) 

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** SSO

Şifrelenmiş SAML belirteçlerini almak için desteklenen tüm SAML uygulamasında artık yapılandırabilirsiniz. Azure AD, bir uygulama ile kullanılan ve yapılandırılmış olduğunda, Azure AD'de depolanan bir sertifika alınan bir ortak anahtar kullanarak yayılan SAML onaylamalarını şifreler.

SAML belirteci şifreleme yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma Azure AD SAML belirteci şifreleme](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Azure AD Erişim Gözden Geçirmelerini kullanarak gruplar veya uygulamalar için erişim incelemesi oluşturma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Erişim Gözden Geçirmeleri  
**Ürün özelliği:** İdare

Artık birden çok gruba dahil edebilirsiniz veya tek bir Azure AD uygulamalarında grup üyeliğini veya uygulama ataması için gözden geçirme erişebilirsiniz. Erişim gözden geçirmeleri ile birden çok grup veya aynı ayarları kullanarak uygulamalar ayarlamak ve dahil edilen tüm gözden geçirenler aynı anda bildirilir.

Hakkında daha fazla bilgi için Azure AD erişim gözden geçirmelerini kullanarak bir erişim gözden geçirmesi oluşturma, bkz: [Azure AD erişim gözden geçirmeleri grupları ve uygulamaları, erişim gözden geçirmesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanıma sunuldu - Şubat 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Şubat 2019 ' federasyon ile 27 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT PARMAK](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [ IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), İzin öğesini [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [Sismik ](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [Paylaşımı A Dream](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [iki Web yöntemini tümleştirme bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [herhangi bir Bilgi Bankası LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope veri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [PureCloud Genesystarafından](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp üretkenlik platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Gelişmiş birleşik MFA/SSPR kaydı

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Self Servis Parola Sıfırlama  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması
 
Müşteri geri bildirimine yanıt olarak, birleştirilmiş MFA/SSPR kayıt Önizleme deneyimini daha hızlı bir şekilde MFA ve SSPR için güvenlik bilgilerini kaydetmek için kullanıcıların yardımcı olma geliştirdik. 

**Kullanıcılarınız için geliştirilmiş deneyim hemen açmak için şu adımları izleyin:**

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi oturum açın ve gidin **Azure Active Directory > Kullanıcı Ayarları > erişim paneli Önizleme özellikleri için ayarları yönetme**. 

2. İçinde **Önizleme kullanabilen kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için özellikleri – Yenile** seçeneğinde, özelliklerini açmak seçin bir **kullanıcı seçili grup** veya **tüm kullanıcılar** .

Önümüzdeki birkaç hafta boyunca biz bunu açık yoksa kiracılar için eski birleşik MFA/SSPR kayıt Önizleme deneyimini etkinleştirmek için özelliği kaldırılıyor.

**Kiracınız için Denetim kaldırılacaktır görmek için aşağıdaki adımları izleyin:**

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi oturum açın ve gidin **Azure Active Directory > Kullanıcı Ayarları > erişim paneli Önizleme özellikleri için ayarları yönetme**.  

2.  Varsa **kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilen kullanıcılar** seçeneği **hiçbiri**, kiracınızdan seçeneği kaldırılır.

Açık olup olmadığı, daha önce eski birleşik MFA/SSPR kayıt bağımsız olarak kullanıcılar için deneyimi veya Önizleme, gelecekteki bir tarihte eski deneyimi kapatılır. Bu nedenle kesin için yeni ve geliştirilmiş deneyimini olabildiğince çabuk taşıma öneririz.

Geliştirilmiş kayıt deneyimi hakkında daha fazla bilgi için bkz. [seyrek erişimli geliştirmeler Azure AD MFA birleştirilir ve parola sıfırlama kayıt deneyimi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Kullanıcı akışları için güncelleştirilmiş ilke yönetimi deneyimi

**Türü:** Değişen özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Kullanıcı akışları (daha önce olarak bilinen, yerleşik ilkeleri) daha kolay ilke oluşturma ve yönetme işlemlerinde güncelleştirdik. Bu yeni deneyim artık tüm Azure AD kiracılar için varsayılandır.

Gülümseme Gönder'i kullanarak ek geri bildirim ve öneriler sağlamak ya da simgeleri frown **bize geri bildirim gönderin** portal ekranın üst kısmındaki alan.

Yeni ilke yönetimi deneyimi hakkında daha fazla bilgi için bkz. [Azure AD B2C şimdi olan JavaScript özelleştirme ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blogu.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Azure AD B2C tarafından sağlanan belirli sayfa öğesi sürümlerini seçme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Şimdi, Azure AD B2C tarafından sağlanan sayfa öğeleri belirli bir sürümünü de seçebilirsiniz. Belirli bir sürüm seçerek yaptığınız güncelleştirmeler bir sayfada görünürler ve tahmin edilebilir davranış alabilirsiniz önce sınayabilirsiniz. Ayrıca, artık JavaScript özelleştirmeleri izin vermek için belirli bir sayfaya sürümlerini zorlamak için kabul. Bu özelliği etkinleştirmek için şuraya gidin: **özellikleri** kullanıcı akışlarınızda sayfası.

Sayfa öğelerinin belirli sürümler seçme hakkında daha fazla bilgi için bkz. [Azure AD B2C şimdi olan JavaScript özelleştirme ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blogu.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>B2C için yapılandırılabilir son kullanıcı parola gereksinimleri (GA)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Yerel kullanmak zorunda olmak yerine, son kullanıcılar için artık kuruluşunuzun parola karmaşıklığını ayarlayabilirsiniz Azure AD parola ilkesi. Gelen **özellikleri** dikey penceresinde, kullanıcı akışları (daha önce yerleşik ilkeleri olarak da bilinir), parola karmaşıklığını seçebilirsiniz **basit** veya **güçlü**, veya oluşturma bir **özel** gereksinimleri bir dizi.

Parola karmaşıklık gereksinimini yapılandırması hakkında daha fazla bilgi için bkz. [parolaların karmaşıklık gereksinimleri Azure Active Directory B2C'de yapılandırma](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Özel markalı kimlik doğrulaması deneyimleri için yeni varsayılan şablonlar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Bulunan yeni varsayılan şablonlarımız, kullanabileceğiniz **sayfa düzenleri** (daha önce yerleşik ilkeleri olarak da bilinir), kullanıcı Akışları'nın dikey penceresini kullanıcılarınız için kimlik doğrulama deneyimi markalı bir özel oluşturmak için.

Şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure AD B2C şimdi olan JavaScript özelleştirme ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595).

---

## <a name="january-2019"></a>Ocak 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Bir kerelik geçiş kodu kimlik doğrulaması kullanarak Active Directory B2B işbirliği (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

Azure AD gibi başka bir yolla, bir Microsoft hesabı (MSA) veya Google Federasyon kimlik doğrulaması yapılamayan B2B Konuk kullanıcılar için bir kerelik geçiş kodu kimlik doğrulaması (OTP) tanıttık. Bu yeni kimlik doğrulama yöntemi, kullanıcıların yeni bir Microsoft hesabı oluşturmanız gerekmez, Konuk anlamına gelir. Bunun yerine, bir davet kuponumu kullanmakta veya paylaşılan bir kaynağa erişme, Konuk kullanıcı bir e-posta adresine gönderilecek geçici bir kod isteyebilir. Bu geçici bir kod kullanarak Konuk kullanıcı oturum açmak devam edebilirsiniz.

Daha fazla bilgi için [e-posta bir kerelik geçiş kodu kimlik doğrulama (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) ve blog [Azure AD'ye yapar paylaşım ve işbirliği sorunsuz herhangi bir kullanıcı herhangi bir hesap için](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Yeni Azure AD uygulama ara sunucusu tanımlama bilgisi ayarları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün özelliği:** Erişim Denetimi

Üç yeni tanımlama bilgisi ayarları, uygulama proxy'si aracılığıyla yayımlandığından uygulamalarınız için kullanılabilir tanıttık:

- **Yalnızca HTTP tanımlama bilgisi kullanın.** Kümeleri **HTTPOnly** , uygulama ara sunucusu erişim ve oturum tanımlama bilgileri bayrakta. Bu ayarın etkinleştirilmesi, kopyalama veya istemci tarafı komut dosyası kullanarak tanımlama bilgilerinin değiştirme önlemeye yardımcı olma gibi ek güvenlik fayda sağlar. Bu bayrağı açmanızı öneririz (seçin **Evet**) avantajlar için.

- **Güvenli bir tanımlama bilgisi kullanın.** Kümeleri **güvenli** , uygulama ara sunucusu erişim ve oturum tanımlama bilgileri bayrakta. Bu ayarın etkinleştirilmesi, tanımlama bilgileri yalnızca HTTPS gibi güvenli TLS Kanallar üzerinden aktarılan vererek, ek güvenlik avantaj sağlar. Bu bayrağı açmanızı öneririz (seçin **Evet**) avantajlar için.

- **Kalıcı bir tanımlama bilgisi kullanın.** Erişim tanımlama bilgileri, web tarayıcı kapatıldığında süresinin dolmasını önler. Bu tanımlama bilgileri, erişim belirtecinin ömrü boyunca son. Ancak, tanımlama bilgisi süre sonu ulaşılırsa veya kullanıcıyı el ile tanımlama bilgisi siler sıfırlanır. Varsayılan ayar tutmanızı öneririz **Hayır**, yalnızca ayar tanımlama bilgilerini işlemler arasında paylaşmayın eski uygulamaları kapatma.

Yeni tanımlama hakkında daha fazla bilgi için bkz: [Azure Active Directory'de şirket içi uygulamalara erişmek için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanıma sunuldu - Ocak 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Ocak 2019 ' federasyon ile 35 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [beceri palet](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco terimdir](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet erişimi Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [süre sonu anımsatıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Görüntüleyicisi](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [fiil](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Kapat Pavaso dijital](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Araç Seti](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [ Bulut hizmeti PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Kurumsal ARES](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [Office 365 için K2](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Ferry ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Yeni Azure AD Identity Protection geliştirmeleri (Genel önizleme)

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Kimlik Güvenliği ve Koruması

Biz Azure AD kimlik koruması genel önizlemeye sunuldu teklifin aşağıdaki geliştirmeleri ekledik duyurmaktan Mutluluk duyuyoruz dahil olmak üzere:

- Güncelleştirilmiş ve daha tümleşik kullanıcı arabirimi

- Diğer API’ler

- Makine öğrenimi aracılığıyla Gelişmiş risk değerlendirmesi

- Ürün genelinde hizalama riskli kullanıcılar ve riskli oturum açma işlemleri

İyileştirmeler hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması nedir (yenilenebilecek)?](https://aka.ms/IdentityProtectionDocs) Daha fazla bilgi edinin ve fikirlerinizi ürün içindeki istemleri aracılığıyla paylaşın.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>iOS ve Android cihazlarda Microsoft Authenticator uygulaması için yeni Uygulama Kilidi özelliği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Microsoft Authenticator Uygulaması  
**Ürün özelliği:** Kimlik Güvenliği ve Koruması

Bir kerelik geçiş kodları, uygulama bilgilerini ve uygulama ayarlarını daha güvenli tutmak için Microsoft Authenticator uygulamasını Uygulama kilidi özelliğini kapatabilirsiniz. Uygulama kilidi açma, Microsoft Authenticator uygulamasını her açışlarında PIN'İNİZİ kullanarak kimlik doğrulaması için sorulan veya biyometrik olacaksınız anlamına gelir.

Daha fazla bilgi için [Microsoft Authenticator uygulaması hakkında SSS](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Gelişmiş Azure AD Privileged Identity Management (PIM) dışarı aktarma özellikleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management

Privileged Identity Management (PIM) yöneticileri, artık tüm alt kaynaklar için rol atamalarını içeren belirli bir kaynak için tüm etkin ve uygun rolü atamalarını dışarı aktarabilirsiniz. Daha önce yöneticilerin bir abonelik için rol atamalarını tam bir listesini almak için zordu ve her belirli bir kaynak rolü atamalarını dışarı aktarma gerekiyordu.

Daha fazla bilgi için [PIM Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="novemberdecember-2018"></a>Kasım/aralık 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Eşitleme kapsamından kaldırılan kullanıcılar artık yalnızca bulut hesaplarına geçmez

**Türü:** Sabit  
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün özelliği:** Dizin

>[!Important]
>Biz aldık ve nedeniyle bu düzeltme, sıkıntıya anlayın. Alana kadar bu nedenle, biz bu değişikliği düzeltme, kuruluşunuzda uygulamak daha kolay yapabiliyoruz, geri.

Bir kullanıcının DirSyncEnabled bayrağını deneyebileceğinizi moduna geçiş yapılamaz için bir hatayı düzelttik **False** Active Directory etki alanı Hizmetleri (AD DS) nesnesinin eşitleme kapsamının dışında bırakılan ve ardından geri dönüşüm kutusu için taşınır zaman Azure AD üzerinde aşağıdaki eşitleme döngüsü. Kullanıcı eşitleme kapsamının dışında bırakılan ve daha sonra Azure AD Geri Dönüşüm Kutusu'ndan, geri bu düzeltmenin sonucu olarak, kullanıcı hesabı gelen eşitlenmiş olarak kalır AD, beklendiği gibi şirket içinde ve bulutta kaynağına yetki başlangıcı (SoA) olarak kalır beri yönetilemez Böylece, şirket içi AD.

Bu düzeltme önce bir sorun oluştu, DirSyncEnabled bayrağını yanlış olarak değiştirildi. Bu hesaplar için yalnızca bulutta yer alan nesneler dönüştürüldü ve bulutta yönetilen hesapları yanlış izlenimini verdiği. Hesapları geldiğini, SoA şirket içi ve tüm eşitlenmiş özellikleri (gölge öznitelikleri) olarak yine de korunur ancak şirket içi AD. Bu durumun nedeni, Azure AD'de birden çok sorunla ve bu hesaplar AD'den eşitlenen olarak değerlendirilecek bekleniyordu, ancak artık yalnızca bulut hesapları gibi mu davrandığı, diğer bulut iş yüklerini (Exchange Online gibi).

Şu anda gerçekten eşitlenmiş gelen-AD hesabı yalnızca bulut hesabına dönüştürmek için tek DirSync SoA aktarmak için bir arka uç işlemi tetikler ve Kiracı düzeyinde devre dışı bırakarak yoludur. Bu tür bir SoA değişiklik gerektirir (ancak bunlarla sınırlı değil) tüm şirket içi temizleme ilişkili öznitelikleri (LastDirSyncTime gibi ve gölge öznitelikler) ve diğer bulut iş yüklerini yalnızca bulutta yer alan bir hesap için çok dönüştürülen ilgili nesne için sinyal gönderme .

Bu düzeltme, sonuç olarak gerekli olan bazı senaryolarda geçmişte doğrudan İmmutableıd özniteliği AD'den eşitlenen bir kullanıcı güncelleştirmeleri engeller. Adından da anlaşılacağı gibi tasarım gereği, Azure AD'de bir nesnenin Immutableıd sabit olması amaçlanmıştır. Bu tür senaryolara Azure AD Connect Health ve Azure AD Connect eşitlemesi istemcisinde uygulanan yeni özellikler mevcuttur:

- **Çok aşamalı bir yaklaşım kullanıcılar için büyük ölçekli Immutableıd güncelleştirme**
  
  Örneğin, uzun bir AD DS ormanlar arası geçiş yapmanız gerekir. Çözüm: Azure AD Connect'e kullanın **kaynak bağlantısını yapılandır** ve kullanıcı geçirir, mevcut Immutableıd değerlerini Azure AD'den yerel AD DS kullanıcının ms-DS-tutarlılık-Guid özniteliği yeni ormanın kopyalayın. Daha fazla bilgi için [ms-DS-Consistencyguid'i sourceAnchor olarak kullanma](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Tek seferde çok sayıda kullanıcı için büyük ölçekli Immutableıd güncelleştirmeleri**

  Örneğin, Azure AD Connect uygulama çalışırken bir hata yaparsanız ve artık SourceAnchor özniteliği değiştirmeniz gerekir. Çözüm: DirSync ve Kiracı düzeyinde devre dışı bırakın ve tüm geçersiz Immutableıd değerleri temizleyin. Daha fazla bilgi için [Office 365 için dizin eşitleme devre dışı bırakma](/office365/enterprise/turn-off-directory-synchronization).

- **Azure AD'de mevcut bir kullanıcının şirket içi kullanıcıyla rematch** Örneğin, AD DS'de yeniden oluşturulmuş bir kullanıcı bir yinelenen bir var olan Azure AD hesabı (yalnız bırakılmış nesneye) ile rematching yerine Azure AD hesabı oluşturur. Çözüm: Azure AD Connect Health, Azure portalında kaynak bağlayıcı/Immutableıd yeniden eşlemek için kullanın. Daha fazla bilgi için [Kitaplar'daki nesne senaryo](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Yeni değişiklik: Denetim ve Azure İzleyici aracılığıyla oturum açma günlükleri şema güncelleştirmeleri

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Günlük dosyaları, SIEM araçlarınızı veya Log Analytics ile sorunsuz bir şekilde tümleştirebilmeniz için biz şu anda Azure İzleyici aracılığıyla denetim hem oturum açma günlük akışları yayımlıyor. Geri bildirimleriniz ve bu özelliğin genel kullanılabilirlik duyurusuyla hazırlığında bağlı olarak, aşağıdaki değişiklikleri bizim şemaya yapıyoruz. Bu şema değişiklikleri ve onun ilişkili belge güncelleştirmeleri Ocak ilk haftası tarafından gerçekleşir.

#### <a name="new-fields-in-the-audit-schema"></a>Yeni alan denetim şeması
Ekliyoruz yeni **işlem türü** işlemi türünü sağlamak için alan, kaynak üzerinde gerçekleştirilen. Örneğin, **Ekle**, **güncelleştirme**, veya **Sil**.

#### <a name="changed-fields-in-the-audit-schema"></a>Denetim şeması değiştirilen alanları
Aşağıdaki alanları denetim şemada değiştiriyorsunuz:

|Alan adı|Değişiklikler|Eski değer|Yeni değerleri|
|----------|------------|----------|----------|
|Kategori|Bu **hizmet adı** alan. Artık **denetim kategorisini** alan. **Hizmet adı** adlandırıldı **loggedByService** alan.|<ul><li>Hesap Sağlama</li><li>Çekirdek Dizin</li><li>Self Servis parola sıfırlama</li></ul>|<ul><li>Kullanıcı Yönetimi</li><li>Grup Yönetimi</li><li>Uygulama Yönetimi</li></ul>|
|targetResources|İçerir **TargetResourceType** en üst düzeyde.|&nbsp;|<ul><li>İlke</li><li>Uygulama</li><li>Kullanıcı</li><li>Grup</li></ul>|
|loggedByService|Denetim günlüğü oluşturulan hizmetin adını sağlar.|Null|<ul><li>Hesap Sağlama</li><li>Çekirdek Dizin</li><li>Self servis parola sıfırlama</li></ul>|
|Sonuç|Denetim günlüklerini sonucu sağlar. Daha önce bu listelenmiş, ancak artık gerçek değeri göstereceğiz.|<ul><li>0</li><li>1</li></ul>|<ul><li>Başarılı</li><li>Hata</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Oturum açma şema değiştirilen alanları
Aşağıdaki alanları, oturum açma şemada değiştiriyorsunuz:

|Alan adı|Değişiklikler|Eski değer|Yeni değerleri|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|Bu **conditionalaccessPolicies** alan. Artık **appliedConditionalAccessPolicies** alan.|Değişiklik yok|Değişiklik yok|
|conditionalAccessStatus|Oturum açma işlemi sırasında koşullu erişim ilkesi durumu sonucu sağlar. Daha önce bu listelenmiş, ancak artık gerçek değeri göstereceğiz.|<ul><li>0</li><li>1.</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|
|appliedConditionalAccessPolicies: sonuç|Oturum açma işlemi sırasında bireysel koşullu erişim ilkesi durumu sonucu sağlar. Daha önce bu listelenmiş, ancak artık gerçek değeri göstereceğiz.|<ul><li>0</li><li>1.</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|

Şeması hakkında daha fazla bilgi için bkz. [yorumlama Azure AD denetim günlükleri şema Azure İzleyici (Önizleme)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Denetimli makine öğrenimi modeli ve risk puanı altyapısına yönelik Identity Protection geliştirmeleri

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Risk Puanları

Kimlik koruması ile ilgili kullanıcı ve oturum açma risk değerlendirmesi altyapısı iyileştirmeleri kullanıcı risk doğruluk ve kapsamını artırmak yardımcı olabilir. Yöneticiler kullanıcı risk düzeyi artık doğrudan belirli algılamalar risk düzeyine bağlıdır ve sayısı ve riskli oturum açma olaylarını düzeyi arasında bir artış olduğunu fark edebilirsiniz.

Risk algılama artık, denetimli makine öğrenimi, kullanıcının oturum açma ek özellikleri ve algılamalar desenini kullanarak kullanıcı riski hesaplar modelinde tarafından değerlendirilir. Bu kullanıcı ile ilişkili olan algılama düşük veya Orta riskini olsa bile bu modelini temel alan kullanıcı yüksek risk puanları ile yönetici bulabilirsiniz. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Yöneticiler Microsoft Authenticator uygulamasını kullanarak kendi parolalarını sıfırlayabilir (Genel önizleme)

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Self Servis Parola Sıfırlama  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Azure AD yöneticileri artık Microsoft Authenticator uygulama içi bildirimler veya herhangi bir mobil kimlik doğrulayıcısı uygulaması veya donanım bir kod kullanarak kendi parola sıfırlama belirteci. Kullanıcıların kendi parolalarını sıfırlamak için Yöneticiler artık iki aşağıdaki yöntemlerden birini kullanmanız mümkün olacaktır:

- Microsoft Authenticator uygulama bildirimi

- Diğer Mobil kimlik doğrulayıcısı uygulaması / donanım belirteç kodu

- Email

- Telefon araması

- Kısa mesaj

Parola sıfırlama için Microsoft Authenticator uygulamasını kullanma hakkında daha fazla bilgi için bkz. [Azure AD Self Servis parola sıfırlama - mobil uygulama ve SSPR (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Yeni Azure AD Bulut Cihaz Yöneticisi rolü (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz Kaydı ve Yönetimi  
**Ürün özelliği:** Erişim denetimi

Yöneticiler, kullanıcıların bulut cihaz Yöneticisi görevleri gerçekleştirmek için yeni bulut cihaz yöneticisi rolü atayabilirsiniz. Bulut cihaz Yöneticiler rolünün atandığı kullanıcılar etkinleştirebilir, devre dışı ve Windows 10 BitLocker Anahtarları (varsa) Azure Portalı'nda okuma yetkisi olan yanı sıra Azure AD'de cihazları silin.

Rolleri ve izinleri hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Azure AD'de yeni etkinlik zaman damgasını kullanarak cihazlarınızı yönetin (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz Kaydı ve Yönetimi  
**Ürün özelliği:** Cihaz Yaşam Döngüsü Yönetimi

Biz, zamanla yenileyin ve gerekir, ortamınızda asılı geçici olarak eski cihazları zorunda kalmamak için Azure AD'de, kuruluşların cihazları devre dışı bırakma olduğunu unutmayın. Bu işlemde size yardımcı olacak artık Azure AD, cihaz yaşam döngüsünü yönetmenize yardımcı olacak cihazlarınızı yeni bir etkinlik damgasıyla güncelleştirir.

Alın ve bu zaman damgasından kullanma hakkında daha fazla bilgi için bkz. [nasıl yapılır: Azure AD'de eski cihazları yönetme](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Yöneticiler kullanıcıların her cihazda bir Kullanım Koşulları'nı kabul etmesini gerektirebilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare
 
Yöneticiler şimdi Aç **kullanıcıların her cihazda kabul etmesini zorunlu tut** kiracınızda kullandıkları her bir cihazdaki kullanım koşullarınızı kabul etmelerini zorunlu hale getirin.

Daha fazla bilgi için [cihaz başına kullanım özelliği, Azure Active Directory koşulları bölümünü kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Yöneticiler Kullanım Koşulları'nı yinelenen bir zamanlamaya göre süresi dolacak şekilde yapılandırabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare
 

Yöneticiler şimdi Aç **sona onayları** kullanım koşullarını hale getirme seçeneği süresi dolacak tüm kullanıcılarınız, belirtilen yinelenme çizelgesine dayalıdır. Zamanlama, BI aylık, üç aylık dönem veya aylık olarak yıllık olabilir. Kullanım koşullarını süresi dolduktan sonra kullanıcıların yeniden kabul etmesini gerektirmek gerekir.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları kullanım bölümünü ekleme koşullarını](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Yöneticiler her kullanıcının zamanlamaya göre süresi dolacak şekilde kullanım koşulları yapılandırabilirsiniz.

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare

Yöneticiler artık bir süre belirtin, kullanıcı kullanım koşullarını artırmasını gerekir. Örneğin, Yöneticiler, kullanıcılar her 90 günde Kullanım Koşulları'nı artırmasını gerekir belirtebilirsiniz.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları kullanım bölümünü ekleme koşullarını](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Azure Active Directory rolleri için yeni Azure AD Privileged Identity Management (PIM) e-postaları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Azure AD Privileged Identity Management (PIM) kullanan müşteriler artık son yedi gün için aşağıdaki bilgiler dahil olmak üzere bir Haftalık Özet e-posta alabilir:

- En uygun ve kalıcı rol atamaları genel bakış

- Rol etkinleştirme kullanıcı sayısı

- PIM rollerine atanan kullanıcıların sayısı

- PIM dışında rollerine atanan kullanıcıların sayısı

- Kullanıcılar "yapılmış kalıcı" PIM sayısı

PIM ve kullanılabilir e-posta bildirimleri hakkında daha fazla bilgi için bkz. [e-posta bildirimleri PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Grup tabanlı lisanslama genel kullanıma sunulmuştur

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün özelliği:** Dizin

Grup tabanlı lisanslama, genel Önizleme dışında olan ve genel kullanıma sunulmuştur. Bu genel sürüm bir parçası olarak bu özellik daha ölçeklenebilir hale getirdik ve tek bir kullanıcının grup tabanlı lisans atamalarını suretiyle olanağı ve Office 365 E3/A3 lisanslarıyla grup tabanlı lisanslama kullanma olanağı ekledik.

Grup tabanlı lisanslama hakkında daha fazla bilgi için bkz. [grup tabanlı Azure Active Directory lisansı nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanıma sunuldu - Kasım 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Kasım 2018'de Federasyon ile bu 26 yeni uygulamalar için uygulama Galerisi desteği ekledik:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [gri Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://www.drivedollar.com/Account/Login), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [sonsuz kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [ HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [kayması](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy iş merkezi 365](https://accounting.zenegy.com/), [Everbridge üye portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Hizmet Yöneticisi'ni (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [parçalı uygulamaları - Klasik Test](https://test.plexonline.com/signon), [parçalı uygulamaları – Klasik](https://www.plexonline.com/signon), [parçalı uygulamaları - UX Test](https://test.cloud.plex.com/sso), [parçalı uygulamaları – UX](https://cloud.plex.com/sso), [parçalı uygulamaları – IAM](https://accounts.plex.com/), [UĞRAŞIYOR - Childcare kayıtları, katılımcı ve finansal izleme sistemi](https://getcrafts.ca/craftsregistration) 

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

## <a name="october-2018"></a>Ekim 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD Günlükleri şimdi Azure Log Analytics (Genel önizleme) ile çalışıyor

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Azure Log Analytics için artık Azure AD günlüklerinizi iletebilir duyurmaktan Mutluluk duyuyoruz! Bu en çok istenen özellik analytics'e işletmenizi, işlemleri ve güvenlik yanı sıra altyapınızı izlemeye yardımcı olmak için bir yol için daha iyi erişmesini yardımcı olur. Daha fazla bilgi için [Azure Active Directory etkinlik günlükleri artık Azure Log Analytics'te](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) blogu.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanıma sunuldu - Ekim 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Ekim 2018'de Federasyon ile 14 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Ödül Puanlarım](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), tuş takımı, [ON24 sanal ortam](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler üç](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho kayıt](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services E-posta Bildirimleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün özelliği:** Azure AD Domain Services

Azure AD Domain Services yönetilen etki alanınız ile ilgili sorunları veya yanlış yapılandırmalar hakkında Azure portalındaki uyarılar sağlar. Bu uyarılar, desteğe başvurun gerek kalmadan sorunları düzeltmek deneyebilmek adım adım kılavuzlar içerir.

Ekim ayında başlayarak, yönetilen etki alanınız için bildirim ayarlarını özelleştirmek mümkün olacaktır için yeni uyarılar ortaya çıktığında, insanların sürekli güncelleştirmeler için portalı denetleyin gereğini ortadan kaldırır, atanmış bir gruba e-posta gönderilir.

Daha fazla bilgi için [bildirim ayarları Azure AD Etki Alanı Hizmetleri'nde](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD portalı, özel etki alanlarını silmek için ForceDelete etki alanı API'sini kullanmayı destekler 

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün özelliği:** Dizin

Artık ForceDelete etki alanı API başvuruları, kullanıcılar, gruplar ve uygulamalar gibi özel bir etki alanı adınızla (contoso.com) öğesinden geri ilk varsayılan etki alanı adı (zaman uyumsuz olarak yeniden adlandırarak özel etki alanı silmek için kullanabileceğiniz duyurmaktan Mutluluk duyuyoruz contoso.onmicrosoft.com).

Bu değişiklik, özel etki alanı adı kuruluşunuz artık kullanıyorsa veya başka bir Azure AD ile etki alanı adı kullanmanız gerekiyorsa daha hızlı bir şekilde silmek için yardımcı olur.

Daha fazla bilgi için [bir özel etki alanı adını silme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>Eylül 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Dinamik gruplar için güncelleştirilmiş yönetici rolü izinleri

**Türü:** Sabit  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** İş Birliği

Özel yönetici rolleri oluşturabilir ve grubun sahibi olmanıza gerek kalmadan dinamik Üyelik kuralları güncelleştirmek için bir sorunu düzelttik.

Rolü şunlardır:

- Genel yönetici

- Intune yöneticisi

- Kullanıcı yöneticisi

Daha fazla bilgi için [dinamik bir grup oluşturun ve durumunu denetle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Bazı üçüncü taraf uygulamalar için Basitleştirilmiş Çoklu Oturum Açma (SSO) yapılandırma ayarları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** SSO

Biz yukarı çoklu oturum açma (SSO) için yazılım (SaaS) hizmet olarak ayarlama uygulamalar her apps yapılandırması benzersiz niteliği nedeniyle zor olabileceğini unutmayın. Aşağıdaki üçüncü taraf SaaS uygulamaları için SSO yapılandırma ayarlarını otomatik olarak doldurmak için Basitleştirilmiş yapılandırma deneyimi oluşturduk:

- Zendesk

- Arcgıs Online

- Jamf Pro

Bu tek tıklamayla deneyimi kullanmaya başlamak için Git **Azure portalında** > **SSO yapılandırma** sayfasını. Daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Azure Active Directory - Verileriniz nerede bulunuyor? sayfası

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün özelliği:** GoLocal

Şirketinizin bölgeyi seçin **Azure etkin verilerinizin bulunduğu dizin -** sayfasına hangi Azure veri merkezinde kalan tüm Azure AD Hizmetleri, Azure AD verileri barındırıldığı görünümü. Tarafından özel bilgileri filtreleyebilirsiniz şirketinizin bölge için Azure AD Hizmetleri.

Daha fazla bilgi için bu özelliğe erişmek için [Azure etkin verilerinizin bulunduğu dizin -](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Uygulamalarım Erişim paneli için yeni dağıtım planı kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün özelliği:** SSO

My Apps erişim panelinde kullanılabilen yeni dağıtım planını göz atın (https://aka.ms/deploymentplans).
My Apps erişim panelinde bulmak ve uygulamalarını erişmek için tek bir yerde kullanıcılara sağlar. Bu portal, uygulamaları ve gruplara erişim isteği veya diğerleri adına bu kaynaklara erişimi yönetme gibi fırsatlar Self Servis kullanıcılarla da sağlar.

Daha fazla bilgi için [uygulamalarım portalı nedir?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure portal Oturum Açma Günlükleri sayfasında yeni Sorun Giderme ve Destek sekmesi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Yeni **sorun giderme ve Destek** sekmesinde **oturum açma işlemleri** sayfasında Azure portalı, yöneticilerin yardımcı olmak için tasarlanmıştır ve destek mühendisleri, Azure AD oturum açma işlemleri için ilgili sorun giderme. Bu yeni sekme hata kodu, hata iletisi ve düzeltme önerileri (varsa) sorunun çözümüne yardımcı olmak için sağlar. Sorunu çözmek yapamıyorsanız, ayrıca, kullanarak bir destek bileti oluşturmak için yeni bir yolunu sunuyoruz **Panoya Kopyala** deneyimi, hangi doldurur **istek kimliği** ve **Tarih(UTC)** destek biletiniz günlük dosyasında alanları.  

![Yeni sekmesini gösteren oturum açma günlükleri](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Dinamik üyelik kuralları oluşturmak için kullanılan özel uzantı özellikleri için gelişmiş destek

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** İş Birliği

Bu güncelleştirmeyle, artık tıklayabilirsiniz **özel uzantı özelliklerini alma** bağlantı dinamik kullanıcı grubu kuralı Oluşturucusu'ndan, benzersiz uygulama Kimliğinizi girin ve dinamik oluşturulurken kullanılacak özel uzantı özelliklerinin tam listesini al kullanıcılar için üyelik kuralı. Bu liste, bu uygulama için tüm yeni özel uzantı özellikleri almak için aynı zamanda yenilenebilir.

Özel uzantı özellikleri için dinamik üyelik kurallarını kullanma hakkında daha fazla bilgi için bkz. [uzantı özellikleri ve özel uzantı özellikleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün özelliği:** Kimlik güvenliği ve koruması

Aşağıdaki uygulamalar listede yer [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement):

- Microsoft To-Do

- Microsoft Stream

Daha fazla bilgi için bkz.

- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Windows 7/8/8.1 Kilit ekranında Self Servis Parola Sıfırlama için yeni destek

**Türü:** Yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Bu yeni özelliği ayarlandıktan sonra kullanıcılarınız parolalarını sıfırlamak için bir bağlantı göreceksiniz **kilit** Windows 7, Windows 8 veya Windows 8.1 çalıştıran bir cihaz ekranı. Bu bağlantıya tıklayarak, kullanıcı, web tarayıcısı gibi aynı parola sıfırlama işlem akışında yönlendirilir.

Daha fazla bilgi için [parola Windows 7, 8 ve 8.1 sıfırlama olanağı tanıma](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: Bu yetkilendirme kodları için yeniden kullanılabilir 

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik Doğrulamaları (Oturum Açma İşlemleri)  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

15 Kasım 2018'de başlayarak, Azure AD uygulamaları için daha önce kullanılan kimlik doğrulama kodlarını kabul durdurur. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen uygulama invalid_grant hata alırsınız.

Bu ve diğer protokolleri güvenlikle ilgili değişiklikler için bkz. [kimlik doğrulaması için Yenilikler tam listesini](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Eylül 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Eylül 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet yazılım işe](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [Arcgıs Kurumsal](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [ JDA bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), ı, Azure için Enlyft SSO [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ek talep dönüşümü yöntemleri için destek

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** SSO

ToLower() ve SAML belirteçlerini SAML tabanlı uygulanabilir ToUpper(), yeni talep dönüştürme yöntemleri tanıttık **çoklu oturum açma yapılandırması** sayfası.

Daha fazla bilgi için [Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Güncelleştirilmiş SAML tabanlı uygulama yapılandırması kullanıcı arabirimi (önizleme)

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** SSO

Bizim güncelleştirilmiş SAML tabanlı uygulama yapılandırması kullanıcı Arabirimi bir parçası olarak, elde edecekleriniz:

- SAML tabanlı uygulamalarınızı yapılandırmak için bir güncelleştirilmiş izlenecek deneyimi.

- Eksik veya yanlış yapılandırmanızda sunulanlar hakkında daha fazla görünürlük sağlar.

- Birden çok e-posta adresleri için sona erme sertifika bildirim ekleme yeteneği.

- Yeni Talep dönüştürme yöntemleri ToLower() ve ToUpper() ve daha fazlası.

- Kendi belirteç imzalama sertifikası, Kurumsal uygulamalarınız için karşıya yüklemek için bir yol.

- SAML uygulamaları için Nameıd biçimi yolunu ve dizin uzantılarını olarak Nameıd değeri ayarlamak için bir yol.

Bu güncelleştirilmiş bir görünüm üzerinde etkinleştirmek için tıklayın **yeni deneyimimizi deneyin** üst öğesinden bağlantısı **çoklu oturum açma** sayfası. Daha fazla bilgi için [Öğreticisi: SAML tabanlı çoklu oturum açma bir uygulama için Azure Active Directory ile yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).