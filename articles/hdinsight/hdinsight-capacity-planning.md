---
title: Küme kapasitesi Azure HDInsight planlama
description: Bir HDInsight kümesi için kapasite ve performans belirleme.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: hrasheed
ms.openlocfilehash: c910ed9f1160d30e1d4bda2e85b029eb2ad85b02
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237145"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Kapasite için HDInsight kümeleri planlama

Bir HDInsight kümesi dağıtmadan önce istenen küme kapasitesi için gerekli performansı ve ölçeği belirleyerek planlayın. Bu planlama yardımcı olur, kullanılabilirlik ve maliyetleri hem iyileştirin. Bazı küme kapasite kararları dağıtımdan sonra değiştirilemez. Performans parametrelerini değiştirirseniz, bir küme dismantled ve depolanan veri kaybı olmadan yeniden oluşturulacak.

Kapasite planlaması için sorulacak anahtar sorular şunlardır:

* Hangi coğrafi bölgede kümenizi dağıtmanız gerekir mi?
* Ne kadar depolama alanı gerekiyor?
* Küme türü, gereken dağıtma?
* Hangi boyutu ve sanal makine (VM) türü, Küme düğümlerinizi kullanmalısınız?
* Ne kadar çok çalışan düğümü kümenizi kullanmalı mı?

## <a name="choose-an-azure-region"></a>Bir Azure bölgesi seçin

Burada, küme fiziksel olarak sağlanan Azure bölgesini belirler. Okuma ve yazma gecikme süresini en aza indirmek için küme verilerinizi olması gerekir.

