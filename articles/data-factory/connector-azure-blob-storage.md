---
title: Data Factory kullanarak ya da Azure Blob depolamadan/depolamaya veri kopyalama | Microsoft Docs
description: Data Factory kullanarak Azure Blob Depolama için desteklenen kaynak veri depolarından veya desteklenen havuz veri depolarına Blob depolamadan veri kopyalama hakkında bilgi edinin.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 1ff20322f1d4f6024d4f41037ca18c327a0cc21f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233192"
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure Blob depolamadan/depolamaya veri kopyalayın
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-blob-connector.md)
> * [Geçerli sürüm](connector-azure-blob-storage.md)

Bu makalede, Azure Blob Depolama ve veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Blob Bağlayıcısı için aşağıdaki etkinlikleri desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak/havuz Matrisi](copy-activity-overview.md)
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)

Özellikle, bu Blob Depolama Bağlayıcısı destekler:

- Gelen genel amaçlı Azure depolama hesapları ve sık/seyrek erişimli blob depolama ve BLOB'ları kopyalanıyor. 
- Azure kaynaklarında kimlik doğrulamaları için hesap anahtarı, hizmet paylaşılan erişim imzası, asıl veya yönetilen hizmet kimliği kullanarak BLOB'ları kopyalanıyor.
- Blok kopyalama bloblarından eklemek ya da blok blobları, sayfa blobları ve yalnızca veriler kopyalanıyor.
- Veya ayrıştırma veya oluşturma gibi blobları kopyalama blobları ile [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

>[!NOTE]
>Etkinleştirirseniz _"İzin ver güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin"_ ADF olmadığından, Azure depolama Güvenlik Duvarı ayarları, Azure Integration Runtime'ı kullanarak Blob depolama alanına bağlanma seçeneği bir Yasak hatası ile başarısız olur Güvenilen Microsoft hizmet olarak kabul edilir. Bunun yerine şirket içinde barındırılan tümleştirme çalışma zamanı bağlanın.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Blob depolama alanına belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Blob Bağlayıcısı aşağıdaki kimlik doğrulama türlerini desteklemek için ilgili ayrıntılar bölümüne bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Paylaşılan erişim imzası kimlik doğrulaması](#shared-access-signature-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik](#managed-identity)

>[!NOTE]
>Hdınsights, Azure Machine Learning ve Azure SQL veri ambarı PolyBase yükleme yalnızca Azure Blob Depolama hesabı anahtar kimlik doğrulamasını destekler.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtarı kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobStorage** (önerilen) veya **AzureStorage** (aşağıdaki notlara bakın). |Evet |
| connectionString | ConnectionString özelliği için depolama alanına bağlanmak için gereken bilgileri belirtin. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Hesap anahtarı Azure Key Vault ve çekme koyabilirsiniz `accountKey` yapılandırma bağlantı dizesini dışında. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağdaysa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" tür bağlı hizmeti kullanıyorsanız, hala olarak desteklenmektedir-olduğu sırada bu yeni "AzureBlobStorage" kullanmak için bağlı hizmet türü ileriye dönük önerilir.

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: hesap anahtarı Azure Key Vault'ta depolama**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;"
            },
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }            
    }
}
```

### <a name="shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir istemci belirli bir süre boyunca depolama hesabınızdaki nesnelere sınırlı vermek için paylaşılan erişim imzası kullanabilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası için depolama kaynak kimliği doğrulanmış erişim için gerekli tüm bilgileri sorgu parametrelerini kapsayan bir URI'dir. Paylaşılan erişim imzası ile depolama kaynaklarına erişmek için istemci yalnızca uygun oluşturucu veya yöntemi için paylaşılan erişim imzasını geçmesi gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları: Paylaşılan erişim imzası modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory artık destekler hem de **hizmet paylaşılan erişim imzaları** ve **hesabı paylaşılan erişim imzaları**. Bu iki tür ve bunları oluşturmak nasıl hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları türleri](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures).
>- Daha sonra veri kümesi Yapılandırması'nda klasör yolu kapsayıcı düzeyinden başlayarak mutlak bir yoludur. Bir yolu, SAS URI'sini birlikte hizalanır yapılandırmanız gerekir.

> [!TIP]
> Depolama hesabınız için hizmet paylaşılan erişim imzası oluşturmak için aşağıdaki PowerShell komutlarını yürütebilirsiniz. Yer tutucuları değiştirmeniz ve gerekli izni verin.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Paylaşılan erişim imzası kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobStorage** (önerilen) veya **AzureStorage** (aşağıdaki notlara bakın). |Evet |
| sasUri | Blob/kapsayıcı gibi depolama kaynakları için paylaşılan erişim imzası URI'si belirtin. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Ayrıca, otomatik döndürme yararlanın ve belirteç bölümünü kaldırmak için Azure Key Vault'ta SAS belirteci koyabilirsiniz. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanının kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" tür bağlı hizmeti kullanıyorsanız, hala olarak desteklenmektedir-olduğu sırada bu yeni "AzureBlobStorage" kullanmak için bağlı hizmet türü ileriye dönük önerilir.

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<container>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: hesap anahtarı Azure Key Vault'ta depolama**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<container>.blob.core.windows.net/>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Paylaşılan erişim imzası URI'si oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Uygun okuma/yazma izinleri (okuma, yazma, okuma/yazma) bağlı hizmet, data factory'de nasıl kullanıldığına göre nesneler üzerinde ayarlayın.
- Ayarlama **süre sonu** uygun şekilde. İşlem hattının etkin dönemini içinde depolama nesnelerine erişim süresi sona ermiyor emin olun.
- URI, ihtiyaca göre doğru kapsayıcı/blob sırasında oluşturulmalıdır. Data Factory, bu belirli bir bloba erişmek bir blob için paylaşılan erişim imzası URI'si sağlar. Data Factory, bu kapsayıcıdaki blobları yinelemek Blob Depolama kapsayıcısına bir paylaşılan erişim imzası URI'si sağlar. Daha sonra daha fazla veya daha az nesne erişim sağlamak veya paylaşılan erişim imzası URI'si güncelleştirmek için yeni bir URI'ya bağlı hizmet güncelleştirmeyi unutmayın.

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Azure depolama için hizmet sorumlusu kimlik doğrulaması için genel olarak, başvuru [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](../storage/common/storage-auth-aad.md).

Hizmet sorumlusu kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. Azure Active Directory (Azure AD) uygulama varlık kaydınızı [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusu uygun Azure Blob Depolama alanında izni. Başvurmak [RBAC Azure depolama verileriyle Yönet erişim hakları](../storage/common/storage-auth-aad-rbac.md) rolleri hakkında daha fazla ayrıntı içeren.

    - **Kaynak olarak**, erişim denetimi (IAM), en az izni **depolama Blob verileri okuyucu** rol.
    - **Havuz olarak**, erişim denetimi (IAM), en az izni **depolama Blob verileri katkıda bulunan** rol.

Bu özellikler bir Azure Blob Depolama bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobStorage**. |Evet |
| serviceEndpoint | Azure Blob Depolama Hizmeti uç noktası ile desenini belirtin `https://<accountName>.blob.core.windows.net/`. |Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| tek | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağdaysa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Hizmet sorumlusu kimlik doğrulaması yalnızca "AzureBlobStorage" türü tarafından desteklenen bağlı hizmeti, ancak değil önceki "AzureStorage" bağlantılı hizmet türü.

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Bu yönetilen kimlik, Blob Depolama kimlik doğrulama kendi hizmet sorumlusunu kullanmaya benzer doğrudan kullanabilirsiniz. Bu belirlenen / için Blob depolamanızın erişim ve kopyalama veri fabrikasına sağlar.

