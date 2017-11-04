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
ms.openlocfilehash: d7af89409cb908f98f86288a0d673ab287e3aaaa
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Müşteriler, uygulamanızın uygulama Öngörüler Funnels ile nasıl kullandığını keşfedin

Anlama müşteri işinize utmost önem deneyimidir. Uygulamanız birden çok aşamaları içeriyorsa, müşterilerin çoğu tüm süreci devam veya belirli bir noktada işlem sona erecektir bilmeniz gerekir. Bir dizi adımı bir web uygulaması aracılığıyla ilerlemeyi "Huni" bilinir. Kullanıcılarınızı ve izleme hakkında adım adım dönüştürme oranları Öngörüler elde etmek için uygulama Öngörüler Funnels kullanabilirsiniz. 

## <a name="get-started-with-the-funnels-blade"></a>Funnels dikey ile çalışmaya başlama
Funnels hakkında bilgi edinmek için kolay bir örnek ancak yürütmek için yoludur. Aşağıdaki çizimler bir e-ticaret iş adımları sahipleri müşterilerine kendi web uygulaması ile nasıl etkileşim öğrenmek için harcanacak göstermektedir.  

### <a name="create-your-funnel"></a>Huni oluşturma
Huni oluşturmadan önce yanıt istediğiniz soru karar vermeniz gerekir. Örneğin, bir tanıtım giriş sayfasını tıklatın görüntüleme kaç müşteriler bilmek isteyebilirsiniz. Bu örnekte, Fabrikam Fiber şirket sahipleri öğeleri geçen ay sırasında kendi alışveriş sepetine ekledikten sonra satın almasını kolaylaştıran müşteriler yüzdesi bilmek ister.

Burada, bunlar kendi Huni oluşturmak için adımlar bulunmaktadır.

1. Funnels dikey yeni düğmesini tıklatın.
1. "Geçen ay" zaman aralığını seçin **zaman aralığı** açılır. 
1. Seçin **ürün sayfası** olayından **1. adım** aşağı açılan liste. 
1. Seçin **eklemek alışveriş sepetine** olayından **2. adım** aşağı açılan liste.
1. Seçin **tıklatın satın alma** olayından **adım 3** aşağı açılan liste.
1. Huni için bir ad eklemek ve tıklayın **kaydetmek**.

Aşağıdaki çizimde Funnels dikey veriler üretir gösterilmektedir. Fabrikam buradan sahipleri, geçen hafta sırasında %22.7 kendi alışveriş sepetine eklenen bir öğe müşterilerinin satın alma tamamlandı görebilirsiniz. Bunlar %1 müşterilerin kendi satın alma tamamladıktan sonra ürün sayfası ve % 20'müşterilerinin ziyaret'ı oturumunuz önce bir reklam tıklattınız de görebilirsiniz.


![Verilerle funnels dikey penceresi](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a>Sonraki adımlar
  * [Kullanıma genel bakış](app-insights-usage-overview.md)
  * [Kullanıcıları, oturumlar ve olaylar](app-insights-usage-segmentation.md)
  * [Bekletme](app-insights-usage-retention.md)
  * [Çalışma kitapları](app-insights-usage-workbooks.md)
  * [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
