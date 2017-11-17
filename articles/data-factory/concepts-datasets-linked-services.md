---
title: "Veri kümeleri ve bağlı Azure Data Factory Hizmetleri'nde | Microsoft Docs"
description: "Veri kümeleri ve veri fabrikasında bağlı hizmetler hakkında bilgi edinin. Bağlı hizmetler data factory işlem/veri depoları bağlayın. Veri kümeleri girdi/çıktı verilerini temsil eder."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: 
ms.date: 09/05/2017
ms.author: shlo
ms.openlocfilehash: a13e19c7e1a22581b14d1a96e20b8a649c303fc3
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="datasets-and-linked-services-in-azure-data-factory"></a>Veri kümelerini ve Azure Data Factory öğesinde bağlantılı hizmet 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-create-datasets.md)
> * [Sürüm 2 - Önizleme](concepts-datasets-linked-services.md)

Bu makalede hangi veri kümeleri, JSON biçiminde nasıl tanımlanan açıklanmıştır ve içinde kullanılan nasıl Azure veri fabrikası V2 ardışık düzenleri. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 kümelerinde](v1/data-factory-create-datasets.md).

Data factory'yi yeni istiyorsanız bkz [Azure Data Factory'ye giriş](introduction.md) bir genel bakış. 

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. A **ardışık düzen** mantıksal bir gruplandırmasıdır **etkinlikleri** görev birlikte gerçekleştirin. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, bir şirket içi SQL Server'dan Azure Blob depolama alanına veri kopyalamak için kopyalama etkinliği kullanabilirsiniz. Ardından, veri işlemek için çıktı verileri üretemedi Blob depolama alanından gönderilmiş olan bir Azure Hdınsight kümesinde bir Hive betiği çalıştıran bir Hive etkinliği kullanabilir. Son olarak, çıktı verilerini Azure SQL Data Warehouse için çözümleri yerleşik raporlama hangi iş zekası üstünde (BI) kopyalamak için ikinci bir kopyalama etkinliği kullanabilirsiniz. Ardışık Düzen ve etkinlikleri hakkında daha fazla bilgi için bkz: [işlem hatlarının ve etkinliklerin](concepts-pipelines-activities.md) Azure veri fabrikası'nda.

Şimdi, bir **dataset** adlandırılmış bir görünüm yalnızca işaret veya kullanmak istediğiniz verileri başvuruda bulunan verilerin, **etkinlikleri** girişleri ve çıkışları olarak. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin, bir Azure Blob veri kümesini etkinlik verileri okumalısınız Blob depolama alanına blob kapsayıcısı ve klasörü belirtir.

Bir veri kümesi oluşturmadan önce oluşturmanız gerekir bir **bağlantılı hizmeti** data factory veri deponuza bağlamak için. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu, bu şekilde düşünün; veri kümesi bağlantılı veri depoları içindeki verilerin yapısını temsil eder ve veri kaynağına bağlantı bağlantılı hizmet tanımlar. Örneğin, bir Azure depolama hizmeti bağlantıları bir depolama hesabı data factory bağlantılı. Bir Azure Blob dataset blob kapsayıcısında ve işlenmesi için girdi BLOB, Azure depolama hesabı klasördeki temsil eder.

Bir örnek senaryo aşağıda verilmiştir. Blob depolama alanından bir SQL veritabanına veri kopyalamak için iki bağlı hizmet oluşturma: Azure Storage ve Azure SQL veritabanı. Ardından, iki veri kümesi oluşturun: (Azure Storage bağlı hizmeti ifade eder) Azure Blob veri kümesi ve (Azure SQL bağlı veritabanı hizmeti ifade eder) Azure SQL tablosu veri kümesi. Azure Storage ve Azure SQL veritabanı bağlı Hizmetleri, Azure Storage ve Azure SQL Database, sırasıyla bağlanmak için çalışma zamanında Data Factory kullandığı bağlantı dizeleri içerir. Azure Blob dataset blob kapsayıcısı ve Blob Depolama alanınızın giriş bloblar içeren blob klasörü belirtir. Azure SQL tablosu veri kümesi SQL tablosu SQL veritabanınız veri ekleneceğine dair belirtir.

