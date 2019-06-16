---
title: Azure Stream Analytics işi izlemeyi anlama
description: Bu makalede, Azure portalında Azure Stream Analytics işlerini izleme açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 43dd8be998e0f8f3b5a2b783c6a01d5b5ef3da12
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65506916"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Stream Analytics işi izleme ve sorguları izleme anlama

## <a name="introduction-the-monitor-page"></a>Giriş: İzlenecekler sayfası
Azure portal her ikisi de izleme ve sorgu ve iş performansınızı sorun giderme için kullanılan temel performans ölçümlerini yüzey. Bu ölçümler görmek için Gözat görmeniz için ölçümleri ilgilendiğiniz ve görüntülemek Stream Analytics işi **izleme** bölümüne genel bakış sayfasında.  

![Stream Analytics işini bağlantı izleme](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Penceresinde gösterildiği gibi görünür:

![Stream Analytics işi izleme Panosu](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Stream Analytics için mevcut olan ölçümler
| Ölçüm                 | Tanım                               |
| ---------------------- | ---------------------------------------- |
| Biriktirme Listesindeki Giriş Olayları       | Biriktirme listesindeki giriş olayların sayısı. Bu ölçüm için sıfır olmayan bir değer işinizi, gelen olay sayısı ile tutmak mümkün olmadığını gösterir. Bu değer yavaş artan ya da sürekli olarak sıfır ise, projeyi ölçeklendirmeniz gerekir. Ederek daha fazla bilgi [anlayın ve akış birimi Ayarla](stream-analytics-streaming-unit-consumption.md). |
| Veri Dönüştürme Hataları | Beklenen çıktıyı şemaya dönüştürülemiyor çıkış olaylarının sayısı. Hata İlkesi 'Bu senaryoyla karşılaşırsanız olayları bırakmak için açılan' değiştirilebilir. |
| Erken Giriş Olayları       | Olayları, uygulama zaman kendi varış süresi 5 dakikadan daha öncesi. |
| Başarısız İşlev İstekleri | Başarısız (varsa) Azure Machine Learning işlevi çağrı sayısı. |
| İşlev Olayları        | (Varsa) Azure Machine Learning işleve gönderilen olay sayısı. |
| İşlev İstekleri      | Azure Machine Learning işlevi (varsa) yapılan çağrı sayısı. |
| Giriş Serileştirme Kaldırma Hataları       | Giriş olayları seri durumdan çıkarılamadı sayısı.  |
| Giriş Olayı Bayt Sayısı      | Stream Analytics işinde bayt tarafından alınan veri miktarı. Bu olaylar için giriş kaynağı gönderildiğini doğrulamak için kullanılabilir. |
| Giriş Olayları           | Giriş olayları seri durumdan kayıt sayısı. Bu sayı, seri durumundan çıkarma hatalara neden gelen olayları içermez. |
| Alınan Giriş Kaynakları       | İş tarafından alınan iletilerin sayısı. Olay hub'ı için bir tek EventData iletisidir. BLOB için bir ileti tek bir blobudur. Giriş kaynakları önce seri durumundan çıkarma sayıldığını unutmayın. Serileştirmeyi kaldırma hataları varsa, giriş kaynakları giriş olaylarını büyük olabilir. Aksi takdirde, küçük veya buna eşit her ileti birden çok olayı içerebileceğinden olayları giriş olabilir. |
| Geç Giriş Olayları      | Daha fazla yapılandırılmış geç varış toleransı penceresi gelen olayları. Daha fazla bilgi edinin [Azure Stream Analytics olay sırası konuları](stream-analytics-out-of-order-and-late-events.md) . |
| Sıra dışı olayları    | Bırakılan veya olay sıralama ilkesine göre ayarlanmış bir zaman damgası, verilen sıranın dışında alınan olay sayısı. Out of toleransı penceresi ayarının yapılandırmasını tarafından etkilenmiş. |
| Çıkış Olayları          | Çıkış hedefte olayların sayısı Stream Analytics işi tarafından gönderilen veri miktarı. |
| Çalışma Zamanı Hataları         | Sorgu işleme (olayları almak veya sonuç çıktısı bulunan hataları hariç) ilişkili hatalarının toplam sayısı |
| SU Kullanım Yüzdesi       | Akış birimi kullanımına iş ölçek sekmesinden bir işe atanmış. Bu gösterge % 80 düzeyine ulaşması gerektiğini, yukarıdaki var veya olay işlenmesi gecikebilir veya ilerleme durduruldu yüksek olasılık. |
| Eşik Gecikmesi       | İşin tüm çıkışları tüm bölümleri arasında en yüksek eşik gecikmesi. |

Bu ölçümler için kullanabileceğiniz [Stream Analytics işinizin performansını izleme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Azure portalında izleme özelleştirme
Grafikte gösterilen ölçümleri türünü ayarlamak ve zaman aralığı grafiği Düzenle ayarlarında. Ayrıntılar için bkz [özelleştirme izleme nasıl](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Stream Analytics, izleme zaman grafiği sorgulama](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Son çıkış
İşi izlemek için başka bir ilgi çekici veri noktası genel bakış sayfasında gösterilen son çıkış zamanı gelmiştir.
Bu uygulamanın süresi (Olay verileri itibaren zaman damgası kullanarak zaman)'dır işinin en son çıktı.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

