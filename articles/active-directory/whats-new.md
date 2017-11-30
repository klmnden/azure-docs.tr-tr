---
title: "Yenilikler neler? Azure Active Directory için sürüm notları | Microsoft Docs"
description: "Azure Active en son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı bırakılan işlevsellik ve yaklaşan değişiklikleri de dahil olmak üzere dizinle (Azure AD) yenilikleri öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f1538e1c26cfe658c7f42ccdd57d8bf5aca0b1fb
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'de yenilikler nelerdir?




> Bu abone olarak Azure Active Directory'de yenilikler ile güncel kalmasını [akış](https://docs.microsoft.com/api/search/rss?search=%22what%27s%20new%20in%20azure%20active%20directory%3F%22&locale=en-us) bir RSS Okuyucu sık kullanılan olarak.



Biz, sürekli olarak Azure Active Directory geliştirme. En son gelişmeler ile güncel kalmasını sağlamak için bu konuda, hakkında bilgi sağlar:

-   En son sürümleri 
-   Bilinen sorunlar 
-   Hata düzeltmeleri 
-   Kullanım dışı bırakılan işlevsellik 
-   Değişiklikleri planları 

Lütfen bu sayfayı yeniden ziyaret biz aylık olarak güncelleştirdiğiniz gibi düzenli olarak.

## <a name="november-2017"></a>Kasım 2017
 
### <a name="retiring-acs"></a>ACS devre dışı bırakma



**Tür:** değişiklik planı  
**Hizmet kategorisi:** ACS  
**Ürün yeteneği:** erişim denetimi hizmeti 


Microsoft Azure Active Directory erişim denetimi (erişim denetimi hizmeti veya ACS olarak da bilinir) içinde geç 2018 kullanımdan kaldırılacaktır.  Daha fazla bilgi, üst düzey Geçiş Kılavuzu & ayrıntılı zamanlama dahil olmak üzere sonraki birkaç hafta içinde sağlanacaktır. Bu arada, ACS ilgili herhangi bir sorunuz ile bu sayfada yorum bırakın ve ekibimiz üyesi yanıtlamanıza yardımcı.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune yönetilen tarayıcı tarayıcı erişimi kısıtlama 


**Tür:** değişiklik planı  
**Hizmet kategorisi:** koşullu erişim  
**Ürün yeteneği:** kimlik güvenlik ve koruma




Bu davranışı ile Office 365 ve Intune yönetilen tarayıcı onaylanmış bir uygulama olarak kullanarak diğer Azure AD bağlı bulut uygulamaları tarayıcı erişimi kısıtlama mümkün olacaktır. 

Bu değişiklik, uygulama bağlı olarak koşullu erişim için aşağıdaki koşul yapılandırmanıza olanak sağlar:

**İstemci uygulamaları:** tarayıcı

**Değişiklik etkisi nedir?**

Günümüzde, bu koşul kullanırken erişimi engellenir. Bu davranış önizlemesini kullanılabilir olduğunda, tüm erişim yönetilen tarayıcı uygulaması kullanımını gerektirir. 

Bu özellik için ve diğer yaklaşan blogları ve sürüm notlarına bakın. 

