---
title: Data Factory kullanarak verileri Salesforce'tan taşıma | Microsoft Docs
description: Azure Data Factory kullanarak verileri Salesforce'tan taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: aac1ed82a01477b081f4bc146f199eba87d97859
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60309207"
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Salesforce veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-salesforce-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-salesforce.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de Salesforce Bağlayıcısı](../connector-salesforce.md).

Bu makalede, kopyalama etkinliği Azure data factory'de Salesforce'tan havuz sütunu altında listelenen herhangi bir veri deposuna veri kopyalamak için nasıl kullanabilirsiniz özetlenmektedir [desteklenen kaynaklar ve havuzlar](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Bu makalede yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ve desteklenen veri deposu bileşimleri ile veri taşıma genel bir bakış sunar.

Azure Data Factory şu anda yalnızca Salesforce verileri destekler [havuz veri depolarına desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats), ancak diğer veriler veri taşıma desteği için Salesforce depolar.

## <a name="supported-versions"></a>Desteklenen sürümler
Bu bağlayıcı, Salesforce aşağıdaki sürümleri destekler: Geliştirici sürümü, Professional sürümü, Enterprise Edition veya sınırsız sürümü. Ve Salesforce üretim, korumalı ve özel etki alanı kopyalamayı destekler.

## <a name="prerequisites"></a>Önkoşullar
* API izin etkinleştirilmesi gerekir. Bkz: [nasıl izin kümesi tarafından API erişimi salesforce'taki etkinleştirebilirim?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* Şirket içi veri depoları için Salesforce veri kopyalamak için en az olmalıdır veri yönetimi ağ geçidi, şirket içi ortamınızda yüklü 2.0.

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları
Salesforce API isteklerinin toplam hem eşzamanlı API istekleri için sınırlara sahiptir. Aşağıdaki noktalara dikkat edin:

- Eş zamanlı istek sayısı sınırı aşarsa, azaltma gerçekleşir ve rastgele hatalara görürsünüz.
- Toplam istek sayısı sınırı aşarsa, 24 saat için Salesforce hesabındaki engellenir.

Her iki senaryoda da "REQUEST_LIMIT_EXCEEDED" hatası alabilirsiniz. "API isteği sınırları" bölümüne bakın [Salesforce Geliştirici sınırları](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) makale Ayrıntılar için.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak Salesforce veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Salesforce veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri Azure Blob Salesforce'tan kopyalama](#json-example-copy-data-from-salesforce-to-azure-blob) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli Salesforce'a tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, Salesforce bağlantılı hizmete özgü JSON öğelerinin açıklamaları verilmiştir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Salesforce**. |Evet |
| environmentUrl | URL, Salesforce örneği belirtin. <br><br> -Varsayılan değer "https:\//login.salesforce.com". <br> Korumalı alan ' veri kopyalamak için belirtin "https://test.salesforce.com". <br> Özel etki alanından veri kopyalamak için örneğin, "https://[domain].my.salesforce.com" belirtin. |Hayır |
| username |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| securityToken |Kullanıcı hesabı için güvenlik belirtecini belirtin. Bkz: [güvenlik belirteci alın getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) bir güvenlik belirteci sıfırlama/alma konusunda yönergeler için. Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri için (Azure SQL, Azure blob, Azure tablosu ve benzeri) benzerdir.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölüm türü için bir veri kümesi **RelationalTable** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Salesforce'taki tablosunun adı. |Hayır (varsa bir **sorgu** , **RelationalSource** belirtilir) |

> [!IMPORTANT]
> API adı "__c" bölümü, herhangi özel bir nesne için gereklidir.

![Data Factory - Salesforce Bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları özellikleri gibi ve çeşitli ilkeleri, tüm etkinlik türleri için kullanılabilir.

Etkinlik typeProperties bölümünün kullanılabilir özellikleri, diğer yandan, her etkinlik türü ile değişir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kopya etkinlikteki kaynak türü olduğunda, **RelationalSource** (Salesforce içeren), typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| query |Verileri okumak için özel sorgu kullanın. |Bir SQL 92 sorgu veya [Salesforce nesne sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgu. Örneğin: `select * from MyTable__c`. |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

> [!IMPORTANT]
> API adı "__c" bölümü, herhangi özel bir nesne için gereklidir.

![Data Factory - Salesforce Bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Sorgu ipuçları
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>WHERE kullanarak veri alma DateTime sütunu yan tümcesi
Ne zaman SOQL veya SQL sorgusu DateTime biçimi fark dikkat belirtin. Örneğin:

* **SOQL örnek**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL örneği**:
    * **Bir sorgu belirtmek için kopyalama Sihirbazı'nı kullanma:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Bir sorgu belirtmek için düzenleme JSON kullanarak (doğru kaçış karakteri):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Salesforce raporundan veri alma
Sorgu olarak belirterek, Salesforce raporlarından veri alabilir `{call "<report name>"}`, örneğin,. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Salesforce Geri Dönüşüm Kutusu'ndan kayıtları silinen alma
Salesforce Geri Dönüşüm Kutusu'nu geçici silinen kayıtlarını sorgulamak için belirtebileceğiniz **"IsDeleted = 1"** sorgunuzda. Örneğin,

* Yalnızca silinen kayıtlar sorgulamak için belirtin "seçin * MyTable__c gelen **burada IsDeleted = 1**"
* Tüm mevcut ve Silinen dahil olmak üzere kayıtları sorgulamak için belirtin "seçin * MyTable__c gelen **burada IsDeleted = 0 ya da IsDeleted = 1**"

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a>JSON örneği: Azure Blob Salesforce veri kopyalayın
Aşağıdaki örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, verileri Azure Blob depolama alanına Salesforce'tan kopyalamak nasıl gösterir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

Senaryoyu uygulamak için oluşturmak için ihtiyacınız olan Data Factory yapıtlarının aşağıda verilmiştir. Listenin aşağıdaki bölümlerde bu adımlar hakkında ayrıntılı bilgi sağlar.

* Bağlı hizmet türü [Salesforce](#linked-service-properties)
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties)
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**Salesforce bağlı hizmeti**

Bu örnekte **Salesforce** bağlı hizmeti. Bkz: [Salesforce bağlı hizmet](#linked-service-properties) bu bağlı hizmeti tarafından desteklenen özellikler bölümü. Bkz: [güvenlik belirteci alın getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) sıfırlama/Güvenlik belirtecini alma hakkında yönergeler için.

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
**Azure Storage bağlı hizmeti**

```json
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
**Salesforce giriş veri kümesi**

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
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

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

> [!IMPORTANT]
> API adı "__c" bölümü, herhangi özel bir nesne için gereklidir.

![Data Factory - Salesforce Bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama etkinliği ile işlem hattı**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış ve saatte bir çalışacak şekilde zamanlanmış kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource**ve **havuz** türü ayarlandığında **BlobSink**.

Bkz: [RelationalSource türü özellikleri](#copy-activity-properties) RelationalSource tarafından desteklenen özelliklerin listesi için.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]
    }
}
```
> [!IMPORTANT]
> API adı "__c" bölümü, herhangi özel bir nesne için gereklidir.

![Data Factory - Salesforce Bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Tür eşlemesi için Salesforce

| Salesforce türü | . AĞ tabanlı türü |
| --- | --- |
| Auto Number |String |
| Checkbox |Boolean |
| Para birimi |Decimal |
| Tarih |DateTime |
| Tarih/Saat |DateTime |
| Email |String |
| Kimlik |String |
| Lookup Relationship |String |
| Multi-Select Picklist |String |
| Sayı |Decimal |
| Yüzde |Decimal |
| Telefon |String |
| Picklist |String |
| Text |String |
| Text Area |String |
| Text Area (Long) |String |
| Text Area (Rich) |String |
| Text (Encrypted) |String |
| URL'si |String |

> [!NOTE]
> Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performans ve ayar
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
