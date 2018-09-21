---
title: Azure Machine Learning dışındaki diğer Microsoft makine öğrenimi ürünleri | Microsoft Docs
description: Azure Machine Learning'e ek olarak Microsoft'ta makine öğrenimi modellerini derlemek, dağıtmak ve yönetmek için kullanılabilecek birçok seçenek vardır.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: garyericson, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: overview
ms.date: 04/11/2018
ms.openlocfilehash: 138fbb64ca2d157059c952d9e82541cfac3ff597
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579063"
---
# <a name="other-machine-learning-products-and-services-from-microsoft"></a>Microsoft'un diğer makine öğrenimi ürünleri ve hizmetleri

[Azure Machine Learning'e](overview-what-is-azure-ml.md) ek olarak Microsoft’ta makine öğrenimi modellerini derlemek, dağıtmak ve yönetmek için kullanılabilecek birçok seçenek vardır. 
* SQL Server Machine Learning Hizmetleri
* Microsoft Machine Learning Sunucusu
* Azure Veri Bilimi Sanal Makinesi
* HDInsight'ta Spark MLLib
* Batch AI Eğitim Hizmeti
* Microsoft Bilişsel Araç Seti (CNTK)
* Azure Bilişsel Hizmetler


## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Hizmetleri
[SQL Server Microsoft Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) R veya Python kullanarak makine öğrenimi modelleri çalıştırmanızı, eğitmenizi ve dağıtmanızı sağlar. Şirket içindeki ve SQL Server veritabanlarındaki verileri kullanabilirsiniz. 

Şirket içinde veya Microsoft SQL Server üzerinde model eğitmeniz veya dağıtmanız gerekiyorsa Microsoft Machine Learning Hizmetleri'ni kullanın. Machine Learning Hizmetleri ile derlenen modeller Azure Machine Learning Model Yönetimi kullanılarak dağıtılabilir. 

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu 
[Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) R ve Python işlemlerinin paralel ve dağıtılmış iş yüklerini barındırmak ve yönetmek için kullanılan kurumsal sunucudur. Microsoft Machine Learning Sunucusu Linux, Windows, Hadoop ve Apache Spark üzerinde çalışır. [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) ile de kullanılabilir. [Microsoft Machine Learning paketleri](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) kullanılarak derlenmiş çözümler için bir yürütme altyapısı sağlar ve açık kaynak R ve Python çözümlerini aşağıdaki senaryolar için sunulan destekle genişletir:

- Yüksek performanslı analiz
- istatistiksel analiz
- Makine öğrenimi
- Çok büyük veri kümeleri

Sunucuyla birlikte yüklenen özel paketler sayesinde ek işlevler sağlanır. Geliştirme için [Visual Studio için R Araçları](https://www.visualstudio.com/vs/rtvs/) ve [Visual Studio için Python Araçları](https://www.visualstudio.com/vs/python/) gibi IDE'leri kullanabilirsiniz.

Aşağıdaki durumda Microsoft Machine Learning Sunucusu'nu kullanın:

- R ve Python ile derlenen modelleri bir sunucuda derlemek ve dağıtmak için
- R ve Python eğitimini bir Hadoop veya Spark kümesinde ölçekli olarak dağıtmak için

## <a name="azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi
[Veri Bilimi Sanal Makinesi](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) veri bilimi için Microsoft Azure bulutunda derlenmiş olan özel bir sanal makine ortamıdır. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. Veri Bilimi Sanal Makinesi, Windows Server 2016 ve 2012, Ubuntu 16.04 LTS ve OpenLogic CentOS 7.4 tabanlı dağıtımların yüklü olduğu Linux sanal makinesi ile kullanılabilir. 

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

## <a name="microsoft-cognitive-toolkit-cntk"></a>Microsoft Bilişsel Araç Seti (CNTK)
[Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) sinir ağlarını yönlü grafik üzerinde işlem adımları olarak tanımlayan birleşik derin öğrenme araç setidir. Bu yönlü grafikte yaprak düğümler giriş değerlerini veya ağ parametrelerini, diğer düğümler ise girişlerle matris işlemlerini temsil eder. Bilişsel Araç Seti akış iletme DNN'leri, kıvrımlı ağlar (CNN'ler) ve yinelenen ağlar (RNN'ler/LSTM'ler) gibi popüler model türlerini kolayca gerçekleştirmenizi ve birleştirmenizi sağlar. Stokastik aşama azaltma (SGD, hata geri yayılımı) öğrenmeyi birden fazla GPU ve sunucu üzerinde otomatik farklılaştırma ve paralelleştirme ile uygulamaya alır.

Bilişsel Araç Setini derin öğrenmeyi kullanarak bir model derlemek istediğinizde kullanın.  Bilişsel Araç Seti önceki hizmetlerin herhangi birinde kullanılabilir.

## <a name="azure-cognitive-services"></a>Azure Bilişsel Hizmetler
[Azure Bilişsel Hizmetler](https://docs.microsoft.com/azure/#pivot=products&panel=ai) doğal iletişim yöntemlerini kullanmanızı sağlayan 30 API'lik bir settir. Bu API'ler birkaç satırlık kodlarla uygulamalarınızın görmesini, duymasını, konuşmasını, anlamasını ve ihtiyaçları yorumlamasını sağlar. Uygulamalarınıza aşağıdaki gibi akıllı özellikleri kolayca ekleyebilirsiniz: 

- Duygu ve düşünceleri algılama
- Görme ve konuşma Tanıma
- Language understanding (LUIS)
- Bilgi ve arama

Bilişsel Hizmetler farklı cihaz ve platformlarda uygulama geliştirmenizi sağlar. API'ler sürekli olarak geliştirilir ve kolayca ayarlanabilir. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](overview-what-is-azure-ml.md)’e genel bakışı okuyun.
