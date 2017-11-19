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
ms.date: 11/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: a44faec6c21c338dfc6b1507af6f716e089c7038
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
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

**Tür:** işlevler de kullanım dışı  
**Hizmet kategorisi:** ACS  
**Ürün yeteneği:** erişim denetimi hizmeti 

<a name="acs-retirement"></a>

Microsoft Azure Active Directory erişim denetimi (erişim denetimi hizmeti veya ACS olarak da bilinir) Kasım 2018 devre dışı bırakıldı.  Yüksek düzey Geçiş Kılavuzu & ayrıntılı zamanlama dahil olmak üzere daha fazla bilgi bulunabilir [bu sayfadaki](./develop/active-directory-acs-migration.md).

---


## <a name="october-2017"></a>Ekim 2017

**Tür:** değişiklik planı  
**Hizmet kategorisi:** raporlama  
**Ürün yeteneği:** kimlik yaşam döngüsü yönetimi  


**Azure AD raporları (beta sürümü) onaysız kılınmadan API'leri altında `https://graph.windows.net/<tenant-name>/reports/` düğümü**

Azure portal ile sağlar:

- Yeni bir Azure Active Directory Yönetim Konsolu 
- Etkinlik ve güvenlik raporları için yeni API'leri
 
Bu yeni özellikler, raporun API'leri altında **/reports** uç nokta 10 Aralık 2017 üzerinde Çekildi. 

---

**Tür:** sabit   
**Hizmet kategorisi:** uygulamalarım  
**Ürün yeteneği:** SSO  


