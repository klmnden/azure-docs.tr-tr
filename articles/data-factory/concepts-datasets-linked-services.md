---
title: Veri kümeleri ve bağlı Azure Data Factory Hizmetleri'nde | Microsoft Docs
description: Veri kümeleri ve veri fabrikasında bağlı hizmetler hakkında bilgi edinin. Bağlı hizmetler data factory işlem/veri depoları bağlayın. Veri kümeleri girdi/çıktı verilerini temsil eder.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: 93729646cf1a501b5502e2666ed68944fe474f72
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34616014"
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

Şimdi, bir **dataset** adlandırılmış bir görünüm yalnızca işaret veya kullanmak istediğiniz verileri başvuruda bulunan verilerin, **etkinlikleri** girişleri ve çıkışları olarak. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin Azure Blob veri kümesi, etkinliğin verileri okuması için gereken blob kapsayıcısını ve Blob depolama klasörünü belirtir.

Bir veri kümesi oluşturmadan önce oluşturmanız gerekir bir **bağlantılı hizmeti** data factory veri deponuza bağlamak için. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu, bu şekilde düşünün; veri kümesi bağlantılı veri depoları içindeki verilerin yapısını temsil eder ve veri kaynağına bağlantı bağlantılı hizmet tanımlar. Örneğin, bir Azure depolama hizmeti bağlantıları bir depolama hesabı data factory bağlantılı. Bir Azure Blob dataset blob kapsayıcısında ve işlenmesi için girdi BLOB, Azure depolama hesabı klasördeki temsil eder.

Bir örnek senaryo aşağıda verilmiştir. Blob depolama alanından bir SQL veritabanına veri kopyalamak için iki bağlı hizmet oluşturma: Azure Storage ve Azure SQL veritabanı. Ardından, iki veri kümesi oluşturun: (Azure Storage bağlı hizmeti ifade eder) Azure Blob veri kümesi ve (Azure SQL bağlı veritabanı hizmeti ifade eder) Azure SQL tablosu veri kümesi. Azure Storage ve Azure SQL veritabanı bağlı Hizmetleri, Azure Storage ve Azure SQL Database, sırasıyla bağlanmak için çalışma zamanında Data Factory kullandığı bağlantı dizeleri içerir. Azure Blob dataset blob kapsayıcısı ve Blob Depolama alanınızın giriş bloblar içeren blob klasörü belirtir. Azure SQL tablosu veri kümesi SQL tablosu SQL veritabanınız veri ekleneceğine dair belirtir.

Aşağıdaki diyagramda, veri fabrikasında ardışık düzen, etkinlik, veri kümesi ve bağlı hizmet arasındaki ilişkiler gösterilmektedir:

