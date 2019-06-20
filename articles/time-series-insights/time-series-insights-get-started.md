---
title: Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs
description: Bu makalede, yeni bir zaman serisi görüşleri ortamı oluşturmak için Azure portalını kullanmayı açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/01/2019
ms.custom: seodec18
ms.openlocfilehash: bcab20c654657483fd37322de7ec0827fb171c01
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165721"
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Azure Portal’da yeni Zaman Serisi Görüşleri ortamı oluşturma

Bu makalede, Azure portalını kullanarak yeni bir zaman serisi görüşleri ortamı oluşturmayı açıklar.

Time Series Insights, Azure IOT hub ve Event hubs'ı dakikalar içinde akan verileri sorgulama ve görselleştirme kullanmaya başlamak saniye cinsinden sorgu büyük miktarda zaman serisi verileri için etkinleştirme sağlar.  Nesnelerin interneti (IOT) ölçeği için tasarlanmış ve terabaytlarca veri işleyebilir.

## <a name="steps-to-create-the-environment"></a>Ortam oluşturma adımları

Bir ortam oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **+ yeni** düğmesi.

1. Seçin **nesnelerin interneti** kategorisi ve select **Time Series Insights**.

   ![Zaman Serisi Görüşleri oluşturma ortam](media/time-series-insights-get-started/1-new-tsi.png)

1. Üzerinde **Time Series Insights** sayfasında **Oluştur**.

1. Gerekli parametreleri doldurun. Aşağıdaki tabloda her bir parametre açıklanmıştır:
   
   ![Zaman Serisi Görüşleri oluşturma kaynak grubu](media/time-series-insights-get-started/2-create-tsi.png)
   
   Ayar|Önerilen değer|Açıklama
   ---|---|---
   Ortam adı | Benzersiz bir ad | Bu ad ortamında temsil eder [zaman serisi Gezgininde](https://insights.timeseries.azure.com)
   Abonelik | Aboneliğiniz | Birden fazla aboneliğiniz varsa, tercihen olay kaynağınızı içeren aboneliği seçin. Zaman Serisi Görüşleri aynı abonelikteki mevcut Azure IoT Hub ve Event Hub kaynaklarını otomatik olarak algılayabilir.
   Kaynak grubu | Yeni oluşturun veya var olanı kullan | Kaynak grubu, birlikte kullanılan Azure kaynakları koleksiyonudur. Örneğin, olay hub'ı veya IOT hub'ı içeren tek bir mevcut kaynak grubunu seçebilirsiniz. Veya bu kaynak için diğer kaynakları ilişkili değilse, yeni bir duruma getirebilirsiniz.
   Location | Olay kaynağınızı en yakın | Tercihen, olay kaynağı verilerinizi içeren önlemek için çaba içinde aynı veri merkezi konumuyla bölgeler arası ve bölgeler arası bant genişliği maliyetlerini ve veri bölgesinin dışında taşırken gecikme ek seçin.
   Fiyatlandırma katmanı | S1 | Gerekli aktarım hızını seçin. Düşük maliyetler ve başlangıç kapasitesi, S1'i seçin.
   Kapasite | 1 | Giriş oranı, depolama kapasitesi ve seçilen SKU ile ilişkili maliyeti çarpan uygulandığı kapasitesidir.  Oluşturduktan sonra ortam kapasitesini değiştirebilirsiniz. En düşük maliyetleri için kapasitesi 1'i seçin. 
  
1. Denetleme **panoya Sabitle** kolayca, zaman serisi ortamı gelecekte de erişmek için.

   ![Zaman Serisi Görüşleri oluşturma panoya sabitleme](media/time-series-insights-get-started/3-pin-create.png)

1. Seçin **Oluştur** sağlama işlemini başlatın. Bu işlem birkaç dakika sürebilir.

1. Dağıtım işlemini izlemek için **bildirimleri** sembol (zil simgesi).

   ![Bildirimleri izleme](media/time-series-insights-get-started/4-notifications.png)

Dağıtım başarılı olduktan sonra seçebileceğiniz **kaynağa Git** diğer özellikleri yapılandırmak için veri erişimi ilkeleri ile güvenlik ayarlayın, olay kaynakları ve diğer eylemler ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) ortamınızın güvenliğini sağlamak için.

* [Event Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md) Azure zaman serisi görüşleri ortamınıza.

* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.

* Ortamınızı görüntüleyebilirsiniz [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
