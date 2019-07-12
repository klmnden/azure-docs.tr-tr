---
title: Azure Data Factory kullanarak bir SAP tablosundan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına bir SAP tablosundan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: jingwang
ms.openlocfilehash: 9216f5c00cbdac273b562736abdd1c812d172237
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827765"
---
# <a name="copy-data-from-an-sap-table-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri bir SAP tablodan kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir SAP tablosundan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Daha fazla bilgi için [kopyalama etkinliğine genel bakış](copy-activity-overview.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna bir SAP tablosundan veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP tablo bağlayıcı'yı destekler:

- Bir SAP tablosundan veri kopyalama:

  - SAP ERP merkezi bileşeni (SAP ECC) 7.01 veya sonraki bir sürümü (yığındaki son SAP destek paketi 2015 tarihinden sonra yayımlanan).
  - SAP Business Warehouse (SAP BW) 7.01 veya sonraki bir sürümü.
  - SAP S/4HANA.
  - Diğer ürünler için SAP Business Suite 7.01 veya sonraki bir sürümü.

- Kopyalama hem SAP saydam tablo, havuza alınmış bir tablo, kümelenmiş bir tablo ve görünüm verileri.
- SNC yapılandırılmışsa, temel kimlik doğrulaması veya güvenli ağ iletişimi (SNC) kullanarak veri kopyalama.
- Bir SAP uygulama sunucusu veya SAP ileti sunucusu bağlanma.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP tablo bağlayıcıyı kullanmak için yapmanız:

- Bir şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan (sürüm 3.17 veya sonrası) ayarlayın. Daha fazla bilgi için [oluşturun ve şirket içinde barındırılan tümleştirme çalışma zamanı yapılandırma](create-self-hosted-integration-runtime.md).

