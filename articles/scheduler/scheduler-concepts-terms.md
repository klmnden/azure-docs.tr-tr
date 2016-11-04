---
title: Scheduler kavramları, terimleri ve varlıkları | Microsoft Docs
description: İşler ve iş koleksiyonları dahil Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi.  Zamanlanan bir işin kapsamlı bir örneği gösterilmektedir.
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''

ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli

---
# Scheduler kavramları ve terminolojisi + varlık hiyerarşisi
## Scheduler varlık hiyerarşisi
Aşağıdaki tabloda Scheduler API’si tarafından gösterilen ya da kullanılan ana kaynaklar açıklanır:

| Kaynak | Açıklama |
| --- | --- |
| **İş koleksiyonu** |İş koleksiyonu, bir grup işi içerir ve koleksiyondaki işler tarafından paylaşılan ayarlar, kotalar ve kısıtlamaları tutar. İş koleksiyonu abonelik sahibi tarafından oluşturulur ve işleri kullanım veya uygulama sınırlarına göre gruplandırır. Tek bir bölge için kısıtlanmıştır. Ayrıca, bu koleksiyondaki tüm işlerin kullanımını kısıtlamak için kotaları da uygulamayı sağlar. Kotalar MaxJobs ve MaxRecurrence değerlerini içerir. |
| **İş** |Bir iş, yürütmek üzere basit ya da karmaşık stratejilerle tek bir yinelenen eylemi tanımlar. Eylemler HTTP, depolama kuyruğu, hizmet veri yolu kuyruğu ya da hizmet veri yolu konusu isteklerini içerebilir. |
| **İş geçmişi** |İş geçmişi bir işin yürütülmesine ilişkin ayrıntılarını temsil eder. Bu başarılı ve hatalı karşılaştırmasının yanı sıra tüm yanıt ayrıntılarını içerir. |

## Scheduler varlık yönetimi
Yüksek düzeyde, scheduler ve hizmet yönetimi API’si kaynaklarda aşağıdaki işlemleri kullanır:

| Özellik | Açıklama ve URI adresi |
| --- | --- |
| **İş koleksiyonu yönetimi** |İş koleksiyonları ve burada yer alan işleri oluşturma ve değiştirmeye yönelik AL, KOY ve SİL desteği. İş koleksiyonu, kotalar ve paylaşılan ayarlara yönelik işler ve eşlemeler için kapsayıcıdır. İleride açıklanan kota örnekleri, maksimum iş sayısı ve en küçük yineleme aralığıdır. <p>KOY ve SİL: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>AL: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p> |
| **İş yönetimi** |İşleri oluşturma ve değiştirmeye yönelik AL, KOY, YAYIMLA, DÜZELTME EKİ UYGULA ve SİL desteği. Açık oluşturma olmaması için, tüm işler önceden mevcut bir iş koleksiyonuna ait olmalıdır. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| **İş geçmişi yönetimi** |İş geçen süresi ve iş yürütme sonuçları gibi, 60 günlük iş yürütme geçmişi getirmek üzere AL desteği. Durum ve duruma göre filtreleme için sorgu dizesi parametresi desteği ekler. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## İş türleri
Birden çok iş türü vardır: HTTP işleri (SSL destekleyen HTTPS işleri dahil), depolama kuyruğu işleri, hizmet veri yolu kuyruğu işleri ve hizmet veri yolu konusu işleri. HTTP işleri, mevcut bir iş yükünde ya da hizmette uç noktaya sahipseniz idealdir. Bu işleri depolama kuyrukları kullanan iş yükleri için ideal olacağından, depolama kuyruğu işlerini depolama kuyruklarında ileti yayımlamakta kullanabilirsiniz. Benzer şekilde, hizmet veri yolu işleri hizmet veri yolu kuyrukları ve konuları kullanan iş yükleri için idealdir.

## Ayrıntılı "job" varlığı
Temel düzeyde, zamanlanan bir iş birkaç bölümden oluşur:

* İş zamanlayıcı başlatıldığında gerçekleştirilecek iş  
* (İsteğe bağlı) İşin çalıştırma süresi  
* (İsteğe bağlı) İşin ne zaman ve ne sıklıkta tekrarlanacağı  
* (İsteğe bağlı) Birincil eylem başarısız olursa tetiklenecek eylem  

Dahili olarak, zamanlanan bir iş ayrıca sonraki zamanlanan yürütme zamanı gibi sistem tarafından sağlanan verileri de içerir.

Aşağıdaki kodda zamanlanan bir işin kapsamlı örneği gösterilmektedir. Ayrıntılar sonraki bölümlerde verilmiştir.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Yukarıdaki örnek zamanlanan işte görülüğü gibi, bir iş tanımı birkaç bölümden oluşur:

* Başlangıç zamanı ("startTime")  
* Hata eylemi ("errorAction") içeren eylem ("eylem")
* Yineleme ("recurrence")  
* Durum ("state")  
* Durum ("status")  
* Yeniden deneme ilkesi ("retryPolicy")  

Şimdi bunların her birini ayrıntılı inceleyelim:

