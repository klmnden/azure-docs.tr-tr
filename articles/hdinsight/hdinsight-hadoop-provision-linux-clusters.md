---
title: "Kurulum Hadoop, Spark, Kafka, HBase veya R Server - Azure Hdınsight küme | Microsoft Docs"
description: "Hadoop, Kafka, Spark, HBase, R Server veya Storm kümeleri Hdınsight için bir tarayıcı, Azure CLI, Azure PowerShell, REST veya SDK ayarlayın."
keywords: "hadoop kümesi kurulumu, kafka Küme kurulumu, spark Küme kurulumu, hadoop kümesinde nedir"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 23a01938-3fe5-4e2e-8e8b-3368e1bbe2ca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2017
ms.author: jgao
ms.openlocfilehash: af4538bb398e6b18aeb9703ba5099b0e2c70fa64
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="set-up-clusters-in-hdinsight-with-hadoop-spark-kafka-and-more"></a>Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ayarlama ve hdınsight'ta Hadoop, Spark, Kafka, etkileşimli sorgusu, HBase, R Server veya Storm kümelerini yapılandırma konusunda bilgi edinin. Ayrıca, kümelerini özelleştirme ve bir etki alanına katılarak güvenlik ekleme hakkında bilgi edinin.

Hadoop kümesi birkaç sanal makinelerin görevleri dağıtılmış işlem için kullanılan (düğümler) oluşur. Yalnızca genel yapılandırma bilgileri girmeniz gerekir böylece azure Hdınsight uygulama ayrıntılarını yükleme ve yapılandırma bireysel düğümleri, işler. 

> [!IMPORTANT]
>Hdınsight küme faturalandırma bir küme oluşturulur ve küme silindiğinde durdurur sonra başlar. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bilgi nasıl [bir küme silin.](hdinsight-delete-cluster.md)
>

## <a name="cluster-setup-methods"></a>Küme kurulumu yöntemleri
Aşağıdaki tabloda bir Hdınsight kümesini ayarlamak için kullanabileceğiniz farklı yöntemler gösterir.

