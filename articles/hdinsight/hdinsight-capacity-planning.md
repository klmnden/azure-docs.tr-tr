---
title: Küme kapasite Azure Hdınsight'ta planlama | Microsoft Docs
description: Bir Hdınsight kümesi için kapasite ve performans belirleme.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: maxluk
ms.openlocfilehash: 8a8344388e9d31846770d5989d1ddd43fbe15336
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37047488"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Kapasite Hdınsight kümeleri için planlama

Hdınsight kümesi dağıtmadan önce istenen küme kapasiteyi gerekli performansı ve ölçeği belirleyerek planlayın. Bu planlama yardımcı kullanılabilirliğini ve maliyetleri en iyi duruma getirme. Bazı küme kapasite kararları dağıtımdan sonra değiştirilemez. Performans parametreleri değiştirirseniz, bir küme dismantled ve depolanan veri kaybı olmadan yeniden oluşturulacak.

Kapasite planlama için sorulacak anahtar sorular şunlardır:

* Hangi coğrafi bölgede kümenizi dağıtmanız gerekir?
* Ne kadar depolama alanı gerekiyor?
* Küme türü, gereken dağıtmak?
* Hangi boyutu ve sanal makine (VM) türü, Küme düğümlerinizi kullanmalısınız?
* Kaç tane çalışan düğümleri kümenizi kullanmalı mı?

## <a name="choose-an-azure-region"></a>Bir Azure bölgesi seçin

Azure bölgesinde kümenizi fiziksel olarak nerede sağlandığında belirler. Okuma ve yazma işlemlerinin gecikmesini en aza indirmek için küme verilerinizi olması gerekir.

