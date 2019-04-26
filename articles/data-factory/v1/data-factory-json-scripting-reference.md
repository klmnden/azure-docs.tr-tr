---
title: Azure Data Factory - JSON betik oluşturma başvurusu | Microsoft Docs
description: Data Factory varlıkları için JSON şemalarının sağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 25cf9c3b7968be16dcc22f4140725efc22d785f2
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528416"
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Data Factory - JSON betik oluşturma başvurusu
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir.


Bu makalede, Azure Data Factory varlıklarını (işlem hattı, etkinlik, veri kümesi ve bağlı hizmet) tanımlamak için JSON şemalarının ve örnekler sağlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="pipeline"></a>İşlem hattı
İşlem hattı için üst düzey yapısını aşağıdaki gibidir:

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

Aşağıdaki tabloda, işlem hattı JSON tanımını içindeki özellikleri açıklar:

| Özellik | Açıklama | Gerekli
-------- | ----------- | --------
| ad | İşlem hattının adı. Eylemi temsil eden bir ad belirtin etkinlik veya işlem hattı yapmak için yapılandırılır<br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi ile başlamalıdır (\_)</li><li>Karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama |Ne işlem hattı ve etkinlik için kullanıldığını açıklayan metin | Hayır |
| etkinlikler | Etkinliklerin listesini içerir. | Evet |
| start |İşlem hattının başlangıç tarihi / saati. Olmalıdır [ISO biçimi](https://en.wikipedia.org/wiki/ISO_8601). Örneğin: 2014-10-14T16:32:41. <br/><br/>Yerel saati, örneğin bir Tah belirtmek mümkündür. Bir örnek aşağıda verilmiştir: `2016-02-27T06:00:00**-05:00`, 6 AM tahmini olduğu<br/><br/>Başlangıç ve bitiş özellikleri işlem hattının etkin dönemini birlikte belirtin. Çıktı dilimleri yalnızca ile bu etkin dönem içinde oluşturulur. |Hayır<br/><br/>End özelliği için bir değer belirtirseniz, başlangıç özelliği için değer belirtmeniz gerekir.<br/><br/>Başlangıç ve bitiş saatleri hem de bir işlem hattı oluşturmak için boş olabilir. Çalıştırılacak işlem hattının etkin bir süresini ayarlamak için her iki değer belirtmeniz gerekir. Başlangıç ve bitiş zamanı belirtmezseniz, işlem hattını oluştururken, bunları daha sonra Set-AzDataFactoryPipelineActivePeriod cmdlet'ini kullanarak ayarlayabilirsiniz. |
| end |İşlem hattının son tarih-saat. Belirtilen ISO biçiminde olmalıdır. Örneğin: 2014-10-14T17:32:41 <br/><br/>Yerel saati, örneğin bir Tah belirtmek mümkündür. Bir örnek aşağıda verilmiştir: `2016-02-27T06:00:00**-05:00`, 6 AM tahmini olduğu<br/><br/>İşlem hattını süresiz olarak çalıştırmak için 9999-09-09 son özelliğinin değeri olarak belirtin. |Hayır <br/><br/>Başlangıç özellik için bir değer belirtirseniz, end özelliği için değer belirtmeniz gerekir.<br/><br/>İçin Notlar'a bakın **Başlat** özelliği. |
| isPaused |İşlem hattı true olarak ayarlanırsa çalışmazsa. Varsayılan değer = false. Bu özelliği etkinleştirmek veya devre dışı bırakmak için kullanabilirsiniz. |Hayır |
| pipelineMode |İşlem hattı çalıştırmaları zamanlamak için yöntem. İzin verilen değerler: (varsayılan), zamanlanmış onetime.<br/><br/>'Zamanlanmış' işlem hattı, belirtilen zaman aralığı (başlangıç ve bitiş saati) etkin süresinin göre çalıştırıldığını gösterir. 'Onetime' işlem hattı yalnızca bir kez çalıştırıldığını gösterir. Tek seferlik işlem hatları oluşturulduktan sonra değişiklik ve güncelleştirilmiş olamaz. Bkz: [Onetime işlem hattı](data-factory-create-pipelines.md#onetime-pipeline) onetime ayarı hakkında ayrıntılı bilgi için. |Hayır |
| expirationTime |İşlem hattı geçerli olduğunu ve sağlanan kalmalıdır, oluşturulduktan sonra süre. Tüm etkin, başarısız, yok veya işlem hattı çalıştırmaları otomatik olarak bir kez silinir sona erme zamanı ulaşır. |Hayır |


## <a name="activity"></a>Etkinlik
Bir işlem hattı tanımındaki (etkinlikleri öğesi) bir etkinlik için üst düzey yapısını aşağıdaki gibidir:

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
    },
    "scheduler":
    {
    }
}
```

Tablo, içinde etkinlik JSON tanımındaki özellikler açıklanmaktadır:

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinliğin adı. Eylemi temsil eden bir ad belirtin, etkinlik yapılandırılması<br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi ile başlamalıdır (\_)</li><li>Karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama |Etkinliğin ne için kullanıldığını açıklayan metin. |Hayır |
| type |Etkinlik türünü belirtir. Bkz: [veri DEPOLARI](#data-stores) ve [veri dönüştürme etkinlikleri](#data-transformation-activities) bölümleri farklı etkinlik türleri için. |Evet |
| girişler |Etkinlik tarafından kullanılan giriş tablosu<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Hdınsightstreaming ve SqlServerStoredProcedure etkinlikler için Hayır <br/> <br/> Diğer tümü için Evet |
| çıkışlar |Etkinlik tarafından kullanılan çıkış tablolar.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Evet |
| linkedServiceName |Etkinlik tarafından kullanılan bağlı hizmetin adı. <br/><br/>Bir etkinlik için gerekli işlem ortamına bağlanan bağlı hizmeti belirtmeniz gerekebilir. |HDInsight etkinlikleri, Azure Machine Learning etkinlikleri ve saklı yordam etkinliği için Evet. <br/><br/>Diğer tümü için hayır |
| typeProperties |TypeProperties bölümündeki özellikler etkinlik türüne bağlıdır. |Hayır |
| ilke |Etkinliğin çalışma zamanı davranışını etkileyen ilkeler. Belirtilmezse, varsayılan ilkeler kullanılır. |Hayır |
| Zamanlayıcı |"Zamanlayıcı" özelliği, istenen etkinlik için zamanlama tanımlamak için kullanılır. Onun alt dışındaki aynıdır [bir veri kümesi kullanılabilirlik özelliğinde](data-factory-create-datasets.md#dataset-availability). |Hayır |

### <a name="policies"></a>İlkeler
Özellikle, bir tablonun dilim işlendiğinde ilkeler bir etkinliğin çalışma zamanı davranışını etkiler. Aşağıdaki tabloda ayrıntılar sağlar.

| Özellik | İzin verilen değerler | Varsayılan Değer | Açıklama |
| --- | --- | --- | --- |
| Eşzamanlılık |Tamsayı <br/><br/>En büyük değer: 10 |1 |Etkinliğin eşzamanlı yürütmelerinin sayısı.<br/><br/>Bu, üzerinde farklı dilimleri oluşabilir paralel Etkinlik yürütme sayısını belirler. Örneğin, bir etkinlik geçtikleri gerekiyorsa, çok sayıda büyük eşzamanlılık değeri, kullanılabilir verilerin veri işleme hızı artar. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |İşlenmekte olan veri dilimi sıralama belirler.<br/><br/>Örneğin, varsa (4'te, bir gerçekleşmesini ve başka bir saat 17: 00) 2 böler ve hem de yürütme olması. Dilim saat 17: 00, executionPriorityOrder NewestFirst olacak şekilde ayarlarsanız, önce işlenir. ExecutionPriorityORder OldestFIrst olacak şekilde ayarlarsanız, benzer şekilde ardından 4'te en işlenir. |
| retry |Tamsayı<br/><br/>En büyük değer 10 olabilir |0 |Dilimin veri işleme hatası olarak işaretlenmeden önce yeniden deneme sayısı. Veri dilimi için etkinlik yürütme belirtilen yeniden deneme sayısı en fazla yeniden denenir. Yeniden deneme hatadan sonra mümkün olan en kısa sürede gerçekleştirilir. |
| timeout |TimeSpan |00:00:00 |Etkinlik için zaman aşımı. Örnek: 00:10:00 (zaman aşımı 10 dakika anlamına gelir)<br/><br/>Bir değer belirtilmezse veya 0'dır, zaman aşımı sonsuz olur.<br/><br/>Dilim üzerinde veri işleme süresi zaman aşımı değerini aşarsa, iptal edilir ve sistem işleme yeniden dener. Yeniden deneme sayısını, yeniden deneme özelliğine bağlıdır. Zaman aşımı meydana geldiğinde, durum zaman aşımına uğradı için ayarlanır. |
| gecikme |TimeSpan |00:00:00 |Veri işleme dilim başlatılmadan önce gecikme belirtin.<br/><br/>Etkinlik bir veri diliminin yürütülmesi, gecikmenin beklenen yürütme süresi sonra başlatılır.<br/><br/>Örnek: 00:10:00 (10 dakika gecikme anlamına gelir) |
| longRetry |Tamsayı<br/><br/>En büyük değer: 10 |1 |Dilim yürütme başarısız olmadan önce uzun yeniden deneme sayısı.<br/><br/>denemeleri longRetry, longretryınterval gibi tarafından aralıklandırılmış. Yeniden deneme girişimleri arasındaki süre belirtmeniz gerekiyorsa, bu nedenle longRetry kullanın. Yeniden deneme longRetry belirtilirse, yeniden deneme girişimleri longRetry içerir ve yeniden deneme girişimlerinin sayısı en fazla olan * longRetry.<br/><br/>Örneğin etkinlik ilkesinde aşağıdaki ayarları sunuyoruz:<br/>Yeniden deneme: 3<br/>longRetry: 2<br/>longretryınterval gibi: 01:00:00<br/><br/>Yürütmek için yalnızca bir dilim olduğu varsayılır (Durum Bekliyor) ve her etkinlik yürütme başarısız olur. İlk 3 ardışık yürütme girişimleri olacaktır. Her girişimden sonra dilim durumu yeniden deneme olacaktır. İlk 3 deneme üzerinden sonra dilim durumu LongRetry olacaktır.<br/><br/>Bir saat sonra (diğer bir deyişle, longRetryInteval'ın değer), 3 ardışık yürütme girişimleri başka bir dizi olacaktır. Bundan sonra dilim durumu başarısız ve daha fazla yeniden deneme yok çalıştı. Bu nedenle genel 6 denemesi yapıldı.<br/><br/>Herhangi bir yürütme başarılı olursa, dilim durumu hazır olur ve daha fazla yeniden deneme yok çalıştı.<br/><br/>longRetry olduğu bağımlı veri belirleyici olmayan zamanlarda ulaşır ya da genel ortamının hangi verileri işlemesi altında güvenilir olmayan durumlarda kullanılabilir. Bu gibi durumlarda, bunun yapılması deneme birbiri ardına yardımcı ve bunun yapılması bir aralıktan sonra istenen çıkış sonuçlarında zaman.<br/><br/>Uyarı: longRetry veya longretryınterval gibi yüksek değerlerini ayarlamayın. Genellikle, yüksek değerler sistemle ilgili diğer konuları da kapsıyor. |
| longRetryInterval |TimeSpan |00:00:00 |Uzun yeniden deneme girişimleri arasındaki gecikme |

### <a name="typeproperties-section"></a>typeProperties bölümünün
TypeProperties bölümünün her etkinlik için farklıdır. Dönüştürme etkinlikleri, yalnızca tür özellikleri vardır. Bkz: [veri dönüştürme etkinlikleri](#data-transformation-activities) dönüştürme etkinlikleri bir işlem hattında tanımlayan JSON örnekleri için bu makaledeki bölümü.

**Kopyalama etkinliği** typeProperties bölümünün iki alt bölümlere sahiptir: **kaynak** ve **havuz**. Bkz: [veri DEPOLARI](#data-stores) bir kaynak ve/veya havuz veri kullanmayı gösteren JSON örneklerini depolamak için bu makaledeki bölümü.

### <a name="sample-copy-pipeline"></a>Örnek kopyalama işlem hattı
Aşağıdaki örnek işlem hattında, **Etkinlikler** bölümünde **Kopyalama** türünde olan bir etkinlik vardır. Bu örnekte [kopyalama etkinliği](data-factory-data-movement-activities.md) verileri bir Azure Blob depolama alanından Azure SQL veritabanına kopyalar.

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

Bkz: [veri DEPOLARI](#data-stores) bir kaynak ve/veya havuz veri kullanmayı gösteren JSON örneklerini depolamak için bu makaledeki bölümü.

Bu işlem hattını oluşturmak üzere izlenecek tam yol için bkz: [Öğreticisi: Blob depolama alanından SQL veritabanı'na veri kopyalamak](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

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
* **Tanımlar** bölümü, hive betiğine Hive yapılandırma değerleri olarak geçirilen çalışma zamanı ayarlarını belirtmek için kullanılır (ör. `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Bkz: [veri dönüştürme etkinlikleri](#data-transformation-activities) dönüştürme etkinlikleri bir işlem hattında tanımlayan JSON örnekleri için bu makaledeki bölümü.

Bu işlem hattını oluşturmak üzere izlenecek tam yol için bkz: [Öğreticisi: Hadoop kümesi kullanarak verileri işlemek için ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md).

## <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmet tanımı için üst düzey yapısını aşağıdaki gibidir:

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

Tablo, içinde etkinlik JSON tanımındaki özellikler açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| ad | Bağlı hizmetin adı. | Evet |
| Özellikler - türü | Bağlı hizmet türü. Örneğin: Azure depolama, Azure SQL veritabanı. |
| typeProperties | TypeProperties bölümünün her veri deposu için farklı işlem ortamı öğeleri var. Bağlı hizmetler tüm veri depolamak için veri depolarını bölümüne bakın ve [ortamları işlem](#compute-environments) için tüm işlem bağlı Hizmetleri |

## <a name="dataset"></a>Veri kümesi
Azure Data factory'de bir veri kümesi şu şekilde tanımlanır:

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

Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| ad | Veri kümesinin adı. Bkz: [Azure Data Factory - adlandırma kuralları](data-factory-naming-rules.md) adlandırma kuralları. |Evet |NA |
| type | Veri kümesi türü. Azure Data Factory tarafından desteklenen türlerinden birini belirtin (örneğin: AzureBlob, AzureSqlTable). Bkz: [veri DEPOLARI](#data-stores) bölümde tüm Data Factory tarafından desteklenen veri türleri ve veri depoları için. |
| yapısı | Şema kümesi. Bu sütun, türleri, vb. içerir. | Hayır |NA |
| typeProperties | Seçili türüne karşılık gelen özellikleri. Bkz: [veri DEPOLARI](#data-stores) desteklenen türleri ve bunların özelliklerini bölümü. |Evet |NA |
| external | Bir veri kümesi açıkça bir veri fabrikası işlem hattı tarafından veya üretilen olup olmadığını belirlemek için Boole bayrağı. |Hayır |false |
| availability | Veri kümesi üretim için dilimleme modelinin veya işleme penceresini tanımlar. Model dilimleme veri kümesi hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makalesi. |Evet |NA |
| ilke |Ölçüt veya veri kümesinin dilimlerini karşılamalıdır koşulu tanımlar. <br/><br/>Ayrıntılar için bkz: veri kümesi İlkesi bölümü. |Hayır |NA |

Her sütunda **yapısı** bölümü aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü.  |Hayır |
| Kültür |.NET tabanlı türü belirtildi ve .NET türü olduğunda kullanılacak kültürü `Datetime` veya `Datetimeoffset`. `en-us` varsayılan değerdir. |Hayır |
| biçim |Biçim türü belirtildi ve .NET türü olduğunda kullanılacak dize `Datetime` veya `Datetimeoffset`. |Hayır |

Aşağıdaki örnekte, üç sütun bir veri kümesine sahiptir `slicetimestamp`, `projectname`, ve `pageviews` ve bunlar türü: Dize, dize ve ondalık sırasıyla.

```json
structure:
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Aşağıdaki tabloda kullanabileceğiniz özellikleri açıklanmaktadır **kullanılabilirlik** bölümü:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilim üretim yönelik zaman birimini belirtir.<br/><br/><b>Sıklık desteklenen</b>: Dakika, saat, gün, hafta, ay |Evet |NA |
| interval |Sıklığı çarpanı belirtir<br/><br/>"X sıklık aralığı" ne sıklıkta dilim üretilir belirler.<br/><br/>Veri kümesinin saatlik olarak dilimlenmiş gerekiyorsa, ayarladığınız <b>sıklığı</b> için <b>saat</b>, ve <b>aralığı</b> için <b>1</b>.<br/><br/><b>Not</b>: Sıklığını dakika belirtmeniz durumunda da en az 15'e aralığı ayarlamanızı öneririz |Evet |NA |
| Stil |Dilim aralığı başlangıç/bitiş sırasında üretilen olup olmadığını belirtir.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Frequency ay ayarlanır ve stil EndOfInterval için ayın son gününde dilim üretilir. Stili için StartOfInterval ayarlarsanız, ayın ilk gününde dilim üretilir.<br/><br/>Sıklığı gün olarak ayarlanır ve stil EndOfInterval için dilim günün son bir saat içinde üretilmez.<br/><br/>Sıklık saat olarak ayarlanır ve stil EndOfInterval için dilim saatin sonunda üretilmez. Örneğin, 2 saat 13 – PM dönem için bir dilim için 2 saat dilim üretilir. |Hayır |EndOfInterval |
| anchorDateTime |Zamanlayıcı tarafından veri kümesi dilim sınırlarını hesaplamak için kullanılan zaman içinde mutlak konum tanımlar. <br/><br/><b>Not</b>: AnchorDateTime sıklığından daha ayrıntılı tarih kısımlarını varsa daha ayrıntılı bölümleri göz ardı edilir. <br/><br/>Örneğin, varsa <b>aralığı</b> olduğu <b>saatlik</b> (Sıklık: saat ve aralığı: (1) ve <b>AnchorDateTime</b> içeren <b>dakika ve saniye</b> sonra <b>dakika ve saniye</b> AnchorDateTime kısımlarını yok sayılır. |Hayır |01/01/0001 |
| uzaklık |Başlangıç ve bitiş tüm veri kümesi dilim olarak kaydırılan bir TimeSpan değeri. <br/><br/><b>Not</b>: AnchorDateTime hem uzaklık belirtilirse, sonuç birleşik bir kaydırmadır. |Hayır |NA |

Aşağıdaki kullanılabilirlik bölümü çıktı veri kümesi üretilen saatlik (veya) giriş olduğunu belirtir veri kümesi, kullanılabilir saat:

```json
"availability":
{
    "frequency": "Hour",
    "interval": 1
}
```

**İlke** veri kümesi tanımı bölümünde ölçütleri veya veri kümesinin dilimlerini karşılamalıdır koşulu tanımlar.

| İlke Adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Doğrulama verilerde bir **Azure blob** (megabayt cinsinden) en küçük boyut gereksinimlerini karşılıyor. |Azure Blob |Hayır |NA |
| minimumRows |Doğrulama verilerde bir **Azure SQL veritabanı** veya bir **Azure tablo** en az sayıda satır içerir. |<ul><li>Azure SQL Veritabanı</li><li>Azure Tablosu</li></ul> |Hayır |NA |

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

Azure Data Factory tarafından üretilen veri kümesi sürece bu olarak işaretlenmelidir **dış**. Etkinlik veya işlem hattı zincirleme kullanılmadığı sürece bu ayar genellikle bir işlem hattındaki ilk etkinliğin girişleri için geçerlidir.

| Ad | Açıklama | Gerekli | Varsayılan Değer |
| --- | --- | --- | --- |
| dataDelay |Belirtilen dilim için dış veri kullanılabilirliğini kontrolü gecikme süresi. Verilerin saatlik varsa, örneğin, dış veri kullanılabilir ve karşılık gelen dilimi hazır olduğunu görmek için onay dataDelay kullanılarak ertelenebilir.<br/><br/>Yalnızca mevcut bir süre için geçerlidir.  Örneğin, 13: 00'te şu anda ise ve bu değer 10 dakikadır doğrulama 13: 10'te başlatır.<br/><br/>Bu ayar, geçmiş dilimler etkilemez (dilimler dilim bitiş zamanı + dataDelay < artık) herhangi bir gecikme olmadan işlenir.<br/><br/>Saat 23:59 saat gereken kullanarak belirtilenden daha büyük `day.hours:minutes:seconds` biçimi. Örneğin, 24 saat belirtmek için 24:00:00 kullanmayın; Bunun yerine, 1.00:00:00 kullanın. 24:00:00 kullanırsanız, 24 gün (24.00:00:00) kabul edilir. 1 gün ve 4 saat, 1:04:00:00 belirtin. |Hayır |0 |
| Retryınterval |Sonraki hata arasındaki bekleme süresi yeniden deneme girişimi. Bir deneme başarısız olursa, sonraki deneyin Retryınterval sonra olur. <br/><br/>1:00 PM şu anda ise, ilk denemede başlamadan. İlk doğrulama denetimini tamamlamak için süre 1 dakika ve işlem başarısız oldu, sonraki yeniden deneme ise 1:00 + 1 dakika (süre) + 1 dakika (yeniden deneme aralığı) 13: 02'te =. <br/><br/>Geçmiş dilimler için gecikme yoktur. Yeniden deneme hemen gerçekleşir. |Hayır |00:01:00 (1 dakika) |
| retryTimeout |Her yeniden deneme girişimi için zaman aşımı.<br/><br/>Bu özellik, 10 dakika olarak ayarlanır, doğrulama, 10 dakika içinde tamamlanması gerekir. Doğrulamayı gerçekleştirmek için 10 dakikadan uzun sürerse, yeniden deneme zaman aşımına uğradı.<br/><br/>Doğrulama için tüm girişimleri zaman aşımına uğrarsa, dilim zaman aşımına uğradı işaretlenir. |Hayır |00:10:00 (10 dakika) |
| maximumRetry |Dış veri kullanılabilirliğini denetleyin sayısı. İzin verilen en yüksek değer 10'dur. |Hayır |3 |


## <a name="data-stores"></a>VERİ DEPOLARI
[Bağlı hizmet](#linked-service) bağlı hizmetler tüm türleri için ortak olan JSON öğeler için sağlanan bölüm açıklamalar. Bu bölümde, her bir veri deposuna özel JSON öğeleri hakkında ayrıntılar sağlar.

[Veri kümesi](#dataset) tüm veri kümesi türü için ortak olan JSON öğeler için sağlanan bölüm açıklamalar. Bu bölümde, her bir veri deposuna özel JSON öğeleri hakkında ayrıntılar sağlar.

[Etkinlik](#activity) tüm etkinlik türleri için ortak olan JSON öğelerinin açıklamaları sağlanan bölüm. Bu bölümde, bir kopyalama etkinliği kaynak/havuz olarak kullanıldığında, her bir veri deposuna özel JSON öğeleri hakkında ayrıntılar sağlar.

Kopyalama etkinliği için kaynak/havuz bağlantılı hizmet ve veri kümesi için JSON şemalarının görmek ilgilendiğiniz deposu bağlantısına tıklayın.

| Kategori | Veri deposu
|:--- |:--- |
| **Azure** |[Azure Blob Depolama](#azure-blob-storage) |
| &nbsp; |Azure Data Lake Store |
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
| &nbsp; |Web Tablosu |

## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmetler, iki tür vardır: Azure depolama bağlı hizmeti ve Azure depolama SAS bağlı hizmeti.

#### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
Kullanarak, Azure depolama hesabınızı veri fabrikasına bağlamak için **hesap anahtarı**, bir Azure depolama bağlı hizmeti oluşturma. Bağlı hizmeti bir Azure depolama tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureStorage**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| bağlantı dizesi |ConnectionString özelliği için Azure depolamaya bağlanmak için gereken bilgileri belirtin. |Evet |

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

#### <a name="azure-storage-sas-linked-service"></a>Azure depolama SAS bağlı hizmeti
Azure depolama SAS bağlı hizmet, bir paylaşılan erişim imzası (SAS) kullanarak bir Azure data factory'de bir Azure depolama hesabı bağlantı sağlar. Data factory ile kısıtlı/zamana bağlı depolama (blob/kapsayıcı) tüm/özel kaynaklarına erişimi sağlar. Paylaşılan erişim imzası kullanarak Azure depolama hesabınızı veri fabrikasına bağlamak için bir Azure depolama SAS bağlı hizmet oluşturun. Bağlı hizmeti Azure depolama SAS tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureStorageSas**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| sasUri |Blob, kapsayıcı veya tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI'si belirtin. |Evet |

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

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz. [Azure Blob Depolama Bağlayıcısı](data-factory-azure-blob-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure Blob veri kümesi tanımlamak için **türü** için veri kümesinin **AzureBlob**. Ardından, aşağıdaki Azure Blob belirli özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Kapsayıcı ve blob depolama alanında bir klasör yolu. Örnek: myblobcontainer\myblobfolder\ |Evet |
| fileName |Blob adı. İsteğe bağlı ve büyük küçük harfe duyarlı dosya adıdır.<br/><br/>Etkinlik (kopyalama dahil), bir filename belirtirseniz, belirli bir blobu üzerinde çalışır.<br/><br/>Dosya adı belirtilmemişse, kopya tüm BLOB'ları folderPath için giriş veri kümesi içerir.<br/><br/>Oluşturulan dosyanın adını bir çıktı veri kümesi için dosya adı belirtilmediği durumlarda, aşağıdaki olacaktır Bu biçim: `Data.<Guid>.txt` (örneğin:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy isteğe bağlı bir özelliktir. Bir dinamik folderPath ve zaman serisi verileri için dosya adı belirtmek için kullanabilirsiniz. Örneğin, saatte veri folderPath parametreli olabilir. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

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


Daha fazla bilgi için [Azure Blob Bağlayıcısı](data-factory-azure-blob-connector.md#dataset-properties) makalesi.

### <a name="blobsource-in-copy-activity"></a>Kopyalama etkinliğindeki BlobSource
Bir Azure Blob depolamadan veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **BlobSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |(Varsayılan değer) true, False |Hayır |

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
### <a name="blobsink-in-copy-activity"></a>Kopyalama etkinliğindeki BlobSink
Bir Azure Blob depolama alanına veri kopyalama verilirse **Havuz türü** , kopyalama etkinliğine **BlobSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |<b>PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><br/><b>FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik adına sahip. <br/><br/><b>MergeFiles (varsayılan):</b> tüm dosyaları kaynak klasörden bir dosya birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilmezse, belirtilen adı olur; Aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

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

Daha fazla bilgi için [Azure Blob Bağlayıcısı](data-factory-azure-blob-connector.md#copy-activity-properties) makalesi.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Azure Data Lake Store tanımlamak için bağlı hizmetinin türü **birlikte AzureDataLakeStore**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureDataLakeStore** | Evet |
| dataLakeStoreUri | Azure Data Lake Store hesabınız hakkında bilgiler belirtin. Aşağıdaki biçimdedir: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store ait olduğu azure abonelik kimliği. | Havuz için gerekli |
| resourceGroupName | Data Lake Store ait olduğu azure kaynak grubu adı. | Havuz için gerekli |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet (hizmet sorumlusu kimlik doğrulaması için) |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. | Evet (hizmet sorumlusu kimlik doğrulaması için) |
| kiracı | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet (hizmet sorumlusu kimlik doğrulaması için) |
| Yetkilendirme | Tıklayın **Authorize** düğmesine **Data Factory düzenleyici** ve bu özelliği otomatik olarak oluşturulan yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet (kimlik bilgisi için kullanıcı kimlik doğrulaması)|
| oturum kimliği | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Bu ayar, Data Factory Düzenleyici kullandığınızda otomatik olarak oluşturulur. | Evet (kimlik bilgisi için kullanıcı kimlik doğrulaması) |

#### <a name="example-using-service-principal-authentication"></a>Örnek: hizmet sorumlusu kimlik doğrulaması kullanma
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

Daha fazla bilgi için [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure Data Lake Store veri kümesini tanımlamak için **türü** için veri kümesinin **birlikte AzureDataLakeStore**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| folderPath |Kapsayıcı ve Azure veri Gölü'nde klasörü yoluna depolar. |Evet |
| fileName |Azure Data Lake Store'da dosyasının adı. İsteğe bağlı ve büyük küçük harfe duyarlı dosya adıdır. <br/><br/>Bir dosya adı belirtirseniz, etkinlik (kopyalama dahil) belirli bir dosya üzerinde çalışır.<br/><br/>Dosya adı belirtilmemişse, kopyalama folderPath giriş veri kümesi için tüm dosyaları içerir.<br/><br/>Oluşturulan dosyanın adını bir çıktı veri kümesi için dosya adı belirtilmediği durumlarda, aşağıdaki olacaktır Bu biçim: `Data.<Guid>.txt` (örneğin:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy isteğe bağlı bir özelliktir. Bir dinamik folderPath ve zaman serisi verileri için dosya adı belirtmek için kullanabilirsiniz. Örneğin, saatte veri folderPath parametreli olabilir. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

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

Daha fazla bilgi için [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#dataset-properties) makalesi.

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Azure Data Lake Store kaynağında kopyalama etkinliği
Bir Azure Data Lake Store ' veri kopyalıyorsanız ayarlayın **kaynak türünü** , kopyalama etkinliğine **kümesinin kullanılması gerekir**, aşağıdaki özellikleri belirtin **kaynak**bölümü:

**Kümesinin kullanılması gerekir** aşağıdaki özellikleri destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |(Varsayılan değer) true, False |Hayır |

#### <a name="example-azuredatalakestoresource"></a>Örnek: Kümesinin kullanılması gerekir

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

Daha fazla bilgi için [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#copy-activity-properties) makalesi.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Kopyalama etkinliğindeki havuz Azure Data Lake Store
Bir Azure Data Lake Store için veri kopyalama verilirse **Havuz türü** , kopyalama etkinliğine **AzureDataLakeStoreSink**, aşağıdaki özellikleri belirtin **havuz** Bölüm:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kopyalama davranışını belirtir. |<b>PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><br/><b>FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk düzeyini hedef klasörü içinde oluşturulur. Hedef dosyalar otomatik adıyla oluşturulur.<br/><br/><b>MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilmezse, belirtilen adı olur; Aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

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

Daha fazla bilgi için [Azure Data Lake Store bağlayıcı](data-factory-azure-datalake-connector.md#copy-activity-properties) makalesi.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Azure Cosmos DB tanımlamak için Ayarla **türü** bağlı hizmetinin **DocumentDb**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| bağlantı dizesi |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. |Evet |

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
Daha fazla bilgi için [Azure Cosmos DB Bağlayıcısı](data-factory-azure-documentdb-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure Cosmos DB veri kümesini tanımlamak için **türü** için veri kümesinin **DocumentDbCollection**ve şu özelliklerde belirtin **typeProperties** bölümü:

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| collectionName |Azure Cosmos DB koleksiyonu adı. |Evet |

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
Daha fazla bilgi için [Azure Cosmos DB Bağlayıcısı](data-factory-azure-documentdb-connector.md#dataset-properties) makalesi.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Kopyalama etkinliği, Azure Cosmos DB koleksiyonu kaynağı
Bir Azure Cosmos DB'den verileri kopyalama verilirse **kaynak türü** için kopyalama etkinliği, **DocumentDbCollectionSource**, aşağıdaki özellikleri belirtin **kaynak**bölümü:


| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için bir sorgu belirtin. |Azure Cosmos DB tarafından desteklenen dize sorgulayın. <br/><br/>Örnek: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmezse, yürütülen SQL deyimi: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş belirtmek için özel karakter |Herhangi bir karakter. <br/><br/>Azure Cosmos DB iç içe geçmiş yapılar burada izin verilen bir NoSQL JSON belgeleri için deposudur. Azure Data Factory sağlayan kullanıcı hiyerarşisi olan nestingSeparator aracılığıyla belirtmek "." Yukarıdaki örneklerde. Ayırıcı ile kopyalama etkinliği ile üç alt öğeleri "Name" nesne ilk olarak, oluşturacak Orta ve son olarak, "Name.First", "Name.Middle" ve "Name.Last" Tablo tanımındaki göre. |Hayır |

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

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Kopyalama etkinliği, Azure Cosmos DB koleksiyonu havuz
Azure Cosmos DB'ye veri kopyalıyorsanız ayarlamak **Havuz türü** , kopyalama etkinliğine **DocumentDbCollectionSink**, aşağıdaki özellikleri belirtin **havuz** bölümü :

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| nestingSeparator |Bir iç içe geçmiş belge belirtmek için kaynak sütun adı özel karakterler gereklidir. <br/><br/>Örneğin yukarıdaki: `Name.First` Cosmos DB belgesini aşağıdaki JSON yapısında tablo çıktısında oluşturur:<br/><br/>"Name": {<br/>    "First": "John"<br/>}, |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir.<br/><br/>Varsayılan değer `.` (nokta). |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir. <br/><br/>Varsayılan değer `.` (nokta). |
| writeBatchSize |Belgeleri oluşturmak için Azure Cosmos DB hizmetine paralel isteklerinin sayısı.<br/><br/>Bu özelliği kullanarak Azure Cosmos DB/deposundan veri kopyalama işlemi sırasında performans hassas ayarlamalar yapabilirsiniz. Azure Cosmos DB için daha fazla paralel istekler gönderildiği writeBatchSize artırdığınızda daha iyi bir performans bekleyebilirsiniz. Ancak, azaltmayı önlemek gerekir, hata iletisi oluşturabilecek: "İstek oranı büyük".<br/><br/>Azaltma, belgeler, belgeleri koşullarını sayısı boyutu da dahil olmak üzere, dizin oluşturma ilkesi hedef koleksiyon, vb. faktörleri sayısına göre belirlenir. Kopyalama işlemleri için en iyi kullanılabilir işleme sağlamak için daha iyi bir koleksiyon (örneğin, S3) kullanabilirsiniz (2.500 istek birimi/saniye). |Tamsayı |Hayır (varsayılan: 5) |
| writeBatchTimeout |İşlem zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |Zaman aralığı<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |

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

Daha fazla bilgi için [Azure Cosmos DB Bağlayıcısı](data-factory-azure-documentdb-connector.md#copy-activity-properties) makalesi.

## <a name="azure-sql-database"></a>Azure SQL Veritabanı

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti Azure SQL veritabanı tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureSqlDatabase**, aşağıdaki özellikleri belirtin **typeProperties** Bölüm:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| bağlantı dizesi |ConnectionString özelliği için Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

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

Daha fazla bilgi için [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure SQL veritabanı veri kümesi tanımlamak için **türü** için veri kümesinin **AzureSqlTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm bağlı hizmeti Azure SQL veritabanı örneğinde başvurduğu adı. |Evet |

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
Daha fazla bilgi için [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#dataset-properties) makalesi.

### <a name="sql-source-in-copy-activity"></a>Kopyalama etkinliğindeki SQL kaynağı
Bir Azure SQL veritabanı'ndan veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **SqlSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablo verilerini okuyan saklı yordamın adı. |Saklı yordamın adı. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

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
Daha fazla bilgi için [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#copy-activity-properties) makalesi.

### <a name="sql-sink-in-copy-activity"></a>Kopyalama etkinliğindeki SQL havuz
Azure SQL veritabanı'na veri kopyalama verilirse **Havuz türü** , kopyalama etkinliğine **SqlSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |Zaman aralığı<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin veri Temizlenen şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Bir sorgu deyimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilimin verileri temizlemek için kullanılan otomatik dilim tanımlayıcısı ile doldurmak için bir sütun adı belirtin. |Bir sütunun veri türüyle binary(32) sütun adı. |Hayır |
| sqlWriterStoredProcedureName |Saklı yordamın adını bu upsert eder (güncelleştirmeleri/eklemeleri) verileri hedef tabloya. |Saklı yordamın adı. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle kopyalanan verileri birleştirebilirsiniz. |Bir tablo türü adı. |Hayır |

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

Daha fazla bilgi için [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#copy-activity-properties) makalesi.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Azure SQL veri ambarı tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureSqlDW**, aşağıdaki özellikleri belirtin **typeProperties** Bölüm:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| bağlantı dizesi |ConnectionString özelliği için Azure SQL veri ambarı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |



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

Daha fazla bilgi için [Azure SQL veri ambarı Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure SQL veri ambarı veri kümesi tanımlamak için **türü** için veri kümesinin **AzureSqlDWTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL veri ambarı veritabanı adı. |Evet |

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

Daha fazla bilgi için [Azure SQL veri ambarı Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) makalesi.

### <a name="sql-dw-source-in-copy-activity"></a>Kopyalama etkinliğindeki SQL DW kaynağı
Azure SQL veri ambarından veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **SqlDWSource**, aşağıdaki özellikleri belirtin **kaynak** Bölüm:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablo verilerini okuyan saklı yordamın adı. |Saklı yordamın adı. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

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

Daha fazla bilgi için [Azure SQL veri ambarı Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) makalesi.

### <a name="sql-dw-sink-in-copy-activity"></a>Kopyalama etkinliğindeki SQL DW havuz
Azure SQL veri ambarı'na veri kopyalama verilirse **Havuz türü** , kopyalama etkinliğine **SqlDWSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Belirli bir dilimin veri Temizlenen şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Bir sorgu deyimi. |Hayır |
| Bulunan'allowpolybase |PolyBase, (uygunsa) yerine BULKINSERT mekanizması kullanılıp kullanılmayacağını belirtir. <br/><br/> **PolyBase kullanarak SQL Data Warehouse'a veri yükleme için önerilen yoldur.** |True <br/>False (varsayılan) |Hayır |
| polyBaseSettings |Bir grup olabilir özellik belirtilen **Bulunan'allowpolybase** özelliği **true**. |&nbsp; |Hayır |
| rejectValue |Sayı veya sorgu başarısız olmadan önce reddedilemiyor satırları yüzdesini belirtir. <br/><br/>PolyBase'nın içinde reddetme seçeneklerini hakkında daha fazla bilgi **bağımsız değişkenleri** bölümünü [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) konu. |0 (varsayılan), 1, 2... |Hayır |
| rejectType |RejectValue seçeneği değişmez değer veya bir yüzdesi olarak belirtilen belirtir. |Değer (varsayılan), yüzde |Hayır |
| rejectSampleValue |Reddedilen satırların yüzdesi PolyBase yeniden hesaplar önce almak için satır sayısını belirler. |1, 2, … |Evet, varsa **rejectType** olduğu **yüzdesi** |
| useTypeDefault |PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünden bu özellik hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |TRUE, False (varsayılan) |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |Zaman aralığı<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |

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

Daha fazla bilgi için [Azure SQL veri ambarı Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) makalesi.

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Azure Search tanımlamak için Ayarla **türü** bağlı hizmetinin **Azure Search**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

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

Daha fazla bilgi için [Azure Search bağlayıcı](data-factory-azure-search-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure Search veri kümesini tanımlamak için **türü** için veri kümesinin **AzureSearchIndex**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Type özelliği ayarlanmalıdır **AzureSearchIndex**.| Evet |
| indexName | Azure Search dizininin adı. Veri fabrikası, Dizin oluşturulmaz. Azure Search'te dizin varolmalıdır. | Evet |

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

Daha fazla bilgi için [Azure Search bağlayıcı](data-factory-azure-search-connector.md#dataset-properties) makalesi.

### <a name="azure-search-index-sink-in-copy-activity"></a>Kopyalama etkinliği, Azure arama dizini havuz
Bir Azure Search dizinine veri kopyalıyorsanız ayarlamak **Havuz türü** , kopyalama etkinliğine **AzureSearchIndexSink**, aşağıdaki özellikleri belirtin **havuz** Bölüm:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Bir belge dizinde zaten mevcut olduğunda değiştirin ya da birleştirme belirtir. | (Varsayılan) birleştirme<br/>Karşıya Yükle| Hayır |
| WriteBatchSize | Arabellek boyutu writeBatchSize ulaştığında, verileri Azure Search dizinine yükler. | 1 ila 1.000. Varsayılan değer 1000'dir. | Hayır |

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

Daha fazla bilgi için [Azure Search bağlayıcı](data-factory-azure-search-connector.md#copy-activity-properties) makalesi.

## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmetler, iki tür vardır: Azure depolama bağlı hizmeti ve Azure depolama SAS bağlı hizmeti.

#### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
Kullanarak, Azure depolama hesabınızı veri fabrikasına bağlamak için **hesap anahtarı**, bir Azure depolama bağlı hizmeti oluşturma. Bağlı hizmeti bir Azure depolama tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureStorage**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorage** |Evet |
| bağlantı dizesi |ConnectionString özelliği için Azure depolamaya bağlanmak için gereken bilgileri belirtin. |Evet |

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

#### <a name="azure-storage-sas-linked-service"></a>Azure depolama SAS bağlı hizmeti
Azure depolama SAS bağlı hizmet, bir paylaşılan erişim imzası (SAS) kullanarak bir Azure data factory'de bir Azure depolama hesabı bağlantı sağlar. Data factory ile kısıtlı/zamana bağlı depolama (blob/kapsayıcı) tüm/özel kaynaklarına erişimi sağlar. Paylaşılan erişim imzası kullanarak Azure depolama hesabınızı veri fabrikasına bağlamak için bir Azure depolama SAS bağlı hizmet oluşturun. Bağlı hizmeti Azure depolama SAS tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureStorageSas**. Ardından, aşağıdaki özellikleri belirtebilirsiniz **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **AzureStorageSas** |Evet |
| sasUri |Blob, kapsayıcı veya tablo gibi Azure Storage kaynakları için paylaşılan erişim imzası URI'si belirtin. |Evet |

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

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz. [Azure tablo depolama Bağlayıcısı](data-factory-azure-table-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Azure tablosu veri kümesi tanımlamak için **türü** için veri kümesinin **AzureTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti Azure tablo veritabanı örneğinde tablonun adını gösterir. |Evet. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Sorguyu karşılayan bir tablodaki kayıtları bir azureTableSourceQuery de belirtilirse, hedefe kopyalanamadı. |

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

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz. [Azure tablo depolama Bağlayıcısı](data-factory-azure-table-connector.md#dataset-properties) makalesi.

### <a name="azure-table-source-in-copy-activity"></a>Kopyalama etkinliği, Azure tablo kaynağı
Azure tablo Depolama'dan veri kopyalıyorsanız ayarlamak **kaynak türünü** , kopyalama etkinliğine **AzureTableSource**, aşağıdaki özellikleri belirtin **kaynak** Bölüm:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableSourceQuery |Verileri okumak için özel sorgu kullanın. |Azure tablo sorgu dizesi. Sonraki bölümdeki örneklere bakın. |Hayır. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Sorguyu karşılayan bir tablodaki kayıtları bir azureTableSourceQuery de belirtilirse, hedefe kopyalanamadı. |
| azureTableSourceIgnoreTableNotFound |Swallow özel durum tablosunun mevcut olup olmadığını gösterir. |TRUE<br/>FALSE |Hayır |

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

Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz. [Azure tablo depolama Bağlayıcısı](data-factory-azure-table-connector.md#copy-activity-properties) makalesi.

### <a name="azure-table-sink-in-copy-activity"></a>Kopyalama etkinliğindeki havuz Azure tablosu
Azure tablo depolama alanına veri kopyalama verilirse **Havuz türü** için kopyalama etkinliği, **AzureTableSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Havuz tarafından kullanılan varsayılan bölüm anahtarı değeri. |Bir dize değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, AzureTableDefaultPartitionKeyValue bölüm anahtarı olarak kullanılır. |Sütun adı. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Sütun adı. |Hayır |
| azureTableInsertType |Azure tabloya veri eklemek için modu.<br/><br/>Bu özellik, var olan satır bölüm ve satır anahtarları eşleşen çıktı tablosu değiştirildi veya birleştirilmiş değerlerine sahip olup olmadığını denetler. <br/><br/>Bu ayarları (birleştirme ve Değiştir) nasıl çalıştığı hakkında bilgi edinmek için bkz. [Ekle ya da birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [Ekle veya Değiştir varlık](https://msdn.microsoft.com/library/azure/hh452242.aspx) konuları. <br/><br> Bu ayar, tablo düzeyinde değil satır düzeyinde uygulanır ve her iki seçeneği giriş bulunmayan çıkış tablosundaki satırları siler. |(Varsayılan) birleştirme<br/>Değiştir |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout isabet edildiğinde verileri Azure tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout isabet edildiğinde verileri Azure tablosuna ekler. |Zaman aralığı<br/><br/>Örnek: "00: 20:00" (20 dakika) |Hayır (depolama istemci varsayılan zaman aşımı süresi için varsayılan değer 90 saniye) |

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
Bu bağlantılı hizmetler hakkında daha fazla bilgi için bkz. [Azure tablo depolama Bağlayıcısı](data-factory-azure-table-connector.md#copy-activity-properties) makalesi.

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Bağlı hizmet
Bir Amazon Redshift tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **AmazonRedshift**, aşağıdaki özellikleri belirtin **typeProperties** bölümü :

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |IP adresi veya ana bilgisayar adı Amazon Redshift sunucusunun. |Evet |
| port |Amazon Redshift sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası sayısı. |Hayır, varsayılan değer: 5439 |
| veritabanı |Amazon Redshift veritabanının adı. |Evet |
| kullanıcı adı |Veritabanına erişimi olan kullanıcı adı. |Evet |
| password |Kullanıcı hesabı için parola. |Evet |

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

Daha fazla bilgi için Amazon Redshift Bağlayıcısı makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir Amazon Redshift veri kümesi tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Amazon Redshift veritabanındaki bağlı hizmet başvurduğu tablonun adı. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |


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
Daha fazla bilgi için Amazon Redshift Bağlayıcısı makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Verileri Amazon Redshift'ten kopyalama verilirse **kaynak türü** için kopyalama etkinliği, **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

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
Daha fazla bilgi için Amazon Redshift Bağlayıcısı makalesine bakın.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Bağlı hizmet
Bir IBM DB2 tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesDB2**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |DB2 sunucusunun adı. |Evet |
| veritabanı |DB2 veritabanı adı. |Evet |
| Şema |Veritabanı şemasının adı. Şema adı büyük/küçük harfe duyarlıdır. |Hayır |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi DB2 veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

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
Daha fazla bilgi için IBM DB2 Bağlayıcısı makalesine bakın.

### <a name="dataset"></a>Veri kümesi
DB2 veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti DB2 veritabanı örneğinde tablonun adını gösterir. TableName, büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir)

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

Daha fazla bilgi için IBM DB2 Bağlayıcısı makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
IBM DB2'den veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `"query": "select * from "MySchema"."MyTable""`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

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
Daha fazla bilgi için IBM DB2 Bağlayıcısı makalesine bakın.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Bağlı hizmet
Bir MySQL tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesMySql**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |MySQL sunucusunun adı. |Evet |
| veritabanı |MySQL veritabanının adı. |Evet |
| Şema |Veritabanı şemasının adı. |Hayır |
| authenticationType |MySQL veritabanına bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: `Basic`. |Evet |
| kullanıcı adı |MySQL veritabanına bağlanmak için kullanıcı adı belirtin. |Evet |
| password |Belirtilen kullanıcı hesabı için parola belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi MySQL veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

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

Daha fazla bilgi için [MySQL bağlayıcısını](data-factory-onprem-mysql-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir MySQL veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti MySQL veritabanı örneğinde tablonun adını gösterir. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

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
Daha fazla bilgi için [MySQL bağlayıcısını](data-factory-onprem-mysql-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Bir MySQL veritabanından veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |


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

Daha fazla bilgi için [MySQL bağlayıcısını](data-factory-onprem-mysql-connector.md#copy-activity-properties) makalesi.

## <a name="oracle"></a>Oracle

### <a name="linked-service"></a>Bağlı hizmet
Oracle tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesOracle**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| driverType | / Oracle veritabanına veri kopyalamak için kullanılacak sürücüyü belirtin. İzin verilen değerler **Microsoft** veya **ODP** (varsayılan). Sürücü ayrıntıları üzerinde desteklenen sürümü ve yükleme bölümüne bakın. | Hayır |
| bağlantı dizesi | ConnectionString özelliği için Oracle veritabanı örneğine bağlanmak için gereken bilgileri belirtin. | Evet |
| gatewayName | Şirket içi Oracle sunucusuna bağlanmak için kullanılan ağ geçidinin adı |Evet |

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

Daha fazla bilgi için [Oracle Bağlayıcısı](data-factory-onprem-oracle-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Oracle veri kümesini tanımlamak için **türü** için veri kümesinin **OracleTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetini ifade eder Oracle veritabanı tablosunun adı. |Hayır (varsa **oracleReaderQuery** , **OracleSource** belirtilir) |

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
Daha fazla bilgi için [Oracle Bağlayıcısı](data-factory-onprem-oracle-connector.md#dataset-properties) makalesi.

### <a name="oracle-source-in-copy-activity"></a>Kopya etkinlikteki kaynak Oracle
Bir Oracle veritabanından veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **OracleSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin, `select * from MyTable` <br/><br/>Belirtilmezse, yürütülen SQL deyimi: `select * from MyTable` |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

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

Daha fazla bilgi için [Oracle Bağlayıcısı](data-factory-onprem-oracle-connector.md#copy-activity-properties) makalesi.

### <a name="oracle-sink-in-copy-activity"></a>Kopyalama etkinliğindeki havuz Oracle
' Te Oracle veritabanına veri kopyalama verilirse **Havuz türü** için kopyalama etkinliği, **OracleSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |Zaman aralığı<br/><br/> Örnek: 00:30:00 (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Belirli bir dilimin veri Temizlenen şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Bir sorgu deyimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilimin verileri temizlemek için kullanılan otomatik dilim tanımlayıcısı ile doldurmak için sütun adı belirtin. |Bir sütunun veri türüyle binary(32) sütun adı. |Hayır |

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
Daha fazla bilgi için [Oracle Bağlayıcısı](data-factory-onprem-oracle-connector.md#copy-activity-properties) makalesi.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Bağlı hizmet
Bir PostgreSQL tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesPostgreSql**, aşağıdaki özellikleri belirtin **typeProperties** Bölüm:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |PostgreSQL sunucusu adı. |Evet |
| veritabanı |PostgreSQL veritabanı adı. |Evet |
| Şema |Veritabanı şemasının adı. Şema adı büyük/küçük harfe duyarlıdır. |Hayır |
| authenticationType |PostgreSQL veritabanı'na bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi PostgreSQL veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

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
Daha fazla bilgi için [PostgreSQL bağlayıcı](data-factory-onprem-postgresql-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir PostgreSQL veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti PostgreSQL veritabanı örneğinde tablonun adını gösterir. TableName, büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

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
Daha fazla bilgi için [PostgreSQL bağlayıcı](data-factory-onprem-postgresql-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Bir PostgreSQL veritabanından veri kopyalıyorsanız ayarlamak **kaynak türünü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** Bölüm:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: "query": "seçin * öğesinden \"MySchema\".\" MyTable\"". |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

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

Daha fazla bilgi için [PostgreSQL bağlayıcı](data-factory-onprem-postgresql-connector.md#copy-activity-properties) makalesi.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Bağlı hizmet
SAP Business Warehouse (BW) tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **SapBw**, aşağıdaki özellikleri belirtin **typeProperties** bölümü :

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP BW örneği yer aldığı sunucunun adı. | string | Evet
systemNumber | SAP BW sisteminin sistem numarası. | İki basamaklı ondalık sayı bir dize olarak temsil edilir. | Evet
ClientID | SAP W sisteminde istemcinin istemci kimliği. | Bir dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | string | Evet
password | Kullanıcının parolası. | string | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP BW örneğine bağlanmak için kullanması gereken ağ geçidi adı. | string | Evet
encryptedCredential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

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

Daha fazla bilgi için [SAP Business Warehouse Bağlayıcısı](data-factory-sap-business-warehouse-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
SAP BW veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**. SAP BW veri kümesi türü için desteklenen türe özgü özellikler yoktur **RelationalTable**.

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
Daha fazla bilgi için [SAP Business Warehouse Bağlayıcısı](data-factory-sap-business-warehouse-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
SAP Business Warehouse veri kopyalıyorsanız ayarlayın **kaynak türünü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** Bölüm:


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

Daha fazla bilgi için [SAP Business Warehouse Bağlayıcısı](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) makalesi.

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Bağlı hizmet
SAP HANA tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **SapHana**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP HANA örneği yer aldığı sunucunun adı. Sunucunuz özelleştirilmiş bir bağlantı noktası kullanıyorsa, belirtin `server:port`. | string | Evet
authenticationType | Kimlik doğrulaması türü. | dize. "Temel" veya "Windows" | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | string | Evet
password | Kullanıcının parolası. | string | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP HANA örneğine bağlanmak için kullanması gereken ağ geçidi adı. | string | Evet
encryptedCredential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

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
Daha fazla bilgi için [SAP HANA Bağlayıcısı](data-factory-sap-hana-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
SAP HANA veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**. SAP HANA veri kümesi türü için desteklenen türe özgü özellikler yoktur **RelationalTable**.

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
Daha fazla bilgi için [SAP HANA Bağlayıcısı](data-factory-sap-hana-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
SAP HANA veri deposundan veri kopyalamayı verilirse **kaynak türünü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** Bölüm:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP HANA örneği verileri okumak için SQL sorgusu belirtir. | SQL sorgusu. | Evet |


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

Daha fazla bilgi için [SAP HANA Bağlayıcısı](data-factory-sap-hana-connector.md#copy-activity-properties) makalesi.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmet türü oluşturma **OnPremisesSqlServer** bir şirket içi SQL Server veritabanına bir veri fabrikasına bağlamak için. Aşağıdaki tabloda, şirket içi SQL Server bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

Aşağıdaki tabloda, SQL Server bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| bağlantı dizesi |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak üzere gereken bağlantı dizesi bilgilerini belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |
| kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\username**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |

Kimlik bilgilerini kullanarak şifreleyebilirsiniz **yeni AzDataFactoryEncryptValue** cmdlet'i ve bunları aşağıdaki örnekte gösterildiği gibi bağlantı dizesini kullanın (**EncryptedCredential** özellik):

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
#### <a name="example-json-for-using-windows-authentication"></a>Örnek: Windows kimlik doğrulaması kullanmak için JSON

Kullanıcı adı ve parolası belirtilmişse, ağ geçidi bunları şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabının kimliğine bürün için kullanır. Aksi takdirde, ağ geçidi SQL Server Ağ Geçidi (kendi başlangıç hesabı) güvenlik bağlamı ile doğrudan bağlanır.

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

Daha fazla bilgi için [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir SQL Server veri kümesini tanımlamak için **türü** için veri kümesinin **SqlServerTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm bağlı hizmeti SQL Server veritabanı örneğindeki adını gösterir. |Evet |

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

Daha fazla bilgi için [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#dataset-properties) makalesi.

### <a name="sql-source-in-copy-activity"></a>Kopyalama etkinliğindeki SQL kaynağı
Bir SQL Server veritabanından veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **SqlSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. Birden çok tablo giriş veri kümesi tarafından başvurulan veritabanından başvurabilir. Belirtilmezse, yürütülen SQL deyimi: MyTable arasından seçin. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablo verilerini okuyan saklı yordamın adı. |Saklı yordamın adı. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği, verileri almak için SQL Server veritabanı kaynak karşı bu sorgu çalıştırır.

Alternatif olarak, bir saklı yordam belirterek belirtebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordamın parametreleri sürerse).

SqlReaderQuery ya da sqlReaderStoredProcedureName belirtmezseniz yapı bölümünde tanımlanan sütunları SQL Server veritabanında çalıştırmak için bir select sorgusu oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

> [!NOTE]
> Kullanırken **sqlReaderStoredProcedureName**, yine de için bir değer belirtmeniz gerekiyorsa **tableName** veri kümesi JSON özelliğinde. Ancak bu tabloya karşı gerçekleştirilen başka bir doğrulama vardır.


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

Bu örnekte, **sqlReaderQuery** SqlSource için belirtilir. Kopyalama etkinliği, verileri almak için SQL Server veritabanı kaynak karşı bu sorguyu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirtebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordamın parametreleri sürerse). Giriş veri kümesi tarafından başvurulan veritabanına birden fazla tablo sqlReaderQuery başvurabilirsiniz. Yalnızca veri kümesinin tableName typeProperty ayarlayın tabloya sınırlı değildir.

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz yapı bölümünde tanımlanan sütunları SQL Server veritabanında çalıştırmak için bir select sorgusu oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

Daha fazla bilgi için [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#copy-activity-properties) makalesi.

### <a name="sql-sink-in-copy-activity"></a>Kopyalama etkinliğindeki SQL havuz
Bir SQL Server veritabanına veri kopyalama verilirse **Havuz türü** , kopyalama etkinliğine **SqlSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |Zaman aralığı<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin veri Temizlenen şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. Daha fazla bilgi için Yinelenebilirlik bölümüne bakın. |Bir sorgu deyimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilimin verileri temizlemek için kullanılan otomatik dilim tanımlayıcısı ile doldurmak için sütun adı belirtin. Daha fazla bilgi için Yinelenebilirlik bölümüne bakın. |Bir sütunun veri türüyle binary(32) sütun adı. |Hayır |
| sqlWriterStoredProcedureName |Saklı yordamın adını bu upsert eder (güncelleştirmeleri/eklemeleri) verileri hedef tabloya. |Saklı yordamın adı. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordamda kullanılan tablo türü adı belirtin. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle kopyalanan verileri birleştirebilirsiniz. |Bir tablo türü adı. |Hayır |

#### <a name="example"></a>Örnek
İşlem hattı, bu girdi ve çıktı veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **BlobSource** ve **havuz** türü ayarlandığında **SqlSink**.

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

Daha fazla bilgi için [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#copy-activity-properties) makalesi.

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Bağlı hizmet
Bir Sybase tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesSybase**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Sybase sunucusunun adı. |Evet |
| veritabanı |Sybase veritabanı adı. |Evet |
| Şema |Veritabanı şemasının adı. |Hayır |
| authenticationType |Sybase veritabanına bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi Sybase veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

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

Daha fazla bilgi için [Sybase bağlayıcı](data-factory-onprem-sybase-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Sybase veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti Sybase veritabanı örneğinde tablonun adını gösterir. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

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

Daha fazla bilgi için [Sybase bağlayıcı](data-factory-onprem-sybase-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Bir Sybase veritabanından veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü :


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

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

Daha fazla bilgi için [Sybase bağlayıcı](data-factory-onprem-sybase-connector.md#copy-activity-properties) makalesi.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Bağlı hizmet
Bir Teradata tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesTeradata**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Teradata sunucusunun adı. |Evet |
| authenticationType |Teradata veritabanına bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi Teradata veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

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

Daha fazla bilgi için [Teradata bağlayıcı](data-factory-onprem-teradata-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Teradata Blob veri kümesi tanımlamak için **türü** için veri kümesinin **RelationalTable**. Şu anda Teradata veri kümesi için desteklenen hiçbir tür özellikleri vardır.

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

Daha fazla bilgi için [Teradata bağlayıcı](data-factory-onprem-teradata-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Bir Teradata veritabanından veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** Bölüm:

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

Daha fazla bilgi için [Teradata bağlayıcı](data-factory-onprem-teradata-connector.md#copy-activity-properties) makalesi.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Bağlı hizmet
Cassandra bağlı hizmetini tanımlamak için **türü** bağlı hizmetinin **OnPremisesCassandra**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| konak |Bir veya daha fazla IP adresleri veya Cassandra sunucusunun ana bilgisayar adını.<br/><br/>IP adreslerini veya aynı anda tüm sunuculara bağlanmak için ana bilgisayar adlarını virgülle ayrılmış listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. |Hayır, varsayılan değer: 9042 |
| authenticationType |Temel veya anonim |Evet |
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

Daha fazla bilgi için [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Cassandra veri kümesini tanımlamak için **türü** için veri kümesinin **CassandraTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| anahtar alanı |Anahtar alanı veya Cassandra veritabanındaki şema adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |
| tableName |Cassandra veritabanındaki tablonun adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |

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

Daha fazla bilgi için [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#dataset-properties) makalesi.

### <a name="cassandra-source-in-copy-activity"></a>Kopyalama etkinliği Cassandra kaynakta
Cassandra veri kopyalıyorsanız ayarlayın **kaynak türü** için kopyalama etkinliği, **CassandraSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |92 SQL sorgusu veya CQL sorgusu. Bkz: [CQL başvuru](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgu kullanarak belirtmeniz **anahtar alanı name.table adı** sorgulamak istediğiniz tablosunu temsil edecek. |Hayır (tableName ve veri kümesi üzerinde anahtar alanı tanımlanmışsa). |
| consistencyLevel |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce kaç çoğaltmalar için Okuma isteği yanıtlamalıdır belirtir. Cassandra Okuma isteği karşılamak veriler için çoğaltmaları belirtilen sayısını denetler. |BİR, İKİ, ÜÇ SANAL ÇEKİRDEK, TÜMÜ LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Bkz: [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) Ayrıntılar için. |Hayır. Varsayılan değer biridir. |

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

Daha fazla bilgi için [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#copy-activity-properties) makalesi.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir MongoDB tanımlamak için Ayarla **türü** bağlı hizmetinin **OnPremisesMongoDB**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |IP adresi veya ana bilgisayar adı MongoDB sunucusunun. |Evet |
| port |MongoDB sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. |İsteğe bağlı, varsayılan değer: 27017 |
| authenticationType |Temel veya anonim. |Evet |
| kullanıcı adı |MongoDB erişmek için kullanıcı hesabı'nı tıklatın. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| password |Kullanıcının parolası. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| authSource |Kimlik doğrulaması için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanının adı. |(Temel kimlik doğrulaması kullanılıyorsa) isteğe bağlı. Varsayılan: yönetici hesabı ve databaseName özelliği kullanılarak belirtilen veritabanı kullanır. |
| databaseName |Erişmek istediğiniz MongoDB veritabanının adı. |Evet |
| gatewayName |Veri deposu erişen bir ağ geçidi adı. |Evet |
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

Daha fazla bilgi için [MongoDB Bağlayıcısı makalesi](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Veri kümesi
MongoDB veri kümesini tanımlamak için **türü** için veri kümesinin **MongoDbCollection**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| collectionName |MongoDB veritabanındaki koleksiyonun adı. |Evet |

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

Daha fazla bilgi için [MongoDB Bağlayıcısı makalesi](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>Kopya etkinlikteki kaynak MongoDB
Mongodb'deki verileri kopyalama verilirse **kaynak türü** , kopyalama etkinliğine **MongoDbSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL 92 sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır (varsa **collectionName** , **veri kümesi** belirtilir) |

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

Daha fazla bilgi için [MongoDB Bağlayıcısı makalesi](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Bağlı hizmet
Amazon S3 tanımlamak için bağlı hizmeti, ayarlayın **türü** bağlı hizmetinin **AwsAccessKey**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| accessKeyID |Gizli erişim anahtarı kimliği. |string |Evet |
| secretAccessKey |Gizli erişim anahtarı kendisi. |Şifrelenmiş gizli dize |Evet |

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

Daha fazla bilgi için [Amazon S3 Bağlayıcısı makalesi](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Veri kümesi
Bir Amazon S3 veri kümesi tanımlamak için **türü** için veri kümesinin **AmazonS3**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 demetini adı. |String |Evet |
| anahtar |S3 nesnesinin anahtarı. |String |Hayır |
| Ön eki |S3 nesnesinin anahtarı için önek. Seçili bir nesne anahtarları bu öneki ile başlayın. Yalnızca anahtar boş olduğunda geçerlidir. |String |Hayır |
| version |S3 sürümü oluşturma etkinse, S3 nesnesinin sürümü. |String |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır | |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır | |


> [!NOTE]
> bucketName + anahtarı burada S3 nesneleri için kök kapsayıcı demet oluşturuyorum ve anahtar S3 nesnesinin tam yolunu S3 nesnesinin konumunu belirtir.

#### <a name="example-sample-dataset-with-prefix"></a>Örnek: Örnek veri kümesiyle ön eki

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

#### <a name="example-dynamic-paths-for-s3"></a>Örnek: S3 için dinamik yolları
Bu örnekte anahtar ve bucketName özelliklerinde Amazon S3 veri kümesi için sabit değerlerini kullanın.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

Data Factory SliceStart gibi sistem değişkenlerini kullanarak kayıt anahtarını ve bucketName zamanında dinamik olarak hesaplamak olabilir.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Bir Amazon S3 veri kümesi önek özelliği için aynısını yapabilir. Bkz: [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md) desteklenen işlevler ve değişkenler içeren bir liste için.

Daha fazla bilgi için [Amazon S3 Bağlayıcısı makalesi](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Kopyalama etkinliği dosya sistem kaynağı
Amazon S3'ten veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **FileSystemSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Yinelemeli olarak S3 listesinde olup olmadığını belirtir dizinin altındaki nesneleri. |true/false |Hayır |


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

Daha fazla bilgi için [Amazon S3 Bağlayıcısı makalesi](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Dosya Sistemi


### <a name="linked-service"></a>Bağlı hizmet
Bir Azure data factory ile bir şirket içi dosya sistemine bağlanabilirsiniz **şirket içi dosya sunucusu** bağlı hizmeti. Aşağıdaki tabloda, şirket içi dosya sunucusuna bağlı hizmete özgü JSON öğelerinin açıklamaları verilmiştir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlandığından emin olun **OnPremisesFileServer**. |Evet |
| konak |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Bağlı örnek hizmet ve veri kümesi tanımları örnekler için bkz. |Evet |
| Kullanıcı Kimliği |Sunucu erişimi olan kullanıcının kimliği belirtin. |Hayır (encryptedCredential seçerseniz) |
| password |(Kullanıcı kimliği) kullanıcının parolasını belirtin. |Hayır (encryptedCredential seçin |
| encryptedCredential |New-AzDataFactoryEncryptValue cmdlet çalıştırılarak elde edebilirsiniz şifrelenmiş kimlik bilgilerini belirtin. |Hayır (kullanıcı kimliği ve parola düz metin olarak belirtmek isterseniz varsa) |
| gatewayName |Data Factory şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidi adını belirtir. |Evet |

#### <a name="sample-folder-path-definitions"></a>Örnek klasör yolu tanımları

| Senaryo | Bağlı hizmet tanımında barındırın | veri kümesi tanımında folderPath |
| --- | --- | --- |
| Veri Yönetimi ağ geçidi makinesinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |D:\\ \\ (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/> localhost (daha önceki sürümler için veri yönetimi ağ geçidi 2.0) |. \\ \\ veya klasör\\\\alt klasör (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/>D:\\ \\ veya D:\\\\klasör\\\\alt klasör (için ağ geçidi sürüm 2.0 altında) |
| Paylaşılan uzak klasör: <br/><br/>Örnekler: \\ \\myserver\\paylaşmak\\ \* veya \\ \\myserver\\paylaşmak\\klasör\\alt\\* |\\\\\\\\myserver\\\\paylaşın |. \\ \\ veya klasör\\\\alt |


#### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: Kullanıcı adı ve parola düz metin olarak kullanma

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

#### <a name="example-using-encryptedcredential"></a>Örnek: Encryptedcredential kullanma

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

Daha fazla bilgi için [dosya sistemi Bağlayıcısı makalesi](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Veri kümesi
Bir dosya sistemi veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Yükleme kökü klasörüne belirtir. Çıkış karakterini kullanma ' \' dizesinde özel karakterler için. Bağlı örnek hizmet ve veri kümesi tanımları örnekler için bkz.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün yol tabanlı slice başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>FileName için bir çıktı veri kümesi belirtilmediğinde, oluşturulan dosyanın adı şu biçimdedir: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: "fileFilter": "* .log"<br/>Örnek 2: "fileFilter": 2016-1-?.txt"<br/><br/>Bu fileFilter girdi FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| partitionedBy |PartitionedBy dinamik bir folderPath/fileName için zaman serisi verilerini belirtmek için kullanabilirsiniz. FolderPath için verileri saatte parametreli bir örnektir. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**; ve desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

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

Daha fazla bilgi için [dosya sistemi Bağlayıcısı makalesi](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Kopyalama etkinliği dosya sistem kaynağı
Dosya sisteminden veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **FileSystemSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |TRUE, False (varsayılan) |Hayır |

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
Daha fazla bilgi için [dosya sistemi Bağlayıcısı makalesi](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>Dosya sistemi kopyalama etkinliğindeki havuz
Dosya sistemi veri kopyalıyorsanız ayarlayın **Havuz türü** , kopyalama etkinliğine **FileSystemSink**, aşağıdaki özellikleri belirtin **havuz** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |**PreserveHierarchy:** Hedef klasördeki ise dosya hiyerarşisini korur. Diğer bir deyişle, kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><br/>**FlattenHierarchy:** Tüm dosyaları kaynak klasörden hedef klasöre ilk düzeyde yer oluşturulur. Hedef dosyalar bir otomatik olarak oluşturulan adıyla oluşturulur.<br/><br/>**MergeFiles:** Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı/blob adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. |Hayır |

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

Daha fazla bilgi için [dosya sistemi Bağlayıcısı makalesi](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Bağlı hizmet
FTP tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **Ftp_sunucusu**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| konak |FTP sunucusunun adı veya IP adresi |Evet |&nbsp; |
| authenticationType |Kimlik doğrulaması türünü belirtme |Evet |Temel, anonim |
| kullanıcı adı |FTP sunucusuna erişimi olan kullanıcı |Hayır |&nbsp; |
| password |(Kullanıcı adı) kullanıcının parolasını |Hayır |&nbsp; |
| encryptedCredential |FTP sunucusuna erişmek için şifrelenmiş kimlik bilgileri |Hayır |&nbsp; |
| gatewayName |Şirket içi FTP sunucusuna bağlanmak için veri yönetimi ağ geçidi adı |Hayır |&nbsp; |
| port |FTP sunucusunun dinlediği bağlantı noktası |Hayır |21 |
| enableSsl |FTP SSL/TLS kanalı üzerinden kullanıp kullanmayacağınızı belirtin |Hayır |true |
| enableServerCertificateValidation |FTP üzerinden SSL/TLS kanal kullanırken sunucu SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğinizi belirtin |Hayır |true |

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulaması

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

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Örnek: Kullanıcı adı ve parola düz metin olarak temel kimlik doğrulaması için kullanma

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

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Örnek: Bağlantı noktası, enableSsl enableServerCertificateValidation kullanma

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

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Örnek: Kimlik doğrulaması ve ağ geçidi için encryptedCredential kullanma

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

Daha fazla bilgi için [FTP Bağlayıcısı](data-factory-ftp-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir FTP veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasörünün yolu. Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Bağlı örnek hizmet ve veri kümesi tanımları örnekler için bkz.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün yol tabanlı slice başlangıç/bitiş tarih saatleri. |Evet
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>Oluşturulan dosyanın adını bir çıktı veri kümesi için dosya adı belirtilmediği durumlarda, aşağıdaki olacaktır bu biçimi: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>1. örnekler: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter girdi FileShare veri kümesi için geçerlidir. Bu özellik, HDFS ile desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örneğin, verilerin her saat için parametreli folderPath. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**; ve desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: TRUE. Bu özellik, yalnızca ilişkili bağlantılı hizmet türü türü olduğunda kullanılabilir: FtpServer. |Hayır |

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

Daha fazla bilgi için [FTP Bağlayıcısı](data-factory-ftp-connector.md#dataset-properties) makalesi.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama etkinliği dosya sistem kaynağı
Bir FTP sunucusundan veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **FileSystemSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |TRUE, False (varsayılan) |Hayır |

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

Daha fazla bilgi için [FTP Bağlayıcısı](data-factory-ftp-connector.md#copy-activity-properties) makalesi.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Bağlı hizmet
Bir HDFS tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **Hdfs**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Hdfs** |Evet |
| Url |HDFS URL'si |Evet |
| authenticationType |Anonim veya Windows. <br><br> Kullanılacak **Kerberos kimlik doğrulaması** için HDFS Bağlayıcısı, şirket içi ortamınızı uygun şekilde ayarlamak için bu bölüme bakın. |Evet |
| Kullanıcı adı |Kullanıcı adı için Windows kimlik doğrulaması. |Evet (Windows kimlik doğrulaması için) |
| password |Windows kimlik doğrulaması için parola. |Evet (Windows kimlik doğrulaması için) |
| gatewayName |Data Factory hizmetinin HDFS'ye bağlanmak için kullanması gereken ağ geçidi adı. |Evet |
| encryptedCredential |[Yeni AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) erişim kimlik bilgisi çıktısı. |Hayır |

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulaması

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

Daha fazla bilgi için HDFS bağlayıcı makalesine bakın.

### <a name="dataset"></a>Veri kümesi
HDFS veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasör yolu. Örnek: `myfolder`<br/><br/>Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Örneğin: folder\subfolder için klasörü belirtin\\\\alt d:\samplefolder için d: belirtin\\\\ÖrnekKlasör.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün yol tabanlı slice başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>Oluşturulan dosyanın adını bir çıktı veri kümesi için dosya adı belirtilmediği durumlarda, aşağıdaki olacaktır bu biçimi: <br/><br/>`Data.<Guid>.txt` (örneğin:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örnek: veri her saat için parametreli folderPath. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

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

Daha fazla bilgi için HDFS bağlayıcı makalesine bakın.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama etkinliği dosya sistem kaynağı
Verileri HDFS Kopyalamakta olduğunuz verilirse **kaynak türü** için kopyalama etkinliği, **FileSystemSource**ve aşağıdaki özellikleri belirtin **kaynak** bölümü:

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |TRUE, False (varsayılan) |Hayır |

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

Daha fazla bilgi için HDFS bağlayıcı makalesine bakın.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir SFTP tanımlamak için Ayarla **türü** bağlı hizmetinin **Sftp**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| konak | SFTP sunucusunun adı veya IP adresi. |Evet |
| port |SFTP sunucusunun dinlediği bağlantı noktası. Varsayılan değerdir: 21 |Hayır |
| authenticationType |Kimlik doğrulama türünü belirtin. İzin verilen değerler: **Temel**, **SshPublicKey**. <br><br> Kullanarak temel kimlik doğrulaması için bakın ve [kullanarak SSH ortak anahtarı kimlik doğrulaması](#using-ssh-public-key-authentication) daha fazla özellik ve JSON örneklerini sırasıyla bölümler. |Evet |
| skipHostKeyValidation | Konak anahtar doğrulamasını atlayın verilip verilmeyeceğini belirtin. | Hayır. Varsayılan değer: false |
| hostKeyFingerprint | Konak anahtarı parmak izi belirtin. | Yanıt Evet ise `skipHostKeyValidation` false olarak ayarlanır.  |
| gatewayName |Şirket içi SFTP sunucusuna bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi SFTP sunucusundan veri kopyalama, Evet. |
| encryptedCredential | SFTP sunucusuna erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan temel kimlik doğrulaması (kullanıcı adı ve parola) ya da SshPublicKey kimlik doğrulaması (kullanıcı adı ve özel anahtar yolu veya içerik) Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim kutusunda belirttiğiniz zaman. | Hayır. Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

#### <a name="example-using-basic-authentication"></a>Örnek: Temel kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak için ayarlanmış `authenticationType` olarak `Basic`ve SFTP Bağlayıcısı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | (Kullanıcı adı) kullanıcı parolası. | Evet |

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

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Örnek: **Şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması**

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

#### <a name="using-ssh-public-key-authentication"></a>**SSH ortak anahtar kimlik doğrulamasını kullanma:**

Temel kimlik doğrulaması kullanmak için ayarlanmış `authenticationType` olarak `SshPublicKey`ve SFTP Bağlayıcısı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı |SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Belirtin özel anahtar dosyasının mutlak yolu, ağ geçidinin erişebilir. | Seçeneklerinden birini belirtin `privateKeyPath` veya `privateKeyContent`. <br><br> Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |
| privateKeyContent | Özel anahtar içeriği seri hale getirilmiş bir dize. Kopyalama Sihirbazı'nı, özel anahtar dosyasını okuma ve özel anahtar içeriği otomatik olarak ayıklayın. Herhangi diğer araca/SDK'ya kullanıyorsanız privateKeyPath özelliğini kullanın. | Seçeneklerinden birini belirtin `privateKeyPath` veya `privateKeyContent`. |
| Parola | Geçişi tümcecik/anahtar dosyası bir parola deyimi tarafından korunuyorsa, özel anahtarın şifresini çözmek için parola belirtin. | Özel anahtar dosyasını bir parola deyimi tarafından korunuyorsa, Evet. |

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Örnek: **Özel anahtar içeriğini kullanarak SshPublicKey kimlik doğrulaması**

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

Daha fazla bilgi için [SFTP Bağlayıcısı](data-factory-sftp-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir SFTP veri kümesini tanımlamak için **türü** için veri kümesinin **FileShare**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasörünün yolu. Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Bağlı örnek hizmet ve veri kümesi tanımları örnekler için bkz.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün yol tabanlı slice başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>Oluşturulan dosyanın adını bir çıktı veri kümesi için dosya adı belirtilmediği durumlarda, aşağıdaki olacaktır bu biçimi: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>1. örnekler: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter girdi FileShare veri kümesi için geçerlidir. Bu özellik, HDFS ile desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örneğin, verilerin her saat için parametreli folderPath. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: TRUE. Bu özellik, yalnızca ilişkili bağlantılı hizmet türü türü olduğunda kullanılabilir: FtpServer. |Hayır |

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

Daha fazla bilgi için [SFTP Bağlayıcısı](data-factory-sftp-connector.md#dataset-properties) makalesi.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama etkinliği dosya sistem kaynağı
Bir SFTP kaynaktan veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **FileSystemSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |TRUE, False (varsayılan) |Hayır |



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

Daha fazla bilgi için [SFTP Bağlayıcısı](data-factory-sftp-connector.md#copy-activity-properties) makalesi.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir HTTP tanımlamak için Ayarla **türü** bağlı hizmetinin **Http**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| url | Web sunucusu için temel URL | Evet |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler şunlardır: **Anonim**, **temel**, **Özet**, **Windows**, **ClientCertificate**. <br><br> Daha fazla özellik ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlere sırasıyla bakın. | Evet |
| enableServerCertificateValidation | Kaynak HTTPS Web sunucusu ise sunucu SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğinizi belirtin | Hayır, varsayılan değer true şeklindedir |
| gatewayName | Bir şirket içi HTTP kaynağına bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi HTTP kaynaktan veri kopyalama, Evet. |
| encryptedCredential | HTTP uç noktasına erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim kimlik doğrulama bilgilerini yapılandırın. | Hayır. Yalnızca bir şirket içi HTTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Örnek: Temel, Özet veya Windows kimlik doğrulamasını kullanma
Ayarlama `authenticationType` olarak `Basic`, `Digest`, veya `Windows`ve HTTP Bağlayıcısı yukarıda tanıtılan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | HTTP uç noktasına erişmek için kullanıcı adı. | Evet |
| password | (Kullanıcı adı) kullanıcı parolası. | Evet |

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

Temel kimlik doğrulaması kullanmak için ayarlanmış `authenticationType` olarak `ClientCertificate`ve HTTP Bağlayıcısı yukarıda tanıtılan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| embeddedCertData | Kişisel bilgi değişimi (PFX) dosyasının ikili verileri Base64 ile kodlanmış içeriği. | Seçeneklerinden birini belirtin `embeddedCertData` veya `certThumbprint`. |
| Certthumbprınt | Ağ geçidi makinenizin sertifika deposunda yüklü sertifika parmak izi. Yalnızca bir şirket içi HTTP kaynaktan veri kopyalama işlemi sırasında uygulanır. | Seçeneklerinden birini belirtin `embeddedCertData` veya `certThumbprint`. |
| password | Sertifikayla ilişkili parola. | Hayır |

Kullanırsanız `certThumbprint` yerel bilgisayarın kişisel depoda kimlik doğrulaması ve sertifika yüklü için ağ geçidi hizmeti için okuma izni vermeniz gerekir:

1. Microsoft Yönetim Konsolu (MMC) başlatın. Ekleme **sertifikaları** hedefleyen eklentisini **yerel bilgisayar**.
2. Genişletin **sertifikaları**, **kişisel**, tıklatıp **sertifikaları**.
3. Kişisel deposundan sertifikayı sağ tıklatın ve seçin **tüm görevler**->**özel anahtarları Yönet...**
3. Üzerinde **güvenlik** sekmesinde, altında veri yönetimi ağ geçidi ana bilgisayar hizmetinin çalıştığı okuma erişimi ile sertifikayı kullanıcı hesabı ekleyin.

**Örnek: istemci sertifikası kullanarak:** Bu veri fabrikanıza şirket içi HTTP web sunucusuna bağlı hizmeti. Veri Yönetimi ağ ile geçidi yüklü olduğu makinede yüklü olan bir istemci sertifikası kullanır.

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
Bu veri fabrikanıza şirket içi HTTP web sunucusuna bağlı hizmeti. Veri Yönetimi ağ ile geçidi yüklü olduğu makinedeki bir istemci sertifikası dosyası kullanır.

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

Daha fazla bilgi için [HTTP Bağlayıcısı](data-factory-http-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
HTTP veri kümesi tanımlamak için **türü** için veri kümesinin **Http**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Yalnızca bağlı hizmet tanımında belirtilen URL yolu belirtilmemiş olduğunda kullanılır. <br><br> Dinamik URL oluşturmak için kullanabileceğiniz [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md), örnek: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **alma** veya **POST**. | Hayır. `GET` varsayılan değerdir. |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| Includesearchresults: true | HTTP istek gövdesi. | Hayır |
| biçim | Yalnızca isterseniz **HTTP uç noktasından veri alma-olan** ayrıştırma olmadan, bu biçimi ayarları atlayın. <br><br> Kopyalama sırasında HTTP yanıt içeriği ayrıştırılamıyor istiyorsanız, şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

#### <a name="example-using-the-get-default-method"></a>Örnek: (varsayılan) alma yöntemini kullanma

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
Daha fazla bilgi için [HTTP Bağlayıcısı](data-factory-http-connector.md#dataset-properties) makalesi.

### <a name="http-source-in-copy-activity"></a>Kopyalama etkinliğindeki HTTP kaynağı
Bir HTTP kaynaktan veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **HttpSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| httpRequestTimeout | Zaman aşımı süresi (zaman) HTTP isteği bir yanıt alın. Yanıt verileri okumak için zaman aşımını değil bir yanıt almak için zaman aşımı olan. | Hayır. Varsayılan değer: 00:01:40 |


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

Daha fazla bilgi için [HTTP Bağlayıcısı](data-factory-http-connector.md#copy-activity-properties) makalesi.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Bağlı hizmet
Bir OData tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OData**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| url |OData hizmeti URL'si. |Evet |
| authenticationType |OData kaynağına bağlanmak için kullanılan kimlik doğrulaması türü. <br/><br/> Bulut OData anonim, temel ve OAuth (Not Azure Active Directory tabanlı OAuth Azure Data Factory şu anda yalnızca destek) olası değerler şunlardır. <br/><br/> Anonim, temel ve Windows, şirket içi OData için olası değerler şunlardır. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Evet (yalnızca temel kimlik doğrulaması kullanıyorsanız) |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Evet (yalnızca temel kimlik doğrulaması kullanıyorsanız) |
| authorizedCredential |OAuth kullanıyorsanız **Authorize** Data Factory Kopyalama Sihirbazı'nı veya düzenleyicide düğmesine tıklayın ve sonra da bu özelliğin değeri otomatik olarak oluşturulan kimlik bilgilerinizi girin. |Evet (yalnızca OAuth kimlik doğrulaması kullanıyorsanız) |
| gatewayName |Data Factory hizmetinin şirket içi OData hizmetine bağlanmak için kullanması gereken ağ geçidi adı. Yalnızca şirket içi OData kaynağı üzerindeki veri kopyalama, belirtin. |Hayır |

#### <a name="example---using-basic-authentication"></a>Temel kimlik doğrulaması kullanan örnek-
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Örnek - anonim kimlik doğrulaması

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Örnek - kimlik doğrulaması kullanarak Windows erişme OData kaynağı şirket içinde

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

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Örnek - bulut OData kaynağına erişme OAuth kimlik doğrulaması kullanma
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

Daha fazla bilgi için [OData Bağlayıcısı](data-factory-odata-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir OData veri kümesini tanımlamak için **türü** için veri kümesinin **ODataResource**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| yol |OData kaynağı yolu |Hayır |

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

Daha fazla bilgi için [OData Bağlayıcısı](data-factory-odata-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Bir OData kaynaktan veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

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

Daha fazla bilgi için [OData Bağlayıcısı](data-factory-odata-connector.md#copy-activity-properties) makalesi.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Bağlı hizmet
Bir ODBC tanımlamak için bağlı hizmeti, Ayarla **türü** bağlı hizmetinin **OnPremisesOdbc**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| bağlantı dizesi |Bağlantı dizesini ve isteğe bağlı bir şifrelenmiş kimlik bilgileri olmayan erişim kimlik bilgileri bölümü. Aşağıdaki bölümlerde örneklere bakın. |Evet |
| kimlik bilgisi |Erişim kimlik bilgisi sürücüye özel özellik-değer biçiminde belirtilen bağlantı dizesi kısmı. Örnek: `“Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.` |Hayır |
| authenticationType |ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim ve temel. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin ODBC veri deposuna bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

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
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Şifrelenmiş kimlik bilgileriyle temel kimlik doğrulaması kullanan örnek-
Kimlik bilgilerini kullanarak şifreleyebilirsiniz [yeni AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) cmdlet'i.

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

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulaması

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

Daha fazla bilgi için [ODBC Bağlayıcısı](data-factory-odbc-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir ODBC veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |ODBC veri deposundaki tablosunun adı. |Evet |


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

Daha fazla bilgi için [ODBC Bağlayıcısı](data-factory-odbc-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Bir ODBC veri deposundan veri kopyalamayı verilirse **kaynak türünü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü :

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

Daha fazla bilgi için [ODBC Bağlayıcısı](data-factory-odbc-connector.md#copy-activity-properties) makalesi.

## <a name="salesforce"></a>Salesforce


### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Salesforce tanımlamak için Ayarla **türü** bağlı hizmetinin **Salesforce**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| environmentUrl | URL, Salesforce örneği belirtin. <br><br> -Varsayılan değer "https:\//login.salesforce.com". <br> Korumalı alan ' veri kopyalamak için belirtin "https://test.salesforce.com". <br> Özel etki alanından veri kopyalamak için örneğin, "https://[domain].my.salesforce.com" belirtin. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| securityToken |Kullanıcı hesabı için güvenlik belirtecini belirtin. Bkz: [güvenlik belirteci alın getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) bir güvenlik belirteci sıfırlama/alma konusunda yönergeler için. Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Evet |

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

Daha fazla bilgi için [Salesforce Bağlayıcısı](data-factory-salesforce-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Salesforce veri kümesini tanımlamak için **türü** için veri kümesinin **RelationalTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Salesforce'taki tablosunun adı. |Hayır (varsa bir **sorgu** , **RelationalSource** belirtilir) |

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

Daha fazla bilgi için [Salesforce Bağlayıcısı](data-factory-salesforce-connector.md#dataset-properties) makalesi.

### <a name="relational-source-in-copy-activity"></a>Kopyalama etkinliğindeki ilişkisel bir kaynak
Salesforce veri kopyalıyorsanız ayarlayın **kaynak türü** , kopyalama etkinliğine **RelationalSource**, aşağıdaki özellikleri belirtin **kaynak** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |Bir SQL 92 sorgu veya [Salesforce nesne sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgu. Örneğin:  `select * from MyTable__c`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

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
> API adı "__c" bölümü, herhangi özel bir nesne için gereklidir.

Daha fazla bilgi için [Salesforce Bağlayıcısı](data-factory-salesforce-connector.md#copy-activity-properties) makalesi.

## <a name="web-data"></a>Web veri

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Web tanımlamak için Ayarla **türü** bağlı hizmetinin **Web**, aşağıdaki özellikleri belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Url |Web kaynağına URL'si |Evet |
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

Daha fazla bilgi için [Web tablosu bağlayıcı](data-factory-web-table-connector.md#linked-service-properties) makalesi.

### <a name="dataset"></a>Veri kümesi
Bir Web veri kümesini tanımlamak için **türü** için veri kümesinin **WebTable**ve şu özelliklerde belirtin **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Veri kümesi türü. ayarlanmalıdır **WebTable** |Evet |
| yol |Tablo içeren bir kaynak için göreli bir URL. |Hayır. Yalnızca bağlı hizmet tanımında belirtilen URL yolu belirtilmemiş olduğunda kullanılır. |
| dizin |Kaynak tablodaki dizini. Get bkz bir HTML sayfasında bir tablo dizininin alma adımları için bir HTML sayfası bölümü tablosu dizini. |Evet |

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

Daha fazla bilgi için [Web tablosu bağlayıcı](data-factory-web-table-connector.md#dataset-properties) makalesi.

### <a name="web-source-in-copy-activity"></a>Kopyalama etkinliği Web kaynağında
Web tablodan veri kopyalıyorsanız ayarlamak **kaynak türünü** , kopyalama etkinliğine **WebSource**. Şu anda, kopyalama etkinliği kaynak olduğunda tür **WebSource**, hiçbir ek özellikler desteklenir.

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

Daha fazla bilgi için [Web tablosu bağlayıcı](data-factory-web-table-connector.md#copy-activity-properties) makalesi.

## <a name="compute-environments"></a>ORTAM İŞLEM
Aşağıdaki tabloda Data Factory ve bunlar üzerinde çalışabilecek dönüştürme etkinlikleri tarafından desteklenen işlem ortamlarının listeler. Bir veri fabrikasına bağlamak için bağlı hizmet için JSON şemalarının görmek ilgilendiğiniz işlem bağlantısına tıklayın.

| İşlem ortamı | Etkinlikler |
| --- | --- |
| [İsteğe bağlı HDInsight kümesi](#on-demand-azure-hdinsight-cluster) veya [kendi HDInsight kümenizi](#existing-azure-hdinsight-cluster) |[.NET özel etkinliği](#net-custom-activity), [Hive etkinliği](#hdinsight-hive-activity), [Pig etkinliği](#hdinsight-pig-activity), [MapReduce etkinliği](#hdinsight-mapreduce-activity), streaming etkinliği, Hadoop [Spark etkinliği](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[.NET özel etkinliği](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Machine Learning batch yürütme etkinliği](#machine-learning-batch-execution-activity), [Machine Learning kaynak güncelleştirme etkinliği](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics'i](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Azure SQL veritabanı](#azure-sql-database-1), [Azure SQL veri ambarı](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1) |[Saklı Yordam](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>İsteğe bağlı Azure HDInsight kümesi
Azure Data Factory hizmetinin, bir Windows/Linux tabanlı isteğe bağlı HDInsight kümesi verileri işlemek için otomatik olarak oluşturabilirsiniz. Küme, kümeyle ilişkili depolama hesabı (JSON özelliğinde linkedServiceName) ile aynı bölgede oluşturulur. Bu bağlı hizmeti üzerinde aşağıdaki dönüştürme etkinliklerini çalıştırabilirsiniz: [.NET özel etkinliği](#net-custom-activity), [Hive etkinliği](#hdinsight-hive-activity), [Pig etkinliği](#hdinsight-pig-activity), [MapReduce Etkinlik](#hdinsight-mapreduce-activity), streaming etkinliği, Hadoop [Spark etkinliği](#hdinsight-spark-activity).

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda, bir isteğe bağlı HDInsight bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır **Hdınsightondemand**. |Evet |
| clusterSize |Kümedeki çalışan/veri düğümü sayısı. HDInsight kümesi için bu özelliği belirtmeniz çalışan düğümleri sayısını 2 baş düğüm ile oluşturulur. 4 çekirdek, 4 çalışan düğümü küme 24 çekirdek sürer olan işler için standart_d3 boyutunu düğümlerdir (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*baş düğümleri için 4 = 8 çekirdek). Bkz: [oluşturma Linux tabanlı Hadoop kümeleri HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) işler için standart_d3 katmanı hakkında ayrıntılı bilgi için. |Evet |
| TimeToLive |İsteğe bağlı HDInsight kümesi için izin verilen boşta kalma süresi. Ne kadar süreyle isteğe bağlı HDInsight kümesi kümede hiç bir etkin iş olduğunda çalıştırmak bir etkinlik tamamlandıktan sonra canlı kalmasını belirtir.<br/><br/>Örneğin, bir etkinlik çalıştırması 6 dakika sürer ve timetolive 5 dakika olarak ayarlanmıştır, küme etkinlik işleme 6 dakika çalıştırdıktan sonra 5 dakika boyunca Canlı kalır. Başka bir etkinlik çalıştırması 6 dakika penceresiyle yürütülürse, aynı küme tarafından işlenir.<br/><br/>Bir isteğe bağlı HDInsight kümesi oluşturma bir yeniden bir isteğe bağlı HDInsight kümesi kullanarak veri fabrikası performansını artırmak için bu ayarı olarak gerekli bunu kullanmak, pahalı işlem (biraz sürebilir) bağlıdır.<br/><br/>Timetolive değeri 0 olarak ayarlarsanız, etkinlik Çalıştır olarak işlenen küme silinir. Öte yandan, yüksek bir değere ayarlarsanız, kümenin yüksek maliyetleri gereksiz yere kaynaklanan boşta kalır. Bu nedenle, ihtiyaçlarınıza uygun değeri ayarlamak önemlidir.<br/><br/>Timetolive özellik değeri uygun şekilde ayarlanmışsa isteğe bağlı HDInsight kümesi'nın aynı örneğine birden fazla işlem hattını paylaşabilirsiniz |Evet |
| version |HDInsight küme sürümü. Ayrıntılar için bkz [desteklenen Azure Data Factory'de HDInsight sürümleri](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Hayır |
| linkedServiceName |Depolama ve veri işleme için isteğe bağlı küme tarafından kullanılacak azure depolama bağlı hizmeti. <p>Şu anda, bir Azure Data Lake Store depolama alanı olarak kullanan bir isteğe bağlı HDInsight kümesi oluşturulamıyor. HDInsight bir Azure Data Lake Store içinde işleme sonuç verileri depolamak istiyorsanız, Azure Data Lake Store için Azure Blob depolama alanından verileri kopyalamak için kopyalama etkinliğini kullanın.</p>  | Evet |
| additionalLinkedServiceNames |Ek depolama hesapları için HDInsight bağlı hizmeti Data Factory hizmetinin sizin adınıza kaydedebilmesi belirtir. |Hayır |
| osType |İşletim sistemi türü. İzin verilen değerler şunlardır: (Varsayılan) Windows ve Linux |Hayır |
| hcatalogLinkedServiceName |Azure SQL adını bağlı HCatalog veritabanına işaret eden hizmeti. Meta veri deposu olarak Azure SQL veritabanı kullanarak isteğe bağlı HDInsight kümesi oluşturulur. |Hayır |

### <a name="json-example"></a>JSON örneği
Aşağıdaki JSON, Linux tabanlı bir isteğe bağlı HDInsight bağlı hizmeti tanımlar. Data Factory hizmetinin otomatik olarak oluşturur bir **Linux tabanlı** veri dilimi işlerken HDInsight kümesi.

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

Daha fazla bilgi için [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md) makalesi.

## <a name="existing-azure-hdinsight-cluster"></a>Var olan Azure HDInsight kümesi
Kendi HDInsight kümenizi Data Factory'ye kaydetmeniz için bir Azure HDInsight bağlı hizmeti oluşturabilirsiniz. Bu bağlı hizmeti üzerinde aşağıdaki veri dönüştürme etkinlikleri çalıştırabilirsiniz: [.NET özel etkinliği](#net-custom-activity), [Hive etkinliği](#hdinsight-hive-activity), [Pig etkinliği](#hdinsight-pig-activity), [ MapReduce etkinliği](#hdinsight-mapreduce-activity), streaming etkinliği, Hadoop [Spark etkinliği](#hdinsight-spark-activity).

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda, bir Azure HDInsight bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır **HDInsight**. |Evet |
| Küme Uri'si |HDInsight küme URİ'si. |Evet |
| kullanıcı adı |Mevcut bir HDInsight kümesine bağlanmak için kullanılacak kullanıcı adını belirtin. |Evet |
| password |Kullanıcı hesabı için parola belirtin. |Evet |
| linkedServiceName | HDInsight küme tarafından kullanılan Azure blob Depolama'ya başvuran Azure depolama bağlı hizmetin adı. <p>Şu anda bu özellik için bir Azure Data Lake Store bağlı belirtemezsiniz. HDInsight kümesi için Data Lake Store erişimi varsa Hive/Pig betikleri Azure Data Lake Store içinde verilerine erişebilir. </p>  |Evet |

HDInsight kümeleri desteklenen sürümleri için bkz: [desteklenen HDInsight sürümleri](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).

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
Data factory ile bir Batch havuzu sanal makineler (VM'ler) kaydetmek için bir Azure Batch bağlı hizmeti oluşturabilirsiniz. Azure Batch ya da Azure HDInsight'ı kullanarak .NET özel etkinlikleri çalıştırabilirsiniz. Çalıştırabileceğiniz bir [.NET özel etkinliği](#net-custom-activity) bu bağlı hizmeti.

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda, bir Azure Batch bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır **AzureBatch**. |Evet |
| accountName |Azure Batch hesabı adı. |Evet |
| accessKey |Azure Batch hesabı için erişim anahtarı. |Evet |
| poolName |Sanal makine havuzu adı. |Evet |
| linkedServiceName |Ad Azure depolama bağlı hizmeti bu bağlı Azure Batch hizmeti ile ilişkilendirilmiş. Bu bağlı hizmeti, etkinlik ve Etkinlik yürütme günlüklerini depolamak çalıştırmak için gereken hazırlık dosyalar için kullanılır. |Evet |


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
Machine Learning toplu Puanlama uç noktası bir data factory ile kaydetmek için bir Azure Machine Learning bağlı hizmet oluşturun. Bu bağlı hizmet üzerinde çalışabilen iki veri dönüştürme etkinlikleri: [Machine Learning batch yürütme etkinliği](#machine-learning-batch-execution-activity), [Machine Learning kaynak güncelleştirme etkinliği](#machine-learning-update-resource-activity).

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tabloda, bir Azure Machine Learning bağlı hizmeti Azure JSON tanımında kullanılan özellikleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Type |Type özelliği ayarlanmalıdır: **AzureML**. |Evet |
| mlEndpoint |Toplu işlem Puanlama URL'si. |Evet |
| ApiKey |Yayımlanan çalışma alanı modelinin API. |Evet |

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
Oluşturduğunuz bir **Azure Data Lake Analytics** kullanmadan önce bir Azure Data Lake Analytics bağlamak için bağlı hizmet bir Azure data factory hizmetine işlem [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md) ardışık düzeninde.

### <a name="linked-service"></a>Bağlı hizmet

Aşağıdaki tabloda, bir Azure Data Lake Analytics bağlı hizmeti JSON tanımında kullanılan özellikleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Type |Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. |Evet |
| accountName |Azure Data Lake Analytics hesap adı. |Evet |
| dataLakeAnalyticsUri |Azure Data Lake Analytics URI. |Hayır |
| Yetkilendirme |Yetkilendirme kodu, tıkladıktan sonra otomatik olarak alınır **Authorize** OAuth oturum açma işlemi tamamlandıktan ve Data Factory Düzenleyicisi'nde düğmesi. |Evet |
| subscriptionId |Azure abonelik kimliği |Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| resourceGroupName |Azure kaynak grubu adı |Hayır (belirtilmezse, data Factory kaynak grubu kullanılır). |
| oturum kimliği |OAuth yetkilendirme oturumundan oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyicisi'ni kullandığınızda, bu kimliği otomatik olarak üretilir. |Evet |


#### <a name="json-example"></a>JSON örneği
Aşağıdaki örnek, bir Azure Data Lake Analytics bağlı hizmeti için JSON tanımını sağlar.

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

## <a name="azure-sql-database"></a>Azure SQL Veritabanı
Bir Azure SQL bağlı hizmeti oluşturma ve kullanılmakta olan [saklı yordam etkinliğine](#stored-procedure-activity) Data Factory işlem hattı bir saklı yordam çağırmak için.

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti Azure SQL veritabanı tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureSqlDatabase**, aşağıdaki özellikleri belirtin **typeProperties** Bölüm:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| bağlantı dizesi |ConnectionString özelliği için Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

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

Bkz: [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties) bu bağlı hizmeti hakkında bilgi için makalenin.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Bir Azure SQL veri ambarı bağlı hizmetini oluşturmak ve kullanılmakta olan [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için.

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmeti bir Azure SQL veri ambarı tanımlamak için Ayarla **türü** bağlı hizmetinin **AzureSqlDW**, aşağıdaki özellikleri belirtin **typeProperties** Bölüm:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| bağlantı dizesi |ConnectionString özelliği için Azure SQL veri ambarı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

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

Daha fazla bilgi için [Azure SQL veri ambarı Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) makalesi.

## <a name="sql-server"></a>SQL Server
SQL Server bağlı hizmeti oluşturma ve kullanılmakta olan [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için.

### <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmet türü oluşturma **OnPremisesSqlServer** bir şirket içi SQL Server veritabanına bir veri fabrikasına bağlamak için. Aşağıdaki tabloda, şirket içi SQL Server bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

Aşağıdaki tabloda, SQL Server bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| bağlantı dizesi |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak üzere gereken bağlantı dizesi bilgilerini belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |
| kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\username**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |

Kimlik bilgilerini kullanarak şifreleyebilirsiniz **yeni AzDataFactoryEncryptValue** cmdlet'i ve bunları aşağıdaki örnekte gösterildiği gibi bağlantı dizesini kullanın (**EncryptedCredential** özellik):

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
#### <a name="example-json-for-using-windows-authentication"></a>Örnek: Windows kimlik doğrulaması kullanmak için JSON

Kullanıcı adı ve parolası belirtilmişse, ağ geçidi bunları şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabının kimliğine bürün için kullanır. Aksi takdirde, ağ geçidi SQL Server Ağ Geçidi (kendi başlangıç hesabı) güvenlik bağlamı ile doğrudan bağlanır.

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

Daha fazla bilgi için [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makalesi.

## <a name="data-transformation-activities"></a>VERİ DÖNÜŞTÜRME ETKİNLİKLERİ

Etkinlik | Açıklama
-------- | -----------
[HDInsight Hive etkinliği](#hdinsight-hive-activity) | Hive sorguları kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi Data Factory işlem hattındaki HDInsight Hive etkinliği yürütür.
[HDInsight Pig etkinliği](#hdinsight-pig-activity) | HDInsight Pig etkinliği bir Data Factory işlem hattında, Pig sorgu kendiniz veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi yürütür.
[HDInsight MapReduce Etkinliği](#hdinsight-mapreduce-activity) | MapReduce programlarını kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi Data Factory işlem hattındaki HDInsight MapReduce etkinliği yürütür.
[HDInsight Akış Etkinliği](#hdinsight-streaming-activity) | HDInsight akış etkinliği bir Data Factory işlem hattında, Hadoop akış programları kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi yürütür.
[HDInsight Spark Etkinliği](#hdinsight-spark-activity) | HDInsight Spark etkinliği bir Data Factory işlem hattı, Spark programlarını kendi HDInsight kümesinde yürütür.
[Machine Learning Batch Yürütme Etkinliği](#machine-learning-batch-execution-activity) | Azure Data Factory, kolayca Tahmine dayalı analiz için yayımlanan bir Azure Machine Learning web hizmetini kullanan işlem hatları oluşturmanıza olanak sağlar. Bir Azure Data Factory işlem hattı, Batch yürütme etkinliği kullanarak verileri toplu tahminlerde bulunmak üzere Machine Learning web hizmetini çağırabilirsiniz.
[Machine Learning Kaynak Güncelleştirme Etkinliği](#machine-learning-update-resource-activity) | Zaman içinde yeni bir giriş veri kümeleri kullanarak eğitilebileceği denemeleri Puanlama Machine learning'de Tahmine dayalı modelleri gerekir. Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti ile retrained Machine Learning modeli güncelleştirmek istiyorsunuz. Web hizmeti ile yeni eğitilen modeli güncelleştirmek için güncelleştirme kaynak etkinliği'ni kullanabilirsiniz.
[Saklı Yordam Etkinliği](#stored-procedure-activity) | Aşağıdaki veri depolarını birinde bir saklı yordam çağırmak için saklı yordam etkinliği bir Data Factory işlem hattında kullanabilirsiniz: Azure SQL veritabanı, Azure SQL veri ambarı, SQL Server veritabanı kuruluşunuza veya bir Azure VM.
[Data Lake Analytics U-SQL etkinliği](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL etkinliği, bir Azure Data Lake Analytics kümesinde bir U-SQL betiği çalıştırır.
[.NET özel etkinliği](#net-custom-activity) | Verileri Data Factory tarafından desteklenmeyen bir şekilde dönüştürmek isterseniz, kendi veri işleme mantığı ile özel bir etkinlik oluşturma ve işlem hattı etkinliğini kullanın. Bir Azure Batch hizmeti ya da bir Azure HDInsight kümesi kullanarak çalıştırmak için özel bir .NET etkinliği yapılandırabilirsiniz.


## <a name="hdinsight-hive-activity"></a>HDInsight Hive Etkinliği
Bir Hive etkinliği JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **Hdınsighthive**. Bir HDInsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** için Hdınsighthive etkinliği türünü ayarladığınızda, bölüm:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| script |Hive betiği satır içi belirtin |Hayır |
| betik yolu |Hive betiği bir Azure blob depolama alanında Store ve dosyanın yolunu belirtin. 'Script' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harfe duyarlıdır. |Hayır |
| tanımlar |Hive betiği 'hiveconf' kullanarak içinde başvurmak için anahtar/değer çiftleri parametrelerini belirtin |Hayır |

Bu tür özellikleri için Hive etkinliği özgüdür. Diğer özelliklerini (dışında typeProperties bölümünün) tüm etkinlikler için desteklenir.

### <a name="json-example"></a>JSON örneği
Aşağıdaki JSON bir işlem hattındaki HDInsight Hive etkinliği tanımlar.

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

Daha fazla bilgi için [Hive etkinliği](data-factory-hive-activity.md) makalesi.

## <a name="hdinsight-pig-activity"></a>HDInsight Pig Etkinliği
Pig etkinliği JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightPig**. Bir HDInsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için HDInsightPig ayarladığınızda:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| script |Pig betiği satır içi belirtin |Hayır |
| betik yolu |Pig betiği bir Azure blob depolama alanında Store ve dosyanın yolunu belirtin. 'Script' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harfe duyarlıdır. |Hayır |
| tanımlar |Pig betiği içinde başvurmak için anahtar/değer çiftleri parametrelerini belirtin |Hayır |

Bu tür özellikleri için Pig etkinliği özgüdür. Diğer özelliklerini (dışında typeProperties bölümünün) tüm etkinlikler için desteklenir.

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

Pig etkinliği daha fazla bilgi için bkz. makale.

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce Etkinliği
MapReduce etkinliği JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightMapReduce**. Bir HDInsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için HDInsightMapReduce ayarladığınızda:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| jarLinkedService | JAR dosyasını içeren Azure depolama için bağlı hizmetin adı. | Evet |
| jarFilePath | Azure storage'da JAR dosyası yolu. | Evet |
| className | JAR dosyasını ana sınıfının adı. | Evet |
| bağımsız değişkenler | MapReduce programını bağımsız değişkenleri virgülle ayrılmış listesi. Çalışma zamanında, gördüğünüz bazı ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle değişkenleriniz ayırt etmek için hem seçeneği hem de değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s, giriş,--çıktısı, vb. değerlerine göre hemen ardından seçenekleri olan) | Hayır |

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

Daha fazla bilgi için [MapReduce etkinliği](data-factory-map-reduce.md) makalesi.

## <a name="hdinsight-streaming-activity"></a>HDInsight Akış Etkinliği
Bir Hadoop akış etkinlik JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **Hdınsightstreaming**. Bir HDInsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için Hdınsightstreaming ayarladığınızda:

| Özellik | Açıklama |
| --- | --- |
| Eşleyici | Yürütülebilir Eşleyici adı. Bu örnekte cat.exe yürütülebilir eşleyicisidir.|
| Azaltıcı | Yürütülebilir Azaltıcı adı. Bu örnekte wc.exe yürütülebilir Azaltıcı ' dir. |
| giriş | (Konum dahil) giriş dosyası Eşleştiricisi için. Örnekte: `"wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt"`: adfsample blob kapsayıcısını, örnek/data/Gutenberg klasördür ve davinci.txt blob. |
| çıkış | Çıktı dosyası (konum dahil) için Azaltıcı. Hadoop akışı tanımlı işlemin çıktısını, bu özellik için belirtilen konuma yazılır. |
| filePaths | Yolları Eşleyici ve azaltıcı yürütülebilir dosyalar için. Örnekte: "adfsample/example/apps/wc.exe" adfsample blob kapsayıcısını, örnek/uygulamaları klasördür ve wc.exe çalıştırılabilir. |
| fileLinkedService | FilePaths bölümünde belirtilen dosyalar içeren bir Azure depolama temsil eden azure depolama bağlı hizmeti. |
| bağımsız değişkenler | MapReduce programını bağımsız değişkenleri virgülle ayrılmış listesi. Çalışma zamanında, gördüğünüz bazı ek bağımsız değişkenler (örneğin: mapreduce.job.tags) MapReduce çerçeveden. MapReduce bağımsız değişkenleriyle değişkenleriniz ayırt etmek için hem seçeneği hem de değer bağımsız değişken olarak aşağıdaki örnekte gösterildiği gibi kullanmayı düşünün (- s, giriş,--çıktısı, vb. değerlerine göre hemen ardından seçenekleri olan) |
| getDebugInfo | İsteğe bağlı bir öğe. Başarısız olduğunda ayarlandığında, günlükleri yalnızca başarısız olduğunda indirilir. Tüm ayarlandığında, günlükleri yürütme durumu bağımsız olarak daima yüklenir. |

> [!NOTE]
> Hadoop akış etkinliğinde'için bir çıktı veri kümesi belirtmelisiniz **çıkarır** özelliği. Bu veri kümesi (saatlik, günlük, vb.) işlem hattı zamanlama sürücü için gerekli olan yalnızca bir işlevsiz veri kümesi olabilir. Etkinliği bir girdi almazsa, girdi veri kümesi için etkinliğin belirtme atlayabilirsiniz **girişleri** özelliği.

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

Daha fazla bilgi için [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md) makalesi.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark Etkinliği
Bir Spark etkinliği JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **HDInsightSpark**. Bir HDInsight bağlı hizmeti ilk oluşturun ve değeri olarak adını belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için HDInsightSpark ayarladığınızda:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| rootPath | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harfe duyarlıdır. | Evet |
| entryFilePath | Spark kodun/paketin kök klasörünün göreli yolu. | Evet |
| className | Uygulamanın Java/Spark temel sınıfı | Hayır |
| bağımsız değişkenler | Spark programı için komut satırı bağımsız değişkenleri listesi. | Hayır |
| Proxyuserpassword | Spark programının yürütülecek kimliğine bürünmek için kullanıcı hesabı | Hayır |
| sparkConfig | Spark yapılandırma özellikleri. | Hayır |
| getDebugInfo | HDInsight kümesi tarafından kullanılan Azure depolama için Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService belirtilir. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: Yok. | Hayır |
| sparkJobLinkedService | Azure depolama bağlı iş dosyası, bağımlılıklar ve günlükleri Spark tutan hizmeti.  Bu özellik için bir değer belirtmezseniz, HDInsight kümesi ile ilişkili depolama kullanılır. | Hayır |

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

- **Türü** özelliği **HDInsightSpark**.
- **RootPath** ayarlanır **adfspark\\pyFiles** burada adfspark Azure Blob kapsayıcısı ve pyFiles kapsayıcıdaki ince klasördür. Bu örnekte, Spark kümesi ile ilişkili bir Azure Blob depolama alanıdır. Farklı bir Azure depolama için dosyayı karşıya yükleyebilirsiniz. Bunu yaparsanız, depolama hesabınızı veri fabrikasına bağlamak için bir Azure depolama bağlı hizmeti oluşturma. Ardından için bir değer olarak bağlı hizmetin adı belirtin **sparkJobLinkedService** özelliği. Spark etkinliğini bu özellik ve Spark etkinliği tarafından desteklenen diğer özellikler hakkında ayrıntılı bilgi için bkz.
- **EntryFilePath** ayarlanır **test.py**, python dosyası olduğu.
- **Getdebugınfo** özelliği **her zaman**, günlük dosyaları her zaman anlamına gelir (başarı veya başarısızlık) oluşturulur.

    > [!IMPORTANT]
    > Bir sorunu gidermeye çalışıyor değilseniz, bu özellik her zaman bir üretim ortamında ayarlamanız önerilir.
- **Çıkarır** bölümünde bir çıktı veri kümesi bulunur. Spark programının hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi belirtmelisiniz. Çıktı veri kümesi (saatlik, günlük, vb.) işlem hattı için zamanlamayı beraberinde getirir.

Etkinlik hakkında daha fazla bilgi için bkz. [Spark etkinliği](data-factory-spark.md) makalesi.

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning Batch Yürütme Etkinliği
Azure Machine Learning Studio'da Batch yürütme etkinliği JSON tanımı, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **AzureMLBatchExecution**. Bir Azure Machine Learning ilk bağlı hizmeti oluşturma ve bu adı için bir değer olarak belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için AzureMLBatchExecution ayarladığınızda:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
hem WebServiceInput | Azure Machine Learning studio web hizmeti için bir giriş olarak geçirilecek veri kümesi. Bu veri kümesi için etkinlik girişlerinde de eklenmelidir. |Veya hem WebServiceInput hem de Webserviceınputs kullanın. |
Webserviceınputs | Veri kümeleri, Azure Machine Learning studio web hizmeti için girdi olarak geçirilecek belirtin. Web hizmetini birden fazla giriş aldığı durumlarda hem WebServiceInput özelliğini kullanarak yerine Webserviceınputs özelliğini kullanın. Tarafından başvurulan veri kümeleri **Webserviceınputs** etkinliğinde eklenmelidir **girişleri**. | Veya hem WebServiceInput hem de Webserviceınputs kullanın. |
webServiceOutputs | Azure Machine Learning studio web hizmeti için çıktı olarak atanmış olan veri kümeleri. Web hizmeti, bu veri kümesi çıktı verilerini döndürür. | Evet |
globalParameters | Bu bölümde web hizmeti parametreleri için değerler belirtin. | Hayır |

### <a name="json-example"></a>JSON örneği
Bu örnekte, etkinlik bir veri kümesine sahiptir. **MLSqlInput** giriş olarak ve **MLSqlOutput** çıktı olarak. **MLSqlInput** kullanılarak geçirilen bir web hizmeti tarafından giriş olarak **WebServiceInput** JSON özelliği. **MLSqlOutput** çıkış olarak Web hizmeti tarafından kullanılarak geçirilir **webServiceOutputs** JSON özelliği.

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

JSON örnekte, Azure SQL veritabanı ' / için veri okuma/yazma için bir okuyucu ve yazıcı modülü dağıtılan Azure Machine Learning Web hizmetini kullanır. Bu Web hizmetini aşağıdaki dört parametre sunar:  Veritabanı sunucusu adı, veritabanı adı, sunucu kullanıcı hesabı adını ve Server kullanıcı hesabı parolası.

> [!NOTE]
> Yalnızca giriş ve çıkışları AzureMLBatchExecution etkinliğin Web hizmetine parametre olarak geçirilebilir. Örneğin, yukarıdaki JSON parçacığında, bir Web hizmeti giriş olarak WebServiceInput parametresi geçirilen AzureMLBatchExecution etkinliği bir girdi MLSqlInput olur.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning Kaynak Güncelleştirme Etkinliği
Azure Machine Learning Studio'da güncelleştirme kaynak etkinlik JSON tanımı, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **AzureMLUpdateResource**. Bir Azure Machine Learning ilk bağlı hizmeti oluşturma ve bu adı için bir değer olarak belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** için AzureMLUpdateResource etkinliği türünü ayarladığınızda, bölüm:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
trainedModelName | Retrained modelin adı. | Evet |
trainedModelDatasetName | Yeniden eğitme işlem tarafından döndürülen olan iLearner dosyasını işaret eden bir veri kümesi. | Evet |

### <a name="json-example"></a>JSON örneği
İşlem hattı iki etkinlik içerir: **AzureMLBatchExecution** ve **AzureMLUpdateResource**. Azure Machine Learning studio Batch yürütme etkinliği, girdi olarak eğitim verileri alır ve çıktı olarak bir iLearner dosyası üretir. Etkinlik, giriş eğitim verilerle eğitim web hizmeti (bir web hizmeti olarak kullanıma sunulan eğitim denemesini) çağırır ve webservice olan ilearner dosyasını alır. PlaceholderBlob yalnızca Azure Data Factory hizmeti tarafından işlem hattını çalıştırmak için gerekli olan bir işlevsiz bir çıktı veri kümesidir.


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
U-SQL etkinliği JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **DataLakeAnalyticsU SQL**. Bir Azure Data Lake Analytics bağlı hizmeti oluşturma ve bu adı için bir değer olarak belirtmeniz gerekir **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** DataLakeAnalyticsU-SQL etkinliği türünü ayarladığınızda, bölüm:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| ScriptPath |U-SQL komut dosyasını içeren klasörün yolu. Dosyanın adı büyük/küçük harfe duyarlıdır. |Hayır (komut dosyası kullanırsanız) |
| scriptLinkedService |Bağlantılar için veri komut dosyasını içeren depolama bağlı hizmeti |Hayır (komut dosyası kullanırsanız) |
| script |ScriptPath ve scriptLinkedService belirtmek yerine satır içi betiği belirtin. Örneğin: "betik": "CREATE DATABASE test". |Hayır (scriptPath ve scriptLinkedService kullanıyorsanız) |
| degreeOfParallelism |Aynı anda işi çalıştırmak için kullanılan düğümlerin sayısı. |Hayır |
| öncelik |Sıraya alınan tüm önce çalıştırılması gerektiğini belirler. Alt sayısı, öncelik o kadar yüksektir. |Hayır |
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

Daha fazla bilgi için [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md).

## <a name="stored-procedure-activity"></a>Saklı Yordam Etkinliği
Bir saklı yordam etkinliği JSON tanımında, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **SqlServerStoredProcedure**. Aşağıdaki bağlı hizmetler, bir tane oluşturun ve bağlı hizmet adı için bir değer olarak belirtmeniz gerekir **linkedServiceName** özelliği:

- SQL Server
- Azure SQL Veritabanı
- Azure SQL Veri Ambarı

Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için SqlServerStoredProcedure ayarladığınızda:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| storedProcedureName |Çıktı tablosu kullanan bağlı hizmetiyle temsil edilen Azure SQL veri ambarı ve Azure SQL veritabanı saklı yordamın adını belirtin. |Evet |
| storedProcedureParameters |Saklı yordam parametrelerinin değerlerini belirtin. Bir parametre için null değeri geçirmeye gerekiyorsa, söz dizimini kullanın: "param1": null (küçük harflerle). Aşağıdaki örnek bu özelliği kullanma hakkında bilgi edinmek için bkz. |Hayır |

Girdi veri kümesi belirtirseniz, ('Hazır' durumunda) kullanılabilir olmalıdır çalıştırmak saklı yordam etkinliği. Giriş veri kümesi saklı yordam, bir parametre olarak kullanılamıyor. Yalnızca, saklı yordam etkinliği başlamadan önce bağımlılık denetlemek için kullanılır. Bir saklı yordam etkinliği için bir çıktı veri kümesi belirtmelisiniz.

Çıktı veri kümesi belirtir **zamanlama** saklı yordam etkinliği (saatlik, haftalık, aylık, vb.). Çıktı veri kümesi kullanmalısınız bir **bağlı hizmet** Azure SQL veritabanı veya bir Azure SQL veri ambarı veya SQL Server veritabanı saklı yordamı çalıştırmak için istediğiniz başvuruyor. Çıktı veri kümesi için saklı yordam sonucu başka bir etkinlik tarafından işleme sonraki geçirmek için bir yol olarak hizmet verebilen ([zincirleme etkinlikleri](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) işlem hattındaki. Ancak, Data Factory otomatik olarak bir saklı yordam çıktısı bu veri kümesine yazmaz. Bu çıktı veri kümesini işaret eden bir SQL tablosunu yazan saklı yordam aynıdır. Bazı durumlarda, çıktı veri kümesi olabilir bir **işlevsiz bir veri kümesi**, yalnızca saklı yordam etkinliği çalıştırmak için zamanlamayı belirtmek için kullanılır.

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "usp_sample",
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

Daha fazla bilgi için [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) makalesi.

## <a name="net-custom-activity"></a>.NET özel etkinliği
Bir .NET özel etkinliği JSON tanımı, aşağıdaki özellikleri belirtebilirsiniz. Etkinlik türü özelliği olması gerekir: **DotNetActivity**. Bir Azure HDInsight bağlı hizmeti oluşturmanız gerekir veya bağlı bir Azure Batch hizmeti ve bağlı hizmet adı için bir değer olarak belirtin **linkedServiceName** özelliği. Aşağıdaki özellikler desteklenir **typeProperties** bölümünde etkinlik türü için DotNetActivity ayarladığınızda:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| AssemblyName | Derlemenin adı. Bu örnekte, değil: **MyDotnetActivity.dll**. | Evet |
| Giriş noktası |Idotnetactivity arabirimi uygulayan sınıfın adı. Bu örnekte, değil: **MyDotNetActivityNS.MyDotNetActivity** burada MyDotNetActivityNS ad alanı ve MyDotNetActivity sınıftır.  | Evet |
| PackageLinkedService | Özel etkinliğin zip dosyasını içeren blob depolama alanına işaret eden bir Azure depolama bağlı hizmetinin adı. Bu örnekte, değil: **AzureStorageLinkedService**.| Evet |
| PackageFile | ZIP dosyasının adı. Bu örnekte olduğu: **customactivitycontainer/MyDotNetActivity.zip**. | Evet |
| ExtendedProperties | Tanımlayabilir ve geçirmek .NET kodu için genişletilmiş özellikler. Bu örnekte, **SliceStart** değişkeni SliceStart sistem değişkeninde dayalı bir değere ayarlanır. | Hayır |

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

Ayrıntılı bilgi için bkz. [Data Factory'de özel etkinlikler kullanma](data-factory-use-custom-activities.md) makalesi.

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki öğreticilere bakın:

- [Öğretici: kopyalama etkinliği ile işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Öğretici: bir hive etkinliği ile işlem hattı oluşturma](data-factory-build-your-first-pipeline-using-editor.md)
