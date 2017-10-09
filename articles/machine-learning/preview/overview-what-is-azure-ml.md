---
title: Azure Machine Learning nedir? | Microsoft Belgeleri
description: "Uzman veri bilimcilerinin bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmesini, üzerinde deneme yapmasını ve dağıtmasını sağlayan tümleşik, uçtan uca veri bilimi çözümü olan Azure Machine Learning Denemesi ve Model Yönetimi'ne genel bakış."
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: c4919fb679eeb4d25eb0066b9bf617b057d44354
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---

# <a name="what-is-azure-machine-learning"></a>Azure Machine Learning nedir?

Azure Machine Learning tümleşik ve uçtan uca bir veri bilimi ve gelişmiş analiz çözümüdür. Bu çözüm veri bilimcilerin bulut ölçeğinde veri hazırlamasını, deneme geliştirmesini ve model dağıtmasını sağlar.

Azure Machine Learning'in ana bileşenleri şunlardır:
- Azure Machine Learning Workbench
- Azure Machine Learning Denemesi Hizmeti
- Azure Machine Learning Model Yönetimi Hizmeti
- Apache Spark için Microsoft Machine Learning Kitaplıkları (MMLSpark Kitaplığı)
- AI için Visual Studio Code Araçları

Bu uygulamalar ve hizmetler birlikte veri bilimi proje geliştirme ve dağıtma süreçlerinizi hızlandırmanıza yardımcı olur. 

![Azure Machine Learning Kavramları](media/overview-what-is-azure-ml/aml-concepts.png)

## <a name="open-source-compatible"></a>Açık kaynak uyumluluğu

Azure Machine Learning, açık kaynak teknolojileri için tam destek sunar. Aşağıdaki makine öğrenimi çerçeveleri gibi on binlerce açık kaynak Python paketini kullanabilirsiniz:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

