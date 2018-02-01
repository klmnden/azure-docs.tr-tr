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
ms.date: 01/03/2018
ms.author: shlo
ms.openlocfilehash: fc34cfbab796c6e1e4cd25ce13dcc63c39c6699d
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Azure Data Factory'de işlem hattı çalıştırma ve tetikleyiciler
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-scheduling-and-execution.md)
> * [Sürüm 2 - Önizleme](concepts-pipeline-execution-triggers.md)

_İşlem hattı çalıştırması_, Azure Data Factory sürüm 2'de kullanılan ve bir işlem hattı çalıştırma örneğini tanımlayan bir terimdir. Örneğin, 08.00, 09.00 ve 10.00'da çalışan bir işlem hattınız olduğunu kabul edelim. Bu durumda, işlem hattının üç ayrı çalıştırması (işlem hattı çalıştırması) olacaktır. Her işlem hattı çalıştırmasında belirli işlem hattı çalıştırmasını benzersiz bir şekilde tanımlayan GUID olan benzersiz bir işlem hattı çalıştırması kimliği vardır. İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımlanan parametrelere iletilmesiyle oluşturulur. Bir işlem hattı iki şekilde çalıştırılabilir: el ile veya bir _tetikleyici_ kullanılarak. Bu makalede her iki işlem hattı çalıştırma yöntemiyle ilgili ayrıntılı bilgiler yer almaktadır.

