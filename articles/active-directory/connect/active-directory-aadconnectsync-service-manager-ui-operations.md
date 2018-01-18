---
title: "Azure AD Connect Eşitleme Hizmeti Yöneticisi'ni işlemleri | Microsoft Docs"
description: "İçin Azure AD Connect Eşitleme Hizmeti Yöneticisi'nde işlemleri sekmesinde anlayın."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91210edc3306b834cbd68f0f028845a7f36dd0b5
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Eşitleme Hizmeti Yöneticisi'ni işlemleri sekmesini kullanarak

![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

İşlem sekmesi en son işlemleri sonuçlarından gösterir. Bu sekme, anlama ve sorunlarını gidermek için anahtardır.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>İşlemler sekmesinde görünür bilgileri anlamak
Üst yarısındaki tüm metinler kronolojik sırada gösterir. Varsayılan olarak, operations son yedi gün tutar bilgilerini günlüğe, ancak bu ayar değiştirilebilir [Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md). Başarı durumunu göstermez herhangi çalıştırmak için aramak istediğiniz. Üstbilgilerini tıklatarak sıralama değiştirebilirsiniz.

**Durum** sütunu en önemli bilgiler ve bir çalışma için en önemli bir sorun gösterir. En sık kullanılan durumlarını araştırmak için öncelik sırasına göre hızlı bir özeti aşağıda verilmiştir (burada * birkaç olası hata dizeleri gösterir).

| Durum | Yorum |
| --- | --- |
| durdurulmuş-* |Çalıştır tamamlanamadı. Örneğin, uzak sistem kapalı ve bağlantı kurulamıyor. |
| durduruldu-hata-sınırı |5. 000'den fazla hataları vardır. Çalıştır otomatik olarak çok sayıda hata nedeniyle durduruldu. |
| Tamamlanan -\*-hataları |Çalıştırma tamamlandı, ancak araştırılması gereken bir hata (daha az 5.000). |
| Tamamlanan -\*-uyarıları |Çalıştırma tamamlandı, ancak bazı verileri beklenen durumda değil. Hatalar varsa, bu ileti genellikle yalnızca bir belirti içindir. Hataları ele kadar uyarıları araştırmanız gereken değil. |
| başarılı |Sorunu yok. |

Bir satır seçtiğinizde, alt çalıştıran ayrıntılarını göstermek için güncelleştirir. Bir liste bildiren olabilir solundaki alt için **adım #**. Burada her etki alanı adımı tarafından temsil edilen ormanınızdaki birden çok etki alanı varsa, bu listede yalnızca görünür. Etki alanı adı başlığı altında bulunabilir **bölüm**. Altında **eşitleme istatistikleri**, işlenen değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilmiş nesneleri listesini almak için bağlantıları tıklatabilirsiniz. Hatalarla nesneniz varsa, bu hataları altında görünmesini **eşitleme hatalarını**.

Daha fazla bilgi için bkz: [not synchronızıng bir nesne sorun giderme](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
