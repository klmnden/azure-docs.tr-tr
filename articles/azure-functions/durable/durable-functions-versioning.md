---
title: Dayanıklı işlevler - Azure sürüm oluşturma
description: Sürüm oluşturma, Azure işlevleri için dayanıklı işlevler uzantısını uygulamayı öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: azfuncdf
ms.openlocfilehash: 33ca6c36cd11d53a3c50a8374181c511fd2f8c3e
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549093"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) sürüm oluşturma

Bu işlevler eklendi, kaldırılacak ve bir uygulamanın ömrü boyunca değiştirilmiş olduğunu kaçınılmazdır. [Dayanıklı işlevler](durable-functions-overview.md) zincirleme işlevleri birlikte daha önce mümkün olmayan yollarla ve sürüm oluşturma nasıl işleyebileceğini etkiler Bu zincir sağlar.

## <a name="how-to-handle-breaking-changes"></a>Bozucu değişiklikleri işlemek nasıl

Bozucu değişiklikleri dikkat edilmesi gereken çeşitli örneklerini vardır. Bu makalede, en yaygın olanlarından açıklanmaktadır. Ana arkasında tümünün iki yeni ve mevcut işlevi düzenlemeleri işlev kodu değişikliklerinden etkilendiğini temadır.

### <a name="changing-activity-function-signatures"></a>Değiştirmeyi aktivite işlev imzası

İmzayı Değiştir adı, giriş veya çıkış bir işlevin bir değişiklik gösterir. Bir etkinlik işlevi için bu tür değişiklik yaptıysanız, bağımlı orchestrator işlevi bozar. Bu değişikliğe uyum sağlamak için orchestrator işlevi güncelleştirirseniz, mevcut uçuşan örnekleri bozar.

Örneğin, aşağıdaki işlevi sahibiz varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu Basitleştirilmiş işlevi sonuçlarını alır **Foo** ve buna ileten **çubuğu**. Dönüş değerini değiştirmek ihtiyacımız varsayalım **Foo** gelen `bool` için `int` daha geniş bir sonuç değerleri çeşitliliğini destekleyecek şekilde. Sonucu şöyle görünür:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu değişiklik tüm yeni örneklerini orchestrator işlevi için düzgün çalışır, ancak yürütülen tüm örneklerini keser. Örneğin, burada düzenleme örneği çağrıları bir durum düşünün **Foo**, bir Boole değeri ve kontrol noktaları geri alır. İmzayı değiştir Bu noktada dağıtılırsa, ne zaman sürdürür ve çağrı başlayarak yeniden oynatılır hemen belirttiğinizde örneği başarısız olur `context.CallActivityAsync<int>("Foo")`. Geçmiş tablosu sonucu olmasıdır `bool` ancak yeni kod içine seri durumdan dener `int`.

Birçok farklı yöntem imzasını Değiştir mevcut örneklerdeki bozabilir yalnızca biri budur. Genel olarak, bir orchestrator biçimini değiştirmek gerekiyorsa bir işlev çağırır ve sonra büyük olasılıkla sorunlu bir değişikliktir.

### <a name="changing-orchestrator-logic"></a>Orchestrator mantıksal değiştirme

Bir sürüm oluşturma sorunları sınıfının orchestrator işlev kodunu uçuşan örnekleri için yeniden yürütme mantığı kafasını karıştırabilirler şekilde değiştirmesini gelir.

Aşağıdaki orchestrator işlevi göz önünde bulundurun:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Artık başka bir işlev çağrısı eklemek için görünüşte zararsız bir değişiklik yapmak istediğiniz varsayalım.

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

Bu değişiklik yeni bir işlev çağrısı için ekler **SendNotification** arasında **Foo** ve **çubuğu**. İmza bir değişiklik bulunmamaktadır. Var olan bir örneğini çağrısından çıktığında sorun ortaya çıkar **çubuğu**. Özgün çağırırsanız yeniden yürütme sırasında **Foo** döndürülen `true`, orchestrator yeniden yürütme içine çağıracak sonra **SendNotification** yürütme geçmişini değil. Sonuç olarak, ile dayanıklı görev Framework başarısız bir `NonDeterministicOrchestrationException` çağrısı karşılaştığından **SendNotification** zaman beklenen bir çağrı görmek **çubuğu**.

