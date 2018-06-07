---
title: Performansı ve ölçeği dayanıklı işlevlerinde - Azure
description: Azure işlevleri dayanıklı işlevleri uzantısı için giriş.
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
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 110f393e723c7e784a4bd7e79559dd9d55147140
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599441"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Performansı ve ölçeği dayanıklı işlevlerinde (Azure işlevleri)

Performans ve ölçeklenebilirlik iyileştirmek için benzersiz ölçeklendirme özelliklerini anlamak önemlidir [dayanıklı işlevleri](durable-functions-overview.md).

Ölçek davranışlarını anlamak için bazı temel Azure depolama sağlayıcısı ayrıntılarını anlamak zorunda.

## <a name="history-table"></a>Geçmiş tablosu

**Geçmişi** tablodur görev hub içindeki tüm orchestration örnekleri için geçmiş olaylarını içeren bir Azure Storage tablo. Bu tablo adı biçimindedir *TaskHubName*geçmişi. Örnekleri çalıştırmak gibi yeni satırlar bu tabloya eklenir. Bu tablonun bölüm anahtarı orchestration örneği Kimliğinden türetilir. Bir örnek kimliği, Azure Storage iç bölüm en iyi dağıtım sağlar çoğu durumda, rastgele bir seçimdir.

Orchestration örneği gerektiğinde, geçmiş tablosu uygun satırlarını belleğe yüklenir. Bunlar *geçmiş olaylarını* önceden belirttiğinizde durumuna geri dönmek için orchestrator işlevi koda daha sonra yeniden. Durum bu şekilde yeniden yürütme geçmişini kullanımını tarafından etkilenir [olay kaynak Hizmeti'nden düzeni](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing).

## <a name="instances-table"></a>Örnek tablo

**Örnekleri** tablodur görev hub içindeki tüm orchestration örnekleri durumları içeren başka bir Azure Storage tablo. Örnekleri oluşturuldukça yeni satırlar bu tabloya eklenir. Bu tablonun bölüm anahtarı orchestration örnek kimliği ve satır anahtarını sabit sabiti. Orchestration örneği başına bir satır yok.

Bu tablo gelen örnek sorgu isteklerini karşılamak için kullanılan [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_System_String_) API yanı sıra [durum sorgusu HTTP API](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-http-api#get-instance-status). Sonuçta tutarlı içeriğini tutulur **geçmişi** tablo daha önce bahsedilen. Bu şekilde örnek sorgu işlemleri verimli bir şekilde karşılamak için ayrı bir Azure Storage tablo kullanımını tarafından etkilenir [komut ve sorgu sorumluluk ayrımı (CQRS) deseni](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs).

## <a name="internal-queue-triggers"></a>İç queue Tetikleyicileri

Orchestrator işlevler ve etkinlik işlevleri hem de iç sıralarda işlevi uygulamanın görev hub tarafından tetiklenir. Bu şekilde sıralar kullanarak güvenilir "en az bir kere" ileti teslimat garantileriyle sağlar. Dayanıklı işlevleri kuyruklarda iki tür vardır: **denetim sırası** ve **çalışma öğesi kuyruk**.

### <a name="the-work-item-queue"></a>İş öğesi sırası

Bir iş öğesi kuyruk dayanıklı işlevleri görev hub başına yoktur. Temel bir sıra ve diğer benzer şekilde davranır `queueTrigger` Azure işlevlerinde sırası. Bu sıra durum bilgisiz tetiklemek için kullanılan *etkinlik işlevleri* dequeueing aynı anda tek bir ileti tarafından. Bu iletiler her etkinlik işlevi girişleri ve yürütmek için hangi işlevi gibi ek meta veriler içerir. Ne zaman dayanıklı işlevleri uygulama çıkışı birden çok VM, tüm iş öğesi sırasından iş edinmeye rekabet bu VM'ler için ölçeklendirir.

### <a name="control-queues"></a>Denetim kuyrukların

Vardır birden çok *kontrol sıraları* dayanıklı işlevleri görev hub başına. A *denetim sırası* daha basit çalışma öğesi kuyruk daha karmaşık değil. Denetim kuyruklar, durum bilgisi olan orchestrator işlevleri tetiklemek için kullanılır. Orchestrator işlevi örnekleri durum bilgisi olan teklileri olduğundan, VM'ler üzerindeki yük dağıtmak için bir rakip tüketici modeli kullanmak mümkün değil. Bunun yerine, yük dengeli denetim kuyrukta orchestrator iletileri. Sonraki bölümlerde bu davranış hakkında daha fazla ayrıntı bulunabilir.

Denetim kuyruklar çeşitli orchestration yaşam döngüsü ileti türlerini içerir. Örnekler [orchestrator denetim iletileri](durable-functions-instance-management.md), etkinlik işlevi *yanıt* iletileri ve Zamanlayıcı iletileri. Tek bir yoklama bir denetim kuyruktan olarak en fazla 32 iletileri kuyruktan çıkarıldı. Bu iletiler, yükü verileri için hazırlanmıştır hangi orchestration örneği dahil meta verileri içerir. Birden çok dequeued iletiler için aynı orchestration örneği yönelikse, toplu iş olarak işlenir.

## <a name="storage-account-selection"></a>Depolama hesabı seçimi

Kuyruklar, tablolar ve dayanıklı işlevler tarafından kullanılan BLOB'lar tarafından yapılandırılan Azure depolama hesabı oluşturulur. Kullanılacak hesabı kullanılarak belirtilebilir `durableTask/azureStorageConnectionStringName` ayarı **host.json** dosya.

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
  }
}
```

Belirtilmezse, varsayılan `AzureWebJobsStorage` depolama hesabı kullanılır. Performans duyarlı iş yükleri için Bununla birlikte, varsayılan olmayan depolama hesabı yapılandırma tavsiye edilir. Dayanıklı işlevleri Azure Storage yoğun bir şekilde kullanır ve bir adanmış depolama hesabı kullanarak dayanıklı işlevleri depolama alanı kullanımı Azure işlevleri ana bilgisayarı tarafından iç kullanımdan yalıtır.

## <a name="orchestrator-scale-out"></a>Orchestrator genişletme

Etkinlik otomatik olarak VM'ler ekleyerek durum bilgisiz ve genişletilmiş işlevlerdir. Orchestrator, diğer yandan işlevlerdir *bölümlenmiş* arasında bir veya daha fazla denetim sıralar. Denetim sıraların sayısı tanımlanan **host.json** dosya. Aşağıdaki örnek host.json parçacığı kümeleri `durableTask/partitionCount` özelliğine `3`.

```json
{
  "durableTask": {
    "partitionCount": 3
  }
}
```
Bir görev hub'ı bölüm 1 ile 16 arasında yapılandırılabilir. Belirtilmezse, varsayılan bölüm sayısı olan **4**.

(Genellikle üzerinde farklı VM'ler) birden çok işlev konak örneklerine ölçeğini, her bir örnek bir kilit denetim sıraları birinde alır. Bu kilit depolama kiraları blob gibi dahili olarak uygulanır ve orchestration örneği aynı anda yalnızca bir tek ana bilgisayar örneği üzerinde çalışır emin olun. Bir görev hub üç denetim kuyruklarla yapılandırdıysanız, orchestration örnekleri olarak en fazla üç VM'ler üzerindeki yük dengeli olabilir. Etkinlik işlevi yürütme kapasiteyi artırmak için ek sanal makineleri eklenebilir.

Aşağıdaki diyagramda, Azure işlevleri konak genişletilmiş bir ortamda depolama varlıkları ile nasıl etkileşim kurduğu gösterilmektedir.

![Ölçek diyagramı](media/durable-functions-perf-and-scale/scale-diagram.png)

Önceki diyagramda gösterildiği gibi tüm VM'ler iletileri iş öğesi sırasına rekabet. Ancak, yalnızca üç VM'ler denetim sıralardan iletileri elde edebilir ve tek bir denetim sırası her VM kilitler.

Orchestration örnekleri tüm denetim sıra örnekleri arasında dağıtılır. Dağıtım orchestration örnek kimliği karma tarafından gerçekleştirilir. Varsayılan örneği kimlikleri, örnekleri tüm denetim sıraları arasında eşit olarak dağıtılır sağlayarak, rastgele guıd'lerdir.

Genel olarak bakıldığında, orchestrator işlevleri basit olacak şekilde tasarlanmıştır ve büyük miktarda bilgi işlem gücü gerekmez. Bu nedenle sıra bölümleri büyük verimi almak için çok sayıda denetimi oluşturmak gerekli değildir. Ağır iş çoğunu sonsuz Genişletilebilir durum bilgisiz etkinlik işlevlerde yapılmalıdır.

## <a name="auto-scale"></a>Otomatik Ölçeklendirme

Tüm Azure tüketim planında çalıştıran işlevleri ile dayanıklı işlevler aracılığıyla otomatik ölçek desteklediği [Azure işlevleri ölçek denetleyicisi](functions-scale.md#runtime-scaling). Düzenli aralıklarla vererek tüm sıraları gecikme ölçek denetleyicisi izler _gözlem_ komutları. Peeked iletileri gecikmeleri üzerinde bağlı olarak, ölçeği denetleyicisi VM'ler ekleyip karar verir.

Ölçek denetleyicisi denetim kuyruk iletisi gecikmeleri çok yüksek olduğunu belirlerse, bu VM örnekleri için kabul edilebilir bir düzeye ileti gecikmesini azaltır ya da Denetim sıra bölüm sayısı ulaşana kadar ekler. Benzer şekilde, iş öğesi kuyruk gecikme olması durumunda yüksek, bölüm sayısı bakılmaksızın ölçek denetleyicisi sürekli olarak VM örnekleri ekleyin.

## <a name="thread-usage"></a>İş parçacığı kullanımı

Orchestrator işlevleri yürütme arasında birçok yürütmelerini belirleyici olabilir emin olmak için tek bir iş parçacığı üzerinde yürütülür. Bu tek iş parçacıklı yürütme nedeniyle orchestrator işlevi iş parçacığı değil CPU-yoğun görevleri, g/ç yapın veya herhangi bir nedenle engelleme olduğunu önemlidir. Herhangi bir iş engellemek, g/ç gerektirebilir veya birden çok iş parçacığı etkinliği işlevlerini taşınması gereken.

Etkinlik işlevleri hepsi aynı davranışları normal sıra tetiklemeli işlevleri olarak var. Bunlar güvenli bir şekilde g/ç yapmak için CPU yoğunluklu işlemlerini yürütmek ve birden çok iş parçacığı kullanın. Etkinlik Tetikleyicileri durum bilgisiz olduğundan, bunlar serbestçe çıkışı VM'ler sınırsız bir sayıya ölçeklendirebilirsiniz.

## <a name="concurrency-throttles"></a>Eşzamanlılık kısıtlar

Azure işlevlerini destekleyen bir tek uygulama örneği içinde eşzamanlı olarak birden çok işlevleri yürütme. Bu eş zamanlı yürütme paralellik artırmaya yardımcı olur ve "Normal uygulama zamanla yaşar soğuk başlatır" sayısını en aza indirir. Ancak, yüksek eşzamanlılık VM başına yüksek bellek kullanımında neden olabilir. İşlev uygulaması gereksinimlerine bağlı olarak yüksek yük durumlarda belleğin tükenmesinden olasılığını önlemek için örnek başına eşzamanlılık kısıtlama gerekebilir.

Her iki etkinlik işlevi ve orchestrator işlevi eşzamanlılık sınırları içinde yapılandırılabilir **host.json** dosya. İlgili ayarlar `durableTask/maxConcurrentActivityFunctions` ve `durableTask/maxConcurrentOrchestratorFunctions` sırasıyla.

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
  }
}
```

