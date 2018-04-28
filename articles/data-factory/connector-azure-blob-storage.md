---
title: Veri Fabrikası kullanarak veri veya Azure Blob depolama biriminden kopyalayın | Microsoft Docs
description: Veri Fabrikası kullanarak Azure Blob Depolama için desteklenen kaynak veri depolarını veya desteklenen havuz veri depolarına, Blob Depolama veri kopyalamak öğrenin.
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 04/27/2018
ms.author: jingwang
ms.openlocfilehash: 1c214dc34361bea49ad00cb5dfab71a7e6855996
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri veya Azure Blob depolama biriminden kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel kullanıma sunuldu](v1/data-factory-azure-blob-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-blob-storage.md)

Bu makalede kopya etkinliği Azure Data Factory'de ve Azure Blob depolama biriminden veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, veri fabrikası 1 sürümünü kullanıyorsanız bkz [Blob Depolama Bağlayıcısı sürüm 1](v1/data-factory-azure-blob-connector.md).


## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen kaynak veri deposundan verileri Blob depolama alanına kopyalayabilirsiniz. Ayrıca, verileri Blob depolama alanından herhangi desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md) tablo.

Özellikle, bu Blob Depolama bağlayıcı destekler:

- BLOB'ları Azure genel amaçlı depolama hesapları gelen ve sık erişimli cool blob depolama kopyalanıyor. 
- Hesap anahtarı ve hizmet kullanarak BLOB kopyalama erişim imzası kimlik doğrulamaları paylaşılan.
- Blok, kopyalama bloblarından eklemek ya da sayfa blobları ve verileri yalnızca kopyalama blok. Sayfa blobları tarafından yedeklenen olduğundan azure Premium depolama havuzu olarak desteklenmiyor.
- BLOB veya ayrıştırma veya oluşturma gibi BLOB kopyalama [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Blob depolama alanına tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

### <a name="use-an-account-key"></a>Hesap anahtarı kullanın

Depolama bağlantılı hizmeti hesap anahtarı kullanarak oluşturabilirsiniz. Genel erişim ile veri fabrikası depolama sağlar. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **AzureStorage**. |Evet |
| connectionString | ConnectionString özelliği için depolama alanına bağlanmak için gereken bilgileri belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda ise) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
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

### <a name="use-service-shared-access-signature-authentication"></a>Kullanım hizmeti paylaşılan erişim imzası kimlik doğrulaması

Depolama bağlantılı hizmeti bir paylaşılan erişim imzası kullanarak da oluşturabilirsiniz. Veri Fabrikası depolama alanındaki tüm/özel kaynakları (blob/kapsayıcısı) kısıtlanmış/zaman sınırlı erişim sağlar.

Paylaşılan erişim imzası temsilci depolama hesabınızdaki kaynaklara erişim sağlar. Bir istemci belirli bir süre sınırlı depolama hesabındaki nesnelere izinleri vermek için bir paylaşılan erişim imzası kullanabilirsiniz. Hesap erişim tuşlarınızı paylaşmak gerekmez. Paylaşılan erişim imzası depolama kaynağı için kimlik doğrulamalı erişim için gerekli tüm bilgileri kendi sorgu parametrelerini kapsayan bir URI değil. Paylaşılan erişim imzası ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun oluşturucunun ya da yöntemi paylaşılan erişim imzası geçirmek gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları: paylaşılan erişim imzası modelini anlamanıza](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Veri Fabrikası artık yalnızca hizmet paylaşılan erişim imzaları ancak hesap paylaşılan erişim imzaları destekler. Bu iki türleri ve bunları oluşturma hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları türlerini](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). Paylaşılan erişim Azure portal ya da Azure Storage Gezgini üretilen imza desteklenmeyen bir hesap paylaşılan erişim imzası URL'dir.

