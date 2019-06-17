---
title: Etkinlik performansı ve ayarlama Kılavuzu kopyalama | Microsoft Docs
description: Kopyalama etkinliği kullandığınızda, Azure Data factory'de veri taşımayı performansını etkileyen anahtar Etkenler hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: ec8c58e4ced0d8df958e242b9c1671aeed8c2ee6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60488259"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Etkinlik performansı ve ayarlama Kılavuzu kopyalayın

> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-copy-activity-performance.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-performance.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [kopyalama etkinliği performansı ve ayarlama kılavuzu için Data Factory'ye](../copy-activity-performance.md).

Azure Data Factory kopyalama etkinliği, çözüm yüklenirken birinci sınıf bir güvenli, güvenilir ve yüksek performanslı veri sunar. Birçok farklı bulut her gün terabaytlarca veriyi onlarca kopyasını sağlar ve şirket içi veri depoları. İnanılmaz derecede hızlı veri yükleme performansını anahtarıdır çekirdek "büyük veri" soruna odaklanmak emin olmak için: Gelişmiş analiz çözümleri oluşturmak ve tüm bu verilerden ayrıntılı Öngörüler alma.

Azure Kurumsal düzeyde veri depolama ve veri ambarı çözümleri sunmaktadır ve kopyalama etkinliği yapılandırmak ve ayarlamak kolaydır deneyimi yüklenirken yüksek oranda iyileştirilmiş bir veri sunar. Yalnızca tek bir kopyalama etkinlikli, elde edebilirsiniz:

* Verileri yükleme **Azure SQL veri ambarı** adresindeki **1,2 GB/sn**. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).
* Verileri yükleme **Azure Blob Depolama** adresindeki **1,0 GB/sn**
* Verileri yükleme **Azure Data Lake Store** adresindeki **1,0 GB/sn**

Bu makalede açıklanır:

