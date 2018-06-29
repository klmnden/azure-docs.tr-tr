---
title: Azure hdınsight'ta ML hizmetlerine giriş | Microsoft Docs
description: ML Hizmetleri Hdınsight'ta büyük veri analizi için uygulamalar oluşturmak için nasıl kullanılacağını öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 9633a7df1cb72f3e9e5ee79be0c332565e7e8f2a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054111"
---
# <a name="introduction-to-ml-services-and-open-source-r-capabilities-on-hdinsight"></a>ML Hizmetleri ve açık kaynaklı R yetenekleri hdınsight'ta giriş

> [!NOTE]
> Eylül 2017 ' altında yeni adı Microsoft R Server yayımlanan **Microsoft Machine Learning sunucusu** veya ML sunucu. Sonuç olarak, hdınsight'ta R Server küme şimdi adlı **Machine Learning Hizmetleri** veya **ML Hizmetleri** Hdınsight kümesinde. R Server adı değişikliği hakkında daha fazla bilgi için bkz: [Microsoft R Server şu an Microsoft Machine Learning sunucusu](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server#get-support-for-r-server).

Azure Hdınsight kümeleri oluşturduğunuzda Microsoft Machine Learning sunucusu bir dağıtım seçeneği olarak kullanılabilir. Bu seçenek sağlar küme türü adlı **ML Hizmetleri**. Bu özellik, veri bilimcileri, istatistikçiler ve ölçeklenebilir, isteğe bağlı erişimi olan R programcıları hdınsight'ta analytics yöntemlerinin dağıtılmış sağlar.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

ML Hizmetleri hdınsight'ta R tabanlı analiz için en yeni özelliklere Azure Blob veya Data Lake depolama birimine yüklenen neredeyse herhangi bir boyuttaki veri kümeleri sağlar. ML Hizmetleri küme açık kaynak R kurulu olduğundan, yapı R tabanlı uygulamalar 8000 + açık kaynak R paketlerinden birini yararlanabilirsiniz. ScaleR çalışmalarında, Microsoft'un büyük veri analizi paketi de kullanılabilir.

Kümenin kenar düğümü kümeye bağlanın ve R komut dosyalarını çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile ScaleR parallelized dağıtılmış işlevleri kenar düğümü sunucu Çekirdeğinde çalıştırırken seçeneğiniz vardır. Ayrıca bunları küme düğümleri arasında ScaleR'ın eşlemesi Hadoop azaltmak kullanarak çalıştırabilirsiniz veya Spark işlem bağlamı.

Şirket içi kullanım için modelleri veya çözümlemesinden neden tahminleri indirilebilir. Bunlar ayrıca başka bir yerde Azure'da, özellikle aracılığıyla operationalized [Azure Machine Learning Studio](http://studio.azureml.net) [web hizmeti](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-ml-services-on-hdinsight"></a>Hdınsight üzerinde ML hizmetleri kullanmaya başlama

Azure Hdınsight'ta bir ML Hizmetleri kümesi oluşturmak için seçin **ML Hizmetleri** Azure portalını kullanarak bir Hdınsight kümesi oluştururken küme türü. ML Hizmetleri küme türü ML sunucu kümesinin veri düğümlerini ve ML Hizmetleri tabanlı analiz için giriş bölgesi görevi gören bir edge düğümünü içerir. Bkz: [hdınsight'ta ML Hizmetleri ile çalışmaya başlama](r-server-get-started.md) küme oluşturma konusunda kılavuz.

## <a name="why-choose-ml-services-in-hdinsight"></a>Neden ML Hizmetleri Hdınsight'ta nasıl seçecekler?

