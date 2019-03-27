---
title: Azure Data Factory'de etkinlik performansı ve ayarlama Kılavuzu kopyalama | Microsoft Docs
description: Kopyalama etkinliği kullandığınızda, Azure Data factory'de veri taşımayı performansını etkileyen anahtar Etkenler hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: jingwang
ms.openlocfilehash: 47b9ede2d529f78b14c21f53c6cd18ed691a3df3
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445838"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Etkinlik performansı ve ayarlama Kılavuzu kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-activity-performance.md)
> * [Geçerli sürüm](copy-activity-performance.md)


Azure Data Factory kopyalama etkinliği, çözüm yüklenirken birinci sınıf bir güvenli, güvenilir ve yüksek performanslı veri sunar. Birçok farklı bulut her gün terabaytlarca veriyi onlarca kopyasını sağlar ve şirket içi veri depoları. İnanılmaz derecede hızlı veri yükleme performansını anahtarıdır çekirdek "büyük veri" soruna odaklanmak emin olmak için: Gelişmiş analiz çözümleri oluşturmak ve tüm bu verilerden ayrıntılı Öngörüler alma.

Azure Kurumsal düzeyde veri depolama ve veri ambarı çözümleri sunmaktadır ve kopyalama etkinliği yapılandırmak ve ayarlamak kolaydır deneyimi yüklenirken yüksek oranda iyileştirilmiş bir veri sunar. Yalnızca tek bir kopyalama etkinlikli, elde edebilirsiniz:

* Verileri yükleme **Azure SQL veri ambarı** adresindeki **1,2 GB/sn**.
* Verileri yükleme **Azure Blob Depolama** adresindeki **1,0 GB/sn**
* Verileri yükleme **Azure Data Lake Store** adresindeki **1,0 GB/sn**

Bu makalede açıklanır:

