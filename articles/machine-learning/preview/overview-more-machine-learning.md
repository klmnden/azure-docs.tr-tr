---
title: Azure Machine Learning nedir? | Microsoft Docs
description: Uzman veri bilimcilerinin bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmesini, üzerinde deneme yapmasını ve dağıtmasını sağlayan tümleşik, uçtan uca veri bilimi çözümü olan Azure Machine Learning Denemesi ve Model Yönetimi'ne genel bakış.
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: 1c4826959bb92e2e515867261094b1154a3c805c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="machine-learning-solutions-from-microsoft"></a>Microsoft’tan makine öğrenimi çözümleri

Azure Machine Learning'e ek olarak Microsoft’ta makine öğrenimi modellerini derlemek, dağıtmak ve yönetmek için kullanılabilecek birçok seçenek vardır. 
* SQL Server Machine Learning Hizmetleri
* Microsoft Machine Learning Sunucusu
* Azure Machine Learning Services
* Azure Machine Learning Studio
* Veri Bilimi Sanal Makinesi
* HDInsight'ta Spark MLLib
* Batch AI Eğitim Hizmeti
* Microsoft Bilişsel Araç Seti
* Microsoft Bilişsel Hizmetleri


## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Hizmetleri
[Microsoft Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) R veya Python kullanarak makine öğrenimi modelleri çalıştırmanızı, eğitmenizi ve dağıtmanızı sağlar. Şirket içindeki ve SQL Server veritabanlarındaki verileri kullanabilirsiniz. 

Şirket içinde veya Microsoft SQL Server üzerinde model eğitmeniz veya dağıtmanız gerekiyorsa Microsoft Machine Learning Hizmetleri'ni kullanın. Machine Learning Hizmetleri ile derlenen modeller Azure Machine Learning Model Yönetimi kullanılarak dağıtılabilir. 

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu 
[Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) R ve Python işlemlerinin paralel ve dağıtılmış iş yüklerini barındırmak ve yönetmek için kullanılan kurumsal sunucudur. Microsoft Machine Learning Sunucusu Linux, Windows, Hadoop ve Apache Spark üzerinde çalışır. [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) ile de kullanılabilir. [Microsoft Machine Learning paketleri](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) kullanılarak derlenmiş çözümler için bir yürütme altyapısı sağlar ve açık kaynak R ve Python çözümlerini aşağıdaki senaryolar için sunulan destekle genişletir:

- Yüksek performanslı analiz
- istatistiksel analiz
- Makine öğrenimi
- Çok büyük veri kümeleri

