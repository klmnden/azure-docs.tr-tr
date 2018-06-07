---
title: Azure zaman serisi Öngörüler ortamınızı ölçeklendirme | Microsoft Docs
description: Bu makalede, Azure zaman serisi Öngörüler ortamınızın ölçeğini açıklar. Eklemek veya içinde bir fiyatlandırma SKU kapasitesi çıkarmak için Azure Portalı'nı kullanın.
ms.service: time-series-insights
services: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 7e603cc9c130de6b65ae1935ac974557f5e74737
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652000"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Zaman serisi Öngörüler ortamınızı ölçeklendirme

Bu makalede, Azure portalını kullanarak zaman serisi Öngörüler ortamınızı ortamınızı kapasitesini değiştirmek açıklar. Kapasite Giriş oranı, depolama kapasitesi ve maliyet, seçilen SKU ile ilişkili uygulanan çarpanı toplamıdır. 

Artırmak veya belirli bir fiyatlandırma SKU kapasitesiyle azaltmak için Azure portalını kullanabilirsiniz. 

Ancak, fiyatlandırma katmanı değiştirilirse SKU izin verilmiyor. Örneğin, SKU fiyatlandırma bir S1 olan bir ortamda bir S2 içine veya tersi dönüştürülemiyor. 


## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU giriş hızları ve kapasiteleri

| S1 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | 1 GB (1 milyon olay) | Aylık 30 GB (30 milyon olay) |
| 10 | 10 GB (10 milyon olay) | Ayda 300 GB (300 milyon olay) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU giriş hızları ve kapasiteleri

| S2 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | 10 GB (10 milyon olay) | Ayda 300 GB (300 milyon olay) |
| 10 | 100 GB (100 milyon olay) | Aylık 3 TB (3 milyon olay) |

Kapasiteleri doğrusal olarak, S1 SKU kapasitesi 2 ile her ay gün giriş oranı ve 60 GB (60 milyon olayları) başına 2 GB (2 milyon) olay destekler şekilde ölçeklendirilir.

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızı kapasitesini değiştirme
1. Azure portalında bulun ve zaman serisi Öngörüler ortamınızı seçin. 

2. Zaman serisi Insighs ortamınızı menüsünü **yapılandırma**.

   ![Configure.PNG](media/scale-your-environment/configure.png)

3. Ayarlama **kapasite** giriş ücretlerinizi gereksinimlerini karşılayan kapasitesini ve depolama kapasitesini seçmek için kaydırıcıyı. Bildirim **giriş oranı**, **depolama kapasitesi**, ve **maliyet tahmini** dinamik olarak değişikliğin etkisini göstermek için güncelleştirme. 

   ![Kaydırıcı](media/scale-your-environment/slider.png)

   Alternatif olarak, kaydırıcıyı sağa metin kutusuna kapasite çarpanı sayısını yazın. 

4. Seçin **kaydetmek** ortamı ölçeklendirme. İlerleme göstergesi, kısa bir süre içinde değişiklik uygulanır kadar görüntülenir. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Yeni Kapasite azaltma önlemek yeterli olduğunu doğrulayın](time-series-insights-diagnose-and-solve-problems.md).
