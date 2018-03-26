---
title: Azure Data Factory - JSON betik oluşturma başvurusu | Microsoft Docs
description: Data Factory varlıkları için JSON şemaları sağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 198fa15b7ee8cce6781e6a2575844a9666185be9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Data Factory - JSON betik oluşturma başvurusu
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir.


Bu makalede, Azure Data Factory varlıkları (ardışık düzen, etkinlik, veri kümesi ve bağlantılı hizmet) tanımlamak için JSON şemaları ve örnekler sağlar.  

## <a name="pipeline"></a>İşlem hattı 
Ardışık düzen tanımı için üst düzey yapısı aşağıdaki gibidir: 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Aşağıdaki tabloda özellikleri JSON tanımını ardışık düzen içinde açıklanmaktadır:

| Özellik | Açıklama | Gerekli
-------- | ----------- | --------
| ad | İşlem hattının adı. Eylemi temsil eden bir ad belirtin etkinlik veya ardışık düzen yapmak için yapılandırılır<br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi (_) ile başlamalıdır</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama |Hangi etkinlik veya ardışık düzen açıklayan metin için kullanılır | Hayır |
| etkinlikler | Etkinliklerin listesini içerir. | Evet |
| start |Ardışık düzeni için başlangıç tarihi / saati. Olmalıdır [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601). Örneğin: 2014-10-14T16:32:41. <br/><br/>Yerel saat örneğin tahmini süre belirlemek mümkündür. Örnek aşağıda verilmiştir: `2016-02-27T06:00:00**-05:00`, 6'da tahmini olduğu<br/><br/>Başlangıç ve bitiş özellikleri birlikte ardışık düzen etkin süresini belirtin. Çıktı dilimler yalnızca ile etkin bu dönemde üretilir. |Hayır<br/><br/>End özelliği için bir değer belirtirseniz, başlangıç özelliği için değer belirtmeniz gerekir.<br/><br/>Başlangıç ve bitiş zamanlarını hem de bir işlem hattı oluşturmak için boş olabilir. Çalıştırmak ardışık düzeni için etkin bir süre ayarlamak için her iki değer belirtmeniz gerekir. Başlangıç ve bitiş zamanlarını belirtmezseniz, bir işlem hattı oluştururken, bunları daha sonra Set-AzureRmDataFactoryPipelineActivePeriod cmdlet'ini kullanarak ayarlayabilirsiniz. |
| end |Ardışık düzeni için bitiş tarihi / saati. Belirtilen ISO biçiminde olmalıdır. Örneğin: 2014-10-14T17:32:41 <br/><br/>Yerel saat örneğin tahmini süre belirlemek mümkündür. Örnek aşağıda verilmiştir: `2016-02-27T06:00:00**-05:00`, 6'da tahmini olduğu<br/><br/>İşlem hattını süresiz olarak çalışacak şekilde 9999-09-09 end özelliği için değer olarak belirtin. |Hayır <br/><br/>Başlangıç özellik için bir değer belirtirseniz, son özelliği için değer belirtmeniz gerekir.<br/><br/>İçin notlarına bakın **Başlat** özelliği. |
| isPaused |Ardışık Düzen true olarak ayarlandığında çalışmazsa. Varsayılan değer = false. Bu özelliği etkinleştirmek veya devre dışı bırakmak için kullanabilirsiniz. |Hayır |
| pipelineMode |Zamanlama için yöntem ardışık düzeni için çalışır. İzin verilen değerler: (varsayılan), zamanlanmış kez.<br/><br/>'Zamanlanmış' ardışık düzen belirtilen süre aralığında etkin süresinin (başlangıç ve bitiş saati) göre çalıştırıldığını gösterir. 'Kez' ardışık düzen yalnızca bir kez çalıştırıldığını gösterir. Oluşturulduktan sonra kez ardışık düzen şu anda değiştiren/güncelleştirilemiyor. Bkz: [Onetime ardışık düzen](data-factory-create-pipelines.md#onetime-pipeline) kez ayar hakkındaki ayrıntılar için. |Hayır |
| expirationTime |Kendisi için ardışık düzeni geçerli olduğunu ve sağlanan kalacağı oluşturulduktan sonra süre. Tüm etkin başarısız oldu, yok veya sona erme zamanı ulaştığında çalıştığında, ardışık düzen otomatik olarak silinmesi durumunda. |Hayır |


## <a name="activity"></a>Etkinlik 
Ardışık düzen tanımı (etkinlikleri öğesi) içinde bir etkinlik için üst düzey yapısı aşağıdaki gibidir:

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    }
    "scheduler":
    {
    }
}
```

Tablo Özellikleri JSON tanımını etkinlik içinde açıklanmıştır:

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinliğin adı. Eylemi temsil eden bir ad belirtin, etkinlik yapılandırılması<br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi (_) ile başlamalıdır</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama |Etkinlik hangi amaçla kullanıldığına açıklayan metin. |Hayır |
| type |Etkinlik türünü belirtir. Bkz: [veri DEPOLARINA](#data-stores) ve [veri dönüştürme etkinlikleri](#data-transformation-activities) bölümleri etkinlikler farklı türde. |Evet |
| Girişleri |Etkinlik tarafından kullanılan giriş tabloları<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |HDInsightStreaming ve SqlServerStoredProcedure etkinlikleri için Hayır'ı <br/> <br/> Diğer tüm kullanıcılar için Evet |
| çıkışlar |Etkinlik tarafından kullanılan çıkış tabloları.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Evet |
| linkedServiceName |Etkinlik tarafından kullanılan bağlı hizmetin adı. <br/><br/>Bir etkinlik için gerekli işlem ortamına bağlanan bağlı hizmeti belirtmeniz gerekebilir. |Hdınsight etkinlikleri, Azure Machine Learning etkinlikleri ve saklı yordam etkinliği için Evet. <br/><br/>Diğer tümü için hayır |
| typeProperties |Özellikler typeProperties bölümünde etkinlik türüne bağlıdır. |Hayır |
| ilke |Etkinliğin çalışma zamanı davranışını etkileyen ilkeler. Belirtilmezse, varsayılan ilkeler kullanılır. |Hayır |
| Zamanlayıcı |"Zamanlayıcı" özelliği, istenen etkinliği için zamanlama tanımlamak için kullanılır. Onun alt Listedekilerin aynıdır [dataset kullanılabilirliği özelliğinde](data-factory-create-datasets.md#dataset-availability). |Hayır |

### <a name="policies"></a>İlkeler
İlkeler, özellikle bir tablonun dilim işlendiğinde bir etkinlik çalışma zamanı davranışını etkiler. Aşağıdaki tabloda ayrıntılar sağlar.

| Özellik | İzin verilen değerler | Varsayılan Değer | Açıklama |
| --- | --- | --- | --- |
| Eşzamanlılık |Tamsayı <br/><br/>En yüksek değeri: 10 |1 |Etkinliğin eşzamanlı yürütmeleri sayısı.<br/><br/>Üzerinde farklı dilimler oluşabilir paralel etkinlik yürütmeleri sayısını belirler. Örneğin, bir etkinlik geçtikleri gerekiyorsa bir büyük daha büyük bir eşzamanlılık değer sahip kullanılabilir veri kümesi, veri işleme hızı artar. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |İşlenen veri dilimleri sıralama belirler.<br/><br/>Örneğin, varsa (bir oluşmasını 4 pm adresindeki ve 17: 00 saatleri sırasında başka bir) 2 böler ve hem de yürütme olması. ExecutionPriorityOrder NewestFirst olacak şekilde ayarlarsanız, 17: 00 saatleri sırasında dilim önce işlenir. ExecutionPriorityORder OldestFIrst olacak şekilde ayarlarsanız, benzer şekilde ardından 4 PM adresindeki dilim işlenir. |
| retry |Tamsayı<br/><br/>En büyük değer 10 olabilir |0 |Veri işleme ve dilim için hata olarak işaretlenmiş önce yeniden deneme sayısı. Etkinlik yürütme veri dilimi için belirtilen yeniden deneme sayısı kadar yeniden denenir. Yeniden deneme mümkün olan en kısa sürede hatasından sonra yapılır. |
| timeout |TimeSpan |00:00:00 |Etkinlik için zaman aşımı. Örnek: 00:10: (zaman aşımı 10 dakika anlamına gelir) 00<br/><br/>Bir değer belirtilmemiş ya da 0'dır, zaman aşımını sonsuz olur.<br/><br/>Dilim üzerinde veri işleme süresi zaman aşımı değerini aşarsa, iptal edilir ve sistem işlemeyi yeniden dener. Yeniden deneme sayısı yeniden deneme özelliğe bağlıdır. Zaman aşımı oluştuğunda durumu süresi sona erdi için ayarlanır. |
| gecikme |TimeSpan |00:00:00 |Veri işleme dilim başlatır gecikme belirtin.<br/><br/>Beklenen yürütme süresi gecikme tamamlandıktan sonra bir veri dilimi için etkinlik yürütülmesini başlatılır.<br/><br/>Örnek: 00:10: (10 dakika gecikme anlamına gelir) 00 |
| longRetry |Tamsayı<br/><br/>En yüksek değeri: 10 |1 |Dilim yürütme başarısız olmadan önce uzun yeniden deneme sayısı.<br/><br/>longRetry girişimleri tarafından longRetryInterval aralarına aralık eklenir. Yeniden deneme girişimleri arasındaki süre belirtmeniz gerekiyorsa, bu nedenle longRetry kullanın. Yeniden deneme ve longRetry belirtilirse, her bir longRetry denemesi denemeleri içerir ve en fazla deneme sayısını yeniden deneme * longRetry.<br/><br/>Örneğin, biz etkinlik ilkesinde aşağıdaki ayarları varsa:<br/>Yeniden deneme: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Yürütmek için yalnızca bir dilim olduğu varsayılır (Durum Bekliyor) ve Etkinlik yürütme her zaman başarısız olur. Başlangıçta 3 ardışık yürütme deneme olacaktır. Her girişiminden sonra Yeniden Dene'yi dilim durum olacaktır. İlk 3 deneme üzerinden sonra dilim durum LongRetry olur.<br/><br/>Bir saat sonra (diğer bir deyişle, longRetryInteval'ın değeri), 3 ardışık yürütme deneme başka bir dizi olacaktır. Bundan sonra dilim durumu başarısız ve daha fazla yeniden deneme yok denenmesi. Bu nedenle genel 6 deneme yapıldı.<br/><br/>Hiçbir yürütme başarılı olursa, dilim durum hazır olur ve daha fazla yeniden deneme yok çalıştı.<br/><br/>longRetry, belirleyici olmayan zamanlarda bağımlı veri ulaştığında veya genel ortamında hangi veri işleme gerçekleşir altında anormal olduğu durumlarda kullanılabilir. Böyle durumlarda, yeniden deneme birbiri ardından yardımcı yapmak ve sonra bir aralık böylece istenen çıkış sonuçlarında zaman.<br/><br/>Uyarı: longRetry veya longRetryInterval yüksek değerleri ayarlı değil. Genellikle, daha yüksek değerleri sistemle ilgili diğer sorunlar kapsıyor. |
| longRetryInterval |TimeSpan |00:00:00 |Uzun yeniden deneme girişimleri arasında gecikme |

### <a name="typeproperties-section"></a>typeProperties bölümü
Her etkinlik için farklı typeProperties bölümüdür. Dönüştürme etkinlikleri yalnızca türü özellikleri vardır. Bkz: [veri dönüştürme etkinlikleri](#data-transformation-activities) ardışık düzeninde dönüştürme etkinlikleri tanımlayan JSON örnekleri için bu makalenin bölümünde. 

**Kopya etkinliği** typeProperties bölümünde iki alt bölümleri vardır: **kaynak** ve **havuz**. Bkz: [veri DEPOLARINA](#data-stores) veri kullanmayı gösteren JSON örnekleri bir kaynak ve/veya havuz depolamak için bu makaledeki bölüm. 

### <a name="sample-copy-pipeline"></a>Örnek kopyalama işlem hattı
Aşağıdaki örnek işlem hattında, **Etkinlikler** bölümünde **Kopyalama** türünde olan bir etkinlik vardır. Bu örnekte [kopyalama etkinliğini](data-factory-data-movement-activities.md) verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalar. 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde, **türü** **Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
* Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır.
* **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.

Bkz: [veri DEPOLARINA](#data-stores) veri kullanmayı gösteren JSON örnekleri bir kaynak ve/veya havuz depolamak için bu makaledeki bölüm.    

Bu ardışık düzen oluşturma izlenecek tam yol için bkz: [Öğreticisi: veri kopyalama Blob depolama alanından SQL veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="sample-transformation-pipeline"></a>Örnek dönüştürme işlem hattı
Aşağıdaki örnek işlem hattında, **etkinlikler** bölümünde **HDInsightHive** türünde olan bir etkinlik vardır. Bu örnekte [HDInsight Hive etkinliği](data-factory-hive-activity.md), bir Azure HDInsight Hadoop kümesinde Hive betik dosyası çalıştırarak verileri bir Azure Blob depolamadan dönüştürür. 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

Aşağıdaki noktalara dikkat edin: 

* Etkinlikler bölümünde **türü** **HDInsightHive** olarak ayarlanmış yalnızca bir etkinlik vardır.
* **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **AzureStorageLinkedService** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.
* **Tanımlar** bölümü hive betiğine Hive yapılandırma değerleri olarak geçirilir çalışma zamanı ayarlarını belirtmek için kullanılır (örneğin `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Bkz: [veri dönüştürme etkinlikleri](#data-transformation-activities) ardışık düzeninde dönüştürme etkinlikleri tanımlayan JSON örnekleri için bu makalenin bölümünde.

Bu ardışık düzen oluşturma izlenecek tam yol için bkz: [Öğreticisi: Hadoop kümesi kullanarak verileri işlemek için ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md). 

## <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmet tanımı için üst düzey yapısı aşağıdaki gibidir:

```json
{
    "name": "<name of the linked service>",
    "properties": {
        "type": "<type of the linked service>",
        "typeProperties": {
        }
    }
}
```

Tablo Özellikleri JSON tanımını etkinlik içinde açıklanmıştır:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- | 
| ad | Bağlı hizmetin adı. | Evet | 
| özellikleri - türü | Bağlantılı hizmet türü. Örneğin: Azure Storage, Azure SQL veritabanı. |
| typeProperties | TypeProperties bölüm için her bir veri deposu farklı veya ortam işlem öğesine sahip. Bkz: [veri depoları](#datastores) bağlı hizmetler tüm verileri depolamak için bölüm ve [ortamları işlem](#compute-environments) için tüm işlem bağlı Hizmetleri |   

## <a name="dataset"></a>Veri kümesi 
Azure Data Factory kümesinde aşağıdaki gibi tanımlanır:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag to indicate external data. only for input datasets>,
        "linkedServiceName": "<Name of the linked service that refers to a data store.>",
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

Aşağıdaki tabloda yukarıdaki JSON özelliklerinde açıklanmaktadır:   

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| ad | Veri kümesi adı. Bkz: [Azure Data Factory - adlandırma kuralları](data-factory-naming-rules.md) adlandırma kuralları. |Evet |NA |
| type | Veri kümesi türü. Azure Data Factory ile desteklenen türlerden biri belirtin (örneğin: AzureBlob, AzureSqlTable). Bkz: [veri DEPOLARINA](#data-stores) tüm veri depoları ve veri fabrikası tarafından desteklenen veri türleri için bölüm. | 
| yapısı | Veri kümesi şemasını. Bu sütun, türleri, vb. içerir. | Hayır |NA |
| typeProperties | Seçili türüne karşılık gelen özellikler. Bkz: [veri DEPOLARINA](#data-stores) desteklenen türlerini ve bunların özelliklerini bölümü. |Evet |NA |
| external | Bir veri kümesi açıkça data factory işlem hattı tarafından veya üretilen olup olmadığını belirlemek için mantıksal bayrak. |Hayır |false |
| availability | İşleme penceresi veya veri kümesi üretim dilimleme modelini tanımlar. Model dilimleme veri kümesi hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makalesi. |Evet |NA |
| ilke |Ölçüt ya da veri kümesi dilimler karşılamanız gerekmektedir koşulu tanımlar. <br/><br/>Ayrıntılar için bkz [Dataset İlkesi](#Policy) bölümü. |Hayır |NA |

Her sütunda **yapısı** bölüm, aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü.  |Hayır |
| Kültür |.NET tabanlı türü belirtilir ve .NET türü olduğunda kullanılacak kültürü `Datetime` veya `Datetimeoffset`. `en-us` varsayılan değerdir. |Hayır |
| Biçimi |Biçim türü belirtilir ve .NET türü olduğunda kullanılacak dize `Datetime` veya `Datetimeoffset`. |Hayır |

Aşağıdaki örnekte, üç sütun kümesi sahip `slicetimestamp`, `projectname`, ve `pageviews` ve bunlar türü: dize, dize ve ondalık sırasıyla.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Aşağıdaki tabloda, kullanabileceğiniz özellikleri açıklanmaktadır **kullanılabilirlik** bölümü:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilim üretim için zaman birimini belirtir.<br/><br/><b>Sıklık desteklenen</b>: dakika, saat, gün, hafta, ay |Evet |NA |
| interval |Sıklığı çarpanı belirtir<br/><br/>"X sıklığı aralığını" ne sıklıkta dilim üretilen belirler.<br/><br/>Saatlik olarak başka bir dilimlenebilir dataset gerekiyorsa, ayarladığınız <b>sıklığı</b> için <b>saat</b>, ve <b>aralığı</b> için <b>1</b>.<br/><br/><b>Not</b>: sıklığını dakika belirtirseniz, aralık en az 15'e ayarlayın öneririz |Evet |NA |
| stili |Dilim aralığı başlangıç/bitiş üretilen olup olmadığını belirtir.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Sıklık aya ayarlanır ve stil EndOfInterval için dilim ayın son gününde üretilmez. Stil StartOfInterval için ayarlarsanız, dilim ayın ilk günü oluşturulur.<br/><br/>Sıklığı gün olarak ayarlanır ve stili için EndOfInterval dilim günün son bir saatte üretilmez.<br/><br/>Sıklık saate ayarlanır ve stil EndOfInterval için dilim saat sonunda üretilmez. Örneğin, 13'te – 2 PM dönem için bir dilim için 2 saat dilimi oluşturulur. |Hayır |EndOfInterval |
| anchorDateTime |Veri kümesi dilim sınırlarını işlem için Zamanlayıcı tarafından kullanılan zaman içinde mutlak konum tanımlar. <br/><br/><b>Not</b>: AnchorDateTime sıklığından daha ayrıntılı tarih bölümden oluşur sonra daha ayrıntılı bölümleri göz ardı edilir. <br/><br/>Örneğin, varsa <b>aralığı</b> olan <b>saatlik</b> (sıklığı: saat ve aralığı: 1) ve <b>AnchorDateTime</b> içeren <b>dakika ve saniyeleri</b> sonra <b>dakika ve saniyeleri</b> AnchorDateTime bölümlerini yok sayılır. |Hayır |01/01/0001 |
| uzaklık |Tarafından başlangıç ve bitiş tüm veri kümesi dilim gölgeye Timespan. <br/><br/><b>Not</b>: anchorDateTime ve uzaklık belirtilirse, birleştirilmiş shift sonucudur. |Hayır |NA |

Aşağıdaki kullanılabilirlik bölümü çıktı veri kümesi üretilen saatlik (veya) giriş olduğunu belirtir dataset kullanılabilir saatlik:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

**İlkesi** veri kümesi tanımı bölümünde ölçütleri veya veri kümesi dilimler karşılamanız gerekmektedir koşulu tanımlar.

| İlke Adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Doğrular verilerde bir **Azure blob** (megabayt cinsinden) en düşük boyut gereksinimlerini karşılıyor. |Azure Blob |Hayır |NA |
| minimumRows |Doğrular verilerde bir **Azure SQL veritabanı** veya bir **Azure tablo** en az satır sayısını içerir. |<ul><li>Azure SQL Database</li><li>Azure Tablosu</li></ul> |Hayır |NA |

**Örnek:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

Azure Data Factory tarafından üretilen bir veri kümesi sürece bunu olarak işaretlenmelidir **dış**. Etkinlik veya ardışık düzen zincirleme kullanılmadığı sürece bu ayar genellikle bir ardışık düzendeki ilk etkinlik girişleri için geçerlidir.

| Ad | Açıklama | Gerekli | Varsayılan Değer |
| --- | --- | --- | --- |
| dataDelay |Verilen dilim için dış veri kullanılabilirliğini onay gecikme süresi. Verilerin saatlik varsa, örneğin, dış veri kullanılabilir ve karşılık gelen dilimi hazır denetleyin dataDelay kullanılarak ertelenebilir.<br/><br/>Yalnızca mevcut süre için geçerlidir.  Örneğin, 1:00 PM şimdi ise ve bu değer 10 dakikadır doğrulama 13: 10'te başlatır.<br/><br/>Bu ayar, geçmişte dilimler etkilemez (dilimler dilim bitiş saati + dataDelay < şimdi) herhangi bir gecikme işlenir.<br/><br/>Saat 23:59 saatleri gereken kullanarak belirtilen büyük `day.hours:minutes:seconds` biçimi. Örneğin, 24 saat belirtmek için 24:00:00 kullanmayın; Bunun yerine, 1.00:00:00 kullanın. 24:00:00 kullanırsanız, 24 gün (24.00:00:00) kabul edilir. 1 gün ve 4 saat için 1:04:00:00 belirtin. |Hayır |0 |
| retryInterval |Bir hata ile sonraki arasındaki bekleme süresini girişimi yeniden deneyin. Bir deneme başarısız olursa, sonraki deneyin sonra Retryınterval olur. <br/><br/>1:00 PM şimdi ise, ilk denemede başlamadan. İlk doğrulama denetimi tamamlamak için süre 1 dakika ve işlem başarısız oldu, sonraki yeniden deneme ise 1:00 1 dak (süresi) + 1 dak (yeniden deneme aralığı) = 1:02 PM. <br/><br/>Geçmişte dilimler için gecikme yoktur. Yeniden deneme hemen gerçekleşir. |Hayır |00:01:00 (1 minute) |
| retryTimeout |Yeniden deneme girişimleri için zaman aşımı.<br/><br/>Bu özellik 10 dakika olarak ayarlanırsa, doğrulama 10 dakika içinde tamamlanması gerekir. Yeniden deneme doğrulamayı gerçekleştirmek için 10 dakikadan uzun sürüyorsa, zaman aşımına uğradı.<br/><br/>Tüm denemeleri doğrulama için zaman aşımına uğrarsa, dilim süresi sona erdi işaretlenir. |Hayır |00:10:00 (10 dakika) |
| maximumRetry |Dış veri kullanılabilirliğini denetleyin sayısı. İzin verilen en büyük değer 10'dur. |Hayır |3 |


## <a name="data-stores"></a>VERİ DEPOLARI
[Bağlantılı hizmeti](#linked-service) tüm türleri bağlı hizmetler için ortak olan JSON öğeleri için sağlanan bölüm açıklamalar. Bu bölümde her veri deposuna belirli JSON öğeleri hakkında ayrıntılar sağlar.

[Dataset](#dataset) veri kümeleri tüm türleri için ortak olan JSON öğeleri için sağlanan bölüm açıklamalar. Bu bölümde her veri deposuna belirli JSON öğeleri hakkında ayrıntılar sağlar.

[Etkinlik](#activity) tüm türleri etkinlikler için ortak olan JSON öğeleri için sağlanan bölüm açıklamalar. Bu bölümde bir kopyalama etkinliği kaynak/havuz olarak kullanıldığında, her veri deposuna özel JSON öğeleri hakkında ayrıntılar sağlar.  

Bağlantılı hizmeti, veri kümesi ve kaynak/havuz kopyalama etkinliği için JSON şemaları görmek ilgilendiğiniz deposu için bağlantıya tıklayın.

| Kategori | Veri deposu 
|:--- |:--- |
| **Azure** |[Azure Blob Depolama](#azure-blob-storage) |
| &nbsp; |[Azure Data Lake Store](#azure-datalake-store) |
| &nbsp; |[Azure Cosmos DB](#azure-cosmos-db) |
| &nbsp; |[Azure SQL Veritabanı](#azure-sql-database) |
| &nbsp; |[Azure SQL Veri Ambarı](#azure-sql-data-warehouse) |
| &nbsp; |[Azure Search](#azure-search) |
| &nbsp; |[Azure Tablo Depolama](#azure-table-storage) |
| **Veritabanları** |[Amazon Redshift](#amazon-redshift) |
| &nbsp; |[IBM DB2](#ibm-db2) |
| &nbsp; |[MySQL](#mysql) |
| &nbsp; |[Oracle](#oracle) |
| &nbsp; |[PostgreSQL](#postgresql) |
| &nbsp; |[SAP Business Warehouse](#sap-business-warehouse) |
| &nbsp; |[SAP HANA](#sap-hana) |
| &nbsp; |[SQL Server](#sql-server) |
| &nbsp; |[Sybase](#sybase) |
| &nbsp; |[Teradata](#teradata) |
| **NoSQL** |[Cassandra](#cassandra) |
| &nbsp; |[MongoDB](#mongodb) |
| **Dosya** |[Amazon S3](#amazon-s3) |
| &nbsp; |[Dosya Sistemi](#file-system) |
| &nbsp; |[FTP](#ftp) |
| &nbsp; |[HDFS](#hdfs) |
| &nbsp; |[SFTP](#sftp) |
| **Diğer** |[HTTP](#http) |
| &nbsp; |[OData](#odata) |
| &nbsp; |[ODBC](#odbc) |
| &nbsp; |[Salesforce](#salesforce) |
| &nbsp; |[Web tablo](#web-table) |

## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmetler iki tür vardır: Azure depolama bağlantılı hizmeti ve Azure depolama SAS bağlantılı hizmeti.

#### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
Azure storage hesabınızı kullanarak bir data factory'ye bağlamak için **hesap anahtarı**, bir Azure Storage bağlı hizmeti oluşturma. Bağlantılı hizmeti bir Azure depolama alanını tanımlayın, Ayarla **türü** bağlantılı hizmetinin **AzureStorage**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| connectionString |ConnectionString özelliği için Azure depolama alanına bağlanmak için gereken bilgileri belirtin. |Evet |

##### <a name="example"></a>Örnek  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a>Azure Storage SAS bağlı hizmeti
Azure depolama bağlı SAS hizmeti bir paylaşılan erişim imzası (SAS) kullanarak Azure data factory için bir Azure depolama hesabı bağlantı sağlar. Veri Fabrikası depolama alanındaki tüm/özel kaynakları (blob/kapsayıcısı) kısıtlanmış/zaman sınırlı erişim sağlar. Paylaşılan erişim imzası kullanarak bir veri fabrikası Azure depolama hesabınıza bağlamak için bir Azure depolama bağlı SAS hizmeti oluşturun. Bağlantılı hizmeti bir Azure depolama SAS tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureStorageSas**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:   

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| sasUri |Blob, kapsayıcı ya da tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI belirtin. |Evet |

##### <a name="example"></a>Örnek

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz: [Azure Blob Storage bağlayıcı](data-factory-azure-blob-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Azure Blob veri kümesini tanımlamak için **türü** için veri kümesinin **AzureBlob**. Ardından aşağıdaki Azure Blob belirli özelliklerinde belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Kapsayıcı ve klasöre blob depolamada yolu. Örnek: myblobcontainer\myblobfolder\ |Evet |
| fileName |Blob adı. isteğe bağlıdır ve büyük küçük harfe duyarlı dosya adıdır.<br/><br/>Bir dosya adı belirtirseniz, (kopyalama dahil) etkinlik belirli Blob üzerinde çalışır.<br/><br/>Dosya adı belirtilmediğinde kopyalama tüm BLOB'lar girdi veri kümesi için folderPath içerir.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır Bu biçim: veri. <Guid>.txt (örnek:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy isteğe bağlı bir özelliktir. Bir dinamik folderPath ve zaman serisi verileri için dosya adı belirtmek için kullanabilirsiniz. Örneğin, folderPath için verileri saatte parametreli olabilir. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
 ```


Daha fazla bilgi için bkz: [Azure Blob bağlayıcı](data-factory-azure-blob-connector.md#dataset-properties) makalesi.

### <a name="blobsource-in-copy-activity"></a>Kopyalama etkinliğinde BlobSource
Bir Azure Blob depolama alanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **BlobSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |(Varsayılan değer) false değerini true |Hayır |

#### <a name="example-blobsource"></a>Örnek: **BlobSource**
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a>Kopyalama etkinliğinde BlobSink
Bir Azure Blob depolama alanına veri kopyalıyorsanız ayarlamak **Havuz türü** kopyalama etkinliği **BlobSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |<b>PreserveHierarchy</b>: Dosya hiyerarşisi hedef klasördeki korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yolunu aynıdır.<br/><br/><b>FlattenHierarchy</b>: tüm kaynak klasörü hedef klasör ilk düzeyi dosyalarıdır. Hedef dosyalar otomatik adına sahip. <br/><br/><b>MergeFiles (varsayılan):</b> bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilirse, belirtilen ad olur; Aksi takdirde otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

#### <a name="example-blobsink"></a>Örnek: BlobSink

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure Blob bağlayıcı](data-factory-azure-blob-connector.md#copy-activity-properties) makalesi. 

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmeti bir Azure Data Lake Store tanımlamak için bağlantılı hizmet türüne ayarlayın **AzureDataLakeStore**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureDataLakeStore** | Evet |
| dataLakeStoreUri | Azure Data Lake Store hesabı bilgilerini belirtin. Aşağıdaki biçimdedir: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store ait olduğu azure abonelik kimliği. | Havuz için gerekli |
| resourceGroupName | Data Lake Store ait olduğu azure kaynak grubu adı. | Havuz için gerekli |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet (hizmet sorumlusu kimlik doğrulaması için) |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. | Evet (hizmet sorumlusu kimlik doğrulaması için) |
| kiracı | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet (hizmet sorumlusu kimlik doğrulaması için) |
| Yetkilendirme | Tıklatın **Authorize** düğmesini **Data Factory düzenleyici** ve bu özelliği otomatik olarak oluşturulan yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet (kullanıcı kimlik bilgileri doğrulaması için)|
| SessionID | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyici kullandığınızda bu ayarı otomatik olarak oluşturulur. | Evet (kullanıcı kimlik bilgileri doğrulaması için) |

#### <a name="example-using-service-principal-authentication"></a>Örnek: hizmet asıl kimlik doğrulaması kullanma
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a>Örnek: kullanıcı kimlik bilgilerinin kullanma
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Azure Data Lake Store veri kümesini tanımlamak için **türü** için veri kümesinin **AzureDataLakeStore**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| folderPath |Kapsayıcı ve Azure Data Lake klasörü yoluna depolar. |Evet |
| fileName |Azure Data Lake Store'da dosyasının adı. isteğe bağlıdır ve büyük küçük harfe duyarlı dosya adıdır. <br/><br/>Bir dosya adı belirtirseniz, (kopyalama dahil) etkinlik belirli bir dosya üzerinde çalışır.<br/><br/>FileName belirtilmediğinde kopyalama folderPath girdi veri kümesi için tüm dosyaları içerir.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır Bu biçim: veri. <Guid>.txt (örnek:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy isteğe bağlı bir özelliktir. Bir dinamik folderPath ve zaman serisi verileri için dosya adı belirtmek için kullanabilirsiniz. Örneğin, folderPath için verileri saatte parametreli olabilir. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

#### <a name="example"></a>Örnek
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#dataset-properties) makalesi. 

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Kopya etkinliği Azure Data Lake Store kaynağında
Bir Azure Data Lake Deposu'ndan veri veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **AzureDataLakeStoreSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

**AzureDataLakeStoreSource** aşağıdaki özellikleri destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |(Varsayılan değer) false değerini true |Hayır |

#### <a name="example-azuredatalakestoresource"></a>Örnek: AzureDataLakeStoreSource

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#copy-activity-properties) makalesi.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Kopya etkinliği Azure Data Lake Store havuzunda
Bir Azure Data Lake Store'a veri kopyalamak istiyorsanız, ayarlayın **Havuz türü** kopyalama etkinliği **AzureDataLakeStoreSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kopyalama davranışını belirtir. |<b>PreserveHierarchy</b>: Dosya hiyerarşisi hedef klasördeki korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yolunu aynıdır.<br/><br/><b>FlattenHierarchy</b>: tüm dosyaları kaynak klasörden hedef klasöre ilk düzeyi oluşturulur. Hedef dosyalar otomatik adıyla oluşturulur.<br/><br/><b>MergeFiles</b>: bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilirse, belirtilen ad olur; Aksi takdirde otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

#### <a name="example-azuredatalakestoresink"></a>Örnek: AzureDataLakeStoreSink
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#copy-activity-properties) makalesi. 

## <a name="azure-cosmos-db"></a>Azure Cosmos DB  

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmeti bir Azure Cosmos DB tanımlamak için Ayarla **türü** bağlantılı hizmetinin **DocumentDb**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| connectionString |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
Daha fazla bilgi için bkz: [Azure Cosmos DB bağlayıcı](data-factory-azure-documentdb-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure Cosmos DB veri kümesini tanımlamak için **türü** için veri kümesinin **DocumentDbCollection**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| collectionName |Azure Cosmos DB koleksiyonunun adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
Daha fazla bilgi için bkz: [Azure Cosmos DB bağlayıcı](data-factory-azure-documentdb-connector.md#dataset-properties) makalesi.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Kopya etkinliği Azure Cosmos DB koleksiyon kaynağında
Bir Azure Cosmos Veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **DocumentDbCollectionSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için sorgu belirtin. |Sorgu dizesindeki Azure Cosmos DB tarafından desteklenir. <br/><br/>Örnek: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmezse, yürütülen SQL deyimi: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş belirtmek için özel karakter |Herhangi bir karakter. <br/><br/>Azure Cosmos DB iç içe geçmiş yapılar burada izin verilen bir NoSQL JSON belgeleri için deposudur. Azure Data Factory sağlar hiyerarşisi olan nestingSeparator aracılığıyla belirtmek kullanıcı "." Yukarıdaki örneklerde. Ayırıcı olmadan kopyalama etkinliği üç alt öğeleri "Ad" nesnesiyle ilk olarak, oluşturacak Orta ve son "Name.First", "Name.Middle" ve "Name.Last" Tablo tanımında göre. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Kopya etkinliği Azure Cosmos DB koleksiyonu havuzunda
Azure Cosmos Veritabanına veri kopyalamak istiyorsanız, ayarlayın **Havuz türü** kopyalama etkinliği **DocumentDbCollectionSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| nestingSeparator |Bir özel karakter iç içe geçmiş belge belirtmek için kaynak sütun adı gereklidir. <br/><br/>Örneğin yukarıdaki: `Name.First` çıktıda tablo Cosmos DB belgede aşağıdaki JSON yapısını oluşturur:<br/><br/>"Name": {<br/>    "First": "John"<br/>}, |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir.<br/><br/>Varsayılan değer `.` (nokta). |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir. <br/><br/>Varsayılan değer `.` (nokta). |
| writeBatchSize |Azure Cosmos DB hizmeti belgeleri oluşturmak için paralel isteklerin sayısı.<br/><br/>Bu özelliği kullanarak Azure Cosmos DB öğesine/öğesinden veri kopyalama işlemi sırasında performans ince ayar yapabilirsiniz. Daha fazla paralel istekler için Azure Cosmos DB'ye gönderildiğinden writeBatchSize artırdığınızda daha iyi bir performans düşüklüğü görebilir. Ancak, azaltma önlemek gerekir, hata iletisi atabilirsiniz: "oranıdır büyük istek".<br/><br/>Azaltmayı bir dizi etkene, belgeler, belgeleri koşullarını sayısı boyutunu dahil olmak üzere, hedef koleksiyon, vb. İlkesi dizin tarafından belirlenir. Kopyalama işlemleri için en çok kullanılabilir verimlilik sağlamak için daha iyi bir koleksiyonunu (örneğin, S3) kullanabilirsiniz (2.500 istek birimleri/saniye). |Tamsayı |Hayır (varsayılan: 5) |
| writeBatchTimeout |İşlemin zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

Daha fazla bilgi için bkz: [Azure Cosmos DB bağlayıcı](data-factory-azure-documentdb-connector.md#copy-activity-properties) makalesi.

## <a name="azure-sql-database"></a>Azure SQL Database

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmeti bir Azure SQL veritabanı tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureSqlDatabase**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| connectionString |ConnectionString özelliği için Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL bağlayıcı](data-factory-azure-sql-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Azure SQL veritabanı veri kümesini tanımlamak için **türü** için veri kümesinin **AzureSqlTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm bağlantılı hizmetinin Azure SQL veritabanı örneğinde başvurduğu adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için bkz: [Azure SQL bağlayıcı](data-factory-azure-sql-connector.md#dataset-properties) makalesi. 

### <a name="sql-source-in-copy-activity"></a>Kopyalama etkinliğinde SQL kaynağı
Bir Azure SQL veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **SqlSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. |Saklı yordam adı. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Daha fazla bilgi için bkz: [Azure SQL bağlayıcı](data-factory-azure-sql-connector.md#copy-activity-properties) makalesi. 

### <a name="sql-sink-in-copy-activity"></a>Kopyalama etkinliği SQL havuzunda
Azure SQL veritabanına veri kopyalamak istiyorsanız, ayarlayın **Havuz türü** kopyalama etkinliği **SqlSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Sorgu bildirimi. |Hayır |
| sliceIdentifierColumnName |Ne zaman yeniden çalıştırılacağını belirli bir dilim verileri temizlemek için kullanılan otomatik dilim tanımlayıcı doldurmak kopyalama etkinliği için bir sütun adı belirtin. |Binary(32) veri türüne sahip bir sütunun sütun adı. |Hayır |
| sqlWriterStoredProcedureName |Saklı yordam adı hedef tabloda bu upserts (güncelleştirmeler/ekler) verileri. |Saklı yordam adı. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği taşınan veri geçici bir tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu ardından var olan verilerle kopyalanan verileri birleştirebilirsiniz. |Bir tablo türü adı. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL bağlayıcı](data-factory-azure-sql-connector.md#copy-activity-properties) makalesi. 

## <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin Azure SQL Data Warehouse tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureSqlDW**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| connectionString |ConnectionString özelliği için Azure SQL Data Warehouse örneğine bağlanmak için gereken bilgileri belirtin. |Evet |



#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Azure SQL Data Warehouse veri kümesini tanımlamak için **türü** için veri kümesinin **AzureSqlDWTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL veri ambarı veritabanındaki adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) makalesi. 

### <a name="sql-dw-source-in-copy-activity"></a>Kopyalama etkinliğinde SQL DW kaynağı
Azure SQL veri ambarından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **SqlDWSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. |Saklı yordam adı. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) makalesi. 

### <a name="sql-dw-sink-in-copy-activity"></a>SQL DW havuz kopyalama etkinliğinde
Azure SQL Data Warehouse'a veri kopyalıyorsanız ayarlamak **Havuz türü** kopyalama etkinliği **SqlDWSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Sorgu bildirimi. |Hayır |
| allowPolyBase |BULKINSERT mekanizması yerine PolyBase (varsa) kullanılıp kullanılmayacağını belirtir. <br/><br/> **PolyBase kullanarak SQL Data Warehouse'a veri yükleme için önerilen yoldur.** |True <br/>False (varsayılan) |Hayır |
| polyBaseSettings |Bir grup olabilir özellik belirtilen **Bulunan'allowpolybase** özelliği ayarlanmış **doğru**. |&nbsp; |Hayır |
| rejectValue |Sayı veya yüzde değeri sorgu başarısız önce reddedilemiyor satır belirtir. <br/><br/>PolyBase'nın reddetme seçenekleri hakkında daha fazla bilgi **bağımsız değişkenleri** bölümünü [CREATE dış TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) konu. |0 (varsayılan), 1, 2... |Hayır |
| rejectType |RejectValue seçeneği bir hazır değer veya bir yüzde belirtilen belirtir. |Değer (varsayılan), yüzde |Hayır |
| rejectSampleValue |PolyBase reddedilen satırları yüzdesini yeniden hesaplar önce almak için satır sayısını belirler. |1, 2, … |Evet, varsa **rejectType** olan **yüzdesi** |
| useTypeDefault |PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünde bu özelliği hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |TRUE, False (varsayılan) |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) makalesi. 

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmeti bir Azure Search tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureSearch**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| url | Azure Search hizmeti için URL. | Evet |
| anahtar | Azure Search hizmeti için yönetici anahtarı. | Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure Search Bağlayıcısı](data-factory-azure-search-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure Search veri kümesini tanımlamak için **türü** için veri kümesinin **AzureSearchIndex**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Type özelliği ayarlamak **AzureSearchIndex**.| Evet |
| indexName | Azure Search dizini adı. Veri Fabrikası dizinini oluşturmaz. Azure Search'te dizin mevcut olması gerekir. | Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure Search Bağlayıcısı](data-factory-azure-search-connector.md#dataset-properties) makalesi.

### <a name="azure-search-index-sink-in-copy-activity"></a>Kopya etkinliği Azure arama dizini havuzunda
Bir Azure Search dizinine veri kopyalıyorsanız ayarlamak **Havuz türü** kopyalama etkinliği **AzureSearchIndexSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Birleştir veya bir belge dizinde zaten mevcut olduğunda Değiştir belirtir. | Merge (varsayılan)<br/>Karşıya Yükle| Hayır |
| WriteBatchSize | Arabellek boyutu writeBatchSize ulaştığında Azure Search dizinine veri yükler. | 1 için 1.000. Varsayılan değer 1000'dir. | Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Azure Search Bağlayıcısı](data-factory-azure-search-connector.md#copy-activity-properties) makalesi.

## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmetler iki tür vardır: Azure depolama bağlantılı hizmeti ve Azure depolama SAS bağlantılı hizmeti.

#### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
Azure storage hesabınızı kullanarak bir data factory'ye bağlamak için **hesap anahtarı**, bir Azure Storage bağlı hizmeti oluşturma. Bağlantılı hizmeti bir Azure depolama alanını tanımlayın, Ayarla **türü** bağlantılı hizmetinin **AzureStorage**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorage** |Evet |
| connectionString |ConnectionString özelliği için Azure depolama alanına bağlanmak için gereken bilgileri belirtin. |Evet |

**Örnek:**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a>Azure Storage SAS bağlı hizmeti
Azure depolama bağlı SAS hizmeti bir paylaşılan erişim imzası (SAS) kullanarak Azure data factory için bir Azure depolama hesabı bağlantı sağlar. Veri Fabrikası depolama alanındaki tüm/özel kaynakları (blob/kapsayıcısı) kısıtlanmış/zaman sınırlı erişim sağlar. Paylaşılan erişim imzası kullanarak bir veri fabrikası Azure depolama hesabınıza bağlamak için bir Azure depolama bağlı SAS hizmeti oluşturun. Bağlantılı hizmeti bir Azure depolama SAS tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureStorageSas**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:   

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorageSas** |Evet |
| sasUri |Blob, kapsayıcı ya da tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI belirtin. |Evet |

**Örnek:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz: [Azure Table Storage bağlayıcı](data-factory-azure-table-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Azure Table veri kümesini tanımlamak için **türü** için veri kümesinin **AzureTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlantılı hizmet başvurduğu Azure tablo veritabanı örneğinde tablonun adı. |Evet. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Bir azureTableSourceQuery de belirtilirse, sorgu karşılayan tablodaki kayıtları hedefe kopyalanır. |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz: [Azure Table Storage bağlayıcı](data-factory-azure-table-connector.md#dataset-properties) makalesi. 

### <a name="azure-table-source-in-copy-activity"></a>Kopya etkinliği Azure tablo kaynağında
Azure tablo depolamasından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **AzureTableSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableSourceQuery |Verileri okumak için özel sorgu kullanın. |Azure tablo sorgu dizesi. Sonraki bölümdeki örneklere bakın. |Hayır. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Bir azureTableSourceQuery de belirtilirse, sorgu karşılayan tablodaki kayıtları hedefe kopyalanır. |
| azureTableSourceIgnoreTableNotFound |Swallow tablosunun özel durum var olup olmadığını gösterir. |TRUE<br/>FALSE |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz: [Azure Table Storage bağlayıcı](data-factory-azure-table-connector.md#copy-activity-properties) makalesi. 

### <a name="azure-table-sink-in-copy-activity"></a>Kopya etkinliği Azure tablo havuzunda
Azure Table depolama alanına veri kopyalıyorsanız ayarlamak **Havuz türü** kopyalama etkinliği **AzureTableSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Havuzu tarafından kullanılan varsayılan bölüm anahtar değeri. |Bir dize değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütun adını belirtin. Belirtilmezse, AzureTableDefaultPartitionKeyValue bölüm anahtarı olarak kullanılır. |Sütun adı. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Sütun adı. |Hayır |
| azureTableInsertType |Azure tabloya veri ekleme modu.<br/><br/>Bu özellik, var olan satırları eşleşen bölüm ve satır anahtarlarla çıkış tablosuna birleştirilmiş veya değiştirilmesi değerlerine sahip olup olmadığını denetler. <br/><br/>Bu ayarları (birleştirme ve Değiştir) nasıl çalıştığı hakkında bilgi edinmek için [ekleme veya birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [ekleme veya değiştirme varlık](https://msdn.microsoft.com/library/azure/hh452242.aspx) Konular. <br/><br> Bu ayar, tablo düzeyinde değil satır düzeyinde uygulanır ve girdide var olmayan satır çıkış tablosuna hiçbiri seçeneği siler. |Merge (varsayılan)<br/>Değiştir |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler. |TimeSpan<br/><br/>Örnek: "00: 20:00" (20 dakika) |Hayır (depolama İstemcisi varsayılan zaman aşımı için varsayılan değer 90 saniye) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz: [Azure Table Storage bağlayıcı](data-factory-azure-table-connector.md#copy-activity-properties) makalesi. 

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir Amazon Redshift tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AmazonRedshift**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |IP adresi veya ana bilgisayar Amazon Redshift sunucunun adıdır. |Evet |
| port |İstemci bağlantılarını dinlemek için Amazon Redshift sunucunun kullandığı TCP bağlantı noktası numarası. |Hayır, varsayılan değer: 5439 |
| veritabanı |Amazon Redshift veritabanının adı. |Evet |
| kullanıcı adı |Veritabanına erişimi olan kullanıcı adı. |Evet |
| password |Kullanıcı hesabının parolası. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

Daha fazla bilgi için bkz: [Amazon Redshift bağlayıcı](#data-factory-amazon-redshift-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Amazon Redshift veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Amazon Redshift veritabanında bağlantılı hizmet başvurduğu tablonun adı. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |


#### <a name="example"></a>Örnek

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Daha fazla bilgi için bkz: [Amazon Redshift bağlayıcı](#data-factory-amazon-redshift-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında 
Amazon Redshift veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **tableName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Daha fazla bilgi için bkz: [Amazon Redshift bağlayıcı](#data-factory-amazon-redshift-connector.md#copy-activity-properties) makalesi.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir IBM DB2 tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OnPremisesDB2**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |DB2 sunucunun adıdır. |Evet |
| veritabanı |DB2 veritabanının adı. |Evet |
| Şema |Veritabanı şemasında adı. Şema adı büyük/küçük harf duyarlıdır. |Hayır |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi DB2 veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Daha fazla bilgi için bkz: [IBM DB2 Bağlayıcısı](#data-factory-onprem-db2-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir DB2 veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |DB2 veritabanına örneğinde bağlantılı hizmet başvurduğu tablonun adı. TableName büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) 

#### <a name="example"></a>Örnek
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [IBM DB2 Bağlayıcısı](#data-factory-onprem-db2-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
IBM DB2'den veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `"query": "select * from "MySchema"."MyTable""`. |Hayır (varsa **tableName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Daha fazla bilgi için bkz: [IBM DB2 Bağlayıcısı](#data-factory-onprem-db2-connector.md#copy-activity-properties) makalesi.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir MySQL tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OnPremisesMySql**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |MySQL sunucu adı. |Evet |
| veritabanı |MySQL veritabanının adı. |Evet |
| Şema |Veritabanı şemasında adı. |Hayır |
| authenticationType |MySQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: `Basic`. |Evet |
| kullanıcı adı |MySQL veritabanına bağlanmak için kullanıcı adını belirtin. |Evet |
| password |Belirttiğiniz kullanıcı hesabı için parola belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi MySQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

Daha fazla bilgi için bkz: [MySQL bağlayıcı](data-factory-onprem-mysql-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir MySQL veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |MySQL veritabanı örneğinde bağlantılı hizmet başvurduğu tablonun adı. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için bkz: [MySQL bağlayıcı](data-factory-onprem-mysql-connector.md#dataset-properties) makalesi. 

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Bir MySQL veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **tableName** , **dataset** belirtilir) |


#### <a name="example"></a>Örnek
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [MySQL bağlayıcı](data-factory-onprem-mysql-connector.md#copy-activity-properties) makalesi. 

## <a name="oracle"></a>Oracle 

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir Oracle tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OnPremisesOracle**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| driverType | Hangi sürücünün/Oracle veritabanına veri kopyalamak için kullanılacağını belirtin. İzin verilen değerler **Microsoft** veya **ODP** (varsayılan). Bkz: [desteklenen sürümü ve yükleme](#supported-versions-and-installation) sürücü ayrıntıları bölümü. | Hayır |
| connectionString | ConnectionString özelliği için Oracle veritabanı örneğine bağlanmak için gereken bilgileri belirtin. | Evet |
| gatewayName | Ağ geçidinin adı, şirket içi Oracle sunucusuna bağlanmak için kullanılır |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Oracle veri kümesini tanımlamak için **türü** için veri kümesinin **OracleTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Oracle veritabanında bağlantılı hizmet başvurduğu tablonun adı. |Hayır (varsa **oracleReaderQuery** , **OracleSource** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için bkz: [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#dataset-properties) makalesi.

### <a name="oracle-source-in-copy-activity"></a>Kopyalama etkinliği Oracle kaynağında
Bir Oracle veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **OracleSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin, `select * from MyTable` <br/><br/>Belirtilmezse, yürütülen SQL deyimi: `select * from MyTable` |Hayır (varsa **tableName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#copy-activity-properties) makalesi.

### <a name="oracle-sink-in-copy-activity"></a>Kopyalama etkinliği Oracle havuzunda
Am Oracle veritabanına veri kopyalamak istiyorsanız, ayarlayın **Havuz türü** kopyalama etkinliği **OracleSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: 00:30:00 (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Sorgu bildirimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilim verileri temizlemek için kullanılan otomatik dilim tanımlayıcı doldurmak için sütun adı belirtin. |Binary(32) veri türüne sahip bir sütunun sütun adı. |Hayır |

#### <a name="example"></a>Örnek
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Daha fazla bilgi için bkz: [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#copy-activity-properties) makalesi.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir PostgreSQL tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OnPremisesPostgreSql**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |PostgreSQL sunucunun adıdır. |Evet |
| veritabanı |PostgreSQL veritabanının adı. |Evet |
| Şema |Veritabanı şemasında adı. Şema adı büyük/küçük harf duyarlıdır. |Hayır |
| authenticationType |PostgreSQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi PostgreSQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Daha fazla bilgi için bkz: [PostgreSQL bağlayıcı](data-factory-onprem-postgresql-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir PostgreSQL veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlantılı hizmet başvurduğu PostgreSQL veritabanı örneğinde tablonun adı. TableName büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

#### <a name="example"></a>Örnek
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için bkz: [PostgreSQL bağlayıcı](data-factory-onprem-postgresql-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Bir PostgreSQL veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: "sorgu": "seçin * gelen \"MySchema\".\" MyTable\"". |Hayır (varsa **tableName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [PostgreSQL bağlayıcı](data-factory-onprem-postgresql-connector.md#copy-activity-properties) makalesi.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin SAP Business Warehouse (BW) tanımlamak için Ayarla **türü** bağlantılı hizmetinin **SapBw**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP BW örneği bulunduğu sunucunun adıdır. | string | Evet
systemNumber | SAP BW sisteminin sistem numarası. | Bir dize olarak gösterilen iki basamaklı ondalık sayı. | Evet
clientId | SAP W sistem istemcisinde istemci kimliği. | Bir dize olarak gösterilen üç basamaklı ondalık sayı. | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | string | Evet
password | Kullanıcının parolası. | string | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP BW örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
encryptedCredential | Şifrelenmiş kimlik bilgileri dizesi. | string | Hayır

#### <a name="example"></a>Örnek

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için bkz: [SAP Business Warehouse Bağlayıcısı](data-factory-sap-business-warehouse-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
SAP BW veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**. SAP BW veri kümesi türü için desteklenen türüne özgü özellikler yok **RelationalTable**.  

#### <a name="example"></a>Örnek

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Daha fazla bilgi için bkz: [SAP Business Warehouse Bağlayıcısı](data-factory-sap-business-warehouse-connector.md#dataset-properties) makalesi. 

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
SAP Business Warehouse veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP BW örneğinden verileri okumak için MDX Sorgusu belirtir. | MDX Sorgusu. | Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [SAP Business Warehouse Bağlayıcısı](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) makalesi. 

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin SAP HANA tanımlamak için Ayarla **türü** bağlantılı hizmetinin **SapHana**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP HANA örneği bulunduğu sunucunun adıdır. Sunucunuz özelleştirilmiş bir bağlantı noktası kullanıyorsa belirtin `server:port`. | string | Evet
authenticationType | Kimlik doğrulama türü. | Dize. "Temel" veya "Windows" | Evet 
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | string | Evet
password | Kullanıcının parolası. | string | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP HANA örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
encryptedCredential | Şifrelenmiş kimlik bilgileri dizesi. | string | Hayır

#### <a name="example"></a>Örnek

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
Daha fazla bilgi için bkz: [SAP HANA bağlayıcı](data-factory-sap-hana-connector.md#linked-service-properties) makalesi.
 
### <a name="dataset"></a>Veri kümesi
SAP HANA veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**. SAP HANA veri kümesi türü için desteklenen türüne özgü özellikler yok **RelationalTable**. 

#### <a name="example"></a>Örnek

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Daha fazla bilgi için bkz: [SAP HANA bağlayıcı](data-factory-sap-hana-connector.md#dataset-properties) makalesi. 

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
SAP HANA veri deposundan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP HANA örneğinden verileri okumak için SQL sorgusu belirtir. | SQL sorgusu. | Evet |


#### <a name="example"></a>Örnek


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [SAP HANA bağlayıcı](data-factory-sap-hana-connector.md#copy-activity-properties) makalesi.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmet türü oluşturma **OnPremisesSqlServer** bir şirket içi SQL Server veritabanını data factory'ye bağlamak için. Aşağıdaki tabloda şirket içi SQL Server bağlantılı hizmete özgü JSON öğeleri açıklamasını sağlar.

Aşağıdaki tabloda, SQL Server bağlantılı hizmete özgü JSON öğeleri açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| connectionString |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak için gereken connectionString bilgilerini belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\kullanıcıadı**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |

Kimlik bilgilerini kullanarak şifreleyebilirsiniz **yeni AzureRmDataFactoryEncryptValue** cmdlet'i ve bunları aşağıdaki örnekte gösterildiği gibi bağlantı dizesini kullanın (**EncryptedCredential** özellik):  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Örnek: SQL kimlik doğrulaması kullanmak için JSON

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Örnek: JSON'ı Windows kimlik doğrulaması kullanma

Kullanıcı adı ve parolası belirtilmişse, ağ geçidi bunları şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabının kimliğine bürün kullanır. Aksi takdirde, ağ geçidi SQL Server Ağ Geçidi (kendi başlangıç hesabı) güvenlik bağlamı ile doğrudan bağlanır.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir SQL Server veri kümesini tanımlamak için **türü** için veri kümesinin **SqlServerTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm hizmeti bağlı SQL Server veritabanı örneğinde başvurduğu adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#dataset-properties) makalesi. 

### <a name="sql-source-in-copy-activity"></a>Kopyalama etkinliğinde SQL kaynağı
Bir SQL Server veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **SqlSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. Birden çok tablo girdi veri kümesi tarafından başvurulan veritabanından başvurabilir. Belirtilmezse, yürütülen SQL deyimi: MyTable arasından seçin. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. |Saklı yordam adı. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği veri almak için SQL Server veritabanı kaynağında bu sorguyu çalıştırır.

Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz yapısı bölümünde tanımlanan sütunları SQL Server veritabanına karşı çalıştırmak için seçme sorgusu oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

> [!NOTE]
> Kullandığınızda **sqlReaderStoredProcedureName**, yine de için bir değer belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik. Yine de bu tabloya karşı gerçekleştirilen başka doğrulama vardır.


#### <a name="example"></a>Örnek
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Bu örnekte, **sqlReaderQuery** SqlSource için belirtilir. Kopyalama etkinliği bu sorguyu veri almak için SQL Server veritabanı kaynağına karşı çalışır. Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa). SqlReaderQuery girdi veri kümesi tarafından başvurulan veritabanına birden çok tablolarına başvuruda bulunabilir. Yalnızca veri kümesi'nin tableName typeProperty ayarlamak tabloya sınırlı değildir.

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz yapısı bölümünde tanımlanan sütunları SQL Server veritabanına karşı çalıştırmak için seçme sorgusu oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

Daha fazla bilgi için bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#copy-activity-properties) makalesi. 

### <a name="sql-sink-in-copy-activity"></a>Kopyalama etkinliği SQL havuzunda
Bir SQL Server veritabanına veri kopyalamak istiyorsanız, ayarlayın **Havuz türü** kopyalama etkinliği **SqlSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği sorgusunu belirtin. Daha fazla bilgi için bkz: [Yinelenebilirlik](#repeatability-during-copy) bölümü. |Sorgu bildirimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilim verileri temizlemek için kullanılan otomatik dilim tanımlayıcı doldurmak için sütun adı belirtin. Daha fazla bilgi için bkz: [Yinelenebilirlik](#repeatability-during-copy) bölümü. |Binary(32) veri türüne sahip bir sütunun sütun adı. |Hayır |
| sqlWriterStoredProcedureName |Saklı yordam adı hedef tabloda bu upserts (güncelleştirmeler/ekler) verileri. |Saklı yordam adı. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak tablo türü adı belirtin. Kopyalama etkinliği taşınan veri geçici bir tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu ardından var olan verilerle kopyalanan verileri birleştirebilirsiniz. |Bir tablo türü adı. |Hayır |

#### <a name="example"></a>Örnek
Ardışık Düzen bu girdi ve çıktı veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **BlobSource** ve **havuz** türü ayarlanmış **SqlSink**.

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#copy-activity-properties) makalesi. 

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Bağlı hizmet
Bir Sybase tanımlamak için hizmeti bağlı, Ayarla **türü** bağlantılı hizmetinin **OnPremisesSybase**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Sybase sunucunun adıdır. |Evet |
| veritabanı |Sybase veritabanının adı. |Evet |
| Şema |Veritabanı şemasında adı. |Hayır |
| authenticationType |Sybase veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi Sybase veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Sybase bağlayıcı](data-factory-onprem-sybase-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Sybase veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Sybase veritabanı örneğinde bağlantılı hizmet başvurduğu tablonun adı. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [Sybase bağlayıcı](data-factory-onprem-sybase-connector.md#dataset-properties) makalesi. 

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Bir Sybase veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **tableName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [Sybase bağlayıcı](data-factory-onprem-sybase-connector.md#copy-activity-properties) makalesi.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Bağlı hizmet
Bir Teradata tanımlamak için hizmeti bağlı, Ayarla **türü** bağlantılı hizmetinin **OnPremisesTeradata**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Teradata sunucunun adıdır. |Evet |
| authenticationType |Teradata veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi Teradata veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Teradata bağlayıcı](data-factory-onprem-teradata-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Teradata Blob veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**. Şu anda Teradata veri kümesi için desteklenen tür özellik yok. 

#### <a name="example"></a>Örnek
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [Teradata bağlayıcı](data-factory-onprem-teradata-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Bir Teradata veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

Daha fazla bilgi için bkz: [Teradata bağlayıcı](data-factory-onprem-teradata-connector.md#copy-activity-properties) makalesi.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Bağlı hizmet
Bir bağlı Cassandra hizmet tanımlamak için **türü** bağlantılı hizmetinin **OnPremisesCassandra**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| konak |Bir veya daha fazla IP adresleri veya ana bilgisayar adlarını Cassandra sunucuları.<br/><br/>IP adreslerini veya aynı anda tüm sunucularına bağlanmak için ana bilgisayar adlarını virgülle ayrılmış listesini belirtin. |Evet |
| port |İstemci bağlantılarını dinlemek için Cassandra sunucusunun kullandığı TCP bağlantı noktası. |Hayır, varsayılan değer: 9042 |
| authenticationType |Basic veya Anonymous |Evet |
| kullanıcı adı |Kullanıcı hesabının kullanıcı adını belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| gatewayName |Şirket içi Cassandra veritabanına bağlanmak için kullanılan ağ geçidi adı. |Evet |
| encryptedCredential |Ağ Geçidi tarafından şifrelenmiş kimlik bilgileri. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Cassandra veri kümesini tanımlamak için **türü** için veri kümesinin **CassandraTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| keyspace |Keyspace veya Cassandra veritabanındaki şema adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |
| tableName |Cassandra veritabanı tablosunun adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |

#### <a name="example"></a>Örnek

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#dataset-properties) makalesi. 

### <a name="cassandra-source-in-copy-activity"></a>Kopyalama etkinliği Cassandra kaynağında
Cassandra veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **CassandraSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL-92 sorgusu veya CQL sorgusu. Bkz: [CQL başvuru](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgusu kullanırken belirtin **keyspace name.table adı** sorgulamak istediğiniz tabloyu temsil etmek için. |Hayır (tableName ve veri kümesi üzerinde keyspace tanımlanmışsa). |
| consistencyLevel |Tutarlılık düzeyi kaç çoğaltmaları Okuma isteği için veri istemci uygulamasına geri dönmeden önce yanıt vermesi gereken belirtir. Cassandra Okuma isteği karşılamak veriler için çoğaltmaları belirtilen sayısını denetler. |ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Bkz: [veri tutarlılığını yapılandırma](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) Ayrıntılar için. |Hayır. Varsayılan değer biridir. |

#### <a name="example"></a>Örnek
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#copy-activity-properties) makalesi.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir MongoDB tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OnPremisesMongoDB**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |IP adresi veya ana bilgisayar MongoDB sunucunun adıdır. |Evet |
| port |İstemci bağlantılarını dinlemek için MongoDB sunucusunun kullandığı TCP bağlantı noktası. |İsteğe bağlı, varsayılan değer: 27017 |
| authenticationType |Basic veya Anonymous. |Evet |
| kullanıcı adı |MongoDB erişmek için kullanıcı hesabı. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| password |Kullanıcının parolası. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| authSource |Kimlik doğrulaması için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanı adı. |(Temel kimlik doğrulaması kullanılıyorsa) isteğe bağlı. Varsayılan: yönetici hesabı ve databaseName özelliği kullanılarak belirtilen veritabanı kullanır. |
| databaseName |Erişmek istediğiniz MongoDB veritabanı adı. |Evet |
| gatewayName |Veri deposu erişen ağ geçidi adı. |Evet |
| encryptedCredential |Ağ Geçidi tarafından şifrelenmiş kimlik bilgileri. |İsteğe bağlı |

#### <a name="example"></a>Örnek

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için bkz: [MongoDB bağlayıcı makale](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Veri kümesi
MongoDB veri kümesini tanımlamak için **türü** için veri kümesinin **MongoDbCollection**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| collectionName |MongoDB veritabanı koleksiyonunda adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

Daha fazla bilgi için bkz: [MongoDB bağlayıcı makale](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>Kopyalama etkinliği MongoDB kaynağında
Veri adresinden kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **MongoDbSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL-92 sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **collectionName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [MongoDB bağlayıcı makale](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir Amazon S3 tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AwsAccessKey**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| accessKeyID |Gizli erişim anahtarı kimliği. |string |Evet |
| secretAccessKey |Gizli erişim anahtar kendisi. |Şifrelenmiş gizli dize |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Amazon S3 bağlayıcı makale](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Veri kümesi
Bir Amazon S3 dataset tanımlamak için **türü** için veri kümesinin **AmazonS3**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 demetini adı. |Dize |Evet |
| anahtar |S3 nesne anahtarı. |Dize |Hayır |
| önek |S3 nesne anahtarı için önek. Seçilen nesneler, anahtarları Bu önek ile başlatın. Yalnızca anahtar boş olduğunda geçerlidir. |Dize |Hayır |
| sürüm |S3 sürüm etkinleştirilirse S3 nesne sürümü. |Dize |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır | |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır | |


> [!NOTE]
> bucketName + anahtarı burada demet S3 nesneleri için kök kapsayıcı ve anahtar S3 nesnenin tam yolunun S3 nesnenin konumunu belirtir.

#### <a name="example-sample-dataset-with-prefix"></a>Örnek: Örnek veri kümesi önekiyle

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a>Örnek: Örnek veri kümesiyle (sürüm)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a>Örnek: S3 dinamik yollar
Örnekte, Amazon S3 dataset içindeki anahtar ve bucketName özellikler için sabit değerleri kullanın.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

Veri Fabrikası sistem değişkenleri SliceStart gibi kullanarak anahtar ve çalışma zamanında dinamik olarak bucketName hesaplama olabilir.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Bir Amazon S3 dataset önek özelliği için aynı yapabilirsiniz. Bkz: [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md) desteklenen işlevleri ve değişkenler listesi.

Daha fazla bilgi için bkz: [Amazon S3 bağlayıcı makale](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Dosya sistem kaynağını kopyalama etkinliği
Verileri Amazon S3'ten kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **FileSystemSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Özyinelemeli S3 listesinde olup olmadığını belirtir dizini altındaki nesneleri. |true/false |Hayır |


#### <a name="example"></a>Örnek


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [Amazon S3 bağlayıcı makale](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Dosya Sistemi


### <a name="linked-service"></a>Bağlı hizmet
Bir Azure data factory ile bir şirket içi dosya sistemi bağlayabilirsiniz **şirket içi dosya sunucusu** bağlı hizmeti. Aşağıdaki tabloda şirket içi dosya sunucusu bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlandığından emin olun **OnPremisesFileServer**. |Evet |
| konak |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için. |Evet |
| Kullanıcı Kimliği |Sunucusuna erişimi olan kullanıcı Kimliğini belirtin. |Hayır (encryptedCredential seçerseniz) |
| password |(UserID) kullanıcının parolasını belirtin. |Hayır (encryptedCredential seçerseniz |
| encryptedCredential |Yeni AzureRmDataFactoryEncryptValue cmdlet'ini çalıştırarak alabilirsiniz şifreli kimlik bilgilerini belirtin. |Hayır (kullanıcı kimliği ve parola düz metin olarak belirtmek isterseniz) |
| gatewayName |Veri Fabrikası şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidi adını belirtir. |Evet |

#### <a name="sample-folder-path-definitions"></a>Örnek klasör yolu tanımları 
| Senaryo | Bağlantılı hizmet tanımında ana bilgisayar | veri kümesi tanımında folderPath |
| --- | --- | --- |
| Veri Yönetimi ağ geçidi makine üzerinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |D:\\ \\ (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/> localhost (daha önceki sürümler için veri yönetimi ağ geçidi 2. 0) |. \\ \\ veya klasör\\\\alt klasör (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/>D:\\ \\ veya D:\\\\klasörü\\\\alt klasör (için ağ geçidi sürüm 2.0 altında) |
| Uzak paylaşılan klasör: <br/><br/>Örnekler: \\ \\myserver\\paylaşmak\\ \* veya \\ \\myserver\\paylaşmak\\klasörü\\alt klasör\\* |\\\\\\\\myserver\\\\paylaşma |. \\ \\ veya klasör\\\\alt klasör |


#### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: kullanıcı adı ve parola düz metin olarak kullanma

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a>Örnek: encryptedcredential kullanma

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için bkz: [dosya sistemi bağlayıcı makale](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Veri kümesi
Bir dosya sistemi veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasöre belirtir. Kaçış karakteri kullanmak ' \' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı şu biçimdedir: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: "fileFilter": "* .log"<br/>Örnek 2: "fileFilter": 2016 - 1-? txt"<br/><br/>Bu fileFilter bir giriş FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| partitionedBy |PartitionedBy dinamik folderPath/için bir dosya adı time series verilerini belirtmek için kullanabilirsiniz. İçin verileri saatte parametreli folderPath örneğidir. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**; ve desteklenen düzeyler: **Optimal** ve **en hızlı**. bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanamazsınız.

#### <a name="example"></a>Örnek

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [dosya sistemi bağlayıcı makale](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Dosya sistem kaynağını kopyalama etkinliği
Dosya sisteminden veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **FileSystemSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Daha fazla bilgi için bkz: [dosya sistemi bağlayıcı makale](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>Dosya sistemi havuzu kopyalama etkinliği
Dosya sistemi veri kopyalıyorsanız ayarlamak **Havuz türü** kopyalama etkinliği **FileSystemSink**ve aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |**PreserveHierarchy:** dosya hiyerarşisi hedef klasördeki korur. Diğer bir deyişle, kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><br/>**FlattenHierarchy:** tüm dosyaları kaynak klasörden hedef klasöre ilk düzeyi oluşturulur. Hedef dosyalar otomatik olarak oluşturulur adıyla oluşturulur.<br/><br/>**MergeFiles:** bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Dosya adı/blob adı belirtilirse, birleştirilmiş dosya adı belirtilen addır. Aksi halde, bir otomatik olarak oluşturulan dosya adı değil. |Hayır |
otomatik-

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [dosya sistemi bağlayıcı makale](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin FTP tanımlamak için Ayarla **türü** bağlantılı hizmetinin **Ftp_sunucusu**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| konak |FTP sunucusunun adı veya IP adresi |Evet |&nbsp; |
| authenticationType |Kimlik doğrulaması türünü belirtme |Evet |Temel, anonim |
| kullanıcı adı |FTP sunucusuna erişimi olan kullanıcı |Hayır |&nbsp; |
| password |(Kullanıcı adı) kullanıcı parolası |Hayır |&nbsp; |
| encryptedCredential |FTP sunucusuna erişmek için şifrelenmiş kimlik bilgileri |Hayır |&nbsp; |
| gatewayName |Şirket içi FTP sunucusuna bağlanmak için veri yönetimi ağ geçidi ağ geçidinin adı |Hayır |&nbsp; |
| port |FTP sunucusunun dinlediği bağlantı noktası |Hayır |21 |
| enableSsl |FTP SSL/TLS kanalı üzerinden kullanıp kullanmayacağınızı belirtin |Hayır |true |
| enableServerCertificateValidation |Sunucu SSL sertifika doğrulamasını FTP SSL/TLS kanalı üzerinden kullanırken etkinleştirilip etkinleştirilmeyeceğini belirtin |Hayır |true |

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulamasını kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Örnek: kullanıcı adı ve parola düz metin olarak temel kimlik doğrulaması kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Örnek: Bağlantı noktası, enableSsl, enableServerCertificateValidation kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Örnek: kimlik doğrulama ve ağ geçidi encryptedCredential kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

Daha fazla bilgi için bkz: [FTP Bağlayıcısı](data-factory-ftp-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir FTP veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasörün alt yolu. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet 
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır bu biçimi: <br/><br/>Veriler. <Guid>.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik ile HDFS desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy filename zaman serisi veri için dinamik bir folderPath belirtmek için kullanılabilir. Örneğin, her veri saat için parametreli folderPath. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**; ve desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: True. İlişkili bağlantılı hizmet türü türü olduğunda bu özellik yalnızca kullanılabilir: Ftp_sunucusu. |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.

#### <a name="example"></a>Örnek

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için bkz: [FTP Bağlayıcısı](data-factory-ftp-connector.md#dataset-properties) makalesi.

### <a name="file-system-source-in-copy-activity"></a>Dosya sistem kaynağını kopyalama etkinliği
Bir FTP sunucusundan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **FileSystemSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [FTP Bağlayıcısı](data-factory-ftp-connector.md#copy-activity-properties) makalesi.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir HDFS tanımlamak için Ayarla **türü** bağlantılı hizmetinin **Hdfs**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Hdfs** |Evet |
| Url |HDFS URL'si |Evet |
| authenticationType |Anonim veya Windows. <br><br> Kullanılacak **Kerberos kimlik doğrulaması** HDFS bağlayıcı için başvurmak [Bu bölümde](#use-kerberos-authentication-for-hdfs-connector) şirket içi ortamınıza uygun şekilde ayarlamak için. |Evet |
| Kullanıcı adı |Kullanıcı adı için Windows kimlik doğrulaması. |Evet (Windows kimlik doğrulaması için) |
| password |Windows kimlik doğrulaması için parola. |Evet (Windows kimlik doğrulaması için) |
| gatewayName |Data Factory hizmetinin HDFS bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| encryptedCredential |[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential. |Hayır |

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulamasını kullanma

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a>Örnek: Windows kimlik doğrulaması kullanma

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için bkz: [HDFS bağlayıcı](#data-factory-hdfs-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
HDFS veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasör yolu. Örnek: `myfolder`<br/><br/>Kaçış karakteri kullanmak ' \ ' dize özel karakter. Örneğin: folder\subfolder için klasör belirtin\\\\alt ve d:\samplefolder için d: belirtin\\\\ÖrnekKlasör.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır bu biçimi: <br/><br/>Veriler. <Guid>.txt (örnek:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy filename zaman serisi veri için dinamik bir folderPath belirtmek için kullanılabilir. Örnek: veri her saat için parametreli folderPath. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.

#### <a name="example"></a>Örnek

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için bkz: [HDFS bağlayıcı](#data-factory-hdfs-connector.md#dataset-properties) makalesi. 

### <a name="file-system-source-in-copy-activity"></a>Dosya sistem kaynağını kopyalama etkinliği
HDFS veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **FileSystemSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [HDFS bağlayıcı](#data-factory-hdfs-connector.md#copy-activity-properties) makalesi.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir SFTP tanımlamak için Ayarla **türü** bağlantılı hizmetinin **Sftp**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- | --- |
| konak | SFTP sunucunun adı veya IP adresi. |Evet |
| port |SFTP sunucunun dinlediği bağlantı noktası. Varsayılan değer: 21 |Hayır |
| authenticationType |Kimlik doğrulama türü belirtin. İzin verilen değerler: **temel**, **SshPublicKey**. <br><br> Başvurmak [kullanarak temel kimlik doğrulaması](#using-basic-authentication) ve [kullanarak SSH ortak anahtar kimlik doğrulaması](#using-ssh-public-key-authentication) daha fazla özellikleri ve JSON örnekleri sırasıyla bölümler. |Evet |
| skipHostKeyValidation | Ana anahtar doğrulama atlamak bu seçeneği belirtin. | Hayır. Varsayılan değeri: false |
| hostKeyFingerprint | Ana makine anahtarı, parmak izi belirtin. | Yanıt Evet ise `skipHostKeyValidation` false olarak ayarlayın.  |
| gatewayName |Bir şirket içi SFTP sunucusuna bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi SFTP sunucusundan veri kopyalama, Evet. |
| encryptedCredential | SFTP sunucuya erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim temel kimlik doğrulaması (kullanıcı adı + parola) veya SshPublicKey kimlik (kullanıcı adı + özel anahtar yolu veya içerik) belirttiğinizde. | Hayır. Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

#### <a name="example-using-basic-authentication"></a>Örnek: temel kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak üzere ayarlanmış `authenticationType` olarak `Basic`ve SFTP bağlayıcı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- | --- |
| kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | (Kullanıcı adı) kullanıcının parolası. | Evet |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Örnek: **şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a>**SSH ortak anahtar kimlik doğrulaması kullanma:**

Temel kimlik doğrulaması kullanmak üzere ayarlanmış `authenticationType` olarak `SshPublicKey`ve SFTP bağlayıcı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- | --- |
| kullanıcı adı |SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Belirtin özel anahtar dosyasının mutlak yolu, ağ geçidi erişebilir. | Belirtin `privateKeyPath` veya `privateKeyContent`. <br><br> Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |
| privateKeyContent | Özel anahtar içeriğinin seri hale getirilmiş bir dize. Kopyalama Sihirbazı'nı, özel anahtar dosyası okumak ve özel anahtar içeriği otomatik olarak ayıklayın. Tüm diğer aracı/SDK kullanıyorsanız, bunun yerine privateKeyPath özelliğini kullanın. | Belirtin `privateKeyPath` veya `privateKeyContent`. |
| Parola | Geçişi tümcecik/anahtar dosyası bir parola deyimi tarafından korunuyorsa, özel anahtarın şifresini çözmek için parola belirtin. | Evet, özel anahtar dosyası bir parola deyimi tarafından korunuyorsa. |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Örnek: **özel anahtar içeriği kullanarak SshPublicKey kimlik doğrulaması**

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

Daha fazla bilgi için bkz: [SFTP bağlayıcı](data-factory-sftp-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
SFTP veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasörün alt yolu. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır bu biçimi: <br/><br/>Veriler. <Guid>.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik ile HDFS desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy filename zaman serisi veri için dinamik bir folderPath belirtmek için kullanılabilir. Örneğin, her veri saat için parametreli folderPath. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: True. İlişkili bağlantılı hizmet türü türü olduğunda bu özellik yalnızca kullanılabilir: Ftp_sunucusu. |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.

#### <a name="example"></a>Örnek

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için bkz: [SFTP bağlayıcı](data-factory-sftp-connector.md#dataset-properties) makalesi. 

### <a name="file-system-source-in-copy-activity"></a>Dosya sistem kaynağını kopyalama etkinliği
SFTP kaynaktan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **FileSystemSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |



#### <a name="example"></a>Örnek

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [SFTP bağlayıcı](data-factory-sftp-connector.md#copy-activity-properties) makalesi.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmeti bir HTTP tanımlama, Ayarla **türü** bağlantılı hizmetinin **Http**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| url | Web sunucusu için temel URL | Evet |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler: **anonim**, **temel**, **Özet**, **Windows**, **ClientCertificate**. <br><br> Daha fazla özellikleri ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlerde sırasıyla bakın. | Evet |
| enableServerCertificateValidation | Sunucu SSL sertifika doğrulamasını kaynak HTTPS Web sunucusu ise etkinleştirilip etkinleştirilmeyeceğini belirtin | Hayır, varsayılan değer true şeklindedir |
| gatewayName | Bir şirket içi HTTP kaynağına bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi HTTP kaynaktan veri kopyalama, Evet. |
| encryptedCredential | HTTP uç noktasına erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim kimlik doğrulama bilgilerini yapılandırın. | Hayır. Yalnızca bir şirket içi HTTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Örnek: Temel, Özet veya Windows kimlik doğrulaması kullanma
Ayarlama `authenticationType` olarak `Basic`, `Digest`, veya `Windows`ve HTTP Bağlayıcısı yukarıda sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | HTTP uç noktasına erişmek için kullanıcı adı. | Evet |
| password | (Kullanıcı adı) kullanıcının parolası. | Evet |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a>Örnek: ClientCertificate kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak üzere ayarlanmış `authenticationType` olarak `ClientCertificate`ve HTTP Bağlayıcısı yukarıda sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| embeddedCertData | İkili veriler kişisel bilgi değişimi (PFX) dosyası Base64 ile kodlanmış içeriği. | Belirtin `embeddedCertData` veya `certThumbprint`. |
| certThumbprint | Ağ geçidi makinenizin sertifika deposunda yüklü sertifika parmak izi. Yalnızca bir şirket içi HTTP kaynaktan veri kopyalama işlemi sırasında uygulanır. | Belirtin `embeddedCertData` veya `certThumbprint`. |
| password | Sertifikayla ilişkili parola. | Hayır |

Kullanırsanız `certThumbprint` kimlik doğrulaması ve sertifika yüklü için yerel bilgisayarın kişisel deposunda, ağ geçidi hizmeti okuma izni vermesi gerekir:

1. Microsoft Yönetim Konsolu (MMC) başlatın. Ekleme **sertifikaları** hedefleyen eklentisi **yerel bilgisayar**.
2. Genişletme **sertifikaları**, **kişisel**, tıklatıp **Sertifikalar**.
3. Kişisel deposundaki sertifikayı sağ tıklatın ve seçin **tüm görevler**->**özel anahtarları Yönet...**
3. Üzerinde **güvenlik** sekmesinde, altında veri yönetimi ağ geçidi ana bilgisayar hizmeti çalıştığı okuma erişimi sertifikayı kullanıcı hesabını ekleyin.  

**Örnek: istemci sertifikası kullanarak:** bu hizmeti, veri fabrikası bir şirket içi HTTP web sunucusuna bağlı. Veri Yönetimi ağ ile geçidi yüklü olduğu makinede yüklü bir istemci sertifikası kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Örnek: istemci sertifikası bir dosyada kullanma
Bu hizmet bağlantılar, veri fabrikası bir şirket içi HTTP web sunucusuna bağlı. Veri Yönetimi ağ ile geçidi yüklü olduğu makinedeki bir istemci sertifikası dosyası kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

Daha fazla bilgi için bkz: [HTTP Bağlayıcısı](data-factory-http-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir HTTP veri kümesini tanımlamak için **türü** için veri kümesinin **Http**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bağlantılı hizmet tanımında belirtilen URL yolu belirtilmediğinde kullanılır. <br><br> Dinamik URL oluşturmak için kullanabileceğiniz [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md), örneğin: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **almak** veya **POST**. | Hayır. `GET` varsayılan değerdir. |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| requestBody | HTTP istek gövdesi. | Hayır |
| Biçimi | Yalnızca istiyorsanız, **HTTP uç noktası olarak veri almak-olduğu** ayrıştırma olmadan, bu biçim ayarlarını atla. <br><br> HTTP yanıt içeriği kopyalama sırasında ayrıştırma istiyorsanız, aşağıdaki biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

#### <a name="example-using-the-get-default-method"></a>Örnek: (varsayılan) GET yöntemini kullanma

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-the-post-method"></a>Örnek: POST yöntemini kullanma

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
Daha fazla bilgi için bkz: [HTTP Bağlayıcısı](data-factory-http-connector.md#dataset-properties) makalesi.

### <a name="http-source-in-copy-activity"></a>Kopyalama etkinliğinde HTTP kaynağı
Bir HTTP kaynaktan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **HttpSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| httpRequestTimeout | Zaman aşımı (TimeSpan) için bir yanıt almak HTTP isteği. Bu zaman aşımı yanıt verileri okumak için zaman aşımına bir yanıt elde etmektir. | Hayır. Varsayılan değer: 00:01:40 |


#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [HTTP Bağlayıcısı](data-factory-http-connector.md#copy-activity-properties) makalesi.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir OData tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OData**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| url |OData hizmeti URL'si. |Evet |
| authenticationType |OData kaynağına bağlanmak için kullanılan kimlik doğrulama türü. <br/><br/> OData bulut için olası değerler şunlardır anonim, temel ve OAuth (Not OAuth Azure Active Directory tabanlı şu anda yalnızca Azure Data Factory destek). <br/><br/> Anonim, temel ve Windows, şirket içi OData için olası değerler şunlardır. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Evet (yalnızca temel kimlik doğrulaması kullanıyorsanız) |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Evet (yalnızca temel kimlik doğrulaması kullanıyorsanız) |
| authorizedCredential |OAuth kullanıyorsanız **Authorize** veri fabrikası Kopyalama Sihirbazı'nı veya düzenleyicide düğmesine tıklayın ve sonra da bu özelliğin değeri otomatik olarak oluşturulan kimlik bilgilerinizi girin. |Evet (yalnızca OAuth kimlik doğrulaması kullanıyorsanız) |
| gatewayName |Data Factory hizmetinin şirket içi OData hizmetine bağlanmak için kullanması gereken ağ geçidinin adı. Yalnızca şirket içi OData kaynak sunucudan kopyaladığınız verileri belirtin. |Hayır |

#### <a name="example---using-basic-authentication"></a>Temel kimlik doğrulaması kullanan örnek-
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Anonim kimlik doğrulaması kullanan örnek-

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Örnek - kullanan Windows kimlik doğrulaması erişme OData kaynağı şirket içi

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Örnek - bulut OData kaynağı erişme OAuth kimlik doğrulaması kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

Daha fazla bilgi için bkz: [OData bağlayıcı](data-factory-odata-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir OData veri kümesini tanımlamak için **türü** için veri kümesinin **ODataResource**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| yol |OData kaynak yolu |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Daha fazla bilgi için bkz: [OData bağlayıcı](data-factory-odata-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Bir OData kaynaktan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | Örnek | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |"? $select adı, açıklama ve $top = 5 =" |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

Daha fazla bilgi için bkz: [OData bağlayıcı](data-factory-odata-connector.md#copy-activity-properties) makalesi.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir ODBC tanımlamak için Ayarla **türü** bağlantılı hizmetinin **OnPremisesOdbc**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| connectionString |Bağlantı dizesi ve isteğe bağlı şifrelenmiş kimlik bilgileri olmayan erişim kimlik bilgileri bölümü. Aşağıdaki bölümlerde örneklere bakın. |Evet |
| kimlik bilgisi |Sürücü özgü özellik değer biçiminde belirtilen bağlantı dizesi erişim kimlik bilgisi bölümü. Örnek: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ". |Hayır |
| authenticationType |ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: anonim ve temel. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin ODBC veri deposuna bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example---using-basic-authentication"></a>Temel kimlik doğrulaması kullanan örnek-

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Temel kimlik doğrulaması ile şifrelenmiş kimlik bilgileri kullanılarak örnek-
Kullanarak kimlik bilgilerini şifrelemek [yeni AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (Azure PowerShell 1.0 sürümü) cmdlet'ini veya [yeni AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (Azure PowerShell 0,9 veya önceki sürüm).  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulamasını kullanma

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için bkz: [ODBC bağlayıcı](data-factory-odbc-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir ODBC veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |ODBC veri deposundaki tablonun adı. |Evet |


#### <a name="example"></a>Örnek

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [ODBC bağlayıcı](data-factory-odbc-connector.md#dataset-properties) makalesi. 

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Bir ODBC veri deposundan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

Daha fazla bilgi için bkz: [ODBC bağlayıcı](data-factory-odbc-connector.md#copy-activity-properties) makalesi.

## <a name="salesforce"></a>Salesforce


### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin bir Salesforce tanımlamak için Ayarla **türü** bağlantılı hizmetinin **Salesforce**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| environmentUrl | URL, Salesforce örneği belirtin. <br><br> -Varsayılan değer "https://login.salesforce.com". <br> -Korumalı alan veri kopyalamak için belirtin "https://test.salesforce.com". <br> Özel etki alanından veri kopyalamak için örneğin, "https://[domain].my.salesforce.com" belirtin. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| securityToken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bkz: [güvenlik belirteci alma getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) bir güvenlik belirteci sıfırlama/get ilgili yönergeler için. Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

Daha fazla bilgi için bkz: [Salesforce bağlayıcı](data-factory-salesforce-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Salesforce veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Salesforce tablo adı. |Hayır (varsa bir **sorgu** , **RelationalSource** belirtilir) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için bkz: [Salesforce bağlayıcı](data-factory-salesforce-connector.md#dataset-properties) makalesi. 

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliği ilişkisel kaynağında
Salesforce veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **RelationalSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |Bir SQL-92 sorgu veya [Salesforce nesnesi sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgu. Örneğin:  `select * from MyTable__c`. |Hayır (varsa **tableName** , **dataset** belirtilir) |

#### <a name="example"></a>Örnek  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

Daha fazla bilgi için bkz: [Salesforce bağlayıcı](data-factory-salesforce-connector.md#copy-activity-properties) makalesi. 

## <a name="web-data"></a>Web verileri 

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin Web tanımlamak için Ayarla **türü** bağlantılı hizmetinin **Web**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Url |Web kaynağı URL'si |Evet |
| authenticationType |Anonim. |Evet |
 

#### <a name="example"></a>Örnek


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

Daha fazla bilgi için bkz: [Web tablo bağlayıcı](data-factory-web-table-connector.md#linked-service-properties) makalesi. 

### <a name="dataset"></a>Veri kümesi
Bir Web veri kümesini tanımlamak için **türü** için veri kümesinin **WebTable**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü: 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Veri kümesi türü. ayarlanmalıdır **WebTable** |Evet |
| yol |Tabloyu içeren kaynak için göreli bir URL. |Hayır. Bağlantılı hizmet tanımında belirtilen URL yolu belirtilmediğinde kullanılır. |
| dizin |Tablo kaynak dizini. Bkz: [bir HTML sayfasında tablosunun Get dizini](#get-index-of-a-table-in-an-html-page) bir HTML sayfasında bir tablo dizininin alma adımları için bölüm. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için bkz: [Web tablo bağlayıcı](data-factory-web-table-connector.md#dataset-properties) makalesi. 

### <a name="web-source-in-copy-activity"></a>Kopyalama etkinliğinde Web kaynağı
Bir web tablodan veri kopyalıyorsanız ayarlamak **kaynak türünü** kopyalama etkinliği **WebSource**. Şu anda kopyalama etkinliği kaynağında olduğunda türü **WebSource**, hiçbir ek özellikler desteklenir.

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için bkz: [Web tablo bağlayıcı](data-factory-web-table-connector.md#copy-activity-properties) makalesi. 

## <a name="compute-environments"></a>ORTAMLAR İŞLEM
Aşağıdaki tablo, veri fabrikası ve bunlar üzerinde çalışan dönüştürme etkinlikleri tarafından desteklenen bilgi işlem ortamları listeler. Bir data factory'ye bağlamak bağlı hizmet JSON şemaları görmek ilgilendiğiniz işlem bağlantısına tıklayın. 

| İşlem ortamı | Etkinlikler |
| --- | --- |
| [İsteğe bağlı Hdınsight kümesi](#on-demand-azure-hdinsight-cluster) veya [kendi Hdınsight kümenizi](#existing-azure-hdinsight-cluster) |[.NET özel etkinlik](#net-custom-activity), [Hive etkinliğini](#hdinsight-hive-activity), [Pig etkinlik] (#hdinsight-pig-etkinliği [MapReduce etkinliği](#hdinsight-mapreduce-activity), [Hadoop etkinlik akış](#hdinsight-streaming-activityd), [ Spark etkinliği](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[.NET özel etkinliği](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Toplu iş yürütme etkinliği makine](#machine-learning-batch-execution-activity), [kaynak güncelleştirme etkinliği makine](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Azure SQL veritabanı](#azure-sql-database-1), [Azure SQL veri ambarı](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1) |[Saklı Yordam](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>İsteğe bağlı Azure Hdınsight kümesi
Azure Data Factory hizmetinin otomatik olarak bir Windows/Linux tabanlı isteğe bağlı Hdınsight kümesi verileri işlemek için oluşturabilirsiniz. Küme kümeyle ilişkili depolama hesabı (JSON özelliğinde linkedServiceName) ile aynı bölgede oluşturulur. Bu bağlı hizmete aşağıdaki dönüştürme etkinlikleri çalıştırabilirsiniz: [.NET özel etkinlik](#net-custom-activity), [Hive etkinliğini](#hdinsight-hive-activity), [Pig etkinlik] (#hdinsight-pig-etkinliği [MapReduce etkinliği](#hdinsight-mapreduce-activity), [Hadoop etkinlik akış](#hdinsight-streaming-activityd), [Spark etkinlik](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Bağlı hizmet 
Aşağıdaki tabloda bir isteğe bağlı Hdınsight bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalı **HDInsightOnDemand**. |Evet |
| clusterSize |Kümedeki çalışan/veri düğüm sayısı. Hdınsight kümesi için bu özelliği belirtmeniz çalışan düğüm sayısı ile birlikte 2 baş düğümler ile oluşturulur. Düğüm boyutu 4 çalışan düğümlü bir küme 24 çekirdek alır, böylece 4 çekirdeğe sahip Standard_D3 olduğundan (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*4 = 8 çekirdek baş düğümler için). Bkz: [Hdınsight oluşturma Linux tabanlı Hadoop kümeleri](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) Standard_D3 katmanı hakkında ayrıntılı bilgi için. |Evet |
| timetolive |İsteğe bağlı Hdınsight kümesi için izin verilen boşta kalma süresi. Ne kadar süreyle isteğe bağlı Hdınsight kümesi kümedeki diğer etkin iş yok varsa bir etkinlik tamamlandıktan sonra canlı kalır belirtir.<br/><br/>Örneğin, bir etkinlik Çalıştır 6 dakika sürer ve timetolive 5 dakika olarak ayarlanmıştır, küme 6 etkinlik işleme dakika çalıştırdıktan sonra 5 dakika boyunca etkin kalır. Başka bir etkinlik 6 dakika penceresiyle yürütülürse, aynı küme tarafından işlenir.<br/><br/>İsteğe bağlı Hdınsight kümesi oluşturma bir pahalı işlemi (işlem zaman alabilir), bunu kullanımı isteğe bağlı Hdınsight kümesi yeniden kullanarak bir veri fabrikası performansını artırmak için bu ayarı olarak gerekli olur.<br/><br/>Timetolive değeri 0 olarak ayarlarsanız, etkinliği çalıştırmak hemen işlenen küme silinir. Diğer taraftan, yüksek bir değer ayarlarsanız, kümenin yüksek maliyetlerini gereksiz yere kaynaklanan boşta kalır. Bu nedenle, gereksinimlerinize göre uygun değere ayarlamak önemlidir.<br/><br/>Timetolive özellik değerini uygun şekilde ayarlanmışsa, birden çok ardışık düzen isteğe bağlı Hdınsight kümesi aynı kopyasını paylaştığında |Evet |
| sürüm |Hdınsight küme sürümü. Ayrıntılar için bkz [desteklenen Azure Data Factory'de Hdınsight sürümleri](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Hayır |
| linkedServiceName |Depolamak ve veri işleme için isteğe bağlı küme tarafından kullanılacak azure depolama bağlı hizmeti. <p>Şu anda bir Azure Data Lake Store depolama alanı olarak kullanan bir isteğe bağlı Hdınsight kümesi oluşturulamıyor. Bir Azure Data Lake Store'da işleme Hdınsight sonuç verileri depolamak istiyorsanız, Azure Blob depolama alanından Azure Data Lake Store'a veri kopyalamak için kopyalama etkinliği kullanın.</p>  | Evet |
| additionalLinkedServiceNames |Data Factory hizmetinin bunları sizin adınıza kaydedebilirsiniz böylece Hdınsight için ek depolama hesapları hizmeti bağlı belirtir. |Hayır |
| osType |İşletim sistemi türü. İzin verilen değerler: (varsayılan) Windows ve Linux |Hayır |
| hcatalogLinkedServiceName |Azure SQL adını HCatalog veritabanına işaret hizmeti bağlı. İsteğe bağlı Hdınsight kümesi meta depo Azure SQL veritabanı kullanılarak oluşturulur. |Hayır |

### <a name="json-example"></a>JSON örneği
Aşağıdaki JSON Linux tabanlı isteğe bağlı Hdınsight bağlı hizmeti tanımlar. Data Factory hizmetinin otomatik olarak oluşturur bir **Linux tabanlı** veri dilimi işlerken Hdınsight kümesi. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

Daha fazla bilgi için bkz: [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md) makalesi. 

## <a name="existing-azure-hdinsight-cluster"></a>Var olan Azure Hdınsight kümesi
Kendi Hdınsight kümenizi Data Factory ile kaydetmek için bir Azure Hdınsight bağlı hizmeti oluşturabilirsiniz. Bu bağlı hizmete aşağıdaki veri dönüştürme etkinlikleri çalıştırabilirsiniz: [.NET özel etkinlik](#net-custom-activity), [Hive etkinliğini](#hdinsight-hive-activity), [Pig etkinlik] (#hdinsight-pig-etkinliği [MapReduce etkinliği](#hdinsight-mapreduce-activity), [Hadoop etkinlik akış](#hdinsight-streaming-activityd), [Spark etkinlik](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda Azure Hdınsight bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalı **Hdınsight**. |Evet |
| clusterUri |Hdınsight küme URI'si. |Evet |
| kullanıcı adı |Var olan bir Hdınsight kümesine bağlanmak için kullanılacak kullanıcı adını belirtin. |Evet |
| password |Kullanıcı hesabı için parola belirtin. |Evet |
| linkedServiceName | Hdınsight küme tarafından kullanılan Azure blob depolama başvurduğu Azure Storage bağlı hizmetin adı. <p>Şu anda bu özellik için bir Azure Data Lake Store bağlı belirtemezsiniz. Hdınsight kümesi için Data Lake Store erişimi varsa Azure Data Lake Store'da verileri Hive/Pig komut dosyalarından erişebilir. </p>  |Evet |

Hdınsight kümeleri desteklenen sürümleri için bkz: [desteklenen Hdınsight sürümleri](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). 

#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a>Azure Batch
Batch havuzundaki sanal makineler (VM'ler) kaydetmek için bir Azure Batch bağlantılı hizmeti bir data factory ile oluşturabilirsiniz. .NET özel etkinlikler Azure Batch ya da Azure Hdınsight kullanarak çalıştırabilirsiniz. Çalıştırabilirsiniz bir [.NET özel etkinlik](#net-custom-activity) bu hizmeti bağlı. 

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda bir Azure Batch bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalı **AzureBatch**. |Evet |
| accountName |Azure toplu işlem hesabının adı. |Evet |
| accessKey |Azure Batch hesabı için erişim anahtarı. |Evet |
| poolName |Sanal makinelerin havuzunun adı. |Evet |
| linkedServiceName |Azure depolama adı hizmeti bağlı Azure Batch hizmeti ile ilişkili bağlı. Bu bağlı hizmetin etkinlik ve Etkinlik yürütme günlüklerini depolamak çalıştırmak için gerekli hazırlama dosyaları için kullanılır. |Evet |


#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a>Azure Machine Learning
Bir Machine Learning toplu Puanlama uç noktası data factory ile kaydetmek için bir Azure Machine Learning bağlantılı hizmeti oluşturun. Bu bilgisayarda çalıştırabilir iki veri dönüştürme etkinlikleri bağlantılı hizmeti: [Machine Learning toplu iş yürütme etkinliği](#machine-learning-batch-execution-activity), [Machine Learning kaynak güncelleştirme etkinliği](#machine-learning-update-resource-activity). 

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda bir Azure Machine Learning bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Tür |Type özelliği ayarlanmalıdır: **AzureML**. |Evet |
| mlEndpoint |Toplu Puanlama URL. |Evet |
| apiKey |Yayımlanan çalışma alanı modelinin API. |Evet |

#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlı hizmet kullanmadan önce bir Azure data factory hizmetine işlem [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md) ardışık düzeninde.

### <a name="linked-service"></a>Bağlı hizmet

Aşağıdaki tabloda bir Azure Data Lake Analytics bağlantılı hizmeti JSON tanımında kullanılan özellikleri için açıklamalar sağlanır. 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Tür |Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. |Evet |
| accountName |Azure Data Lake Analytics hesap adı. |Evet |
| dataLakeAnalyticsUri |Azure Data Lake Analytics URI. |Hayır |
| Yetkilendirme |Yetkilendirme kodu, tıkladıktan sonra otomatik olarak alınır **Authorize** Data Factory Düzenleyici'düğmesine tıklayın ve OAuth oturum açma tamamlanıyor. |Evet |
| subscriptionId |Azure abonelik kimliği |Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| resourceGroupName |Azure kaynak grubu adı |Hayır (belirtilmezse, kaynak grubu data Factory kullanılır). |
| SessionID |OAuth yetkilendirme oturumundan oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyici kullandığınızda, bu otomatik olarak oluşturulan kimliğidir. |Evet |


#### <a name="json-example"></a>JSON örneği
Aşağıdaki örnek bir Azure Data Lake Analytics bağlantılı hizmeti için JSON tanımını sağlar.

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL bağlı hizmeti oluşturma ve onunla kullanma [saklı yordam etkinliği](#stored-procedure-activity) Data Factory işlem hattı bir saklı yordam çağırmak için. 

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmeti bir Azure SQL veritabanı tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureSqlDatabase**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| connectionString |ConnectionString özelliği için Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Bkz: [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Bir Azure SQL Data Warehouse bağlı hizmet oluşturma ve onunla kullanma [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. 

### <a name="linked-service"></a>Bağlı hizmet
Bağlantılı hizmetinin Azure SQL Data Warehouse tanımlamak için Ayarla **türü** bağlantılı hizmetinin **AzureSqlDW**ve aşağıdaki özellikleri belirtin **typeProperties** bölümü:  

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| connectionString |ConnectionString özelliği için Azure SQL Data Warehouse örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Daha fazla bilgi için bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) makalesi. 

## <a name="sql-server"></a>SQL Server 
Bir SQL Server bağlantılı hizmet oluşturma ve onunla kullanma [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. 

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmet türü oluşturma **OnPremisesSqlServer** bir şirket içi SQL Server veritabanını data factory'ye bağlamak için. Aşağıdaki tabloda şirket içi SQL Server bağlantılı hizmete özgü JSON öğeleri açıklamasını sağlar.

Aşağıdaki tabloda, SQL Server bağlantılı hizmete özgü JSON öğeleri açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| connectionString |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak için gereken connectionString bilgilerini belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\kullanıcıadı**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |

Kimlik bilgilerini kullanarak şifreleyebilirsiniz **yeni AzureRmDataFactoryEncryptValue** cmdlet'i ve bunları aşağıdaki örnekte gösterildiği gibi bağlantı dizesini kullanın (**EncryptedCredential** özellik):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Örnek: SQL kimlik doğrulaması kullanmak için JSON

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Örnek: JSON'ı Windows kimlik doğrulaması kullanma

Kullanıcı adı ve parolası belirtilmişse, ağ geçidi bunları şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabının kimliğine bürün kullanır. Aksi takdirde, ağ geçidi SQL Server Ağ Geçidi (kendi başlangıç hesabı) güvenlik bağlamı ile doğrudan bağlanır.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makalesi.

## <a name="data-transformation-activities"></a>VERİ DÖNÜŞTÜRME ETKİNLİKLERİ

Etkinlik | Açıklama
-------- | -----------
[Hdınsight Hive etkinliği](#hdinsight-hive-activity) | Data Factory işlem hattı Hdınsight Hive etkinliğinde, Hive sorguları kendi veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. 
[Hdınsight Pig etkinliği](#hdinsight-pig-activity) | Data Factory işlem hattı Hdınsight Pig etkinliğinde kendi Pig sorgular veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür.
[HDInsight MapReduce Etkinliği](#hdinsight-mapreduce-activity) | Data Factory işlem hattı Hdınsight MapReduce etkinliğinde kendi MapReduce programları veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür.
[HDInsight Akış Etkinliği](#hdinsight-streaming-activity) | Hdınsight akış etkinliğinde Data Factory işlem hattı Hadoop akış programlar kendi veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür.
[HDInsight Spark Etkinliği](#hdinsight-spark-activity) | Data Factory işlem hattı Hdınsight Spark etkinliğinde Spark programlar kendi Hdınsight kümesinde yürütür. 
[Machine Learning Batch Yürütme Etkinliği](#machine-learning-batch-execution-activity) | Azure Data Factory, Tahmine dayalı analiz için yayımlanan bir Azure Machine Learning web hizmetini kullan ardışık düzen kolayca oluşturmanıza olanak sağlar. Toplu iş yürütme etkinliği Azure Data Factory ardışık düzeninde kullanarak, bir Machine Learning web hizmeti toplu veriler üzerinde tahminlerde çağırabilirsiniz. 
[Machine Learning Kaynak Güncelleştirme Etkinliği](#machine-learning-update-resource-activity) | Zaman içinde denemeler Puanlama Machine Learning Tahmine dayalı modelleri yeni giriş veri kümeleri kullanarak retrained gerekir. Yeniden eğitme ile tamamladıktan sonra ile retrained Machine Learning modeli Puanlama web hizmetini güncelleştirmek istiyor. Web hizmeti ile yeni eğitilen modeli güncelleştirmek için güncelleştirme kaynağı etkinliğini kullanabilirsiniz.
[Saklı Yordam Etkinliği](#stored-procedure-activity) | Aşağıdaki veri depolarına birinde bir saklı yordam çağrılacak bir Data Factory işlem hattı saklı yordam etkinliği kullanabilirsiniz: Azure SQL Database, Azure SQL Data Warehouse, SQL Server veritabanı kuruluşunuzdaki veya bir Azure VM. 
[Data Lake Analytics U-SQL etkinliği](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL etkinliği Azure Data Lake Analytics kümede bir U-SQL komut dosyasını çalıştırır.  
[.NET özel etkinliği](#net-custom-activity) | Veri fabrikası tarafından desteklenmeyen bir şekilde veri dönüştürme ihtiyacınız varsa, kendi veri işleme mantığı ile özel bir etkinlik oluşturmak ve ardışık düzeninde etkinlik kullanın. Bir Azure Batch hizmeti ya da Azure Hdınsight kümesi kullanarak çalıştırmak için özel .NET etkinliği yapılandırabilirsiniz. 

     
## <a name="hdinsight-hive-activity"></a>HDInsight Hive Etkinliği
Bir Hive etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **Hdınsighthive**. Hdınsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölüm için Hdınsighthive etkinliği türünü ayarladığınızda:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| betik |Hive betiği satır içi belirtin |Hayır |
| komut dosyası yolu |Hive betiği bir Azure blob storage'da depolamak ve dosyanın yolunu belirtin. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| tanımlar |'Hiveconf' kullanarak Hive betiğini içinde başvurmak için anahtar/değer çiftleri olarak parametrelerini belirtin |Hayır |

Bu tür özellikleri için Hive etkinliği özgüdür. Diğer özellikler (dışında typeProperties bölüm) tüm etkinlikler için desteklenir.   

### <a name="json-example"></a>JSON örneği
Aşağıdaki JSON ardışık düzeninde Hdınsight Hive etkinliği tanımlar.  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

Daha fazla bilgi için bkz: [Hive etkinliği](data-factory-hive-activity.md) makalesi. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig Etkinliği
Pig etkinlik JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightPig**. Hdınsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için HDInsightPig ayarladığınızda: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| betik |Pig betiği satır içi belirtin |Hayır |
| komut dosyası yolu |Pig betiği bir Azure blob storage'da depolamak ve dosyanın yolunu belirtin. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| tanımlar |Pig betiği içinde başvurmak için anahtar/değer çiftleri olarak parametrelerini belirtin |Hayır |

Bu tür özellikleri, Pig etkinlik özgüdür. Diğer özellikler (dışında typeProperties bölüm) tüm etkinlikler için desteklenir.   

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

Daha fazla bilgi için bkz: [Pig etkinlik](#data-factory-pig-activity.md) makalesi. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce Etkinliği
Bir MapReduce etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightMapReduce**. Hdınsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için HDInsightMapReduce ayarladığınızda: 

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| jarLinkedService | JAR dosyasını içeren Azure depolama bağlı hizmetin adı. | Evet |
| jarFilePath | Azure storage'da JAR dosyasının yolu. | Evet | 
| className | JAR dosyasını ana sınıfının adı. | Evet | 
| Bağımsız değişkenler | Bağımsız değişkenleri MapReduce program virgülle ayrılmış listesi. Çalışma zamanında gördüğünüz birkaç ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle ayırt etmek için seçeneği ve değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s,--giriş,--çıkış vb. olan göre bunların değerleri hemen ardından seçenekleri) | Hayır | 

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix to determine the similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce to generate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

Daha fazla bilgi için bkz: [MapReduce etkinliği](data-factory-map-reduce.md) makalesi. 

## <a name="hdinsight-streaming-activity"></a>HDInsight Akış Etkinliği
Bir Hadoop akış etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightStreaming**. Hdınsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için HDInsightStreaming ayarladığınızda: 

| Özellik | Açıklama | 
| --- | --- |
| Eşleyici | Yürütülebilir Eşleyici adı. Örnekte, cat.exe yürütülebilir Eşleyici ' dir.| 
| reducer | Yürütülebilir reducer adı. Örnekte, wc.exe yürütülebilir reducer ' dir. | 
| Giriş | Eşleyici için girdi dosyası (konum dahil). Örnekte: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample blob kapsayıcısı, veri/örnek/Gutenberg klasördür ve davinci.txt blob. |
| çıkış | Çıktı dosyası (konum dahil) reducer için. Hadoop akış işi çıkışı, bu özellik için belirtilen konuma yazılır. |
| filePaths | Eşleyici ve reducer yürütülebilir dosyalar için yollar. Örnekte: "adfsample/example/apps/wc.exe" adfsample blob kapsayıcısı, örnek/uygulamaları klasördür ve wc.exe çalıştırılabilir. | 
| fileLinkedService | FilePaths bölümünde belirtilen dosyaları içeren Azure depolama temsil eden azure depolama bağlı hizmeti. | 
| Bağımsız değişkenler | Bağımsız değişkenleri MapReduce program virgülle ayrılmış listesi. Çalışma zamanında gördüğünüz birkaç ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle ayırt etmek için seçeneği ve değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s,--giriş,--çıkış vb. olan göre bunların değerleri hemen ardından seçenekleri) | 
| Getdebugınfo | İsteğe bağlı bir öğe. Hata için ayarlandığında, günlükleri yalnızca hatada indirilir. Tüm ayarlandığında, günlükleri yürütme durumu bağımsız olarak daima yüklenir. | 

> [!NOTE]
> Hadoop akış etkinliği için bir çıkış veri kümesi belirtmelisiniz **çıkarır** özelliği. Bu veri kümesi yalnızca ardışık düzen zamanlama (saatlik, günlük, vs.) sürücü için gereken bir kukla dataset olabilir. Etkinliği bir girdi almazsa, etkinliği için bir giriş veri kümesi belirtme atlayabilirsiniz **girişleri** özelliği.  

## <a name="json-example"></a>JSON örneği

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

Daha fazla bilgi için bkz: [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md) makalesi. 

## <a name="hdinsight-spark-activity"></a>HDInsight Spark Etkinliği
Bir Spark etkinlik JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightSpark**. Hdınsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için HDInsightSpark ayarladığınızda: 

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| rootPath | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harf duyarlıdır. | Evet |
| entryFilePath | Spark kod/paketi kök klasörüne göreli yolu. | Evet |
| className | Uygulamanın Java/Spark ana sınıfı | Hayır | 
| Bağımsız değişkenler | Spark programın komut satırı bağımsız değişkenleri listesi. | Hayır | 
| proxyUser | Spark program yürütülmeye kimliğine bürünmek için kullanıcı hesabı | Hayır | 
| sparkConfig | Spark yapılandırma özellikleri. | Hayır | 
| Getdebugınfo | Hdınsight küme tarafından kullanılan Azure depolama Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır | 
| sparkJobLinkedService | Azure Storage bağlı Spark iş dosyası, bağımlılıklar ve günlükleri tutan hizmeti.  Bu özellik için bir değer belirtmezseniz, Hdınsight kümesi ile ilişkili depolama kullanılır. | Hayır |

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
Aşağıdaki noktalara dikkat edin: 

- **Türü** özelliği ayarlanmış **HDInsightSpark**.
- **RootPath** ayarlanır **adfspark\\pyFiles** burada adfspark, Azure Blob kapsayıcısında ve pyFiles kapsayıcıdaki ince klasördür. Bu örnekte, Azure Blob Storage Spark kümesi ile ilişkili adrestir. Farklı bir Azure depolama dosyayı karşıya yükleyebilirsiniz. Bunu yaparsanız, bu depolama hesabı data factory'ye bağlamak için bir Azure depolama bağlantılı hizmeti oluşturun. Ardından, bağlı hizmetin adı için bir değer olarak belirtin **sparkJobLinkedService** özelliği. Bkz: [Spark etkinlik özellikleri](#spark-activity-properties) bu özellik ve Spark etkinlik tarafından desteklenen diğer özellikleri hakkında ayrıntılı bilgi için.
- **EntryFilePath** ayarlanır **test.py**, python dosyası değil. 
- **Getdebugınfo** özelliği ayarlanmış **her zaman**, günlük dosyaları her zaman anlamına gelir (başarılı veya başarısız) oluşturulur.  

    > [!IMPORTANT]
    > Bir sorunu gidermeye çalışıyor değilseniz, bu özellik her zaman bir üretim ortamında ayarlamak değil kullanmanızı öneririz. 
- **Çıkarır** bölümü bir çıkış veri kümesi yok. Spark program herhangi bir çıktı üretmez olsa bile bir çıkış veri kümesi belirtmeniz gerekir. Çıktı veri kümesi, ardışık düzen (saatlik, günlük, vs.) için zamanlamayı sürücüleri.

Etkinliği hakkında daha fazla bilgi için bkz: [Spark etkinlik](data-factory-spark.md) makalesi.  

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning Batch Yürütme Etkinliği
Azure ML toplu iş yürütme etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **AzureMLBatchExecution**. Bağlantılı hizmet ilk öğrenme Azure bir makine oluşturma ve için bir değer olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için AzureMLBatchExecution ayarladığınızda:

Özellik | Açıklama | Gerekli 
-------- | ----------- | --------
webServiceInput | Azure ML web hizmeti için bir giriş olarak geçirilecek veri kümesi. Bu veri kümesi de etkinliğinin girdi bulunması gerekir. |WebServiceInput etkinliğine veya webServiceInputs kullanın. | 
webServiceInputs | Azure ML web hizmeti girişleri olarak geçirilecek veri kümeleri belirtin. Web hizmeti birden fazla girdi aldığı durumlarda WebServiceInput etkinliğine özelliğini kullanmak yerine webServiceInputs özelliğini kullanın. Tarafından başvurulan veri kümeleri **webServiceInputs** de dahil etkinliğin **girişleri**. | WebServiceInput etkinliğine veya webServiceInputs kullanın. | 
webServiceOutputs | Çıktı Azure ML web hizmeti olarak atanan veri kümesi. Web hizmeti, bu veri kümesini çıktı verilerini döndürür. | Evet | 
globalParameters | Bu bölümde web hizmeti parametreleri için değerleri belirtin. | Hayır | 

### <a name="json-example"></a>JSON örneği
Bu örnekte, etkinlik bir veri kümesine sahiptir **MLSqlInput** giriş olarak ve **MLSqlOutput** çıktı olarak. **MLSqlInput** kullanarak geçirilen web hizmeti tarafından bir girdi olarak **WebServiceInput etkinliğine** JSON özelliği. **MLSqlOutput** çıkış olarak Web hizmeti tarafından kullanılarak geçirilir **webServiceOutputs** JSON özelliği. 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

JSON örnekte dağıtılan Azure Machine Learning Web hizmeti, başlangıç/bitiş bir Azure SQL veritabanı veri okuma/yazma için bir okuyucu ve yazıcı modülü kullanır. Aşağıdaki dört parametre bu Web hizmetini sunar: veritabanı sunucu adı, veritabanı adı, sunucu kullanıcı hesabı adı ve sunucu kullanıcı hesabı parolası.

> [!NOTE]
> Yalnızca girişleri ve çıkışları AzureMLBatchExecution etkinliğin Web hizmeti parametreleri olarak geçirilebilir. Örneğin, yukarıdaki JSON parçacığında, bir giriş WebServiceInput etkinliğine parametresi Web hizmeti için bir girdi olarak geçirilen AzureMLBatchExecution etkinliğine MLSqlInput olabilir.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning Kaynak Güncelleştirme Etkinliği
Azure ML güncelleştirme kaynak etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **AzureMLUpdateResource**. Bağlantılı hizmet ilk öğrenme Azure bir makine oluşturma ve için bir değer olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölüm için AzureMLUpdateResource etkinliği türünü ayarladığınızda:

Özellik | Açıklama | Gerekli 
-------- | ----------- | --------
trainedModelName | Retrained modelin adı. | Evet |  
trainedModelDatasetName | Yeniden eğitme işlem tarafından döndürülen iLearner dosyasını işaret eden bir veri kümesi. | Evet | 

### <a name="json-example"></a>JSON örneği
Ardışık düzen iki etkinlik vardır: **AzureMLBatchExecution** ve **AzureMLUpdateResource**. Azure ML toplu iş yürütme etkinliği giriş olarak eğitim verileri alır ve bir iLearner dosya bir çıktı olarak üretir. Etkinlik giriş eğitim verilerle eğitim web hizmeti (web hizmeti olarak sunulan eğitim denemenizi) çağırır ve ilearner dosya webservice alır. PlaceholderBlob Azure Data Factory hizmeti tarafından ardışık çalıştırmak için gerekli olan yalnızca bir kukla çıktı veri kümesi ' dir.


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
U-SQL etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **DataLakeAnalyticsU SQL**. Bir Azure Data Lake Analytics bağlı hizmeti oluşturma ve için bir değer olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** DataLakeAnalyticsU-SQL etkinliği türünü ayarladığınızda, bölüm: 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| scriptPath |U-SQL komut dosyasını içeren klasörün yolu. Dosyanın adı büyük/küçük harf duyarlıdır. |Hayır (komut dosyası kullanırsanız) |
| scriptLinkedService |Veri Fabrikası için komut dosyasını içeren depolamayı bağlı hizmet |Hayır (komut dosyası kullanırsanız) |
| betik |ScriptPath ve scriptLinkedService belirtme yerine satır içi betiği belirtin. Örneğin: "komut dosyası": "Veritabanı oluştur test". |Hayır (scriptPath ve scriptLinkedService kullanıyorsanız) |
| degreeOfParallelism |Aynı anda işi çalıştırmak için kullanılan düğümlerin sayısı. |Hayır |
| öncelik |İlk çalıştırmak için sıraya alınan tüm işlerden seçili belirler. Alt sayısı, öncelik o kadar yüksektir. |Hayır |
| parametreler |U-SQL betiği için parametreler |Hayır |

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

Daha fazla bilgi için bkz: [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md). 

## <a name="stored-procedure-activity"></a>Saklı Yordam Etkinliği
Bir saklı yordam etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **SqlServerStoredProcedure**. Aşağıdaki bağlı hizmetler, bir tane oluşturun ve değeri olarak bağlı hizmetin adı belirtmeniz gerekir **linkedServiceName** özelliği:

- SQL Server 
- Azure SQL Database
- Azure SQL Veri Ambarı

Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için SqlServerStoredProcedure ayarladığınızda:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| storedProcedureName |Azure SQL veritabanı ya da çıktı tablosu kullanır bağlantılı hizmet tarafından temsil edilen Azure SQL Data Warehouse saklı yordamın adını belirtin. |Evet |
| storedProcedureParameters |Saklı yordam parametreleri için değerleri belirtin. Bir parametre için null geçmesi gerekiyorsa, söz dizimini kullanın: "param1": null (tüm küçük harf). Bu özellik kullanma hakkında bilgi edinmek için aşağıdaki örnek bakın. |Hayır |

Bir giriş veri kümesi belirtirseniz, ('Hazır' durumunda) kullanılabilir olmalıdır çalıştırmak saklı yordam etkinliği. Girdi veri kümesi saklı yordam, bir parametre olarak kullanılamaz. Yalnızca saklı yordam etkinliği başlatmadan önce bağımlılık denetlemek için kullanılır. Bir çıkış veri kümesi için bir saklı yordam etkinliği belirtmeniz gerekir. 

Çıktı veri kümesi belirtir **zamanlama** saklı yordam etkinliği (saatlik, haftalık, aylık, vb.). Çıktı veri kümesi kullanmalısınız bir **bağlantılı hizmeti** bir Azure SQL Database veya bir Azure SQL Data Warehouse veya çalıştırmak için saklı yordam istediğiniz SQL Server veritabanı başvuruyor. Çıktı veri kümesi için saklı yordam sonucunu başka bir etkinlik tarafından işleme sonraki geçirmek için bir yol olarak hizmet verebilir ([etkinlikleri zincirleme](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) ardışık düzeninde. Ancak, veri fabrikası otomatik olarak bir saklı yordam çıktısını bu veri kümesine yazmaz. Çıktı veri kümesi işaret eden bir SQL tablosuna Yazar saklı yordam değil. Bazı durumlarda, çıktı veri kümesi olabilir bir **kukla dataset**, yalnızca saklı yordam etkinliği çalıştırmak için zamanlama belirtmek için kullanılır.  

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

Daha fazla bilgi için bkz: [saklı yordam etkinliği](data-factory-stored-proc-activity.md) makalesi. 

## <a name="net-custom-activity"></a>.NET özel etkinliği
.NET özel etkinliğinde JSON tanımını aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **DotNetActivity**. Azure Hdınsight bağlı hizmeti oluşturmak veya bağlı bir Azure Batch hizmeti ve bağlı hizmetin adı için bir değer olarak belirtin **linkedServiceName** özelliği. Aşağıdaki özellikler de desteklenen **typeProperties** bölümünde etkinlik türü için DotNetActivity ayarladığınızda:
 
| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| AssemblyName | Derleme adı. Örnek,: **MyDotnetActivity.dll**. | Evet |
| EntryPoint |IDotNetActivity arabirimini uygulayan sınıfı adı. Örnek,: **MyDotNetActivityNS.MyDotNetActivity** burada MyDotNetActivityNS ad alanı ve MyDotNetActivity sınıfı.  | Evet | 
| PackageLinkedService | Özel Etkinlik zip dosyasını içeren blob depolama alanına işaret Azure Storage bağlı hizmetin adı. Örnek,: **AzureStorageLinkedService**.| Evet |
| PackageFile | ZIP dosyasının adı. Örnek,: **customactivitycontainer/MyDotNetActivity.zip**. | Evet |
| extendedProperties | Tanımlayın ve .NET kodu için geçirin genişletilmiş özellikler. Bu örnekte, **SliceStart** değişkeni SliceStart sistem değişkeni göre bir değere ayarlanır. | Hayır | 

### <a name="json-example"></a>JSON örneği

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

Ayrıntılı bilgi için bkz: [veri fabrikasında özel etkinlikleri kullanmak](data-factory-use-custom-activities.md) makalesi. 

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki öğreticilere bakın: 

- [Öğretici: kopyalama etkinliği ile işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Öğretici: hive etkinliği ile işlem hattı oluşturma](data-factory-build-your-first-pipeline-using-editor.md)
