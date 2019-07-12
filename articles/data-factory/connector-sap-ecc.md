---
title: Azure Data Factory kullanarak SAP ECC veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına SAP ECC bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 7ccd2e7a804c6495f6caf5e264b1f7c2a36cb02e
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827781"
---
# <a name="copy-data-from-sap-ecc-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP ECC veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de veri SAP Kurumsal merkezi bileşeni (ECC gelen) kopyalamak için nasıl kullanılacağını özetlenmektedir. Daha fazla bilgi için [kopyalama etkinliğine genel bakış](copy-activity-overview.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP ECC tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP ECC bağlayıcı'yı destekler:

- SAP NetWeaver sürüm 7.0 ve üzeri üzerinde SAP ECC veri kopyalama.
- Gibi SAP ECC OData Hizmetleri tarafından kullanıma sunulan tüm nesneler veri kopyalama:

  - SAP tabloları veya görünümleri.
  - İş uygulaması programlama arabirimi [BAPI] nesneleri.
  - Veri ayıklayıcıları.
  - Veri veya SAP işlem tümleştirme'için (PI), OData göreli bağdaştırıcıları alınabilir gönderilen Ara belgeler (IDoc gönderir).

- Temel kimlik doğrulaması kullanarak veri kopyalama.

>[!TIP]
>SAP ECC, bir SAP tablo veya Görünüm veri kopyalamak için kullanın [SAP tablo](connector-sap-table.md) daha hızlı ve daha ölçeklenebilir Bağlayıcısı.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak, SAP ECC varlıkları SAP ağ geçidi ile OData Hizmetleri yoluyla kullanıma sunar. SAP ECC bu bağlayıcıyı kullanmak için gerekir:

- **SAP ağ geçidi ayarlama**. Daha fazla 7.4 SAP NetWeaver sürümleriyle sunucular için SAP ağ geçidine zaten yüklü. Önceki sürümler için katıştırılmış SAP ağ geçidine veya SAP ağ geçidine hub sistem OData hizmetlerini SAP ECC verilerine gösterme önce yüklemelisiniz. SAP ağ geçidini ayarlamak için bkz: [Yükleme Kılavuzu](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Etkinleştirme ve SAP OData hizmeti yapılandırma**. Saniye cinsinden OData hizmeti aracılığıyla TCODE SICF etkinleştirebilirsiniz. Ayrıca, hangi nesnelerin sağlamak ihtiyaç yapılandırabilirsiniz. Daha fazla bilgi için [adım adım rehberlik](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli bir Data Factory varlıklarını SAP ECC bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP ECC bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `type` Özelliği ayarlanmalıdır `SapEcc`. | Evet |
| `url` | SAP ECC OData hizmeti URL'si. | Evet |
| `username` | SAP ECC için bağlanmak için kullanılan kullanıcı adı. | Hayır |
| `password` | SAP ECC için bağlanmak için kullanılan düz metin parolası. | Hayır |
| `connectVia` | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure tümleştirme çalışma zamanı kullanabilirsiniz. Bir çalışma zamanı belirtmezseniz `connectVia` varsayılan Azure Integration runtime'ı kullanır. | Hayır |

### <a name="example"></a>Örnek

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData URL, e.g., http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of integration runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md). Aşağıdaki bölümde, SAP ECC veri kümesi tarafından desteklenen özellikler listesini sağlar.

SAP ECC verileri kopyalamak için ayarlanmış `type` veri kümesine özelliği `SapEccResource`.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `path` | SAP ECC OData varlık yolu. | Evet |

### <a name="example"></a>Örnek

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typeProperties": {
            "path": "<entity path, e.g., dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). Aşağıdaki bölümde SAP ECC kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sap-ecc-as-a-source"></a>SAP ECC kaynağı olarak

SAP ECC verileri kopyalamak için ayarlanmış `type` özelliğinde `source` kopyalama etkinliğine bölümünü `SapEccSource`.

Kopyalama etkinliği ait aşağıdaki özellikler desteklenir `source` bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `type` Kopyalama etkinliği'nin özelliği `source` bölümü ayarlanmalıdır `SapEccSource`. | Evet |
| `query` | Verileri filtrelemek için OData sorgu seçenekleri. Örneğin:<br/><br/>`"$select=Name,Description&$top=10"`<br/><br/>SAP ECC Bağlayıcısı verileri birleşik URL'den kopyalar:<br/><br/>`<URL specified in the linked service>/<path specified in the dataset>?<query specified in the copy activity's source section>`<br/><br/>Daha fazla bilgi için [OData URL'si bileşenleri](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

### <a name="example"></a>Örnek

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

## <a name="data-type-mappings-for-sap-ecc"></a>SAP ECC için veri türü eşlemeleri

SAP ECC veri kopyalama yapılırken, şu eşlemeler OData veri türlerinden SAP ECC verileri Azure veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| `Edm.Binary` | `String` |
| `Edm.Boolean` | `Bool` |
| `Edm.Byte` | `String` |
| `Edm.DateTime` | `DateTime` |
| `Edm.Decimal` | `Decimal` |
| `Edm.Double` | `Double` |
| `Edm.Single` | `Single` |
| `Edm.Guid` | `String` |
| `Edm.Int16` | `Int16` |
| `Edm.Int32` | `Int32` |
| `Edm.Int64` | `Int64` |
| `Edm.SByte` | `Int16` |
| `Edm.String` | `String` |
| `Edm.Time` | `TimeSpan` |
| `Edm.DateTimeOffset` | `DateTimeOffset` |

> [!NOTE]
> Karmaşık veri türleri şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
