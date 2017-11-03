---
title: "İzleme anlama akış analizi işi | Microsoft Docs"
description: "Akış analizi işi izleme anlama"
keywords: Sorgu izlemesi
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 12545dd3875e81f8f2248acceb66d2d840cf6702
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Akış analizi işi izleme ve sorguları izleme anlama

## <a name="introduction-the-monitor-page"></a>Giriş: İzleme sayfası
Azure portal her ikisi de izlemek ve sorgu ve iş performans sorunlarını gidermek için kullanılan temel performans ölçümlerini yüzey. Bu ölçümler görmek için Gözat ölçümlerini görmeniz ilgilendiğiniz ve görüntülemek Stream Analytics işi **izleme** genel bakış sayfasında bölüm.  

![Bağlantı izleme](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Pencerenin gösterildiği gibi görünür:

![İzleme işi Panosu](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Akış analizi için kullanılabilir ölçümleri
| Ölçüm                 | Tanım                               |
| ---------------------- | ---------------------------------------- |
| SU % kullanımı       | Akış birimleri kullanımını işe iş ölçek sekmesinden atanır. Bu gösterge % 80 ulaşmalıdır ya da yukarıdaki mevcut olay işleme gecikebilir veya ilerleme durduruldu yüksek olasılık. |
| Giriş olayları           | Stream Analytics işinde olayların sayısı tarafından alınan veri miktarı. Bu olaylar giriş kaynağına gönderilme doğrulamak için kullanılabilir. |
| Çıkış olayları          | Çıktı hedeflenecek olayların sayısını akış analizi işi tarafından gönderilen veri miktarı. |
| Düzen dışı olayları    | Bırakılan veya olay sıralama ilkesini temel alarak ayarlanmış bir zaman damgası, verilen sıra dışında alınan olayların sayısı. Bu sırası tolerans penceresi ayarını yapılandırma tarafından etkilenebilir. |
| Veri dönüştürme hataları | Akış analizi işi tarafından oluşturulan veri dönüştürme hatası sayısı. |
| Çalışma zamanı hataları         | Akış analizi işi yürütme sırasında gerçekleşen hataları toplam sayısı. |
| Geç giriş olayları      | Geç gerçekleşme tolerans penceresi ayarı olay sıralama İlkesi yapılandırmasına bağlı olarak, ya da bırakılmış olan kaynak ya da kendi zaman damgası geç gelen olayların sayısının ayarlandı. |
| İşlev istekleri      | Azure Machine Learning işlevi (varsa) çağrı sayısı. |
| Başarısız işleve istekleri | Başarısız Azure Machine Learning işlev çağrıları (varsa) sayısı. |
| İşlev olayları        | Azure Machine Learning işlevine (varsa) gönderilen olay sayısıdır. |
| Giriş olayı bayt      | Stream Analytics işinde bayt tarafından alınan veri miktarı. Bu olaylar giriş kaynağına gönderilme doğrulamak için kullanılabilir. |


## <a name="customizing-monitoring-in-the-azure-portal"></a>Azure portalında izleme özelleştirme
Grafik, gösterilen ölçümleri türünü ayarlayın ve grafiği Düzenle ayarlarında aralık süresi. Ayrıntılar için bkz [özelleştirme izleme nasıl](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Sorgu izleme saati grafiği](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>En son çıktı
İşinizi izlemek için başka bir ilginç veri noktası genel bakış sayfasında görüntülenen son çıktı saattir.
Bu uygulamanın süresi (Olay verileri zaman damgasını kullanarak zaman)'dır işinin en son çıktı.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

