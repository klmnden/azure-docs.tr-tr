---
title: Azure Data Factory kullanarak PostgreSQL gelen verileri taşımak | Microsoft Docs
description: Azure Data Factory kullanarak PostgreSQL veritabanından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1d566a719abaf09146965a677868d363d33d10e5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34622773"
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Azure Data Factory kullanarak PostgreSQL taşıma verileri
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-onprem-postgresql-connector.md)
> * [Sürüm 2 - Önizleme](../connector-postgresql.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 PostgreSQL Bağlayıcısı](../connector-postgresql.md).


Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi PostgreSQL veritabanından veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Bir şirket içi PostgreSQL veri deposundan verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Veri Fabrikası şu anda veri taşımayı PostgreSQL veritabanından diğer veri depolarına, ancak verileri diğer veri depolarına bir PostgreSQL veritabanına taşıma değil destekler. 

## <a name="prerequisites"></a>Önkoşullar

Data Factory hizmetinin şirket içi PostgreSQL kaynaklarına veri yönetimi ağ geçidi kullanarak bağlanmayı destekler. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale veri yönetimi ağ geçidi ve ağ geçidi kurun ayarlama hakkında adım adım yönergeleri hakkında bilgi edinin.

Bir Azure Iaas sanal PostgreSQL veritabanına barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanıp sürece veri deposu olarak aynı Iaas VM veya farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bkz: [ağ geçidi sorunlarını giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) ilgili sorunlar bağlantı/ağ geçidi sorun giderme ipuçları için.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Veri Yönetimi PostgreSQL veritabanına bağlanmak ağ geçidi için yükleme [PostgreSQL için Ngpsql veri sağlayıcısı](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 ve veri yönetimi ağ geçidi ile aynı sistemde 3.1.9 arasında sürümüyle. PostgreSQL sürüm 7.4 ve üzeri desteklenir.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi PostgreSQL veri deposundan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun. 

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz. 
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: 
    - Azure portalına
    - Visual Studio
    - Azure PowerShell
    - Azure Resource Manager şablonu
    - .NET API’si
    - REST API

     Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir şirket içi PostgreSQL veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama PostgreSQL Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir PostgreSQL veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri PostgreSQL bağlantılı hizmete özgü açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesPostgreSql** |Evet |
| sunucu |PostgreSQL sunucunun adıdır. |Evet |
| veritabanı |PostgreSQL veritabanının adı. |Evet |
| Şema |Veritabanı şemasında adı. Şema adı büyük/küçük harf duyarlıdır. |Hayır |
| authenticationType |PostgreSQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi PostgreSQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için benzerdir.

TypeProperties bölümü dataset her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **RelationalTable** (PostgreSQL veri kümesi içeren) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlantılı hizmet başvurduğu PostgreSQL veritabanı örneğinde tablonun adı. TableName büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **RelationalSource** (içeren PostgreSQL), aşağıdaki özellikler typeProperties bölümünde kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: `"query": "select * from \"MySchema\".\"MyTable\""`. |Hayır (varsa **tableName** , **dataset** belirtilir) |

> [!NOTE]
> Şema ve tablo adları büyük/küçük harfe duyarlıdır. Bunları içine `""` (çift tırnak) sorgu.  

**Örnek:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a>JSON örnek: veri kopyalama PostgreSQL Azure Blob
Bu örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, verileri Azure Blob depolama alanına PostgreSQL veritabanından kopyalamak nasıl gösterir. Ancak, veri herhangi belirtildiği havuzlarını kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.   

> [!IMPORTANT]
> Bu örnek, JSON parçacıklarını sağlar. Data factory oluşturmak için adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. [Ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek veri PostgreSQL veritabanına bir sorgu sonucunda bir blobu saatte kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidi kurun ayarlayın. Yönergeler bulunan [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**Bağlantılı PostgreSQL hizmeti:**

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
**Azure Blob storage bağlı hizmeti:**

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

Örnek bir tablo "MyTable" PostgreSQL içinde oluşturduğunuz ve zaman serisi veri için "zaman damgası" adlı bir sütun içerdiği varsayar.

Ayarı `"external": true` Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

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

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

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

**Kopyalama etkinliği ile kanal:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırılmış ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **RelationalSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği PostgreSQL veritabanında public.usstates tablodan veri seçer.

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
## <a name="type-mapping-for-postgresql"></a>Tür eşlemesi için PostgreSQL
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Veri PostgreSQL için taşırken, aşağıdaki eşlemelerini PostgreSQL türünden .NET türü için kullanılır.

| Bir PostgreSQL veritabanı türü | PostgresSQL diğer adlar | .NET framework türü |
| --- | --- | --- |
| abstime | |Tarih saat | &nbsp;
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [(n)] | |Byte [], dize | &nbsp;
| değişen [(n)] bit |varbit |Byte [], dize |
| boole |bool |Boole |
| Kutusu | |Byte [], dize |&nbsp;
| bytea | |Byte [], dize |&nbsp;
| karakter [(n)] |char [(n)] |Dize |
| [(n)] değişen karakter |varchar [(n)] |Dize |
| CID | |Dize |&nbsp;
| CIDR | |Dize |&nbsp;
| Daire | |Byte [], dize |&nbsp;
| tarih | |Tarih saat |&nbsp;
| daterange | |Dize |&nbsp;
| çift duyarlıklı |FLOAT8 |Çift |
| INet | |Byte [], dize |&nbsp;
| intarry | |Dize |&nbsp;
| int4range | |Dize |&nbsp;
| int8range | |Dize |&nbsp;
| integer |int, int4 |Int32 |
| aralığı [alanlar] [(p)] | |Timespan |&nbsp;
| json | |Dize |&nbsp;
| jsonb | |Byte] |&nbsp;
| satır | |Byte [], dize |&nbsp;
| lseg | |Byte [], dize |&nbsp;
| macaddr | |Byte [], dize |&nbsp;
| para | |Ondalık |&nbsp;
| sayısal [(p, s)] |ondalık [(p, s)] |Ondalık |
| numrange | |Dize |&nbsp;
| OID | |Int32 |&nbsp;
| yol | |Byte [], dize |&nbsp;
| pg_lsn | |Int64 |&nbsp;
| noktası | |Byte [], dize |&nbsp;
| Çokgen | |Byte [], dize |&nbsp;
| Gerçek |float4 |Tek |
| tamsayı |int2 |Int16 |
| smallserial |serial2 |Int16 |
| seri |serial4 |Int32 |
| metin | |Dize |&nbsp;

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
