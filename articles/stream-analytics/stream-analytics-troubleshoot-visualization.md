---
title: Görselleştirme ve Azure akış analizi işi sorunlarını giderme
description: Bu makalede, Self Servis sorun giderme yapmak için tanılama diyagramı özelliğini kullanarak Stream Analytics işi, görselleştirme açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 523802f1f9a1dda19c5b6a66da7bc26fee851bd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Görselleştirme ve akış analizi işleri sorun giderme
Stream Analytics içinde diğer teknolojilerle bulut tabanlı gibi sorun giderme bazen neden bir işi beklenen çıktı (veya bu herhangi bir çıktı) üretmez içine aramak için gereklidir. Bu kavram aklınızda Stream Analytics iş akışında görselleştirme için yeteneği sağlar. Bu da bir modelleme araç olarak kullanışlı ve işlerine gerektiren bu belgeleri yan avantajına sahiptir.

Görselleştirme panelinde girişleri yürütülmekte sorgu ve yapılandırılmış sonra tüm çıktıları yanı sıra görünür. Bağlantı veya yapılandırma sorunları daha belirgin hale gelebilir ve ayrıca yapılandırmanızı görsel gösterimi görmek yararlı olabilir.

## <a name="using-the-diagnosis-diagram-tool"></a>Tanılama diyagramı Aracı'nı kullanma
Bu Görselleştirici erişmek için Stream Analytics işi, "Ayarlar" alanında "Tanılama diyagramı" düğmesine tıklamanız yeterlidir.

![stream-analytics-troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Her giriş ve çıkış o bileşenin durumunu renk kodlanmış geçerli göstermek için aşağıda gösterildiği gibi ' dir.

![stream-analytics-troubleshoot-visualization-input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Kullanıcı bir iş içindeki veri akışı desenleri anlamak için Ara sorgu adımları bakmak istediğinde görselleştirme aracı bileşeni adımları ve akışı dizisi sorgu dökümünü bir görünümünü sağlar. Her sorgu adımını tıklatarak karşılık gelen gösterildiği gibi bölmesinde düzenleme sorguda gösterir. 

![stream-analytics-troubleshoot-visualization-intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

