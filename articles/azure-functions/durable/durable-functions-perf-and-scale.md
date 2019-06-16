---
title: Performans ve ölçek dayanıklı işlevler - Azure
description: Azure işlevleri için dayanıklı işlevler uzantısını giriş.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: azfuncdf
ms.openlocfilehash: e6ae4cc527ae0828f530ab7f3904d2b3c64c910b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60733258"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Performans ve ölçek dayanıklı işlevler (Azure işlevleri)

Performans ve ölçeklenebilirlik iyileştirmek için benzersiz ölçeklendirme özelliklerini anlamak önemlidir [dayanıklı işlevler](durable-functions-overview.md).

Ölçek davranışını anlamak için bazı temel alınan Azure depolama sağlayıcısının ayrıntılarını anlamak zorunda.

## <a name="history-table"></a>Geçmiş tablosu

**Geçmişi** bir görev hub'ındaki tüm düzenleme örnekleri için geçmiş olaylar içeren bir Azure depolama tablosuna bir tablodur. Bu tablonun adı biçimindedir *TaskHubName*geçmişi. Örneklerin gibi bu tabloya yeni satır eklenir. Bu tablonun bölüm anahtarı, orchestration örneği Kimliğinden elde edilir. Örnek kimliği iç bölümlerin Azure storage'da en iyi dağıtım sağlar, çoğu durumda, rastgele belirlenir.

Orchestration örneği çalıştırmak gerektiğinde, geçmiş tablodaki uygun satırları belleğe yüklenir. Bunlar *geçmiş olaylar* önceden belirttiğinizde, duruma geri dönmek için orchestrator işlevi koda daha sonra yeniden. Durum bu şekilde yeniden yürütme geçmişini kullanımını tarafından etkilenir [olay kaynağını belirleme düzeni](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).

## <a name="instances-table"></a>Örnek tablo

**Örnekleri** bir görev hub'ındaki tüm düzenleme örneklerinin durumları içeren başka bir Azure depolama tablosuna bir tablodur. Örneklerin oluşturulduğu gibi bu tabloya yeni satır eklenir. Orchestration örnek kimliği bu tablonun bölüm anahtarı olduğu ve satır anahtarı sabit bir sabittir. Orchestration örneği başına bir satır var.

Bu tabloda örnek sorgu istekleri karşılamak için kullanılan [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_System_String_) (.NET) ve `getStatus` (JavaScript) API'lerini hem de [durum sorgusu HTTP API](durable-functions-http-api.md#get-instance-status). İçeriğiyle birlikte sonunda tutarlı tutulur **geçmişi** tablo daha önce bahsedilen. Tarafından bu şekilde örnek sorgu işlemleri verimli bir şekilde karşılamak için ayrı bir Azure depolama tablo kullanımını etkileyen [komut ve sorgu sorumluluğu ayrımı (CQRS) düzeni](https://docs.microsoft.com/azure/architecture/patterns/cqrs).

## <a name="internal-queue-triggers"></a>İç sıra Tetikleyiciler

Orchestrator işlevler ve etkinlik işlevler hem de iç sıralarda işlevi uygulamanın görev hub tarafından tetiklenir. Bu şekilde kuyruklarını kullanarak güvenilir "en az bir kez" ileti teslimat garantileriyle sağlar. Dayanıklı işlevler kuyruklarda iki tür vardır: **denetim kuyruk** ve **iş öğesi kuyruk**.

### <a name="the-work-item-queue"></a>İş öğesi kuyruk

Dayanıklı işlevler görev hub başına bir iş öğesi kuyruk yok. Temel bir sıra ve diğer benzer şekilde davranır `queueTrigger` Azure işlevleri'nde kuyruk. Durum bilgisi olmayan tetiklemek için bu kuyruğu kullanılır *etkinlik işlevlerini* tarafından dequeueing aynı anda tek bir ileti. Her biri bu iletiler, etkinlik işlev giriş ve yürütmek için hangi işlev gibi ek meta veriler içerir. Dayanıklı işlevler uygulama için birden çok VM Ölçeklendirmesi eşitlenene, iş öğesi kuyruğu'ndan iş almak için bu VM'lerin tüm rekabet.

### <a name="control-queues"></a>Denetim kuyrukları

