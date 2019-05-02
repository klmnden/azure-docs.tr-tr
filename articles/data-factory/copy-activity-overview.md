---
title: Azure veri fabrikasında kopyalama etkinliği | Microsoft Docs
description: Azure Data Factory kopyalama etkinliği, verileri desteklenen kaynak veri deposundan desteklenen bir havuz veri deposuna kopyalamak için kullanabileceğiniz öğrenin.
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
ms.openlocfilehash: d04bb965ddf9616aaa01f4c8822ac42aea6dab2d
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869552"
---
# <a name="copy-activity-in-azure-data-factory"></a>Azure veri fabrikasında kopyalama etkinliği

## <a name="overview"></a>Genel Bakış

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-data-movement-activities.md)
> * [Geçerli sürüm](copy-activity-overview.md)

Azure Data Factory'de arasında veri depoları şirket içindeki ve buluttaki veri kopyalamak için kopyalama etkinliği'ni kullanabilirsiniz. Veri kopyalandıktan sonra daha fazla dönüştürülür ve analiz edilebilir. Kopyalama etkinliği, dönüştürme ve iş zekası (BI) ve uygulama tüketimini analiz sonuçları yayımlamak için de kullanabilirsiniz.

![Kopyalama etkinliği rolü](media/copy-activity-overview/copy-activity.png)

Kopyalama Etkinliği yürütüldüğünde bir [Integration Runtime](concepts-integration-runtime.md). Farklı veri kopyalama senaryosu için Integration Runtime'nın farklı flavor yararlanılabilir:

