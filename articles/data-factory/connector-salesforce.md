---
title: Azure Data Factory kullanarak veri öğesinden ve salesforce'a kopyalama | Microsoft Docs
description: Bir data factory işlem hattında kopyalama etkinliği'ni kullanarak desteklenen havuz veri depolarına Salesforce veya salesforce'a desteklenen kaynak veri depolarından veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: jingwang
ms.openlocfilehash: 6056df9aa9079887bfb06ca20ad564eb52baff38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60546581"
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri öğesinden ve salesforce'a kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-salesforce-connector.md)
> * [Geçerli sürüm](connector-salesforce.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Salesforce veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Salesforce veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Ayrıca, tüm desteklenen kaynak veri deposundan Salesforce veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Salesforce Bağlayıcısı destekler:

- Salesforce geliştirici, Professional, Enterprise veya sınırsız sürümleri.
- Gelen ve Salesforce üretim, korumalı ve özel etki alanı veri kopyalama.

Salesforce Bağlayıcısı ile Salesforce REST/toplu API temelinde oluşturulmuştur [v45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) veri kopyalama ve [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) için veri kopyalayın.

## <a name="prerequisites"></a>Önkoşullar

API izin Salesforce'ta etkinleştirilmesi gerekir. Daha fazla bilgi için [izin kümesi tarafından salesforce'taki etkinleştirme API erişimi](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları

Salesforce API isteklerinin toplam hem eşzamanlı API istekleri için sınırlara sahiptir. Aşağıdaki noktalara dikkat edin:

- Eş zamanlı istek sayısı sınırı aşarsa, azaltma gerçekleşir ve rastgele hatalara bakın.
- Toplam istek sayısı sınırı aşarsa, Salesforce hesabındaki 24 saat boyunca engellenir.

Her iki senaryoda da "REQUEST_LIMIT_EXCEEDED" hata iletisini alabilirsiniz. "API isteği sınırları" bölümünde daha fazla bilgi için bkz. [Salesforce Geliştirici sınırları](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için Salesforce Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler Salesforce bağlı hizmeti için desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır **Salesforce**. |Evet |
| environmentUrl | Salesforce örneği URL'sini belirtin. <br> -Varsayılan `"https://login.salesforce.com"`. <br> Korumalı alan ' veri kopyalamak için belirtin `"https://test.salesforce.com"`. <br> Özel etki alanından veri kopyalamak için örneğin, belirlediğiniz `"https://[domain].my.salesforce.com"`. |Hayır |
| username |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin.<br/><br/>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| securityToken |Kullanıcı hesabı için güvenlik belirtecini belirtin. Sıfırla ve bir güvenlik belirteci almak yönergeler için bkz: [bir güvenlik belirteci alma](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Güvenlik belirteçleri hakkında genel bilgi edinmek için [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak bağlı Hayır kaynağı için Evet havuz için hizmet Integration runtime yok |

>[!IMPORTANT]
>Salesforce verileri kopyaladığınızda, kopyası yürütmek için varsayılan Azure Integration Runtime kullanılamaz. Kaynağınıza bağlı diğer bir deyişle, hizmet bir belirtilen bir tümleştirme çalışma zamanı açıkça yok [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Salesforce örneğinizin yakın bir konum. Aşağıdaki örnekte olduğu gibi Salesforce bağlı hizmet ilişkilendirin.

**Örnek: Data factory'de Store kimlik bilgileri**

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

**Örnek: Anahtar Kasası'nda Store kimlik bilgileri**

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Salesforce veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Gelen ve Salesforce veri kopyalamak için dataset öğesinin type özelliği ayarlamak **SalesforceObject**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **SalesforceObject**.  | Evet |
| objectApiName | Verileri almak için Salesforce nesne adı. | Kaynak, havuz için Evet Hayır |

> [!IMPORTANT]
> "__C" bölümünü **API adı** herhangi özel bir nesne için gereklidir.

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
>Geriye dönük uyumluluk için: Önceki "RelationalTable" türü veri kümesini kullanıyorsanız, Salesforce veri kopyaladığınızda, yeni bir "SalesforceObject" türe dönüştürmek için bir öneri gördüğünüz sırasında çalışmaya devam eder.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **RelationalTable**. | Evet |
| tableName | Salesforce'taki tablosunun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Salesforce kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="salesforce-as-a-source-type"></a>Salesforce kaynak türü

Salesforce veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SalesforceSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **SalesforceSource**. | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. Kullanabileceğiniz [Salesforce nesne sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgusu veya 92 SQL sorgusu. Daha fazla ipuçlarını bkz [sorgu ipuçları](#query-tips) bölümü. Sorgu belirtilmezse, veri kümesi "objectApiName" belirtilen Salesforce nesnesi tüm veriler alınır. | Yok (veri kümesinde "objectApiName" belirtilmişse) |
| readBehavior | Var olan kayıtların sorgu veya sorgu tüm kayıtları silinen olanlar da dahil olmak üzere görüntülenip görüntülenmeyeceğini gösterir. Belirtilmezse, varsayılan davranışı eski olur. <br>İzin verilen değerler: **sorgu** (varsayılan), **queryAll**.  | Hayır |

> [!IMPORTANT]
> "__C" bölümünü **API adı** herhangi özel bir nesne için gereklidir.

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
>Geriye dönük uyumluluk için: Önceki "RelationalSource" türü kopya kullanırsanız, Salesforce veri kopyaladığınızda, yeni bir "SalesforceSource" türe dönüştürmek için bir öneri görmenize rağmen kaynak çalışmaya devam eder.

### <a name="salesforce-as-a-sink-type"></a>Bir havuz türü olarak Salesforce

Salesforce veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SalesforceSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **SalesforceSink**. | Evet |
| writeBehavior | İşlem için yazma davranışı.<br/>İzin verilen değerler **Ekle** ve **Upsert**. | Hayır (varsayılan değer ekleme) |
| externalIdFieldName | Upsert işlem Dış kimlik alanının adı. Belirtilen alan, Salesforce nesne "Dış kimlik alanı" tanımlanmalıdır. İlgili girdi verileri NULL değerlere sahip olamaz. | "Upsert" için Evet |
| writeBatchSize | Salesforce'a her toplu işlemde yazılan veriler satır sayısı. | Hayır (varsayılan değer 5000) |
| ignoreNullValues | Giriş verilerinden NULL değerler yazma işlemi sırasında yok sayılacak belirtir.<br/>İzin verilen değerler **true** ve **false**.<br>- **True**: Verileri hedef nesneyi bir upsert veya güncelleştirme işlemi yaptığınızda değiştirmeden bırakın. Bir ekleme işlemi yaptığınızda, tanımlanan varsayılan bir değer ekleyin.<br/>- **False**: Bir upsert veya güncelleştirme işlemi yaptığınızda hedef nesnedeki verileri NULL olarak güncelleştirin. Bir ekleme işlemi yaptığınızda, bir NULL değer ekleyin. | Hayır (varsayılan değer: false) |

**Örnek: Kopyalama etkinliğindeki havuz Salesforce**

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

Bir sorgu olarak belirterek, Salesforce raporlarından veri alabilir `{call "<report name>"}`. `"query": "{call \"TestReport\"}"` bunun bir örneğidir.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Salesforce Geri Dönüşüm Kutusu'ndan silinen kayıtları alın

Salesforce dönüşüm geçici silinen kayıtlarını sorgulamak için belirtebileceğiniz `readBehavior` olarak `queryAll`. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>Sorgu söz dizimi SOQL ve SQL arasındaki fark

Salesforce veri kopyalama yapılırken, SOQL sorgu veya SQL sorgusu kullanabilirsiniz. Bu iki olmadığını farklı söz dizimi ve İşlevler destek unutmayın, bu karıştırmayın. Salesforce tarafından yerel olarak desteklenen SOQL sorgusunu kullanmak için önerilir. Aşağıdaki tabloda farklar listelenmektedir:

| Sözdizimi | SOQL modu | SQL modu |
|:--- |:--- |:--- |
| Sütun Seçimi | Ör sorgusunda kopyalanacak alanları listelemek gerekiyor `SELECT field1, filed2 FROM objectname` | `SELECT *` Sütun seçimini ek olarak desteklenir. |
| Tırnak işaretleri | Dosyalanmış nesne adları tırnak içine olamaz. | Alan/nesne adları, örneğin tırnak içine `SELECT "id" FROM "Account"` |
| Tarih/Saat biçimi |  Ayrıntılara bakın [burada](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm) ve sonraki bölümdeki örnekler. | Ayrıntılara bakın [burada](https://docs.microsoft.com/sql/odbc/reference/develop-app/date-time-and-timestamp-literals?view=sql-server-2017) ve sonraki bölümdeki örnekler. |
| Boole değerleri | Olarak temsil edilen `False` ve `True`, örneğin `SELECT … WHERE IsDeleted=True`. | Örneğin 0 veya 1 temsil edilen `SELECT … WHERE IsDeleted=1`. |
| Sütun yeniden adlandırma | Desteklenmiyor. | Desteklenir, örneğin: `SELECT a AS b FROM …`. |
| İlişki | Örneğin, desteklenen `Account_vod__r.nvs_Country__c`. | Desteklenmiyor. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Where kullanarak veri DateTime sütunu yan tümcesi

SOQL veya SQL sorgusu belirttiğinizde, DateTime biçimi farka dikkat edin. Örneğin:

* **SOQL örnek**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL örneği**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformedquerytruncated"></a>MALFORMED_QUERY hata: kesilmiş

Hata ulaşırsanız "MALFORMED_QUERY: Kesildi", normalde son olduğu JunctionIdList türü sütun veri varsa ve Salesforce gibi çok sayıda satırı verilerle destekleyen bir sınırlama. JunctionIdList sütununu hariç tutun veya kopyalamak için satır sayısını sınırlandırmak azaltmak için deneyin (birden çok kopyasını etkinlik çalıştırmalarını bölümleyebilirsiniz).

## <a name="data-type-mapping-for-salesforce"></a>Eşleme için Salesforce veri türü

Salesforce veri kopyaladığınızda, aşağıdaki eşlemeler Salesforce veri türlerinden veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemelerini nasıl hakkında bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Salesforce veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Auto Number |String |
| Checkbox |Boolean |
| Currency |Decimal |
| Date |DateTime |
| Date/Time |DateTime |
| Email |String |
| Id |String |
| Lookup Relationship |String |
| Multi-Select Picklist |String |
| Number |Decimal |
| Percent |Decimal |
| Phone |String |
| Picklist |String |
| Text |String |
| Text Area |String |
| Text Area (Long) |String |
| Text Area (Rich) |String |
| Text (Encrypted) |String |
| URL |String |

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
