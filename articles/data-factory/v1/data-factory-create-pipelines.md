---
title: Ardışık düzen oluşturma/zamanlaması, veri fabrikası'nda etkinlikler zincir | Microsoft Docs
description: Verileri taşımak ve dönüştürmek için Azure Data Factory içinde veri ardışık oluşturmayı öğrenin. Bilgileri kullanmak için hazır üretmek için veri tabanlı iş akışı oluşturursunuz.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: be071c8138a6782ad144a42d52d737f248ff7a7b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>İşlem hatlarının ve etkinliklerin Azure veri fabrikası
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-create-pipelines.md)
> * [Sürüm 2 - Önizleme](../concepts-pipelines-activities.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [ardışık düzenlerinde V2](../concepts-pipelines-activities.md).

Bu makale, Azure Data Factory’de işlem hatlarını ve etkinlikleri anlamanıza ve veri hareketi ile veri işleme senaryolarınız için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur.  

> [!NOTE]
> Bu makalede, ilerlemiş olduğunu varsayar [Azure Data Factory'ye giriş](data-factory-introduction.md). Hands-on-data Factory oluşturma konusunda deneyim yoksa oluşturulmak [veri dönüştürme öğretici](data-factory-build-your-first-pipeline.md) ve/veya [veri taşıma öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) , bu makalede daha iyi anlamanıza yardımcı.  

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, kopyalama etkinliğini kullanarak şirket içi SQL Server’dan Azure Blob Depolama’ya veri kopyalayabilirsiniz. Ardından bir Azure HDInsight kümesinde Hive betiği çalıştıran bir Hive etkinliği kullanıp blob depolamadaki verileri işleyerek/dönüştürerek çıktı verileri üretebilirsiniz. Son olarak, ikinci bir kopyalama etkinliği kullanarak üzerinde iş zekası (BI) raporlama çözümlerinin oluşturulduğu bir Azure SQL Veri Ambarı’na çıktı verilerini kopyalayabilirsiniz. 

Bir etkinliğin sıfır veya sıfırdan çok giriş [veri kümesi](data-factory-create-datasets.md) olabilir ve her etkinlik bir veya birden çok çıkış [veri kümesi](data-factory-create-datasets.md) oluşturabilir. Aşağıdaki diyagramda, Data Factory içindeki işlem hattı, etkinlik ve veri kümesi arasındaki ilişki gösterilmektedir: 

![Ardışık düzen, etkinlik ve veri kümesi arasındaki ilişki](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Bir ardışık düzen tek tek etkinliklerin her biri yerine bir küme olarak yönetmenize olanak sağlar. Örneğin, dağıtma, zamanlama, askıya alma ve ardışık düzendeki etkinlik ile bağımsız olarak ilgilenme yerine bir ardışık düzen Sürdür.

Data Factory iki tür etkinliği destekler: veri taşıma etkinlikleri ve veri dönüştürme etkinlikleri. Her etkinlik sıfır veya daha fazla giriş olabilir [veri kümeleri](data-factory-create-datasets.md) ve bir veya daha fazla çıkış veri kümeleri oluşturma.

Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Bir veri kümesi oluşturduktan sonra, bu kümeyi bir işlem hattındaki etkinliklerle birlikte kullanabilirsiniz. Örneğin, veri kümesi bir Kopyalama Etkinliğinin veya HDInsightHive Etkinliğinin giriş/çıkış veri kümesi olabilir. Veri kümeleri hakkında daha fazla bilgi için [Azure Data Factory'de Veri Kümeleri](data-factory-create-datasets.md) makalesine bakın.

### <a name="data-movement-activities"></a>Veri taşıma etkinlikleri
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> * taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

Daha fazla bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

### <a name="data-transformation-activities"></a>Veri dönüştürme etkinlikleri
[!INCLUDE [data-factory-transformation-activities](../../../includes/data-factory-transformation-activities.md)]

Daha fazla bilgi için [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) makalesine bakın.

### <a name="custom-net-activities"></a>Özel .NET etkinlikleri 
Kopyalama etkinliği olmayan desteği veya kendi mantığı kullanarak verileri oluşturun, bir veri/verileri depolamak taşımanız gerekirse, bir **özel .NET etkinlik**. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

## <a name="schedule-pipelines"></a>Zamanlama komut zincirleri
İşlem hattı yalnızca arasında etkin olduğu kendi **Başlat** zaman ve **son** saat. Bu başlangıç saatinden önce veya sonra bitiş saati yürütülmedi. Ardışık Düzen duraklatıldığında, kendi başlangıç ve bitiş zamanı yedeklemiş yürütülmedi. Çalıştırmak ardışık düzeni için bu altyapının duraklatılması değil. Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) zamanlama ve yürütme şeklini Azure Data Factory'de anlamak için.

## <a name="pipeline-json"></a>İşlem Hattı JSON
Bir işlem hattının JSON biçiminde nasıl tanımlandığına daha yakından bakalım. Bir işlem hattının genel yapısı şu şekilde görünür:

```json
{
    "name": "PipelineName",
    "properties": 
    {
        "description" : "pipeline description",
        "activities":
        [

        ],
        "start": "<start date-time>",
        "end": "<end date-time>",
        "isPaused": true/false,
        "pipelineMode": "scheduled/onetime",
        "expirationTime": "15.00:00:00",
        "datasets": 
        [
        ]
    }
}
```

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad |İşlem hattının adı. İşlem hattının gerçekleştirdiği eylemi temsil eden bir ad belirtin. <br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi (_) ile başlamalıdır</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama | İşlem hattının ne için kullanıldığını açıklayan metni belirtin. |Evet |
| etkinlikler | **Etkinlikler** bölümünde tanımlanmış bir veya daha fazla etkinlik olabilir. Etkinlikler JSON öğesi hakkında ayrıntılı bilgi için sonraki bölüme bakın. | Evet |  
| start | Ardışık düzeni için başlangıç tarihi / saati. Olmalıdır [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601). Örneğin: `2016-10-14T16:32:41Z`. <br/><br/>Yerel saat örneğin tahmini süre belirlemek mümkündür. Örnek aşağıda verilmiştir: `2016-02-27T06:00:00-05:00`", 6'da tahmini olduğu<br/><br/>Başlangıç ve bitiş özellikleri birlikte ardışık düzen etkin süresini belirtin. Çıktı dilimler yalnızca ile etkin bu dönemde üretilir. |Hayır<br/><br/>End özelliği için bir değer belirtirseniz, başlangıç özelliği için değer belirtmeniz gerekir.<br/><br/>Başlangıç ve bitiş zamanlarını hem de bir işlem hattı oluşturmak için boş olabilir. Çalıştırmak ardışık düzeni için etkin bir süre ayarlamak için her iki değer belirtmeniz gerekir. Başlangıç ve bitiş zamanlarını belirtmezseniz, bir işlem hattı oluştururken, bunları daha sonra Set-AzureRmDataFactoryPipelineActivePeriod cmdlet'ini kullanarak ayarlayabilirsiniz. |
| end | Ardışık düzeni için bitiş tarihi / saati. Belirtilen ISO biçiminde olmalıdır. Örneğin, `2016-10-14T17:32:41Z` <br/><br/>Yerel saat örneğin tahmini süre belirlemek mümkündür. Örnek aşağıda verilmiştir: `2016-02-27T06:00:00-05:00`, 6'da tahmini olduğu<br/><br/>İşlem hattını süresiz olarak çalışacak şekilde 9999-09-09 end özelliği için değer olarak belirtin. <br/><br/> Yalnızca kendi başlangıç ve bitiş zamanı arasında bir ardışık düzen etkindir. Bu başlangıç saatinden önce veya sonra bitiş saati yürütülmedi. Ardışık Düzen duraklatıldığında, kendi başlangıç ve bitiş zamanı yedeklemiş yürütülmedi. Çalıştırmak ardışık düzeni için bu altyapının duraklatılması değil. Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) zamanlama ve yürütme şeklini Azure Data Factory'de anlamak için. |Hayır <br/><br/>Başlangıç özellik için bir değer belirtirseniz, son özelliği için değer belirtmeniz gerekir.<br/><br/>İçin notlarına bakın **Başlat** özelliği. |
| isPaused | TRUE olarak ardışık düzen çalışmazsa. Duraklatılmış durumda. Varsayılan değer = false. Bu özelliği etkinleştirmek veya devre dışı bir ardışık düzen için kullanabilirsiniz. |Hayır |
| pipelineMode | Zamanlama için yöntem ardışık düzeni için çalışır. İzin verilen değerler: (varsayılan), zamanlanmış kez.<br/><br/>'Zamanlanmış' ardışık düzen belirtilen süre aralığında etkin süresinin (başlangıç ve bitiş saati) göre çalıştırıldığını gösterir. 'Kez' ardışık düzen yalnızca bir kez çalıştırıldığını gösterir. Oluşturulduktan sonra kez ardışık düzen şu anda değiştiren/güncelleştirilemiyor. Bkz: [Onetime ardışık düzen](#onetime-pipeline) kez ayar hakkındaki ayrıntılar için. |Hayır |
| expirationTime | Kendisi için oluşturulduktan sonra süreyi [tek seferlik ardışık düzen](#onetime-pipeline) geçerli olduğunu ve sağlanan kalmalıdır. Her etkin başarısız oldu, yok veya çalıştığında, ardışık düzen otomatik olarak bir kez silinmiş, sona erme zamanı ulaşır. Varsayılan değer: `"expirationTime": "3.00:00:00"`|Hayır |
| Veri kümeleri |Ardışık düzeninde tanımlanan etkinlikleri tarafından kullanılacak veri kümeleri listesi. Bu özellik, bu ardışık düzen özeldir ve data factory içinde tanımlı değil veri kümeleri tanımlamak için kullanılabilir. Bu ardışık düzen içinde tanımlanan veri kümeleri yalnızca bu ardışık düzen tarafından kullanılabilir ve paylaşılamaz. Bkz: [veri kümeleri kapsamlı](data-factory-create-datasets.md#scoped-datasets) Ayrıntılar için. |Hayır |

## <a name="activity-json"></a>Etkinlik JSON
**Etkinlikler** bölümünde tanımlanmış bir veya daha fazla etkinlik olabilir. Her etkinlik, aşağıdaki üst düzey yapısını sahiptir:

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

Aşağıdaki tabloda, etkinlik JSON tanımındaki özellikler açıklamaktadır:

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad | Etkinliğin adı. Etkinliğin gerçekleştirdiği eylemi temsil eden bir ad belirtin. <br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi (_) ile başlamalıdır</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama | Etkinliğin ne olduğunu veya ne için kullanıldığını açıklayan metin |Evet |
| type | Etkinliğin türü. Bkz: [veri taşıma etkinlikleri](#data-movement-activities) ve [veri dönüştürme etkinlikleri](#data-transformation-activities) bölümleri etkinlikler farklı türde. |Evet |
| Girişleri |Etkinlik tarafından kullanılan giriş tabloları<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Evet |
| çıkışlar |Etkinlik tarafından kullanılan çıkış tabloları.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Evet |
| linkedServiceName |Etkinlik tarafından kullanılan bağlı hizmetin adı. <br/><br/>Bir etkinlik için gerekli işlem ortamına bağlanan bağlı hizmeti belirtmeniz gerekebilir. |Hdınsight etkinliği ve Azure Machine Learning toplu iş Puanlama etkinliği için Evet <br/><br/>Diğer tümü için hayır |
| typeProperties |Özelliklerinde **typeProperties** bölümü etkinlik türüne bağlıdır. Bir etkinliğin tür özelliklerini görmek için önceki bölümde verilen etkinlik bağlantılarına tıklayın. | Hayır |
| ilke |Etkinliğin çalışma zamanı davranışını etkileyen ilkeler. Belirtilmezse, varsayılan ilkeler kullanılır. |Hayır |
| Zamanlayıcı | "Zamanlayıcı" özelliği, istenen etkinliği için zamanlama tanımlamak için kullanılır. Onun alt Listedekilerin aynıdır [dataset kullanılabilirliği özelliğinde](data-factory-create-datasets.md#dataset-availability). |Hayır |


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

## <a name="sample-copy-pipeline"></a>Örnek kopyalama işlem hattı
Aşağıdaki örnek işlem hattında, **Etkinlikler** bölümünde **Kopyalama** türünde olan bir etkinlik vardır. Bu örnekte [kopyalama etkinliği](data-factory-data-movement-activities.md), verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalar. 

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
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
} 
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde, **türü** **Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
* Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır. JSON biçiminde veri kümeleri tanımlamak için [Veri Kümeleri](data-factory-create-datasets.md) makalesine bakın. 
* **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir. İçinde [veri taşıma etkinlikleri](#data-movement-activities) bölümü, verileri depolamak/o veri deposundan veri taşıma hakkında daha fazla bilgi için bir kaynak veya bir havuz olarak kullanmak istediğiniz tıklatın. 

Bu ardışık düzen oluşturma izlenecek tam yol için bkz: [Öğreticisi: veri kopyalama Blob depolama alanından SQL veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Örnek dönüştürme işlem hattı
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
        "start": "2016-04-01T00:00:00Z",
        "end": "2016-04-02T00:00:00Z",
        "isPaused": false
    }
}
```

Aşağıdaki noktalara dikkat edin: 

* Etkinlikler bölümünde **türü** **HDInsightHive** olarak ayarlanmış yalnızca bir etkinlik vardır.
* **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **AzureStorageLinkedService** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.
* `defines` Bölümü hive betiğine Hive yapılandırma değerleri olarak geçirilir çalışma zamanı ayarlarını belirtmek için kullanılır (örneğin `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

**TypeProperties** bölümü her bir dönüştürme etkinliği için farklıdır. Dönüştürme etkinliğine için desteklenen tür özellikleri hakkında bilgi edinmek için dönüştürme etkinliğine tıklayın [veri dönüştürme etkinlikleri](#data-transformation-activities) tablo. 

Bu ardışık düzen oluşturma izlenecek tam yol için bkz: [Öğreticisi: Hadoop kümesi kullanarak verileri işlemek için ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md). 

## <a name="multiple-activities-in-a-pipeline"></a>Bir işlem hattında birden çok etkinlik
Önceki iki örnekte işlem hatları yalnızca bir etkinlik içeriyordu. Bir işlem hattında birden fazla etkinliğiniz olabilir.  

Etkinlikler için giriş veri dilimi hazır olduğunuzda bir ardışık düzende birden çok etkinliği varsa ve bir etkinlik çıktısını başka bir etkinliğin bir giriş değil, etkinlikleri paralel olarak çalışabilir. 

Çıkış veri kümesi bir etkinlik diğer etkinliğin girdi veri kümesi olarak sağlayarak iki etkinlik zincir. İkinci etkinlik, yalnızca ilk başarıyla tamamlandığında yürütür.

![Aynı ardışık düzendeki etkinlik zincirleme](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Bu örnekte, ardışık düzen iki etkinlik vardır: Activity1 ve Activity2. Activity1 Dataset1 bir girdi olarak alır ve bir çıktı üretir Dataset2. Etkinlik Dataset2 bir girdi olarak alır ve bir çıktı üretir Dataset3. Activity1 çıktısını bu yana (Dataset2) olan Activity2, etkinlik başarıyla tamamlanır ve Dataset2 dilimi üreten sonra Activity2 çalıştırır girişi. Activity1 herhangi bir nedenden dolayı başarısız olursa ve Dataset2 dilim üretmez etkinlik 2, dilim için çalışmaz (örneğin: 09: 00 için 10: 00). 

Ayrıca, farklı ardışık düzenlerinde etkinlikleri zincir.

![İki ardışık düzende zincirleme etkinlikleri](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Bu örnekte, Pipeline1 Dataset1 bir girdi olarak alır ve Dataset2 bir çıktı olarak üretir yalnızca bir etkinlik vardır. Pipeline2 de girdi ve çıktı olarak Dataset3 Dataset2 alır yalnızca bir etkinlik vardır. 

Daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="create-and-monitor-pipelines"></a>Oluşturma ve işlem hatlarını izleme
Ardışık Düzen Bu araçlar ya da SDK'ları birini kullanarak oluşturabilirsiniz. 

- Kopyalama Sihirbazı. 
- Azure portalına
- Visual Studio
- Azure PowerShell
- Azure Resource Manager şablonu
- REST API
- .NET API’si

Bu araçlar ya da SDK'ları biri kullanılarak işlem hatlarını oluşturmak için aşağıdaki öğreticileri adım adım yönergeler için bkz.
 
- [Veri dönüştürme etkinliğine sahip işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)
- [Veri taşıma etkinliği ile işlem hattı oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Oluşturulan ve dağıtılan bir ardışık düzen olduktan sonra yönetebilir ve Azure portal dikey penceresi veya izleme ve yönetme uygulaması kullanılarak işlem hatlarınızı izlemek. Adım adım yönergeler için aşağıdaki konulara bakın. 

- [İzleme ve Azure portal dikey penceresi kullanılarak işlem hatlarını yönetme](data-factory-monitor-manage-pipelines.md).
- [İzleme ve işlem hatlarını izleme ve yönetme uygulaması'nı kullanarak yönetme](data-factory-monitor-manage-app.md)


## <a name="onetime-pipeline"></a>Kez ardışık düzen
Oluşturma ve düzenli olarak çalıştırmak için bir işlem hattı zamanlama (örneğin: saatlik veya günlük) ardışık düzen tanımı'nda belirttiğiniz başlangıç ve bitiş zamanlarını içinde. Bkz: [etkinlikleri zamanlama](#scheduling-and-execution) Ayrıntılar için. Yalnızca bir kez çalışır bir ardışık düzen de oluşturabilirsiniz. Bunu yapmak için ayarladığınız **pipelineMode** ardışık düzen tanımına özelliğinde **kez** aşağıdaki JSON örnekte gösterildiği gibi. Bu özellik için varsayılan değer **zamanlanmış**.

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ]
                "name": "CopyActivity-0"
            }
        ]
        "pipelineMode": "OneTime"
    }
}
```

Şunlara dikkat edin:

* **Başlat** ve **son** kez ardışık düzeni için belirtilmemiş.
* **Kullanılabilirlik** giriş ve çıkış veri kümeleri belirtilir (**sıklığı** ve **aralığı**), veri fabrikası değerleri kullanmıyor olsa bile.  
* Diyagram görünümü tek seferlik ardışık düzen göstermez. Bu davranış tasarım gereğidir.
* Tek seferlik ardışık düzen güncelleştirilemiyor. Tek seferlik Ardışık düzenin kopyalama, yeniden adlandırmak, özelliklerini güncelleştirmek ve başka bir tane oluşturmak dağıtma.


## <a name="next-steps"></a>Sonraki Adımlar
- Veri kümeleri hakkında daha fazla bilgi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. 
- Ardışık Düzen nasıl zamanlanmış ve yürütülen hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme Azure veri fabrikası'nda](data-factory-scheduling-and-execution.md) makalesi. 
  

