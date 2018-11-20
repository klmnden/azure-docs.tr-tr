---
title: Betik eylemi - Azure HDInsight üzerinde Jupyter paketlerle Python yükleme
description: Dış python paketlerini kullanmak için adım adım yönergeleri HDInsight Spark kümeleri ile Jupyter not defterleri kullanılabilir yapılandırmak üzere betik eylemi kullanın.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 46ce112f420a6534140d293332e7ca7efc2def94
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975695"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>HDInsight üzerinde Apache Spark kümeleri Jupyter not defterleri için dış Python paketlerini yüklemek üzere betik eylemi kullanın
> [!div class="op_single_selector"]
> * [Hücre Sihri kullanarak](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Betik eylemi kullanarak](apache-spark-python-package-installation.md)
>
>

Betik eylemleri, harici, topluluk tarafından katkıda bulunulan kullanmak için (Linux) HDInsight üzerinde Apache Spark kümesi yapılandırmak için kullanmayı öğrenin **python** kümede olmayan paketleri dahil kullanıma hazır.

> [!NOTE]
> Jupyter Not Defteri kullanarak da yapılandırabilirsiniz `%%configure` magic dış paketleri kullanma. Yönergeler için [, HDInsight üzerinde Apache Spark kümeleri Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Arama yapabilirsiniz [paket dizinini](https://pypi.python.org/pypi) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketler listesini alabilirsiniz. Örneğin, kullanılabilir hale paketleri yükleyebilirsiniz [conda oluşturmasına](https://conda-forge.org/feedstocks/).

Bu makalede, nasıl yükleyeceğinizi öğrenin [TensorFlow](https://www.tensorflow.org/) kümenizde betik eylemi kullanarak paket ve Jupyter notebook örnek olarak kullanın.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > HDInsight Linux üzerinde bir Spark kümesi zaten yoksa, betik eylemleri, küme oluşturma sırasında çalıştırabilirsiniz. Belgeleri ziyaret [özel betik eylemlerini kullanmak nasıl](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 
   
   ## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynaklı yazılım desteği

Microsoft Azure HDInsight hizmeti, açık kaynak teknolojilerini Hadoop geçici olarak biçimlendirilmiş bir kaynak ekosisteminiz kullanır. Microsoft Azure için açık kaynak teknolojilerini genel düzeyde destek sağlar. Daha fazla bilgi için **destek kapsamı** bölümünü [Azure desteği SSS Web sitesine](https://azure.microsoft.com/support/faq/). HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

HDInsight hizmetinde kullanılabilir açık kaynak bileşenleri iki tür vardır:

* **Yerleşik bileşenlerini** -bu bileşenler HDInsight kümelerinde önceden yüklü olan ve kümeyi temel işlevlerini sağlar. Örneğin, YARN ResourceManager, Hive sorgu dili (HiveQL) ve Mahout kitaplığı, bu kategoriye aittir. Tam küme bileşenleri listesini kullanılabilir [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Özel bileşenler** -, kümenin bir kullanıcı olarak yükleyebilir veya herhangi bir bileşeni Topluluğu'nda kullanılabilir veya sizin tarafınızdan oluşturulan iş yükünüzü kullanın.

> [!WARNING]
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir. Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Microsoft desteği sorunu çözmek mümkün olabilir veya bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak için isteyebilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Jupyter not defterleri ile dış paketleri kullanma

1. [Azure Portal](https://portal.azure.com/)’daki başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   

2. Spark kümesi dikey penceresinden tıklayın **betik eylemleri** sol bölmeden. Betik türünü "Özel" kullanın ve betik eylemi için bir kolay ad girin. Betiği çalıştırın **baş ve çalışan düğümleri** ve parametreleri alanı boş bırakın. Bash betiği başvurulabilir: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh belgeleri ziyaret [özel betik eylemlerini kullanmak nasıl](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Kümedeki yüklemeler iki python var. Spark Anaconda python yükleme konumunda bulunan kullanacağı `/usr/bin/anaconda/bin`. Bu yükleme, özel eylemleri başvuru `/usr/bin/anaconda/bin/pip` ve `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Bir PySpark Jupyter not defterini açın

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Yeni bir Jupyter not defteri oluşturma")

4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Not defteri adını belirtme")

5. Artık olacak `import tensorflow` ve bir Merhaba Dünya örneği çalıştırın. 

    Kodu kopyalayın:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    Sonucu şöyle görünür:
    
    ![TensorFlow kod yürütme](./media/apache-spark-python-package-installation/execution.png "yürütme TensorFlow kod")

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [HDInsight üzerinde Apache Spark kümeleri Jupyter not defterlerinde ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
