---
title: Azure Data Factory'de veri kümeleri oluşturma | Microsoft Docs
description: Uzaklık ve anchorDateTime gibi özellikleri kullanma örnekleri içeren Azure Data Factory'de veri kümeleri oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 6a3401f620f7dfe8b42bad9ed1a3981325b2ce1e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34620488"
---
# <a name="datasets-in-azure-data-factory"></a>Azure Data Factory'deki veri kümelerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-create-datasets.md)
> * [Sürüm 2 - Önizleme](../concepts-datasets-linked-services.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 kümelerinde](../concepts-datasets-linked-services.md).

Bu makalede hangi veri kümeleri, JSON biçiminde nasıl tanımlanan açıklanmıştır ve Azure Data Factory içinde kullanılan nasıl ardışık düzenleri. Bu veri kümesi JSON tanımında her bölüm (örneğin, yapısı, kullanılabilirlik ve ilke) hakkında ayrıntılar sağlar. Ayrıca makale kullanmaya ilişkin örnekler verilmektedir **uzaklık**, **anchorDateTime**, ve **stili** dataset JSON tanımında özellikleri.

> [!NOTE]
> Data factory'yi yeni istiyorsanız bkz [Azure Data Factory'ye giriş](data-factory-introduction.md) bir genel bakış. Veri fabrikaları oluşturma ile uygulamalı deneyim yoksa daha iyi okuyarak anlamak kazanmadan [veri dönüştürme öğretici](data-factory-build-your-first-pipeline.md) ve [veri taşıma Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. A **ardışık düzen** mantıksal bir gruplandırmasıdır **etkinlikleri** görev birlikte gerçekleştirin. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, bir şirket içi SQL Server'dan Azure Blob depolama alanına veri kopyalamak için kopyalama etkinliği kullanabilirsiniz. Ardından, veri işlemek için çıktı verileri üretemedi Blob depolama alanından gönderilmiş olan bir Azure Hdınsight kümesinde bir Hive betiği çalıştıran bir Hive etkinliği kullanabilir. Son olarak, çıktı verilerini Azure SQL Data Warehouse için çözümleri yerleşik raporlama hangi iş zekası üstünde (BI) kopyalamak için ikinci bir kopyalama etkinliği kullanabilirsiniz. Ardışık Düzen ve etkinlikleri hakkında daha fazla bilgi için bkz: [işlem hatlarının ve etkinliklerin Azure Data Factory](data-factory-create-pipelines.md).

Bir etkinlik sıfır veya daha fazla giriş alabilir **veri kümeleri**ve bir veya daha fazla çıkış veri kümeleri oluşturma. Bir giriş veri kümesi işlem hattındaki bir etkinliğin girdi ve çıktı veri kümesi etkinliğinin çıkış temsil eder. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin, bir Azure Blob dataset ardışık düzen veri okumalısınız Blob depolama alanına blob kapsayıcısı ve klasörü belirtir. 

Bir veri kümesi oluşturmadan önce oluşturun bir **bağlantılı hizmeti** data factory veri deponuza bağlamak için. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Veri kümelerini SQL tablolarının, dosyalar, klasörler ve belgeler gibi bağlantılı veri depoları içindeki verileri tanımlamak. Örneğin, bir Azure depolama hizmeti bağlantıları bir depolama hesabı data factory bağlantılı. Bir Azure Blob dataset blob kapsayıcısında ve işlenmesi için girdi BLOB'lar içeren klasörü temsil eder. 

Bir örnek senaryo aşağıda verilmiştir. Blob depolama alanından bir SQL veritabanına veri kopyalamak için iki bağlı hizmet oluşturma: Azure Storage ve Azure SQL veritabanı. Ardından, iki veri kümesi oluşturun: (Azure Storage bağlı hizmeti ifade eder) Azure Blob veri kümesi ve (Azure SQL bağlı veritabanı hizmeti ifade eder) Azure SQL tablosu veri kümesi. Azure Storage ve Azure SQL veritabanı bağlı Hizmetleri, Azure Storage ve Azure SQL Database, sırasıyla bağlanmak için çalışma zamanında Data Factory kullandığı bağlantı dizeleri içerir. Azure Blob dataset blob kapsayıcısı ve Blob Depolama alanınızın giriş bloblar içeren blob klasörü belirtir. Azure SQL tablosu veri kümesi SQL tablosu SQL veritabanınız veri ekleneceğine dair belirtir.

Aşağıdaki diyagramda, veri fabrikasında ardışık düzen, etkinlik, veri kümesi ve bağlı hizmet arasındaki ilişkiler gösterilmektedir: 

![Ardışık düzen, etkinlik, veri kümesi, bağlı hizmetler arasındaki ilişki](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>JSON veri kümesi
Veri fabrikasında bir veri kümesini JSON biçiminde şu şekilde tanımlanır:

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
| ad |Veri kümesi adı. Bkz: [Azure Data Factory - adlandırma kuralları](data-factory-naming-rules.md) adlandırma kuralları. |Evet |NA |
| type |Veri kümesi türü. Data Factory ile desteklenen türlerden biri belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için bkz [veri kümesi türü](#Type). |Evet |NA |
| yapısı |Veri kümesi şemasını.<br/><br/>Ayrıntılar için bkz [veri kümesi yapısı](#Structure). |Hayır |NA |
| typeProperties | Tür özellikleri her türü için farklı (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında daha fazla bilgi için bkz: [veri kümesi türü](#Type). |Evet |NA |
| external | Bir veri kümesi açıkça data factory işlem hattı tarafından veya üretilen olup olmadığını belirlemek için mantıksal bayrak. Bir etkinliğin girdi veri kümesi geçerli ardışık düzen tarafından üretilen değil, bu bayrağı true olarak ayarlanır. Bu bayrak düzenindeki ilk etkinliğin girdi veri kümesi için true olarak ayarlayın.  |Hayır |false |
| availability | İşleme penceresi (örneğin, saatlik veya günlük) veya veri kümesi üretim dilimleme modelini tanımlar. Her veri birimi, tüketilen ve etkinlik çalışması tarafından üretilen veri dilimi adı verilir. Bir çıkış veri kümesi kullanılabilirliğini (sıklığı - Day, aralığı - 1) günlük olarak ayarlanırsa, bir dilim günlük oluşturulur. <br/><br/>Ayrıntılar için bkz [Dataset kullanılabilirliği](#Availability). <br/><br/>Model dilimleme veri kümesi hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makalesi. |Evet |NA |
| ilke |Ölçüt ya da veri kümesi dilimler karşılamanız gerekmektedir koşulu tanımlar. <br/><br/>Ayrıntılar için bkz [Dataset İlkesi](#Policy) bölümü. |Hayır |NA |

## <a name="dataset-example"></a>Veri kümesi örneği
Aşağıdaki örnekte, adlı bir tablo veri kümesini temsil eden **MyTable** bir SQL veritabanında.

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
* **tableName** type özelliği (AzureSqlTable türüne belirli) MyTable için ayarlanır.
* **linkedServiceName** sonraki JSON parçacığında tanımlanan AzureSqlDatabase türündeki bağlı hizmetin başvuruyor. 
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

Gördüğünüz gibi bağlantılı hizmet bir SQL veritabanına bağlanma tanımlar. Hangi tablo girdi olarak kullanılır ve bir ardışık düzen etkinliğin çıktı veri kümesi tanımlar.   

> [!IMPORTANT]
> Bir veri kümesi ardışık düzen tarafından üretilen sürece bunu olarak işaretlenmelidir **dış**. Bu ayar genellikle bir ardışık düzendeki ilk etkinlik girişleri için de geçerlidir.   


## <a name="Type"></a> Veri kümesi türü
Veri kümesi türü kullandığınız veri deposunda bağlıdır. Data Factory ile desteklenen veri depoları listesi için aşağıdaki tabloya bakın. Bağlı hizmet ve bir veri kümesi, veri deposu için nasıl oluşturulacağını öğrenmek için bir veri deposu'ı tıklatın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Veri depolar ile * şirket içi olabilir veya hizmet (Iaas) olarak Azure altyapısı. Bu veri depolarına yüklemek ihtiyaç duyduğunuz [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).

Önceki bölümdeki örnekte dataset türünü ayarlamak **AzureSqlTable**. Benzer şekilde, veri kümesi türü Azure Blob veri kümesi için ayarlanmış **AzureBlob**aşağıdaki JSON gösterildiği gibi:

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
**Yapısı** bölümdür isteğe bağlıdır. Dataset şeması tarafından içeren bir koleksiyon adları ve sütunların veri türlerini tanımlar. Türleri dönüştürme ve kaynaktan hedef sütunlara eşlemek için kullanılan türü bilgileri sağlamak için yapısı bölümü kullanın. Aşağıdaki örnekte, üç sütun kümesi vardır: `slicetimestamp`, `projectname`, ve `pageviews`. Bunlar dize, dize ve ondalık, sırasıyla türüdür.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Her sütun yapısı içinde aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü.  |Hayır |
| Kültür |. Tür .NET türü olduğunda kullanılacak NET tabanlı kültürü: `Datetime` veya `Datetimeoffset`. Varsayılan değer `en-us`. |Hayır |
| Biçimi |Biçim türü .NET türü olduğunda kullanılacak dize: `Datetime` veya `Datetimeoffset`. |Hayır |

Aşağıdaki yönergeleri yapısı bilgileri içerecek şekilde ne zaman ve ne eklenecek belirlemenize yardımcı **yapısı** bölümü.

* **Yapılandırılmış veri kaynakları için**, yalnızca sütun havuz için kaynak sütunları eşlemek istediğiniz ve adlarının aynı olmayan yapısı bölüm belirtin. Bu türdeki yapılandırılmış veri kaynağının veri yanı sıra veri şeması ve tür bilgileri depolar. SQL Server, Oracle ve Azure tablo yapılandırılmış veri kaynakları örneklerindendir. 
  
    Tür bilgileri zaten yapılandırılmış veri kaynakları için kullanılabilir olduğundan yapısı bölüm eklediğinizde türü bilgileri içermemelidir.
* **Şema okuma veri kaynaklarında (özellikle Blob Depolama) için**, herhangi bir şema veya türü bilgi verilerle depolamadan veri depolamayı seçebilirsiniz. Sütunları havuz için kaynak sütunları eşleme istediğinizde bu tür veri kaynağı yapısı içerir. Ayrıca yapısı kopyalama etkinliği için bir giriş veri kümesi olduğunda ve kaynak veri kümesinin veri türleri yerel türleri için havuz dönüştürülüp içerir. 
    
    Veri Fabrikası yapısındaki türü bilgileri sağlamak için aşağıdaki değerleri destekler: **Int16, Int32, Int64, tek, Double, Decimal, bayt [], Boolean, dize, GUID, Datetime, Datetimeoffset ve Timespan**. Ortak dil belirtimi (CLS) bu değerler-uyumlu. NET tabanlı türü değerleri.

Veri Fabrikası tür dönüşümleri veri bir havuz veri deposu için bir kaynak veri deposundan taşırken otomatik olarak gerçekleştirir. 
  

## <a name="dataset-availability"></a>DataSet kullanılabilirliği
**Kullanılabilirlik** bir veri kümesi bölümünde veri kümesi için işleme penceresi (örneğin, saatlik, günlük veya haftalık) tanımlar. Etkinlik pencereleri hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md).

Aşağıdaki kullanılabilirlik bölümü çıktı veri kümesi saatlik oluşturulur veya giriş veri kümesi saatlik kullanılabilir belirtir:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Ardışık Düzen aşağıdaki başlangıç ve bitiş zamanları varsa:  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

Çıktı veri kümesi üretilen saatlik içinde ardışık düzeni başlangıç ve bitiş zamanları. Bu nedenle, bu ardışık düzen, her etkinlik penceresinin (00: 00-1 AM, 1 AM - 2 AM, 2: 00 - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM) için bir tane tarafından üretilen beş veri kümesi dilimler vardır. 

Aşağıdaki tabloda kullanılabilirlik bölümünde kullanabileceğiniz özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilim üretim için zaman birimini belirtir.<br/><br/><b>Sıklık desteklenen</b>: dakika, saat, gün, hafta, ay |Evet |NA |
| interval |Sıklığı çarpanı belirtir.<br/><br/>"X sıklığı aralığını" ne sıklıkta dilim üretilen belirler. Örneğin, veri kümesinin saatlik olarak başka bir dilimlenebilir gerekiyorsa, ayarladığınız <b>sıklığı</b> için <b>saat</b>, ve <b>aralığı</b> için <b>1</b>.<br/><br/>Belirtirseniz unutmayın **sıklığı** olarak **Minute**, aralık en az 15 olarak ayarlamanız gerekir. |Evet |NA |
| stili |Dilim başlangıç veya Bitiş aralığı olarak üretilen belirtir.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Varsa **sıklığı** ayarlanır **ay**, ve **stili** ayarlanır **EndOfInterval**, dilim ayın son gününde üretilir. Varsa **stili** ayarlanır **StartOfInterval**, dilim ayın ilk günü üretilir.<br/><br/>Varsa **sıklığı** ayarlanır **gün**, ve **stili** ayarlanır **EndOfInterval**, dilim günün son bir saat içinde oluşturulur.<br/><br/>Varsa **sıklığı** ayarlanır **saat**, ve **stili** ayarlanır **EndOfInterval**, dilim saat sonunda üretilir. Örneğin, 1 PM - 2 PM dönem için bir dilim için 2 saat dilimi oluşturulur. |Hayır |EndOfInterval |
| anchorDateTime |Veri kümesi dilim sınırlarını işlem için Zamanlayıcı tarafından kullanılan zaman içinde mutlak konum tanımlar. <br/><br/>Bu propoerty belirtilen sıklığından daha ayrıntılı tarih kısımlarını varsa, daha ayrıntılı bölümleri göz ardı edilir unutmayın. Örneğin, varsa **aralığı** olan **saatlik** (sıklığı: saat ve aralığı: 1) ve **anchorDateTime** içeren **dakika ve saniyeleri**, sonra dakika ve saniyeleri bölümlerini **anchorDateTime** göz ardı edilir. |Hayır |01/01/0001 |
| uzaklık |Tarafından başlangıç ve bitiş tüm veri kümesi dilim gölgeye Timespan. <br/><br/>Her iki unutmayın **anchorDateTime** ve **uzaklık** belirtilirse, sonucudur birleşik shift. |Hayır |NA |

### <a name="offset-example"></a>uzaklık örneği
Varsayılan olarak, her gün (`"frequency": "Day", "interval": 1`) dilimler 00: 00 (gece yarısı) Eşgüdümlü Evrensel Saat (UTC) başlatın. Başlangıç zamanı 6 AM UTC saati yerine olmasını istiyorsanız, uzaklık aşağıdaki kod parçacığında gösterildiği gibi ayarlayın: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime örneği
Aşağıdaki örnekte, veri kümesi 23 saatte bir kez oluşturulur. İlk dilim tarafından belirtilen zaman başlar **anchorDateTime**, ayarlanmış `2017-04-19T08:00:00` (UTC).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>uzaklık/stil örneği
Aşağıdaki veri kümesi aylık ve 8: 00'da her ayın 3 üretilir (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>Veri kümesi İlkesi
**İlkesi** veri kümesi tanımı bölümünde ölçütleri veya veri kümesi dilimler karşılamanız gerekmektedir koşulu tanımlar.

### <a name="validation-policies"></a>Doğrulama ilkeleri
| İlke adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Doğrular verilerde **Azure Blob Depolama** (megabayt cinsinden) en düşük boyut gereksinimlerini karşılıyor. |Azure Blob depolama |Hayır |NA |
| minimumRows |Doğrular verilerde bir **Azure SQL veritabanı** veya bir **Azure tablo** en az satır sayısını içerir. |<ul><li>Azure SQL veritabanı</li><li>Azure tablosu</li></ul> |Hayır |NA |

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
Dış veri kümeleri veri fabrikası'nda çalışan bir ardışık düzen tarafından üretilmeyen olanlardır. Veri kümesi olarak işaretlenmişse **dış**, **ExternalData** İlkesi, veri kümesi dilim kullanılabilirlik davranışını etkilemek için tanımlanabilir.

Bir veri kümesi Data Factory tarafından üretilen sürece bunu olarak işaretlenmelidir **dış**. Etkinlik veya ardışık düzen zincirleme kullanılmadığı sürece bu ayar genellikle ardışık düzendeki, ilk etkinliğin girdi için geçerlidir.

| Ad | Açıklama | Gerekli | Varsayılan değer |
| --- | --- | --- | --- |
| dataDelay |Verilen dilim için dış veri kullanılabilirliğini onay gecikme süresi. Örneğin, bu ayarı kullanarak bir saatlik onay geciktirebilir.<br/><br/>Ayar, yalnızca mevcut süre için geçerlidir.  Örneğin, 1:00 PM şimdi ise ve bu değer 10 dakikadır doğrulama 13: 10'te başlatır.<br/><br/>Bu ayar geçmişte dilimler etkilemez unutmayın. İle dilimler **dilim bitiş saati** + **dataDelay** < **şimdi** herhangi bir gecikme işlenir.<br/><br/>23:59 büyük saatleri saat kullanarak belirtilmelidir `day.hours:minutes:seconds` biçimi. Örneğin, 24 saat belirtmek için 24:00:00 kullanmayın. Bunun yerine, 1.00:00:00 kullanın. 24:00:00 kullanırsanız, 24 gün (24.00:00:00) kabul edilir. 1 gün ve 4 saat için 1:04:00:00 belirtin. |Hayır |0 |
| Retryınterval |Bir hata ve sonraki denemesi arasındaki bekleme süresi. Bu ayar için mevcut zaman geçerlidir. Önceki başarısız çalışırsanız, sonraki deneyin sonradır **Retryınterval** süresi. <br/><br/>1:00 PM şimdi ise, ilk denemede başlamadan. İlk doğrulama denetimi tamamlamak için süre 1 dakika ve işlem başarısız oldu, sonraki yeniden deneme ise 1:00 1 dak (süresi) + 1 dak (yeniden deneme aralığı) = 1:02 PM. <br/><br/>Geçmişte dilimler için gecikme yoktur. Yeniden deneme hemen gerçekleşir. |Hayır |00:01:00 (1 dakika) |
| retryTimeout |Yeniden deneme girişimleri için zaman aşımı.<br/><br/>Bu özellik 10 dakika olarak ayarlanırsa, doğrulama 10 dakika içinde tamamlanmalıdır. Yeniden deneme doğrulamayı gerçekleştirmek için 10 dakikadan uzun sürüyorsa, zaman aşımına uğradı.<br/><br/>Tüm denemeleri doğrulama zaman aşımı için dilim olarak işaretlenmişse, **süresi sona erdi**. |Hayır |00:10:00 (10 dakika) |
| maximumRetry |Kaç kez dış veri kullanılabilirliğini denetleyin. Değer izin verilen en fazla 10'dur. |Hayır |3 |


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu araçlar ya da SDK'ları birini kullanarak veri kümeleri oluşturabilirsiniz: 

- Kopyalama Sihirbazı 
- Azure portalı
- Visual Studio
- PowerShell
- Azure Resource Manager şablonu
- REST API
- .NET API’si

Bu araçlar ya da SDK'ları birini kullanarak ardışık düzen ve veri kümeleri oluşturmak için aşağıdaki öğreticileri adım adım yönergeler için bkz:
 
- [Veri dönüştürme etkinliğine sahip işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)
- [Veri taşıma etkinliği ile işlem hattı oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Ardışık düzenin oluşturulan ve dağıtılan sonra yönetebilir ve Azure portal dikey penceresi veya izleme ve yönetim uygulaması kullanılarak işlem hatlarınızı izlemek. Adım adım yönergeler için aşağıdaki konulara bakın: 

- [İzleme ve Azure portal dikey penceresi kullanılarak işlem hatlarını yönetme](data-factory-monitor-manage-pipelines.md)
- [İzleme ve işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetme](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a>Kapsamlı veri kümeleri
Kullanarak bir işlem hattı için kapsamlı veri kümeleri oluşturabilirsiniz **veri kümeleri** özelliği. Bu veri kümeleri yalnızca kullanılabilir etkinlikler içinde bu ardışık düzen tarafından diğer ardışık etkinlikler tarafından değil. Aşağıdaki örnek, iki veri kümesi (InputDataset rdc ve OutputDataset rdc) içinde ardışık düzeni kullanılacak sahip işlem hattı tanımlar.  

> [!IMPORTANT]
> Kapsamlı veri kümeleri, yalnızca tek seferlik ardışık düzen ile desteklenir (burada **pipelineMode** ayarlanır **OneTime**). Bkz: [Onetime ardışık düzen](data-factory-create-pipelines.md#onetime-pipeline) Ayrıntılar için.
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
- Ardışık Düzen hakkında daha fazla bilgi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md). 
- Ardışık Düzen nasıl zamanlanmış ve yürütülen hakkında daha fazla bilgi için bkz: [zamanlama ve yürütme Azure Data factory'de](data-factory-scheduling-and-execution.md). 
