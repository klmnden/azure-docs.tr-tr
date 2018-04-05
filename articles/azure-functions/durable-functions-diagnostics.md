---
title: Dayanıklı işlevlerinde - Azure tanılama
description: Azure işlevleri için dayanıklı işlevleri uzantılı sorunları tanılamak öğrenin.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: f2fc1c87a0eee9e822ffc997f67320ed23dd5916
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="diagnostics-in-durable-functions-azure-functions"></a>Tanılama dayanıklı işlevlerinde (Azure işlevleri)

İle ilgili sorunları tanılamak için birkaç seçenek vardır [dayanıklı işlevleri](durable-functions-overview.md). Bu seçeneklerden bazısı normal işlevler için aynıdır ve bazıları dayanıklı işlevleri için benzersizdir.

## <a name="application-insights"></a>Application Insights

[Application Insights](../application-insights/app-insights-overview.md) tanılama ve Azure işlevleri izleme yapmak için önerilen yoldur. Aynı dayanıklı işlevleri için geçerlidir. İşlevi uygulamanıza Application Insights yararlanmak nasıl genel bakış için bkz: [İzleyici Azure işlevleri](functions-monitoring.md).

Azure işlevleri dayanıklı uzantısı da yayar *olayları izleme* bir orchestration uçtan uca yürütmeyi izlemesini sağlar. Bu bulunabilir ve kullanarak sorgulanan [uygulama Öngörüler Analytics](../application-insights/app-insights-analytics.md) Azure portalında aracı.

### <a name="tracking-data"></a>İzleme verileri

Her yaşam döngüsü olay orchestration örneğinin yazılması bir izleme olayı neden **izlemeleri** Application Insights koleksiyonu. Bu olay içeren bir **customDimensions** çeşitli alanlarıyla yükü.  Alan adları tüm $a ile `prop__`.

* **hubName**:, düzenlemelerin çalışan görev hub'ın adı.
* **appName**: işlevi uygulamasının adı. Bu, aynı Application Insights örnek paylaşımı birden çok işlev uygulamalarının olduğunda yararlıdır.
* **slotName**: [dağıtım yuvası](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) geçerli işlev uygulaması çalıştığı içinde. Bu sürüm için dağıtım yuvaları, düzenlemelerin yararlanan durumunda faydalı olur.
* **functionName**: orchestrator veya etkinlik işlevin adı.
* **functionType**: işlevi türü gibi **Orchestrator** veya **etkinlik**.
* **InstanceId**: orchestration örneğinin benzersiz kimliği.
* **Durum**: örneğinin yaşam döngüsü yürütme durumu. Geçerli değerler şunlardır:
    * **Zamanlanmış**: işlevi çalıştırılmak üzere zamanlandı ancak henüz çalıştıran başlamadığı.
    * **Başlatılan**: işlevi çalışmaya başladı ancak henüz henüz beklemenin veya tamamlandı.
    * **Beklemenin**: orchestrator bazı iş zamanlanmış ve tamamlanmasını bekliyor.
    * **Dinleme**: orchestrator için bir dış olay bildirimi dinliyor.
    * **Tamamlanan**: işlevi başarıyla tamamlandı.
    * **Başarısız**: işlev bir hata ile başarısız oldu.
* **neden**: izleme olayla ilişkili ek veriler. Örneğin, bir dış olay bildirimi için bekliyor. bir örneği varsa, bu alan için bekleyen olay adını gösterir. Bir işlev başarısız olursa, bu hata ayrıntılarını içerir.
* **isReplay**: izleme olayı için olup olmadığını belirten Boolean değer yeniden yürütme.
* **extensionVersion**: dayanıklı görev uzantısı'nın sürümü. Bu özellikle önemli uzantısı'nda olası hataları bildirilirken verilerdir. Bir güncelleştirme çalışırken ortaya çıkarsa uzun süre çalışan örnekleri birden çok sürümü bildirebilir. 
* **sequenceNumber**: bir olay için yürütme sıra numarası. Olayları yürütme zamanına göre sıralamak için zaman damgası yardımcı birlikte. *Bu sayı her zaman zaman damgası tarafından ilk sıralama önemlidir örneği çalışırken, ana bilgisayar yeniden başlatılırsa sıfıra sıfırlama sonra sequenceNumber olacağını unutmayın.*

İzleme verilerini Application Insights'a yayılan ayrıntı yapılandırılabilir `logger` bölümünü `host.json` dosya.

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

Varsayılan olarak, tüm izleme olaylarını gösterilen. Veri birimi ayarlayarak azaltılabilir `Host.Triggers.DurableTask` için `"Warning"` veya `"Error"` ; bu durumda olayları izleme yalnızca olağanüstü durumlar için yayınlaması.

