---
title: Sürüm oluşturma dayanıklı işlevlerinde - Azure
description: Sürüm oluşturma için Azure işlevleri dayanıklı işlevleri uzantısı'nda uygulama hakkında bilgi edinin.
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
ms.openlocfilehash: 0a86e4a87f5ec23c284aa4e5cfb2c67622b3ebe9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23838558"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Dayanıklı işlevleri (Azure işlevleri) sürüm oluşturma

Bu işlevler eklenen, kaldırılacak ve bir uygulama ömrü boyunca değişti, kaçınılmazdır. [Dayanıklı işlevleri](durable-functions-overview.md) zincirleme işlevleri birlikte önceden mümkün doğru şekilde ve bu zincirleme sürüm nasıl işleneceğini etkiler sağlar.

## <a name="how-to-handle-breaking-changes"></a>Önemli değişiklikler nasıl ele alınacağını

Dikkat edilmesi gereken önemli değişiklikler, çeşitli örnekler verilmiştir. Bu makalede en yaygın olanları anlatılmaktadır. Bunların tümünün arkasında ana teması, her iki yeni ve mevcut işlevi düzenlemelerin işlevi kod değişikliklerinden etkilenen öğesidir.

### <a name="changing-activity-function-signatures"></a>Etkinlik işlevi imzalarını değiştirme

İmza değişiklik adını, giriş veya çıkış işlevinin bir değişiklik gösterir. Bir etkinlik işlevi bu tür bir değişiklik yaptıysanız, bağımlı orchestrator işlevi bozar. Bu değişikliği karşılamak için orchestrator işlevi güncelleştirirseniz, mevcut yürütülen örnekleri bozar.

Örnek olarak, aşağıdaki işlev sahibiz varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu simplistic işlev sonuçlarını alır **Foo** ve buna ileten **çubuğu**. Dönüş değerini değiştirmek ihtiyacımız varsayalım **Foo** gelen `bool` için `int` çok çeşitli sonuç değerleri desteklemek için. Sonuç şöyle görünür:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu değişiklik orchestrator işlevinin tüm yeni örnekler için düzgün çalışır ancak yürütülen tüm örneklerini keser. Örneğin, burada orchestration örneği çağırır durumu göz önünde bulundurun **Foo**, bir boolean değeri ve kontrol noktaları geri alır. İmza değişiklik bu noktada dağıtılırsa ne zaman sürdürür ve çağrısı başlayarak yeniden oynatılır hemen belirttiğinizde örneği başarısız olur `context.CallActivityAsync<int>("Foo")`. Geçmiş tablosu sonucunda olmasıdır `bool` ancak yeni kod içine seri durumdan dener `int`.

Bu yalnızca bir imza değişiklik mevcut örnekleri bölünebilir birçok farklı yolu biridir. Genel olarak, bir orchestrator şeklini değiştirmek gerekiyorsa işlevi çağırır sonra büyük olasılıkla sorunlu bir değişikliktir.

### <a name="changing-orchestrator-logic"></a>Orchestrator mantığı değiştirme

Sürüm oluşturma sorunları başka bir sınıf gelen orchestrator işlev kodu yürütülen örnekleri için yeniden yürütme mantığı kafasını karıştırabilirler şekilde değiştirme.

