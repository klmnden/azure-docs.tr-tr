---
title: Azure Data factory'de sınırlandırılmış metin biçimi | Microsoft Docs
description: Bu konu, Azure Data factory'de sınırlandırılmış metin biçimi uğraşmanız açıklar.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 4e365ef01b8c7a89d61efad25bd18943e7b2d1a4
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64877976"
---
# <a name="delimited-text-format-in-azure-data-factory"></a>Azure Data factory'de sınırlandırılmış metin biçimi

Aşağıdakileri yapmak istediğinizde bu makaleyi takip **sınırlandırılmış metin dosyalarını ayrıştırmak veya verileri sınırlandırılmış metin biçiminde yazmak**. 

Sınırlandırılmış metin biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md), [Azure Data Lake depolama Gen2](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [Dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google bulut depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md)ve [ SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, sınırlandırılmış metin veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Dataset öğesinin type özelliği ayarlanmalıdır **DelimitedText**. | Evet      |
| location         | Dosya konumunu ayarlar. Her dosya tabanlı bağlayıcı kendi konum türü ve desteklenen özellikleri altında `location`. **Makale bağlayıcı -> veri kümesi özellikleri bölümündeki ayrıntılara bakın**. | Evet      |
| columnDelimiter  | Bir dosyadaki sütunları ayırmak için kullanılan karakter. Şu anda çok char sınırlayıcısı, yalnızca veri akışı eşleme ancak kopyalama etkinliği için desteklenir. <br>Varsayılan değer **virgülle `,`** , sütun sınırlayıcısı anlamına sınırlayıcı, satırın tamamını tek bir sütun olarak alınır boş dize olarak tanımlanır. | Hayır       |
| rowDelimiter     | Tek bir karakter veya "\r\n" bir dosyadaki satırları ayırmak için kullanılır.<br>Aşağıdaki değerlerden herhangi birini varsayılan değer: **okunur: ["\r\n", "\r", "\n"]**, ve **"\n" veya "\r\n" yazma** eşleme veri akışı ve kopyalama etkinliği. <br>Zaman `rowDelimiter` yok (boş dize), sınırlayıcı kümesi `columnDelimiter` de tüm içeriği tek bir değer değerlendirilecek yani olarak sınırlayıcı (boş dize) olarak ayarlanmalıdır. | Hayır       |
| quoteChar        | Sütun sınırlayıcısı içeriyorsa sütun değerleri alıntılamak tek karakter. <br>Varsayılan değer **çift tırnak** `"`. <br>Eşleme, veri akışı için `quoteChar` boş bir dize olamaz. <br>Kopyalama etkinliği için zaman `quoteChar` tanımlanan boş bir dize hiçbir tırnak işareti karakteri var ve sütun değeri değil teklif geldiğini ve `escapeChar` sütun sınırlayıcısı ile kendisi kaçış için kullanılır. | Hayır       |
| escapeChar       | Tırnak işaretleri içinde tırnak içine alınmış bir değer kaçış tek karakter.<br>Varsayılan değer **ters eğik çizgi `\`** . <br>Eşleme, veri akışı için `escapeChar` boş bir dize olamaz. <br/>Kopyalama etkinliği için zaman `escapeChar` boş dize olarak tanımlanır `quoteChar` da boş dize ayarlamak, bu durumda tüm sütun değerleri sınırlayıcılar içermeyen emin olun olmalıdır. | Hayır       |
| firstRowAsHeader | Kabul/ilk satırı üst bilgi satırı sütunların adlarıyla oluşturmak belirtir.<br>İzin verilen değerler **true** ve **false** (varsayılan). | Hayır       |
| nullValue        | Null değeri dize gösterimini belirtir. <br>Varsayılan değer **boş dize**. | Hayır       |
| encodingName     | Test dosyaları okuma/yazma için kullanılan kodlama türü. <br>İzin verilen değerler aşağıdaki gibidir: "UTF-8", "UTF-16", "UTF-16BE TÜRLERİNDEN", "UTF-32", "UTF-32BE", "US-ASCII", "UTF-7", "BIG5", "JP EUC", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", " IBM852 ","IBM855","IBM857","IBM860","IBM861","IBM863","IBM864","IBM865","IBM869","IBM870","IBM01140","IBM01141","IBM01142","IBM01143","IBM01144","IBM01145","IBM01146","IBM01147","IBM01148","IBM01149","ISO-2022-JP","ISO-2022-KR ","ISO-8859-1","ISO-8859-2","ISO-8859-3","ISO-8859-4","ISO-8859-5","ISO-8859-6","ISO-8859-7","ISO-8859-8","ISO-8859-9","ISO-8859-13","ISO-8859-15","WINDOWS-874","WINDOWS-1250","WINDOWS-1251","WINDOWS-1252","WINDOWS-1253"," WINDOWS 1254 ","WINDOWS-1255","WINDOWS-1256","WINDOWS-1257","WINDOWS-1258".<br>Not eşleme veri akışı, UTF-7 kodlaması desteklemiyor. | Hayır       |
| compressionCodec | Metin dosyalarını okuma/yazma için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler **bzıp2**, **gzip**, **deflate**, **ZipDeflate**, **snappy**, veya **lz4**. Dosya kaydedilirken kullanmak için. <br>Şu anda "snappy" & "lz4" kopyalama etkinliğinin desteklemediği ve veri akışı eşleme "ZipDeflate" desteklemiyor unutmayın. | Hayır       |
| compressionLevel | Sıkıştırma oranı. <br>İzin verilen değerler **Optimal** veya **en hızlı**.<br>- **Hızlı:** Sonuç dosyası en uygun şekilde sıkıştırılmaz bile sıkıştırma işlemi mümkün olduğunca hızlı bir şekilde tamamlamanız gerekir.<br>- **En iyi**: Bile işlemin tamamlanması çok uzun sürüyor sıkıştırma işlemi en uygun şekilde, sıkıştırılmış olması gerekir. Daha fazla bilgi için [sıkıştırma düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konu. | Hayır       |

Azure Blob Depolama'da sınırlandırılmış metin kümesinin bir örnek aşağıda verilmiştir:

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
                "folderPath": "folder/subfolder",
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, sınırlandırılmış metin kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="delimited-text-as-source"></a>Kaynak olarak sınırlandırılmış metin 

Kopyalama etkinliği aşağıdaki özellikler desteklenir ***\*kaynak\**** bölümü.

| Özellik       | Açıklama                                                  | Gerekli |
| -------------- | ------------------------------------------------------------ | -------- |
| type           | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **DelimitedTextSource**. | Evet      |
| formatSettings | Özellikler grubu. Başvurmak **sınırlandırılmış metin okuma ayarları** aşağıdaki tabloda. | Hayır       |
| storeSettings  | Bir veri deposundan veri okuma özellikleri grubudur. Her dosya tabanlı kendi desteklenen bir okuma ayarlarında bağlayıcının `storeSettings`. **Kopyalama etkinliği özellikler bölümü -> bağlayıcı makalede ayrıntılara bakın**. | Hayır       |

Desteklenen **sınırlandırılmış metin okuma ayarları** altında `formatSettings`:

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | FormatSettings türünü ayarlamak **DelimitedTextReadSetting**. | Evet      |
| skipLineCount | Sayısını gösteren **boş** veri giriş dosyalarından okuma sırasında atlanacak satır. <br>Hem skipLineCount hem de firstRowAsHeader parametresi belirtilirse önce satırlar atlanır, ardından giriş dosyasındaki üst bilgi bilgileri okunur. | Hayır       |

### <a name="delimited-text-as-sink"></a>Havuz olarak sınırlandırılmış metin

Kopyalama etkinliği aşağıdaki özellikler desteklenir ***\*havuz\**** bölümü.

| Özellik       | Açıklama                                                  | Gerekli |
| -------------- | ------------------------------------------------------------ | -------- |
| type           | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **DelimitedTextSink**. | Evet      |
| formatSettings | Özellikler grubu. Başvurmak **sınırlandırılmış metin yazma ayarları** aşağıdaki tabloda. |          |
| storeSettings  | Bir veri deposuna veri yazmaya yönelik özellikler grubu. Her dosya tabanlı kendi desteklenen yazma ayarlarında bağlayıcının `storeSettings`. **Kopyalama etkinliği özellikler bölümü -> bağlayıcı makalede ayrıntılara bakın**. | Hayır       |

Desteklenen **sınırlandırılmış metin yazma ayarları** altında `formatSettings`:

| Özellik      | Açıklama                                                  | Gerekli                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | FormatSettings türünü ayarlamak **DelimitedTextWriteSetting**. | Evet                                                   |
| fileExtension | Örneğin çıkış dosyalarının adı için kullanılan dosya uzantısı `.csv`, `.txt`. Olmalıdır belirtilen `fileName` çıktısında belirtilmemiş DelimitedText veri kümesi. | Çıkış veri kümesinde dosya adı belirtilmemişse, Evet |

## <a name="mapping-data-flow-properties"></a>Veri akışı özellikleri eşleme

Ayrıntıları öğrenin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı olarak.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)