---
title: Azure Data Factory kullanarak Azure BLOB'dan/dosya sistemi veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak dosya sistemine desteklenen kaynak veri depolarından (veya) desteklenen havuz veri depolarına dosya sisteminden veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: jingwang
ms.openlocfilehash: eed630038169deb9a161d320ff1521bfade2b113
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60534005"
---
# <a name="copy-data-to-or-from-a-file-system-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri ya da bir dosya sisteminden kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-onprem-file-system-connector.md)
> * [Geçerli sürüm](connector-file-system.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve dosya sistemi veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Dosya sisteminden tüm desteklenen havuz veri deposuna veri kopyalamak ya da dosya sistemine herhangi bir desteklenen kaynak veri deposundan veri kopyalayın. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu dosya sistemi bağlayıcısını destekler:

- / İçin yerel makine veya ağ dosya paylaşımına dosyaları kopyalanıyor. Linux dosya paylaşımını kullanmak için yükleme [Samba](https://www.samba.org/) Linux sunucunuzdaki.
- Dosyalar kopyalanıyor kullanarak **Windows** kimlik doğrulaması.
- Dosyaları olarak kopyalama- ya da ayrıştırma/oluşturma dosyalarıyla [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Önkoşullar

/ İçin genel olarak erişilebilir değil bir dosya sistemi veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli dosya sistemine tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Dosya sistemi bağlantılı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Dosya sunucusu**. | Evet |
| konak | Kopyalamak istediğiniz klasörün kök yolunu belirtir. Çıkış karakterini kullanma "\" dizedeki özel karakterleri. Bkz: [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekler. | Evet |
| Kullanıcı Kimliği | Sunucu erişimi olan kullanıcının kimliği belirtin. | Evet |
| password | (Kullanıcı kimliği) kullanıcının parolasını belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

### <a name="sample-linked-service-and-dataset-definitions"></a>Bağlı hizmet ve veri kümesi tanımları örneği

| Senaryo | bağlı hizmet tanımında "ana" | veri kümesi tanımında "folderPath" |
|:--- |:--- |:--- |
| Tümleştirme çalışma zamanı makinesinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |JSON'da: `D:\\`<br/>Kullanıcı Arabirimi üzerindeki: `D:\` |Giriş JSON: `.\\` veya `folder\\subfolder`<br>Üzerinde kullanıcı Arabirimi: `.\` veya `folder\subfolder` |
| Paylaşılan uzak klasör: <br/><br/>Örnekler: \\ \\myserver\\paylaşmak\\ \* veya \\ \\myserver\\paylaşmak\\klasör\\alt\\* |JSON'da: `\\\\myserver\\share`<br/>Kullanıcı Arabirimi üzerindeki: `\\myserver\share` |Giriş JSON: `.\\` veya `folder\\subfolder`<br/>Üzerinde kullanıcı Arabirimi: `.\` veya `folder\subfolder` |

>[!NOTE]
>Kullanıcı Arabirimi yazılırken çift ters eğik çizgi giriş gerekmez (`\\`) gibi JSON erişmek için tek bir ters eğik çizgi belirtin.

**Örnek:**

```json
{
    "name": "FileLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "<host>",
            "userid": "<domain>\\<user>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, dosya sistemi veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Azure BLOB'dan/dosya sistemi kopyalamak için dataset öğesinin type özelliği ayarlamak **FileShare**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **Dosya Paylaşımı** |Evet |
| folderPath | Klasör yolu. Joker karakter filtresi desteklenir, joker karakterlere izin şunlardır: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanmak `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br/><br/>Örnekler: rootfolder/alt/daha fazla örneklere bakın [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) ve [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Hayır |
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
    "name": "FileSystemDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<file system linked service name>",
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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, dosya sistemi kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="file-system-as-source"></a>Dosya sistemi kaynak olarak

Dosya sisteminden veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **FileSystemSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **FileSystemSource** |Evet |
| özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanan/oluşturulmuş olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<file system input dataset name>",
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

### <a name="file-system-as-sink"></a>Havuz olarak dosya sistemi

Dosya sistemi veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **FileSystemSink**. Aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır: **FileSystemSink** |Evet |
| copyBehavior | Kaynak dosyaları dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><b>-FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik adına sahip. <br/><b>-MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilmezse, belirtilen adı olur; Aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<file system output dataset name>",
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