Vardır birden çok *denetim kuyrukları* dayanıklı işlevler görev hub başına. A *denetim kuyruk* daha basit bir iş öğesi kuyruk daha fazla karmaşık olan. Denetim kuyruklar, durum bilgisi olan orchestrator işlevleri tetiklemek için kullanılır. Orchestrator işlevi örnekleri durum bilgisi olan teklileri olduğundan, sanal makineler arasında yük dağıtmak için rakip bir tüketici modeli kullanmak mümkün değildir. Bunun yerine, orchestrator iletileri, yük dengeli arasında denetim sıralar. Sonraki bölümlerde bu davranışı hakkında daha fazla ayrıntı bulunabilir.

Denetim kuyrukları çeşitli düzenleme yaşam döngüsü ileti türlerini içerir. Örnekler [orchestrator denetim iletileri](durable-functions-instance-management.md), etkinlik işlevi *yanıt* iletileri ve Zamanlayıcı iletileri. Tek bir yoklama bir denetim kuyruktan 32 adede kadar ileti kuyruktan çıkarıldı. Bu iletiler, meta verileri için hedeflenen hangi düzenleme örneği de dahil olmak üzere yanı sıra yük verisi içerir. Dequeued birden çok ileti aynı düzenleme örneği için yönelikse, toplu iş olarak işlenir.

### <a name="queue-polling"></a>Kuyruk yoklama

Dayanıklı görev uzantısı boş kuyruk üzerinde depolama işlem maliyetleri yoklama etkisini azaltmak için bir rastgele üstel geri alma algoritması uygular. Bir ileti bulunduğunda, çalışma zamanı için başka bir ileti hemen denetler; ileti bulunduğunda, yeniden denemeden önce bir süre bekler. Bir kuyruk iletisi almak için sonraki başarısız girişimden sonra bekleme süresi, 30 saniye olarak varsayılan olarak en fazla bekleme zamanı ulaşıncaya kadar artmaya devam eder.

