---
title: Azure Data Factory kullanarak Salesforce veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak Salesforce desteklenen havuz veri depolarına kopyalama öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: jingwang
ms.openlocfilehash: c819b3e3e715427632e793ce77d31554914d248e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-salesforce-using-azure-data-factory"></a>Azure Data Factory kullanarak Salesforce verilerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-salesforce-connector.md)
> * [Sürüm 2 - Önizleme](connector-salesforce.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir Salesforce veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Salesforce Bağlayıcısı](v1/data-factory-salesforce-connector.md).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Tüm desteklenen havuz veri deposuna Salesforce veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Salesforce bağlayıcı Salesforce aşağıdaki sürümlerini destekler: **Geliştirici sürümü, Professional Edition, Enterprise Edition veya sınırsız Edition**. Salesforce veri kopyalamayı destekler **üretim, korumalı alan ve özel etki alanı**.

## <a name="prerequisites"></a>Ön koşullar

* API izni Salesforce'ta etkinleştirilmesi gerekir. Bkz: [nasıl Salesforce API erişim izni kümesi tarafından etkinleştirebilirim?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları

Salesforce toplam API istekleri ve eşzamanlı API istekleri için sınırları vardır. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı istek sayısı sınırı aşarsa, azaltma oluşur ve rastgele hatalara bakın.
- Toplam istek sayısı sınırı aşarsa, 24 saat Salesforce hesap engellendi.

Ayrıca, her iki senaryoda "REQUEST_LIMIT_EXCEEDED" hata iletisi. "API istek sınırları" bölümüne bakın [Salesforce Geliştirici sınırları](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) Ayrıntılar için makale.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, belirli Data Factory varlıklarını Salesforce bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Salesforce bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **Salesforce**. |Evet |
| environmentUrl | URL, Salesforce örneği belirtin. <br><br> -Varsayılan `"https://login.salesforce.com"`. <br> -Korumalı alan verileri kopyalamak için belirtmek `"https://test.salesforce.com"`. <br> Özel etki alanından veri kopyalamak için örneğin, belirlediğiniz `"https://[domain].my.salesforce.com"`. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| securityToken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bkz: [güvenlik belirteci alma getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) bir güvenlik belirteci sıfırlama/get ilgili yönergeler için. Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Evet |

**Örnek:**

```json
{
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        }
    },
    "name": "SalesforceLinkedService"
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Salesforce veri kümesi tarafından desteklenen özellikler listesini sağlar.

Salesforce verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | Salesforce veritabanı tablosunun adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Örnek:**

```json
{
    "name": "SalesforceDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable__c"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Salesforce kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="salesforce-as-source"></a>Salesforce kaynağı olarak

Salesforce verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. Bir SQL-92 sorgu kullanabilirsiniz veya [Salesforce nesnesi sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgu. Örneğin: `select * from MyTable__c`. | (Veri kümesi "tableName" belirtilmişse) yok |

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="query-tips"></a>Sorgu ipuçları

### <a name="retrieving-data-from-salesforce-report"></a>Salesforce raporundan veri alma

Sorgu olarak belirterek Salesforce raporlarını veri alabilirsiniz `{call "<report name>"}. Example: `"sorgu": "{çağrısı \"TestReport\"}"'.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Kayıtları Salesforce Geri Dönüşüm Kutusu'ndan silinen alma

Salesforce geri dönüşüm kutusu geçici silinen kayıtlarını sorgulamak için belirleyebileceğiniz **"IsDeleted = 1"** Sorgunuzdaki. Örneğin,

* Yalnızca Silinmiş kayıtlar sorgulamaya belirtin "seçin * MyTable__c gelen **burada IsDeleted = 1**"
* Tüm mevcut ve Silinen dahil olmak üzere kayıtlarını sorgulamak üzere belirtin "seçin * MyTable__c gelen **burada IsDeleted = 0 ya da IsDeleted = 1**"

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>WHERE kullanarak veri alma DateTime sütun yan tümcesi

Ne zaman SOQL veya SQL sorgusu DateTime biçimi fark dikkat belirtin. Örneğin:

* **SOQL örnek**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', <datetime parameter>, <datetime parameter>)`
* **SQL örneği**:`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', <datetime parameter>, <datetime parameter>)`

## <a name="data-type-mapping-for-salesforce"></a>Salesforce için eşleme veri türü

Salesforce veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Salesforce veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Salesforce veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Otomatik numara |Dize |
| Onay kutusu |Boole değeri |
| Para birimi |Çift |
| Tarih |Tarih saat |
| Tarih/saat |Tarih saat |
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


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).