* [Performans başvuru sayıları](#performance-reference) desteklenen; proje planlamanıza yardımcı olması için kaynak ve havuz veri deposu
* Kopyalama aktarım hızı dahil olmak üzere farklı senaryolarda artırabilir özellikleri [bulut veri taşıma birimleri](#cloud-data-movement-units), [paralel kopyalama](#parallel-copy), ve [kopyalama aşamalı](#staged-copy);
* [Performans ayarlama Kılavuzu](#performance-tuning-steps) performans ve kopyalama performansı etkileyen önemli faktörlerin ayarlama konusunda.

> [!NOTE]
> Genel kopyalama etkinliği ile ilgili bilgi sahibi değilseniz, bkz. [kopyalama etkinliğiyle veri taşıma](data-factory-data-movement-activities.md) bu makaleyi okuduktan önce.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru, aşağıdaki tabloda şirket içi teste dayanan verilen kaynak ve havuz çiftleri için MB/sn olarak kopyalama aktarım hızı sayısı görüntülenir. Karşılaştırma için ayrıca nasıl farklı ayarları gösterir [bulut veri taşıma birimleri](#cloud-data-movement-units) veya [veri yönetimi ağ geçidi ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md) (birden çok ağ geçidi düğümleri) kopyalama performansı üzerinde yardımcı olabilir.

![Performans Matrisi](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>Azure Data Factory sürüm 1, en az bulut verisi taşıma birimlerinin Bulut Bulut kopyalama için olan iki. Belirtilmezse, varsayılan olarak kullanılan veri taşıma birimleri görmek [bulut veri taşıma birimleri](#cloud-data-movement-units).

**Dikkat edilecek noktalar:**
* Aşağıdaki formülü kullanarak aktarım hızı hesaplanır: [kaynak Okuma boyutu veri] / [kopyalama etkinliği çalıştırma süresi].
* Tablo performans başvuru sayıları ile ölçülür [TPC-H](http://www.tpc.org/tpch/) veri kümesinde tek bir kopyalama etkinliği çalıştırma.
* Azure veri depoları, kaynak ve havuz aynı Azure bölgesinde olan.
* Karma kopyalama şirket içi ve bulut arasında veri depoları, her ağ geçidi düğümü altındaki belirtimi ile şirket içi veri deposundan ayrı bir makinede çalışıyordu. Kopyalama işlemi, tek bir etkinlik ağ geçidi üzerinde çalışırken, yalnızca küçük bir kısmını test makinenin CPU, bellek veya ağ bant genişliği tüketilen. Daha fazla bilgi [veri yönetimi ağ geçidi için göz önünde bulundurarak](#considerations-for-data-management-gateway).
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
> Varsayılandan daha fazla veri taşıma birimleri (DMUs) yararlanarak daha yüksek aktarım hızı elde edebileceğiniz bir bulut buluta kopyalama etkinliği çalıştırma için 32'dir maksimum DMUs. Örneğin, 100 DMUs ile Azure Blob veri kopyalamayı Azure Data Lake Store içine elde edebileceğiniz **1.0GBps**. Bkz: [bulut veri taşıma birimleri](#cloud-data-movement-units) bölümü bu özellik ve desteklenen bir senaryo hakkındaki ayrıntılar için. İlgili kişi [Azure Destek](https://azure.microsoft.com/support/) daha fazla DMUs istemek için.

## <a name="parallel-copy"></a>Paralel kopyalama
Veri kaynağından okumak veya veri hedefe yazma **paralel bir kopyalama etkinliği çalıştırma içinde**. Bu özellik, bir kopyalama işleminin aktarım hızını geliştirir ve verilerini taşımak için gereken süreyi azaltır.

Bu ayar farklıdır **eşzamanlılık** etkinliği tanımındaki özelliği. **Eşzamanlılık** özelliği sayısını belirler **eşzamanlı kopyalama etkinliği çalıştığında** (1 AM için 02: 00 AM 2 3'te, AM 3 ve 4'te ve benzeri) farklı bir etkinlik Windows'dan verileri işlemek için. Geçmiş yük gerçekleştirdiğinizde bu yararlı bir özelliktir. Paralel kopyalama özelliği için geçerli bir **tek bir etkinlik çalıştırması**.

Bir örnek senaryo göz atalım. Aşağıdaki örnekte, birden fazla geçmiş dilimler işlenmesi gerekir. Veri fabrikası, bir kopyalama etkinliği (etkinlik çalıştırma) örneği her dilim için çalışır:

* Veri dilimi penceresinden ilk etkinlik (1 AM için 02: 00) == > etkinlik çalıştırması 1
* Veri dilimi penceresinden ikinci etkinlik (02: 00 için 3'te) == > etkinlik 2 çalıştırın
* Veri dilimi penceresinden ikinci etkinlik (3'te için 04: 00) == > etkinlik çalıştırın 3

Etki alanları bu hiyerarşi sıralamasıyla devam eder.

Bu örnekte, zaman **eşzamanlılık** değeri 2'ye ayarlanır **etkinliği 1 çalıştırmak** ve **etkinliği 2 çalıştırmak** iki etkinlik Windows'dan veri kopyalama **eşzamanlı olarak** veri taşıma performansını artırmak için. Birden çok dosya 1 Çalıştır etkinliği ile ilişkili, ancak veri taşıma Hizmeti'nde dosyaları kaynak sunucudan hedef bir dosyaya aynı anda kopyalar.

### <a name="cloud-data-movement-units"></a>Bulut verisi taşıma birimlerinin
A **bulut verisi taşıma birimi (DMU)** Data Factory içinde tek bir birim (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçüdür. DMU geçerli bulut buluta kopyalama işlemleri için ancak bir karma kopyalama.

**Kopyalama etkinliği çalıştırmasının güçlendirmek için en az bulut verisi taşıma birimlerinin iki olur.** Belirtilmezse, aşağıdaki tabloda farklı kopyalama senaryolarında kullanılan varsayılan DMUs listelenmektedir:

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan DMUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | 4 ile 16 sayısı ve dosyaların boyutuna bağlı olarak arasında. |
| Tüm diğer kopyalama senaryolarında | 4 |

Bu varsayılanı geçersiz kılmak için bir değer belirtin. **cloudDataMovementUnits** özelliğini aşağıdaki gibi. **İzin verilen değerler** için **cloudDataMovementUnits** özelliği, 2, 4, 8, 16, 32 olan. **Bulut DMUs gerçek sayısını** eşit veya daha az, veri modelini bağlı olarak yapılandırılmış bir değeri, kopyalama işleminin çalışma zamanında kullanır. Özel kopyalama kaynağı ve havuz için daha fazla birimi yapılandırırken alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

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
> Daha fazla bulut DMUs daha yüksek bir aktarım hızı için gerekiyorsa, kişi [Azure Destek](https://azure.microsoft.com/support/). 8 ayarlamak ve üzerinde şu anda yalnızca çalışır, **birden çok dosyayı Blob Depolama/Data Lake Store/Azure Blob Depolama/Data Lake Store/Amazon'dan S3/bulut FTP/bulut SFTP kopyalamak SQL veritabanı**.
>

### <a name="parallelcopies"></a>parallelCopies
Kullanabileceğiniz **parallelCopies** kopyalama etkinliği, kullanmak istediğiniz paralellik belirtmek için özelliği. Bu özellik veri kaynağınızdan okuyabilen veya, havuz veri depolarına paralel yazma kopyalama etkinliği içinde iş parçacığı sayısı olarak düşünebilirsiniz.

Her kopya etkinlik çalıştırma için Data Factory veri depolamak ve için hedef veri deposu kaynak sunucudan veri kopyalamak için paralel kopya sayısını belirler. Varsayılan sayısıyla onu kullanan paralel kaynak ve havuz kullanmakta olduğunuz türüne bağlıdır.

| Kaynak ve havuz | Hizmet tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı depolama alanları (Blob Depolama; arasında veri kopyalama Data Lake Store; Amazon S3 '; Şirket içi dosya sistemi; bir şirket içi HDFS'ye) |1 ile 32 arasında. İki bulut veri deposu ya da fiziksel yapılandırma (veya bir şirket içi veri deposundan veri kopyalamak için) bir karma kopyalama için kullanılan ağ geçidi makinesinin arasında veri kopyalamak için kullanılan dosyaları bulut verisi taşıma birimlerini (DMUs) sayısı ve boyutuna bağlıdır. |
| Deposundan veri kopyalamayı **herhangi bir kaynak veri deposu Azure tablo depolama** |4 |
| Diğer tüm kaynak ve havuz çiftleri |1 |

Genellikle, varsayılan davranışı en iyi aktarım hızı vermeniz gerekir. Ancak, verilerinizi barındıran makinelerin yükünü denetlemek için depolar veya kopyalama performansı ayarlamak için varsayılan değeri geçersiz kılmak ve için bir değer belirtmek seçebilirsiniz **parallelCopies** özelliği. Değer 1 ile 32 (her ikisi de dahil) arasında olmalıdır. Çalışma zamanında, en iyi performans için ayarladığınız değerine eşit veya daha az olan bir değer kopyalama etkinliği kullanır.

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

* Dosya tabanlı depoları arasında veri kopyalama **parallelCopies** paralellik dosya düzeyinde belirlenir. Tek bir dosyada Öbekleme altındaki otomatik ve şeffaf şekilde olacağını ve paralel ve parallelCopies dikgen verileri yüklemek için en uygun öbek boyutu için belirtilen kaynak veri deposu türü kullanmak için tasarlanmıştır. Gerçek veri taşıma Hizmeti'nde kopyalama işleminin çalışma zamanında kullandığı paralel kopya sayısı sahip olduğunuz dosyaların sayısı, en fazla ' dir. Kopyalama davranışını ise **mergeFile**, kopyalama etkinliği, dosya düzeyinde paralellik avantajlarından alamaz.
* İçin bir değer belirtirseniz **parallelCopies** özelliği, bir karma kopyalama ise, kaynak ve havuz veri deposu ve ağ geçidine yük artışı göz önünde bulundurun. Bu, özellikle birden çok etkinlikler veya aynı veri deposuna karşı çalışan aynı etkinliklerden eş zamanlı çalıştırma olduğunda gerçekleşir. Veri deposu ya da ağ geçidi yük ile doludur fark ederseniz, azaltma **parallelCopies** yükle hafifletmek için değer.
* Dosya tabanlı depoları için dosya tabanlı olmayan depolarından verileri kopyaladığınızda, veri taşıma Hizmeti'nde yok sayar **parallelCopies** özelliği. Paralellik belirtilmiş olsa bile, bu durumda uygulanmaz.

> [!NOTE]
> Kullanmak için veri yönetimi ağ geçidi 1.11 veya sonraki bir sürümü kullanmanız gerekir **parallelCopies** karma kopyalama bunu yaptığınızda özelliği.
>
>

Bu iki özellik daha iyi kullanmak üzere ve veri taşıma aktarım hızınızı geliştirmek için kullanım örneği bakın. Yapılandırmanız gerekmez **parallelCopies** varsayılan davranışı yararlanmak için. Yapılandırırsanız ve **parallelCopies** birden çok bulut DMUs değil tamamı kullanılana çok küçük.

### <a name="billing-impact"></a>Faturalama etkisi
Sahip **önemli** tabanlı kopyalama işlemi toplam zamanında ücretlendirilir unutmayın. Bir kopyalama işi bir saat bir bulut birimiyle almak için kullanılan ve artık bu dört bulut birimiyle 15 dakika sürer, toplam fatura neredeyse aynı kalır. Örneğin, dört bulut birimi kullanın. Birinci bulut biriminin 10 dakika, ikinci bir geçirdiği 10 dakika, üçüncü bir, 5 dakika ve dördüncü 5 dakikada bir, her bir kopyalama etkinliği çalıştırma. 10 + 10 + 5 + 5 = 30 dakika toplam kopyalama (veri hareketi) süresi için ücretlendirilir. Kullanarak **parallelCopies** faturalama etkisi yoktur.

## <a name="staged-copy"></a>Hazırlanmış kopya
Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

1. **PolyBase aracılığıyla SQL veri ambarı'na çeşitli veri depolarından veri alabilen istediğiniz**. SQL veri ambarı PolyBase, SQL veri ambarı'na büyük miktarda veri yüklemek için yüksek performanslı mekanizması olarak kullanır. Ancak, kaynak verileri Blob Depolama alanında olması gerekir ve diğer ölçütleri karşılaması gerekir. Blob Depolama dışındaki bir veri deposundan veri yüklediğinizde, verileri geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda, Data Factory, PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümleri gerçekleştirir. Ardından verileri SQL Data Warehouse'a veri yüklemek için PolyBase kullanır. Daha fazla ayrıntı için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).
2. **Bazı durumlarda bir karma veri taşıma işlemini gerçekleştirmek için bir süre sürer (diğer bir deyişle, arasında bir şirket içi veri kopyalamak için depolama ve bir bulut veri depolama) yavaş ağ bağlantısı üzerinden**. Performansı artırmak için hazırlama veri deposu bulutta veri taşıma daha az zaman alır, böylece şirket içi veri sıkıştırabilirsiniz. Hedef veri deposuna yüklemeden önce hazırlama deposu verileri genişletmek.
3. **80 numaralı bağlantı noktası dışındaki bağlantı noktaları açın ve kurumsal BT ilkeleri nedeniyle, güvenlik duvarında 443 bağlantı noktasına istemediğiniz**. Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu için bir şirket içi veri deposundan veri kopyaladığınızda, Windows Güvenlik Duvarı hem kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimine'ı etkinleştirmeniz gerekir. Bu senaryoda, ilk veri kopyalama ağ geçidine bir Blob Depolama hazırlama örneği için HTTP veya HTTPS üzerinden bağlantı noktası 443 üzerinde yararlanın. Ardından verileri Blob Depolama hazırlık ortamından SQL veritabanı veya SQL veri ambarı yükleyebilirsiniz. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopya çalışır
Hazırlama özelliğini etkinleştirdiğinizde, ilk veriler kaynak veri deposundan hazırlama veri deposuna kopyalanır (kendi işleyicinizi getirin). Ardından, veri hazırlama veri deposundan havuz veri deposuna kopyalanır. Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Veri taşıma tamamlandıktan sonra veri fabrikası geçici verileri hazırlama depolama biriminden da temizler.

Bulut kopyalama senaryosunda (bulutta depolarıdır hem kaynak hem de havuz veri) ağ geçidi kullanılmaz. Data Factory hizmetinin kopyalama işlemleri gerçekleştirir.

![Hazırlanmış kopya: Bulut senaryosu](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Karma kopyalama senaryoda (kaynak, şirket içinde ve havuz bulutta olan), ağ geçidi verileri bir hazırlama veri deposu için kaynak veri deposundan taşır. Data Factory hizmeti, havuz veri deposuna hazırlama veri deposundan veri taşır. Hazırlama yoluyla bir şirket içi veri deposuna bir bulut veri deposundan veri kopyalamayı ters flow ile desteklenir.

![Hazırlanmış kopya: Karma senaryo](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Hazırlama deposu kullanarak veri taşıma etkinleştirdiğinizde, verileri bir geçiş veya hazırlama veri deposu için kaynak veri deposundan veri taşımadan önce sıkıştırılmış ve ardından bir geçiş veri taşıma ve veri hazırlık önce eklenmişti isteyip istemediğinizi belirtebilirsiniz Havuz veri deposuna depolayın.

Şu anda, hazırlama deposu kullanarak iki şirket içi veri depoları arasında veri kopyalanamıyor. Bu seçeneği hemen kullanılabilir olmasını bekliyoruz.

### <a name="configuration"></a>Yapılandırma
Yapılandırma **enableStaging** verileri Blob Depolama alanında çoğaltılmadan önce bir hedef veri deposuna yüklemek isteyip istemediğinizi belirtmek için kopyalama etkinliği ayarlama. Ayarladığınızda **enableStaging** TRUE ise sonraki tabloda listelenen ek özellikler belirtmek için. Yoksa, depolama paylaşılan erişim imzası bağlı hizmeti hazırlama için veya bir Azure depolama oluşturulması gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| **enableStaging** |Veri deposunu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| **linkedServiceName** |Adını bir [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bağlı hizmeti, geçici bir hazırlama deposu kullanan depolama örneğine başvurur. <br/><br/> PolyBase aracılığıyla SQL veri ambarı'na veri yüklemek için depolama ile paylaşılan erişim imzası kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlayın |
| **Yolu** |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolu belirtin. Hizmet, bir yol belirtmezseniz, geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici veri belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| **enableCompression** |Hedefe kopyalamadan önce verilerin sıkıştırılmasının gerekli olup olmadığını belirtir. Bu ayar, aktarılan veri hacmini azaltır. |False |Hayır |

Kopyalama etkinliği önceki tabloda açıklanan özellikler ile bir örnek tanımı aşağıda verilmiştir:

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
Bağlı olarak iki adımı ücretlendirilir: kopyalama süresi ve kopyalayın türü.

* (Başka bir bulut veri deposu bir bulut veri deposundan veri kopyalamayı) bulut kopyalama sırasında Hazırlama [toplam kopyalama süresi adım 1 ve 2. adım] [bulut kopyalama birim fiyatı] x kullandığınızda ücretlendirilir.
* Kullandığınızda (bulut veri deposu için bir şirket içi veri deposundan veri kopyalamayı) karma kopyalama sırasında Hazırlama [karma kopyalama süresi] ödersiniz x [karma kopyalama birim fiyatı] + [bulut kopyalama süresi] x [bulut kopyalama birim fiyatı].

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları
Kopyalama etkinliği, Data Factory hizmetine performansını ayarlamak için aşağıdaki adımları atmanız öneririz:

1. **Bir taban çizgisi oluşturma**. Geliştirme aşamasında, bir temsilci veri örneği karşı kopyalama etkinliği'ni kullanarak işlem hattınızı test etme. Data Factory kullanabileceğiniz [model dilimleme](data-factory-scheduling-and-execution.md) çalıştığınız veri miktarını sınırlamak için.

   Yürütme süresi ve performans özelliklerini kullanarak toplamak **izleme ve yönetim uygulaması**. Seçin **izleme ve yönetme** Data Factory giriş sayfası. Ağaç görünümünde seçin **çıktı veri kümesi**. İçinde **etkinlik Windows** listesinde, çalıştırmak kopyalama etkinliği'ni seçin. **Etkinlik Windows** kopyalama etkinliği süresi ve kopyalanan verilerin boyutuna göre listeler. Aktarım hızı listelenen **etkinlik penceresi Gezgini**. Uygulama hakkında daha fazla bilgi için bkz: [İzleyicisi ve izleme ve yönetim uygulaması kullanarak Azure Data Factory işlem hatlarını yönetmek](data-factory-monitor-manage-app.md).

   ![Etkinlik çalışma ayrıntıları](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Makalede, performans ve senaryonuz için kopyalama etkinliği'nin yapılandırılmasını karşılaştırabilirsiniz [Performans başvurusu](#performance-reference) bizim testlerden.
2. **Tanılama ve performans iyileştirme**. Siz gözleyin performans beklentilerinizi karşılamıyorsa, performans sorunlarını tanımlamak gerekir. Ardından, kaldırmak veya performans etkisini azaltmak için performansı iyileştirin. Performans Tanılama tam açıklamasını bu makalenin kapsamı dışındadır, ancak bazı genel konular şunlardır:

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Bulut verisi taşıma birimlerinin](#cloud-data-movement-units)
     * [Hazırlanmış kopya](#staged-copy)
     * [Veri Yönetimi ağ geçidi ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Veri Yönetimi Ağ Geçidi](#considerations-for-data-management-gateway)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Serileştirme ve seri durumundan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşleme](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)
3. **Tüm veri kümeniz Yapılandırması**. Yürütme sonuçları ve performans ile memnun kaldığınızda, tanım ve işlem hattı etkin dönemini tüm veri kümeniz kapsayacak şekilde genişletebilirsiniz.

## <a name="considerations-for-data-management-gateway"></a>Veri Yönetimi ağ geçidi için dikkat edilmesi gerekenler
**Ağ geçidi**: Ana veri yönetimi ağ geçidi için ayrılmış bir makine kullanmanızı öneririz. Bkz: [veri yönetimi ağ geçidi kullanma konuları](data-factory-data-management-gateway.md#considerations-for-using-gateway).

**Ağ geçidi izleme ve yukarı/genişleme**: Tek bir mantıksal ağ geçidi ile bir veya daha fazla ağ geçidi düğümleri, aynı anda aynı anda birden fazla kopyalama etkinliği çalıştırma görebilir. Bir ağ geçidi makinesine Azure portalında sınırı karşı çalışan eşzamanlı iş sayısını gördüğünüz gibi iyi kaynak kullanımı (CPU, bellek, network(in/out) vb.) neredeyse gerçek zamanlı anlık görüntüsünü görüntüleyebileceğiniz [İzleyici ağ geçidiportalında](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Karma veri hareketi çok sayıda eşzamanlı kopyalama etkinliği çalıştırma veya büyük miktarlarda veri kopyalamak için üzerinde ağır gerekmesi durumunda için göz önünde [ölçeğini artırır veya ağ geçidi](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) sağlamak için ya da kaynağınızı daha iyi kullanmak için kopyalama güçlendirmek için daha fazla kaynak.

## <a name="considerations-for-the-source"></a>Kaynağı için konular
### <a name="general"></a>Genel
Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını değil emin olun.

Microsoft veri depoları için bkz: [izleme ve ayarlama konuları](#performance-reference) veri depoları ve veri performans özelliklerini depolamak, yanıt süreleri en aza indirmek ve aktarım hızını en üst düzeye anlamanıza yardım özgü olan.

Blob depolamadan SQL veri ambarı'na veri kopyalama kullanmayı **PolyBase** performansını artırmak üzere. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob Depolama, Data Lake Store, Amazon S3, şirket içi dosya sistemlerine ve şirket içi HDFS'ye içerir)*

* **Dosya boyutu ve dosya sayısı ortalaması**: Kopyalama etkinliği, bir kerede veri bir dosya aktarır. İle aynı taşınacak veri miktarına, hizmetin genel performansını veriler nedeniyle her dosya için önyükleme aşaması birkaç büyük dosyalar yerine çok sayıda küçük dosya oluşuyorsa düşüktür. Bu nedenle, mümkünse, küçük dosyaları daha yüksek performans elde etmek için daha büyük dosyalarına birleştirin.
* **Dosya biçimi ve sıkıştırma**: Performansı artırmak daha fazla yolu için bkz. [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) bölümler.
* İçin **şirket içi dosya sistemi** senaryosu, hangi **veri yönetimi ağ geçidi** olan gerekli, bkz [veri yönetimi ağ geçidi için Değerlendirmeler](#considerations-for-data-management-gateway) bölümü.

### <a name="relational-data-stores"></a>İlişkisel veri deposu
*(SQL veritabanı bulunur; SQL veri ambarı; Amazon Redshift; SQL Server veritabanlarını; ve Oracle, MySQL, DB2, Teradata, Sybase ve PostgreSQL veritabanları, vs.)*

* **Veri modelini**: Tablo şemanızı kopyalama aktarım hızını etkiler. Büyük satır boyutu, küçük satır boyutu, aynı miktarda veri kopyalamak için daha iyi bir performans sunar. Veritabanı daha az satır içeren veri daha az toplu işler daha verimli bir şekilde alabilirsiniz nedenidir.
* **Sorgu veya saklı yordam**: Sorgu veya saklı yordam verileri daha verimli bir şekilde getirmek için kopyalama etkinliği kaynak belirttiğiniz mantığını iyileştirin.
* İçin **şirket içi ilişkisel veritabanlarını**, SQL Server ve kullanımını gerektiren, Oracle gibi **veri yönetimi ağ geçidi**, veri yönetimi ağ geçidi bölümüne dikkat edilecek noktalara bakın.

## <a name="considerations-for-the-sink"></a>Havuz için dikkat edilmesi gerekenler
### <a name="general"></a>Genel
Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını değil emin olun.

Microsoft veri depoları için başvurmak [izleme ve ayarlama konuları](#performance-reference) özgü veri depoları. Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

Veri kopyalıyorsanız **Blob Depolama** için **SQL veri ambarı**, kullanmayı **PolyBase** performansını artırmak üzere. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob Depolama, Data Lake Store, Amazon S3, şirket içi dosya sistemlerine ve şirket içi HDFS'ye içerir)*

* **Kopyalama davranışı**: Farklı dosya tabanlı veri deposundan verileri kopyalarsanız, kopyalama etkinliği ile üç seçenek vardır. **copyBehavior** özelliği. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyayı birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performansa sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: Bkz [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) performansını artırmak daha yollarını bölümler.
* **BLOB Depolama**: Şu anda, Blob Depolama, en iyi duruma getirilmiş veri aktarımı ve aktarım hızı için yalnızca blok bloblarını destekler.
* İçin **şirket içi dosya sistemleri** kullanımını gerektiren senaryolar **veri yönetimi ağ geçidi**, bkz: [veri yönetimi ağ geçidi için Değerlendirmeler](#considerations-for-data-management-gateway) bölümü.

### <a name="relational-data-stores"></a>İlişkisel veri deposu
*(SQL veritabanı, SQL veri ambarı, SQL Server veritabanlarını ve Oracle veritabanlarını içerir)*

* **Kopyalama davranışı**: Bağlı olarak özellikleri için ayarladığınız **sqlSink**, kopyalama etkinliği farklı şekillerde hedef veritabanına veri yazar.
  * Varsayılan olarak, en iyi performans sağlayan modu, veri taşıma hizmeti kullandığı veri eklemek için toplu kopyalama API ekleyin.
  * Havuza bir saklı yordam yapılandırırsanız, veritabanı veri bir satırı toplu yükleme yerine anda geçerlidir. Performansı ciddi ölçüde düştüğü. Veri kümeniz varsa, büyük ise kullanmayı göz önünde bulundurun **sqlWriterCleanupScript** özelliği.
  * Yapılandırırsanız **sqlWriterCleanupScript** çalıştırmak için her bir kopyalama etkinliği özelliği, hizmeti betik tetikler ve sonra verileri eklemek için toplu kopyalama API'sini kullanırsınız. Örneğin, en son verileriyle tüm tablo üzerine yazmak için önce toplu yeni veri kaynağından yükleme önce tüm kayıtları silmek için bir betik belirtebilirsiniz.
* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemanızı kopyalama aktarım hızını etkiler. Veritabanı daha verimli bir şekilde daha az toplu veri kaydedebilir miyim çünkü aynı miktarda veri kopyalamak için büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, toplu bir dizi içinde veri ekler. Bir toplu işte satır sayısını kullanarak ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satır varsa, ayarlayabileceğiniz **writeBatchSize** özellik toplu işlem ek yükü azaltır ve daha yüksek performans avantajlarından yararlanarak daha yüksek bir değere sahip. Verilerinizin satır boyutu büyükse, artırdığınızda dikkat **writeBatchSize**. Yüksek bir değere bir kopyalama hatasının veritabanı aşırı yüklenerek neden neden olabilir.
* İçin **şirket içi ilişkisel veritabanlarını** SQL Server ve kullanımını gerektiren, Oracle gibi **veri yönetimi ağ geçidi**, bkz: [veri yönetimi ağ geçididikkatealınacaknoktalar](#considerations-for-data-management-gateway)bölümü.

### <a name="nosql-stores"></a>NoSQL depoları
*(Tablo depolama ve Azure Cosmos DB içerir)*

* İçin **tablo depolama**:
  * **bölüm**: Araya eklemeli bölümlere önemli ölçüde veri yazma, performansı düşürür. Veri kaynağınızı bölüm anahtarına göre sıralama verileri verimli bir şekilde bir bölüme diğerinden sonra eklenir veya tek bir bölüm için veri yazmak için mantığı ayarlayın.
* İçin **Azure Cosmos DB**:
  * **Yığın boyutu**: **WriteBatchSize** özelliği belgeleri oluşturmak için Azure Cosmos DB hizmetini paralel istek sayısını ayarlar. Daha iyi performans, artırdığınızda, bekleyebileceğiniz **writeBatchSize** çünkü daha fazla paralel istekler için Azure Cosmos DB gönderilir. Ancak, Azure Cosmos DB (hata iletisi "istek oranı büyük" dır) yazdığınızda azaltma için izleyin. Belge boyutunu dahil olmak üzere, belgeler ve dizin oluşturma ilkesini hedef koleksiyonun terimleri sayısını azaltma çeşitli etkenler neden olabilir. Kopya daha yüksek performans sağlamak için daha iyi bir koleksiyon, örneğin, S3 kullanmayı düşünün.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için dikkat edilmesi gerekenler
Serileştirme ve seri durumundan çıkarma girdi veri kümesini veya çıkış veri kümesi bir dosya olduğunda ortaya çıkabilir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) kopyalama etkinliği tarafından desteklenen dosya biçimleri hakkında daha fazla ayrıntı içeren.

**Kopyalama davranışı**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Aynı veya hiçbir dosya biçimi ayarları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda, ikili serileştirme veya seri durumundan çıkarma olmadan kopya yürütür. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek bir aktarım hızı görürsünüz.
  * Ne zaman giriş ve çıkış veri kümeleri hem metin biçimi ve yalnızca kodlama olan türü farklı olmadığı, veri taşıma Hizmeti'nde yalnızca kodlama dönüştürme yapar. Herhangi bir serileştirme yapmaz ve seri durumdan yükü ikili kopyasını kıyasla bazı performans neden olur.
  * Farklı dosya biçimleri veya sınırlayıcılar gibi farklı yapılandırmaları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda akışı, dönüştürme ve belirttiğiniz çıkış biçimine seri hale için kaynak verileri seri durumdan çıkarır. Bu işlem ek yükü diğer senaryolarda kıyasla çok daha önemli bir performans sonuçlanır.
* (Örneğin, bir dosya tabanlı depolama alanından ilişkisel bir veri deposuna) dosya tabanlı olmayan bir veri deposuna/deposundan veri dosyalarını kopyalarken, serileştirme veya seri durumundan çıkarma adım gereklidir. Bu adım, önemli performans ek yükü oluşur.

**Dosya biçimi**: Seçtiğiniz dosya biçimi kopyalama performansı etkileyebilir. Örneğin, Avro verileriyle meta verileri depolar sıkıştırılmış bir ikili biçimi ' dir. Hadoop ekosistemindeki işleme ve sorgulama için geniş destek vardır. Ancak, Avro daha pahalı serileştirme ve seri kaldırma metin biçimine kıyasla daha düşük kopyalama aktarım hızı ile sonuçlanır. Dosya biçimi işleme akışı boyunca tercih ettiğiniz bütünlüklü olarak yapın. Hangi veri formu ile Başlat, kaynak veri depolarını veya dış sistemlerden ayıklanacak depolanır; Depolama, analitik işleme ve sorgulama için en iyi biçimi; ve hangi biçimde uygulamasına raporlama ve görselleştirme araçları için veri reyonları veri verilmesi. Bazen için yetersiz bir dosya biçimi okuma ve yazma performansını genel analitik işlemi düşünürken iyi bir seçim olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma dikkate alınacak noktalar
Giriş veya çıkış veri kümesi bir dosya olduğunda, hedef konuma verileri yazar gibi sıkıştırma veya sıkıştırmayı açma gerçekleştirmek için kopyalama etkinliği ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir denge duruma ve CPU. Bilgi işlem kaynaklarının ek veri maliyetlerini sıkıştırma. Ancak, ağ g/ç ve depolama azaltır. Verilere bağlı olarak bir artırma genel kopyalama aktarım hızının görebilirsiniz.

**Codec**: Kopyalama etkinliği, gzip ve bzıp2 sönme sıkıştırma türleri destekler. Azure HDInsight, tüm üç işlem için kullanabilir. Her bir sıkıştırma codec avantajları vardır. Örneğin, bzıp2 düşük kopyalama aktarım hızına sahip, ancak işleme için bölme çünkü bzıp2 ile en iyi Hive sorgu performansı elde. Gzip en dengeli bir seçenektir ve en sık kullanılır. Uçtan uca senaryonuza uygun codec bileşeni seçin.

**Düzey**: Her bir sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: hızlı sıkıştırılmış ve verimli sıkıştırılmış. Sıkıştırılmış en hızlı seçenek bile elde edilen dosyanın en uygun şekilde sıkıştırılmamış veri, mümkün olan en kısa sürede sıkıştırır. En uygun şekilde sıkıştırılmış seçeneği sıkıştırma üzerinde daha fazla zaman harcadığını ve en az miktarda veri ortaya çıkarır. İki seçenek de durumunuzdaki genel daha iyi performans sağlayan görmek için test edebilirsiniz.

**Önemli bir unsur**: Büyük miktarda bir şirket içi depolama ile bulut arasında veri kopyalamak için geçici blob depolama ile sıkıştırın göz önünde bulundurun. Geçici depolama kullanarak şirket ağınıza ve Azure hizmetlerinizi bant sınırlayan faktör ve giriş veri kümesi ve çıktı veri kümesi hem sıkıştırılmamış biçiminde olmasını istediğiniz olduğunda yararlıdır. Özellikle, tek bir kopyalama etkinliği iki kopyalama etkinliklerine bozabilir. İlk kopyalama etkinliği kaynak sıkıştırılmış formunda bir geçiş veya hazırlama blobu kopyalar. İkinci kopyalama etkinliği sıkıştırılmış veri hazırlık ortamından kopyalar ve ardından havuza yazarken açar.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için dikkat edilmesi gerekenler
Ayarlayabileceğiniz **Bunun amacı** kopyalama etkinliği özelliği tüm harita veya çıkış sütunları için giriş sütun alt kümesi. Veri taşıma Hizmeti'nde veri kaynağından okur. sonra havuz için verileri yazar önce veriler üzerinde sütun eşleme gerçekleştirmek gerekir. Bu ek işlem kopyalama performansını düşürür.

Kaynak veri deponuzda sorgulanabilir, örneğin, SQL veritabanı veya SQL Server gibi ilişkisel bir mağaza veya tablo depolama veya Azure Cosmos DB gibi bir NoSQL deposu olması durumunda sütun filtreleme gönderme ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özellik. Veri taşıma Hizmeti'nde çok daha verimli olduğu kaynak veri deposundan veri okurken bu şekilde yansıtma gerçekleşir.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar
Kopyalamak istediğiniz veri boyutu büyükse, Data Factory'de dilimleme mekanizmasını kullanarak verileri diğer bölümlere ayırmak için iş mantığınızı ayarlayabilirsiniz. Daha sonra her kopya etkinliği çalıştırmak için veri boyutunu azaltmak için daha sık çalıştırmak için kopyalama etkinliği planlayın.

Veri kümeleri ve aynı anda aynı veri deposuna bağlayıcıya gerektiren Data Factory kopyalama etkinlikleri hakkında dikkatli olun. Birçok eş zamanlı kopyası işleri bir veri deposu azaltma ve performansın düşmesine neden, kopyalama işi iç deneme ve bazı durumlarda, yürütme hatalarını sağlama.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: Bir şirket içi SQL Server'dan Blob depolamaya kopyalama
**Senaryo**: Bir işlem hattı, bir şirket içi SQL Server'dan Blob Depolama alanında CSV biçimi veri kopyalamak için oluşturulmuştur. Kopyalama işini daha hızlı hale getirmek için CSV dosyaları bzıp2 biçimine sıkıştırılmasının gerekli.

**Test ve analiz**: Performans Kıyaslama yavaş olduğu değerinden 2 MB/sn aktarım hızı kopyalama etkinliği var.

**Performans Analizi ve ayarlama**: Performans sorunu gidermek için nasıl veri işlenen taşınır ve adresindeki gözden geçirelim.

1. **Veri Okuma**: Ağ geçidi, bir SQL Server bağlantısı açar ve sorguyu gönderir. SQL Server ağ geçidi intranet veri akışını göndererek yanıt verir.
2. **Seri hale getirmek ve verileri sıkıştırmak**: Ağ geçidi CSV biçimi veri akışa serileştirir ve bir bzıp2 akış verileri sıkıştırır.
3. **Veri yazma**: Ağ geçidi bzıp2 akış Internet üzerinden Blob depolamaya yükler.

Gördüğünüz gibi verileri ettiğinden işlenir ve akış sıralı bir şekilde taşındı: SQL Server > LAN > ağ geçidi > WAN > Blob Depolama. **Genel performansını en düşük aktarım hızı ile işlem hattı Geçitli**.

![Veri akışı](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla aşağıdaki faktörleri performans sorunu neden olabilir:

* **Kaynak**: SQL Server kendisini aktarım hızının düşük olmasını nedeniyle ağır yükleri vardır.
* **Veri Yönetimi ağ geçidi**:
  * **LAN**: Ağ geçidi gölgeden uzak SQL Server makinesinde bulunduğu yer ve düşük bant genişliğine sahip bağlantısı vardır.
  * **Ağ geçidi**: Ağ geçidi aşağıdaki işlemleri gerçekleştirmek için kendi yük sınırlamaları ulaştı:
    * **Serileştirme**: CSV biçimi için veri akışı seri hale getirme, yavaş aktarım hızına sahip.
    * **Sıkıştırma**: Yavaş sıkıştırma codec (Core i7 ile 2.8 MB/sn, örneğin, bzıp2,) seçtiğiniz.
  * **WAN**: Kurumsal ağ ve Azure hizmetlerinizi arasında bant genişliği düşük olduğu (örneğin, T1 1,544 kbps; = T2 6,312 kbps =).
* **Havuz**: BLOB Depolama, aktarım hızının düşük olmasını sahiptir. (En az 60 MB/sn kendi SLA'sı garanti eder, çünkü bu senaryo bir olasılıktır.)

Bu durumda, bzıp2 veri sıkıştırma tüm işlem hattını yavaşlatmasını. Gzip sıkıştırma codec bileşenine geçiş, bu performans sorunu kolaylaştırmak.

## <a name="sample-scenarios-use-parallel-copy"></a>Örnek senaryoları: Paralel kopyayı kullanın
**Senaryo I:** 1.000 1 MB'lık dosyaları, şirket içi dosya sisteminden Blob depolama alanına kopyalayın.

**Analiz ve performans ayarlama**: Dört çekirdekli makine üzerinde ağ geçidi yüklediyseniz, örneğin, Data Factory eşzamanlı olarak Blob depolama alanına dosyaları dosya sisteminden taşımak için 16 paralel kopyalarını kullanır. Bu paralel yürütme, yüksek performans elde neden olur. Paralel kopya sayısı da açıkça belirtebilirsiniz. Çok sayıda küçük dosya kopyaladığınızda, paralel kopyalar önemli ölçüde işleme kaynakları kullanarak daha etkili bir şekilde yardımcı olur.

![Senaryo 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Senaryo II**: 500 MB'ın 20 blobları Blob depolamadan Data Lake Store Analytics'e kopyalayın ve ardından performansını ayarlama.

**Analiz ve performans ayarlama**: Bu senaryoda, Data Factory verileri Blob depolamadan Data Lake Store için tek kopyası kullanarak kopyalar (**parallelCopies** 1 olarak ayarlayın) ve tek bulut veri taşıma birimleri. Aktarım hızı, inceleyin, yakın olarak açıklanacaktır [performans başvuru bölümüne](#performance-reference).

![Senaryo 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Senaryo III**: Tek tek dosya boyutu, MB onlarca büyüktür ve toplam büyük birimdir.

**Analiz ve performans açma**: Artan **parallelCopies** kopyalama daha iyi performans tek bulut DMU kaynak sınırlamaları nedeniyle neden olmaz. Bunun yerine, daha fazla bulut veri taşımayı gerçekleştirmek için daha fazla kaynak almak için DMUs belirtmeniz gerekir. İçin bir değer belirtmeyin **parallelCopies** özelliği. Veri Fabrikası paralellik sizin yerinize çözer. Bu durumda, ayarlandıysa **cloudDataMovementUnits** 4'e, aktarım hızı hakkında dört kez gerçekleşir.

![Senaryo 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Başvuru
Performans izleme ve desteklenen veri depolarının bazılarını başvuruları ayarlama şunlardır:

* Azure Storage (Blob Depolama ve tablo depolama gibi): [Azure depolama ölçeklenebilirlik hedefleri](../../storage/common/storage-scalability-targets.md) ve [Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../../storage/common/storage-performance-checklist.md)
* Azure SQL Veritabanı: Yapabilecekleriniz [performansını izleme](../../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimi (DTU) yüzde denetleyin
* Azure SQL veri ambarı: Kendi özellik veri ambarı birimi (Dwu) ölçülür bkz: [Yönet işlem gücünü Azure SQL veri ambarı (Genel)](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Azure Cosmos DB'de performans düzeyleri](../../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [İzleme ve performansı ayarlama](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [Dosya sunucuları için performans ayarı](https://msdn.microsoft.com/library/dn567661.aspx)