En yüksek yoklama gecikmesidir aracılığıyla yapılandırılabilir `maxQueuePollingInterval` özelliğinde [host.json dosya](../functions-host-json.md#durabletask). Bu daha yüksek bir değere ayarlanması daha yüksek ileti işleme gecikmeleri neden olabilir. Daha yüksek gecikme süreleri, yalnızca etkin olmadığı dönemler sonra beklenen. Bu daha düşük bir değere ayarlanması artan depolama işlemleri nedeniyle daha yüksek depolama maliyetine neden olabilir.

> [!NOTE]
> Azure işlevleri tüketim ve Premium planlardaki çalıştırırken [Azure işlevlerini ölçeklendirme denetleyicisi](../functions-scale.md#how-the-consumption-and-premium-plans-work) her denetimi ve iş öğesi kuyruk 10 saniyede yoklama yapar. Bu ek yoklama işlevi uygulama örnekleri ve ölçek kararlar için ne zaman belirlemek gereklidir. Yazma sırasında bu 10 ikinci aralığı sabittir ve yapılandırılamaz.

## <a name="storage-account-selection"></a>Depolama hesabı seçme

Kuyruklar, tablolar ve dayanıklı işlevler tarafından kullanılan BLOB'lar yapılandırılmış bir Azure depolama hesabı oluşturulur. Kullanılacak hesabı kullanılarak belirtilebilir. `durableTask/azureStorageConnectionStringName` ayarı **host.json** dosya.

### <a name="functions-1x"></a>İşlevler 1.x

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
  }
}
```

### <a name="functions-2x"></a>İşlevler 2.x

```json
{
  "extensions": {
    "durableTask": {
      "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
    }
  }
}
```

Belirtilmezse, varsayılan `AzureWebJobsStorage` depolama hesabı kullanılır. Ancak, performans açısından duyarlı iş yükleri için varsayılan olmayan depolama hesabı yapılandırma önerilir. Dayanıklı işlevler Azure depolama yoğun bir şekilde kullanır ve ayrılmış bir depolama hesabı kullanarak Azure işlevleri ana bilgisayarı tarafından iç kullanım dayanıklı işlevler depolama kullanımından yalıtır.

## <a name="orchestrator-scale-out"></a>Orchestrator ölçek genişletme

Etkinlik otomatik olarak ekleyerek durum bilgisiz ve genişletilmiş işlevlerdir. Orchestrator, diğer taraftan, işlevlerdir *bölümlenmiş* arasında bir veya daha fazla denetim sıralar. Denetim sıraların sayısı tanımlanan **host.json** dosya. Aşağıdaki örnek host.json kod parçacığı kümeleri `durableTask/partitionCount` özelliğini `3`.

### <a name="functions-1x"></a>İşlevler 1.x

```json
{
  "durableTask": {
    "partitionCount": 3
  }
}
```

### <a name="functions-2x"></a>İşlevler 2.x

```json
{
  "extensions": {
    "durableTask": {
      "partitionCount": 3
    }
  }
}
```

Bir görev hub'ı 1 ile 16 bölümler arasında yapılandırılabilir. Belirtilmezse, varsayılan bölüm sayısı olan **4**.

(Genellikle üzerinde farklı VM) birden çok işlev konak örneğe genişletme, her örnek bir denetim kuyrukların kilit alır. Bu kilitleri, blob depolama kiraları gibi dahili olarak uygulanır ve düzenleme örneği aynı anda yalnızca tek bir ana bilgisayar örneği üzerinde çalıştığından emin olun. Görev hub üç denetim kuyrukları ile yapılandırılmışsa, orchestration örnekleri üç adede kadar sanal makineler arasında Yük Dengelemesi yapılmış olabilir. Etkinlik işlevi yürütme için kapasiteyi artırmak için ek Vm'lere eklenebilir.

Aşağıdaki diyagramda, Azure işlevleri konak ölçeği genişletilmiş bir ortamda depolama varlıkları ile nasıl etkileşim kurduğu gösterilmektedir.

![Ölçek diyagramı](./media/durable-functions-perf-and-scale/scale-diagram.png)

Önceki diyagramda gösterildiği gibi tüm sanal makineler, iş öğesi kuyruk iletileri için rekabet. Ancak yalnızca üç VM iletileri denetim sıralardan elde edebilir ve her sanal makine tek bir denetim kuyruk kilitler.

Orchestration örnekleri tüm denetim kuyruk örnek arasında dağıtılır. Dağıtım, orchestration örnek kimliği karması oluşturularak gerçekleştirilir. Varsayılan örnek kimlikleri, örnekleri tüm denetim sıraları arasında eşit olarak dağıtılan sağlama, rastgele guıd'lerdir.

Genel olarak bakıldığında, orchestrator işlevleri basit olacak şekilde tasarlanmıştır ve büyük miktarda bilgi işlem gücüne yapılması gerekmez. Bu nedenle denetimi çok sayıda harika işleme almak için kuyruk bölümler oluşturmak gerekli değildir. Ağır işlerin çoğunu sonsuz Genişletilebilir durum bilgisi olmayan etkinlik işlevlerde yapılmalıdır.

## <a name="auto-scale"></a>Otomatik Ölçeklendirme

Tüm Azure tüketim planında çalıştırmayı işlevleri ile otomatik ölçeklendirme aracılığıyla dayanıklı işlevler desteklediği [Azure işlevleri ölçek denetleyicisi](../functions-scale.md#runtime-scaling). Düzenli olarak göndererek, tüm kuyrukları gecikme süresini ölçek denetleyicisi izler _gözlem_ komutları. Düşük gecikme sürelerini peeked iletilerin bağlı olarak, Ölçek denetleyicisi eklemeyi veya Vm'leri kaldırıp karar verir.

Ölçek denetleyicisi denetimi kuyruk iletisi gecikme süreleri çok yüksek olduğunu belirlerse, ileti gecikme süresi için kabul edilebilir bir düzeye düşürür ya da Denetim kuyruk bölüm sayısı ulaşana kadar sanal makine örnekleri ekleme. Benzer şekilde, iş öğesi kuyruk gecikme süreleri, yüksek bölüm sayısı bakılmaksızın ölçek denetleyiciyi sürekli olarak VM örnekleri ekleme.

## <a name="thread-usage"></a>İş parçacığı kullanımı

Orchestrator İşlevler, yürütme arasında birçok olumsuzlukları belirleyici olabilir emin olmak için tek bir iş parçacığında yürütülür. Bu tek iş parçacıklı yürütme nedeniyle orchestrator işlevi iş parçacıkları değil CPU-yoğun görevleri, g/ç yapın veya herhangi bir nedenle engelleme, önemlidir. Herhangi bir iş engelleme, g/ç gerektirebilir veya birden çok iş parçacığı etkinliği işlevlere taşınmasını gerektirir.

Etkinlik işlevlerinin, aynı normal kuyruk ile tetiklenen işlev davranışları yoktur. Bunlar güvenli bir şekilde g/ç yapın, CPU yoğunluklu işlemlerdir yürütün ve birden çok iş parçacığı kullanın. Etkinlik Tetikleyicileri durum bilgisiz olduğundan, bunlar ücretsiz olarak sınırsız sayıda sanal makineleri için ölçeği genişletebilirsiniz.

## <a name="concurrency-throttles"></a>Eşzamanlılık kısıtlamalar

Azure işlevleri, tek bir uygulama örneği içinde eşzamanlı olarak birden çok işlev yürütülürken destekler. Bu eş zamanlı yürütme paralellik artırmaya yardımcı olur ve "zaman içinde normal bir uygulama deneyimi soğuk başlar" sayısını en aza indirir. Ancak, VM başına yüksek bellek kullanımı yüksek eşzamanlılık neden olabilir. İşlev uygulaması gereksinimlerine bağlı olarak, yüksek yük durumlarda bellek çalıştıran olasılığını önlemek için örnek başına eşzamanlılık kısıtlama gerekebilir.

Her iki etkinlik işlevi ve orchestrator işlevi Eş zamanlılık limitlerine yapılandırılabilir **host.json** dosya. İlgili ayarlar `durableTask/maxConcurrentActivityFunctions` ve `durableTask/maxConcurrentOrchestratorFunctions` sırasıyla.

### <a name="functions-1x"></a>İşlevler 1.x

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10
  }
}
```

