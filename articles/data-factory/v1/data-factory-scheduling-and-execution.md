---
title: "Zamanlama ve yürütme Data Factory ile | Microsoft Docs"
description: "Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: a729b38b2fc0b8ef759037976753e0030557a6fa
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="data-factory-scheduling-and-execution"></a>Veri Fabrikası zamanlama ve yürütme
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [kanal yürütme ve Tetikleyicileri](../concepts-pipeline-execution-triggers.md) makalesi.

Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. Bu makalede, etkinlik, ardışık düzen, bağlı hizmetler ve veri kümeleri dahil olmak üzere, veri fabrikası uygulama modeli kavramlarını temelleri anladığınızı varsayar. Azure Data Factory temel kavramları aşağıdaki makalelere bakın:

* [Data Factory'ye giriş](data-factory-introduction.md)
* [İşlem hatları](data-factory-create-pipelines.md)
* [Veri kümeleri](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Başlangıç ve bitiş zamanlarını ardışık
İşlem hattı yalnızca arasında etkin olduğu kendi **Başlat** zaman ve **son** saat. Bu başlangıç saatinden önce veya sonra bitiş saati yürütülmedi. Ardışık Düzen duraklatıldığında, kendi başlangıç ve bitiş zamanı bağımsız olarak yürütülemiyor. Çalıştırmak ardışık düzeni için bu altyapının duraklatılması değil. Bu ayarları (Başlangıç, son, duraklatıldı) ardışık düzen tanımı'nda bulunamıyor: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Bu özellikleri daha fazla bilgi için bkz: [işlem hatlarını oluşturmak](data-factory-create-pipelines.md) makalesi. 


## <a name="specify-schedule-for-an-activity"></a>Bir etkinlik için zamanlamayı belirtin
Yürütülen ardışık düzen değil. Ardışık Düzen genel bağlamda yürütülen ardışık düzendeki etkinlik olur. Kullanarak bir etkinlik için yinelenen bir zamanlama belirtebilirsiniz **Zamanlayıcı** JSON etkinliği bölümü. Örneğin, saatlik gibi çalıştırmak için bir etkinlik zamanlayabilirsiniz:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Aşağıdaki çizimde gösterildiği gibi bir dizi etkinlik için bir zamanlama oluşturur belirtme ardışık düzen başlangıç windows ile dönen ve bitiş saatlerini. Dönen windows sabit boyutlu çakışmayan, bitişik zaman aralıkları bir dizi var. Bir etkinlik için bu mantıksal dönen windows adlı **etkinlik windows**.

![Etkinlik Zamanlayıcısı örneği](media/data-factory-scheduling-and-execution/scheduler-example.png)

**Zamanlayıcı** özelliği bir etkinlik için isteğe bağlı. Bu özelliği belirtirseniz, etkinliğin çıktı veri kümesi tanımında belirttiğiniz tempoyla eşleşmesi gerekir. Şu anda zamanlamayı çıktı veri kümesi yürütmektedir. Bu nedenle, etkinlik herhangi bir çıktı üretmez olsa bile bir çıkış veri kümesi oluşturmanız gerekir. 

## <a name="specify-schedule-for-a-dataset"></a>Bir veri kümesi için zamanlamayı belirtin
Data Factory işlem hattı bir etkinlikte sıfır veya daha fazla giriş alabilir **veri kümeleri** ve bir veya daha fazla çıkış veri kümeleri oluşturma. Bir etkinlik için hangi giriş verileri mevcut değil veya çıktı verilerini kullanarak üretilen tempoyla belirtebilirsiniz **kullanılabilirlik** dataset tanımları bölümünde. 

**Sıklık** içinde **kullanılabilirlik** bölümü zaman birimini belirtir. Sıklık için izin verilen değerler: dakika, saat, gün, hafta ve ay. **Aralığı** Özellik kullanılabilirliği bölümünde sıklığı çarpanı belirtir. Örneğin: sıklığı gün olarak ayarlanır ve aralığı bir çıkış veri kümesi için 1 olarak ayarlanmış, çıktı verilerini günlük üretilir. Sıklığını dakika belirtirseniz, aralık en az 15 olarak ayarlamanızı öneririz. 

Aşağıdaki örnekte, girdi verileri saatlik kullanılabilir ve çıktı verilerini saatlik üretilen (`"frequency": "Hour", "interval": 1`). 

**Girdi veri kümesi:** 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


**Çıktı veri kümesi**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Şu anda **çıktı veri kümesi zamanlamayı sürücüleri**. Diğer bir deyişle, çıktı veri kümesi için belirtilen zamanlaması, çalışma zamanında bir aktivite çalıştırmak için kullanılır. Bu nedenle, etkinlik herhangi bir çıktı üretmez olsa bile bir çıkış veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. 

Aşağıdaki ardışık düzen tanımında **Zamanlayıcı** özelliği etkinliğinin zamanlama belirtmek için kullanılır. Bu özellik isteğe bağlıdır. Şu anda, zamanlama etkinliğinin çıkış veri kümesi için belirtilen zamanlaması eşleşmesi gerekir.
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

Bu örnekte, etkinlik saatlik başlangıç ve bitiş zamanlarını ardışık arasında çalışır. Çıktı verilerini saatlik üç saatlik windows (8: 00 - 9'da, 09: 00 - 10'da ve 10: 00 - 11: 00) oluşturulur. 

Her birim tüketilen veya bir etkinlik tarafından üretilen verilerin adlı bir **veri dilimi**. Aşağıdaki diyagramda bir giriş veri kümesi olmayan bir etkinliği örneği gösterir ve bir çıkış veri kümesi: 

![Kullanılabilirlik Zamanlayıcı](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Diyagram girdi ve çıktı veri kümesi için saatlik veri dilimlerinin gösterir. Diyagram işlenmeye hazır üç girdi dilimlerinin gösterir. 10-11 AM etkinlik 10-11 AM çıktı diliminin oluşturan devam ediyor. 

Değişkenleri kullanılarak JSON veri kümesi geçerli dilimi ilişkili zaman aralığı erişebilirsiniz: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) ve [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). Benzer şekilde, bir etkinlik penceresiyle WindowStart ve WindowEnd kullanarak ilişkili zaman aralığı erişebilir. Bir etkinlik zamanlamasını etkinliğinin çıkış veri kümesi zamanlamayı eşleşmesi gerekir. Bu nedenle, SliceStart ve SliceEnd değerleri WindowStart ve WindowEnd değerleri sırasıyla aynıdır. Bu değişkenleri hakkında daha fazla bilgi için bkz: [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md#data-factory-system-variables) makaleleri.  

Etkinlik JSON farklı amaçlar için bu değişkenleri kullanabilirsiniz. Örneğin, bunları zaman serisi verilerini temsil eden giriş ve çıkış veri kümeleri verileri seçmek için kullanabileceğiniz (örneğin: 8: 00 için 09: 00). Bu örnek ayrıca kullanır **WindowStart** ve **WindowEnd** ilgili seçmek için bir etkinliğin verilerini Çalıştır'ı ve uygun bir blob kopyalayın **folderPath**. **FolderPath** her saat için ayrı bir klasör için parametreli.  

Önceki örnekte, zamanlama için giriş belirtilen ve çıkış veri kümeleri (saatlik) aynı değil. Etkinliğinin girdi veri kümesi deyin 15 dakikada bir farklı bir sıklık, varsa, çıktı veri kümesi ne etkinlik zamanlaması sürücüleri olduğundan bu çıkış veri kümesini üreten etkinlik hala saatte bir kez çalışır. Daha fazla bilgi için bkz: [modeli farklı sıklıklarını veri kümeleriyle](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>DataSet kullanılabilirliği ve ilkeleri
Kullanım sıklığı ve veri kümesi tanımı kullanılabilirlik bölümünü aralığı özelliklerinde gördünüz. Diğer birkaç planlama etkileyen özellikler ve etkinlik yürütülmesini vardır. 

### <a name="dataset-availability"></a>DataSet kullanılabilirliği 
Aşağıdaki tabloda, kullanabileceğiniz özellikleri açıklanmaktadır **kullanılabilirlik** bölümü:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilim üretim için zaman birimini belirtir.<br/><br/><b>Sıklık desteklenen</b>: dakika, saat, gün, hafta, ay |Evet |NA |
| interval |Sıklığı çarpanı belirtir<br/><br/>"X sıklığı aralığını" ne sıklıkta dilim üretilen belirler.<br/><br/>Saatlik olarak başka bir dilimlenebilir dataset gerekiyorsa, ayarladığınız <b>sıklığı</b> için <b>saat</b>, ve <b>aralığı</b> için <b>1</b>.<br/><br/><b>Not</b>: sıklığını dakika belirtirseniz, aralık en az 15'e ayarlayın öneririz |Evet |NA |
| stili |Dilim aralığı başlangıç/bitiş üretilen olup olmadığını belirtir.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Sıklık aya ayarlanır ve stil EndOfInterval için dilim ayın son gününde üretilmez. Stil StartOfInterval için ayarlarsanız, dilim ayın ilk günü oluşturulur.<br/><br/>Sıklığı gün olarak ayarlanır ve stili için EndOfInterval dilim günün son bir saatte üretilmez.<br/><br/>Sıklık saate ayarlanır ve stil EndOfInterval için dilim saat sonunda üretilmez. Örneğin, 13'te – 2 PM dönem için bir dilim için 2 saat dilimi oluşturulur. |Hayır |EndOfInterval |
| anchorDateTime |Veri kümesi dilim sınırlarını işlem için Zamanlayıcı tarafından kullanılan zaman içinde mutlak konum tanımlar. <br/><br/><b>Not</b>: AnchorDateTime sıklığından daha ayrıntılı tarih bölümden oluşur sonra daha ayrıntılı bölümleri göz ardı edilir. <br/><br/>Örneğin, varsa <b>aralığı</b> olan <b>saatlik</b> (sıklığı: saat ve aralığı: 1) ve <b>AnchorDateTime</b> içeren <b>dakika ve saniyeleri</b>, sonra <b>dakika ve saniyeleri</b> AnchorDateTime bölümlerini yok sayılır. |Hayır |01/01/0001 |
| uzaklık |Tarafından başlangıç ve bitiş tüm veri kümesi dilim gölgeye Timespan. <br/><br/><b>Not</b>: anchorDateTime ve uzaklık belirtilirse, birleştirilmiş shift sonucudur. |Hayır |NA |

### <a name="offset-example"></a>uzaklık örneği
Varsayılan olarak, her gün (`"frequency": "Day", "interval": 1`) dilimler 00: 00 UTC zaman (gece yarısı) başlatın. Başlangıç zamanı 6 AM UTC saati yerine olmasını istiyorsanız, uzaklık aşağıdaki kod parçacığında gösterildiği gibi ayarlayın: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime örneği
Aşağıdaki örnekte, veri kümesi 23 saatte bir kez oluşturulur. İlk dilim ayarlanır anchorDateTime tarafından belirtilen zaman başlar `2017-04-19T08:00:00` (UTC saati).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>uzaklık/örnek stili
Aşağıdaki veri kümesi aylık bir veri kümesi ve 8: 00'da her ayın 3 üretilir (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>Veri kümesi İlkesi
Bir veri kümesi kullanıma hazır hale gelmeden önce bir dilim yürütme tarafından oluşturulan verilerin nasıl doğrulanabilir belirten bir doğrulama ilkesi tanımlı olabilir. Dilim yürütme sona erdikten sonra bu gibi durumlarda çıktı dilim durumu olarak değiştirildiğinde **bekleyen** substatus ile **doğrulama**. Dilimler doğrulandıktan sonra dilim durum değişikliklerini **hazır**. Veri dilimi üretilen, ancak doğrulamayı geçemedi etkinlik çalışması için bu dilimine bağlıdır aşağı akış dilimleri işlenmez. [İzleme ve ardışık düzen yönetme](data-factory-monitor-manage-pipelines.md) veri fabrikasında veri dilimleri çeşitli durumlarını kapsar.

**İlkesi** veri kümesi tanımı bölümünde ölçütleri veya veri kümesi dilimler karşılamanız gerekmektedir koşulu tanımlar. Aşağıdaki tabloda, kullanabileceğiniz özellikleri açıklanmaktadır **İlkesi** bölümü:

| İlke Adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Doğrular verilerde bir **Azure blob** (megabayt cinsinden) en düşük boyut gereksinimlerini karşılıyor. |Azure Blob |Hayır |NA |
| minimumRows | Doğrular verilerde bir **Azure SQL veritabanı** veya bir **Azure tablo** en az satır sayısını içerir. |<ul><li>Azure SQL Database</li><li>Azure Tablosu</li></ul> |Hayır |NA |

#### <a name="examples"></a>Örnekler
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

Bu özellikler ve örnekler hakkında daha fazla bilgi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. 

## <a name="activity-policies"></a>Etkinlik ilkeleri
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

Daha fazla bilgi için bkz: [ardışık düzen](data-factory-create-pipelines.md) makalesi. 

## <a name="parallel-processing-of-data-slices"></a>Veri dilimi paralel işlenmesi
Ardışık düzeni için başlangıç tarihi geçmişte ayarlayabilirsiniz. Bunu yaptığınızda, veri fabrikası otomatik olarak (arka dolgu) tüm veri dilimleri geçmişte hesaplar ve bunları işlemeye başlar. Örneğin: 2017-04-01 başlangıç tarihi ile işlem hattı oluşturma ve 2017-04-10 geçerli tarihidir. Çıktı veri kümesi tempoyla günlük, veri fabrikası başlatır 2017-04-01 gelen tüm dilimleri 2017-04-09 için işleme ise hemen başlangıç tarihi geçmişte olduğundan. Kullanılabilirlik bölümünde stil özelliğinin değeri EndOfInterval olduğundan 2017-04-10 dilimden varsayılan henüz işlenmedi. Eski dilim işlenen önce varsayılan olarak executionPriorityOrder OldestFirst değeri. Stil özelliği açıklaması için bkz: [dataset kullanılabilirliği](#dataset-availability) bölümü. ExecutionPriorityOrder bölüm açıklaması için bkz: [etkinlik ilkeleri](#activity-policies) bölümü. 

Paralel olarak ayarlayarak işlenmek üzere geri doldurulmuş veri dilimleri yapılandırabilirsiniz **eşzamanlılık** özelliğinde **İlkesi** JSON etkinliği bölümü. Bu özellik üzerinde farklı dilimler oluşabilir paralel etkinlik yürütmeleri sayısını belirler. Eşzamanlılık özelliği için varsayılan değer 1'dir. Bu nedenle, bir dilim aynı anda varsayılan olarak işlenir. En büyük değer 10'dur. Kullanılabilir veri, daha büyük bir eşzamanlılık değer sahip büyük bir dizi ile gitmek bir işlem hattı gerektiği zaman veri işleme hızı artar. 

## <a name="rerun-a-failed-data-slice"></a>Başarısız olan veri dilimi yeniden çalıştırın
Veri dilimi işlenirken bir hata ortaya çıkarsa, Azure portal dikey penceresi veya izleme ve yönetme uygulaması'nı kullanarak bir dilim işlenmesini neden başarısız anlamak bulabilirsiniz. Bkz: [izleme ve Azure portal dikey penceresi kullanılarak işlem hatlarını yönetme](data-factory-monitor-manage-pipelines.md) veya [izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md) Ayrıntılar için.

İki etkinlik gösteren aşağıdaki örnekte, göz önünde bulundurun. Activity1 ve aktivite 2. Activity1 Dataset1 dilimin kullanır ve bir giriş olarak bir dilim son veri kümesi oluşturmak için Activity2 tarafından tüketilen Dataset2 dilimin üretir.

![Başarısız dilim](./media/data-factory-scheduling-and-execution/failed-slice.png)

Diyagram, 9-10'da dilim için Dataset2 oluşturan bir hata oluştu, üç son dilim dışında gösterir. Veri Fabrikası bağımlılık zaman serisi veri kümesi için otomatik olarak izler. Sonuç olarak, etkinliğin 9-10'da aşağı akış dilimi için çalışma başlatılmaz.

Data Factory izleme ve Yönetim Araçları, kolayca kök nedeni sorunu bulmak ve düzeltmek başarısız dilim için tanılama günlüklerini incelemek izin verin. Sorunu düzelttikten sonra etkinliğin başarısız dilim üretmek için çalışma kolayca başlatabilirsiniz. Yeniden çalıştırın ve veri dilimi için durumu geçişleri anlama konusunda daha fazla bilgi için bkz: [izleme ve Azure portal dikey penceresi kullanılarak işlem hatlarını yönetme](data-factory-monitor-manage-pipelines.md) veya [izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md).

9-10'da dilimin yeniden sonra **Dataset2**, veri fabrikası son veri kümesine 9-10'da bağımlı dilimi için çalışma başlatır.

![Başarısız dilimi yeniden çalıştırın](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Bir işlem hattında birden çok etkinlik
Bir işlem hattında birden fazla etkinliğiniz olabilir. Etkinlikler için giriş veri dilimi hazır olduğunuzda bir ardışık düzende birden çok etkinliği varsa ve bir etkinlik çıktısını başka bir etkinliğin bir giriş değil, etkinlikleri paralel olarak çalışabilir.

Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Etkinlikler aynı ardışık düzen veya farklı bir ardışık düzen olabilir. İkinci etkinlik, yalnızca ilk başarıyla tamamlandığında yürütür.

Örneğin, bir işlem hattı iki etkinlik olduğu aşağıdaki durum göz önünde bulundurun:

1. Dış giriş veri kümesi D1 ve D2 üretir çıktı veri kümesi gerektiren etkinlik A1.
2. Çıkış dataset D3 D2 kümesinden giriş gerektirir ve üreten etkinlik A2.

Bu senaryoda, A1 ve A2 aynı ardışık düzeninde etkinliklerdir. Etkinliği, dış veriler kullanılabilir olduğunda ve zamanlanan kullanılabilirlik sıklığı ulaşıldıktan A1 çalıştırır. Etkinlik A2 D2 gelen zamanlanmış dilimler kullanılabilir hale gelir ve zamanlanan kullanılabilirlik sıklığı sınırına başlatıldığında çalışır. Veri kümesi D2 dilimleri birinde bir hata varsa, kullanılabilir oluncaya kadar A2 bu dilim için çalışmaz.

Diyagram görünümü ile aynı ardışık düzende iki etkinlik, aşağıdaki diyagramda gibi görünür:

![Aynı ardışık düzendeki etkinlik zincirleme](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Daha önce belirtildiği gibi etkinlikler farklı ardışık düzenlerinde olabilir. Böyle bir senaryoda, diyagram görünümünü Aşağıdaki diyagramda gibi görünür:

![İki ardışık düzende zincirleme etkinlikleri](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Bkz: [sırayla kopyalamak](#copy-sequentially) bir örnek için ek bölümünde.

## <a name="model-datasets-with-different-frequencies"></a>Farklı sıklıklarını modeli kümeleriyle
Örnekleri için girdi ve çıktı veri kümelerini ve etkinlik zamanlama penceresi sıklıkları aynı yoktu. Bazı senaryolar sıklıkla bir veya daha fazla girdi sıklık farklı bir çıkış üretemeyecek yeteneği gerektirir. Veri Fabrikası bu senaryo modelleme destekler.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Örnek 1: her saat kullanılabilir giriş verileri için bir günlük çıkışı raporu oluşturmak
Azure Blob storage'da her saat algılayıcılar kullanılabilir ölçüm verileri Giriş bir senaryo düşünün. İle gün için ortalama, maksimum ve minimum gibi istatistikleri ile günlük toplama bir rapor oluşturmak için istediğiniz [Data Factory hive etkinliği](data-factory-hive-activity.md).

Bu senaryo Data Factory ile nasıl model aşağıda verilmiştir:

**Girdi veri kümesi**

Saatlik girdi dosyaları için belirli gün klasöründe bırakılır. Giriş için kullanılabilirlik ayarlanırsa **saat** (sıklığı: saat, aralığı: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Çıktı veri kümesi**

Bir çıkış dosyası her gün günün klasöründe oluşturulur. Çıktı kullanılabilirliğini ayarlanırsa **gün** (sıklığı: gün ve aralığı: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Etkinlik: ardışık düzeninde hive etkinliği**

Hive betiği uygun alan *DateTime* bilgileri kullandığınız parametre olarak **WindowStart** aşağıdaki kod parçacığında gösterildiği gibi değişkeni. Hive betiği, verileri güne ait doğru klasörden yüklemek ve çıktı üretmek için toplama çalıştırmak için bu değişkeni kullanır.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
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
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

Aşağıdaki diyagramda açısından bir veri bağımlılığı senaryosu gösterilmiştir.

![Veri bağımlılığı](./media/data-factory-scheduling-and-execution/data-dependency.png)

Bir giriş veri kümesinden 24 saat dilimi her gün için çıktı diliminin bağlıdır. Veri Fabrikası üretilecek çıktı diliminin aynı saat diliminde kalan dilimler giriş verileri hesaplayarak Bu bağımlılıklar otomatik olarak hesaplar. Herhangi bir 24 girdi dilimlerinin yoksa, veri fabrikası girdi dilimi günlük etkinliğin çalışma başlatmadan önce hazır olmasını bekler.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Örnek 2: bağımlılık, ifadeler ve Data Factory işlevleri ile belirtin.
Şirketinizdeki başka bir senaryo düşünün. İki giriş veri kümesi işleyen bir hive etkinliği olduğunu varsayalım. Bunlardan birini yeni veri günlük olsa da, bunlardan biri her hafta yeni verileri alır. İki girdi arasında bir birleştirme yapın ve her gün bir çıktı oluşturmak istediğinizi varsayalım.

Basit bir yaklaşım hangi veri fabrikası'nda otomatik olarak veri dilimin zaman dönemi çalışmıyor çıkış hizalayarak işlemek için dilimler sağa çıkışı rakamları girin.

Her etkinlik için veri fabrikası Geçen haftaki veri dilimi haftalık giriş veri kümesi için kullanması gerektiğini belirtmeniz gerekir. Azure Data Factory işlevleri aşağıdaki kod parçacığında gösterildiği gibi bu davranış uygulamak için kullanın.

**Input1: Azure blob**

İlk giriş günlük güncelleştirilmekte Azure blob olabilir.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Input2: Azure blob**

Input2 haftalık güncelleştirilmekte Azure blob ' dir.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**Çıkış: Azure blob**

Bir çıkış dosyası her gün için günün klasöründe oluşturulur. Çıktı kullanılabilirliğini ayarlanmış **gün** (sıklığı: Day, aralığı: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Etkinlik: ardışık düzeninde hive etkinliği**

Hive etkinliği iki girdi alır ve her gün bir çıktı diliminin üretir. Önceki haftanın girdi dilimi haftalık girişi için aşağıdaki gibi bağımlı için her günün çıktı diliminin belirtebilirsiniz.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

Bkz: [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md) işlevler ve Data Factory destekleyen sistem değişkenleri listesi.

## <a name="appendix"></a>Ek

### <a name="example-copy-sequentially"></a>Örnek: sıralı olarak Kopyala
Birden çok kopyalama işlemleri birbiri ardından sıralı/sıralı bir şekilde çalıştırmak mümkündür. Örneğin, aşağıdaki giriş verileri çıktı veri kümeleriyle (CopyActivity1 ve CopyActivity2) ardışık düzende iki kopya etkinlik olabilir:   

CopyActivity1

Giriş: veri kümesi. Çıkış: Dataset2.

CopyActivity2

Giriş: Dataset2.  Çıkış: Dataset3.

CopyActivity2 yalnızca CopyActivity1 başarıyla çalıştırıldı ve Dataset2 kullanılabilir çalışır.

JSON örnek ardışık düzeni şöyledir:

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

Örnekte, ikinci etkinlik için giriş olarak çıkış veri kümesi ilk kopyalama etkinliği (Dataset2) belirtilen dikkat edin. Bu nedenle, yalnızca ilk etkinliğin çıktı veri kümesi hazır olduğunda ikinci etkinlik çalışır.  

Örnekte, CopyActivity2 Dataset3 gibi farklı bir giriş olabilir ancak CopyActivity1 tamamlanana kadar etkinlik çalışmaz şekilde, Dataset2 CopyActivity2, girdi olarak belirtin. Örneğin:

CopyActivity1

Giriş: Dataset1. Çıkış: Dataset2.

CopyActivity2

Girişler: Dataset3, Dataset2. Çıkış: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

İkinci kopya etkinliği için iki giriş veri kümesi örnek belirtilen dikkat edin. Birden çok girişi belirtildiğinde, yalnızca ilk girdi veri kümesi veri kopyalamak için kullanılır, ancak diğer veri kümeleri bağımlılıklar olarak kullanılır. Yalnızca aşağıdaki koşullar sonra CopyActivity2 başlatmak:

* CopyActivity1 başarıyla tamamlandı ve Dataset2 kullanılabilir. Bu veri kümesi, veri Dataset4 için kopyalarken kullanılmaz. Yalnızca CopyActivity2 için zamanlama bağımlılık olarak davranır.   
* Dataset3 kullanılabilir. Bu veri kümesi için hedef kopyalanan verileri temsil eder. 