---
title: "Microsoft Azure Hdınsight Spark derin öğrenme için ile Bilişsel Araç Seti | Microsoft Docs"
description: "Bir Azure Hdınsight Spark kümesinde Spark Python API kullanarak bir veri kümesi Microsoft Bilişsel Araç Seti derin learning modeli nasıl uygulanabilir öğrenin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: 036efd040370a821befbbd57beec24372fd0d204
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Microsoft Azure Hdınsight Spark küme modeliyle öğrenme derin bir Bilişsel araç kullanma

Bu makalede, aşağıdaki adımları uygulayın.

1. Bir Azure Hdınsight Spark kümesinde Microsoft Bilişsel Araç Seti yüklemek için özel bir komut dosyasını çalıştırın.

2. Bir Azure Blob Depolama hesabı kullanarak dosyaları Microsoft Bilişsel Araç Seti derin learning modeli uygulamak nasıl görmek için Spark kümesi Jupyter not defteri karşıya [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).

* **Azure Hdınsight Spark kümesi**. Bu makalede, bir Spark 2.0 kümesi oluşturun. Yönergeler için bkz: [Azure hdınsight'ta Apache Spark oluşturma küme](apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Bu çözümün nasıl akıyor?

Bu çözüm, bu makalede ve bu öğreticinin bir parçası olarak karşıya Jupyter not defteri arasında bölünür. Bu makalede, aşağıdaki adımları tamamlayın:

* Betik eylemi Microsoft Bilişsel Araç Seti ve Python paketlerini yüklemek için bir Hdınsight Spark kümesinde çalıştırın.
* Hdınsight Spark kümesi çözümü çalıştıran Jupyter not defteri karşıya yükleyin.

Aşağıdaki adımları Jupyter not defteri ele alınmıştır.

- Yük örnek görüntülere Spark Resiliant dağıtılmış Dataset ya da RDD
   - Modülleri yüklemek ve hazır tanımlayın
   - Veri kümesi Spark küme üzerinde yerel olarak indir
   - Veri kümesi bir RDD Dönüştür
- Bilişsel Araç Seti modeli kullanarak görüntüleri puan
   - Spark kümesi eğitim Bilişsel Araç Seti modeli indirme
   - Alt düğümler tarafından kullanılacak işlevleri tanımlama
   - Görüntüleri çalışan düğümlerine puan
   - Model doğruluğundan değerlendir


## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti yükleyin

Betik eylemi kullanarak Spark kümesinde Microsoft Bilişsel Araç Seti yükleyebilirsiniz. Betik eylemi özel komut dosyaları varsayılan olarak kullanılabilir değil küme üzerinde bileşenleri yüklemek için kullanır. Hdınsight .NET SDK kullanarak ya da Azure PowerShell kullanarak Azure portalından özel komut dosyası kullanabilirsiniz. Araç Seti ya da küme oluşturma veya kümeye çalışır durumda sonra bir parçası olarak yüklemek için komut dosyasını da kullanabilirsiniz. 

Bu makalede, Küme oluşturulduktan sonra araç seti yüklemek için Portalı'nı kullanın. Özel komut dosyasını çalıştırmak diğer yolları için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Azure Portal’ı kullanma

Betik eylemi çalıştırmak için Azure Portalı'nı kullanma hakkında daha fazla yönerge için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Microsoft Bilişsel Araç Seti yüklemek için aşağıdaki girişleri sağladığınızdan emin olun.

* Betik eylem adı için bir değer girin.

* İçin **betik URI Bash**, girin `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Yalnızca head ve çalışan düğümlerine komut dosyasını çalıştırın ve tüm diğer onay kutularını temizleyin emin olun.

* **Oluştur**’a tıklayın.

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Azure Hdınsight Spark kümesinde Jupyter not defteri karşıya yükle

Microsoft Bilişsel araç seti ile Azure Hdınsight Spark kümesi kullanmak üzere Jupyter not defteri yük **CNTK_model_scoring_on_Spark_walkthrough.ipynb** Azure Hdınsight Spark küme. Bu not defteri github'da kullanılabilir [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. GitHub deposunu kopyalayın [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Kopyalamak yönergeler için bkz: [depo kopyalama](https://help.github.com/articles/cloning-a-repository/).

2. Azure Portalı'ndan zaten sağlanmış tıklatın Spark kümesi dikey penceresini açmak **küme Panosu**ve ardından **Jupyter not defteri**.

    URL'sine giderek Jupyter not defteri başlatabilirsiniz `https://<clustername>.azurehdinsight.net/jupyter/`. Değiştir \<clustername > Hdınsight kümenize adı.

3. Jupyter not defteri tıklatın **karşıya** içinde sağ üst köşe ve GitHub deposunu kopyaladığınız konuma gidin.

    ![Azure Hdınsight Spark kümesinde Jupyter not defteri karşıya](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Azure Hdınsight Spark kümesi için karşıya yükleme Jupyter not defteri")

4. Tıklatın **karşıya** yeniden.

5. Not Defteri karşıya yüklendikten sonra dizüstü bilgisayar adına tıklayın ve veri kümesi yükleme ve öğreticiyi gerçekleştirme hakkında dizüstü kendisini'ndaki yönergeleri izleyin.

## <a name="see-also"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight’ta Spark kullanarak Application Insight telemetri verilerinin analizi](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
