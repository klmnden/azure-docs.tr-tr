---
title: "Azure hdınsight'ta R Server Giriş | Microsoft Docs"
description: "R Server Hdınsight'ta büyük veri analizi için uygulamalar oluşturmak için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 57e28215124bc0330517c541e4cb74a66d939ff5
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
#<a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>R Server ve açık kaynaklı R yetenekleri hdınsight'ta giriş

Azure Hdınsight kümeleri oluşturduğunuzda Microsoft R Server bir dağıtım seçeneği olarak kullanılabilir. Bu yeni özellik, veri bilimcileri, istatistikçiler ve ölçeklenebilir, isteğe bağlı erişimi olan R programcıları hdınsight'ta analytics yöntemlerinin dağıtılmış sağlar.

Kümeler projeler ve elinizdeki görevleri uygun şekilde boyutlandırılmış ve artık gerekmediğinde sonra bozuk. Azure Hdınsight parçası olup olmadıklarını olduğundan, bu kümeleri kuruluş düzeyinde 7/24 destek, % 99,9 çalışma süresi SLA'sı ve diğer bileşenler Azure ekosistemi ile tümleştirme özelliği gelir.

Hdınsight'ta R Server Azure Blob veya Data Lake depolama birimine yüklenen neredeyse herhangi bir boyuttaki veri kümeleri R tabanlı analytics için en son özellikleri sağlar. R Server açık kaynak R kurulu olduğundan, yapı R tabanlı uygulamalar 8000 + R açık kaynak paketlerinden birini yararlanabilirsiniz. ScaleR, Microsoft'un büyük veri analizi paket R Server ile gelen çalışmalarında de kullanılabilir.

Kümenin kenar düğümü kümeye bağlanın ve R komut dosyalarını çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile ScaleR parallelized dağıtılmış işlevleri kenar düğümü sunucu Çekirdeğinde çalıştırırken seçeneğiniz vardır. Ayrıca bunları küme düğümleri arasında ScaleR'ın eşlemesi Hadoop azaltmak kullanarak çalıştırabilirsiniz veya Spark işlem bağlamı.

