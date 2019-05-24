---
title: Azure Data Factory'de veri kümeleri oluşturma | Microsoft Docs
description: Uzaklık ve anchorDateTime gibi özellikleri kullanan örnekler ile Azure Data Factory'de veri kümeleri oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 6b16b6c4de8c8d2d7a821dd476f07c8ab1135408
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60487266"
---
# <a name="datasets-in-azure-data-factory"></a>Azure Data factory'deki veri kümelerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-create-datasets.md)
> * [Sürüm 2 (geçerli sürüm)](../concepts-datasets-linked-services.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 veri kümelerinde](../concepts-datasets-linked-services.md).

Bu makale, JSON biçiminde nasıl tanımlandığına hangi veri kümelerinin olduğunu açıklar ve nasıl kullanıldığına Azure Data Factory işlem hatları. Bu veri kümesi JSON tanımında her bölüm (örneğin, yapı, kullanılabilirlik ve ilke) hakkında ayrıntılar sağlar. Ayrıca makalede kullanma örnekleri sağlar **uzaklığı**, **anchorDateTime**, ve **stili** bir veri kümesi JSON tanımındaki özellikler.

> [!NOTE]
> Data Factory kullanmaya yeni başladıysanız bkz [Azure Data Factory'ye giriş](data-factory-introduction.md) genel bakış. Veri fabrikaları oluşturma ile uygulamalı deneyim yoksa, daha iyi okuyarak anlamak kazanmadan [veri dönüştürme öğreticisini](data-factory-build-your-first-pipeline.md) ve [veri taşıma öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. A **işlem hattı** mantıksal bir gruplandırmasıdır **etkinlikleri** birlikte gerçekleştiren bir görev. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, verileri bir şirket içi SQL Server'dan Azure Blob depolama alanına kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Ardından, verileri işlemek için çıkış verileri üretmek üzere Blob depolama alanından gönderilmiş olan bir Azure HDInsight kümesinde bir Hive betiği çalıştıran bir Hive etkinliği kullanabilirsiniz. Son olarak, çıktı verilerini Azure SQL veri ambarı'na çözümleri oluşturulur hangi iş zekası raporlama en üstünde (BI) kopyalamak için ikinci bir kopyalama etkinliği kullanabilirsiniz. İşlem hatları ve etkinlikler hakkında daha fazla bilgi için bkz: [işlem hatları ve etkinlikler Azure Data factory'de](data-factory-create-pipelines.md).

Bir etkinliğin sıfır veya daha fazla giriş sürebilir **veri kümeleri**ve bir veya daha fazla çıkış veri kümesi üretir. Girdi veri kümesi işlem hattındaki bir etkinliğin girdi temsil eder ve bir çıktı veri kümesi etkinliğin çıktısını temsil eder. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin, bir Azure Blob veri kümesi işlem hattı verileri sona okuması gereken Blob Depolama alanında blob kapsayıcısını ve klasörü belirtir.

Bir veri kümesi oluşturmadan önce oluşturun bir **bağlı hizmet** data factory'de veri deponuza bağlamak için. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Veri kümeleri, SQL tablolarını, dosyalar, klasörler ve belgeler gibi bağlı veri depolarındaki verileri tanımlar. Örneğin, bir Azure depolama bir depolama hesabını veri fabrikasına bağlı hizmeti. Bir Azure Blob veri kümesi blob kapsayıcıyı ve işlenecek giriş bloblarını içeren klasörü temsil eder.

Örnek senaryo aşağıda verilmiştir. Verileri Blob depolama alanından SQL veritabanına kopyalamak için iki bağlı hizmet oluşturursunuz: Azure depolama ve Azure SQL veritabanı. Ardından, iki veri kümesi oluşturursunuz: (Azure depolama bağlı hizmetini ifade eder) azure Blob veri kümesi ve Azure SQL tablosu veri kümesi (Bu, Azure SQL veritabanı bağlı hizmetini ifade eder). Azure depolama ve Azure SQL veritabanı bağlı hizmeti, Data Factory, Azure depolama ve Azure SQL veritabanı, sırasıyla bağlanmak için çalışma zamanında kullandığı bağlantı dizeleri içerir. Azure Blob veri kümesi blob kapsayıcısı ve Blob Depolama alanınızda giriş bloblarını içeren blob klasörü belirtir. Azure SQL tablosu veri kümesi, verilerin kopyalanacağı olduğu SQL veritabanınızda SQL tablosunu belirtir.

Aşağıdaki diyagramda, Data Factory'de işlem hattı, etkinlik, veri kümesi ve bağlı hizmet arasındaki ilişkiler gösterilmektedir:

![İşlem hattı, etkinlik, veri kümesi, bağlı hizmetler arasındaki ilişki](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>Dataset JSON
Bir veri kümesinde Data Factory JSON biçiminde şu şekilde tanımlanır:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": "<boolean flag to indicate external data. only for input datasets>",
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
| name |Veri kümesinin adı. Bkz: [Azure Data Factory - adlandırma kuralları](data-factory-naming-rules.md) adlandırma kuralları. |Evet |NA |
| type |Veri kümesi türü. Data Factory tarafından desteklenen türlerinden birini belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için bkz [veri kümesi türü](#Type). |Evet |NA |
| structure |Şema kümesi.<br/><br/>Ayrıntılar için bkz [Dataset yapısını](#Structure). |Hayır |NA |
| typeProperties | Tür özellikleri her türü için farklı (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında daha fazla bilgi için bkz: [veri kümesi türü](#Type). |Evet |NA |
| external | Bir veri kümesi açıkça bir veri fabrikası işlem hattı tarafından veya üretilen olup olmadığını belirlemek için Boole bayrağı. Bir etkinliğin giriş veri kümesi geçerli işlem hattı tarafından üretilen değil, bu bayrağı true olarak ayarlayın. Bu bayrak, işlem hattının birinci etkinliğin giriş veri kümesi için true olarak ayarlayın.  |Hayır |false |
| availability | İşleme penceresini (örneğin, saatlik veya günlük) veya veri kümesi üretim dilimleme modelini tanımlar. Her bir birimi kullanılan ve bir etkinlik çalışması tarafından üretilen veriler, veri dilim adı verilir. Bir çıktı veri kümesi Kullanılabilirliği (sıklığı - gün, interval - 1) günlük olarak ayarlanırsa, bir dilim günlük oluşturulur. <br/><br/>Ayrıntılar için veri kümesinin kullanılabilirliğine bakın. <br/><br/>Model dilimleme veri kümesi hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makalesi. |Evet |NA |
| policy |Ölçüt veya veri kümesinin dilimlerini karşılamalıdır koşulu tanımlar. <br/><br/>Ayrıntılar için bkz [veri kümesi ilke](#Policy) bölümü. |Hayır |NA |

## <a name="dataset-example"></a>Veri kümesi örneği
Aşağıdaki örnekte adlı bir tablo veri kümesini temsil eden **MyTable** bir SQL veritabanı'nda.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Aşağıdaki noktalara dikkat edin:

* **tür** AzureSqlTable için ayarlanır.
* **tableName** type özelliği (AzureSqlTable türüne özel) MyTable için ayarlanır.
* **linkedServiceName** sonraki JSON kod parçacığında tanımlanan AzureSqlDatabase türünde bir bağlı hizmetini ifade eder.
* **Kullanılabilirlik sıklığı** gün olarak ayarlanır ve **aralığı** 1 olarak ayarlayın. Bu veri kümesi dilim günlük üretilen anlamına gelir.

**AzureSqlLinkedService** şu şekilde tanımlanır:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

Yukarıdaki JSON parçacığında:

* **tür** AzureSqlDatabase için ayarlanır.
* **connectionString** türü özelliği, bir SQL veritabanına bağlanmak için gereken bilgileri belirtir.

Gördüğünüz gibi bağlı hizmet bir SQL veritabanı'na bağlanma tanımlar. Hangi tablo girdi olarak kullanılan ve bir işlem hattındaki etkinliğin çıkış veri kümesini tanımlar.

> [!IMPORTANT]
> Bir veri kümesi, işlem hattı tarafından üretilmekte olan sürece bu olarak işaretlenmelidir **dış**. Bu ayar genellikle bir işlem hattındaki ilk etkinliğin girişleri için de geçerlidir.

## <a name="Type"></a> Veri kümesi türü
Veri kümesi türü, kullandığınız veri deposuna bağımlı. Data Factory tarafından desteklenen veri depolarının listesi için aşağıdaki tabloya bakın. Bir veri deposunu bağlı hizmet ve bu veri deposu için bir veri kümesi oluşturma hakkında bilgi edinmek için tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Taşıyan veri depoları * şirket içi olabilir ya da Azure altyapı (ıaas) olarak. Bu veri depoları yüklemenizi gerektirir [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).

Önceki bölümdeki örnekte, veri kümesi türü kümesine **AzureSqlTable**. Benzer şekilde, bir Azure Blob veri kümesi için veri kümesi türü ayarlanacağını **AzureBlob**aşağıdaki JSON'da gösterildiği gibi:

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

## <a name="Structure"></a>Veri kümesi yapısı
**Yapısı** bölümüne, isteğe bağlıdır. Bu veri kümesi şemasını içeren bir koleksiyon adları ve sütunların veri türlerini tarafından tanımlar. Türleri dönüştürme ve kaynaktan hedef sütunlara eşlemek için kullanılan tür bilgileri sağlamak için yapı bölümünü kullanın. Aşağıdaki örnekte, üç sütun bir veri kümesine sahiptir: `slicetimestamp`, `projectname`, ve `pageviews`. Bunlar dize, dize ve ondalık, sırasıyla türüdür.

```json
structure:
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Her sütunda yapısı aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| name |Sütunun adı. |Evet |
| type |Sütunun veri türü.  |Hayır |
| culture |. Türü bir .NET türü olduğunda kullanılacak kültürü NET tabanlı: `Datetime` veya `Datetimeoffset`. Varsayılan değer: `en-us`. |Hayır |
| format |Biçim türü .NET türü olduğunda kullanılacak dize: `Datetime` veya `Datetimeoffset`. |Hayır |

Aşağıdaki yönergeleri yapı bilgileri içerecek şekilde ne zaman ve ne eklenecek belirlemenize yardımcı **yapısı** bölümü.

* **Yapılandırılmış veri kaynakları için**, yalnızca sütunları havuz için kaynak sütunları eşlemek istediğiniz ve adları aynı değildir yapısı kısmında belirtin. Bu türdeki yapılandırılmış veri kaynağının veri yanı sıra veri şema ve tür bilgilerini depolar. SQL Server, Oracle ve Azure tablo yapılandırılmış veri kaynağı örnekleri içerir.
  
    Tür bilgilerini zaten yapılandırılmış veri kaynakları için mevcut olduğundan, yapısı Bölümü eklediğinizde türü bilgi içermemelidir.
* **Şema (özel olarak Blob Depolama) salt okunur veri kaynaklarında**, herhangi bir şema veya türü bilgi veri depolamadan veri saklamayı da seçebilirsiniz. Sütunları havuz için kaynak sütunları eşlemek istediğiniz zaman bu veri kaynağı türleri için yapısı içerir. Ayrıca bir kopyalama etkinliği için girdi veri kümesidir ve havuz için yerel türler için kaynak veri kümesinin veri türleri dönüştürülür yapısı içerir.
    
    Data Factory yapısındaki tür bilgileri sağlamak için aşağıdaki değerleri destekler: **Int16, Int32, Int64, tek, Double, ondalık, bayt [], Boolean, dize, Guid, Datetime, Datetimeoffset ve Timespan**. Ortak dil belirtimi (CLS) bu değerler-uyumlu. AĞ tabanlı türü değerleri.

Veri fabrikası, verileri bir kaynak veri deposundan bir havuz veri deposuna taşırken tür dönüştürmeleri otomatik olarak gerçekleştirir.

## <a name="dataset-availability"></a>Veri kümesi kullanılabilirlik
**Kullanılabilirlik** bölümünde bir veri kümesini veri kümesi için bir işleme penceresini (örneğin, saatlik, günlük veya haftalık) tanımlar. Etkinlik pencereleri hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md).

Aşağıdaki kullanılabilirlik bölümü çıktı veri kümesinin saatlik oluşturulur veya giriş veri kümesi saatlik kullanılabilir belirtir:

```json
"availability":
{
    "frequency": "Hour",
    "interval": 1
}
```

İşlem hattı aşağıdaki başlangıç ve bitiş zamanlarını varsa:

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

Çıktı veri kümesi üretilen saatlik işlem hattı başlangıç ve bitiş saatleri. Bu nedenle, bu işlem hattı, her etkinlik penceresi (12: 00 - 1'da, AM 1 - 2 AM, 02: 00 - 3'te, 3'te - 4'te, 04: 00 - 5 AM) için bir tane tarafından üretilen beş veri dilimi vardır.

Aşağıdaki tabloda kullanılabilirlik bölümünde kullanabileceğiniz özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilim üretim yönelik zaman birimini belirtir.<br/><br/><b>Sıklık desteklenen</b>: Dakika, saat, gün, hafta, ay |Evet |NA |
| interval |Sıklığı çarpanı belirtir.<br/><br/>"X sıklık aralığı" ne sıklıkta dilim üretilir belirler. Örneğin, veri kümesinin saatlik olarak dilimlenmiş gerekiyorsa, ayarladığınız <b>sıklığı</b> için <b>saat</b>, ve <b>aralığı</b> için <b>1</b>.<br/><br/>Belirttiğiniz gerçekleştiriyorsanız **sıklığı** olarak **dakika**, aralığı en az 15'e ayarlamanız gerekir. |Evet |NA |
| style |Dilim başlangıç veya Bitiş aralığı olarak üretilen belirtir.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Varsa **sıklığı** ayarlanır **ay**, ve **stili** ayarlanır **EndOfInterval**, ayın son gününde dilim üretilir. Varsa **stili** ayarlanır **StartOfInterval**, ayın ilk gününde dilim üretilir.<br/><br/>Varsa **sıklığı** ayarlanır **gün**, ve **stili** ayarlanır **EndOfInterval**, günün son bir saat içinde dilim üretilir.<br/><br/>Varsa **sıklığı** ayarlanır **saat**, ve **stili** ayarlanır **EndOfInterval**, saatin sonunda dilim üretilir. Örneğin, 2 saat 13 - PM dönem için bir dilim için 2 saat dilim üretilir. |Hayır |EndOfInterval |
| anchorDateTime |Zamanlayıcı tarafından veri kümesi dilim sınırlarını hesaplamak için kullanılan zaman içinde mutlak konum tanımlar. <br/><br/>Bu özellik, belirtilen sıklığından daha ayrıntılı tarih kısımlarını varsa, daha ayrıntılı bölümleri göz ardı edilir unutmayın. Örneğin, varsa **aralığı** olduğu **saatlik** (Sıklık: saat ve aralığı: 1) ve **anchorDateTime** içeren **dakika ve saniye**, ardından dakika ve saniye bölümlerini **anchorDateTime** göz ardı edilir. |Hayır |01/01/0001 |
| offset |Başlangıç ve bitiş tüm veri kümesi dilim olarak kaydırılan bir TimeSpan değeri. <br/><br/>Her iki unutmayın **anchorDateTime** ve **uzaklığı** belirtilirse, sonuç, birleşik kaydırma. |Hayır |NA |

### <a name="offset-example"></a>uzaklık örneği
Varsayılan olarak her gün (`"frequency": "Day", "interval": 1`) dilimleri 12: 00 (gece yarısı) Eşgüdümlü Evrensel Saat (UTC) Başlat. Uzaklık, başlangıç zamanı, 6 AM UTC saati yerine olmasını istiyorsanız, aşağıdaki kod parçacığında gösterildiği gibi ayarlayın:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime örneği
Aşağıdaki örnekte, veri kümesini 23 saatte bir kez üretilir. İlk dilim tarafından belirlenen süre başlar **anchorDateTime**, Hosted `2017-04-19T08:00:00` (UTC).

```json
"availability":
{
    "frequency": "Hour",
    "interval": 23,
    "anchorDateTime":"2017-04-19T08:00:00"
}
```

### <a name="offsetstyle-example"></a>uzaklık/stilini örneği
Aşağıdaki veri kümesi aylık ve 8: 00'da, her ayın 3 üretilir (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>Veri kümesi İlkesi
**İlke** veri kümesi tanımı bölümünde ölçütleri veya veri kümesinin dilimlerini karşılamalıdır koşulu tanımlar.

### <a name="validation-policies"></a>Doğrulama ilkeleri
| İlke adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Doğrulama verilerde **Azure Blob Depolama** (megabayt cinsinden) en küçük boyut gereksinimlerini karşılıyor. |Azure Blob depolama |Hayır |NA |
| minimumRows |Doğrulama verilerde bir **Azure SQL veritabanı** veya bir **Azure tablo** en az sayıda satır içerir. |<ul><li>Azure SQL veritabanı</li><li>Azure tablosu</li></ul> |Hayır |NA |

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

**minimumRows:**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Dış veri kümeleri
Dış veri kümeleri, çalışan bir veri fabrikasındaki işlem hattı tarafından üretilmeyen olanlardır. Veri kümesi olarak işaretlenmişse **dış**, **ExternalData** İlkesi, veri kümesi dilim kullanılabilirliği davranışını etkilemek için tanımlanabilir.

Bir veri kümesi Data Factory tarafından üretilen sürece bu olarak işaretlenmelidir **dış**. Etkinlik veya işlem hattı zincirleme kullanılmadığı sürece bu ayar genellikle, işlem hattındaki ilk etkinlik girişleri için geçerlidir.

| Ad | Açıklama | Gerekli | Varsayılan değer |
| --- | --- | --- | --- |
| dataDelay |Belirtilen dilim için dış veri kullanılabilirliğini kontrolü gecikme süresini. Örneğin, bu ayarı kullanarak bir saatlik onay erteleyebilirsiniz.<br/><br/>Ayar, yalnızca zamandan için geçerlidir. Örneğin, 13: 00'te şu anda ise ve bu değer 10 dakikadır doğrulama 13: 10'te başlatır.<br/><br/>Bu ayar geçmiş dilimler etkilemez unutmayın. İle dilimler **dilim bitiş zamanı** + **dataDelay** < **artık** herhangi bir gecikme olmadan işlenir.<br/><br/>23:59 büyüktür saatleri saat kullanarak belirtilmelidir `day.hours:minutes:seconds` biçimi. Örneğin, 24 saat belirtmek için 24:00:00 kullanmayın. Bunun yerine, 1.00:00:00 kullanın. 24:00:00 kullanırsanız, 24 gün (24.00:00:00) kabul edilir. 1 gün ve 4 saat, 1:04:00:00 belirtin. |Hayır |0 |
| retryInterval |Bir hata ile sonraki denemesi arasındaki bekleme süresi. Bu ayar, mevcut bir süre için geçerlidir. Önceki başarısız çalışırsanız, sonraki try sonradır **Retryınterval** süresi. <br/><br/>1:00 PM şu anda ise, ilk denemede başlamadan. İlk doğrulama denetimini tamamlamak için süre 1 dakika ve işlem başarısız oldu, sonraki yeniden deneme ise 1:00 + 1 dakika (süre) + 1 dakika (yeniden deneme aralığı) 13: 02'te =. <br/><br/>Geçmiş dilimler için gecikme yoktur. Yeniden deneme hemen gerçekleşir. |Hayır |00:01:00 (1 dakika) |
| retryTimeout |Her yeniden deneme girişimi için zaman aşımı.<br/><br/>Bu özellik, 10 dakika olarak ayarlanır, doğrulama 10 dakika içinde tamamlanmalıdır. Doğrulamayı gerçekleştirmek için 10 dakikadan uzun sürerse, yeniden deneme zaman aşımına uğradı.<br/><br/>Tüm girişimleri doğrulama zaman aşımı için dilim olarak işaretlenmişse **zaman aşımına uğradı**. |Hayır |00:10:00 (10 dakika) |
| maximumRetry |Kaç defa dış veri kullanılabilirliğini denetleyin. Değer izin verilen üst sınırı 10'dur. |Hayır |3 |


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu araçlar ve SDK'lar birini kullanarak veri kümeleri oluşturabilirsiniz:

- Kopyalama Sihirbazı
- Azure portalı
- Visual Studio
- PowerShell
- Azure Resource Manager şablonu
- REST API
- .NET API’si

Bu araçlar ve SDK'lar birini kullanarak işlem hatlarını ve veri kümeleri oluşturmak için adım adım yönergeler için aşağıdaki öğreticilere bakın:

- [Veri dönüştürme etkinliğine sahip işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)
- [Veri taşıma etkinliği ile işlem hattı oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Bir işlem hattı oluşturulup dağıtıldığında sonra yönetebilir ve Azure portalı dikey pencerelerinin ya da izleme ve yönetim uygulaması kullanılarak işlem hatlarınızı izlemek. Adım adım yönergeler için aşağıdaki konulara bakın:

- [Azure portal dikey penceresi kullanılarak işlem hatlarını yönetme ve izleme](data-factory-monitor-manage-pipelines.md)
- [İzleme ve işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetme](data-factory-monitor-manage-app.md)

## <a name="scoped-datasets"></a>Kapsamı belirlenmiş veri kümeleri
Kullanarak bir işlem hattı için kapsamlı veri kümeleri oluşturabilirsiniz **veri kümeleri** özelliği. Bu veri kümeleri yalnızca kullanılabilir diğer işlem hatlarında etkinlikleri tarafından değil, bu işlem hattı içindeki etkinlikleri. Aşağıdaki örnek, iki veri kümesi (rdc Inputdataset ve OutputDataset rdc) işlem hattı içinde kullanılan sahip işlem hattı tanımlar.

> [!IMPORTANT]
> Kapsamı belirlenmiş veri kümeleri yalnızca tek seferlik işlem hatları ile desteklenir (burada **pipelineMode** ayarlanır **OneTime**). Bkz: [Onetime işlem hattı](data-factory-create-pipelines.md#onetime-pipeline) Ayrıntılar için.
>
>

```json
{
    "name": "CopyPipeline-rdc",
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
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
- İşlem hatları hakkında daha fazla bilgi için bkz. [işlem hatları oluşturma](data-factory-create-pipelines.md).
- İşlem hatlarını nasıl zamanlanmış ve yürütülen hakkında daha fazla bilgi için bkz. [zamanlama ve yürütme Azure Data factory'de](data-factory-scheduling-and-execution.md).
