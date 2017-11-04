---
title: "Veri Fabrikası işlevleri ve sistem değişkenleri | Microsoft Docs"
description: "Azure Data Factory işlevler ve sistem değişkenleri listesini sağlar"
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 3c506ee95281e1250a721a9c150bd839b4c1fcdb
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - işlevler ve sistem değişkenleri
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [veri fabrikasında sürüm 2 sistem değişkenleri](../control-flow-system-variables.md).

Bu makalede, işlevleri ve değişkenler Azure Data Factory ile desteklenen hakkında bilgi sağlar.

## <a name="data-factory-system-variables"></a>Veri Fabrikası sistem değişkenleri
| Değişken adı | Açıklama | Nesne kapsamı | JSON kapsamı ve kullanım örnekleri |
| --- | --- | --- | --- |
| WindowStart |Geçerli etkinlik penceresini çalıştırmak için zaman aralığı başlangıcı |Etkinlik |<ol><li>Veri seçimi sorguları belirtin. Başvurulan bağlayıcı makalelerine bakın [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi.</li> |
| WindowEnd |Geçerli etkinlik penceresini çalıştırmak için zaman aralığı sonu |Etkinlik |WindowStart ile aynıdır. |
| SliceStart |Zaman aralığı için üretilen veri dilimi başlangıcı |Etkinlik<br/>Veri kümesi |<ol><li>Dinamik klasör yolu belirtin ve dosya adları ile çalışırken [Azure Blob](data-factory-azure-blob-connector.md) ve [dosya sistemi veri kümeleri](data-factory-onprem-file-system-connector.md).</li><li>Veri Fabrikası işlevleriyle etkinlik girişleri koleksiyonu giriş bağımlılıkları belirtin.</li></ol> |
| SliceEnd |Geçerli veri dilimi için zaman aralığı sonu. |Etkinlik<br/>Veri kümesi |SliceStart ile aynıdır. |

> [!NOTE]
> Şu anda veri fabrikası etkinliğin tam olarak belirtilen zamanlaması çıktı veri kümesi kullanılabilirliğini içinde belirtilen zamanlaması eşleştiğini gerektirir. Bu nedenle, WindowStart, WindowEnd ve SliceStart ve SliceEnd her zaman aynı zaman dilimi ve tek bir çıktı dilim eşlenir.
> 

### <a name="example-for-using-a-system-variable"></a>Bir sistem değişkeni kullanma örneği
Aşağıdaki örnek, yıl, ay, gün ve saati de **SliceStart** tarafından kullanılan ayrı değişkenleri içine ayıklanan **folderPath** ve **fileName** özellikleri.

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

## <a name="data-factory-functions"></a>Veri Fabrikası işlevleri
Aşağıdaki amaçlarla sistem değişkenleri yanı sıra veri fabrikasında işlevleri kullanabilirsiniz:

1. Veri seçimi sorguları belirtme (başvurduğu bağlayıcı makalelerine bakın [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi.
   
   Bir veri fabrikası işlevi çağırmak için söz dizimi:  **$$ <function>**  veri seçim sorguları ve diğer özellikleri etkinliği ve veri kümeleri için.  
2. Veri Fabrikası işlevleriyle etkinlik girişleri koleksiyonu giriş bağımlılıkları belirtme.
   
    $$ Giriş bağımlılık ifadeleri belirtmek için gerekli değildir.     

Aşağıdaki örnekte, **sqlReaderQuery** bir JSON dosyası bir özellik tarafından döndürülen bir değer atanması `Text.Format` işlevi. Bu örnek ayrıca adlı bir sistem değişkeni kullanır **WindowStart**, etkinlik penceresinin başlangıç zamanı temsil eder.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Bkz: [özel tarih ve saat biçim dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx) kullanabileceğiniz farklı biçimlendirme seçenekleri açıklar konu (örneğin: yyyy karşılaştırması ay). 

### <a name="functions"></a>İşlevler
Aşağıdaki tablolarda, Azure Data Factory uygulamasında tüm işlevleri listelenmektedir:

| Kategori | İşlevi | Parametreler | Açıklama |
| --- | --- | --- | --- |
| Zaman |AddHours(X,Y) |X: DateTime <br/><br/>Y: int |Y saatleri verilen süre X ekler. <br/><br/>Örnek:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Zaman |AddMinutes(X,Y) |X: DateTime <br/><br/>Y: int |Y dakika X ekler.<br/><br/>Örnek:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Zaman |StartOfHour(X) |X: Datetime |X saat bileşeni tarafından temsil edilen bir saat için başlangıç saatini alır. <br/><br/>Örnek:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Tarih |AddDays(X,Y) |X: DateTime<br/><br/>Y: int |Y günleri X ekler. <br/><br/>Örnek: 9/15/2013 12:00:00 PM + 2 gün = 9/17/2013 12:00:00 PM.<br/><br/>Negatif bir sayı Y belirterek gün çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Tarih |AddMonths(X,Y) |X: DateTime<br/><br/>Y: int |X Y ay ekler.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>Negatif bir sayı Y belirterek ay çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Tarih |AddQuarters(X,Y) |X: DateTime <br/><br/>Y: int |Y ekler * x 3 ay.<br/><br/>Örnek:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Tarih |AddWeeks(X,Y) |X: DateTime<br/><br/>Y: int |Y ekler * x 7 gün<br/><br/>Örnek: 9/15/2013 12:00:00 PM + 1 hafta = 9/22/2013 12:00:00 PM<br/><br/>Negatif bir sayı Y belirterek hafta çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Tarih |AddYears(X,Y) |X: DateTime<br/><br/>Y: int |Y yıl X ekler.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>Negatif bir sayı Y belirterek yıl çok çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Tarih |Day(X) |X: DateTime |X değerinin gün bileşenini alır.<br/><br/>Örnek: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Tarih |DayOfWeek(X) |X: DateTime |Haftanın günü bileşenini x alır.<br/><br/>Örnek: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Tarih |DayOfYear(X) |X: DateTime |X yıl bileşenini tarafından temsil edilen yılın günü alır.<br/><br/>Örnekler:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Tarih |DaysInMonth(X) |X: DateTime |X parametresi ay bileşenini tarafından temsil edilen aydaki gün alır.<br/><br/>Örnek: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`. |
| Tarih |EndOfDay(X) |X: DateTime |Tarih-saat x (gün bileşenini) gün sonunu temsil eden alır.<br/><br/>Örnek: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Tarih |EndOfMonth(X) |X: DateTime |X parametresi ay bileşen tarafından temsil edilen ayın sonunu alır. <br/><br/>Örnek: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (güncel Eylül ayın sonunu temsil eden saat) |
| Tarih |StartOfDay(X) |X: DateTime |X parametresi gün bileşeni tarafından temsil edilen günün başlangıcını alır.<br/><br/>Örnek: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| Tarih saat |FROM(X) |X: dize |Tarih saat X dizeye ayrıştırılamıyor. |
| Tarih saat |Ticks(X) |X: DateTime |Ticks X parametresi özelliğini alır. Bir değer 100 nanosaniye eşittir. Bu özelliğin değeri, 12:00:00 gece'den itibaren 1 Ocak 0001 geçen çizgilerine sayısını temsil eder. |
| Metin |Format(X) |X: Dize değişkeni |Metin biçimleri (kullanmak `\\'` kaçınmak için birleşimi `'` karakter).|

> [!IMPORTANT]
> Başka bir işlev içinde bir işlevi kullanılırken kullanmak gerekmez  **$$**  iç işlevi için önek. Örneğin: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' ve RowKey ge \\' {0: yyyy-aa-gg ss: dd:}\\'', Time.AddHours (SliceStart, -6)). Bu örnekte, dikkat  **$$**  için önek kullanılmaz **Time.AddHours** işlevi. 

#### <a name="example"></a>Örnek
Aşağıdaki örnekte, Hive etkinliği için girdi ve çıktı parametreleri kullanılarak belirlenir `Text.Format` işlevi ve SliceStart sistem değişkeni. 

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

Aşağıdaki örnekte, DateTime parametresi saklı yordam etkinliği için metin kullanılarak belirlenir. İşlev ve SliceStart değişkeni biçimlendirin. 

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
SliceStart tarafından temsil edilen gün yerine önceki gün verilerini okumak için aşağıdaki örnekte gösterildiği gibi AddDays işlevini kullanın: 

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

Bkz: [özel tarih ve saat biçim dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx) kullanabileceğiniz farklı biçimlendirme seçenekleri açıklar konu (örneğin: yy yyyy karşılaştırması). 