### <a name="functions-2x"></a>İşlevler 2.x

```json
{
  "extensions": {
    "durableTask": {
      "maxConcurrentActivityFunctions": 10,
      "maxConcurrentOrchestratorFunctions": 10
    }
  }
}
```

Önceki örnekte, en fazla 10 orchestrator işlevleri ve 10 etkinlik işlevlerini tek bir VM üzerinde birlikte çalışabilir. Belirtilmezse, eş zamanlı etkinliği ve orchestrator işlev yürütme sayısı, sanal çekirdek sayısı X 10 ücret alınır.

> [!NOTE]
> Bu ayarlar, bellek ve CPU kullanımını tek bir VM'de yönetmenize yardımcı olmak faydalıdır. Ancak, ölçeği birden çok VM arasında her VM sınırları kendi kümesine sahiptir. Bu ayarlar, genel düzeyde eşzamanlılık denetimi için kullanılamaz.

## <a name="orchestrator-function-replay"></a>Orchestrator işlevi yeniden yürütme

Daha önce belirtildiği gibi orchestrator işlevleri içeriğini kullanarak yeniden oynatılır **geçmişi** tablo. Toplu iletiler sıradan çıkarılan her zaman bir denetim kuyruktan varsayılan olarak, orchestrator işlev kodunu tekrarlanır.

Bu agresif yeniden yürütme davranışı etkinleştirerek devre dışı bırakılabilir **oturumları Genişletilmiş**. Genişletilmiş Oturumlar etkinleştirildiğinde, orchestrator işlevi örnekleri uzun ve yeni iletileri tam bir yeniden yürütme işlenebilir bellekte tutulur. Genişletilmiş oturumları etkin ayarlayarak `durableTask/extendedSessionsEnabled` için `true` içinde **host.json** dosya. `durableTask/extendedSessionIdleTimeoutInSeconds` Ayarı ne kadar boşta olan bir oturum bellekte tutulan denetlemek için kullanılır:

### <a name="functions-1x"></a>İşlevler 1.x

