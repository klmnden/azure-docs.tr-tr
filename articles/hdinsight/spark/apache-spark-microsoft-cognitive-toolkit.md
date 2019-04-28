---
title: Microsoft Bilişsel araç seti ile ayrıntılı öğrenme için Azure HDInsight Spark
description: Bir Azure HDInsight Spark kümesinde Spark Python API'si kullanarak bir veri kümesi için Microsoft Bilişsel araç seti, ayrıntılı öğrenme eğitilen bir modelin nasıl uygulanabileceğini öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 3462255311eaa6e418f97de5da598eb985b2a935
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097296"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Microsoft Bilişsel Araç Seti derin öğrenme modeli Azure HDInsight Spark kümesi ile kullanma

Bu makalede, aşağıdaki adımları uygulayın.

1. Yüklemek için özel betik çalıştırma [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) bir Azure HDInsight Spark kümesi üzerinde.

2. Karşıya bir [Jupyter not defteri](https://jupyter.org/) için [Apache Spark](https://spark.apache.org/) eğitilmiş bir Microsoft Bilişsel araç seti, ayrıntılı öğrenme modelini kullanarak bir Azure Blob Depolama hesabı dosyaların uygulamak nasıl görmek için küme [ Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).

* **Azure HDInsight Spark kümesi**. Bu makale için bir Spark 2.0 kümesi oluşturun. Yönergeler için [Azure HDInsight Apache Spark kümesi](apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Bu çözümü nasıl akıyor?

Bu çözüm, bu makalede ve bu öğreticinin bir parçası olarak yüklemek bir Jupyter not defteri arasında bölünür. Bu makalede, aşağıdaki adımları tamamlayın:

* Betik eylemi, Microsoft Bilişsel Araç Seti ve Python paketlerini yüklemek için bir HDInsight Spark kümesi üzerinde çalıştırın.
* Çözümü, HDInsight Spark kümesine çalıştıran Jupyter not defterini karşıya yükleyin.

Jupyter not defterine aşağıdaki kalan adımları ele alınmaktadır.

- Örnek görüntüleri bir Spark Resiliant Dağıtılmış veri kümesi veya RDD içine yükleyin.
   - Modülleri yükleme ve hazır tanımlayın.
   - Veri kümesi üzerinde Spark kümesi için bir yerel olarak indirin.
   - Veri kümesi bir RDD dönüştürün.
- Eğitilen bir Bilişsel Araç Seti modeli kullanarak görüntüleri puan.
   - Spark kümesine eğitim Bilişsel Araç Seti modeli indirin.
   - Alt düğümler tarafından kullanılacak işlevleri tanımlayın.
   - Görüntüleri çalışan düğümlerinde puan.
   - Model doğruluğu değerlendirin.


## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft Bilişsel araç setini yükleme

Microsoft Bilişsel araç seti, script action kullanarak Spark kümesinde yükleyebilirsiniz. Betik eylemi, varsayılan olarak kullanılabilir değil küme üzerinde bileşenleri yüklemek için özel betikleri kullanılır. HDInsight .NET SDK kullanarak veya Azure PowerShell kullanarak özel betik Azure portalından kullanabilirsiniz. Küme oluşturma veya kümeye çalışır duruma geldikten sonra bir parçası olarak ya da araç setini yükleme betiğini de kullanabilirsiniz. 

Bu makalede, Küme oluşturulduktan sonra Araç Seti'ni yüklemek için portalı kullanın. Özel betik çalıştırmak diğer yolları için bkz. [özelleştirme HDInsight kümelerini betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Betik eylemi çalıştırmak için Azure portalını kullanma hakkında yönergeler için bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Microsoft Bilişsel Araç Seti yüklemek için aşağıdaki girişleri sağladığınızdan emin olun.

* Betik eylem adı için bir değer sağlayın.

* İçin **Bash betiği URI'si**, girin `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Betik yalnızca baş ve çalışan düğümleri üzerinde çalışmak ve tüm diğer onay kutularını temizleyin emin olun.

* **Oluştur**’a tıklayın.

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark kümesine Jupyter not defterini karşıya yükleme

Microsoft Bilişsel araç seti ile Azure HDInsight Spark kümesi kullanmak için Jupyter not defteri yük **CNTK_model_scoring_on_Spark_walkthrough.ipynb** Azure HDInsight Spark kümesine. Bu Not github'da kullanılabilir [ https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration ](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. GitHub deposunu kopyalayın [ https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration ](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Kopyalama yönergeleri için bkz. [bir depoyu kopyalama](https://help.github.com/articles/cloning-a-repository/).

2. Azure portalından zaten hazırlandı tıklayın Spark kümesi dikey penceresini açın **küme Panosu**ve ardından **Jupyter not defteri**.

    Jupyter not defteri URL'sine giderek da başlatabilirsiniz `https://<clustername>.azurehdinsight.net/jupyter/`. Değiştirin \<clustername >, HDInsight kümenizin adıyla.

3. Jupyter not defterinden tıklayın **karşıya** sağ üst köşe ve ardından GitHub deposunu kopyaladığınız konuma gidin.

    ![Azure HDInsight Spark kümesine Jupyter not defterini karşıya](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Azure HDInsight Spark kümesine karşıya Jupyter not defteri")

4. Tıklayın **karşıya** yeniden.

5. Not defterini karşıya yüklendikten sonra not defteri adına tıklayın ve ardından bir veri kümesi yükleyin ve hızlı başlangıç öğreticisinde konusunda not kendisini yönergeleri izleyin.

## <a name="see-also"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight Apache Spark'ı kullanarak application Insight telemetri verilerinin analizi](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
