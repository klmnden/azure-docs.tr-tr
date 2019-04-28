---
title: Gelen PostgreSQL Azure Data Factory ile verileri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak PostgreSQL veritabanından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: bd39b0aae5b76f37e2153f8e4c4502be994fa5b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462012"
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>PostgreSQL Azure Data Factory ile verileri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-onprem-postgresql-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-postgresql.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de PostgreSQL bağlayıcı](../connector-postgresql.md).


Bu makalede, bir şirket içi PostgreSQL veritabanından veri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Şirket içi PostgreSQL veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data factory, veri taşımayı bir PostgreSQL veritabanından verileri diğer veri depolarına bir PostgreSQL veritabanı'na taşımak için değil ancak diğer veri depoları için şu anda destekler.

## <a name="prerequisites"></a>Önkoşullar

Data Factory hizmeti, veri yönetimi ağ geçidi kullanarak şirket içi PostgreSQL kaynaklarına bağlanmayı destekler. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalenin veri yönetimi ağ geçidi ve ağ geçidini ayarlamadan adım adım yönergeleri hakkında bilgi edinin.

PostgreSQL veritabanı bir Azure Iaas VM'de barındırılıyor olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabilir sürece veri deposu olarak aynı Iaas VM'de veya farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Veri Yönetimi PostgreSQL veritabanına bağlanmak ağ geçidi için yükleme [Ngpsql PostgreSQL için veri sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 ve veri yönetimi ağ geçidi ile aynı sistemde 3.1.9 arasında sürümüne sahip. PostgreSQL sürüm 7.4 ve üzerinde desteklenir.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi PostgreSQL veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz:
  - Azure portal
  - Visual Studio
  - Azure PowerShell
  - Azure Resource Manager şablonu
  - .NET API’si
  - REST API

    Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Şirket içi PostgreSQL veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Veri PostgreSQL için Azure Blob kopyalama](#json-example-copy-data-from-postgresql-to-azure-blob) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir PostgreSQL veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, JSON öğeleri PostgreSQL bağlantılı hizmete özgü açıklama belirler.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesPostgreSql** |Evet |
| sunucu |PostgreSQL sunucusu adı. |Evet |
| veritabanı |PostgreSQL veritabanı adı. |Evet |
| Şema |Veritabanı şemasının adı. Şema adı büyük/küçük harfe duyarlıdır. |Hayır |
| authenticationType |PostgreSQL veritabanı'na bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi PostgreSQL veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri için benzerdir.

TypeProperties bölümünün her tür veri kümesi için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **RelationalTable** (PostgreSQL veri kümesini içeren) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti PostgreSQL veritabanı örneğinde tablonun adını gösterir. TableName, büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **RelationalSource** (PostgreSQL içeren), typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `"query": "select * from \"MySchema\".\"MyTable\""`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

> [!NOTE]
> Şema ve tablo adları büyük/küçük harfe duyarlıdır. İçine alınmaları `""` (çift tırnak) sorgu.

**Örnek:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a>JSON örneği: PostgreSQL için Azure Blob kopyalama verileri
Bu örnekte kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar veri PostgreSQL veritabanı'ndan Azure Blob Depolama'ya kopyalama işlemini göstermektedir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

> [!IMPORTANT]
> Bu örnek JSON parçacıklarını sağlar. Veri Fabrikası oluşturmaya yönelik adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. [İşlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri PostgreSQL veritabanına bir sorgu sonucunda bir bloba saatte kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi ayarlama. Yönergeleri bulunan [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**PostgreSQL bağlı hizmeti:**

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
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
**Azure Blob Depolama bağlı hizmeti:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```
**PostgreSQL girdi veri kümesi:**

Örneği, PostgreSQL "MyTable" bir tablo oluşturdunuz ve zaman serisi verileri için "TIMESTAMP" adlı bir sütun içerdiği varsayılır.

Ayar `"external": true` Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
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

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopyalama etkinliği ile işlem hattı:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği PostgreSQL veritabanı public.usstates tablodaki verileri seçer.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
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
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>PostgreSQL için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği kaynak türünden aşağıdaki 2 adımlı yaklaşım türleriyle havuz otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

PostgreSQL için veri taşıma, aşağıdaki eşlemeler PostgreSQL türünden .NET türü kullanılır.

| PostgreSQL veritabanı türü | PostgresSQL diğer adları | .NET framework türü |
| --- | --- | --- |
| abstime | |DateTime |
| bigint |Int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [(n)] | |Bayt [], dize |
| bit değişen [(n)] |varbit |Bayt [], dize |
| boole |bool |Boolean |
| Kutusu | |Bayt [], dize |
| bytea | |Bayt [], dize |
| karakter [(n)] |char [(n)] |String |
| [(n)] değişen karakter |varchar [(n)] |String |
| CID | |String |
| cidr | |String |
| Daire | |Bayt [], dize |
| date | |DateTime |
| daterange | |String |
| çift duyarlık |FLOAT8 |Double |
| inet | |Bayt [], dize |
| intarry | |String |
| int4range | |String |
| int8range | |String |
| integer |int, int4 |Int32 |
| aralığı [alanları] [(p)] | |Timespan |
| json | |String |
| jsonb | |Byte[] |
| satır | |Bayt [], dize |
| lseg | |Bayt [], dize |
| macaddr | |Bayt [], dize |
| money | |Decimal |
| sayısal [(p, s)] |ondalık [(p, s)] |Decimal |
| numrange | |String |
| OID | |Int32 |
| path | |Bayt [], dize |
| pg_lsn | |Int64 |
| nokta | |Bayt [], dize |
| Çokgen | |Bayt [], dize |
| real |float4 |Single |
| smallint |int2 |Int16 |
| smallserial |serial2 |Int16 |
| seri |serial4 |Int32 |
| metin | |String |

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
