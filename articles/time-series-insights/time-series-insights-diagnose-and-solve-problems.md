---
title: "Sorunları tanılamak ve | Microsoft Docs"
description: "Bu öğretici, zaman serisinin Öngörüler ortamınızdaki sorunları tanılamada ve alınmaktadır"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 4e10a009eb67706d927ece5692134d802094cdf9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Zaman serisi Öngörüler ortamınızdaki sorunları tanılamada ve

## <a name="i-dont-see-my-data"></a>Verilerimi görmüyorum
Neden değil verilerinizi içinde gördüğünüz ortamınızda bazı nedenler şunlardır [Azure zaman serisi Insights portalında](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>Olay kaynağı JSON biçiminde veri yok
Azure zaman serisi Öngörüler yalnızca JSON verilerini bugün destekler. JSON örnekler için bkz: [desteklenen JSON şekiller](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a>Olay kaynağı kaydolurken gerekli izne sahip anahtar sağlamadı
* Bir IOT hub için sahip anahtarı sağlamanız gerekir. **service bağlanma** izni.

   ![IOT hub hizmeti connect izni](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Önceki görüntüde ilkelerinin ya da gösterildiği gibi **iothubowner** ve **hizmet** çalışır, her ikisi de olduğundan **service bağlanma** izni.
* Bir event hub için sahip anahtarı sağlamanız gerekir. **dinleme** izni.

   ![Olay hub'ı dinleme izni](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Önceki görüntüde ilkelerinin ya da gösterildiği gibi **okuma** ve **yönetmek** çalışır, her ikisi de olduğundan **dinleme** izni.

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a>Sağlanan bir tüketici grubundaki zaman serisi Insights'a özel değildir
IOT hub'ı veya bir olay hub'ı için kayıt sırasında verilerinizi okumak için kullanılması gereken tüketici grubu belirtmek isteriz. Bu bir tüketici grubundaki paylaşılmayan gerekir. Paylaşılıyorsa, temel olay hub'ı otomatik olarak okuyucular birini rastgele bağlantısını keser.

## <a name="i-see-my-data-but-theres-a-lag"></a>Verilerimi bakın, ancak bir gecikme yok
Neden kısmi veri içinde gördüğünüz ortamınızda nedenleri [zaman serisi Insights portalında](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>Ortamınızı azaltıldı
Azaltma sınırını ortamı SKU türüne ve kapasite göre uygulanır. Tüm olay kaynakları ortamında bu kapasite paylaşır. IOT hub'ını veya olay hub'ı için olay kaynağı veri zorlanan sınırları ötesinde itmesi olursa, azaltma ve bir gecikme bakın.

Aşağıdaki diyagramda, bir SKU S1 ve 3 kapasiteye sahip bir zaman serisi Öngörüler ortamı gösterilmektedir. Gün başına giriş 3 milyon olayları dağıtabilirsiniz.

![Ortam SKU geçerli kapasitesi](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Aşağıdaki diyagramda gösterildiği giriş oranıyla iletiler event hub'ındaki bu ortam alma varsayın:

![Bir olay hub'ına yönelik örnek giriş oranı](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Aşağıdaki çizimde gösterildiği gibi günlük giriş ~ 67,000 iletileri hızıdır. Bu oran 46 iletileri dakikada kabaca çevirir. Her olay hub'ı ileti tek bir zaman serisi Öngörüler olay düzleştirilmiş varsa, bu ortam hiçbir azaltma görür. Ardından her olay hub'ı ileti 100 zaman serisi Öngörüler olaylarına düzleştirilmiş, 4,600 olayları dakikada alınan. Dakikada 3 kapasiteye sahip bir S1 SKU ortam için giriş yalnızca 2,100 olayları (1 milyon olay günde 3 birimlerde dakikada 700 olayları = dakikada 2,100 olayları =). Bu nedenle, kısıtlama nedeniyle bir gecikme görürsünüz. 

Mantığı düzleştirme nasıl çalıştığını üst düzey anlamak için bkz [desteklenen JSON şekiller](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Önerilen adımlar
Gecikme düzeltmek için ortamınıza SKU kapasitesini artırın. Daha fazla bilgi için bkz: [zaman serisi Öngörüler ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Geçmiş verileri dağıtmaya ve yavaş giriş neden
Varolan bir olay kaynağına bağlanıyorsanız, IOT hub'ını veya olay hub'ı zaten verilerin içinde olduğunu olasıdır. Bu nedenle olay kaynağının ileti saklama döneminin başından veri çekme ortamı başlatır. 

Bu davranış varsayılan davranıştır ve değiştirilemiyor. Azaltma gerçekleştirmesine ve geçmiş verileri alma Yakala için biraz zaman alabilir.

#### <a name="recommended-steps"></a>Önerilen adımlar
Gecikme düzeltmek için aşağıdaki adımları uygulayın:
1. (Bu durumda 10) değeri izin verilen en yüksek SKU kapasitesini artırın. Kapasitesi artırılır sonra çok daha hızlı yakalama giriş işlemi başlatır. Ne kadar hızlı, kullanılabilirlik grafikte aracılığıyla yakalama görselleştirebilirsiniz [zaman serisi Insights portalında](https://insights.timeseries.azure.com). Artan kapasite için sizden ücret kesilir.
2. Gecikme yakalanan sonra SKU kapasitesi normal giriş hızınızı azaltın.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>My olay kaynağının *zaman damgası özelliği adı* ayarı çalışmıyor
Ad ve değer aşağıdaki kurallara uygun emin olun:
* Zaman damgası özelliği adı _büyük küçük harfe duyarlı_.
* Olay kaynağınız bir JSON dizesi olarak geldiği zaman damgası özelliği değeri biçiminde olması _yyyy-MM-ddTHH. FFFFFFFK_. Bu tür bir dize örneğidir "2008-04-12T12:53Z".
