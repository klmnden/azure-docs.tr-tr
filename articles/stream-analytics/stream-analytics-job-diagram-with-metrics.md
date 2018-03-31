---
title: Azure akış veri temelli iş diyagramı kullanarak hata ayıklama analizi | Microsoft Docs
description: Stream Analytics işiniz iş diyagramı ve ölçümleri kullanarak sorun giderin.
keywords: ''
documentationcenter: ''
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/01/2017
ms.author: jeanb
ms.openlocfilehash: 65eeeee7daa22b94074f55defdfd1219049774c9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a>Veri temelli iş diyagramı kullanarak hata ayıklama

İş diyagramı **izleme** dikey Azure portalında iş hattınızı görselleştirmenize yardımcı olabilir. Giriş, çıkış ve sorgu adımları gösterir. Her adım sorunları giderirken daha hızlı bir sorunun kaynağını yalıtmak için ölçümleri incelemek için iş diyagramı kullanabilirsiniz.

## <a name="using-the-job-diagram"></a>İş diyagramı kullanma

Azure portalında altında bir Stream Analytics işinde while **destek + sorun giderme**seçin **iş diyagramı**:

![Ölçümleri - konumu ile iş diyagramı](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

Her sorgu adım bölmesinde düzenleme sorgu karşılık gelen bölümünde görmek için seçin. Adım için bir ölçüm grafik sayfasında alt bölmesinde görüntülenir.

![Ölçümleri - temel iş ile iş diyagramı](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Azure Event Hubs'a giriş bölümlerini görmek için seçin **...** Bir bağlam menüsü görüntülenir. Giriş birleşme de görebilirsiniz.

![Diyagram ölçümlerle iş - bölümü genişletin](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

Yalnızca tek bir bölüm için ölçüm grafik görmek için bölüm düğümünü seçin. Ölçümleri sayfasının en altında gösterilir.

![Ölçümleri - daha fazla ölçümleri ile iş diyagramı](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

Bir birleşme ölçümleri grafik görmek için birleşme düğümünü seçin. Aşağıdaki grafikte hiçbir olay bırakılan veya ayarlanmış gösterir.

![İş diyagramı - ölçümlerle kılavuz](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

Saat ve ölçüm değeri ayrıntılarını görmek için grafik üzerine gelin.

![Diyagram ölçümlerle iş - getirin](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Ölçümleri kullanarak sorun giderme

**QueryLastProcessedTime** ölçüm, belirli bir adıma veri alındığında gösterir. Topoloji bakarak, hangi adımın veri almıyor görmek için çıkış işlemcisine geriye doğru çalışabilir. Bir adım veri alamıyorsanız, hemen önce sorgu adıma gidin. Önceki sorgu adımı bir zaman penceresi sahip olup olmadığını belirler ve yeterli bir süre için çıktı verilerini geçti olmadığını denetleyin. (Windows saate tutturulur Not bundan.)
 
Önceki sorgu adımı Giriş bir işlemci ise, giriş ölçümleri yanıt yardımcı olması için aşağıdaki hedeflenen soruları kullanın. Bunlar, bir iş giriş kaynaklardan veri alma olup olmadığını belirlemenize yardımcı olabilir. Sorgu bölümlendirilmişse her bir bölümü inceleyin.
 
### <a name="how-much-data-is-being-read"></a>Ne kadar veri okunan?

*   **InputEventsSourcesTotal** okuma veri birimleri sayısıdır. Örneğin, BLOB sayısı.
*   **InputEventsTotal** okuma olay sayısıdır. Bu ölçüm her bölüm için kullanılabilir.
*   **InputEventsInBytesTotal** okunan bayt sayısı.
*   **InputEventsLastArrivalTime** alınan her olayın sıraya alınan saatiyle güncelleştirilir.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Zaman ilerleyen? Gerçek olayları okuyorsanız noktalama verilen değil.

*   **InputEventsLastPunctuationTime**, zamanın ilerlemesini sağlamak için bir noktalama işaretinin ne zaman verildiğini gösterir. Noktalama işaretleri verilmemiş, veri akışı engellenen.
 
### <a name="are-there-any-errors-in-the-input"></a>Girdide hataları var mı?

*   **InputEventsEventDataNullTotal** null veri olayları sayısıdır.
*   **InputEventsSerializerErrorsTotal** doğru serisi kaldırılamadı olayların sayısıdır.
*   **InputEventsDegradedTotal** seri durumdan çıkarma dışında bir sorunla olayların sayısıdır.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Olayları bırakılan veya ayarlanmış misiniz?

*   **InputEventsEarlyTotal** yüksek Filigran önce bir uygulama zaman damgası olan olayları sayısıdır.
*   **InputEventsLateTotal** yüksek Filigran sonra bir uygulama zaman damgası olan olayları sayısıdır.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** iş başlangıç saatinden önce bırakılan sayı olayı.
 
### <a name="are-we-falling-behind-in-reading-data"></a>Biz veri okuma dönmeden?

*   **InputEventsSourcesBackloggedTotal** olay hub'ları ve Azure IOT Hub girdileri okumak kaç tane daha fazla ileti gereksinim söyler.


## <a name="get-help"></a>Yardım alın
Ek Yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
