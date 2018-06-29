---
title: include dosyası
description: include dosyası
services: virtual-machines-linux, virtual-machines-windows
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 05/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 32a438d393077cfe4cb7f6ee62f3a01edfce0571
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34152232"
---
Kuruluşların büyük ölçekli bilgi işlem gereksinimleri vardır. Bu Big Compute iş yükleri, mühendislik tasarımı ve analizi, finansal risk hesaplamaları, görüntü işleme, karmaşık modelleme, Monte Carlo benzetimleri ve daha fazla bilgi içerir. 

Azure bulut verimli bir şekilde işlem yoğunluklu Linux ve Windows iş yükleri, geleneksel HPC benzetimleri paralel toplu işlerini çalıştırmak için kullanın. HPC çalıştırın ve toplu iş yükleri seçiminizi ile Azure altyapı hizmetleri, kılavuz yöneticileri, Market çözümleri ve satıcı barındırılan (SaaS) uygulamaları işlem. Azure iş dağıtmak ve sanal makineleri veya çekirdek binlerce için ölçeklendirme ve daha az kaynak gerektiğinde de ölçeklendirmeyi azaltın için esnek çözümler sağlar. 



## <a name="solution-options"></a>Çözüm seçenekleri



* **Yazılanları çözümleri**
    * Kendi Azure sanal makineleri küme ortamında ayarlama veya [sanal makine ölçek kümeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Yükselt ve şirket içi küme shift veya yeni bir Azure kümede ek kapasite dağıtabilirsiniz. 
    * Önde gelen dağıtmak için Azure Resource Manager şablonlarını kullanma [iş yükü yöneticileri](#workload-managers), altyapı ve [uygulamaları](#hpc-applications). 
    * Seçin [HPC ve GPU VM boyutları](#hpc-and-gpu-sizes) MPI veya GPU iş yükleri için özel donanım ve ağ bağlantılarını içerir. 
    * Ekleme [yüksek performanslı depolama](#hpc-storage) g/Ç kullanımı yoğun iş yükleri için.
* **Karma çözümleri**
    * ("Veri bloğu") yoğun iş yükleri için Azure altyapı boşaltmak için şirket içi çözümünüzü genişletme
    * Bulut bilgi işlem isteğe bağlı, var olan kullanmak [iş yükü Yöneticisi](#workload-manager).
    * Yararlanmak [HPC ve GPU VM boyutları](#hpc-and-gpu-sizes) MPI veya GPU iş yükleri için.
* **Bir hizmet olarak Big Compute çözümleri**
    * Özel Big Compute çözümleri ve iş akışlarını kullanarak geliştirme [Azure Batch](#azure-batch) ve ilgili [Azure Hizmetleri](#related-azure-services).
    * Azure etkin mühendislik ve benzetimi çözümlerin de dahil olmak üzere satıcılardan çalıştırılması [Altair](http://www.altair.com/), [yeniden Ölçeklendir](https://www.rescale.com/azure/), ve [Cycle Computing](https://cyclecomputing.com/) (şimdi [ile birleştirilmiş Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
    * Kullanım bir [Cray Süper bilgisayar](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) Azure üzerinde barındırılan bir hizmet olarak.
* **Market çözümleri**
    * Ölçek kullanmak [HPC uygulamaları](#hpc-applications) ve [çözümleri](#marketplace-solutions) içinde sunulan [Azure Marketi](https://azuremarketplace.microsoft.com/). 
    


Aşağıdaki bölümler destek teknolojiler ve yönergelere bağlantılar hakkında daha fazla bilgi sağlar.



## <a name="marketplace-solutions"></a>Market çözümleri

Ziyaret [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/) Linux ve Windows VM görüntüleri ve HPC için tasarlanmış çözümler. Örneklere şunlar dahildir:

* [RogueWave CentOS tabanlı HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [SUSE Linux Enterprise Server HPC için](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO kılavuz sunucu altyapısı](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Azure veri bilimi Windows ve Linux için VM](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [D3vıew](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)
* [Lustre için Intel bulut Edition](https://azuremarketplace.microsoft.com/marketplace/apps/intel.lustre-cloud-edition-eval?tab=Overview)


 
## <a name="hpc-applications"></a>HPC uygulamaları

Ticari veya özel HPC uygulamaları Azure üzerinde çalışır. Bu bölümdeki birkaç örnekleri, ek VM ile birlikte ölçeklendirilmesine veya çekirdek işlem benchmarked. Ziyaret [Azure Marketi](https://marketplace.azure.com) çözümlerini dağıtmak için hazır.

> [!NOTE]
> Herhangi bir ticari uygulama bulutta çalıştırmak için lisans ya da başka kısıtlamalar için satıcıyla denetleyin. Satıcıların tümü kullandıkça öde lisansı sunmaz. Çözümünüz için bulutta bir lisans sunucusuna ihtiyacınız olabilir veya bir şirket içi lisans sunucusuna bağlanın.

### <a name="engineering-applications"></a>Mühendislik uygulamaları


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB dağıtılmış bilgi işlem sunucusu](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM +](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Grafikler ve işleme

* [Autodesk Maya, 3ds Max ve Arnold](../articles/batch/batch-rendering-service.md) Azure batch 

### <a name="ai-and-deep-learning"></a>AI ve derin öğrenme

* [Toplu AI](../articles/batch-ai/overview.md) derin öğrenme modelleri için eğitim
* [Microsoft Bilişsel Araç Seti](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Derin VM öğrenme](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Toplu Shipyard tarif derin öğrenme için](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)






## <a name="hpc-and-gpu-vm-sizes"></a>HPC ve GPU VM boyutları
Azure'un sunduğu boyutları için bir aralığı [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) yoğun iş yükleri için tasarlanmış boyutları dahil olmak üzere sanal makineleri,. Örneğin, H16r ve H16mr VM'ler yüksek verimlilik arka uç RDMA ağa bağlanabilir. Bu bulut ağ altında çalışan sıkı şekilde bağlı paralel uygulamaların performansı [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) veya Intel MPI. 

N-serisi VM'ler NVIDIA yapay Intelligence (AI) öğrenme ve görselleştirme dahil olmak üzere işlem yoğunluklu veya grafik yoğun uygulamalar için tasarlanmış GPU özelliği. 

Daha fazla bilgi edinin:

* Yüksek performanslı işlem boyutları [Linux](../articles/virtual-machines/linux/sizes-hpc.md) ve [Windows](../articles/virtual-machines/windows/sizes-hpc.md) VM'ler 
* GPU etkin boyutları [Linux](../articles/virtual-machines/linux/sizes-gpu.md) ve [Windows](../articles/virtual-machines/windows/sizes-gpu.md) VM'ler 

Şunları nasıl yapacağınızı öğrenin:

* [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI uygulamaları çalıştırmak için Microsoft HPC Pack ile Windows RDMA kümesi ayarlama](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Batch havuzlarında kullanımı yoğun VM'ler](../articles/batch/batch-pool-compute-intensive-sizes.md)



## <a name="azure-batch"></a>Azure Batch
[Toplu](../articles/batch/batch-technical-overview.md) (HPC) uygulamalarını bulutta verimli bir şekilde bir platform için büyük ölçekli paralel çalıştırmak için hizmet ve yüksek performanslı bilgi işlem. Sanal makineler yönetilen bir havuz çalıştırmak için Azure Batch zamanlamaları işlem yoğunluklu iş ve ölçek işleriniz ihtiyaçlarını karşılamak için kaynakları işlem otomatik olarak. 

SaaS sağlayıcısı veya geliştiriciler, HPC uygulamaları veya kapsayıcı iş yükleri, Azure, aşama verileri Azure ile tümleştirmek için Batch SDK'ları ve Araçları'nı kullanın ve iş yürütme işlem hatları oluşturabilir. 

Şunları nasıl yapacağınızı öğrenin:

* [Batch ile geliştirme kullanmaya başlama](../articles/batch/quick-run-dotnet.md)
* [Azure Batch kod örnekleri kullanın](https://github.com/Azure/azure-batch-samples)
* [Düşük öncelikli sanal makineleri Batch ile kullanma](../articles/batch/batch-low-pri-vms.md)
* [Kapsayıcılı HPC iş yükleri ile toplu Shipyard çalıştırın](https://github.com/Azure/batch-shipyard)
* [Paralel R iş yüklerini toplu olarak çalıştırma](https://github.com/Azure/doAzureParallel)
* [İsteğe bağlı Spark üzerinde toplu işleri çalıştırma](https://github.com/Azure/aztk)

## <a name="workload-managers"></a>İş yükü yöneticileri

Azure altyapısı için çalıştırabilirsiniz küme ve iş yükü yöneticileri örnekleri verilmiştir. Tek başına kümeleri Azure VM'ler veya veri bloğu içinde bir şirket içi kümeden Azure Vm'lerine oluşturun. 
* [Alces uçuş işlem](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Açık Küme Yöneticisi](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM Spektrumun Symphony ve Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx) -çalıştırmak için seçenekleri görmek [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM'ler 



## <a name="hpc-storage"></a>HPC depolama

Veri depolama ve erişim için geleneksel bulut dosya sistemleri özelliklerini aşan taleplerini büyük ölçekli Batch ve HPC iş yükleri vardır. Paralel dosya sistemi Azure'da gibi çözümlerin [Lustre](http://lustre.org/) ve [BeeGFS](http://www.beegfs.com/content/).

Daha fazla bilgi edinin:

* [Azure sanal dosya sistemlerinde paralel](https://azure.microsoft.com/resources/parallel-virtual-file-systems-on-microsoft-azure/)
* Yüksek performanslı bulut depolama çözümleri [Avere](http://www.averesystems.com/about-us/about-avere) (şimdi [Microsoft ile birleştirilmiş](https://blogs.microsoft.com/blog/2018/01/03/microsoft-to-acquire-avere-systems-accelerating-high-performance-computing-innovation-for-media-and-entertainment-industry-and-beyond/))


## <a name="related-azure-services"></a>İlgili Azure Hizmetleri

Azure sanal makineler, sanal makine ölçek kümeleri, toplu ve ilgili işlem Hizmetleri çoğu Azure HPC çözümleri temelidir. Çözümünüzü, ancak birçok ilgili Azure hizmetlerinin avantajından yararlanabilirsiniz. Kısmi bir liste aşağıda verilmiştir:

### <a name="storage"></a>Depolama

* [BLOB, tablo ve kuyruk depolama](../articles/storage/storage-introduction.md)
* [Dosya depolama](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Veri ve analiz
* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md)
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake Store](../articles/data-lake-store/data-lake-store-overview.md)
* [Databricks](../articles/azure-databricks/what-is-azure-databricks.md)
* [SQL Database](../articles/sql-database/sql-database-technical-overview.md)

### <a name="ai-and-machine-learning"></a>AI ve makine öğrenme
* [Machine Learning Hizmetleri](../articles/machine-learning/service/overview-what-is-azure-ml.md)
* [Batch AI](../articles/batch-ai/overview.md)
* [Genomics](../articles/genomics/overview-what-is-genomics.md)

### <a name="networking"></a>Ağ
* [Sanal Ağ](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Kapsayıcılar
* [Kapsayıcı Hizmeti](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Azure Kubernetes hizmeti (AKS)](../articles/aks/intro-kubernetes.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Müşteri hikayeleri

İş sorunlarını Azure HPC çözümleri ile Çözüldü müşteriler örnekleri:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA genel P & C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3vıew](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ senedi uluslararası](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Sonraki adımlar
* Big Compute çözümleri hakkında daha fazla bilgi [benzetimi mühendislik](https://simulation.azure.com/), [işleme](https://azure.microsoft.com/solutions/big-compute/rendering/), [bankacılık ve büyük harf pazarda](https://finance.azure.com/), ve [genomics](https://enterprise.microsoft.com/en-us/industries/health/genomics/) .
* Son duyurular için bkz. [Microsoft HPC ve Batch ekip blogu](http://blogs.technet.com/b/windowshpc/) ve [Azure blogu](https://azure.microsoft.com/blog/tag/hpc/).

* Yönetilen ve ölçeklenebilir Azure kullanmak [toplu](https://azure.microsoft.com/services/batch/) altyapının yönetmek zorunda kalmadan işlem yoğunluklu iş yüklerini çalıştırmak için hizmeti [daha fazla bilgi edinin](https://azure.microsoft.com/solutions/architecture/hpc-big-compute-saas/)



