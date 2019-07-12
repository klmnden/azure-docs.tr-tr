---
title: Mysql'i Azure Data Factory ile verileri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak MySQL veritabanından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/06/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 20dd86a46ac1b50f5ce20da6ecf9dff251a8c0b0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839016"
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a>Gelen MySQL Azure Data Factory ile veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-onprem-mysql-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-mysql.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de MySQL bağlayıcısını](../connector-mysql.md).


Bu makalede, bir şirket içi MySQL veritabanından veri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Şirket içi MySQL veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca veri taşımayı MySQL veri deposundan verileri diğer veri depolarına bir MySQL veri deposuna taşımak için değil ancak diğer veri depolarını destekler. 

## <a name="prerequisites"></a>Önkoşullar
Data Factory hizmeti, veri yönetimi ağ geçidi kullanarak şirket içi MySQL kaynaklarına bağlanmayı destekler. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalenin veri yönetimi ağ geçidi ve ağ geçidini ayarlamadan adım adım yönergeleri hakkında bilgi edinin.

MySQL veritabanı, Azure Iaas sanal makine'de (VM) barındırılıyor olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabilir sürece veri deposu olarak aynı VM'de ya da farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Veri Yönetimi MySQL veritabanına bağlanmak ağ geçidi için yüklemeniz gereken [MySQL Connector/Net için Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (sürüm 6.6.5 6.10.7 arasındaki) veri yönetimi ağ geçidi ile aynı sistemde. Bu 32 bit sürücüsü, 64-bit veri yönetimi ağ geçidi ile uyumludur. MySQL sürüm 5.1 ve üzeri desteklenir.

> [!TIP]
> MySQL Connector/Net, daha yüksek bir sürüme yükseltmek için "Uzak taraf taşıma akışını. kapatıldığı için kimlik doğrulaması başarısız oldu" hata ulaşırsanız göz önünde bulundurun.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. 

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz. 
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. 
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Şirket içi MySQL veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri MySQL'den Azure Blob kopyalama](#json-example-copy-data-from-mysql-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir MySQL veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, JSON öğeleri MySQL bağlantılı hizmete özgü açıklama belirler.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| türü |Type özelliği ayarlanmalıdır: **OnPremisesMySql** |Evet |
| server |MySQL sunucusunun adı. |Evet |
| database |MySQL veritabanının adı. |Evet |
| schema |Veritabanı şemasının adı. |Hayır |
| authenticationType |MySQL veritabanına bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: `Basic`. |Evet |
| userName |MySQL veritabanına bağlanmak için kullanıcı adı belirtin. |Evet |
| password |Belirtilen kullanıcı hesabı için parola belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi MySQL veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **RelationalTable** (MySQL veri kümesini içeren) aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti MySQL veritabanı örneğinde tablonun adını gösterir. |Hayır (varsa **sorgu** , **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları gibi özellikleri olan ilkeleri, tüm etkinlik türleri için kullanılabilir.

Diğer yandan bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kopya etkinlikteki kaynak türünde olduğunda **RelationalSource** (MySQL içeren), typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| query |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * MyTable öğesinden. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a>JSON örneği: MySQL için Azure Blob kopyalama verileri
Bu örnekte kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu, bir Azure Blob depolama alanına bir şirket içi MySQL veritabanından veri kopyalamak nasıl gösterir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

> [!IMPORTANT]
> Bu örnek JSON parçacıklarını sağlar. Veri Fabrikası oluşturmaya yönelik adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri bir MySQL veritabanı sorgu sonucunda bir bloba saatlik kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi'ni ayarlayın. Yönergeleri bulunan [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**MySQL hizmeti bağlı:**

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

**Azure depolama bağlı hizmeti:**

```JSON
    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }
```

**MySQL giriş veri kümesi:**

Örnek MySQL "MyTable" bir tablo oluşturdunuz ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiği varsayılır.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içinde seçer.

```JSON
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a>MySQL için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Mysql'e veri taşıma, aşağıdaki eşlemeler MySQL türlerinden .NET türleri için kullanılır.

| MySQL veritabanı türü | .NET framework türü |
| --- | --- |
| işaretsiz büyük tamsayı |Decimal |
| bigint |Int64 |
| bit |Decimal |
| blob |Byte[] |
| bool |Boole değeri |
| char |Dize |
| date |Datetime |
| datetime |Datetime |
| decimal |Decimal |
| çift duyarlık |Double |
| double |Double |
| Sabit listesi |Dize |
| float |Single |
| işaretsiz int |Int64 |
| int |Int32 |
| işaretsiz tamsayı |Int64 |
| integer |Int32 |
| uzun varbinary |Byte[] |
| uzun varchar |Dize |
| longblob |Byte[] |
| LONGTEXT |Dize |
| mediumblob |Byte[] |
| İmzasız mediumint |Int64 |
| mediumint |Int32 |
| mediumtext |Dize |
| numeric |Decimal |
| real |Double |
| set |Dize |
| işaretsiz tamsayı |Int32 |
| smallint |Int16 |
| text |Dize |
| time |TimeSpan |
| timestamp |Datetime |
| tinyblob |Byte[] |
| İmzasız Mini tamsayı |Int16 |
| tinyint |Int16 |
| tinytext |Dize |
| varchar |Dize |
| yıl |Int |

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
