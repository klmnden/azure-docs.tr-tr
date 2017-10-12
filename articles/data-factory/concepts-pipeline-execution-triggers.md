---
title: "Azure Data Factory'de işlem hattı çalıştırma ve tetikleyiciler | Microsoft Docs"
description: "Bu makalede Azure Data Factory'de istek üzerine veya tetikleyici oluşturarak işlem hattı çalıştırma konusunda bilgi verilmektedir."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: c319979cce23da69965d4fbab037919461f67b3a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Azure Data Factory'de işlem hattı çalıştırma ve tetikleyiciler 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-scheduling-and-execution.md)
> * [Sürüm 2 - Önizleme](concepts-pipeline-execution-triggers.md)

**İşlem hattı çalıştırması**, Azure Data Factory Sürüm 2'de kullanılan ve bir işlem hattı çalıştırma örneğini tanımlayan bir terimdir. Örneğin, 08:00, 09:00 ve 10:00'da çalışan bir işlem hattınız olduğunu kabul edelim. Bu durumda işlem hattının üç ayrı çalıştırması (işlem hattı çalıştırması) olacaktır. Her işlem hattı çalıştırmasında belirli işlem hattı çalıştırmasını benzersiz bir şekilde tanımlayan GUID olan benzersiz bir işlem hattı çalıştırması kimliği vardır. İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımlanan parametrelere iletilmesiyle oluşturulur. Bir işlem hattı iki şekilde çalıştırılabilir: **el ile** veya bir **tetikleyici** ile. Bu makalede iki işlem hattı çalıştırma yöntemiyle ilgili ayrıntılı bilgiler yer almaktadır. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory Sürüm 1'de zamanlama ve yürütme](v1/data-factory-scheduling-and-execution.md).

## <a name="run-pipeline-on-demand"></a>İşlem hattını istek üzerine çalıştırma
Bu yöntemde işlem hattınızı el ile çalıştıracaksınız. Bu yöntem, işlem hattının istek üzerine çalıştırılması olarak da kabul edilebilir. 

Örneğin çalıştırmak istediğiniz **copyPipeline** adlı bir işlem hattına sahip olduğunuzu düşünelim. Bu işlem hattı, Azure Blob Depolama içindeki kaynak klasörden aynı depolama alanı içindeki hedef klasöre kopyalama yapan basit bir işlem hattıdır. Basit işlem hattının tanımı aşağıda verilmiştir: 

```json
{
  "name": "copyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```
İşlem hattı iki parametre kabul eder: JSON tanımında gösterilen şekilde sourceBlobContainer ve sinkBlobContainer. Çalışma zamanında bu parametrelere değer verirsiniz. 

İşlem hattını el ile çalıştırmak için şu yöntemlerden birini kullanabilirsiniz: .NET, PowerShell, REST ve Python. 

### <a name="rest-api"></a>REST API
Örnek bir REST komutu aşağıda verilmiştir:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```
Tam kapsamlı bir örnek için bkz. [Hızlı başlangıç: REST API kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-rest-api.md).

### <a name="powershell"></a>PowerShell
Örnek bir PowerShell komutu aşağıda verilmiştir: 

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Parametreleri istek yükünün gövde kısmında iletirsiniz. .NET, Powershell ve Python ortamlarında değerleri, çağrıya bağımsız değişken olarak iletilen bir sözlükte iletirsiniz.

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

Yanıt yükü, işlem hattı çalıştırmasının benzersiz kimliği olur:

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```