| Oluşturulan kümeleri | Web tarayıcısı | Komut satırı | REST API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure portalı](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI (ver 1.0)](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Azure Resource Manager şablonları](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Hızlı oluştur: temel Küme kurulumu
Bu makalede, Kurulum'da anlatılmaktadır [Azure portal](https://portal.azure.com), bir Hdınsight kümesi kullanarak oluşturabileceğiniz *hızlı Oluştur* veya *özel*. 

![hdınsight oluşturma seçenekleri özel hızlı oluştur](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-options.png)

Temel Küme kurulumu yapmak için ekrandaki yönergeleri izleyin. Ayrıntılar için aşağıda verilmiştir:

* [Kaynak grubu adı](#resource-group-name)
* [Küme türleri ve yapılandırma](#cluster-types) 
* [Küme oturum açma ve SSH kullanıcı adı](#cluster-login-and-ssh-username)
* [Konum](#location)

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight 3.3 devre dışı bırakma](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a name="resource-group-name"></a>Kaynak grup adı 

[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) yardımcı olan bir grup olarak, uygulamanızdaki kaynaklarla çalışma başvurduğu bir Azure kaynak grubu. Dağıtmak, güncelleştirme, izlemek veya tüm kaynakları tek ve eşgüdümlü bir işlemde, uygulamanız için silin.

## <a name="cluster-types"></a>Küme türleri ve yapılandırma
Azure Hdınsight şu anda aşağıdaki küme türü, her biri belirli işlevlerin sağlamak için bileşenleri kümesi sağlar.

> [!IMPORTANT]
> Hdınsight kümeleri, her bir tek iş yükü veya teknoloji için çeşitli türlerde kullanılabilir. Bir küme üzerinde Storm ve HBase gibi birden çok tür birleştiren bir küme oluşturmak için desteklenen yöntem yoktur. Çözümünüzün birden çok Hdınsight küme türleri arasında yayılır teknolojileri gerektiriyorsa bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türleri bağlanabilir. 
>
>

| Küme türü | İşlev |
| --- | --- |
| [Hadoop](hadoop/apache-hadoop-introduction.md) |Toplu sorgu ve depolanan veri analizi |
| [HBase](hbase/apache-hbase-overview.md) |Büyük miktarlarda şemasız, NoSQL veri işleme |
| [Etkileşimli sorgu](./interactive-query/apache-interactive-query-get-started.md) |Etkileşimli ve daha hızlı Hive sorguları için bellek içi önbelleğe alma |
| [Kafka](kafka/apache-kafka-introduction.md) | Gerçek Zamanlı Akış veri ardışık düzen ve uygulamaları oluşturmak için kullanılan dağıtılmış bir akış platformu |
| [R Server](r-server/r-server-overview.md) |Çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi özellikleri |
| [Spark](spark/apache-spark-overview.md) |Bellek içi işleme, etkileşimli sorgular mikro toplu iş akışı işleme |
| [Storm](storm/apache-storm-overview.md) |Gerçek zamanlı olay işleme |


### <a name="hdinsight-version"></a>Hdınsight sürümü
Bu küme için Hdınsight'ın sürümünü seçin. Daha fazla bilgi için bkz: [desteklenen Hdınsight sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="enterprise-security-package"></a>Kurumsal güvenlik paketi

Hadoop, Spark ve etkileşimli sorgu küme türleri için etkinleştirmeyi seçebilirsiniz **Kurumsal güvenlik paketi**. Bu paket, Apache bırakabilmenizi kullanarak ve Azure Active Directory ile tümleştirmek için daha güvenli bir Küme kurulumu seçeneği sağlar. Daha fazla bilgi için bkz: [Kurumsal güvenlik paketi Azure hdınsight'ta](./domain-joined/apache-domain-joined-introduction.md).

![hdınsight oluşturma seçenekleri Kurumsal güvenlik paketi seçin](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-enterprise-security-package.png)

Etki alanına katılmış Hdınsight oluşturma hakkında daha fazla bilgi için küme için bkz: [oluşturma etki alanına katılmış Hdınsight sandbox ortamını](./domain-joined/apache-domain-joined-configure.md).


## <a name="cluster-login-and-ssh-user-name"></a>Küme oturum açma ve SSH kullanıcı adı
Hdınsight kümeleri ile küme oluşturma sırasında iki kullanıcı hesapları yapılandırabilirsiniz:

* HTTP kullanıcı: varsayılan kullanıcı adı *yönetici*. Azure Portal'da temel yapılandırmayı kullanır. Bazen "kullanıcı küme." çağrılır
* SSH kullanıcı (Linux kümeleri): SSH aracılığıyla kümeye bağlanmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Kurumsal güvenlik paketi, Hdınsight Apache bırakabilmenizi ve Active Directory ile tümleştirmenize olanak sağlar. Birden çok kullanıcı Enteprise güvenlik paketi kullanılarak oluşturulabilir.

## <a name="location"></a>Kümeleri ve depolama için konum (bölge)

Küme konumun açıkça belirtmeniz gerekmez: varsayılan depolama ile aynı konumda kümedir. Desteklenen bölgelerin bir listesi için tıklatın **bölge** aşağı açılan listede [Hdınsight fiyatlandırma](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Küme için depolama uç noktaları

Bulutta Hadoop şirket içi yüklemesini kümesindeki depolama alanını için Hadoop dağıtılmış dosya sistemi (HDFS) kullansa da, kümeye bağlı depolama uç noktaları kullanın. Hdınsight kümeleri kullanın ya da [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) veya [Azure Storage blobları](hdinsight-hadoop-use-blob-storage.md). Azure Storage veya Data Lake Store kullanarak, verilerinizi bekletirken hesaplama için kullanılan Hdınsight kümelerinin silebileceğiniz anlamına gelir. 

> [!WARNING]
> Hdınsight kümesi farklı bir konumdan bir ek depolama hesabıyla desteklenmiyor.

Yapılandırması sırasında varsayılan depolama uç noktası için bir Azure Storage hesabı veya bir Data Lake Store bir blob kapsayıcısını belirtin. Uygulama ve sistem varsayılan depolama içeren günlükleri. İsteğe bağlı olarak, ek bağlı Azure Storage hesaplarını ve küme erişebilmeniz için Data Lake Store hesapları belirtebilirsiniz. Hdınsight kümesi ve bağımlı depolama hesapları aynı Azure konumuna olması gerekir.

![Küme depolama ayarları: HDFS uyumlu depolama uç noktaları](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>İsteğe bağlı meta deponuz
İsteğe bağlı Hive veya Oozie meta depolar oluşturabilirsiniz. Ancak, tüm küme türleri meta deponuz desteklemez ve Azure SQL Data Warehouse meta deponuz ile uyumlu değil. 

> [!IMPORTANT]
> Özel bir meta depo oluşturduğunuzda, kısa çizgiler, kısa çizgi veya veritabanı adında boşluk kullanmayın. Bu, küme oluşturma işleminin başarısız olmasına neden olabilir.

### <a name="use-hiveoozie-metastore"></a>Hive meta depo

Hdınsight kümesi silindikten sonra Hive tablolarını korumak istiyorsanız, özel bir meta depo kullanın. Bu gibi durumlarda, meta depo sonra başka bir Hdınsight kümesine ekleyebilirsiniz.

Bir Hdınsight kümesi sürüm oluşturulan Hdınsight meta depo farklı Hdınsight küme sürümleri arasında paylaşılamaz. Hdınsight sürümlerinin listesi için bkz: [desteklenen Hdınsight sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie meta depo

Oozie kullanırken, performansı artırmak için özel bir meta depo kullanın. Kümenizi sildikten sonra bir meta depo ayrıca Oozie iş verilerine erişim sağlayabilir. 

> [!IMPORTANT]
> Özel bir Oozie meta depo tekrar kullanamazsınız. Özel bir Oozie meta depo kullanmak için Hdınsight kümesi oluştururken, boş bir Azure SQL veritabanı sağlamanız gerekir.

## <a name="configure-cluster-size"></a>Küme boyutunu yapılandırın

Kümenin mevcut olduğu sürece için düğüm kullanım için faturalandırılır. Bir küme oluşturulur ve küme silindiğinde durdurduğunda faturalama başlatır. Kümeleri XML'deki ayrıldı veya beklemeye.


### <a name="number-of-nodes-for-each-cluster-type"></a>Her küme türü için düğüm sayısı
Her küme türü düğümleri, düğümleri ve varsayılan VM boyutu terminolojisi kendi sayısına sahip. Aşağıdaki tabloda, her düğüm türü için düğümleri parantez içinde sayısıdır.

| Tür | Düğümler | Diyagram |
| --- | --- | --- |
| Hadoop |Baş düğümü (2) veri düğümü (1 +) |![Hdınsight Hadoop küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |HEAD sunucusu (2), bölge (1 +), ana/ZooKeeper düğümü (3) |![Hdınsight HBase küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus düğümü (2), yönetici sunucu (1 +), ZooKeeper düğümü (3) |![Hdınsight Storm küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Baş düğümü (2), çalışan düğümüne (1 +), ZooKeeper düğümü (3) (A1 ZooKeeper VM boyutu için boş) |![Hdınsight Spark küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Daha fazla bilgi için bkz: [varsayılan düğümü yapılandırması ve sanal makine boyutları kümeleri için](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) "Hadoop bileşenleri ve Hdınsight sürümlerde nedir?",

Hdınsight kümeleri maliyetini düğümlerin ve düğümler için sanal makine boyutları sayısı tarafından belirlenir. 

Farklı küme türü farklı düğüm türleri, düğümleri ve düğümü boyutları sayısı vardır:
* Hadoop küme türü varsayılan: 
    * İki *baş düğümler*  
    * Dört *veri düğümler*
* Storm kümesi türü varsayılan: 
    * İki *Nimbus düğümü*
    * Üç *ZooKeeper düğümleri*
    * Dört *yönetici düğümler* 

Yalnızca Hdınsight çalışıyorsanız, bir veri düğümü kullanmanızı öneririz. Hdınsight fiyatlandırma hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> Küme boyutu Azure abonelikleri arasında değişiklik gösterir. Kişi [Azure Fatura Desteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) sınırını artırın.
>

Küme yapılandırmak için Azure Portalı'nı kullandığınızda, düğüm boyutu aracılığıyla kullanılabilir **düğüm fiyatlandırma katmanları** dikey. Portalda, farklı bir düğüme boyutlarıyla ilişkili maliyeti de görebilirsiniz. 

![Hdınsight VM düğümü boyutları](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Sanal makine boyutları 
Küme dağıtımı dağıtmayı planladığınız çözümü temel alan işlem kaynakları seçin. Aşağıdaki VM'ler Hdınsight kümeleri için kullanılır:
* A ve D1 4 serisi VMs: [genel amaçlı Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11-14 serisi VM: [bellek için iyileştirilmiş Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Değer çıkışı bulmak için farklı SDK'ları kullanarak bir küme oluştururken bir VM boyutu belirtin veya Azure PowerShell kullanırken görmek için kullanması gereken [Hdınsight kümeleri için kullanmak üzere VM boyutları](../cloud-services/cloud-services-sizes-specs.md#size-tables). Bu bağlantılı makaleden değeri kullanmak **boyutu** tabloların sütun.

> [!IMPORTANT]
> Bir kümede 32'den fazla alt düğüm gerekirse, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir.
>
>

Daha fazla bilgi için bkz: [sanal makineler için Boyutlar](../virtual-machines/windows/sizes.md). Çeşitli boyutlarda fiyatlandırma hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Özel küme Kurulumu
Özel küme Kurulumu derlemeleri üzerinde hızlı ayarlar oluşturun ve aşağıdaki seçenekleri ekler:
- [Hdınsight uygulamaları](#hdinsight-applications)
- [Küme boyutu](#cluster-size)
- Gelişmiş ayarlar
  - [Betik eylemleri](#customize-clusters-using-script-action)
  - [Sanal ağ](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>Kümelere HDInsight uygulamaları yükleme

HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Microsoft, üçüncü tarafların veya, kendi ürettiğiniz sağlanan uygulamaları kullanabilir. Daha fazla bilgi için bkz: [Azure Hdınsight'ta üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

Hdınsight uygulamalarının çoğunu bir boş kenar düğümüne yüklenir.  Bir boş kenar düğümüne yüklenir ve yapılandırılır. baş düğüm olduğu gibi aynı istemci araçları ile Linux sanal makine var. Kenar düğümüne küme erişmek, istemci uygulamalarınızı test etme ve istemci uygulamalarını barındırmak için kullanabilirsiniz. Daha fazla bilgi için bkz: [Hdınsight'ta boş kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Gelişmiş ayarlar: betik eylemleri

Ek bileşenleri yüklemek veya oluşturma sırasında komut dosyalarını kullanarak küme yapılandırmasını özelleştirebilirsiniz. Bu tür komut dosyaları aracılığıyla çağrılır **betik eylemi**, Azure portal, Hdınsight Windows PowerShell cmdlet'lerini veya Hdınsight .NET SDK'sı kullanılabilir bir yapılandırma seçeneği değil. Daha fazla bilgi için bkz: [betik eylemi kullanarak özelleştirme Hdınsight kümesi](hdinsight-hadoop-customize-cluster-linux.md).

Mahout ve basamaklama, gibi yerel bazı Java bileşenleri kümede Java arşiv (JAR) dosyaları olarak çalıştırabilirsiniz. Bu JAR dosyalarını Azure depolama alanına dağıtılmış ve Hdınsight kümeleri için Hadoop iş gönderme mekanizmaları ile gönderilir. Daha fazla bilgi için bkz: [gönderme Hadoop işleri program aracılığıyla](hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]
> Hdınsight kümelerine JAR dosyalarını dağıtma sorunları var veya Hdınsight kümelerinde JAR dosyalarını çağırma başvurun [Microsoft Support](https://azure.microsoft.com/support/options/).
>
> Geçişli Hdınsight tarafından desteklenmiyor ve Microsoft Support uygun değil. Desteklenen bileşenlerin bir listesi için bkz: [Hdınsight tarafından sağlanan küme sürümlerindeki yenilikler](hdinsight-component-versioning.md).
>
>

Bazı durumlarda, aşağıdaki yapılandırma dosyalarını oluşturma işlemi sırasında yapılandırmak istediğiniz:

* clusterIdentity.xml
* Core-site.xml
* Gateway.XML
* hbase env.xml
* hbase-site.xml
* hdfs-site.xml
* Hive env.xml
* Hive-site.xml
* mapred site
* oozie-site.xml
* oozie env.xml
* Storm-site.xml
* Tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Daha fazla bilgi için bkz: [önyükleme kullanarak özelleştirme Hdınsight kümelerini](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Gelişmiş ayarlar: sanal ağ kümeleriyle genişletme
Çözümünüzün birden çok Hdınsight küme türleri arasında yayılır teknolojileri gerektiriyorsa bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türleri bağlanabilir. Bu yapılandırma, kümeler ve birbirleriyle doğrudan iletişim kurmak için bunları dağıttığınız herhangi bir kod sağlar.

Hdınsight ile Azure sanal ağı kullanma hakkında daha fazla bilgi için bkz: [genişletmek Hdınsight Azure sanal ağlar ile](hdinsight-extend-hadoop-virtual-network.md).

Bir Azure sanal ağı içindeki iki küme türleri kullanma örneği için bkz: [kullanım Spark yapılandırılmış akış ile Kafka](hdinsight-apache-kafka-spark-structured-streaming.md). Sanal ağ için belirli yapılandırma gereksinimlerini içeren bir sanal ağ ile Hdınsight kullanma hakkında daha fazla bilgi için bkz: [Azure Virtual Network kullanarak genişletme Hdınsight yetenekleri](hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Erişim denetimi sorunlarını giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

- [Hdınsight, Hadoop ekosistemi ve Hadoop kümeleri nedir?](hadoop/apache-hadoop-introduction.md)
- [HDInsight'ta Hadoop kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Bir Windows PC Hdınsight'ta Hadoop ile çalışma](hdinsight-hadoop-windows-tools.md)