Aşağıdaki diyagramda, veri fabrikasında ardışık düzen, etkinlik, veri kümesi ve bağlı hizmet arasındaki ilişkiler gösterilmektedir:

![Ardışık düzen, etkinlik, veri kümesi, bağlı hizmetler arasındaki ilişki](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>JSON veri kümesi
Veri fabrikasında bir veri kümesini JSON biçiminde şu şekilde tanımlanır:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                 "type": "LinkedServiceReference",
        },
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}

```
Aşağıdaki tabloda yukarıdaki JSON özelliklerinde açıklanmaktadır:

Özellik | Açıklama | Gerekli | Varsayılan
-------- | ----------- | -------- | -------
ad | Veri kümesi adı. | Bkz: [Azure Data Factory - adlandırma kuralları](naming-rules.md). | Evet | NA
type | Veri kümesi türü. | Data Factory ile desteklenen türlerden biri belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için bkz [Dataset türleri](#dataset-types). | Evet | NA
yapısı | Veri kümesi şemasını. | Ayrıntılar için bkz [veri kümesi yapısı](#dataset-structure). | Hayır | NA
typeProperties | Tür özellikleri her türü için farklı (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında daha fazla bilgi için bkz: [veri kümesi türü](#dataset-type). | Evet | NA

## <a name="dataset-example"></a>Veri kümesi örneği
Aşağıdaki örnekte, bir SQL veritabanında MyTable adlı bir tablo veri kümesini temsil eder.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": {
                "referenceName": "MyAzureSqlLinkedService",
                 "type": "LinkedServiceReference",
        },
        "typeProperties":
        {
            "tableName": "MyTable"
        },
    }
}

```
Aşağıdaki noktalara dikkat edin:

- türü için AzureSqlTable ayarlanır.
- tableName type özelliği (AzureSqlTable türüne belirli) MyTable için ayarlanır.
- bağlı hizmet türü sonraki JSON parçacığında tanımlanan AzureSqlDatabase linkedServiceName başvurur.

## <a name="linked-service-example"></a>Bağlantılı hizmet örneği
AzureSqlLinkedService şu şekilde tanımlanır:

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

- **tür** AzureSqlDatabase için ayarlanır.
- **connectionString** türü özelliği, bir SQL veritabanına bağlanmak için gereken bilgileri belirtir.

Gördüğünüz gibi bağlantılı hizmet bir SQL veritabanına bağlanma tanımlar. Hangi tablo girdi olarak kullanılır ve bir ardışık düzen etkinliğin çıktı veri kümesi tanımlar.

## <a name="dataset-type"></a>Veri kümesi türü
Kullandığınız veri deposu bağlı olarak veri kümeleri, birçok farklı türde vardır. Data Factory ile desteklenen veri depoları listesi için aşağıdaki tabloya bakın. Bağlı hizmet ve bir veri kümesi, veri deposu için nasıl oluşturulacağını öğrenmek için bir veri deposu'ı tıklatın.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

