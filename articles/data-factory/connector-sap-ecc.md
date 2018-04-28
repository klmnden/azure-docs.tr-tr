---
title: Azure Data Factory kullanarak SAP ECC veri kopyalama | Microsoft Docs
description: Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak SAP ECC desteklenen havuz veri depolarına kopyalama öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jingwang
ms.openlocfilehash: 68e3775be36b434acb5c25b522f9e28bec1b6125
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="copy-data-from-sap-ecc-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP ECC veri kopyalama

Bu makalede kopya etkinliği Azure Data Factory'de SAP ECC (SAP Kurumsal merkezi bileşeni) verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP ECC verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP ECC bağlayıcı destekler:

- SAP ECC SAP NetWeaver sürüm 7.0 ve üzeri veri kopyalama. 
- Herhangi (örn. SAP tablo/görünüm, BAPI, veri ayıklayıcıları, vb.) SAP ECC OData Hizmetleri tarafından sunulan nesneleri ya da göreli bağdaştırıcıları aracılığıyla OData olarak alınan SAP PI gönderilen veri/IDoc veri kopyalama.
- Temel kimlik doğrulaması kullanarak veri kopyalama.

## <a name="prerequisites"></a>Önkoşullar

Genellikle, SAP ECC varlıklar SAP ağ geçidi üzerinden OData Hizmetleri aracılığıyla kullanıma sunar. Bu SAP ECC bağlayıcıyı kullanmak için aktarmanız gerekir:

- **SAP ağ geçidi kurun**. SAP NetWeaver sürümü 7.4 yüksek olan sunucular için SAP ağ geçidi zaten yüklü. Karışık ağ geçidi veya ağ geçidi yüklemenize gerek Aksi takdirde OData Hizmetleri aracılığıyla SAP ECC verilerin açığa önce hub. SAP ağ geçidinden ayarlama öğrenin [Yükleme Kılavuzu](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Etkinleştirin ve SAP OData hizmetini yapılandırma**. Saniye cinsinden TCODE SICF OData hizmetleriyle etkinleştirebilirsiniz. Ayrıca, hangi açığa çıkarılması gereken nesneleri yapılandırabilirsiniz. Örnek bir işte [adım adım yönergeler](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="getting-started"></a>Başlarken

.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, belirli Data Factory varlıklarını SAP ECC bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler SAP ECC bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapEcc** | Evet |
| url | SAP ECC OData hizmeti URL'si. | Evet |
| kullanıcı adı | SAP ECC bağlanmak için kullanılan kullanıcı adı. | Hayır |
| password | SAP ECC bağlanmak için kullanılan düz metin parolası. | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData url e.g. http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, SAP ECC veri kümesi tarafından desteklenen özellikler listesini sağlar.

SAP ECC verileri kopyalamak için kümesine tür özelliği ayarlamak **SapEccResource**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| yol | SAP ECC OData varlık yolu. | Evet |

**Örnek**

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typePoperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP ECC kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sap-ecc-as-source"></a>Kaynak olarak SAP ECC

SAP ECC verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SapEccSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **SapEccSource** | Evet |
| sorgu | Verileri filtrelemek için OData sorgu seçenekleri. Örnek: "$select = ad, açıklama & $top = 10".<br/><br/>SAP ECC bağlayıcı birleşik URL'den verileri kopyalar: (url belirtilen bağlantılı hizmetteki) / (yolu belirtilen kümesinde)? (kopyalama etkinliği kaynağında belirtilen sorgu). Başvurmak [OData URL bileşenleri](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
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
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-ecc"></a>SAP ECC için veri türü eşlemesi

SAP ECC veri kopyalama işlemi sırasında aşağıdaki eşlemelerini OData veri türlerinden SAP ECC verileri Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |:--- |
| Edm.Binary | Dize |
| Edm.Boolean | Bool |
| Edm.Byte | Dize |
| Edm.DateTime | DateTime |
| Edm.Decimal | Ondalık |
| Edm.Double | Çift |
| Edm.Single | Tek |
| Edm.Guid | Dize |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Karmaşık veri türleri artık desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
