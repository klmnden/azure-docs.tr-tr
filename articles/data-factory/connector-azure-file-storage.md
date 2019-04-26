---
title: Azure Data Factory kullanarak Azure BLOB'dan/Azure dosya depolama veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Azure dosya depolama için desteklenen bir havuz veri depolarına (veya) Azure dosya depolama için desteklenen kaynak veri depolarından veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: b43532ade35e4e01573abbd3f47d009beadd9c60
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387851"
---
# <a name="copy-data-from-or-to-azure-file-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri veya Azure dosya Depolama'ya kopyalayın

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Azure dosya depolama (Azure dosyaları) veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure dosya depolama'yı tüm desteklenen havuz veri deposuna veri kopyalamak ya da herhangi bir desteklenen kaynak veri deposundan Azure dosya depolama alanına veri kopyalama. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure dosya depolama bağlayıcı kopyalama dosyaları gibi destekler- ya da ayrıştırma/oluşturma dosyalarıyla [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure dosya depolama için belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure dosya depolama bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Dosya sunucusu**. | Evet |
| konak | Azure dosya depolama uç noktası olarak belirtir: <br/>-Kullanıcı arabirimini kullanarak: belirtin `\\<storage name>.file.core.windows.net\<file service name>`<br/>-JSON'ı kullanma: `"host": "\\\\<storage name>.file.core.windows.net\\<file service name>"`. | Evet |
| Kullanıcı Kimliği | Azure dosya depolama alanı olarak erişmek için kullanıcı belirtin: <br/>-Kullanıcı arabirimini kullanarak: belirtin `AZURE\<storage name>`<br/>-JSON'ı kullanma: `"userid": "AZURE\\<storage name>"`. | Evet |
| password | Depolama erişim anahtarı belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Kaynak, havuz için Evet Hayır |

>[!IMPORTANT]
> - Azure tümleştirme çalışma zamanı, açıkça kullanarak Azure dosya depolama alanına veri kopyalamak için [Azure IR oluşturma](create-azure-integration-runtime.md#create-azure-ir) aşağıdaki örnek olarak, dosya depolama ve bağlı hizmette ilişkilendirme konumu ile.
> - Azure dışındaki şirket içinde barındırılan Integration Runtime'ı kullanarak Azure dosya depolama / için veri kopyalamak için yerel ağınızdaki giden TCP bağlantı noktası 445'i açmak unutmayın.

>[!TIP]
>Yazma için ADF kullanıcı arabirimini kullanarak, hangi underneath türü oluşturur, bağlı hizmet oluşturmak için "Azure dosya depolama" belirli bir giriş bulabilirsiniz `FileServer` nesne.

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure dosya depolama veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Veri kopyalama, BLOB'dan/Azure dosya depolama için dataset öğesinin type özelliği ayarlamak **FileShare**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **Dosya Paylaşımı** |Evet |
| folderPath | Klasör yolu. <br/><br/>Joker karakter filtresi desteklenir, joker karakterlere izin şunlardır: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanmak `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br/><br/>Örnekler: rootfolder/alt/daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Evet |
| fileName | **Adı veya joker karakter filtresi** belirtilen "folderPath" altında dosyaları için. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek dosya adı varsa, kaçış için.<br/><br/>Dosya adı değil belirtildiği zaman için bir çıktı veri kümesi ve **preserveHierarchy** belirtilmediyse etkinliği havuz kopyalama etkinliği, dosya adı şu deseni ile otomatik olarak oluşturur: "*Veri. [etkinlik çalıştırma kimliği GUID]. [GUID, FlattenHierarchy]. [biçim] yapılandırılmışsa. [yapılandırdıysanız sıkıştırma]* ", örneğin "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; Tablo adı yerine sorgu kullanarak tablo kaynağından kopyalarsanız, adı desendir "*[tablo adı]. [ Biçim]. [yapılandırdıysanız sıkıştırma]* ", örneğin "MyTable.csv". |Hayır |
| modifiedDatetimeStart | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| biçim | İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** joker karakter Filtresi ile.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, hala olarak desteklenmektedir-olduğu sırada "bundan sonra dosya adı için" eklenen yeni filtre özelliği kullanmak için önerilir.

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure dosya depolama kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-file-storage-as-source"></a>Kaynak olarak Azure dosya depolama

Azure dosya depolama'yı veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **FileSystemSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **FileSystemSource** |Evet |
| özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanan/oluşturulmuş olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |

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

### <a name="azure-file-storage-as-sink"></a>Azure dosya depolama havuzu olarak

Azure dosya depolama alanına veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **FileSystemSink**. Aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır: **FileSystemSink** |Evet |
| copyBehavior | Kaynak dosyaları dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><b>-FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik adına sahip. <br/><b>-MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilmezse, belirtilen adı olur; Aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. | Hayır |

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

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, sonuçta elde edilen davranışını klasör yolu ve dosya adı joker filtrelerle açıklanmaktadır.

| folderPath | fileName | özyinelemeli | Kaynak klasör yapısını ve filtre sonucunu (dosyalar **kalın** alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (boş, varsayılan kullanın) | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (boş, varsayılan kullanın) | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri

Bu bölümde, elde edilen davranışını özyinelemeli ve copyBehavior değer farklı birleşimleri kopyalama işlemi açıklanmaktadır.

| özyinelemeli | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1, kaynağı olarak aynı yapıya sahip oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir |
| false |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |flattenHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. Fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
