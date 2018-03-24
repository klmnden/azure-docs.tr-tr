---
title: Azure Data Factory kullanarak DB2'den veri taşıma | Microsoft Docs
description: Azure Data Factory kopyalama etkinliği kullanarak bir şirket içi DB2 veritabanından veri taşıma hakkında bilgi edinin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 0e597574c1993e2f2a5421d24063cf9f42a7e57b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Azure Data Factory kopyalama etkinliği kullanarak DB2 taşıma verileri
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-onprem-db2-connector.md)
> * [Sürüm 2 - Önizleme](../connector-db2.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 DB2 Bağlayıcısı](../connector-db2.md).


Bu makalede kopya etkinliği Azure Data Factory'de veri bir şirket içi DB2 veritabanından veri deposuna kopyalamak için nasıl kullanabileceğinizi açıklar. Desteklenen bir havuz olarak listelenen tüm depolama, veri kopyalayabilirsiniz [Data Factory veri taşıma etkinlikleri](data-factory-data-movement-activities.md#supported-data-stores-and-formats) makalesi. Bu konu, kopyalama etkinliği kullanarak veri taşıma genel bir bakış sunar ve desteklenen veri deposu birleşimlerini listeler Data Factory makale oluşturur. 

Veri Fabrikası şu anda bir DB2 veritabanından yalnızca veri taşımayı destekleyen bir [desteklenen havuz veri deposu](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Verileri diğer veriler taşıma veritabanı desteklenmiyor bir DB2 depolar.

## <a name="prerequisites"></a>Önkoşullar
Veri Fabrikası destekleyen kullanarak bir şirket içi DB2 veritabanına bağlanma [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md). Verilerinizi taşımak için ağ geçidi veri ardışık ayarlamak adım adım yönergeler için bkz: [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

DB2 Azure Iaas sanal üzerinde barındırılıyorsa bile bir ağ geçidi gereklidir. Veri deposu olarak aynı Iaas VM ağ geçidi yükleyebilirsiniz. Ağ geçidi veritabanına bağlanıyorsanız, farklı bir sanal ağ geçidi yükleyebilirsiniz.

Veri Yönetimi ağ geçidi yerleşik bir DB2 sürücü sağlar, DB2'den verileri kopyalamak için bir sürücüyü el ile yüklemeniz gerekmez.

> [!NOTE]
> Bağlantı ve ağ geçidi sorunları giderme hakkında ipuçları için bkz: [ağ geçidi sorunlarını giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) makalesi.


## <a name="supported-versions"></a>Desteklenen sürümler
Veri Fabrikası DB2 Bağlayıcısı'nı aşağıdaki IBM DB2 platformları ve dağıtılmış ilişkisel veritabanı mimarisi (DRDA) SQL Erişim Yöneticisi sürüm 9, 10 ve 11 sürümleriyle destekler:

* IBM DB2 z/OS sürümü 11.1 için
* IBM DB2 z/OS sürümü 10.1 için
* IBM DB2'i (AS400) sürüm 7.2 için
* IBM DB2'i (AS400) sürüm 7.1 için
* IBM DB2 Linux, UNIX ve Windows (LUW) sürüm 11
* IBM DB2 sürüm 10.5 LUW için
* IBM DB2 sürüm 10.1 LUW için

> [!TIP]
> "Bir SQL deyimi yürütme isteğine karşılık gelen paket bulunamadı. hata iletisi alırsanız SQLSTATE 51002 SQLCODE =-805, = "işletim sistemi normal kullanıcı için gerekli bir paketi oluşturulmaz nedenidir. Bu sorunu çözmek için DB2 sunucu türü için bu yönergeleri izleyin:
> - DB2 için i (AS400): kopyalama etkinliği çalıştırmadan önce normal bir kullanıcı için koleksiyonu oluşturun power user olanak tanır. Koleksiyonu oluşturmak için komutu kullanın: `create collection <username>`
> - DB2 için z/OS veya LUW: ortak izin kopya kez çalıştırmak için--verme yürütme yüksek ayrıcalıklı bir hesap--power user veya paket yetkilileri ve bağlama, BINDADD, sahip yönetim kullanın. Gerekli paketi kopyalama sırasında otomatik olarak oluşturulur. Daha sonra sonraki kopyalama çalışmalarınız için normal kullanıcının geçiş yapabilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar ve API'ler kullanarak bir şirket içi DB2 veri deposundan verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturabilirsiniz: 

- Bir işlem hattı oluşturmak için en kolay yolu, Azure Data Factory Kopyalama Sihirbazı'nı kullanmaktır. Hızlı bir anlatım Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hakkında bilgi için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md). 
- Azure portalı, Visual Studio, Azure PowerShell, Azure Resource Manager şablonu, .NET API ve REST API de dahil olmak üzere bir işlem hattı oluşturmak için araçlar da kullanabilirsiniz. Kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Veri depoları, veri fabrikası için çıkış ve giriş bağlantısı bağlantılı Hizmetleri oluşturun.
2. Kopyalama işlemi için girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturma. 
3. Bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile işlem hattı oluşturacaksınız. 

Data Factory bağlantılı için JSON tanımları, kopyalama Sihirbazı'nı kullandığınızda, hizmetler, veri kümelerini ve ardışık düzen varlıklar otomatik olarak sizin için oluşturulur. Araçları veya API'ler (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak Data Factory varlıklarını tanımlayın. [JSON örnek: veri kopyalama DB2'den Azure Blob depolama alanına](#json-example-copy-data-from-db2-to-azure-blob) bir şirket içi DB2 veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları gösterir.

Aşağıdaki bölümler, belirli bir DB2 veri deposuna Data Factory varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="db2-linked-service-properties"></a>DB2 bağlantılı hizmet özellikleri
Aşağıdaki tabloda bir DB2 bağlantılı hizmete özel JSON özellikleri listeler.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Türü** |Bu özelliği ayarlamak **OnPremisesDb2**. |Evet |
| **server** |DB2 sunucunun adıdır. |Evet |
| **Veritabanı** |DB2 veritabanının adı. |Evet |
| **schema** |DB2 veritabanında şema adı. Bu özellik, büyük/küçük harf duyarlıdır. |Hayır |
| **authenticationType** |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, temel ve Windows. |Evet |
| **Kullanıcı adı** |Basic veya Windows kimlik doğrulaması kullanıyorsanız, kullanıcı hesabının adı. |Hayır |
| **Parola** |Kullanıcı hesabının parolası. |Hayır |
| **gatewayName** |Data Factory hizmetinin şirket içi DB2 veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümelerini tanımlamak için kullanılabilir özelliklerin listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler, gibi **yapısı**, **kullanılabilirlik**ve **İlkesi** bir veri kümesi için JSON, tüm veri kümesi türleri (Azure SQL, Azure Blob Depolama, Azure Table depolama için benzer vb.).

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. **TypeProperties** bir veri kümesi için bir bölüm türü **RelationalTable**, DB2 veri kümesini içeren aşağıdaki özelliğe sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **tableName** |DB2 veritabanı örneğinde bağlantılı hizmet başvurduğu tablonun adı. Bu özellik, büyük/küçük harf duyarlıdır. |Hayır (varsa **sorgu** türü kopyalama etkinliği özelliğinin **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümleri ve kopyalama etkinlikleri tanımlamak için kullanılabilir olan özelliklerin listesi için bkz [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Etkinlik özellikleri gibi kopyalamak **adı**, **açıklama**, **girişleri** tablo **çıkarır** tablo ve **İlkesi**, tüm etkinlikler türleri için kullanılabilir. Kullanılabilir özellikler **typeProperties** etkinlik bölümünü farklılık her etkinlik türü için. Kopya etkinliği için özellikleri, veri kaynaklarının ve havuzlarını türlerine bağlı olarak farklılık gösterir.

Kopyalama kaynağı türü olduğunda etkinliği için **RelationalSource** (içeren DB2), aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **Sorgu** |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin, `"query": "select * from "MySchema"."MyTable""` |Hayır (varsa **tableName** özellik kümesinin belirtilen) |

> [!NOTE]
> Şema ve tablo adları büyük/küçük harfe duyarlıdır. Sorgu deyimini kullanarak özellik adları alın "" (çift tırnak).

## <a name="json-example-copy-data-from-db2-to-azure-blob-storage"></a>JSON örnek: veri kopyalama DB2'den Azure Blob depolama alanına
Bu örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Örnek veri DB2 veritabanından Blob depolama alanına kopyalama gösterilmektedir. Ancak, veriler için kopyalanabilir [desteklenen herhangi bir veriyi depolamak Havuz türü](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kullanarak Azure Data Factory kopyalama etkinliği.

Örnek aşağıdaki Data Factory varlıklarını sahiptir:

- Bir DB2 bağlantılı hizmet türü [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)
- Bir Azure Blob Depolama bağlantılı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
- Bir giriş [dataset](data-factory-create-datasets.md) türü [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) özellikleri

Örnek veri DB2 veritabanına bir sorgu sonucunda bir Azure blob saatlik kopyalar. Aşağıdaki örnekte kullanılan JSON özellikleri varlık tanımları izleyen bölümlerde açıklanmıştır.

İlk adım olarak yükleyin ve bir veri ağ geçidi yapılandırın. Yönergeler [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**DB2 bağlı hizmet**

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

**Azure Blob storage bağlı hizmeti**

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

**DB2 girdi veri kümesi**

Örnek DB2 "zaman serisi veri için" zaman damgası"etiketli bir sütun sahip MyTable" adlı bir tablo oluşturduğunuzu varsayar.

**Dış** özelliği ayarlanmış "true" Bu ayar, bu veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin bildirir. Dikkat **türü** özelliği ayarlanmış **RelationalTable**.


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

Veri yazıldığı için yeni bir blob saatte ayarlayarak **sıklığı** "Saat" özelliğine ve **aralığı** özelliği 1. **FolderPath** özelliği blob dinamik olarak değerlendirilmesi için işleniyor dilim başlangıç zamanı temel alarak. Klasör yolu yıl, ay, gün ve saati başlangıç saatinden bölümlerini kullanır.

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

**Ardışık düzeni için kopyalama etkinliği**

Ardışık Düzen belirtilen giriş ve çıktı veri kümeleri için yapılandırılmış ve saatte bir çalışacak şekilde zamanlanmış kopyalama etkinliği içerir. Ardışık düzeni için JSON tanımında **kaynak** türü ayarlanmış **RelationalSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği "Siparişler" tablosundan verileri seçer.

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
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopyalama etkinliği türü aşağıdaki iki aşamalı yaklaşımı kullanarak havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türünden bir .NET türüne dönüştürün
2. Bir .NET türünden bir yerel havuz türüne dönüştürün

Kopya etkinliği bir DB2 türünden bir .NET türü veri dönüştürdüğünde aşağıdaki eşlemelerini kullanılır:

| DB2 veritabanı türü | .NET framework türü |
| --- | --- |
| Tamsayı |Int16 |
| Tamsayı |Int32 |
| BigInt |Int64 |
| Real |Bekar |
| Çift |Çift |
| Kayan nokta |Çift |
| Ondalık |Ondalık |
| DecimalFloat |Ondalık |
| sayısal |Ondalık |
| Tarih |DateTime |
| Zaman |TimeSpan |
| Zaman damgası |DateTime |
| Xml |Byte] |
| char |Dize |
| VarChar |Dize |
| LongVarChar |Dize |
| DB2DynArray |Dize |
| İkili |Byte] |
| VarBinary |Byte] |
| LONGVARBINARY |Byte] |
| Grafiği |Dize |
| VarGraphic |Dize |
| LongVarGraphic |Dize |
| Clob |Dize |
| Blob |Byte] |
| DbClob |Dize |
| Tamsayı |Int16 |
| Tamsayı |Int32 |
| BigInt |Int64 |
| Real |Bekar |
| Çift |Çift |
| Kayan nokta |Çift |
| Ondalık |Ondalık |
| DecimalFloat |Ondalık |
| sayısal |Ondalık |
| Tarih |DateTime |
| Zaman |TimeSpan |
| Zaman damgası |DateTime |
| Xml |Byte] |
| char |Dize |

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak veri kümesinde sütun eşleme hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri deposundan verileri kopyaladığınızda, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Yeniden deneme de yapılandırabilirsiniz **İlkesi** özelliği için bir veri kümesi bir hata oluştuğunda bir dilimi yeniden çalıştırın. Aynı veri dilimi yeniden kaç kez olsun ve dilim yeniden nasıl bakılmaksızın okuduğunuzdan emin olun. Daha fazla bilgi için bkz: [Repeatable okur ilişkisel kaynaklardan](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama etkinliği ve performansı iyileştirmek için yollar performansını etkileyen anahtar Etkenler hakkında bilgi edinin [kopya etkinliği performansının ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