HDInsight, Azure bölgelerinde kullanılabilir. En yakın bölgeyi bulmak için bkz: *HDInsight* altında girdisi *Analytics* içinde [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

## <a name="choose-storage-location-and-size"></a>Depolama konumu ve boyutu seçin

### <a name="location-of-default-storage"></a>Varsayılan depolama konumu

Bir Azure depolama hesabına veya Azure Data Lake Storage, varsayılan depolama kümeniz ile aynı konumda olmalıdır. Azure depolama tüm konumlarda kullanılabilir. Data Lake depolama Gen1 bazı bölgelerde kullanılabilir: geçerli bir Data Lake Storage kullanılabilirlik seçeneğinin altında bkz *depolama* içinde [bölgeye göre Azure ürünleri kullanılabilir](https://azure.microsoft.com/regions/services/).

### <a name="location-of-existing-data"></a>Mevcut veri konumu

Zaten bir depolama hesabı veya Data Lake, verilerinizi içeren depolama ve bu depolama alanı, kümenin varsayılan depolama alanı olarak kullanmak istediğiniz kümenizin aynı yerde dağıtmanız gerekir.

### <a name="storage-size"></a>Depolama boyutu

Dağıtılmış bir HDInsight kümesine sonra ek Azure depolama hesapları ekleme ya da başka bir Data Lake depolama erişim. Tüm depolama hesapları, kümeniz ile aynı konumda bulunmalıdır. Bu, bazı veri okuma/yazma gecikmelere neden olabilir ancak bir Data Lake Storage farklı bir konumda olabilir.

Azure depolama alanına sahip bazı [kapasite sınırları](../azure-subscription-service-limits.md#storage-limits), Data Lake depolama Gen1 neredeyse sınırsızdır.

Bir küme farklı depolama hesaplarında birleşimi erişebilirsiniz. Tipik örnekleri şunlardır:

* Ne zaman veri miktarı tek bir blob depolama kapsayıcısı depolama kapasitesini aşacak.
* Ne zaman blob kapsayıcısına erişim hızı azaltma oluştuğu eşiği aşabilir.
* Veri yapmak istediğinizde, küme için kullanılabilir bir blob kapsayıcısını zaten yüklemiş.
* Güvenlik nedenleriyle depolama farklı bölümlerini ayırmak veya yönetimini basitleştirmek için istediğinizde.

48 düğümlü bir küme için 4-8 depolama hesapları öneririz. Yeterli toplam depolama alanı zaten olabilir ancak her depolama hesabı, işlem düğümleri için ek ağ bant genişliği sağlar. Birden çok depolama hesabında sahip olduğunuzda, bir önek olmayan her bir depolama hesabı için rastgele bir ad kullanın. Rastgele adlandırma amacı dışında tüm hesaplarda (azaltma) depolama performans sorunlarını veya ortak modu hataları olasılığını azaltır. Daha iyi performans için depolama hesabı başına yalnızca bir kapsayıcı kullanın.

## <a name="choose-a-cluster-type"></a>Bir küme türü seçin

HDInsight kümenizi gibi çalıştırmak üzere yapılandırılmış iş yükünün küme türünü belirler [Apache Hadoop](https://hadoop.apache.org/), [Apache Storm](https://storm.apache.org/), [Apache Kafka](https://kafka.apache.org/), veya [ Apache Spark](https://spark.apache.org/). Kullanılabilir küme türleri ayrıntılı bir açıklaması için bkz. [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md#cluster-types-in-hdinsight). Düğüm sayısı ve boyutu için gereksinimleri içeren bir belirli dağıtım topolojisi her küme türü vardır.

## <a name="choose-the-vm-size-and-type"></a>VM boyutunu ve türünü seçin

Her küme türü düğüm türleri kümesi vardır ve belirli seçenekler için VM boyutunu ve türünü her düğüm türü vardır.

Uygulamanız için en uygun küme boyutunu belirlemek için küme kapasitesinden Kıyaslama ve gösterildiği gibi boyutunu artırın. Örneğin, benzetilmiş bir iş yükü kullanabilirsiniz veya *kanarya sorgu*. Sanal bir iş yükü ile istediğiniz performans ulaşılana kadar kademeli olarak artırıldığında farklı boyutta kümelerinde, beklenen iş yükleri çalıştırılır. Kanarya bir sorgu kümesi yeterli kaynaklara sahip olup olmadığını göstermek için diğer üretim sorguları arasında düzenli olarak eklenebilir.

VM boyutunu ve türünü CPU işleme güç, RAM boyutu ve ağ gecikmesi tarafından belirlenir:

* CPU: Çekirdek sayısı ve VM boyutunu belirler. Daha fazla çekirdek, yüksek düzeyde Paralel hesaplama her düğüm elde edebilirsiniz. Ayrıca, bazı VM türleri daha hızlı çekirdeğe sahip.

* RAM: VM boyutunu, VM kullanılabilir RAM miktarını da belirler. Disk okuma, çalışan düğümlerinizin sağlamak yerine belleğindeki işleme için veri depolama iş yükleri için verileri sığdırmak için yeterli belleğe sahip.

* Ağ: Çoğu küme türleri için küme tarafından işlenen değil yerel diskte değil, Data Lake Storage veya Azure depolama gibi bir dış depolama hizmeti verilerdir. Düğüm sanal makine ve depolama hizmeti arasında aktarım hızı ve ağ bant genişliğini göz önünde bulundurun. Bir VM için kullanılabilir ağ bant genişliği, genellikle daha büyük boyutları ile artırır. Ayrıntılar için bkz [VM boyutları genel bakış](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

## <a name="choose-the-cluster-scale"></a>Küme ölçek seçin

Bir kümenin ölçeğini, VM düğümlerinin miktarı tarafından belirlenir. Tüm küme türleri için belirli bir ölçek sahip düğüm türleri ve ölçeği genişletme desteği düğüm türleri vardır. Örneğin, bir küme tam olarak üç gerektirebilir [Apache ZooKeeper](https://zookeeper.apache.org/) düğümleri veya iki baş düğüm. Dağıtılmış bir biçimde veri işleme çalışması gerçekleştirmek çalışan düğümü, ek çalışan düğümleri ekleyerek ölçek genişletmesi yararlı olabilir.

Küme türüne bağlı olarak çalışan düğümlerinin sayısını artırmak (örneğin, daha fazla çekirdeği) ek işlem kapasitesi ekler, ancak toplam bellek içi depolama işlenmekte olan verinin desteklemek tüm küme için gerekli bellek miktarını da ekleyebilirsiniz. Tercih ettiğiniz VM boyutunu ve türünü gibi doğru küme ölçek seçerek genellikle türü, sanal iş yükleri veya kanarya sorguları kullanarak ulaşıldı.

Yoğun yük taleplerini karşılamak ve sonra ölçeği onu tekrar ek düğümleri artık gerekli olmadığında, kümeniz ölçeklendirebilirsiniz. Daha fazla bilgi için [ölçek HDInsight kümeleri](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Küme yaşam döngüsü

Bir kümenin ömrü boyunca ücretlendirilir. Yalnızca belirli saatler kümesi oluşturma ve çalıştırma ihtiyacınız varsa, yapabilecekleriniz [Azure Data Factory kullanarak isteğe bağlı kümeler oluşturma](hdinsight-hadoop-create-linux-clusters-adf.md). Sağlama ve kümenizi sildiğinizden PowerShell betikleri oluşturabilir ve ardından bu komut dosyalarını kullanarak zamanlama [Azure Otomasyonu](https://azure.microsoft.com/services/automation/).

> [!NOTE]  
> Bir küme silindiğinde, varsayılan Hive meta veri deposu da silinir. Sonraki küme yeniden oluşturma için meta veri deposu kalıcı hale getirmek için Azure veritabanı gibi bir dış meta veri deposunu kullanın veya [Apache Oozie](https://oozie.apache.org/).
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Küme iş hataları yalıtın

Bazen hatalar birden çok eşlemelerinin Paralel yürütme nedeniyle oluşabilir ve çok düğümlü bir küme bileşenleri azaltın. Sorunu, eşzamanlı olarak çalışan Dağıtılmış test etmeyi denemek yardımcı olmak için tek düğümlü bir küme üzerinde birden çok iş genişletin bu yaklaşım, birden fazla işi aynı anda birden fazla düğüm içeren kümelerinde çalıştırılır. Azure'da bir tek düğümlü HDInsight kümesi oluşturmak için kullanın *Gelişmiş* seçeneği.

Ayrıca, bir tek düğümlü geliştirme ortamını yerel bilgisayarınıza yükleyin ve çözümü burada test. Hortonworks, Hadoop tabanlı çözümler için kullanışlı, kavram kanıtı ilk geliştirme ve test bir tek düğümlü yerel geliştirme ortamı sağlar. Daha fazla bilgi için [Hortonworks korumalı alanı](https://hortonworks.com/products/hortonworks-sandbox/).

Yerel bir tek düğümlü bir küme sorunu tanımlamak için başarısız olan işler yeniden çalıştırın ve giriş verileri ihtiyaçlarımızı veya daha küçük veri kümeleri kullanın. Bu işleri nasıl çalıştırdığınız platform ve uygulama türüne bağlıdır.

## <a name="quotas"></a>Kotalar

Hedef küme VM boyutu, ölçek ve türünü belirledikten sonra aboneliğinizi geçerli kota kapasite sınırları denetleyin. Bir kota sınırına ulaştığında, yeni kümeler dağıtmak veya daha fazla alt düğüm ekleyerek mevcut kümelerin ölçeğini genişletme mümkün olmayabilir. Yalnızca kota sınırı, her abonelik için bölge düzeyinde bulunan CPU çekirdek kotasını ' dir. Örneğin, aboneliğinizin çekirdek limiti 30 Doğu ABD bölgesinde olabilir. Bir kota artırım talebinde bulunmak gerekiyorsa aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Seçin **Yardım + Destek** sayfanın sol alt tarafında.
1. SELECT deyiminde **yeni destek isteği**.
1. Üzerinde **yeni destek isteği** sayfasındaki **Temelleri** sekmesinde, aşağıdaki seçenekleri belirleyin:
   - **Sorun türü**: **Hizmet ve abonelik sınırlarını (kotalar)**
   - **Abonelik**: değiştirmek istediğiniz abonelik
   - **Kota türü**: **HDInsight**
    
     ![HDInsight çekirdek kotasını artırmak için bir destek isteği oluşturun](./media/hdinsight-capacity-planning/hdinsight-quota-support-request.png)

1. Seçin **sonraki: Çözümleri >>** .
1. Üzerinde **ayrıntıları** sayfasında, sorunun bir açıklama girin, önem derecesi sorunu, tercih ettiğiniz iletişim yöntemi ve diğer gerekli alanları seçin.
1. Seçin **sonraki: Gözden geçir + Oluştur >>** .
1. Üzerinde **gözden geçir + Oluştur** sekmesinde **Oluştur**.

> [!NOTE]  
> Özel bir bölgesini HDInsight çekirdek kotasını artırmak gerekiyorsa [beyaz liste talebinizi](https://aka.ms/canaryintwhitelist).

Yapabilecekleriniz [bir kota artırım talebinde bulunmak Destek ekibiyle iletişime geçin](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

Ancak, bazı sabit kota sınırları vardır, örneğin, tek bir Azure aboneliği en fazla 10.000 çekirdek olabilir. Bu sınırlar hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits).

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop, Spark, Kafka ve daha fazlası ile HDInsight kümelerinde ayarlama](hdinsight-hadoop-provision-linux-clusters.md): Ayarlama ve Apache Hadoop, Spark, Kafka, Interactive Hive, HBase, ML Hizmetleri veya Storm ile HDInsight kümelerini yapılandırma konusunda bilgi edinin.
* [Küme performansını izleme](hdinsight-key-scenarios-to-monitor.md): Kümenizin kapasitesi etkileyebilecek HDInsight kümeniz için izlemek için önemli senaryolar hakkında bilgi edinin.
