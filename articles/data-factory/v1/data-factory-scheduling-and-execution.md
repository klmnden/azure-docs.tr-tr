---
title: Zamanlama ve yürütme Data Factory ile | Microsoft Docs
description: Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2d7fc45faf1fb77c7d9181e5a2419096dd1ad0f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61258987"
---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory zamanlama ve yürütme
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [işlem hattı yürütme ve Tetikleyicileri](../concepts-pipeline-execution-triggers.md) makalesi.

Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. Bu makale, Data Factory uygulama modelinin kavramları, etkinlik, işlem hatları, bağlı hizmetler ve veri kümeleri dahil olmak üzere temellerini anladığınızı varsayar. Azure Data Factory temel kavramlarını için aşağıdaki makalelere bakın:

* [Data Factory'ye giriş](data-factory-introduction.md)
* [İşlem hatları](data-factory-create-pipelines.md)
* [Veri kümeleri](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>İşlem hattının başlangıç ve bitiş saatleri
Bir işlem hattı yalnızca arasında etkindir, **Başlat** zaman ve **son** zaman. Başlangıç zamanından önce veya sonra bitiş saati yürütülmez. İşlem hattı duraklatılmışsa, bağımsız olarak kendi başlangıç ve bitiş zamanı yürütülmez. Çalıştırmak için işlem hattı için duraklatma değil. İşlem hattı tanımındaki'de bu ayarları (Başlangıç, bitiş, duraklatıldı) bulun: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Daha fazla bilgi için bkz. Bu özellikler [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. 


## <a name="specify-schedule-for-an-activity"></a>Bir etkinlik zamanlamasını belirtin
Yürütülen işlem hattı değil. Bu işlem hattının genel bağlamda yürütülen işlem hattındaki etkinlikler, olur. Kullanarak bir etkinlik için yinelenen bir zamanlama belirtebilirsiniz **Zamanlayıcı** etkinlik JSON bölümü. Örneğin, bir etkinlik gibi saatte bir çalışacak şekilde zamanlayabilirsiniz:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Aşağıdaki diyagramda gösterildiği gibi belirten bir dizi etkinlik için bir zamanlama oluşturur ile windows atlayan işlem hattı başlangıç ve bitiş saatlerini. Atlayan pencereler sabit boyutlu, çakışmayan, bitişik zaman aralıkları dizisidir. Bir etkinlik için bu mantıksal atlayan pencereleri denmektedir **etkinlik pencereleri**.

![Etkinlik Zamanlayıcı örneği](media/data-factory-scheduling-and-execution/scheduler-example.png)

**Zamanlayıcı** özelliği için bir etkinlik, isteğe bağlıdır. Bu özelliği belirtirseniz, etkinliğin çıkış veri kümesi tanımında belirttiğiniz temposu eşleşmesi gerekir. Şu anda zamanlamayı çıktı veri kümesi yürütmektedir. Bu nedenle etkinlik hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi oluşturmanız gerekir. 

## <a name="specify-schedule-for-a-dataset"></a>Bir veri kümesi için zamanlamayı belirtin
Data Factory işlem hattı bir etkinlik giriş sıfır veya daha fazla sürebilir **veri kümeleri** ve bir veya daha fazla çıkış veri kümesi üretir. Bir etkinlik için giriş verilerinin kullanılabilir veya çıktı verilerini kullanılarak üretilen temposu belirtebilirsiniz **kullanılabilirlik** bölümünde veri kümesini tanımlar. 

**Sıklık** içinde **kullanılabilirlik** bölümü zaman birimini belirtir. Sıklığı için izin verilen değerler şunlardır: Dakika, saat, gün, haftalık ve aylık. **Aralığı** kullanılabilirlik bölümünü özelliğinde sıklığı çarpanı belirtir. Örneğin: sıklığı gün olarak ayarlanır, aralıksa 1 için bir çıktı veri kümesi ise çıktı verilerini günlük oluşturulur. Sıklığını dakika belirtirseniz, aralığı en az 15'e ayarlamak öneririz. 

Aşağıdaki örnekte, girdi verilerini saatlik kullanılabilir ve çıktı verilerini saatlik olarak üretilen (`"frequency": "Hour", "interval": 1`). 

**Giriş veri kümesi:** 

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

Şu anda **çıktı veri kümesi zamanlamayı sürücüleri**. Diğer bir deyişle, çıktı veri kümesi için belirtilen zamanlaması, çalışma zamanında bir aktivite çalıştırmak için kullanılır. Bu nedenle etkinlik hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. 

Aşağıdaki işlem hattı tanımındaki **Zamanlayıcı** özelliği etkinliği için bir zamanlama belirtmek için kullanılır. Bu özellik isteğe bağlıdır. Şu anda, zamanlama etkinliğinin çıkış veri kümesi için belirtilen zamanlaması eşleşmesi gerekir.
 
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

Bu örnekte, etkinlik, saatlik başlangıç ve bitiş zamanları işlem hattının arasında çalıştırır. Çıktı verilerini, üç saatlik windows (8: 00 - 9'da, 09: 00 - 10: 00 ve 10: 00 - 11: 00) için saatlik oluşturulur. 

Her bir etkinlik tarafından üretilen veya tüketilen veri birimi olarak adlandırılan bir **veri dilimi**. Aşağıdaki diyagramda bir etkinliği bir girdi veri kümesi örneği gösterilmektedir ve bir çıkış veri kümesi: 

![Kullanılabilirlik Zamanlayıcı](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Girdi ve çıktı veri kümesinin saatlik veri dilimleri diyagramda gösterilmektedir. İşlem için hazır olan üç giriş dilimleri diyagramda gösterilmektedir. 10-11 AM etkinliği 10-11 AM çıktı dilimi üreten, devam ediyor. 

JSON veri kümesi geçerli dilimi değişkenlerini kullanarak ilişkili zaman aralığını erişebilirsiniz: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) ve [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). Benzer şekilde, zaman aralığını WindowEnd ve WindowStart kullanarak bir etkinlik penceresiyle ilişkilendirilmiş erişebilirsiniz. Bir etkinlik zamanlaması, etkinliğin çıkış veri kümesi zamanlamayı eşleşmesi gerekir. Bu nedenle, SliceStart ve SliceEnd değerleri WindowStart ve WindowEnd değerlerini sırasıyla aynıdır. Bu değişkenler hakkında daha fazla bilgi için bkz. [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md#data-factory-system-variables) makaleler.  

Etkinlik JSON farklı amaçlar için bu değişkenleri kullanabilirsiniz. Örneğin, bunları zaman serisi verilerini temsil eden girdi ve çıktı veri kümeleri verileri seçmek için kullanabileceğiniz (örneğin: 8 9'da AM). Bu örnekte ayrıca **WindowStart** ve **WindowEnd** ilgili seçmek için bir etkinliğin veri çalıştırın ve uygun bir bloba kopyalama **folderPath**. **FolderPath** her saat için ayrı bir klasör için parametreli.  

Önceki örnekte, giriş için zamanlama belirtilen ve çıkış veri kümeleri (saatlik) aynı olduğu. Etkinliğin giriş veri kümesi Örneğin 15 dakikada farklı bir sıklıkta varsa, çıktı veri kümesi olduğundan etkinlik zamanlamayı yönetendir; Bu çıktı veri kümesini üreten etkinlik hala saatte bir çalışır. Daha fazla bilgi için [Model farklı frekansları veri kümeleriyle](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>Veri kümesinin kullanılabilirliğine ve ilkeleri
Kullanım sıklığı ve veri kümesi tanımı'nin kullanılabilirlik bölümünü de aralık özellikleri gördünüz. Diğer birkaç zamanlama etkileyen özellikler ve etkinlik yürütülmesini barındırabilen vardır. 

### <a name="dataset-availability"></a>Veri kümesi kullanılabilirlik 
Aşağıdaki tabloda kullanabileceğiniz özellikleri açıklanmaktadır **kullanılabilirlik** bölümü:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilim üretim yönelik zaman birimini belirtir.<br/><br/><b>Sıklık desteklenen</b>: Dakika, saat, gün, hafta, ay |Evet |NA |
| interval |Sıklığı çarpanı belirtir<br/><br/>"X sıklık aralığı" ne sıklıkta dilim üretilir belirler.<br/><br/>Veri kümesinin saatlik olarak dilimlenmiş gerekiyorsa, ayarladığınız <b>sıklığı</b> için <b>saat</b>, ve <b>aralığı</b> için <b>1</b>.<br/><br/><b>Not</b>: Sıklığını dakika belirtmeniz durumunda da en az 15'e aralığı ayarlamanızı öneririz |Evet |NA |
| Stil |Dilim aralığı başlangıç/bitiş sırasında üretilen olup olmadığını belirtir.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Frequency ay ayarlanır ve stil EndOfInterval için ayın son gününde dilim üretilir. Stili için StartOfInterval ayarlarsanız, ayın ilk gününde dilim üretilir.<br/><br/>Sıklığı gün olarak ayarlanır ve stil EndOfInterval için dilim günün son bir saat içinde üretilmez.<br/><br/>Sıklık saat olarak ayarlanır ve stil EndOfInterval için dilim saatin sonunda üretilmez. Örneğin, 2 saat 13 – PM dönem için bir dilim için 2 saat dilim üretilir. |Hayır |EndOfInterval |
| anchorDateTime |Zamanlayıcı tarafından veri kümesi dilim sınırlarını hesaplamak için kullanılan zaman içinde mutlak konum tanımlar. <br/><br/><b>Not</b>: AnchorDateTime sıklığından daha ayrıntılı tarih kısımlarını varsa daha ayrıntılı bölümleri göz ardı edilir. <br/><br/>Örneğin, varsa <b>aralığı</b> olduğu <b>saatlik</b> (Sıklık: saat ve aralığı: (1) ve <b>AnchorDateTime</b> içeren <b>dakika ve saniye</b>, ardından <b>dakika ve saniye</b> AnchorDateTime kısımlarını yok sayılır. |Hayır |01/01/0001 |
| uzaklık |Başlangıç ve bitiş tüm veri kümesi dilim olarak kaydırılan bir TimeSpan değeri. <br/><br/><b>Not</b>: AnchorDateTime hem uzaklık belirtilirse, sonuç birleşik bir kaydırmadır. |Hayır |NA |

### <a name="offset-example"></a>uzaklık örneği
Varsayılan olarak her gün (`"frequency": "Day", "interval": 1`) dilimleri 12: 00 UTC zaman (gece yarısı) başlatın. Uzaklık, başlangıç zamanı, 6 AM UTC saati yerine olmasını istiyorsanız, aşağıdaki kod parçacığında gösterildiği gibi ayarlayın: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime örneği
Aşağıdaki örnekte, veri kümesini 23 saatte bir kez üretilir. İlk dilim ayarlanır anchorDateTime tarafından belirlenen süre başlar `2017-04-19T08:00:00` (UTC saati).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Örnek uzaklığı/stil
Aşağıdaki veri kümesinin aylık bir veri kümesidir ve 3 saat 8: 00'da, her ayın üretilir (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>Veri kümesi İlkesi
Bir veri kümesi için kullanılmaya hazır hale gelmeden önce bir dilim yürütme tarafından oluşturulan verileri nasıl doğrulanabilir belirten tanımlı bir doğrulama ilkesi olabilir. Dilim, yürütme tamamlandıktan sonra bu gibi durumlarda, çıkış dilimi durumu olarak değiştirilir **bekleyen** ile alt **doğrulama**. Dilimler doğrulandıktan sonra dilim durumu değişerek **hazır**. Veri dilimi üretildiğini, ancak doğrulamayı geçemedi bu slice bağımlı aşağı akış dilimleri için etkinlik çalıştırmalarını işlenmez. [İzleme ve işlem hatlarını yönetme](data-factory-monitor-manage-pipelines.md) Data factory'de veri dilimleri çeşitli durumları kapsar.

**İlke** veri kümesi tanımı bölümünde ölçütleri veya veri kümesinin dilimlerini karşılamalıdır koşulu tanımlar. Aşağıdaki tabloda kullanabileceğiniz özellikleri açıklanmaktadır **ilke** bölümü:

| İlke Adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Doğrulama verilerde bir **Azure blob** (megabayt cinsinden) en küçük boyut gereksinimlerini karşılıyor. |Azure Blob |Hayır |NA |
| minimumRows | Doğrulama verilerde bir **Azure SQL veritabanı** veya bir **Azure tablo** en az sayıda satır içerir. |<ul><li>Azure SQL Veritabanı</li><li>Azure Tablosu</li></ul> |Hayır |NA |

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

Daha fazla bilgi için [işlem hatları](data-factory-create-pipelines.md) makalesi. 

## <a name="parallel-processing-of-data-slices"></a>Paralel işlenmesini veri dilimleri
İşlem hattının başlangıç tarihi geçmiş ayarlayabilirsiniz. Bunu yaptığınızda, Data Factory, otomatik olarak (geri doldurur) tüm veri dilimleri geçmişte hesaplar ve bunları işlemeye başlar. Örneğin: Başlangıç tarihi ile 2017-04-01 işlem hattı oluşturma ve geçerli tarihin 2017-04-10 olması. Çıktı veri kümesi temposu günlük, 2017-04-01'den tüm dilimleri 2017-04-09 için işleme Data Factory başlar, başlangıç tarihi geçmiş hemen olduğu için. Kullanılabilirlik bölümünde stil özelliğinin değeri EndOfInterval olduğundan 2017-04-10 dilimden varsayılan olarak henüz işlenmedi. Eski dilim işlenir önce varsayılan olarak executionPriorityOrder OldestFirst değeridir. Stil özelliği açıklaması için bkz: [veri kümesinin kullanılabilirliğine](#dataset-availability) bölümü. ExecutionPriorityOrder bölümü açıklaması için bkz: [etkinlik ilkeleri](#activity-policies) bölümü. 

Ayarlayarak paralel olarak işlenecek arka doldurulmuş veri dilimleri yapılandırabileceğiniz **eşzamanlılık** özelliğinde **ilke** bölümü etkinliğin JSON. Bu özellik üzerinde farklı dilimleri oluşabilir paralel Etkinlik yürütme sayısını belirler. Eşzamanlılık özelliğinin varsayılan değeri 1'dir. Bu nedenle, bir dilimin varsayılan olarak teker teker işlenir. En yüksek değer 10'dur. Bir işlem hattı daha büyük bir eşzamanlılık değeri, kullanılabilir verilerin çok sayıda Git gerektiğinde, veri işleme hızı artar. 

## <a name="rerun-a-failed-data-slice"></a>Başarısız olan veri dilimi yeniden çalıştırın
Veri dilimi işlenirken bir hata oluştuğunda neden Azure portalı dikey pencerelerinin veya izleme ve yönetme uygulaması'nı kullanarak bir dilimin işlenmesini dağıtamadığını bulabilirsiniz. Bkz: [izleme ve Azure portal dikey penceresi kullanılarak işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md) veya [izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md) Ayrıntılar için.

İki etkinliği gösteren aşağıdaki örneği inceleyin. Activity1 ve etkinlik 2. Activity1 Dataset1 dilimin tüketir ve girdi olarak son veri kümesinin bir dilimi üretmek için Activity2 tarafından tüketilen Dataset2, bir dilimin üretir.

![Başarısız dilim](./media/data-factory-scheduling-and-execution/failed-slice.png)

Diyagram, Dataset2 için 9-10'da dilimi üreten bir hata oluştu, üç son dilimler dışında gösterir. Veri fabrikası, zaman serisi veri kümesi için bağımlılık otomatik olarak izler. Sonuç olarak, 9-10'da aşağı akış dilimi için çalıştırılan etkinlik başlamıyor.

Data Factory izleme ve Yönetim Araçları, bir kolayca sorununu kök nedeni bulmak ve düzeltmek için başarısız dilim için tanılama günlüklerini incelemek izin verin. Sorunu düzelttikten sonra etkinlik başarısız dilim üretmek için çalıştırma kolayca başlayabilirsiniz. Yeniden çalıştırın ve veri dilimi için durumu geçişleri anlama hakkında daha fazla bilgi için bkz. [izleme ve Azure portal dikey penceresi kullanılarak işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md) veya [izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md).

9-10'da dilimin yeniden çalıştırdıktan sonra **Dataset2**, Data Factory son veri kümesinin üzerinde çalıştırmak için 9-10'da bağımlı dilim başlatır.

![Yeniden çalıştırma başarısız dilim](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Bir işlem hattında birden çok etkinlik
Bir işlem hattında birden fazla etkinliğiniz olabilir. Etkinlikler için giriş veri dilimi hazır olup olmadığını bir işlem hattında birden fazla etkinlik varsa ve bir etkinliğin çıktısını başka bir etkinliğin girdi, etkinlikler paralel olarak çalışabilir.

Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Etkinlikleri işlem hattının aynı veya farklı işlem hatları olabilir. İkinci etkinlik, yalnızca ilki başarıyla tamamlandığı yürütür.

Örneğin, bir işlem hattı iki etkinliği sahip olduğu aşağıdaki örneği inceleyin:

1. Dış girdi veri kümesi D1 ve D2 üretir çıktı veri kümesi gerektirir A1 etkinlik.
2. Veri kümesi D3 D2 kümesinden giriş gerektirir ve üretir etkinlik A2 çıktı.

Bu senaryoda, aynı işlem hattında etkinlikleri A1 ve A2 var. Etkinlik, dış veri kullanılabilir olduğunda ve zamanlanan kullanılabilirlik sıklığı ulaşıldığında A1 çalıştırır. Etkinlik, D2 zamanlanmış dilimler kullanılabilir hale gelir ve zamanlanan kullanılabilirlik sıklığı ulaşıldığında A2 çalıştırır. Veri kümesi D2 dilimleri birinde bir hata varsa, kullanılabilir olana kadar A2, branchcache'den çalıştırmaz.

Aynı işlem hattının her iki etkinliklerle diyagram görünümü, aşağıdaki diyagramda gibi görünür:

![Aynı işlem hattındaki etkinlikler, zincirleme](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Daha önce bahsedildiği gibi etkinlikler farklı işlem hatlarında olabilir. Diyagram görünümü, böyle bir senaryoda, aşağıdaki diyagramda gibi görünür:

![Zincirleme iki işlem hattı içindeki etkinlikleri](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Bir örnek için ek sırayla konusundaki kopyalama konusuna bakın.

## <a name="model-datasets-with-different-frequencies"></a>Farklı bir sıklık ile model veri kümeleri
Örnekler, girdi ve çıktı veri kümeleri ve etkinlik zamanlama penceresi sıklıklardan aynı olduğu. Bazı senaryolarda, bir veya daha fazla giriş sıklık farklı bir sıklıkta çıkış üretmesi ölçeklendirebilmeniz gerekir. Data Factory, bu senaryo modelleme destekler.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Örnek 1: Her saat kullanılabilir giriş verileri için günlük bir çıkış raporu oluşturmak
Azure Blob depolama alanındaki her saat sensörlere ölçüm verileri Giriş bir senaryo düşünün. Gün için ortalama, maksimum ve minimum gibi istatistikleri ile günlük toplama bir rapor oluşturmak istediğiniz [Data Factory hive etkinliği](data-factory-hive-activity.md).

Bu senaryoda Data Factory ile nasıl model şu şekildedir:

**Giriş veri kümesi**

Saatlik giriş dosyaları için belirli bir günde klasöründe bırakılır. Giriş için kullanılabilirlik ayarlanırsa **saat** (sıklığı: Hour, interval: 1).

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

Bir çıkış dosyası, her gün günün klasöründe oluşturulur. Çıkış kullanılabilirliğini ayarlanırsa **gün** (sıklığı: Day ve interval: 1).

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

**Etkinliği: işlem hattındaki hive etkinliği**

Hive betiğinin uygun alan *DateTime* kullanın parametreleri olarak bilgi **WindowStart** aşağıdaki kod parçacığında gösterildiği gibi bir değişken. Hive betiğinin verileri güne ait doğru klasörden yüklemek ve çıktı üretmek için toplama işlemini çalıştırmak için bu değişkeni kullanır.

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

Aşağıdaki diyagramda, bir veri bağımlılık açısından senaryo gösterilmektedir.

![Veri bağımlılığı](./media/data-factory-scheduling-and-execution/data-dependency.png)

Çıktı dilimi her gün için bir giriş veri kümesinden 24 saatlik dilim bağlıdır. Data Factory'ye giriş verileri olarak üretilecek çıktı dilimi aynı zaman dönemi içindeki kalan dilimleri hesaplayarak bu bağımlılıkları otomatik olarak hesaplar. Herhangi bir 24 giriş dilimi kullanılamıyorsa, Data Factory giriş dilimi günlük etkinlik çalıştırması başlamadan önce hazır olmasını bekler.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Örnek 2: Data Factory işlevleri ve ifadeleri ile bağımlılık belirtin
Şimdi başka bir senaryo düşünün. İki giriş veri kümesi işleyen bir hive etkinliği olduğunu varsayalım. Bunlardan biri yeni veriler günlük olsa da, bunlardan birinin her hafta yeni veri alır. İki girdi arasında birleştirme yapın ve her gün bir çıkış üretmesine istediğinizi varsayalım.

Hangi Data factory'de basit yaklaşım cevabı sağa hizalama veri diliminin zaman dönemi çalışmıyor çıktı tarafından işlenecek dilimleri otomatik olarak girin.

Çalıştırın ve her etkinlik için haftalık giriş veri kümesi için geçen haftaki veri dilimi Data Factory kullanacağını belirtmeniz gerekir. Azure Data Factory işlevleri aşağıdaki kod parçacığında gösterildiği gibi bu davranışı uygulamak için kullanırsınız.

**Input1: Azure blob**

İlk giriş, günlük güncelleştirilen Azure blobudur.

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

Input2 haftalık olarak güncelleştirilen Azure blob ' dir.

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

Bir çıkış dosyası her gün, gün için klasöründe oluşturulur. Çıkış, kullanılabilirlik kümesine **gün** (sıklığı: Gün, aralığı: 1).

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

**Etkinliği: işlem hattındaki hive etkinliği**

Hive etkinliği, iki giriş alır ve her gün bir çıktı dilimi oluşturur. Önceki haftanın giriş dilimi haftalık bir giriş için şu şekilde bağımlı için her günün çıktı dilimi belirtebilirsiniz.

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

Bkz: [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md) işlevler ve Data Factory destekler sistem değişkenleri listesi.

## <a name="appendix"></a>Ek

### <a name="example-copy-sequentially"></a>Örnek: sıralı olarak Kopyala
Birden çok kopyalama işlemleri birbiri ardına sıralı/sıralı bir şekilde çalıştırmak mümkündür. Örneğin, iki kopyalama etkinliği aşağıdaki giriş verileri çıkış veri kümeleri ile bir işlem hattı (CopyActivity1 ve CopyActivity2) olabilir:   

CopyActivity1

Giriş: Veri kümesi. Çıkış: Dataset2.

CopyActivity2

Giriş: Dataset2.  Çıkış: Dataset3.

CopyActivity2 yalnızca CopyActivity1 başarıyla çalıştırıldı ve Dataset2 kullanılabilir çalışır.

Örnek işlem hattı JSON şu şekildedir:

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

Örnekte, ikinci etkinliği için girdi olarak ilk kopyalama etkinliği (Dataset2) çıkış veri kümesini belirtildiğine dikkat edin. Bu nedenle, ilk etkinliğin çıkış veri kümesi hazır olduğunda ikinci etkinlik çalışır.  

Örnekte, CopyActivity2 Dataset3 gibi farklı bir giriş olabilir ancak CopyActivity1 bitene kadar etkinlik çalışmıyor bu nedenle, Dataset2 CopyActivity2, girdi olarak belirtin. Örneğin:

CopyActivity1

Giriş: DataSet1. Çıkış: Dataset2.

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

Örnekte, iki giriş veri kümesi için ikinci kopyalama etkinliği belirtildiğinden emin dikkat edin. Birden çok giriş belirtildiğinde, yalnızca ilk girdi veri kümesi, verileri kopyalamak için kullanılır, ancak diğer veri kümelerine bağımlılıkları olarak kullanılır. Yalnızca aşağıdaki koşullar sonra CopyActivity2 başlar:

* CopyActivity1 başarıyla tamamlandı ve Dataset2 kullanılabilir. Bu veri kümesi, veri için Dataset4 kopyalarken kullanılmaz. Yalnızca CopyActivity2 için zamanlama bağımlılık olarak davranır.   
* Dataset3 kullanılabilir. Bu veri kümesi hedefe kopyalanan verileri temsil eder. 
