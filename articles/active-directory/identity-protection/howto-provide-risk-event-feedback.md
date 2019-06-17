---
title: Azure AD kimlik koruması - Azure Active Directory risk olayları hakkında geri bildirim sağlayın
description: Neden ve nasıl kimlik koruması risk olayları hakkında geri bildirim sağlamanız gerekir.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66d53590e89afb1a903b22ff60e32871a1502ada
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827913"
---
# <a name="how-to-give-risk-feedback-in-azure-ad-identity-protection"></a>Nasıl Yapılır: Azure AD kimlik koruması risk görüş bildirin

Azure AD kimlik koruması, risk değerlendirmesini temel görüşlerinizi sağlar. Aşağıdaki belge, Azure AD kimlik Koruması'nın risk değerlendirmesine ve biz bunu nasıl birleştirme hakkında geri bildirim sağlamak istediğiniz senaryolar listelenmiştir.

## <a name="what-is-a-detection"></a>Bir algılama nedir?

Kimlik koruması algılama kimlik risk açısından kuşkulu etkinliğin göstergesidir. Bu kuşkulu etkinliklerin risk olayları çağrılır. Kimlik tabanlı bu algılamaların amacı, buluşsal yöntemler üzerinde temel alabilir, machine learning veya iş ortağı ürünleri gelebilir. Bu algılamaların amacı, oturum açma riski ve kullanıcı riski belirlemek için kullanılır,

* Kullanıcı riski kimliğin gizliliğinin olasılık temsil eder.
* Oturum açma riski bir oturum açma biri riske olasılık temsil eder (örneğin, oturum açma kimlik sahibi tarafından yetkili değil).

## <a name="why-should-i-give-risk-feedback-to-azure-ads-risk-assessments"></a>Neden miyim risk Azure AD'nin risk değerlendirmeleri için görüşlerinizi? 

Neden Azure AD risk geri bildirimde birkaç nedeni vardır:

1. **Azure AD'nin kullanıcı veya oturum açma risk değerlendirmesine yanlış bulunan**. Örneğin, 'Riskli oturum açma işlemleri' raporda bir oturum açma zararsız ve hatalı pozitif sonuçları, oturum açma üzerindeki tüm algılamalar düzeltildi.
1. **Azure AD'nin kullanıcı doğrulanmış veya oturum açma risk değerlendirmesi doğru**. Örneğin, 'Riskli oturum açma işlemleri' raporda bir oturum açma gerçekten kötü amaçlı ve Azure AD, oturum açma üzerindeki tüm algılamalar gerçek pozitif sonuç olduğunu bilmek istiyorsunuz.
1. **Bu kullanıcı Azure AD kimlik koruması dışında riskine düzeltilen** ve kullanıcının risk düzeyi güncelleştirilmesini istiyorsanız.

## <a name="how-does-azure-ad-use-my-risk-feedback"></a>Azure AD risk bildirimimi nasıl kullanır?

Azure AD, temel alınan kullanıcı ve/veya oturum açma riskini güncelleştirmek için geri bildirim kullanır. Bu geri bildirim son kullanıcı güvenliğini sağlamaya yardımcı olur. Örneğin, bir oturum açma tehlikeye doğruladıktan sonra hemen Azure AD kullanıcı riski ve oturum açma ait toplam riski (değil gerçek zamanlı risk) yüksek artırır. Bu kullanıcı parolalarını güvenli bir şekilde sıfırlamak için yüksek riskli kullanıcıları zorlamak için kullanıcı risk ilkesinde yer alıyorsa kullanıcı otomatik olarak kendisini kullanıcılar oturum açtığında geçersiz kılar.

## <a name="how-should-i-give-risk-feedback-and-what-happens-under-the-hood"></a>Nasıl miyim vermek risk geri bildirim ve bileşenler ne olur?

Senaryolar ve Azure AD'ye risk görüş mekanizmaları aşağıda verilmiştir.

