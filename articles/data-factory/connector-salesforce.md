---
title: Azure Data Factory kullanarak ilk ve son Salesforce veri kopyalama | Microsoft Docs
description: "Bir data factory işlem hattı kopyalama etkinliği kullanarak desteklenen havuz veri depolarına Salesforce veya desteklenen kaynak veri depolarına Salesforce veri kopyalamak öğrenin."
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
ms.date: 02/26/2018
ms.author: jingwang
ms.openlocfilehash: 3d48f1f3df7b626ec33b07b6275581821453f626
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Azure Data Factory kullanarak ilk ve son Salesforce veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel kullanıma sunuldu](v1/data-factory-salesforce-connector.md)
> * [Sürüm 2 - Önizleme](connector-salesforce.md)

Bu makalede kopya etkinliği Azure Data Factory'de ilk ve son Salesforce veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, veri fabrikası 1 sürümünü kullanıyorsanız bkz [Salesforce Bağlayıcısı sürüm 1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Salesforce verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Ayrıca, tüm desteklenen kaynak veri deposundan Salesforce veri kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Salesforce bağlayıcı destekler:

- Salesforce geliştirici, Professional, Enterprise veya sınırsız sürümleri.
- İlk ve son Salesforce üretim, korumalı alan ve özel etki alanı veri kopyalama.

## <a name="prerequisites"></a>Önkoşullar

API izni Salesforce'ta etkinleştirilmesi gerekir. Daha fazla bilgi için bkz: [Salesforce API etkinleştir erişim izni kümesi tarafından](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları

Salesforce toplam API istekleri ve eşzamanlı API istekleri için sınırları vardır. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı istek sayısı sınırı aşarsa, azaltma oluşur ve rastgele hatalara bakın.
- Toplam istek sayısı sınırı aşarsa, 24 saat Salesforce hesap engellendi.