Tam kapsamlı bir örnek için bkz. [Hızlı başlangıç: PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

### <a name="net"></a>.NET 
Örnek bir .NET çağrısı aşağıda verilmiştir: 

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Tam kapsamlı bir örnek için bkz. [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> .NET API'sini kullanarak Azure İşlevlerinden kendi web hizmetlerinize Data Factory işlem hatları çağırma gibi işlemler yapabilirsiniz.

## <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler bir işlem hattı çalıştırmasını yürütmenin ikinci yoludur. Tetikleyiciler, bir işlem hattı çalıştırmasının başlatılması gereken zamanı belirleyen işlem birimini temsil eder. Şu anda Data Factory, bir işlem hattını duvar saati zamanlamasıyla çağıran tetikleyicileri destekler. Buna **Zamanlayıcı Tetikleyicisi** denir. Data Factory şu anda dosya ulaştığında bir işlem hattı çalıştırılmasının tetikleyicisi gibi olay tabanlı tetikleyicileri desteklememektedir.

İşlem hatları ve tetikleyiciler "n-m" ilişkisine sahiptir. Birden çok tetikleyici tek bir işlem hattını, bir tetikleyici de birden fazla işlem hattını başlatabilir. Aşağıdaki JSON tetikleyici tanımında **pipelines** özelliği belirli bir tetikleyici tarafından tetiklenen işlem hattı listesine ve işlem hattı parametresi değerlerine başvurmaktadır.

### <a name="basic-trigger-definition"></a>Temel tetikleyici tanımı: 
```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
        },
        "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                          "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
            }
        ]
    }
```

## <a name="scheduler-trigger"></a>Zamanlayıcı tetikleyicisi
Zamanlayıcı tetikleyicisi işlem hatlarını duvar saati zamanlamasıyla çalıştırır. Bu tetikleyici düzenli ve gelişmiş takvim seçeneklerini (haftalık, Pazartesi saat 17:00 ve Perşembe saat 21:00) destekler. Veri kümesi deseni belirsiz olduğundan ve zaman serisi ile zaman serisi harici veriler arasında ayrım olmadığından esnek bir çözümdür.

### <a name="scheduler-trigger-json-definition"></a>Zamanlayıcı tetikleyicisi JSON tanımı
Zamanlayıcı tetikleyicisi oluşturduğunuzda bu bölümdeki örnekte gösterilen şekilde JSON kullanarak zamanlama ve yinelenme bildirimi yapabilirsiniz. 

Zamanlayıcı tetikleyicinizin bir işlem hattı çalıştırmasını başlatması için tetikleyici tanımındaki belirli işlem hattının işlem hattı başvurusunu ekleyin. İşlem hatları ve tetikleyiciler "n-m" ilişkisine sahiptir. Birden çok tetikleyici tek bir işlem hattını başlatabilir. Aynı tetikleyici birden fazla işlem hattını başlatabilir.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": <<default UTC>>
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ] 
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name> : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

### <a name="overview-scheduler-trigger-schema"></a>Genel Bakış: Zamanlayıcı tetikleyicisi şeması
Aşağıdaki tabloda bir tetikleyici içindeki yinelenme ve zamanlamayla ilgili ana öğelerin genel bir özeti verilmiştir:

JSON özelliği |     Açıklama
------------- | -------------
startTime | startTime, Tarih-Saat türündedir. Basit zamanlamalar için startTime ilk yinelenme zamanıdır. Karmaşık zamanlamalar için tetikleyici startTime değerinden önce başlamaz.
yineleme | recurrence nesnesi tetikleyici için yinelenme kurallarını belirtir. recurrence nesnesi şu öğeleri destekler: frequency, interval, endTime, count ve schedule. recurrence tanımlanmışsa frequency değerinin de tanımlanması gerekir. Diğer recurrence öğeleri isteğe bağlıdır.
frequency | Tetikleyicinin yineleneceği sıklık birimini temsil eder. Desteklenen değerler: `minute`, `hour`, `day`, `week` veya `month`.
interval | interval pozitif tamsayıdır. Tetikleyicinin çalışma sıklığını belirten frequency için aralığı belirtir. Örneğin interval değeri 3, frequency değeri de "week" ise tetikleyici 3 haftada bir yinelenir.
endTime | Tetikleyici için bitiş tarihini ve saatini belirtir. Tetikleyici bu tarihten sonra çalıştırılmaz. endTime değeri geçmişte olamaz.
schedule | Belirtilen sıklıktaki bir tetikleyici, yinelenmesini bir yinelenme zamanlamasına göre değiştirir. schedule; dakika, saat, haftanın günü, ayın günü ve hafta numarası tabanlı değişiklikleri içerir.

### <a name="overview-scheduler-trigger-schema-defaults-limits-and-examples"></a>Genel Bakış: Zamanlayıcı tetikleyicisi şema varsayılanları, sınırlar ve örnekler

JSON adı | Değer türü | Gerekli mi? | Varsayılan değer | Geçerli değerler | Örnek
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Dize | Evet | None | ISO-8601 Tarih-Saatleri | ```"startTime" : "2013-01-09T09:30:00-08:00"```
yineleme | Nesne | Evet | None | Yinelenme nesnesi | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Sayı | Hayır | 1 | 1-1000 arası. | ```"interval":10```
endTime | Dize | Evet | None | Gelecekteki bir zamanı temsil eden Tarih-Saat değeri | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Nesne | Hayır | None | Zamanlama nesnesi | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Ayrıntılı Bakış: startTime
Aşağıdaki tabloda startTime öğesinin bir tetikleyicinin çalıştırılmasını nasıl denetlediği gösterilmektedir:

startTime değeri | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme
--------------- | --------------------------- | ------------------------
Başlangıç zamanı geçmişte | Başlangıç zamanından sonraki ilk gelecek çalıştırma zamanını hesaplar ve belirtilen zamanda çalıştırır.<p>İlk çalıştırma zamanına göre hesaplayarak sonraki çalıştırmaları gerçekleştirir.</p><p>Bu tablonun altındaki örneğe bakın.</p> | Tetikleyici belirtilen başlangıç zamanından _önce_ başlamaz. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir. <p>Sonraki çalıştırmaları yinelenme zamanlamasına göre gerçekleştirir</p>
Başlangıç zamanı gelecekte veya güncel | Belirtilen başlangıç zamanında bir kez çalışır. <p>İlk çalıştırma zamanına göre hesaplayarak sonraki çalıştırmaları gerçekleştirir.</p> | Tetikleyici belirtilen başlangıç zamanından _önce_ başlamaz. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir.<p>Sonraki çalıştırmaları yinelenme zamanlamasına göre gerçekleştirir.</p>

startTime geçmişte olduğunda ve yinelenme olduğunda ancak zamanlama olmadığında neler olacağını gösteren örneğe bakalım. Geçerli zamanın `2017-04-08 13:00` olduğunu, startTime değerinin `2017-04-07 14:00` olduğunu ve yinelenmenin iki günde bir (frequency: day ve interval: 2 ile tanımlanmış) olduğunu kabul edelim. startTime değerinin geçmişte olduğuna ve geçerli zamanın öncesinde gerçekleştiğine dikkat edin.

Bu şartlar altında ilk çalıştırma zamanı `2017-04-09 at 14:00` olacaktır. Zamanlayıcı altyapısı çalıştırma yinelenmelerini başlangıç zamanından itibaren hesaplar. Geçmişteki örnekler dikkate alınmaz. Altyapı gelecekte gerçekleşen bir sonraki örneği kullanır. Bu nedenle bu durumda startTime `2017-04-07 at 2:00pm` ve sonraki örnek bu zamandan iki gün sonrası olan `2017-04-09 at 2:00pm` olur.

İlk çalıştırma zamanı startTime `2017-04-05 14:00` veya `2017-04-01 14:00` olsa bile değişmez. İlk çalıştırma sonrasındaki çalıştırmalar zamanlama kullanılarak hesaplanır. Bu nedenle `2017-04-11 at 2:00pm`, sonra `2017-04-13 at 2:00pm`, ardından `2017-04-15 at 2:00pm` vs. olur.

Son olarak bir tetikleyici zamanlamaya sahipse zamanlamada saat ve/veya dakika belirtilmemiş olsa dahi varsayılan olarak ilk çalıştırmanın saat ve/veya tarih değerini alır.

### <a name="deep-dive-schedule"></a>Ayrıntılı Bakış: schedule
schedule bir yandan tetikleyici çalıştırma sayısını sınırlayabilir. Örneğin frequency parametresi "month" olan bir tetikleyici yalnızca 31. günde çalışan bir zamanlamaya sahipse tetikleyici yalnızca 31. günü olan aylarda çalışır.

Diğer taraftan schedule, tetikleyici çalıştırma sayısını da genişletebilir. Örneğin frequency parametresi "month" olan bir tetikleyici ayın 1. ve 2. gününde çalışan bir zamanlamaya sahipse tetikleyici ayda bir kez yerine ayın 1. ve 2. günlerinde çalışır.

Birden fazla schedule öğesi belirtilmişse değerlendirme sırası en büyükten en küçüğe doğru (hafta numarası, ayın günü, haftanın günü, saat ve dakika) olur.

Aşağıdaki tabloda schedule öğeleri ayrıntılı bir şekilde açıklanmıştır:


JSON adı | Açıklama | Geçerli Değerler
--------- | ----------- | ------------
minutes | Tetikleyicinin çalıştığı dakika değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul>
hours | Tetikleyicinin çalıştığı saat değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul>
weekDays | Tetikleyicinin çalıştığı hafta günleri. Yalnızca weekly frequency değeri ile belirtilebilir. | <ul><li>Monday, Tuesday, Wednesday, Thursday, Friday, Saturday veya Sunday</li><li>Yukarıdaki değerlerden oluşan dizi (maksimum dizi boyutu: 7)</li></p>Büyük/küçük harf duyarlı değildir</p>
monthlyOccurrences | Tetikleyicinin ayın hangi günlerinde çalışacağını belirler. Yalnızca monthly frequency değeri ile belirtilebilir. | monthlyOccurence nesneleri dizisi: `{ "day": day,  "occurrence": occurence }`. <p> day, tetikleyicinin çalıştığı haftanın günüdür. Örneğin `{Sunday}`, ayın her Pazar günüdür. Gereklidir.<p>occurrence, ayın ay içindeki yinelenme günüdür. Örneğin `{Sunday, -1}`, ayın son Pazar günüdür. İsteğe bağlı.
monthDays | Tetikleyicinin çalıştığı ayın günü. Yalnızca monthly frequency değeri ile belirtilebilir. | <ul><li><= -1 ve >= -31 şartlarını karşılayan herhangi bir değer</li><li>>= 1 ve <= 31 şartlarını karşılayan herhangi bir değer</li><li>Yukarıdaki değerlerden oluşan bir dizi</li>


## <a name="examples-recurrence-schedules"></a>Örnekler: yineleme zamanlamaları
Bu bölümde schedule nesnesine ve alt öğelerine odaklanan yinelenme zamanlaması örnekleri verilmiştir.

Bu örnekte interval değerinin 1 olduğu kabul edilmiştir. Aynı zamanda schedule içindeki doğru frequency değerinin kullanıldığını kabul edin. Örneğin frequency için "day" değerini kullanıyorsanız schedule içinde "monthDays" değeri bulunamaz. Bu kısıtlamalar önceki bölümde yer alan tabloda belirtilmiştir. 

Örnek | Açıklama
------- | -----------
`{"hours":[5]}` | Her gün saat 05:00'te çalışır
`{"minutes":[15], "hours":[5]}` | Her gün saat 05:15'te çalışır
`{"minutes":[15], "hours":[5,17]}` | Her gün 05:15 ve 17:15 saatleri arasında çalışır
`{"minutes":[15,45], "hours":[5,17]}` | Her gün 05:15, 05:45, 17:15 ve 17:45 saatlerinde çalışır
`{"minutes":[0,15,30,45]}` | 15 dakikada bir çalışır
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Saatte bir çalışır. Bu tetikleyici saatte bir çalışır. Dakika belirtilmişse startTime tarafından denetlenir. Belirtilmemişse oluşturma zamanı tarafından belirlenir. Örneğin başlangıç zamanı veya oluşturma zamanı (hangisi geçerliyse) 12:25 ise tetikleyici 00:25, 01:25, 02:25, …, 23:25 saatlerinde çalışır. Zamanlama frequency değeri "hour", interval değeri 1 olan ve zamanlama olmayan tetikleyiciye eşittir. Farkı bu zamanlamanın farklı frequency ve interval değerleriyle kullanılarak başka tetikleyicilerin de oluşturulabilmesidir. Örneğin frequency değeri "month" olursa zamanlama ayda bir, frequency değeri "day" olduğunda ise her gün çalışır.
`{"minutes":[0]}` | Her saat başı çalışır. Bu tetikleyici de saatte bir ancak saat başında çalışır (örneğin 00:00, 01:00, 02:00 vs.). Bu ayar frequency değeri "hour", startTime değeri sıfır dakika ve frequency değeri "day" olduğunda zamanlama olmayan ayara eşittir ancak frequency değeri "week" veya "month" olduğunda zamanlama yalnızca haftada bir gün veya ayda bir gün çalıştırılır.
`{"minutes":[15]}` | Her saat başını 15 dakika geçe çalışır. 00:15'ten başlayarak 01:15, 02:15 gibi saatte bir çalışır ve 22:15 ile 23:15'te biter.
`{"hours":[17], "weekDays":["saturday"]}` | Her hafta Cumartesi günleri saat 17:00'de çalışır
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17:00'de çalışır
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17:15 ve 17:45'te çalışır
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Haftanın her günü 15 dakikada bir çalışır
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Haftanın her günü 09:00 ve 16:45 saatleri arasında 15 dakikada bir çalışır
`{"weekDays":["tuesday", "thursday"]}` | Salı ve Perşembe günleri başlangıç saatinde çalışır.
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | Her ayın 28. gününde saat 06:00'da çalışır (frequency değerinin month olduğu kabul edildiğinde)
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Ayın son günü saat 06:00'da çalışır. Bir tetikleyiciyi ayın son gününde çalıştırmak isterseniz 28, 29, 30 veya 31 yerine -1 değerini kullanın.
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Her ayın ilk ve son günü saat 06:00'da çalışır
`{monthDays":[1,14]}` | Her ayın ilk ve on dördüncü gününde belirtilen başlangıç zamanında çalışır.
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Her ayın ilk Cuma günü saat 05:00'te çalışır
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Her ayın ilk Cuma gününde belirtilen başlangıç zamanında çalışır.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Her ayın üçüncü Cuma gününde başlayıp ay sonuna kadar başlangıç zamanında çalışır
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Her ayın ilk ve son Cuma günü saat 05:15'te çalışır
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Her ayın ilk ve son Cuma gününde belirtilen başlangıç zamanında çalışır
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Her ayın beşinci Cuma gününde çalışır. Ayda beşinci Cuma günü yoksa işlem hattı yalnızca beşinci Cuma günlerinde çalışacak şekilde zamanlandığından çalışmaz.  Tetikleyiciyi ayın son yinelenen Cuma gününde çalıştırmak istiyorsanız occurrence değeri için 5 yerine -1 yazın.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Ayın son Cuma gününde 15 dakikada bir çalışır.
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Her ayın üçüncü Çarşamba günü 05:15, 05:45, 17:15 ve 17:45'te çalışır.




## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki öğreticilere bakın: 

- [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
