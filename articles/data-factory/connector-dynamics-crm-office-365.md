---
title: Azure Data Factory kullanarak ilk ve son Dynamics CRM veya Dynamics 365 veri kopyalama | Microsoft Docs
description: "Microsoft Dynamics CRM'den veri kopyalama öğrenin veya desteklenen için Microsoft Dynamics 365 veri depolarına havuzu veya gelen kaynak desteklenen veri depoları Dynamics CRM veya Dynamics 365 bir data factory işlem hattı kopyalama etkinliği kullanarak."
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
ms.date: 01/30/2018
ms.author: jingwang
ms.openlocfilehash: 9481d8d9bbdb5081eae9b9a3d4b9a280cba86be5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="copy-data-from-and-to-dynamics-365-or-dynamics-crm-by-using-azure-data-factory"></a>Azure Data Factory kullanarak ilk ve son Dynamics 365 veya Dynamics CRM veri kopyalama

Bu makalede kopya etkinliği Azure Data Factory'de gelen ve Microsoft Dynamics 365 veya Microsoft Dynamics CRM veri kopyalamak için nasıl kullanılacağını açıklar. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, veri fabrikası 1 sürümünü kullanıyorsanız bkz [sürüm 1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri herhangi bir desteklenen havuz veri deposuna Dynamics 365 veya Dynamics CRM kopyalayabilirsiniz. Tüm desteklenen kaynak veri deposundan Dynamics 365 veya Dynamics CRM veri kopyalayabilirsiniz. Kaynakları veya havuzlarını olarak kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Bu Dynamics bağlayıcı aşağıdaki Dynamics sürümleri ve kimlik doğrulama türlerini destekler. (IFD kısaltması Internet'e dağıtımıdır.)