Daha fazla bilgi için bkz: [koşullu erişim Azure Active Directory'de](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

 
**Tür:** değişiklik planı  
**Hizmet kategorisi:** koşullu erişim  
**Ürün yeteneği:** kimlik güvenlik ve koruma




Aşağıdaki uygulamalar listesine eklemek için planlanan [istemci uygulamaları onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)

- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Birden çok dil için kullanım desteği koşulları



**Tür:** yeni özellik    
**Hizmet kategorisi:** kullanım koşulları  
**Ürün yeteneği:** idare/uyumluluk





Yöneticiler artık birden çok PDF belgelerini içerir (TOU) kullanım yeni koşulları oluşturabilirsiniz. Bu PDF belgeleri karşılık gelen dil olan etiketleyebilirsiniz. Kalan kullanıcılar kapsamında PDF tercihlerine göre eşleşen dil ile gösterilir. Eşleşme yoksa, varsayılan dil gösterilir.


---
 

### <a name="realtime-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemci durumu



**Tür:** yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** kullanıcı kimlik doğrulaması


 

Artık, şirket içi parola geri yazma istemci durumunu gözden geçirebilirsiniz. Bu seçenek kullanılabilir **şirket içi tümleştirme** bölümünü  **[parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)**  sayfası. 

Şirket içi geri yazma istemciye bağlantınızda bir sorun varsa, sizinle sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemciniz neden bağlanamıyor hakkında daha fazla bilgi 
- Bir bağlantı belgelerine sorunun çözümlenmesinde yardımcı olur. 


Daha fazla bilgi için bkz: [şirket içi tümleştirme](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama temelli koşullu erişim 



 
**Tür:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** kimlik güvenlik ve koruma





Şimdi Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişimi kısıtlayabilirsiniz [istemci uygulamaları onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) Intune App koruma ilkeleri kullanılarak destekleyen [Azure AD uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md). Intune uygulama koruma ilkeleri yapılandırmak ve bu istemci uygulamaları üzerindeki şirket verilerini korumak için kullanılır.

Birleştirme tarafından [uygulama tabanlı](active-directory-conditional-access-mam.md) ile [aygıt tabanlı](active-directory-conditional-access-policy-connected-applications.md) koşullu erişim ilkeleri, kişisel verileri ve şirket cihazları korumak için esnekliğe sahip.

Aşağıdaki koşullar ve denetimleri artık uygulama bağlı olarak koşullu erişim ile kullanılmak üzere kullanılabilir:

**Desteklenen bir platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylanmış istemci uygulamasını gerektirir


Daha fazla bilgi için bkz: [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md).

 
---

### <a name="managing-azure-ad-devices-in-the-azure-portal"></a>Azure portalında Azure AD cihazları yönetme



**Tür:** yeni özellik  
**Hizmet kategorisi:** cihaz kaydı ve Yönetimi  
**Ürün yeteneği:** kimlik güvenlik ve koruma

 



Tüm cihazlar için Azure AD bağlı artık bulabilirsiniz ve cihaz ilgili etkinlikleri tek bir yerde. Tüm cihaz kimliklerini ve Azure portalında ayarlarını yönetmek için yeni bir yönetim deneyimi yoktur. Bu sürümde şunları yapabilirsiniz:

- Azure AD içinde koşullu erişim için uygun olan tüm cihazlarınıza görüntüleyin

- Görünüm Özellikleri, karma Azure AD dahil olmak üzere, katılmış cihazlarda

- Azure AD alanına katılmış cihazlar için BitLocker anahtarları bulmak, Cihazınızı Intune ve daha fazla ile yönetebilirsiniz.

- Azure AD cihaz ilgili ayarlarını yönet


Daha fazla bilgi için bkz: [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md).



 
---

### <a name="support-for-macos-as-device-platform-for-azure-ad-conditional-access"></a>MacOS olarak Azure AD koşullu erişimi için cihaz platform desteği 



**Tür:** yeni özellik    
**Hizmet kategorisi:** koşullu erişim  
**Ürün yeteneği:** kimlik güvenlik ve koruma 
 

Artık içerir (dışlamak macOS Azure AD koşullu erişim ilkenizi cihaz platformu koşulu olarak veya). Desteklenen cihaz platformlarının macOS eklenmesi ile şunları yapabilirsiniz:

- **MacOS cihazları Intune kullanarak kaydetmek ve yönetmek** -iOS ve Android gibi diğer platformlarda benzeyen, şirket portal uygulaması birleşik kayıtları yapmak için macOS için kullanılabilir. MacOS için yeni şirket portalı uygulaması, bir cihazı Intune hizmetine kaydetmek ve Azure AD ile kaydetmenize olanak sağlar.
 
- **MacOS aygıtları uyması Intune'da tanımlanan kuruluşunuzun uyumluluk ilkelerine olun** -Intune'da Azure Portal'da, şimdi ayarlayabilirsiniz macOS cihazları için Uyumluluk ilkeleri. 
  
- **Yalnızca uyumlu macOS cihazlara Azure AD'de, uygulamalara erişimi kısıtlama** -koşullu erişim ilkesi yazma macOS birbirinden ayrı cihaz platformu seçeneği olarak sahiptir. Bu seçenek macOS belirli koşullu erişim ilkeleri için Azure hedeflenen uygulama kümesinde Yazar olanak sağlar.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazlar için cihaz uyumluluk ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="nps-extension-for-azure-mfa"></a>Azure MFA için NPS uzantısı 


**Tür:** yeni özellik    
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** kullanıcı kimlik doğrulaması




Azure MFA için ağ ilkesi sunucusu (NPS) uzantısı, var olan sunucuları kullanarak kimlik doğrulaması altyapınız için bulut tabanlı MFA özellikleri ekler. NPS uzantısıyla yüklemeniz, yapılandırmanız ve yeni sunucuların bakımını yapmak zorunda kalmadan, var olan kimlik doğrulama akışı telefon araması, SMS mesajı ya da telefon uygulama doğrulama ekleyebilirsiniz. 

Bu uzantı, Azure MFA sunucusu dağıtmadan VPN bağlantıları korumak istediğiniz kuruluşlar için oluşturuldu. NPS uzantısı arasındaki RADIUS Azure MFA bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı federe veya kullanıcıları gibi davranır.


Daha fazla bilgi için bkz: [varolan NPS altyapınızı Azure multi-Factor Authentication ile tümleştirme](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Geri yüklemek veya silinen kullanıcılar kalıcı olarak kaldırma


**Tür:** yeni özellik    
**Hizmet kategorisi:** kullanıcı yönetimi  
**Ürün yeteneği:** dizini 



Azure AD Yönetim merkezinde, şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleme 
- Bir kullanıcıyı kalıcı olarak sil 


**Deneyin için:**

1. Azure AD Yönetim Merkezi'nde seçin [ **tüm kullanıcılar** ](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) içinde **Yönet** bölümü. 

2. Gelen **Göster** listesinde **son kullanıcılar'ı silinmiş**. 

4. Bir veya daha fazla yakın zamanda silinmiş kullanıcı seçin ve ardından ya da bunları geri veya kalıcı olarak silin.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

 
**Tür:** değiştirilen özelliği  
**Hizmet kategorisi:** koşullu erişim  
**Ürün yeteneği:** kimlik güvenlik ve koruma


Aşağıdaki uygulamalar listesine eklenmiş olan [istemci uygulamaları onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Microsoft Planlayıcısı

- Microsoft Azure Information Protection 


Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md)


---

### <a name="ability-to-or-between-controls-in-a-conditional-access-policy"></a>Özelliği için 'veya' arasında bir koşullu erişim ilkesi denetimlerinde 


**Tür:** değiştirilen özelliği    
**Hizmet kategorisi:** koşullu erişim  
**Ürün yeteneği:** kimlik güvenlik ve koruma

 
Yeteneği 'Veya' (Seçili denetimleri birini gerektirir) koşullu erişim denetimleri yayımlandı. Bu özellik ile ilkeleri oluşturmanızı sağlayan bir **veya** erişim denetimleri arasında. Örneğin, çok faktörlü kimlik doğrulaması kullanarak oturum açmalarını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz **veya** uyumlu bir cihaz üzerinde olmalıdır.

Daha fazla bilgi için bkz: [Azure Active Directory koşullu erişim denetimleri](active-directory-conditional-access-controls.md).

 
---

### <a name="aggregation-of-realtime-risk-events"></a>Gerçek zamanlı risk olaylarını toplama


**Tür:** değiştirilen özelliği    
**Hizmet kategorisi:** kimlik koruması  
**Ürün yeteneği:** kimlik güvenlik ve koruma


Azure AD Identity Protection'ın yönetim deneyiminizi geliştirmek için belirli bir günde aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk olaylarını şimdi her risk olay türü için toplanır. Bu değişiklik, kullanıcı güvenlik herhangi bir değişiklik olmadan gösterilen risk olayı sınırlar.

Temel alınan gerçek zamanlı algılama kullanıcı her oturum açışında çalışır. Bir oturum açma riski güvenlik ilkesi Kurulum erişimi MFA veya engelleyecek şekilde varsa, her riskli oturum açma sırasında hala tetiklenir.

 
---
 




## <a name="october-2017"></a>Ekim 2017


### <a name="deprecating-azure-ad-reports"></a>Azure AD raporları onaysız kılınmadan


**Tür:** değişiklik planı  
**Hizmet kategorisi:** raporlama  
**Ürün yeteneği:** kimlik yaşam döngüsü yönetimi  



Azure portal ile sağlar:

- Yeni bir Azure Active Directory Yönetim Konsolu 
- Etkinlik ve güvenlik raporları için yeni API'leri
 
Bu yeni özellikler, raporun API'leri altında **/reports** uç nokta 10 Aralık 2017 üzerinde Çekildi. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alan algılama


**Tür:** sabit   
**Hizmet kategorisi:** uygulamalarım  
**Ürün yeteneği:** SSO  



Azure Active Directory HTML kullanıcı adı ve parola alanı işleme uygulamaları için otomatik oturum açma alan algılama destekler.  Bu adımları belgelenmiştir [otomatik olarak oturum açma alanları bir uygulama için yakalama](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Ekleyerek bu yeteneği bulabilirsiniz bir *olmayan galeri* uygulaması **kurumsal uygulamalar** sayfasındaki [Azure portal](http://aad.portal.azure.com). Ayrıca, yapılandırabileceğiniz **çoklu oturum açma** bu yeni uygulama modu **parola tabanlı çoklu oturum açma**, bir web URL'si girerek ve sayfa kaydetme.
 
Bir hizmet sorunu nedeniyle bu işlevselliği geçici olarak bir süre için devre dışı bırakıldı. Sorunu Çözümlendi ve otomatik oturum açma alan algılama yeniden kullanılabilir.

---

### <a name="new-mfa-features"></a>Yeni MFA özellikleri


**Tür:** yeni özellik  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** kimlik güvenlik ve koruma  



Çok faktörlü kimlik doğrulaması (MFA), kuruluşunuzun koruma önemli bir parçasıdır. Kimlik bilgileri daha Uyarlamalı ve deneyimi daha kolay hale getirmek için aşağıdaki özellikler eklenmiştir: 

- Çok faktörlü sınama sonuçları tümleştirmeye MFA sonuçları için programlı erişim dahil olmak üzere doğrudan Azure AD oturum açma raporu

- Azure AD yapılandırma içine MFA yapılandırmasının daha ayrıntılı tümleştirme deneyimi Azure portalında

Bu genel önizlemede, MFA yönetim ve raporlama bir tümleşik çekirdek Azure AD yapılandırma deneyimi parçasıdır. Her iki özellik bir araya getirildiği Azure AD deneyimi içinde MFA Yönetim Portalı işlevselliğini yönetmenizi sağlar.

Daha fazla bilgi için bkz: [Azure portalında raporlama çok faktörlü kimlik doğrulaması için başvurusu](active-directory-reporting-activity-sign-ins-mfa.md) 


---

### <a name="introducing-terms-of-use"></a>Kullanım koşulları Tanıtımı



**Tür:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün yeteneği:** idare  



Azure AD koşulları bilgi son kullanıcılara sunmak için basit bir yöntem sağlar. Bu, kullanıcılar ilgili bildirimler için hukuk veya uyumluluk gereksinimlerine bakın sağlar.

Azure AD kullanım koşullarını aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşulları. 

- Belirli bir kullanıcının özniteliklerine göre (örneğin kullanım koşulları Doktorlar) VS nurses veya yurtiçi vs uluslararası çalışanlar, dinamik grupların tarafından yapılır. 

- Yüksek iş etkisi uygulamalarına erişmek için belirli kullanım koşullarını Salesforce ister.

Daha fazla bilgi için bkz: [Azure Active Directory kullanım koşulları](active-directory-tou.md).


---

### <a name="enhancements-to-privileged-identity-management"></a>Ayrıcalıklı kimlik yönetimi geliştirmeleri


**Tür:** yeni özellik  
**Hizmet kategorisi:** PIM  
**Ürün yeteneği:** Privileged Identity Management  


İle Azure Active Directory ayrıcalıklı Kimlik Yönetimi (PIM), artık yönetebilir, denetleme ve izleme için kuruluşunuzdaki Azure kaynaklarına (Önizleme) erişim:

- Abonelikler
- Kaynak grupları
- Sanal makineler. 

Azure rol tabanlı erişim denetimi (RBAC) işlevselliği yararlanan tüm kaynakları Azure portalındaki sunmak için Azure AD PIM sahip tüm güvenlik ve yaşam döngüsü yönetimi özellikleri yararlanabilir.

Daha fazla bilgi için bkz: [PIM Azure kaynakları için](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="introducing-access-reviews"></a>Erişim Tanıtımı incelemeleri


**Tür:** yeni özellik  
**Hizmet kategorisi:** erişim gözden geçirme  
**Ürün yeteneği:** idare  



Erişim incelemeler (Önizleme) verimli bir şekilde grup üyeliklerini yönetmek ve kurumsal uygulamalara erişmek kuruluşlar etkinleştirin: 

- Konuk kullanıcı erişimi erişimleri uygulamalara erişim incelenmesi ve grupların üyeliklerini kullanarak yeniden onayla. Erişim incelemeler tarafından sağlanan bilgileri gözden geçirenler verimli bir şekilde erişim konuklar devam olup olmadığını karar vermek etkinleştirin.

- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz: [Azure AD erişim incelemeleri](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hiding-third-party-applications-from-my-apps-and-the-office-365-launcher"></a>Üçüncü taraf uygulamalardan My uygulamaları ve Office 365 Başlatıcısı gizleme



**Tür:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün yeteneği:** SSO  



Artık, kullanıcı portalı ile yeni bir üzerinde görünmesini uygulamaları daha iyi yönetebilirsiniz **uygulama Gizle** özelliği. Uygulama kutucuklarına durumlarda arka uç hizmet veya yinelenen döşeme için gösteren olan ve son kullanıcının uygulama launchers alanınızda karışıklık ile uygulamaları gizleme yardımcı olur. İki durumlu etiketlenir ve üçüncü taraf uygulama özellikleri bölümünde bulunan **kullanıcıya görünür?** Ayrıca, bir uygulamayı programlı olarak PowerShell aracılığıyla gizleyebilirsiniz. 

Daha fazla bilgi için bkz: [bir üçüncü taraf uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle](active-directory-coreapps-hide-third-party-app.md). 


**Kullanılabilir nedir?**

 Yeni yönetim konsoludur geçiş işleminin bir parçası olarak, Azure AD etkinlik günlükleri almak için 2 yeni API'leri kullanılabilir. Yeni API kümesi daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinliklerini sağlama ek işlevsellik sağlar. Güvenlik raporları önceden mevcut verileri artık Microsoft Graph kimlik koruması risk olayları API aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-microsoft-identity-manager"></a>Düzeltme için Microsoft Identity Manager


**Tür:** değiştirilen özelliği  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** kimlik yaşam döngüsü yönetimi  



Düzeltme paketi (yapı 4.4.1642.0) itibariyle 25 Eylül 2017 Microsoft Identity Manager (MIM) 2016 2016 Service Pack 1 (SP1) için kullanılabilir. Bu döküm paketi:

- Sorunları giderir ve geliştirmeleri içerir
- Microsoft Identity Manager 2016 için yapı 4.4.1459.0 kadar tüm MIM 2016 SP1 güncelleştirmeleri değiştirir birikmeli bir güncelleştirmedir. 
- Sahip olmanızı gerektirir **Microsoft Identity Manager 2016 4.4.1302.0 oluşturun.** 

Daha fazla bilgi için bkz: [düzeltme paketi (yapı 4.4.1642.0) için Microsoft Identity Manager 2016 SP1 kullanılabilir](https://support.microsoft.com/en-us/help/4021562). 

---
