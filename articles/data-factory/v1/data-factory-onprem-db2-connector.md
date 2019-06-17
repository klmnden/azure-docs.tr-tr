---
title: Azure Data Factory kullanarak verileri DB2'den taşıma | Microsoft Docs
description: Azure Data Factory kopyalama etkinliği'ni kullanarak bir şirket içi DB2 veritabanından veri taşıma konusunda bilgi edinin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 72c88ef10bf1df217ec6e24ac744d0b30386b4a3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60824023"
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Azure Data Factory kopyalama etkinliği'ni kullanarak DB2 verileri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-onprem-db2-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-db2.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [DB2 Bağlayıcısı V2'de](../connector-db2.md).


Bu makalede, kopyalama etkinliği Azure Data Factory'de bir şirket içi DB2 veritabanından bir veri deposuna veri kopyalamak için nasıl kullanabileceğinizi açıklar. Desteklenen bir havuz olarak listelenen tüm deposuna veri kopyalama [Data Factory veri taşıma etkinlikleri](data-factory-data-movement-activities.md#supported-data-stores-and-formats) makalesi. Bu konu, kopyalama etkinliği'ni kullanarak veri taşıma genel bir bakış sunar ve desteklenen veri deposu birleşimlerini listeler Data Factory makalesinde oluşturur. 

