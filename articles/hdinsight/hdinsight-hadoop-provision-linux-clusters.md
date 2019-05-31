---
title: Apache Hadoop, Apache Spark, Apache Kafka, Apache HBase ve R Server - Azure HDInsight için Kurulum küme
description: Hadoop, Kafka, Spark, HBase, R Server veya Storm kümeleri HDInsight için bir tarayıcı, Klasik Azure CLI, Azure PowerShell, REST veya SDK'sı ayarlayın.
keywords: hadoop kümesi kurulumu, kafka küme kurulum hadoop kümesi nedir, spark kümesi Kurulumu
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 05/28/2019
ms.openlocfilehash: 351b6a8e056d22fa8f2d695a2722b39b9771c8b0
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299382"
---
# <a name="set-up-clusters-in-hdinsight-with-apache-hadoop-apache-spark-apache-kafka-and-more"></a>Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight kümelerinde ayarlama

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ayarlama ve Apache Hadoop, Apache Spark, Apache Kafka, Interactive Query, Apache HBase, ML Hizmetleri veya Apache Storm ile HDInsight kümelerini yapılandırma konusunda bilgi edinin. Ayrıca, kümelerini özelleştirin ve bir etki alanına katılarak güvenlik ekleme konusunda bilgi edinin.

Bir Hadoop kümesi çeşitli görevleri dağıtılmış işlem için kullanılan sanal makinelerin (düğümler) oluşur. Azure HDInsight, uygulama ayrıntılarını yükleme ve yapılandırma tek tek düğümlerin yalnızca genel yapılandırma bilgilerini zorunda işler.

> [!IMPORTANT]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bilgi edinmek için nasıl [küme silme.](hdinsight-delete-cluster.md)

## <a name="cluster-setup-methods"></a>Küme kurulumu yöntemleri
Aşağıdaki tabloda, bir HDInsight kümesini ayarlamak için kullanabileceğiniz farklı yöntemler gösterilmektedir.

