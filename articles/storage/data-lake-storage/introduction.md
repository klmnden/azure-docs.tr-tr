---
title: Azure Data Lake Storage Gen2 Önizleme giriş
description: Azure Data Lake Storage Gen2 önizlemesi genel bir bakış sağlar
services: storage
documentationcenter: ''
author: jamesbak
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 0631b1d0c8da925858f0b7fb1d778cb1161eb737
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061533"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage Gen2 Önizleme giriş

Azure Data Lake Storage Gen2 Önizleme üstünde oluşturulmuş büyük veri analizi için ayrılmış özellikler kümesidir [Azure Blob Depolama](../blobs/storage-blobs-introduction.md). Her iki dosya sistemi ve nesne depolama örneklerinde kullanarak verilerinizi ile arabirim sağlar. Bu Data Lake Storage Gen2 analytics değer tüm verilerinizi almanıza olanak sağlayan bulut tabanlı yalnızca birden çok kalıcı depolama hizmeti sağlar.

Data Lake Storage Gen2 analiz verileri tam yaşam döngüsü için gerekli olan tüm nitelikleri özellikleri. Bu iki varolan depolama hizmetlerimizle yeteneklerini yakınsamaya sonuçlanır. Öğesinden özellikleri [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), dosya sistemi sematiğini gibi dosya-güvenlik düzeyine hem de ölçek, düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleri ve büyük bir SDK/Araçları ile birleştirilir gelen ekosistemi [Azure Blob Depolama](../blobs/storage-blobs-introduction.md). Bir dosya sistemi arabirimi avantajları ekleme analizi için iş yüklerini en iyi duruma getirilmiş sırada Data Lake Storage Gen2 içinde nesne depolama tüm niteliklerini kalır.

## <a name="designed-for-enterprise-big-data-analytics"></a>Kurumsal büyük veri analizi için tasarlanmış

Data Lake Storage Gen2 kurumsal veri Göller (EDL) oluşturmak için temel depolama Azure üzerinde hizmetidir. Başından verimlilik Gigabit yüzlerce sürekliliğiyle sırasında bilgilerin birden çok Petabayt hizmet vermek için tasarlanmış, Data Lake Storage Gen2 oldukça büyük miktardaki verileri yönetmek için kolay bir yol sağlar.

Temel Data Lake Storage Gen2 eklenmesi özelliğidir bir [hiyerarşik ad alanı](./namespace.md) Blob Depolama hizmetine dizinlerin kullanıcı veri erişimi için bir hiyerarşiye nesneleri/dosyaları düzenler. Hiyerarşik ad alanı da aynı zamanda sistem örneklerinde dosya ve her iki nesne deposu desteklemek Data Lake depolama Gen2 sağlar. Örneği için ortak bir nesne deposu adlandırma kuralı hiyerarşik klasör yapısını taklit edecek şekilde adında eğik çizgi kullanır. Bu yapı Data Lake Storage Gen2 ile gerçek haline gelir. Yeniden adlandırma veya dizin silme gibi işlemleri dizin yerine numaralandırma ve dizin adı öneki paylaşan tüm nesneleri işleme atomik meta veri işlemleri olur.

Geçmişte, bulut tabanlı analytics performans, yönetim ve güvenlik alanlarında tehlikeye gerekiyordu. Data Lake Storage Gen2 her bu yönlerinin biri aşağıdaki yollarla ele alır:

- **Performans** kopyalayın veya veri çözümleme için bir önkoşul olarak dönüştürmek gerekmediği için optimize edilmiştir. Hiyerarşik ad büyük ölçüde genel iş performansını artıran dizin yönetim işlemlerini performansını artırır.

- **Yönetim** düzenleyebilir ve dizinler ve alt dizinleri aracılığıyla dosyaları işlemek için daha kolay olur.

- **Maliyet verimliliği** Data Lake Storage Gen2 üstünde düşük maliyetli yerleşik olarak mümkün hale getirilir [Azure Blob Depolama](../blobs/storage-blobs-introduction.md). Ek özellikler daha düşük sahip olma maliyeti büyük veri analizi Azure üzerinde çalışan için daha fazla.

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake Storage Gen2 temel özellikleri

