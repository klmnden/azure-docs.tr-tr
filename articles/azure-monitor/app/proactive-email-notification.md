---
title: Azure Application Insights, akıllı algılama – varsayılan bildirim alıcılarını yaklaşan değişiklik | Microsoft Docs
description: Azure Application Insights ile uygulama izlemeleri için izleme telemetrisi olağandışı desenleri izleyin.
services: application-insights
author: harelbr
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/13/2019
ms.author: harelbr
ms.openlocfilehash: f984a34be1c5d5fdd18a00812107318df8f5d9bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61299010"
---
# <a name="smart-detection-e-mail-notification-change"></a>Akıllı algılama e-posta bildirimi değiştirme

1 Nisan 2019, Müşteri geribildirimine dayalı akıllı algılama e-posta bildirimleri alması varsayılan rolleri değiştirirsiniz.

## <a name="what-is-changing"></a>Değişen nedir?

Akıllı algılama e-posta bildirimleri varsayılan olarak şu anda, gönderilen _abonelik sahibi_, _abonelik katkıda bulunanı_, ve _abonelik okuyucusu_ rolleri. Bu roller genellikle izlemeyle aktif olarak ilgilenmeyen kullanıcıları içerdiğinden bu durum, bu kullanıcıların gereksiz bildirimler almasına neden olur. Böylece e-posta bildirimleri yalnızca Git bu deneyimi geliştirmek için bir değişiklik yapıyoruz [izleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [izleme katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) varsayılan roller.

## <a name="scope-of-this-change"></a>Bu değişiklik kapsamı

Bu değişiklik aşağıdakiler dışında tüm Akıllı Algılama kurallarını etkileyecek:

* Önizleme olarak işaretlenen Akıllı Algılama kuralları. Bu akıllı algılama kuralları e-posta bildirimleri bugün desteklemez.

* Hata Anomalileri kuralı. Bu kural, birleştirilmiş uyarılar platformu için Klasik bir uyarıdan geçirildikten sonra yeni varsayılan rolleri hedefleyen başlar (daha fazla bilgi edinilebilir [burada](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement).)

## <a name="how-to-prepare-for-this-change"></a>Bu değişikliğe hazırlanmak nasıl?

Akıllı algılama e-posta bildirimleri ilgili kullanıcılara gönderildiğinden emin olmak için söz konusu kullanıcıların atanmalıdır [izleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) veya [izleme katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) aboneliğin rolleri.

Azure portal aracılığıyla izleme okuyucusu veya izleme katkıda bulunan rollerine kullanıcıları atamak için açıklanan adımları izleyin. [bir rol ataması Ekle](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) makalesi. Seçtiğinizden emin olun _izleme okuyucusu_ veya _izleme katılımcı_ kullanıcılar atanan rol olarak.

> [!NOTE]
> Akıllı algılama uyarıları, kullanılarak yapılandırılan belirli alıcıları _ek e-posta alıcılarını_ kuralı ayarlar seçeneği, bu değişiklikten etkilenmez. Bu alıcı, e-posta bildirimleri almaya devam eder.

Sorularınız veya bu değişiklik hakkında endişeleriniz varsa, çekinmeyin [bizimle](mailto:smart-alert-feedback@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar

Akıllı algılama hakkında bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anormallikleri](../../azure-monitor/app/proactive-performance-diagnostics.md)