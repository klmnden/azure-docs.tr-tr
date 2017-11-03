---
title: "Görselleştirme ve akış analizi işleri sorunlarını giderme | Microsoft Docs"
description: "Self Servis tanılama diyagramı özelliğini kullanarak sorun giderme akış analizi işi ardışık görselleştirmek öğrenin."
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 820b73a5dbf9bb108e189313cf6ee2b924ab04c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Görselleştirme ve akış analizi işleri sorun giderme
Stream Analytics içinde diğer teknolojilerle bulut tabanlı gibi sorun giderme bazen neden bir işi beklenen çıktı (veya bu herhangi bir çıktı) üretmez içine aramak için gereklidir. Bu kavram aklınızda Stream Analytics iş akışında görselleştirme için yeteneği sağlar. Bu da bir modelleme araç olarak kullanışlı ve işlerine gerektiren bu belgeleri yan avantajına sahiptir.

Görselleştirme panelinde girişleri yürütülmekte sorgu ve yapılandırılmış sonra tüm çıktıları yanı sıra görünür. Bağlantı veya yapılandırma sorunları daha belirgin hale gelebilir ve ayrıca yapılandırmanızı görsel gösterimi görmek yararlı olabilir.

## <a name="using-the-diagnosis-diagram-tool"></a>Tanılama diyagramı Aracı'nı kullanma
Bu Görselleştirici erişmek için Stream Analytics işi, "Ayarlar" alanında "Tanılama diyagramı" düğmesine tıklamanız yeterlidir.

![Stream-Analytics-Troubleshoot-visualization-Diagnosis-Diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Her giriş ve çıkış o bileşenin durumunu renk kodlanmış geçerli göstermek için aşağıda gösterildiği gibi ' dir.

![Stream-Analytics-Troubleshoot-visualization-input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Kullanıcı bir iş içindeki veri akışı desenleri anlamak için Ara sorgu adımları bakmak istediğinde görselleştirme aracı bileşeni adımları ve akışı dizisi sorgu dökümünü bir görünümünü sağlar. Her sorgu adımını tıklatarak karşılık gelen gösterildiği gibi bölmesinde düzenleme sorguda gösterir. 

![Stream-Analytics-Troubleshoot-visualization-intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

