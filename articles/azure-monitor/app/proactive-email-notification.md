---
title: Azure Application Insights, akıllı algılama – varsayılan bildirim alıcılarını yaklaşan değişiklik | Microsoft Docs
description: Azure Application Insights ile uygulama izlemeleri için izleme telemetrisi olağandışı desenleri izleyin.
services: application-insights
author: harelbr
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 02/12/2019
ms.author: harelbr
ms.openlocfilehash: 4bcbed82a78caff62a9459ecb44c6513f367f6b7
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56457996"
---
# <a name="smart-detection-e-mail-notification-change"></a>Akıllı algılama e-posta bildirimi değiştirme

1 Nisan 2019, Müşteri geribildirimine dayalı akıllı algılama e-posta bildirimleri alması varsayılan rolleri değiştirirsiniz.

## <a name="what-is-changing"></a>Değişen nedir?

Akıllı algılama e-posta bildirimleri varsayılan olarak şu anda, gönderilen _abonelik sahibi_, _abonelik katkıda bulunanı_, ve _abonelik okuyucusu_ rolleri. Bu roller genellikle birçok gereksiz yere bildirimleri almak için bu kullanıcıların neden olan izleme, aktif olmayan kullanıcılar içerir. Böylece e-posta bildirimleri yalnızca Git bu deneyimi geliştirmek için bir değişiklik yapıyoruz [izleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [izleme katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) varsayılan roller.

## <a name="scope-of-this-change"></a>Bu değişiklik kapsamı

Bu değişiklik aşağıdaki sorguyu hariç tüm akıllı algılama kuralları etkiler:

* Akıllı algılama kuralları önizleme olarak işaretlenmiş. Bu akıllı algılama kuralları e-posta bildirimleri bugün desteklemez.

* Hata Anomalileri kuralı. Bu kural, birleştirilmiş uyarılar platformu için Klasik bir uyarıdan geçirildikten sonra yeni varsayılan rolleri hedefleyen başlar (daha fazla bilgi edinilebilir [burada](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement).)

## <a name="how-to-prepare-for-this-change"></a>Bu değişikliğe hazırlanmak nasıl?

Akıllı algılama e-posta bildirimleri için ilgili kullanıcıları gönderildiğinden emin olmak için söz konusu kullanıcıların atanmalıdır [izleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [izleme katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rolleri. Atama (Abonelikteki tüm Application Insights kaynaklarını etkileyen) abonelik düzeyinde veya (yalnızca bu belirli kaynak etkileyen) Application Insights kaynak düzeyinde yapılması gerekir.

Azure portal aracılığıyla izleme okuyucusu veya izleme katkıda bulunan rollerine kullanıcıları atamak için açıklanan adımları izleyin. [bir rol ataması Ekle](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) makalesi. Seçtiğinizden emin olun _izleme okuyucusu_ veya _izleme katılımcı_ kullanıcılar atanan rol olarak.

> [!NOTE]
> Akıllı algılama uyarıları, kullanılarak yapılandırılan belirli alıcıları _ek e-posta alıcılarını_ kuralı ayarlar seçeneği, bu değişiklikten etkilenmez. Bu alıcı, e-posta bildirimleri almaya devam eder.

Sorularınız veya bu değişiklik hakkında endişeleriniz varsa, çekinmeyin [bizimle](mailto:smart-alert-feedback@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar

Akıllı algılama hakkında bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anormallikleri](../../azure-monitor/app/proactive-performance-diagnostics.md)