![Ardışık düzen, etkinlik, veri kümesi, bağlı hizmetler arasındaki ilişki](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>JSON bağlantılı hizmeti
Veri fabrikasında bağlı hizmet JSON biçiminde şu şekilde tanımlanır:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Aşağıdaki tabloda yukarıdaki JSON özelliklerinde açıklanmaktadır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
ad | Bağlı hizmetin adı. Bkz: [Azure Data Factory - adlandırma kuralları](naming-rules.md). |  Evet |
type | Bağlantılı hizmet türü. Örneğin: AzureStorage (veri deposu) veya AzureBatch (işlem). TypeProperties açıklamasına bakın. | Evet |
typeProperties | Tür özellikleri için her bir veri deposu farklı veya işlem. <br/><br/> İçin desteklenen veri türlerini ve bunların türü özelliklerini depolamak için bkz: [veri kümesi türü](#dataset-type) bu makalede tablo. Bir veri depolama alanına belirli tür özellikleri hakkında bilgi edinmek için veri deposu bağlayıcı makale gidin. <br/><br/> Desteklenen işlem türlerini ve bunların tür özellikleri için bkz [işlem bağlı Hizmetleri](compute-linked-services.md). | Evet |
connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır

## <a name="linked-service-example"></a>Bağlantılı hizmet örneği
Aşağıdaki bağlantılı hizmeti bir Azure Storage bağlı hizmetidir. Türü için AzureStorage ayarlandığına dikkat edin. Tür özellikleri Azure Storage bağlı hizmeti için bir bağlantı dizesi içerir. Data Factory hizmetinin çalışma zamanında veri deposuna bağlanmak için bu bağlantı dizesini kullanır. 

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

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

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
ad | Veri kümesi adı. Bkz: [Azure Data Factory - adlandırma kuralları](naming-rules.md). |  Evet |
type | Veri kümesi türü. Data Factory ile desteklenen türlerden biri belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için bkz [Dataset türleri](#dataset-type). | Evet |
yapısı | Veri kümesi şemasını. Ayrıntılar için bkz [veri kümesi yapısı](#dataset-structure). | Hayır |
typeProperties | Tür özellikleri her türü için farklı (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında daha fazla bilgi için bkz: [veri kümesi türü](#dataset-type). | Evet |

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
**Yapısı** bölümdür isteğe bağlıdır. Dataset şeması tarafından içeren bir koleksiyon adları ve sütunların veri türlerini tanımlar. Türleri dönüştürme ve kaynaktan hedef sütunlara eşlemek için kullanılan türü bilgileri sağlamak için yapısı bölümü kullanın.

Her sütun yapısı içinde aşağıdaki özellikleri içerir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Sütunun adı. | Evet
type | Sütunun veri türü. Veri Fabrikası izin verilen değerler olarak aşağıdaki geçici veri türlerini destekler: **Int16, Int32, Int64, tek, Double, Decimal, bayt [], Boolean, dize, GUID, Datetime, Datetimeoffset ve Timespan** | Hayır
Kültür | . Tür .NET türü olduğunda kullanılacak NET tabanlı kültürü: `Datetime` veya `Datetimeoffset`. Varsayılan değer `en-us`. | Hayır
Biçimi | Biçim türü .NET türü olduğunda kullanılacak dize: `Datetime` veya `Datetimeoffset`. Başvurmak [özel tarih ve saat biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) datetime biçimine üzerinde. | Hayır

### <a name="example"></a>Örnek
Aşağıdaki örnekte, Blob veri kaynağı CSV biçiminde olduğunu ve üç sütun içeren varsayalım: UserID, adı ve lastlogindate. Bunlar Int64, dize ve Datetime haftanın günü için kısaltılmış Fransızca adları kullanarak bir özel datetime biçimiyle türüdür.

Blob veri kümesi yapısı sütunlar için tür tanımları birlikte aşağıdaki gibi tanımlayın:

```json
"structure":
[
    { "name": "userid", "type": "Int64"},
    { "name": "name", "type": "String"},
    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
]
```

### <a name="guidance"></a>Rehber

Aşağıdaki yönergeleri yapısı bilgileri içerecek şekilde ne zaman ve ne eklenecek anlamanıza yardımcı **yapısı** bölümü. Veri Fabrikası havuz için kaynak verilerini nasıl eşlendiğini ve zaman yapısı bilgileri belirtmek daha fazla bilgi edinin [şema ve tür eşlemesi](copy-activity-schema-and-type-mapping.md).

- **Güçlü şema veri kaynakları için**, yalnızca sütun havuz için kaynak sütunları eşlemek istediğiniz ve adlarının aynı olmayan yapısı bölüm belirtin. Bu türdeki yapılandırılmış veri kaynağının veri yanı sıra veri şeması ve tür bilgileri depolar. SQL Server, Oracle ve Azure SQL veritabanı yapılandırılmış veri kaynakları örneklerindendir.<br/><br/>Tür bilgileri zaten yapılandırılmış veri kaynakları için kullanılabilir olduğundan yapısı bölüm eklediğinizde türü bilgileri içermemelidir.
- **İçin yok/zayıf şeması örneğin metin dosyasında blob depolama veri kaynakları**, yapısı kopyalama etkinliği için bir giriş veri kümesi olduğunda ve kaynak veri kümesinin veri türleri dönüştürülmesi gereken havuz için yerel türler içerir. Ve sütunları havuz için kaynak sütunları eşleme istediğinizde yapısı ekleyin...

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
