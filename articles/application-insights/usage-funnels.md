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
ms.openlocfilehash: 0396c59d9d95ab71f0af04029d87afbb6e47dc35
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Müşteriler, uygulamanızın uygulama Öngörüler Funnels ile nasıl kullandığını keşfedin

Anlama müşteri işinize utmost önem deneyimidir. Uygulamanız birden çok aşamaları içeriyorsa, müşterilerin çoğu tüm süreci devam veya belirli bir noktada işlem sona erecektir bilmeniz gerekir. Bir dizi adımı bir web uygulaması aracılığıyla ilerlemeyi "Huni" bilinir. Kullanıcılarınızı ve izleme hakkında adım adım dönüştürme oranları Öngörüler elde etmek için uygulama Öngörüler Funnels kullanabilirsiniz. 

## <a name="create-your-funnel"></a>Huni oluşturma
Huni oluşturmadan önce yanıt istediğiniz soru karar vermeniz gerekir. Örneğin, kaç kullanıcının giriş sayfası görüntülediğiniz bilmek görüntüleme müşteri profili ve bir anahtar oluşturma isteyebilirsiniz. Bu örnekte, Fabrikam Fiber şirket sahipleri başarıyla bir müşteri bileti oluşturun müşteriler yüzdesi bilmek ister.

Burada, bunlar kendi Huni oluşturmak için adımlar bulunmaktadır.

1. Funnels araç çubuğundaki Yeni düğmesini tıklatın.
1. "Son 90 gün" zaman aralığını seçin **zaman aralığı** açılır. Şunlardan birini seçin "Funnels Belgelerim" veya "funnels paylaşılan"
1. Seçin **dizin** olayından **1. adım** aşağı açılan liste. 
1. Seçin **müşteri** olayından **2. adım** aşağı açılan liste.
1. Seçin **oluşturma** olayından **adım 3** aşağı açılan liste.
1. Huni için bir ad eklemek ve tıklayın **kaydetmek**.

Aşağıdaki çizimde Funnels aracı veriler üretir gösterilmektedir. Fabrikam buradan sahipleri, Son 90 gün içinde bir müşteri bileti %54.3 giriş sayfasını ziyaret müşterilerinin oluşturulan görebilirsiniz. Bunlar ayrıca görebilir 2.7 k müşterilerinin dizine giriş sayfasından gelen, bu yenileme sorun olduğunu gösteriyor olabilir.


![Verilerle funnels aracı](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnel-features"></a>Huni özellikleri
1. Uygulamanızı örneklenen örnekleme başlık görürsünüz. Başlığında tıklandığında örnekleme devre dışı bırakmak nasıl söyleyen bir bağlam bölmesi açılır. 
2. Huni verebilirsiniz [Power BI](app-insights-export-power-bi.md).
3. Sağ tarafta daha ayrıntılı Öngörüler almak için bir adım tıklayın. 
4. Geçmiş dönüştürme dönüştürme Son 90 gün içinde gösterir. 
5. Funnels kullanıcılar Aracı'na giderek kullanıcılarınıza daha iyi anlayın. Her adım seçkin, kullanıcıların filtreleri verecektir. 

## <a name="next-steps"></a>Sonraki adımlar
  * [Kullanıma genel bakış](app-insights-usage-overview.md)
  * [Kullanıcıları, oturumlar ve olaylar](app-insights-usage-segmentation.md)
  * [Bekletme](app-insights-usage-retention.md)
  * [Çalışma kitapları](app-insights-usage-workbooks.md)
  * [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
  * [Power BI’a aktarma](app-insights-export-power-bi.md)

