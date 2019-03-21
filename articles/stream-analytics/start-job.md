---
title: Azure Stream Analytics işi başlatma
description: Bu makalede, Stream Analytics işini başlatmak açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: fb1d724907c09e2eb77930f5a235336ca8cd3a25
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886856"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi başlatma

Azure portalı, Visual Studio ve PowerShell kullanarak Azure Stream Analytics işinizi başlayabilirsiniz. Bir işi başlattığınızda bir zaman çıkış oluşturmaya başlamak iş için seçin. Her Azure portalı, Visual Studio ve PowerShell başlangıç saati ayarlamak için farklı yöntemler vardır. Bu yöntemleri aşağıda tanımlanmıştır.

## <a name="azure-portal"></a>Azure portal

Azure portal ve select işinize gidin **Başlat** genel bakış sayfasında. Seçin bir **iş çıkışı başlangıç zamanı** seçip **Başlat**.

Üç seçenek için **iş çıkışı başlangıç zamanı**: *Artık*, *özel*, ve *son durdurulduğunda*. Seçme *artık* şu anda işini başlatır. Seçme *özel* geçmiş veya gelecek başlamak iş için özel bir zaman ayarlamanıza olanak tanır. Durdurulan bir işi veri kaybetmeksizin sürdürmek için seçin *son durdurulduğunda*.

## <a name="visual-studio"></a>Visual Studio

Proje Görünümü'nde işi başlatmak için yeşil ok düğmesini seçin. Ayarlama **iş çıkışı başlangıç modu** seçip **Başlat**. İş durumu değişir **çalıştıran**.

Üç seçenek için **iş çıkışı başlangıç modu**: *JobStartTime*, *CustomTime*, ve *LastOutputEventTime*. Bu özellik yoksa, varsayılan değer *JobStartTime*.

*JobStartTime* iş başlatıldığında aynı akışı çıkış olayı başlangıç noktası sağlar.

*CustomTime* çıkış belirtilen özel bir zaman başlar *OutputStartTime* parametresi.

*LastOutputEventTime* son olayın aynı çıkış zamanı çıkış olay akışının başlangıç noktası sağlar.

## <a name="powershell"></a>PowerShell

PowerShell kullanarak işinizi başlatmak için aşağıdaki cmdlet'i kullanın:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Üç seçenek için **OutputStartMode**: *JobStartTime*, *CustomTime*, ve *LastOutputEventTime*. Bu özellik yoksa, varsayılan değer *JobStartTime*.

*JobStartTime* iş başlatıldığında aynı akışı çıkış olayı başlangıç noktası sağlar.

*CustomTime* çıkış belirtilen özel bir zaman başlar *OutputStartTime* parametresi.

*LastOutputEventTime* son olayın aynı çıkış zamanı çıkış olay akışının başlangıç noktası sağlar.

Daha fazla bilgi için `Start-AzStreamAnalyitcsJob` cmdlet'i, Görünüm [başlangıç AzStreamAnalyticsJob başvuru](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob).

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı Başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı Başlangıç: Visual Studio için Azure Stream Analytics araçları kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
