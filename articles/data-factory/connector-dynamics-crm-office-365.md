---
title: Veri kopyalama/Dynamics CRM ve Azure Data Factory kullanarak 365 | Microsoft Docs
description: "Dynamics CRM verileri ve 365 desteklenen havuz veri depolarına (veya) desteklenen kaynak veri depolarına Dynamics CRM ve 365 kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak kopyalamak öğrenin."
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
ms.date: 11/09/2017
ms.author: jingwang
ms.openlocfilehash: ec1b9868ca94392cd00875ef2913d4c14a608110
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="copy-data-fromto-dynamics-365dynamics-crm-using-azure-data-factory"></a>Veri kopyalama/Dynamics 365 / Dynamics CRM Azure Data Factory kullanma

Bu makalede kopya etkinliği Azure Data Factory'de ilk ve son Dynamics 365 / Dynamics CRM verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Dynamics 365 / Dynamics CRM'den tüm desteklenen havuz veri deposuna kopyalamak ya da veri tüm desteklenen kaynak veri deposundan Dynamics 365 / Dynamics CRM kopyalayın. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Bu Dynamics bağlayıcı Dynamics sürümleri ve kimlik doğrulama türleri destekler (*IFD olduğu kısaltması Internet'e yönelik dağıtımınız*):

| Dynamics sürümleri | Kimlik doğrulama türleri | Bağlantılı hizmet örnekleri |
|:--- |:--- |:--- |
| Dynamics 365 çevrimiçi <br> Dynamics CRM online | Office365 | [Çevrimiçi Dynamics + Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 ile şirket içi IFD <br> Dynamics CRM 2016 ile şirket içi IFD <br> Dynamics CRM 2015 ile şirket içi IFD | IFD | [Dynamics ile şirket içi IFD + IFD kimlik doğrulama](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Dynamics 365 özellikle, aşağıdaki uygulama türleri desteklenir:

- Dynamics 365 satış
- Dynamics 365 Müşteri Hizmetleri
- Dynamics 365 alan hizmeti
- Dynamics 365 proje hizmet otomasyonu için
- Dynamics 365 pazarlama

> [!NOTE]
> Dynamics bağlayıcıyı kullanmak için Azure anahtar kasası parolanızı depolayın ve buradan kopyalama etkinliklere çekme veri kopyalama gerçekleştirirken sağlar. Bkz. yapılandırmak [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü.

## <a name="getting-started"></a>Başlarken

.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, belirli Data Factory varlıklarını Dynamics tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Dynamics bağlantılı hizmeti için desteklenir:

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 ve Dynamics CRM Online

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Dynamics**. | Evet |
| deploymentType | Dynamics örnek dağıtım türü. Olmalıdır **"Çevrimiçi"** Dynamics Çevrimiçi. | Evet |
| Kuruluş adı | Dynamics örneğinin kuruluş adı. | Kullanıcıyla ilişkili birden fazla Dynamics örnekleri olduğunda Hayır, belirtmeniz gerekir. |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulama türü. Belirtin **"Office365"** Dynamics Çevrimiçi. | Evet |
| kullanıcı adı | Dynamics bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Azure anahtar kasası parola koyun ve parola bir "AzureKeyVaultSecret" olarak yapılandırmanız gerekir. ' Dan daha fazla bilgi edinin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak havuzu için Evet için Hayır'ı |

>[!IMPORTANT]
>Verileri açıkça Dynamics kopyalamak için [Azure IR oluşturmak](create-azure-integration-runtime.md#create-azure-ir) Dynamics ve bağlantılı hizmet ilişkilendirme yakın bir konum aşağıdaki örnekteki gibi.

**Örnek: Dynamics Çevrimiçi Office365 kimlik doğrulaması kullanma**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "organizationName": "orga02d9c75",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
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

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 Dynamics CRM şirket içi ve IFD ile

*Dyanmics için çevrimiçi karşılaştırma ek özellikler "ana bilgisayar adı" ve "port" bağlıdır.*

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Dynamics**. | Evet |
| deploymentType | Dynamics örnek dağıtım türü. Olmalıdır **"OnPremisesWithIfd"** Dynamics ile şirket içi IFD için.| Evet |
| **ana bilgisayar adı** | Şirket içi Dynamics sunucusunun ana bilgisayar adı. | Evet |
| **bağlantı noktası** | Şirket içi Dynamics sunucusu bağlantı noktası. | Hayır, varsayılan ise 443'tür |
| Kuruluş adı | Dynamics örneğinin kuruluş adı. | Evet |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulama türü. Belirtin **"Ifd"** Dynamics ile şirket içi IFD için. | Evet |
| kullanıcı adı | Dynamics bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Azure anahtar kasası parola koyun ve parola bir "AzureKeyVaultSecret" olarak yapılandırmak zorunda unutmayın. ' Dan daha fazla bilgi edinin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak havuzu için Evet için Hayır'ı |

>[!IMPORTANT]
>Verileri açıkça Dynamics kopyalamak için [Azure IR oluşturmak](create-azure-integration-runtime.md#create-azure-ir) aşağıdaki örnekteki gibi Dynamics ve bağlantılı hizmet ilişkilendirme yakın konumla.

**Örnek: Dynamics ile şirket içi IFD kimlik doğrulaması kullanarak IFD**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Dynamics veri kümesi tarafından desteklenen özellikler listesini sağlar.

Veri kopyalama/Dynamics için veri kümesi türü özelliğini ayarlayın **DynamicsEntity**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **DynamicsEntity** |Evet |
| EntityName | Alınacak varlığın mantıksal adı. | Havuz için Evet ("etkinlik kaynağında sorgu" belirtilirse) kaynak için Hayır'ı |

> [!IMPORTANT]
>- **Veri Dynamics kopyalarken "yapısı" bölümü gereklidir** Dynamics kümesinde üzerinden kopyalamak istediğiniz Dynamics veriler için sütun adı ve veri türünü tanımlayan. ' Dan daha fazla bilgi edinin [veri kümesi yapısı](concepts-datasets-linked-services.md#dataset-structure) ve [Dynamics için veri türü eşlemesi](#data-type-mapping-for-dynamics).
>- **Dynamics veri kopyalama işlemi sırasında "yapısı" isteğe bağlı bölümüdür** Dynamics kümesindeki. Hangi sütunları içine kopyalamak için kaynak veri şeması tarafından belirlenir. Kaynağınız CSV dosyası üst bilgi içermeyen ise girdi veri kümesi "yapısı" sırada tek tek CSV dosyasındaki alanlar eşleştiren sütun adı ve veri türü belirtin.

**Örnek:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typePoperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Dynamics kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="dynamics-as-source"></a>Kaynak olarak Dynamics

Dynamics verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **DynamicsSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **DynamicsSource**  | Evet |
| sorgu  | FetchXML olan Microsoft Dynamics kullanılan özel sorgu dili (çevrimiçi & şirket içi). Aşağıdaki örnekte görebilir ve'dan daha fazla bilgi edinin [yapı FeachXML sorgularıyla](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | (Veri kümesinde "entityName" belirtilmişse) yok  |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>Örnek FetchXML sorgu

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-sink"></a>Havuz olarak Dynamics

Dynamics verileri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **DynamicsSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **DynamicsSink**  | Evet |
| WriteBehavior | İşlemi yazma davranışını.<br/>Değer izin verilen: **"Upsert"**. | Evet |
| writeBatchSize | Dynamics her toplu işlemde yazılan veriler satır sayısı. | Hayır (varsayılan değer 10) |
| ignoreNullValues | Yazma işlemi sırasında (dışında anahtar alanları) giriş verisi null değerleri yoksay gösterir.<br/>İzin verilen değerler: **true**, ve **false**.<br>-true: ekleme işlemi yaparken hedef veriler değişmeden upsert/güncelleştirme işlemini yaparken nesne ve Ekle bırakın tanımlanan varsayılan değeri.<br/>-false: upsert/güncelleştirme işlemini yaparken hedef nesnenin verileri NULL olarak güncelleştirir ve NULL değer ekleme işlemi yaparken ekleyin.  | Hayır (varsayılan değer false) |

>[!NOTE]
>Havuz writeBatchSize ve kopyalama etkinliği varsayılan değerini [parallelCopies](copy-activity-performance.md#parallel-copy) Dynamics eşzamanlı olarak gönderilmesini 100 kayıt başka bir deyişle, her iki 10 Dynamics havuz için olan.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Eşleme Dynamics için veri türü

Dynamics veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Dynamics veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

Veri kümesi yapısında, Dynamics veri kaynağına göre karşılık gelen Data Factory veri türünü yapılandırmak aşağıdaki eşleme tabloyu kullanarak yazın:

| Dynamics veri türü | Veri Fabrikası geçici veri türü | Kaynak olarak desteklenen | Havuzu olarak desteklenir |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Uzun | ✓ | ✓ |
| AttributeTypeCode.Boolean | Boole | ✓ | ✓ |
| AttributeType.Customer | GUID | ✓ |  |
| AttributeType.DateTime | Tarih saat | ✓ | ✓ |
| AttributeType.Decimal | Ondalık | ✓ | ✓ |
| AttributeType.Double | Çift | ✓ | ✓ |
| AttributeType.EntityName | Dize | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | GUID | ✓ |  |
| AttributeType.ManagedProperty | Boole | ✓ |  |
| AttributeType.Memo | Dize | ✓ | ✓ |
| AttributeType.Money | Ondalık | ✓ |  |
| AttributeType.Owner | GUID | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | GUID | ✓ | ✓ |
| AttributeType.String | Dize | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ |  |
| AttributeType.Status | Int32 | ✓ |  |


> [!NOTE]
> Dynamics veri türü AttributeType.CalendarRules ve AttributeType.PartyList desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).