> [!TIP]
> Depolama hesabınız için hizmet paylaşılan erişim imzası oluşturmak için aşağıdaki PowerShell komutları çalıştırabilirsiniz. Yer tutucuları değiştirin ve gerekli izni verin.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Hizmet paylaşılan erişim imzası kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **AzureStorage**. |Evet |
| sasUri | Blob, kapsayıcı ya da tablo gibi depolama kaynakları için paylaşılan erişim imzası URI belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Paylaşılan erişim imzası URI oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Bağlantılı hizmet (okuma, yazma, okuma/yazma), veri fabrikası nasıl kullanıldığına göre nesneler üzerinde uygun okuma/yazma izinleri ayarlayın.
- Ayarlama **sona erme saati** uygun şekilde. Depolama nesnelere erişimi ardışık düzen etkin süresi içinde süresi sona ermiyor emin olun.
- URI gereksinimleri temelinde sağ kapsayıcı/blob veya tablo düzeyinde oluşturulmalıdır. Bir blob için bir paylaşılan erişim imzası URI veri fabrikası'nın bu belirli blob erişim sağlar. Bir Blob Depolama kapsayıcısını URI paylaşılan erişim imzası BLOB'ları bu kapsayıcıda yinelemek Data Factory sağlar. Daha sonra daha fazla veya daha az nesne erişim sağlamak veya paylaşılan erişim imzası URI güncelleştirmek için yeni bir URI ile bağlantılı hizmet güncelleştirmeyi unutmayın.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Blob Depolama veri kümesi tarafından desteklenen özellikler listesini sağlar.

Depolamaya ve Blob depolamadan veri kopyalamak için veri kümesi için tür özelliği ayarlamak **AzureBlob**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak **AzureBlob**. |Evet |
| folderPath | Kapsayıcı ve klasöre blob depolamada yolu. Örnek myblobcontainer/myblobfolder /. |Evet |
| fileName | **Adı veya joker karakter filtresini** belirtilen "folderPath" altında blob(s) için. Bu özellik için bir değer belirtmezseniz, veri kümesi klasöründeki tüm BLOB'lar işaret eder. <br/><br/>Filtre için joker karakter verilir: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/><br/>Dosya adı değil belirtildiği zaman bir çıkış veri kümesi için ve **preserveHierarchy** değil belirtilen etkinlik havuzunda kopyalama etkinliği, blob adı aşağıdaki desen ile otomatik olarak oluşturur: `Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]`. "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz" örneğidir. |Hayır |
| Biçimi | Depoları arasında (ikili kopya), dosya tabanlı olarak dosyaları kopyalamak girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** altında özellik **biçimi** şu değerlerden biri için. Daha fazla bilgi için bkz: [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopyalama senaryosu) |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasörü altındaki tüm BLOB'lar kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir blob kopyalamak için belirtin **folderPath** klasörü bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında BLOB kopyalamak için belirtmek **folderPath** klasörü bölümüyle ve **fileName** joker karakter Filtresi ile. 

**Örnek:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "myfile.csv.gz",
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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Blob Depolama kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="blob-storage-as-a-source-type"></a>Bir kaynak türü olarak BLOB Depolama

Blob depolama alanından verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **BlobSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **BlobSource**. |Evet |
| Özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. Özyinelemeli ayarlandığında true ve havuz dosya tabanlı depolama, bir boş klasör veya alt klasör değil kopyaladığınız veya havuz oluşturulmuş olduğunu unutmayın.<br/>İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |

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

### <a name="blob-storage-as-a-sink-type"></a>Havuz türü olarak BLOB Depolama

Blob depolama alanına veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **BlobSink**. Aşağıdaki özellikler de desteklenen **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak **BlobSink**. |Evet |
| copyBehavior | Kaynak dosyalarını bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler:<br/><b>-PreserveHierarchy (varsayılan)</b>: Dosya hiyerarşisi hedef klasördeki korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yolunu aynıdır.<br/><b>-FlattenHierarchy</b>: tüm kaynak klasörü hedef klasör ilk düzeyi dosyalarıdır. Hedef dosyalar otomatik olarak oluşturulur adlara sahip. <br/><b>-MergeFiles</b>: bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Dosya veya blob adı belirtilirse, birleştirilmiş dosya adı belirtilen addır. Aksi halde, bir otomatik olarak oluşturulur dosya adı değil. | Hayır |

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

### <a name="some-recursive-and-copybehavior-examples"></a>Özyinelemeli ve copyBehavior örnekler

Bu bölümde, sonuçta elde edilen davranışını özyinelemeli ve copyBehavior değerleri farklı birleşimlerini kopyalama işlemi açıklanmaktadır.

| Özyinelemeli | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 kaynak aynı yapısını oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulur adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulur adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulur adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulur adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulur adı |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 + dosya3 + File4 + File5 içeriği otomatik olarak oluşturulur dosya adına sahip bir dosya halinde birleştirilir. |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 Subfolder1 toplanma değil. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulur adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulur adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 toplanma değil. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 içeriği otomatik olarak oluşturulur dosya adına sahip bir dosya halinde birleştirilir. dosya1 için otomatik olarak oluşturulur adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 toplanma değil. |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
