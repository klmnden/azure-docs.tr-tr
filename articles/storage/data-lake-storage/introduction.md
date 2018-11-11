---
title: Azure Data Lake depolama Gen2 Önizleme giriş
description: Azure Data Lake depolama Gen2 Önizlemesi'ne genel bakış sağlar
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: c86609ae5b993328beced468b74c7f2a1b65def4
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283623"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake depolama Gen2 Önizleme giriş

Azure Data Lake depolama Gen2 önizlemesi yerleşik, büyük veri analizi için ayrılmış özellikleri kümesidir [Azure Blob Depolama](../blobs/storage-blobs-introduction.md). Her iki dosya sistemi ve nesne depolama paradigmalarını kullanarak verilerinizi ile arabirim oluşturmasını sağlar. Data Lake depolama Gen2 eklenmesi, tüm verilerinizi analiz değeri ayıklamak yalnızca bulutta yer alan çok modlu platformda, Azure depolama sağlar.

Data Lake depolama Gen2 analiz verilerini Azure Storage'a tam yaşam döngüsü için gerekli olan tüm kalitelerini getirir. Bizim iki var olan depolama hizmetleri, Azure Blob Depolama ve Azure Data Lake depolama Gen1 yeteneklerini yakınsamaya sonucudur. Öğesinden özellikleri [Azure Data Lake depolama Gen1](../../data-lake-store/index.md)dosya sistemi sematiğini gibi dosya düzeyinde güvenlik ve ölçek ile düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özelliklerini birleştirilir [Azure BLOB Depolama](../blobs/storage-blobs-introduction.md).

## <a name="designed-for-enterprise-big-data-analytics"></a>Kurumsal büyük veri analizi için tasarlanmış

Data Lake depolama 2. nesil, kurumsal veri gölleri Azure'da oluşturmaya yönelik temel Azure depolama sağlar. Aktarım hızının Gigabit yüzlerce dayanıklılık geçtikten bilgilerini petabaytlarca birden çok hizmet vermek için baştan tasarlanan Data Lake depolama Gen2 oldukça büyük miktardaki verileri kolayca yönetmenize olanak sağlar.

Ek temel Data Lake depolama Gen2'ye ait olduğu bir [hiyerarşik ad alanı](./namespace.md) nesneleri/dosyaları dizinleri verimli veri erişimi için bir hiyerarşi halinde düzenler Blob Depolama hizmetine. Hiyerarşik ad alanı, Data Lake depolama Gen2'ye iki nesne deposu destekler ve aynı anda sistem paradigmalarını dosya da sağlar. Örneğin, ortak bir nesne deposu adlandırma kuralı hiyerarşik klasör yapısını taklit edecek şekilde adlarında eğik çizgi kullanır. Bu yapı ile Data Lake depolama Gen2 gerçek haline gelir. Bir dizini silme veya yeniden adlandırma gibi işlemler atomik meta veri işlemleri dizin yerine numaralandırma ve dizin adı öneki paylaşan tüm nesneleri işleme haline gelir.

Geçmişte, bulut tabanlı analiz performansı, yönetim ve güvenlik alanlarında tehlikeye gerekiyordu. Data Lake depolama Gen2 her biri bu görünüşler aşağıdaki yollarla ele alır:

- **Performans** kopyalayın veya verileri analiz için bir önkoşul olarak dönüştürme gerekmez çünkü optimize edilmiştir. Hiyerarşik ad alanı, büyük ölçüde genel iş performansı artıran dizin yönetimi işlemlerini performansını artırır.

- **Yönetim** düzenleyebilir ve dizinler ile alt dizinleri aracılığıyla dosyaları yönetmek için daha kolaydır.

- **Güvenlik** klasörler veya kişiler dosyaları üzerinde POSIX izinler tanımlayabilirsiniz uygulanabilir olmasıdır.

- **Maliyet uygunluğu** Data Lake depolama Gen2'ye üzerine düşük maliyetli şekilde gerçekleştirilir [Azure Blob Depolama](../blobs/storage-blobs-introduction.md). Ek özellikleri daha düşük maliyetli sahipliği büyük veri analizini Azure'da çalıştırmaya yönelik daha fazla.

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake depolama Gen2 temel özellikleri

