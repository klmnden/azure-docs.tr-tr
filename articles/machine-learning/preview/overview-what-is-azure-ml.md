---
title: Azure Machine Learning nedir? | Microsoft Docs
description: Uzman veri bilimcilerinin bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmesini, üzerinde deneme yapmasını ve dağıtmasını sağlayan tümleşik, uçtan uca veri bilimi çözümü olan Azure Machine Learning Denemesi ve Model Yönetimi'ne genel bakış.
services: machine-learning
author: mwinkle
ms.author: mwinkle
manager: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.openlocfilehash: e5716e3fc519c48aaea3ec17939d11008a1b1fd4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
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

Daha fazla bilgi için [Deneme Hizmeti Yapılandırması](experimentation-service-configuration.md) konusunu inceleyin.

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
Azure'da Azure Machine Learning'e ek olarak makine öğrenimi modellerini derlemek, dağıtmak ve yönetmek için kullanılabilecek birçok seçenek vardır. [Buradan bilgi edinebilirsiniz.](overview-more-machine-learning.md)

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Machine Learning'i yükleme ve içerik oluşturma](quickstart-installation.md)