> [!NOTE]
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Genel kullanıma açık Data Factory sürüm 1’i kullanıyorsanız bkz. [Azure Data Factory sürüm 1'de zamanlama ve yürütme](v1/data-factory-scheduling-and-execution.md).

## <a name="manual-execution-on-demand"></a>El ile yürütme (isteğe bağlı)
Bir işlem hattının el ile yürütülmesine _isteğe bağlı_ yürütme de denir.

Örneğin çalıştırmak istediğiniz **copyPipeline** adlı basit bir işlem hattına sahip olduğunuzu düşünelim. Bu işlem hattı, bir Azure Blob Depolama kaynak klasöründen aynı depolama alanı içindeki hedef bir klasöre kopyalama yapan tek bir etkinlik içerir. Aşağıdaki JSON tanımında bu örnek işlem hattı gösterilmektedir:

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

JSON tanımında, işlem hattı iki parametre kabul eder: **sourceBlobContainer** ve **sinkBlobContainer**. Çalışma zamanında bu parametrelere değer verirsiniz.

Aşağıdaki metotları kullanarak işlem hattınızı el ile çalıştırabilirsiniz:
- .NET SDK.
- Azure PowerShell modülü.
- REST API.
- Python SDK.

### <a name="the-rest-api"></a>REST API
Aşağıdaki örnek komutta REST API kullanarak işlem hattınızı nasıl el ile çalıştırabileceğiniz gösterilmiştir:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```

Tam kapsamlı bir örnek için bkz. [Hızlı başlangıç: REST API kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-rest-api.md).

### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örnek komutta Azure PowerShell kullanarak işlem hattınızı nasıl el ile çalıştırabileceğiniz gösterilmiştir:

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Parametreleri istek yükünün gövde kısmında iletirsiniz. .NET SDK, Azure Powershell ve Python SDK’da değerleri, çağrıya bağımsız değişken olarak iletilen bir sözlükte iletirsiniz:

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

Tam kapsamlı bir örnek için bkz. [Hızlı başlangıç: Azure PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

### <a name="the-net-sdk"></a>.NET SDK
Aşağıdaki örnek çağrıda .NET SDK kullanarak işlem hattınızı nasıl el ile çalıştırabileceğiniz gösterilmiştir:

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Tam kapsamlı bir örnek için bkz. [Hızlı başlangıç: .NET SDK kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> .NET SDK kullanarak Azure İşlevlerinden, kendi web hizmetlerinizden Azure Data Factory işlem hatları çağırma gibi işlemler yapabilirsiniz.

<h2 id="triggers">Yürütme tetikleme</h2>
Tetikleyiciler bir işlem hattı çalıştırmasını yürütmenin ikinci yoludur. Tetikleyiciler, bir işlem hattı çalıştırmasının başlatılması gereken zamanı belirleyen işlem birimini temsil eder. Azure Data Factory şu anda iki tür tetikleyiciyi destekler:
- Zamanlama tetikleyicisi: Bir işlem hattını duvar saati zamanlamasıyla çağıran bir tetikleyici.
- Atlayan pencere tetikleyicisi: Durumunu koruyarak düzenli bir aralıkta çalışan bir tetikleyici. Azure Data Factory şu anda olay tabanlı tetikleyicileri desteklememektedir. Örneğin, bir dosya varış olayına yanıt veren bir işlem hattı çalıştırmasının tetikleyicisi.

İşlem hatları ve tetikleyiciler çoka çok ilişkisine sahiptir. Birden çok tetikleyici tek bir işlem hattını başlatabilirken, bir tetikleyici birden fazla işlem hattını başlatabilir. Aşağıdaki tetikleyici tanımında, **pipelines** özelliği belirli bir tetikleyici tarafından tetiklenen işlem hattı listesini ifade eder. Özellik tanımı, işlem hattı parametrelerinin değerlerini içerir.

### <a name="basic-trigger-definition"></a>Temel tetikleyici tanımı

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

## <a name="schedule-trigger"></a>Zamanlama tetikleyicisi
Zamanlama tetikleyicisi, işlem hatlarını duvar saati zamanlamasıyla çalıştırır. Bu tetikleyici düzenli ve gelişmiş takvim seçeneklerini destekler. Örneğin, “haftalık” veya “Pazartesi saat 17:00 ve Perşembe saat 21:00” gibi aralıkları destekler. Veri kümesi deseni belirsiz olduğundan ve tetikleyici zaman serisi verileri ile zaman serisi dışı veriler arasında ayrım yapmadığından zamanlama tetikleyicisi esnektir.

Zamanlama tetikleyicileri hakkında daha fazla bilgi ve örnekler için bkz. [Zamanlama tetikleyicisi oluşturma](how-to-create-schedule-trigger.md).

## <a name="tumbling-window-trigger"></a>Atlayan pencere tetikleyicisi
Atlayan pencere tetikleyicileri, durumu korurken belirtilen bir başlangıç zamanından itibaren periyodik bir zaman aralığında başlatılan bir tetikleyici türüdür. Atlayan pencereler sabit boyutlu, çakışmayan ve bitişik zaman aralıkları dizisidir. Atlayan pencere tetikleyicileri hakkında daha fazla bilgi ve örnekler için bkz. [Atlayan pencere tetikleyicisi oluşturma](how-to-create-tumbling-window-trigger.md).

## <a name="schedule-trigger-definition"></a>Zamanlama tetikleyicisi tanımı
Bir zamanlama tetikleyicisi oluştururken JSON tanımı kullanarak zamanlamayı ve yinelemeyi belirtirsiniz. 

Zamanlama tetikleyicinizin bir işlem hattı çalıştırmasını başlatması için tetikleyici tanımındaki belirli işlem hattının işlem hattı başvurusunu ekleyin. İşlem hatları ve tetikleyiciler çoka çok ilişkisine sahiptir. Birden çok tetikleyici tek bir işlem hattını başlatabilir. Tek bir tetikleyici birden fazla işlem hattını başlatabilir.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // How often to fire
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
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
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
> **parameters** özelliği, **pipelines** öğesinin zorunlu bir özelliğidir. İşlem hattınız herhangi bir parametre almasa bile, **parameters** özelliği için boş bir JSON tanımı eklemeniz gerekir.

### <a name="schema-overview"></a>Şemaya genel bakış
Aşağıdaki tabloda bir tetikleyicinin yinelenmesi ve zamanlanmasıyla ilgili ana şema öğelerinin genel bir özeti verilmiştir:

| JSON özelliği | Açıklama |
|:--- |:--- |
| **startTime** | Bir Tarih-Saat değeri. Basit zamanlamalar için **startTime** özelliğinin değeri ilk oluşum için geçerli olur. Karmaşık zamanlamalar için tetikleyici belirtilen **startTime** değerinden önce başlamaz. |
| **endTime** | Tetikleyicinin bitiş tarihi ve saati. Tetikleyici belirtilen bitiş tarihi ve saatinden sonra yürütülmez. Bu özelliğin değeri geçmişte olamaz. <!-- This property is optional. --> |
| **timeZone** | Saat dilimi. Şu anda yalnızca UTC saat dilimi desteklenmektedir. |
| **recurrence** | Tetikleyici için yinelenme kurallarını belirten bir yinelenme nesnesi. recurrence nesnesi şu öğeleri destekler: **frequency**, **interval**, **endTime**, **count** ve **schedule**. Bir yinelenme nesnesi tanımlanırken **frequency** öğesi gereklidir. Yinelenme nesnesinin diğer öğeleri isteğe bağlıdır. |
| **frequency** | Tetikleyicinin yineleneceği sıklık birimi. “Minute”, “hour”, “day”, “week” ve “month” değerleri desteklenir. |
| **interval** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin **interval** değeri 3, **frequency** değeri de "week" ise tetikleyici 3 haftada bir yinelenir. |
| **schedule** | Tetikleyicinin yinelenme zamanlaması. **frequency** değeri belirtilen bir tetikleyici, yinelenmesini bir yinelenme zamanlamasına göre değiştirir. **schedule** özelliği, yinelenme için dakika, saat, haftanın günü, ayın günü ve hafta numarası tabanlı değişiklikleri içerir.

### <a name="schedule-trigger-example"></a>Zamanlama tetikleyicisi örneği

```json
{
    "properties": {
        "name": "MyTrigger",
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-01T09:00:00-08:00",
                "endTime": "2017-11-02T22:00:00-08:00"
            }
        },
        "pipelines": [{
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToBlobPipeline"
                },
                "parameters": {}
            },
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToAzureSQLPipeline"
                },
                "parameters": {}
            }
        ]
    }
}
```

### <a name="schema-defaults-limits-and-examples"></a>Şema varsayılanları, sınırlar ve örnekler

| JSON özelliği | Tür | Gerekli | Varsayılan değer | Geçerli değerler | Örnek |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Dize | Yes | Yok | ISO-8601 Tarih-Saatleri | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Nesne | Yes | Yok | Yinelenme nesnesi | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Sayı | Hayır | 1 | 1-1.000 | `"interval":10` |
| **endTime** | Dize | Yes | Yok | Gelecekteki bir zamanı temsil eden Tarih-Saat değeri. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Nesne | Hayır | None | Zamanlama nesnesi | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>startTime özelliği
Aşağıdaki tabloda **startTime** özelliğinin bir tetikleyici çalıştırmasını nasıl denetlediği gösterilmektedir:

| startTime değeri | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme |
|:--- |:--- |:--- |
| Başlangıç zamanı geçmişte | Başlangıç zamanından sonraki ilk gelecek yürütme zamanını hesaplar ve bu zamanda çalıştırır.<br/><br/>Son yürütme zamanına göre yaptığı hesaplamalarla sonraki yürütmeleri çalıştırır.<br/><br/>Bu tablonun altındaki örneğe bakın. | Tetikleyici belirtilen başlangıç zamanından _önce_ başlamaz. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamayı temel alır.<br/><br/>Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırır. |
| Başlangıç zamanı gelecekte veya güncel | Belirtilen başlangıç zamanında bir kez çalışır.<br/><br/>Son yürütme zamanına göre yaptığı hesaplamalarla sonraki yürütmeleri çalıştırır. | Tetikleyici belirtilen başlangıç zamanından _önce_ başlamaz. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamayı temel alır.<br/><br/>Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırır. |

Başlangıç zamanı geçmişte olduğunda ve bir yinelenme olmasına rağmen zamanlama olmadığında neler olacağını gösteren bir örneğe bakalım. Geçerli zamanın `2017-04-08 13:00`, başlangıç zamanının `2017-04-07 14:00` ve yinelenmenin iki günde bir olduğunu varsayalım. (**recurrence** değeri, **frequency** özelliği “day” olarak, **interval** özelliği 2 olarak ayarlanarak tanımlanır.) **startTime** değerinin geçmişte olduğuna ve geçerli zamanın öncesinde gerçekleştiğine dikkat edin.

Bu şartlar altında ilk çalıştırma zamanı `2017-04-09 at 14:00` olacaktır. Zamanlayıcı altyapısı çalıştırma yinelenmelerini başlangıç zamanından itibaren hesaplar. Geçmişteki örnekler dikkate alınmaz. Altyapı gelecekte gerçekleşen bir sonraki örneği kullanır. Bu senaryoda startTime `2017-04-07 at 2:00pm` ve sonraki örnek bu zamandan iki gün sonrası olan `2017-04-09 at 2:00pm` olur.

İlk yürütme zamanı **startTime** değeri `2017-04-05 14:00` veya `2017-04-01 14:00` olsa bile değişmez. İlk yürütme sonrasındaki yürütmeler zamanlama kullanılarak hesaplanır. Bu nedenle, sonraki yürütmeler sırasıyla `2017-04-11 at 2:00pm`, `2017-04-13 at 2:00pm`, `2017-04-15 at 2:00pm` itibarıyla gerçekleşir ve bu düzende devam eder.

Son olarak, bir tetikleyicinin zamanlamasında saatler veya dakikalar ayarlanmazsa varsayılan olarak ilk yürütmenin saat veya dakikaları kullanılır.

### <a name="schedule-property"></a>schedule özelliği
Diğer yandan, zamanlama kullanımı tetikleyici yürütme sayısını sınırlayabilir. Örneğin, aylık sıklığa sahip bir tetikleyici 31. günde çalışacak şekilde zamanlanırsa, tetikleyici yalnızca 31. günü olan aylarda çalışır.

Diğer yandan zamanlama, tetikleyici çalıştırma sayısını da genişletebilir. Örneğin, aylık sıklığa sahip olan ve ayın 1. ve 2. günlerinde çalışacak şekilde zamanlanan bir tetikleyici, ayda bir kez çalışmak yerine ayın 1. ve 2. günlerinde çalışır.

Birden fazla **schedule** öğesi belirtilmişse değerlendirme sırası en büyük zamanlama ayarından en küçüğe doğru olur. Değerlendirme hafta numarasıyla başlar ve arkasından ayın günü, haftanın günü, saat ve son olarak dakika gelir.

Aşağıdaki tabloda **schedule** öğeleri ayrıntılı bir şekilde açıklanmıştır:

| JSON öğesi | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |
| **minutes** | Tetikleyicinin çalıştığı dakika değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul>
| **hours** | Tetikleyicinin çalıştığı saat değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul> |
| **weekDays** | Tetikleyicinin çalıştığı hafta günleri. Bu değer yalnızca haftalık bir sıklık ile belirtilebilir. | <ul><li>Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</li><li>Gün değerleri dizisi (en fazla dizi boyutu 7’dir)</li><li>Gün değerleri büyük/küçük harfe duyarlı değildir</li></ul> |
| **monthlyOccurrences** | Tetikleyicinin çalıştığı ay günleri. Bu değer yalnızca aylık bir sıklık ile belirtilebilir. | <ul><li>**monthlyOccurence** nesneleri dizisi: `{ "day": day,  "occurrence": occurence }`.</li><li>**day** özniteliği, tetikleyicinin çalıştığı gündür. Örneğin, **day** değeri `{Sunday}` olan bir **monthlyOccurrences** özelliği, ayın her Pazar günü anlamına gelir. **day** özniteliği gereklidir.</li><li>**occurrence** özniteliği, ay içinde belirtilen **day** değerinin gerçekleşmesidir. Örneğin, **day** ve **occurrence** değerleri `{Sunday, -1}` olan bir **monthlyOccurrences** özelliği, ayın son Pazar günü anlamına gelir. **occurrence** özniteliği isteğe bağlıdır.</li></ul> |
| **monthDays** | Tetikleyicinin çalıştığı ay günü. Bu değer yalnızca aylık bir sıklık ile belirtilebilir. | <ul><li><= -1 ve >= -31 şartlarını karşılayan herhangi bir değer</li><li>>= 1 ve <= 31 şartlarını karşılayan herhangi bir değer</li><li>Değer dizisi</li></ul> |

## <a name="examples-of-trigger-recurrence-schedules"></a>Tetikleyici yineleme zamanlaması örnekleri
Bu bölümde yinelenme zamanlaması örnekleri sağlanmış ve **schedule** nesnesi ile bunun öğelerine odaklanılmıştır.

Örneklerde **interval** değerinin 1 olduğu ve zamanlama tanımına göre **frequency** değerinin doğru olduğu varsayılmıştır. Örneğin, hem "day" şeklinde bir **frequency** değeri hem de **schedule** nesnesinde bir "monthDays" değişikliği olamaz. Bu gibi kısıtlamalar önceki bölümde yer alan tabloda belirtilmiştir.

| Örnek | Açıklama |
|:--- |:--- |
| `{"hours":[5]}` | Her gün 05.00'te çalıştır. |
| `{"minutes":[15], "hours":[5]}` | Her gün 05.15'te çalıştır. |
| `{"minutes":[15], "hours":[5,17]}` | Her gün 05.15 ve 17.15’te çalıştır. |
| `{"minutes":[15,45], "hours":[5,17]}` | Her gün 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |
| `{"minutes":[0,15,30,45]}` | 15 dakikada bir çalıştır. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Saatte bir çalıştır. Bu tetikleyici saatte bir çalışır. **startTime** değeri belirtilirse dakikalar bu değer tarafından denetlenir. Bir değer belirtilmezse dakikalar, oluşturma zamanı tarafından denetlenir. Örneğin başlangıç zamanı veya oluşturma zamanı (hangisi geçerliyse) 12:25 ise tetikleyici 00.25, 01.25, 02.25, ... ve 23.25 saatlerinde çalışır.<br/><br/>Bu zamanlama, **frequency** değeri “hour”, **interval** değeri 1 olan ve **schedule** olmayan bir tetikleyiciye sahip olmakla eşdeğerdir. Bu zamanlama farklı **frequency** ve **interval** değerleriyle kullanılarak başka tetikleyiciler oluşturulabilir. Örneğin, **frequency** değeri “day” olduğunda her gün çalışan zamanlama, **frequency** değeri “month” olduğunda yalnızca ayda bir kere çalışır. |
| `{"minutes":[0]}` | Her saat başı çalıştır. Bu tetikleyici 12.00, 01.00, 02.00 gibi bir saatte başlayarak saat başı çalışır.<br/><br/>Bu zamanlama, **frequency** değeri “hour” olup **startTime** değeri sıfır dakika olan ya da **schedule** değeri olmayıp **frequency** değeri “day” olan bir tetikleyiciye eşdeğerdir. **frequency** değeri “week” veya “month” olursa zamanlama yalnızca haftada bir gün veya ayda bir gün çalışır. |
| `{"minutes":[15]}` | Her saat başını 15 dakika geçe çalıştır. Bu tetikleyici, 00.15, 01.15, 02.15, vb. bir saatte başlayıp 23.15’te bitecek şekilde her saat başını 15 dakika geçe çalışır. |
| `{"hours":[17], "weekDays":["saturday"]}` | Her hafta Cumartesi günleri saat 17.00'de çalıştır. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.00'de çalıştır. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.15 ve 17.45'te çalıştır. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Haftanın her günü 15 dakikada bir çalıştır. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Haftanın her günü 09.00 ile 16.45 arasında 15 dakikada bir çalıştır. |
| `{"weekDays":["tuesday", "thursday"]}` | Salı ve Perşembe günleri belirtilen başlangıç saatinde çalıştır. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Her ayın 28. gününde saat 06.00'da çalıştır (**frequency** değerinin “month” olduğu kabul edilir). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Ayın son günü saat 06.00'da çalıştır. Bir tetikleyiciyi ayın son gününde çalıştırmak için 28, 29, 30 veya 31 yerine -1 değerini kullanın. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Her ayın ilk ve son günü saat 06.00'da çalıştır. |
| `{monthDays":[1,14]}` | Her ayın birinci ve 14. gününde belirtilen başlangıç zamanında çalıştır. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Her ayın ilk Cuma günü saat 05.00'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Her ayın ilk Cuma gününde belirtilen başlangıç zamanında çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Her ay, ayın sondan üçüncü Cuma gününde belirtilen zamanda çalıştır. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Her ayın ilk ve son Cuma günü saat 05.15'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Her ayın ilk ve son Cuma günü belirtilen başlangıç zamanında çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Her ayın beşinci Cuma günü belirtilen başlangıç zamanında çalıştır. Bir ayda beşinci Cuma günü yoksa, işlem hattı yalnızca beşinci Cuma günlerinde çalışacak şekilde zamanlandığından çalışmaz. Tetikleyiciyi ayın son Cuma gününde çalıştırmak istiyorsanız **occurrence** değeri için 5 yerine -1 yazın. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Ayın son Cuma günü 15 dakikada bir çalıştır. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Her ayın üçüncü Çarşamba günü 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |

## <a name="trigger-type-comparison"></a>Tetikleyici türü karşılaştırması
Hem atlayan pencere tetikleyicisi hem de zamanlama tetikleyicisi zaman sinyalleriyle çalışıyorsa bunları farklı yapan nedir?

Aşağıdaki tabloda atlayan pencere tetikleyicisi ile zamanlama tetikleyicisinin karşılaştırması sağlanmıştır:

|  | Atlayan&nbsp;pencere&nbsp;tetikleyicisi | Zamanlama&nbsp;tetikleyici |
|:--- |:--- |:--- |
| **Doldurma&nbsp;senaryoları** | Destekleniyor. Geçmişteki aralıklar için işlem hattı çalıştırmaları zamanlanabilir. | Desteklenmiyor. İşlem hattı çalıştırmaları yalnızca geçerli zamandan sonraki ve gelecekteki zaman dönemlerinde yürütülebilir. |
| **Güvenilirlik** | %100 güvenilirlik. Belirtilen bir başlangıç tarihinden itibaren boşluk olmayacak şekilde tüm aralıklar için işlem hattı çalıştırması zamanlanabilir. | Daha az güvenilir. |
| **Yeniden deneme&nbsp;özelliği** | Destekleniyor. Başarısız işlem hattı çalıştırmaları varsayılan olarak 0 yeniden deneme ilkesine veya kullanıcı tarafından tetikleyici tanımında belirtilen bir ilkeye sahiptir. İşlem hattı çalıştırmaları eş zamanlılık/sunucu/azaltma sınırları (yani 400: Kullanıcı Hatası, 429: Çok fazla istek var ve 500: İç Sunucu hatası durum kodları) nedeniyle başarısız olursa otomatik olarak yeniden denenir. | Desteklenmiyor. |
| **Eşzamanlılık** | Destekleniyor. Kullanıcılar tetikleyicinin eş zamanlılık sınırlarını açıkça ayarlayabilir. 1 ile en fazla 50 eş zamanlı tetiklenmiş işlem hattı çalıştırmasına izin verir. | Desteklenmiyor. |
| **Sistem&nbsp;değişkenleri** | **WindowStart** ve **WindowEnd** sistem değişkenlerinin kullanımını destekler. Kullanıcılar, tetikleyici tanımında tetikleyici sistem değişkenleri olarak `triggerOutputs().windowStartTime` ve `triggerOutputs().windowEndTime` değişkenine erişebilir. Değerler sırasıyla aralık başlangıç zamanı ve aralık bitiş zamanı olarak kullanılır. Örneğin, saat başı çalışan bir atlayan pencere tetikleyicisi için 01.00 ile 02.00 arası aralığın tanımı `triggerOutputs().WindowStartTime = 2017-09-01T01:00:00Z` ve `triggerOutputs().WindowEndTime = 2017-09-01T02:00:00Z` şeklinde olur. | Desteklenmiyor. |
| **Tetiklenecek&#x2011;işlem&#x2011;hattı ilişkisi** | Bire bir ilişkileri destekler. Yalnızca bir işlem hattı tetiklenebilir. | Çoka çok ilişkileri destekler. Birden çok tetikleyici tek bir işlem hattını başlatabilir. Tek bir tetikleyici birden fazla işlem hattını başlatabilir. | 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki öğreticilere bakın:

- [Hızlı başlangıç: .NET SDK kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Zamanlama tetikleyicisi oluşturma](how-to-create-schedule-trigger.md)
- [Atlayan pencere tetikleyicisi oluşturma](how-to-create-tumbling-window-trigger.md)
