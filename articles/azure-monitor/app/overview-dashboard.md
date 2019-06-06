---
title: Azure Application Insights Genel Bakış Panosu | Microsoft Docs
description: Azure Application Insights ve genel bakış Panosu'nu işlevsellikle uygulamalarını izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: mbullwin
ms.openlocfilehash: d1823779f8a8070149811e2349fc9f4281072d38
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497159"
---
# <a name="application-insights-overview-dashboard"></a>Application Insights Genel Bakış Panosu

Application ınsights'ı her zaman hızlı ve bir bakışta değerlendirmesi uygulamanızın sistem durumu ve performans izin vermek için bir Özet genel bakış bölmesi sağlanan. Yeni genel bakış Panosu'nu daha hızlı, daha esnek bir deneyim sağlar.

## <a name="how-do-i-test-out-the-new-experience"></a>Nasıl erişemiyorsa yeni deneyimi test?

Yeni genel bakış panosunun artık varsayılan olarak başlatılır:

![Genel Önizleme Bölmesi](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>Daha iyi performans

Zaman aralığı seçimi için basit bir tek tıklamayla arabirimi basitleştirilmiştir.

![Zaman aralığı](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

Genel performans büyük ölçüde artırıldı. Tek tıklamayla erişim gibi popüler özelliklere sahip **arama** ve **Analytics**. Dinamik olarak KPI kutucuk güncelleştirme her varsayılan karşılık gelen Application Insights özellikleri hakkında Öngörüler sağlar. Hakkında daha fazla bilgi edinmek için istekleri seçin başarısız **hataları** altında **Araştır** üst bilgi:

![hataları](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Uygulama panosu

Uygulama Panosu, uygulama durumunu ve performansını tamamen özelleştirilebilir bölmeden bir görünümünü sağlamak için Azure var olan Pano teknolojisi yararlanır.

Varsayılan Pano seçme erişmeye _uygulama Panosu_ sol üst köşedeki.

![Pano görünümü](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

Bu panoya erişme ilk kez ise, varsayılan görünüm başlatılır:

![Pano görünümü](./media/overview-dashboard/0001-dashboard.png)

Bu şekilde, varsayılan görünüm tutabilirsiniz. Veya, ayrıca, ekleyebilir ve panodan, takımınızın ihtiyaçlarını en iyi sığacak kadar sil.

> [!NOTE]
> Application Insights kaynağına erişimi olan tüm kullanıcılar, aynı uygulama Pano deneyimini paylaşır. Bir kullanıcı tarafından yapılan değişiklikleri tüm kullanıcılara ait görünümü değiştirir.

Gitmek için yalnızca bir genel bakış deneyime geri seçin:

![Genel Bakış düğmesi](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>Sorun giderme

Seçerseniz **kutucuk ayarlarını yapılandır** ve 31 günün verilerini, varsayılan veri saklama süresi 90 gün olsa da ötesinde panonuzu görüntülemez 31 gün aşan bir özel zaman aralığı ayarlayın. Şu anda bu davranışı için geçici çözüm yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [Huniler](../../azure-monitor/app/usage-funnels.md)
- [Bekletme](../../azure-monitor/app/usage-retention.md)
- [Kullanıcı Akışları](../../azure-monitor/app/usage-flows.md)