| İle oluşturulmuş kümeleri | Web tarayıcısı | Komut satırı | REST API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure portal](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Azure Resource Manager şablonları](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Hızlı oluştur: Temel Küme kurulumu
Bu makale, kurulumda size [Azure portalında](https://portal.azure.com)kullanarak bir HDInsight kümesi oluşturabileceğiniz *hızlı oluşturma* veya *özel*. 

![hdınsight oluşturma özel Hızlı oluşturma seçenekleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-options.png)

Temel Küme kurulumu için ekrandaki yönergeleri izleyin. Ayrıntılar için aşağıda verilmiştir:

* [Kaynak grubu adı](#resource-group-name)
* [Küme türleri ve yapılandırma](#cluster-types) 
* Küme oturum açma ve SSH kullanıcı adı
* [Konum](#location)

## <a name="resource-group-name"></a>Kaynak grubu adı

[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kaynaklarla bir grup olarak uygulamanızdaki yardımcı bir Azure kaynak grubu için denir. Dağıtma, güncelleştirme, izleme veya tek bir Eşgüdümlü işlemle uygulamanıza yönelik tüm kaynakları silin.

## <a name="cluster-types"></a> Küme türleri ve yapılandırma
Azure HDInsight, şu anda aşağıdaki küme türlerinden her biri belirli işlevleri sağlamak için bileşenler kümesi sağlar.

> [!IMPORTANT]  
> HDInsight kümeleri, her bir tek iş yükü veya teknoloji için çeşitli türlerde kullanılabilir. Bir küme üzerinde Storm ve HBase gibi birden birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur. Çözümünüz birden çok HDInsight küme türleri arasında yayılır teknolojileri gerektiriyorsa bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türleri bağlanabilirsiniz. 

| Küme türü | İşlevi |
| --- | --- |
| [Hadoop](hadoop/apache-hadoop-introduction.md) |Toplu işlem sorguları ve depolanan verilerin analizi |
| [HBase](hbase/apache-hbase-overview.md) |Büyük miktarlarda şemasız, NoSQL verileri için işleme |
| [Etkileşimli sorgu](./interactive-query/apache-interactive-query-get-started.md) |Etkileşimli ve daha hızlı Hive sorguları için bellek içi caching |
| [Kafka](kafka/apache-kafka-introduction.md) | Gerçek Zamanlı Akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılan dağıtılan bir akış platformu |
| [ML Hizmetleri](r-server/r-server-overview.md) |Çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi özellikleri |
| [Spark](spark/apache-spark-overview.md) |Bellek içi işleme, etkileşimli sorgular, akış mikro toplu işleme |
| [Storm](storm/apache-storm-overview.md) |Gerçek zamanlı olay işleme |


### <a name="hdinsight-version"></a>HDInsight sürümü
Bu küme için HDInsight'ın sürümünü seçin. Daha fazla bilgi için [HDInsight desteklenen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).


## <a name="cluster-login-and-ssh-username"></a>Küme oturum açma ve SSH kullanıcı adı
HDInsight kümeleri ile küme oluşturma sırasında iki kullanıcı hesaplarını yapılandırabilirsiniz:

* HTTP kullanıcısı: Varsayılan kullanıcı adı *admin* şeklindedir. Azure portalında temel yapılandırmayı kullanır. Bazen "kullanıcı küme" adlandırılır
* SSH kullanıcısı: SSH üzerinden kümeye bağlanmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Kurumsal güvenlik paketi, HDInsight Apache ranger'ın Active Directory ile tümleştirmenize olanak tanır. Birden çok kullanıcı Kurumsal güvenlik paketi kullanılarak oluşturulabilir.

## <a name="location"></a>Kümeleri ve depolama için konum (bölge)

Küme konumu açıkça belirtmeniz gerekmez: Varsayılan depolama alanı ile aynı konumda bir kümedir. Desteklenen bölgelerin bir listesi için tıklatın **bölge** aşağı açılan listede [HDInsight fiyatlandırma](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Kümeler için depolama uç noktaları

Bulutta Hadoop şirket içi yüklemesini kümede depolama için Hadoop dağıtılmış dosya sistemi (HDFS) kullansa da, kümeye bağlı depolama uç noktaları kullanın. HDInsight kümeleri kullanın ya da [Azure Data Lake Storage](hdinsight-hadoop-use-data-lake-store.md) veya [Azure Depolama'daki blobları](hdinsight-hadoop-use-blob-storage.md). Azure Depolama'da veya Data Lake Storage kullanarak yine de verilerinizi korurken hesaplama için kullanılan HDInsight kümelerinin silebileceğiniz anlamına gelir. 

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabı kullanma desteklenmiyor.

Yapılandırma sırasında varsayılan depolama uç noktası için bir Azure depolama hesabı veya Data Lake Storage bir blob kapsayıcısını belirtin. Uygulama ve sistem varsayılan depolama alanı içeren günlükleri. İsteğe bağlı olarak, ek bağlı Azure Storage hesaplarını ve kümeye erişmek Data Lake Storage hesaplarını belirtebilirsiniz. HDInsight kümesi ve bağımlı depolama hesapları aynı Azure konumunda olmalıdır.

![Küme depolama ayarları: HDFS uyumlu depolama uç noktaları](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>İsteğe bağlı meta depolar
İsteğe bağlı Hive veya Apache Oozie meta depolar oluşturabilirsiniz. Ancak, tüm küme türleri meta depolar destek ve Azure SQL veri ambarı meta depolar ile uyumlu değil. 

Daha fazla bilgi için [Azure HDInsight, harici meta veri depolarını kullanma](./hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]  
> Özel bir meta veri deposu oluşturduğunuzda, kısa çizgi, kısa çizgi veya veritabanı adı boşluk kullanmayın. Bu, küme oluşturma işlemi başarısız olmasına neden olabilir.

### <a name="use-hiveoozie-metastore"></a>Hive meta veri deposu

Bir HDInsight kümesi silindikten sonra Hive tablolarını korumak istiyorsanız, özel bir meta veri deposu kullanın. Ardından, meta veri deposu başka bir HDInsight kümesine ekleyebilirsiniz.

Bir HDInsight kümesi sürüm oluşturulan bir HDInsight meta veri deposu farklı HDInsight küme sürümleri arasında paylaşılamaz. HDInsight sürümleri listesi için bkz. [HDInsight desteklenen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie meta veri deposu

Oozie kullanırken performansı artırmak için özel bir meta veri deposu kullanın. Kümenizi sildikten sonra bir meta veri deposu Oozie iş verilerine erişim de sağlayabilirsiniz. 

> [!IMPORTANT]  
> Özel Oozie meta veri deposu yeniden kullanamazsınız. Özel Oozie meta veri deposu kullanmak için HDInsight kümesi oluştururken boş bir Azure SQL veritabanı sağlamanız gerekir.


## <a name="custom-cluster-setup"></a>Özel küme Kurulumu
Hızlı Kurulum derlemelerinde özel küme ayarlarını oluşturun ve aşağıdaki seçenekleri ekler:
- [Kurumsal güvenlik paketi](#enterprise-security-package)
- [HDInsight uygulamaları](#install-hdinsight-applications-on-clusters)
- [Küme boyutu](#configure-cluster-size)
- [Betik eylemleri](#advanced-settings-script-actions)
- [Sanal ağ](#advanced-settings-extend-clusters-with-a-virtual-network)
 
## <a name="enterprise-security-package"></a>Kurumsal güvenlik paketi

Hadoop, Spark, HBase, Kafka ve etkileşimli sorgu kümesi türleri için etkinleştirmeyi seçebilirsiniz **Kurumsal güvenlik paketi**. Bu paket, Azure Active Directory ile tümleştirme ve Apache Ranger'ı kullanarak daha güvenli bir Küme kurulumu için seçeneği sağlar. Daha fazla bilgi için [Azure HDInsight, Kurumsal güvenlik paketi](./domain-joined/apache-domain-joined-introduction.md).

![hdınsight oluşturma seçenekleri Kurumsal güvenlik paketi seçin](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-enterprise-security-package.png)

Küme etki alanına katılmış HDInsight oluşturma hakkında daha fazla bilgi için bkz: [Oluştur etki alanına katılmış HDInsight korumalı ortamda](./domain-joined/apache-domain-joined-configure.md). 

## <a name="install-hdinsight-applications-on-clusters"></a>Kümelere HDInsight uygulamaları yükleme

HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Microsoft, üçüncü tarafların veya, kendinizi geliştirin sağlanan uygulamaları kullanabilir. Daha fazla bilgi için [Azure HDInsight üzerinde üçüncü taraf Apache Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

HDInsight uygulamaları çoğu bir boş kenar düğümüne yüklenir.  Aynı istemci araçları yüklü ve yapılandırılmış olduğu gibi baş düğüm ile Linux sanal makinesi bir boş kenar düğümüdür. Kümeye erişen istemci uygulamalarınızı test etme ve istemci uygulamalarınızı barındırmak için kenar düğümünü kullanabilirsiniz. Daha fazla bilgi için [HDInsight içinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md).

## <a name="configure-cluster-size"></a>Küme boyutu yapılandırın

Kümenin var olduğu sürece düğüm kullanım için faturalandırılırsınız. Faturalandırma küme oluşturulduğunda ve küme silindiğinde sona erer başlar. Kümeleri edilemez veya beklemeye alınamaz.

### <a name="number-of-nodes-for-each-cluster-type"></a>Her küme türü için düğüm sayısı
Her küme türü kendi sayısı düğüm, düğümleri ve varsayılan VM boyutu için terimler vardır. Aşağıdaki tabloda, parantez içinde her düğüm türü için düğümler sayısıdır.

| Type | Düğümler | Diyagram |
| --- | --- | --- |
| Hadoop |Baş düğüm (2) çalışan düğümü (1 +) |![HDInsight Hadoop küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |HEAD sunucu (2), bölge sunucusu (1 +), master/ZooKeeper düğümü (3) |![HDInsight HBase küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus düğümü (2), gözetmen sunucusu (1 +), ZooKeeper düğümü (3) |![HDInsight Storm küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Baş düğüm (2) çalışan düğümü (1 +), ZooKeeper düğümü (3) (A1 ZooKeeper VM boyutu için ücretsiz) |![HDInsight Spark küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Daha fazla bilgi için [varsayılan küme düğümü yapılandırması ve sanal makine boyutları](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) "Hadoop bileşenleri ve HDInsight sürümlerinde nelerdir?",

HDInsight kümeleri maliyetini düğümlerin ve düğümler için sanal makine boyutları sayısına göre belirlenir. 

Farklı küme türü farklı bir düğüme türlerinin sayıda düğüm ve düğüm boyutları vardır:
* Hadoop küme türü varsayılan: 
    * İki *baş düğümlerine*  
    * Dört *çalışan düğümleri*
* Storm küme türü varsayılan: 
    * İki *Nimbus düğümleri*
    * Üç *ZooKeeper düğümleri*
    * Dört *gözetmen düğümleri* 

HDInsight yalnızca deniyorsanız, bir çalışan düğümü kullanmanızı öneririz. HDInsight fiyatlandırma hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırma](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]  
> Küme boyutu sınırı, Azure abonelikleri arasında değişiklik gösterir. İlgili kişi [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) sınırını artırın.

Küme yapılandırmak için Azure portalı kullandığınızda, düğüm boyutunu aracılığıyla **düğüm fiyatlandırma katmanları** dikey penceresi. Portalda farklı düğümü boyutları ile ilişkili maliyeti de görebilirsiniz. 

![HDInsight VM düğümü boyutları](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Sanal makine boyutları 
Kümeleri dağıtırken dağıtmayı planladığınız çözüme göre işlem kaynaklarını seçin. Aşağıdaki sanal makineleri, HDInsight kümeleri için kullanılır:
* Ve A serisi VM'ler D1-4: [Genel amaçlı bir Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11-14 serisi VM: [Bellek için iyileştirilmiş bir Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Değer dışarı bulmak için farklı SDK'larını kullanarak bir küme oluştururken bir VM boyutu veya Azure PowerShell kullanırken kullanmalısınız [HDInsight kümeleri için kullanılacak VM boyutları](../cloud-services/cloud-services-sizes-specs.md#size-tables). Bu bağlantılı makalesinden değeri kullandığınızdan **boyutu** tabloların sütun.

> [!IMPORTANT]  
> Bir kümedeki 32'den fazla alt düğüme ihtiyacınız varsa, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir.

Daha fazla bilgi için [sanal makine boyutları](../virtual-machines/windows/sizes.md). Çeşitli boyutlardaki fiyatlandırması hakkında daha fazla bilgi için bkz: [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="advanced-settings-script-actions"></a>Gelişmiş ayarlar: Betik eylemleri

Ek bileşenleri yüklemek veya oluşturma sırasında komut dosyalarını kullanarak küme yapılandırmasını özelleştirebilirsiniz. Bu betikler aracılığıyla çağrılan **betik eylemi**, Azure portalı, HDInsight Windows PowerShell cmdlet'leri veya HDInsight .NET SDK'sı kullanılabilir bir yapılandırma seçeneği olan. Daha fazla bilgi için [betik eylemi kullanarak özelleştirme HDInsight küme](hdinsight-hadoop-customize-cluster-linux.md).

Apache Mahout ve basamaklama, gibi yerel bazı Java bileşenlerini kümesinde Java arşiv (JAR) dosyaları olarak çalıştırabilirsiniz. Bu JAR dosyaları Azure depolama alanına dağıtılmış ve HDInsight kümelerine Hadoop işi gönderme mekanizmaları ile gönderildi. Daha fazla bilgi için [gönderme Apache Hadoop işlerini program aracılığıyla](hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]  
> Sorunları JAR dosyalarını HDInsight kümelerine dağıtma veya HDInsight kümelerinde JAR dosyaları ile iletişime geçin [Microsoft Support](https://azure.microsoft.com/support/options/).
>
> Geçişli HDInsight tarafından desteklenmiyor ve Microsoft Support uygun değil. Desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan küme sürümlerindeki yenilikler](hdinsight-component-versioning.md).

Bazı durumlarda, aşağıdaki yapılandırma dosyalarını oluşturma işlemi sırasında yapılandırmak istiyorsanız:

* clusterIdentity.xml
* Core-site.xml
* Gateway.XML
* hbase env.xml
* hbase-site.xml
* hdfs-site.xml
* Hive env.xml
* Hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Daha fazla bilgi için [özelleştirme HDInsight kümeleri Bootstrap ile](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Gelişmiş ayarlar: Bir sanal ağ ile kümeleri genişletme
Çözümünüz birden çok HDInsight küme türleri arasında yayılır teknolojileri gerektiriyorsa bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türleri bağlanabilirsiniz. Bu yapılandırma, kümeler ve doğrudan birbirleri ile iletişim kurmak için bunlara dağıttığınız herhangi bir kod sağlar.

HDInsight ile bir Azure sanal ağı kullanma hakkında daha fazla bilgi için bkz. [genişletmek HDInsight ile Azure sanal ağları](hdinsight-extend-hadoop-virtual-network.md).

Bir Azure sanal ağ içindeki iki küme türleri kullanma örneği için bkz: [kullanın Apache Spark yapılandırılmış akışı ile Apache Kafka](hdinsight-apache-kafka-spark-structured-streaming.md). Sanal ağ için belirli yapılandırma gereksinimlerine de dahil olmak üzere bir sanal ağda HDInsight'ı kullanma hakkında daha fazla bilgi için bkz. [kullanarak Azure sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md).


## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight, Apache Hadoop ekosistemindeki ve Hadoop kümeleri nedir?](hadoop/apache-hadoop-introduction.md)
- [HDInsight Apache Hadoop kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Bir Windows PC ile gelen HDInsight üzerinde Apache Hadoop çalışma](hdinsight-hadoop-windows-tools.md)
