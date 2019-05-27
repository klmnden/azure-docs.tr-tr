---
title: Data Factory kullanarak Azure Data Lake depolama Gen2'ye gelen veya veri kopyalama | Microsoft Docs
description: Azure Data Lake depolama Gen2'ye gelen ve giden veri kopyalama hakkında bilgi edinin Azure Data Factory kullanarak.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jingwang
ms.openlocfilehash: c0591a7850516a419cb59045754cc4eb02979dfd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66122592"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Azure Data Lake depolama Gen2'ye gelen veya veri kopyalama kullanarak Azure Data Factory

Azure Data Lake depolama Gen2'ye (ADLS Gen2), bir dizi yerleşik, büyük veri analizi için ayrılmış özellikleri [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md). Her iki dosya sistemi ve nesne depolama paradigmalarını kullanarak verilerinizi ile arabirim oluşturmasını sağlar.

Bu makalede Azure Data Lake depolama Gen2'ye ve veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Data Lake depolama Gen2 Bağlayıcısı için aşağıdaki etkinlikleri desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak/havuz Matrisi](copy-activity-overview.md)
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)

Özellikle, bu bağlayıcıyı destekler:

- Azure kaynaklarını kimlik doğrulamaları için hesap anahtarı, hizmet sorumlusu veya yönetilen kimlik kullanarak veri kopyalama.
- Dosyaları olarak kopyalama-ayrıştırma veya ile dosya oluşturma [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

>[!TIP]
>Hiyerarşik ad alanı etkinleştirirseniz, şu anda yoktur arasında Blob ve ADLS Gen2 API işlemlerinin hiçbir birlikte çalışabilirlik. Hatayı isabet durumunda "hata kodu FilesystemNotFound =" ayrıntılı bir ileti ile "Belirtilen dosya yok.", belirtilen havuz kaynaklanır dosya sistemi oluşturulduğu ADLS Gen2 API yerine Blob API aracılığıyla başka bir yerde. Bu sorunu düzeltmek için lütfen bir Blob kapsayıcısı adı olarak var olmayan bir ada sahip yeni bir dosya sistemi belirtin ve ADF otomatik olarak veri kopyalama sırasında o dosya sistemi oluşturur.

>[!NOTE]
>Etkinleştirirse _"İzin ver güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin"_ Azure depolama Güvenlik Duvarı ayarları, Azure Integration Runtime'ı kullanarak Data Lake depolama 2. nesil bağlanma seçeneği olarak ADF Yasak hatası ile başarısız olur Güvenilen Microsoft hizmet olarak kabul edilmediği. Lütfen bunun yerine aracılığıyla bağlanma gibi şirket içinde barındırılan Integration Runtime'ı kullanın.

## <a name="get-started"></a>başlarken

>[!TIP]
>Data Lake depolama Gen2 Bağlayıcısı'nı kullanarak bir kılavuz için bkz. [Azure Data Lake depolama 2. nesil ile veri yükleme](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için Data Lake depolama Gen2 tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake depolama Gen2 Bağlayıcısı aşağıdaki kimlik doğrulama türlerini desteklemek, ilgili ayrıntılar bölümüne bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik](#managed-identity)

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtarı kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Uç nokta için desenini ile Data Lake depolama Gen2'ye `https://<accountname>.dfs.core.windows.net`. | Evet |
| accountKey | Data Lake depolama Gen2'ye hizmet hesabı anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağdaysa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. Azure Active Directory (Azure AD) uygulama varlık kaydınızı [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusu uygun izni verin.

    - **Kaynak olarak**, Depolama Gezgini içinde en az izni **okuma + yürütme** listelemek ve klasör ve alt klasörlerde dosyaları kopyalayın veya ver izni **okuma** tek bir dosyayı kopyalama izni. Alternatif olarak, erişim denetimi (IAM), en az izni **depolama Blob verileri okuyucu** rol.
    - **Havuz olarak**, Depolama Gezgini içinde en az izni **yazma + yürütme** klasörde alt öğeler oluşturmak için izni. Alternatif olarak, erişim denetimi (IAM), en az izni **depolama Blob verileri katkıda bulunan** rol.

>[!NOTE]
>Listeye klasörleri, hesap düzeyine veya bağlantıyı sınamak için başlatma izni için izin verilen hizmet sorumlusunun ayarlamanız gerekir. **IAM "Yürütme" iznine sahip depolama hesabı**. Kullandığınızda, bu durum geçerlidir:
>- **Kopyalama aracı veri** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için. 
>Hesap düzeyinde izin verme üzerinde kaygısı varsa, bağlantıyı test et ve Giriş yolu el ile yazma sırasında atlayabilirsiniz. Hizmet sorumlusu kopyalanacak uygun ile dosyalarını izni sürece kopyalama etkinliği çalışmaya devam eder.

Bu özellikleri bağlı hizmetinde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Uç nokta için desenini ile Data Lake depolama Gen2'ye `https://<accountname>.dfs.core.windows.net`. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| tek | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağdaysa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
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

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Bu yönetilen kimlik doğrudan kendi hizmet sorumlusunu kullanmaya benzer ADLS Gen2'ye kimlik doğrulaması için de kullanabilirsiniz. ADLS Gen2'ye / için veri erişimi ve kopyalama belirlenen Bu fabrika sağlar.

Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak için bu adımları izleyin:

1. [Data factory yönetilen kimlik bilgileri almak](data-factory-service-identity.md#retrieve-managed-identity) "Hizmet kimliği uygulama fabrikanızı birlikte oluşturulan kimliği" değerini kopyalayarak.

2. Yönetilen kimlik uygun izni verin. 

    - **Kaynak olarak**, Depolama Gezgini içinde en az izni **okuma + yürütme** listelemek ve klasör ve alt klasörlerde dosyaları kopyalayın veya ver izni **okuma** tek bir dosyayı kopyalama izni. Alternatif olarak, erişim denetimi (IAM), en az izni **depolama Blob verileri okuyucu** rol.
    - **Havuz olarak**, Depolama Gezgini içinde en az izni **yazma + yürütme** klasörde alt öğeler oluşturmak için izni. Alternatif olarak, erişim denetimi (IAM), en az izni **depolama Blob verileri katkıda bulunan** rol.

>[!NOTE]
>Listeye klasörleri, hesap düzeyine veya bağlantıyı sınamak için başlatma izni için izin verilen yönetilen kimlik ayarlamanız gerekir. **IAM "Yürütme" iznine sahip depolama hesabı**. Kullandığınızda, bu durum geçerlidir:
>- **Kopyalama aracı veri** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için. 
>Hesap düzeyinde izin verme üzerinde kaygısı varsa, bağlantıyı test et ve Giriş yolu el ile yazma sırasında atlayabilirsiniz. Yönetilen kimlik kopyalanacak uygun ile dosyalarını izni sürece kopyalama etkinliği çalışmaya devam eder.

>[!IMPORTANT]
>SQL DW, ADLS Gen2 kullanarak kimlik doğrulaması yönetildiğinde içine ADLS Gen2 verileri yüklenemiyor, doğru MSI ADLS Gen2'ye depolama kullanmak için SQL DW de yapılandırdığınızdan emin olun. PolyBase kullanırsanız, #3.b #1 adımlarını izleyin [bu kılavuz](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage). Kendisinden verileri yüklemek için PolyBase kullanılacak ADLS Gen2'ye VNet Hizmeti uç noktası ile yapılandırılmışsa, yönetilen kimlik doğrulaması kullanmanız gerekir.

Bu özellikleri bağlı hizmetinde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Uç nokta için desenini ile Data Lake depolama Gen2'ye `https://<accountname>.dfs.core.windows.net`. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağdaysa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
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

ADLS 2. nesil'deki ve veri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı bir veri kümesini ve desteklenen ayarlar. Aşağıdaki özellikler altında ADLS Gen2 için desteklenen `location` biçimi tabanlı bir veri kümesi ayarlarında:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Type özelliği altında `location` kümesinde ayarlanmalıdır **AzureBlobFSLocation**. | Evet      |
| fileSystem | ADLS Gen2'ye dosya sistemi adı.                              | Hayır       |
| folderPath | Belirtilen dosya sistemi altında klasör yolu. Joker karakter filtresi klasörüne kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |
| fileName   | Belirtilen dosya sistemi + folderPath altında dosya adı. Joker karakter filtresi dosyalarını kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |

> [!NOTE]
> **AzureBlobFSFile** sonraki bölümde bahsedilen Parquet/metin biçimine sahip tür veri kümesi olarak desteklenen hala-için geriye dönük uyumluluk, ancak arama/kopyalama/GetMetadata etkinliği eşleme veri akışı ile çalışmaz. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
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

ADLS 2. nesil'deki ve veri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **AzureBlobFSFile**. |Evet |
| folderPath | Data Lake depolama Gen2'ye klasöründe yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir, joker karakterlere izin şunlardır: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanmak `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br/><br/>Örnekler: dosya/klasör/daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Hayır |
| fileName | **Adı veya joker karakter filtresi** belirtilen "folderPath" altında dosyaları için. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek dosya adı varsa, kaçış için.<br/><br/>Dosya adı değil belirtildiği zaman için bir çıktı veri kümesi ve **preserveHierarchy** belirtilmediyse etkinliği havuz kopyalama etkinliği, dosya adı şu deseni ile otomatik olarak oluşturur: "*Veri. [etkinlik] kimliği GUID çalıştırın. [GUID, FlattenHierarchy]. [biçim] yapılandırılmışsa. [yapılandırdıysanız sıkıştırma]* ", örneğin "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; Tablo adı yerine sorgu kullanarak tablo kaynağından kopyalarsanız, adı desendir " *[tablo adı]. [ Biçim]. [yapılandırdıysanız sıkıştırma]* ", örneğin "MyTable.csv". |Hayır |
| modifiedDatetimeStart | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre devasa miktarda bir dosyaları dosya istediğinizde, bu ayarı etkinleştirerek etkilenecek dikkat edin. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre devasa miktarda bir dosyaları dosya istediğinizde, bu ayarı etkinleştirerek etkilenecek dikkat edin. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| format | (İkili kopya) depoları arasında dosya tabanlı olduğu gibi dosyaları kopyalamak girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** joker karakter Filtresi ile. 

**Örnek:**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz. [etkinlik yapılandırmalarına kopyalama](copy-activity-overview.md#configuration) ve [işlem hatları ve etkinlikler](concepts-pipelines-activities.md) makalesi. Bu bölümde, Data Lake depolama 2. nesil kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake depolama 2. nesil bir kaynak türü

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- Kopyalama gibi diğer biçimlerinden **ORC/Avro/JSON/ikili biçimi**, başvurmak [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

ADLS 2. nesil'deki verileri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Aşağıdaki özellikler altında ADLS Gen2 için desteklenen `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureBlobFSReadSetting**. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| wildcardFolderPath       | Veri kümesi filtresi kaynak klasörleri için yapılandırılan belirli dosya sistemi altında joker karakterlerle klasör yolu. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br>Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Hayır                                            |
| wildcardFileName         | Belirtilen dosya sistemi + folderPath/wildcardFolderPath filtre kaynak dosyalarının altında joker karakterler içeren dosya adı. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için.  Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Yanıt Evet ise `fileName` kümesinde belirtilmedi |
| modifiedDatetimeStart    | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır                                            |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **AzureBlobFSSource** sonraki bölümde bahsedilen türü kopyalama etkinliği kaynağı olarak desteklenen hala-geriye dönük uyumluluğu içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
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
                    "type": "AzureBlobFSReadSetting",
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

ADLS 2. nesil'deki verileri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler kopyalama etkinliğinde desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **AzureBlobFSSource**. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur.<br/>İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
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
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Bir havuz türü olarak Azure Data Lake depolama Gen2

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi havuz](#parquet-and-delimited-text-format-sink) bölümü.
- Kopyalama gibi diğer biçimlere **ORC/Avro/JSON/ikili biçimi**, başvurmak [diğer biçim havuz](#other-format-sink) bölümü.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet ve sınırlandırılmış metin biçimi havuz

ADLS 2. nesil'deki veri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) biçimi tabanlı kopyalama etkinliği havuz makale ve desteklenen ayarlar. Aşağıdaki özellikler altında ADLS Gen2 için desteklenen `storeSettings` biçimi tabanlı kopyalama havuz ayarları:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureBlobFSWriteSetting**. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır       |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **AzureBlobFSSink** sonraki bölümde bahsedilen türü kopyalama etkinliği havuz olarak desteklenen hala-geriye dönük uyumluluğu içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
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
                    "type": "AzureBlobFSWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Diğer biçim havuz

ADLS 2. nesil'deki veri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **AzureBlobFSSink**. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
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
| `Folder*` | (boş, varsayılan kullanın) | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (boş, varsayılan kullanın) | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

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

## <a name="preserve-acls-from-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ACL'sinden koru

>[!TIP]
>Kopyalama için genel olarak, Azure Data Lake depolama Gen1 Gen2 içine verileri görmek [veri kopyalama Azure Data Lake depolama Gen1 Azure Data Factory ile Gen2'ye](load-azure-data-lake-storage-gen2-from-gen1.md) gözden geçirme ve en iyi yöntemler.

Kopyalama Gen2 için Azure Data Lake Storage (ADLS) Gen1 dosyaları, POSIX erişim denetim listeleri (ACL'ler) verileriyle birlikte korumak seçebilirsiniz. Ayrıntılı erişim denetimi için başvurmak [erişim denetimi, Azure Data Lake depolama Gen1](../data-lake-store/data-lake-store-access-control.md) ve [Azure Data Lake depolama Gen2'deki erişim denetimi](../storage/blobs/data-lake-storage-access-control.md).

Azure Data Factory kopyalama etkinliğini kullanarak ACL'leri aşağıdaki türde korunabilir, bir veya daha fazla türlerini seçebilirsiniz:

- **ACL**: Kopyalama ve korumak **POSIX erişim denetim listeleri** dosyaları ve dizinleri. Bu tam mevcut ACL'leri havuz kaynağından kopyalayın. 
- **Sahibi**: Kopyalama ve korumak **sahip olan kullanıcı** dosya ve dizinleri. Havuz ADLS Gen2 süper kullanıcı erişimi gereklidir.
- **Grup**: Kopyalama ve korumak **sahip olan grup** dosya ve dizinleri. Süper kullanıcı erişimi (sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi ise) ADLS Gen2'ye veya sahip olan kullanıcı havuz için gereklidir.

Bir klasöre kopyalamak için belirtirseniz, Data Factory, verilen klasörün yanı sıra dosyaları ve dizinleri altındaki ACL'leri çoğaltır (varsa `recursive` ayarlanır true). Bu dosya üzerinde tek bir dosyadan, ACL'ler kopyalamak için belirtirseniz kopyalanır.

>[!IMPORTANT]
>ACL'ler korumak seçtiğinizde, yüksek havuzunuzu Gen2 ADLS hesabına karşı çalışılacak ADF için yeterli izinleri vermeniz emin olun. Örneğin, hesap anahtar kimlik doğrulamasını kullanmak veya hizmet sorumlusu ve yönetilen kimliği için depolama Blob verileri sahip rolü atayabilirsiniz.

Kaynak ADLS Gen1 ikili kopyalama seçeneği/ikili biçimi ve havuz ADLS Gen2'ye ikili kopyalama seçeneği/ikili biçim ile yapılandırdığınızda, bulabilirsiniz **korumak** seçeneğini **kopyalama veri Aracı Ayarları sayfasını** veya içinde **kopyalama etkinliği -> Ayarlar** etkinlik yazma için sekmesinde.

![Gen2'ye ADLS Gen1 ACL koru](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

İşte bir örnek JSON yapılandırma (bkz `preserve`): 

```json
"activities":[
    {
        "name": "CopyFromGen1ToGen2",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            },
            "preserve": [
                "ACL",
                "Owner",
                "Group"
            ]
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen1 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="mapping-data-flow-properties"></a>Veri akışı özellikleri eşleme

Ayrıntıları öğrenin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı olarak.

## <a name="next-steps"></a>Sonraki adımlar

Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