Modelleri veya çözümlemeler sonucundan indirilebilir tahminleri şirket içi kullanın. Bunlar ayrıca başka bir yerde Azure'da, özellikle aracılığıyla operationalized [Azure Machine Learning Studio](http://studio.azureml.net) [web hizmeti](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Hdınsight'ta R kullanmaya başlama
R Server bir Hdınsight kümesine dahil etmek için Azure portalını kullanarak bir Hdınsight kümesi oluştururken R Server küme türü seçmelisiniz. R Server küme türü R Server kümesinin veri düğümlerini ve R Server tabanlı analiz için giriş bölgesi görevi gören bir edge düğümünü içerir. Bkz: [hdınsight'ta R Server ile çalışmaya başlama](r-server-get-started.md) küme oluşturma konusunda kılavuz.

## <a name="learn-about-data-storage-options"></a>Veri depolama seçenekleri hakkında bilgi edinin
Hdınsight kümeleri HDFS dosya sistemi için varsayılan depolama bir Azure Storage hesabı veya bir Azure Data Lake deposu ile ilişkili olabilir. Bu ilişkilendirme hangi verilerin karşıya yüklendiğini sağlar kümeye depolama Çözümleme sırasında kalıcı yapılır. Veri aktarımı, depolama hesabının portal tabanlı karşıya yükleme olanağı dahil olmak üzere seçtiğiniz depolama seçeneğine işlemek için çeşitli araçlar vardır ve [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programı.

Erişim için ek Blob ekleme seçeneğiniz vardır ve sağlama işlemi bağımsız kullanılan birincil depolama seçeneği olarak küme sırasında Data lake depolar. Bkz: [hdınsight'ta R Server kullanmaya başlama](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) erişim için ek hesapları ekleme hakkında bilgi için. Tamamlayıcı bkz [hdınsight'ta R Server için Azure Depolama Seçenekleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) makalenin birden çok depolama hesabı kullanma hakkında daha fazla bilgi edinin.

Aynı zamanda [Azure dosyaları](../../storage/files/storage-how-to-use-files-linux.md) kenar düğümünü kullanmak için bir depolama seçeneği olarak. Azure dosyaları Azure Storage'da Linux dosya sistemine oluşturulmuş bir dosya paylaşımını bağlama olanak sağlar. Hdınsight kümesi R Server için bu veri depolama seçenekleri hakkında daha fazla bilgi için bkz: [Azure depolama seçenekleri hdınsight'ta R Server kümeleri için](r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Erişim R Server kümede
R sunucuya bir tarayıcı kullanarak kenar düğümüne bağlanabilirsiniz. Küme oluşturma sırasında varsayılan olarak yüklenir. Daha fazla bilgi için bkz: [hdınsight'ta R Server ile stared](r-server-get-started.md).

Ayrıca, R Server R konsoluna erişmek için SSH/PuTTY kullanarak komut satırı bağlanabilir. 

## <a name="develop-and-run-r-scripts"></a>Geliştirme ve R komut dosyalarını çalıştır
Oluşturma ve çalıştırma R betiklerini paralel birkaç ölçeklendirin ve dağıtılmış yordamları yanı sıra 8000 + R açık kaynak paketlerinden birini ScaleR Kitaplığı'nda kullanabilirsiniz. Genel olarak, R Server edge düğümü üzerinde çalışacak bir betik içinde R yorumlayıcı bu düğüm üzerinde çalışır. Ayarlanmış bir işlem bağlamına sahip ScaleR işlevi çağırmak için gereken adımları istisnaları Hadoop harita azaltmak (RxHadoopMR) veya Spark (RxSpark). Bu durumda, işlevi başvurulan veri ile ilişkili olan bu verileri (görev) kümenin düğümleri arasında dağıtılmış bir şekilde çalışır. Farklı işlem bağlamı seçenekleri hakkında daha fazla bilgi için bkz: [işlem hdınsight'ta R Server için içerik seçeneklerini](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Bir model faaliyete
Veri modellemesi tamamlandığında, Azure ve şirket içi'nden ya da yeni veriler için tahminlerde modelini faaliyete geçirebilirsiniz. Bu işlem Puanlama olarak bilinir. Puanlama Hdınsight, Azure Machine Learning veya şirket içi yapılabilir.

### <a name="score-in-hdinsight"></a>Hdınsight'ta puan
Hdınsight'ta Puanlama amacıyla depolama hesabınıza yüklediğiniz yeni bir veri dosyası için tahminlerde modelinizi çağıran bir R işlevi yazma. Ardından depolama hesabına tahminleri kaydedin. Kümenizin veya zamanlanmış bir iş kullanarak kenar düğümüne rutin isteğe bağlı çalıştırabilirsiniz.  

### <a name="score-in-azure-machine-learning-aml"></a>Puan azure'da makine öğrenme (AML)
Bir AML web hizmetini kullanarak Puanlama amacıyla olarak bilinen açık kaynaklı Azure Machine Learning R paketi kullanan [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) modelinizi bir Azure web hizmeti olarak yayımlamak için. Kolaylık olması için bu paket edge düğüm üzerinde önceden büyük/küçük harf yüklenir. Ardından, web hizmeti için kullanıcı arabirimi oluşturmak için Machine Learning tesislerde kullanın ve ardından Puanlama için gerektiği gibi web hizmeti çağrısı.

Bu seçeneği seçerseniz, web hizmeti ile kullanılmak üzere eşdeğeri açık kaynak modeli nesnelere herhangi ScaleR model nesneleri dönüştürmeniz gerekir. ScaleR zorlama işlevleri gibi kullandığınız `as.randomForest()` bu dönüştürme için ensemble tabanlı modelleri için.

### <a name="score-on-premises"></a>Puan şirket içi
Modelinizi oluşturduktan sonra şirket içi Puanlama amacıyla R modelinde seri, indirir seri durumdan ve yeni veri Puanlama için kullanmak. Daha önce açıklanan yaklaşımı kullanarak, yeni verilerinizi puanlamada [Hdınsight'ta Puanlama](#scoring-in-hdinsight) veya kullanarak [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Küme koru
### <a name="install-and-maintain-r-packages"></a>Yükleme ve R paketleri koru
Kullandığınız R paketleri çoğunu kenar düğümüne var. çalıştırmak, R betiklerini çoğu adımları bu yana gereklidir. Kenar düğümüne ek R paketlerini yüklemek için normal kullanabileceğiniz `install.packages()` r yöntemi

Yalnızca yordamları ScaleR Kitaplığı'ndan küme üzerinde kullanıyorsanız, genellikle veri düğümlerinde ek R paketlerini yüklemek gerekmez. Ancak, kullanımını desteklemek için ek paketler gerekebilir **rxExec** veya **RxDataStep** veri düğümlerde yürütme.

Kümeyi oluşturduktan sonra bu durumlarda, bir komut dosyası eylemi ek paketler yüklenebilir. Daha fazla bilgi için bkz: [R sunucusuyla bir Hdınsight kümesi oluşturma](r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Harita Hadoop azaltmak bellek ayarlarını değiştirme
Bir küme bir harita azaltmak işi çalıştırılırken R sunucusunda kullanılabilir bellek miktarını değiştirmek için değiştirilebilir. Bir küme değiştirmek için Apache Ambari kümeniz için Azure portal dikey penceresi aracılığıyla kullanılabilir olan kullanıcı arabirimini kullanın. Kümeniz için Ambari UI erişme hakkında yönergeler için bkz: [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md).

R Server çağrısında Hadoop anahtarlarını kullanarak kullanılabilir bellek miktarını değiştirmek mümkündür **RxHadoopMR** gibi:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Kümenizi ölçeklendirme
Varolan bir kümeye yukarı veya aşağı portalı üzerinden genişletilebilir. Ölçeklendirme, büyük işleme görevler için gereksinim duyabileceğiniz ek kapasite elde edebilirsiniz veya boşta olduğunda, bir küme geri ölçeklendirebilirsiniz. Küme ölçeklendirme hakkında yönergeler için bkz: [Hdınsight kümelerini yönetme](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Sistem koruma
İşletim sistemi yamalarını ve diğer güncelleştirmeleri uygulamak için bakım saatlerde temel Linux VM'ler Hdınsight kümesi için gerçekleştirilir. Genellikle, bakım 03:30 da (VM için yerel saate göre) her Pazartesi ve Salı yapılır. Güncelleştirmeleri, birden fazla çeyreği küme birer birer etkisi yoktur şekilde gerçekleştirilir.  

Baş düğümler yedekli ve tüm veri düğümleri etkilenen olduğundan, bu süre boyunca çalıştıran herhangi bir işi yavaşlayabilir. Bunlar hala tamamlanıncaya kadar ancak çalışması gerekir. Yıkıcı bir hata, bir küme yeniden oluşturma gerektiren oluşmadığı sürece bu bakım olaylar arasında herhangi bir özel yazılım veya sahip olduğunuz yerel veriler korunur.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Hdınsight kümesinde R Server IDE seçenekleri hakkında bilgi edinin
Hdınsight kümesinin Linux kenar düğümüne R tabanlı çözümleme için giriş bölgedir. Hdınsight en son sürümleri Rstudio'dan sunucusunun varsayılan yüklemesini kenar düğümüne tarayıcı tabanlı bir IDE sağlar. Bir IDE geliştirmeye yönelik olarak Rstudio'dan sunucu kullanımını ve R betiklerinin yürütülmesi R konsol kullanmaktan daha önemli ölçüde daha üretken olabilirler.

Başka bir tam IDE Masaüstü IDE yüklemek ve bir uzak eşleme azaltın veya Spark işlem bağlamı kullanarak kümeye erişmelerini kullanmak için bir seçenektir. Seçenekleriniz Microsoft'un [R araçları Visual Studio için](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) Rstudio'dan ve Walware Eclipse tabanlı [StatET](http://www.walware.de/goto/statet).

Son olarak, kenar düğümüne R Server konsolda yazarak erişebileceğiniz **R** SSH veya PuTY aracılığıyla bağlandıktan sonra Linux komut isteminde. Konsol arabirimini kullanırken, bir metin düzenleyicisi R betiği geliştirme için başka bir penceresinde çalıştırın ve kesip kodunuzu bölümlerini R konsola gerektiğinde kullanışlıdır.

## <a name="learn-about-pricing"></a>Fiyatlandırma hakkında bilgi edinin
R Server Hdınsight kümesiyle ilişkili ücretleri standart Hdınsight kümeleri ücretlerinin benzer şekilde yapılandırılmıştır. Bunlar temel VM'ler boyutlandırma üzerinde adı, veri ve çekirdek saatlik uplift eklenmesi ile kenar düğümler arasında temel alır. Hdınsight fiyatlandırma ve 30 günlük ücretsiz deneme kullanılabilirliği hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Sonraki adımlar
R Server Hdınsight kümeleri ile kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Hdınsight'ta R Server kullanmaya başlama](r-server-get-started.md)
* [HDInsight üzerinde R Server için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde R Server için Azure Depolama seçenekleri](r-server-storage.md)