* Veriler arasında veri kopyalama hem de genel olarak erişilebilir olduğunu depoladığında, kopyalama etkinliği tarafından desteklenmesini **Azure Integration Runtime**, güvenli, güvenilir, ölçeklenebilir ve [küresel olarak kullanılabilir](concepts-integration-runtime.md#integration-runtime-location).
* Bulunan şirket içi verileri kopyalama/veri depolarına veya ayarlamak gereken erişim denetimi (örneğin, Azure sanal ağı) içeren bir ağda olduğunda bir **tümleşik çalışma zamanı barındırabileceğiniz** veri kopyalama olanağı.

Tümleştirme çalışma zamanı her kaynak ve havuz veri deposu ile ilişkilendirilmesi gerekir. Hakkında ayrıntılı bilgi edinin. kopyalama etkinliği [kullanılacak IR'yi belirler](concepts-integration-runtime.md#determining-which-ir-to-use).

Kopyalama etkinliği, verileri bir kaynaktan havuza kopyalamak için aşağıdaki aşamalara üzerinden gider. Kopyalama etkinliği'ni destekleyen hizmet:

1. Bir kaynak veri deposundan veri okur.
2. Serileştirme/seri durumundan çıkarma, sıkıştırma/açma, sütun eşleme, vb. gerçekleştirir. Bunu, giriş veri kümesi, çıktı veri kümesi ve kopyalama etkinliği yapılandırmalarına göre bu işlemleri yapar.
3. Veri havuz/hedef veri deposuna yazar.

![Kopyalama Etkinliği’ne Genel Bakış](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Desteklenen veri depoları ve biçimler

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri

Kopyalama etkinliği için kullanabileceğiniz **olarak dosya kopyalama-olan** içinde çalışması verilerin kopyalandığı verimli bir şekilde tüm serileştirme/seri kaldırma, iki dosya tabanlı veri depoları arasında.

Kopyalama etkinliği, okuma ve yazma belirtilen biçimde dosyalara da destekler: **Metin, JSON, Avro, ORC ve Parquet**, sıkıştırma ve aşağıdakileri dosyalarıyla boyutunda: **GZip, Deflate, Bzıp2 ve ZipDeflate**. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](supported-file-formats-and-compression-codecs.md) ayrıntılarla.

Örneğin, aşağıdaki kopyalama etkinlikleri yapabilirsiniz:

* Şirket içi SQL Server verileri kopyalayın ve Azure Data Lake depolama Gen2 Parquet biçiminde yazmak.
* Dosyaları (CSV) metin biçiminde şirket içi dosya sisteminden kopyalama ve Azure Blob Avro biçiminde yazmak.
* Şirket içi dosya sisteminden sıkıştırılmış dosyaları kopyalayın ve sonra Azure Data Lake depolama Gen2'ye land açılamadı.
* Verileri Azure Blobundan GZip sıkıştırılmış metni (CSV) biçiminde kopyalayın ve Azure SQL veritabanı'na yazın.
* Ve daha fazla çoğunlukla serileştirme/seri durumundan çıkarma veya sıkıştırma/açma gerekir.

## <a name="supported-regions"></a>Desteklenen bölgeler

Kopyalama etkinliği'ni destekleyen hizmet genel olarak bölgelerinde kullanılabilir durumdadır ve coğrafyalar listelenen [Azure Integration Runtime konumları](concepts-integration-runtime.md#integration-runtime-location). Dünya çapında topolojisi genellikle bölgeler arası atlama önler verimli veri taşıma sağlar. Bkz: [bölgelere göre Hizmetler](https://azure.microsoft.com/regions/#services) Data Factory veri taşıma bir bölge ve kullanılabilirlik için.

## <a name="configuration"></a>Yapılandırma

Azure veri fabrikasında kopyalama etkinliği kullanmak için yapmanız:

1. **Kaynak veri deposu ve havuz veri deposu için bağlı hizmetler oluşturacaksınız.** Yapılandırma ve desteklenen özellikleri bağlayıcı makalenin "bağlı hizmet özellikleri" bölümüne bakın. Desteklenen bağlayıcı listesinde bulabilirsiniz [desteklenen veri depoları ve biçimler](#supported-data-stores-and-formats) bölümü.
2. **Kaynak ve havuz için veri kümeleri oluşturun.** Kaynağını ve nasıl yapılandırılacağını ve desteklenen özellikleri bağlayıcı makalelerdeki "Veri kümesi özellikleri" bölümündeki havuz.
3. **Kopyalama etkinliği ile işlem hattı oluşturursunuz.** Sonraki bölümde, bir örnek sağlar.

### <a name="syntax"></a>Sözdizimi

Kopyalama etkinliği, aşağıdaki şablon desteklenen özelliklerin kapsamlı bir liste içerir. Senaryonuza uygun olanları belirtin.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği öğesinin type özelliği ayarlanmalıdır: **Kopyala** | Evet |
| girişler | Kaynak verileri hangi noktalara oluşturduğunuz veri kümesi belirtin. Kopyalama etkinliği, yalnızca tek bir giriş destekler. | Evet |
| çıkışlar | Havuz veri hangi noktalara oluşturduğunuz veri kümesi belirtin. Kopyalama etkinliği, yalnızca tek bir çıktı destekler. | Evet |
| typeProperties | Kopyalama etkinliği yapılandırmak için özellikler grubu. | Evet |
| source | Kopyalama kaynağı türü ve karşılık gelen özelliklere veri almak nasıl belirtin.<br/><br/>"Kopyalama etkinliğinin özellikleri" bölümündeki bağlayıcı makalede listelenen daha ayrıntılı bilgi [desteklenen veri depoları ve biçimler](#supported-data-stores-and-formats). | Evet |
| Havuz | Kopyalama Havuz türü ve karşılık gelen özelliklere veri yazma konusunda belirtin.<br/><br/>"Kopyalama etkinliğinin özellikleri" bölümündeki bağlayıcı makalede listelenen daha ayrıntılı bilgi [desteklenen veri depoları ve biçimler](#supported-data-stores-and-formats). | Evet |
| Translator | Kaynak havuzu için açıkça bir sütun eşlemelerini belirtin. Varsayılan kopyalama davranışı gereksinimi yerine getiremiyor uygulanır.<br/><br/>Ayrıntıları öğrenin [şema ve veri türü eşlemesi](copy-activity-schema-and-type-mapping.md). | Hayır |
| dataIntegrationUnits | ' In powerfulness belirtin [Azure Integration Runtime](concepts-integration-runtime.md) veri kopyalama olanağı. Eski veri taşıma birimleri (DMU) bulut olarak bilinir. <br/><br/>Ayrıntıları öğrenin [veri tümleştirme birimleri](copy-activity-performance.md#data-integration-units). | Hayır |
| parallelCopies | Kopyalama etkinliği, havuz için veri kaynağı ve veri yazma ait okunurken kullanılacak istediğiniz paralellik belirtin.<br/><br/>Ayrıntıları öğrenin [paralel kopyalama](copy-activity-performance.md#parallel-copy). | Hayır |
| enableStaging<br/>stagingSettings | Geçici verileri doğrudan veri kopyalama havuz kaynağından yerine bir blob depolama alanındaki hazırlamak bu seçeneği seçin.<br/><br/>Yararlı senaryoları ve yapılandırma ayrıntılarını öğrenmek [kopyalama aşamalı](copy-activity-performance.md#staged-copy). | Hayır |
| Enableskipıncompatiblerow<br/>Redirectıncompatiblerowsettings| Havuz kaynaktan veri kopyalama sırasında uyumsuz satırların işlemek nasıl seçin.<br/><br/>Ayrıntıları öğrenin [hataya dayanıklılık](copy-activity-fault-tolerance.md). | Hayır |

## <a name="monitoring"></a>İzleme

Kopyalama etkinliği Azure Data Factory "Yazar ve İzleyici" kullanıcı arabiriminde veya programlama yoluyla çalıştırmasının izleyebilirsiniz. Ardından performans ve senaryonuz için kopyalama etkinliği'nin yapılandırılmasını karşılaştırabilirsiniz [Performans başvurusu](copy-activity-performance.md#performance-reference) şirket içi test.

### <a name="monitor-visually"></a>Görsel olarak izleme

Kopyalama etkinliği çalıştırmasının görsel olarak izlemek için veri fabrikanıza gidin -> **yazar ve İzleyici** -> **İzleyici sekmesi**, işlem hattı listesini çalıştırır bir"Etkinlikçalıştırmalarınıgörüntüle"bağlantısınabakın **Eylemler** sütun.

![İşlem hattı çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Bu işlem hattı çalıştırmasını etkinlikler listesini görmek için tıklayın. İçinde **eylemleri** sütun kopyalama etkinliği giriş, çıkış, (kopyalama etkinliği çalıştırma başarısız olursa) hataları ve ayrıntıları bağlantılar bulunur.

![Etkinlik çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Tıklayın "**ayrıntıları**" altında bağlantı **eylemleri** kopyalama etkinliği'nin yürütme ayrıntıları ve performans özelliklerini görmek için. Bu havuz için kaynak, aktarım hızı, karşılık gelen süre ile geçtiği ve yapılandırmaları kopyalama senaryonuz için kullanılan adımları dahil olmak üzere birim/satır/dosyaları veri kopyalanan bilgiler gösterir.

>[!TIP]
>Bazı senaryolarda, ayrıca görürsünüz "**performans ayarlama ipuçları**" örnek ayrıntılarlatanımlananperformanssorunuolduğunusöylervenekopyalamaaktarımhızıartırmakiçindeğiştirmeksize,sayfada,izlemekopyalamaüzerinebakın[burada](#performance-and-tuning).

**Örnek: Azure Data Lake Store için Amazon S3'ten kopyalama**
![İzleyici etkinlik çalışma ayrıntıları](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Örnek: Azure SQL veritabanından Azure SQL veri ambarı'nı kullanarak kopyalama için hazırlanan kopyalama**
![İzleyici etkinlik çalışma ayrıntıları](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Program aracılığıyla izleyin

Kopyalama etkinliğinin yürütme ayrıntıları ve performans özelliklerini de döndürülür kopyalama etkinliği çalıştırma sonucu -> çıkış bölümü. Kapsamlı bir liste aşağıda verilmiştir; Yalnızca kopya senaryonuza uygun ayarlara gösterilir. Çalıştırılan Etkinlik izleme hakkında bilgi edinin [hızlı başlangıç bölümünde izleme](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Özellik adı  | Açıklama | Birim |
|:--- |:--- |:--- |
| DataRead | Kaynaktan okunan veri boyutu | Int64 değeri **bayt** |
| DataWritten | Havuz için yazılan veri boyutu | Int64 değeri **bayt** |
| filesRead | Dosya depolama'yı veri kopyalama işlemi sırasında kopyalanan dosyaların sayısıdır. | Int64 değeri (birim) |
| fileScanned | Kaynak dosya depolama'yı taranan dosya sayısı. | Int64 değeri (birim) |
| filesWritten | Dosya depolama alanına veri kopyalama işlemi sırasında kopyalanan dosyaların sayısıdır. | Int64 değeri (birim) |
| rowsCopied | (İkili kopya için geçerli değildir) Kopyalanan satırların sayısı. | Int64 değeri (birim) |
| rowsSkipped | İki tanesinden uyumsuz satırların sayısı. True olarak Ayarla "Enableskipıncompatiblerow" tarafından özelliğini kapatabilirsiniz. | Int64 değeri (birim) |
| Aktarım hızı | Aktarılan ve veri oranı | Kayan noktalı sayı olarak **KB/sn** |
| copyDuration | Kopyalama süresi | Int32 değeri saniye |
| sqlDwPolyBase | PolyBase, SQL veri ambarı'na veri kopyalama işlemi sırasında kullanılıyorsa. | Boole |
| redshiftUnload | UNLOAD veri Redshift'ten kopyalarken kullanılıyorsa. | Boole |
| hdfsDistcp | Verileri HDFS kopyalarken DistCp kullanılıyorsa. | Boole |
| effectiveIntegrationRuntime | Etkinlik çalıştırma, biçiminde güçlendirmek için kullanılan tümleştirme Runtime(s) show `<IR name> (<region if it's Azure IR>)`. | Metin (dize) |
| usedDataIntegrationUnits | Kopyalama sırasında etkili veri tümleştirme birimi. | Int32 değeri |
| usedParallelCopies | Kopyalama sırasında etkili parallelCopies. | Int32 değeri|
| redirectRowPath | Blob depolamada Atlanan uyumsuz satırların günlük yolu "Redirectıncompatiblerowsettings" altında yapılandırın. Aşağıdaki örnekte bakın. | Metin (dize) |
| executionDetails | Kopyalama etkinliği, geçer aşamaları ve ilgili adımlarda, süre, kullanılan yapılandırmaları, vb. hakkında daha fazla bilgi. Bu bölümde, değişiklik gösterebileceği için ayrıştırılacak önermedi. | Dizi |

```json
"output": {
    "dataRead": 107280845500,
    "dataWritten": 107280845500,
    "filesRead": 10,
    "filesWritten": 10,
    "copyDuration": 224,
    "throughput": 467707.344,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US 2)",
    "usedDataIntegrationUnits": 32,
    "usedParallelCopies": 8,
    "executionDetails": [
        {
            "source": {
                "type": "AmazonS3"
            },
            "sink": {
                "type": "AzureDataLakeStore"
            },
            "status": "Succeeded",
            "start": "2018-01-17T15:13:00.3515165Z",
            "duration": 221,
            "usedDataIntegrationUnits": 32,
            "usedParallelCopies": 8,
            "detailedDurations": {
                "queuingDuration": 2,
                "transferDuration": 219
            }
        }
    ]
}
```

## <a name="schema-and-data-type-mapping"></a>Şema ve veri türü eşlemesi

Bkz: [şema ve veri türü eşlemesi](copy-activity-schema-and-type-mapping.md), kopyalama etkinliği havuz için kaynak verilerinizi nasıl eşlendiğini açıklar.

## <a name="fault-tolerance"></a>Hataya dayanıklılık

Kopyalama etkinliği, varsayılan olarak veri kopyalama durdurur ve uyumsuz veri kaynağı ve havuz arasında karşılaştığında laravel'den hata döndürür. Açıkça atlayın ve uyumsuz satırları oturum ve yalnızca kopyalama başarılı olmak için bu uyumlu veri kopyalamak için yapılandırabilirsiniz. Bkz: [kopyalama etkinliği hataya dayanıklılık](copy-activity-fault-tolerance.md) hakkında daha fazla bilgi.

## <a name="performance-and-tuning"></a>Performans ve ayar

Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](copy-activity-performance.md), Azure Data factory'deki veri taşıma (kopyalama etkinliği) performansını etkileyen önemli faktörlerin açıklar. Ayrıca, iç test sırasında gözlemlenen performans listeler ve kopyalama etkinliği performansı iyileştirmek için çeşitli yollar ele alınmaktadır.

Bazı durumlarda, bir kopyalama etkinliği, ADF'de yürüttüğünüzde doğrudan görürsünüz "**performans ayarlama ipuçları**" üst kısmındaki [kopyalama etkinliği izleme sayfası](#monitor-visually) aşağıdaki örnekte gösterildiği gibi. Yalnızca belirli bir kopya çalıştırmak için tanımlanan sorunu bildiren, ancak Ayrıca, ne kopyalama aktarım hızı artırmak için değiştirmek size yol gösterir. Performans ayarı ipuçları şu anda önerileri PolyBase veri kaynağında yan depoladığınızda, Azure Cosmos DB RU veya Azure SQL DB DTU artırmak için Azure SQL veri ambarı'na veri kopyalama işlemi sırasında kullanmak ister sağlamak gereksiz aşamalı kaldırmak için kaynaklanıyor kopyalama, vb. Performans kuralları ayarlama kademeli olarak de zenginleştirilmiş.

**Örnek: performans ayarlama ipuçları Azure SQL veritabanına kopyalama**

Bu örnekte, yazma işlemleri yavaşlatır yüksek DTU kullanımı havuz Azure SQL DB ulaştığında ADF bildirimi çalışan kopyalama sırasında böylece öneri Azure SQL veritabanı katmanı ile daha fazla DTU artırmaktır.

![Performans ayarlama ipuçları ile izleme kopyalayın](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="incremental-copy"></a>Artımlı kopyalama
Data Factory, artımlı olarak delta veriler kaynak veri deposundan hedef veri deposuna kopyalamak için senaryoları destekler. Bkz: [öğretici: verileri artımlı olarak kopyalama](tutorial-incremental-copy-overview.md).

## <a name="read-and-write-partitioned-data"></a>Bölümlenmiş veri okuma ve yazma
Sürüm 1'de, Azure Data Factory SliceStart/SliceEnd/WindowStart/WindowEnd sistem değişkenlerini kullanarak bölümlenmiş verileri yazma veya okuma desteklenmiyor. Geçerli sürümde, parametrenin değeri bir işlem hattı parametresi ve tetikleyicinin başlangıç saati/zamanlanan saat'ı kullanarak bu davranışı elde edebilirsiniz. Daha fazla bilgi için [okumak veya yazmak nasıl veri bölümlenmiş](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki Hızlı Başlangıç kılavuzlarımız, öğreticilerimiz ve örneklerimizle bakın:

- [Verileri bir konumdan aynı Azure Blob depolama alanındaki başka bir konuma kopyalayın.](quickstart-create-data-factory-dot-net.md)
- [Verileri Azure Blob depolama alanından Azure SQL veritabanı'na kopyalayın.](tutorial-copy-data-dot-net.md)
- [Verileri şirket içi SQL Server'dan Azure'a kopyalama](tutorial-hybrid-copy-powershell.md)