Azure Active Directory HTML kullanıcı adı ve parola alanı işleme uygulamaları için otomatik oturum açma alan algılama destekler.  Bu adımları belgelenmiştir [otomatik olarak oturum açma alanları bir uygulama için yakalama](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Ekleyerek bu yeteneği bulabilirsiniz bir *olmayan galeri* uygulaması **kurumsal uygulamalar** sayfasındaki [Azure portal](http://aad.portal.azure.com). Ayrıca, yapılandırabileceğiniz **çoklu oturum açma** bu yeni uygulama modu **parola tabanlı çoklu oturum açma**, bir web URL'si girerek ve sayfa kaydetme.
 
Bir hizmet sorunu nedeniyle bu işlevselliği geçici olarak bir süre için devre dışı bırakıldı. Sorunu Çözümlendi ve otomatik oturum açma alan algılama yeniden kullanılabilir.

---

**Tür:** yeni özellik  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** kimlik güvenlik ve koruma  


Biz Live'da dünyasında, çok faktörlü kimlik doğrulaması (MFA) kuruluşunuz korumanın önemli bir bölümünü oluşturur. Microsoft Identity ekibi daha Uyarlamalı kimlik bilgilerini ve deneyimi daha kolay hale getirmek için çok faktörlü kimlik doğrulaması gelişen. Bugün bu gezisine iki önemli adımda duyurmaktan mutluluk ben: 

- Çok faktörlü sınama sonuçları tümleştirmeye MFA sonuçları için programlı erişim dahil olmak üzere doğrudan Azure AD oturum açma raporu

- Daha ayrıntılı tümleştirme çekirdek Azure AD yapılandırma içine MFA yapılandırmasının Azure portalında deneyimi

Bu genel Önizleme ile MFA yönetim ve raporlama çekirdek Azure AD yapılandırma deneyimi, Azure AD deneyimi içinde MFA Yönetim Portalı işlevselliğini yönetmenize olanak sağlayan tümleşik bir parçası olan.

Daha fazla bilgi için bkz: [Azure portalında raporlama çok faktörlü kimlik doğrulaması için başvurusu](active-directory-reporting-activity-sign-ins-mfa.md) 


---
**Tür:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün yeteneği:** idare  


**Azure AD koşulları** bilgi son kullanıcılara sunmak için basit bir yöntem sağlar. Bu, kullanıcılar ilgili bildirimler için hukuk veya uyumluluk gereksinimlerine bakın sağlar.

Azure AD kullanım koşullarını aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşulları. 

- Belirli bir kullanıcının özniteliklerine göre (örneğin kullanım koşulları Doktorlar) VS nurses veya yurtiçi vs uluslararası çalışanlar, dinamik grupların tarafından yapılır. 

- Yüksek iş etkisi uygulamalarına erişmek için belirli kullanım koşullarını Salesforce ister.

Daha fazla bilgi için bkz: [Azure Active Directory kullanım koşulları](active-directory-tou.md).


---
**Tür:** yeni özellik  
**Hizmet kategorisi:** PIM  
**Ürün yeteneği:** Privileged Identity Management  


İle Azure Active Directory ayrıcalıklı Kimlik Yönetimi (PIM), artık yönetebilir, denetleme ve izleme kuruluşunuz içinde (Önizleme) Azure kaynaklarına erişim. Bu abonelik, kaynak grupları ve hatta sanal makineleri içerir. Azure rol tabanlı erişim denetimi (RBAC) işlevselliği yararlanan tüm kaynakları Azure portalındaki sunmak için Azure AD PIM sahip tüm harika güvenlik ve yaşam döngüsü yönetimi özellikleri yararlanabilir ve getirmek planlıyoruz bazı harika yeni özellikler Yakında Azure AD rolleri için.

Daha fazla bilgi için bkz: [PIM Azure kaynakları için](privileged-identity-management/azure-pim-resource-rbac.md).


---
**Tür:** yeni özellik  
**Hizmet kategorisi:** erişim gözden geçirme  
**Ürün yeteneği:** idare  


Erişim incelemeler (Önizleme) verimli bir şekilde grup üyeliklerini yönetmek ve kurumsal uygulamalara erişmek kuruluşlar etkinleştirin: 

- Konuk kullanıcı erişimi erişimleri uygulamalara erişim incelenmesi ve grupların üyeliklerini kullanarak yeniden onayla. Erişim incelemeler tarafından sağlanan bilgileri gözden geçirenler verimli bir şekilde erişim konuklar devam olup olmadığını karar vermek etkinleştirin.

- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz: [Azure AD erişim incelemeleri](active-directory-azure-ad-controls-access-reviews-overview.md).


---
**Tür:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün yeteneği:** SSO  


**Üçüncü taraf uygulamalardan My uygulamaları ve Office 365 Başlatıcısı Gizle yeteneği**

Artık, kullanıcı portalı ile yeni bir üzerinde görünmesini uygulamaları daha iyi yönetebilirsiniz **uygulama Gizle** özelliği. Bu durumlarda uygulama kutucuklarına arka uç hizmet veya yinelenen döşeme için gösteren olan ve son kullanıcının uygulama launchers alanınızda karışıklık ile yardımcı olur. İki durumlu etiketlenir ve üçüncü taraf uygulama özellikleri bölümünde bulunan **kullanıcıya görünür?**. Ayrıca, bir uygulamayı programlı olarak PowerShell aracılığıyla gizleyebilirsiniz. 

Daha fazla bilgi için bkz: [bir üçüncü taraf uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle](active-directory-coreapps-hide-third-party-app.md). 


**Kullanılabilir nedir?**

 Yeni yönetim konsoludur geçiş işleminin bir parçası olarak, Azure AD etkinlik günlükleri almak için şu 2 yeni API'leri yaptınız. Yeni API kümesi daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinliklerini sağlama ek işlevsellik sağlar. Güvenlik raporları önceden mevcut verileri artık Microsoft Graph kimlik koruması risk olayları API aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

**Tür:** değiştirilen özelliği  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** kimlik yaşam döngüsü yönetimi  


Düzeltme paketi (yapı 4.4.1642.0) itibariyle 25 Eylül 2017 Microsoft Identity Manager (MIM) 2016 2016 Service Pack 1 (SP1) için kullanılabilir. Bu döküm paketi:

- Sorunları giderir ve geliştirmeleri içerir
- Microsoft Identity Manager 2016 için yapı 4.4.1459.0 kadar tüm MIM 2016 SP1 güncelleştirmeleri değiştirir birikmeli bir güncelleştirmedir. 
- Sahip olmanızı gerektirir **Microsoft Identity Manager 2016 4.4.1302.0 oluşturun.** 

Daha fazla bilgi için bkz: [düzeltme paketi (yapı 4.4.1642.0) için Microsoft Identity Manager 2016 SP1 kullanılabilir](https://support.microsoft.com/en-us/help/4021562). 

---
