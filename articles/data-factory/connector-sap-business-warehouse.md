---
title: SAP BW Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına SAP Business Warehouse bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: jingwang
ms.openlocfilehash: 9a0abcd70b4aeb2369604bafa924136122206e0a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60309122"
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory"></a>SAP Business Warehouse Azure Data Factory kullanarak verileri kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-sap-business-warehouse-connector.md)
> * [Geçerli sürüm](connector-sap-business-warehouse.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir SAP Business Warehouse (BW) gelen verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP Business Warehouse tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP Business Warehouse Bağlayıcısı destekler:

- SAP Business Warehouse **sürüm 7.x**.
- Veri kopyalama **Infocubes ve QueryCubes** (BEx) MDX sorguları kullanarak sorguları.
- Temel kimlik doğrulaması kullanarak veri kopyalama.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP Business Warehouse Bağlayıcısı'nı kullanmak için gerekir:

- Şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Yükleme **SAP NetWeaver Kitaplığı** tümleştirme çalışma zamanı makinesinde. SAP Netweaver kitaplığını SAP yöneticinizden veya doğrudan alabilirsiniz [SAP Software Download Center](https://support.sap.com/swdc). Arama **SAP notu #1025361** en son sürümü için indirme konumunu almak için. Seçtiğinizden emin olun **64-bit** Integration Runtime yüklemenizle eşleşen SAP NetWeaver kitaplığı. Ardından tüm dosyaları SAP Note göre SAP NetWeaver RFC SDK'sı dahil yükleyin. SAP NetWeaver kitaplığı ayrıca SAP Client Tools yüklemesine de dahil edilir.

>[!TIP]
>SAP BW için bağlantı sorununu gidermek için emin olun:
>- NetWeaver RFC SDK'sından ayıklanan tüm bağımlılık kitaplıkları %windir%\system32 klasörü bir yerinde bulunan. Genellikle bu icudt34.dll, icuin34.dll, icuuc34.dll, libicudecnumber.dll, librfc32.dll, libsapucum.dll, sapcrypto.dll, sapcryto_old.dll, sahip sapnwrfc.dll.
>- SAP sunucusuna bağlanmak için kullanılan gerekli bağlantı noktaları genellikle bağlantı noktası 3300 ve 3201 olduğu şirket içinde barındırılan IR makine üzerinde etkindir.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP Business Warehouse bağlayıcısına belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP Business Warehouse (BW) bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapBw** | Evet |
| server | SAP BW örneği yer aldığı sunucunun adı. | Evet |
| systemNumber | SAP BW sisteminin sistem numarası.<br/>İzin verilen değer: bir dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet |
| clientId | SAP W sisteminde istemcinin istemci kimliği.<br/>İzin verilen değer: bir dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| userName | SAP sunucusuna erişimi olan kullanıcı adı. | Evet |
| password | Kullanıcının parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek:**

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, SAP BW veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP BW verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. SAP BW veri kümesi için desteklenen hiçbir türe özgü özellikler varken RelationalTable yazın.

**Örnek:**

```json
{
    "name": "SAPBWDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<SAP BW linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP BW kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-bw-as-source"></a>SAP BW kaynağı olarak

SAP BW verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **RelationalSource** | Evet |
| query | SAP BW örneğinden verileri okumak için MDX Sorgusu belirtir. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPBW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW input dataset name>",
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
                "query": "<MDX query for SAP BW>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw"></a>SAP BW için veri türü eşlemesi

SAP BW veri kopyalama işlemi sırasında aşağıdaki eşlemeler SAP BW veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP BW veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ACCP | Int |
| CHAR | String |
| CLNT | String |
| CURR | Decimal |
| CUKY | String |
| DEC | Decimal |
| FLTP | Double |
| INT1 | Byte |
| INT2 | Int16 |
| INT4 | Int |
| LANG | String |
| LCHR | String |
| LRAW | Byte[] |
| PREC | Int16 |
| QUAN | Decimal |
| RAW | Byte[] |
| RAWSTRING | Byte[] |
| STRING | String |
| UNIT | String |
| DATS | String |
| NUMC | String |
| TIMS | String |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