Ayrıca, her iki senaryoda "REQUEST_LIMIT_EXCEEDED" hata iletisini alabilirsiniz. Daha fazla bilgi için "API istek sınırlarını" bölümüne bakın [Salesforce Geliştirici sınırları](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Salesforce bağlayıcısıyla Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri bağlantılı Salesforce hizmeti için desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlamak **Salesforce**. |Evet |
| environmentUrl | Salesforce örneği URL'sini belirtin. <br> -Varsayılan `"https://login.salesforce.com"`. <br> -Korumalı alan verileri kopyalamak için belirtmek `"https://test.salesforce.com"`. <br> Özel etki alanından veri kopyalamak için örneğin, belirlediğiniz `"https://[domain].my.salesforce.com"`. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin.<br/><br/>Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| securityToken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Sıfırlama ve bir güvenlik belirteci almak yönergeler için bkz: [bir güvenlik belirteci alma](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak bağlanmışsa Hayır kaynak için Evet havuz için hizmet tümleştirmesi çalışma zamanı yok |

>[!IMPORTANT]
>Salesforce veri kopyaladığınızda, Azure tümleştirmesi çalışma zamanı varsayılan kopyalama yürütmek için kullanılamaz. Kaynağınız bağlı diğer bir deyişle, hizmet belirtilen tümleştirmesi çalışma zamanı açıkça yok [Azure tümleştirmesi çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Salesforce örneğinizi yakın bir konum. Aşağıdaki örnekte olduğu gibi Salesforce bağlantılı hizmet ilişkilendirin.

**Örnek: veri fabrikasında kimlik bilgilerini depolamak**

```json
{
    "name": "SalesforceLinkedService",
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
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: anahtar kasasına kimlik bilgilerini depolamak**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Salesforce veri kümesi tarafından desteklenen özellikler listesini sağlar.

İlk ve son Salesforce verileri kopyalamak için kümesine tür özelliği ayarlamak **SalesforceObject**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **SalesforceObject**.  | Evet |
| objectApiName | Verilerin alınacağı Salesforce nesne adı. | Kaynak havuzu için Evet için Hayır'ı |

> [!IMPORTANT]
> "__C" bölümünü **API adı** herhangi bir özel nesnesi için gereklidir.

![Veri Fabrikası Salesforce Bağlantısı API adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Örnek:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Geriye dönük uyumluluk için: önceki "RelationalTable" türü veri kümesi kullanıyorsanız, Salesforce verileri kopyaladığınızda, yeni "SalesforceObject" türüne geçmek için bir öneri bakın çalışıp tutar.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak **RelationalTable**. | Evet |
| tableName | Salesforce tablo adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Salesforce kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="salesforce-as-a-source-type"></a>Salesforce bir kaynak türü

Salesforce verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SalesforceSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **SalesforceSource**. | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. Bir SQL-92 sorgu kullanabilirsiniz veya [Salesforce nesnesi sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgu. `select * from MyTable__c` bunun bir örneğidir. | (Veri kümesinde "tableName" belirtilmişse) yok |
| readBehavior | Silinen olanlar da dahil olmak üzere tüm kayıtları sorgu veya mevcut kayıtları sorguyu gösterir. Belirtilmezse, varsayılan davranışı eski olur. <br>İzin verilen değerler: **sorgu** (varsayılan), **queryAll**.  | Hayır |

> [!IMPORTANT]
> "__C" bölümünü **API adı** herhangi bir özel nesnesi için gereklidir.

![Veri Fabrikası Salesforce Bağlantısı API adı listesi](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Geriye dönük uyumluluk için: önceki "RelationalSource" türü kopya kullanırsanız Salesforce verileri kopyaladığınızda, yeni "SalesforceSource" türüne geçmek için bir öneri bakın kaynak çalışma tutar.

### <a name="salesforce-as-a-sink-type"></a>Havuz türü olarak Salesforce

Salesforce veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SalesforceSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak **SalesforceSink**. | Evet |
| writeBehavior | İşlem için yazma davranışı.<br/>İzin verilen değerler **Ekle** ve **Upsert**. | Hayır (varsayılan değer Ekle) |
| externalIdFieldName | Dış kimliği alan adını upsert işlemi için. Belirtilen alan "Dış kimlik alanı olarak" Salesforce nesnesinde tanımlanması gerekir. İlgili girdi verileri NULL değerlere sahip olamaz. | "Upsert" için Evet |
| writeBatchSize | Salesforce her toplu işlemde yazılan veriler satır sayısı. | Hayır (varsayılan değer 5000) |
| ignoreNullValues | Giriş verilerinden NULL değerler yazma işlemi sırasında yoksay gösterir.<br/>İzin verilen değerler **true** ve **false**.<br>- **Doğru**: hedef nesnedeki verileri bir upsert veya güncelleştirme işlemi yaptığınızda değiştirmeden bırakın. Bir ekleme işlemi yaptığınızda tanımlanan varsayılan bir değer ekleyin.<br/>- **Yanlış**: upsert veya güncelleştirme işlemi yaptığınızda, hedef nesnenin verileri NULL olarak güncelleştirir. Bir ekleme işlemi yaptığınızda NULL bir değer ekleyin. | Hayır (varsayılan değer false) |

**Örnek: Salesforce havuzunda kopyalama etkinliği**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Sorgu ipuçları

### <a name="retrieve-data-from-a-salesforce-report"></a>Salesforce raporundan veri alın

Bir sorgu olarak belirterek Salesforce raporlarını veri alabilirsiniz `{call "<report name>"}`. `"query": "{call \"TestReport\"}"` bunun bir örneğidir.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Salesforce Geri Dönüşüm Kutusu'ndan silinen kayıtları alma

Salesforce dönüşüm geçici silinen kayıtlarını sorgulamak için belirleyebileceğiniz **"IsDeleted = 1"** Sorgunuzdaki. Örneğin:

* Yalnızca Silinmiş kayıtlar sorgulamaya belirtin "seçin * MyTable__c gelen **burada IsDeleted = 1**."
* Varolan dahil olmak üzere tüm kayıtları ve Silinen sorgulamak üzere belirtin "seçin * MyTable__c gelen **burada IsDeleted = 0 ya da IsDeleted = 1**."

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Where kullanarak veri almak DateTime sütun yan tümcesi

SOQL veya SQL sorgusu belirttiğinizde, DateTime biçimi fark dikkat edin. Örneğin:

* **SOQL örnek**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL örneği**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Salesforce için eşleme veri türü

Salesforce verileri kopyaladığınızda, aşağıdaki eşlemelerini Salesforce veri türlerinden Data Factory geçici veri türleri için kullanılır. Nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Salesforce veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Otomatik numara |Dize |
| Onay kutusu |Boole |
| Para birimi |Çift |
| Tarih |DateTime |
| Tarih/Saat |DateTime |
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
| URL'si |Dize |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).