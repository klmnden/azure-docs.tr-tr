---
title: Dayanıklı işlevler - Azure tanılama
description: Azure işlevleri için dayanıklı işlevler uzantısını sorunları tanılamayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 167f697d4928d88114a30739a1d39a576c87ac84
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608495"
---
# <a name="diagnostics-in-durable-functions-in-azure"></a>Dayanıklı işlevler Azure tanılama

Sorunları tanılamak için birkaç seçenek vardır [dayanıklı işlevler](durable-functions-overview.md). Bu seçeneklerden bazıları normal işlevlerle aynıdır ve bazıları da Dayanıklı İşlevler için benzersizdir.

## <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) tanılama ve Azure işlevleri'nde İzleme yapmak için önerilen yoldur. Aynı dayanıklı işlevler için geçerlidir. İşlev uygulamanızda Application Insights kullanmayı genel bakış için bkz. [İzleyici Azure işlevleri](../functions-monitoring.md).

Azure işlevleri dayanıklı uzantısı da yayan *olayları izleme* düzenleme uçtan uca yürütmeyi izlemesini izin verir. Bunlar bulunabilir ve kullanarak sorgulanan [Application Insights Analytics](../../azure-monitor/app/analytics.md) Azure portalında aracı.

### <a name="tracking-data"></a>İzleme verileri

Orchestration örneğinin her bir yaşam döngüsü olay yazılması bir izleme olayına neden olmadan **izlemeleri** Application ınsights'ta koleksiyonu. Bu olay içeren bir **customDimensions** yüke sahip çeşitli alanları.  Alan adları tüm $a ile `prop__`.

* **hubName**: Düzenlemeleri çalıştığı görev hub adı.
* **appName**: İşlev uygulamasının adı. Aynı Application Insights örneği paylaşımı birden fazla işlev uygulamasına sahip olduğunda bu kullanışlıdır.
* **slotName**: [Dağıtım yuvası](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) çalıştığı geçerli işlev uygulaması içinde. Sürümü için dağıtım yuvaları, düzenlemeleri yararlanan bu kullanışlıdır.
* **functionName**: Orchestrator veya etkinlik işlevin adı.
* **functionType**: İşlev türü gibi **Orchestrator** veya **etkinlik**.
* **InstanceId**: Orchestration örneğinin benzersiz kimliği.
* **Durum**: Örneği yaşam döngüsü yürütme durumu. Geçerli değerler şunlardır:
  * **Zamanlanmış**: İşlev yürütme için zamanlandı ancak çalışan henüz başlamadı.
  * **Başlatılan**: İşlev çalışmaya başladı ancak henüz henüz bekleniyor veya tamamlandı.
  * **Bekleniyor**: Orchestrator, bazı işleri zamanlanmış ve tamamlanmasını bekliyor.
  * **Dinleme**: Orchestrator için bir dış olay bildirimi dinliyor.
  * **Tamamlanan**: İşlev başarıyla tamamlandı.
  * **Başarısız**: İşlev bir hata ile başarısız oldu.
* **neden**: İzleme olayı ile ilgili ek veriler. Örneğin, bir örneği için bir dış olay bildirimi bekliyorsa, bu alan için bekleyen olay adını gösterir. Bir işlev başarısız olursa, bu hata ayrıntılarını içerir.
* **isReplay**: İzleme olayı için yeniden yürütülmüş bir yürütme olup olmadığını gösteren Boole değeri.
* **extensionVersion**: Dayanıklı görev uzantısı sürümü. Bu özellikle önemli veri uzantısı'nda olası hataları raporlama olur. Bir güncelleştirme çalışırken ortaya çıkarsa, uzun süre çalışan örnekleri birden çok sürümü bildirebilir.
* **sequenceNumber**: Bir olay için yürütme sıra numarası. Birlikte olayları yürütme zamanına göre sıralamak için zaman damgası yardımcı olur. *Bu sayı örneği çalışırken, her zaman damgası tarafından sıralamanız önemlidir konak yeniden başlatılırsa sıfıra sıfırlama sonra sequenceNumber olacağını unutmayın.*

İzleme verilerini Application Insights'a yayılan ayrıntı yapılandırılabilir `logger` (1.x işlevleri) veya `logging` (2.x işlevleri) bölümünü `host.json` dosya.

#### <a name="functions-1x"></a>İşlevler 1.x

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

#### <a name="functions-2x"></a>İşlevler 2.x

```json
{
    "logging": {
        "logLevel": {
          "Host.Triggers.DurableTask": "Information",
        },
    }
}
```

