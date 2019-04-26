---
title: Data Factory işlevleri ve sistem değişkenleri | Microsoft Docs
description: Azure Data Factory işlevleri ve sistem değişkenlerini bir listesini sağlar
documentationcenter: ''
author: sharonlo101
manager: craigg
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 1d1c9ef5ba355f1944a362bf0e6f5d7ba91a700a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60486524"
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - işlevler ve sistem değişkenleri
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [sistem değişkenlerini Data factory'de](../control-flow-system-variables.md).

Bu makalede, işlevleri ve Azure Data Factory tarafından desteklenen değişkenler hakkında bilgi sağlar.

## <a name="data-factory-system-variables"></a>Data Factory sistem değişkenleri

| Değişken adı | Açıklama | Nesne kapsamı | JSON kapsamı ve kullanım örnekleri |
| --- | --- | --- | --- |
| WindowStart |Zaman aralığı için geçerli etkinlik penceresini çalıştırma başlangıcı |etkinlik |<ol><li>Veri seçimi sorgularında belirtin. Başvuru Bağlayıcısı makalelerine bakın [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi.</li> |
| WindowEnd |Geçerli etkinlik penceresini çalıştırma için zaman aralığı sonu |etkinlik |WindowStart ile aynıdır. |
| SliceStart |Üretilen veri dilimi için zaman aralığı başlangıcı |etkinlik<br/>Veri kümesi |<ol><li>Dinamik klasör yolları belirtin ve dosya adları ile çalışırken [Azure Blob](data-factory-azure-blob-connector.md) ve [dosya sistemi veri kümeleri](data-factory-onprem-file-system-connector.md).</li><li>Data factory işlevleriyle etkinlik giriş koleksiyonuna giriş bağımlılıkları belirtin.</li></ol> |
| SliceEnd |Zaman aralığı için geçerli bir veri dilimine sonu. |etkinlik<br/>Veri kümesi |SliceStart ile aynıdır. |

> [!NOTE]
> Şu anda data factory etkinliğinde tam olarak belirtilen zamanlamayı çıktı veri kümesinin kullanılabilirlik belirtilen zamanlaması eşleştiğini gerektirir. Bu nedenle, WindowStart, WindowEnd ve SliceStart ve SliceEnd her zaman aynı süre ve tek çıktı dilimi eşlenir.
> 

### <a name="example-for-using-a-system-variable"></a>Örneğin, bir sistem değişkeni kullanılarak
Aşağıdaki örnekte, yıl, ay, gün ve saati de **SliceStart** tarafından kullanılan ayrı değişkenleri ayıklanan **folderPath** ve **fileName** özellikleri.

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a>Data Factory işlevleri
Aşağıdaki amaçlarla sistem değişkenleri yanı sıra data factory'de işlevleri kullanabilirsiniz:

1. Veri seçimi sorgularında belirtme (tarafından başvurulan bağlayıcı makalelerine bakın [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi.
   
   Bir veri fabrikası işlevi çağırmak için sözdizimi aşağıdaki gibidir:  **$$ \<işlevi >** veri seçimi sorgularında ve etkinlik ve veri kümelerinde diğer özellikler için.  
2. Data factory işlevleriyle etkinlik giriş koleksiyonuna giriş bağımlılıklarını belirtme.
   
    $ Giriş bağımlılık ifadeleri belirtmek için gerekli değildir.     

Aşağıdaki örnekte, **sqlReaderQuery** bir JSON dosyası bir özellik tarafından döndürülen bir değer atanır `Text.Format` işlevi. Bu örnek adlı bir sistem değişkeni de kullanır. **WindowStart**, etkinlik çalıştırması penceresinin başlangıç zamanı temsil eder.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Bkz: [özel tarih ve saat biçim dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx) kullanabileceğiniz farklı biçimlendirme seçeneklerini açıklayan bir konu (örneğin: yyyy karşılaştırması ay). 

### <a name="functions"></a>İşlevler
Aşağıdaki tablolarda Azure Data Factory içindeki tüm işlevlere listeleyin:

| Kategori | İşlev | Parametreler | Açıklama |
| --- | --- | --- | --- |
| Zaman |AddHours(X,Y) |X: DateTime <br/><br/>Y: int |Verilen süre X Y saat ekler. <br/><br/>Örnek: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Zaman |AddMinutes(X,Y) |X: DateTime <br/><br/>Y: int |Y dakika X ekler.<br/><br/>Örnek: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Zaman |StartOfHour(X) |X: DateTime |X saat bileşeni tarafından temsil edilen saat için başlangıç saatini alır. <br/><br/>Örnek: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Tarih |AddDays(X,Y) |X: DateTime<br/><br/>Y: int |X-Y gün ekler. <br/><br/>Örnek: 15/9/2013 12:00:00 PM'den + 2 gün = 9/17/2013 12:00:00 PM.<br/><br/>Y negatif bir sayı olarak belirterek gün çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Tarih |AddMonths(X,Y) |X: DateTime<br/><br/>Y: int |X-Y ay ekler.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>Y negatif bir sayı olarak belirterek ay çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Tarih |AddQuarters(X,Y) |X: DateTime <br/><br/>Y: int |Ekler Y * x 3 ay.<br/><br/>Örnek: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Tarih |AddWeeks(X,Y) |X: DateTime<br/><br/>Y: int |Ekler Y * x 7 gün<br/><br/>Örnek: 15/9/2013 12:00:00 PM'den + 1 hafta = 9/22 //Build/ 2013 12:00:00 PM<br/><br/>Y negatif bir sayı olarak belirterek hafta çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Tarih |AddYears(X,Y) |X: DateTime<br/><br/>Y: int |X-Y yıl ekler.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>Y negatif bir sayı olarak belirterek yıl çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Tarih |Day(X) |X: DateTime |X gün bileşenini alır.<br/><br/>Örnek: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Tarih |DayOfWeek(X) |X: DateTime |Haftanın günü bileşenini x alır.<br/><br/>Örnek: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Tarih |DayOfYear(X) |X: DateTime |X yıl bileşenini tarafından temsil edilen yılın günü alır.<br/><br/>Örnekler:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Tarih |DaysInMonth(X) |X: DateTime |X parametresi ay bileşenini tarafından temsil edilen aydaki gün alır.<br/><br/>Örnek: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`. |
| Tarih |EndOfDay(X) |X: DateTime |Tarih-saat x (gün bileşenini) gün sonunu temsil eden alır.<br/><br/>Örnek: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Tarih |EndOfMonth(X) |X: DateTime |Ay bileşeninin X parametresi tarafından temsil edilen ayın sonunu alır. <br/><br/>Örnek: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (tarihi Eylül ayın sonunu temsil eden saat) |
| Tarih |StartOfDay(X) |X: DateTime |Parametre X gün bileşenini tarafından temsil edilen günün başlangıcını alır.<br/><br/>Örnek: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| DateTime |FROM(X) |X: String |Bir tarih saat dizesi X ayrıştırılamıyor. |
| DateTime |Ticks(X) |X: DateTime |İşaretleri X parametresinin özelliğini alır. Bir değer çizgisi 100 nanosaniyelik eşittir. Bu özelliğin değeri, 12:00:00 gece yarısından itibaren 1 Ocak 0001 geçen tıklarının sayısını temsil eder. |
| Text |Format(X) |X: String değişkeni |Metin biçimleri (kullanın `\\'` kaçış birleşimi `'` karakter).|

> [!IMPORTANT]
> Başka bir işlev içinde bir işlevi kullanılırken kullanın gerekmez **$$** iç işlev için önek. Örneğin: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' ve RowKey ge \\' {0: yyyy-aa-gg SS}\\'', Time.AddHours (SliceStart, -6)). Bu örnekte, dikkat **$$** ön eki için kullanılmaz **Time.AddHours** işlevi. 

#### <a name="example"></a>Örnek
Aşağıdaki örnekte, Hive etkinliğiyle ilgili girdi ve çıktı parametreleri kullanılarak belirlenir `Text.Format` işlevi ve SliceStart sistem değişkeni. 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a>Örnek 2

Aşağıdaki örnekte, DateTime parametresi için saklı yordam etkinliğine metin kullanılarak belirlenir. Biçimlendirme işlevinde ve SliceStart değişkeni. 

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
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a>Örnek 3
Önceki günden SliceStart tarafından temsil edilen gün yerine verileri okumak için aşağıdaki örnekte gösterildiği gibi AddDays işlevi kullanın: 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
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

Bkz: [özel tarih ve saat biçim dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx) kullanabileceğiniz farklı biçimlendirme seçeneklerini açıklayan bir konu (örneğin: yyyy karşılaştırması yy). 