## <a name="mitigation-strategies"></a>Risk azaltma stratejisi

Sürüm oluşturma sorunları ilgilenmekten sorumlu stratejiler bazıları şunlardır:

* Hiçbir şey yapma
* Yürütülen tüm örnekleri Durdur
* Yan yana dağıtımlar

### <a name="do-nothing"></a>Hiçbir şey yapma

Bir değişiklik yapmanın en kolay yolu örnekleri başarısız uçuşan düzenleme sağlamaktır. Yeni örnekleri başarıyla değiştirilen kodu çalıştırın.

Bu bir sorun olup olmadığını uçuşan örneklerinizin önem derecesine bağlıdır. Etkin geliştirme aşamasındadır ve uçuşan örnekleri hakkında düşünmeniz gerekmez, bu yeterince iyi olabilir. Ancak, özel durumları ve tanılama işlem hattınızdaki hataları uğraşmanız gerekir. Bunları önlemek istiyorsanız, bir sürüm seçenekleri göz önünde bulundurun.

### <a name="stop-all-in-flight-instances"></a>Yürütülen tüm örnekleri Durdur

Yürütülen tüm örneklerinin durdurulması başka bir seçenektir. Bu iç içeriğini temizleyerek yapılabilir **denetim kuyruk** ve **iş öğesi kuyruk** sıralar. Örnekleri burada olduklarından, ancak telemetrinizi hata iletileri ile doldurmaz sonsuza kadar takılmış. Bu hızlı prototip geliştirme idealdir.

> [!WARNING]
> Bu kuyruklar ayrıntılarını, üretim iş yükleri için bu tekniği güvenmeyin böylece zaman içinde değişebilir.

### <a name="side-by-side-deployments"></a>Yan yana dağıtımlar

Bozucu değişiklikleri güvenli bir şekilde dağıtıldığından emin olmak için en başarısız kavram yan yana, önceki sürümlerle dağıtarak yoludur. Bu yapılabilir aşağıdaki tekniklerden birini kullanarak:

* Tamamen yeni işlevler olarak tüm güncelleştirmeleri dağıtma (yeni adları).
* Tüm güncelleştirmeleri, farklı bir depolama hesabı ile yeni bir işlev uygulaması olarak dağıtabilirsiniz.
* İşlev uygulamasının ancak güncelleştirilmiş ile yeni bir kopyasını dağıtma `TaskHub` adı. Önerilen yöntem budur.

### <a name="how-to-change-task-hub-name"></a>Görev hub adını değiştirme

Görev hub yapılandırılabilir *host.json* aşağıdaki gibi:

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "durableTask": {
        "HubName": "MyTaskHubV2"
    }
}
```

#### <a name="functions-2x"></a>İşlevler 2.x

Varsayılan değer `DurableFunctionsHub` şeklindedir.

Tüm Azure depolama varlıkları göre adlandırılır `HubName` yapılandırma değeri. Görev hub'ı yeni bir ad sağlayarak, ayrı sıralar ve geçmiş tablosu, uygulamanızın yeni sürümü için oluşturulduğundan emin.

İşlev uygulamasının yeni sürümünü yeni bir dağıtmanızı öneririz [dağıtım yuvası](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/). Dağıtım yuvaları ile etkin olarak bunlardan yalnızca biri, işlev uygulaması yan yana birden çok kopyasını çalıştırmanıza izin verir *üretim* yuvası. Mevcut altyapınızla yeni düzenleme mantığı kullanıma hazır olduğunuzda, yeni sürümü üretim yuvasına olarak değiştirme gibi basit olabilir.

> [!NOTE]
> Orchestrator işlevleri HTTP ve Web kancası Tetikleyicileri kullandığınızda, bu strateji en iyi şekilde çalışır. Kuyrukları veya Event Hubs gibi HTTP olmayan Tetikleyiciler için tetikleyici tanımında gereken [bir uygulama ayarı türetilen](../functions-bindings-expressions-patterns.md#binding-expressions---app-settings) değiştirme işlemi bir parçası olarak güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans işlemek ve sorunları ölçeklendirme hakkında bilgi edinin](durable-functions-perf-and-scale.md)
