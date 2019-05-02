---
title: Azure Data factory'de parquet biçimi | Microsoft Docs
description: Bu konu, Azure Data factory'de Parquet biçimi uğraşmanız açıklar.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 5b711fac2a7bc41d11bcc80927f460390888cc3e
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64878066"
---
# <a name="parquet-format-in-azure-data-factory"></a>Azure Data factory'de parquet biçimi

Aşağıdakileri yapmak istediğinizde bu makaleyi takip **Parquet dosyalarını ayrıştırmak veya verileri Parquet biçiminde yazmak**. 

Parquet biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md), [Azure Data Lake depolama Gen2](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [Dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google bulut depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md)ve [ SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Parquet veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Dataset öğesinin type özelliği ayarlanmalıdır **Parquet**. | Evet      |
| location         | Dosya konumunu ayarlar. Her dosya tabanlı bağlayıcı kendi konum türü ve desteklenen özellikleri altında `location`. **Makale bağlayıcı -> veri kümesi özellikleri bölümündeki ayrıntılara bakın**. | Evet      |
| compressionCodec | Parquet dosyasına yazarken kullanılacak sıkıştırma codec bileşeni. Parquet dosyadan okurken, Data Factory otomatik olarak belirlemek dosya meta verileri temel alarak sıkıştırma codec bileşeni.<br>Desteklenen türler "**hiçbiri**","**gzip**","**snappy**" (varsayılan), ve "**lzo**". Şu anda LZO kopyalama etkinliğinin desteklemediği unutmayın. | Hayır       |

> [!NOTE]
> Sütun adı boşluk Parquet dosyalarını için desteklenmiyor.

Azure Blob Depolama'da Parquet kümesinin bir örnek aşağıda verilmiştir:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
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
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Parquet kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="parquet-as-source"></a>Kaynak olarak parquet

Kopyalama etkinliği aşağıdaki özellikler desteklenir ***\*kaynak\**** bölümü.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **ParquetSource**. | Evet      |
| storeSettings | Bir veri deposundan veri okuma özellikleri grubudur. Her dosya tabanlı kendi desteklenen bir okuma ayarlarında bağlayıcının `storeSettings`. **Kopyalama etkinliği özellikler bölümü -> bağlayıcı makalede ayrıntılara bakın**. | Hayır       |

### <a name="parquet-as-sink"></a>Havuz olarak parquet

Kopyalama etkinliği aşağıdaki özellikler desteklenir ***\*havuz\**** bölümü.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **ParquetSink**. | Evet      |
| storeSettings | Bir veri deposuna veri yazmaya yönelik özellikler grubu. Her dosya tabanlı kendi desteklenen yazma ayarlarında bağlayıcının `storeSettings`. **Kopyalama etkinliği özellikler bölümü -> bağlayıcı makalede ayrıntılara bakın**. | Hayır       |

## <a name="mapping-data-flow-properties"></a>Veri akışı özellikleri eşleme

Ayrıntıları öğrenin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı olarak.

## <a name="data-type-support"></a>Veri türü desteği

Karmaşık veri türleri şu anda kullanıma alınmamış parquet (örneğin, harita, LIST, yapı) desteklenir.

## <a name="using-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanını kullanma

> [!IMPORTANT]
> Kopyalama şirket içinde barındırılan tümleştirme çalışma zamanı tarafından örneğin şirket içi ile bulut arasında yetkilendirilmiş için Parquet dosyalarını kopyalıyorsanız değil, verilerin depolandığı **olarak-olan**, yüklemeniz gerekir **64 bit JRE 8 (Java Çalışma zamanı ortamı) veya OpenJDK** IR makinenizde. Daha fazla ayrıntı içeren aşağıdaki paragrafa bakın.

Parquet dosyası serileştirme/seri kaldırma ile şirket içinde barındırılan IR üzerinde çalışan kopya için ilk olarak kayıt defteri denetleyerek ADF Java Çalışma zamanı bulur *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* JRE, aksi takdirde için bulunamadı, sistem değişkeni ikincisidenetimi*`JAVA_HOME`* OpenJDK için.

- **JRE kullanılacak**: 64-bit IR 64 bit JRE gerekir. Buradan bulabilirsiniz [burada](https://go.microsoft.com/fwlink/?LinkId=808605).
- **OpenJDK kullanılacak**: sürüm 3.13 IR itibaren desteklenir. Paketi diğer tüm jvm.dll OpenJDK derlemelerinin şirket içinde barındırılan IR makine ve JAVA_HOME ortam değişken Ayarla sistem uygun şekilde gerekli.

> [!TIP]
> Kopyalarsanız veri gönderip buralardan veri Parquet biçimi kullanılarak şirket içinde barındırılan tümleştirme çalışma zamanı ve hata bildiren isabet "java çağrılırken bir hata oluştu. ileti: **java.lang.OutOfMemoryError:Java yığın alanı**", bir ortam değişkeni ekleyebilirsiniz. `_JAVA_OPTIONS` böyle kopyalama olanağı JVM için en düşük/en yüksek yığın boyutunu ayarlamak için şirket içinde barındırılan IR barındıran makine, işlem hattını yeniden.

![Şirket içinde barındırılan IR üzerinde JVM öbek boyutunu Ayarla](C:/AzureContent/azure-docs-pr/articles/data-factory/media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: değişken Ayarla `_JAVA_OPTIONS` değerle `-Xms256m -Xmx16g`. Bayrağı `Xms` bir Java sanal makinesi (JVM için), ilk bellek ayırma havuzu belirtir ancak `Xmx` maksimum bellek ayırma havuzu belirtir. Bu JVM ile başlayacağı anlamına gelir `Xms` bellek miktarı ve en fazla kullanabilmek için `Xmx` bellek miktarı. Varsayılan olarak, ADF en az 64 MB'ı kullanın ve en fazla 1 G.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)