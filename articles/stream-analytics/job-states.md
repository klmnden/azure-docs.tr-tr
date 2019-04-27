---
title: Azure Stream Analytics iş durumları
description: Bu makalede bir Stream Analytics işi farklı durumları
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/06/2019
ms.openlocfilehash: 28e0e69d3a6a4d3a38146cbf2c49426b3b16c784
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60789463"
---
# <a name="azure-stream-analytics-job-states"></a>Azure Stream Analytics iş durumları

Bir Stream Analytics işi, belirli bir zamanda dört durumdan birinde olabilir. Stream Analytics işinizin genel bakış sayfasında işinizin durumunu Azure portalında bulabilirsiniz. 

| Durum | Açıklama | Önerilen eylemler |
| --- | --- | --- |
| **Çalıştıran** | İşinizi Azure'da tanımlanan giriş kaynaklardan gelen, işledikten ve sonuçları yapılandırılmış çıktı havuzlarından yazma olayları okumaya çalışıyor. | İzleme tarafından işinizin performansını izlemek için en iyi bir uygulamadır [ana ölçümleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). |
| **Durduruldu** | İşiniz durduruldu ve olayları işlemez. | NA | 
| **Düzeyi düşürülmüş** | Geçici hatalar büyük olasılıkla iş metriklerini. Stream Analytics, bu tür hataları kurtarmak ve (birkaç dakika içinde) çalışıyor durumuna döndürmek hemen çalışacaktır. Bu hatalar sorun ağ sorunlarından, diğer Azure kaynaklarının hataları vb. seri durumdan çıkarma. Proje düzeyi düşürülmüş bir durumda olduğunda, işinizin performansı etkilenebilir.| Bakabilirsiniz [Tanılama veya etkinlik günlükleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) bu geçici hataların nedeni hakkında daha fazla bilgi için. Serileştirmeyi kaldırma hataları gibi durumlarda, olayları hatalı biçimlendirilmiş değil emin olmak için düzeltici eylemlerde önerilir. İşin kaynak kullanımı sınırı ulaşmasını tutar, SU sayısını artırmayı deneyin veya [işinizi paralel hale getirmek](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). Burada, alamaz herhangi bir eylem diğer durumlarda, Stream Analytics için kurtarmayı deneyecek bir *çalıştıran* durumu.  |
| **Başarısız** | İş başarısız durumda bunun sonucunda kritik bir hatayla karşılaştı. Olayları okuma ve işlenir. Çalışma zamanı hataları hatalı bir durumda bitiş işleri için yaygın bir nedeni var. | Yapabilecekleriniz [uyarıları yapılandırma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) böylece iş başarısız durumuna geçtiğinde bildirim alın. <br> <br>Kullanarak hata ayıklama [etkinlik ve tanılama günlüklerini](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) kök nedeni belirlemek ve sorunu çözmek için.|

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics işleri için uyarıları ayarlama](stream-analytics-set-up-alerts.md)
* [Stream Analytics'te mevcut olan ölçümler](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [Etkinlik ve tanılama günlüklerini kullanarak sorun giderme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)
