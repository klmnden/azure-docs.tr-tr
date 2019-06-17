---
title: OData kaynaklardan veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak OData kaynaklardan veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: b2c665de94750c4c6f41bda47960fdb9ba17e819
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60824046"
---
# <a name="move-data-from-an-odata-source-using-azure-data-factory"></a>Gelen bir OData kaynak Azure Data Factory ile verileri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-odata-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-odata.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de OData Bağlayıcısı](../connector-odata.md).


Bu makalede, bir OData kaynaktan verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Bir OData kaynağından tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca veri taşımayı OData kaynaktan gelen verileri diğer veri depolarına bir OData kaynağına taşımak için değil ancak diğer veri depolarına destekler.

## <a name="supported-versions-and-authentication-types"></a>Desteklenen sürümler ve kimlik doğrulama türleri
Bu OData bağlayıcı destekleyen OData sürüm 3.0 ve 4.0 ve veri OData hem buluttan ve şirket içi OData kaynakları kopyalayabilirsiniz. İkincisi için veri yönetimi ağ geçidi yüklemeniz gerekir. Bkz: [şirket içi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) veri yönetimi ağ geçidi hakkında bilgi için makalenin.

Kimlik doğrulaması türleri desteklenir:

* Erişim için **bulut** OData akışındaki, kullanabileceğiniz anonim, temel (kullanıcı adı ve parola) veya Azure Active Directory tabanlı OAuth kimlik doğrulaması.
* Erişim için **şirket içi** OData akışındaki, kullanabileceğiniz anonim, temel (kullanıcı adı ve parola) veya Windows kimlik doğrulaması.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir OData kaynaktan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Bir OData kaynaktan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri Azure Blob OData kaynaktan kopyalama](#json-example-copy-data-from-odata-source-to-azure-blob) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli OData kaynağı tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, bağlı bir OData hizmetine özel JSON öğeleri için bir açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OData** |Evet |
| url |OData hizmeti URL'si. |Evet |
| authenticationType |OData kaynağına bağlanmak için kullanılan kimlik doğrulaması türü. <br/><br/> Bulut OData anonim, temel ve OAuth (Not Azure Active Directory tabanlı OAuth Azure Data Factory şu anda yalnızca destek) olası değerler şunlardır. <br/><br/> Anonim, temel ve Windows, şirket içi OData için olası değerler şunlardır. |Evet |
| username |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Evet (yalnızca temel kimlik doğrulaması kullanıyorsanız) |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Evet (yalnızca temel kimlik doğrulaması kullanıyorsanız) |
| authorizedCredential |OAuth kullanıyorsanız **Authorize** Data Factory Kopyalama Sihirbazı'nı veya düzenleyicide düğmesine tıklayın ve sonra da bu özelliğin değeri otomatik olarak oluşturulan kimlik bilgilerinizi girin. |Evet (yalnızca OAuth kimlik doğrulaması kullanıyorsanız) |
| gatewayName |Data Factory hizmetinin şirket içi OData hizmetine bağlanmak için kullanması gereken ağ geçidi adı. Yalnızca şirket içi OData kaynağı üzerindeki veri kopyalama, belirtin. |Hayır |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulaması kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulaması
```json
{
    "name": "ODataLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Şirket içi OData kaynağına erişme Windows kimlik doğrulaması kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a>OAuth kimlik doğrulaması erişen bulut OData kaynağı kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **ODataResource** (OData veri kümesini içeren) aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| path |OData kaynağı yolu |Hayır |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Etkinliğin typeProperties bölümündeki özellikler, diğer yandan her etkinlik türü ile değişir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **RelationalSource** (OData içeren) typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | Örnek | Gerekli |
| --- | --- | --- | --- |
| query |Verileri okumak için özel sorgu kullanın. |"? $select adı, açıklama ve $top = 5 =" |Hayır |

## <a name="type-mapping-for-odata"></a>OData için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir.

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

OData veri taşıma, aşağıdaki eşlemeler OData türlerinden .NET türü kullanılır.

| OData veri türü | .NET türü |
| --- | --- |
| Edm.Binary |Byte[] |
| Edm.Boolean |Bool |
| Edm.Byte |Byte[] |
| Edm.DateTime |DateTime |
| Edm.Decimal |Decimal |
| Edm.Double |Double |
| Edm.Single |Single |
| Edm.Guid |Guid |
| Edm.Int16 |Int16 |
| Edm.Int32 |Int32 |
| Edm.Int64 |Int64 |
| Edm.SByte |Int16 |
| Edm.String |String |
| Edm.Time |TimeSpan |
| Edm.DateTimeOffset |DateTimeOffset |

> [!Note]
> OData karmaşık veri türlerini örn nesnesi desteklenmez.

## <a name="json-example-copy-data-from-odata-source-to-azure-blob"></a>JSON örneği: Azure Blob OData kaynaktan veri kopyalayın
Bu örnekte kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, bir Azure Blob depolama alanına bir OData kaynaktan veri kopyalama işlemini göstermektedir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de. Örnek, aşağıdaki Data Factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OData](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [ODataResource](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek, bir Azure blob'a OData kaynağına karşı saatte sorgulamasını veri kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Bağlı bir OData hizmeti:** Bu örnek anonim kimlik doğrulamasını kullanır. Bkz: [OData bağlı hizmet](#linked-service-properties) bölüm için farklı türde kimlik doğrulaması kullanabilirsiniz.

```json
{
    "name": "ODataLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

**Azure depolama bağlı hizmeti:**

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

**OData giriş veri kümesi:**

"Dış" ayarını: "true" bildirir Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Belirtme **yolu** kümesinde tanımı isteğe bağlıdır.

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**OData kaynak ve havuz Blob ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği OData kaynak sunucudan en son (en yeni) verileri seçer.

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
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
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

Belirtme **sorgu** işlem hattında tanımı isteğe bağlıdır. **URL** Data Factory hizmetinin veri almak için kullanır: + Veri kümesi (isteğe bağlı) + (isteğe bağlı) işlem hattı sorguda belirtilen yolu (gerekli) bağlı hizmette belirtilen URL.

### <a name="type-mapping-for-odata"></a>OData için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki 2 adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

OData veri türleri, OData veri verileri depoladığında, .NET türlerine eşlenir.

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
