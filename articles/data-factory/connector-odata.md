---
title: Azure Data Factory kullanarak OData kaynaklardan veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak OData kaynaklardan desteklenen havuz veri depolarına kopyalama öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: d26adec8c273d015a671c745f2136fc6251fd291
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="copy-data-from-odata-source-using-azure-data-factory"></a>Azure Data Factory kullanarak OData kaynağından veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-odata-connector.md)
> * [Sürüm 2 - Önizleme](connector-odata.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir OData kaynaktan veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [OData V1 Connector'daki](v1/data-factory-odata-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna OData kaynaktan veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu OData bağlayıcı destekler:

- OData **sürüm 3.0 ve 4.0**.
- Aşağıdaki kimlik doğrulamaları kullanarak veri kopyalama: **anonim**, **temel**, ve **Windows**.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, belirli Data Factory varlıklarını OData bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri, bağlantılı OData hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **OData** |Evet |
| URL | OData hizmet kök URL'si. |Evet |
| authenticationType | OData kaynağına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **anonim**, **temel**, ve **Windows**. Not OAuth desteklenmiyor. | Evet |
| Kullanıcı adı | Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. | Hayır |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan SecureString işaretleyin. | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek 1: Anonim kimlik doğrulamasını kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Temel kimlik doğrulaması kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 3: Windows kimlik doğrulaması kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source>",
            "authenticationType": "Windows",
            "userName": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde OData veri kümesi tarafından desteklenen özellikler listesini sağlar.

OData veri kopyalamak için veri kümesi için tür özelliği ayarlamak **ODataResource**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **ODataResource** | Evet |
| Yol | OData kaynak yolu. | Hayır |

**Örnek**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "linkedServiceName": {
            "referenceName": "<OData linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "path": "Products"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde OData kaynağı tarafından desteklenen özellikler listesini sağlar.

### <a name="odata-as-source"></a>OData kaynağı olarak

OData veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | Verileri filtrelemek için OData sorgu seçenekleri. Örnek: "? $select adı, açıklama ve $top = 5 =".<br/><br/>En son olarak, OData bağlayıcı birleşik URL'den verileri kopyalar dikkat edin: `[url specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Başvurmak [OData URL bileşenleri](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromOData",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OData input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "?$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-odata"></a>Eşleme OData için veri türü

OData veri kopyalama işlemi sırasında aşağıdaki eşlemelerini OData veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Edm.Binary | Byte] |
| Edm.Boolean | bool |
| Edm.Byte | Byte] |
| Edm.DateTime | Tarih saat |
| Edm.Decimal | Ondalık |
| Edm.Double | Çift |
| Edm.Single | Tek |
| Edm.Guid | GUID |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!Note]
> OData karmaşık veri türleri (örneğin, nesne) desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).