Başvurmak [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](../storage/common/storage-auth-aad.md) genel Azure depolama, kimlik doğrulaması için. Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak için bu adımları izleyin:

1. [Data factory yönetilen kimlik bilgileri almak](data-factory-service-identity.md#retrieve-managed-identity) "Hizmet kimliği uygulama fabrikanızı birlikte oluşturulan kimliği" değerini kopyalayarak.

2. Azure Blob Depolama alanında yönetilen kimlik uygun izni verin. Başvurmak [RBAC Azure depolama verileriyle Yönet erişim hakları](../storage/common/storage-auth-aad-rbac.md) rolleri hakkında daha fazla ayrıntı içeren.

    - **Kaynak olarak**, erişim denetimi (IAM), en az izni **depolama Blob verileri okuyucu** rol.
    - **Havuz olarak**, erişim denetimi (IAM), en az izni **depolama Blob verileri katkıda bulunan** rol.

Bu özellikler bir Azure Blob Depolama bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobStorage**. |Evet |
| serviceEndpoint | Azure Blob Depolama Hizmeti uç noktası ile desenini belirtin `https://<accountName>.blob.core.windows.net/`. |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağdaysa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

> [!NOTE]
> Kimlik doğrulaması yalnızca "AzureBlobStorage" tarafından desteklenen Azure kaynakları için yönetilen kimlikleri bağlantılı hizmet türü ancak değil önceki "AzureStorage" bağlantılı hizmet türü. 

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

- İçin **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi veri kümesi](#parquet-and-delimited-text-format-dataset) bölümü.
- Diğer biçimlerden için **ORC/Avro/JSON/ikili biçimi**, başvurmak [diğer biçim veri kümesi](#other-format-dataset) bölümü.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet ve sınırlandırılmış metin biçimi veri kümesi

Parquet sınırlandırılmış metin biçimi de Blob Depolama ve veri kopyalamak için başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı bir veri kümesi ve desteklenen ayarlar. Aşağıdaki özellikler altında Azure Blob için desteklenen `location` biçimi tabanlı bir veri kümesi ayarlarında:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesi konumda öğesinin type özelliği ayarlanmalıdır **AzureBlobStorageLocation**. | Evet      |
| container  | Blob kapsayıcısı.                                          | Evet      |
| folderPath | Belirtilen kapsayıcıdaki yolu. Joker karakter filtresi klasörüne kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |
| fileName   | Belirtilen kapsayıcı + folderPath altında dosya adı. Joker karakter filtresi dosyalarını kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |

> [!NOTE]
>
> **AzureBlob** sonraki bölümde bahsedilen Parquet/metin biçimine sahip tür veri kümesi olarak desteklenen hala-için geriye dönük uyumluluk, ancak arama/kopyalama/GetMetadata etkinliği eşleme veri akışı ile çalışmaz. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Diğer biçim veri kümesi

ORC/Avro/JSON/ikili biçimde Blob Depolama ve veri kopyalamak için dataset öğesinin type özelliği ayarlamak **AzureBlob**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **AzureBlob**. |Evet |
| folderPath | Kapsayıcı ve blob depolama alanında bir klasör yolu. <br/><br/>Joker karakter filtresi için kapsayıcı adı hariç yolu desteklenir. Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br/><br/>Örnek: myblobcontainer/myblobfolder/daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |GetMetadata etkinliği için Hayır kopyalama/arama etkinliği için Evet |
| fileName | **Adı veya joker karakter filtresi** altında belirtilen "folderPath" inputblobpath için. Bu özellik için bir değer belirtmezseniz, klasördeki tüm blobları için veri kümesini işaret eder. <br/><br/>Filtre için joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek dosya adı varsa, kaçış için.<br/><br/>Dosya adı değil belirtildiği zaman için bir çıktı veri kümesi ve **preserveHierarchy** belirtilmediyse etkinliği havuz kopyalama etkinliği, blob adı şu deseni ile otomatik olarak oluşturur: "*Veri. [etkinlik] kimliği GUID çalıştırın. [GUID, FlattenHierarchy]. [biçim] yapılandırılmışsa. [yapılandırdıysanız sıkıştırma]* ", örneğin "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; Tablo adı yerine sorgu kullanarak tablo kaynağından kopyalarsanız, adı desendir " *[tablo adı]. [ Biçim]. [yapılandırdıysanız sıkıştırma]* ", örneğin "MyTable.csv". |Hayır |
| modifiedDatetimeStart | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre devasa miktarda bir dosyaları dosya istediğinizde, bu ayarı etkinleştirerek etkilenecek dikkat edin. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre devasa miktarda bir dosyaları dosya istediğinizde, bu ayarı etkinleştirerek etkilenecek dikkat edin. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| format | (İkili kopya) depoları arasında dosya tabanlı olduğu gibi dosyaları kopyalamak girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Tüm BLOB'ları bir klasör altındaki kopyalamak belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip bir blobun kopyalamak belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir klasör altındaki BLOB'ları kümesini kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** joker karakter Filtresi ile. 

**Örnek:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Blob Depolama kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="blob-storage-as-a-source-type"></a>BLOB Depolama kaynak türü

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- Kopyalama gibi diğer biçimlerinden **ORC/Avro/JSON/ikili biçimi**, başvurmak [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

Parquet veya sınırlandırılmış metin biçimi Blob depolamadan veri kopyalamak için başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Aşağıdaki özellikler altında Azure Blob için desteklenen `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureBlobStorageReadSetting**. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| wildcardFolderPath       | Veri kümesi filtresi kaynak klasörleri için yapılandırılan belirli kapsayıcısı altında joker karakterlerle klasör yolu. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br>Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Hayır                                            |
| wildcardFileName         | Belirtilen kapsayıcı + folderPath/wildcardFolderPath filtre kaynak dosyalarının altında joker karakterler içeren dosya adı. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için.  Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Yanıt Evet ise `fileName` kümesinde belirtilmedi |
| modifiedDatetimeStart    | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır                                            |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **BlobSource** sonraki bölümde bahsedilen türü kopyalama etkinliği kaynağı olarak desteklenen hala-geriye dönük uyumluluğu içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobStorageReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Başka bir biçim kaynağı

ORC/Avro/JSON/ikili biçimde Blob depolamadan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **BlobSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **BlobSource**. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur.<br/>İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
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
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Bir havuz türü olarak BLOB Depolama alanı

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi havuz](#parquet-and-delimited-text-format-sink) bölümü.
- Kopyalama gibi diğer biçimlere **ORC/Avro/JSON/ikili biçimi**, başvurmak [diğer biçim havuz](#other-format-sink) bölümü.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet ve sınırlandırılmış metin biçimi havuz

Parquet sınırlandırılmış metin biçimi de Blob depolama alanına veri kopyalamak için başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği havuzu ve desteklenen ayarlar. Aşağıdaki özellikler altında Azure Blob için desteklenen `storeSettings` biçimi tabanlı kopyalama havuz ayarları:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureBlobStorageWriteSetting**. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya ya da blob adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır       |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **BlobSink** sonraki bölümde bahsedilen türü kopyalama etkinliği havuz olarak desteklenen hala-geriye dönük uyumluluğu içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobStorageWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Diğer biçim havuz

Blob depolama alanına veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **BlobSink**. Aşağıdaki özellikler desteklenir **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **BlobSink**. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya ya da blob adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, sonuçta elde edilen davranışını klasör yolu ve dosya adı joker filtrelerle açıklanmaktadır.

| folderPath | fileName | recursive | Kaynak klasör yapısını ve filtre sonucunu (dosyalar **kalın** alınır)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (boş, varsayılan kullanın) | false | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | (boş, varsayılan kullanın) | true | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | `*.csv` | false | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | `*.csv` | true | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Özyinelemeli ve copyBehavior örnekler

Bu bölümde, elde edilen davranışını özyinelemeli ve copyBehavior değer farklı birleşimleri kopyalama işlemi açıklanmaktadır.

| recursive | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1, kaynağı olarak aynı yapıya sahip oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + File5 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |

## <a name="mapping-data-flow-properties"></a>Veri akışı özellikleri eşleme

Ayrıntıları öğrenin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı olarak.

## <a name="next-steps"></a>Sonraki adımlar

Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