Hdınsight birçok Azure bölgelerde kullanılabilir. En yakın bölgeyi bulmak için bkz: *Hdınsight Linux* altında girdisi *veri + analiz* içinde [bölgeye göre Azure ürünleri kullanılabilen](https://azure.microsoft.com/regions/services/).

## <a name="choose-storage-location-and-size"></a>Depolama konumu ve boyutu seçin

### <a name="location-of-default-storage"></a>Varsayılan depolama konumunu

Bir Azure depolama hesabı veya bir Azure Data Lake Store, varsayılan depolama kümeniz ile aynı konumda olması gerekir. Azure depolama tüm konumlarda kullanılabilir. Data Lake Store bazı bölgelerde kullanılabilir - geçerli Data Lake Store kullanılabilirlik altında bkz *depolama* içinde [bölgeye göre Azure ürünleri kullanılabilen](https://azure.microsoft.com/regions/services/).

### <a name="location-of-existing-data"></a>Mevcut verilerin konumu

Zaten verilerinizi içeren bir depolama hesabı veya Data Lake Store varsa ve bu depolama, kümenin varsayılan depolama alanı olarak kullanmak istediğiniz, kümenizi aynı yerde dağıtmanız gerekir.

### <a name="storage-size"></a>Depolama boyutu

Dağıtılan bir Hdınsight kümesi oluşturduktan sonra ek Azure depolama hesapları ekleme veya diğer veri Gölü depoları erişim. Tüm depolama hesapları kümeniz ile aynı konumda bulunması gerekir. Bu bazı veri okuma/yazma gecikmesi çıkarabilir olsa da, bir Data Lake Store farklı bir konumda olabilir.

Azure depolama alanına sahip bazı [kapasite limitlerini](../azure-subscription-service-limits.md#storage-limits), Data Lake Store neredeyse sınırsız olsa.

Bir küme farklı depolama hesapları bileşimini erişebilir. Tipik örnekler şunlardır:

* Ne zaman veri miktarını tek blob depolama kapsayıcısını depolama kapasitesini aşan olasıdır.
* Ne zaman blob kapsayıcısını erişim hızı azaltma oluştuğu eşiğini aşabilir.
* Veri yapmak istediğinizde, küme için kullanılabilir bir blob kapsayıcısını zaten yüklemiş.
* Güvenlik nedenleriyle depolama farklı kısımlarını ayırmak veya yönetimini basitleştirmek için istediğinizde.

48 düğümlü bir küme için 4-8 depolama hesapları öneririz. Yeterli toplam depolama alanı zaten olabilir ancak her depolama hesabı işlem düğümlerini için ek ağ bant genişliği sağlar. Birden çok depolama hesapları varsa bir öneki olmadan her depolama hesabı için rastgele bir ad kullanın. Rastgele adlandırma amacı dışında tüm hesapları arasında depolama performans sorunlarını (azaltma) veya ortak modu hataları olasılığını azaltma. Daha iyi performans için depolama hesabı başına yalnızca bir kapsayıcı kullanın.

## <a name="choose-a-cluster-type"></a>Bir küme türü seçin

Küme türü Hdınsight kümenizi, Hadoop, Storm, Kafka veya Spark gibi çalışacak şekilde yapılandırılmış iş yükü belirler. Kullanılabilir küme türleri ayrıntılı bir açıklaması için bkz: [Azure Hdınsight giriş](hadoop/apache-hadoop-introduction.md#cluster-types-in-hdinsight). Her küme türü düğüm sayısı ve boyutu gereksinimlerini içeren bir belirli dağıtım topolojisi sahiptir.

## <a name="choose-the-vm-size-and-type"></a>VM boyutunu ve türünü seçin

Her küme türü düğüm türleri kümesi, ve her düğüm türü kendi VM boyutu ve türü için belirli seçenekleri sahiptir.

Uygulamanız için en uygun küme boyutunu belirlemek için küme kapasite Kıyaslama ve belirtildiği gibi boyutunu artırın. Örneğin, sanal bir iş yükü kullanabilirsiniz veya *yalancı sorgu*. Sanal bir iş yükü ile istediğiniz performans ulaşılana kadar kademeli olarak boyutunu artırmayı farklı boyutu kümelerinde, beklenen iş yükleri çalıştırın. Yalancı sorgu küme yeterli kaynaklara sahip olup olmadığını göstermek için diğer üretim sorgular arasında düzenli olarak eklenebilir.

VM boyutunu ve türünü işleme güç, RAM boyutu ve ağ gecikmesini CPU tarafından belirlenir:

* CPU: Çekirdek sayısı ve VM boyutunu belirler. Daha fazla çekirdek, Paralel hesaplama büyük ölçüde her düğüm elde edebilirsiniz. Ayrıca, bazı VM türleri daha hızlı çekirdeğe sahip.

* RAM: VM boyutu VM'yi kullanılabilir RAM miktarını da belirler. Disk okuma çalışan düğümleri sağlamak yerine işlemeyle ilgili bellekte verileri depolamak iş yükleri için verileri sığdırmak için yeterli belleğe sahip.

* : Çoğu küme türleri için küme tarafından işlenen veri değil yerel diskte değil, Data Lake Store veya Azure depolama gibi bir dış depolama hizmeti ağdır. VM düğümü ve depolama hizmeti arasında üretilen iş ve ağ bant genişliğini göz önünde bulundurun. Bir VM için kullanılabilir ağ bant genişliği genellikle daha büyük boyutları ile artırır. Ayrıntılar için bkz [VM boyutları genel bakış](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

## <a name="choose-the-cluster-scale"></a>Küme ölçek seçme

Bir kümenin ölçek VM düğümlerinden miktarı tarafından belirlenir. Tüm küme türleri için belirli bir ölçek sahip düğüm türleri ve genişletme desteği düğüm türleri vardır. Örneğin, bir küme tam olarak üç ZooKeeper düğümleri veya iki baş düğüm gerektirebilir. Dağıtılmış bir şekilde veri işleme yapmak çalışan düğümleri, ek alt düğüm ekleyerek ölçeğini ndan yararlanabilirsiniz.

Türüne bağlı olarak, küme, alt düğümlerin sayısını artırmak (örneğin, daha fazla sayıda çekirdek) ek hesaplama kapasitesi ekler, ancak toplam bellek içi işlenmekte olan verilerin depolanmasını desteklemek üzere tüm küme için gerekli bellek miktarını da ekleyebilirsiniz. Seçiminde, VM boyutunu ve türünü değiştirme gibi doğru küme ölçek seçiliyor genellikle empirically, benzetimli iş yükleri veya yalancı sorguları kullanarak ulaşıldı.

Yoğun yük taleplerini karşılamak, ardından ölçeklemek aşağıya ek düğümleri artık gerekmediğinde kümenizin ölçeklendirebilirsiniz. Daha fazla bilgi için bkz: [ölçek Hdınsight kümeleri](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Küme yaşam döngüsü

Bir kümenin ömrü için sizden ücret kesilir. Yalnızca küme ve çalışan gereken belirli zamanlar varsa, şunları yapabilirsiniz [Azure Data Factory kullanarak isteğe bağlı kümelerini oluşturmak](hdinsight-hadoop-create-linux-clusters-adf.md). Ayrıca, sağlamak ve kümenizi sildiğinizden PowerShell komut dosyaları oluşturmak ve bu komut dosyalarını kullanarak zamanlama [Azure Otomasyonu](https://azure.microsoft.com/services/automation/).

> [!NOTE]
> Bir küme silindiğinde, kendi varsayılan Hive meta depo de silinir. Sonraki küme yeniden oluşturma meta depo kalıcı hale getirmek için bir dış meta veri deposu Azure veritabanı veya Oozie gibi kullanın.
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Küme iş hataları yalıtma

Bazen hataları birden çok harita Paralel yürütme nedeniyle oluşur ve çok düğümlü bir küme üzerinde düşürün. Sorunu yalıtmak, dağıtılmış eşzamanlı çalıştırarak test etmeyi denemek yardımcı olmak için tek düğümlü bir küme üzerindeki birden çok işleri'ni genişletin birden fazla düğüm içeren kümeler üzerinde aynı anda birden çok işlerini çalıştırmak için bu yaklaşım. Bir tek düğümlü Hdınsight kümesi oluşturmak için kullanın *Gelişmiş* seçeneği.

Ayrıca, yerel bilgisayarınızda bir tek düğümlü geliştirme ortamını yükleyin ve çözümü vardır sınayın. Hortonworks Hadoop tabanlı çözümler için yararlı kavramı, kanıtını ilk geliştirme ve test bir tek düğümlü yerel geliştirme ortamı sağlar. Daha fazla bilgi için bkz: [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/).

Yerel bir tek düğümlü bir küme sorunu tanımlamak için başarısız olan işler yeniden çalıştırın ve giriş verilerini ayarlamak veya daha küçük veri kümeleri kullanın. Bu işleri çalışma şeklini platform ve uygulama türüne bağlıdır.

## <a name="quotas"></a>Kotalar

Hedef küme VM boyutu, ölçek ve tür belirlendikten sonra aboneliğiniz geçerli kota kapasitesi sınırlarını denetleyin. Kota sınırına ulaştığında, yeni kümeler dağıtmak veya daha fazla alt düğüm ekleyerek var olan kümeleri ölçeklendirme mümkün olmayabilir. En yaygın kota sınırına ulaşıldı abonelik, bölgeye ve VM serisi düzeyleri var. CPU çekirdekleri kota ' dir. Örneğin, aboneliğinizi 30 çekirdek sınırına bölgenizde ve 30 çekirdek sınırına sahip bir 200 çekirdek toplam sınırı VM örnekleri üzerinde olabilir. Yapabilecekleriniz [kota artışı isteği göndermek üzere desteğe başvurun](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

Ancak, bazı sabit kota sınırları vardır, örneğin tek bir Azure aboneliği en fazla 10.000 çekirdeğe sahip olabilir. Bu sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits#limits-and-the-azure-resource-manager).

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](hdinsight-hadoop-provision-linux-clusters.md): ayarlamak ve hdınsight'ta Hadoop, Spark, Kafka, etkileşimli Hive, HBase, ML Hizmetleri veya Storm kümeleri yapılandırma hakkında bilgi edinin.
* [Küme performansı izleyerek](hdinsight-key-scenarios-to-monitor.md): kümenizin kapasite etkileyebilecek Hdınsight kümeniz için izleme senaryoları hakkında bilgi edinin.
