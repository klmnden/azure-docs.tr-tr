---
title: Azure Active Directory (Azure AD) kimlik koruması güvenliğine genel bakış | Microsoft Docs
description: Nasıl 'güvenliğine genel bakış', kuruluşunuzun güvenlik duruşunu bir anlayış sağladığını öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 610668768c7baca13cb60caf1d810cced31ebec3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60452968"
---
# <a name="azure-active-directory-identity-protection---security-overview"></a>Azure Active Directory kimlik koruması - güvenliğine genel bakış

[Güvenliğine genel bakış](https://aka.ms/IdentityProtectionRefresh) , kuruluşunuzun güvenlik duruşunu bir anlayış verir. Olası saldırıları belirleyin ve ilkelerinizin etkinliğini anlamanıza yardımcı olur.

'Güvenliğine genel bakış' problem iki bölüme ayrılır:

- Eğilimler, sol tarafta, risk, kuruluşunuzdaki bir zaman çizelgesi belirtin.

- Sağda, kutucukları, kuruluşunuzdaki temel devam eden sorunlardan vurgulayın ve hızla harekete nasıl önerin.


![Güvenliğe genel bakış](./media/security-overview/01.png)
  
## <a name="trends"></a>Eğilimler

### <a name="new-risky-users-detected"></a>Yeni riskli kullanıcı algılandı

Bu grafik seçilen süre boyunca algılanan yeni riskli kullanıcı sayısını gösterir. Kullanıcı risk düzeyine (düşük, Orta, yüksek) göre bu grafik görünümünü filtreleyebilirsiniz. O gün için algılanan riskli kullanıcı sayısını görmek için UTC tarih artışlarla üzerine gelin. Bu grafik click 'Riskli kullanıcılar' raporu çıkarır. Risk altındaki kullanıcılar sorunu düzeltmek için kullanıcıların parolalarını değiştirmeden göz önünde bulundurun.

### <a name="new-risky-sign-ins-detected"></a>Yeni riskli oturum açma algılandı işlemleri

Bu grafik seçilen süre boyunca algılanan riskli oturum açma sayısını gösterir. Bu grafik oturum açma riski türü (gerçek zamanlı veya toplama) ve oturum açma riski düzeyi (Orta Düşük, yüksek) tarafından görünümünü filtreleyebilirsiniz. Korumasız oturum açma MFA beden değildi başarılı gerçek zamanlı riskli oturum açma ' dir. (Not: Çevrimdışı algılamalar nedeniyle riskli Sign-ins korunamaz oturum açma riski İlkesi tarafından gerçek zamanlı). Oturum açma risk o gün için algılanan sayısını görmek için UTC tarih artışlarla üzerine gelin. Bu grafik click 'Riskli oturum açma işlemleri' raporu çıkarır.

## <a name="tiles"></a>Kutucukları
 
### <a name="high-risk-users"></a>Yüksek riskli kullanıcılar

'Yüksek riskli kullanıcılar' kutucuk kimliğinin tehlike yüksek olasılık ile son kullanıcı sayısı gösterilir. Bunlar, araştırma için en önemli öncelik olmalıdır. 'Yüksek riskli kullanıcılar' kutucuğa tıklayarak, yalnızca kullanıcı yüksek risk düzeyine sahip gösteriliyor 'Riskli kullanıcılar' raporun filtrelenmiş bir görünüm için yönlendirir. Bu raporu kullanarak, daha fazla bilgi edinin ve bu kullanıcıları parola sıfırlama ile düzeltin.

![Güvenliğe genel bakış](./media/security-overview/02.png)


### <a name="medium-risk-users"></a>Orta düzeyde riskli kullanıcılar
'Orta düzeyde riskli kullanıcılar' döşeme Orta kimlik güvenliğinin aşılması olasılığını ile en son kullanıcıların sayısını gösterir. Kutucuğa 'Orta düzeyde riskli kullanıcılar' a tıklayın, 'Riskli kullanıcılar' raporun yalnızca orta risk düzeyine sahip kullanıcılar gösteren filtrelenmiş bir görünüm için yönlendirir. Bu raporu kullanarak, daha fazla araştırmak ve bu kullanıcıları düzeltin.

### <a name="unprotected-risky-sign-ins"></a>Korumasız riskli oturum açma işlemleri

'Korumasız riskli oturum açma işlemleri' kutucuk, son haftanın sayısını başarılı, gerçek zamanlı riskli oturum açma diğerinden engellenen işlemleri ya da bir koşullu erişim ilkesi, kimlik koruması risk İlkesi veya kullanıcı başına MFA tarafından sınandı mfa'yı gösterir. Bunlar başarılı riskli olabilecek oturum açma bilgileri ve değil MFA sınandı. Gelecekte bu oturum açma işlemleri korumak için bir oturum açma risk İlkesi uygulayın. 'Korumasız riskli oturum açma işlemleri' kutucuğa tıklayarak, bir oturum açma ile belirtilen risk düzeyi üzerinde MFA gerektirmek için oturum açma riski İlkesi yapılandırabileceğiniz oturum açma riski ilkesi yapılandırma dikey penceresine yönlendirir.


### <a name="legacy-authentication"></a>Eski bir kimlik doğrulama

'Eski kimlik doğrulaması' kutucuk, kuruluşunuzda eski kimlik doğrulamaları Geçen haftaki sayısını gösterir. Eski bir kimlik doğrulama protokolleri, bir MFA gibi modern güvenlik yöntemleri desteklemez. Eski bir kimlik doğrulama engellemek için koşullu erişim ilkesi uygulayabilirsiniz. 'Eski kimlik doğrulaması' kutucuğa tıklayarak 'kimlik güvenli puanına' yönlendirir.


### <a name="identity-secure-score"></a>Güvenli kimlik puanı

Güvenli kimlik puanı ölçer ve güvenlik duruşunuzu sektör desenleri karşılaştırır. 'Kimlik güvenli puanı (Önizleme)' kutucuğuna tıklarsanız, burada daha fazla güvenlik duruşunuzu iyileştirme hakkında bilgi edinebilirsiniz 'Kimlik güvenli puanı (Önizleme)' dikey penceresine yönlendirir.


## <a name="next-steps"></a>Sonraki adımlar

- [Kanal 9: Azure AD kimlik gösterin: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

- [Azure Active Directory kimlik Koruması'nı etkinleştirme](enable.md)

