---
title: "Betik eylemi - Azure hdınsight'ta Jupyter ile yükleme Python paketlerini | Microsoft Docs"
description: "Dış python paketlerini kullanmak için adım adım yönergeler betik eylemi kullanarak Hdınsight Spark kümeleri ile Jupyter not defterleri kullanılabilir yapılandırmak."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: nitinme
ms.openlocfilehash: c2921c6d7a0f46322fc4e0b3c84b743ee98e4a4d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Hdınsight'ta Apache Spark kümeleri Jupyter not defterleri için dış Python paketlerini yüklemek için betik eylemi kullanın
> [!div class="op_single_selector"]
> * [Hücre Sihirli kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Betik eylemi kullanarak](apache-spark-python-package-installation.md)
>
>

Dış, topluluk katkıda bulunan kullanmak için (Linux) Hdınsight'ta bir Apache Spark kümesi yapılandırmak için betik eylemleri kullanmayı öğrenin **python** kümede olmayan paketler dahil Giden kutusu.

> [!NOTE]
> Jupyter Not Defteri kullanarak da yapılandırabilirsiniz `%%configure` Sihirli dış paketleri kullanma. Yönergeler için bkz: [hdınsight'ta Apache Spark kümeleri Jupyter not defterlerinde ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Arama yapabilirsiniz [paket dizini](https://pypi.python.org/pypi) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketlerin listesini alabilirsiniz. Örneğin, kullanılabilir hale paketlerini yükleyebilirsiniz [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) veya [conda oluşturmasına](https://conda-forge.github.io/feedstocks.html).

Bu makalede, nasıl yükleneceğini öğreneceksiniz [TensorFlow](https://www.tensorflow.org/) kümenizde betik eylemi kullanarak paketini ve Jupyter not defteri kullanın.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Hdınsight Linux üzerinde Spark kümesi zaten yoksa, küme oluşturma sırasında betik eylemleri çalıştırabilirsiniz. Belgeleri ziyaret [özel betik eylemlerini kullanmak nasıl](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Jupyter not defterleri ile dış paketleri kullanma

1. [Azure Portal](https://portal.azure.com/)’daki başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   

2. Spark kümesi dikey penceresinden tıklatın **betik eylemleri** sol bölmeden. Baş düğümler ve çalışan düğümleri TensorFlow yükler özel eylemini çalıştırın. Bash betik başvurulabilir: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh üzerinde belgeleri ziyaret [özel betik eylemlerini kullanmak nasıl](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Küme yüklemelerde iki python vardır. Spark konumunda bulunan Anaconda python yükleme kullanacağınız `/usr/bin/anaconda/bin`. Bu yükleme, özel eylemleri başvuru `/usr/bin/anaconda/bin/pip` ve `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Bir PySpark Jupyter not defteri açın

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Yeni bir Jupyter not defteri oluşturma")

4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Not defteri adını belirtme")

5. Şimdi olacak `import tensorflow` ve Merhaba Dünya örneği çalıştırın. 

    Kopyalamak için kodu:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    Sonuç şuna benzeyecektir:
    
    ![TensorFlow kod yürütmeyi](./media/apache-spark-python-package-installation/execution.png "yürütme TensorFlow kodu")



## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Hdınsight'ta Apache Spark kümeleri Jupyter not defterlerinde ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
