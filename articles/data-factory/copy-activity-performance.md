---
title: "Etkinlik performans ve ayarlama Kılavuzu Azure Data Factory kopyalama | Microsoft Docs"
description: "Kopyalama etkinliği kullandığınızda, Azure Data factory'de veri taşımayı performansını etkileyen anahtar Etkenler hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: jingwang
ms.openlocfilehash: 841e053418dedb6b41262d1277ab4bdc9d4800c6
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Etkinlik performans ve ayarlama Kılavuzu kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-activity-performance.md)
> * [Sürüm 2 - Önizleme](copy-activity-performance.md)


Azure Data Factory kopyalama etkinliği çözümü bir birinci sınıf güvenli, güvenilir ve yüksek performanslı veri yükleme sunar. Zengin bulut çeşitli her gün terabayt veri onlarca kopyaya sağlar ve şirket içi veri depolarına. Üstün hızlı veri performans yükleme anahtarıdır odaklanmak çekirdek "büyük veri" sorunu emin olmak için: Gelişmiş analiz çözümleri oluşturmak ve tüm bu verilerden ayrıntılı Öngörüler alma.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [kopyalama etkinliği performans veri fabrikası sürüm 1](v1/data-factory-copy-activity-performance.md).

Azure veri depolama ve veri ambarı çözümleri Kurumsal düzeyde bir dizi sağlar ve yüksek oranda iyileştirilmiş bir veri yapılandırmak ve ayarlamak kolay deneyimi yükleme kopyalama etkinliği sunar. Yalnızca tek kopyalama etkinliği ile elde edebilirsiniz:

* Verileri yükleme **Azure SQL Data Warehouse** adresindeki **1,2 GB/sn**.
* Verileri yükleme **Azure Blob Depolama** adresindeki **1.0 GB/sn**
* Verileri yükleme **Azure Data Lake Store** adresindeki **1.0 GB/sn**

Bu makalede açıklanır:

* [Performans referans numaraları](#performance-reference) desteklenen projenizi; planlamanıza yardımcı olması için kaynak ve havuz veri depoları
* Kopya işleme dahil olmak üzere farklı senaryolarda artırabilir özellikleri [veri taşıma birimleri bulut](#cloud-data-movement-units), [paralel kopyalama](#parallel-copy), ve [kopyalama hazırlanan](#staged-copy);
* [Performans ayarlama yönergeleri](#performance-tuning-steps) performans ve kopyalama performansını etkileyebilir anahtar Etkenler ayarlama konusunda.

> [!NOTE]
> Genel kopyalama etkinliği ile bilmiyorsanız, bkz: [kopyalama etkinliği genel bakış](copy-activity-overview.md) bu makaleyi okumadan önce.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru olarak aşağıdaki tabloyu kopyalama üretilen iş sayısını gösterir. **MB/sn cinsinden** verilen kaynak ve havuz çiftleri için **çalıştırmak tek kopyalama etkinliğinde** şirket içi testlere dayanmaktadır. Karşılaştırma için ayrıca farklı ayarlarını gösterir [bulut veri taşıma birimleri](#cloud-data-movement-units) veya [Self-hosted tümleştirmesi çalışma zamanı ölçeklenebilirlik](concepts-integration-runtime.md#self-hosted-integration-runtime) (birden çok düğümler) kopyalama performansına yardımcı olabilir.

![Performans Matrisi](./media/copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>Azure Data Factory sürüm 2, kopya etkinliği Azure bir tümleştirme Çalışma Zamanı Modülü üzerinde çalıştırıldığında en az izin verilen bulut veri taşıma birimidir iki. Belirtilmezse, varsayılan veri taşıma birimleri kullanıldığına bakın [bulut veri taşıma birimleri](#cloud-data-movement-units).

Dikkat edilecek noktalar:

* Verimlilik, aşağıdaki formül kullanılarak hesaplanır: [verilerin boyutu okuma kaynağından] / [kopyalama çalışma süresini etkinliği].
* Tabloda yer alan performans başvuru numaralarını kullanarak ölçülen [TPC-H](http://www.tpc.org/tpch/) veri kümesi bir tek kopyalama etkinliğinde çalıştırın.
* Azure veri depolarında kaynak ve havuz aynı Azure bölgesinde ' dir.
* Karma kopyalama şirket içi ve bulut arasında veri depolarına, her Self-hosted tümleştirmesi Çalışma Zamanı Modülü düğüme belirtimi aşağıda ile veri deposundan ayrı bir makinede çalışıyordu. Tek bir etkinliğin çalıştırırken kopyalama işlemi yalnızca küçük bir bölümü test makinenin CPU, bellek veya ağ bant genişliği tüketilen.
    <table>
    <tr>
        <td>CPU</td>
        <td>32 2.20 GHz Intel Xeon E5-2660 v2 çekirdek</td>
    </tr>
    <tr>
        <td>Bellek</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Ağ</td>
        <td>Internet arabirimi: 10 GB/sn; intranet arabiriminde: 40 GB/sn</td>
    </tr>
    </table>


> [!TIP]
> Bulut Bulut kopyalama etkinliği çalıştırmak için 32 olan maksimum DMUs izin varsayılandan daha fazla veri taşıma birimleri (DMUs) kullanarak yüksek verimlilik elde edebilirsiniz. Örneğin, 100 DMUs ile Azure Blob kopyalama verileri Azure Data Lake Store içine elde edebilirsiniz **1.0GBps**. Bkz: [bulut veri taşıma birimleri](#cloud-data-movement-units) bu özellik ve desteklenen senaryo hakkındaki ayrıntılar için bölüm. Kişi [Azure Destek](https://azure.microsoft.com/support/) daha fazla DMUs istemek için.

## <a name="cloud-data-movement-units"></a>Bulut veri taşıma birimleri

A **bulut veri taşıma birimi (DMU)** veri fabrikası'nda tek bir birimi (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçüdür. **DMU yalnızca uygulanır [Azure tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)**, ama [Self-hosted tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime).

**Kopyalama etkinliği çalıştırmak güçlendirmeniz en az bulut veri taşıma birimleri iki olur.** Belirtilmezse, aşağıdaki tabloda farklı kopyalama senaryosunda kullanılan varsayılan DMUs listelenmektedir:

| Kopyalama senaryosu | Hizmeti tarafından belirlenen varsayılan DMUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | 4 ile sayısı ve dosya boyutuna bağlı olarak 32 arasında. |
| Diğer tüm kopyalama senaryoları | 4 |

Bu varsayılanı geçersiz kılmak için için bir değer belirtin **cloudDataMovementUnits** şekilde özelliği. **İzin verilen değerler** için **cloudDataMovementUnits** özelliği olan 2, 4, 8, 16 ve 32. **Bulut DMUs gerçek sayısını** eşit veya bu değerden azsa yapılandırılan, veri deseni bağlı olarak, kopyalama işlemini çalışma zamanında kullanır. Daha fazla birimi belirli kopya kaynak ve havuz için yapılandırdığınızda alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

Bir etkinlik izlerken çıktı kopyalama etkinliğinde çalıştırın her kopya için gerçekten kullanılan bulut veri taşıma birimleri görebilirsiniz. Ayrıntıları öğrenmek [etkinliğini izleme kopyalama](copy-activity-overview.md#monitoring).

> [!NOTE]
> Daha fazla bulut DMUs için daha yüksek işleme gerekiyorsa başvurun [Azure Destek](https://azure.microsoft.com/support/). 8 ayarlama ve yukarıdaki şu anda yalnızca çalışır, **birden çok Blob Depolama/Data Lake Store/Amazon S3/bulut FTP/bulut SFTP için başka bir bulut veri depolarına kopyalayın**.
>

**Örnek:**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```

### <a name="cloud-data-movement-units-billing-impact"></a>Bulut veri taşıma birimleri faturalama etkisi

Bunun **önemli** tabanlı kopyalama işlemi toplam zamanında ücretlendirilen unutmayın. Veri taşıma için toplam süreyi, faturalandırılır DMUs süresi toplamıdır. Bir kopyalama işi bir saat iki bulut birimleriyle çekmek için kullanılan ve şimdi sekiz bulut birimleri ile 15 dakika sürer, genel fatura neredeyse aynı kalır.

## <a name="parallel-copy"></a>Paralel kopyalama

Kullanabileceğiniz **parallelCopies** kullanmak için kopyalama etkinliği istediğiniz paralellik belirtmek için özelliği. Bu özelliğin kaynağınızdan okuma veya paralel, havuz veri depolarında yazma kopyalama etkinliği içinde iş parçacığı sayısı olarak düşünebilirsiniz.

Her kopya Çalıştır etkinliği için veri fabrikası veri depolamak ve hedef veri depolamak kaynaktan veri kopyalamak için kullanılacak paralel kopya sayısını belirler. Kullandığı paralel kopya varsayılan sayısını kaynak ve kullandığınız havuz türüne bağlıdır:

| Kopyalama senaryosu | Hizmeti tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı depoları arasında veri kopyalama |1 ile 64 arasında. Dosyaları ve iki bulut veri depolarını veya Self-hosted tümleştirmesi çalışma zamanı makinenin fiziksel yapılandırması arasında veri kopyalamak için kullanılan bulut veri taşıma birimleri (DMUs) sayıda boyutuna bağlıdır. |
| Tüm kaynak veri deposundan Azure Table depolama alanına veri kopyalama |4 |
| Diğer tüm kopyalama senaryoları |1 |

Genellikle, varsayılan davranışı en iyi verimlilik vermesi gerekir. Ancak, verilerinizi barındıran makineler üzerindeki yükü denetlemek için depolar veya kopya performansı ayarlamak için varsayılan değeri geçersiz kılmak ve için bir değer belirtmek seçebilirsiniz **parallelCopies** özelliği. Değer 1'e eşit veya sıfırdan büyük bir tamsayı olmalıdır. Çalışma zamanında, en iyi performans için kopyalama etkinliği ayarladığınız değerine eşit veya daha küçük bir değer kullanır.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

Dikkat edilecek noktalar:

* Veri depoları, dosya tabanlı arasında kopyaladığınızda **parallelCopies** dosya düzeyinde paralellik belirleyin. Tek bir dosyada Öbekleme altında otomatik ve şeffaf şekilde olacağını ve verileri paralel ve parallelCopies resme yüklemek için belirtilen kaynak veri deposu türü için en iyi uygun öbek boyutu kullanmak için tasarlanmıştır. Gerçek çalışma zamanında kopyalama işlemi için veri taşıma hizmeti kullanan paralel kopyaları sahip dosyaların sayısı en çok sayısıdır. Kopyalama davranışını ise **mergeFile**, kopyalama etkinliği, dosya düzeyinde paralellik avantajlarından alamaz.
* İçin bir değer belirtirseniz **parallelCopies** özelliği, kopyalama etkinliği tarafından Örneğin, karma kopyalama için yetkilendirilmiş yük arttıkça, kaynak ve havuz veri depoları ve Self-Hosted tümleştirmesi çalışma zamanı için göz önünde bulundurun. Bu, özellikle birden çok etkinlikleri veya aynı veri deposu karşı çalıştırmak aynı etkinliklerin eşzamanlı çalışır olduğunda gerçekleşir. Veri deposu ya da Self-hosted tümleştirmesi çalışma zamanı yük ile doludur fark ederseniz, azaltma **parallelCopies** yük hafifletmek için değer.
* Dosya tabanlı depoları için dosya tabanlı olmayan depoları veri kopyalama, veri taşıma Hizmeti'nde yoksayar **parallelCopies** özelliği. Paralellik belirtilmiş olsa bile, bu durumda uygulanmıyor.
* **parallelCopies** için resme olan **cloudDataMovementUnits**. Önceki tüm bulut verilerini taşıma birimler arasında sayılır.

## <a name="staged-copy"></a>Hazırlanan kopyalama

Bir havuz veri deposu için bir kaynak veri deposundan verileri kopyaladığınızda, geçici bir hazırlama deposu olarak Blob storage kullanmayı seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

- **PolyBase aracılığıyla SQL veri ambarında verileri çeşitli veri depoları alma istediğiniz**. SQL veri ambarı PolyBase büyük miktarda veri SQL Data Warehouse'a veri yükleme için yüksek verimlilik mekanizması olarak kullanır. Ancak, veri kaynağını Blob storage veya Azure Data Lake Store olmalıdır ve ek ölçütleri karşılaması gerekir. Blob Depolama dışındaki bir veri deposu ya da Azure Data Lake Store'a veri yükleme zaman verileri aracılığıyla geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda, veri fabrikası PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümleri gerçekleştirir. Ardından verileri SQL Data Warehouse'a verimli bir şekilde yüklemek için PolyBase kullanır. Daha fazla bilgi için bkz: [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Bazen bir karma veri taşımayı gerçekleştirmek için biraz zaman alır (yani, bir şirket içi kopyalamak için verileri bir bulut veri deposuna depolamak) bir yavaş ağ bağlantısı üzerinden**. Performansı artırmak için böylece bulutta hazırlama veri deposuna veri taşıyın ve ardından hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri sıkıştırmasını açmak için daha az zaman alır içi verileri sıkıştırmak için hazırlanmış kopyayı kullanabilirsiniz.
- **Bağlantı noktası 80 dışındaki bağlantı noktaları açın ve kurumsal BT ilkeleri nedeniyle, Güvenlik Duvarı'nda 443 numaralı bağlantı istemediğiniz**. Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL Data Warehouse havuz için bir şirket içi veri deposundan veri kopyaladığınızda, hem Windows Güvenlik Duvarı hem de kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimi etkinleştirmeniz gerekir. Bu senaryoda, hazırlanmış kopyalama ilk HTTP veya HTTPS bağlantı noktası 443 üzerinden örneği hazırlama bir Blob depolama alanına veri kopyalamak için Self-hosted Integration zamanının yararlanmak sonra verileri Blob Depolama hazırlamadan SQL Database veya SQL Data Warehouse yüklemek. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopyalama çalışır

Hazırlama özelliği etkinleştirdiğinizde, ilk veri kaynağı veri deposundan hazırlama Blob depolama alanına kopyalanır (Getir kendi). Ardından, verileri hazırlama veri deposundan havuz veri deposuna kopyalanır. Veri Fabrikası iki aşamalı akış sizin için otomatik olarak yönetir. Veri taşıma işlemi tamamlandıktan sonra veri fabrikası aynı zamanda hazırlama depolama biriminden geçici verileri temizler.

![Hazırlanan kopyalama](media/copy-activity-performance/staged-copy.png)

Hazırlama depolama kullanarak veri taşıma etkinleştirdiğinizde, verileri veri kaynağına veri deposundan bir geçiş veya hazırlama veri deposuna taşımadan önce sıkıştırılır ve bir geçiş verilerini taşıma veya veri deposu havuz veri deposuna hazırlama önce sıkıştırması açılmış isteyip istemediğinizi belirtebilirsiniz.

Şu anda, bir hazırlama deposu kullanarak iki şirket içi veri depoları arasında veri kopyalanamıyor.

### <a name="configuration"></a>Yapılandırma

Yapılandırma **enableStaging** bir hedef veri deposuna yükleme önce Blob depolama alanına aşamalı için verileri isteyip istemediğinizi belirtmek için kopyalama etkinliği ayarlama. Ayarladığınızda **enableStaging** için `TRUE`, sonraki tabloda listelenen ek özellikleri belirtin. Yoksa, depolama paylaşılan erişim imzası bağlantılı hizmeti hazırlama için veya bir Azure Storage oluşturmak gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| **enableStaging** |Veri deposu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| **linkedServiceName** |Adını belirtin bir [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) bağlı bir geçici hazırlama deposu olarak kullanmak depolama örneğinin başvurduğu hizmeti. <br/><br/> PolyBase aracılığıyla SQL veri ambarında verileri yüklemek için depolama ile paylaşılan erişim imzası kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, ne zaman **enableStaging** TRUE olarak ayarlayın |
| **yol** |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolunu belirtin. Bir yol belirtmezseniz, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici verilerin belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| **Aracılığıyla** |Hedefe kopyalamadan önce verilerin sıkıştırılmasının gerekli olup olmadığını belirtir. Bu ayar aktarılan veri hacmini azaltır. |False |Hayır |

Kopyalama etkinliği yukarıdaki tabloda açıklanan özelliklere sahip bir örnek tanımı aşağıda verilmiştir:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Faturalama etkisi kopyalama hazırlanan

Temel alınarak iki adımı ücretlendirilen: süresi kopyalama ve kopyalama türü.

* (Verileri başka bir bulut veri deposu, her iki aşamaları Azure tümleştirmesi çalışma zamanı tarafından yetkilendirilmiş bir bulut veri deposundan kopyalayarak) bulut kopyalama sırasında Hazırlama [kopyalama sürenin toplamı adım 1 ve 2. adım] [bulut kopya Birim Fiyat] x kullandığınızda ücretlendirilirsiniz.
* Kullandığınızda (verileri bir bulut veri deposu Self-hosted tümleştirmesi çalışma zamanı tarafından yetkilendirilmiş bir aşamada bir şirket içi veri deposundan kopyalayarak) karma kopyalama sırasında hazırlama, [karma kopyalama süresince] ücretlendirilen x [karma kopya Birim Fiyat] + [bulut kopyalama süre] x [bulut kopya Birim Fiyat].

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları

Veri Fabrikası hizmetiniz kopyalama etkinliği ile performansı ayarlamak için şu adımları uygulamanız öneririz:

1. **Bir taban çizgisi oluşturma**. Geliştirme aşamasında, temsili veri örneği karşı kopyalama etkinliği kullanarak hattınızı sınayın. Aşağıdaki performans özellikleri ve yürütme ayrıntıları toplamak [etkinliğini izleme kopyalama](copy-activity-overview.md#monitoring).

2. **Tanılama ve performansı en iyi duruma**. Gözlemlemek performans beklentilerinizi karşılamıyorsa, performans sorunlarını tanımlamak gerekir. Ardından, kaldırmak veya performans etkisini azaltmak için performansı en iyi duruma. Performans Tanılama ilgili tam açıklama için bu makalenin kapsamı dışındadır olsa da, bazı ortak noktalar şunlardır:

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Bulut veri taşıma birimleri](#cloud-data-movement-units)
     * [Hazırlanan kopyalama](#staged-copy)
     * [Kendini barındıran tümleştirmesi çalışma zamanı ölçeklenebilirlik](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Kendini barındıran tümleştirmesi çalışma zamanı](#considerations-for-self-hosted-integration-runtime)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Seri hale getirme ve seri durumdan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşlemesi](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)

3. **Tüm Veri kümenizi Yapılandırması**. Yürütme sonuçları ve performans ile memnun kaldığınızda, tanım ve tüm veri kümenizi karşılamak üzere ardışık düzen genişletebilirsiniz.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Kendini barındıran tümleştirmesi çalışma zamanı için ilgili önemli noktalar

Kopyalama etkinliği üzerinde Self-hosted tümleştirmesi Çalışma Zamanı Modülü yürütülürse, aşağıdakilere dikkat edin:

**Kurulum**: ana bilgisayar tümleştirme çalışma zamanı için ayrılmış bir makine kullanmanızı öneririz. Bkz: [Self-hosted tümleştirmesi çalışma zamanı kullanmak için dikkat edilecek noktalar](concepts-integration-runtime.md).

**Ölçeği genişletme**: tek bir mantıksal Self-hosted tümleştirmesi çalışma zamanı bir veya daha fazla düğüm ile aynı anda aynı anda birden çok kopya etkinliği çalıştırır hizmet verebilir. İçin çok sayıda eş zamanlı kopyalama etkinliği çalışır veya büyük miktarda veri kopyalamak için karma veri taşıma üzerinde ağır gereksiniminiz varsa göz önünde bulundurun [Self-hosted tümleştirmesi çalışma zamanı genişletme](create-self-hosted-integration-runtime.md#high-availability-and-scalability) daha fazla kaynak sağlamak amacıyla kopya güçlendirmeniz.

## <a name="considerations-for-the-source"></a>Kaynak için ilgili önemli noktalar

### <a name="general"></a>Genel

Temel alınan veri deposu veya onu karşı çalışan diğer iş yükleri tarafından doludur değil olduğunu unutmayın.

Microsoft veri depoları için bkz: [izleme ve konuları ayarlama](#performance-reference) özgü veri depoları ve veri performans özellikleri depolamak, yanıt sürelerini en aza indirmek ve üretilen işi en üst düzeye anlamanıza yardım.

* Verileri kopyalarsanız **SQL Data Warehouse için Blob depolama biriminden**, kullanmayı **PolyBase** performansını artırma. Bkz: [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için.
* Verileri kopyalarsanız **HDFS Azure Blob/Azure Data Lake Store için gelen**, kullanmayı **Distcp'yi** performansını artırma. Bkz: [kullanım HDFS verileri kopyalamak için Distcp'yi](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) Ayrıntılar için.
* Verileri kopyalarsanız **Redshift Azure SQL veri ambarı/Azure BLob/Azure Data Lake Store için gelen**, kullanmayı **UNLOAD** performansını artırma. Bkz: [Amazon Redshift verileri kopyalamak için kullanım UNLOAD](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) Ayrıntılar için.

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Ortalama dosya boyutu ve dosya sayısı**: kopyalama etkinliği, aynı anda bir veri dosyası aktarır. İle aynı taşınacak veri miktarı, genel üretilen işi verileri her dosya için önyükleme aşaması nedeniyle birkaç büyük dosyalar yerine pek çok küçük dosya oluşuyorsa düşüktür. Bu nedenle, mümkünse, küçük dosyalar daha yüksek verimlilik elde etmek için daha büyük dosyalar birleştirin.
* **Dosya biçimi ve sıkıştırma**: performansını artırmak diğer yolları için bkz: [seri hale getirme ve seri durumundan çıkarma için ilgili önemli noktalar](#considerations-for-serialization-and-deserialization) ve [sıkıştırma için ilgili önemli noktalar](#considerations-for-compression) bölümler.

### <a name="relational-data-stores"></a>İlişkisel veri depoları

* **Veri deseni**: Tablo şemanızı kopyalama verimlilik etkiler. Büyük satır boyutu küçük satır boyutu, aynı miktarda veri kopyalamak için daha iyi performans sağlar. Veritabanında daha az sayıda satır içeren veri daha az toplu daha verimli bir şekilde alabilir nedenidir.
* **Sorgu veya saklı yordam**: sorgu veya saklı yordam verileri daha verimli bir şekilde getirmek için kopyalama etkinliği kaynağında belirttiğiniz mantığını en iyi duruma getirme.

## <a name="considerations-for-the-sink"></a>Havuz için ilgili önemli noktalar

### <a name="general"></a>Genel

Temel alınan veri deposu veya onu karşı çalışan diğer iş yükleri tarafından doludur değil olduğunu unutmayın.

Microsoft veri depoları için başvurmak [izleme ve konuları ayarlama](#performance-reference) özgü verileri depolar. Bu konularda veri deposu performans özellikleri ve yanıt sürelerini en aza indirmek ve üretilen işi en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

* Verileri kopyalarsanız **SQL Data Warehouse için Blob depolama biriminden**, kullanmayı **PolyBase** performansını artırma. Bkz: [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için.
* Verileri kopyalarsanız **HDFS Azure Blob/Azure Data Lake Store için gelen**, kullanmayı **Distcp'yi** performansını artırma. Bkz: [kullanım HDFS verileri kopyalamak için Distcp'yi](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) Ayrıntılar için.
* Verileri kopyalarsanız **Redshift Azure SQL veri ambarı/Azure BLob/Azure Data Lake Store için gelen**, kullanmayı **UNLOAD** performansını artırma. Bkz: [Amazon Redshift verileri kopyalamak için kullanım UNLOAD](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) Ayrıntılar için.

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Kopyalama davranışı**: farklı dosya tabanlı veri deposundan verileri kopyalarsanız, kopyalama etkinliği ile üç seçenek vardır **copyBehavior** özelliği. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyaları birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performans yüküne sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: bkz [seri hale getirme ve seri durumundan çıkarma için ilgili önemli noktalar](#considerations-for-serialization-and-deserialization) ve [sıkıştırma için ilgili önemli noktalar](#considerations-for-compression) performansını artırmak diğer yolları için bölümler.

### <a name="relational-data-stores"></a>İlişkisel veri depoları

* **Davranış kopyalama**: özellikleri için ayarlanmış bağlı olarak **sqlSink**, kopyalama etkinliği farklı şekillerde hedef veritabanına veri yazar.
  * Varsayılan olarak, veri taşıma hizmeti kullanan veri eklemek için toplu kopyalama API en iyi performans sağlayan moda ekleyin.
  * Saklı yordam havuzunda yapılandırırsanız, veritabanı veri bir satır yerine bir anda bir toplu yükleme olarak uygulanır. Performansı önemli ölçüde bırakır. Veri kümenizi uygulanabilir olduğunda büyükse, kullanmaya geçmeniz önerilir **preCopyScript** özelliği.
  * Yapılandırırsanız, **preCopyScript** özelliği her kopyalama etkinliği için çalıştırabilir, hizmet betik tetikler ve sonra toplu kopyalama API veri eklemek için kullanın. Örneğin, tüm tablo en son verilerle üzerine yazmak için ilk önce toplu yeni veri kaynağından yükleme önce tüm kayıtları silmek için bir betik belirtebilirsiniz.
* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemasını kopyalama verimlilik etkiler. Veritabanı daha verimli bir şekilde daha az veri toplu yürütme çünkü aynı miktarda veri kopyalayın, bir büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, toplu bir dizi veri ekler. Kullanarak bir toplu işlemde satır sayısını ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satırları varsa, ayarlayabileceğiniz **writeBatchSize** daha az toplu iş yükü ve daha yüksek verimlilik yararlanmak için daha yüksek bir değere sahip özelliği. Verilerinizin satır boyutu büyükse, artırmanız dikkatli olun **writeBatchSize**. Yüksek bir değer veritabanı aşırı yüklemesi nedeniyle bir kopyalama hatası neden.

### <a name="nosql-stores"></a>NoSQL depoları

* İçin **tablo depolama**:
  * **Bölüm**: veri araya eklemeli bölümlere önemli ölçüde yazma performansı düşürür. Veri kaynağınızı bölüm anahtarı tarafından Sırala veriler verimli bir şekilde bir bölüme sonra başka bir eklenir veya tek bir bölüm için veri yazmak için mantığı ayarlayın.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için ilgili önemli noktalar

Giriş veri kümesini veya çıkış veri kümesi bir dosyadır seri hale getirme ve seri durumdan çıkarma ortaya çıkabilir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](supported-file-formats-and-compression-codecs.md) kopyalama etkinliği tarafından desteklenen dosya biçimleri hakkında ayrıntılarla.

**Davranış kopyalama**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Aynı veya hiçbir dosya biçimi ayarları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda yürüten bir **ikili kopyalama** herhangi bir seri hale getirme veya seri durumdan çıkarma olmadan. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek verimlilik bakın.
  * Ne zaman giriş ve çıkış veri kümeleri hem metin biçimi ve yalnızca kodlama olan türü farklı olan, veri taşıma hizmeti yalnızca kodlama dönüştürme yapar. Tüm serileştirme yapmaz ve seri durumdan yükü ikili kopyasını karşılaştırıldığında bazı performans neden olur.
  * Farklı dosya biçimleri veya sınırlayıcıları gibi farklı yapılandırmaları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda akış, dönüştürme ve belirttiğiniz çıkış biçimine serileştirmek için kaynak verileri seri durumdan çıkarır. Bu işlem yükü diğer senaryolar için kıyasla daha önemli bir performans sonuçlanır.
* Dosyaları (örneğin, bir dosya tabanlı depolama alanından bir ilişkisel deposuna) dosya tabanlı olmayan bir veri deposu/gruptan kopyaladığınızda, seri hale getirme veya seri durumdan çıkarma adım gereklidir. Bu adım, önemli performans ek yükü sonuçlanır.

**Dosya biçimi**: seçtiğiniz dosya biçimi kopyalama performansını etkileyebilir. Örneğin, Avro meta verilerle depolar sıkıştırılmış bir ikili biçimi ' dir. İşleme ve sorgulama için Hadoop ekosistemindeki geniş bir desteğe sahiptir. Ancak, Avro seri hale getirme ve metin biçimine kıyasla daha düşük kopyalama üretilen iş sonuçları seri durumdan için daha pahalı olması. Seçtiğiniz dosya biçimi işleme akışı boyunca bütünsel olun. Hangi veri form ile başlangıç, kaynak veri depolarını veya dış sistemlerden ayıklanacak depolanır; Depolama, analitik işleme ve sorgulama için en iyi biçimi; ve hangi biçiminde raporlama ve görselleştirme araçları için data Mart içinde veri verilmesi. Bazen için uygun olmayan bir dosya biçimi okuma ve yazma performansı genel analitik işlemi göz önüne aldığınızda iyi bir seçimdir olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma için ilgili önemli noktalar

Giriş veya çıkış Veri kümenizi bir dosyası olduğunda, hedef veri yazar gibi sıkıştırma veya açma işlemi gerçekleştirmek için kopyalama etkinliği ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir kolaylığını yapmak ve CPU. İşlem kaynakları ek veri maliyetleri sıkıştırma. Ancak buna karşılık, ağ g/ç ve depolama azaltır. Verilerinizi bağlı olarak artırma genel kopyalama üretilen işi de görebilirsiniz.

**Codec**: her bir sıkıştırma codec avantajları vardır. Örneğin, bzıp2 düşük kopyalama üretilen işi olan, ancak işleme için bölmek için bzıp2 ile en iyi Hive sorgu performansı alırsınız. Gzip en dengeli bir seçenektir ve en sık kullanılır. Uçtan uca senaryonuza uygun codec seçin.

**Düzey**: her bir sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: hızlı sıkıştırılmış ve verimli sıkıştırılmış. Sonuç dosyası en iyi şekilde sıkıştırılmaz olsa bile Hızlı sıkıştırılmış seçeneği veri mümkün olan en kısa sürede sıkıştırır. En iyi şekilde sıkıştırılmış seçenek sıkıştırmayı daha fazla zaman harcayan ve en az miktarda veri ortaya çıkarır. Çalışmanızın daha iyi genel performans sağlayan görmek için her iki seçenek test edebilirsiniz.

**Önemli bir unsur**: büyük miktarda bir şirket içi depolama ve bulut arasında veri kopyalamak için kullanmayı [kopyalama hazırlanan](#staged-copy) sıkıştırma ile. Geçici depolama kullanarak şirket ağınıza ve Azure hizmetlerinizi bant genişliği sınırlayıcı faktördür ve girdi veri kümesi hem de çıkış veri kümesi sıkıştırılmamış formunda olacak şekilde istediğinizde yararlıdır.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için ilgili önemli noktalar

Ayarlayabileceğiniz **columnMappings** tüm harita veya çıktı sütunları için bir giriş sütun alt kümesini kopyalama etkinliğinde özelliği. Veri Taşıma hizmeti veri kaynağından okuduktan sonra havuz için verileri yazar önce sütun eşlemesi verileri gerçekleştirmek gerekir. Bu ek işleme kopyalama performansını düşürür.

Kaynak veri deposu sorgulanabilir ise, örneğin, SQL Database veya SQL Server gibi ilişkisel bir mağaza ise ya da bir NoSQL deposu Table storage veya Azure Cosmos DB gibi ise sütun filtreleme dağıtmaya ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özelliği. Veri Taşıma hizmeti çok daha verimli olduğu kaynak veri deposundan verileri okurken bu şekilde, projeksiyon oluşur.

' Dan daha fazla bilgi edinin [kopyalama etkinliği şema eşleme](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Diğer konular

Kopyalamak istediğiniz verilerin boyutu büyükse, daha fazla bölüm veri ve her kopya etkinliği çalıştırmak için verilerin boyutunu küçültmek için daha sık çalıştırmak için kopyalama etkinliği zamanlamak için iş mantığınızı ayarlayabilirsiniz.

Veri kümeleri ve aynı anda aynı veri deposuna bağlanmak Data Factory gerektiren kopyalama etkinliklerin sayısını dikkatli olun. Çok sayıda eş zamanlı kopyalama iş bir veri deposu kısıtlama ve performans, kopyalama işi iç yeniden denemeleri ve bazı durumlarda, yürütme hataları neden olabilir.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: bir şirket içi SQL Server bir kopyasından Blob Depolama

**Senaryo**: Blob Depolama birimine CSV biçiminde bir şirket içi SQL Server'dan veri kopyalamak için bir işlem hattı oluşturulmuştur. Kopyalama işini hızlandırmak için CSV dosyaları bzıp2 biçime sıkıştırılmış.

**Test ve analiz**: performans Kıyaslama çok daha yavaş olduğu kopyalama etkinliği verimini değerinden 2 MB/sn, açık.

**Performans Analizi ve ayarlama**: performans sorunu gidermek için nasıl veri işlenen taşınır ve konumundaki bakalım.

1. **Veri Okuma**: tümleştirme çalışma zamanı SQL Server'a bağlantı açar ve sorgu gönderir. SQL Server Integration çalışma zamanı intranet üzerinden veri akışı göndererek yanıt verir.
2. **Seri hale getirmek ve verileri sıkıştırmak**: tümleştirme çalışma zamanı CSV biçiminde veri akışa serileştirir ve bir bzıp2 akış verileri sıkıştırır.
3. **Veri yazma**: tümleştirme çalışma zamanı bzıp2 akış Internet üzerinden Blob depolama alanına yükler.

Gördüğünüz gibi veri okunduğunu işlenir ve bir akış sıralı şekilde taşındı: SQL Server > LAN > tümleştirmesi çalışma zamanı > WAN > Blob Depolama. **Genel performansı göre en düşük işleme ardışık düzeni geçişli**.

![Veri akışı](./media/copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla bilgi için aşağıdaki etkenleri performans düşüklüğü neden olabilir:

* **Kaynak**: SQL Server kendisini nedeniyle ağır yük düşük işleme sahiptir.
* **Tümleştirme çalışma zamanı'kendi kendini barındıran**:
  * **LAN**: tümleştirme çalışma zamanı gölgeden uzak SQL Server makinesinde bulunur ve düşük bant genişlikli bağlantısı vardır.
  * **Tümleştirme çalışma zamanı**: tümleştirme çalışma zamanı, aşağıdaki işlemleri gerçekleştirmek için yük kısıtlamalarını ulaştı:
    * **Seri hale getirme**: veri akışı CSV biçiminde seri hale getirme yavaş işleme sahiptir.
    * **Sıkıştırma**: yavaş sıkıştırma codec (2.8 MB/sn Çekirdek i7 ile olan Örneğin, bzıp2,) seçtiniz.
  * **WAN**: şirket ağına ve Azure hizmetlerinizi arasında bant genişliği düşükse (örneğin, T1 1,544 kbps; = T2 6,312 kbps =).
* **Havuz**: Blob depolama alanına sahip düşük işleme. (Bu senaryoda en az 60 MB/sn SLA'sını güvence altına alır çünkü düşüktür.)

Bu durumda, bzıp2 veri sıkıştırma tüm ardışık yavaşlamasının. Gzip sıkıştırma codec için geçiş bu performans sorunu kolaylaştırır.

## <a name="reference"></a>Başvuru

İzleme ve bazı desteklenen veri depoları için başvuru ayarlama performans şöyledir:

* Azure Storage (Blob Depolama ve tablo depolama dahil): [Azure Storage ölçeklenebilirlik hedefleri](../storage/common/storage-scalability-targets.md) ve [Azure Storage performans ve ölçeklenebilirlik Yapılacaklar listesi](../storage/common/storage-performance-checklist.md)
* Azure SQL Database: Yapabilecekleriniz [performansını izlemek](../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimi (DTU) yüzde denetleyin
* Azure SQL Data Warehouse: Veri ambarı birimlerini (Dwu'lar); kendi yeteneği ölçülür bkz: [Yönet işlem güç Azure SQL Data warehouse'da (genel bakış)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Azure Cosmos veritabanı performans düzeyleri](../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [İzleyici ve performans ayarlama](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [dosya sunucuları için performans ayarlama](https://msdn.microsoft.com/library/dn567661.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopya etkinliği genel bakış](copy-activity-overview.md)
- [Kopya etkinliği şema eşleme](copy-activity-schema-and-type-mapping.md)
- [Etkinlik hata toleransı kopyalayın](copy-activity-fault-tolerance.md)
