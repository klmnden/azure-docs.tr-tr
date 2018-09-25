---
title: MMLSpark Machine Learning'in nasıl kullanılacağını | Microsoft Docs
description: Azure Machine Learning ile MMLSpark Kitaplığı Kullanma Kılavuzu.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 6167e10219791466ca275ff02cc051e3227634e8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980058"
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning kitaplığı için Apache Spark kullanma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

## <a name="introduction"></a>Giriş

[Apache Spark için Microsoft Machine Learning Kitaplığı](https://github.com/Azure/mmlspark) (MMLSpark) kolayca araçları, büyük veri kümeleri için ölçeklenebilir makine öğrenimi modelleri oluşturma sağlar. SparkML işlem hatları ile tümleştirilmesi içerdiği [Microsoft Bilişsel Araç Seti ](https://github.com/Microsoft/CNTK) ve [OpenCV](http://www.opencv.org/), olanak sağlar: 
 * Giriş ve önceden işlem görüntü verileri
 * Özellik kazandırın görüntü ve metin kullanarak ayrıntılı öğrenme modelleri önceden eğitilmiş
 * Eğitim ve kullanarak örtük özellik kazandırma sayesinde sınıflandırma ve regresyon modellerini puanlamanıza.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda adımlamak için için gerekir:
- [Azure Machine Learning Workbench'i yükleyin](quickstart-installation.md)
- [Azure HDInsight Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Denemenizi Docker kapsayıcısında çalıştırma

MMLSpark denemeleri yerel olarak veya uzaktan VM Docker kapsayıcısında çalıştırma sırasında kullanmak için Azure Machine Learning Workbench yapılandırılır. Bu özellik, kolayca hata ayıklayın ve bunları ölçek kümesinde çalıştırmadan önce PySpark Modellerinizi ile denemeler olanak tanır. 

Bir örnek ile çalışmaya başlamak için yeni bir proje oluşturun ve "MMLSpark üzerinde yetişkinlere yönelik Görselleştirmenizdeki" galeri örneğini seçin. Proje panosundan işlem bağlamı olarak "Docker"'ı seçin ve "Çalıştır" öğesini tıklatın Azure Machine Learning Workbench PySpark programı çalıştırmak için Docker kapsayıcısı oluşturur ve ardından programı yürütür.

Çalıştırma tamamlandıktan sonra Azure Machine Learning Workbench'i çalıştırma geçmişi görünümünde sonuçlarını görüntüleyebilirsiniz.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark kümesinde MMLSpark yükleyin.

Bu ve aşağıdaki adımı tamamlamak için öncelikle gereken [bir Azure HDInsight Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Denemenizi çalıştırdığınızda varsayılan olarak, Azure Machine Learning Workbench kümenizde MMLSpark paketi yükler. Bu davranışını denetlemek ve diğer Spark paketleri adlı bir dosya düzenleyerek yükleme _aml_config/spark_dependencies.yml_ proje klasörünüzde.

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
    version: "0.9.9"
```

MMLSpark doğrudan, HDInsight Spark kümesi kullanarak da yükleyebilirsiniz [betik eylemi](https://github.com/Azure/mmlspark#hdinsight).

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>İşlem hedef olarak Azure HDInsight Spark kümesi ayarlama

"Dosya" menüsüne giderek Azure Machine Learning Workbench CLI penceresi açın ve "komut istemini Aç" tıklayın

CLI penceresine aşağıdaki komutları çalıştırın:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

Şimdi kümeyi proje için hedef işlem olarak kullanılabilir.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark kümesi üzerinde çalıştırma denemesi.

"MMLSpark üzerinde yetişkinlere yönelik Görselleştirmenizdeki" örnek proje Panosu'na geri dönün. Kümenizi işlem hedefi seçin ve Çalıştır'a tıklayın.

Azure Machine Learning Workbench, kümeye spark işi gönderir. İlerleme durumunu izleyebilir ve sonuçları çalıştırma geçmişini görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar
MMLSpark kitaplığı ve örnek hakkında daha fazla bilgi için bkz: [MMLSpark GitHub deposu](https://github.com/Azure/mmlspark)

*Apache®, Apache Spark ve Spark® kayıtlı ticari markaları veya ticari Amerika Birleşik Devletleri ve/veya diğer ülkelerde Apache Software Foundation'ın olan.*
