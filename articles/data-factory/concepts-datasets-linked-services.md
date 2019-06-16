---
title: Azure Data factory'deki veri kümelerini | Microsoft Docs
description: Data factory'deki veri kümelerini hakkında bilgi edinin. Veri kümeleri, girdi/çıktı verilerini temsil eder.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: shlo
ms.openlocfilehash: 6b74f217d296b5de8886f608b1bc92e908b5d8b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64866461"
---
# <a name="datasets-in-azure-data-factory"></a>Azure Data factory'deki veri kümelerini
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-create-datasets.md)
> * [Geçerli sürüm](concepts-datasets-linked-services.md)

Bu makale, JSON biçiminde nasıl tanımlandığına hangi veri kümelerinin olduğunu açıklar ve nasıl kullanıldığına Azure Data Factory işlem hatları.

Data Factory kullanmaya yeni başladıysanız bkz [Azure Data Factory'ye giriş](introduction.md) genel bakış.

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. A **işlem hattı** mantıksal bir gruplandırmasıdır **etkinlikleri** birlikte gerçekleştiren bir görev. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Artık, bir **veri kümesi** yalnızca işaret eden veya kullanmak istediğiniz verilere başvuran verilerin adlandırılmış bir görünümüdür olduğundan, **etkinlikleri** girdi ve çıktı olarak. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin Azure Blob veri kümesi, etkinliğin verileri okuması için gereken blob kapsayıcısını ve Blob depolama klasörünü belirtir.

Bir veri kümesi oluşturmadan önce oluşturmanız gerekir bir [ **bağlı hizmet** ](concepts-linked-services.md) data factory'de veri deponuza bağlamak için. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu, bu şekilde düşünün; veri kümesi bağlı veri depolarındaki veri yapısını temsil eder ve bağlı hizmet, veri kaynağına bağlantı tanımlar. Örneğin, bir Azure depolama bir depolama hesabını veri fabrikasına bağlı hizmeti. Bir Azure Blob veri kümesi blob kapsayıcıyı ve işlenecek giriş bloblarını içeren Azure depolama hesap dahilindeki klasörü temsil eder.

Örnek senaryo aşağıda verilmiştir. Verileri Blob depolama alanından SQL veritabanına kopyalamak için iki bağlı hizmet oluşturursunuz: Azure depolama ve Azure SQL veritabanı. Ardından, iki veri kümesi oluşturursunuz: (Azure depolama bağlı hizmetini ifade eder) azure Blob veri kümesi ve Azure SQL tablosu veri kümesi (Bu, Azure SQL veritabanı bağlı hizmetini ifade eder). Azure depolama ve Azure SQL veritabanı bağlı hizmeti, Data Factory, Azure depolama ve Azure SQL veritabanı, sırasıyla bağlanmak için çalışma zamanında kullandığı bağlantı dizeleri içerir. Azure Blob veri kümesi blob kapsayıcısı ve Blob Depolama alanınızda giriş bloblarını içeren blob klasörü belirtir. Azure SQL tablosu veri kümesi, verilerin kopyalanacağı olduğu SQL veritabanınızda SQL tablosunu belirtir.

Aşağıdaki diyagramda, Data Factory'de işlem hattı, etkinlik, veri kümesi ve bağlı hizmet arasındaki ilişkiler gösterilmektedir:

![İşlem hattı, etkinlik, veri kümesi, bağlı hizmetler arasındaki ilişki](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)


## <a name="dataset-json"></a>Dataset JSON
Bir veri kümesinde Data Factory aşağıdaki JSON biçiminde tanımlanır:

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
Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanmaktadır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
name | Veri kümesinin adı. Bkz: [Azure Data Factory - adlandırma kuralları](naming-rules.md). |  Evet |
türü | Veri kümesi türü. Data Factory tarafından desteklenen türlerinden birini belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için bkz [veri kümesi türleri](#dataset-type). | Evet |
structure | Şema kümesi. Ayrıntılar için bkz [veri kümesi şemasını](#dataset-structure-or-schema). | Hayır |
typeProperties | Tür özellikleri her türü için farklı (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında daha fazla bilgi için bkz: [veri kümesi türü](#dataset-type). | Evet |

### <a name="data-flow-compatible-dataset"></a>Veri akışı uyumlu veri kümesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Bkz: [desteklenen veri türleri](#dataset-type) olan veri kümesi türlerinin bir listesi için [veri akışı](concepts-data-flow-overview.md) uyumlu. Veri akışı için uyumlu olan veri kümeleri, dönüştürmeleri için ayrıntılı veri kümesi tanımları gerektirir. Bu nedenle, JSON tanımı biraz farklıdır. Yerine bir _yapısı_ veri akışı uyumlu olan veri kümeleri özelliğe sahip bir _şema_ özelliği.

Veri kümeleri, veri akışı kaynak ve havuz dönüşümlerini kullanılır. Veri kümeleri temel veri şemaları tanımlayın. Verilerinizi hiçbir şema varsa, kaynak ve havuz için şema değişikliklerini kullanabilirsiniz. Şema kümesindeki şekli ve fiziksel veri türünü temsil eder.

Veri kümesi şema tanımlayarak ilişkili bağlı hizmetinden ilgili veri türleri, veri biçimleri, dosya konumu ve bağlantı bilgilerini alırsınız. Meta veri kümelerinden kaynak dönüşümünüzü kaynağı olarak görünür *projeksiyon*. Projeksiyonun kaynak dönüşümü, veri akışı veri tanımlanmış adları ve türleri ile temsil eder.

Bir veri akışı veri kümesi şemasını içeri aktardığınızda seçin **şema içeri aktarma** düğmesine tıklayın ve kaynak veya yerel bir dosyadan içeri aktarmak seçin. Çoğu durumda, doğrudan kaynaktan Şemayı içeri aktaracağız. Ancak, bir yerel şema dosyası (Parquet dosya veya üst bilgi içeren CSV) zaten varsa, bu dosyada şema temel Data Factory'nin yönlendirebilir.


```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "schema": [
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

Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanmaktadır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
name | Veri kümesinin adı. Bkz: [Azure Data Factory - adlandırma kuralları](naming-rules.md). |  Evet |
türü | Veri kümesi türü. Data Factory tarafından desteklenen türlerinden birini belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için bkz [veri kümesi türleri](#dataset-type). | Evet |
schema | Şema kümesi. Ayrıntılar için bkz [uyumlu veri kümeleri veri akışı](#dataset-type). | Hayır |
typeProperties | Tür özellikleri her türü için farklı (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında daha fazla bilgi için bkz: [veri kümesi türü](#dataset-type). | Evet |


## <a name="dataset-example"></a>Veri kümesi örneği
Aşağıdaki örnekte, bir SQL veritabanı'nda MyTable adlı bir tablo veri kümesini temsil eder.

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

- tür AzureSqlTable için ayarlanır.
- tableName türü özellik (AzureSqlTable türüne özel) MyTable için ayarlanır.
- linkedServiceName sonraki JSON kod parçacığında tanımlanan AzureSqlDatabase türünde bir bağlı hizmetini ifade eder.

## <a name="dataset-type"></a>Veri kümesi türü
Farklı türlerde veri kümeleri, kullandığınız veri deposuna bağlı olarak vardır. Data Factory tarafından desteklenen veri depolarının listesi için aşağıdaki tabloya bakın. Bir veri deposunu bağlı hizmet ve bu veri deposu için bir veri kümesi oluşturma hakkında bilgi edinmek için tıklayın.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-dataflow.md)]

Önceki bölümdeki örnekte, veri kümesi türü kümesine **AzureSqlTable**. Benzer şekilde, bir Azure Blob veri kümesi için veri kümesi türü ayarlanacağını **AzureBlob**aşağıdaki JSON'da gösterildiği gibi:

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

## <a name="dataset-structure-or-schema"></a>DataSet yapısını veya şema
**Yapısı** bölüm veya **şema** bölüm DataSet (veri akışı uyumlu), isteğe bağlıdır. Bu veri kümesi şemasını içeren bir koleksiyon adları ve sütunların veri türlerini tarafından tanımlar. Türleri dönüştürme ve kaynaktan hedef sütunlara eşlemek için kullanılan tür bilgileri sağlamak için yapı bölümünü kullanın.

Her sütunda yapısı aşağıdaki özellikleri içerir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Sütunun adı. | Evet
type | Sütunun veri türü. Data Factory izin verilen değerler aşağıdaki geçici veri türlerini destekler: **Int16, Int32, Int64, tek, Double, ondalık, bayt [], Boolean, dize, Guid, Datetime, Datetimeoffset ve Timespan** | Hayır
culture | . Türü bir .NET türü olduğunda kullanılacak kültürü NET tabanlı: `Datetime` veya `Datetimeoffset`. Varsayılan değer: `en-us`. | Hayır
format | Biçim türü .NET türü olduğunda kullanılacak dize: `Datetime` veya `Datetimeoffset`. Başvurmak [özel tarih ve saat biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) datetime biçimine üzerinde. | Hayır

### <a name="example"></a>Örnek
Aşağıdaki örnekte, kaynak Blob verilerini CSV biçiminde ve üç sütun içeren varsayalım: kullanıcı kimliği, adı ve lastlogindate. Bunlar Int64, dize ve Datetime haftanın günü için Fransızca kısaltılmış kullanarak özel bir tarih saat biçiminde türüdür.

Blob veri kümesi yapısı sütunları için tür tanımları ile birlikte aşağıdaki gibi tanımlayın:

```json
"structure":
[
    { "name": "userid", "type": "Int64"},
    { "name": "name", "type": "String"},
    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
]
```

### <a name="guidance"></a>Rehber

Aşağıdaki yönergeleri yapı bilgileri içerecek şekilde ne zaman ve ne eklenecek anlamanıza yardımcı **yapısı** bölümü. Veri Fabrikası havuz için kaynak verilerini nasıl eşlendiğini ve yapı bilgilerini belirtmek ne zaman üzerinde daha fazla bilgi edinin [şema ve tür eşlemesi](copy-activity-schema-and-type-mapping.md).

- **Güçlü şema veri kaynakları için**, yalnızca sütunları havuz için kaynak sütunları eşlemek istediğiniz ve adları aynı değildir yapısı kısmında belirtin. Bu türdeki yapılandırılmış veri kaynağının veri yanı sıra veri şema ve tür bilgilerini depolar. SQL Server, Oracle ve Azure SQL veritabanı yapılandırılmış veri kaynağı örnekleri içerir.<br/><br/>Tür bilgilerini zaten yapılandırılmış veri kaynakları için mevcut olduğundan, yapısı Bölümü eklediğinizde türü bilgi içermemelidir.
- **Hayır/zayıf şema için örneğin metin dosyası blob depolama alanındaki veri kaynakları**, bir kopyalama etkinliği için girdi veri kümesidir ve kaynak veri kümesi veri türlerini havuz için yerel türlerine dönüştürülmesi gereken yapısı içerir. Ve sütunları havuz için kaynak sütunları eşlemek istediğiniz yapısı işlemlerinde...

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu araçlar ve SDK'lar birini kullanarak veri kümeleri oluşturabilirsiniz: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager şablonu ve Azure portalı

## <a name="current-version-vs-version-1-datasets"></a>Sürüm 1 veri kümeleri ve geçerli sürüm

Data Factory ile veri fabrikası sürüm 1 veri kümeleri arasındaki bazı farklar şunlardır:

- Dış özellik geçerli sürümde desteklenmiyor. Tarafından değiştirilen bir [tetikleyici](concepts-pipeline-execution-triggers.md).
- İlke ve kullanılabilirlik özellikleri geçerli sürümünde desteklenmez. Bağımlı bir işlem hattı için başlangıç saatini [Tetikleyicileri](concepts-pipeline-execution-triggers.md).
- Kapsamı belirlenmiş veri kümeleri (ardışık düzeninde tanımlanan veri kümeleri), geçerli sürümde desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Bu araçlar ve SDK'lar birini kullanarak işlem hatlarını ve veri kümeleri oluşturmak için adım adım yönergeler için aşağıdaki öğreticiye bakın.

- [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Hızlı Başlangıç: PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md)
- [Hızlı Başlangıç: REST API kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-rest-api.md)
- [Hızlı Başlangıç: Azure portalını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)