## startTime
"startTime" başlangıç zamanıdır ve arayanın hat üzerinde [ISO 8601 biçiminde](http://en.wikipedia.org/wiki/ISO_8601) bir saat dilimi farkı belirtmesini sağlar.

## action ve errorAction
“action” her meydana gelişte çağrılan eylemdir ve hizmet başlatma türünü açıklar. Eylem, sağlanan zamanlamadan yürütülecek olan şeydir. Scheduler, HTTP, depolama kuyruğu, hizmet veri yolu konusu ve hizmet veri yolu kuyruğu eylemlerini destekler.

Yukarıdaki örnekteki eylem bir HTTP eylemidir. Bir depolama kuyruğu eylemi örneği aşağıdadır:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Bir hizmet veri yolu konusu eylemi örneği aşağıdadır:

  "action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }

Bir hizmet veri yolu kuyruğu eylemi örneği aşağıdadır:

  "action": { "serviceBusQueueMessage": { "queueName": "q1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // netMessaging ya da AMQP olabilir "authentication": {  
        "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",  
      "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }

"errorAction", birincil eylem başarısız olduğunda çağrılan hata işleyicidir. Bu değişkeni bir hata işleme uç noktasını çağırmak ya da kullanıcı bildirimi göndermek için kullanabilirsiniz. Bu, birincilin kullanılabilir olmaması durumunda (örneğin, uç nokta sitesinde olağanüstü durum halinde) ikincil uç noktaya erişim için veya bir hata işleme uç noktasını bildirmek için kullanılabilir. Birincil eylemde olduğu gibi, hata eylemi diğer eylemler temelinde basit ya da birleşik mantıklı olabilir. SAS belirteci oluşturmayı öğrenmek için bkz. [Paylaşılan Erişim İmzası Oluşturma ve Kullanma](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## yineleme
Yineleme birkaç bölümden oluşur:

* Sıklık: Dakika, saat, gün, hafta, ay, yıldan biri  
* Aralık: Yineleme için belirlenen sıklıktaki aralık  
* Belirtilen zamanlama: Yinelemeye ilişkin dakika, saat, hafta içi günü, ay ve ayın günü değerlerini belirtin  
* Sayı: Yineleme sayısı  
* Bitiş zamanı: Belirtilen bitiş zamanından sonra hiç bir iş yürütülmez.  

Bir iş, JSON tanımında belirtilen yinelenen nesneye sahipse yinelenendir. Sayı ve endTime belirtilmişse, önce meydana gelen tamamlama kuralı uygulanır.

## durum
İşin durumu dört değerden biridir: etkin, devre dışı, tamamlandı ya da hatalı. Etkin ya da devre dışı durumuna güncelleştirmek amacıyla işleri KOYABİLİR ya da DÜZELTME EKİ UYGULAYABİLİRSİNİZ. Bir işin durumu tamamlandı ya da hatalı ise, bu güncelleştirilemeyecek son aşamadır (iş yine de SİLİNEBİLİR) Durum özelliğinin bir örneği aşağıdaki gibidir:

        "state": "disabled", // enabled, disabled, completed, or faulted
Tamamlanan ve hatalı işler 60 gün sonra silinir.

## durum
Scheduler işi başladıktan sonra, işin geçerli durumu hakkında bilgi döndürülür. Bu nesne kullanıcı tarafından ayarlanamaz— sistem tarafından ayarlanır. Ancak, bir kimsenin işin durumunu kolayca edinebileceği şekilde iş nesnesinde yer alır (ayrı bağlantılı bir kaynak yerine).

İş durumu, önceki yürütme saatini (varsa), sonraki zamanlanan yürütmeyi (devam eden işler için) ve iş yürütme sayısını içerir.

## retryPolicy
Scheduler işi başarısız olursa, eylemin yeniden denenip denenmeyeceğini ve bunun nasıl yapılacağını belirlemek için bir yeniden deneme ilkesi belirlemek mümkündür. Bu **retryType** nesnesi tarafından belirlenir; yukarıda gösterildiği gibi yeniden deneme ilkesi yoksa, **yok** olarak ayarlanır. Yeniden deneme ilkesi varsa, **sabit** olarak ayarlayın.

Bir yeniden deneme ilkesi ayarlamak için iki ek ayar belirtilebilir: yeniden deneme aralığı (**retryInterval**) ve yeniden deneme sayısı (**retryCount**).

**retryInterval** nesnesi ile belirtilen yeniden deneme aralığı, yeniden denemeler arasındaki aralıktır. Varsayılan değer 30 saniye, minimum yapılandırılabilir değeri 15 saniye, ve maksimum değeri 18 aydır. Ücretsiz iş koleksiyonlarındaki işleri 1 saat şeklinde minimum yapılandırabilir değere sahiptir.  ISO 8601 biçiminde tanımlıdır. Benzer şekilde, yeniden deneme sayısı değeri **retryCount** nesnesiyle belirtilir; bu yeniden denemeye çalışılacak sayıdır. Varsayılan değeri 4’tür ve maksimum değeri 20'dir\.. **retryInterval** ve **retryCount** nesneleri isteğe bağlıdır. **retryType** **sabit** olara ayarlanır ve açık şekilde bir değer belirtilmezse, bunlar belirtilen varsayılan değerlerdir.

## Ayrıca bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure portalında Scheduler’ı kullanmaya başlayın](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturma](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

<!--HONumber=Sep16_HO5-->


