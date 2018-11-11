---
title: Microsoft Bilişsel araç seti ile ayrıntılı öğrenme için Azure HDInsight Spark
description: Bir Azure HDInsight Spark kümesinde Spark Python API'si kullanarak bir veri kümesi için Microsoft Bilişsel araç seti, ayrıntılı öğrenme eğitilen bir modelin nasıl uygulanabileceğini öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 4bcf6df37e7341baf227b9c77b718a955526823d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010942"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Microsoft Bilişsel Araç Seti derin öğrenme modeli Azure HDInsight Spark kümesi ile kullanma

Bu makalede, aşağıdaki adımları uygulayın.

1. Bir Azure HDInsight Spark kümelerinde Microsoft Bilişsel Araç Seti yüklemek için özel bir betik çalıştırın.

2. Jupyter Not Defteri kullanarak Spark kümesi kullanarak bir Azure Blob Depolama hesabı dosyaların eğitilen bir Microsoft Bilişsel araç seti, ayrıntılı öğrenme modeli uygulamak nasıl görmek için yükleyin [Spark Python API'si (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).

* **Azure HDInsight Spark kümesi**. Bu makale için bir Spark 2.0 kümesi oluşturun. Yönergeler için [Azure HDInsight Apache Spark kümesi](apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Bu çözümü nasıl akıyor?

Bu çözüm, bu makalede ve bu öğreticinin bir parçası olarak yüklemek bir Jupyter not defteri arasında bölünür. Bu makalede, aşağıdaki adımları tamamlayın:

* Betik eylemi, Microsoft Bilişsel Araç Seti ve Python paketlerini yüklemek için bir HDInsight Spark kümesi üzerinde çalıştırın.
* Çözümü, HDInsight Spark kümesine çalıştıran Jupyter not defterini karşıya yükleyin.

Jupyter not defterine aşağıdaki kalan adımları ele alınmaktadır.

- Bir Spark Resiliant Dağıtılmış veri kümesi veya RDD yük örnek görüntüleri
   - Modülleri yükleme ve hazır ayarlarını tanımlama
   - Veri kümesi üzerinde Spark kümesi için bir yerel olarak indir
   - Veri kümesi bir RDD Dönüştür
- Eğitilen bir Bilişsel Araç Seti modeli kullanarak görüntüleri Puanlama
   - Spark kümesine eğitim Bilişsel Araç Seti modeli indirin
   - Alt düğümler tarafından kullanılacak işlevleri tanımlayın
   - Görüntüleri çalışan düğümlerinde Puanlama
   - Model doğruluğu değerlendir


## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft Bilişsel araç setini yükleme

Microsoft Bilişsel araç seti, script action kullanarak Spark kümesinde yükleyebilirsiniz. Betik eylemi, varsayılan olarak kullanılabilir değil küme üzerinde bileşenleri yüklemek için özel betikleri kullanılır. HDInsight .NET SDK kullanarak veya Azure PowerShell kullanarak özel betik Azure Portal'dan kullanabilirsiniz. Küme oluşturma veya kümeye çalışır duruma geldikten sonra bir parçası olarak ya da araç setini yükleme betiğini de kullanabilirsiniz. 

Bu makalede, Küme oluşturulduktan sonra Araç Seti'ni yüklemek için portalı kullanın. Özel betik çalıştırmak diğer yolları için bkz. [özelleştirme HDInsight kümelerini betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Azure Portal’ı kullanma

Betik eylemi çalıştırmak için Azure Portal'ı kullanma hakkında yönergeler için bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](../hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Microsoft Bilişsel Araç Seti yüklemek için aşağıdaki girişleri sağladığınızdan emin olun.

* Betik eylem adı için bir değer sağlayın.

* İçin **Bash betiği URI'si**, girin `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Betik yalnızca baş ve çalışan düğümleri üzerinde çalışmak ve tüm diğer onay kutularını temizleyin emin olun.

* **Oluştur**’a tıklayın.

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark kümesine Jupyter not defterini karşıya yükleme

Microsoft Bilişsel araç seti ile Azure HDInsight Spark kümesi kullanmak için Jupyter not defteri yük **CNTK_model_scoring_on_Spark_walkthrough.ipynb** Azure HDInsight Spark kümesine. Bu Not github'da kullanılabilir [ https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration ](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. GitHub deposunu kopyalayın [ https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration ](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Kopyalama yönergeleri için bkz. [bir depoyu kopyalama](https://help.github.com/articles/cloning-a-repository/).

2. Azure Portalı'ndan zaten hazırlandı tıklayın Spark kümesi dikey penceresini açın **küme Panosu**ve ardından **Jupyter not defteri**.

    Jupyter not defteri URL'sine giderek da başlatabilirsiniz `https://<clustername>.azurehdinsight.net/jupyter/`. Değiştirin \<clustername >, HDInsight kümenizin adıyla.

3. Jupyter not defterinden tıklayın **karşıya** sağ üst köşe ve ardından GitHub deposunu kopyaladığınız konuma gidin.

    ![Azure HDInsight Spark kümesine Jupyter not defterini karşıya](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Azure HDInsight Spark kümesine karşıya Jupyter not defteri")

4. Tıklayın **karşıya** yeniden.

5. Not defterini karşıya yüklendikten sonra not defteri adına tıklayın ve ardından bir veri kümesi yükleyin ve hızlı başlangıç öğreticisinde konusunda not kendisini yönergeleri izleyin.

## <a name="see-also"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
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