> [!WARNING]
> Varsayılan olarak, Application Insights telemetri verileri çok sık yayma önlemek için Azure işlevleri çalışma zamanı tarafından örneklenen. Bu izleme bilgilerini kısa bir süre içinde birçok yaşam döngüsü olaylar meydana geldiğinde kaybolmasına neden olabilir. [Azure işlevleri izleme makale](functions-monitoring.md#configure-sampling) bu davranışı yapılandırmak açıklanmaktadır.

### <a name="single-instance-query"></a>Tek örnek sorgu

Geçmiş izleme verilerini tek bir örneği için aşağıdaki sorguyu gösterir [Hello dizisi](durable-functions-sequence.md) işlev düzenleme. Kullanılarak yazılmış [uygulama Öngörüler sorgu dili (AIQL)](https://docs.loganalytics.io/docs/Language-Reference). Bu nedenle, yalnızca yeniden yürütme yürütme filtreleri *mantıksal* yürütme yolu gösterilir. Olayları göre sıralayarak sipariş edilen `timestamp` ve `sequenceNumber` sorguda gösterildiği gibi: 

```AIQL
let targetInstanceId = "ddd1aaa685034059b545eb004b15d4eb";
let start = datetime(2018-03-25T09:20:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend sequenceNumber = tolong(customDimensions["prop__sequenceNumber"]) 
| where isReplay == false
| where instanceId == targetInstanceId
| sort by timestamp asc, sequenceNumber asc
| project timestamp, functionName, state, instanceId, sequenceNumber, appName = cloud_RoleName
```

Artan düzende yürütme süresi göre sıralanmış tüm etkinlik işlevler de dahil olmak üzere, orchestration yürütme yolunu gösteren olayları izleme listesi sonucudur.

![Uygulama Öngörüler sorgu](media/durable-functions-diagnostics/app-insights-single-instance-ordered-query.png)


### <a name="instance-summary-query"></a>Örnek Özet sorgusu

Aşağıdaki sorgu, belirtilen zaman aralığı içinde çalıştırılan tüm orchestration örneklerinin durumunu görüntüler.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay == false
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```
Örnek kimlikleri listesini ve bunların geçerli çalışma zamanı durumu sonucudur.

![Uygulama Öngörüler sorgu](media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Günlüğe kaydetme

Orchestrator günlükleri doğrudan bir orchestrator işlevinden yazarken yeniden yürütme davranışı göz önünde bulundurmanız önemlidir. Örneğin, aşağıdaki orchestrator işlevi göz önünde bulundurun:

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```

Sonuçta elde edilen günlük verilerini aşağıdaki gibi bir şeyi aramak için geçiyor:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> F1, F2 ve F3 çağırmak günlüklerde talep olsa da, bu işlevler yalnızca olduğunu unutmayın *gerçekten* karşılaştı ilk kez çağrılır. Yeniden yürütme sırasında gerçekleşecek sonraki çağrılar atlanır ve çıkışları orchestrator mantığı yeniden oynatılır.

Yalnızca yeniden yürütme olmayan yürütülmesine oturum istiyorsanız, günlük eksikse koşullu ifade yazabilirsiniz `IsReplaying` olan `false`. Yukarıdaki örnekte, ancak bu kez yeniden yürütme denetimleri ile göz önünde bulundurun.

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    if (!ctx.IsReplaying) log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    if (!ctx.IsReplaying) log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    if (!ctx.IsReplaying) log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```
Bu değişiklikle günlük çıktı aşağıdaki gibidir:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="debugging"></a>Hata ayıklama

Hata ayıklama işlevi kodu doğrudan azure işlevleri destekler ve aynı destekleyen taşır İleri dayanıklı İşlevler, Azure'da çalışan olup olmadığını veya yerel olarak. Ancak, hata ayıklama sırasında dikkat edilmesi gereken birkaç davranışları vardır:

* **Yeniden yürütme**: Orchestrator işlevleri düzenli olarak yeniden yürütme yeni girişler alındığında. Tek bir yani *mantıksal* bir orchestrator işlevi yürütülmesi sonuçlanabilir birden çok kez aynı kesme noktası basarsa özellikle erken işlev kodu ayarlanmışsa.
* **Await**: her bir `await` olduğu karşılaştı, onu denetim dayanıklı görev Framework dağıtıcısıyla verir. Bu özellikle ilk kez ise `await` bırakıldı ilişkili görev, karşılaşılan, *hiçbir zaman* sürdürüldü. Görev hiçbir zaman sürdürür çünkü atlama *üzerinden* bekleme (Visual Studio'da F10) gerçekte mümkün değildir. Bir görev tekrarlanır zaman üzerinden atlama yalnızca çalışır.
* **Zaman aşımları Mesajlaşma**: dayanıklı işlevleri iletileri sürücü yürütmesi orchestrator işlevler ve etkinlik işlevleri için dahili olarak kullanır. Çoklu VM ortamında, uzun süre için hata ayıklama içine çiğnemekten yinelenen yürütme kaynaklanan ileti almak başka bir VM neden olabilir. Bu davranış yalnızca normal sıra tetikleyici işlevlerini de var, ancak sıraları uygulama ayrıntılarını olduğundan bu bağlamda noktası önemlidir.

> [!TIP]
> Kesme noktaları, yalnızca yeniden yürütme olmayan yürütülmesine bölün isterseniz ayarı seçtiğinizde, o sonları eksikse koşullu kesme noktası ayarlayabilirsiniz `IsReplaying` olan `false`.

## <a name="storage"></a>Depolama

Varsayılan olarak, dayanıklı işlevleri durumu Azure depolama alanında depolar. Bu gibi araçları kullanılarak, düzenlemelerin durumunu incelemek anlamına gelir [Microsoft Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Azure Depolama Gezgini ekran görüntüsü](media/durable-functions-diagnostics/storage-explorer.png)

Bu, bir düzenleme olabilir tam olarak hangi durumunu görmek için hata ayıklama için faydalıdır. Sıralarındaki iletileri de incelenmesi hangi iş bekleyen olduğunu öğrenmek için (veya bazı durumlarda takılmış).

> [!WARNING]
> Tablo depolama yürütme geçmişini görmek kullanışlı olsa da, bu tabloda herhangi bir bağımlılığı almaktan kaçının. Dayanıklı işlevleri uzantısı geliştikçe değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı zamanlayıcılar kullanmayı öğrenin](durable-functions-timers.md)