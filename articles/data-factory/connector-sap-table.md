---
title: Azure Data Factory kullanarak SAP tablodan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına SAP tablosundan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: jingwang
ms.openlocfilehash: 4dee0e994c9e7be9677a8f1051481850990998e9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247177"
---
# <a name="copy-data-from-sap-table-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP tablodan veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir SAP tablosundan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP tablosundan tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu tablo SAP connector'ı destekler:

- SAP tablosundan veri kopyalama **7.01 veya üzeri bir sürüm ile SAP Business Suite** (yığındaki son SAP destek paketi 2015 yıl sonra yayımlanan) veya **S/4hana'yı**.
- Hem de veri kopyalama **SAP saydam tablo** ve **görünümü**.
- Kullanarak verileri kopyalama **temel kimlik doğrulaması** veya **SNC** (ağ SNC yapılandırılmışsa, iletişimi güvenli hale getirme).
- Bağlanma **uygulama sunucusu** veya **ileti sunucusu**.

## <a name="prerequisites"></a>Önkoşullar

Bu tablo SAP bağlayıcısını kullanmak için yapmanız:

- Bir şirket içinde barındırılan tümleştirme çalışma zamanı sürümü 3.17 ile veya üzeri olarak ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.

- İndirme **64-bit [SAP .NET bağlayıcı 3.0](https://support.sap.com/en/product/connectors/msnet.html)**  SAP'nin sitesinden ve şirket içinde barındırılan IR makineye yükleyin. Ne zaman yükleme, isteğe bağlı kurulum adımlarını penceresinde seçtiğinizden emin olun **yükleme derlemeleri GAC'ye** seçeneği. 

    ![SAP .NET bağlayıcısını yükleme](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Veri Fabrikası SAP Tablosu bağlayıcısında kullanılan SAP kullanıcının aşağıdaki izinleri olmalıdır: 

    - RFC için yetkilendirme. 
    - "Yürütme" etkinlik "S_SDSAUTH" yetkilendirme nesnesinin izinleri.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli SAP tablo bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP Business Warehouse açık bağlı Hub hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapTable** | Evet |
| sunucu | SAP örneğini yer aldığı sunucunun adı.<br/>Geçerli bağlanmak isterseniz **SAP uygulama sunucusu**. | Hayır |
| systemNumber | SAP sistemine sistem numarası.<br/>Geçerli bağlanmak isterseniz **SAP uygulama sunucusu**.<br/>İzin verilen değer: bir dize olarak temsil edilen iki basamaklı ondalık sayı. | Hayır |
| messageServer | SAP ileti sunucusu konak adı.<br/>Geçerli bağlanmak isterseniz **SAP ileti sunucusu**. | Hayır |
| messageServerService | Hizmet adı veya bağlantı noktası ileti sunucusu sayısı.<br/>Geçerli bağlanmak isterseniz **SAP ileti sunucusu**. | Hayır |
| systemId | Tablonun bulunduğu SAP sistemine Systemıd.<br/>Geçerli bağlanmak isterseniz **SAP ileti sunucusu**. | Hayır |
| logonGroup | SAP sistemi için oturum açma grubu.<br/>Geçerli bağlanmak isterseniz **SAP ileti sunucusu**. | Hayır |
| clientId | SAP sistemine istemcinin istemci kimliği.<br/>İzin verilen değer: bir dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| language | SAP sisteminin kullandığı dil. | Hayır (varsayılan değer **tr**)|
| userName | SAP sunucusuna erişimi olan kullanıcı adı. | Evet |
| password | Kullanıcının parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| sncMode | Tablonun bulunduğu SAP sunucusuna erişmek için SNC etkinleştirme göstergesi.<br/>SNC SAP sunucusuna bağlanmak için kullanmak istiyorsanız uygulanabilir.<br/>İzin verilen değerler şunlardır: **0** (kapalı, varsayılan) veya **1** (açık). | Hayır |
| sncMyName | Tablonun bulunduğu SAP sunucusuna erişmek için başlatıcının SNC adı.<br/>Uygun olduğunda `sncMode` açıktır. | Hayır |
| sncPartnerName | Tablonun bulunduğu SAP sunucusuna erişmek için iletişim ortağının SNC adı.<br/>Uygun olduğunda `sncMode` açıktır. | Hayır |
| sncLibraryPath | Tablonun bulunduğu SAP sunucusuna erişmek için dış güvenlik ürünün kitaplığı.<br/>Uygun olduğunda `sncMode` açıktır. | Hayır |
| sncQop | SNC kalite koruma.<br/>Uygun olduğunda `sncMode` açıktır. <br/>İzin verilen değerler şunlardır: **1** (kimlik doğrulaması) **2** (bütünlüğü) **3** (gizlilik) **8** (varsayılan), **9** (maksimum). | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek 1: SAP uygulama sunucusuna bağlanılıyor**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
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

**Örnek 2: bağlama için SAP ileti sunucusu**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system id>",
            "logonGroup": "<logon group>",
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

**Örnek 3: SNC kullanarak bağlanma**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "snc myname",
            "sncPartnerName": "snc partner name",
            "sncLibraryPath": "snc library path",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, SAP tablosu veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Gelen ve SAP BW Open Hub'ına veri kopyalamak için aşağıdaki özellikleri desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Type özelliği ayarlanmalıdır **SapTableResource**. | Evet |
| tableName | Verileri kopyalamak için SAP tablonun adı. | Evet |

**Örnek:**

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "linkedServiceName": {
            "referenceName": "<SAP Table linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<SAP table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP tablo kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-table-as-source"></a>SAP tablo kaynağı olarak

SAP tablodan veri kopyalamak için aşağıdaki özellikleri desteklenir.

| Özellik                         | Açıklama                                                  | Gerekli |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| türü                             | Type özelliği ayarlanmalıdır **SapTableSource**.       | Evet      |
| Satır sayısı                         | Alınacak satırların sayısı.                              | Hayır       |
| rfcTableFields                   | SAP tablosundan kopyalamak için alanları. Örneğin, `column0, column1`. | Hayır       |
| rfcTableOptions                  | SAP tablosundaki satırları filtre seçenekleri. Örneğin, `COLUMN0 EQ 'SOMEVALUE'`. | Hayır       |
| customRfcReadTableFunctionModule | SAP tablosundan verileri okumak için kullanılan özel RFC işlev modülü. | Hayır       |
| partitionOption                  | SAP tablosundan okumak için bölüm mekanizması. Desteklenen Seçenekler şunlardır: <br/>- **Yok**<br/>- **PartitionOnInt** (normal tamsayı veya sıfır doldurma soldaki 0000012345 gibi değerlerle tamsayı)<br/>- **PartitionOnCalendarYear** ("YYYY" biçiminde 4 basamak)<br/>- **PartitionOnCalendarMonth** (6 basamaklı biçimi "YYYYMM")<br/>- **PartitionOnCalendarDate** (8 basamak biçiminde "YYYYMMDD") | Hayır       |
| partitionColumnName              | Verileri bölümlemek için sütun adı. | Hayır       |
| partitionUpperBound              | Belirtilen sütunun en yüksek değeri `partitionColumnName` kullanılacak devam etmeden bölümleme için. | Hayır       |
| partitionLowerBound              | Belirtilen sütunun en düşük değeri `partitionColumnName` kullanılacak devam etmeden bölümleme için. | Hayır       |
| maxPartitionsNumber              | Verileri bölmek için bölümleri maksimum sayısı. | Hayır       |

>[!TIP]
>- Yüksek hacimli verilerin çeşitli milyarlarca satır gibi SAP tablonuzun olması durumunda kullanmanız `partitionOption` ve `partitionSetting` veri küçük bölümlere ayırmak için bu durumda veriler bölümleri ve her veri bölümü tarafından okunur SAP sunucunuzdan tek tek aracılığıyla alınır RFC çağrısı.<br/>
>- Alma `partitionOption` olarak `partitionOnInt` tarafından hesaplanan örnek olarak, her bölümdeki satır sayısı (toplam satırlar arasında kalan *partitionUpperBound* ve *partitionLowerBound*) /*maxPartitionsNumber*.<br/>
>- Daha fazla kopya hızlandırmak için paralel bölümleri çalıştırmak istiyorsanız, yapmanız önerilir `maxPartitionsNumber` değerini katları olarak `parallelCopies` (daha fazla bilgi [paralel kopyalama](copy-activity-performance.md#parallel-copy)).

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP Table input dataset name>",
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
                "type": "SapTableSource",
                "partitionOption": "PartitionOnInt",
                "partitionSettings": {
                     "partitionColumnName": "<partition column name>",
                     "partitionUpperBound": "2000",
                     "partitionLowerBound": "1",
                     "maxPartitionsNumber": 500
                 }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-table"></a>SAP tablosu için veri türü eşlemesi

SAP tablodan veri kopyalama işlemi sırasında aşağıdaki eşlemeler SAP tablo veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP ABAP türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| C (dize) | String |
| Ben (tamsayı) | Int32 |
| F (kayan nokta) | Double |
| D (tarih) | String |
| T (saat) | String |
| P (BCD paketlenmiş, para birimi, Decimal, miktar) | Decimal |
| N (sayısal) | String |
| (İkili ve ham) X | String |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