Denemelerinizi Docker kapsayıcıları ve Spark kümeleri gibi yönetilen ortamlarda yürütebilirsiniz. Yürütmeyi hızlandırmak için [Azure'daki GPU etkin sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu) gibi gelişmiş donanımları da kullanabilirsiniz.

Azure Machine Learning, aşağıdaki açık kaynak teknolojilerinin üzerine kurulmuştur:

- [Jupyter Notebook](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Aynı zamanda [Apache Spark için Microsoft Machine Learning Kitaplığı](https://github.com/Azure/mmlspark) ve Bilişsel Araç Seti gibi Microsoft'un kendi açık kaynak teknolojilerini de içerir.

Ayrıca Microsoft tarafından büyük ölçekli sorunları çözmek için geliştirilmiş olan en gelişmiş, denenmiş ve doğrulanmış makine öğrenimi teknolojilerinden de faydalanabilirsiniz. Bu teknolojiler aşağıdakiler gibi birçok Microsoft ürününde zorlu testlere tabi tutulmuştur:

- Windows
- Bing
- Xbox
- Office
- SQL Server

Aşağıda Azure Machine Learning'e dahil edilmiş olan Microsoft makine öğrenimi teknolojilerinden bazıları listelenmiştir:

- [PROSE](https://microsoft.github.io/prose/) (PROgram Synthesis using Examples)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench
Azure Machine Learning Workbench, hem Windows hem de macOS üzerinde desteklenen masaüstü uygulaması ve komut satırı araçlarından oluşur. Veri bilimi yaşam döngünüzün tamamında makine öğrenimi çözümlerini yönetmenizi sağlar:

- Veri alımı ve hazırlığı
- Model geliştirme ve deneme yönetimi
- Farklı hedef ortamlarında model dağıtımı

Azure Machine Learning Workbench tarafından sunulan temel işlevler şunlardır:

- Örneklerle veri dönüştürme mantığını öğrenebilen veri hazırlama aracı.
- UX ve Python kodu aracılığıyla erişilebilen veri kaynağı soyutlaması.
- Görsel olarak oluşturulan veri hazırlık paketlerini çağırmak için Python SDK'sı.
- Yerleşik Jupyter Notebook hizmeti ve müşteri UX.
- Çalıştırma geçmişi görünümleriyle deneme izleme ve yönetim.
- Azure Active Directory aracılığıyla paylaşım ve işbirliği yapılmasını sağlayan rol tabanlı erişim denetimi.
- Her çalıştırma için otomatik proje anlık görüntüleri ve yerel Git tümleştirmesiyle etkinleştirilen özel sürüm denetimi. 
- Popüler Python IDE'leriyle tümleştirme.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [Veri Hazırlama Kullanıcı Kılavuzu](data-prep-user-guide.md)
- [Azure Machine Learning ile Git kullanma](using-git-ml-project.md)
- [Azure Machine Learning'de Jupyter Notebook kullanma](how-to-use-jupyter-notebooks.md)
- Dolaşım ve Paylaşma
- [Çalıştırma Geçmişi Kılavuzu](how-to-use-run-history-model-metrics.md)
- [IDE Tümleştirme](how-to-configure-your-IDE.md)

## <a name="azure-machine-learning-experimentation-service"></a>Azure Machine Learning Denemesi Hizmeti
Deneme Hizmeti makine öğrenimi denemelerinin yürütülmesini gerçekleştirir. Ayrıca Workbench için proje yönetimi, Git tümleştirmesi, erişim denetimi, dolaşım ve paylaşım desteği sunar. 

Kolay yapılandırma sayesinde denemelerinizi farklı işlem ortamı seçeneklerinde yürütebilirsiniz:

- Yerel yerel
- Yerel Docker kapsayıcısı
- Uzak sanal makinedeki Docker kapsayıcısı
- Spark kümesinin ölçeğini Azure ile genişletme

Deneme Hizmeti betiğinizin ayrı bir şekilde ve tekrarlanabilir sonuçlarla yürütüldüğünden emin olmak için sanal ortamlar oluşturur. Çalıştırma geçmişi bilgilerini kaydeder ve geçmişi görsel olarak sunar. Bu sayede deneme çalıştırmalarınızın arasından en iyi modeli kolayca seçebilirsiniz. 

Daha fazla bilgi için bkz. [Deneme Yürütme Yapılandırması](experiment-execution-configuration.md).

## <a name="azure-machine-learning-model-management-service"></a>Azure Machine Learning Model Yönetimi Hizmeti

Model Yönetimi Hizmeti veri bilimcilerin ve geliştirme ekiplerinin tahmine dayalı modelleri çok çeşitli ortamlarda dağıtmasını sağlar. Model sürümleri ve özellik alanı farklı eğitim çalıştırmalarından dağıtımlara kadar takip edilir. Modeller bulutta depolanır, kaydedilir ve yönetilir. 

Basit CLI komutlarını kullanarak modelinizi, puanlama betiklerinizi ve bağımlılıklarınızı Docker görüntüsündeki kapsayıcılara dahil edebilirsiniz. Bu görüntüler Azure'da barındırılan Docker kayıt defterine (Azure Container Registry) kaydedilir. Bunlar aşağıdaki hedeflere güvenilir şekilde dağıtılabilir:

- Yerel makineler
- Şirket içi sunucular
- Bulut
- IoT Edge cihazları

Dağıtım ölçeğinin bulut ile genişletilmesi için Azure Container Service (ACS) içinde çalışan Kubernetes kullanılır. Model yürütme telemetrisi görsel analiz için AppInsights içinde yakalanır. 

Model Yönetimi Hizmeti hakkında daha fazla bilgi almak için [Model Yönetimine Genel Bakış](model-management-overview.md)'a bakın.


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Apache Spark için Microsoft Machine Learning Kitaplığı

[MMLSpark](https://github.com/Azure/mmlspark)(Apache Spark için Microsoft Machine Learning Kitaplığı), Apache Spark için ayrıntılı öğrenme ve veri bilimi araçlarını sağlayan açık kaynaklı Spark paketidir. [Spark Machine Learning İşlem Hatlarını](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) ve [OpenCV](http://opencv.org/) kitaplığıyla tümleştirir. Büyük resim ve metin veri kümeleri için hızla güçlü, yüksek oranda ölçeklendirilebilir tahmine dayalı modeller ve analiz modelleri oluşturmanızı sağlar. Bazı önemli noktalar şunlardır:

- HDFS görüntülerini kolayca Spark DataFrame'e alın
- OpenCV dönüşümlerini kullanarak görüntü verilerine ön işlem uygulayın
- Microsoft Bilişsel Araç Seti'ni kullanan önceden eğitilmiş derin sinir ağlarını kullanarak görüntülere özellik kazandırın 
- Medikal varlık ayıklama için önceden eğitilmiş çift yönlü Keras LSTM'lerini kullanın
- Azure'daki N Serisi GPU sanal makinelerinde DNN tabanlı görüntü sınıflandırma modellerini eğitin
- Tek bir dönüştürücüyle SparkML içindeki temel elemanların üzerinde kullanışlı API'leri kullanarak serbest biçimli metin verilerine özellik kazandırın
- Verilere örtük özellik kazandırma sayesinde sınıflandırma ve regresyon modellerini kolayca eğitin
- Örnek başına metrikler dahil olmak üzere zengin metrik değerlendirmesi gerçekleştirin

Daha fazla bilgi için bkz. [Azure Machine Learning'de MMLSpark Kullanma](how-to-use-mmlspark.md).

## <a name="visual-studio-code-tools-for-ai"></a>AI için Visual Studio Code Araçları
AI için Visual Studio Code Araçları, Visual Studio Code'da bulunan ve Derin Öğrenme ile AI çözümlerinin derlenmesini, test edilmesini ve dağıtılmasını sağlayan bir uzantıdır. Aşağıdakiler dahil olmak üzere Azure Machine Learning'de birçok tümleştirme noktası sunar:
- Eğitim çalıştırmalarının performansını ve günlüğe kaydedilmiş metrikleri görüntüleyen çalıştırma geçmişi görünümü.
- Kullanıcıların Microsoft Bilişsel Araç Seti, TensorFlow ve diğer birçok derin öğrenme çerçevesiyle yeni projelere göz atmasını ve onları önyüklemesini sağlayan galeri görünümü. 
- Betiklerinizin yürüteceği işlem hedeflerini seçmek için gezgin görünümü.
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Microsoft tarafından sunulan makine öğrenimi seçenekleri nelerdir?
Azure'da Azure Machine Learning'e ek olarak makine öğrenimi modellerini derlemek, dağıtmak ve yönetmek için kullanılabilecek birçok seçenek vardır. 
* SQL Server'daki Microsoft Machine Learning Hizmetleri
* Microsoft Machine Learning Sunucusu
* Veri Bilimi Sanal Makinesi
* HDInsight'ta Spark MLLib
* Batch AI Eğitim Hizmeti
* Microsoft Bilişsel Araç Seti
* Microsoft Bilişsel Hizmetleri


### <a name="microsoft-machine-learning-services-in-sql-server"></a>SQL Server'daki Microsoft Machine Learning Hizmetleri
[Microsoft Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) R veya Python kullanarak makine öğrenimi modelleri çalıştırmanızı, eğitmenizi ve dağıtmanızı sağlar. Şirket içindeki ve SQL Server veritabanlarındaki verileri kullanabilirsiniz. 

Şirket içinde veya Microsoft SQL Server üzerinde model eğitmeniz veya dağıtmanız gerekiyorsa Microsoft Machine Learning Hizmetleri'ni kullanın. Machine Learning Hizmetleri ile derlenen modeller Azure Machine Learning Model Yönetimi kullanılarak dağıtılabilir. 

### <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu 
[Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) R ve Python işlemlerinin paralel ve dağıtılmış iş yüklerini barındırmak ve yönetmek için kullanılan kurumsal sunucudur. Microsoft Machine Learning Sunucusu Linux, Windows, Hadoop ve Apache Spark üzerinde çalışır. [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) ile de kullanılabilir. [Microsoft Machine Learning paketleri](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) kullanılarak derlenmiş çözümler için bir yürütme altyapısı sağlar ve açık kaynak R ve Python çözümlerini aşağıdaki senaryolar için sunulan destekle genişletir:

- yüksek performanslı analiz
- istatistiksel analiz
- makine öğrenimi
- çok büyük veri kümeleri

Sunucuyla birlikte yüklenen özel paketler sayesinde ek işlevler sağlanır. Geliştirme için [Visual Studio için R Araçları](https://www.visualstudio.com/vs/rtvs/) ve [Visual Studio için Python Araçları](https://www.visualstudio.com/vs/python/) gibi IDE'leri kullanabilirsiniz.

Aşağıdaki durumda Microsoft Machine Learning Sunucusu'nu kullanın:

- R ve Python ile derlenen modelleri bir sunucuda derlemek ve dağıtmak için
- R ve Python eğitimini bir Hadoop veya Spark kümesinde ölçekli olarak dağıtmak için

### <a name="data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi
[Veri Bilimi Sanal Makinesi (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview) veri bilimi için Microsoft Azure bulutunda derlenmiş olan özel bir sanal makine görüntüsüdür. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. Windows Server ve Linux üzerinde kullanılabilir. DSVM'nin Windows sürümünü Server 2016 ve Server 2012'de sunuyoruz. DSVM'nin Linux sürümünü Ubuntu 16.04 LTS ve OpenLogic 7.2 CentOS tabanlı Linux dağıtımlarında sunuyoruz. 

Veri Bilimi Sanal Makinesini işlerinizi tek bir düğüm üzerinde çalıştırmanız veya barındırmanız gerektiğinde kullanın. İşlemlerinizi tek bir makinede uzaktan ölçeklendirmeniz gerektiğinde de kullanabilirsiniz. Veri Bilimi Sanal Makinesi hem Azure Machine Learning Denemesi hem de Azure Machine Learning Model Yönetimi için hedef olarak kullanılabilir. 

### <a name="spark-mllib-in-hdinsight"></a>HDInsight'ta Spark MLLib
[HDInsight'ta Spark MLLib](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) büyük veriler üzerinde yürütülen Spark işlerinin bir parçası olarak modeller oluşturmanızı sağlar. Spark verileri kolayca dönüştürüp hazırlamanızı ve ardından model oluşturma işleminin ölçeğini tek bir işte genişletmenizi sağlar. Spark MLLib ile oluşturulan modeller Azure Machine Learning Model Yönetimi ile dağıtılabilir, yönetilebilir ve izlenebilir. Eğitim çalıştırmaları Azure Machine Learning Denemesi ile dağıtılabilir ve yönetilebilir. Spark ayrıca Machine Learning Workbench'te oluşturulan veri hazırlama işlerinin ölçeğini genişletmek için de kullanılabilir. 

Veri işleme ve oluşturma modellerinizin ölçeğini veri işlem hattının bir parçası olarak genişletmeniz gerektiğinde Spark'ı kullanabilirsiniz. Spark işlerini Scala, Java, Python veya R ile oluşturabilirsiniz. 

### <a name="batch-ai-training"></a>Batch AI Eğitimi 
[Azure Batch AI Eğitimi](https://aka.ms/batchaitraining) herhangi bir çerçeveyi kullanarak AI modellerinizle paralel olarak deneme yapmanızı sağlar ve onları kümelenmiş GPU'lar ile büyük ölçekte eğitir. Çalıştırılacak işin gereksinimlerini ve yapılandırmasını tanımladıktan sonra gerisini bize bırakabilirsiniz. 

Batch AI Eğitimi derin öğrenme işlerinin ölçeğini aşağıdakiler gibi çerçeveleri kullanarak kümelenmiş GPU'lar üzerinde genişletmenizi sağlar:

- Bilişsel Araç Seti
- Caffe
- Chainer
- TensorFlow

Azure Machine Learning Model Yönetimi Batch AI Eğitimi modellerini dağıtmak, yönetmek ve izlemek için kullanılabilir.  Batch AI Eğitimi gelecekte Azure Machine Learning Denemesi ile tümleştirilecektir. 

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti
[Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) sinir ağlarını yönlü grafik üzerinde işlem adımları olarak tanımlayan birleşik derin öğrenme araç setidir. Bu yönlü grafikte yaprak düğümler giriş değerlerini veya ağ parametrelerini, diğer düğümler ise girişlerle matris işlemlerini temsil eder. Bilişsel Araç Seti akış iletme DNN'leri, kıvrımlı ağlar (CNN'ler) ve yinelenen ağlar (RNN'ler/LSTM'ler) gibi popüler model türlerini kolayca gerçekleştirmenizi ve birleştirmenizi sağlar. Stokastik aşama azaltma (SGD, hata geri yayılımı) öğrenmeyi birden fazla GPU ve sunucu üzerinde otomatik farklılaştırma ve paralelleştirme ile uygulamaya alır.

Bilişsel Araç Setini derin öğrenmeyi kullanarak bir model derlemek istediğinizde kullanın.  Bilişsel Araç Seti önceki hizmetlerin herhangi birinde kullanılabilir.

### <a name="microsoft-cognitive-services"></a>Microsoft Bilişsel Hizmetleri
Microsoft Bilişsel Hizmetleri doğal iletişim yöntemlerini kullanmanızı sağlayan 30 API'lik bir settir. Bu API'ler birkaç satırlık kodlarla uygulamanızın görmesini, duymasını, konuşmasını, anlamasını ve ihtiyaçları yorumlamasını sağlar. Uygulamalarınıza aşağıdaki gibi akıllı özellikleri kolayca ekleyebilirsiniz: 

- Duygu ve düşünceleri algılama
- Görme ve konuşma Tanıma
- Dil anlama
- Bilgi ve arama

Microsoft Bilişsel Hizmetleri farklı cihaz ve platformlarda uygulama geliştirmenizi sağlar. API'ler sürekli olarak geliştirilir ve kolayca ayarlanabilir. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Machine Learning'i yükleme ve içerik oluşturma](quickstart-installation.md)