| Senaryo | Geri bildirim sağlamak nasıl? | Başlık altında ne olacak? | Notlar |
| --- | --- | --- | --- |
| **Tehlikeye oturum (hatalı pozitif sonuç)** <br> 'Riskli oturum açma işlemleri' rapor gösterir bir risk altında bulunan oturum açma [Risk durumu risk =] oturum açma değil aşılmış ancak bu. | Oturum açma seçin ve 'Onayla oturum açma için güvenli ' tıklayın. | Azure AD oturum açma ait toplam riski yok taşınır [Risk durumu onaylı güvenli; = Risk düzeyini (toplama) =-] ve etkisini kullanıcı riskine geri alacaksınız. | 'Güvenli oturum açma Onayla' seçeneği şu anda yalnızca 'Riskli oturum açma işlemleri' raporda olarak kullanılabilir. |
| **Tehlikeye oturum (gerçek pozitif sonuç)** <br> 'Riskli oturum açma işlemleri' rapor gösterir bir risk altında bulunan oturum açma [Risk durumu risk =] düşük riskli [Risk düzeyini (toplama) düşük =] ve bu oturum açma gerçekten aşılmış. | Oturum açma seçin ve 'Onayla tehlikeye oturum üzerinde' tıklayın. | Azure AD oturum açma ın birleşik risk ve kullanıcı riski yüksek olarak taşınır [Risk durumu tehlikeye; onaylı = Risk düzeyi yüksek =]. | 'Onayla tehlikeye oturum' seçeneği şu anda yalnızca 'Riskli oturum açma işlemleri' raporda olarak kullanılabilir. |
| **Kullanıcı gizliliği (gerçek pozitif sonuç)** <br> 'Riskli kullanıcılar' rapor gösterir risk altında bulunan bir kullanıcı [Risk durumu risk =] düşük riskli [Risk düzeyini Düşük =] ve kullanıcının gerçekten aşılmış. | Kullanıcıyı seçin ve 'Onayla kullanıcı gizliliği' tıklayın. | Azure AD kullanıcı riski yüksek olarak hareket ettirecektir [Risk durumu tehlikeye; onaylı = Risk düzeyi yüksek =] ve 'Yönetici onayladı kullanıcı gizliliği' yeni bir algılama ekler. | 'Onayla kullanıcı gizliliği' seçeneği şu anda yalnızca 'Riskli kullanıcılar' raporda olarak kullanılabilir. <br> Algılama 'Yönetici onayladı kullanıcı gizliliği' 'bir oturum açma için bağlı olmayan Risk olayları' sekmesinde 'Riskli kullanıcılar' raporunda gösterilir. |
| **Kullanıcı Azure AD kimlik koruması dışında (gerçek pozitif sonuç + Remediated) düzeltilen** <br> Risk altında bulunan bir kullanıcı 'Riskli kullanıcılar' rapor gösterir ve daha sonra kullanıcının Azure AD kimlik koruması dışında düzeltilen. | 1. Kullanıcı seçin ve 'Onayla kullanıcı gizliliği' yı tıklatın. (Bu işlem Azure AD'ye kullanıcı gerçekten aşılmış onaylar.) <br> 2. Kullanıcının 'Risk düzeyi' bekleyin yüksek'e gidin. (Bu süre tanıdığından risk altyapısına yukarıdaki geri bildirim almak için Azure AD gerekli zaman.) <br> 3. Kullanıcıyı seçin ve 'kullanıcı riski Kapat' tıklayın. (Bu işlem Azure AD'ye kullanıcı artık tehlikeye girmemesini onaylar.) |  Azure AD taşır kullanıcı riski yok [Risk durumu çıkarıldı; = Risk düzeyi =-] ve tüm mevcut oturum açma etkin riski sahip işlemleri riskine kapatır. | 'Kullanıcı riski Kapat' ı tıklatarak, kullanıcı ve oturum açma geçen tüm risk sona erecektir. Bu eylem geri alınamaz. |
| **Kullanıcı gizliliğinin tehlikeye (hatalı pozitif sonuç)** <br> Dalgalanmasını kullanıcı, 'Riskli kullanıcılar' rapor gösterir ancak kullanıcı gizliliğinin tehlikeye. | Kullanıcıyı seçin ve 'kullanıcı riski Kapat' tıklayın. (Bu işlem Azure AD'ye kullanıcı gizliliğinin tehlikeye girmemesini onaylar.) | Azure AD taşır kullanıcı riski yok [Risk durumu çıkarıldı; = Risk düzeyi =-]. | 'Kullanıcı riski Kapat' ı tıklatarak, kullanıcı ve oturum açma geçen tüm risk sona erecektir. Bu eylem geri alınamaz. |
| Kullanıcı riski kapatmak istediğiniz, ancak güvenliği aşılmış / güvenli kullanıcı olup emin değilim. | Kullanıcıyı seçin ve 'kullanıcı riski Kapat' tıklayın. (Bu işlem Azure AD'ye kullanıcı artık tehlikeye girmemesini onaylar.) | Azure AD taşır kullanıcı riski yok [Risk durumu çıkarıldı; = Risk düzeyi =-]. | 'Kullanıcı riski Kapat' ı tıklatarak, kullanıcı ve oturum açma geçen tüm risk sona erecektir. Bu eylem geri alınamaz. Kullanıcı, 'Üzerinde parolayı Sıfırla' tıklayarak düzeltmek veya kimlik bilgilerini güvenli bir şekilde sıfırlama/değiştirme kullanıcıya istek öneririz. |

Kimlik koruması, kullanıcı risk olayları hakkında geri bildirim çevrimdışı işlenir ve güncelleştirilmesi biraz zaman alabilir. Durum sütununun işlenmesi risk geri bildirim işleme geçerli durumunu sağlar.

![Risk işleme durumu için riskli kullanıcı raporu](./media/howto-provide-risk-event-feedback/risky-users-provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory kimlik koruması, risk olayları başvurusu](risk-events-reference.md)