---
title: Azure Data Lake depolama Gen2'ye Giriş
description: Azure Data Lake depolama Gen2'ye genel bakış sağlar
services: storage
author: jamesbak
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 8777a7504c48b22d0e670dd9f0d28016ac8918db
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009472"
---
# <a name="introduction-to-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2'ye Giriş

Azure Data Lake depolama Gen2 üzerinde oluşturulmuş, büyük veri analizi için ayrılmış özellikleri kümesidir [Azure Blob Depolama](storage-blobs-introduction.md). Data Lake depolama Gen2 bizim iki var olan depolama hizmetleri, Azure Blob Depolama ve Azure Data Lake depolama Gen1 yeteneklerini yakınsamaya sonucudur. Öğesinden özellikleri [Azure Data Lake depolama Gen1](https://docs.microsoft.com/azure/data-lake-store/index)gibi dosya sistemi sematiğini, dizin ve dosya düzeyinde güvenlik ve ölçek ile düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özelliklerini birleştirilir[Azure Blob Depolama](storage-blobs-introduction.md).

## <a name="designed-for-enterprise-big-data-analytics"></a>Kurumsal büyük veri analizi için tasarlanmış

Data Lake depolama 2. nesil, kurumsal veri gölleri Azure'da oluşturmaya yönelik temel Azure depolama sağlar. Aktarım hızının Gigabit yüzlerce dayanıklılık geçtikten bilgilerini petabaytlarca birden çok hizmet vermek için baştan tasarlanan Data Lake depolama Gen2 oldukça büyük miktardaki verileri kolayca yönetmenize olanak sağlar.

Ek temel Data Lake depolama Gen2'ye ait olduğu bir [hiyerarşik ad alanı](data-lake-storage-namespace.md) Blob Depolama. Hiyerarşik ad alanı nesneler/dosya dizinleri verimli veri erişimi için bir hiyerarşi halinde düzenler. Ortak bir nesne deposu adlandırma kuralı, bir hiyerarşik dizin yapısını taklit edecek şekilde adlarında eğik çizgi kullanır. Bu yapı ile Data Lake depolama Gen2 gerçek haline gelir. Bir dizini silme veya yeniden adlandırma gibi işlemler atomik meta veri işlemleri dizin yerine numaralandırma ve dizin adı öneki paylaşan tüm nesneleri işleme haline gelir.

Geçmişte, bulut tabanlı analiz performansı, yönetim ve güvenlik alanlarında tehlikeye gerekiyordu. Data Lake depolama Gen2 her biri bu görünüşler aşağıdaki yollarla ele alır:

-   **Performans** kopyalayın veya verileri analiz için bir önkoşul olarak dönüştürme gerekmez çünkü optimize edilmiştir. Hiyerarşik ad alanı büyük ölçüde genel iş performansı artıran dizin yönetimi işlemlerini performansını artırır.

-   **Yönetim** düzenleyebilir ve dizinler ile alt dizinleri aracılığıyla dosyaları yönetmek için daha kolaydır.

-   **Güvenlik** , uygulanabilir, çünkü dizin veya tek tek dosyalar üzerinde POSIX izinler tanımlayabilirsiniz.

-   **Maliyet uygunluğu** Data Lake depolama Gen2'ye üzerine düşük maliyetli şekilde gerçekleştirilir [Azure Blob Depolama](storage-blobs-introduction.md). Ek özellikleri daha düşük maliyetli sahipliği büyük veri analizini Azure'da çalıştırmaya yönelik daha fazla.

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake depolama Gen2 temel özellikleri

-   **Hadoop uyumlu erişim**: Data Lake depolama Gen2'ye yönetmenizi ve sahip olduğu gibi veri erişim sağlayan bir [Hadoop dağıtılmış dosya sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Yeni [ABFS sürücü](data-lake-storage-abfs-driver.md) dahil olmak üzere tüm Apache Hadoop ortamlar içinde kullanılabilir [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/index)*,* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/index), ve [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/) Data Lake depolama 2. nesil'deki depolanan verilere erişmek için.

-   **POSIX izinleri kümesi**: ACL ve POSIX izinlerle birlikte için Data Lake depolama Gen2'ye özel bazı ek ayrıntı Data Lake Gen2 güvenlik modelini destekler. Ayarları, Depolama Gezgini veya Hive ve Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

-   **Uygun maliyetli**: Data Lake depolama Gen2, düşük maliyetli depolama kapasitesi ve işlem sunar. Tam yaşam döngüsü aracılığıyla veri geçişi faturalandırma ücretleri tutma maliyetleri minimum yerleşik özellikleri aracılığıyla geçin; örneğin [Azure Blob Depolama yaşam döngüsü](storage-lifecycle-management-concepts.md).

-   **En iyi duruma getirilmiş sürücü**: ABFS sürücü [özellikle en iyi duruma getirilmiş](data-lake-storage-abfs-driver.md) büyük veri analizi için. Karşılık gelen REST API uç noktası aracılığıyla çıkmış `dfs.core.windows.net`.

### <a name="scalability"></a>Ölçeklenebilirlik

Azure depolama tasarım gereği ölçeklenebilir olup Data Lake depolama Gen2'ye veya Blob Depolama arabirimleri erişin. Depolama ve hizmet *birçok eksabaytlarca veriyi*. Bu depolama miktarını en yüksek düzeylerde giriş/çıkış işlemi (IOPS) saniye başına Gigabit / saniye (Gbps) cinsinden ölçülen aktarım hızı ile kullanılabilir. Yalnızca Kalıcılık hizmeti, hesap ve dosya düzeyinde ölçülen her istek için sabit yakın gecikmeleri, işleme yürütülür.

### <a name="cost-effectiveness"></a>Ekonomi

Düşük maliyetli depolama kapasitesi ve işlemleri Azure Blob Depolama üzerine Data Lake depolama Gen2 oluşturmanın birçok avantaj biridir. Diğer bulut Depolama hizmetlerinin aksine, Data Lake depolama 2. nesil'deki depolanan verileri taşınamaz veya Analiz gerçekleştirmeden önce dönüştürülmüş için gerekli değildir. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage).

Ayrıca, aşağıdakiler gibi özellikleri [hiyerarşik ad alanı](data-lake-storage-namespace.md) birçok analytics işlerini genel performansını önemli ölçüde geliştirmek. Bu performans artışı uçtan uca analizi işi için bir alt toplam sahip olma maliyetini (TCO) bunun sonucunda aynı miktarda veri işlemek için daha az işlem gücü gerektiren anlamına gelir.

### <a name="one-service-multiple-concepts"></a>Bir hizmet, birden çok kavramları

Data Lake depolama Gen2, büyük veri analizi, Azure Blob Depolama üzerine kurulu için ek bir özelliktir. Varken birçok Avantajdan yararlanarak mevcut platform bileşenleri oluşturmak ve analiz için veri gölleri işletmek için BLOB içinde aynı, paylaşılan öğeleri açıklayan birden çok kavramlara neden olmaz.

Eşdeğer varlıklar farklı kavramları açıklandığı aşağıda verilmiştir. Bu varlıkların doğrudan eşanlamlı aksi belirtilmediği sürece:

| Kavram                                | Üst düzey kuruluş | Daha düşük bir düzenleme düzeyi                                            | Veri kapsayıcısı |
|----------------------------------------|------------------------|---------------------------------------------------------------------|----------------|
| BLOB'lar-genel amaçlı nesne depolama | Kapsayıcı              | Sanal dizin (SDK'sı yalnızca – sağlamaz atomik işleme) | Blob           |
| ADLS Gen2 – depolama analizi          | Dosya sistemi             | Dizin                                                           | Dosya           |

