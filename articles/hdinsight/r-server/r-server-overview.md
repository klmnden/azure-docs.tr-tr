---
title: Azure hdınsight'ta R Server Giriş | Microsoft Docs
description: R Server Hdınsight'ta büyük veri analizi için uygulamalar oluşturmak için nasıl kullanılacağını öğrenin.
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
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: 19c286db9a8a2aa537badc83d98a1b74b73e9873
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>R Server ve açık kaynaklı R yetenekleri hdınsight'ta giriş

Microsoft R Server (Microsoft Machine Learning sunucusu da bilinir) Azure Hdınsight kümeleri oluşturduğunuzda, bir dağıtım seçeneği olarak kullanılabilir. Bu seçenek sağlar küme türü adlı **R Server**. Bu özellik, veri bilimcileri, istatistikçiler ve ölçeklenebilir, isteğe bağlı erişimi olan R programcıları hdınsight'ta analytics yöntemlerinin dağıtılmış sağlar.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Hdınsight'ta R Server Azure Blob veya Data Lake depolama birimine yüklenen neredeyse herhangi bir boyuttaki veri kümeleri R tabanlı analytics için en son özellikleri sağlar. R Server küme açık kaynak R kurulu olduğundan, yapı R tabanlı uygulamalar 8000 + açık kaynak R paketlerinden birini yararlanabilirsiniz. ScaleR, Microsoft'un büyük veri analizi paket R Server ile gelen çalışmalarında de kullanılabilir.

Kümenin kenar düğümü kümeye bağlanın ve R komut dosyalarını çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile ScaleR parallelized dağıtılmış işlevleri kenar düğümü sunucu Çekirdeğinde çalıştırırken seçeneğiniz vardır. Ayrıca bunları küme düğümleri arasında ScaleR'ın eşlemesi Hadoop azaltmak kullanarak çalıştırabilirsiniz veya Spark işlem bağlamı.

Şirket içi kullanım için modelleri veya çözümlemesinden neden tahminleri indirilebilir. Bunlar ayrıca başka bir yerde Azure'da, özellikle aracılığıyla operationalized [Azure Machine Learning Studio](http://studio.azureml.net) [web hizmeti](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-server-on-hdinsight"></a>HDInsight üzerinde R Server kullanma

Azure Hdınsight'ta R Server küme oluşturmak için seçin **R Server** Azure portalını kullanarak bir Hdınsight kümesi oluştururken küme türü. R Server küme türü R Server kümesinin veri düğümlerini ve R Server tabanlı analiz için giriş bölgesi görevi gören bir edge düğümünü içerir. Bkz: [hdınsight'ta R Server ile çalışmaya başlama](r-server-get-started.md) küme oluşturma konusunda kılavuz.

## <a name="why-choose-r-server-in-hdinsight"></a>Neden Hdınsight'ta R Server seçecekler?

R Server hdınsight'ta R Server Azure üzerinde kullanma seçeneği sağlar. R Server içerir:

+ **En iyi AI yenilik Microsoft ve açık kaynak**

  AI her kişi ve kuruluş için erişilebilir hale getirmek Microsoft içindedir. R Server içeren zengin bir algoritmaları düzeyde ölçeklenebilir, dağıtılmış kümesi gibi [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), ve [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) daha büyük veri boyutları üzerinde çalışabilir fiziksel bellek ve çok çeşitli platformlarda dağıtılmış bir şekilde çalıştırılmasında boyutundan. Özel Microsoft Topluluğu hakkında daha fazla bilgi edinin [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) ve [Python paketlerini](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) ürünle birlikte gelen.
  
  R Server arasında köprü bu Microsoft yenilikler ve bunlar tüm tek Kurumsal düzeyde platform en üstünde açık kaynak topluluğu (R, Python ve AI araç takımları)'ten gelen. Herhangi bir açık kaynak makine öğrenme R veya Python paket Microsoft'un mülkiyet herhangi yenilik yan yana çalışabilirsiniz. 