| Dynamics sürümleri | Kimlik doğrulama türleri | Bağlantılı hizmet örnekleri |
|:--- |:--- |:--- |
| Dynamics 365 çevrimiçi <br> Dynamics CRM Online | Office365 | [Çevrimiçi Dynamics + Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 ile şirket içi IFD <br> Dynamics CRM 2016 ile şirket içi IFD <br> Dynamics CRM 2015 ile şirket içi IFD | IFD | [Dynamics ile şirket içi IFD + IFD kimlik doğrulama](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Dynamics 365 özellikle, aşağıdaki uygulama türleri desteklenir:

- Dynamics 365 satış
- Dynamics 365 Müşteri Hizmetleri
- Dynamics 365 alan hizmeti
- Dynamics 365 proje hizmet otomasyonu için
- Dynamics 365 pazarlama

Diğer uygulama türleri ör işlemleri ve Finans, beceri, vb. desteklenmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Dynamics tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Dynamics bağlantılı hizmeti için desteklenir.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 ve Dynamics CRM Online

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **Dynamics**. | Evet |
| deploymentType | Dynamics örnek dağıtım türü. Bunun olması **"Çevrimiçi"** Dynamics Çevrimiçi. | Evet |
| Kuruluş adı | Dynamics örneğinin kuruluş adı. | Kullanıcıyla ilişkili birden fazla Dynamics örnekleri olduğunda Hayır, belirtmeniz gerekir |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulama türü. Belirtin **"Office365"** Dynamics Çevrimiçi. | Evet |
| kullanıcı adı | Dynamics bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak bağlanmışsa Hayır kaynak için Evet havuz için hizmet tümleştirmesi çalışma zamanı yok |

>[!IMPORTANT]
>Dynamics veri kopyaladığınızda, Azure tümleştirmesi çalışma zamanı varsayılan kopyalama yürütmek için kullanılamaz. Kaynağınız bağlı diğer bir deyişle, hizmet belirtilen tümleştirmesi çalışma zamanı açıkça yok [Azure tümleştirmesi çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Dynamics örneğinizi yakın bir konum. Aşağıdaki örnekte olduğu gibi Dynamics bağlantılı hizmetindeki ilişkilendirin.

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
                "type": "SecureString",
                "value": "<password>"
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

*Dynamics Çevrimiçi karşılaştırmak ek özellikler "ana bilgisayar adı" ve "port" bağlıdır.*

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **Dynamics**. | Evet |
| deploymentType | Dynamics örnek dağıtım türü. Bunun olması **"OnPremisesWithIfd"** Dynamics ile şirket içi IFD için.| Evet |
| hostName | Şirket içi Dynamics sunucusunun ana bilgisayar adı. | Evet |
| port | Şirket içi Dynamics sunucusu bağlantı noktası. | Hayır, varsayılan ise 443'tür |
| Kuruluş adı | Dynamics örneğinin kuruluş adı. | Evet |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulama türü. Belirtin **"Ifd"** Dynamics ile şirket içi IFD için. | Evet |
| kullanıcı adı | Dynamics bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak havuzu için Evet için Hayır'ı |

>[!IMPORTANT]
>Verileri açıkça Dynamics kopyalamak için [Azure tümleştirmesi çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Dynamics örneğinizi yakın konumu ile. Aşağıdaki örnekte olduğu gibi bağlantılı hizmet ilişkilendirin.

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
                "type": "SecureString",
                "value": "<password>"
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

İlk ve son Dynamics verileri kopyalamak için kümesine tür özelliği ayarlamak **DynamicsEntity**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak **DynamicsEntity**. |Evet |
| EntityName | Alınacak varlığın mantıksal adı. | Havuz için Evet (etkinlik kaynağındaki "sorgu" belirtilmişse) kaynak için Hayır'ı |

> [!IMPORTANT]
>- Dynamics veri kopyaladığınızda, "yapısı" bölümü Dynamics kümesinde gereklidir. Üzerinden kopyalamak istediğiniz Dynamics veri sütun adı ve veri türü tanımlar. Daha fazla bilgi için bkz: [veri kümesi yapısı](concepts-datasets-linked-services.md#dataset-structure) ve [Dynamics için veri türü eşlemesi](#data-type-mapping-for-dynamics).
>- Dynamics veri kopyaladığınızda, "yapısı" bölümü Dynamics kümesinde isteğe bağlıdır. Hangi sütunların kopyalayın kaynak veri şema tarafından belirlenir. Kaynağınız bir üst bilgi içermeyen bir CSV dosyası ise girdi veri kümesi "yapısı" sütun adı ve veri türü ile belirtin. Bunlar, sipariş CSV dosyasında tek tek alanlarda eşlenir.

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Dynamics kaynak ve havuz türleri tarafından desteklenen özellikler listesini sağlar.

### <a name="dynamics-as-a-source-type"></a>Bir kaynak türü olarak Dynamics

Dynamics verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **DynamicsSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **DynamicsSource**. | Evet |
| sorgu | FetchXML olduğu Dynamics kullanılan özel sorgu dili (çevrimiçi ve şirket içi). Aşağıdaki örneğe bakın. Daha fazla bilgi için bkz: [yapı FeachXML sorgularıyla](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | (Veri kümesinde "entityName" belirtilmişse) yok |

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

### <a name="dynamics-as-a-sink-type"></a>Havuz türü olarak Dynamics

Dynamics verileri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **DynamicsSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak **DynamicsSink**. | Evet |
| writeBehavior | İşlemi yazma davranışını.<br/>Değer izin verilen **"Upsert"**. | Evet |
| writeBatchSize | Dynamics her toplu işlemde yazılan veriler satır sayısı. | Hayır (varsayılan değer 10) |
| ignoreNullValues | Bir yazma işlemi sırasında (dışında anahtar alanları) giriş verisi null değerleri yoksay gösterir.<br/>İzin verilen değerler **true** ve **false**.<br>- **Doğru**: hedef nesnedeki verileri upsert/güncelleştirme işlemini yaptığınızda değiştirmeden bırakın. Bir ekleme işlemi yaptığınızda tanımlanan varsayılan bir değer ekleyin.<br/>- **Yanlış**: upsert/güncelleştirme işlemi yaptığınızda, hedef nesnenin verileri NULL olarak güncelleştirir. Bir ekleme işlemi yaptığınızda NULL bir değer ekleyin. | Hayır (varsayılan değer false) |

>[!NOTE]
>Havuz writeBatchSize ve kopyalama etkinliği varsayılan değerini [parallelCopies](copy-activity-performance.md#parallel-copy) Dynamics havuz için her iki 10 olan. Bu nedenle, 100 kayıt, Dynamics eşzamanlı olarak gönderilir.

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

Dynamics veri kopyaladığınızda, aşağıdaki eşlemelerini Dynamics veri türlerinden Data Factory geçici veri türleri için kullanılır. Kopyalama etkinliği kaynak şema ve veri türü için havuz nasıl eşlendiğini öğrenmek için bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

Aşağıdaki eşleme tablosunu kullanarak, kaynağına Dynamics veri türüne göre bir veri kümesi yapısında karşılık gelen Data Factory veri türünü yapılandırın.

| Dynamics veri türü | Veri Fabrikası geçici veri türü | Kaynak olarak desteklenen | Havuzu olarak desteklenir |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Uzun | ✓ | ✓ |
| AttributeTypeCode.Boolean | Boole | ✓ | ✓ |
| AttributeType.Customer | Guid | ✓ | |
| AttributeType.DateTime | Tarih saat | ✓ | ✓ |
| AttributeType.Decimal | Ondalık | ✓ | ✓ |
| AttributeType.Double | Çift | ✓ | ✓ |
| AttributeType.EntityName | Dize | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | Guid | ✓ | |
| AttributeType.ManagedProperty | Boole | ✓ | |
| AttributeType.Memo | Dize | ✓ | ✓ |
| AttributeType.Money | Ondalık | ✓ | ✓ |
| AttributeType.Owner | Guid | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | Guid | ✓ | ✓ |
| AttributeType.String | Dize | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> AttributeType.CalendarRules ve AttributeType.PartyList Dynamics veri türleri desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