Aşağıdaki orchestrator işlevi göz önünde bulundurun:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Artık başka bir işlev çağrısı ekleyin zararsız görünen bir değişiklik yapmak istediğiniz varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    if (result)
    {
        await context.CallActivityAsync("SendNotification");
    }

    await context.CallActivityAsync("Bar", result);
}
```

Bu değişiklik yeni bir işlev çağrısı ekler **SendNotification** arasında **Foo** ve **çubuğu**. İmza değişiklik yoktur. Var olan bir örneği için çağrısından çıktığında sorun ortaya **çubuğu**. Özgün için çağırırsanız yeniden yürütme sırasında **Foo** döndürülen `true`, orchestrator yeniden yürütme içine çağıracak sonra **SendNotification** yürütme geçmişiyle değil. Sonuç olarak, ile dayanıklı görev Framework başarısız bir `NonDeterministicOrchestrationException` yapılan bir çağrı karşılaştığından **SendNotification** zaman beklenen bir çağrı görmek **çubuğu**.

## <a name="mitigation-strategies"></a>Hafifletme stratejileri

Sürüm oluşturma zorluklar ilgilenmek için stratejileri bazıları şunlardır:

* Hiçbir şey yapma
* Yürütülen tüm örneklerini Durdur
* Yan yana dağıtımlar

### <a name="do-nothing"></a>Hiçbir şey yapma

Önemli bir değişiklik işlemek için en kolay yolu örnekleri başarısız yürütülen orchestration kurmanızı sağlamaktır. Yeni örnekleri başarıyla değiştirilen kodu çalıştırın.

Bu bir sorun olup olmadığını yürütülen örneklerinizi önemini bağlıdır. İçinde active geliştirme ve yürütülen örnekleri hakkında önemli değil, bu kadar iyi olabilir. Ancak, özel durum ve tanılama hattınızı hataları uğraşmanız gerekir. Bunlar önlemek, bir sürüm seçenekleri göz önünde bulundurun.

### <a name="stop-all-in-flight-instances"></a>Yürütülen tüm örneklerini Durdur

Yürütülen tüm örneklerini durdurmak başka bir seçenektir. Bu içeriği iç temizleyerek yapılabilir **denetim sırası** ve **WorkItem sıra** sıralar. Örneği burada olduklarından, ancak bunlar hata iletileri ile telemetrinizi dağıtmayı değil sonsuza kadar takılmış. Bu, hızlı prototip geliştirme idealdir.

> [!WARNING]
> Bu kuyruklar ayrıntılarını üretim iş yükleri için bu tekniği güvenmeyin böylece zaman içinde değişebilir.

### <a name="side-by-side-deployments"></a>Yan yana dağıtımlar

Önemli değişiklikler güvenli bir şekilde dağıtıldığından emin olmak için en başarısız kanıt yana birimi eski sürümlerinizi ile dağıtarak yoludur. Bu yapılabilir aşağıdaki tekniklerden herhangi birini kullanarak:

* Tüm güncelleştirmeler tamamen yeni işlevler olarak dağıtma (yeni adları).
* Farklı bir depolama hesabı ile yeni bir işlev uygulaması olarak tüm güncelleştirmeleri dağıtın.
* Yeni bir kopya işlev uygulaması, ancak güncelleştirilmiş dağıtmak `TaskHub` adı. Bu önerilen bir tekniktir.

### <a name="how-to-change-task-hub-name"></a>Görev hub adını değiştirme

Görev hub yapılandırılabilir *host.json* gibi dosya:

```json
{
    "durableTask": {
        "HubName": "MyTaskHubV2"
    }
}
```

Varsayılan değer `DurableFunctionsHub`.

Tüm Azure Storage varlıklar göre adlandırılır `HubName` yapılandırma değeri. Görev hub'ı yeni bir ad verip, ayrı kuyruklar ve geçmiş tablosu, uygulamanın yeni sürümü için oluşturduğunuzdan emin olun.

İşlev uygulamasının yeni sürümünü yeni bir dağıtmanızı öneririz [dağıtım yuvası](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/). Dağıtım yuvaları, işlevi uygulama yan yana birden çok kopyasını yalnızca bunlardan birinin etkin olarak çalıştırmak izin *üretim* yuvası. Yeni orchestration mantığı mevcut altyapınıza kullanıma hazır olduğunuzda, yeni sürümü üretim yuvasına olarak değiştirme gibi basit olabilir.

> [!NOTE]
> Orchestrator işlevleri için HTTP ve Web kancası Tetikleyicileri kullandığınızda bu strateji en iyi şekilde çalışır. Kuyrukları veya Event Hubs gibi HTTP olmayan Tetikleyiciler için tetikleyici tanımı değiştirme işlemi bir parçası olarak güncelleştirilir bir uygulama ayarı öğesinden türetilen.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans işlemek ve sorunları ölçeklendirme hakkında bilgi edinin](durable-functions-perf-and-scale.md)
