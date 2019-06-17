---
title: Azure HDInsight üzerinde ML hizmetlerine giriş
description: ML Hizmetleri HDInsight, büyük veri analizi için uygulamalar oluşturmak için kullanmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 06/12/2019
ms.openlocfilehash: d24686a094c524c5ce913eee4b711daf1c60100d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67130629"
---
# <a name="what-is-ml-services-in-azure-hdinsight"></a>Azure HDInsight, ML Hizmetleri nedir

Azure HDInsight kümeleri oluşturduğunuzda Microsoft Machine Learning sunucusu bir dağıtım seçeneği olarak kullanılabilir. Bu seçenek sağlayan küme türü olarak adlandırılır **ML Hizmetleri**. Bu özellik, veri uzmanlarının, istatistikçilerin ve R programcılarının ölçeklenebilir, isteğe bağlı erişim HDInsight üzerinde analitik yöntemlerine dağıtılmış sağlar.

HDInsight üzerinde ML Hizmetleri, Azure Blob veya Data Lake depolama birimine yüklenen, neredeyse her boyuttaki veri kümelerinde R tabanlı analiz için en son özellikleri sağlar. ML Hizmetleri küme üzerinde açık kaynak R ile kurulu olduğundan, oluşturduğunuz R tabanlı uygulama 8000 + açık kaynak R paketlerinin hiçbirini yararlanabilirsiniz. ScaleR çalışmalarında, Microsoft'un büyük veri analizi paketi de mevcuttur.

Bir küme kenar düğümüne kümeye bağlanın ve, R betikleri çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile çalıştırmanın dağıtılmış ScaleR işlevlerini Paralel Kenar düğümü sunucusunun çekirdek arasında seçeneğiniz vardır. Ayrıca bunları tüm küme düğümlerine ScaleR'ın Hadoop Map Reduce veya Apache Spark işlem bağlamlarının kullanarak çalıştırabilirsiniz.

Analiz sonucu Öngörüler ve modeller, şirket içi kullanım için indirilebilir. Bunlar ayrıca başka bir yerde, Azure'da özellikle aracılığıyla kullanıma hazır hale getirdiniz [Azure Machine Learning Studio](https://studio.azureml.net) [web hizmetini](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-ml-services-on-hdinsight"></a>HDInsight üzerinde ML hizmetleri kullanmaya başlayın

Azure HDInsight ML Hizmetleri kümesi oluşturmak için seçin **ML Hizmetleri** Azure portalını kullanarak bir HDInsight kümesi oluştururken küme türü. ML Hizmetleri küme türü, ML Server kümesinin veri düğümlerini ve ML Hizmetleri tabanlı analiz için bir giriş bölge olarak hizmet veren bir kenar düğümü içerir. Bkz: [ML hizmetlerinde, HDInsight ile çalışmaya başlama](r-server-get-started.md) kümesinin nasıl oluşturulacağı hakkında kılavuz.

## <a name="why-choose-ml-services-in-hdinsight"></a>Neden ML Hizmetleri HDInsight seçmeliyim?

HDInsight ML Hizmetleri aşağıdaki avantajları sağlar:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>Microsoft ve açık kaynak yeniliği yapay ZEKA

  ML hizmetleri içeren yüksek oranda ölçeklenebilir, dağıtılmış algoritmaları kümesi gibi [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), ve [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) büyük veri boyutlarına çalışabilir fiziksel bellek ve çok çeşitli platformlarda dağıtılmış bir şekilde çalıştırma boyutu. Özel Microsoft koleksiyonu hakkında daha fazla bilgi edinin [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) ve [Python paketlerini](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) ürünle birlikte gelen.
  
  ML Hizmetleri bu Microsoft yeniliklerini ve tüm kurumsal düzeyde tek bir platform üzerinde açık kaynak topluluğu (R, Python ve yapay ZEKA araç Setleri) geldiğini katkılar arasında köprü. Herhangi bir açık kaynaklı makine öğrenimi R veya Python paket, Microsoft gelen tüm özel yenilik yan yana çalışabilir.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Basit, güvenli ve yüksek ölçekli kullanıma hazır hale getirme ve yönetim

  Geleneksel paradigmalarını ve ortamları bağlı olan kuruluşlar, daha az zaman ve çaba kullanıma hazır hale getirme doğrultusunda yaparlar. Bu sonuçları inflated maliyetlerini ve modelleri, bunları geçerliliğini sürdürmek için yineleme ve geçerli, yasal onay ve kullanıma hazır hale getirme yoluyla izinlerinin yönetilmesi için çeviri zaman dahil olmak üzere geciktirir.

  ML hizmetleri sunan Kurumsal düzeydeki [kullanıma hazır hale getirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)machine learning modeli tamamlandıktan sonra web hizmetleri API'leri oluşturmak için yalnızca birkaç tıklamayla alır. Bu,. Bunlar [web Hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) sunucu kılavuz bulutta barındırılan ve satır iş kolu uygulamaları ile tümleştirilebilir. Bir esnek bir kılavuz sağlar hem de toplu ve gerçek zamanlı Puanlama için kendi iş gereksinimlerinizi ile sorunsuz bir şekilde ölçeklendirmek için dağıtma yeteneği. Yönergeler için [HDInsight üzerinde kullanıma hazır hale getirme ML Hizmetleri](r-server-operationalize.md).

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