Önceki örnekte, en fazla 10 orchestrator işlevleri ve 10 etkinlik işlevleri tek bir VM üzerinde çalışabilir. Belirtilmezse, eş zamanlı etkinlik ve orchestrator işlevi yürütmeleri sayısı 10 VM'de çekirdek sayısı X tutulabilir.

> [!NOTE]
> Bu ayarlar, bellek ve CPU kullanımını tek bir VM'de yönetmenize yardımcı olmak faydalıdır. Ancak, çıkışı arasında birden çok VM ölçeği, her bir VM sınırları kendi kümesi kümesine sahip. Bu ayarlar, genel düzeyde eşzamanlılık denetlemek için kullanılamaz.

## <a name="orchestrator-function-replay"></a>Orchestrator işlevi yeniden yürütme
Daha önce belirtildiği gibi orchestrator işlevleri içeriğini kullanarak yeniden oynatılır **geçmişi** tablo. Toplu iletiler kuyruktan çıkarıldı her zaman bir denetim kuyruktan varsayılan olarak, orchestrator işlev kodu tekrarlanır.

Bu agresif yeniden yürütme davranışı etkinleştirerek devre dışı bırakılabilir **oturumları Genişletilmiş**. Genişletilmiş oturumları etkinleştirildiğinde, orchestrator işlevi örnekleri uzun ve yeni iletileri tam bir yeniden yürütme işlenebilir bellekte tutulur. Genişletilmiş oturumları ayarlayarak etkin `durableTask/extendedSessionsEnabled` için `true` içinde **host.json** dosya. `durableTask/extendedSessionIdleTimeoutInSeconds` Ayarı ne kadar süreyle boşta oturum bellekte tutulan denetlemek için kullanılır:

