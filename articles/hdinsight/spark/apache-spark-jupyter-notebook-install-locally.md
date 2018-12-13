---
title: Jupyter yerel olarak yükleyin ve Azure HDInsight Spark bağlanma
description: Jupyter not defterine bilgisayarınıza yerel olarak yükleyin ve bir Apache Spark kümesine bağlanma hakkında bilgi edinin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 92f6bc358fe8cc5ab8f7242d94edc3004eaab4b9
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163387"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter not defterine bilgisayarınıza yükleyin ve HDInsight üzerinde Apache spark'a bağlanma

Bu makalede (Python için) özel PySpark ve Spark Sihirli ile (Scala için) Apache Spark defterleri ile Jupyter Not Defteri, yükleme ve not defterini bir HDInsight kümesine bağlanma hakkında bilgi edinin. Birkaç Jupyter yerel bilgisayarınıza yüklemek için neden olabilir ve bazı zorluklar de olabilir. Hakkında daha fazla bilgi için bu konudaki [neden yüklediğimde Jupyter bilgisayarımda](#why-should-i-install-jupyter-on-my-computer) bu makalenin sonunda.

Jupyter ve Spark Sihirli bilgisayarınızda yüklemeyle ilgili üç önemli adım vardır.

* Jupyter not defteri yükleyin
* PySpark ve Spark çekirdekler Spark ile Sihirli yükleyin
* HDInsight Spark kümesine erişmek için Spark Sihirli yapılandırın

Özel çekirdekler ve HDInsight kümesi ile Jupyter not defterleri için Spark Sihirli hakkında daha fazla bilgi için bkz. [için Jupyter not defterlerinde kullanılabilen çekirdekler Apache Spark Linux kümeleri HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Önkoşullar
Burada listelenen önkoşulları Jupyter yüklemek için değildir. Bu, Not defterini yüklendikten sonra Jupyter not defterini bir HDInsight kümesine bağlamak için kullanılır.

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter not defterine bilgisayarınıza yükleyin.

Jupyter not defterleri yükleyebilmek için önce Python yüklemeniz gerekir. Hem Python hem de Jupyter olarak kullanılabilir parçası [Anaconda dağıtım](https://www.anaconda.com/download/). Anaconda yüklediğinizde, bir Python dağıtımını yükleyin. Anaconda yüklendikten sonra Jupyter yükleme uygun komutları çalıştırarak ekleyin.

1. İndirme [Anaconda yükleyici](https://www.anaconda.com/download/) platform ve Kurulumu çalıştırın. Kurulum Sihirbazı çalıştırırken, yol değişkeninize Anaconda ekleme seçeneği seçtiğinizden emin olun.
1. Jupyter yüklemek için aşağıdaki komutu çalıştırın.

        conda install jupyter

    Jupyter yükleme hakkında daha fazla bilgi için bkz. [Anaconda kullanarak Jupyter yükleme](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Çekirdekler ve Spark Sihirli yükleyin

Spark Sihirli yükleme hakkında yönergeler için PySpark ve Spark çekirdekler yükleme yönergelerini izleyin [sparkmagic belgeleri](https://github.com/jupyter-incubator/sparkmagic#installation) GitHub üzerinde. İlk adımı Spark Sihirli belgelerinde Spark Sihirli yüklemenizi ister. Bu ilk adım bağlantıda bağlanacağınız HDInsight küme sürümüne bağlı olarak aşağıdaki komutu değiştirin. Bundan sonra Spark Sihirli belgelerinde kalan adımları izleyin. Farklı çekirdekler yüklemek istiyorsanız, Spark Sihirli yükleme yönergeleri bölümünde 3. adım gerçekleştirmeniz gerekir.

* Kümeleri v3.4 için yürüterek sparkmagic 0.2.3 yükleyin. `pip install sparkmagic==0.2.3`

* Kümeleri v3.5 ve v3.6 sparkmagic 0.11.2 yürüterek yükleyin `pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark Sihirli HDInsight Spark kümesine bağlanmak için yapılandırın

Bu bölümde Azure HDInsight oluşturmuş olmanız gerekir bir Apache Spark kümesine bağlanmak için daha önce yüklediğimiz Spark Sihirli yapılandırın.

1. Jupyter yapılandırma bilgileri genellikle kullanıcıların giriş dizininde depolanır. Giriş dizininize herhangi bir işletim sistemi platformda bulmak için aşağıdaki komutları yazın.

    Python Kabuğu'nu başlatın Bir komut penceresinde aşağıdaki komutu yazın:

        python

    Python Kabuğu'na giriş dizini bulmak için aşağıdaki komutu girin.

        import os
        print(os.path.expanduser('~'))

1. Giriş dizinine gidin ve adlı bir klasör oluşturun **.sparkmagic** zaten yoksa.
1. Adlı bir dosya klasör içindeki oluşturun **config.json** ve içine aşağıdaki JSON kod parçacığını ekleyin.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

1. Yedek **{USERNAME}**, **{CLUSTERDNSNAME}**, ve **{BASE64ENCODEDPASSWORD}** uygun değerlere sahip. Yardımcı programları en sevdiğiniz programlama dili veya çevrimiçi bir dizi gerçek parolanızı base64 olarak kodlanmış bir parola oluşturmak için kullanabilirsiniz.

1. Doğru sinyal ayarlarında yapılandırdığınız `config.json`. Bu ayarlar, aynı düzeyde eklemelisiniz `kernel_python_credentials` ve `kernel_scala_credentials` kod parçacıkları, eklenen önceki. Örneği nasıl ve nerede sinyal ayarları eklemek için bu bkz [örnek config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * İçin `sparkmagic 0.2.3` (v3.4 kümeleri) içerir:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * İçin `sparkmagic 0.11.2` (kümeleri v3.5 ve v3.6) içerir:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Oturumlarının sızdırılmaz emin olmak için sinyaller gönderilir. Bir bilgisayar uyku moduna geçer veya kapatıldığında, sinyal gönderilmedi, oturumu olduğu kaynaklanan temizlenir. Kümeleri v3.4 için bu davranışı devre dışı bırakmak istiyorsanız Livy config ayarlayabilirsiniz `livy.server.interactive.heartbeat.timeout` için `0` Ambari arabiriminden. Yukarıdaki 3.5 yapılandırma ayarlamazsanız kümeleri v3.5'için oturum silinmez.

1. Jupyter başlatın. Komut isteminden aşağıdaki komutu kullanın.

        jupyter notebook

1. Jupyter Not Defteri kullanarak kümeye bağlanabilir ve ile çekirdekler kullanılabilir Spark Sihirli kullanabileceğiniz doğrulayın. Aşağıdaki adımları uygulayın.

    a. Yeni bir not defteri oluşturun. Sağ taraftaki köşesdeki **yeni**. Varsayılan çekirdek görmelisiniz **Python2** ve yüklediğiniz, iki yeni çekirdekler **PySpark** ve **Spark**. Tıklayın **PySpark**.

    ![Jupyter not defterleri](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Jupyter Not Defterleri")

    b. Aşağıdaki kod parçacığını çalıştırın.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Çıkış başarıyla alabilir, HDInsight kümesine bağlantınızı test edilir.

    >[!TIP]
    >Farklı bir kümeye bağlanmak için not defteri yapılandırmasını güncelleştirmek istiyorsanız, config.json yukarıdaki adım 3'te gösterildiği gibi değerleri, yeni bir dizi ile güncelleştirin.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Bilgisayarımda Jupyter neden yüklemeliyim?
Pek çok neden Jupyter bilgisayarınıza yükleyin ve HDInsight üzerinde Apache Spark kümesi bağlanmak isteyebileceğiniz olabilir.

* Jupyter not defterleri Azure HDInsight Spark kümesinde kullanılabilir durumda olsa da, Jupyter bilgisayarınıza yüklemesini, yerel olarak not defterlerinizi oluşturmak, çalışan bir küme uygulamanızı test etmek ve ardından karşıya yükleme seçeneği sunar kümeye not defterleri. Kümeye not defterlerini karşıya yüklemek için çalışan Jupyter Not Defteri veya küme kullanarak karşıya yükleyin veya kümeyle ilişkili depolama hesabına /HdiNotebooks klasörüne kaydedin. Not Defterleri küme üzerinde nasıl depolandığı ile ilgili daha fazla bilgi için [Jupyter not defterleri depolandığı](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Yerel olarak not defterlerini kullanılabilir uygulama ihtiyacınıza göre farklı Spark kümelerine bağlanabilir.
* Bir kaynak denetim sistemi uygulamak ve not defterleri için sürüm denetimi sağlamak için GitHub'ı kullanabilirsiniz. Ayrıca, burada birden çok kullanıcı aynı not defteri ile çalışabilmeniz için işbirliğine dayalı bir ortam olabilir.
* Bir küme bile kalmadan not defterleri ile yerel olarak çalışabilirsiniz. Yalnızca el ile not defterlerinizi veya bir geliştirme ortamı yönetmemeyi defterlerinizi karşı test etmek için bir küme gerekir.
* Küme üzerinde Jupyter yüklemesini yapılandırmak yerine kendi yerel geliştirme ortamınızı yapılandırmak daha kolay olabilir.  Bir veya daha fazla uzak kümelerini yapılandırma olmadan yerel olarak yüklü tüm yazılımların yararlanabilirsiniz.

> [!WARNING]
> Yerel bilgisayarınızda yüklü Jupyter ile birden çok kullanıcı aynı not defterini kullanarak aynı Spark kümesi üzerinde aynı anda çalıştırabilirsiniz. Böyle bir durumda, birden çok Livy oturumu oluşturulur. Bir sorunla çalıştırın ve, hata ayıklamak isterseniz, karmaşık bir görevin hangi Livy oturumu izlemek için hangi kullanıcıya ait olur.
>
>

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
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
