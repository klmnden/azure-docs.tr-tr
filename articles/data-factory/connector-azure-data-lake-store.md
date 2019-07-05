---
title: Azure Data Lake depolama Gen1 gelen veya veri kopyalama kullanarak Data Factory | Microsoft Docs
description: Data Factory kullanarak desteklenen havuz depolarına, Data Lake Store veya Azure Data Lake Store için desteklenen kaynak veri depolarından veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: df88c3e2e07165182c917eaf30a5f37451fbd073
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509587"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-using-azure-data-factory"></a>Azure Data Lake depolama Gen1 gelen veya veri kopyalama kullanarak Azure Data Factory
> [!div class="op_single_selector" title1="Azure Data Factory, kullanmakta olduğunuz sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-datalake-connector.md)
> * [Geçerli sürüm](connector-azure-data-lake-store.md)

Bu makalede Azure Data Lake depolama Gen1 ve veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Data Lake depolama Gen1 Bağlayıcısı için aşağıdaki etkinlikleri desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak veya havuz Matrisi](copy-activity-overview.md)
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)

Özellikle, bu bağlayıcıyı kullanarak şunları yapabilirsiniz:

- Kimlik doğrulama aşağıdaki yöntemlerden birini kullanarak dosyaları kopyalayın: hizmet sorumlusu veya yönetilen kimliklerini Azure kaynakları için.
- Ayrıştırma veya dosyaları oluşturmak gibi dosyaları kopyalayın [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Şirket içinde barındırılan Integration runtime'ı kullanarak verileri kopyalama, giden trafiğe izin vermek için Kurumsal güvenlik duvarı yapılandırma `<ADLS account name>.azuredatalakestore.net` ve `login.microsoftonline.com/<tenant>/oauth2/token` bağlantı noktası 443 üzerinden. İkincisi Azure güvenlik belirteci, tümleştirme çalışma zamanı erişim belirteci almak için iletişim kurması için gereken hizmetidir.

## <a name="get-started"></a>başlarken

> [!TIP]
> Azure Data Lake Store bağlayıcıyı kullanmak üzere nasıl talimatları için bkz. [Azure Data Lake Store ile veri yükleme](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, belirli bir Data Factory varlıkları Azure Data Lake Store için tanımlamak için kullanılan özellikleri hakkında bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake Store bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | `type` Özelliği ayarlanmalıdır **birlikte AzureDataLakeStore**. | Evet |
| dataLakeStoreUri | Azure Data Lake Store hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini alır: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store hesabına ait olduğu Azure abonelik kimliği. | Havuz için gerekli |
| resourceGroupName | Data Lake Store hesabına ait olduğu Azure kaynak grubu adı. | Havuz için gerekli |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Data store özel bir ağda yer alıyorsa, Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

### <a name="use-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması kullanma

Hizmet sorumlusu kimlik doğrulaması kullanmak için bir uygulama varlığı Azure Active Directory'ye kaydetmeniz ve Data Lake Store erişimi verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı
- Kiracı Kimliği

>[!IMPORTANT]
> Hizmet sorumlusu uygun izni Data Lake Store içinde verin:
>- **Kaynak olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **okuma + yürütme** izni listeler ve klasör ve alt klasörlerde dosyaları kopyalayın. Ya da size verebilir **okuma** tek bir dosyayı kopyalama izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Hesap düzeyinde erişim denetimi (IAM) gereksinimi yoktur.
>- **Havuz olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **yazma + yürütme** klasörde alt öğeler oluşturmak için izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de bulutta) verme IAM içinde en az **okuyucu** Data Factory'ye Data Lake Store için bölge algılamak izin vermek için rol. Bu IAM rol açıkça önlemek istediğiniz [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Data Lake Store konumu ile. Data Lake Store Batı Avrupa'ise, örneğin, bir Azure tümleştirme çalışma zamanı "Batı Avrupa için" ayarladığınız konumu ile oluşturma Data Lake Store bağlı hizmetinde aşağıdaki örnekte gösterildiği gibi bunları ilişkilendirin.

>[!NOTE]
>Listeye klasör, kökten başlayarak izni için izin verilen hizmet sorumlusunun ayarlamalısınız **"Yürütme" iznine sahip kök düzeyinde**. Kullandığınızda, bu durum geçerlidir:
>- **Kopya veri aracını** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için.
>Geliştirme sırasında kök düzeyinde izni verme hakkında endişeleriniz test bağlantısı ve yolu belirtilen izni paraent yoluyla ardından gelen göz atmak için giriş atlayın. Hizmet sorumlusu kopyalanması uygun ile dosyalarını izni olduğu sürece, etkinlik works kopyalayın.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir `SecureString` Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| tenant | Kiracı bilgileri gibi etki alanı adı belirtin veya Kiracı kimliği, uygulamanızın bulunduğu. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet |

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Data Lake Store kimlik doğrulaması, kendi hizmet sorumlusunu kullanmaya benzer şekilde, bu yönetilen kimlik doğrudan kullanabilirsiniz. Belirlenen Bu fabrika, verilere erişmek ve bunları kopyalamak için veya Data Lake Store sağlar.

Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak için:

1. [Data factory yönetilen kimlik bilgilerini alma](data-factory-service-identity.md#retrieve-managed-identity) "hizmet kimliği uygulama fabrikanızı birlikte oluşturulan kimliği" değerini kopyalayarak.
2. Yönetilen kimlik için Data Lake Store, bu notlar aşağıdaki hizmet sorumlusu için yaptığınız şekilde erişim.

>[!IMPORTANT]
> Data Lake Store data factory yönetilen kimlik uygun izin verme emin olun:
>- **Kaynak olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **okuma + yürütme** izni listeler ve klasör ve alt klasörlerde dosyaları kopyalayın. Ya da size verebilir **okuma** tek bir dosyayı kopyalama izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Hesap düzeyinde erişim denetimi (IAM) gereksinimi yoktur.
>- **Havuz olarak**: İçinde **Veri Gezgini** > **erişim**, en az izni **yazma + yürütme** klasörde alt öğeler oluşturmak için izni. Eklemek seçebileceğiniz **bu klasör ve tüm alt öğeleri** için özyinelemeli olarak ekleyin **erişim izni ve varsayılan izin girdisi**. Kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de bulutta) verme IAM içinde en az **okuyucu** Data Factory'ye Data Lake Store için bölge algılamak izin vermek için rol. Bu IAM rol açıkça önlemek istediğiniz [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md#create-azure-ir) Data Lake Store konumu ile. Data Lake Store bağlı hizmetinde aşağıdaki örnekte gösterildiği gibi bunları ilişkilendirin.

>[!NOTE]
>Listeye klasör, kökten başlayarak izni için izin verilen yönetilen kimlik ayarlamalısınız **"Yürütme" iznine sahip kök düzeyinde**. Kullandığınızda, bu durum geçerlidir:
>- **Kopya veri aracını** Yazar kopyalama işlem hattını için.
>- **Data Factory kullanıcı arabirimini** bağlantı ve geliştirme sırasında klasörleri gezinme test etmek için.
>Geliştirme sırasında kök düzeyinde izni verme hakkında endişeleriniz test bağlantısı ve yolu belirtilen izni olan bir üst yol ardından gelen göz atmak için giriş atlayın. Hizmet sorumlusu kopyalanması uygun ile dosyalarını izni olduğu sürece, etkinlik works kopyalayın.

Azure Data Factory'de bağlı hizmet Data Lake Store genel bilgilerin yanı sıra herhangi bir özelliği belirtmeniz gerekmez.

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
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

- Parquet ve sınırlandırılmış metin biçimi için bkz: [Parquet ve sınırlandırılmış metin biçimi veri kümesi](#parquet-and-delimited-text-format-dataset) bölümü.
- ORC, Avro, JSON veya ikili biçimi gibi diğer biçimleri için bkz. [diğer biçim veri kümesi](#other-format-dataset) bölümü.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet ve sınırlandırılmış metin biçimi veri kümesi

Veri ve Azure Data Lake Store Gen1 parquet veya sınırlandırılmış metin biçimi kopyalamak için bkz: [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) biçimi tabanlı bir veri kümesi ve desteklenen ayarlar. Aşağıdaki özellikler için Azure Data Lake Store Gen1 altında desteklenen `location` biçimi tabanlı veri kümesi ayarlarında:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| türü       | Type özelliği altında `location` kümesinde ayarlanmalıdır **AzureDataLakeStoreLocation**. | Evet      |
| folderPath | Bir klasörün yolu. Joker karakter filtresi klasörlere kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |
| fileName   | Verilen folderPath altında dosya adı. Joker karakter filtresi dosyalarını kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |

> [!NOTE]
>
> **AzureDataLakeStoreFile** aşağıdaki bölümde belirtildiği parquet veya metin biçimi türü kümesiyle GetMetadata etkinliği geriye dönük uyumluluk için kopyalama ve arama için olduğu gibi yine de desteklenir. Ancak eşleme veri akış özelliği ile çalışmaz. İleride bu yeni model kullanmanızı öneririz. Data Factory UI yazma bu yeni türleri oluşturur.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen1 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureDataLakeStoreLocation",
                "folderPath": "root/folder/subfolder"
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

Azure Data Lake Store Gen1 ORC, Avro, JSON veya ikili biçimi ve veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Dataset öğesinin type özelliği ayarlanmalıdır **AzureDataLakeStoreFile**. |Evet |
| folderPath | Data Lake Store içinde bir klasörün yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir. Joker karakterlere izin verilir `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter). Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br/><br/>Örneğin: rootfolder/alt /. Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Hayır |
| fileName | Belirtilen "folderPath" altında dosya adı veya joker karakter Filtresi. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre, izin verilen joker karakterler olan `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek dosya adı varsa, kaçış için.<br/><br/>Dosya adı değil belirtildiği zaman için bir çıktı veri kümesi ve **preserveHierarchy** belirtilmediyse etkinliği havuz kopyalama etkinliği, dosya adı şu deseni ile otomatik olarak oluşturur: "*Veri. [etkinlik] kimliği GUID çalıştırın. [GUID, FlattenHierarchy]. [biçim] yapılandırılmışsa. [yapılandırdıysanız sıkıştırma]* "Örneğin,"Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". Tablo adı yerine bir sorgu kullanarak bir tablo kaynaktan kopyalarsanız, adı desendir " *[tablo adı]. [ Biçim]. [yapılandırdıysanız sıkıştırma]* "Örneğin,"MyTable.csv". |Hayır |
| modifiedDatetimeStart | Son değiştirme özniteliğini göre dosyaları filtreleme. Dosya, son değiştirilme zamanı zaman aralığı içinde ise seçili `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre dosyaları büyük miktarlarda dosya istediğinizde, bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler, yani hiçbir dosya öznitelik filtresi, veri kümesine uygulanır ve NULL olabilir. Zaman `modifiedDatetimeStart` bir datetime değerine sahip ancak `modifiedDatetimeEnd` null, daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri eşit seçilir. Zaman `modifiedDatetimeEnd` bir datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değerinden daha küçük dosyaların seçili anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Son değiştirme özniteliğini göre dosyaları filtreleme. Dosya, son değiştirilme zamanı zaman aralığı içinde ise seçili `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre dosyaları büyük miktarlarda dosya istediğinizde, bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler, yani hiçbir dosya öznitelik filtresi, veri kümesine uygulanır ve NULL olabilir. Zaman `modifiedDatetimeStart` bir datetime değerine sahip ancak `modifiedDatetimeEnd` null, daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri eşit seçilir. Zaman `modifiedDatetimeEnd` bir datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değerinden daha küçük dosyaların seçili anlamına gelir.| Hayır |
| format | (İkili kopya) depoları arasında dosya tabanlı olduğu gibi dosyaları kopyalamak hem girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |


>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** bir joker karakter Filtresi ile. 

**Örnek:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). Bu bölümde, Azure Data Lake Store kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-store-as-source"></a>Kaynak olarak Azure Data Lake Store

- Parquet veya sınırlandırılmış metin biçimi kopyalamak için bkz: [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- ORC, Avro, JSON veya ikili biçimi gibi diğer biçimlere kopyalamak için bkz: [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

Parquet veya sınırlandırılmış metin biçimi, Azure Data Lake Store Gen1 verileri kopyalamak için bkz: [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Aşağıdaki özellikler için Azure Data Lake Store Gen1 altında desteklenen `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureDataLakeStoreReadSetting**. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli ayarlandığında true ve havuz bir dosya tabanlı depolama, bir boş klasör veya alt kopyaladığınız veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| wildcardFolderPath       | Kaynak klasörleri filtrelemek için joker karakter içeren klasör yolu. <br>Joker karakterlere izin verilir `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter). Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br>Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Hayır                                            |
| wildcardFileName         | Joker karakter filtresi kaynak dosyalarının belirli folderPath/wildcardFolderPath altında içeren dosya adı. <br>Joker karakterlere izin verilir `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter). Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Yanıt Evet ise `fileName` kümesinde belirtilmemiş |
| modifiedDatetimeStart    | Son değiştirme özniteliğini göre dosyaları filtreleme. Dosya, son değiştirilme zamanı zaman aralığı içinde ise seçili `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikler, yani hiçbir dosya öznitelik filtresi, veri kümesine uygulanır ve NULL olabilir. Zaman `modifiedDatetimeStart` bir datetime değerine sahip ancak `modifiedDatetimeEnd` null, daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri eşit seçilir. Zaman `modifiedDatetimeEnd` bir datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değerinden daha küçük dosyaların seçili anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır                                            |

> [!NOTE]
> Parquet veya sınırlandırılmış metin biçimi **kümesinin kullanılması gerekir** türü kopyalama etkinliği kaynağı aşağıdaki bölümde belirtildiği geriye dönük uyumluluk için olduğu gibi yine de desteklenir. İleride bu yeni model kullanmanızı öneririz. Data Factory UI yazma bu yeni türleri oluşturur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
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
                    "type": "AzureDataLakeStoreReadSetting",
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

Azure Data Lake Store Gen1 ORC, Avro, JSON veya ikili biçimi verileri kopyalamak için kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | `type` Kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **kümesinin kullanılması gerekir**. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Zaman `recursive` true olarak ve havuz boş bir klasörü bir dosya tabanlı depolama veya alt değil ya da kopyalanır havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen1 input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Havuz olarak Azure Data Lake Store

- Parquet ve sınırlandırılmış metin biçimi kopyalamak için bkz: [Parquet ve sınırlandırılmış metin biçimi havuz](#parquet-and-delimited-text-format-sink) bölümü.
- ORC, Avro, JSON veya ikili biçimi gibi diğer biçimlere kopyalamak için bkz: [diğer biçim havuz](#other-format-sink) bölümü.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet ve sınırlandırılmış metin biçimi havuz

Parquet veya sınırlandırılmış metin biçimi için Azure Data Lake Store Gen1 veri kopyalamak için bkz: [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) biçimi tabanlı kopyalama etkinliği havuzu ve desteklenen ayarlar. Aşağıdaki özellikler için Azure Data Lake Store Gen1 altında desteklenen `storeSettings` biçimi tabanlı kopyalama havuz ayarları:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AzureDataLakeStoreWriteSetting**. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır       |

> [!NOTE]
> Parquet veya sınırlandırılmış metin biçimi **AzureDataLakeStoreSink** aşağıdaki bölümde belirtildiği türü kopyalama etkinliği havuz geriye dönük uyumluluk için olduğu gibi yine de desteklenir. İleride bu yeni model kullanmanızı öneririz. Data Factory UI yazma bu yeni türleri oluşturur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
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
                    "type": "AzureDataLakeStoreWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Diğer biçim havuz

Azure Data Lake Store Gen1 ORC, Avro, JSON veya ikili biçimi veri kopyalamak için aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | `type` Kopyalama etkinliği havuz özelliği ayarlanmalıdır **AzureDataLakeStoreSink**. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><b>-FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörü içinde ilk düzeyi var. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, dosya otomatik olarak oluşturulan addır. | Hayır |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen1 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
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

### <a name="examples-of-behavior-of-the-copy-operation"></a>Kopyalama işlemi davranışını örnekleri

Bu bölümde farklı birleşimlerini kopyalama işlemi elde edilen davranışını açıklar `recursive` ve `copyBehavior` değerleri.

| recursive | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef kaynak olarak aynı yapıya sahip Klasör1 oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + File5 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Olmayan Subfolder1 dosya3, File4 ve File5 teslim alındı. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Olmayan Subfolder1 dosya3, File4 ve File5 teslim alındı. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. fıle1'de otomatik olarak oluşturulan adı<br/><br/>Olmayan Subfolder1 dosya3, File4 ve File5 teslim alındı. |

## <a name="preserve-acls-to-data-lake-storage-gen2"></a>Data Lake depolama Gen2'ye ACL'leri koru

Çoğaltmak istiyorsanız Data Lake depolama 2. nesil için Data Lake depolama Gen1 ' yükseltme yaptığınızda erişim denetim listeleri (ACL'ler) veri dosyaları ile birlikte, bkz: [Data Lake depolama Gen1 korumak ACL'sinden](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1).

## <a name="mapping-data-flow-properties"></a>Eşleme veri akışı özellikleri

Daha fazla bilgi edinin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı özelliği.

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
