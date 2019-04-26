---
title: Azure AD Connect eşitleme hizmeti yöneticisi işlemleri | Microsoft Docs
description: İçin Azure AD Connect Eşitleme Hizmeti Yöneticisi'nde işlemleri sekmesini anlayın.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/13/2017
ms.date: 11/12/2018
ms.component: hybrid
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474000d1d4d7e1358682d1421125d482e3782049
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60381422"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Eşitleme Hizmeti Yöneticisi'ni işlemleri sekmesini kullanarak

![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

En son işlem sonuçlardan işlemleri sekmesini gösterir. Bu sekme, anlama ve sorunlarını giderme anahtardır.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>' Nin İşlemler sekmesinde görünen bilgileri anlama
Üst kısmında kronolojik sırayla tüm çalıştırmalar gösterir. Varsayılan olarak, son yedi gün saklar bilgilerini işlemleri günlüğe, ancak bu ayar değiştirilebilir [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md). Başarı durumunu göstermez her çalıştırma için görünmesini istediğiniz. Başlıklarını tıklatarak sıralama değiştirebilirsiniz.

**Durumu** sütun en önemli bilgiler, bir çalıştırma için en önemli bir sorun gösterir. Araştırmak için öncelik sırasına göre en yaygın durumlar hızlı bir özeti aşağıda verilmiştir (burada * birden çok olası hata dizeleri belirtmek).

| Durum | Açıklama |
| --- | --- |
| durduruldu-\* |Çalıştırma işlemi tamamlanamadı. Örneğin, uzak sistem kapalı ve bağlantı kurulamıyor. |
| durduruldu-hata-sınırı |5. 000'den fazla hataları vardır. Çalıştırma, çok sayıda hata nedeniyle otomatik olarak durduruldu. |
| Tamamlanan -\*-hata |Çalıştırma tamamlandı, ancak araştırılması gereken hataları (az 5.000) vardır. |
| Tamamlanan -\*-uyarılar |Çalıştırması tamamlandı, ancak bazı veriler beklenen durumda değil. Hatalar varsa, daha sonra bu ileti genellikle yalnızca bir belirtisidir. Hataları da giderdik kadar uyarıları araştırmanız gereken değil. |
| başarılı |Sorun yok. |

Bir satırı seçin, çalıştırılan ayrıntılarını göstermek için alt güncelleştirir. En solda alt, liste söze sahip olabileceğiniz **adım #**. Burada her etki alanı adımı tarafından temsil edilen ormanınızdaki birden çok etki alanı varsa, bu liste yalnızca görünür. Etki alanı adı başlığı altında bulunabilir **bölüm**. Altında **eşitleme istatistikleri**, işlenen değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilmiş nesneleri listesini almak için bağlantıları tıklatabilirsiniz. Hatalarla nesneniz varsa, bu hataları görünür **eşitleme hatalarını**.

Daha fazla bilgi için [not synchronızıng bir nesneyle ilgili sorunları giderme](tshoot-connect-object-not-syncing.md)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

