---
title: Azure Time Series Insights ortamınızı ölçeklendirme | Microsoft Docs
description: Bu makalede, Azure Time Series Insights ortamınızı ölçeklendirme açıklar. Eklemek veya bir fiyatlandırma SKU kapasitesiyle çıkarmak için Azure portalını kullanın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.custom: seodec18
ms.openlocfilehash: f6c2c8c49f34c707c7b0fb4d525337fbfa169a3e
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692434"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Time Series Insights ortamınızı ölçeklendirme

Bu makalede, Azure portalını kullanarak zaman serisi görüşleri ortamınıza ortamınızın kapasiteyi değiştirmek açıklar. Giriş oranı, depolama kapasitesi ve maliyeti, seçilen SKU ile ilişkili uygulanan çarpan kapasitesidir. 

Artırmak veya belirli bir fiyatlandırma SKU kapasitesiyle azaltmak için Azure portalını kullanabilirsiniz. 

Ancak, fiyatlandırma katmanını değiştirme SKU izin verilmiyor. Örneğin, S2 veya tam tersi bir S1 fiyatlandırma SKU'su bir ortam dönüştürülemez. 


## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU'ya giriş oranları ve kapasite

| S1 SKU kapasitesi | Giriş oranı | En fazla depolama kapasitesi
| --- | --- | --- |
| 1 | 1 GB (1 milyon olay) | 30 GB / ay (30 milyona kadar olay) |
| 10 | 10 GB (10 milyon olay) | Ayda 300 GB (300 milyon olay) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU giriş oranları ve kapasite

| S2 SKU kapasitesi | Giriş oranı | En fazla depolama kapasitesi
| --- | --- | --- |
| 1 | 10 GB (10 milyon olay) | Ayda 300 GB (300 milyon olay) |
| 10 | 100 GB (100 milyon arası olay) | (3 milyar olaya) 3 TB / ay |

Kapasiteleri doğrusal olarak, ölçeği, böylece bir S1 SKU kapasitesi 2 ile aylık günlük giriş oranı ve 60 GB (60 milyon olay) başına 2 GB (2 milyon) olayları destekler.

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızın kapasitesini değiştirme
1. Azure portalında bulun ve zaman serisi görüşleri ortamınızı seçin. 

2. Zaman serisi görüşleri ortamınız için menüde **yapılandırma**.

   ![Configure.PNG](media/scale-your-environment/configure.png)

3. Ayarlama **kapasite** kaydırıcısının giriş ücretlerinizi gereksinimlerini karşılayan kapasite ve depolama kapasitesini seçin. Bildirim **giriş oranı**, **depolama kapasitesi**, ve **tahmini maliyet** dinamik olarak değişikliğin etkisini göstermek için güncelleştirme. 

   ![Kaydırıcı](media/scale-your-environment/slider.png)

   Alternatif olarak, metin kutusuna kaydırıcı sağındaki kapasite çarpanı sayısını yazın. 

4. Seçin **Kaydet** ortamı ölçeklendirebilirsiniz. İlerleme göstergesi, kısa bir süre içinde değişiklik kaydedilinceye kadar görüntülenir. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Yeni Kapasite azaltmayı önlemek için yeterli olduğundan emin olun](time-series-insights-diagnose-and-solve-problems.md).
