---
title: "Azure Data Factory kullanarak/Salesforce için veri kopyalama | Microsoft Docs"
description: "Kopya etkinliği Azure Data Factory ardışık düzeninde kullanarak desteklenen havuz veri depolarına Salesforce (veya) Salesforce için desteklenen kaynak veri depolarına verileri kopyalamak öğrenin."
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
ms.date: 11/24/2017
ms.author: jingwang
ms.openlocfilehash: d0db2bd3a7e4d93a8d0690fcb4535c4552cef7ab
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="copy-data-fromto-salesforce-using-azure-data-factory"></a>Veri kopyalama/Azure Data Factory kullanarak Salesforce için
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-salesforce-connector.md)
> * [Sürüm 2 - Önizleme](connector-salesforce.md)

Bu makalede kopya etkinliği Azure Data Factory'de ilk ve son Salesforce veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Salesforce Bağlayıcısı](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Salesforce verileri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz veya veya veri kopyalama tüm desteklenen kaynak veri deposundan Salesforce. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Salesforce bağlayıcı destekler:

- Salesforce aşağıdaki sürümleri: **Geliştirici sürümü, Professional Edition, Enterprise Edition veya sınırsız Edition**.
- Veri kopyalama/Salesforce için **üretim, korumalı alan ve özel etki alanı**.

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
| environmentUrl | URL, Salesforce örneği belirtin. <br> -Varsayılan `"https://login.salesforce.com"`. <br> -Korumalı alan verileri kopyalamak için belirtmek `"https://test.salesforce.com"`. <br> Özel etki alanından veri kopyalamak için örneğin, belirlediğiniz `"https://[domain].my.salesforce.com"`. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin.<br/><br/>Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretle ve veri kopyalama gerçekleştirirken etkinliklere çekme buradan kopyalama-'dan daha fazla bilgi edinin ADF izin seçebilirsiniz [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). |Evet |
| securityToken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bkz: [güvenlik belirteci alma getirin](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) bir güvenlik belirteci sıfırlama/get ilgili yönergeler için. Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Bu alan ADF içinde güvenli şekilde depolayın veya güvenlik belirteci Azure anahtar kasası depolamak için bir SecureString olarak işaretle ve veri kopyalama gerçekleştirirken etkinliklere çekme buradan kopyalama-'dan daha fazla bilgi edinin ADF izin seçebilirsiniz [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak havuzu için Evet için Hayır'ı |

>[!IMPORTANT]
>Salesforce açıkça verileri kopyalamak için [Azure IR oluşturmak](create-azure-integration-runtime.md#create-azure-ir) Salesforce ve bağlantılı hizmet ilişkilendirme yakın bir konum aşağıdaki örnekteki gibi.

**Örnek: kimlik bilgisi ADF içinde depolamak.**

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

**Örnek: Azure anahtar kasası kimlik bilgisi depolanması**

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

/ Salesforce için verileri kopyalamak için kümesine tür özelliği ayarlamak **SalesforceObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SalesforceObject**  | Evet |
| objectApiName | Verilerin alınacağı Salesforce nesne adı. | Kaynak havuzu için Evet için Hayır'ı |

> [!IMPORTANT]
> API adı "__c" bölümü için herhangi bir özel nesne gereklidir.

![Veri Fabrikası - Salesforce bağlantı - API adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

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
>Yeni "SalesforceObject" türüne geçmek için önerilen sırada Salesforce veri kopyalama işlemi sırasında geri uyumluluk için, önceki "RelationalTable" türü veri kümesini kullanarak, çalışmaya devam.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | Salesforce tablo adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Salesforce kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="salesforce-as-source"></a>Salesforce kaynağı olarak

Salesforce verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SalesforceSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **SalesforceSource** | Evet |
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
>Yeni "SalesforceSource" türüne geçmek için önerilen sırada Salesforce veri kopyalama işlemi sırasında geri uyumluluk için, önceki "RelationalSource" türü kopyalama kaynağı kullanarak, çalışmaya devam.

### <a name="salesforce-as-sink"></a>Salesforce havuz olarak

Salesforce veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SalesforceSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **SalesforceSink** | Evet |
| WriteBehavior | İşlem için yazma davranışı.<br/>İzin verilen değerler: **Ekle**, ve **Upsert**. | Hayır (varsayılan değer Ekle) |
| externalIdFieldName | Dış kimliği alan adını upsert işlemi için. Salesforce nesnesindeki belirtilen alan "Dış kimlik alanı olarak" tanımlanması gerekir ve ilgili girdi verileri NULL değerler olamaz. | "Upsert" için Evet |
| writeBatchSize | Salesforce her toplu işlemde yazılan veriler satır sayısı. | Hayır (varsayılan değer 5000) |
| ignoreNullValues | Girdi null değerleri yoksaymak için veri yazma işlemi olup olmadığını gösterir.<br/>İzin verilen değerler: **true**, ve **false**.<br>- **doğru**: ekleme işlemi yaparken hedef veriler değişmeden upsert/güncelleştirme işlemini yaparken nesne ve Ekle bırakın tanımlanan varsayılan değeri.<br/>- **yanlış**: upsert/güncelleştirme işlemini yaparken hedef nesnenin verileri NULL olarak güncelleştirir ve NULL değer ekleme işlemi yaparken ekleyin. | Hayır (varsayılan değer false) |

### <a name="example-salesforce-sink-in-copy-activity"></a>Örnek: Salesforce havuzu kopyalama etkinliği

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

### <a name="retrieving-data-from-salesforce-report"></a>Salesforce raporundan veri alma

Sorgu olarak belirterek Salesforce raporlarını veri alabilirsiniz `{call "<report name>"}`. Örnek: `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Kayıtları Salesforce Geri Dönüşüm Kutusu'ndan silinen alma

Salesforce geri dönüşüm kutusu geçici silinen kayıtlarını sorgulamak için belirleyebileceğiniz **"IsDeleted = 1"** Sorgunuzdaki. Örneğin,

* Yalnızca Silinmiş kayıtlar sorgulamaya belirtin "seçin * MyTable__c gelen **burada IsDeleted = 1**"
* Tüm mevcut ve Silinen dahil olmak üzere kayıtlarını sorgulamak üzere belirtin "seçin * MyTable__c gelen **burada IsDeleted = 0 ya da IsDeleted = 1**"

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>WHERE kullanarak veri alma DateTime sütun yan tümcesi

Ne zaman SOQL veya SQL sorgusu DateTime biçimi fark dikkat belirtin. Örneğin:

* **SOQL örnek**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL örneği**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Salesforce için eşleme veri türü

Salesforce veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Salesforce veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Salesforce veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Otomatik numara |Dize |
| Onay kutusu |Boole |
| Para birimi |Çift |
| Tarih |Tarih Saat |
| Tarih/saat |Tarih Saat |
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
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).