## <a name="supported-open-source-platforms"></a>Açık kaynak platformlar desteklenir

Data Lake depolama Gen2'ye birden fazla açık kaynak platformları destekler. Bu platformlar aşağıdaki tabloda görüntülenir.

> [!NOTE]
> Bu tabloda görünen sürümleri desteklenir.

| Platform |  Desteklenen sürümler | Daha Fazla Bilgi |
| --- | --- | --- |
| [HDInsight](https://azure.microsoft.com/services/hdinsight/) | 3.6 + | [Apache Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilen nelerdir?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning?toc=%2Fen-us%2Fazure%2Fhdinsight%2Fstorm%2FTOC.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
| [Hadoop](https://hadoop.apache.org/) | 3.2+ | [Apache Hadoop arşiv serbest bırakır.](https://hadoop.apache.org/release.html) |
| [Cloudera](https://www.cloudera.com/) | 6.1 + | [Cloudera Enterprise 6.x sürüm notları](https://www.cloudera.com/documentation/enterprise/6/release-notes/topics/rg_cdh_6_release_notes.html) |
| [Azure Databricks](https://azure.microsoft.com/services/databricks/) | 5.1+ | [Databricks çalışma zamanı sürümleri](https://docs.databricks.com/release-notes/runtime/databricks-runtime-ver.html) |
|[Hortonworks](https://hortonworks.com/)| 3.1.x++ | [Bulut veri erişimi yapılandırma](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cloudbreak-2.9.0/cloud-data-access/content/cb_configuring-access-to-adls2.html) |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler bazı ana kavramlar Data Lake depolama Gen2'ye ve ayrıntı depolamak için erişim, yönetmek ve verilerinizden Öngörüler elde etmeye açıklar:

-   [Hiyerarşik ad alanı](data-lake-storage-namespace.md)
-   [Depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)
-   [Azure Databricks'te bir Data Lake depolama Gen2 hesabı kullanın](data-lake-storage-quickstart-create-databricks-account.md)