Data Factory şu anda yalnızca bir DB2 veritabanına verileri destekler bir [desteklenen havuz veri deposu](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Diğer veriler veri taşıma, bir DB2 veritabanında desteklenmeyen depolar.

## <a name="prerequisites"></a>Önkoşullar
Data Factory destekler kullanarak bir şirket içi DB2 veritabanına bağlanma [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md). Ağ geçidi veri işlem hattı, verileri taşımak için ayarlamak adım adım yönergeler için bkz: [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

DB2 Azure Iaas sanal makinesinde barındırılan olsa bile bir ağ geçidi gereklidir. Veri deposu olarak aynı Iaas VM ağ geçidi yükleyebilirsiniz. Ağ geçidi veritabanına bağlanabilir, farklı bir sanal ağ geçidi yükleyebilirsiniz.

Veri Yönetimi ağ geçidi yerleşik bir DB2 sürücüsü sağlar. böylece DB2 veri kopyalamak için bir sürücüyü el ile yüklemeniz gerekmez.

> [!NOTE]
> Bağlantı ve ağ geçidiyle ilgili sorunları giderme ipuçları için bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) makalesi.


## <a name="supported-versions"></a>Desteklenen sürümler
Data Factory DB2 Bağlayıcısı aşağıdaki IBM DB2 platformlara ve sürümlere 9, 10 ve 11 dağıtılmış ilişkisel veritabanı mimarisi (DRDA) SQL Erişim Yöneticisi sürümleri destekler:

* IBM DB2 z/OS sürümü 11.1 için
* IBM DB2 z/OS sürümü 10.1 için
* IBM DB2'i (AS400) sürüm 7.2 için
* IBM DB2'i (AS400) sürüm 7.1 için
* IBM DB2 Linux, UNIX ve Windows (LUW) sürüm 11
* IBM DB2 sürüm 10.5 LUW için
* IBM DB2 sürüm 10.1 LUW için

> [!TIP]
> "Bir SQL deyimi yürütme isteğine karşılık gelen paket bulunamadı. hata iletisi alırsanız SQLSTATE 51002 SQLCODE =-805, = "normal kullanıcı işletim sistemi için gerekli paket oluşturulmaz nedenidir. Bu sorunu çözmek için DB2 sunucu türü için bu yönergeleri izleyin:
> - DB2 için i (AS400): Kopyalama etkinliği çalıştırmadan önce için normal bir kullanıcı koleksiyonu oluşturma power user olanak tanır. Koleksiyonu oluşturmak için komutu kullanın: `create collection <username>`
> - DB2 z/OS veya LUW için: Yüksek ayrıcalıklı bir hesap--power user veya paket yetkilileri ve bağlama BINDADD, kopya bir kez çalıştırmak için izinleri verme YÜRÜTMEK için genel--yönetici kullanın. Gerekli paket kopyalama sırasında otomatik olarak oluşturulur. Daha sonra sonraki kopyalama çalışmalarınız için normal kullanıcıya geri geçiş yapabilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araç ve API'leri kullanarak bir şirket içi DB2 veri deposundan veri taşımak için bir kopyalama etkinlikli bir işlem hattı oluşturabilirsiniz: 

- Bir işlem hattı oluşturmanın en kolay yolu, Azure Data Factory Kopyalama Sihirbazı'nı kullanmaktır. Hızlı bir kılavuz Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hakkında bilgi için bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md). 
- Azure portalı, Visual Studio, Azure PowerShell, Azure Resource Manager şablonu, .NET API ve REST API dahil olmak üzere bir işlem hattı oluşturmak için araçlar da kullanabilirsiniz. Kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Giriş bağlantı ve veri depolarında veri fabrikanıza çıktı bağlı hizmetleri oluşturun.
2. Kopyalama işleminin girdi ve çıktı verilerini temsil eden veri kümeleri oluşturun. 
3. Bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile işlem hattı oluşturma. 

Data Factory bağlı için JSON tanımları, kopyalama Sihirbazı'nı kullandığınızda, hizmetler, veri kümeleri ve işlem hattı varlıkları otomatik olarak sizin için oluşturulur. Araç veya API'lerden (dışında .NET API'si) kullandığınızda, Data Factory varlıkları JSON biçimini kullanarak tanımlayın. JSON örneği: DB2 kopyalama verileri Azure Blob Depolama, şirket içi DB2 veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları gösterir.

Aşağıdaki bölümler belirli bir DB2 veri deposuna Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="db2-linked-service-properties"></a>DB2 bağlı hizmeti özellikleri
Aşağıdaki tabloda bir DB2 bağlı hizmeti için özel JSON özellikleri listeler.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **type** |Bu özellik ayarlanmalıdır **OnPremisesDb2**. |Evet |
| **Sunucu** |DB2 sunucunun adı. |Evet |
| **Veritabanı** |DB2 veritabanı adı. |Evet |
| **Şema** |DB2 veritabanında şemasının adı. Bu özellik, büyük/küçük harf duyarlıdır. |Hayır |
| **authenticationType** |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| **Kullanıcı adı** |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı hesabı adı. |Hayır |
| **Parola** |Kullanıcı hesabının parolası. |Hayır |
| **gatewayName** |Data Factory hizmetinin şirket içi DB2 veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikler listesi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler, gibi **yapısı**, **kullanılabilirlik**ve **ilke** tüm veri kümesi türleri (Azure SQL, Azure Blob Depolama, Azure tablo depolama için bir veri kümesi için JSON benzerdir vb.).

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. **TypeProperties** türü için bir veri kümesi bölümünü **RelationalTable**, özelliği var. Bu DB2 veri kümesi içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **TableName** |Bağlı hizmetini ifade eder DB2 veritabanı tablosunun adı. Bu özellik, büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** türünde bir kopyalama etkinliği özelliği **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve kopyalama etkinlikleri tanımlamak için kullanılabilir özellikler listesi için bkz. [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Kopyalama etkinliğinin özellikleri, aşağıdaki gibi **adı**, **açıklama**, **girişleri** tablo **çıkarır** tablo ve **İlkesi**, tüm etkinlik türleri için kullanılabilir. Kullanılabilir özellikler **typeProperties** etkinlik bölümünde her bir etkinlik türü için farklılık gösterir. Kopyalama etkinliği için özellikleri veri kaynağı ve havuz türlerine bağlı olarak farklılık gösterir.

Kopyalama etkinliği kaynak türü olduğunda için **RelationalSource** (DB2 içeren), aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **query** |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin, `"query": "select * from "MySchema"."MyTable""` |Hayır (varsa **tableName** özellik kümesinin belirtilen) |

> [!NOTE]
> Şema ve tablo adları büyük/küçük harfe duyarlıdır. Sorgu deyiminde özellik adları kullanarak alın "" (çift tırnak).

## <a name="json-example-copy-data-from-db2-to-azure-blob-storage"></a>JSON örneği: DB2'den Azure Blob depolama alanına veri kopyalama
Bu örnekte kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Örnek, DB2 veritabanından Blob depolama alanına veri kopyalama işlemini göstermektedir. Ancak, veriler için kopyalanabilir [desteklenen tüm veriler, Havuz türü depolamak](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kullanarak Azure Data Factory kopyalama etkinliği.

Örnek, aşağıdaki Data Factory varlıklarını sahiptir:

- Bir DB2 bağlı hizmet türü [OnPremisesDb2](data-factory-onprem-db2-connector.md)
- Bir Azure Blob Depolama bağlantılı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
- Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinlikli [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) özellikleri

Örnek verileri DB2 veritabanına bir sorgu sonucunda Azure blobuna saatlik kopyalar. Örnekte kullanılan JSON özellikleri varlık tanımlarını bölümlerde açıklanmıştır.

İlk adım olarak yükleyin ve bir veri ağ geçidi yapılandırın. Yönergeler [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**DB2 bağlı hizmeti**

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

**Azure Blob Depolama bağlı hizmeti**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**DB2 giriş veri kümesi**

Örnek, DB2 "zaman serisi verilerinin ve" TIMESTAMP"etiketli bir sütunda MyTable" adlı bir tablo oluşturduğunuz varsayılır.

**Dış** özelliği "true" Bu ayar, bu veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil, Data Factory hizmetinin sizi bilgilendirir. Dikkat **türü** özelliği **RelationalTable**.


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Azure Blob çıktı veri kümesi**

Veriler yazılır yeni bir blob için saatte ayarlayarak **sıklığı** "Hour" özelliğini ve **aralığı** özelliği 1. **FolderPath** özelliğinin blob dinamik olarak değerlendirilmesi için temel üzerinde işlenmekte olan dilimin başlangıç zamanı. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama etkinliği için işlem hattı**

İşlem hattını saatte bir çalışacak şekilde zamanlanmış olan ve belirtilen giriş ve çıkış veri kümesi için yapılandırılmış bir kopyalama etkinliği içeriyor. İşlem hattının JSON tanımında **kaynak** türü ayarlandığında **RelationalSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği "Siparişler" tablosundan verileri seçer.

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for the copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a>DB2 için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, türü, aşağıdaki iki adımlı yaklaşım kullanarak havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türünden bir .NET türüne dönüştürün
2. Bir .NET türünden bir yerel havuz türüne dönüştürün

Kopyalama etkinliği verileri DB2 türünden bir .NET türe dönüştürdüğünü şu eşlemeler kullanılır:

| DB2 veritabanı türü | .NET framework türü |
| --- | --- |
| Integer |Int16 |
| Integer |Int32 |
| BigInt |Int64 |
| Real |Single |
| Double |Double |
| Float |Double |
| Ondalık |Decimal |
| DecimalFloat |Decimal |
| Numeric |Decimal |
| Tarih |DateTime |
| Zaman |TimeSpan |
| Timestamp |DateTime |
| Xml |Byte[] |
| Char |String |
| VarChar |String |
| LongVarChar |String |
| DB2DynArray |String |
| binary |Byte[] |
| VarBinary |Byte[] |
| LongVarBinary |Byte[] |
| Graphic |String |
| VarGraphic |String |
| LongVarGraphic |String |
| Clob |String |
| Blob |Byte[] |
| DbClob |String |
| Integer |Int16 |
| Integer |Int32 |
| BigInt |Int64 |
| Real |Single |
| Double |Double |
| Float |Double |
| Ondalık |Decimal |
| DecimalFloat |Decimal |
| Numeric |Decimal |
| Tarih |DateTime |
| Zaman |TimeSpan |
| Timestamp |DateTime |
| Xml |Byte[] |
| Char |String |

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemeyle ilgili bilgi edinmek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>İlişkisel kaynaklar tekrarlanabilir okuma
İlişkisel veri deposundan veri kopyaladığınızda, Yinelenebilirlik istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Yeniden deneme de yapılandırabilirsiniz **ilke** özelliği için bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi. Aynı veri dilimi yeniden çalıştırmak kaç kez ne olursa olsun ve dilimi yeniden çalıştırmak nasıl bağımsız olarak okunur emin olun. Daha fazla bilgi için [Repeatable okur ilişkisel kaynaklardan](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama etkinliği ve performansı iyileştirmek için yollar performansını etkileyen önemli faktörlerin öğrenin [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
