---
title: Azure Data Factory kullanarak/Azure dosya depolama alanına veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Azure File Storage desteklenen havuz veri depolarına (veya) Azure File Storage için desteklenen kaynak veri depolarına veri kopyalamak öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: d16ca88d844607721f4172352149596764b08248
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="copy-data-from-or-to-azure-file-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri veya Azure dosya depolama alanına kopyalayın

Bu makalede kopya etkinliği Azure Data Factory'de ilk ve son Azure File Storage (Azure dosyaları) verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Azure dosya depolama biriminden tüm desteklenen havuz veri deposuna kopyalamak ya da veri tüm desteklenen kaynak veri deposundan Azure dosya depolama alanına kopyalayın. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure File Storage bağlayıcı kopyalama dosyaları olarak destekler- ya da ayrıştırma/oluşturma dosyalarıyla [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümlerde Azure File Storage için Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Azure dosya depolama bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **DosyaSunucusu**. | Evet |
| konak | Azure File Storage uç noktası olarak belirtir `"host": "\\\\<storage name>.file.core.windows.net\\<file service name>"`. | Evet |
| Kullanıcı Kimliği | Azure File Storage olarak erişmek için kullanıcı belirtin `"userid": "AZURE\\<storage name>"`. | Evet |
| password | Depolama erişim anahtarı belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Kaynak havuzu için Evet için Hayır'ı |

>[!IMPORTANT]
> - Azure tümleştirmesi çalışma zamanı, açıkça kullanarak Azure dosya depolama alanına veri kopyalamak için [Azure IR oluşturmak](create-azure-integration-runtime.md#create-azure-ir) aşağıdaki örnekteki gibi dosya depolama ve ilişkilendirme bağlantılı hizmet konumu ile.
> - Başlangıç/bitiş Azure File Storage kullanma Azure dışında Self-hosted tümleştirmesi çalışma zamanı verileri kopyalamak için yerel ağınızda giden TCP bağlantı noktası 445'i açmak unutmayın.

**Örnek:**

```json
{
    "name": "AzureFileStorageLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "\\\\<storage name>.file.core.windows.net\\<file service name>",
            "userid": "AZURE\\<storage name>",
            "password": {
                "type": "SecureString",
                "value": "<storage access key>"
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Azure File Storage veri kümesi tarafından desteklenen özellikler listesini sağlar.

/ Azure File Storage için verileri kopyalamak için kümesine tür özelliği ayarlamak **FileShare**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **dosya paylaşımı** |Evet |
| folderPath | Klasör yolu. |Evet |
| fileName | **Adı veya joker karakter filtresini** belirtilen "folderPath" altında dosyalar için. Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret eder. <br/><br/>Filtre için joker karakter verilir: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/><br/>Dosya adı değil belirtildiği zaman bir çıkış veri kümesi için ve **preserveHierarchy** değil belirtilen etkinlik havuzunda kopyalama etkinliği, bir dosya adı şu biçimi ile otomatik olarak oluşturur: `Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]`. "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz" örneğidir. |Hayır |
| Biçimi | İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopyalama senaryosu) |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasörü altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasörü bölümüyle ve **fileName** dosya adına sahip.<br>Bir klasör altındaki dosyalar kümesini kopyalamak için belirtin **folderPath** klasörü bölümüyle ve **fileName** joker karakter Filtresi ile.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, hala olarak desteklenmektedir-"ileride dosyaadı" eklenen yeni filtre özellikten yararlanabilmek için önerilen, açıkken.

**Örnek:**

```json
{
    "name": "AzureFileStorageDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<Azure File Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure File Storage kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-file-storage-as-source"></a>Kaynak olarak Azure dosya depolama

Azure dosya depolama alanından verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **FileSystemSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **FileSystemSource** |Evet |
| Özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanır ve oluşturulan olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure File Storage input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-file-storage-as-sink"></a>Havuz olarak Azure dosya depolama

Azure dosya depolama alanına veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **FileSystemSink**. Aşağıdaki özellikler de desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **FileSystemSink** |Evet |
| copyBehavior | Kaynak dosyaları dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler:<br/><b>-PreserveHierarchy (varsayılan)</b>: Dosya hiyerarşisi hedef klasördeki korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yolunu aynıdır.<br/><b>-FlattenHierarchy</b>: tüm kaynak klasörü hedef klasör ilk düzeyi dosyalarıdır. Hedef dosyalar otomatik adına sahip. <br/><b>-MergeFiles</b>: bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilirse, belirtilen ad olur; Aksi takdirde otomatik olarak oluşturulan dosya adı olacaktır. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure File Storage output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri

Bu bölümde, sonuçta elde edilen davranışını özyinelemeli ve copyBehavior değerleri farklı birleşimlerini kopyalama işlemi açıklanmaktadır.

| Özyinelemeli | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 kaynak aynı yapısını oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan adı |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. dosya1 için otomatik olarak oluşturulan adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).