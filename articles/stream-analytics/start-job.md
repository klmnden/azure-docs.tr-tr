---
title: Azure Stream Analytics işi başlatma
description: Bu makalede, Stream Analytics işini başlatmak açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: 9bc3e4132919e5fc5baadc78841e66efd3c34bcd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59005951"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi başlatma

Azure portalı, Visual Studio ve PowerShell kullanarak Azure Stream Analytics işinizi başlayabilirsiniz. Bir işi başlattığınızda bir zaman çıkış oluşturmaya başlamak iş için seçin. Her Azure portalı, Visual Studio ve PowerShell başlangıç saati ayarlamak için farklı yöntemler vardır. Bu yöntemleri aşağıda tanımlanmıştır.

## <a name="start-options"></a>Başlatma seçenekleri
Bir işi başlatmak üç aşağıdaki seçenekler kullanılabilir. Aşağıda belirtilen her zaman içinde belirtilenlerden olduğunu unutmayın [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics). TIMESTAMP BY belirtilmediği takdirde geliş saati kullanılacaktır.
* **Artık**: İş başlatıldığında aynı akışı çıkış olayı başlangıç noktası sağlar. Zamana bağlı bir işleç kullanılıyorsa (örneğin zaman penceresi, bekleme veya BİRLEŞMEDE), Azure Stream Analytics otomatik olarak görünür geri veri kaynağındaki giriş. Örneğin, "Şimdi" bir iş başlatabilirsiniz ve sorgunuzu bir 5 dakika atlayan pencere kullanıyorsa, Azure Stream Analytics 5 dakika önce giriş verilerini arama.
İlk olası çıkış olayı eşit veya daha fazla geçerli bir zaman damgası var ve mantıksal olarak çıktı edilebilir tüm giriş olayları hesaba için ASA garanti eder. Örneğin, hiçbir kısmi pencereli toplamlar oluşturulur. Bu her zaman tam toplanmış değerdir.

* **Özel**: Çıkış başlangıç noktası seçebilirsiniz. Benzer şekilde **artık** zamana bağlı bir işleç kullanılıyorsa seçeneği, Azure Stream Analytics bu saatten önce verileri otomatik olarak okuyup 

* **Son durdurulduğunda**. Bu seçenek, işi daha önce başlatıldı, ancak el ile durduruldu veya başarısız olduğunda kullanılabilir. Bu seçenek belirlendiğinde Azure Stream Analytics işi hiçbir veri kaybı, bu nedenle yeniden başlatmak için son çıkış saati kullanır. Zamana bağlı bir işleç kullanılıyorsa, benzer şekilde önceki seçenekleri için Azure Stream Analytics otomatik olarak bu saatten önce verileri okur. Giriş bölümlerini farklı zaman olabileceği tüm bölümlerin erken durdurma saati kullanılır, bunun sonucunda bazı yinelemeler çıktısında görülebilir. Hakkında daha fazla bilgi tam olarak-bir kez işlenmesini sayfasında kullanılabilir [olay teslimat Garantileriyle](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics).


## <a name="azure-portal"></a>Azure portal

Azure portal ve select işinize gidin **Başlat** genel bakış sayfasında. Seçin bir **iş çıkışı başlangıç zamanı** seçip **Başlat**.

İçin seçeneklerden birini **iş çıkışı başlangıç zamanı**. Seçenekler *artık*, *özel*, ve daha önce iş çalıştırılırsa, *son durdurulduğunda*. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.

## <a name="visual-studio"></a>Visual Studio

Proje Görünümü'nde işi başlatmak için yeşil ok düğmesini seçin. Ayarlama **iş çıkışı başlangıç modu** seçip **Başlat**. İş durumu değişir **çalıştıran**.

Üç seçenek için **iş çıkışı başlangıç modu**: *JobStartTime*, *CustomTime*, ve *LastOutputEventTime*. Bu özellik yoksa, varsayılan değer *JobStartTime*. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.


## <a name="powershell"></a>PowerShell

PowerShell kullanarak işinizi başlatmak için aşağıdaki cmdlet'i kullanın:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Üç seçenek için **OutputStartMode**: *JobStartTime*, *CustomTime*, ve *LastOutputEventTime*. Bu özellik yoksa, varsayılan değer *JobStartTime*. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.

Daha fazla bilgi için `Start-AzStreamAnalyitcsJob` cmdlet'i, Görünüm [başlangıç AzStreamAnalyticsJob başvuru](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob).

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı Başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı Başlangıç: Visual Studio için Azure Stream Analytics araçları kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
