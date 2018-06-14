---
title: Azure Stream Analytics işi izleme anlama
description: Bu makalede Azure akış analizi işleri izleme
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 4b048705c80b7776ab0ab6823e27659a01eedeb5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907460"
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
| SU Kullanım Yüzdesi       | Akış birimleri kullanımını işe iş ölçek sekmesinden atanır. Bu gösterge % 80 ulaşmalıdır ya da yukarıdaki mevcut olay işleme gecikebilir veya ilerleme durduruldu yüksek olasılık. |
| Giriş Olayları           | Stream Analytics işinde olayların sayısı tarafından alınan veri miktarı. Bu olaylar giriş kaynağına gönderilme doğrulamak için kullanılabilir. |
| Çıkış Olayları          | Çıktı hedeflenecek olayların sayısını akış analizi işi tarafından gönderilen veri miktarı. |
| Düzen dışı olayları    | Bırakılan veya olay sıralama ilkesini temel alarak ayarlanmış bir zaman damgası, verilen sıra dışında alınan olayların sayısı. Bu sırası tolerans penceresi ayarını yapılandırma tarafından etkilenebilir. |
| Veri Dönüştürme Hataları | Akış analizi işi tarafından oluşturulan veri dönüştürme hatası sayısı. |
| Çalışma Zamanı Hataları         | Sorgu işleme (olayları veya outputing sonuçları alma sırasında bulunan hatalar dışında) ilgili hatalarının toplam sayısı |
| Geç Giriş Olayları      | Geç gerçekleşme tolerans penceresi ayarı olay sıralama İlkesi yapılandırmasına bağlı olarak, ya da bırakılmış olan kaynak ya da kendi zaman damgası geç gelen olayların sayısının ayarlandı. |
| İşlev İstekleri      | Azure Machine Learning işlevi (varsa) çağrı sayısı. |
| Başarısız İşlev İstekleri | Başarısız Azure Machine Learning işlev çağrıları (varsa) sayısı. |
| İşlev Olayları        | Azure Machine Learning işlevine (varsa) gönderilen olay sayısıdır. |
| Giriş Olayı Bayt Sayısı      | Stream Analytics işinde bayt tarafından alınan veri miktarı. Bu olaylar giriş kaynağına gönderilme doğrulamak için kullanılabilir. |


## <a name="customizing-monitoring-in-the-azure-portal"></a>Azure portalında izleme özelleştirme
Grafik, gösterilen ölçümleri türünü ayarlayın ve grafiği Düzenle ayarlarında aralık süresi. Ayrıntılar için bkz [özelleştirme izleme nasıl](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Sorgu izleme saati grafiği](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>En son çıktı
İşinizi izlemek için başka bir ilginç veri noktası genel bakış sayfasında görüntülenen son çıktı saattir.
Bu uygulamanın süresi (Olay verileri zaman damgasını kullanarak zaman)'dır işinin en son çıktı.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