Sunucuyla birlikte yüklenen özel paketler sayesinde ek işlevler sağlanır. Geliştirme için [Visual Studio için R Araçları](https://www.visualstudio.com/vs/rtvs/) ve [Visual Studio için Python Araçları](https://www.visualstudio.com/vs/python/) gibi IDE'leri kullanabilirsiniz.

Aşağıdaki durumda Microsoft Machine Learning Sunucusu'nu kullanın:

- R ve Python ile derlenen modelleri bir sunucuda derlemek ve dağıtmak için
- R ve Python eğitimini bir Hadoop veya Spark kümesinde ölçekli olarak dağıtmak için

## <a name="data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi
[Veri Bilimi Sanal Makinesi (DSVM)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) veri bilimi için Microsoft Azure bulutunda derlenmiş olan özel bir sanal makine görüntüsüdür. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. Windows Server ve Linux üzerinde kullanılabilir. DSVM'nin Windows sürümünü Server 2016 ve Server 2012'de sunuyoruz. DSVM'nin Linux sürümünü Ubuntu 16.04 LTS ve OpenLogic 7.2 CentOS tabanlı Linux dağıtımlarında sunuyoruz. 

Veri Bilimi Sanal Makinesini işlerinizi tek bir düğüm üzerinde çalıştırmanız veya barındırmanız gerektiğinde kullanın. İşlemlerinizi tek bir makinede uzaktan ölçeklendirmeniz gerektiğinde de kullanabilirsiniz. Veri Bilimi Sanal Makinesi hem Azure Machine Learning Denemesi hem de Azure Machine Learning Model Yönetimi için hedef olarak kullanılabilir. 

## <a name="spark-mllib-in-hdinsight"></a>HDInsight'ta Spark MLLib
[HDInsight'ta Spark MLLib](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) büyük veriler üzerinde yürütülen Spark işlerinin bir parçası olarak modeller oluşturmanızı sağlar. Spark verileri kolayca dönüştürüp hazırlamanızı ve ardından model oluşturma işleminin ölçeğini tek bir işte genişletmenizi sağlar. Spark MLLib ile oluşturulan modeller Azure Machine Learning Model Yönetimi ile dağıtılabilir, yönetilebilir ve izlenebilir. Eğitim çalıştırmaları Azure Machine Learning Denemesi ile dağıtılabilir ve yönetilebilir. Spark ayrıca Machine Learning Workbench'te oluşturulan veri hazırlama işlerinin ölçeğini genişletmek için de kullanılabilir. 

Veri işleme ve oluşturma modellerinizin ölçeğini veri işlem hattının bir parçası olarak genişletmeniz gerektiğinde Spark'ı kullanabilirsiniz. Spark işlerini Scala, Java, Python veya R ile oluşturabilirsiniz. 

## <a name="batch-ai-training"></a>Batch AI Eğitimi 
[Azure Batch AI Eğitimi](https://aka.ms/batchaitraining) herhangi bir çerçeveyi kullanarak AI modellerinizle paralel olarak deneme yapmanızı sağlar ve onları kümelenmiş GPU'lar ile büyük ölçekte eğitir. Çalıştırılacak işin gereksinimlerini ve yapılandırmasını tanımladıktan sonra gerisini bize bırakabilirsiniz. 

Batch AI Eğitimi derin öğrenme işlerinin ölçeğini aşağıdakiler gibi çerçeveleri kullanarak kümelenmiş GPU'lar üzerinde genişletmenizi sağlar:

- Bilişsel Araç Seti
- Caffe
- Chainer
- TensorFlow

Azure Machine Learning Model Yönetimi Batch AI Eğitimi modellerini dağıtmak, yönetmek ve izlemek için kullanılabilir.  Batch AI Eğitimi gelecekte Azure Machine Learning Denemesi ile tümleştirilecektir. 

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti
[Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) sinir ağlarını yönlü grafik üzerinde işlem adımları olarak tanımlayan birleşik derin öğrenme araç setidir. Bu yönlü grafikte yaprak düğümler giriş değerlerini veya ağ parametrelerini, diğer düğümler ise girişlerle matris işlemlerini temsil eder. Bilişsel Araç Seti akış iletme DNN'leri, kıvrımlı ağlar (CNN'ler) ve yinelenen ağlar (RNN'ler/LSTM'ler) gibi popüler model türlerini kolayca gerçekleştirmenizi ve birleştirmenizi sağlar. Stokastik aşama azaltma (SGD, hata geri yayılımı) öğrenmeyi birden fazla GPU ve sunucu üzerinde otomatik farklılaştırma ve paralelleştirme ile uygulamaya alır.

Bilişsel Araç Setini derin öğrenmeyi kullanarak bir model derlemek istediğinizde kullanın.  Bilişsel Araç Seti önceki hizmetlerin herhangi birinde kullanılabilir.

## <a name="microsoft-cognitive-services"></a>Microsoft Bilişsel Hizmetleri
Microsoft Bilişsel Hizmetleri doğal iletişim yöntemlerini kullanmanızı sağlayan 30 API'lik bir settir. Bu API'ler birkaç satırlık kodlarla uygulamanızın görmesini, duymasını, konuşmasını, anlamasını ve ihtiyaçları yorumlamasını sağlar. Uygulamalarınıza aşağıdaki gibi akıllı özellikleri kolayca ekleyebilirsiniz: 

- Duygu ve düşünceleri algılama
- Görme ve konuşma Tanıma
- Dil anlama
- Bilgi ve arama

Microsoft Bilişsel Hizmetleri farklı cihaz ve platformlarda uygulama geliştirmenizi sağlar. API'ler sürekli olarak geliştirilir ve kolayca ayarlanabilir. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](overview-what-is-azure-ml.md)’e genel bakışı okuyun.