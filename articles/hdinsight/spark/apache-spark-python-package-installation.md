---
title: Betik eylemi - Azure HDInsight üzerinde Jupyter paketlerle Python yükleme
description: Dış python paketlerini kullanmak için adım adım yönergeleri HDInsight Spark kümeleri ile Jupyter not defterleri kullanılabilir yapılandırmak üzere betik eylemi kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: c07326cc3a4334f1873eef2dc23da05156a93577
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64574652"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>HDInsight üzerinde Apache Spark kümeleri Jupyter not defterleri için dış Python paketlerini yüklemek üzere betik eylemi kullanın
> [!div class="op_single_selector"]
> * [Hücre Sihri kullanarak](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Betik eylemi kullanarak](apache-spark-python-package-installation.md)

Betik eylemleri yapılandırmak için kullanmayı öğrenin bir [Apache Spark](https://spark.apache.org/) harici, topluluk tarafından katkıda bulunulan kullanmak için HDInsight kümesinde **python** kümede olmayan paketleri dahil kullanıma hazır.

> [!NOTE]  
> Jupyter Not Defteri kullanarak da yapılandırabilirsiniz `%%configure` magic dış paketleri kullanma. Yönergeler için [, HDInsight üzerinde Apache Spark kümeleri Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md).

Arama yapabilirsiniz [paket dizinini](https://pypi.python.org/pypi) kullanılabilir paketler tam listesi için. Ayrıca, diğer kaynaklardan kullanılabilir paketler listesini alabilirsiniz. Örneğin, kullanılabilir hale paketleri yükleyebilirsiniz [conda oluşturmasına](https://conda-forge.org/feedstocks/).

Bu makalede, nasıl yükleyeceğinizi öğrenin [TensorFlow](https://www.tensorflow.org/) kümenizde betik eylemi kullanarak paket ve Jupyter notebook örnek olarak kullanın.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]  
   > HDInsight Linux üzerinde bir Spark kümesi zaten yoksa, betik eylemleri, küme oluşturma sırasında çalıştırabilirsiniz. Belgeleri ziyaret [özel betik eylemlerini kullanmak nasıl](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   
## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynaklı yazılım desteği

Microsoft Azure HDInsight hizmeti, açık kaynak teknolojilerini Apache Hadoop geçici olarak biçimlendirilmiş bir kaynak ekosisteminiz kullanır. Microsoft Azure için açık kaynak teknolojilerini genel düzeyde destek sağlar. Daha fazla bilgi için **destek kapsamı** bölümünü [Azure desteği SSS Web sitesine](https://azure.microsoft.com/support/faq/). HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

HDInsight hizmetinde kullanılabilir açık kaynak bileşenleri iki tür vardır:

* **Yerleşik bileşenlerini** -bu bileşenler HDInsight kümelerinde önceden yüklü olan ve kümeyi temel işlevlerini sağlar. Örneğin, Apache Hadoop YARN ResourceManager, Apache Hive sorgu dili (HiveQL) ve Mahout kitaplığı, bu kategoriye aittir. Tam küme bileşenleri listesini kullanılabilir [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerindeki yenilikler](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Özel bileşenler** -, kümenin bir kullanıcı olarak yükleyebilir veya herhangi bir bileşeni Topluluğu'nda kullanılabilir veya sizin tarafınızdan oluşturulan iş yükünüzü kullanın.

> [!IMPORTANT]   
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir. Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Microsoft desteği sorunu çözmek mümkün olabilir veya bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak için isteyebilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Apache projeleri proje siteleri de [ https://apache.org ](https://apache.org), örneğin: [Hadoop](https://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Jupyter not defterleri ile dış paketleri kullanma

1. Gelen [Azure portalında](https://portal.azure.com/), kümenize gidin.  

2. Sol bölmeden altında seçili kümenizle **ayarları**seçin **betik eylemlerini**.

3. Seçin **+ gönderme yeni**.

4. İçin aşağıdaki değerleri girin **betik eylemini Gönder** penceresi:  


    |Parametre | Değer |
    |---|---|
    |Betik türü | Seçin **- özel** aşağı açılan listeden.|
    |Ad |Girin `tensorflow` metin kutusuna.|
    |Bash betiği URI'si |Girin `https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh` metin kutusuna. |
    |Düğüm türleri | Seçin **baş**, ve **çalışan** onay kutuları. |

    `tensorflowinstall.sh` aşağıdakileri içerir:

    ```bash
    #!/usr/bin/env bash
    /usr/bin/anaconda/bin/conda install --yes tensorflow
    ```

5. **Oluştur**’u seçin.  Belgeleri ziyaret [özel betik eylemlerini kullanmak nasıl](../hdinsight-hadoop-customize-cluster-linux.md).

6. Betik için bekleyin.  **Betik eylemlerini** bölmesinde durum **geçerli küme işlemi tamamlandıktan sonra yeni betik eylemleri gönderilebilir** betiği yürütülürken.  Ambari Arabiriminden bir ilerleme çubuğu görüntülenebilir **arka plan işlemleri** penceresi.

7. Bir PySpark Jupyter not defterini açın.  Bkz: [Spark HDInsight üzerinde Create a Jupyter notebook](./apache-spark-jupyter-notebook-kernels.md#create-a-jupyter-notebook-on-spark-hdinsight) adımlar için.

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Yeni bir Jupyter not defteri oluşturma")

8. Artık olacak `import tensorflow` ve bir Merhaba Dünya örneği çalıştırın. Aşağıdaki kodu girin:

    ```
    import tensorflow as tf
    hello = tf.constant('Hello, TensorFlow!')
    sess = tf.Session()
    print(sess.run(hello))
    ```

    Sonucu şöyle görünür:
    
    ![TensorFlow kod yürütme](./media/apache-spark-python-package-installation/execution.png "yürütme TensorFlow kod")

> [!NOTE]  
> Kümedeki yüklemeler iki python var. Spark Anaconda python yükleme konumunda bulunan kullanacağı `/usr/bin/anaconda/bin` ve Python 2.7 ortam varsayılan ayarlanır. PySpark3 çekirdek Python 3.x ve yükleme paketlerini kullanmak için yolu kullanmak `conda` bu ortam ve kullanmak için yürütülebilir `-n` ortamını belirtmek için parametre. Örneğin, komut `/usr/bin/anaconda/envs/py35/bin/conda install -c conda-forge ggplot -n py35`, yükler `ggplot` Python 3.5 ortamı kullanarak paket `conda-forge` kanal.

## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [HDInsight üzerinde Apache Spark kümeleri Jupyter not defterlerinde ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