Önceki bölümdeki örnekte dataset türünü ayarlamak **AzureSqlTable**. Benzer şekilde, veri kümesi türü Azure Blob veri kümesi için ayarlanmış **AzureBlob**aşağıdaki JSON gösterildiği gibi:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
                "referenceName": "MyAzureStorageLinkedService",
                 "type": "LinkedServiceReference",
        }, 
 
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        }
    }
}
```
## <a name="dataset-structure"></a>Veri kümesi yapısı
**Yapısı** bölümdür isteğe bağlıdır. Dataset şeması tarafından içeren bir koleksiyon adları ve sütunların veri türlerini tanımlar. Türleri dönüştürme ve kaynaktan hedef sütunlara eşlemek için kullanılan türü bilgileri sağlamak için yapısı bölümü kullanın. Aşağıdaki örnekte, üç sütun kümesi vardır: zaman damgası, projectname ve pageviews. Bunlar dize, dize ve ondalık, sırasıyla türüdür.

```json
[
    { "name": "timestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Her sütun yapısı içinde aşağıdaki özellikleri içerir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Sütunun adı. | Evet
type | Sütunun veri türü. | Hayır
Kültür | . Tür .NET türü olduğunda kullanılacak NET tabanlı kültürü: `Datetime` veya `Datetimeoffset`. Varsayılan değer `en-us`. | Hayır
Biçimi | Biçim türü .NET türü olduğunda kullanılacak dize: `Datetime` veya `Datetimeoffset`. | Hayır

Aşağıdaki yönergeleri yapısı bilgileri içerecek şekilde ne zaman ve ne eklenecek belirlemenize yardımcı **yapısı** bölümü.

- **Yapılandırılmış veri kaynakları için**, yalnızca sütun havuz için kaynak sütunları eşlemek istediğiniz ve adlarının aynı olmayan yapısı bölüm belirtin. Bu türdeki yapılandırılmış veri kaynağının veri yanı sıra veri şeması ve tür bilgileri depolar. SQL Server, Oracle ve Azure SQL veritabanı yapılandırılmış veri kaynakları örneklerindendir.<br/><br/>Tür bilgileri zaten yapılandırılmış veri kaynakları için kullanılabilir olduğundan yapısı bölüm eklediğinizde türü bilgileri içermemelidir.
- **Şema okuma veri kaynaklarında (özellikle Blob Depolama) için**, herhangi bir şema veya türü bilgi verilerle depolamadan veri depolamayı seçebilirsiniz. Sütunları havuz için kaynak sütunları eşleme istediğinizde bu tür veri kaynağı yapısı içerir. Ayrıca yapısı kopyalama etkinliği için bir giriş veri kümesi olduğunda ve kaynak veri kümesinin veri türleri yerel türleri için havuz dönüştürülüp içerir.<br/><br/> Veri Fabrikası yapısındaki türü bilgileri sağlamak için aşağıdaki değerleri destekler: `Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan`. 

Veri Fabrikası gelen havuz için kaynak verilerini nasıl eşlendiğini hakkında daha fazla bilgi edinin [şema ve tür eşlemesi]( copy-activity-schema-and-type-mapping.md) ve ne zaman yapısı bilgileri belirtin.

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu araçları veya Sdk'lardan birini kullanarak veri kümeleri oluşturabilirsiniz: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager şablonu ve Azure portalı

## <a name="v1-vs-v2-datasets"></a>V1 vs. V2 veri kümeleri

Veri Fabrikası v1 ve v2 veri kümeleri arasındaki bazı farklar aşağıda verilmiştir: 

- Dış özellik v2'desteklenmiyor. Tarafından değiştirilen bir [tetikleyici](concepts-pipeline-execution-triggers.md).
- İlke ve kullanılabilirlik özellikler V2'desteklenmiyor. Bir ardışık düzeni için başlangıç saatini bağlıdır [Tetikleyicileri](concepts-pipeline-execution-triggers.md).
- Kapsamlı veri kümeleri (ardışık düzeninde tanımlanan veri kümeleri) V2'desteklenmez. 

## <a name="next-steps"></a>Sonraki adımlar
Bu araçlar ya da SDK'ları birini kullanarak ardışık düzen ve veri kümeleri oluşturmak için aşağıdaki Öğreticisi Adım adım yönergeler için bkz. 

- [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Hızlı Başlangıç: PowerShell kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md)
- [Hızlı Başlangıç: REST API kullanarak bir veri fabrikası oluşturun](quickstart-create-data-factory-rest-api.md)
- Hızlı Başlangıç: Azure portalını kullanarak bir veri fabrikası oluşturun
