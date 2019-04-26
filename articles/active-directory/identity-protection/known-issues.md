---
title: Sık sorulan sorular ve (Azure Active Directory'de yenilenmiş) kimlik koruması ile ilgili bilinen sorunlar | Microsoft Docs
description: Sık sorulan sorular ve (Azure Active Directory'de yenilenmiş) kimlik koruması ile ilgili bilinen sorunlar.
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
ms.date: 01/24/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 392b7a08d9422658c5620f60e9c1caca074bc85e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60452696"
---
# <a name="faqs-and-known-issues-with-identity-protection-refreshed-in-azure-active-directory"></a>Sık sorulan sorular ve (Azure Active Directory'de yenilenmiş) kimlik koruması ile ilgili bilinen sorunlar


## <a name="dismiss-user-risk-known-issues"></a>Kullanıcı riski bilinen sorunlar Kapat

**Kullanıcı riski yok sayın** kimlik koruması ayarlar aktör kullanıcının risk geçmişinde kimlik koruması için (yenilenmiş) klasikteki **Azure AD'ye**.


**Kullanıcı riski yok sayın** (yenilenmiş) kimlik koruması aktör kullanıcının risk geçmişinde kimlik koruması (yenilenmiş) için ayarlar **\<yöneticinin adı için kullanıcının dikey penceresini gösteren köprü ile\>**.

Kullanıcı risk işten çıkarma akışında gecikme sürelerine neden geçerli bilinen bir sorun yoktur. "Kullanıcı riski İlkesi" varsa, bu ilke "Kullanıcı riskine Kapat" ı dakikalar içinde kapatılmış kullanıcılara uygulama durdurur. Ancak, gecikmeleri "Risk" kapatılmış kullanıcıların durumunu yenileme UX ile bilinen vardır. Geçici çözüm olarak, tarayıcı düzeyinde en son kullanıcı "Risk durumu" görmek için sayfayı yenileyin.


## <a name="risky-users-report-known-issues"></a>Bilinen sorunlar riskli kullanıcılar raporu

Üzerinde sorgular **kullanıcıadı** alan sorgular üzerinde çalışırken duyarlı **adı** alan durumu belirsiz.

Geçiş **Göster olarak tarihleri** gizler **son RISK güncelleştirme** sütun. Sütun tıklayarak rolleriniz için **sütunları** riskli kullanıcılar dikey penceresinin üstünde.

**Tüm olayları kapatılamadı** kimlik koruması, risk olayları için durumu ayarlar klasikteki **kapatıldı (Çözüldü)**.


## <a name="risky-sign-ins-report-known-issues"></a>Riskli oturum açma işlemleri raporu bilinen sorunlar

**Çözmek** üzerinde bir risk olayını durumu ayarlar **kullanıcılar, risk tabanlı ilke tarafından yönetilen MFA geçirilen**.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-event"></a>Her risk olayı için kendi risk düzeyleri neden olarak ayarlanamıyor?

Kimlik koruması, risk düzeyleri duyarlılık algılama üzerinde bağlı ve bizim denetimli makine öğrenimi tarafından desteklenen. Deneyimi kullanıcıların sunulan özelleştirmek için yönetici Ekle/kullanıcı riski ve oturum açma riski ilkeleri belirli kullanıcıları/grupları Dışla.


### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Neden bir oturum açma konumu burada kullanıcının gerçek anlamda gelen oturum açtığı eşleşmiyor?

IP coğrafi konum eşleme sektör genelinde bir zorluktur. Oturum açma işlemleri raporu listelenen konumu asıl konum eşleşmiyor düşünüyorsanız, lütfen desteklemek için ulaşın. 


### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>Kimlik Koruması'nda geri bildirim mekanizmaları nasıl çalışır?

**Onayla tehlikeye** (bir oturum açma) – oturum açma değildi bildirir, Azure AD kimlik koruması kimlik sahibi tarafından gerçekleştirilen ve güvenliğinin gösterir.

- Bu geri bildirim aldıktan sonra biz oturum açma ve kullanıcı risk duruma taşımasını **tehlikeye onaylı** ve risk düzeyine **yüksek**.

- Ayrıca, biz makinemizi sistemler için risk değerlendirmesi gelecekteki yenilikleri öğrenmek için bilgileri sağlayın.

    > [!NOTE]
    > Kullanıcı zaten düzeltildiyse tıklamayın **gizliliği onayla** oturum açma ve kullanıcı risk durumu taşıdığı **tehlikeye onaylı** ve risk düzeyine **yüksek**.

**Güvenli onaylayın** (bir oturum açma) – Azure AD kimlik koruması oturum açma kimlik sahibi tarafından gerçekleştirildi ve güvenliğinin göstermez olduğunu bildirir.

- Bu geri bildirim aldıktan sonra biz oturum açma (kullanıcı değil) taşıma durumuna risk **onaylı güvenli** ve risk düzeyine **-**.

- Ayrıca, biz makinemizi sistemler için risk değerlendirmesi gelecekteki yenilikleri öğrenmek için bilgileri sağlayın.

    > [!NOTE]
    > Kullanıcı gizliliğinin tehlikeye düşünüyorsanız kullanmak **kullanıcı riski yok sayın** kullanmak yerine kullanıcı düzeyinde **onaylı güvenli** oturum düzeyi. A **kullanıcı riski yok sayın** kullanıcı riski ve riskli oturum açma işlemleri ve risk olaylarını geçmişteki tüm kullanıcı düzeyi kapatır.



### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-events-are-shown-in-identity-protection"></a>Riskli oturum açma işlemleri ya da risk olayları kimlik koruması gösterilen bile düşük ile (veya üzeri) kullanıcı risk puanı neden görüyorum?

Belirtilen kullanıcının risk doğası gereği toplanır ve dolmaz, bir kullanıcının, düşük kullanıcı riski olabilir veya olsa bile son riskli oturum açma işlemleri veya yoktur gösterilen kimlik koruması risk olayları. Bir kullanıcı yalnızca kötü amaçlı etkinlik ayrıntılarını riskli oturum açma işlemleri ve risk olaylarını depolarız zaman çerçevesini dışında gerçekleşen varsa bu durum gerçekleşebilir. Kötü aktörleri müşterilerin ortamında 140, saldırı ramping önce günden daha uzun bir tehlikeye atılmış kimlik arkasında kalın bilinmektedir çünkü biz kullanıcı riski dolmaz. Müşteriler, neden giderek bir kullanıcının risk altında olduğunu anlamak için kullanıcının risk zaman çizelgesini gözden geçirebilirsiniz: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>İlişkili algılamalar düşük veya Orta riskini olduğunda neden bir oturum açma yüksek bir "oturum açma riski (toplama)" puan var mı?

Oturum açma veya birden çok algılama, oturum açma için harekete olgu diğer özellikleri yüksek birleşik risk puanı temel alabilir. Ve yüksek riskli oturum açma ile ilişkili algılamalar olsa bile, buna karşılık, bir oturum açma bir oturum açma riskini (toplama) Orta olabilir. 
