---
title: "Azure uygulama Öngörüler Funnels"
description: "Müşteriler uygulamanız ile nasıl etkileşim bulmak için Funnels nasıl kullanabileceğinizi öğrenin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: mbullwin
ms.openlocfilehash: bbb25af888f34737f6a61cf43890ff248c4cc4de
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Müşteriler, uygulamanızın uygulama Öngörüler Funnels ile nasıl kullandığını keşfedin

Müşteri Deneyimini anlama işletmeniz için utmost öneme sahip değil. Uygulamanız birden çok aşamaları içeriyorsa, müşterilerin çoğu tüm süreci devam veya belirli bir noktada işlem sona erecektir bilmeniz gerekir. Bir dizi adımı bir web uygulaması aracılığıyla ilerlemeyi olarak bilinen bir *huni*. Azure uygulama Öngörüler Funnels kullanıcılarınızın Öngörüler elde etmek için kullanabilir ve adım adım dönüştürme oranları izleyin. 

## <a name="create-your-funnel"></a>Huni oluşturma
Huni oluşturmadan önce yanıt istediğiniz soru karar verin. Örneğin, kaç kullanıcının giriş sayfası görüntülediğiniz bilmek görüntüleme müşteri profili ve bir anahtar oluşturma isteyebilirsiniz. Bu örnekte, Fabrikam Fiber şirket sahipleri başarıyla bir müşteri bileti oluşturun müşteriler yüzdesi bilmek ister.

Burada, bunlar kendi Huni oluşturmak için adımlar bulunmaktadır.

1. Uygulama Öngörüler Funnels aracında seçin **yeni**.
1. Gelen **zaman aralığı** açılır menüsünde, select **Son 90 gün**. Şunlardan birini seçin **My funnels** veya **funnels paylaşılan**.
1. Gelen **1. adım** aşağı açılan listesinden, **dizin**. 
1. Gelen **2. adım** listesinde **müşteri**.
1. Gelen **adım 3** listesinde **oluşturma**.
1. Huni için bir ad ekleyin ve seçin **kaydetmek**.

Aşağıdaki ekran görüntüsünde, verilerin Funnels aracı türü örneği oluşturur gösterir. Fabrikam sahipleri, Son 90 gün içinde bir müşteri bileti oluşturulan giriş sayfasını ziyaret müşterilerinin 54.3 yüzde görebilirsiniz. Bunlar 2,700 müşterilerinin giriş sayfasından dizine geldiğini de görebilirsiniz. Bu yenileme sorunu gösterebilir.


![Veri ekran görüntüsü, Funnels aracıyla](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnels-features"></a>Funnels özellikleri
Önceki ekran beş vurgulanan alanları içerir. Bunlar Funnels özellikleridir. Aşağıdaki listede ekran karşılık gelen her alanda hakkında daha fazla açıklanmaktadır:
1. Uygulamanızı örneklenen örnekleme başlık görürsünüz. Başlık seçerek örnekleme devre dışı bırakmak nasıl açıklayan bir bağlam bölmesi açılır. 
2. Huni verebilirsiniz [Power BI](app-insights-export-power-bi.md).
3. Sağ tarafta daha fazla ayrıntı görmek için bir adım seçin. 
4. Geçmiş dönüştürme Grafik dönüştürme oranları Son 90 gün içinde gösterir. 
5. Kullanıcılar aracını erişerek kullanıcılarınıza daha iyi anlayın. Her adımda filtreleri kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
  * [Kullanıma genel bakış](app-insights-usage-overview.md)
  * [Kullanıcıları, oturumlar ve olaylar](app-insights-usage-segmentation.md)
  * [Bekletme](app-insights-usage-retention.md)
  * [Çalışma kitapları](app-insights-usage-workbooks.md)
  * [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
  * [Power BI’a aktarma](app-insights-export-power-bi.md)