## <a name="key-features-of-ml-services-on-hdinsight"></a>HDInsight üzerinde ML Hizmetleri temel özellikleri

Aşağıdaki özellikler HDInsight ML Hizmetleri dahil edilmiştir.

| Özellik kategorisi | Açıklama |
|------------------|-------------|
| R etkin | [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) R içinde bir açık kaynak dağıtımı R betiği yürütme için çalışma zamanı altyapısı ve yazılmış çözümleri için. |
| Python etkin | [Python modüllerini](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) bir Python betiği yürütme için çalışma zamanı altyapısı ve açık kaynak dağıtım ile Python'da yazılmış çözümleri için.
| [Önceden eğitilmiş modeller](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Görsel analiz ve metin duygu analizi için verileri puanlamak hazır size sunar. |
| [Dağıtma ve kullanma](r-server-operationalize.md) | Sunucunuzu çalışır hale getirme ve çözümleri bir web hizmeti olarak dağıtalım. |
| [Uzaktan yürütme](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | Ağınızdaki istemci istasyonunuzdan ML Hizmetleri kümesinde uzak oturum başlatın. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>HDInsight üzerinde ML Hizmetleri için veri depolama seçenekleri

HDInsight kümelerinin HDFS dosya sistemi için varsayılan depolama alanı, bir Azure depolama hesabı veya bir Azure Data Lake Storage ile ilişkilendirilebilir. Bu ilişki verileri karşıya yüklendiğini sağlar kümeye depolama analizi sırasında kalıcı hale ve hatta kümesi silindikten sonra veriler sunulur. Veri aktarımı, depolama hesabının portal tabanlı karşıya yükleme olanağı dahil olmak üzere seçtiğiniz depolama seçeneğine işlemeye yönelik çeşitli araçlar vardır ve [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programı.

Ek Blob erişimini etkinleştirme seçeneğiniz vardır ve küme kullanımı için birincil depolama seçeneğinde bağımsız olarak işlem sağlama sırasında veri gölü depolar. Bkz: [ML Hizmetleri HDInsight kullanmaya başlama](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) ek hesap için erişim ekleme hakkında bilgi için. Bkz: [ML Hizmetleri için Azure depolama seçeneğinin HDInsight üzerinde](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) makalenin birden fazla depolama hesabı kullanma hakkında daha fazla bilgi edinin.

Ayrıca [Azure dosyaları](../../storage/files/storage-how-to-use-files-linux.md) uç düğümde kullanmak için bir depolama seçeneği olarak. Azure dosyaları, Azure Depolama'da Linux dosya sistemine oluşturulmuş bir dosya paylaşımını bağlama olanak sağlar. HDInsight kümesi üzerinde ML Hizmetleri için bu veri depolama seçenekleri hakkında daha fazla bilgi için bkz. [ML Hizmetleri için Azure depolama seçeneğinin HDInsight üzerinde](r-server-storage.md).

## <a name="access-ml-services-edge-node"></a>ML Hizmetleri kenar düğümüne erişim

Microsoft ML Server için bir tarayıcı kullanarak kenar düğümüne bağlanabilirsiniz. Küme oluşturma sırasında varsayılan olarak yüklenir. Daha fazla bilgi için [HDInsight üzerinde ML Hizmetleri ile stared](r-server-get-started.md). R konsolunu erişmek için SSH/PuTTY kullanarak komut satırından küme kenar düğümüne da bağlanabilirsiniz.

## <a name="develop-and-run-r-scripts"></a>Geliştirme ve R betikleri çalıştırma

R betikleri oluşturma ve çalıştırma 8000 + açık kaynak R paketlerine ek paralel ve dağıtılmış yordamların birini ScaleR Kitaplığı'nda kullanabilirsiniz. Genel olarak, ML Hizmetleri ile kenar düğümünde çalışan bir betik içinde İnterpret R, düğüm üzerinde çalışır. Özel durum ile bir işlem bağlamı olarak ayarlanmış bir ScaleR işlevi çağırmak için gereken bu adımlarla Hadoop (RxHadoopMR) Map Reduce veya Spark (dizüstü). Bu durumda, işlev kümenin başvurulan verilerle ilişkili verileri (görev) düğümleri arasında dağıtılmış bir biçimde çalışır. Farklı işlem bağlamı seçenekleri hakkında daha fazla bilgi için bkz: [için işlem bağlamı seçenekleri ML Hizmetleri HDInsight üzerinde](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme

Veri modellemesi tamamlandığında, Azure veya şirket içi yeni veriler için Öngörüler bulunmak üzere modelinizi kullanıma hazır hale getirebilirsiniz. Bu işlem, Puanlama olarak bilinir. Puanlama HDInsight, Azure Machine Learning veya şirket içi yapılabilir.

### <a name="score-in-hdinsight"></a>HDInsight, puanı

HDInsight içinde puanlamak için depolama hesabınıza yüklediğiniz yeni bir veri dosyası için tahminlerde bulunmak üzere modelinizi çağıran bir R işlev yazın. Ardından, Öngörüler, depolama hesabına geri kaydedin. Bu yordam isteğe bağlı kenar düğümündeki kümenizin veya zamanlanmış bir iş kullanarak çalıştırabilirsiniz.

### <a name="score-in-azure-machine-learning-aml"></a>Puan Azure Machine Learning (AML)

Azure Machine Learning kullanarak puanlamak için olarak bilinen açık kaynaklı Azure Machine Learning R paketi kullanın. [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) modelinizi bir Azure web hizmeti olarak yayımlama. Kolaylık olması için bu paketi kenar düğümüne önceden büyük/küçük harf yüklenir. Ardından, özellikleri, Azure Machine Learning web hizmeti için kullanıcı arabirimi oluşturmak için kullanın ve ardından Puanlama için gerektiği gibi web hizmetini çağırın.

Bu seçeneği belirlerseniz, web hizmeti ile kullanmak için açık kaynak modeli eşdeğer nesnelere ScaleR model nesneleri dönüştürmeniz gerekir. ScaleR zorlama işlevleri gibi kullanın `as.randomForest()` bu dönüştürme için topluluğu tabanlı modeller için.

### <a name="score-on-premises"></a>Puan şirket içi

Modelinizi oluşturduktan sonra şirket puanlamak için R modelinde seri hale getirmek, indirdiği seri durumdan çıkarılamıyor ve yeni veri puanlamasında kullanmak. Daha önce açıklanan yaklaşımı kullanarak yeni verileri puanlamak [puanı HDInsight içinde](#score-in-hdinsight) kullanarak veya [web Hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services).

## <a name="maintain-the-cluster"></a>Küme koru

### <a name="install-and-maintain-r-packages"></a>Yüklemek ve bakımını R paketleri

Kullandığınız R paketleri çoğunu kenar düğümüne beri var. çalıştırma R betikleriniz çoğu adımlar gerekir. Kenar düğümüne ek R paketleri yüklemek için kullanabileceğiniz `install.packages()` r'deki yöntemi

Yalnızca yordamları ScaleR Kitaplığı'ndan küme üzerinde kullanıyorsanız, genellikle veri düğümleri ek R paketleri yüklemeniz gerekmez. Ancak, kullanımını desteklemek için ek paketleri gerekebilir **rxExec** veya **RxDataStep** veri düğümleri üzerinde yürütme.

Bu gibi durumlarda, kümeyi oluşturduktan sonra ek paketleri ile betik eylemi yüklenebilir. Daha fazla bilgi için [ML Hizmetleri'nin HDInsight kümesinde](r-server-hdinsight-manage.md).

### <a name="change-apache-hadoop-mapreduce-memory-settings"></a>Apache Hadoop MapReduce bellek ayarlarını değiştirme

Bir küme bir MapReduce işi çalıştırılırken ML Hizmetleri için kullanılabilir bellek miktarını değiştirmek için değiştirilebilir. Bir küme değiştirmek için kümenizin Azure portalı dikey penceresi aracılığıyla kullanılabilir olan Apache Ambari UI'ı kullanın. Kümeniz için Ambari UI erişme hakkında yönergeler için bkz: [yönetme HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak](../hdinsight-hadoop-manage-ambari.md).

ML Hizmetleri çağrısında Hadoop anahtarlar kullanılarak kullanılabilir bellek miktarını değiştirmek mümkündür **RxHadoopMR** gibi:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Kümenizi ölçeklendirme

Var olan ML Hizmetleri bir HDInsight kümesinde portal üzerinden yukarı veya aşağı ölçeklendirilebilir. Büyütme, daha büyük işlem görevleri için ihtiyacınız olabilecek ek kapasite elde edebilir veya boşta olduğunda, bir küme geri ölçeklendirebilirsiniz. Bir kümenin ölçeğini konusunda yönergeler için bkz: [yönetme HDInsight kümeleri](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Sistem Bakımı

Mesai saatleri dışında bir HDInsight kümesinde temel alınan Linux vm'lerde işletim sistemi düzeltme ekleri ve diğer güncelleştirmeleri uygulamak için bakım gerçekleştirilir. Genellikle, bakım 03:30 da (sanal makine için yerel saate göre) her Pazartesi ve Salı gerçekleştirilir. Güncelleştirmeleri, bir küme çeyreği birer birer etkisi olmayan şekilde gerçekleştirilir.

Baş düğümler gereksizdir ve tüm veri düğümleri etkilendiğini olduğundan, bu süre boyunca çalışmakta olan herhangi bir işi yavaşlayabilir. Ancak, bunlar yine de tamamlanana kadar çalışması gerekir. Bir küme yeniden oluşturma gerektiren geri dönülemez bir hata gerçekleşmediği sürece bu bakım olayları arasında herhangi bir özel yazılım veya sahip olduğunuz yerel veri korunur.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>HDInsight üzerinde ML Hizmetleri için IDE seçenekleri

Linux kenar düğümüne bir HDInsight kümesinin R tabanlı analiz için giriş bölgedir. HDInsight'ın yeni sürümlerinin, tarayıcı tabanlı bir IDE kenar düğümünde RStudio Server'ın varsayılan yüklemede sağlar. Geliştirme için bir IDE olarak RStudio Server kullanımını ve R betiklerinin yürütülmesi R Konsolu kullanmaya kıyasla önemli ölçüde daha verimli olabilir.

Ayrıca, bir masaüstü IDE yükleyin ve bir uzak MapReduce veya Spark işlem bağlamını kullanarak kümeye erişmek için kullanın. Microsoft'un seçenekleriniz [Visual Studio için R Araçları](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) RStudio ve Walware Eclipse tabanlı [StatET](http://www.walware.de/goto/statet).

Ek olarak, R konsolunu kenar düğümündeki yazarak erişebileceğiniz **R** , SSH veya PuTTY bağlandıktan sonra Linux komut isteminde. Konsolu arabirimini kullanırken, bir metin düzenleyicisi R betiği geliştirmeye yönelik başka bir penceresinde çalıştırın ve kesip kodunuzu bölümlerini gerektiğinde R konsolunda uygundur.

## <a name="pricing"></a>Fiyatlandırma

Bir ML Hizmetleri HDInsight kümesi ile ilişkili fiyatları, diğer HDInsight küme türleri için fiyatlara benzer şekilde yapılandırılmıştır. Adı, verileri ve çekirdek saati artışlarıyla eklenmesiyle kenar düğümleri, temel alınan sanal makinelerin boyutunu temel alır. Daha fazla bilgi için [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Sonraki adımlar

HDInsight kümelerinde ML Hizmetleri kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [HDInsight kümesinde ML hizmetleri kullanmaya başlayın](r-server-get-started.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight kümesinde ML Hizmetleri için Depolama Seçenekleri](r-server-storage.md)
