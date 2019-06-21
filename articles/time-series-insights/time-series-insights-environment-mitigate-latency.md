---
title: Azure zaman serisi Öngörülerinde azaltma azaltmak ve izlemek nasıl | Microsoft Docs
description: Bu makalede, izleme, tanılama ve gecikme süresi ve azaltma Azure zaman serisi Öngörülerinde neden performans sorunlarını azaltmak açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 129476c833e596d40daa7081e23c0fd6d1b93b30
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165770"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Azure zaman serisi görüşleri'nde gecikme süresini azaltmak için azaltmayı giderme ve izleme

Ortamınızın yapılandırmasını gelen veri miktarı aşarsa, gecikme süresi veya Azure zaman serisi Öngörülerinde azaltma karşılaşabilirsiniz.

Gecikme süresi ve analiz etmek istediğiniz veri miktarı, ortamınızı düzgün şekilde yapılandırarak azaltma önleyebilirsiniz.

Gecikme süresi ve ne zaman azaltma deneyimi büyük olasılıkla:

- Ayrılan giriş oranınız (zaman serisi öngörüleri edinmek için gerekir) aşabilir eski verileri içeren olay kaynağı ekleme.
- Daha fazla olay kaynağı (Bu ortamınızın kapasiteyi aşabilir) ek olaylar depodan Sonuçta, bir ortam ekleyin.
- Olay kaynağı (zaman serisi öngörüleri edinmek için gerekir) bir gecikme bunun sonucunda, büyük miktarlarda geçmiş olaylar gönderin.
- Telemetri, daha büyük olay boyutu elde başvuru verileriyle birleştirin.  Azaltma açısından, paket boyutu 32 KB'lık bir ingressed veri paketi 32 olayları olarak kabul edilir, her 1 KB boyutlu. 32 KB izin verilen en uzun olay boyutudur; 32 KB'den büyük veri paketleri kesilir.

## <a name="video"></a>Video

### <a name="learn-about-time-series-insights-data-ingress-behavior-and-how-to-plan-for-itbr"></a>Time Series Insights veri giriş davranışı ve onun için planlama hakkında daha fazla bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/npeZLAd9lxo]

## <a name="monitor-latency-and-throttling-with-alerts"></a>Gecikme süresi ve azaltma uyarılarla izleme

Uyarılar, ortamınız tarafından neden gecikme sorunlarını tanılamak ve azaltmak amacıyla yardımcı olabilir.

1. Azure portalında **ölçümleri**.

   [![Ölçümleri](media/environment-mitigate-latency/add-metrics.png)](media/environment-mitigate-latency/add-metrics.png#lightbox)

1. **Ölçüm uyarısı ekle**’yi seçin.  

   [![Ölçüm uyarısı Ekle](media/environment-mitigate-latency/add-metric-alert.png)](media/environment-mitigate-latency/add-metric-alert.png#lightbox)

Buradan, aşağıdaki ölçümleri kullanarak uyarıları yapılandırabilirsiniz:

|Ölçüm  |Açıklama  |
|---------|---------|
|**Giriş alınan bayt**     | Ham bayt sayısı, olay kaynaklarından okuyun. Ham sayısı genellikle özellik adı ve değeri içerir.  |  
|**Giriş geçersiz ileti alındı.**     | Geçersiz ileti sayısı, tüm Azure Event Hubs veya Azure IOT Hub olay kaynağından okur.      |
|**Giriş iletileri alındı**   | İletilerin sayısı, tüm olay hub'ları veya IOT hub'ları olay kaynakları okuyun.        |
|**Giriş bayt depolanan**     | Toplam depolanan olayları boyutuna ve sorgu için kullanılabilir. Yalnızca özellik değerine boyutu hesaplanır.        |
|**Giriş olayları depolanan**     |   Düzleştirilmiş olayları depolanır ve sorgu için kullanılabilir sayısı.      |
|**Alınan ileti zaman gecikmesini giriş**    |  İleti sıraya alınan olay olduğu zaman arasındaki saniye cinsinden kaynak ve giriş işlendiği zaman farkı.      |
|**İleti sayısı Lag giriş alındı**    |  Son sıradaki ileti sıra numarası arasındaki fark, giriş işlenmekte olan ileti bölüm ve sıra sayısı olay kaynağı.      |

![Gecikme süresi](media/environment-mitigate-latency/latency.png)

* Aşarak, bir değer görürsünüz *giriş alınan ileti zaman gecikmesini*, kaç saniye, TSI arkasında gerçekleşen süresi iletidir size bildiren İsabetleri (appx dizin oluşturma zamanı dışında. olay kaynağı 30-60 saniye).  *Giriş alınan ileti sayısı Lag* arkasında, ileti sayısını belirlemek sağlayan bir değer de sahip olmalıdır.  Yakalanan için en kolay yolu, fark üstesinden olanak sağlayacak bir boyuta ortamınızın kapasite artırmaktır.  

  Örneğin, bir tek birim S1 ortamınız varsa ve 5,000,000 ileti lag görmek, ortamınıza yakalandı için altı birim için günde bir geçici boyutunu arttırabilir.  Hatta daha fazla sınırlandıramazsınız catch yukarı daha hızlı artırabilir. Oldukça sık karşılaştıkları özellikle bu olayları zaten var. bir olay kaynağı bağlandığınızda başlangıçta bir ortam sağlanırken veya geçmiş verileri karşıya yükleme çok sayıda toplu dönemdir.

* Ayarlamak için başka bir tekniktir bir **depolanan giriş olayları** uyarı > = 2 saat boyunca toplam ortam kapasite biraz aşağıda bir eşiği.  Bu uyarı, gecikme süresi yüksek bir olasılığını gösteren kapasitede sürekli olup olmadığını anlamanıza yardımcı olabilir. 

  Sağlanan üç adet S1 birimi (veya 2100 olay başına dakika alma kapasitesi) varsa, örneğin, ayarlayabilirsiniz bir **depolanan giriş olayları** için uyarı > 1900 olayları için 2 saat =. Sürekli olarak bu eşiğini ve bu nedenle, uyarıyı tetikleyen ise, büyük olasılıkla altında-sağlanır.  

* Kaynaklarınızın azaltılıp şüpheleniyorsanız karşılaştırabileceğiniz, **giriş alınan iletilerin** ile olay kaynağı iletileri egressed.  Olay hub'ıyla giriş sayısından büyükse, **giriş alınan iletilerin**, zaman serisi görüşleri, büyük olasılıkla aşarak.

## <a name="improving-performance"></a>Performansı iyileştirme

Azaltma veya karşılaşılan gecikme süresini azaltmak için bunu düzeltmek için en iyi yolu ortamınızın kapasite artırmaktır.

Gecikme süresi ve analiz etmek istediğiniz veri miktarı, ortamınızı düzgün şekilde yapılandırarak azaltma önleyebilirsiniz. Ortamınız için kapasite ekleme hakkında daha fazla bilgi için bkz. [ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Sonraki adımlar

- Ek sorun giderme adımları için [tanılama ve zaman serisi görüşleri ortamınıza problemleri çözmenize](time-series-insights-diagnose-and-solve-problems.md).

- Ek Yardım için üzerinde bir konuşma Başlat [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). Ayrıca [Azure Destek](https://azure.microsoft.com/support/options/) yardımlı destek seçenekleri için.
