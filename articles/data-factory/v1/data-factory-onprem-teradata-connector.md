---
title: "Azure Data Factory kullanarak Teradata veri taşıma | Microsoft Docs"
description: "Teradata veritabanından veri taşımanıza olanak tanır Data Factory hizmeti Teradata Bağlayıcısı hakkında bilgi edinin"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 98eb76d8-5f3d-4667-b76e-e59ed3eea3ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d371604997d036aea6b4d4112bd124d127a2f3f1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a>Azure Data Factory kullanarak Teradata taşıma verileri
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-onprem-teradata-connector.md)
> * [Sürüm 2 - Önizleme](../connector-teradata.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Teradata Bağlayıcısı](../connector-teradata.md).

Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi Teradata veritabanından veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Bir şirket içi Teradata veri deposundan verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca veri taşımayı Teradata veri deposundan diğer veri depolarına, ancak verileri diğer veri depolarına bir Teradata veri deposuna taşıma değil destekler. 

## <a name="prerequisites"></a>Ön koşullar
Veri Fabrikası şirket içi Teradata kaynaklarına veri yönetimi ağ geçidi aracılığıyla bağlanmayı destekler. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale veri yönetimi ağ geçidi ve ağ geçidi kurun ayarlama hakkında adım adım yönergeleri hakkında bilgi edinin.

Bir Azure Iaas sanal Teradata barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanıp sürece veri deposu olarak aynı Iaas VM veya farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bkz: [ağ geçidi sorunlarını giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) ilgili sorunlar bağlantı/ağ geçidi sorun giderme ipuçları için.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Veri Yönetimi Teradata veritabanına bağlanmak ağ geçidi için yüklemeniz gerekir. [Teradata için .NET veri sağlayıcısı](http://go.microsoft.com/fwlink/?LinkId=278886) sürüm 14 veya üzeri veri yönetimi ağ geçidi ile aynı sistemde. Teradata sürüm 12 ve üzeri desteklenir.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun. 

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz. 
- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir şirket içi Teradata veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama Teradata Azure Blob](#json-example-copy-data-from-teradata-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Teradata veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri Teradata bağlantılı hizmete özgü açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesTeradata** |Evet |
| sunucu |Teradata sunucunun adıdır. |Evet |
| authenticationType |Teradata veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin şirket içi Teradata veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. Şu anda Teradata veri kümesi için desteklenen tür özellik yok.

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **RelationalSource** (içeren Teradata), aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * from MyTable. |Evet |

### <a name="json-example-copy-data-from-teradata-to-azure-blob"></a>JSON örnek: veri kopyalama Teradata Azure Blob
Aşağıdaki örneği kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar Azure Blob depolama alanına Teradata verileri kopyalamak nasıl gösterir. Ancak, veri herhangi belirtildiği havuzlarını kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.   

Örnek aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesTeradata](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. [Ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek veri Teradata veritabanına bir sorgu sonucunda bir blobu saatte kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidi ayarlayın. Yönergeler bulunan [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**Teradata bağlantılı hizmeti:**

```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
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
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**Teradata girdi veri kümesi:**

Örnek bir tablo "MyTable" Teradata içinde oluşturduğunuz ve zaman serisi veri için "zaman damgası" adlı bir sütun içerdiği varsayar.

"Dış" ayarı: true, tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin bildirir.

```json
{
    "name": "TeradataDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {
        },
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

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```json
{
    "name": "AzureBlobTeradataDataSet",
    "properties": {
        "published": false,
        "location": {
            "type": "AzureBlobLocation",
            "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "linkedServiceName": "AzureStorageLinkedService"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
**Kopyalama etkinliği ile kanal:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırılmış ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **RelationalSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içindeki seçer.

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "TeradataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobTeradataDataSet"
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
                "name": "TeradataToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z",
        "isPaused": false
    }
}
```
## <a name="type-mapping-for-teradata"></a>Teradata için tür eşlemesi
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopya etkinliği aşağıdaki 2 adımlı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Teradata için veri taşıma olduğunda, aşağıdaki eşlemelerini Teradata türünden .NET türü için kullanılır.

| Teradata veritabanına türü | .NET framework türü |
| --- | --- |
| char |Dize |
| CLOB |Dize |
| Grafiği |Dize |
| VarChar |Dize |
| VarGraphic |Dize |
| Blob |Byte] |
| Bayt |Byte] |
| VarByte |Byte] |
| BigInt |Int64 |
| ByteInt |Int16 |
| Ondalık |Ondalık |
| Çift |Çift |
| Tamsayı |Int32 |
| Sayı |Çift |
| Tamsayı |Int16 |
| Tarih |Tarih saat |
| Zaman |TimeSpan |
| Saat dilimi süresiyle |Dize |
| zaman damgası |Tarih saat |
| Saat dilimi zaman damgası |DateTimeOffset |
| Aralık gün |TimeSpan |
| Saat gün aralığı |TimeSpan |
| Dakika gün aralığı |TimeSpan |
| İkinci gün aralığı |TimeSpan |
| Aralık saat |TimeSpan |
| Aralık saat dakika |TimeSpan |
| İkinci aralığı saate |TimeSpan |
| Aralık dakika |TimeSpan |
| İkinci için aralığı dakika |TimeSpan |
| Aralığı ikinci |TimeSpan |
| Aralığı yıl |Dize |
| Aralığı yıl ay için |Dize |
| Aralık ayı |Dize |
| Period(Date) |Dize |
| Period(Time) |Dize |
| Süresi (saat dilimi ile) |Dize |
| Period(timestamp) |Dize |
| Süre (saat dilimi damgasıyla) |Dize |
| XML |Dize |

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