> [!NOTE]
> Data Lake depolama Gen2'ye genel Önizleme sırasında aşağıda listelenen özelliklerden bazıları, kullanılabilirlik değişkenlik gösterebilir. Yeni özellikler ve bölgelerde Önizleme programına sırasında yayımlanan gibi bu bilgileri duyurulacaktır.
> [Kaydolun](https://aka.ms/adlsgen2signup) Data Lake depolama Gen2'ın genel önizlemesi için.  

- **Hadoop uyumlu erişim**: Data Lake depolama Gen2'ye yönetmenizi ve sahip olduğu gibi veri erişim sağlayan bir [Hadoop dağıtılmış dosya sistemi (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Yeni [ABFS sürücü](./abfs-driver.md) dahil olmak üzere tüm Apache Hadoop ortamlar içinde kullanılabilir [Azure HDInsight](../../hdinsight/index.yml) ve [Azure Databricks](../../azure-databricks/index.yml) Data Lake Store içinde depolanan verilere erişmek için 2. nesil.

- **POSIX izinleri kümesi**: ACL ve POSIX izinlerle birlikte için Data Lake depolama Gen2'ye özel bazı ek ayrıntı Data Lake Gen2 güvenlik modelini destekler. Ayarları, Yönetim Araçları veya Hive ve Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

- **Uygun maliyetli**: Data Lake depolama Gen2, düşük maliyetli depolama kapasitesi ve işlem sunar. Tam yaşam döngüsü aracılığıyla veri geçişi faturalandırma ücretleri tutma maliyetleri minimum yerleşik özellikleri aracılığıyla geçin; örneğin [Azure Blob Depolama yaşam döngüsü](../common/storage-lifecycle-managment-concepts.md).

- **Blob Depolama Araçlar, çerçeveler ve uygulamalar ile çalışır**: Data Lake depolama Gen2'ye bir çeşit Araçlar, çerçeveler ve Blob Depolama için bugün mevcut uygulamaları çalışmak devam eder.

- **En iyi duruma getirilmiş sürücü**: `abfs` sürücüsü [özellikle en iyi duruma getirilmiş](./abfs-driver.md) büyük veri analizi için. Karşılık gelen REST API'leri aracılığıyla çıkmış `dfs` uç noktasını `dfs.core.windows.net`.

## <a name="scalability"></a>Ölçeklenebilirlik

Azure depolama tasarım gereği ölçeklenebilir olup Data Lake depolama Gen2'ye veya Blob Depolama arabirimleri erişin. Depolama ve hizmet *birçok eksabaytlarca veriyi*. Bu depolama miktarını en yüksek düzeylerde giriş/çıkış işlemi (IOPS) saniye başına Gigabit / saniye (Gbps) cinsinden ölçülen aktarım hızı ile kullanılabilir. Yalnızca Kalıcılık hizmeti, hesap ve dosya düzeyinde ölçülen her istek için sabit yakın gecikmeleri, işleme yürütülür.

## <a name="cost-effectiveness"></a>Ekonomi

Azure Blob Depolama üzerine Data Lake depolama Gen2 oluşturmanın birçok yararından biri [düşük maliyetli](https://azure.microsoft.com/pricing/details/storage) işlem ve depolama kapasitesi. Veriler taşınmış ya da analiz gerçekleştirmeden önce dönüştürülmüş gerekli olmadığından diğer bulut Depolama hizmetlerinin aksine, Data Lake depolama Gen2 maliyetlerini düşürür.

Ayrıca, aşağıdakiler gibi özellikleri [hiyerarşik ad alanı](./namespace.md) birçok analytics işlerini genel performansını önemli ölçüde geliştirmek. Bu performans artışı uçtan uca analizi işi için bir alt toplam sahip olma maliyetini (TCO) bunun sonucunda aynı miktarda veri işlemek için daha az işlem gücü gerektiren anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler bazı ana kavramlar Data Lake depolama Gen2'ye ve ayrıntı depolamak için erişim, yönetmek ve verilerinizden Öngörüler elde etmeye açıklar:

* [Hiyerarşik ad alanı](./namespace.md)
* [Depolama hesabı oluşturma](./quickstart-create-account.md)
* [Azure Data Lake depolama 2. nesil ile bir HDInsight kümesi oluşturma](./quickstart-create-connect-hdi-cluster.md)
* [Azure Databricks'te bir Azure Data Lake depolama Gen2 hesabı kullanın](./quickstart-create-databricks-account.md)