```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

### <a name="functions-2x"></a>İşlevler 2.x

```json
{
  "extensions": {
    "durableTask": {
      "extendedSessionsEnabled": true,
      "extendedSessionIdleTimeoutInSeconds": 30
    }
  }
}
```

Tipik etkisini ilave oturumları etkinleştirme azaltılmış g/ç karşı genel iyi aktarım hızı ve Azure depolama hesabı.

Ancak, bu özelliği olası bir dezavantajı örnekleri uzun bellekte kalır, boşta orchestrator işlevidir. Dikkat edilmesi gereken iki etkisi vardır:

1. Genel işlevi uygulamanın bellek kullanımı artırın.
2. Birçok eş zamanlı, kısa süreli orchestrator işlev yürütmelerini varsa genel performansı azaltır.

Örneğin, varsa `durableTask/extendedSessionIdleTimeoutInSeconds` 1 saniyeden kısa bir süre içinde yürütülen bir kısa süreli orchestrator işlevi bölüm hala 30 saniye bellek kaplayacağı sonra 30 saniye olarak ayarlanır. Ayrıca karşı sayılır `durableTask/maxConcurrentOrchestratorFunctions` daha önce bahsedilen kota olabilecek diğer orchestrator işlevleri çalışmasını önleme.

> [!NOTE]
> Bu ayarlar, yalnızca bir düzenleyici işlevi tam olarak geliştirilen ve Test kaydedildikten sonra kullanılmalıdır. Varsayılan agresif yeniden yürütme davranışını geliştirme zamanında orchestrator işlevlerde Teklik hataları algılamak için yararlı olur.

## <a name="performance-targets"></a>Performans hedefleri

Dayanıklı işlevler bir üretim uygulaması için kullanmak planlama yaparken planlama sürecinin başlarında performansı gereksinimleri dikkate almak önemlidir. Bu bölüm, bazı temel kullanım senaryoları ve beklenen en yüksek aktarım numaraları kapsar.

* **Sıralı Etkinlik yürütme**: Bu senaryoda, bir etkinlik işlevler bir dizi art arda çalıştırılan bir düzenleyici işlevi açıklanmaktadır. En çok benzeyen [işlevi zincirleme](durable-functions-sequence.md) örnek.
* **Paralel Etkinlik yürütme**: Birçok etkinlik işlevleri kullanılarak paralel yürüten bir düzenleyici işlevi bu senaryoyu açıklar [yayma, Clustered](durable-functions-cloud-backup.md) deseni.
* **Paralel işleme yanıt**: Bu senaryo, ikinci yarısında olan [yayma, Clustered](durable-functions-cloud-backup.md) deseni. Bunu Clustered performansı üzerinde odaklanır. Yayma, aksine Clustered tek orchestrator işlevi örneği tarafından yapılır ve bu nedenle yalnızca tek bir sanal makine üzerinde çalıştırılabilir dikkat edin önemlidir.
* **Dış olay işleme**: Bu senaryo, üzerinde bekleyen bir tek orchestrator işlevi örneği temsil eder [dış olayları](durable-functions-external-events.md), bir kerede.

> [!TIP]
> Yayma Clustered işlemleri için tek bir VM sınırlıdır. Uygulamanızın kullandığı yayma, Clustered desen ve Clustered performansla ilgili endişeleriniz varsa, etkinlik işlevi yayma birden çok alt bölme düşünün [alt düzenlemeleri](durable-functions-sub-orchestrations.md).

Aşağıdaki tabloda beklenen gösterilmektedir *maksimum* daha önce açıklandığı gibi senaryolar için üretilen iş sayısı. Tek bir örneğini çalıştıran tek küçük bir düzenleyici işlevi başvurduğu "Örnek" ([A1](../../virtual-machines/windows/sizes-previous-gen.md#a-series)) Azure App Service'te VM. Her durumda, varsayılır [oturumları Genişletilmiş](#orchestrator-function-replay) etkinleştirilir. Gerçek sonuçlar, işlev kodu tarafından gerçekleştirilen CPU veya g/ç iş bağlı olarak değişebilir.

| Senaryo | Aktarım hızı üst sınırı |
|-|-|
| Sıralı Etkinlik yürütme | Örnek başına saniyede 5 etkinliği |
| Paralel Etkinlik yürütme (yaygın) | Örnek başına saniyede 100 etkinliği |
| Yanıt paralel işleme (Clustered) | Örnek başına saniyede 150 yanıtları |
| Dış olay işleme | Örnek başına saniyede 50 olayları |

> [!NOTE]
> Bu dayanıklı işlevler uzantısını v1.4.0 (GA) sürümü itibarıyla geçerli sayılardır. Bu sayı, özellik geliştikçe ve en iyi duruma getirme gerçekleştirilmediğinden zaman içinde değişebilir.

Beklediğiniz üretilen iş sayısı ve CPU görmediğinizden ve bellek kullanımı sağlıklı görünür, nedeni için ilgili olup olmadığını görmek için kontrol [depolama hesabınızın durumu](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance). Dayanıklı işlevler uzantısını önemli koyabilirsiniz bir Azure depolama hesabına yükleyin ve depolama hesabı içinde azaltma yeterince yüksek yük neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [C# dilinde ilk dayanıklı işlevinizi oluşturma](durable-functions-create-first-csharp.md)