+ **Basit, güvenli ve büyük ölçekli operationalization ve yönetim**

  Geleneksel operationalization örneklerinde ve ortamları bağlı olan kuruluşlar, çok zaman ve çaba bu alanı doğru yatırım yukarı sonlandırın. Zorluk noktaları genellikle inflated maliyetlerini kaynaklanan ve gecikmeleri Ekle: modeller, geçerli ve güncel tutmak için yineleme çeviri süredir operationalization aracılığıyla yetkilerinin yasal onay.

  R Server en iyi sınıf sunar [operationalization](https://docs.microsoft.com/machine-learning-server/what-is-operationalization) --isteğe bağlı olarak bir machine learning modelini tamamlandığında zamandan web hizmetleri API'ları oluşturmak için yalnızca birkaç tıklama alır. Bunlar [web Hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) bir sunucu ızgara üzerinde barındırılan şirket içi veya bulutta ve iş kolu satır uygulamaları ile tümleştirilebilir. Hem batch ve gerçek zamanlı Puanlama için kendi iş gereksinimlerinizi ile sorunsuz ölçeklendirme esnek kılavuz sağlar dağıtma yeteneği. Yönergeler için bkz: [hdınsight'ta R Server faaliyete](r-server-operationalize.md).

+ **En iyi toplam sahip olma maliyetini ile müşteri başarılı sunmak için derin ekosistemi katılımlar**

  Uygulamalarını akıllı yapma veya AI ve makine öğrenimi yardımcı olmak için doğru kaynakları başlamak yeni world bilgi isteyen gezisine başlamadan kişiler. Bu belge, ek olarak Microsoft birkaç öğrenme kaynakları sağlar ve yukarı artırmalarını ve hızla üretken yardımcı olması için çeşitli eğitim ortakları gerçekleştiriliyor.


## <a name="key-features-of-r-server-on-hdinsight"></a>Hdınsight'ta R Server temel özellikleri

Aşağıdaki özellikler hdınsight'ta R Server dahil edilmiştir.

| Özellik kategorisi | Açıklama |
|------------------|-------------|
| R etkin | [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) R, R ve çalışma zamanı altyapısı komut dosyasının yürütülmesi için açık kaynak dağıtılması ile yazılmış çözümleri için. |
| Python etkin | [Python modülleri](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) Python ve çalışma zamanı altyapısı komut dosyasının yürütülmesi için açık kaynak dağıtılması ile Python içinde yazılmış çözümleri için.  
| [Önceden eğitilmiş modeller](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Görsel çözümleme ve metin düşünceleri çözümlemesi için veri puan hazır, sağlar. |
| [Dağıtma ve kullanma](r-server-operationalize.md) | Sunucunuz faaliyete ve çözümleri bir web hizmeti olarak dağıtabilirsiniz. |
| [Uzaktan yürütme](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-server-or-client) | Uzak Oturumlar, ağınızdaki istemci iş istasyonundan R sunucuda başlatın. |


## <a name="data-storage-options-for-r-server-on-hdinsight"></a>Hdınsight'ta R Server için veri depolama seçenekleri

Hdınsight kümeleri HDFS dosya sistemi için varsayılan depolama bir Azure Storage hesabı veya bir Azure Data Lake Store ile ilişkili olabilir. Bu ilişkilendirme hangi verilerin karşıya yüklendiğini sağlar kümeye depolama Çözümleme sırasında kalıcı yapılan ve hatta kümesi silindikten sonra veriler kullanılabilir. Veri aktarımı, depolama hesabının portal tabanlı karşıya yükleme olanağı dahil olmak üzere seçtiğiniz depolama seçeneğine işlemek için çeşitli araçlar vardır ve [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programı.

Erişim için ek Blob ekleme seçeneğiniz vardır ve sağlama işlemi bağımsız kullanılan birincil depolama seçeneği olarak küme sırasında Data lake depolar. Bkz: [hdınsight'ta R Server kullanmaya başlama](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) erişim için ek hesapları ekleme hakkında bilgi için. Tamamlayıcı bkz [hdınsight'ta R Server için Azure Depolama Seçenekleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) makalenin birden çok depolama hesabı kullanma hakkında daha fazla bilgi edinin.

Aynı zamanda [Azure dosyaları](../../storage/files/storage-how-to-use-files-linux.md) kenar düğümünü kullanmak için bir depolama seçeneği olarak. Azure dosyaları Azure Storage'da Linux dosya sistemine oluşturulmuş bir dosya paylaşımını bağlama olanak sağlar. Hdınsight kümesi R Server için bu veri depolama seçenekleri hakkında daha fazla bilgi için bkz: [hdınsight'ta R Server için Azure Depolama Seçenekleri](r-server-storage.md).

## <a name="access-machine-learning-server-on-the-cluster"></a>Erişim Machine Learning küme sunucusunda

Bir tarayıcı kullanarak kenar düğümüne Microsoft Machine Learning sunucusuna bağlanabilir. Küme oluşturma sırasında varsayılan olarak yüklenir. Daha fazla bilgi için bkz: [hdınsight'ta R Server ile stared](r-server-get-started.md). Ayrıca, R konsoluna erişmek için SSH/PuTTY kullanarak da komut satırından ML sunucuya bağlanabilirsiniz. 

## <a name="develop-and-run-r-scripts"></a>Geliştirme ve R komut dosyalarını çalıştır

Oluşturma ve çalıştırma R betiklerini 8000 + açık kaynak R paketleri kullanılabilir paralel birkaç ölçeklendirin ve dağıtılmış yordamlarını yanı sıra birini ScaleR Kitaplığı'nda kullanabilirsiniz. Genel olarak, R Server edge düğümü üzerinde çalışacak bir betik içinde R yorumlayıcı bu düğüm üzerinde çalışır. Ayarlanmış bir işlem bağlamına sahip ScaleR işlevi çağırmak için gereken adımları istisnaları Hadoop harita azaltmak (RxHadoopMR) veya Spark (RxSpark). Bu durumda, işlevi başvurulan veri ile ilişkili olan bu verileri (görev) kümenin düğümleri arasında dağıtılmış bir şekilde çalışır. Farklı işlem bağlamı seçenekleri hakkında daha fazla bilgi için bkz: [işlem hdınsight'ta R Server için içerik seçeneklerini](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme

Veri modellemesi tamamlandığında, Azure ve şirket içi'nden ya da yeni veriler için tahminlerde modelini faaliyete geçirebilirsiniz. Bu işlem Puanlama olarak bilinir. Puanlama Hdınsight, Azure Machine Learning veya şirket içi yapılabilir.

### <a name="score-in-hdinsight"></a>Hdınsight'ta puan
Hdınsight'ta Puanlama amacıyla depolama hesabınıza yüklediğiniz yeni bir veri dosyası için tahminlerde modelinizi çağıran bir R işlevi yazma. Ardından, depolama hesabına tahminleri kaydedin. Bu yordamı isteğe bağlı kenar düğümüne kümenizin veya zamanlanmış bir iş kullanarak çalıştırabilirsiniz.

### <a name="score-in-azure-machine-learning-aml"></a>Puan azure'da makine öğrenme (AML)
Azure Machine Learning kullanarak Puanlama amacıyla olarak bilinen açık kaynaklı Azure Machine Learning R paketi kullanmak [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) modelinizi bir Azure web hizmeti olarak yayımlamak için. Kolaylık olması için bu paket edge düğüm üzerinde önceden büyük/küçük harf yüklenir. Ardından, web hizmeti için kullanıcı arabirimi oluşturmak üzere Azure Machine Learning tesislerde kullanın ve ardından Puanlama için gerektiği gibi web hizmeti çağrısı.

Bu seçeneği seçerseniz, web hizmeti ile kullanılmak üzere eşdeğeri açık kaynak modeli nesnelere herhangi ScaleR model nesneleri dönüştürmeniz gerekir. ScaleR zorlama işlevleri gibi kullandığınız `as.randomForest()` bu dönüştürme için ensemble tabanlı modelleri için.

### <a name="score-on-premises"></a>Puan şirket içi
Modelinizi oluşturduktan sonra şirket içi Puanlama amacıyla R modelinde seri, indirir seri durumdan ve yeni veri Puanlama için kullanmak. Daha önce açıklanan yaklaşımı kullanarak, yeni verilerinizi puanlamada [Hdınsight'ta Puanlama](#scoring-in-hdinsight) veya kullanarak [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Küme koru

### <a name="install-and-maintain-r-packages"></a>Yükleme ve R paketleri koru

Kullandığınız R paketleri çoğunu kenar düğümüne var. çalıştırmak, R betiklerini çoğu adımları bu yana gereklidir. Kenar düğümüne ek R paketlerini yüklemek için normal kullanabileceğiniz `install.packages()` r yöntemi

Yalnızca yordamları ScaleR Kitaplığı'ndan küme üzerinde kullanıyorsanız, genellikle veri düğümlerinde ek R paketlerini yüklemek gerekmez. Ancak, kullanımını desteklemek için ek paketler gerekebilir **rxExec** veya **RxDataStep** veri düğümlerde yürütme.

Kümeyi oluşturduktan sonra bu gibi durumlarda, bir komut dosyası eylemi ek paketler yüklenebilir. Daha fazla bilgi için bkz: [R sunucunuzu yönetin Hdınsight kümesi](r-server-hdinsight-manage.md).

### <a name="change-hadoop-mapreduce-memory-settings"></a>Hadoop MapReduce bellek ayarlarını değiştirme

Bir küme bir MapReduce işi çalıştırılırken R sunucusunda kullanılabilir bellek miktarını değiştirmek için değiştirilebilir. Bir küme değiştirmek için Apache Ambari kümeniz için Azure portal dikey penceresi aracılığıyla kullanılabilir olan kullanıcı arabirimini kullanın. Kümeniz için Ambari UI erişme hakkında yönergeler için bkz: [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md).

R Server çağrısında Hadoop anahtarlarını kullanarak kullanılabilir bellek miktarını değiştirmek mümkündür **RxHadoopMR** gibi:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Kümenizi ölçeklendirme

Hdınsight üzerinde var olan bir R Server küme yukarı veya aşağı portalı üzerinden genişletilebilir. Ölçeklendirme, büyük işleme görevler için gereksinim duyabileceğiniz ek kapasite elde edebilirsiniz veya boşta olduğunda, bir küme geri ölçeklendirebilirsiniz. Küme ölçeklendirme hakkında yönergeler için bkz: [Hdınsight kümelerini yönetme](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Sistem koruma

İşletim sistemi yamalarını ve diğer güncelleştirmeleri uygulamak için bakım saatlerde temel Linux VM'ler Hdınsight kümesi için gerçekleştirilir. Genellikle, bakım 03:30 da (VM için yerel saate göre) her Pazartesi ve Salı yapılır. Güncelleştirmeleri, birden fazla çeyreği küme birer birer etkisi yoktur şekilde gerçekleştirilir.  

Baş düğümler yedekli ve tüm veri düğümleri etkilenen olduğundan, bu süre boyunca çalıştıran herhangi bir işi yavaşlayabilir. Ancak, bunlar hala tamamlanıncaya kadar çalıştırmanız gerekir. Yıkıcı bir hata, bir küme yeniden oluşturma gerektiren oluşmadığı sürece bu bakım olaylar arasında herhangi bir özel yazılım veya sahip olduğunuz yerel veriler korunur.

## <a name="ide-options-for-r-server-on-an-hdinsight-cluster"></a>Hdınsight kümesinde R Server için IDE seçenekleri

Hdınsight kümesinin Linux kenar düğümüne R tabanlı çözümleme için giriş bölgedir. Hdınsight en son sürümleri Rstudio'dan sunucusunun varsayılan yüklemesini kenar düğümüne tarayıcı tabanlı bir IDE sağlar. Bir IDE geliştirmeye yönelik olarak Rstudio'dan sunucu kullanımını ve R betiklerinin yürütülmesi R konsol kullanmaktan daha önemli ölçüde daha üretken olabilirler.

Ayrıca, bir masaüstü IDE yükleyebilir ve bir uzak MapReduce veya Spark işlem bağlamı kullanarak kümeye erişmek için kullanabilirsiniz. Seçenekleriniz Microsoft'un [R araçları Visual Studio için](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) Rstudio'dan ve Walware Eclipse tabanlı [StatET](http://www.walware.de/goto/statet).

Son olarak, kenar düğümüne R konsolda yazarak erişebileceğiniz **R** SSH veya PuTY aracılığıyla bağlandıktan sonra Linux komut isteminde. Konsol arabirimini kullanırken, bir metin düzenleyicisi R betiği geliştirme için başka bir penceresinde çalıştırın ve kesip kodunuzu bölümlerini R konsola gerektiğinde kullanışlıdır.

## <a name="pricing"></a>Fiyatlandırma

R sunucusu olan bir Hdınsight kümesi ile ilişkili fiyatlar benzer şekilde, fiyatları standart Hdınsight kümeleri için yapılandırılmıştır. Bunlar temel VM'ler boyutlandırma üzerinde adı, veri ve çekirdek saatlik uplift eklenmesi ile kenar düğümler arasında temel alır. Hdınsight fiyatlandırma hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight kümelerinde R Server kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Hdınsight'ta R Server küme ile çalışmaya başlama](r-server-get-started.md)
* [HDInsight üzerinde R Server kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde R Server kümesi için Azure Depolama seçenekleri](r-server-storage.md)
