---
title: / İşlem hatları zamanlaması oluşturma, Data Factory'deki zincir | Microsoft Docs
description: Veri taşımak ve dönüştürmek için Azure Data Factory'de veri işlem hattı oluşturmayı öğrenin. Bilgi kullanmaya hazır üretmek için veri odaklı iş akışı oluşturun.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: ee34c91787ede0431c71b0fd96d2c040717dbca2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60487427"
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>İşlem hatları ve etkinlikler Azure Data factory'de
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-create-pipelines.md)
> * [Sürüm 2 (geçerli sürüm)](../concepts-pipelines-activities.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 işlem hatlarında](../concepts-pipelines-activities.md).

Bu makale, Azure Data Factory’de işlem hatlarını ve etkinlikleri anlamanıza ve veri hareketi ile veri işleme senaryolarınız için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur.

> [!NOTE]
> Bu makalede çalıştınız olduğunu varsayar [Azure Data Factory'ye giriş](data-factory-introduction.md). Uygulamalı-on-veri fabrikaları oluşturma deneyimi ile yoksa oluşturulmak [veri dönüştürme öğreticisini](data-factory-build-your-first-pipeline.md) ve/veya [veri taşıma öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) bu makalede daha iyi anlamanıza yardımcı.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, kopyalama etkinliğini kullanarak şirket içi SQL Server’dan Azure Blob Depolama’ya veri kopyalayabilirsiniz. Ardından bir Azure HDInsight kümesinde Hive betiği çalıştıran bir Hive etkinliği kullanıp blob depolamadaki verileri işleyerek/dönüştürerek çıktı verileri üretebilirsiniz. Son olarak, ikinci bir kopyalama etkinliği kullanarak üzerinde iş zekası (BI) raporlama çözümlerinin oluşturulduğu bir Azure SQL Veri Ambarı’na çıktı verilerini kopyalayabilirsiniz.

Bir etkinliğin sıfır veya sıfırdan çok giriş [veri kümesi](data-factory-create-datasets.md) olabilir ve her etkinlik bir veya birden çok çıkış [veri kümesi](data-factory-create-datasets.md) oluşturabilir. Aşağıdaki diyagramda, Data Factory içindeki işlem hattı, etkinlik ve veri kümesi arasındaki ilişki gösterilmektedir:

![İşlem hattı, etkinlik ve veri kümesi arasındaki ilişki](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Bir işlem hattı, tek tek etkinlikleri her birinin yerine bir küme olarak yönetmenize olanak sağlar. Örneğin, dağıtma, zamanlama, askıya alma ve bağımsız olarak işlem hattındaki etkinlikler ile ilgilenen yerine bir işlem hattı Sürdür.

Data Factory iki tür etkinliği destekler: veri taşıma etkinlikleri ve veri dönüştürme etkinlikleri. Her etkinliğin sıfır veya daha fazla giriş sağlayabilirsiniz [veri kümeleri](data-factory-create-datasets.md) ve bir veya daha fazla çıkış veri kümesi üretir.

Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Bir veri kümesi oluşturduktan sonra, bu kümeyi bir işlem hattındaki etkinliklerle birlikte kullanabilirsiniz. Örneğin, veri kümesi bir Kopyalama Etkinliğinin veya HDInsightHive Etkinliğinin giriş/çıkış veri kümesi olabilir. Veri kümeleri hakkında daha fazla bilgi için [Azure Data Factory'de Veri Kümeleri](data-factory-create-datasets.md) makalesine bakın.

### <a name="data-movement-activities"></a>Veri taşıma etkinlikleri
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> \* taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

Daha fazla bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

### <a name="data-transformation-activities"></a>Veri dönüştürme etkinlikleri
[!INCLUDE [data-factory-transformation-activities](../../../includes/data-factory-transformation-activities.md)]

Daha fazla bilgi için [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) makalesine bakın.

### <a name="custom-net-activities"></a>Özel .NET etkinlikleri
Kopyalama etkinliği değil desteği ya da kendi mantığınızı kullanarak verileri dönüştürme, oluşturma, veri gönderip buralardan veri deposuna taşımanız gerekiyorsa bir **özel bir .NET etkinliği**. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

## <a name="schedule-pipelines"></a>Zamanlama işlem hatları
Bir işlem hattı yalnızca arasında etkindir, **Başlat** zaman ve **son** zaman. Başlangıç zamanından önce veya sonra bitiş saati yürütülmez. İşlem hattı duraklatılmışsa, bağımsız olarak kendi başlangıç ve bitiş zamanı Yürütülmeyen. Çalıştırmak için işlem hattı için duraklatma değil. Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) zamanlama ve yürütme işleyişi Azure Data Factory'de anlamak için.

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
| ad |İşlem hattının adı. İşlem hattının gerçekleştirdiği eylemi temsil eden bir ad belirtin. <br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi ile başlamalıdır (\_)</li><li>Karakterler kullanılamaz: ".", "+","?", "/", "<",">", "\*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama | İşlem hattının ne için kullanıldığını açıklayan metni belirtin. |Evet |
| etkinlikler | **Etkinlikler** bölümünde tanımlanmış bir veya daha fazla etkinlik olabilir. Etkinliklerin JSON öğesi hakkında ayrıntılı bilgi için sonraki bölüme bakın. | Evet |
| start | İşlem hattının başlangıç tarihi / saati. Olmalıdır [ISO biçimi](https://en.wikipedia.org/wiki/ISO_8601). Örneğin: `2016-10-14T16:32:41Z`. <br/><br/>Yerel saati, örneğin bir Tah belirtmek mümkündür. Bir örnek aşağıda verilmiştir: `2016-02-27T06:00:00-05:00`", 6 AM tahmini olduğu<br/><br/>Başlangıç ve bitiş özellikleri işlem hattının etkin dönemini birlikte belirtin. Çıktı dilimleri yalnızca ile bu etkin dönem içinde oluşturulur. |Hayır<br/><br/>End özelliği için bir değer belirtirseniz, başlangıç özelliği için değer belirtmeniz gerekir.<br/><br/>Başlangıç ve bitiş saatleri hem de bir işlem hattı oluşturmak için boş olabilir. Çalıştırılacak işlem hattının etkin bir süresini ayarlamak için her iki değer belirtmeniz gerekir. Başlangıç ve bitiş zamanı belirtmezseniz, işlem hattını oluştururken, bunları daha sonra Set-AzDataFactoryPipelineActivePeriod cmdlet'ini kullanarak ayarlayabilirsiniz. |
| end | İşlem hattının son tarih-saat. Belirtilen ISO biçiminde olmalıdır. Örneğin, `2016-10-14T17:32:41Z` <br/><br/>Yerel saati, örneğin bir Tah belirtmek mümkündür. Bir örnek aşağıda verilmiştir: `2016-02-27T06:00:00-05:00`, 6 AM tahmini olduğu<br/><br/>İşlem hattını süresiz olarak çalıştırmak için 9999-09-09 son özelliğinin değeri olarak belirtin. <br/><br/> Bir işlem hattı yalnızca kendi başlangıç ve bitiş zamanı arasında etkin değil. Başlangıç zamanından önce veya sonra bitiş saati yürütülmez. İşlem hattı duraklatılmışsa, bağımsız olarak kendi başlangıç ve bitiş zamanı Yürütülmeyen. Çalıştırmak için işlem hattı için duraklatma değil. Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) zamanlama ve yürütme işleyişi Azure Data Factory'de anlamak için. |Hayır <br/><br/>Başlangıç özellik için bir değer belirtirseniz, end özelliği için değer belirtmeniz gerekir.<br/><br/>İçin Notlar'a bakın **Başlat** özelliği. |
| isPaused | TRUE olarak işlem hattı çalışmazsa. Bunu duraklatılmış durumda. Varsayılan değer = false. Bu özellik, etkinleştirme veya devre dışı bir işlem hattı kullanabilirsiniz. |Hayır |
| pipelineMode | İşlem hattı çalıştırmaları zamanlamak için yöntem. İzin verilen değerler: (varsayılan), zamanlanmış onetime.<br/><br/>'Zamanlanmış' işlem hattı, belirtilen zaman aralığı (başlangıç ve bitiş saati) etkin süresinin göre çalıştırıldığını gösterir. 'Onetime' işlem hattı yalnızca bir kez çalıştırıldığını gösterir. Tek seferlik işlem hatları oluşturulduktan sonra değişiklik ve güncelleştirilmiş olamaz. Bkz: [Onetime işlem hattı](#onetime-pipeline) onetime ayarı hakkında ayrıntılı bilgi için. |Hayır |
| expirationTime | Süre, oluşturulduktan sonra [tek seferlik işlem hattı](#onetime-pipeline) geçerli olduğunu ve sağlanan kalmalıdır. Tüm etkin, başarısız, yok veya çalıştığında, işlem hattını otomatik olarak bir kez silindi, sona erme zamanı ulaşır. Varsayılan değer: `"expirationTime": "3.00:00:00"`|Hayır |
| veri kümeleri |İşlem hattında tanımlanmış etkinlikleri tarafından kullanılacak veri kümeleri listesi. Bu özellik, bu işlem hattı için özeldir ve data factory içinde tanımlı değil, veri kümeleri tanımlamak için kullanılabilir. Bu işlem hattı tanımlanmış veri kümeleri yalnızca bu işlem hattı tarafından kullanılabilir olduğunu ve paylaşılamaz. Bkz: [veri kümeleri kapsamlı](data-factory-create-datasets.md#scoped-datasets) Ayrıntılar için. |Hayır |

## <a name="activity-json"></a>Etkinlik JSON
**Etkinlikler** bölümünde tanımlanmış bir veya daha fazla etkinlik olabilir. Her etkinlik, aşağıdaki üst düzey yapıya sahiptir:

```json
{
    "name": "ActivityName",
    "description": "description",
    "type": "<ActivityType>",
    "inputs": "[]",
    "outputs": "[]",
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
| ad | Etkinliğin adı. Etkinliğin gerçekleştirdiği eylemi temsil eden bir ad belirtin. <br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf, sayı veya alt çizgi ile başlamalıdır (\_)</li><li>Karakterler kullanılamaz: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Evet |
| açıklama | Etkinliğin ne olduğunu veya ne için kullanıldığını açıklayan metin |Evet |
| type | Etkinliğin türü. Bkz: [veri taşıma etkinlikleri](#data-movement-activities) ve [veri dönüştürme etkinlikleri](#data-transformation-activities) bölümleri farklı etkinlik türleri için. |Evet |
| girişler |Etkinlik tarafından kullanılan giriş tablosu<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Evet |
| çıkışlar |Etkinlik tarafından kullanılan çıkış tablolar.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Evet |
| linkedServiceName |Etkinlik tarafından kullanılan bağlı hizmetin adı. <br/><br/>Bir etkinlik için gerekli işlem ortamına bağlanan bağlı hizmeti belirtmeniz gerekebilir. |HDInsight etkinliği ve Azure Machine Learning toplu işlem Puanlandırma etkinliği için Evet <br/><br/>Diğer tümü için hayır |
| typeProperties |Özelliklerinde **typeProperties** bölümü etkinlik türüne bağlıdır. Bir etkinliğin tür özelliklerini görmek için önceki bölümde verilen etkinlik bağlantılarına tıklayın. | Hayır |
| ilke |Etkinliğin çalışma zamanı davranışını etkileyen ilkeler. Belirtilmezse, varsayılan ilkeler kullanılır. |Hayır |
| Zamanlayıcı | "Zamanlayıcı" özelliği, istenen etkinlik için zamanlama tanımlamak için kullanılır. Onun alt dışındaki aynıdır [bir veri kümesi kullanılabilirlik özelliğinde](data-factory-create-datasets.md#dataset-availability). |Hayır |

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
* **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir. İçinde [veri taşıma etkinlikleri](#data-movement-activities) bölümü, veri depolama, veri deposundan/veri taşıma hakkında daha fazla bilgi için bir kaynak veya havuz kullanmak istediğiniz tıklatın.

Bu işlem hattını oluşturmak üzere izlenecek tam yol için bkz: [Öğreticisi: Blob depolama alanından SQL veritabanı'na veri kopyalamak](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

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
* `defines` Bölümü, hive betiğine Hive yapılandırma değerleri olarak geçirilen çalışma zamanı ayarlarını belirtmek için kullanılır (ör. `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

**TypeProperties** bölümü her bir dönüştürme etkinliği için farklıdır. İçindeki bir dönüştürme etkinliği için desteklenen tür özellikleri hakkında bilgi edinmek için tıklayın [veri dönüştürme etkinlikleri](#data-transformation-activities) tablo.

Bu işlem hattını oluşturmak üzere izlenecek tam yol için bkz: [Öğreticisi: Hadoop kümesi kullanarak verileri işlemek için ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md).

## <a name="multiple-activities-in-a-pipeline"></a>Bir işlem hattında birden çok etkinlik
Önceki iki örnekte işlem hatları yalnızca bir etkinlik içeriyordu. Bir işlem hattında birden fazla etkinliğiniz olabilir.

Etkinlikler için giriş veri dilimi hazır olup olmadığını bir işlem hattında birden fazla etkinlik varsa ve bir etkinliğin çıktısını başka bir etkinliğin girdi, etkinlikler paralel olarak çalışabilir.

Çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak bir etkinlik sağlayarak iki etkinliği zincirleyebilirsiniz. İkinci etkinlik, yalnızca ilki başarıyla tamamlandığında yürütür.

![Aynı işlem hattındaki etkinlikler, zincirleme](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Bu örnekte, işlem hattı iki etkinlik içerir: Activity1 ve Activity2. Activity1 Dataset1 girdi olarak alır ve bir çıkış üretir Dataset2. Etkinlik Dataset2 girdi olarak alır ve bir çıkış üretir Dataset3. Activity1 çıktısını beri (Dataset2) olan Activity2, etkinlik başarıyla tamamlandıktan ve Dataset2 dilim oluşturur sonra Activity2 çalıştırmalar girişi. Activity1 herhangi bir nedenden dolayı başarısız olur ve Dataset2 dilim üretmez, bu dilimle ilgili etkinlik 2 çalışmaz (örneğin: 9 10'da AM).

Ayrıca, farklı işlem hatlarında etkinlikleri zincirleyebilirsiniz.

![Zincirleme iki işlem hattı içindeki etkinlikleri](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Bu örnekte, Pipeline1 Dataset1 girdi olarak alır ve çıktı olarak Dataset2 üretir, yalnızca bir etkinlik içerir. Pipeline2 de girdi ve çıktı olarak Dataset3 Dataset2 alan yalnızca bir etkinlik vardır.

Daha fazla bilgi için [zamanlama ve yürütme](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).
## <a name="create-and-monitor-pipelines"></a>Oluşturma ve işlem hatlarını izleme
Bu araçlar ve SDK'lar birini kullanarak işlem hatları oluşturabilirsiniz.

- Kopyalama Sihirbazı'nı.
- Azure portal
- Visual Studio
- Azure PowerShell
- Azure Resource Manager şablonu
- REST API
- .NET API’si

Bu araçlar ve SDK'lar birini kullanarak işlem hatları oluşturmaya yönelik adım adım yönergeler için aşağıdaki öğreticilere bakın.

- [Veri dönüştürme etkinliğine sahip işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)
- [Veri taşıma etkinliği ile işlem hattı oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Oluşturulan ve dağıtılan bir işlem hattı olduktan sonra yönetmek ve Azure portalı dikey pencerelerinin veya izleme ve yönetme uygulaması kullanılarak işlem hatlarınızı izlemek. Adım adım yönergeler için aşağıdaki konulara bakın.

- [Azure portal dikey penceresi kullanılarak işlem hatlarını yönetmek ve izlemek](data-factory-monitor-manage-pipelines.md).
- [İzleme ve işlem hatlarını izleme ve yönetme uygulaması'nı kullanarak yönetme](data-factory-monitor-manage-app.md)

## <a name="onetime-pipeline"></a>Tek seferlik işlem hattı
Oluşturma ve zamanlama düzenli aralıklarla çalıştırmak için bir işlem hattı (örneğin: saatlik veya günlük) işlem hattı tanımında belirttiğiniz başlangıç ve bitiş saatleri içinde. Zamanlama etkinlikleri Ayrıntılar için bkz. Ayrıca, yalnızca bir kez çalışan bir işlem hattı oluşturabilirsiniz. Bunu yapmak için ayarladığınız **pipelineMode** özelliği için işlem hattı tanımındaki **onetime** aşağıdaki JSON örnekte gösterildiği gibi. Bu özellik için varsayılan değer **zamanlanmış**.

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
                ],
                "name": "CopyActivity-0"
            }
        ],
        "pipelineMode": "OneTime"
    }
}
```

Şunlara dikkat edin:

* **Başlangıç** ve **son** işlem hattının zamanları belirtilmedi.
* **Kullanılabilirlik** giriş ve çıkış veri kümeleri belirtilen (**sıklığı** ve **aralığı**), Data Factory değerleri kullanmıyor olsa bile.
* Diyagram görünümü, tek seferlik işlem hatları göstermez. Bu davranış tasarım gereğidir.
* Tek seferlik işlem hatları güncelleştirilemiyor. Tek seferlik bir işlem hattı kopyalama, yeniden adlandırmak, özellikleri güncelleştirmek ve başka bir tane oluşturun dağıtalım.

## <a name="next-steps"></a>Sonraki adımlar
- Veri kümeleri hakkında daha fazla bilgi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi.
- İşlem hatlarını nasıl zamanlanmış ve yürütülen hakkında daha fazla bilgi için bkz. [zamanlama ve yürütme Azure Data factory'de](data-factory-scheduling-and-execution.md) makalesi.
