---
title: "İzleme ve Azure zaman serisi Öngörüler gecikme önlemek için azaltma azaltmak nasıl | Microsoft Docs"
description: "Bu makalede, izleme, tanılama ve gecikme süresi ve azaltma Azure zaman serisi öngörü neden performans sorunlarını azaltmak açıklar."
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/15/2017
ms.openlocfilehash: 9d53cd0ee8e15d47ac1daa122331b3145f936adb
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>İzleme ve Azure zaman serisi Öngörüler gecikmesini azaltmak için azaltma azaltmak
Gelen veri miktarına, ortam yapılandırma aşarsa, gecikme veya Azure zaman serisi öngörü azaltma karşılaşabilirsiniz.

Gecikme süresi ve analiz etmek istediğiniz veri miktarı, ortamınızı düzgün şekilde yapılandırarak azaltma önleyebilirsiniz.

Gecikme süresi ve ne zaman azaltma deneyimi büyük olasılıkla:

- (Zaman serisi Öngörüler Yakala gerekir), ayrılan giriş hızı aşabilir eski verileri içeren bir olay kaynağı ekleyin.
- Daha fazla olay kaynakları (ortamınızı 's kapasite aşabilir) ek olaylar depodan sonuçta bir ortam ekleyin.
- Büyük miktarda geçmiş olayları (zaman serisi Öngörüler Yakala gerekir) bir gecikme sonuçta bir olay kaynağı iletin.
- Başvuru verileri büyük olay boyutu elde telemetriyle katılın.  Bir azaltma açısından ingressed veri paketi paket boyutu 32 KB'ın 32 olayları olarak kabul edilir, her 1 KB boyuta sahip. İzin verilen en fazla olay boyutu 32 KB'tır; veri paketlerinin 32 KB'den büyük kesilir.


## <a name="monitor-latency-and-throttling-with-alerts"></a>İzleyici gecikme süresi ve uyarılarla azaltma

Uyarılar tanılamak ve ortamınızın neden gecikmesi sorunları azaltmaya yardımcı olmak için yardımcı olabilir. 

1. Azure portalında tıklatın **ölçümleri**. 

   ![Ölçümler](media/environment-mitigate-latency/add-metrics.png)

2. Tıklatın **ölçüm uyarı Ekle**.  

    ![Ölçüm uyarısı ekleme](media/environment-mitigate-latency/add-metric-alert.png)

Buradan, aşağıdaki ölçümleri kullanarak uyarıları yapılandırabilirsiniz:

|Ölçüm  |Açıklama  |
|---------|---------|
|**Giriş alınan bayt miktarı**     | Olay kaynaklardan okuma ham bayt sayısı. Ham sayısı genellikle özellik adı ve değeri içerir.  |  
|**Giriş geçersiz ileti alındı**     | Geçersiz ileti sayısı, tüm Azure Event Hubs ya da Azure IOT Hub olay kaynağından okuyun.      |
|**Giriş alınan iletileri**   | İleti sayısı tüm Event Hubs ya da IOT hub'ları olay kaynağından okuyun.        |
|**Bayt giriş depolanan**     | Toplam boyut depolanan olayların ve sorgu için kullanılabilir. Boyutu yalnızca özellik değeri hesaplanır.        |
|**Giriş olayları depolanan**     |   Sayısı düzleştirilmiş depolanan ve sorgu için kullanılabilir.      |

![Gecikme süresi](media/environment-mitigate-latency/latency.png)

Bir tekniktir ayarlamak için bir **giriş depolanan olayları** uyarı > bir eşiğinin biraz toplam ortamı kapasitenizi 2 saat boyunca =.  Bu uyarı, bir yüksek gecikme olasılığını gösterir kapasitede sürekli olup olmadığını anlamanıza yardımcı olabilir.  

Örneğin, sağlanan üç S1 birimler (veya dakika giriş kapasite başına 2100 olayları) varsa, ayarlayabileceğiniz bir **giriş depolanan olayları** için uyarı > 1900 olayları = için 2 saat. Sürekli olarak bu Eşiği aşan ve bu nedenle, uyarıyı tetikleyen varsa, büyük olasılıkla altında-sağlanır.  

Ayrıca, kısıtlanan şüpheleniyorsanız karşılaştırabileceğiniz, **giriş alınan iletilerin** , olay ile kaynak iletileri egressed.  Olay Hub'ınızı içine giriş büyükse, **giriş alınan iletilerin**, zaman serisinin Öngörülerinizi olasılıkla kısıtlanan.

## <a name="improving-performance"></a>Performansı iyileştirme 
Azaltma veya gecikme yaşıyor azaltmak için düzeltmek için en iyi yolu ortamınızı 's kapasite artırmaktır. 

Gecikme süresi ve analiz etmek istediğiniz veri miktarı, ortamınızı düzgün şekilde yapılandırarak azaltma önleyebilirsiniz. Ortamınız için kapasite ekleme hakkında daha fazla bilgi için bkz: [ortamınızın ölçeğini](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Sonraki adımlar
- Ek sorun giderme adımları için [Tanıla ve zaman serisi Öngörüler ortamınızdaki sorunları](time-series-insights-diagnose-and-solve-problems.md).
- Ek Yardım için bir konuşma başlatmak [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). De başvurabilirsiniz [Azure Destek](https://azure.microsoft.com/support/options/) yardımlı destek seçenekleri için.