Varsayılan olarak, tüm olmayan replay izleme olaylar gönderilir. Ayarlayarak veri hacmi azaltılabilir `Host.Triggers.DurableTask` için `"Warning"` veya `"Error"` durumda olayları izleme yalnızca olağanüstü durumlar için yayılan.

Ayrıntılı düzenleme yeniden yürütme olayları yayınlama etkinleştirmek için `LogReplayEvents` ayarlanabilir `true` içinde `host.json` altında dosya `durableTask` gösterildiği gibi:

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "durableTask": {
        "logReplayEvents": true
    }
}
```

#### <a name="functions-2x"></a>İşlevler 2.x

```javascript
{
    "extensions": {
        "durableTask": {
            "logReplayEvents": true
        }
    }
}
```

> [!NOTE]
> Varsayılan olarak, Application Insights telemetri verileri çok sık yayma önlemek için Azure işlevleri çalışma zamanı tarafından örneklenir. Bu izleme bilgileri kısa bir süre içinde birçok yaşam döngüsü olaylar meydana geldiğinde kaybolmasına neden olabilir. [Azure işlevleri izleme makale](../functions-monitoring.md#configure-sampling) bu davranışı yapılandırmak açıklanmaktadır.

### <a name="single-instance-query"></a>Tek örnek sorgu

Geçmiş izleme verilerini tek bir örneği için aşağıdaki sorguyu gösterir [Hello dizisi](durable-functions-sequence.md) işlev düzenleme. Kullanılarak yazılmış [Application Insights sorgu dili (AIQL)](https://aka.ms/LogAnalyticsLanguageReference). Yalnızca yeniden yürütme yürütme filtreler *mantıksal* yürütme yolu gösterilir. Olayları göre sıralayarak sıralanabileceği `timestamp` ve `sequenceNumber` sorguda gösterildiği gibi:

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
| where isReplay != true
| where instanceId == targetInstanceId
| sort by timestamp asc, sequenceNumber asc
| project timestamp, functionName, state, instanceId, sequenceNumber, appName = cloud_RoleName
```

Artan düzende yürütme zamanına göre sıralanmış tüm etkinlik işlevler dahil olmak üzere, orchestration yürütme yolunu gösteren olayları izleme listesi sonucudur.

![Application Insights sorgu](./media/durable-functions-diagnostics/app-insights-single-instance-ordered-query.png)

### <a name="instance-summary-query"></a>Özet sorgu örneği

Aşağıdaki sorgu, belirtilen zaman aralığında çalıştırılan tüm düzenleme örneklerinin durumunu görüntüler.

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
| where isReplay != true
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```

Örnek kimlikleri listesini ve geçerli çalışma zamanı durumlarını sonucudur.

![Application Insights sorgu](./media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Günlüğe kaydetme

Orchestrator günlükleri doğrudan bir orchestrator işlevden yazarken yeniden yürütme davranışını akılda tutulması önemlidir. Örneğin, aşağıdaki orchestrator işlevi göz önünde bulundurun:

### <a name="c"></a>C#

```cs
public static async Task Run(
    DurableOrchestrationContext context,
    ILogger log)
{
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    context.log("Calling F1.");
    yield context.df.callActivity("F1");
    context.log("Calling F2.");
    yield context.df.callActivity("F2");
    context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Sonuçta elde edilen günlük verilerini aşağıdaki gibi görünmesini geçiyor:

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
> F1, F2 ve F3 çağırmak için günlüklerde talep olsa da bu işlevleri yalnızca unutmayın *gerçekten* karşılaşılan ilk kez çağrılır. Yeniden yürütme sırasında gerçekleşen sonraki çağrılar atlanır ve çıkışları orchestrator mantığını yeniden yürütülmesi.

Yeniden yürütme olmayan yürütülmesine yalnızca günlüğe kaydetmek istediğiniz, yalnızca şu durumlarda günlüğü için bir koşullu ifade yazabilirsiniz `IsReplaying` olduğu `false`. Yukarıdaki örnekte, ancak bu sefer yeniden yürütme denetimleri göz önünde bulundurun.

#### <a name="c"></a>C#

```cs
public static async Task Run(
    DurableOrchestrationContext context,
    ILogger log)
{
    if (!context.IsReplaying) log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    if (!context.IsReplaying) log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    if (!context.IsReplaying) log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    if (!context.df.isReplaying) context.log("Calling F1.");
    yield context.df.callActivity("F1");
    if (!context.df.isReplaying) context.log("Calling F2.");
    yield context.df.callActivity("F2");
    if (!context.df.isReplaying) context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Bu değişiklik, günlük çıktısı aşağıdaki gibidir:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="custom-status"></a>Özel durumu

Özel düzenleme durumu orchestrator işleviniz için bir özel durum değeri ayarlamanıza olanak tanır. Bu durum HTTP durum sorgusu API sağlanan veya `DurableOrchestrationClient.GetStatusAsync` API. Özel düzenleme durumu orchestrator işlevleri için daha zengin izleme sağlar. Örneğin, orchestrator işlev kodu içerebilir `DurableOrchestrationContext.SetCustomStatus` uzun süre çalışan işlemin ilerleme durumunu güncelleştirmek için çağırır. Bir web sayfası veya diğer dış sistem gibi bir istemci, HTTP durum sorgusu API'leri daha zengin ilerleme bilgisi için düzenli aralıklarla sorgulayabilir. Bir örnek kullanarak `DurableOrchestrationContext.SetCustomStatus` aşağıda verilmiştir:

### <a name="c"></a>C#

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext context)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { completionPercentage = 90.0, status = "Updating database records" };
    context.SetCustomStatus(customStatus);

    // ...do more work...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    const customStatus = { completionPercentage: 90.0, status: "Updating database records", };
    context.df.setCustomStatus(customStatus);

    // ...do more work...
});
```

Düzenleme devam ederken, dış istemcilere bu özel durum getirebilirsiniz:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

İstemcileri şu yanıtı alırsınız:

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "completionPercentage": 90.0, "status": "Updating database records" },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
> Bir Azure tablo depolama sütuna sığamayacak kadar olması gerektiğinden, özel durum yükü 16 KB olarak UTF-16 JSON metnini sınırlıdır. Daha büyük yükü gerekiyorsa, dış depolama kullanabilirsiniz.