```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

Genişletilmiş oturumları etkinleştirme tipik etkisini azaltılmış g/ç Azure depolama hesabı ve genel geliştirilmiş üretilen işi karşı.

Ancak, bu özellik bir olası dezavantajı örnekleri uzun bellekte kalır, boşta orchestrator işlevdir. Dikkat edilmesi gereken iki etkileri vardır:

1. Genel artış işlevi uygulama bellek kullanımı.
2. Çok sayıda eş zamanlı, kısa süreli orchestrator işlevi yürütmeleri varsa genel performansı azaltın.

Örneğin, varsa `durableTask/extendedSessionIdleTimeoutInSeconds` 1 saniyeden az yürüten bir kısa süreli orchestrator işlevi bölüm 30 saniye hala bellek kaplar sonra 30 saniye olarak ayarlanır. Ayrıca karşı sayılacaktır `durableTask/maxConcurrentOrchestratorFunctions` daha önce bahsedilen kota büyük olasılıkla diğer orchestrator işlevleri çalışmasını engelleme.

> [!NOTE]
> Bir orchestrator işlevi tam olarak geliştirilen ve Test kaydedildikten sonra bu ayarlar yalnızca kullanılmalıdır. Varsayılan agresif yeniden yürütme davranışı benzersizlik hataları orchestrator işlevlerde geliştirme anında algılamak için yararlıdır.

## <a name="performance-targets"></a>Performans hedefleri

Bir üretim uygulaması için sürekli işlevleri kullanmak planlama yaparken Planlama işlemi performans gereksinimlerini dikkate almak önemlidir. Bu bölüm, bazı temel kullanım senaryoları ve beklenen en yüksek verimlilik sayıları kapsar.

* **Sıralı Etkinlik yürütme**: Bu senaryo, bir dizi etkinlik işlevleri bir art arda çalıştırır bir orchestrator işlevi açıklar. En çok benzeyen [işlevi zincirleme](durable-functions-sequence.md) örnek.
* **Paralel Etkinlik yürütme**: Bu senaryo, birçok etkinlik işlevleri kullanılarak paralel yürüten bir orchestrator işlevi açıklar [yayma, Fan-in](durable-functions-cloud-backup.md) düzeni.
* **Paralel yanıt işleme**: Bu senaryo, ikinci yarısıdır [yayma, Fan-in](durable-functions-cloud-backup.md) düzeni. Fan-in performansı üzerinde odaklanmıştır. Yayma, aksine fan-in tek orchestrator işlevi örneği tarafından yapılır ve bu nedenle yalnızca tek bir VM üzerinde çalışan olduğunu dikkate almak önemlidir.
* **Dış olay işleme**: Bu senaryo bekler üzerinde bir tek orchestrator işlevi örneğini gösteren [dış olayları](durable-functions-external-events.md), bir seferde bir.

> [!TIP]
> Yayma fan-in işlemleri için tek bir VM'ye sınırlıdır. Uygulamanızı yayma, fan-in deseni kullanır ve fan-in performansı hakkında endişeleriniz varsa, etkinlik işlevi yayma birden çok alt bölme düşünün [alt düzenlemelerin](durable-functions-sub-orchestrations.md).

Aşağıdaki tabloda beklenen gösterilmektedir *maksimum* daha önce açıklanan senaryoları için işleme numaraları. Tek bir küçük üzerinde çalışan bir orchestrator işlevi tek bir örneği için "Örnek" başvuruyor ([A1](../virtual-machines/windows/sizes-previous-gen.md#a-series)) Azure App Service'te VM. Tüm durumlarda varsayılır [oturumları Genişletilmiş](#orchestrator-function-replay) etkinleştirilir. Gerçek sonuçları işlev kodu tarafından gerçekleştirilen CPU veya g/ç iş bağlı olarak değişebilir.

| Senaryo | En yüksek verimlilik |
|-|-|
| Sıralı Etkinlik yürütme | örneği başına saniyede 5 etkinlikleri |
| Paralel Etkinlik yürütme (yayma) | örneği başına saniyede 100 etkinlikleri |
| Paralel yanıt işleme (fan-in) | örneği başına saniyede 150 yanıtları |
| Dış olay işleme | örneği başına saniyede 50 olayları |

> [!NOTE]
> Bu sayı sürekli işlevleri uzantısı v1.4.0 (GA) güncelleştirmesinden itibaren geçerli. Bu sayı özelliği geliştikçe ve en iyi duruma getirme gerçekleştirilmediğinden zaman içinde değişebilir.

Neden için ilgili olup olmadığını görmek için beklediğiniz verimlilik numaraları ve, CPU görmediğinizden ve bellek kullanımı görünür sağlıklı olmadığını denetleyin [depolama hesabınızın durumu](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance). Bir Azure depolama hesabında dayanıklı uzantısı önemli koyabilirsiniz işlevleri yük ve depolama hesabı azaltma içinde yeterince yüksek yükleri neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevleri uzantısı ve örnekleri yükleyin](durable-functions-install.md)
