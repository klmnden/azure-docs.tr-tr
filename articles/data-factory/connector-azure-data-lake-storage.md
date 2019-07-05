---
title: Azure Data Lake depolama Gen2'ye gelen veya veri kopyalama kullanarak Data Factory | Microsoft Docs
description: Azure Data Factory kullanarak Azure Data Lake depolama Gen2'ye gelen ve giden veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 9f60c6258da77c0aaa99d16e178f4b3531ce90d9
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509242"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Azure Data Lake depolama Gen2'ye gelen veya veri kopyalama kullanarak Azure Data Factory

Azure Data Lake depolama Gen2 yerleşik büyük veri analizi için ayrılmış özellikleri kümesidir [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md). Her iki dosya sistemi ve nesne depolama paradigmalarını kullanarak verilerinizi ile arabirim oluşturmak için kullanabilirsiniz.

Bu makalede Azure Data Lake depolama Gen2'ye ve veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Data Lake depolama Gen2 Bağlayıcısı için aşağıdaki etkinlikleri desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak veya havuz Matrisi](copy-activity-overview.md)
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)

Özellikle, bu bağlayıcıyı kullanarak şunları yapabilirsiniz:

- Azure kaynaklarını kimlik doğrulamaları için hesap anahtarı, hizmet sorumlusu veya yönetilen kimlik kullanarak verileri kopyalayın.
- Ayrıştırma veya dosyaları oluşturmak gibi dosyaları kopyalayın [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

>[!TIP]
>Hiyerarşik ad alanı etkinleştirirseniz, şu anda yoktur işlemlerinin Blob ve Data Lake depolama Gen2 API'lerini arasında hiçbir birlikte çalışabilirlik. Hata ulaşırsanız "hata kodu FilesystemNotFound =" "Belirtilen dosya yok" iletisi ile Data Lake depolama Gen2 API yerine Blob API aracılığıyla başka bir yerde oluşturulmuş belirtilen havuz dosya sistemi tarafından neden olur. Bu sorunu düzeltmek için yeni bir dosya sistemi ile bir Blob kapsayıcısı adı olarak mevcut olmayan bir ad belirtin. Daha sonra Data Factory otomatik olarak veri kopyalama sırasında o dosya sistemi oluşturur.

>[!NOTE]
>Etkinleştirirseniz **güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver** seçeneği Azure depolama Güvenlik Duvarı ayarları, Azure tümleştirme çalışma zamanı, Data Lake depolama 2. nesil bağlanmıyor ve Yasak hatası gösterir. Data Factory güvenilir bir Microsoft hizmeti olarak değerlendirildiğinden değil hata iletisi görüntülenir. Bunun yerine bağlanmak için şirket içinde barındırılan tümleştirme çalışma zamanını kullanın.

## <a name="get-started"></a>başlarken

>[!TIP]
>Data Lake depolama Gen2 bağlayıcıyı kullanmak üzere nasıl talimatları için bkz. [Azure Data Lake depolama 2. nesil ile veri yükleme](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, belirli bir Data Factory varlıkları için Data Lake depolama Gen2 tanımlamak için kullanılan özellikleri hakkında bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake depolama Gen2 Bağlayıcısı'nı aşağıdaki kimlik doğrulama türlerini destekler. Ayrıntılar için karşılık gelen bölümlere bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik](#managed-identity)

>[!NOTE]
>Data Lake depolama 2. nesil kaynak sanal ağ uç noktası ile yapılandırılmışsa, SQL veri ambarı'na veri yüklemek için PolyBase kullanarak, PolyBase gerektirdiği gibi yönetilen kimlik doğrulaması kullanmanız gerekir. Bkz: [yönetilen kimlik doğrulama](#managed-identity) daha fazla yapılandırma önkoşulları bölümü.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtarı kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Data Lake depolama Gen2 için uç nokta ile desenini `https://<accountname>.dfs.core.windows.net`. | Evet |
| accountKey | Data Lake depolama Gen2 hesap anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Data store özel bir ağda ise Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

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

Hizmet sorumlusu kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin.

1. Azure Active Directory (Azure AD) uygulama varlık adımları izleyerek elle kaydetmeyi [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusu uygun izni verin. Data Lake depolama Gen2 ' içinde izni birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin [dosyalar ve dizinler üzerinde erişim denetim listeleri](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)

    - **Kaynak olarak**: Depolama Gezgini'nde en az izni **yürütme** ile birlikte kaynak dosya sisteminden başlatma izni **okuma** kopyalanacak dosyaları izni. Alternatif olarak, erişim denetimi (IAM), verme en az **depolama Blob verileri okuyucu** rol.
    - **Havuz olarak**: Depolama Gezgini'nde en az izni **yürütme** ile birlikte havuz dosya sisteminden başlatma izni **yazma** havuz klasörüne izni. Alternatif olarak, erişim denetimi (IAM), verme en az **depolama Blob verileri katkıda bulunan** rol.

>[!NOTE]
>Listeye klasörleri, hesap düzeyine veya bağlantıyı sınamak için başlatma izni için izin verilen hizmet sorumlusunun ayarlamanız gerekir. **IAM "Depolama Blob verileri okuyucu" iznine sahip depolama hesabı**. Kullandığınızda, bu durum geçerlidir:
>- **Kopya veri aracını** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için. 
>Geliştirme sırasında hesap düzeyinde izni verme hakkında endişeleriniz test bağlantısı ve yolu belirtilen izni olan bir üst yol ardından gelen göz atmak için giriş atlayın. Hizmet sorumlusu kopyalanması uygun ile dosyalarını izni olduğu sürece, etkinlik works kopyalayın.

Bu özellikler için bağlı hizmet desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Data Lake depolama Gen2 için uç nokta ile desenini `https://<accountname>.dfs.core.windows.net`. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir `SecureString` Data Factory'de güvenle depolamak için. Veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| tek | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Data store özel bir ağda ise Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

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

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Data Lake depolama Gen2'ye kimlik doğrulama, kendi hizmet sorumlusunu kullanmaya benzer şekilde, bu yönetilen kimlik doğrudan kullanabilirsiniz. Verilere erişmek ve bunları kopyalamak için ya da, Data Lake depolama Gen2'ye atanan bu Fabrika sağlar.

Azure kaynak kimlik doğrulaması için yönetilen kimlikleri kullanmak için bu adımları izleyin.

1. [Data Factory yönetilen kimlik bilgileri almak](data-factory-service-identity.md#retrieve-managed-identity) değerini kopyalayarak **kimliği uygulama kimliği hizmeti** fabrikanızı birlikte oluşturulur.

2. Yönetilen kimlik uygun izni verin. Data Lake depolama Gen2 ' içinde izni birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin [erişim denetim listeleri dosyaların ve dizinlerin](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

    - **Kaynak olarak**: Depolama Gezgini'nde en az izni **yürütme** ile birlikte kaynak dosya sisteminden başlatma izni **okuma** kopyalanacak dosyaları izni. Alternatif olarak, erişim denetimi (IAM), verme en az **depolama Blob verileri okuyucu** rol.
    - **Havuz olarak**: Depolama Gezgini'nde en az izni **yürütme** ile birlikte havuz dosya sisteminden başlatma izni **yazma** havuz klasörüne izni. Alternatif olarak, erişim denetimi (IAM), verme en az **depolama Blob verileri katkıda bulunan** rol.

>[!NOTE]
>Listeye klasörleri, hesap düzeyine veya bağlantıyı sınamak için başlatma izni için izin verilen yönetilen kimlik ayarlamanız gerekir. **IAM "Depolama Blob verileri okuyucu" iznine sahip depolama hesabı**. Kullandığınızda, bu durum geçerlidir:
>- **Kopya veri aracını** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için. 
>Geliştirme sırasında hesap düzeyinde izni verme hakkında endişeleriniz test bağlantısı ve yolu belirtilen izni olan bir üst yol ardından gelen göz atmak için giriş atlayın. Hizmet sorumlusu kopyalanması uygun ile dosyalarını izni olduğu sürece, etkinlik works kopyalayın.

>[!IMPORTANT]
>Data Lake depolama Gen2'için yönetilen kimlik doğrulaması kullanırken, Data Lake depolama Gen2 ' verileri SQL veri ambarı'na yüklemek için PolyBase kullanın, adım 1 ve 2'de izlediğinizden emin olun [bu kılavuz](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) 1) SQL kaydetme Veritabanı sunucusu ile Azure Active Directory (Azure AD) ve 2) SQL veritabanı sunucunuza depolama Blob verileri katkıda bulunan rolü atayın; rest, Data Factory tarafından işlenir. Data Lake depolama Gen2 ', bir Azure sanal ağ uç noktası ile yapılandırılmışsa, kendisinden verileri yüklemek için PolyBase kullanmak için yönetilen kimlik doğrulama PolyBase gerektirdiği kullanmanız gerekir.

Bu özellikler için bağlı hizmet desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Data Lake depolama Gen2 için uç nokta ile desenini `https://<accountname>.dfs.core.windows.net`. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Data store özel bir ağda ise Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md).

- Parquet ve sınırlandırılmış metin biçimi için bkz: [Parquet ve sınırlandırılmış metin biçimi veri kümesi](#parquet-and-delimited-text-format-dataset) bölümü.
- ORC, Avro, JSON veya ikili biçimi gibi diğer biçimleri için bkz. [diğer biçim veri kümesi](#other-format-dataset) bölümü.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet ve sınırlandırılmış metin biçimi veri kümesi

Veri ve Data Lake depolama Gen2 parquet veya sınırlandırılmış metin biçimi kopyalamak için bkz: [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) biçimi tabanlı bir veri kümesi ve desteklenen ayarlar. Data Lake depolama Gen2 altında aşağıdaki özellikler desteklenir `location` biçimi tabanlı veri kümesi ayarlarında:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| türü       | Type özelliği altında `location` kümesinde ayarlanmalıdır **AzureBlobFSLocation**. | Evet      |
| fileSystem | Data Lake depolama Gen2'ye dosya sistemi adı.                              | Hayır       |
| folderPath | Belirtilen dosya sistemi altında bir klasör yolu. Joker karakter filtresi klasörlere kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |
| fileName   | Belirtilen dosya sistemi + folderPath altında dosya adı. Joker karakter filtresi dosyalarını kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |

> [!NOTE]
> **AzureBlobFSFile** aşağıdaki bölümde belirtildiği parquet veya metin biçimi türü kümesiyle kopyalama, arama veya GetMetadata etkinliği geriye dönük uyumluluk için olduğu gibi yine de desteklenir. Ancak eşleme veri akış özelliği ile çalışmaz. İleride bu yeni model kullanmanızı öneririz. Data Factory UI yazma bu yeni türleri oluşturur.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
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

Data Lake depolama Gen2 ORC, Avro, JSON veya ikili biçimi ve veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **AzureBlobFSFile**. |Evet |
| folderPath | Data Lake depolama Gen2'de bir klasörün yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir. Joker karakterlere izin verilir `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter). Kullanım `^` içinde bu kaçış karakteri gerçek klasör adında bir joker karakter veya kaçış. <br/><br/>Örnekler: dosya/klasör /. Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Hayır |
| fileName | Belirtilen "folderPath" altında dosya adı veya joker karakter Filtresi. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre, izin verilen joker karakterler olan `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` içinde bu kaçış karakteri gerçek dosya adında joker karakter veya kaçış.<br/><br/>Dosya adı değil belirtildiği zaman için bir çıktı veri kümesi ve **preserveHierarchy** belirtilmediyse etkinliği havuz kopyalama etkinliği, dosya adı şu deseni ile otomatik olarak oluşturur: "*Veri. [etkinlik] kimliği GUID çalıştırın. [GUID, FlattenHierarchy]. [biçim] yapılandırılmışsa. [yapılandırdıysanız sıkıştırma]* "Örneğin,"Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". Tablo adı yerine bir sorgu kullanarak bir tablo kaynağından kopyalarsanız, adı desendir " *[tablo adı]. [ Biçim]. [yapılandırdıysanız sıkıştırma]* "Örneğin,"MyTable.csv". |Hayır |
| modifiedDatetimeStart | Son değiştirme özniteliğini göre dosyaları filtreleme. Dosya, son değiştirilme zamanı zaman aralığı içinde ise seçili `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre dosyaları büyük miktarlarda dosya istediğinizde, bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler, yani hiçbir dosya öznitelik filtresi, veri kümesine uygulanır ve NULL olabilir. Zaman `modifiedDatetimeStart` bir datetime değerine sahip ancak `modifiedDatetimeEnd` null, daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri eşit seçilir. Zaman `modifiedDatetimeEnd` bir datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değerinden daha küçük dosyaların seçili anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Son değiştirme özniteliğini göre dosyaları filtreleme. Dosya, son değiştirilme zamanı zaman aralığı içinde ise seçili `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre dosyaları büyük miktarlarda dosya istediğinizde, bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler, yani hiçbir dosya öznitelik filtresi, veri kümesine uygulanır ve NULL olabilir. Zaman `modifiedDatetimeStart` bir datetime değerine sahip ancak `modifiedDatetimeEnd` null, daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri eşit seçilir. Zaman `modifiedDatetimeEnd` bir datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değerinden daha küçük dosyaların seçili anlamına gelir.| Hayır |
| format | (İkili kopya) depoları arasında dosya tabanlı olduğu gibi dosyaları kopyalamak girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [ORC biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** bir joker karakter Filtresi ile. 

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [kopyalama etkinliği yapılandırmaları](copy-activity-overview.md#configuration) ve [işlem hatları ve etkinlikler](concepts-pipelines-activities.md). Bu bölümde, Data Lake depolama 2. nesil kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake depolama Gen2'kaynak türü

- Parquet veya sınırlandırılmış metin biçimi kopyalamak için bkz: [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- ORC, Avro, JSON veya ikili biçimi gibi diğer biçimlere kopyalamak için bkz: [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

Parquet veya sınırlandırılmış metin biçiminde Data Lake depolama Gen2 ' veri kopyalamak için bkz: [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Data Lake depolama Gen2 altında aşağıdaki özellikler desteklenir `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureBlobFSReadSetting**. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli ayarlandığında true ve havuz bir dosya tabanlı depolama, bir boş klasör veya alt kopyaladığınız veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| wildcardFolderPath       | Veri kümesi filtresi kaynak klasörleri için yapılandırılan belirli dosya sistemi altında joker karakterlerle klasör yolu. <br>Joker karakterlere izin verilir `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter). Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br>Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Hayır                                            |
| wildcardFileName         | Belirtilen dosya sistemi + folderPath/wildcardFolderPath filtre kaynak dosyalarının altında joker karakterler içeren dosya adı. <br>Joker karakterlere izin verilir `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter). Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Yanıt Evet ise `fileName` kümesinde belirtilmemiş |
| modifiedDatetimeStart    | Son değiştirme özniteliğini göre dosyaları filtreleme. Dosya, son değiştirilme zamanı zaman aralığı içinde ise seçili `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikleri, veri kümesi için hiçbir dosya öznitelik filtresi uygulandığı anlamına gelir NULL olabilir. Zaman `modifiedDatetimeStart` bir datetime değerine sahip ancak `modifiedDatetimeEnd` null, son özniteliği değiştirilmiş dosyaları daha büyük olduğu anlamına gelir veya tarih/saat değerine eşit seçilir. Zaman `modifiedDatetimeEnd` bir datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değerinden daha küçük dosyaların seçili olduğundan anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır                                            |

> [!NOTE]
> Parquet veya sınırlandırılmış metin biçimi **AzureBlobFSSource** türü kopyalama etkinliği kaynağı aşağıdaki bölümde belirtildiği geriye dönük uyumluluk için olduğu gibi yine de desteklenir. İleride bu yeni model kullanmanızı öneririz. Data Factory UI yazma bu yeni türleri oluşturur.

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

Data Lake depolama Gen2 ORC, Avro, JSON veya ikili biçimi verileri kopyalamak için kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **AzureBlobFSSource**. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli ayarlandığında true ve havuz bir dosya tabanlı depolama, bir boş klasör veya alt kopyaladığınız veya havuz oluşturulur.<br/>İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
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

- Belirli bir parquet veya sınırlandırılmış metin biçimi kopyalamak için bkz: [Parquet ve sınırlandırılmış metin biçimi havuz](#parquet-and-delimited-text-format-sink) bölümü.
- ORC, Avro, JSON veya ikili biçimi gibi diğer biçimlere kopyalamak için bkz: [diğer biçim havuz](#other-format-sink) bölümü.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet ve sınırlandırılmış metin biçimi havuz

Parquet veya sınırlandırılmış metin biçimi için Data Lake depolama Gen2 veri kopyalamak için bkz: [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) biçimi tabanlı kopyalama etkinliği havuzu ve desteklenen ayarlar. Data Lake depolama Gen2 altında aşağıdaki özellikler desteklenir `storeSettings` biçimi tabanlı kopyalama havuz ayarları:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureBlobFSWriteSetting**. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır       |

> [!NOTE]
> Parquet veya sınırlandırılmış metin biçimi **AzureBlobFSSink** aşağıdaki bölümde belirtildiği türü kopyalama etkinliği havuz geriye dönük uyumluluk için olduğu gibi yine de desteklenir. İleride bu yeni model kullanmanızı öneririz. Data Factory UI yazma bu yeni türleri oluşturur.

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

Data Lake depolama Gen2 ORC, Avro, JSON veya ikili biçimi veri kopyalamak için aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **AzureBlobFSSink**. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır |
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
| `Folder*` | (Boş, varsayılan kullanın) | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (Boş, varsayılan kullanın) | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Özyinelemeli ve copyBehavior örnekler

Bu bölümde, elde edilen davranışını özyinelemeli ve copyBehavior değer farklı birleşimleri kopyalama işlemi açıklanmaktadır.

| recursive | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef kaynak olarak aynı yapıya sahip Klasör1 oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + File5 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |

## <a name="preserve-acls-from-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ACL'sinden koru

>[!TIP]
>Gen2'ye Azure Data Lake depolama Gen1 verileri genel kopyalamak için bkz: [veri kopyalama Azure Data Lake depolama Gen1 Azure Data Factory ile Gen2'ye](load-azure-data-lake-storage-gen2-from-gen1.md) bir gözden geçirme ve en iyi uygulamalar için.

2\. nesil için Azure Data Lake depolama Gen1 dosyaları kopyaladığınızda, POSIX erişim denetim listeleri (ACL'ler) verileriyle birlikte korumak seçebilirsiniz. Erişim denetimi hakkında daha fazla bilgi için bkz. [erişim denetimi, Azure Data Lake depolama Gen1](../data-lake-store/data-lake-store-access-control.md) ve [Azure Data Lake depolama Gen2'deki erişim denetimi](../storage/blobs/data-lake-storage-access-control.md).

Azure Data Factory kopyalama etkinliği'ni kullanarak aşağıdaki türlerde ACL'leri korunabilir. Bir veya daha fazla türleri seçebilirsiniz:

- **ACL**: Kopyala ve dosyalar ve dizinler POSIX erişim denetim listelerini koruyabilirsiniz. Bu tam mevcut ACL'leri havuz kaynaktan kopyalar. 
- **Sahibi**: Kopyala ve dosyalar ve dizinler sahibi olan kullanıcıyı koruyabilirsiniz. Data Lake depolama Gen2 havuz için süper kullanıcı erişimi gereklidir.
- **Grup**: Kopyala ve dosya ve dizinleri sahip olan Grup koruyabilirsiniz. Data Lake depolama Gen2'ye veya sahip olan kullanıcı (sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi ise) havuz için süper kullanıcı erişimi gereklidir.

Bir klasöre kopyalamak için belirtirseniz, Data Factory, belirtilen klasör ve dosyaları ve dizinleri, altında ACL'leri, çoğaltır `recursive` ayarlanır true. Tek bir dosyadan kopyalamak için belirtirseniz bu dosyada ACL'leri kopyalanır.

>[!IMPORTANT]
>ACL'ler korumak seçtiğinizde, yüksek Data Factory'nin havuzunuzu Data Lake depolama Gen2 hesabına karşı çalıştırmak için yeterli izinleri vermeniz emin olun. Örneğin, hesap anahtar kimlik doğrulamasını kullanmak veya hizmet sorumlusu veya yönetilen kimliği için depolama Blob verileri sahip rolü atayabilirsiniz.

Kaynak Data Lake depolama Gen1 ikili kopyalama seçeneği veya ikili biçimi ve havuz Data Lake depolama Gen2 ikili kopyalama seçeneği veya bir ikili biçimi ile yapılandırdığınızda, bulabilirsiniz **korumak** seçeneğini **kopyalama Veri aracı ayarları** sayfası veya **kopyalama etkinliği** > **ayarları** etkinlik yazma için sekmesinde.

![Data Lake depolama Gen1 Gen2 Koru ACL için](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

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

## <a name="mapping-data-flow-properties"></a>Eşleme veri akışı özellikleri

Daha fazla bilgi edinin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı özelliği.

## <a name="next-steps"></a>Sonraki adımlar

Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