> [!NOTE]
> Data Lake Storage Gen2 genel Önizleme sırasında aşağıda listelenen özelliklerden bazıları, kullanılabilirliklerini farklılık gösterebilir. Yeni özellikler ve bölgeler Önizleme programı sırasında yayımlanan gibi bu bilgileri duyurulacaktır.
> [Kaydolun](https://aka.ms/adlsgen2signup) Data Lake Storage Gen2 genel önizlemesi için.  

- **Hadoop uyumlu erişim**: Data Lake Storage Gen2 sağlar, yönetmek ve ile yaptığınız gibi verilere erişmek bir [Hadoop dağıtılmış dosya sistemi (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Yeni [ABFS sürücü](./abfs-driver.md) dahil olmak üzere tüm Apache Hadoop ortamlar içinde kullanılabilir [Azure Hdınsight](../../hdinsight/index.yml) ve [Azure Databricks](../../azure-databricks/index.yml) Data Lake Store içinde depolanan verilere erişmek için Gen2.

- **Çoklu Protokol ve birden çok kalıcı veri erişimi**: Data Lake Storage Gen2 olarak kabul bir **çok kalıcı** depolama birimi hizmeti olarak nesne deposu ve dosya sistemi arabirimleri aynı verilere sağlar **aynı zaman**. Bu, aynı verilere erişmenizi birden fazla protokol uç noktası sağlayarak gerçekleştirilir. 

    Diğer analytics çözümlerden farklı olarak, Data Lake Storage Gen2 içinde depolanan verileri taşıyabilir veya analiz araçları, çeşitli çalıştırmadan önce dönüştürülmesi gerekmez. Geleneksel aracılığıyla verilere erişebilir [Blob Depolama API'leri](../blobs/storage-blobs-introduction.md) (örneğin: aracılığıyla veri alma [olay hub'ları yakalama](../../event-hubs/event-hubs-capture-enable-through-portal.md)) ve Hdınsight veya Azure Databricks aynı anda kullanarak bu verileri işleme. 

- **Düşük maliyetli**: Data Lake Storage Gen2 özellikleri düşük maliyetli depolama kapasitesi ve işlemler. Kendi tam döngüsü boyunca veri geçişler fatura oranları tutma maliyetleri minimum yerleşik özellikleri aracılığıyla gibi değiştirmek [Azure Blob Depolama yaşam döngüsü](../common/storage-lifecycle-managment-concepts.md).

- **Blob storage araçları, çerçevelerinin ve uygulamalar ile çalışır**: çok çeşitli araçlar, çerçeveler ve bugün için Blob Depolama mevcut uygulamaları çalışmak Data Lake Storage Gen2 devam eder.

- **En iyi duruma getirilmiş sürücü**: `abfs` sürücü [özellikle en iyi duruma getirilmiş](./abfs-driver.md) büyük veri analizi için. Karşılık gelen REST API'leri aracılığıyla çıkmış `dfs` uç noktasını `dfs.core.windows.net`.

## <a name="scalability"></a>Ölçeklenebilirlik

Azure depolama tasarım gereği ölçeklenebilir olup, Data Lake Storage Gen2 veya Blob Depolama arabirimleri erişebilirsiniz. Depolamak ve hizmet *birçok eksabayt boyutlarındaki veriler*. Bu depolama alanı miktarı en yüksek düzeylerde girdi/çıktı işlemleri (IOPS) saniye başına Gigabit / saniye (Gbps) cinsinden ölçülen işleme ile kullanılabilir. Yalnızca Kalıcılık hizmeti, hesap ve dosya düzeyinde ölçülen sabiti yakın istek başına gecikmeleri, işleme yürütülür.

## <a name="cost-effectiveness"></a>Ekonomi

Data Lake Storage Gen2 Azure Blob Depolama üstünde oluşturmanın birçok yararından biri [düşük maliyetli](https://azure.microsoft.com/pricing/details/storage) depolama kapasitesi ve işlemler. Veriler taşınmış veya Analiz gerçekleştirmeden önce dönüştürülmüş için gerekli olmadığından diğer bulut depolama hizmetleri, Data Lake Storage Gen2 maliyetlerini düşürür.

Ayrıca, aşağıdaki gibi özellikleri [hiyerarşik ad alanı](./namespace.md) birçok analytics işleri genel performansını önemli ölçüde artırmak. Bu geliştirme performans anlamına gelir uçtan uca analytics işi için bir alt toplam sahip olma maliyetini (TCO) içinde kaynaklanan aynı miktarda veri işlemek için daha az işlem gücü gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler bazı ana kavramlar Data Lake Storage Gen2 ve ayrıntı depolamak, erişim, yönetmek ve verilerinizden serisidir anlatmaktadır:

* [Hiyerarşik ad alanı](./namespace.md)
* [Depolama hesabı oluşturma](./quickstart-create-account.md)
* [Azure Data Lake Storage Gen2 ile Hdınsight kümesi oluşturma](./quickstart-create-connect-hdi-cluster.md)
* [Bir Azure Data Lake Storage Gen2 hesap Azure Databricks kullanın](./quickstart-create-databricks-account.md) 