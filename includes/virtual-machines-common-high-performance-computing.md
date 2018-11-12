---
title: include dosyası
description: include dosyası
services: virtual-machines-linux, virtual-machines-windows
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 07/02/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 8f43edfe468958bbc4a6fde14e8e03e5b4d4e0f2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264055"
---
Kuruluşların büyük ölçekli bilgi işlem gereksinimlerini vardır. Bu Big Compute iş yükleri, mühendislik tasarımı ve analizi, finansal risk hesaplamaları, görüntü işleme, karmaşık modelleme, Monte Carlo simülasyonları ve daha fazlasını içerir. 

Azure Bulutu, verimli bir şekilde paralel toplu işlemler için geleneksel HPC simülasyonlarını yoğun işlem gücü kullanımlı Linux ve Windows iş yüklerini çalıştırmak için kullanın. Çalıştırın, HPC ve batch iş yükü ile tercih ettiğiniz Azure altyapı hizmetleri, kılavuz yöneticileri, Market çözümlerini ve (SaaS) uygulamaları satıcı tarafından barındırılan işlem. Azure VM veya çekirdekler binlerce ölçeklendirilecek iş dağıtmak ve daha az kaynak gerektiğinde de ölçeklendirmeyi azaltın esnek çözümler sunar. 



## <a name="solution-options"></a>Çözüm seçenekleri



