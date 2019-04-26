---
title: Azure Application Insights Huniler
description: Müşteriler uygulamanızla nasıl etkileşim bulunacak Huniler nasıl kullanabileceğinizi öğrenin.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/17/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: 2cb7e15b701b53e74618c21bf219a355d495f985
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60372930"
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Müşterilerin uygulamanızı Application Insights Huniler ile nasıl kullandığını keşfedin

Müşteri Deneyimini anlama işletmeniz için en iyi dayanıklılığı oldukça önemlidir. Uygulamanızı birden çok aşama içeriyorsa, müşterilerin çoğu tüm sürecinde ilerlediğini veya belirli bir noktada işlemi sona erdirme bilmeniz gerekir. Bir dizi adım bir web uygulaması aracılığıyla ilerlemeyi olarak bilinen bir *huni*. Kullanıcılarınızın Öngörüler edinmek için Azure Application Insights Huniler kullanın ve adım adım dönüştürme oranlarını izleyin. 

## <a name="create-your-funnel"></a>Huni oluşturma
Huni oluşturmadan önce soruya yanıt aradığı karar verin. Örneğin, kaç kullanıcının giriş sayfası görüntülediğiniz bilmek görüntüleme müşteri profili ve bir anahtar oluşturmak isteyebilirsiniz. Bu örnekte, Fabrikam Fiber şirket sahipleri başarıyla müşteri bilet oluşturma müşteriler yüzdesi bilmek istersiniz.

Bunlar, Huni oluşturmak için adımlar aşağıda verilmiştir.

1. Application Insights Huniler aracında seçin **yeni**.
1. Gelen **zaman aralığı** açılan menüsünde, select **Son 90 gün**. Şunlardan birini seçin **hunilerim** veya **paylaşılan huniler**.
1. Gelen **1. adım** aşağı açılan listesinden **dizin**. 
1. Gelen **2. adım** listesinden **müşteri**.
1. Gelen **3. adım** listesinden **Oluştur**.
1. Bir ad için Huni ekleyin ve seçin **Kaydet**.

Aşağıdaki ekran görüntüsünde, Huniler aracı veri türü örneği oluşturur gösterir. Fabrikam sahipleri, Son 90 gün içinde bir müşteri bileti oluşturulan giriş sayfasını ziyaret eden müşterileri 54.3 yüzdesi görebilirsiniz. Bunlar ayrıca, müşterilerin 2,700 dizine giriş sayfasından gelen da görebilirsiniz. Bu yenileme sorun olduğunu gösteriyor olabilir.


![Veri, ekran Huniler aracıyla](media/usage-funnels/funnel1.png)

### <a name="funnels-features"></a>Huniler özellikleri
Önceki ekran beş vurgulanan alanları içerir. Bunlar Huniler özellikleridir. Aşağıdaki listede karşılık gelen her ekran alanı hakkında daha fazla açıklanmaktadır:
1. Uygulamanızı örneklendiği, örnekleme başlığı görürsünüz. Başlık seçme örnekleme devre dışı bırakmak nasıl açıklayan bir bağlam bölmesi açılır. 
2. İçin Huni dışarı aktarabilirsiniz [Power BI](../../azure-monitor/app/export-power-bi.md ).
3. Sağ tarafta diğer ayrıntıları görmek için bir adım seçin. 
4. Geçmiş dönüştürme Grafik Son 90 gün içinde dönüşüm oranlarını gösterir. 
5. Kullanıcılar aracı erişerek kullanıcılarınıza daha iyi anlayın. Her adımda filtreleri kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
  * [Kullanıma genel bakış](usage-overview.md)
  * [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
  * [Bekletme](usage-retention.md)
  * [Çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)
  * [Kullanıcı bağlamı Ekle](usage-send-user-context.md)
  * [Power BI’a aktarma](../../azure-monitor/app/export-power-bi.md )

