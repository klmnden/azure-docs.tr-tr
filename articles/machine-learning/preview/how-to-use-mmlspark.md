---
title: MMLSpark Machine Learning kullanma | Microsoft Docs
description: "Azure Machine Learning ile MMLSpark kitaplığını kullanma yol."
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 9ba2cbe1d6ce4b2010decb8bff4fa46faf0852b3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning kitaplığı için Apache Spark kullanma

## <a name="introduction"></a>Giriş

[Apache Spark için Microsoft Machine Learning Kitaplığı](https://github.com/Azure/mmlspark) (MMLSpark) sağlar kolayca olanak sağlayan araçlar büyük veri kümeleri için ölçeklenebilir makine öğrenimi modellerini oluşturun. SparkML işlem hatları ile tümleştirilmesi içeren [Microsoft Bilişsel Araç Seti ](https://github.com/Microsoft/CNTK) ve [OpenCV](http://www.opencv.org/), olanak sağlar: 
 * Giriş ve önceden işlem görüntü verileri
 * Featurize görüntüler ve metinler modelleri öğrenme önceden eğitilen derin kullanma
 * Eğitmek ve örtük featurization kullanarak sınıflandırma ve regresyon modeli Puanlama.

## <a name="prerequisites"></a>Ön koşullar

Nasıl yapılır bu kılavuzu adım için aktarmanız gerekir:
- [Azure Machine Learning çalışma ekranı yükleyin](quickstart-installation.md)
- [Azure Hdınsight Spark kümesi](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Docker kapsayıcısı denemenizi çalıştırın

Azure Machine Learning çalışma ekranı denemeler Docker kapsayıcısı içinde yerel olarak veya uzaktan VM çalıştırdığınızda MMLSpark kullanacak şekilde yapılandırılır. Bu özellik, kolay hata ayıklama ve bunları bir kümede ölçekte çalıştırmadan önce PySpark modelleriyle denemeler olanak sağlar. 

Örnek kullanmaya başlamak için yeni bir proje oluşturun ve "MMLSpark üzerinde yetişkin Census" galeri örneği seçin. Proje Panosu işlem bağlamı olarak "Docker" seçin ve "Çalıştır" öğesini tıklatın Azure Machine Learning çalışma ekranı PySpark programı çalıştırmak için Docker kapsayıcısı oluşturur ve ardından program yürütür.

Çalıştır tamamlandıktan sonra Azure Machine Learning çalışma ekranı çalıştırma geçmişi görünümünde sonuçlarını görüntüleyebilirsiniz.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Azure Hdınsight Spark kümesinde MMLSpark yükleyin.

Bu ve aşağıdaki adımı tamamlamak için öncelikle gerekir [Azure Hdınsight Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Denemenizi çalıştırdığınızda, varsayılan olarak, Azure Machine Learning çalışma ekranı kümenizde MMLSpark paketi yükler. Bu davranışı denetlemek ve adlı bir dosya düzenleyerek diğer Spark paketleri yüklemeniz _aml_config/spark_dependencies.yml_ proje klasörünüzdeki.

```
# Spark configuration properties.
configuration:
  "spark.app.name": "Azure ML Experiment"
  "spark.yarn.maxAppAttempts": 1

repositories:
  - "https://mmlspark.azureedge.net/maven"
packages:
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.7.9"
```

Ayrıca, doğrudan, Hdınsight Spark küme kullanarak MMLSpark yükleyebilirsiniz [betik eylemi](https://github.com/Azure/mmlspark#hdinsight).

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Azure Hdınsight Spark küme işlem hedefi olarak ayarlama

"Dosya" menüsüne giderek Azure Machine Learning çalışma ekranından CLI penceresini açın ve "komut istemini açın"

CLI penceresinde aşağıdaki komutları çalıştırın:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

Şimdi küme işlem projesi için hedef olarak kullanılabilir.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Azure Hdınsight Spark kümesinde çalışma deneme.

"MMLSpark üzerinde yetişkin Census" örnek proje panoya geri dönün. Kümenizi işlem hedefi olarak seçin ve Çalıştır'ı tıklatın.

Azure Machine Learning çalışma ekranı kümeye spark iş gönderir. İlerlemeyi izlemek ve çalıştırma geçmişi görünümünde sonuçları görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar
MMLSpark kitaplığı ve örnekler hakkında daha fazla bilgi için bkz: [MMLSpark GitHub deposunu](https://github.com/Azure/mmlspark)

*Apache®, Apache Spark ve Spark® kayıtlı ticari markaları ya da Apache yazılım Foundation'da Amerika Birleşik Devletleri ve/veya diğer ülkelerdeki ticari markalarıdır.*