* **Yazılanları çözümleri**
    * Kendi Azure sanal makineleri küme ortamında ayarlama veya [sanal makine ölçek kümeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Lift ve bir şirket içi kümesini kaydırma veya ek kapasite için azure'da yeni bir kümeye dağıtma. 
    * Önde gelen dağıtmak için Azure Resource Manager şablonlarını kullanma [iş yükü yöneticileri](#workload-managers), altyapı ve [uygulamaları](#hpc-applications). 
    * Seçin [HPC ve GPU VM boyutları](#hpc-and-gpu-sizes) MPI veya GPU iş yükleri için özel donanım ve ağ bağlantılarını içerir. 
    * Ekleme [yüksek performanslı depolama](#hpc-storage) g/Ç açısından yoğun iş yükleri için.
* **Hibrit çözümler**
    * ("Patlama") yoğun iş yüklerini Azure altyapısına yük boşaltması için şirket içi çözümünüzü genişletin
    * Bulut bilgi işlem isteğe bağlı, var olan kullanın [iş yükü Yöneticisi](#workload-manager).
    * Yararlanmak [HPC ve GPU VM boyutları](#hpc-and-gpu-sizes) MPI veya GPU iş yükleri için.
* **Hizmet olarak Big Compute çözümleri**
    * Özel Big Compute çözümleri ve iş akışlarını kullanarak geliştirme [Azure Batch](#azure-batch) ve ilgili [Azure Hizmetleri](#related-azure-services).
    * Azure mühendislik ve simülasyon çözümler dahil olmak üzere satıcılardan çalıştırma [Altair](http://www.altair.com/), [yeniden ölçeklendirmek](https://www.rescale.com/azure/), ve [Cycle Computing](https://cyclecomputing.com/) (artık [katılabileceği Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
    * Kullanım bir [Cray Süper bilgisayarı](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) Azure'da barındırılan bir hizmet olarak.
* **Market çözümleri**
    * Ölçek kullanmak [HPC uygulamaları](#hpc-applications) ve [çözümleri](#marketplace-solutions) sunulan [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/). 
    


Aşağıdaki bölümler destek teknolojileri ve yönergeler için bağlantılar hakkında daha fazla bilgi sağlar.



## <a name="marketplace-solutions"></a>Market çözümleri

Ziyaret [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/) Linux ve Windows VM görüntüleri ve çözümler HPC için tasarlanmıştır. Örneklere şunlar dahildir:

* [CentOS tabanlı RogueWave HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [HPC için SUSE Linux Enterprise Server](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO kılavuz sunucu altyapısı](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Azure veri bilimi sanal makinesi için Windows ve Linux](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [d3vıew](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)
* [Intel Cloud Edition Lustre için](https://azuremarketplace.microsoft.com/marketplace/apps/intel.intel-cloud-edition-gs)


 
## <a name="hpc-applications"></a>HPC uygulamaları

Özel veya ticari HPC uygulamalarını Azure'da çalıştırın. Bu bölümde, çeşitli örnekleri, ek Vm'lerle verimli bir şekilde ölçeklendirme veya işlem çekirdek benchmarked. Ziyaret [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace) çözümlerini dağıtmak için hazır.

> [!NOTE]
> Birlikte ticari uygulamaları bulutta çalıştırmak için lisans ya da başka kısıtlamalar için satıcıyla denetleyin. Satıcıların tümü kullandıkça öde lisansı sunmaz. Çözümünüz için bulutta bir lisans sunucusuna ihtiyacınız veya bir şirket içi lisans sunucusuna bağlanın.

### <a name="engineering-applications"></a>Mühendislik Uygulama


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB dağıtılmış bilgi işlem sunucu](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM +](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Grafik ve oluşturma

* [Autodesk Maya, 3ds Max ve Arnold](../articles/batch/batch-rendering-service.md) Azure batch 

### <a name="ai-and-deep-learning"></a>Yapay ZEKA ve derin öğrenme

* [Batch AI](../articles/batch-ai/overview.md) ayrıntılı öğrenme modelleri için eğitim
* [Microsoft Bilişsel Araç Seti](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Derin öğrenme](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Derin öğrenme için Batch Shipyard tarifleri](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)






## <a name="hpc-and-gpu-vm-sizes"></a>HPC ve GPU VM boyutları
Azure, bir dizi boyutları sunar [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) VM'ler, yoğun işlem gücü kullanımlı iş yükleri için tasarlandı boyutları da dahil olmak üzere. Örneğin, H16r ve H16mr VM'ler yüksek aktarım hızı arka uç RDMA ağa bağlanabilir. Bu bulut ağı altında çalışan sıkı bağlı paralel uygulamalar performansını [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) veya Intel MPI. 

N serisi sanal makineler, NVIDIA GPU'ları öğrenme yapay zeka (AI) ve görselleştirme gibi yoğun işlem gücü kullanımlı veya grafik kullanımı yoğun uygulamalar için tasarlanmış özellik. 

Daha fazla bilgi edinin:

* Yüksek performanslı işlem boyutları [Linux](../articles/virtual-machines/linux/sizes-hpc.md) ve [Windows](../articles/virtual-machines/windows/sizes-hpc.md) VM'ler 
* GPU özellikli boyutları [Linux](../articles/virtual-machines/linux/sizes-gpu.md) ve [Windows](../articles/virtual-machines/windows/sizes-gpu.md) VM'ler 

Şunları nasıl yapacağınızı öğrenin:

* [MPI uygulamalarını çalıştırmak için bir Linux RDMA kümesi ayarlama](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI uygulamalarını çalıştırmak için Microsoft HPC Pack ile Windows RDMA kümesi ayarlama](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Batch havuzları, yoğun işlem gücü kullanımlı VM'ler kullanma](../articles/batch/batch-pool-compute-intensive-sizes.md)



## <a name="azure-batch"></a>Azure Batch
[Batch](../articles/batch/batch-technical-overview.md) (HPC) uygulamalarını bulutta verimli bir şekilde bir platform hizmetini çalıştırmak büyük ölçekli paralel ve yüksek performanslı bilgi işlem. Yönetilen bir sanal makine havuzu üzerinde çalıştırmak için Azure Batch zamanlamaları yoğun işlem gücü kullanımlı iş ve ölçek işlerinizi ihtiyaçlarını karşılamak için kaynakları işlem otomatik olarak. 

SaaS sağlayıcıları veya geliştiriciler, Azure, Azure, veri hazırlamak HPC uygulamaları veya kapsayıcı iş yüklerinin tümleştirmek için Batch SDK'ları ve Araçları'nı kullanın ve iş yürütme komut zincirleri oluşturmak. 

Şunları nasıl yapacağınızı öğrenin:

* [Batch ile geliştirmeye başlayın](../articles/batch/quick-run-dotnet.md)
* [Azure Batch kod örnekleri kullanın](https://github.com/Azure/azure-batch-samples)
* [Batch ile düşük öncelikli VM'ler kullanma](../articles/batch/batch-low-pri-vms.md)
* [Batch Shipyard ile kapsayıcılı HPC iş yüklerini çalıştırın](https://github.com/Azure/batch-shipyard)
* [Batch'te paralel R iş yüklerini çalıştırın](https://github.com/Azure/doAzureParallel)
* [İsteğe bağlı Spark üzerinde toplu işleri çalıştırma](https://github.com/Azure/aztk)

## <a name="workload-managers"></a>İş yükü yöneticileri

Azure altyapısı çalışabilen küme ve iş yükü yöneticileri örnekleri aşağıda verilmiştir. Tek başına kümeler Azure Vm'leri veya aşırı bir şirket içi kümesinden Azure Vm'lerine oluşturun. 
* [Alces uçuş işlem](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Parlak Küme Yöneticisi](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM Spektrumun Symphony ve Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx) -çalıştırmak için seçenekleri görmek [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM'ler 



## <a name="hpc-storage"></a>HPC depolama

Büyük ölçekli Batch ve HPC iş yükleri veri depolama ve erişim özellikleri geleneksel bulut dosya sistemlerinin aşan talepleri sahiptir. Paralel dosya sistemi çözümleri gibi Azure'da uygulama [Lustre](http://lustre.org/) ve [BeeGFS](http://www.beegfs.com/content/).

Daha fazla bilgi edinin:

* [Sanal dosya sistemi Azure üzerinde paralel](https://azure.microsoft.com/resources/parallel-virtual-file-systems-on-microsoft-azure/)
* Yüksek performanslı bulut depolama çözümleri [Avere](http://www.averesystems.com/about-us/about-avere) (artık [Microsoft ile birleşik](https://blogs.microsoft.com/blog/2018/01/03/microsoft-to-acquire-avere-systems-accelerating-high-performance-computing-innovation-for-media-and-entertainment-industry-and-beyond/))


## <a name="related-azure-services"></a>İlgili Azure Hizmetleri

Azure sanal makineler, sanal makine ölçek kümeleri, Batch ve ilgili bilgi işlem Hizmetleri çoğu Azure HPC çözümleri temelidir. Ancak, çözümünüzü birçok ilgili Azure Hizmetleri yararlanabilirsiniz. Kısmi bir listesine buradan ulaşabilirsiniz:

### <a name="storage"></a>Depolama

* [BLOB, tablo ve kuyruk depolama](../articles/storage/storage-introduction.md)
* [Dosya depolama](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Veri ve analiz
* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md)
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake depolama Gen1](../articles/data-lake-store/data-lake-store-overview.md)
* [Databricks](../articles/azure-databricks/what-is-azure-databricks.md)
* [SQL Veritabanı](../articles/sql-database/sql-database-technical-overview.md)

### <a name="ai-and-machine-learning"></a>Yapay zeka ve makine öğrenmesi
* [Machine Learning Hizmetleri](../articles/machine-learning/service/overview-what-is-azure-ml.md)
* [Batch AI](../articles/batch-ai/overview.md)
* [Genomiks](../articles/genomics/overview-what-is-genomics.md)

### <a name="networking"></a>Ağ
* [Sanal Ağ](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Kapsayıcılar
* [Kapsayıcı Hizmeti](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Azure Kubernetes Service'i (AKS)](../articles/aks/intro-kubernetes.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Müşteri hikayeleri

Azure HPC çözümleri ile iş sorunlarını tartışıyoruz müşteriler örnekleri:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA genel P & C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3vıew](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [Microsoft Research'ün](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ menkul kıymetler International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [NeuroInitiative](https://customers.microsoft.com/en-us/story/neuroinitiative-health-provider-azure)
* [Schlumberger](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Sonraki adımlar
* Big Compute çözümleri hakkında daha fazla bilgi [mühendislik simülasyonu](https://simulation.azure.com/), [işleme](https://azure.microsoft.com/solutions/big-compute/rendering/), [bankacılık ve sermaye piyasaları](https://finance.azure.com/), ve [genomiks](https://enterprise.microsoft.com/en-us/industries/health/genomics/) .
* Son duyurular için bkz. [Microsoft HPC ve Batch ekip blogu](http://blogs.technet.com/b/windowshpc/) ve [Azure blogu](https://azure.microsoft.com/blog/tag/hpc/).

* Yönetilen ve ölçeklenebilir Azure kullanın [Batch](https://azure.microsoft.com/services/batch/) yoğun işlem gücü kullanımlı iş yükleri, temel alınan altyapı yönetimine gerek kalmadan çalıştırmak için hizmet [daha fazla bilgi edinin](https://azure.microsoft.com/solutions/architecture/hpc-big-compute-saas/)