ML Hizmetleri hdınsight'ta aşağıdaki avantajları sağlar:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>AI yenilik Microsoft ve açık kaynak

  ML hizmetleri içerir algoritmaları düzeyde ölçeklenebilir, dağıtılmış kümesi gibi [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), ve [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) daha büyük veri boyutları üzerinde çalışabilir fiziksel bellek ve çok çeşitli platformlarda dağıtılmış bir şekilde çalıştırılmasında boyutu. Özel Microsoft Topluluğu hakkında daha fazla bilgi edinin [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) ve [Python paketlerini](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) ürünle birlikte gelen.
  
  ML hizmetleri arasında köprü bu Microsoft yenilikleri ve tüm tek Kurumsal düzeyde platform en üstünde açık kaynak topluluk (R, Python ve AI araç takımları)'ten gelen Katkıları. Herhangi bir açık kaynak makine öğrenme R veya Python paket Microsoft'un mülkiyet herhangi yenilik yan yana çalışabilir.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Basit, güvenli ve büyük ölçekli operationalization ve yönetim

  Geleneksel örneklerinde ve ortamları bağlı olan kuruluşlar, çok zaman ve çaba operationalization doğrultusunda yatırımları yaparlar. Bu inflated maliyetlerini sonuçlanır ve modeller, geçerli tutmak için yineleme ve geçerli, yasal onay ve izinleri operationalization aracılığıyla yönetmek için çeviri zaman dahil olmak üzere geciktirir.

  ML hizmetleri sunan Kurumsal düzeyde [operationalization](https://docs.microsoft.com/machine-learning-server/what-is-operationalization), machine learning modelini tamamlandıktan sonra web hizmetleri API'ları oluşturmak için yalnızca birkaç tıklama sürdüğünü içeren. Bunlar [web Hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) sunucu kılavuz bulutta barındırılan ve iş kolu satır uygulamaları ile tümleştirilebilir. Hem batch ve gerçek zamanlı Puanlama için kendi iş gereksinimlerinizi ile sorunsuz ölçeklendirme esnek kılavuz sağlar dağıtma yeteneği. Yönergeler için bkz: [faaliyete ML Hizmetleri hdınsight'ta](r-server-operationalize.md).

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

## <a name="key-features-of-ml-services-on-hdinsight"></a>Hdınsight üzerinde ML Hizmetleri temel özellikleri

Aşağıdaki özellikler hdınsight'ta ML Hizmetleri dahil edilmiştir.

| Özellik kategorisi | Açıklama |
|------------------|-------------|
| R etkin | [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) R, R ve çalışma zamanı altyapısı komut dosyasının yürütülmesi için açık kaynak dağıtılması ile yazılmış çözümleri için. |
| Python etkin | [Python modülleri](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) Python ve çalışma zamanı altyapısı komut dosyasının yürütülmesi için açık kaynak dağıtılması ile Python içinde yazılmış çözümleri için.
| [Önceden eğitilmiş modeller](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Görsel çözümleme ve metin düşünceleri çözümlemesi için veri puan hazır, sağlar. |
| [Dağıtma ve kullanma](r-server-operationalize.md) | Sunucunuz faaliyete ve çözümleri bir web hizmeti olarak dağıtabilirsiniz. |
| [Uzaktan yürütme](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | İstemci iş istasyonundan, ağınızdaki ML Hizmetleri kümede Uzak Oturumlar başlatın. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>Hdınsight üzerinde ML Hizmetleri için veri depolama seçenekleri

Hdınsight kümeleri HDFS dosya sistemi için varsayılan depolama bir Azure Storage hesabı veya bir Azure Data Lake Store ile ilişkili olabilir. Bu ilişkilendirme hangi verilerin karşıya yüklendiğini sağlar kümeye depolama Çözümleme sırasında kalıcı yapılan ve hatta kümesi silindikten sonra veriler kullanılabilir. Veri aktarımı, depolama hesabının portal tabanlı karşıya yükleme olanağı dahil olmak üzere seçtiğiniz depolama seçeneğine işlemek için çeşitli araçlar vardır ve [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programı.

Ek Blob erişimi etkinleştirme seçeneğiniz vardır ve sağlama işlemi bağımsız kullanılan birincil depolama seçeneği olarak küme sırasında Data lake depolar. Bkz: [hdınsight'ta ML Hizmetleri ile çalışmaya başlama](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) erişim için ek hesapları ekleme hakkında bilgi için. Bkz: [hdınsight'ta ML Hizmetleri için Azure Depolama Seçenekleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) birden çok depolama hesabı kullanma hakkında daha fazla bilgi için makalenin.

Aynı zamanda [Azure dosyaları](../../storage/files/storage-how-to-use-files-linux.md) kenar düğümünü kullanmak için bir depolama seçeneği olarak. Azure dosyaları Azure Storage'da Linux dosya sistemine oluşturulmuş bir dosya paylaşımını bağlama olanak sağlar. Hdınsight kümesi ML Hizmetleri için bu veri depolama seçenekleri hakkında daha fazla bilgi için bkz: [hdınsight'ta ML Hizmetleri için Azure Depolama Seçenekleri](r-server-storage.md).

## <a name="access-ml-services-edge-node"></a>Erişim ML Hizmetleri kenar düğümüne

Microsoft ML Server'a bir tarayıcı kullanarak kenar düğümüne bağlanabilirsiniz. Küme oluşturma sırasında varsayılan olarak yüklenir. Daha fazla bilgi için bkz: [hdınsight'ta ML hizmetleriyle stared](r-server-get-started.md). Ayrıca, küme kenar düğümüne R konsoluna erişmek için SSH/PuTTY kullanarak komut satırından bağlanabilir.

## <a name="develop-and-run-r-scripts"></a>Geliştirme ve R komut dosyalarını çalıştır

Oluşturma ve çalıştırma R betiklerini 8000 + açık kaynak R paketleri kullanılabilir paralel birkaç ölçeklendirin ve dağıtılmış yordamlarını yanı sıra birini ScaleR Kitaplığı'nda kullanabilirsiniz. Genel olarak, ML hizmetleriyle kenar düğümü üzerinde çalışacak bir betik içinde R yorumlayıcı bu düğüm üzerinde çalışır. Ayarlanmış bir işlem bağlamına sahip ScaleR işlevi çağırmak için gereken adımları istisnaları Hadoop harita azaltmak (RxHadoopMR) veya Spark (RxSpark). Bu durumda, işlevi başvurulan veri ile ilişkili olan bu verileri (görev) kümenin düğümleri arasında dağıtılmış bir şekilde çalışır. Farklı işlem bağlamı seçenekleri hakkında daha fazla bilgi için bkz: [işlem hdınsight'ta ML Hizmetleri için içerik seçeneklerini](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme

Veri modellemesi tamamlandığında modeli yeni verilere Azure veya şirket içi tahminlerde faaliyete geçirebilirsiniz. Bu işlem Puanlama olarak bilinir. Puanlama Hdınsight, Azure Machine Learning veya şirket içi yapılabilir.

### <a name="score-in-hdinsight"></a>Hdınsight'ta puan

Hdınsight'ta Puanlama amacıyla depolama hesabınıza yüklediğiniz yeni bir veri dosyası için tahminlerde modelinizi çağıran bir R işlevi yazma. Ardından, depolama hesabına tahminleri kaydedin. Bu yordamı isteğe bağlı kenar düğümüne kümenizin veya zamanlanmış bir iş kullanarak çalıştırabilirsiniz.

### <a name="score-in-azure-machine-learning-aml"></a>Puan azure'da makine öğrenme (AML)

Azure Machine Learning kullanarak Puanlama amacıyla olarak bilinen açık kaynaklı Azure Machine Learning R paketi kullanmak [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) modelinizi bir Azure web hizmeti olarak yayımlamak için. Kolaylık olması için bu paket edge düğüm üzerinde önceden büyük/küçük harf yüklenir. Ardından, web hizmeti için kullanıcı arabirimi oluşturmak üzere Azure Machine Learning tesislerde kullanın ve ardından Puanlama için gerektiği gibi web hizmeti çağrısı.

Bu seçeneği seçerseniz, web hizmeti ile kullanılmak üzere eşdeğeri açık kaynak modeli nesnelere herhangi ScaleR model nesneleri dönüştürmeniz gerekir. ScaleR zorlama işlevleri gibi kullandığınız `as.randomForest()` bu dönüştürme için ensemble tabanlı modelleri için.

### <a name="score-on-premises"></a>Puan şirket içi

Modelinizi oluşturduktan sonra şirket içi Puanlama amacıyla R modelinde seri, indirir seri durumdan ve yeni veri Puanlama için kullanmak. Daha önce açıklanan yaklaşımı kullanarak, yeni verilerinizi puanlamada [Hdınsight'ta Puanlama](#scoring-in-hdinsight) veya kullanarak [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Küme koru

### <a name="install-and-maintain-r-packages"></a>Yükleme ve R paketleri koru

Kullandığınız R paketleri çoğunu kenar düğümüne var. çalıştırmak, R betiklerini çoğu adımları bu yana gereklidir. Kenar düğümüne ek R paketlerini yüklemek için kullanabileceğiniz `install.packages()` r yöntemi

Yalnızca yordamları ScaleR Kitaplığı'ndan küme üzerinde kullanıyorsanız, genellikle veri düğümlerinde ek R paketlerini yüklemek gerekmez. Ancak, kullanımını desteklemek için ek paketler gerekebilir **rxExec** veya **RxDataStep** veri düğümlerde yürütme.

Kümeyi oluşturduktan sonra bu gibi durumlarda, bir komut dosyası eylemi ek paketler yüklenebilir. Daha fazla bilgi için bkz: [Hdınsight kümesinde ML Services'ı Yönet](r-server-hdinsight-manage.md).

### <a name="change-hadoop-mapreduce-memory-settings"></a>Hadoop MapReduce bellek ayarlarını değiştirme

Bir küme bir MapReduce işi çalıştırılırken ML Hizmetleri için kullanılabilir olan bellek miktarını değiştirmek için değiştirilebilir. Bir küme değiştirmek için Apache Ambari kümeniz için Azure portal dikey penceresi aracılığıyla kullanılabilir olan kullanıcı arabirimini kullanın. Kümeniz için Ambari UI erişme hakkında yönergeler için bkz: [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md).

ML Hizmetleri çağrısında Hadoop anahtarlarını kullanarak kullanılabilir bellek miktarını değiştirmek mümkündür **RxHadoopMR** gibi:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Kümenizi ölçeklendirme

Var olan bir ML hizmetleri kümesine Hdınsight üzerinde yukarı veya aşağı portalı üzerinden genişletilebilir. Ölçeklendirme, büyük işleme görevler için gereksinim duyabileceğiniz ek kapasite elde edebilirsiniz veya boşta olduğunda, bir küme geri ölçeklendirebilirsiniz. Küme ölçeklendirme hakkında yönergeler için bkz: [Hdınsight kümelerini yönetme](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Sistem koruma

İşletim sistemi yamalarını ve diğer güncelleştirmeleri uygulamak için bakım saatlerde temel Linux VM'ler Hdınsight kümesi için gerçekleştirilir. Genellikle, bakım 03:30 da (VM için yerel saate göre) her Pazartesi ve Salı yapılır. Güncelleştirmeleri, birden fazla çeyreği küme birer birer etkisi yoktur şekilde gerçekleştirilir.

Baş düğümler yedekli ve tüm veri düğümleri etkilenen olduğundan, bu süre boyunca çalıştıran herhangi bir işi yavaşlayabilir. Ancak, bunlar hala tamamlanıncaya kadar çalıştırmanız gerekir. Yıkıcı bir hata, bir küme yeniden oluşturma gerektiren oluşmadığı sürece bu bakım olaylar arasında herhangi bir özel yazılım veya sahip olduğunuz yerel veriler korunur.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>Hdınsight üzerinde ML Hizmetleri için IDE seçenekleri

Hdınsight kümesinin Linux kenar düğümüne R tabanlı çözümleme için giriş bölgedir. Hdınsight en son sürümleri Rstudio'dan sunucusunun varsayılan yüklemesini kenar düğümüne tarayıcı tabanlı bir IDE sağlar. Bir IDE geliştirmeye yönelik olarak Rstudio'dan sunucu kullanımını ve R betiklerinin yürütülmesi R konsol kullanmaktan daha önemli ölçüde daha üretken olabilirler.

Ayrıca, bir masaüstü IDE yükleyebilir ve bir uzak MapReduce veya Spark işlem bağlamı kullanarak kümeye erişmek için kullanabilirsiniz. Seçenekleriniz Microsoft'un [R araçları Visual Studio için](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) Rstudio'dan ve Walware Eclipse tabanlı [StatET](http://www.walware.de/goto/statet).

Ayrıca, kenar düğümüne R konsolda yazarak erişebileceğiniz **R** SSH veya PuTTY üzerinden bağlandıktan sonra Linux komut isteminde. Konsol arabirimini kullanırken, bir metin düzenleyicisi R betiği geliştirme için başka bir penceresinde çalıştırın ve kesip kodunuzu bölümlerini R konsola gerektiğinde kullanışlıdır.

## <a name="pricing"></a>Fiyatlandırma

ML Hizmetleri Hdınsight kümesi ile ilişkili fiyatlar benzer şekilde, fiyatları diğer Hdınsight küme türleri için yapılandırılmıştır. Bunlar temel VM'ler boyutlandırma üzerinde adı, veri ve çekirdek saatlik uplift eklenmesi ile kenar düğümler arasında temel alır. Daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight kümelerinde ML Hizmetleri kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [ML Serices kümede Hdınsight kullanmaya başlama](r-server-get-started.md)
* [Hdınsight üzerinde ML Hizmetleri kümesi için içerik seçeneklerini işlem](r-server-compute-contexts.md)
* [Hdınsight üzerinde ML Hizmetleri küme için Depolama Seçenekleri](r-server-storage.md)