## <a name="debugging"></a>Hata ayıklama

İşlev kodu doğrudan hata ayıklama azure işlevleri destekler ve aynı destekleyen taşır İleri dayanıklı işlevler için yerel olarak mı Azure'da çalışan bakılmaksızın. Ancak, hata ayıklama sırasında dikkat edilmesi gereken bazı davranışları vardır:

* **Yeniden yürütme**: Yeni girişleri alındığında orchestrator işlevleri düzenli olarak yeniden yürütün. Tek bir başka bir deyişle *mantıksal* orchestrator işlevin yürütülmesini neden birden çok kez aynı kesme noktasına ulaşma özellikle erken işlev kodu ayarlanmışsa olur.
* **Await**: Her bir `await` olan karşılaştı, bu denetim dayanıklı görev Framework dağıtıcısıyla verir. Bu özellikle ilk kez ise `await` olmuştur karşılaştı, ilişkili bir görevdir *hiçbir zaman* sürdürüldü. Görev hiçbir zaman sürdürür olduğundan, Adımlama *üzerinden* await (Visual Studio'da F10) gerçekte mümkün değildir. Bir görevi yeniden üzerinden Adımlama yalnızca olduğunda çalışır.
* **Zaman aşımları Mesajlaşma**: Dayanıklı İşlevler, kuyruk iletileri sürücü yürütülmesini orchestrator işlevler hem etkinlik işlevleri için dahili olarak kullanır. Bir çoklu VM ortamında, uzun süre için hata ayıklama içine bozucu yinelenen yürütülmesine neden, ileti almak başka bir VM neden olabilir. Bu davranış yalnızca normal kuyruk tetikleyicisi işlevlerini de var, ancak bir uygulama ayrıntısı kuyrukları olduğundan bu bağlamda tablonuzu önemlidir.

> [!TIP]
> Kesme noktaları, yalnızca yeniden yürütme olmayan yürütülmesine kesmek istiyorsanız ayarlarken, bu sonu yalnızca şu durumlarda bir koşullu kesme noktası ayarlayabilirsiniz. `IsReplaying` olduğu `false`.

## <a name="storage"></a>Depolama

Varsayılan olarak, dayanıklı işlevler durumu Azure Depolama'da depolar. Bu gibi araçları kullanarak, düzenlemeleri durumunu incelemek anlamına gelir [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Azure Depolama Gezgini ekran görüntüsü](./media/durable-functions-diagnostics/storage-explorer.png)

Bu, bir düzenleme olabilir tam olarak hangi durumu görmek için hata ayıklama için kullanışlıdır. Sıralarındaki iletileri de incelenebilir hangi iş Beklemede öğrenin (veya bazı durumlarda takılı).

> [!WARNING]
> Tablo depolama yürütme geçmişini görmek kullanışlı olsa da, bu tabloyu temel bağımlılığın alma kaçının. Dayanıklı işlevler uzantısını geliştikçe değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı zamanlayıcılar kullanmayı öğrenin](durable-functions-timers.md)