* [Performans başvuru sayıları](#performance-reference) desteklenen; proje planlamanıza yardımcı olması için kaynak ve havuz veri deposu
* Kopyalama aktarım hızı dahil olmak üzere farklı senaryolarda artırabilir özellikleri [veri tümleştirme birimleri](#data-integration-units), [paralel kopyalama](#parallel-copy), ve [kopyalama aşamalı](#staged-copy);
* [Performans ayarlama Kılavuzu](#performance-tuning-steps) performans ve kopyalama performansı etkileyen önemli faktörlerin ayarlama konusunda.

> [!NOTE]
> Genel kopyalama etkinliği ile ilgili bilgi sahibi değilseniz, bkz. [kopyalama etkinliğine genel bakış](copy-activity-overview.md) bu makaleyi okuduktan önce.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru, aşağıdaki tabloda kopyalama aktarım hızı sayısı gösterilir. **MB/sn cinsinden** verilen kaynak ve havuz çiftleri için **tek kopyalama etkinliğinde çalıştırın** şirket içi teste dayanan. Karşılaştırma için ayrıca nasıl farklı ayarları gösterir [veri tümleştirme birimleri](#data-integration-units) veya [şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirlik](concepts-integration-runtime.md#self-hosted-integration-runtime) (birden çok düğüm) kopyalama performansı üzerinde yardımcı olabilir.

![Performans Matrisi](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Azure tümleştirme çalışma zamanını bir kopyalama Etkinliği yürütüldüğünde, en az izin verilen veri tümleştirmesi (eski adıyla veri taşıma birimleri da bilinir) iki birimidir. Belirtilmezse, varsayılan veri tümleştirme kullanılmakta birimlerini görmek [veri tümleştirme birimleri](#data-integration-units).

Dikkat edilecek noktalar:

* Aşağıdaki formülü kullanarak aktarım hızı hesaplanır: [kaynak Okuma boyutu veri] / [kopyalama etkinliği çalıştırma süresi].
* Tablo performans başvuru sayıları ile ölçülür [TPC-H](http://www.tpc.org/tpch/) veri kümesi tek bir kopyalama etkinliğinde çalıştırın. Test dosyaları dosya tabanlı depoları için birden çok 10 GB boyutundaki dosyalarıdır.
* Azure veri depoları, kaynak ve havuz aynı Azure bölgesinde olan.
* Karma kopyalama şirket içi ve bulut arasında veri depoları, her şirket içinde barındırılan Integration Runtime düğümü altındaki belirtimi ile veri deposundan ayrı bir makinede çalışıyordu. Tek bir etkinlik çalıştırırken test makinenin CPU, bellek veya ağ bant genişliği için küçük bir bölümü kopyalama işlemi kullanılan.
    <table>
    <tr>
        <td>CPU</td>
        <td>2.20 GHz Intel Xeon E5-2660 v2 32 çekirdek</td>
    </tr>
    <tr>
        <td>Bellek</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Ağ</td>
        <td>Internet arabirimi: 10 GB/sn; intranet arabiriminde: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> Daha fazla veri tümleştirme birimleri (DIU) kullanarak daha yüksek aktarım hızı elde edebilirsiniz. Örneğin, 100 DIUs ile Azure Blob veri kopyalamayı Azure Data Lake Store içine elde edebileceğiniz **1.0GBps**. Bkz: [veri tümleştirme birimleri](#data-integration-units) bölümü bu özellik ve desteklenen bir senaryo hakkındaki ayrıntılar için. 

## <a name="data-integration-units"></a>Veri tümleştirme birimleri

A **veri tümleştirme birim (DIU)** (eski adıyla bulut verisi taşıma birimi veya DMU olarak bilinir) Data factory'de tek bir birim (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçüdür. **Yalnızca DIU uygulandığı [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime)**, ama [şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime).

**Kopyalama etkinliği çalıştırmasının güçlendirmek için en az veri tümleştirme birimi 2'dir.** Belirtilmezse, aşağıdaki tabloda farklı kopyalama senaryolarında kullanılan varsayılan DIUs listelenmektedir:

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan DIUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | 4 ile sayısı ve dosyaların boyutuna bağlı olarak 32 arasında. |
| Tüm diğer kopyalama senaryolarında | 4 |

Bu varsayılanı geçersiz kılmak için bir değer belirtin. **dataIntegrationUnits** özelliğini aşağıdaki gibi. **İzin verilen değerler** için **dataIntegrationUnits** özelliği **en fazla 256**. **DIUs gerçek sayısını** eşit veya daha az, veri modelini bağlı olarak yapılandırılmış bir değeri, kopyalama işleminin çalışma zamanında kullanır. Özel kopyalama kaynağı ve havuz için daha fazla birimi yapılandırırken alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

Bir etkinlik çalıştırması izlerken çıkış kopyalama etkinliği çalıştırma her kopya için gerçekten kullanılan veri tümleştirme birimleri görebilirsiniz. Ayrıntıları öğrenin [kopyalama etkinliği izleme](copy-activity-overview.md#monitoring).

> [!NOTE]
> DIUs ayarı **4 büyük** şu anda yalnızca çalışır, **birden çok dosyayı Blob Depolama/deposundan veri Lake kopyalamayı depolama/Amazon S3/bulut FTP/bulut SFTP'ye herhangi bir bulut veri depolar.**.
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
            "dataIntegrationUnits": 32
        }
    }
]
```

### <a name="data-integration-units-billing-impact"></a>Veri tümleştirme birimleri faturalama etkisi

Sahip **önemli** tabanlı kopyalama işlemi toplam zamanında ücretlendirilir unutmayın. Veri taşıma toplam süresi faturalandırılırsınız arasında DIUs süresi toplamıdır. Bir kopyalama işi bir saat ile iki bulut birimleri almak için kullanılan ve artık bu sekiz bulut birimiyle 15 dakika sürer, toplam fatura neredeyse aynı kalır.

## <a name="parallel-copy"></a>Paralel kopyalama

Kullanabileceğiniz **parallelCopies** kopyalama etkinliği, kullanmak istediğiniz paralellik belirtmek için özelliği. Bu özellik veri kaynağınızdan okuyabilen veya, havuz veri depolarına paralel yazma kopyalama etkinliği içinde iş parçacığı sayısı olarak düşünebilirsiniz.

Her kopya etkinlik çalıştırma için Data Factory veri depolamak ve için hedef veri deposu kaynak sunucudan veri kopyalamak için paralel kopya sayısını belirler. Varsayılan sayısıyla onu kullanan paralel kaynak ve havuz kullanmakta olduğunuz türüne bağlıdır:

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı depoları arasında veri kopyalama |İki bulut veri deposu ya da şirket içinde barındırılan tümleştirme çalışma zamanı makinenin fiziksel yapılandırması arasında veri kopyalamak için kullanılan dosya ve veri tümleştirme birimi (DIUs) boyutuna bağlıdır. |
| Herhangi bir kaynak veri deposundan Azure tablo depolama alanına veri kopyalama |4 |
| Tüm diğer kopyalama senaryolarında |1 |

> [!TIP]
> Dosya tabanlı depoları arasında veri kopyalama işlemi sırasında varsayılan davranışı (otomatik olarak belirlenir) genellikle size en iyi aktarım hızı. 

Verilerinizi barındıran makinelerin yükünü denetlemek için depolar veya kopyalama performansı ayarlamak için varsayılan değeri geçersiz kılmak ve için bir değer belirtmek seçebilirsiniz **parallelCopies** özelliği. Büyük veya 1'e eşit bir tamsayı değeri olmalıdır. Çalışma zamanında, en iyi performans için ayarladığınız değerine eşit veya daha az olan bir değer kopyalama etkinliği kullanır.

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

* Dosya tabanlı depoları arasında veri kopyalama **parallelCopies** paralellik dosya düzeyinde belirlenir. Tek bir dosyada Öbekleme altındaki otomatik ve şeffaf şekilde olacağını ve paralel ve parallelCopies dikgen verileri yüklemek için en uygun öbek boyutu için belirtilen kaynak veri deposu türü kullanmak için tasarlanmıştır. Gerçek veri taşıma Hizmeti'nde kopyalama işleminin çalışma zamanında kullandığı paralel kopya sayısı sahip olduğunuz dosyaların sayısı, en fazla ' dir. Kopyalama davranışını ise **mergeFile**, kopyalama etkinliği, dosya düzeyinde paralellik avantajlarından alamaz.
* İçin bir değer belirtirseniz **parallelCopies** özelliği, kopyalama etkinliği tarafından Örneğin, karma kopyalama için yetkilendirilirler yük artışı, kaynak ve havuz veri deposu ve şirket içinde barındırılan tümleştirme çalışma zamanı için göz önünde bulundurun. Bu, özellikle birden çok etkinlikler veya aynı veri deposuna karşı çalışan aynı etkinliklerden eş zamanlı çalıştırma olduğunda gerçekleşir. Veri deposu ya da şirket içinde barındırılan tümleştirme çalışma zamanı yük ile doludur fark ederseniz, azaltma **parallelCopies** yükle hafifletmek için değer.
* Dosya tabanlı depoları için dosya tabanlı olmayan depolarından verileri kopyaladığınızda, veri taşıma Hizmeti'nde yok sayar **parallelCopies** özelliği. Paralellik belirtilmiş olsa bile, bu durumda uygulanmaz.
* **parallelCopies** dikgen olan **dataIntegrationUnits**. Önceki tüm veri tümleştirme birimlerinizde sayılır.

## <a name="staged-copy"></a>Hazırlanmış kopya

Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

- **PolyBase aracılığıyla SQL veri ambarı'na çeşitli veri depolarından veri alabilen istediğiniz**. SQL veri ambarı PolyBase, SQL veri ambarı'na büyük miktarda veri yüklemek için yüksek performanslı mekanizması olarak kullanır. Ancak, kaynak verileri Blob Depolama veya Azure Data Lake Store olmalıdır ve ek ölçütleri karşılaması gerekir. Blob Depolama farklı bir veri deposu ya da Azure Data Lake Store veri yüklediğinizde, verileri geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda, Data Factory, PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümleri gerçekleştirir. Ardından verileri verimli bir şekilde SQL veri ambarı'na yüklemek için PolyBase kullanır. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Bazı durumlarda bir karma veri taşıma işlemini gerçekleştirmek için bir süre sürer (diğer bir deyişle, bir şirket içi kopyalamak için verileri bir bulut veri deposuna depolamak) yavaş ağ bağlantısı üzerinden**. Performansı artırmak için hazırlama veri deposu bulutta veri taşıma sonra hedef veri deposuna yüklemeden önce hazırlama deposu verileri genişletmek için daha az zaman alır, böylece şirket verileri sıkıştırmak için hazırlanmış kopya kullanabilirsiniz.
- **80 numaralı bağlantı noktası dışındaki bağlantı noktaları açın ve kurumsal BT ilkeleri nedeniyle, güvenlik duvarında 443 bağlantı noktasına istemediğiniz**. Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu için bir şirket içi veri deposundan veri kopyaladığınızda, Windows Güvenlik Duvarı hem kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimine'ı etkinleştirmeniz gerekir. Bu senaryoda, hazırlanmış kopya ilk örneği, HTTP veya HTTPS bağlantı noktası 443 üzerinden hazırlama Blob depolama alanına veri kopyalamak için şirket içinde barındırılan Integration Runtime yararlanmak sonra verileri Blob Depolama hazırlık ortamından SQL veritabanı veya SQL veri ambarı yükleme. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopya çalışır

Hazırlama özelliğini etkinleştirdiğinizde, ilk veriler kaynak veri deposundan hazırlama Blob depolama alanına kopyalanır (kendi işleyicinizi getirin). Ardından, veri hazırlama veri deposundan havuz veri deposuna kopyalanır. Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Veri taşıma tamamlandıktan sonra veri fabrikası geçici verileri hazırlama depolama biriminden da temizler.

![Hazırlanmış kopya](media/copy-activity-performance/staged-copy.png)

Hazırlama deposu kullanarak veri taşıma etkinleştirdiğinizde, verileri bir geçiş veya hazırlama veri deposu için kaynak veri deposundan veri taşımadan önce sıkıştırılmış ve ardından bir geçiş veri taşıma ve veri hazırlık önce eklenmişti isteyip istemediğinizi belirtebilirsiniz Havuz veri deposuna depolayın.

Şu anda, hazırlama deposu kullanarak iki şirket içi veri depoları arasında veri kopyalanamıyor.

### <a name="configuration"></a>Yapılandırma

Yapılandırma **enableStaging** verileri Blob Depolama alanında çoğaltılmadan önce bir hedef veri deposuna yüklemek isteyip istemediğinizi belirtmek için kopyalama etkinliği ayarlama. Ayarladığınızda **enableStaging** için `TRUE`, sonraki tabloda listelenen ek özellikleri belirtin. Yoksa, depolama paylaşılan erişim imzası bağlı hizmeti hazırlama için veya bir Azure depolama oluşturulması gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| **enableStaging** |Veri deposunu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| **linkedServiceName** |Adını bir [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) bağlı hizmeti, geçici bir hazırlama deposu kullanan depolama örneğine başvurur. <br/><br/> PolyBase aracılığıyla SQL veri ambarı'na veri yüklemek için depolama ile paylaşılan erişim imzası kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlayın |
| **Yolu** |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolu belirtin. Hizmet, bir yol belirtmezseniz, geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici veri belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| **enableCompression** |Hedefe kopyalamadan önce verilerin sıkıştırılmasının gerekli olup olmadığını belirtir. Bu ayar, aktarılan veri hacmini azaltır. |False |Hayır |

>[!NOTE]
> Hizmet, bağlantılı blob hazırlama için sıkıştırma etkin, hizmet sorumlusu veya MSI kimlik doğrulaması hazırlanmış kopya kullanıyorsanız desteklenmiyor.

Kopyalama etkinliği önceki tabloda açıklanan özellikler ile bir örnek tanımı aşağıda verilmiştir:

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

### <a name="staged-copy-billing-impact"></a>Faturalama etkisi kopyalama hazırlandı

Bağlı olarak iki adımı ücretlendirilir: kopyalama süresi ve kopyalayın türü.

* Bir bulut kopyalama sırasında (verileri başka bir bulut veri deposu, her iki aşama Azure tümleştirme çalışma zamanı tarafından yetkilendirilmiş bir bulut veri deposundan kopyalama) Hazırlama [toplam kopyalama süresi adım 1 ve 2. adım] [bulut kopyalama birim fiyatı] x kullandığınızda ücretlendirilir.
* Kullandığınızda (verileri bulut veri deposu, tek bir aşamada şirket içinde barındırılan tümleştirme çalışma zamanı tarafından yetkilendirilmiş bir şirket içi veri deposundan kopyalama) karma kopyalama sırasında Hazırlama [karma kopyalama süresi] ödersiniz x [karma kopyalama birim fiyatı] + [bulut kopyalama süresi] x [bulut kopya birim fiyatı].

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları

Kopyalama etkinliği, Data Factory hizmetine performansını ayarlamak için aşağıdaki adımları atmanız öneririz:

1. **Bir taban çizgisi oluşturma**. Geliştirme aşamasında, bir temsilci veri örneği karşı kopyalama etkinliği'ni kullanarak işlem hattınızı test etme. Yürütme ayrıntıları ve performans özelliklerini aşağıdaki toplamak [kopyalama etkinliği izleme](copy-activity-overview.md#monitoring).

2. **Tanılama ve performans iyileştirme**. Siz gözleyin performans beklentilerinizi karşılamıyorsa, performans sorunlarını tanımlamak gerekir. Ardından, kaldırmak veya performans etkisini azaltmak için performansı iyileştirin. 

    Bazı durumlarda, bir kopyalama etkinliği, ADF'de yürüttüğünüzde doğrudan görürsünüz "**performans ayarlama ipuçları**" üst kısmındaki [kopyalama etkinliği izleme sayfası](copy-activity-overview.md#monitor-visually) aşağıdaki örnekte gösterildiği gibi. Yalnızca belirli bir kopya çalıştırmak için tanımlanan sorunu bildiren, ancak Ayrıca, ne kopyalama aktarım hızı artırmak için değiştirmek size yol gösterir. Performans ayarı ipuçları şu anda önerileri PolyBase veri kaynağında yan depoladığınızda, Azure Cosmos DB RU veya Azure SQL DB DTU artırmak için Azure SQL veri ambarı'na veri kopyalama işlemi sırasında kullanmak ister sağlamak gereksiz aşamalı kaldırmak için kaynaklanıyor kopyalama, vb. Performans kuralları ayarlama kademeli olarak de zenginleştirilmiş.

    **Örnek: performans ayarlama ipuçları Azure SQL veritabanına kopyalama**

    Bu örnekte, yazma işlemleri yavaşlatır yüksek DTU kullanımı havuz Azure SQL DB ulaştığında ADF bildirimi çalışan kopyalama sırasında böylece öneri Azure SQL veritabanı katmanı ile daha fazla DTU artırmaktır. 

    ![Performans ayarlama ipuçları ile izleme kopyalayın](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Ayrıca, bazı genel konular aşağıda verilmiştir. Performans Tanılama tam açıklamasını, bu makalenin kapsamı dışındadır ' dir.

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Veri tümleştirme birimleri](#data-integration-units)
     * [Hazırlanmış kopya](#staged-copy)
     * [Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirlik](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Şirket içinde barındırılan tümleştirme çalışma zamanı](#considerations-for-self-hosted-integration-runtime)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Serileştirme ve seri durumundan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşleme](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)

3. **Tüm veri kümeniz Yapılandırması**. Yürütme sonuçları ve performans ile memnun kaldığınızda, tanım ve işlem hattı, tüm veri kümeniz kapsayacak şekilde genişletebilirsiniz.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı için dikkat edilmesi gerekenler

Kopyalama etkinliği, bir şirket içinde barındırılan tümleştirme çalışma zamanını yürütülürse, aşağıdakilere dikkat edin:

**Kurulum**: Integration Runtime konak ayrılmış bir makineye kullanmanızı öneririz. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanını kullanma konuları](concepts-integration-runtime.md).

**Ölçeği genişletme**: Tek bir mantıksal şirket içinde barındırılan tümleştirme çalışma zamanı bir veya daha fazla düğüme sahip aynı anda aynı anda birden fazla kopyalama etkinliği çalıştırma görebilir. Karma veri hareketi çok sayıda eşzamanlı kopyalama etkinliği çalıştırma veya büyük miktarlarda veri kopyalamak için üzerinde ağır gerekmesi durumunda için göz önünde [şirket içinde barındırılan tümleştirme çalışma zamanı ölçeğinizi](create-self-hosted-integration-runtime.md#high-availability-and-scalability) için daha fazla kaynak sağlamak için kopyalama güçlendirin.

## <a name="considerations-for-the-source"></a>Kaynağı için konular

### <a name="general"></a>Genel

Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını değil emin olun.

Microsoft veri depoları için bkz: [izleme ve ayarlama konuları](#performance-reference) veri depoları ve veri performans özelliklerini depolamak, yanıt süreleri en aza indirmek ve aktarım hızını en üst düzeye anlamanıza yardım özgü olan.

* Verileri kopyalarsanız **Blob depolamadan SQL veri ambarı**, kullanmayı **PolyBase** performansını artırmak üzere. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için.
* Verileri kopyalarsanız **Azure Blob/Azure Data Lake Store için hdfs**, kullanmayı **DistCp** performansını artırmak üzere. Bkz: [kullanım verileri HDFS kopyalamak için DistCp](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) Ayrıntılar için.
* Verileri kopyalarsanız **redshift'ten Azure SQL veri ambarı/Azure BLob/Azure Data Lake Store için**, kullanmayı **kaldırma** performansını artırmak üzere. Bkz: [kullanım verileri Amazon Redshift'ten kopyalamak için kaldırma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) Ayrıntılar için.

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Dosya boyutu ve dosya sayısı ortalaması**: Kopyalama etkinliği, bir kerede veri bir dosya aktarır. İle aynı taşınacak veri miktarına, hizmetin genel performansını veriler nedeniyle her dosya için önyükleme aşaması birkaç büyük dosyalar yerine çok sayıda küçük dosya oluşuyorsa düşüktür. Bu nedenle, mümkünse, küçük dosyaları daha yüksek performans elde etmek için daha büyük dosyalarına birleştirin.
* **Dosya biçimi ve sıkıştırma**: Performansı artırmak daha fazla yolu için bkz. [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) bölümler.

### <a name="relational-data-stores"></a>İlişkisel veri deposu

* **Veri modelini**: Tablo şemanızı kopyalama aktarım hızını etkiler. Büyük satır boyutu, küçük satır boyutu, aynı miktarda veri kopyalamak için daha iyi bir performans sunar. Veritabanı daha az satır içeren veri daha az toplu işler daha verimli bir şekilde alabilirsiniz nedenidir.
* **Sorgu veya saklı yordam**: Sorgu veya saklı yordam verileri daha verimli bir şekilde getirmek için kopyalama etkinliği kaynak belirttiğiniz mantığını iyileştirin.

## <a name="considerations-for-the-sink"></a>Havuz için dikkat edilmesi gerekenler

### <a name="general"></a>Genel

Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını değil emin olun.

Microsoft veri depoları için başvurmak [izleme ve ayarlama konuları](#performance-reference) özgü veri depoları. Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

* Verileri kopyalarsanız **Blob depolamadan SQL veri ambarı**, kullanmayı **PolyBase** performansını artırmak üzere. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için.
* Verileri kopyalarsanız **Azure Blob/Azure Data Lake Store için hdfs**, kullanmayı **DistCp** performansını artırmak üzere. Bkz: [kullanım verileri HDFS kopyalamak için DistCp](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs) Ayrıntılar için.
* Verileri kopyalarsanız **redshift'ten Azure SQL veri ambarı/Azure BLob/Azure Data Lake Store için**, kullanmayı **kaldırma** performansını artırmak üzere. Bkz: [kullanım verileri Amazon Redshift'ten kopyalamak için kaldırma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) Ayrıntılar için.

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Kopyalama davranışı**: Farklı dosya tabanlı veri deposundan verileri kopyalarsanız, kopyalama etkinliği ile üç seçenek vardır. **copyBehavior** özelliği. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyayı birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performansa sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: Bkz [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) performansını artırmak daha yollarını bölümler.

### <a name="relational-data-stores"></a>İlişkisel veri deposu

* **Kopyalama davranışı**: Bağlı olarak özellikleri için ayarladığınız **sqlSink**, kopyalama etkinliği farklı şekillerde hedef veritabanına veri yazar.
  * Varsayılan olarak, en iyi performans sağlayan modu, veri taşıma hizmeti kullandığı veri eklemek için toplu kopyalama API ekleyin.
  * Havuza bir saklı yordam yapılandırırsanız, veritabanı veri bir satırı toplu yükleme yerine anda geçerlidir. Performansı ciddi ölçüde düştüğü. Veri kümeniz varsa, büyük ise kullanmayı göz önünde bulundurun **preCopyScript** özelliği.
  * Yapılandırırsanız **preCopyScript** çalıştırmak için her bir kopyalama etkinliği özelliği, hizmeti betik tetikler ve sonra verileri eklemek için toplu kopyalama API'sini kullanırsınız. Örneğin, en son verileriyle tüm tablo üzerine yazmak için önce toplu yeni veri kaynağından yükleme önce tüm kayıtları silmek için bir betik belirtebilirsiniz.
* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemanızı kopyalama aktarım hızını etkiler. Veritabanı daha verimli bir şekilde daha az toplu veri kaydedebilir miyim çünkü aynı miktarda veri kopyalamak için büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, toplu bir dizi içinde veri ekler. Bir toplu işte satır sayısını kullanarak ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satır varsa, ayarlayabileceğiniz **writeBatchSize** özellik toplu işlem ek yükü azaltır ve daha yüksek performans avantajlarından yararlanarak daha yüksek bir değere sahip. Verilerinizin satır boyutu büyükse, artırdığınızda dikkat **writeBatchSize**. Yüksek bir değere bir kopyalama hatasının veritabanı aşırı yüklenerek neden neden olabilir.

### <a name="nosql-stores"></a>NoSQL depoları

* İçin **tablo depolama**:
  * **bölüm**: Araya eklemeli bölümlere önemli ölçüde veri yazma, performansı düşürür. Veri kaynağınızı bölüm anahtarına göre sıralama verileri verimli bir şekilde bir bölüme diğerinden sonra eklenir veya tek bir bölüm için veri yazmak için mantığı ayarlayın.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için dikkat edilmesi gerekenler

Serileştirme ve seri durumundan çıkarma girdi veri kümesini veya çıkış veri kümesi bir dosya olduğunda ortaya çıkabilir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](supported-file-formats-and-compression-codecs.md) kopyalama etkinliği tarafından desteklenen dosya biçimleri hakkında daha fazla ayrıntı içeren.

**Kopyalama davranışı**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Aynı veya hiçbir dosya biçimi ayarları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda yürüten bir **ikili kopya** serileştirme veya seri durumundan çıkarma olmadan. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek bir aktarım hızı görürsünüz.
  * Ne zaman giriş ve çıkış veri kümeleri hem metin biçimi ve yalnızca kodlama olan türü farklı olmadığı, veri taşıma Hizmeti'nde yalnızca kodlama dönüştürme yapar. Herhangi bir serileştirme yapmaz ve seri durumdan yükü ikili kopyasını kıyasla bazı performans neden olur.
  * Farklı dosya biçimleri veya sınırlayıcılar gibi farklı yapılandırmaları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda akışı, dönüştürme ve belirttiğiniz çıkış biçimine seri hale için kaynak verileri seri durumdan çıkarır. Bu işlem ek yükü diğer senaryolarda kıyasla çok daha önemli bir performans sonuçlanır.
* (Örneğin, bir dosya tabanlı depolama alanından ilişkisel bir veri deposuna) dosya tabanlı olmayan bir veri deposuna/deposundan veri dosyalarını kopyalarken, serileştirme veya seri durumundan çıkarma adım gereklidir. Bu adım, önemli performans ek yükü oluşur.

**Dosya biçimi**: Seçtiğiniz dosya biçimi kopyalama performansı etkileyebilir. Örneğin, Avro verileriyle meta verileri depolar sıkıştırılmış bir ikili biçimi ' dir. Hadoop ekosistemindeki işleme ve sorgulama için geniş destek vardır. Ancak, Avro daha pahalı serileştirme ve seri kaldırma metin biçimine kıyasla daha düşük kopyalama aktarım hızı ile sonuçlanır. Dosya biçimi işleme akışı boyunca tercih ettiğiniz bütünlüklü olarak yapın. Hangi veri formu ile Başlat, kaynak veri depolarını veya dış sistemlerden ayıklanacak depolanır; Depolama, analitik işleme ve sorgulama için en iyi biçimi; ve hangi biçimde uygulamasına raporlama ve görselleştirme araçları için veri reyonları veri verilmesi. Bazen için yetersiz bir dosya biçimi okuma ve yazma performansını genel analitik işlemi düşünürken iyi bir seçim olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma dikkate alınacak noktalar

Giriş veya çıkış veri kümesi bir dosya olduğunda, hedef konuma verileri yazar gibi sıkıştırma veya sıkıştırmayı açma gerçekleştirmek için kopyalama etkinliği ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir denge duruma ve CPU. Bilgi işlem kaynaklarının ek veri maliyetlerini sıkıştırma. Ancak, ağ g/ç ve depolama azaltır. Verilere bağlı olarak bir artırma genel kopyalama aktarım hızının görebilirsiniz.

**Codec**: Her bir sıkıştırma codec avantajları vardır. Örneğin, bzıp2 düşük kopyalama aktarım hızına sahip, ancak işleme için bölme çünkü bzıp2 ile en iyi Hive sorgu performansı elde. Gzip en dengeli bir seçenektir ve en sık kullanılır. Uçtan uca senaryonuza uygun codec bileşeni seçin.

**Düzey**: Her bir sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: hızlı sıkıştırılmış ve verimli sıkıştırılmış. Sıkıştırılmış en hızlı seçenek bile elde edilen dosyanın en uygun şekilde sıkıştırılmamış veri, mümkün olan en kısa sürede sıkıştırır. En uygun şekilde sıkıştırılmış seçeneği sıkıştırma üzerinde daha fazla zaman harcadığını ve en az miktarda veri ortaya çıkarır. İki seçenek de durumunuzdaki genel daha iyi performans sağlayan görmek için test edebilirsiniz.

**Önemli bir unsur**: Büyük miktarda bir şirket içi depolama ile bulut arasında veri kopyalamak için kullanmayı [kopyalama aşamalı](#staged-copy) ile sıkıştırma etkin. Geçici depolama kullanarak şirket ağınıza ve Azure hizmetlerinizi bant sınırlayan faktör ve giriş veri kümesi ve çıktı veri kümesi hem sıkıştırılmamış biçiminde olmasını istediğiniz olduğunda yararlıdır.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için dikkat edilmesi gerekenler

Ayarlayabileceğiniz **Bunun amacı** kopyalama etkinliği özelliği tüm harita veya çıkış sütunları için giriş sütun alt kümesi. Veri taşıma Hizmeti'nde veri kaynağından okur. sonra havuz için verileri yazar önce veriler üzerinde sütun eşleme gerçekleştirmek gerekir. Bu ek işlem kopyalama performansını düşürür.

Kaynak veri deponuzda sorgulanabilir, örneğin, SQL veritabanı veya SQL Server gibi ilişkisel bir mağaza veya tablo depolama veya Azure Cosmos DB gibi bir NoSQL deposu olması durumunda sütun filtreleme gönderme ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özellik. Veri taşıma Hizmeti'nde çok daha verimli olduğu kaynak veri deposundan veri okurken bu şekilde yansıtma gerçekleşir.

Daha fazla bilgi [kopyalama etkinliği şema eşleme](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Kopyalamak istediğiniz veri boyutu büyükse, her kopyalama etkinliği çalıştırmak için veri boyutunu azaltmak için daha sık çalıştırmak için kopyalama etkinliği verileri daha fazla ve iş mantığınızı ayarlayabilirsiniz.

Veri kümeleri ve aynı anda aynı veri deposuna bağlanmak Data Factory gerektiren kopyalama etkinlikleri hakkında dikkatli olun. Birçok eş zamanlı kopyası işleri bir veri deposu azaltma ve performansın düşmesine neden, kopyalama işi iç deneme ve bazı durumlarda, yürütme hatalarını sağlama.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: Bir şirket içi SQL Server'dan Blob depolamaya kopyalama

**Senaryo**: Bir işlem hattı, bir şirket içi SQL Server'dan Blob Depolama alanında CSV biçimi veri kopyalamak için oluşturulmuştur. Kopyalama işini daha hızlı hale getirmek için CSV dosyaları bzıp2 biçimine sıkıştırılmasının gerekli.

**Test ve analiz**: Performans Kıyaslama yavaş olduğu değerinden 2 MB/sn aktarım hızı kopyalama etkinliği var.

**Performans Analizi ve ayarlama**: Performans sorunu gidermek için nasıl veri işlenen taşınır ve adresindeki gözden geçirelim.

1. **Veri Okuma**: Tümleştirme çalışma zamanı SQL Server için bir bağlantı açar ve sorguyu gönderir. SQL Server Integration runtime intranet üzerinden veri akışını göndererek yanıt verir.
2. **Seri hale getirmek ve verileri sıkıştırmak**: Integration runtime, veri akışı CSV biçimine serileştiren ve bzıp2 akış verileri sıkıştırır.
3. **Veri yazma**: Integration runtime bzıp2 akış Internet üzerinden Blob depolamaya yükler.

Gördüğünüz gibi verileri ettiğinden işlenir ve akış sıralı bir şekilde taşındı: SQL Server > LAN > Integration runtime > WAN > Blob Depolama. **Genel performansını en düşük aktarım hızı ile işlem hattı Geçitli**.

![Veri akışı](./media/copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla aşağıdaki faktörleri performans sorunu neden olabilir:

* **Kaynak**: SQL Server kendisini aktarım hızının düşük olmasını nedeniyle ağır yükleri vardır.
* **Barındırılan tümleştirme çalışma zamanını**:
  * **LAN**: Integration runtime gölgeden uzak SQL Server makinesinde bulunduğu yer ve düşük bant genişliğine sahip bağlantısı vardır.
  * **Integration runtime**: Tümleştirme çalışma zamanı aşağıdaki işlemleri gerçekleştirmek için kendi yük sınırlamaları ulaştı:
    * **Serileştirme**: CSV biçimi için veri akışı seri hale getirme, yavaş aktarım hızına sahip.
    * **Sıkıştırma**: Yavaş sıkıştırma codec (Core i7 ile 2.8 MB/sn, örneğin, bzıp2,) seçtiğiniz.
  * **WAN**: Kurumsal ağ ve Azure hizmetlerinizi arasında bant genişliği düşük olduğu (örneğin, T1 1,544 kbps; = T2 6,312 kbps =).
* **Havuz**: BLOB Depolama, aktarım hızının düşük olmasını sahiptir. (En az 60 MB/sn kendi SLA'sı garanti eder, çünkü bu senaryo bir olasılıktır.)

Bu durumda, bzıp2 veri sıkıştırma tüm işlem hattını yavaşlatmasını. Gzip sıkıştırma codec bileşenine geçiş, bu performans sorunu kolaylaştırmak.

## <a name="reference"></a>Başvuru

Performans izleme ve desteklenen veri depolarının bazılarını başvuruları ayarlama şu şekildedir:

* Azure Storage (Blob Depolama ve tablo depolama gibi): [Azure depolama ölçeklenebilirlik hedefleri](../storage/common/storage-scalability-targets.md) ve [Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../storage/common/storage-performance-checklist.md)
* Azure SQL Veritabanı: Yapabilecekleriniz [performansını izleme](../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimi (DTU) yüzde denetleyin
* Azure SQL veri ambarı: Kendi özellik veri ambarı birimi (Dwu) ölçülür bkz: [Yönet işlem gücünü Azure SQL veri ambarı (Genel)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Azure Cosmos DB'de performans düzeyleri](../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [İzleme ve performansı ayarlama](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [Dosya sunucuları için performans ayarı](https://msdn.microsoft.com/library/dn567661.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği şema eşleme](copy-activity-schema-and-type-mapping.md)
- [Kopyalama etkinliği hataya dayanıklılık](copy-activity-fault-tolerance.md)
