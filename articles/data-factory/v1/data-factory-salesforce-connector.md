---
title: "Veri Fabrikası kullanarak Salesforce veri taşıma | Microsoft Docs"
description: "Azure Data Factory kullanarak Salesforce veri taşıma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 9e678e947a686b5a672af13cb0f0e60b4a272de9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Salesforce taşıma verileri
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-salesforce-connector.md)
> * [Sürüm 2 - Önizleme](../connector-salesforce.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Salesforce Bağlayıcısı](../connector-salesforce.md).


Bu makalede kopyalama etkinliği bir Azure data factory havuz sütunu altında listelenen herhangi bir veri deposuna Salesforce verileri kopyalamak için nasıl kullanacağınızı özetlenmektedir [desteklenen kaynakları ve havuzlarını](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Bu makalede derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) desteklenen veri deposu birleşimleri kopyalama etkinliği ile veri taşıma için genel bir bakış sunan makalesi.

Azure Data Factory şu anda yalnızca taşıma için Salesforce verilerden destekler [havuz veri depolarına desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats), ancak verileri diğer veriler taşıma desteklemediği için Salesforce depolar.

## <a name="supported-versions"></a>Desteklenen sürümler
Bu bağlayıcı Salesforce aşağıdaki sürümlerini destekler: Geliştirici sürümü, Professional Edition, Enterprise Edition veya sınırsız sürümü. Ve Salesforce üretim, korumalı alan ve özel etki alanı kopyalama destekler.

## <a name="prerequisites"></a>Önkoşullar
* API izni etkinleştirilmesi gerekir. Bkz: [nasıl Salesforce API erişim izni kümesi tarafından etkinleştirebilirim?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* Şirket içi veri depolarına Salesforce verileri kopyalamak için en az olmalıdır veri yönetimi ağ geçidi şirket içi ortamınızda yüklü 2.0.

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları
Salesforce toplam API istekleri ve eşzamanlı API istekleri için sınırları vardır. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı istek sayısı sınırı aşarsa, azaltma gerçekleşir ve rastgele hatalara görürsünüz.
- Toplam istek sayısı sınırı aşarsa, 24 saat Salesforce hesap engellenir.

Ayrıca, her iki senaryoda "REQUEST_LIMIT_EXCEEDED" hata iletisi. "API istek sınırları" bölümüne bakın [Salesforce Geliştirici sınırları](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) Ayrıntılar için makale.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak Salesforce verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Salesforce verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama Salesforce Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, belirli Data Factory varlıklarını Salesforce tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar: 

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda Salesforce bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Salesforce**. |Evet |
| environmentUrl | URL, Salesforce örneği belirtin. <br><br> -Varsayılan değer "https://login.salesforce.com" dir. <br> Korumalı alan veri kopyalamak için "https://test.salesforce.com" belirtin. <br> Özel etki alanından veri kopyalamak için örneğin, "https://[domain].my.salesforce.com" belirtin. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| securityToken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bkz: [güvenlik belirteci alma getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) bir güvenlik belirteci sıfırlama/get ilgili yönergeler için. Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümelerini tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için (Azure SQL, Azure blob, Azure tablo ve benzeri) benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **RelationalTable** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Salesforce tablo adı. |Hayır (varsa bir **sorgu** , **RelationalSource** belirtilir) |

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları özellikler gibi ve çeşitli ilkeleri etkinlikleri tüm türleri için kullanılabilir.

Etkinlik typeProperties bölümünde özellikler, diğer yandan, her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kopyalama etkinliğinde kaynak türü olduğunda **RelationalSource** (içeren Salesforce), aşağıdaki özellikler typeProperties bölümünde kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |Bir SQL-92 sorgu veya [Salesforce nesnesi sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgu. Örneğin:  `select * from MyTable__c`. |Hayır (varsa **tableName** , **dataset** belirtilir) |

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Sorgu ipuçları
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>WHERE kullanarak veri alma DateTime sütun yan tümcesi
Ne zaman SOQL veya SQL sorgusu DateTime biçimi fark dikkat belirtin. Örneğin:

* **SOQL örnek**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL örneği**:
    * **Sorgu belirtmek için kopyalama Sihirbazı'nı kullanarak:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Sorgu belirtmek için düzenleme JSON kullanarak (char düzgün kaçış):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Salesforce raporundan veri alma
Sorgu olarak belirterek Salesforce raporlarını veri alabilirsiniz `{call "<report name>"}`, örneğin,. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Kayıtları Salesforce Geri Dönüşüm Kutusu'ndan silinen alma
Salesforce geri dönüşüm kutusu geçici silinen kayıtlarını sorgulamak için belirleyebileceğiniz **"IsDeleted = 1"** Sorgunuzdaki. Örneğin,

* Yalnızca Silinmiş kayıtlar sorgulamaya belirtin "seçin * MyTable__c gelen **burada IsDeleted = 1**"
* Tüm mevcut ve Silinen dahil olmak üzere kayıtlarını sorgulamak üzere belirtin "seçin * MyTable__c gelen **burada IsDeleted = 0 ya da IsDeleted = 1**"

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a>JSON örnek: veri kopyalama Salesforce Azure Blob
Aşağıdaki örneği kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar Azure Blob depolama alanına Salesforce verileri kopyalamak nasıl gösterir. Ancak, veri herhangi belirtildiği havuzlarını kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.   

Burada, senaryoyu uygulamak için oluşturmanız gerekecek Data Factory yapıtlarının bulunmaktadır. Listede zleyen Bu adımlar hakkında ayrıntılı bilgi sağlar.

* Bağlı hizmet türü [Salesforce](#linked-service-properties)
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Bir giriş [dataset](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties)
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**Salesforce bağlı hizmet**

Bu örnekte **Salesforce** bağlı hizmeti. Bkz: [Salesforce bağlantılı hizmeti](#linked-service-properties) bu bağlantılı hizmet tarafından desteklenen özellikler bölümü.  Bkz: [güvenlik belirteci alma getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) sıfırlama/güvenlik belirteci alma hakkında yönergeler için.

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
**Salesforce girdi veri kümesi**

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

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Azure Blob çıktı veri kümesi**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1).

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

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırılmış ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **RelationalSource**ve **havuz** türü ayarlanmış **BlobSink**.

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
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Tür eşlemesi için Salesforce
| Salesforce türü | . NET tabanlı türü |
| --- | --- |
| Otomatik numara |Dize |
| Onay kutusu |Boole |
| Para birimi |Çift |
| Tarih |Tarih Saat |
| Tarih/Saat |Tarih Saat |
| E-posta |Dize |
| Kimlik |Dize |
| Arama ilişkisi |Dize |
| Çoklu seçim seçim listesi |Dize |
| Sayı |Çift |
| Yüzde |Çift |
| Telefon |Dize |
| Seçim listesi |Dize |
| Metin |Dize |
| Metin alanı |Dize |
| Metin alanı (uzun) |Dize |
| Metin alanı (zengin) |Dize |
| Metin (şifrelenmiş) |Dize |
| URL |Dize |

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performans ve ayar
Bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