- 64 bitlik indirme [Microsoft .NET 3.0 için SAP Bağlayıcısı](https://support.sap.com/en/product/connectors/msnet.html) SAP'nin sitesinden ve şirket içinde barındırılan tümleştirme çalışma zamanı makineye yükleyin. Yükleme sırasında seçtiğinizden emin olun **yükleme derlemeleri GAC'ye** seçeneğini **isteğe bağlı kurulum adımlarını** penceresi.

  ![.NET için SAP bağlayıcısını yükleme](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Data Factory SAP tablo bağlayıcısında kullanılan SAP kullanıcının aşağıdaki izinlere sahip olmalıdır:

  - İşlev çağrısı (RFC) hedefleri kullanmak için yetkilendirme.
  - Yürütme etkinliği bir izni S_SDSAUTH yetkilendirme nesne.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli bir Data Factory varlıklarını SAP tablo bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP BW Open bağlı Hub hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `type` Özelliği ayarlanmalıdır `SapTable`. | Evet |
| `server` | SAP örneğini bulunduğu sunucunun adı.<br/>SAP uygulama sunucusuna bağlanmak için bu seçeneği kullanın. | Hayır |
| `systemNumber` | SAP sistemine sistem numarası.<br/>SAP uygulama sunucusuna bağlanmak için bu seçeneği kullanın.<br/>İzin verilen değer: Bir dize olarak temsil edilen iki basamaklı ondalık sayı. | Hayır |
| `messageServer` | SAP ileti sunucusu konak adı.<br/>SAP ileti sunucusuna bağlanmak için bu seçeneği kullanın. | Hayır |
| `messageServerService` | Hizmet adı veya bağlantı noktası ileti sunucusu sayısı.<br/>SAP ileti sunucusuna bağlanmak için bu seçeneği kullanın. | Hayır |
| `systemId` | Tablonun bulunduğu SAP sistemine kimliği.<br/>SAP ileti sunucusuna bağlanmak için bu seçeneği kullanın. | Hayır |
| `logonGroup` | SAP sistemi için oturum açma grubu.<br/>SAP ileti sunucusuna bağlanmak için bu seçeneği kullanın. | Hayır |
| `clientId` | SAP sistemine istemci kimliği.<br/>İzin verilen değer: Bir dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| `language` | SAP sistemine kullandığı dili.<br/>Varsayılan değer `EN`.| Hayır |
| `userName` | SAP sunucusuna erişimi olan kullanıcı adı. | Evet |
| `password` | Kullanıcının parolası. Bu alanı ile işaretleyin `SecureString` Data Factory'de güvenle depolamak için türü veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| `sncMode` | Tablonun bulunduğu SAP sunucusuna erişmek için SNC etkinleştirme göstergesi.<br/>SNC SAP sunucusuna bağlanmak için kullanmak istiyorsanız bu seçeneği kullanın.<br/>İzin verilen değerler `0` (kapalı, varsayılan) veya `1` (açık). | Hayır |
| `sncMyName` | Tablonun bulunduğu SAP sunucusuna erişmek için başlatıcının SNC adı.<br/>Olduğunda geçerlidir `sncMode` açıktır. | Hayır |
| `sncPartnerName` | Tablonun bulunduğu SAP sunucusuna erişmek için iletişim iş ortağının SNC adı.<br/>Olduğunda geçerlidir `sncMode` açıktır. | Hayır |
| `sncLibraryPath` | Tablonun bulunduğu SAP sunucusuna erişmek için dış güvenlik ürünün kitaplığı.<br/>Olduğunda geçerlidir `sncMode` açıktır. | Hayır |
| `sncQop` | Uygulamak koruma SNC kalite düzeyi.<br/>Olduğunda geçerlidir `sncMode` açıktır. <br/>İzin verilen değerler `1` (kimlik doğrulaması) `2` (bütünlüğü) `3` (gizlilik) `8` (varsayılan), `9` (maksimum). | Hayır |
| `connectVia` | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Daha önce belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gerekli [önkoşulları](#prerequisites). |Evet |

**Örnek 1: SAP uygulama sunucusuna bağlanma**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client ID>",
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

### <a name="example-2-connect-to-an-sap-message-server"></a>Örnek 2: Bağlanmak için bir SAP ileti sunucusu

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system ID>",
            "logonGroup": "<logon group>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="example-3-connect-by-using-snc"></a>Örnek 3: SNC kullanarak bağlanma

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "<SNC myname>",
            "sncPartnerName": "<SNC partner name>",
            "sncLibraryPath": "<SNC library path>",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için özellikleri tam listesi için bkz [veri kümeleri](concepts-datasets-linked-services.md). Aşağıdaki bölümde, SAP tablosu veri kümesi tarafından desteklenen özellikler listesini sağlar.

Gelen ve SAP BW Open bağlı Hub hizmeti, verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `type` Özelliği ayarlanmalıdır `SapTableResource`. | Evet |
| `tableName` | Verileri kopyalamak için SAP tablonun adı. | Evet |

### <a name="example"></a>Örnek

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "linkedServiceName": {
            "referenceName": "<SAP table linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<SAP table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için özellikleri tam listesi için bkz [işlem hatları](concepts-pipelines-activities.md). Aşağıdaki bölümde SAP tablo kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sap-table-as-a-source"></a>SAP tablo kaynağı olarak

Verileri bir SAP tablosundan kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik                         | Açıklama                                                  | Gerekli |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| `type`                             | `type` Özelliği ayarlanmalıdır `SapTableSource`.         | Evet      |
| `rowCount`                         | Alınacak satırların sayısı.                              | Hayır       |
| `rfcTableFields`                   | SAP tablosundan kopyalamak için alanlar (sütun). Örneğin: `column0, column1`. | Hayır       |
| `rfcTableOptions`                  | Bir SAP tablosundaki satırları filtre seçenekleri. Örneğin: `COLUMN0 EQ 'SOMEVALUE'`. Ayrıca bu makalenin devamındaki SAP sorgu işleci tablosuna bakın. | Hayır       |
| `customRfcReadTableFunctionModule` | Bir SAP tablosundan verileri okumak için kullanılan özel bir RFC işlevi modülü.<br>Verilerin SAP sisteminizden alınır ve döndürülen veri Fabrikasına tanımlamak için özel bir RFC işlevi modülü kullanabilirsiniz. Özel işlev modülü benzer uygulanan bir arabirim (içeri aktarma, dışarı aktarma, tabloları) olmalıdır `/SAPDS/RFC_READ_TABLE2`, veri fabrikası tarafından kullanılan varsayılan arabirimi olduğu. | Hayır       |
| `partitionOption`                  | Bir SAP tablosundan okumak için bölüm mekanizması. Desteklenen Seçenekler şunlardır: <ul><li>`None`</li><li>`PartitionOnInt` (normal tamsayı veya sıfır sol tarafta, gibi doldurma ile tamsayı değerleri `0000012345`)</li><li>`PartitionOnCalendarYear` ("YYYY" biçiminde 4 basamak)</li><li>`PartitionOnCalendarMonth` (6 basamaklı "YYYYMM" biçiminde)</li><li>`PartitionOnCalendarDate` ("YYYYMMDD" biçiminde 8 basamak)</li></ul> | Hayır       |
| `partitionColumnName`              | Verileri bölümlemek için kullanılan sütunun adı.                | Hayır       |
| `partitionUpperBound`              | Belirtilen sütunun en yüksek değeri `partitionColumnName` bölümleme ile devam etmek için kullanılır. | Hayır       |
| `partitionLowerBound`              | Belirtilen sütunun en düşük değeri `partitionColumnName` bölümleme ile devam etmek için kullanılır. | Hayır       |
| `maxPartitionsNumber`              | Verileri bölmek için bölümleri maksimum sayısı.     | Hayır       |

>[!TIP]
>SAP tablonuzun gibi birkaç milyardan fazla satır, verileri yüksek hacimde olması durumunda kullanmanız `partitionOption` ve `partitionSetting` verileri daha küçük bölümlere ayırmak için. Bu durumda, bölüm başına veriler okunur ve her veri bölümü SAP sunucunuzdan tek bir RFC çağrısı aracılığıyla alınır.<br/>
<br/>
>Alma `partitionOption` olarak `partitionOnInt` örnek olarak, her bölümdeki satır sayısı bu formüle göre hesaplanır: (toplam satırlar arasında kalan `partitionUpperBound` ve `partitionLowerBound`) /`maxPartitionsNumber`.<br/>
<br/>
>Bölümler kopyalamak yedeklemek hızlandırmak için paralel çalıştırmak için yapma öneririz `maxPartitionsNumber` değerinin katı `parallelCopies` özelliği. Daha fazla bilgi için [paralel kopyalama](copy-activity-performance.md#parallel-copy).

İçinde `rfcTableOptions`, satırları filtrelemek için aşağıdaki SAP ortak sorgu işleçleri kullanabilirsiniz:

| İşleç | Açıklama |
| :------- | :------- |
| `EQ` | Eşittir |
| `NE` | Eşit değildir |
| `LT` | Küçüktür |
| `LE` | Küçüktür veya eşittir |
| `GT` | Büyüktür |
| `GE` | Büyüktür veya eşittir |
| `LIKE` | Olarak `LIKE 'Emma%'` |

### <a name="example"></a>Örnek

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP table input dataset name>",
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

## <a name="data-type-mappings-for-an-sap-table"></a>Bir SAP tablosu için veri türü eşlemeleri

Bir SAP tablosundan veri kopyalama yapılırken, aşağıdaki eşlemeler SAP tablo veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| SAP ABAP türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| `C` (Dize) | `String` |
| `I` (Tamsayı) | `Int32` |
| `F` (Kayan nokta) | `Double` |
| `D` (Tarih) | `String` |
| `T` (Saat) | `String` |
| `P` (BCD, para birimi, Decimal, miktar paketlenmiş) | `Decimal` |
| `N` (Sayısal) | `String` |
| `X` (İkili ve ham) | `String` |

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
