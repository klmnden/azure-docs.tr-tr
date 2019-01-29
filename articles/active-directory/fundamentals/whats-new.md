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
ms.date: 12/10/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.openlocfilehash: b5f3c406996e47792d0a4b907d542066cf6f6e0f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168050"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'deki yenilikler nelerdir?

>Ne zaman kopyalayıp yapıştırarak bu URL'yi güncelleştirmeler için bu sayfayı yeniden ziyaret etmeniz hakkında bilgi edinin: `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` içine, ![RSS simgesi](./media/whats-new/feed-icon-16x16.png) okuyucu akış.

Azure AD iyileştirmeleri düzenli olarak alır. İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

Bu sayfaya ay güncelleştirilir, böylece bunu düzenli olarak tekrar ziyaret. 6 aydan daha eski olan öğeleri arıyorsanız, bunları bulabilirsiniz [Azure Active Directory'deki yenilikler için arşiv](whats-new-archive.md).

---
## <a name="novemberdecember-2018"></a>Kasım/aralık 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Kullanıcıları eşitleme kapsamından uzun anahtarı olmadan yalnızca bulutta yer alan hesaplarına kaldırıldı

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

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Kimlik koruması geliştirmeleri için denetimli makine öğrenimi modeli ve risk puanı altyapısı

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Risk Puanları

Kimlik koruması ile ilgili kullanıcı ve oturum açma risk değerlendirmesi altyapısı iyileştirmeleri kullanıcı risk doğruluk ve kapsamını artırmak yardımcı olabilir. Yöneticiler kullanıcı risk düzeyi artık doğrudan belirli algılamalar risk düzeyine bağlıdır ve sayısı ve riskli oturum açma olaylarını düzeyi arasında bir artış olduğunu fark edebilirsiniz.

Risk algılama artık, denetimli makine öğrenimi, kullanıcının oturum açma ek özellikleri ve algılamalar desenini kullanarak kullanıcı riski hesaplar modelinde tarafından değerlendirilir. Bu kullanıcı ile ilişkili olan algılama düşük veya Orta riskini olsa bile bu modelini temel alan kullanıcı yüksek risk puanları ile yönetici bulabilirsiniz. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Yöneticiler, Microsoft Authenticator uygulamasını (genel Önizleme) kullanarak kendi parolalarını sıfırlayabilir

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

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Yeni Azure AD bulut cihaz yöneticisi rolü (genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz Kaydı ve Yönetimi  
**Ürün özelliği:** Erişim denetimi

Yöneticiler, kullanıcıların bulut cihaz Yöneticisi görevleri gerçekleştirmek için yeni bulut cihaz yöneticisi rolü atayabilirsiniz. Bulut cihaz Yöneticiler rolünün atandığı kullanıcılar etkinleştirebilir, devre dışı ve Windows 10 BitLocker Anahtarları (varsa) Azure Portalı'nda okuma yetkisi olan yanı sıra Azure AD'de cihazları silin.

Rolleri ve izinleri hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Yeni Etkinlik zaman damgası Azure AD'de (genel Önizleme) kullanarak cihazlarınızı yönetme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz Kaydı ve Yönetimi  
**Ürün özelliği:** Cihaz Yaşam Döngüsü Yönetimi

Biz, zamanla yenileyin ve gerekir, ortamınızda asılı geçici olarak eski cihazları zorunda kalmamak için Azure AD'de, kuruluşların cihazları devre dışı bırakma olduğunu unutmayın. Bu işlemde size yardımcı olacak artık Azure AD, cihaz yaşam döngüsünü yönetmenize yardımcı olacak cihazlarınızı yeni bir etkinlik damgasıyla güncelleştirir.

Alın ve bu zaman damgasından kullanma hakkında daha fazla bilgi için bkz. [nasıl yapılır: Azure AD'de eski cihazları yönetme](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Yöneticileri, kullanıcıların her cihazda kullanım koşullarını kabul etmesini zorunlu kılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare
 
Yöneticiler şimdi Aç **kullanıcıların her cihazda kabul etmesini zorunlu tut** kiracınızda kullandıkları her bir cihazdaki kullanım koşullarınızı kabul etmelerini zorunlu hale getirin.

Daha fazla bilgi için [cihaz başına kullanım özelliği, Azure Active Directory koşulları bölümünü kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Yöneticiler, yinelenen bir zamanlamaya göre süresi dolacak şekilde kullanım koşulları yapılandırabilirsiniz

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare
 

Yöneticiler şimdi Aç **sona onayları** kullanım koşullarını hale getirme seçeneği süresi dolacak tüm kullanıcılarınız, belirtilen yinelenme çizelgesine dayalıdır. Zamanlama, BI aylık, üç aylık dönem veya aylık olarak yıllık olabilir. Kullanım koşullarını süresi dolduktan sonra kullanıcıların yeniden kabul etmesini gerektirmek gerekir.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları kullanım bölümünü ekleme koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Yöneticiler her kullanıcının zamanlamaya göre süresi dolacak şekilde kullanım koşulları yapılandırabilirsiniz.

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare

Yöneticiler artık bir süre belirtin, kullanıcı kullanım koşullarını artırmasını gerekir. Örneğin, Yöneticiler, kullanıcılar her 90 günde Kullanım Koşulları'nı artırmasını gerekir belirtebilirsiniz.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları kullanım bölümünü ekleme koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Yeni Azure AD Privileged Identity Management (PIM) Azure Active Directory rolleri için e-posta gönderir.

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

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Kasım 2018

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

- Genel yönetici veya şirket yazıcısı

- Intune Hizmet Yöneticisi

- Kullanıcı Hesabı Yöneticisi

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

![Oturum açma günlükleri yeni sekmesi](media/whats-new/troubleshooting-and-support.png)

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

---

## <a name="august-2018"></a>Ağustos 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP adresi aralıklarındaki değişiklikler

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Diğer  
**Ürün özelliği:** Platform

Daha büyük IP aralıklarını, güvenlik duvarları, yönlendiriciler veya ağ güvenlik grupları, Azure AD IP adresi aralıklarını yapılandırdığınız anlamına güncelleştirmeniz gerekecektir Azure AD'ye kullanıma sunduğumuz. Azure AD yeni uç nokta eklediğinde, güvenlik duvarı, yönlendirici veya ağ güvenlik grupları IP aralığı yapılandırmaları yeniden değiştirmek zorunda kalmamanız için bu güncelleştirmeyi yapıyoruz. 

Ağ trafiği için bu yeni aralıklar sonraki iki ay içinde taşınıyor. Kesintisiz hizmet ile devam etmek için bu güncelleştirilmiş değerleri 10 Eylül 2018'den önce IP adreslerinizi eklemeniz gerekir:

- 20.190.128.0/18 

- 40.126.0.0/18 

Tüm ağ trafiğinizin taşındı kadar yeni aralıklar için eski IP adres aralıklarını kaldırılmıyor kesinlikle öneririz. Güncelleştirmeler taşıma hakkında ve ne zaman eski aralıkları kaldırabilirsiniz öğrenmek için bkz [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: Bu yetkilendirme kodları için yeniden kullanılabilir 

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik Doğrulamaları (Oturum Açma İşlemleri)  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

15 Kasım 2018'de başlayarak, Azure AD uygulamaları için daha önce kullanılan kimlik doğrulama kodlarını kabul durdurur. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen uygulama invalid_grant hata alırsınız.

Bu ve diğer protokolleri güvenlikle ilgili değişiklikler için bkz. [kimlik doğrulaması için Yenilikler tam listesini](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Self servis parola (SSPR) ve Multi-Factor Authentication (MFA) için yakınsanmış güvenlik bilgileri yönetimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması
 
Bu yeni özellik SSPR ve mfa'yı tek bir konum ve deneyimi için güvenlik bilgilerini (örneğin, telefon numarası, mobil uygulama ve benzeri) yönetme kişiler yardımcı olur; kıyasla daha önce burada iki farklı konumlarda bitti dedik.

Bu yakınsanmış deneyimi de SSPR'ı veya mfa'yı kullanan kişiler için çalışır. Ayrıca, kuruluşunuz MFA veya SSPR kayıt zorunlu değil, kişiler uygulamalarım portalında kuruluşunuz tarafından izin verilen tüm MFA veya SSPR güvenlik bilgisi yöntemleri yine de kaydedebilirsiniz.

Bu bir katılım genel önizlemesidir. Yöneticilerin yeni deneyimi (istenirse) bir kiracıdaki tüm kullanıcılar veya seçili bir grup için kapatabilirsiniz. Yakınsanmış deneyimi hakkında daha fazla bilgi için bkz. [Yakınsanan experience blog'a](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD Uygulama Ara Sunucusu uygulamalarında yeni Yalnızca HTTP tanımlama bilgileri ayarı

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün özelliği:** Erişim Denetimi

Çağrılan, bir yeni ayar yoktur **azaltmaya** uygulamalarınızdaki uygulama proxy'si. Bu ayar, her iki uygulama ara sunucusu erişim ve oturum tanımlama bilgileri HTTP yanıt üst bilgisinde HTTPOnly bayrağı dahil olmak üzere, erişim tanımlama bilgisine istemci tarafı komut dosyasından durduruluyor ve daha da kopyalama gibi eylemleri engelleyen ek güvenlik sağlamaya yardımcı olur veya tanımlama bilgisi değiştiriliyor. Bu bayrak daha önce kullanılmamış olsa da, tanımlama bilgilerinizi her zaman silinmiş şifrelenir ve aktarılan yanlış değişikliklere karşı korumaya yardımcı olmak için bir SSL bağlantısı kullanma.

Bu ayarı, Uzak Masaüstü gibi ActiveX denetimlerini kullanan uygulamalar ile uyumlu değildir. Bu durumda kullanıyorsanız, bu ayarı devre dışı bırakmak öneririz.

Azaltmaya ayarı hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Azure kaynakları için Privileged Identity Management (PIM), Yönetim Grubu kaynak türlerini destekler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Just-In-Time etkinleştirme ve atama ayarları şimdi zaten abonelik, kaynak grupları ve kaynakları (örneğin, VM'ler, uygulama hizmetleri ve diğer) için yaptığınız gibi yönetim grubu kaynak türleri için de uygulanabilir. Ayrıca, bir yönetim grubu için yönetici erişimi sağlayan bir rolü olan herkes bulabilir ve bu kaynağın PIM yönetme.

PIM ve Azure kaynakları hakkında daha fazla bilgi için bkz. [bulma ve Privileged Identity Management'ı kullanarak Azure kaynaklarını yönetme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Uygulama erişimi (önizleme) Azure AD portalına daha hızlı erişim sağlar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Bugün, PIM kullanarak rol etkinleştirme yapılırken, üzerinde etkili olması izinler için 10 dakika sürebilir. Şu anda genel Önizleme aşamasında olan uygulama erişimi kullanmayı tercih ederseniz Yöneticiler etkinleştirme isteği tamamlandıktan hemen sonra Azure AD portalına erişebilir.

Şu anda uygulama erişimi yalnızca Azure kaynakları ve Azure AD portalı deneyiminin destekler. PIM ve uygulama erişimi hakkında daha fazla bilgi için bkz: [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Ağustos 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Ağustos 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [ilişkisiz Bridgeline](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Tariflerinizden Labs - mobil ve Web testi](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta ağları bağlayıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [yaptığımız şekilde](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [(tarafından Inlogik) ProMaster](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - gider raporları](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 canlı sohbet](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [ Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Yerel Tableau desteği artık Azure AD Uygulama Ara Sunucusu içinde kullanılabilir

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün özelliği:** Erişim Denetimi

Bizim güncelleştirmesi ile Openıd Connect bizim ön kimlik doğrulama protokolü için OAuth 2.0 kodu verme protokolü, artık Tableau uygulaması Ara sunucusu ile kullanılacak tüm ek yapılandırma adımları gerekir. Bu protokol değişiklik uygulama proxy'si, JavaScript ve HTML etiketleri yaygın olarak desteklenen HTTP yönlendirmeleri kullanarak daha modern uygulamalar daha iyi destek de yardımcı olur.

Tableau için yerel destek hakkında daha fazla bilgi için bkz: [artık yerel Tableau desteği ile Azure AD uygulama proxy'si](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Azure Active Directory'de B2B konuk kullanıcıları için kimlik sağlayıcısı olarak Google eklemek için yeni destek (Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

Google ile bir Federasyon kuruluşunuzdaki ayarlayarak, davet edilen Gmail kullanıcılar oturum paylaşılan uygulamaları ve kişisel bir Microsoft Account (MSA) veya bir Azure AD hesabı oluşturmak zorunda kalmadan, var olan Google hesaplarını kullanarak kaynakları yapmanız gerekiyor.

Bu bir katılım genel önizlemesidir. Google Federasyon hakkında daha fazla bilgi için bkz: [B2B Konuk kullanıcılar için kimlik sağlayıcısı ekle Google](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Temmuz 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory e-posta bildirimleri iyileştirmeleri

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün özelliği:** Kimlik yaşam döngüsü yönetimi
 
Azure Active Directory (Azure AD) e-postaları artık gönderen e-posta adresi ve gönderen görünen adı, değişiklikleri yanı sıra, güncelleştirilmiş bir tasarım aşağıdaki hizmetlerinden gönderilen zaman özellik:
 
- Azure AD erişim gözden geçirmeleri
- Azure AD Connect Health 
- Azure AD Kimlik Koruması 
- Azure AD Privileged Identity Management
- Kurumsal uygulama süresi dolan sertifika bildirimleri
- Kurumsal uygulama sağlama hizmet bildirimleri
 
E-posta bildirimleri şu e-posta adresinden gönderilir ve görünen ad:

- E-posta adresi: azure-noreply@microsoft.com
- Görünen ad: Microsoft Azure
 
Bir örnek, bazı yeni e-posta tasarımları ve daha fazla bilgi için bkz [e-posta bildirimleri Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD Etkinlik Günlükleri artık Azure İzleyici'de kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Azure AD etkinlik günlüklerini Azure İzleyicisi'ni (Azure'nın platform genelinde izleme hizmeti) için genel önizlemede kullanıma sunulmuştur. Azure İzleyici, uzun süreli saklama ve bu geliştirmeler yanı sıra sorunsuz tümleştirme sunar:

- Günlük dosyalarınızın kendi Azure depolama hesabına yönlendirme ile uzun süreli saklama için.

- Yazma veya özel komut dosyaları korumak gerektirmeden sorunsuz SIEM tümleştirmesi.

- Kendi özel çözümler, analiz araçları ve Olay yönetimi çözümleri ile sorunsuz tümleştirme sağlar.

Blogumuzu bu yeni özellikler hakkında daha fazla bilgi için bkz. [Azure AD etkinlik günlükleri Tanılama, artık genel önizlemede Azure İzleyicisi'nde](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) ve Belgelerimizi, [Azure'da Azure Active Directory etkinlik günlükleri İzleyici (Önizleme)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Koşullu erişim bilgileri Azure AD oturum açma raporuna eklendi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** Kimlik Güvenliği ve Koruması
 
Bu güncelleştirme, hangi ilkelerin ilke sonucu ile birlikte bir kullanıcı oturum açtığı zaman değerlendirilen görmenize olanak tanır. Ayrıca, rapor artık eski protokol trafiğini belirleyebilmeniz kullanıcı tarafından kullanılan istemci uygulaması türünü içerir. Rapor girişleri kullanıcıya yönelik hata iletisinde bulunabilir ve belirleme ve giderme eşleşen oturum açma isteği için kullanılan bir bağıntı kimliği için artık aranabilir.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Oturum açma etkinlik günlükleri ile eski kimlik doğrulamalarını görüntüleyin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama
 
Sunulmasıyla birlikte **istemci uygulaması** alan oturum açma etkinlik günlükleri, eski kimlik doğrulama kullanarak müşterilerin can artık bkz: kullanıcılar. Müşteriler, oturum açma MS Graph API'sini kullanarak bu bilgilere erişmek mümkün olacaktır veya oturum açma kullanabileceğiniz Azure AD'ye portalda etkinlik günlükleri **istemci uygulaması** üzerinde eski kimlik doğrulamaları filtrelemek için denetimi. Daha fazla ayrıntı için belgeleri gözden geçirin.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Temmuz 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Temmuz 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Yenilik Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [belirli yönetici SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC hazırlama [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [yayını-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial) , PowerSchool sınıf, birleşik [Eli ekleme](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Uzaktan Destek](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial) , [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow bağlayıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [temel becerileri](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Yeni kullanıcı sağlama SaaS uygulama tümleştirmeleri - Temmuz 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Azure AD, oluşturma, Bakım ve Dropbox, Salesforce, ServiceNow ve diğer SaaS uygulamalarına kullanıcı kimliklerini kaldırılmasını otomatik hale getirmenizi sağlar. Kullanıcı Azure AD uygulama galerisinde şu uygulamalar için destek sağlama Temmuz 2018 tarihinden itibaren ekledik:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Azure AD Galerisi'nde kullanıcı sağlamayı destekleyen tüm uygulamalar listesi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Eşitleme için Connect Health - Yalnız bırakılmış ve çoğaltışmış öznitelik eşitleme hatalarını kolaylıkla düzeltme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** AD Connect  
**Ürün özelliği:** İzleme ve Raporlama
 
Azure AD Connect Health kendi kendine düzeltme vurgulayın ve eşitleme hatalarını düzeltmeyi yardımcı olması için ortaya çıkarır. Bu özellik, yinelenen öznitelik Eşitleme hataları giderir ve yalnız bırakılmış nesneler, Azure AD'den giderir. Bu tanılama aşağıdaki faydaları sağlar:

- Belirli düzeltmeler sağlama yinelenen öznitelik Eşitleme hataları daraltır

- Azure AD senaryoları, tek bir adımda hatalarını çözümlemek için bir düzeltme uygular

- Yükseltme ya da yapılandırma'yı açmak ve bu özelliği kullanmak için gereklidir

Daha fazla bilgi için [Tanıla ve yinelenen öznitelik eşitleme hatalarını Düzelt](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Azure AD ve MSA oturum açma deneyimleri için görsel güncelleştirmeler

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Microsoft online services oturum açma deneyimi için kullanıcı Arabirimi gibi Office 365 ve Azure için güncelleştirdik. Bu değişiklik ekranları, daha az anlaşılamayacak ve daha basit hale getirir. Bu değişiklik hakkında daha fazla bilgi için bkz. [yaklaşan geliştirmeler Azure AD oturum açma deneyimini](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) blogu.

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Yeni Azure AD Connect sürümü - Temmuz 2018

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün özelliği:** Kimlik Yaşam Döngüsü Yönetimi

Azure AD Connect'in en son sürümünü içerir: 

- Hata düzeltmeleri ve desteklenebilirliği güncelleştirmeleri 

- Genel kullanılabilirlik Ping federasyona tümleştirme

- En son SQL 2012 istemci güncelleştirmeleri 

Bu güncelleştirme hakkında daha fazla bilgi için bkz. [Azure AD Connect: Sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Kullanım Koşulları (ToU) son kullanıcı arabirimi güncelleştirmeleri

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare

Kabul dize TOU son kullanıcı arabiriminde güncelleştiriyoruz.

**Geçerli metin.** [Kiracıadı] kaynaklara erişmek için kullanım koşullarını kabul etmeniz gerekir.<br>**Yeni metin.** [Kiracıadı] kaynağa erişebilmeniz için kullanım koşullarını okuyun gerekir.

**Geçerli metin:** Tüm yukarıdaki kullanım koşullarını kabul etmiş olursunuz kabul etmek seçme.<br>**Yeni metin:** Lütfen okudum ve anladım kullanım koşullarını onaylamak üzere kabul Et'E tıklayın.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Doğrudan kimlik doğrulama eski protokolleri ve uygulamaları destekliyor

**Türü:** Değişen özellik  
**Hizmet kategorisi:** Kimlik Doğrulamaları (Oturum Açma İşlemleri)  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması
 
Geçişli kimlik doğrulaması artık eski protokolleri ve uygulamaları destekler. Aşağıdaki sınırlamalar artık tam olarak desteklenir:

- Modern kimlik doğrulaması gerektirmeden eski Office istemci uygulamaları, Office 2010 ve Office 2013, kullanıcı oturum açma işlemleri.

- Takvim Paylaşımı ve ücretsiz/meşgul bilgileri yalnızca karma ortamlarda Office 2010 Exchange erişim.

- Kullanıcı oturum açma işlemleri Skype için modern kimlik doğrulaması gerektirmeden iş istemci uygulamaları için.

- PowerShell sürüm 1.0 için kullanıcı oturum açma işlemleri.

- Apple cihaz kaydı iOS Kurulum Yardımcısı'nı kullanarak programı (Apple DEP). 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Self servis parola sıfırlama ve Multi-Factor Authentication için yakınsanmış güvenlik bilgileri yönetimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Bu yeni özellik için güvenlik bilgilerini (örneğin, telefon numaranız, e-posta adresi, mobil uygulama ve benzeri) Self Servis parola sıfırlama (SSPR) ve multi-Factor Authentication (MFA) tek bir deneyimle yönetme olanağı sunar. Kullanıcıların SSPR ve MFA için iki farklı deneyimlerinde aynı güvenlik bilgilerini kaydetmek artık gerekir. Bu yeni deneyim, SSPR'ı veya MFA sahip kullanıcılar için de geçerlidir.

Bir kuruluş, MFA veya SSPR kayıt zorunlu değil, kullanıcılar aracılığıyla güvenlik bilgileri kaydedebilirsiniz **uygulamalarım** portalı. Burada, kullanıcılar için mfa'yı veya SSPR etkin herhangi bir yöntem kaydedebilirsiniz. 

Bu bir katılım genel önizlemesidir. Yöneticilerin yeni deneyimi (istenirse) seçili bir grup kullanıcı ya da bir kiracıdaki tüm kullanıcılar için etkinleştirebilirsiniz.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Parolanızı sıfırlarken kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanın

**Türü:** Değişen özellik  
**Hizmet kategorisi:** SSPR  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Bu özellik, bir bildirim veya koddan Microsoft Authenticator'ı (ya da herhangi bir kimlik doğrulayıcı uygulama) kullanarak bir parolayı sıfırlarken kimliklerini doğrulama yönetici olmayanlar olanak sağlar. Yöneticileri açtıktan sonra yöntemi bu Self Servis parola sıfırlama, bir mobil uygulama aka.ms/mfasetup veya aka.ms/setupsecurityinfo aracılığıyla kaydolan kullanıcılar kendi mobil uygulama doğrulama yöntemi olarak, parola sıfırlama sırasında kullanabilirsiniz.

Mobil uygulama bildirimi, parolanızı sıfırlamak için iki yöntem gerektiren bir ilke bir parçası olarak yalnızca açılabilir.

---

## <a name="june-2018"></a>Haziran 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Değişiklik bildirimi: Azure AD etkinlik günlüklerini API'sini kullanarak uygulamalar için Temsilcili yetkilendirme akışı güvenlik düzeltme

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Raporlama  
**Ürün özelliği:** İzleme ve Raporlama

Bizim daha güçlü güvenlik zorlama nedeniyle, şu izinleri erişmek için Temsilcili yetkilendirme akışı kullanan uygulamalar için değişiklik vardı [Azure AD etkinlik günlüklerini API'lerini](https://aka.ms/aadreportsapi). Bu değişiklik tarafından ortaya çıkar **26 Haziran 2018'e**.

Tüm uygulamalarınızın Azure AD etkinlik günlüğü API'lerini kullanırsanız, değişiklik yapıldıktan sonra app kesmeyecektir emin olmak için aşağıdaki adımları izleyin.

**Uygulama izinleri güncelleştirmek için**

1. Azure portalında oturum, select **Azure Active Directory**ve ardından **uygulama kayıtları**.
2. Azure AD etkinlik günlüklerini API'si, select kullanan uygulamanızı seçin **ayarları**seçin **gerekli izinler**ve ardından **Windows Azure Active Directory** API.
3. İçinde **temsilci izinleri** alanının **erişimi etkinleştirme** dikey penceresinde yanındaki kutuyu işaretleyin **okuma directory** veri tıklayın ve ardından **Kaydet**.
4. Seçin **izinleri verin**ve ardından **Evet**.
    
    >[!Note]
    >Uygulama izinlerini vermek için genel yönetici olması gerekir.

Daha fazla bilgi için [izinleri verin](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) alan Azure AD raporlama API'si makaleye erişmek için Önkoşullar.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>PCI DSS uyumluluğu için Azure AD Hizmetleri bağlanmak için TLS ayarlarını yapılandırma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Yok  
**Ürün özelliği:** Platform

Aktarım Katmanı Güvenliği (TLS) arasında iki iletişim kuran uygulamaları gizlilik ve veri bütünlüğü sağlar ve Günümüzde kullanılan en yaygın olarak dağıtılan güvenlik protokolü bir protokoldür.

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) TLS Güvenli Yuva Katmanı (SSL) ve önceki sürümlerinde uyumluluk başlangıç ile yeni ve daha güvenli uygulama protokolleri etkinleştirme ile değiştiriliyor devre dışı bırakılmalıdır olduğunu belirledi **30 Haziran 2018**. Bu değişiklik, Azure AD hizmetlerine bağlanmak ve PCI DSS uyumluluğu gerektir, TLS 1.0 mamtelemetrydisabled anlamına gelir. TLS birden çok sürümü kullanılabilir ancak TLS 1.2 en son sürümü Azure Active Directory Hizmetleri için kullanılabilir. Hem istemci/sunucu hem de tarayıcı/sunucu birleşimleri için doğrudan TLS 1.2 taşınması önerilir.

Güncel olmayan tarayıcılar, TLS 1.2 gibi yeni TLS sürümlerini desteklemiyor olabilir. TLS hangi sürümlerinin tarayıcınız tarafından desteklendiğini görmek için Git [Qualys SSL Labs](https://www.ssllabs.com/) tıklayın ve site **tarayıcınızı Test**. Öneririz web tarayıcınızın en son sürümüne yükseltin ve tercihen yalnızca TLS 1.2 etkinleştirin.

**TLS 1.2, tarayıcı tarafından etkinleştirmek için**

- **Microsoft Edge ve Internet Explorer'ın (hem de Internet Explorer'ı kullanarak ayarlanır)**

    1. Internet Explorer'ı açın, **Araçları** > **Internet Seçenekleri** > **Gelişmiş**.
    2. İçinde **güvenlik** alanında **TLS 1.2 kullanmak**ve ardından **Tamam**.
    3. Tüm tarayıcı pencerelerini kapatın ve Internet Explorer'ı yeniden başlatın. 

- **Google Chrome**

    1. Açık Google Chrome, türü *chrome://settings/* tuşuna basın ve adres çubuğuna **Enter**.
    2. Genişletin **Gelişmiş** seçenekleri, Git **sistem** alan ve seçim **proxy ayarları**.
    3. İçinde **Internet Özellikleri** kutusunda **Gelişmiş** sekmesine gidin **güvenlik** alanında **TLS 1.2 kullanmak**ve ardından seçin **Tamam**.
    4. Tüm tarayıcı pencerelerini kapatın ve Google Chrome'u yeniden başlatın.

- **Mozilla Firefox**

    1. Açık Firefox, türü *hakkında: yapılandırma* ENTER tuşuna basın ve adres çubuğuna **Enter**.
    2. Arama terimi için *TLS*ve ardından **security.tls.version.max** girişi.
    3. Değerine **3** TLS 1.2 sürümü için kullanır ve ardından için tarayıcıyı zorlamak için **Tamam**.

        >[!NOTE]
        >Firefox sürümü 60,0 TLS 1.3 destekler böylece de security.tls.version.max değeri ayarlayabilirsiniz **4**.

    4. Tüm tarayıcı pencerelerini kapatın ve Mozilla Firefox yeniden başlatın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Haziran 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** 3. Taraf Tümleştirme
 
Haziran 2018'de Federasyon ile 15 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [müzik sonlandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1.1 belirteci etkin LOB uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ Uç nokta yedekleme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh ağları](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho bir](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [ SharePoint şirket içi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [CX Suite Öngörüyor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD parola koruması genel önizlemeye sunuldu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün özelliği:** Kullanıcı Kimlik Doğrulaması

Ortamınızdaki parolalar kolayca tahmin gidermeye yardımcı olması için Azure AD parola koruması kullanın. Bu parolalar ortadan güvenliğinin aşılması riskine karşı bir saldırı parola ilaç türü riskini azaltmak için yardımcı olur.

Özellikle, Azure AD parola koruması, yardımcı olur:

- Hem Azure AD'de kuruluşunuzun hesapları korumak ve Windows Server Active Directory (AD). 
- Kullanıcılarınızın parolaları listesi en sık kullanılan parolaların 500'den fazla ve bu parolaları 1 milyon karakter değiştirme çözümlenmeyebileceği kullanarak durdurur.
- Her iki Azure AD için Azure AD parola koruması, Azure AD portalında tek bir konumdan yönetmek ve şirket içi Windows Server AD.

Azure AD parola koruması hakkında daha fazla bilgi için bkz. [yanlış parolalar, kuruluşunuzdaki ortadan](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Kullanım koşulları (ToU) oluşturma sırasında oluşturulan yeni "tüm konuklar" koşullu erişim ilkesi şablonu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare

Kullanım koşulları (ToU) oluşturma sırasında "tüm konuklar" ve "tüm uygulamalar" için yeni koşullu erişim ilkesi şablonu da oluşturulur. Bu yeni bir ilke şablonu oluşturma ve zorlama işlemi konuklar için hızlandırma, yeni oluşturulan ToU geçerlidir.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Kullanım koşulları (ToU) oluşturma sırasında oluşturulan yeni "özel" koşullu erişim ilkesi şablonu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım Koşulları  
**Ürün özelliği:** İdare

Kullanım koşulları (ToU) oluşturma sırasında ayrıca yeni bir "özel" koşullu erişim ilkesi şablonu oluşturulur. Bu yeni bir ilke şablonu ToU oluşturmanızı ve el ile portal üzerinden gitmek zorunda kalmadan koşullu erişim ilkesi oluşturma dikey penceresine hemen Git sağlar.

Daha fazla bilgi için [kullanım özelliği, Azure Active Directory koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication'ı dağıtma hakkında daha fazla yeni ve kapsamlı Kılavuzu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün özelliği:** Kimlik Güvenliği ve Koruması
 
Yeni bir Azure multi-Factor Authentication (MFA) kuruluşunuzda dağıtma hakkında adım adım kılavuz yayımladık.

MFA Dağıtım Kılavuzu'nu görüntülemek için Git [kimlik dağıtım kılavuzları](https://aka.ms/DeploymentPlans) github deposu. Dağıtım kılavuzları hakkında geri bildirim sağlamak için kullanın [dağıtım planı geri bildirim formu](https://aka.ms/deploymentplanfeedback). Dağıtım kılavuzları hakkında sorularınız varsa, adresinden bize başvurun [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD rolleri genel Önizleme aşamasındadır uygulama yönetimi temsilcisi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün özelliği:** Erişim Denetimi

Yöneticiler, artık genel yönetici rolü atama olmadan uygulama yönetim görevlerini devredebilirsiniz. Yeni rol ve özellikleri şunlardır:

- **Yeni bir standart Azure AD yönetim rolleri:**

    - **Uygulama Yöneticisi.** Kayıt, SSO ayarlarını, uygulama atamalarını ve lisanslama, uygulama proxy'si ayarları ve onay dahil tüm uygulamalar'ın tüm yönlerini yönetme olanağı veren (Azure AD kaynaklarını hariç).

    - **Bulut uygulaması Yöneticisi.** Şirket içi erişim sağlamaz çünkü uygulama Yöneticisi becerileri, uygulama ara sunucusu hariç tüm verir.

    - **Uygulama geliştiricisi.** Uygulama kayıtları oluşturma yeteneği verir bile **uygulamaları kaydetmesine izin verin** seçeneği devre dışı bırakılmış durumdadır.

- **Sahiplik (uygulama başına kayıt ve kurumsal uygulama, Grup sahipliği işleme benzer ayarlayın:**
 
    - **Uygulama kaydı sahibi.** Sahip olunan bir uygulama kaydı, ek sahipleri ekleme ve uygulama bildirimi dahil olmak üzere tüm yönlerini yönetmek için yeteneği verir.

    - **Kurumsal uygulama sahibi.** Sahip olunan Kurumsal uygulamaları, SSO ayarlarını, uygulama atamalarını ve onay dahil olmak üzere birçok yönden yönetme olanağı veren (Azure AD kaynaklarını hariç).

Genel önizleme hakkında daha fazla bilgi için bkz: [Azure AD rolleri, genel Önizleme aşamasında olan uygulama yönetimi temsilcisi!](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) blog. Rolleri ve izinleri hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---
