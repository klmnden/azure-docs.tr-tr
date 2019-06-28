---
title: Azure Stream Analytics iş durumları
description: Bu makalede bir Stream Analytics işi farklı durumları
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: bef21dc35bbd2b9b50cf7b362624321866773bfe
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331352"
---
# <a name="azure-stream-analytics-job-states"></a>Azure Stream Analytics iş durumları

Bir Stream Analytics işi, belirli bir zamanda dört durumdan birinde olabilir. Stream Analytics işinizin genel bakış sayfasında işinizin durumunu Azure portalında bulabilirsiniz. 

| Eyalet | Açıklama | Önerilen Eylemler |
| --- | --- | --- |
| **Çalıştıran** | İşinizi Azure'da tanımlanan giriş kaynaklardan gelen, işledikten ve sonuçları yapılandırılmış çıktı havuzlarından yazma olayları okumaya çalışıyor. | İzleme tarafından işinizin performansını izlemek için en iyi bir uygulamadır [ana ölçümleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). |
| **Durduruldu** | İşiniz durduruldu ve olayları işlemez. | NA | 
| **Düzeyi düşürülmüş** | Giriş ve çıkış bağlantı aralıklı olarak ortaya çıkan sorunları olabilir. Bu hatalar Degraded durumuna girmesini işinizi yapabileceğiniz geçici hataları çağrılır. Stream Analytics, bu tür hataları kurtarmak ve (birkaç dakika içinde) çalışıyor durumuna döndürmek hemen çalışacaktır. Bu hatalar sorun ağ sorunlarından, diğer Azure kaynaklarının hataları vb. seri durumdan çıkarma. Proje düzeyi düşürülmüş bir durumda olduğunda, işinizin performansı etkilenebilir.| Bakabilirsiniz [Tanılama veya etkinlik günlükleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) bu geçici hataların nedeni hakkında daha fazla bilgi için. Serileştirmeyi kaldırma hataları gibi durumlarda, olayları hatalı biçimlendirilmiş değil emin olmak için düzeltici eylemlerde önerilir. İşin kaynak kullanımı sınırı ulaşmasını tutar, SU sayısını artırmayı deneyin veya [işinizi paralel hale getirmek](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). Burada, alamaz herhangi bir eylem diğer durumlarda, Stream Analytics için kurtarmayı deneyecek bir *çalıştıran* durumu. <br> Kullanabileceğiniz [eşik gecikmesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) bu geçici hatalara işinizin performansını etkileyen olmadığını anlamak için ölçüm.|
| **Başarısız** | İş başarısız durumda bunun sonucunda kritik bir hatayla karşılaştı. Olayları okuma ve işlenir. Çalışma zamanı hataları hatalı bir durumda bitiş işleri için yaygın bir nedeni var. | Yapabilecekleriniz [uyarıları yapılandırma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) böylece iş başarısız durumuna geçtiğinde bildirim alın. <br> <br>Kullanarak hata ayıklama [etkinlik ve tanılama günlüklerini](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) kök nedeni belirlemek ve sorunu çözmek için.|

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics işleri için uyarıları ayarlama](stream-analytics-set-up-alerts.md)
* [Stream Analytics'te mevcut olan ölçümler](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [Etkinlik ve tanılama günlüklerini kullanarak sorun giderme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)
