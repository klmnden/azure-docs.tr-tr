---
title: Kopyalama etkinliği performansı ve ayarlama Kılavuzu Azure Data Factory | Microsoft Docs
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
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: face3719f32ccb44e7479150e94417496141f90b
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509566"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Etkinlik performansı ve ayarlama Kılavuzu kopyalayın
> [!div class="op_single_selector" title1="Azure Data Factory, kullanmakta olduğunuz sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-activity-performance.md)
> * [Geçerli sürüm](copy-activity-performance.md)


Azure Data Factory kopyalama etkinliği, çözüm yüklenirken birinci sınıf bir güvenli, güvenilir ve yüksek performanslı veri sunar. Kopyasına terabaytlarca veriyi her gün boyunca birçok farklı Bulut ve şirket içi veri depolarının onlarca kullanabilirsiniz. Hızlı veri yükleme performansı, çekirdek büyük veri soruna odaklanmak, sağlamak üzere anahtarı: Gelişmiş analiz çözümleri oluşturmak ve tüm bu verilerden ayrıntılı Öngörüler alma.

Azure Kurumsal düzeyde bir dizi veri depolama ve veri ambarı çözümleri sağlar. Kopyalama etkinliği yapılandırmak ve ayarlamak kolaydır deneyimi yüklenirken yüksek oranda iyileştirilmiş bir veri sunar. Bir tek kopyalama etkinliği ile verileri yükleyebilirsiniz:

* Azure SQL veri ambarı'na 1,2 GB/sn.
* Azure Blob Depolama'ya ise 1,0 GB/sn.
* Azure Data Lake Store, 1,0 GB/sn.

Bu makalede açıklanır:

* [Performans başvuru sayıları](#performance-reference) desteklenen proje planlamanıza yardımcı olması için kaynak ve havuz veri deposu.
* Farklı senaryolarda içeren kopyalama aktarım artırabilir özellikleri [veri tümleştirme birimleri](#data-integration-units) (DIUs) [paralel kopyalama](#parallel-copy), ve [kopyalama aşamalı](#staged-copy).
* [Performans ayarlama Kılavuzu](#performance-tuning-steps) performans ve kopyalama performansını etkileyen önemli faktörlerin ayarlama konusunda.

> [!NOTE]
> Genel kopyalama etkinliği ile ilgili bilgi sahibi değilseniz, bkz. [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) önce bu makaleyi okuyun.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru olarak bir tek bir kopyalama etkinliği çalıştırma havuz çiftlerinde şirket içi teste dayanan ve aşağıdaki tabloda kopyalama aktarım hızı sayısı MB/sn olarak belirtilen kaynağı gösterir. Karşılaştırma için ayrıca nasıl farklı ayarları gösterir [veri tümleştirme birimleri](#data-integration-units) veya [barındırılan tümleştirme çalışma zamanı ölçeklenebilirlik](concepts-integration-runtime.md#self-hosted-integration-runtime) (birden çok düğüm) kopyalama performansı üzerinde yardımcı olabilir.

![Performans Matrisi](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Kopyalama etkinliği, bir Azure tümleştirme çalışma zamanı üzerinde çalıştığında, en az izin verilen veri tümleştirmesi (eski adıyla veri taşıma birimleri da bilinir) iki birimidir. Belirtilmezse, varsayılan veri tümleştirme kullanılmakta birimlerini görmek [veri tümleştirme birimleri](#data-integration-units).

**Dikkat edilecek noktalar:**

* Aşağıdaki formülü kullanarak aktarım hızı hesaplanır: [kaynak Okuma boyutu veri] / [kopyalama etkinliği çalıştırma süresi].
* Tablosundaki performans başvuru numaralarını kullanarak ölçülen bir [TPC-H](http://www.tpc.org/tpch/) veri kümesi tek bir kopyalama etkinliğinde çalıştırın. Test dosyaları dosya tabanlı depoları için birden çok 10 GB boyutundaki dosyalarıdır.
* Azure veri depoları, kaynak ve havuz aynı Azure bölgesinde olan.
* Karma kopyalama şirket içi ve bulut arasında veri depoları, her şirket içinde barındırılan Integration runtime düğümü şu belirtime veri deposundan ayrı bir makinede çalışıyordu. Tek bir etkinlik çalıştırırken test makinenin CPU, bellek veya ağ bant genişliği için küçük bir bölümü kopyalama işlemi kullanılan.
    <table>
    <tr>
        <td>CPU</td>
        <td>2\.20 GHz Intel Xeon E5-2660 v2 32 çekirdek</td>
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
> Daha fazla DIUs kullanarak daha yüksek aktarım hızı elde edebilirsiniz. Örneğin, 100 DIUs ile verileri Azure Blob depolama alanından 1,0 GB/sn, Azure Data Lake Store içine kopyalayabilirsiniz. Bu özellik ve desteklenen bir senaryo hakkında daha fazla bilgi için bkz: [veri tümleştirme birimleri](#data-integration-units) bölümü. 

## <a name="data-integration-units"></a>Veri tümleştirme birimleri

Bir veri tümleştirme Azure Data Factory içinde tek bir birim (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçü birimidir. Veri tümleştirme birimi yalnızca uygulandığı [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime), ama [barındırılan tümleştirme çalışma zamanını](concepts-integration-runtime.md#self-hosted-integration-runtime).

Bir kopyalama etkinliği çalıştırma güçlendirmek için en az DIUs iki olur. Belirtilmezse, aşağıdaki tabloda farklı kopyalama senaryolarında kullanılan varsayılan DIUs listelenmektedir:

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan DIUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | 4 ile sayısı ve dosyaların boyutuna bağlı olarak 32 arasında |
| Azure SQL veritabanı veya Azure Cosmos DB veri kopyalama |Havuz Azure SQL veritabanı'nın veya Cosmos DB'nin Katmanı (Dtu/RU sayısı) bağlı olarak 16 ile 4 arasında |
| Tüm diğer kopyalama senaryolarında | 4 |

Bu varsayılanı geçersiz kılmak için bir değer belirtin. **dataIntegrationUnits** özelliğini aşağıdaki gibi. *İzin verilen değerler* için **dataIntegrationUnits** en fazla 256 özelliğidir. *DIUs gerçek sayısını* eşit veya daha az, veri modelini bağlı olarak yapılandırılmış bir değeri, kopyalama işleminin çalışma zamanında kullanır. Özel kopyalama kaynağı ve havuz için daha fazla birimi yapılandırırken alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

Bir etkinlik çalıştırması izlerken kopyalama etkinliği çıkışında çalıştırma her kopya için kullanılan DIUs görebilirsiniz. Daha fazla bilgi için [kopyalama etkinliği izleme](copy-activity-overview.md#monitoring).

> [!NOTE]
> DIUs dört şu anda yalnızca birden fazla dosyaları Azure Storage'dan kopyaladığınızda geçerlidir büyük ayarlama, Azure Data Lake Store, Amazon S3, Google bulut depolama, FTP Bulut veya SFTP için başka bir bulut veri depoları bulut.
>

**Örnek**

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

Tabanlı kopyalama işlemi toplam zamanında ücretlendirildiğiniz olduğunu unutmayın. Veri taşıma toplam süresi faturalandırılırsınız arasında DIUs süresi toplamıdır. Bir kopyalama işi bir saat ile iki bulut birimleri almak için kullanılan ve artık bu sekiz bulut birimiyle 15 dakika sürer, toplam fatura neredeyse aynı kalır.

## <a name="parallel-copy"></a>Paralel kopyalama

Kullanabileceğiniz **parallelCopies** kopyalama etkinliği, kullanmak istediğiniz paralellik belirtmek için özelliği. Bu özellik veri kaynağınızdan okuyabilen veya, havuz veri depolarına paralel yazma kopyalama etkinliği içinde iş parçacığı sayısı olarak düşünebilirsiniz.

Çalıştıran her kopyalama etkinliği için Azure Data Factory veri depolamak ve için hedef veri deposu kaynak sunucudan veri kopyalamak için paralel kopya sayısını belirler. Varsayılan sayısıyla onu kullanan paralel kaynak ve havuz kullandığınız türüne bağlıdır.

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı depoları arasında veri kopyalama |Dosyaları iki bulut veri deposu ya da şirket içinde barındırılan tümleştirme çalışma zamanı makinenin fiziksel yapılandırması arasında veri kopyalamak için kullanılan DIUs sayısı ve boyutuna bağlıdır. |
| Tüm kaynak deposundan Azure tablo depolama alanına veri kopyalama |4 |
| Tüm diğer kopyalama senaryolarında |1 |

> [!TIP]
> Dosya tabanlı depoları arasında veri kopyalama, varsayılan davranışı, genellikle en iyi aktarım hızı sağlar. Varsayılan davranış, kaynak dosya deseni temel alınarak otomatik olarak belirlenir.

Verilerinizi barındıran makinelerin yükünü denetlemek için depolar veya kopyalama performansı ayarlamak için varsayılan değeri geçersiz kılabilir ve için bir değer belirtin **parallelCopies** özelliği. Büyük veya 1'e eşit bir tamsayı değeri olmalıdır. Çalışma zamanında, en iyi performans için ayarladığınız değerine eşit veya daha az olan bir değer kopyalama etkinliği kullanır.

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

**Dikkat edilecek noktalar:**

* Dosya tabanlı depoları arasında veri kopyalama **parallelCopies** dosya düzeyinde paralellik belirler. Tek bir dosyada Öbekleme altında otomatik olarak ve şeffaf bir şekilde gerçekleşir. En uygun öbek paralel veri yükleme belirtilen kaynak veri deposu türü için boyut ve dikgen kullanmak için tasarlanmış **parallelCopies**. Gerçek veri taşıma Hizmeti'nde kopyalama işleminin çalışma zamanında kullandığı paralel kopya sayısı sahip olduğunuz dosyaların sayısı, en fazla ' dir. Kopyalama davranışını ise **mergeFile**, kopyalama etkinliği dosya düzeyinde paralellik yararlanamaz.
* (Dışında Oracle veritabanı etkin veri bölümleme ile kaynak olarak) dosya tabanlı olmayan depolarından veri kopyaladığınızda, dosya tabanlı depoları için veri taşıma Hizmeti'nde yok sayar **parallelCopies** özelliği. Paralellik belirtilmiş olsa bile, bu durumda uygulanmaz.
* **ParallelCopies** özelliktir dikgen **dataIntegrationUnits**. Önceki tüm veri tümleştirme birimlerinizde sayılır.
* İçin bir değer belirtirseniz **parallelCopies** özelliği, kaynak üzerindeki yük artışı göz önünde bulundurun ve havuz veri deposu. Kopyalama etkinliği tarafından Örneğin, karma kopyalama için yetkilendirilirler de şirket içinde barındırılan tümleştirme çalışma zamanı yük artışı göz önünde bulundurun. Bu yük artışı özellikle birden çok etkinlikler veya aynı veri deposuna karşı çalışan aynı etkinliklerden eş zamanlı çalıştırma olduğunda gerçekleşir. Veri deposu ya da şirket içinde barındırılan tümleştirme çalışma zamanı yük ile doludur fark ederseniz, azaltma **parallelCopies** yükle hafifletmek için değer.

## <a name="staged-copy"></a>Hazırlanmış kopya

Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

- **PolyBase aracılığıyla SQL veri ambarı'na çeşitli veri depolarından veri alabilen istiyorsunuz.** SQL veri ambarı PolyBase, SQL veri ambarı'na büyük miktarda veri yüklemek için yüksek performanslı mekanizması olarak kullanır. Kaynak verileri Blob Depolama veya Azure Data Lake Store olmalıdır ve diğer ölçütleri karşılaması gerekir. Blob Depolama farklı bir veri deposu ya da Azure Data Lake Store veri yüklediğinizde, verileri geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda, Azure Data Factory, PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümleri gerçekleştirir. Ardından verileri verimli bir şekilde SQL veri ambarı'na yüklemek için PolyBase kullanır. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Bazı durumlarda bir karma veri taşıma işlemini gerçekleştirmek için bir süre sürer (diğer bir deyişle, bir şirket içi kopyalamak için verileri bir bulut veri deposuna depolamak) yavaş ağ bağlantısı üzerinden.** Performansı artırmak için hazırlama veri deposu bulutta veri taşıma daha az zaman alır, böylece şirket verileri sıkıştırmak için hazırlanmış kopya kullanabilirsiniz. Hedef veri deposuna yüklemeden önce hazırlama deposu verileri genişletmek.
- **Kurumsal BT ilkeleri nedeniyle duvarınızda bağlantı noktası 80 ve 443 numaralı bağlantı noktası dışındaki bağlantı noktaları açmadan istemezsiniz.** Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu için bir şirket içi veri deposundan veri kopyaladığınızda, Windows Güvenlik Duvarı hem kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimine'ı etkinleştirmeniz gerekir. Bu senaryoda, hazırlanmış kopya, ilk örneği, HTTP veya HTTPS bağlantı noktası 443 üzerinden hazırlama Blob depolama alanına veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanı avantajlarından yararlanabilirsiniz. Ardından, verileri SQL veritabanı veya SQL veri ambarı ile Blob Depolama hazırlamadan yükleyebilirsiniz. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopya çalışır

Hazırlama özelliğini etkinleştirdiğinizde, ilk veriler kaynak veri deposundan hazırlama Blob depolama alanına kopyalanır (kendi işleyicinizi getirin). Ardından, veri hazırlama veri deposundan havuz veri deposuna kopyalanır. Azure Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Veri taşıma tamamlandıktan sonra azure Data Factory geçici verileri hazırlama depolama biriminden da temizler.

![Hazırlanmış kopya](media/copy-activity-performance/staged-copy.png)

Hazırlama deposu kullanarak veri taşıma etkinleştirdiğinizde, veri kaynağından veri taşımadan önce sıkıştırılmasının verileri için bir arada depolamak veya hazırlama veri depolamak ve ardından bir geçiş veya hazırlama dat veri taşımadan önce eklenmişti isteyip istemediğinizi belirtebilirsiniz Havuz veri deposu için bir depo.

Şu anda farklı şirket içinde barındırılan IRS, ile ne hazırlanmış kopya olmadan üzerinden bağlanan iki veri depoları arasında veri kopyalanamıyor. Bu senaryo için iki açıkça zincirleme kopyalama etkinliği, hazırlama kaynağından sonra havuz hazırlama alanından kopyalamak için yapılandırabilirsiniz.

### <a name="configuration"></a>Yapılandırma

Yapılandırma **enableStaging** verileri Blob Depolama alanında çoğaltılmadan önce bir hedef veri deposuna yüklemek isteyip istemediğinizi belirtmek için kopyalama etkinliği ayarlama. Ayarladığınızda **enableStaging** için `TRUE`, aşağıdaki tabloda listelenen ek özellikleri belirtin. Bir Azure depolama oluşturulması gerekir veya paylaşılan depolama alanı, yoksa, hazırlama için kullanılan erişim imzası bağlı hizmeti.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| enableStaging |Veri deposunu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| linkedServiceName |Adını bir [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) bağlı hizmeti, geçici bir hazırlama deposu kullanan depolama örneğine başvurur. <br/><br/> PolyBase aracılığıyla SQL veri ambarı'na veri yüklemek için depolama ile paylaşılan erişim imzası kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlayın |
| yol |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolu belirtin. Hizmet, bir yol sağlamazsanız, geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici veri belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| Aracılığıyla |Hedefe kopyalamadan önce verilerin sıkıştırılmasının gerekli olup olmadığını belirtir. Bu ayar, aktarılan veri hacmini azaltır. |False |Hayır |

>[!NOTE]
> Hizmet, bağlantılı blob hazırlama için sıkıştırma etkin, hizmet sorumlusu veya MSI kimlik doğrulaması ile hazırlanmış kopya kullanıyorsanız desteklenmez.

Kopyalama etkinliği ile önceki tabloda açıklanan özellikler, örnek bir tanım aşağıda verilmiştir:

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

Bağlı olarak iki adımı ücretlendirilirsiniz: kopyalama süresi ve kopyalayın türü.

* Kullandığınızda veri kopyalama ve bulut kopyalama sırasında hazırlama bulut veri deposundan başka bir bulut veri deposu, hem aşamaları Azure tümleştirme çalışma zamanı tarafından sunulan, [toplam kopyalama süresi adım 1 ve 2. adım] [bulut kopyalama birim fiyatı] x ücretlendirilirsiniz.
* Kullandığınızda veri bir şirket içi veri deposundan bir bulut veri deposuna kopyalıyor, karma kopyalama sırasında hazırlama tek bir aşamada yetkilendirilmiş bir şirket içinde barındırılan tümleştirme çalışma zamanı tarafından [karma kopyalama süresi] ücretlendirilirsiniz x [karma kopyalama birim fiyatı] + [bulut kopyalama süresi] [bulut kopyalama birim fiyatı] x.

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları

Kopyalama etkinliği, Azure Data Factory hizmetine performansını ayarlamak için bu adımları uygulayın.

1. **Bir taban çizgisi oluşturun.** Geliştirme aşamasında, bir temsilci veri örnek karşı kopyalama etkinliği'ni kullanarak işlem hattınızı test etme. Yürütme ayrıntıları ve performans özelliklerini aşağıdaki toplamak [kopyalama etkinliği izleme](copy-activity-overview.md#monitoring).

2. **Tanılama ve performansını iyileştirin.** Siz gözleyin performans beklentilerinizi karşılamıyorsa, performans sorunlarını belirleyin. Ardından, kaldırmak veya performans etkisini azaltmak için performansı iyileştirin.

    Bazı durumlarda, Azure Data Factory'de bir kopyalama etkinliği'ni çalıştırdığınızda üst kısmındaki "Performans ipuçları ayarlama" iletisini görürsünüz [kopyalama etkinliği izleme sayfası](copy-activity-overview.md#monitor-visually)aşağıdaki örnekte gösterildiği gibi. İleti, belirli bir kopya çalıştırmak için tanımlanan sorunu bildirir. Bu ayrıca, hangi boost kopyalama aktarım hızını değiştirmek size yol gösterir. Performans ayarı ipuçları şu anda önerileri ister sağlar:

    - Azure SQL veri ambarı'na veri kopyalama PolyBase kullanın.
    - Kaynak veri deposu tarafında performans sorunu olduğunda Azure Cosmos DB istek birimi veya Azure SQL veritabanı dtu'ları (veritabanı performans birimleri) artırın.
    - Gereksiz hazırlanmış kopya kaldırın.

    Performans kuralları ayarlama kademeli olarak de zenginleştirilmiş.

    **Örnek: Performans ayarı ipuçları ile Azure SQL veritabanına kopyalama**

    Bu örnekte, Azure Data Factory yazma işlemlerini yavaşlatır yüksek DTU kullanımı, Azure SQL veritabanı ulaştığında havuz çalıştırın, kopyalama sırasında fark eder. Daha fazla dtu'ları ile Azure SQL veritabanı katmanı önerisi artırmaktır. 

    ![Performans ayarlama ipuçları ile izleme kopyalayın](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Ayrıca, bazı genel konular aşağıda verilmiştir. Performans Tanılama tam açıklamasını, bu makalenin kapsamı dışındadır ' dir.

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Veri Tümleştirme Birimleri](#data-integration-units)
     * [Hazırlanmış kopya](#staged-copy)
     * [Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirlik](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Şirket içinde barındırılan integration runtime](#considerations-for-self-hosted-integration-runtime)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Serileştirme ve seri durumundan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşleme](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)

3. **Tüm veri kümenize Yapılandırması'nı genişletin.** Yürütme sonuçları ve performans ile memnun kaldığınızda, tanım ve işlem hattı, veri kümesinin tamamında kapsayacak şekilde genişletebilirsiniz.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı için dikkat edilmesi gerekenler

Kopyalama etkinliği, bir şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde çalışıyorsa, aşağıdakilere dikkat edin:

**Kurulum**: Konak tümleştirme çalışma zamanı için ayrılmış bir makine kullanmanızı öneririz. Bkz: [kullanma konuları şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md).

**Ölçeği genişletme**: Bir veya daha fazla düğüme sahip bir tek mantıksal şirket içinde barındırılan tümleştirme çalışma zamanı, aynı anda aynı anda birden fazla kopyalama etkinliği çalıştırma görebilir. Karma veri hareketi çok sayıda eşzamanlı kopyalama etkinliği çalıştırma veya büyük bir kopyalamak için veri hacmi üzerinde ağır gerekmesi durumunda göz önünde bulundurun [şirket içinde barındırılan tümleştirme çalışma zamanını ölçeklendirme](create-self-hosted-integration-runtime.md#high-availability-and-scalability) daha fazla kaynak sağlamak için kopyalama güçlendirin.

## <a name="considerations-for-the-source"></a>Kaynağı için konular

### <a name="general"></a>Genel

Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını olmadığından emin olun.

Microsoft veri depoları için bkz: [izleme ve ayarlama konuları](#performance-reference) özgü veri depoları. Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

* Blob depolamadan SQL veri ambarı'na veri kopyalama performansını artırmak üzere PolyBase kullanmayı düşünün. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Verileri Azure Blob Depolama veya Azure Data Lake Store için HDFS kopyalamak performansını artırmak üzere DistCp kullanmayı düşünün. Daha fazla bilgi için [kullanım verileri HDFS kopyalamak için DistCp](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Verileri Azure SQL veri ambarı, Azure BLob depolama veya Azure Data Lake Store Redshift'ten kopyalamak kaldırma performansını artırmak üzere kullanmayı düşünün. Daha fazla bilgi için [kullanım verileri Amazon Redshift'ten kopyalamak için kaldırma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Dosya boyutu ve dosya sayısı ortalaması**: Kopyalama etkinliği, bir kerede veri bir dosya aktarır. İle aynı taşınacak veri miktarına, hizmetin genel performansını veriler nedeniyle her dosya için önyükleme aşaması birkaç büyük dosyalar yerine çok sayıda küçük dosya oluşuyorsa düşüktür. Mümkünse, daha yüksek performans elde etmek için daha büyük dosyalarına küçük dosyaları Birleştir.
* **Dosya biçimi ve sıkıştırma**: Performansı artırmak daha fazla yolu için bkz. [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) bölümler.

### <a name="relational-data-stores"></a>İlişkisel veri deposu

* **Veri modelini**: Tablo şemanızı kopyalama aktarım hızını etkiler. Büyük satır boyutu aynı miktarda veri kopyalamak için bir küçük satır boyutu daha iyi performans sağlar. Veritabanı daha az satır içeren veri daha az toplu işler daha verimli bir şekilde alabilirsiniz nedenidir.
* **Sorgu veya saklı yordam**: Kopyalama etkinliği kaynağı verileri daha verimli bir şekilde getirmek için belirttiğiniz sorgu veya saklı yordam mantığını iyileştirin.

## <a name="considerations-for-the-sink"></a>Havuz için dikkat edilmesi gerekenler

### <a name="general"></a>Genel

Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını olmadığından emin olun.

Microsoft veri depoları için bkz: [izleme ve ayarlama konuları](#performance-reference) özgü veri depoları. Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

* Herhangi bir veri deposundan Azure SQL veri ambarı'na veri kopyalama performansını artırmak üzere PolyBase kullanmayı düşünün. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Verileri Azure Blob Depolama veya Azure Data Lake Store için HDFS kopyalamak performansını artırmak üzere DistCp kullanmayı düşünün. Daha fazla bilgi için [kullanım verileri HDFS kopyalamak için DistCp](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Verileri Azure SQL veri ambarı, Azure Blob Depolama veya Azure Data Lake Store Redshift'ten kopyalamak kaldırma performansını artırmak üzere kullanmayı düşünün. Daha fazla bilgi için [kullanım verileri Amazon Redshift'ten kopyalamak için kaldırma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Kopyalama davranışı**: Farklı dosya tabanlı veri deposundan verileri kopyalarsanız, kopyalama etkinliği ile üç seçenek vardır. **copyBehavior** özelliği. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyayı birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performansa sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: Performansı artırmak daha fazla yolu için bkz. [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) bölümler.

### <a name="relational-data-stores"></a>İlişkisel veri deposu

* **Kopyalama davranışı ve performans olduğu çıkarımında**: SQL havuz veri yazmak için farklı yolu vardır. Daha fazla bilgi [açısından en iyisi, Azure SQL veritabanı'na veri yükleme](connector-azure-sql-database.md#best-practice-for-loading-data-into-azure-sql-database).

* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemanızı kopyalama aktarım hızını etkiler. Veritabanı daha verimli bir şekilde daha az toplu veri kaydedebilir miyim çünkü aynı miktarda veri kopyalamak için büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, toplu bir dizi içinde veri ekler. Bir toplu işte satır sayısını kullanarak ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satır varsa, ayarlayabileceğiniz **writeBatchSize** özellik toplu işlem ek yükü azaltır ve daha yüksek performans avantajlarından yararlanarak daha yüksek bir değere sahip. Verilerinizin satır boyutu büyükse, artırdığınızda dikkat **writeBatchSize**. Yüksek bir değere bir kopyalama hatasının veritabanı aşırı yüklenerek neden neden olabilir.

### <a name="nosql-stores"></a>NoSQL depoları

* İçin **tablo depolama**:
  * **bölüm**: Araya eklemeli bölümlere önemli ölçüde veri yazma, performansı düşürür. Verileri verimli bir şekilde bir bölüme diğerinden sonra eklenir olacak şekilde, kaynak verileri bölüm anahtarına göre sırala. Veya, tek bir bölüm için veri yazmak için mantığı ayarlayabilirsiniz.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için dikkat edilmesi gerekenler

Serileştirme ve seri durumundan çıkarma girdi veri kümesini veya çıkış veri kümesi bir dosya olduğunda ortaya çıkabilir. Kopyalama etkinliği tarafından desteklenen dosya biçimleri hakkında daha fazla bilgi için bkz. [desteklenen dosya ve sıkıştırma biçimleri](supported-file-formats-and-compression-codecs.md).

**Kopyalama davranışı**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Aynı veya hiçbir dosya biçimi ayarları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda yürüten bir *ikili kopya* serileştirme veya seri durumundan çıkarma olmadan. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek bir aktarım hızı görürsünüz.
  * Ne zaman giriş ve çıkış veri kümeleri hem metin biçimi ve yalnızca kodlama olan türü farklı olmadığı, veri taşıma Hizmeti'nde yalnızca kodlama dönüştürme yapar. Herhangi bir serileştirme yapmaz ve seri durumdan yükü ikili kopyasını kıyasla bazı performans neden olur.
  * Farklı dosya biçimleri veya sınırlayıcılar gibi farklı yapılandırmaları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda akışı, dönüştürme ve belirttiğiniz çıkış biçimine seri hale için kaynak verileri seri durumdan çıkarır. Bu işlem ek yükü diğer senaryolarda kıyasla çok daha önemli bir performans sonuçlanır.
* Veya, örneğin, bir ilişkisel bir depolama dosya tabanlı depolama alanından dosya tabanlı olmayan bir veri deposundaki dosyaları kopyalayın. serileştirme veya seri durumundan çıkarma adım gereklidir. Bu adım, önemli performans ek yükü oluşur.

**Dosya biçimi**: Seçtiğiniz dosya biçimi kopyalama performansı etkileyebilir. Örneğin, Avro verileriyle meta verileri depolar sıkıştırılmış bir ikili biçimi ' dir. Hadoop ekosistemindeki işleme ve sorgulama için geniş destek vardır. Avro için serileştirme ve seri kaldırma metin biçimine kıyasla daha düşük kopyalama aktarım hızı sonuçlanır daha pahalıdır. 

Dosya biçimi işleme akışı boyunca tercih ettiğiniz bütünlüklü olarak yapın. Başlangıcı:

- Hangi veri formu, kaynak veri depolarını veya dış sistemlerden ayıklanacak depolanır.
- Depolama, analitik işleme ve sorgulama için en iyi biçimi.
- Hangi veri uygulamasına raporlama ve görselleştirme araçları için veri reyonları formatında.

Bazen için yetersiz bir dosya biçimi okuma ve yazma performansını genel analitik işlemi düşünürken iyi bir seçim olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma dikkate alınacak noktalar

Giriş veya çıkış veri kümesi bir dosya olduğunda, hedef konuma verileri yazar gibi sıkıştırma veya sıkıştırmayı açma gerçekleştirmek için kopyalama etkinliği ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir denge duruma ve CPU. Bilgi işlem kaynaklarının ek veri maliyetlerini sıkıştırma. Ancak, ağ g/ç ve depolama azaltır. Verilere bağlı olarak bir artırma genel kopyalama aktarım hızının görebilirsiniz.

**Codec**: Her bir sıkıştırma codec avantajları vardır. Örneğin, bzıp2 düşük kopyalama aktarım hızına sahip, ancak işleme için bölme çünkü bzıp2 ile en iyi Hive sorgu performansı elde. Gzip en dengeli bir seçenektir ve en sık kullanılan. Uçtan uca senaryonuza uygun codec bileşeni seçin.

**Düzey**: Her bir sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: hızlı sıkıştırılmış ve verimli sıkıştırılmış. Sonuç dosyası en uygun şekilde sıkıştırılmaz bile sıkıştırılmış en hızlı seçenek, mümkün olan en kısa sürede verileri sıkıştırır. En uygun şekilde sıkıştırılmış seçeneği sıkıştırma üzerinde daha fazla zaman harcadığını ve en az miktarda veri ortaya çıkarır. İki seçenek de durumunuzdaki genel daha iyi performans sağlayan görmek için test edebilirsiniz.

**Önemli bir unsur**: Büyük miktarda bir şirket içi depolama ile bulut arasında veri kopyalamak için kullanmayı [kopyalama aşamalı](#staged-copy) ile sıkıştırma etkin. Geçici depolama kullanarak şirket ağınıza ve Azure hizmetlerinizi bant sınırlayan faktör olan ve giriş veri kümesi istediğiniz ve sıkıştırılmamış biçiminde olması için çıktı veri kümesi hem faydalıdır.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için dikkat edilmesi gerekenler

Ayarlayabileceğiniz **Bunun amacı** tüm harita veya çıkış sütunları için giriş sütun alt kümesi bir kopyalama etkinliği özelliği. Veri taşıma Hizmeti'nde veri kaynağından okur. sonra havuz için verileri yazar önce veriler üzerinde sütun eşleme gerçekleştirmek gerekir. Bu ek işlem kopyalama performansını düşürür.

Kaynak veri deponuzda sorgulanabilir, örneğin, SQL veritabanı veya SQL Server gibi ilişkisel bir mağaza veya tablo depolama veya Azure Cosmos DB gibi bir NoSQL deposu olması durumunda sütun filtreleme gönderme ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özellik. Veri taşıma Hizmeti'nde çok daha verimli olduğu kaynak veri deposundan veri okurken bu şekilde yansıtma gerçekleşir.

Daha fazla bilgi [kopyalama etkinliği şema eşleme](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Kopyalamak istediğiniz veri boyutu büyükse, sizin işlerinize verileri diğer bölümlere ayırmak için ayarlayabilirsiniz. Kopyalama etkinliği çalıştıran her bir kopyalama etkinliği için veri boyutunu düşürmek için daha sık çalışacak şekilde zamanlayabilirsiniz.

Birçok veri kümesi hakkında dikkatli olmanız ve Azure Data Factory, aynı anda aynı veri deposuna bağlanmak gerekli etkinlikler kopyalayın. Birçok eş zamanlı kopyası işleri bir veri deposu azaltma ve performansın düşmesine neden, kopyalama işi iç deneme ve bazı durumlarda, yürütme hatalarını sağlama.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: Bir şirket içi SQL Server'dan Blob depolamaya kopyalama

**Senaryo**: Bir işlem hattı, bir şirket içi SQL Server'dan Blob Depolama alanında CSV biçimi veri kopyalamak için oluşturulmuştur. Kopyalama işini daha hızlı hale getirmek için CSV dosyaları bzıp2 biçimine sıkıştırılmasının gerekli.

**Test ve analiz**: Performans Kıyaslama yavaş olduğu değerinden 2 MB/sn aktarım hızı kopyalama etkinliği var.

**Performans Analizi ve ayarlama**: Performans sorunu gidermek için nasıl veri işlenen taşınır ve adresindeki gözden geçirelim.

- **Veri Okuma**: Tümleştirme çalışma zamanının SQL Server için bir bağlantı açar ve sorguyu gönderir. SQL Server Integration runtime intranet üzerinden veri akışını göndererek yanıt verir.
- **Seri hale getirmek ve verileri sıkıştırmak**: Integration runtime, veri akışı CSV biçimine serileştiren ve bzıp2 akış verileri sıkıştırır.
- **Veri yazma**: Tümleştirme çalışma zamanının bzıp2 akış Internet üzerinden Blob depolamaya yükler.

Gördüğünüz gibi veriler işlenir ve akış sıralı bir şekilde taşındı: SQL Server > LAN > Integration runtime > WAN > Blob Depolama. Genel performansını en düşük aktarım hızı tarafından ardışık düzeni işlenir.

![Veri akışı](./media/copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla aşağıdaki faktörleri performans sorunu neden olabilir:

* **Kaynak**: SQL Server kendisini aktarım hızının düşük olmasını nedeniyle ağır yükleri vardır.
* **Barındırılan tümleştirme çalışma zamanını**:
  * **LAN**: Integration runtime gölgeden uzak SQL Server makinesinde bulunduğu yer ve düşük bant genişliğine sahip bağlantısı vardır.
  * **Integration runtime**: Tümleştirme çalışma zamanı aşağıdaki işlemleri gerçekleştirmek için kendi yük sınırlamaları ulaştı:
    * **Serileştirme**: CSV biçimi için veri akışı seri hale getirme, yavaş aktarım hızına sahip.
    * **Sıkıştırma**: 2.8 MB/sn ile Core i7 olduğu yavaş sıkıştırma codec, örneğin, bzıp2 seçtiğiniz.
  * **WAN**: Örneğin, T1, kurumsal ağ ve Azure hizmetlerinizi arasındaki bant düşüktür; 1,544 kbps = T2 6,312 kbps =.
* **Havuz**: BLOB Depolama, aktarım hızının düşük olmasını sahiptir. En az 60 MB/sn, hizmet düzeyi sözleşmesinin (SLA) garanti eder, çünkü bu senaryo düşüktür.

Bu durumda, bzıp2 veri sıkıştırma tüm işlem hattını yavaşlatmasını. Gzip sıkıştırma codec bileşenine geçiş, bu performans sorunu kolaylaştırmak.

## <a name="references"></a>Başvurular

Performans izleme ve desteklenen veri depolarının bazılarını başvuruları ayarlama şunlardır:

* Azure depolama, BLOB ve tablo depolama içerir: [Azure depolama ölçeklenebilirlik hedefleri](../storage/common/storage-scalability-targets.md) ve [Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../storage/common/storage-performance-checklist.md).
* Azure SQL Veritabanı: Yapabilecekleriniz [performansını izleme](../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimi (DTU) yüzde değerini denetleyin.
* Azure SQL veri ambarı: Kendi özelliği, veri ambarı birimi (Dwu) ölçülür. Bkz: [yönetme, Azure SQL veri ambarı (genel bakış) güç işlem](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Azure Cosmos DB'de performans düzeyleri](../cosmos-db/performance-levels.md).
* Şirket içi SQL Server: [İzleme ve performansı ayarlama](https://msdn.microsoft.com/library/ms189081.aspx).
* Şirket içi dosya sunucusu: [Dosya sunucuları için performans ayarlama](https://msdn.microsoft.com/library/dn567661.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği şema eşleme](copy-activity-schema-and-type-mapping.md)
- [Kopyalama etkinliği hataya dayanıklılık](copy-activity-fault-tolerance.md)
