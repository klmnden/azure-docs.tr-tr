---
title: "Etkinlik performans ve ayarlama Kılavuzu kopyalama | Microsoft Docs"
description: "Kopyalama etkinliği kullandığınızda, Azure Data factory'de veri taşımayı performansını etkileyen anahtar Etkenler hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 2bec612b1d67eceb0e62b28524b98e852d31ad0f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Etkinlik performans ve ayarlama Kılavuzu kopyalayın
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [kopyalama etkinliği performansı ve veri fabrikası sürüm 2 için ayarlama Kılavuzu](../copy-activity-performance.md).

Azure Data Factory kopyalama etkinliği çözümü bir birinci sınıf güvenli, güvenilir ve yüksek performanslı veri yükleme sunar. Zengin bulut çeşitli her gün terabayt veri onlarca kopyaya sağlar ve şirket içi veri depolarına. Üstün hızlı veri performans yükleme anahtarıdır odaklanmak çekirdek "büyük veri" sorunu emin olmak için: Gelişmiş analiz çözümleri oluşturmak ve tüm bu verilerden ayrıntılı Öngörüler alma.

Azure veri depolama ve veri ambarı çözümleri Kurumsal düzeyde bir dizi sağlar ve yüksek oranda iyileştirilmiş bir veri yapılandırmak ve ayarlamak kolay deneyimi yükleme kopyalama etkinliği sunar. Yalnızca tek kopyalama etkinliği ile elde edebilirsiniz:

* Verileri yükleme **Azure SQL Data Warehouse** adresindeki **1,2 GB/sn**. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).
* Verileri yükleme **Azure Blob Depolama** adresindeki **1.0 GB/sn**
* Verileri yükleme **Azure Data Lake Store** adresindeki **1.0 GB/sn**

Bu makalede açıklanır:

* [Performans referans numaraları](#performance-reference) desteklenen projenizi; planlamanıza yardımcı olması için kaynak ve havuz veri depoları
* Kopya işleme dahil olmak üzere farklı senaryolarda artırabilir özellikleri [bulut veri taşıma birimleri](#cloud-data-movement-units), [paralel kopyalama](#parallel-copy), ve [kopyalama hazırlanan](#staged-copy);
* [Performans ayarlama yönergeleri](#performance-tuning-steps) performans ve kopyalama performansını etkileyebilir anahtar Etkenler ayarlama konusunda.

> [!NOTE]
> Genel kopyalama etkinliği ile bilmiyorsanız, bkz: [Kopyala etkinliğini kullanarak veri taşıma](data-factory-data-movement-activities.md) bu makaleyi okumadan önce.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru olarak aşağıdaki tabloyu şirket içi testlere göre verilen kaynak ve havuz çiftleri için MB/sn olarak kopyalama üretilen iş sayısını gösterir. Karşılaştırma için ayrıca farklı ayarlarını gösterir [bulut veri taşıma birimleri](#cloud-data-movement-units) veya [veri yönetimi ağ geçidi ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md) (birden çok ağ geçidi düğümler) kopyalama performansına yardımcı olabilir.

![Performans Matrisi](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>Azure Data Factory sürüm 1, en az bulut veri taşıma Bulutu bulut kopyalama birimidir iki. Belirtilmezse, varsayılan veri taşıma birimleri kullanıldığına bakın [bulut veri taşıma birimleri](#cloud-data-movement-units).

**Dikkat edilecek noktalar:**
* Verimlilik, aşağıdaki formül kullanılarak hesaplanır: [verilerin boyutu okuma kaynağından] / [kopyalama çalışma süresini etkinliği].
* Tabloda yer alan performans başvuru numaralarını kullanarak ölçülen [TPC-H](http://www.tpc.org/tpch/) veri kümesi bir tek kopyalama etkinliğinde çalıştırın.
* Azure veri depolarında kaynak ve havuz aynı Azure bölgesinde ' dir.
* Karma kopyalama şirket içi ve bulut arasında veri depolarına, her ağ geçidi düğümü altındaki belirtimi ile şirket içi veri deposundan ayrı bir makinede çalışıyordu. Tek bir etkinliğin ağ geçidinde çalıştırırken, kopyalama işlemi yalnızca küçük bir bölümü test makinenin CPU, bellek veya ağ bant genişliği tüketilen. ' Dan daha fazla bilgi edinin [göz önünde bulundurarak veri yönetimi ağ geçidi için](#considerations-for-data-management-gateway).
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
        <td>Internet interface: 10 Gbps; intranet interface: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> Varsayılandan daha fazla veri taşıma birimleri (DMUs) yararlanarak daha yüksek verimlilik elde edebilirsiniz 32 Bulut Bulut kopyalama etkinliği çalıştırmak için en fazla DMUs. Örneğin, 100 DMUs ile Azure Blob kopyalama verileri Azure Data Lake Store içine elde edebilirsiniz **1.0GBps**. Bkz: [bulut veri taşıma birimleri](#cloud-data-movement-units) bu özellik ve desteklenen senaryo hakkındaki ayrıntılar için bölüm. Kişi [Azure Destek](https://azure.microsoft.com/support/) daha fazla DMUs istemek için.

## <a name="parallel-copy"></a>Paralel kopyalama
Veri kaynağından okumak veya hedef veri yazma **çalıştırmak kopyalama etkinliği içinde paralel**. Bu özellik, bir kopyalama işlemi verimliliğini artırır ve verilerin taşınması için gereken süreyi azaltır.

Bu ayar farklıdır **eşzamanlılık** etkinlik tanımının bir özellik. **Eşzamanlılık** özelliği sayısını belirler **eşzamanlı kopyalama etkinliği çalışır** farklı etkinlik Windows (1 AM için 2: 00, 2 AM 3 AM, AM 3 ve 4 AM vb.) verileri işlemek için. Bu özellik, geçmiş bir yükleme gerçekleştirmek yararlıdır. Paralel kopyasını yetenek uygulandığı bir **tek Etkinlik**.

Bir örnek senaryo bakalım. Aşağıdaki örnekte, birden çok dilim Geçmişten işlenmesi gerekir. Data Factory kopyalama etkinliği (etkinlik Çalıştır) bir örneği her dilim için çalışır:

* Veri dilimi ilk etkinliği penceresinden (1 AM için 2: 00) == > etkinliği çalıştırmak 1
* Veri dilimi ikinci etkinliği penceresinden (2: 00 için 3 AM) == > etkinliği çalıştırmak 2
* Veri dilimi ikinci etkinliği penceresinden (3 AM için 4 AM) == > etkinliği çalıştırmak 3

Etki alanları bu hiyerarşi sıralamasıyla devam eder.

Bu örnekte, zaman **eşzamanlılık** değeri 2'ye ayarlanır **etkinliği çalıştırmak 1** ve **etkinliği çalıştırmak 2** iki etkinlik Windows'dan veri kopyalama **eşzamanlı olarak** veri taşıma performansını artırmak için. Birden çok dosya 1 Çalıştır etkinliği ile ilişkiliyse, ancak veri taşıma hizmeti dosyaları kaynak sunucudan hedef bir dosya için birer birer kopyalar.

### <a name="cloud-data-movement-units"></a>Bulut veri taşıma birimleri
A **bulut veri taşıma birimi (DMU)** veri fabrikası'nda tek bir birimi (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçüdür. DMU uygun Bulut Bulut kopyalama işlemleri için ancak bir karma kopyalama.

**Kopyalama etkinliği çalıştırmak güçlendirmeniz en az bulut veri taşıma birimleri iki olur.** Belirtilmezse, aşağıdaki tabloda farklı kopyalama senaryosunda kullanılan varsayılan DMUs listelenmektedir:

| Kopyalama senaryosu | Hizmeti tarafından belirlenen varsayılan DMUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | 2 ile 16 sayısı ve dosya boyutuna bağlı olarak arasında. |
| Diğer tüm kopyalama senaryoları | 2 |

Bu varsayılanı geçersiz kılmak için için bir değer belirtin **cloudDataMovementUnits** şekilde özelliği. **İzin verilen değerler** için **cloudDataMovementUnits** özelliği olan 2, 4, 8, 16 ve 32. **Bulut DMUs gerçek sayısını** eşit veya bu değerden azsa yapılandırılan, veri deseni bağlı olarak, kopyalama işlemini çalışma zamanında kullanır. Daha fazla birimi belirli kopya kaynak ve havuz için yapılandırdığınızda alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
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

> [!NOTE]
> Daha fazla bulut DMUs için daha yüksek işleme gerekiyorsa başvurun [Azure Destek](https://azure.microsoft.com/support/). 8 ayarlama ve yukarıdaki şu anda yalnızca çalışır, **Blob Depolama/Data Lake Store/Azure için Blob Depolama/Data Lake Store/Amazon S3/bulut FTP/bulut SFTP birden çok dosya kopyalama SQL veritabanı**.
>

### <a name="parallelcopies"></a>parallelCopies
Kullanabileceğiniz **parallelCopies** kullanmak için kopyalama etkinliği istediğiniz paralellik belirtmek için özelliği. Bu özelliğin kaynağınızdan okuma veya paralel, havuz veri depolarında yazma kopyalama etkinliği içinde iş parçacığı sayısı olarak düşünebilirsiniz.

Her kopya Çalıştır etkinliği için veri fabrikası veri depolamak ve hedef veri depolamak kaynaktan veri kopyalamak için kullanılacak paralel kopya sayısını belirler. Kullandığı paralel kopya varsayılan sayısını kaynak ve kullandığınız havuz türüne bağlıdır.  

| Kaynak ve havuz | Hizmeti tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı depolar (Blob storage; arasında veri kopyalama Data Lake Store; Amazon S3; bir şirket içi dosya sistemi; bir şirket içi HDFS) |1 ile 32 arasında. İki bulut veri depolarını veya fiziksel yapılandırma (veya bir şirket içi veri deposundaki verileri kopyalamak için) bir karma kopyalama için kullanılan ağ geçidi makinesinin arasında veri kopyalamak için kullanılan dosyalarının boyutunu ve bulut veri taşıma birimlerinin (DMUs) bağlıdır. |
| Veri kopyalamak **tüm kaynak veri deposu Azure tablo depolaması** |4 |
| Diğer tüm kaynak ve havuz çiftleri |1 |

Genellikle, varsayılan davranışı en iyi verimlilik vermesi gerekir. Ancak, verilerinizi barındıran makineler üzerindeki yükü denetlemek için depolar veya kopya performansı ayarlamak için varsayılan değeri geçersiz kılmak ve için bir değer belirtmek seçebilirsiniz **parallelCopies** özelliği. Değer 1 ile 32 (her ikisi de dahil) arasında olmalıdır. Çalışma zamanında, en iyi performans için kopyalama etkinliği ayarladığınız değerine eşit veya daha küçük bir değer kullanır.

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 8
        }
    }
]
```
Dikkat edilecek noktalar:

* Veri depoları, dosya tabanlı arasında kopyaladığınızda **parallelCopies** dosya düzeyinde paralellik belirleyin. Tek bir dosyada Öbekleme altında otomatik ve şeffaf şekilde olacağını ve verileri paralel ve parallelCopies resme yüklemek için belirtilen kaynak veri deposu türü için en iyi uygun öbek boyutu kullanmak için tasarlanmıştır. Gerçek çalışma zamanında kopyalama işlemi için veri taşıma hizmeti kullanan paralel kopyaları sahip dosyaların sayısı en çok sayısıdır. Kopyalama davranışını ise **mergeFile**, kopyalama etkinliği, dosya düzeyinde paralellik avantajlarından alamaz.
* İçin bir değer belirtirseniz **parallelCopies** özelliği, bir karma kopyalama ise yük arttıkça, kaynak ve havuz veri depoları ve ağ geçidi için göz önünde bulundurun. Bu, özellikle birden çok etkinlikleri veya aynı veri deposu karşı çalıştırmak aynı etkinliklerin eşzamanlı çalışır olduğunda gerçekleşir. Veri deposu ya da ağ geçidi yük ile doludur fark ederseniz, azaltma **parallelCopies** yük hafifletmek için değer.
* Dosya tabanlı depoları için dosya tabanlı olmayan depoları veri kopyalama, veri taşıma Hizmeti'nde yoksayar **parallelCopies** özelliği. Paralellik belirtilmiş olsa bile, bu durumda uygulanmıyor.

> [!NOTE]
> Kullanmak için veri yönetimi ağ geçidi 1.11 veya sonraki bir sürümü kullanmanız gerekir **parallelCopies** karma kopyalama yaptığınızda özelliği.
>
>

Bu iki özellik daha iyi kullanımı ve veri taşıma verimliliği artırmak için bkz: [örnek kullanım durumları](#case-study-use-parallel-copy). Yapılandırmanız gerekmez **parallelCopies** varsayılan davranışı yararlanmak için. Yapılandırırsanız ve **parallelCopies** birden çok bulut DMUs değil tam olarak kullanılan çok küçük.  

### <a name="billing-impact"></a>Faturalama etkisi
Bunun **önemli** tabanlı kopyalama işlemi toplam zamanında ücretlendirilen unutmayın. Bir kopyalama işi bir saat bir bulut birimle çekmek için kullanılan ve şimdi dört bulut birimleri ile 15 dakika sürer, genel fatura neredeyse aynı kalır. Örneğin, dört bulut birimleri kullanın. İlk bulut birimi 10 dakika, ikincisi harcadığı 10 dakika, 5 dakika, üçüncü bir ve 5 dakika, dördüncü bir tüm bir kopyalama etkinliğinde çalıştırın. 10 + 10 + 5 + 5 = 30 dakika toplam copy (veri taşıma) saat için sizden ücret kesilir. Kullanarak **parallelCopies** faturalama etkilemez.

## <a name="staged-copy"></a>Hazırlanan kopyalama
Bir havuz veri deposu için bir kaynak veri deposundan verileri kopyaladığınızda, geçici bir hazırlama deposu olarak Blob storage kullanmayı seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

1. **PolyBase aracılığıyla SQL veri ambarında verileri çeşitli veri depoları alma istediğiniz**. SQL veri ambarı PolyBase büyük miktarda veri SQL Data Warehouse'a veri yükleme için yüksek verimlilik mekanizması olarak kullanır. Ancak, veri kaynağını Blob depolama alanına olmalıdır ve ek ölçütleri karşılaması gerekir. Dışında bir Blob Depolama veri deposundan veri yüklediğinizde, verileri aracılığıyla geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda, veri fabrikası PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümleri gerçekleştirir. Ardından SQL Data Warehouse'a veri yüklemek için PolyBase kullanır. Daha fazla ayrıntı için bkz: [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).
2. **Bazen bir karma veri taşımayı gerçekleştirmek için biraz zaman alır (diğer bir deyişle, arasında bir şirket içi veri kopyalamak için depolama ve bulut verilerini depolama) bir yavaş ağ bağlantısı üzerinden**. Performansı artırmak için bulut hazırlama veri deposunda veri taşımak için daha az zaman alır, verileri şirket içi sıkıştırabilirsiniz. Hedef veri deposuna yükleme önce hazırlama deposundaki verileri sıkıştırmasını.
3. **Bağlantı noktası 80 dışındaki bağlantı noktaları açın ve kurumsal BT ilkeleri nedeniyle, Güvenlik Duvarı'nda 443 numaralı bağlantı istemediğiniz**. Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL Data Warehouse havuz için bir şirket içi veri deposundan veri kopyaladığınızda, hem Windows Güvenlik Duvarı hem de kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimi etkinleştirmeniz gerekir. Bu senaryoda, ilk veri Kopyala ağ geçidine bir Blob Depolama hazırlama örneğine HTTP veya HTTPS üzerinden bağlantı noktası 443 üzerinde yararlanın. Sonra verileri Blob Depolama hazırlamadan SQL Database veya SQL Data Warehouse yüklemek. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopyalama çalışır
Hazırlama özelliği etkinleştirdiğinizde, ilk veri kaynağı veri deposundan hazırlama veri deposuna kopyalanır (Getir kendi). Ardından, verileri hazırlama veri deposundan havuz veri deposuna kopyalanır. Veri Fabrikası iki aşamalı akış sizin için otomatik olarak yönetir. Veri taşıma işlemi tamamlandıktan sonra veri fabrikası aynı zamanda hazırlama depolama biriminden geçici verileri temizler.

Bulut kopyalama senaryosunda (bulutta depoları olan kaynak ve havuz veri) ağ geçidi kullanılmaz. Data Factory hizmetinin kopyalama işlemleri gerçekleştirir.

![Kopya hazırlanan: Bulut senaryosu](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Karma kopyalama senaryoda (şirket içi bir kaynaktır ve havuz olduğu bulutta), ağ geçidi veri kaynağına veri deposundan hazırlama bir veri deposuna taşır. Data Factory Hizmeti'ne havuz veri deposuna hazırlama veri deposundan verileri taşır. Bir şirket içi veri depolama hazırlama yoluyla bir bulut veri deposundan veri kopyalama ile ters akışını de desteklenir.

![Kopya hazırlanan: karma senaryo](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Hazırlama depolama kullanarak veri taşıma etkinleştirdiğinizde, verileri veri kaynağına veri deposundan bir geçiş veya hazırlama veri deposuna taşımadan önce sıkıştırılır ve bir geçiş verilerini taşıma veya veri deposu havuz veri deposuna hazırlama önce sıkıştırması açılmış isteyip istemediğinizi belirtebilirsiniz.

Şu anda, bir hazırlama deposu kullanarak iki şirket içi veri depoları arasında veri kopyalanamıyor. Hemen kullanılabilir olması için bu seçeneği bekliyoruz.

### <a name="configuration"></a>Yapılandırma
Yapılandırma **enableStaging** bir hedef veri deposuna yükleme önce Blob depolama alanına aşamalı için verileri isteyip istemediğinizi belirtmek için kopyalama etkinliği ayarlama. Ayarladığınızda **enableStaging** TRUE, sonraki tabloda listelenen ek özellikler belirtmek için. Yoksa, depolama paylaşılan erişim imzası bağlantılı hizmeti hazırlama için veya bir Azure Storage oluşturmak gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| **enableStaging** |Veri deposu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| **linkedServiceName** |Adını belirtin bir [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bağlı bir geçici hazırlama deposu olarak kullanmak depolama örneğinin başvurduğu hizmeti. <br/><br/> PolyBase aracılığıyla SQL veri ambarında verileri yüklemek için depolama ile paylaşılan erişim imzası kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, ne zaman **enableStaging** TRUE olarak ayarlayın |
| **yol** |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolunu belirtin. Bir yol belirtmezseniz, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici verilerin belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| **enableCompression** |Hedefe kopyalamadan önce verilerin sıkıştırılmasının gerekli olup olmadığını belirtir. Bu ayar aktarılan veri hacmini azaltır. |False |Hayır |

Kopyalama etkinliği yukarıdaki tabloda açıklanan özelliklere sahip bir örnek tanımı aşağıda verilmiştir:

```json
"activities":[  
{
    "name": "Sample copy activity",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDBOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlSink"
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob",
            "path": "stagingcontainer/path",
            "enableCompression": true
        }
    }
}
]
```

### <a name="billing-impact"></a>Faturalama etkisi
Temel alınarak iki adımı ücretlendirilen: süresi kopyalama ve kopyalama türü.

* (Verileri bir bulut veri deposundan başka bir bulut veri deposuna kopyalama) bulut kopyalama sırasında Hazırlama [kopyalama sürenin toplamı adım 1 ve 2. adım] [bulut kopya Birim Fiyat] x kullandığınızda ücretlendirilirsiniz.
* Kullandığınızda (bir bulut veri deposuna bir şirket içi veri deposundan veri kopyalama) karma kopyalama sırasında hazırlama, [karma kopyalama süresince] ücretlendirilen x [karma kopya Birim Fiyat] + [bulut kopyalama süre] [bulut kopya Birim Fiyat] x.

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları
Veri Fabrikası hizmetiniz kopyalama etkinliği ile performansı ayarlamak için şu adımları uygulamanız öneririz:

1. **Bir taban çizgisi oluşturma**. Geliştirme aşamasında, temsili veri örneği karşı kopyalama etkinliği kullanarak hattınızı sınayın. Veri Fabrikası kullanabilirsiniz [modeli dilimleme](data-factory-scheduling-and-execution.md) ile çalışırsınız veri miktarını sınırlamak için.

   Yürütme süresi ve performans özelliklerini kullanarak toplamak **izleme ve yönetim uygulaması**. Seçin **İzleyici & Yönet** Data Factory giriş sayfası. Ağaç görünümünde seçin **çıkış dataset**. İçinde **etkinlik Windows** listesinde, çalıştırmak kopyalama etkinliği seçin. **Etkinlik pencerelerine** kopyalama etkinlik süresi ve kopyalanan verilerin boyutuna göre listeler. Üretilen iş listelenen **etkinlik penceresini Explorer**. Uygulama hakkında daha fazla bilgi için bkz: [İzleyici ve Azure Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetme](data-factory-monitor-manage-app.md).

   ![Etkinlik çalışma ayrıntıları](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Makalenin sonraki bölümlerinde, performans ve senaryonuz için kopyalama etkinliği'nin yapılandırılmasını karşılaştırabilirsiniz [Performans başvurusu](#performance-reference) bizim testlerden.
2. **Tanılama ve performansı en iyi duruma**. Gözlemlemek performans beklentilerinizi karşılamıyorsa, performans sorunlarını tanımlamak gerekir. Ardından, kaldırmak veya performans etkisini azaltmak için performansı en iyi duruma. Performans Tanılama ilgili tam açıklama için bu makalenin kapsamı dışındadır olsa da, bazı ortak noktalar şunlardır:

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Bulut veri taşıma birimleri](#cloud-data-movement-units)
     * [Hazırlanan kopyalama](#staged-copy)
     * [Veri Yönetimi ağ geçidi ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Veri Yönetimi Ağ Geçidi](#considerations-for-data-management-gateway)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Seri hale getirme ve seri durumdan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşlemesi](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)
3. **Tüm Veri kümenizi Yapılandırması**. Yürütme sonuçları ve performans ile memnun kaldığınızda, tanım ve ardışık düzen etkin süresini, tüm veri kümelerinin kapsayacak şekilde genişletebilirsiniz.

## <a name="considerations-for-data-management-gateway"></a>Veri Yönetimi ağ geçidi için ilgili önemli noktalar
**Ağ geçidi Kurulum**: ana bilgisayar veri yönetimi ağ geçidi için ayrılmış bir makine kullanmanızı öneririz. Bkz: [veri yönetimi ağ geçidi kullanarak dikkat edilmesi gereken noktalar](data-factory-data-management-gateway.md#considerations-for-using-gateway).  

**Ağ geçidi izleme ve yukarı/genişleme**: tek bir mantıksal ağ geçidi ile bir veya daha fazla ağ geçidi düğümleri aynı anda aynı anda birden çok kopya etkinliği çalıştırır hizmet verebilir. Bir ağ geçidi makinesi sınırı Azure portalında karşı çalışan eşzamanlı iş sayısını görmek olarak iyi kaynak kullanımı (CPU, bellek, network(in/out), vb.) neredeyse gerçek zamanlı görüntüsünü görüntüleyebilirsiniz [İzleyici ağ geçidi portalında](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). İçin çok sayıda eş zamanlı kopyalama etkinliği çalışır veya büyük miktarda veri kopyalamak için karma veri taşıma üzerinde ağır gereksiniminiz varsa göz önünde bulundurun [ölçeği veya ağ geçidi ölçeklendirmek](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) kopyalama güçlendirmeniz daha fazla kaynak sağlamak için ya da daha iyi kaynağınız kullanmalarını amacıyla. 

## <a name="considerations-for-the-source"></a>Kaynak için ilgili önemli noktalar
### <a name="general"></a>Genel
Temel alınan veri deposu veya onu karşı çalışan diğer iş yükleri tarafından doludur değil olduğunu unutmayın.

Microsoft veri depoları için bkz: [izleme ve konuları ayarlama](#performance-reference) özgü veri depoları ve veri performans özellikleri depolamak, yanıt sürelerini en aza indirmek ve üretilen işi en üst düzeye anlamanıza yardım.

SQL veri ambarı için Blob depolama alanından verileri kopyalarsanız, kullanmayı **PolyBase** performansını artırma. Bkz: [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob Depolama, Data Lake Store, Amazon S3, şirket içi dosya sistemleri ve şirket içi HDFS içerir)*

* **Ortalama dosya boyutu ve dosya sayısı**: kopyalama etkinliği, aynı anda bir veri dosyası aktarır. İle aynı taşınacak veri miktarı, genel üretilen işi verileri her dosya için önyükleme aşaması nedeniyle birkaç büyük dosyalar yerine pek çok küçük dosya oluşuyorsa düşüktür. Bu nedenle, mümkünse, küçük dosyalar daha yüksek verimlilik elde etmek için daha büyük dosyalar birleştirin.
* **Dosya biçimi ve sıkıştırma**: performansını artırmak diğer yolları için bkz: [seri hale getirme ve seri durumundan çıkarma için ilgili önemli noktalar](#considerations-for-serialization-and-deserialization) ve [sıkıştırma için ilgili önemli noktalar](#considerations-for-compression) bölümler.
* İçin **şirket içi dosya sistemi** senaryosunda, hangi **veri yönetimi ağ geçidi** olan gerekli bkz [veri yönetimi ağ geçidi için ilgili önemli noktalar](#considerations-for-data-management-gateway) bölümü.

### <a name="relational-data-stores"></a>İlişkisel veri depoları
*(SQL veritabanı içerir; SQL veri ambarı; Amazon Redshift; SQL Server veritabanları; ve Oracle, MySQL, DB2, Teradata, Sybase ve PostgreSQL veritabanları, vs.)*

* **Veri deseni**: Tablo şemanızı kopyalama verimlilik etkiler. Büyük satır boyutu küçük satır boyutu, aynı miktarda veri kopyalamak için daha iyi performans sağlar. Veritabanında daha az sayıda satır içeren veri daha az toplu daha verimli bir şekilde alabilir nedenidir.
* **Sorgu veya saklı yordam**: sorgu veya saklı yordam verileri daha verimli bir şekilde getirmek için kopyalama etkinliği kaynağında belirttiğiniz mantığını en iyi duruma getirme.
* İçin **içi ilişkisel veritabanları**, SQL Server ve kullanılmasını Oracle gibi **veri yönetimi ağ geçidi**, bkz: [veri yönetimi ağ geçidi için ilgili önemli noktalar](#considerations-on-data-management-gateway) bölümü.

## <a name="considerations-for-the-sink"></a>Havuz için ilgili önemli noktalar
### <a name="general"></a>Genel
Temel alınan veri deposu veya onu karşı çalışan diğer iş yükleri tarafından doludur değil olduğunu unutmayın.

Microsoft veri depoları için başvurmak [izleme ve konuları ayarlama](#performance-reference) özgü verileri depolar. Bu konularda veri deposu performans özellikleri ve yanıt sürelerini en aza indirmek ve üretilen işi en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

Verileri kopyalıyorsanız **Blob storage** için **SQL veri ambarı**, kullanmayı **PolyBase** performansını artırma. Bkz: [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob Depolama, Data Lake Store, Amazon S3, şirket içi dosya sistemleri ve şirket içi HDFS içerir)*

* **Kopyalama davranışı**: farklı dosya tabanlı veri deposundan verileri kopyalarsanız, kopyalama etkinliği ile üç seçenek vardır **copyBehavior** özelliği. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyaları birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performans yüküne sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: bkz [seri hale getirme ve seri durumundan çıkarma için ilgili önemli noktalar](#considerations-for-serialization-and-deserialization) ve [sıkıştırma için ilgili önemli noktalar](#considerations-for-compression) performansını artırmak diğer yolları için bölümler.
* **BLOB Depolama**: şu anda, Blob Depolama destekler yalnızca en iyi duruma getirilmiş veri aktarımı ve verimlilik için blok.
* İçin **şirket içi dosya sistemleri** kullanımını gerektiren senaryolar **veri yönetimi ağ geçidi**, bkz: [veri yönetimi ağ geçidi için ilgili önemli noktalar](#considerations-for-data-management-gateway) bölümü.

### <a name="relational-data-stores"></a>İlişkisel veri depoları
*(SQL veritabanı, SQL Data Warehouse, SQL Server veritabanları ve Oracle veritabanları içerir)*

* **Davranış kopyalama**: özellikleri için ayarlanmış bağlı olarak **sqlSink**, kopyalama etkinliği farklı şekillerde hedef veritabanına veri yazar.
  * Varsayılan olarak, veri taşıma hizmeti kullanan veri eklemek için toplu kopyalama API en iyi performans sağlayan moda ekleyin.
  * Saklı yordam havuzunda yapılandırırsanız, veritabanı veri bir satır yerine bir anda bir toplu yükleme olarak uygulanır. Performansı önemli ölçüde bırakır. Veri kümenizi uygulanabilir olduğunda büyükse, kullanmaya geçmeniz önerilir **sqlWriterCleanupScript** özelliği.
  * Yapılandırırsanız, **sqlWriterCleanupScript** özelliği her kopyalama etkinliği için çalıştırabilir, hizmet betik tetikler ve sonra toplu kopyalama API veri eklemek için kullanın. Örneğin, tüm tablo en son verilerle üzerine yazmak için ilk önce toplu yeni veri kaynağından yükleme önce tüm kayıtları silmek için bir betik belirtebilirsiniz.
* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemasını kopyalama verimlilik etkiler. Veritabanı daha verimli bir şekilde daha az veri toplu yürütme çünkü aynı miktarda veri kopyalayın, bir büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, toplu bir dizi veri ekler. Kullanarak bir toplu işlemde satır sayısını ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satırları varsa, ayarlayabileceğiniz **writeBatchSize** daha az toplu iş yükü ve daha yüksek verimlilik yararlanmak için daha yüksek bir değere sahip özelliği. Verilerinizin satır boyutu büyükse, artırmanız dikkatli olun **writeBatchSize**. Yüksek bir değer veritabanı aşırı yüklemesi nedeniyle bir kopyalama hatası neden.
* İçin **içi ilişkisel veritabanları** SQL Server ve kullanılmasını Oracle gibi **veri yönetimi ağ geçidi**, bkz: [veri yönetimi ağ geçidi için ilgili önemli noktalar](#considerations-for-data-management-gateway) bölümü.

### <a name="nosql-stores"></a>NoSQL depoları
*(Tablo depolama ve Azure Cosmos DB içerir)*

* İçin **tablo depolama**:
  * **Bölüm**: veri araya eklemeli bölümlere önemli ölçüde yazma performansı düşürür. Veri kaynağınızı bölüm anahtarı tarafından Sırala veriler verimli bir şekilde bir bölüme sonra başka bir eklenir veya tek bir bölüm için veri yazmak için mantığı ayarlayın.
* İçin **Azure Cosmos DB**:
  * **Yığın boyutu**: **writeBatchSize** özelliği belgeleri oluşturmak için Azure Cosmos DB hizmetine paralel istek sayısını ayarlar. Artırdığınızda, daha iyi performans düşüklüğü görebilir **writeBatchSize** çünkü daha fazla paralel istekler için Azure Cosmos DB gönderilir. Ancak, Azure Cosmos (hata iletisi "istek hızı büyük" olur) DB yazdığınızda azaltma izleyin. Azaltma, belge boyutu dahil olmak üzere belgeler ve dizin oluşturma ilkesini hedef koleksiyonun terimleri sayısı çeşitli etkenler neden olabilir. Daha yüksek kopyalama verimlilik elde etmek için daha iyi bir koleksiyon, örneğin, S3 kullanmayı düşünün.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için ilgili önemli noktalar
Giriş veri kümesini veya çıkış veri kümesi bir dosyadır seri hale getirme ve seri durumdan çıkarma ortaya çıkabilir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) kopyalama etkinliği tarafından desteklenen dosya biçimleri hakkında ayrıntılarla.

**Davranış kopyalama**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Aynı veya hiçbir dosya biçimi ayarları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda ikili kopyasını herhangi bir seri hale getirme veya seri durumdan çıkarma olmadan yürütür. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek verimlilik bakın.
  * Ne zaman giriş ve çıkış veri kümeleri hem metin biçimi ve yalnızca kodlama olan türü farklı olan, veri taşıma hizmeti yalnızca kodlama dönüştürme yapar. Tüm serileştirme yapmaz ve seri durumdan yükü ikili kopyasını karşılaştırıldığında bazı performans neden olur.
  * Farklı dosya biçimleri veya sınırlayıcıları gibi farklı yapılandırmaları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda akış, dönüştürme ve belirttiğiniz çıkış biçimine serileştirmek için kaynak verileri seri durumdan çıkarır. Bu işlem yükü diğer senaryolar için kıyasla daha önemli bir performans sonuçlanır.
* Dosyaları (örneğin, bir dosya tabanlı depolama alanından bir ilişkisel deposuna) dosya tabanlı olmayan bir veri deposu/gruptan kopyaladığınızda, seri hale getirme veya seri durumdan çıkarma adım gereklidir. Bu adım, önemli performans ek yükü sonuçlanır.

**Dosya biçimi**: seçtiğiniz dosya biçimi kopyalama performansını etkileyebilir. Örneğin, Avro meta verilerle depolar sıkıştırılmış bir ikili biçimi ' dir. İşleme ve sorgulama için Hadoop ekosistemindeki geniş bir desteğe sahiptir. Ancak, Avro seri hale getirme ve metin biçimine kıyasla daha düşük kopyalama üretilen iş sonuçları seri durumdan için daha pahalı olması. Seçtiğiniz dosya biçimi işleme akışı boyunca bütünsel olun. Hangi veri form ile başlangıç, kaynak veri depolarını veya dış sistemlerden ayıklanacak depolanır; Depolama, analitik işleme ve sorgulama için en iyi biçimi; ve hangi biçiminde raporlama ve görselleştirme araçları için data Mart içinde veri verilmesi. Bazen için uygun olmayan bir dosya biçimi okuma ve yazma performansı genel analitik işlemi göz önüne aldığınızda iyi bir seçimdir olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma için ilgili önemli noktalar
Giriş veya çıkış Veri kümenizi bir dosyası olduğunda, hedef veri yazar gibi sıkıştırma veya açma işlemi gerçekleştirmek için kopyalama etkinliği ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir kolaylığını yapmak ve CPU. İşlem kaynakları ek veri maliyetleri sıkıştırma. Ancak buna karşılık, ağ g/ç ve depolama azaltır. Verilerinizi bağlı olarak artırma genel kopyalama üretilen işi de görebilirsiniz.

**Codec**: GZIP, bzıp2 ve Deflate sıkıştırma türleri kopyalama etkinliği destekler. Azure Hdınsight, tüm üç tür işleme için kullanabilir. Her bir sıkıştırma codec avantajları vardır. Örneğin, bzıp2 düşük kopyalama üretilen işi olan, ancak işleme için bölmek için bzıp2 ile en iyi Hive sorgu performansı alırsınız. Gzip en dengeli bir seçenektir ve en sık kullanılır. Uçtan uca senaryonuza uygun codec seçin.

**Düzey**: her bir sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: hızlı sıkıştırılmış ve verimli sıkıştırılmış. Sonuç dosyası en iyi şekilde sıkıştırılmaz olsa bile Hızlı sıkıştırılmış seçeneği veri mümkün olan en kısa sürede sıkıştırır. En iyi şekilde sıkıştırılmış seçenek sıkıştırmayı daha fazla zaman harcayan ve en az miktarda veri ortaya çıkarır. Çalışmanızın daha iyi genel performans sağlayan görmek için her iki seçenek test edebilirsiniz.

**Önemli bir unsur**: büyük miktarda bir şirket içi depolama ve bulut arasında veri kopyalamak için geçici blob depolama sıkıştırması ile kullanmayı düşünün. Geçici depolama kullanarak şirket ağınıza ve Azure hizmetlerinizi bant genişliği sınırlayıcı faktördür ve girdi veri kümesi hem de çıkış veri kümesi sıkıştırılmamış formunda olacak şekilde istediğinizde yararlıdır. Daha açık belirtmek gerekirse, iki kopyalama etkinliklerine tek kopyalama etkinliği bozulabilir. İlk kopyalama etkinliği bir geçiş veya hazırlama blob sıkıştırılmış biçimde kaynaktan kopyalar. İkinci kopyalama etkinliği hazırlamadan sıkıştırılmış verileri kopyalar ve havuz için yazarken sonra açar.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için ilgili önemli noktalar
Ayarlayabileceğiniz **columnMappings** tüm harita veya çıktı sütunları için bir giriş sütun alt kümesini kopyalama etkinliğinde özelliği. Veri Taşıma hizmeti veri kaynağından okuduktan sonra havuz için verileri yazar önce sütun eşlemesi verileri gerçekleştirmek gerekir. Bu ek işleme kopyalama performansını düşürür.

Kaynak veri deposu sorgulanabilir ise, örneğin, SQL Database veya SQL Server gibi ilişkisel bir mağaza ise ya da bir NoSQL deposu Table storage veya Azure Cosmos DB gibi ise sütun filtreleme dağıtmaya ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özelliği. Veri Taşıma hizmeti çok daha verimli olduğu kaynak veri deposundan verileri okurken bu şekilde, projeksiyon oluşur.

## <a name="other-considerations"></a>Diğer konular
Kopyalamak istediğiniz verilerin boyutu büyükse, veri fabrikasında dilimleme mekanizmasını kullanarak verileri iş mantığınızı başka bölüme göre ayarlayabilirsiniz. Daha sonra her kopya etkinliği çalıştırmak için verilerin boyutunu küçültmek için daha sık çalıştırmak için kopyalama etkinliği zamanlayın.

Veri kümeleri ve aynı anda aynı veri deposu bağlayıcıya veri fabrikası gerektiren kopyalama etkinliklerin sayısını dikkatli olun. Çok sayıda eş zamanlı kopyalama iş bir veri deposu kısıtlama ve performans, kopyalama işi iç yeniden denemeleri ve bazı durumlarda, yürütme hataları neden olabilir.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: bir şirket içi SQL Server bir kopyasından Blob Depolama
**Senaryo**: Blob Depolama birimine CSV biçiminde bir şirket içi SQL Server'dan veri kopyalamak için bir işlem hattı oluşturulmuştur. Kopyalama işini hızlandırmak için CSV dosyaları bzıp2 biçime sıkıştırılmış.

**Test ve analiz**: performans Kıyaslama çok daha yavaş olduğu kopyalama etkinliği verimini değerinden 2 MB/sn, açık.

**Performans Analizi ve ayarlama**: performans sorunu gidermek için nasıl veri işlenen taşınır ve konumundaki bakalım.

1. **Veri Okuma**: ağ geçidi SQL Server için bir bağlantı açar ve sorgu gönderir. SQL Server veri akışı için ağ geçidi intranet göndererek yanıt verir.
2. **Seri hale getirmek ve verileri sıkıştırmak**: ağ geçidi, CSV biçiminde veri akışa serileştirir ve bir bzıp2 akış verileri sıkıştırır.
3. **Veri yazma**: ağ geçidi bzıp2 akış Internet üzerinden Blob depolama alanına yükler.

Gördüğünüz gibi veri okunduğunu işlenir ve bir akış sıralı şekilde taşındı: SQL Server > LAN > ağ geçidi > WAN > Blob Depolama. **Genel performansı göre en düşük işleme ardışık düzeni geçişli**.

![Veri akışı](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla bilgi için aşağıdaki etkenleri performans düşüklüğü neden olabilir:

* **Kaynak**: SQL Server kendisini nedeniyle ağır yük düşük işleme sahiptir.
* **Veri Yönetimi ağ geçidi**:
  * **LAN**: ağ geçidi gölgeden uzak SQL Server makinesinde bulunur ve düşük bant genişlikli bağlantısı vardır.
  * **Ağ geçidi**: ağ geçidi, aşağıdaki işlemleri gerçekleştirmek için yük kısıtlamalarını ulaştı:
    * **Seri hale getirme**: veri akışı CSV biçiminde seri hale getirme yavaş işleme sahiptir.
    * **Sıkıştırma**: yavaş sıkıştırma codec (2.8 MB/sn Çekirdek i7 ile olan Örneğin, bzıp2,) seçtiniz.
  * **WAN**: şirket ağına ve Azure hizmetlerinizi arasında bant genişliği düşükse (örneğin, T1 1,544 kbps; = T2 6,312 kbps =).
* **Havuz**: Blob depolama alanına sahip düşük işleme. (Bu senaryoda en az 60 MB/sn SLA'sını güvence altına alır çünkü düşüktür.)

Bu durumda, bzıp2 veri sıkıştırma tüm ardışık yavaşlamasının. Gzip sıkıştırma codec için geçiş bu performans sorunu kolaylaştırır.

## <a name="sample-scenarios-use-parallel-copy"></a>Örnek senaryo: paralel kopyasını kullanın
**Senaryo I:** 1.000 1 MB dosyaların kopyalanacağı şirket içi dosya sistemi için Blob Depolama.

**Çözümleme ve performans ayarlama**: dört çekirdekli makine üzerinde ağ geçidi yüklediyseniz, bir örnek için veri fabrikası 16 paralel kopyaları dosyaları dosya sisteminden Blob depolama alanına eşzamanlı olarak taşımak için kullanır. Bu paralel yürütme yüksek performans neden olur. Paralel kopya sayısı da açıkça belirtebilirsiniz. Çok sayıda küçük dosya kopyaladığınızda, paralel kopyaları önemli ölçüde işleme kaynakları daha verimli şekilde kullanarak yardımcı olur.

![Senaryo 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Senaryo II**: 20 BLOB'lar 500 MB'lık Data Lake deposu Analytics için Blob depolama alanından kopyalayın ve performansı ayarlamak.

**Çözümleme ve performans ayarlama**: Bu senaryoda, veri fabrikası verileri Blob depolama alanından Data Lake Store'a tek kopya kullanarak kopyalar (**parallelCopies** 1 olarak ayarlayın) ve tek bulut veri taşıma birimleri. Gözlemlemek verimlilik, yakın içinde açıklanacaktır [performans başvuru bölümünde](#performance-reference).   

![Senaryo 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Senaryo III**: tek tek dosya boyutu MB düzinelerce büyük ve toplam birim büyük değil.

**Çözümleme ve performans dönüş**: artırma **parallelCopies** tek bulut DMU kaynak sınırlamaları nedeniyle kopyalama daha iyi performans elde edemezseniz. Bunun yerine, daha fazla bulut veri taşımayı gerçekleştirmek için daha fazla kaynak almak için DMUs belirtmeniz gerekir. İçin bir değer belirtmezseniz **parallelCopies** özelliği. Veri Fabrikası paralellik işler. Bu durumda, ayarlarsanız **cloudDataMovementUnits** 4'e bir verimini hakkında dört kez oluşur.

![Senaryo 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Başvuru
İzleme ve bazı desteklenen veri depoları için başvuru ayarlama performans şunlardır:

* Azure Storage (Blob Depolama ve tablo depolama dahil): [Azure Storage ölçeklenebilirlik hedefleri](../../storage/common/storage-scalability-targets.md) ve [Azure Storage performans ve ölçeklenebilirlik Yapılacaklar listesi](../../storage/common/storage-performance-checklist.md)
* Azure SQL Database: Yapabilecekleriniz [performansını izlemek](../../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimi (DTU) yüzde denetleyin
* Azure SQL Data Warehouse: Veri ambarı birimlerini (Dwu'lar); kendi yeteneği ölçülür bkz: [Yönet işlem güç Azure SQL Data warehouse'da (genel bakış)](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Azure Cosmos veritabanı performans düzeyleri](../../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [İzleyici ve performans ayarlama](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [dosya sunucuları için performans ayarlama](https://msdn.microsoft.com/library/dn567661.aspx)
