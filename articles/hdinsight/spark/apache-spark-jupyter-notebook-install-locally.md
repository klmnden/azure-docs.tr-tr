---
title: "Jupyter yerel olarak yüklemek ve bir Azure Hdınsight Spark kümesine bağlanma | Microsoft Docs"
description: "Jupyter not defteri bilgisayarınıza yerel olarak yüklemek ve Azure hdınsight'ta Apache Spark kümesinde bağlanmak öğrenin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 5549c175c280961b09f5996e3387a50dae31222f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter not defteri bilgisayarınıza yüklemek ve hdınsight'ta Apache Spark bağlanın

Bu makalede (Python için) özel PySpark ve Spark Sihirli ile (Scala için) Spark çekirdekler ile Jupyter Not Defteri, yükleme ve not defteri bir Hdınsight kümesine bağlanma öğrenin. Bir dizi Jupyter yerel bilgisayarınıza yüklemek için neden olabilir ve bazı zorluklar de olabilir. Hakkında daha fazla bilgi için bu bölüme bakın [neden yüklediğimde Jupyter bilgisayarımda](#why-should-i-install-jupyter-on-my-computer) bu makalenin sonunda.

Jupyter ve Spark Sihirli bilgisayarınızda yükleme ilgili üç önemli adımlar vardır.

* Jupyter not defteri yükleyin
* İle Spark Sihirli PySpark ve Spark tekrar yükleme
* Hdınsight'ta Spark kümesinde erişmek için Spark Sihirli yapılandırın

Özel tekrar ve Hdınsight kümesi ile Jupyter not defterleri için kullanılabilir Spark Sihirli hakkında daha fazla bilgi için bkz: [Jupyter not defterlerinde kullanılabilen çekirdekler Apache Spark Linux kümeleri Hdınsight'ta](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Önkoşullar
Burada listelenen önkoşulları Jupyter yüklemek için değildir. Bu, Not Defteri yüklendikten sonra Jupyter not defteri bir Hdınsight kümesine bağlamak için kullanılır.

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter not defteri bilgisayarınıza yüklemek

Jupyter not defterleri yükleyebilmek için önce Python yüklemeniz gerekir. Python ve Jupyter kullanılabilir olarak parçası [Anaconda dağıtım](https://www.continuum.io/downloads). Anaconda yüklediğinizde, Python bir dağıtımını yükleyin. Anaconda yüklendikten sonra uygun komutlarını çalıştırarak Jupyter yükleme ekleyin.

1. Karşıdan [Anaconda yükleyici](https://www.continuum.io/downloads) platform ve Çalıştır kurulumu için. Kurulum Sihirbazı'nı çalıştırırken PATH değişkenine Anaconda ekleme seçeneğini seçtiğinizden emin olun.
2. Jupyter yüklemek için aşağıdaki komutu çalıştırın.

        conda install jupyter

    Jupyter yükleme hakkında daha fazla bilgi için bkz: [Anaconda kullanarak Jupyter yükleme](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Spark Sihirli ve tekrar yükleyin

Spark Sihirli yükleme hakkında yönergeler için PySpark ve Spark çekirdekleri kurulum yönergelerini izleyin [sparkmagic belgelerine](https://github.com/jupyter-incubator/sparkmagic#installation) github'da. İlk adım Spark Sihirli belgelerinde Spark Sihirli yüklemenizi ister. Bu ilk adım bağlantıda bağlanacağınız Hdınsight kümesi sürümüne bağlı olarak aşağıdaki komutları değiştirin. Bundan sonra Spark Sihirli belgelerindeki kalan adımları izleyin. Farklı tekrar yüklemek istiyorsanız, Spark Sihirli yükleme yönergeleri bölümünde 3. adım gerçekleştirmeniz gerekir.

* Yürüterek sparkmagic 0.2.3 kümeleri v3.4 için yükleyin `pip install sparkmagic==0.2.3`

* Kümeleri v3.5 ve v3.6 için sparkmagic 0.11.2 yürüterek yükleme `pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark Sihirli Hdınsight Spark kümeye bağlanmak için yapılandırın

Bu bölümde daha önce Azure Hdınsight'ta oluşturmuş olmalısınız bir Apache Spark kümesi bağlanmak için yüklü Spark Sihirli yapılandırın.

1. Jupyter yapılandırma bilgileri, genellikle kullanıcıların giriş dizininde depolanır. Giriş dizininize herhangi bir işletim sistemi platformda bulmak için aşağıdaki komutları yazın.

    Python Kabuğu'nu başlatın. Bir komut penceresinde, aşağıdaki komutu yazın:

        python

    Python Kabuğu giriş dizini bulmak için aşağıdaki komutu girin.

        import os
        print(os.path.expanduser('~'))

2. Giriş dizinine gidin ve adlı bir klasör oluşturun **.sparkmagic** zaten yoksa.
3. Klasör içinde adlı bir dosya oluşturun **config.json** ve onun içindeki aşağıdaki JSON parçacığını ekleyin.

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

4. Yedek **{USERNAME}**, **{CLUSTERDNSNAME}**, ve **{BASE64ENCODEDPASSWORD}** uygun değerlere sahip. Yardımcı programlar, sık kullanılan programlama dili veya çevrimiçi çeşitli gerçek parolanızı bir base64 ile kodlanmış parolası oluşturmak için kullanabilirsiniz.

5. Sağ sinyal ayarlarında yapılandırdığınız `config.json`. Bu ayarları aynı düzeyde eklemelisiniz `kernel_python_credentials` ve `kernel_scala_credentials` parçacıkları, eklenen önceki. Örneği nasıl ve nerede sinyal ayarları eklemek bu bkz [örnek config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * İçin `sparkmagic 0.2.3` (v3.4 kümeleri) içerir:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * İçin `sparkmagic 0.11.2` (kümeleri v3.5 ve v3.6) içerir:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Sinyal oturumları sızdırılmaz sağlamak için gönderilir. Bir bilgisayar uyku moduna geçer veya kapatılmış, sinyal gönderilmez, elde edilen oturum temizlendi. Kümeleri v3.4 için bu davranışı devre dışı bırakmak isterseniz, Livy config ayarlayabilirsiniz `livy.server.interactive.heartbeat.timeout` için `0` Ambari UI gelen. Yukarıdaki 3.5 yapılandırma ayarlamazsanız kümeleri v3.5 için oturum silinmez.

6. Jupyter başlatın. Komut isteminde aşağıdaki komutu kullanın.

        jupyter notebook

7. Jupyter Not Defteri kullanarak kümeye bağlanabilir ve ile tekrar kullanılabilir Spark Sihirli kullanabileceğiniz doğrulayın. Aşağıdaki adımları gerçekleştirin.

    a. Yeni bir not defteri oluşturun. Sağ köşesinden tıklatın **yeni**. Varsayılan çekirdek görmelisiniz **Python2** ve yüklediğiniz, iki yeni çekirdekler **PySpark** ve **Spark**. Tıklatın **PySpark**.

    ![Jupyter not defterlerinde çekirdekler](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Jupyter not defterlerinde çekirdekler")

    b. Aşağıdaki kod parçacığını çalıştırın.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Çıktı başarıyla alabilir, Hdınsight kümesi bağlantınızı test edilir.

    >[!TIP]
    >Farklı bir kümeye bağlanmak için dizüstü bilgisayar yapılandırmasını güncelleştirmek istiyorsanız, config.json değerleri, yeni kümesiyle yukarıdaki adım 3'te gösterildiği gibi güncelleştirin.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Bilgisayarımda neden Jupyter yüklediğimde?
Pek çok neden Jupyter bilgisayarınıza yüklemek ve Hdınsight Spark kümesinde bağlanmaya isteyebileceğinizi olabilir.

* Jupyter not defterleri zaten Azure Hdınsight Spark kümesinde kullanılabilir olsa bile, bilgisayarınıza Jupyter yükleme, defterlerinizi oluşturma seçeneğini yerel olarak çalışan bir küme karşı uygulamanızı test etmek ve kümeye not defterlerini karşıya yükleme sağlar. Dizüstü bilgisayarlar kümeye karşıya yüklemek için çalışan Jupyter Not Defteri veya küme kullanarak bunları karşıya yükleme veya bunları kümeyle ilişkili depolama hesabındaki /HdiNotebooks klasörüne kaydedin. Dizüstü bilgisayarlar kümede nasıl depolandığını daha fazla bilgi için bkz: [Jupyter not defterleri depolandığı](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Yerel olarak not defterlerini kullanılabilir uygulama gereksinimi temel alan farklı Spark kümelerine bağlanabilir.
* Kaynak denetim sistemi uygulamak ve dizüstü bilgisayarlar için sürüm denetimi sağlamak için GitHub'ı kullanabilirsiniz. Ayrıca, birden çok kullanıcı aynı not defteri ile burada çalışabilir işbirliğine dayalı bir ortam olabilir.
* Bir küme bile sahip olmadan dizüstü bilgisayarlar ile yerel olarak çalışabilir. Yalnızca el ile defterlerinizi veya geliştirme ortamında yönetmediğinizden defterlerinizi karşı test etmek için bir küme gerekir.
* Kümede Jupyter yüklemesini yapılandırmak yerine, kendi yerel geliştirme ortamı yapılandırmak daha kolay olabilir.  Bir veya daha fazla uzak kümelerini yapılandırma olmadan yerel olarak yüklü tüm yazılımların avantajından yararlanabilirsiniz.

> [!WARNING]
> Yerel bilgisayarınızda yüklü Jupyter ile birden çok kullanıcı aynı dizüstü bilgisayar aynı Spark kümesi üzerinde aynı anda çalıştırabilirsiniz. Böyle bir durumda, birden çok Livy oturumu oluşturulur. Bir sorunu yaşayıp çalıştırın ve, hata ayıklamak istediğiniz, hangi Livy oturumu izlemek için karmaşık bir görev hangi kullanıcıya ait olur.
>
>

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
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
