---
title: Jupyter yerel olarak yükleyin ve Azure HDInsight Spark bağlanma
description: Jupyter not defterine bilgisayarınıza yerel olarak yükleyin ve bir Apache Spark kümesine bağlanma hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: hrasheed
ms.openlocfilehash: 489685485af4e3c8868f7e0281d2f81464a166f6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066186"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter not defterine bilgisayarınıza yükleyin ve HDInsight üzerinde Apache spark'a bağlanma

Bu makalede (Python için) özel PySpark ve Spark Sihirli ile (Scala için) Apache Spark defterleri ile Jupyter Not Defteri, yükleme ve not defterini bir HDInsight kümesine bağlanma hakkında bilgi edinin. Birkaç Jupyter yerel bilgisayarınıza yüklemek için neden olabilir ve bazı zorluklar de olabilir. Hakkında daha fazla bilgi için bu konudaki [neden yüklediğimde Jupyter bilgisayarımda](#why-should-i-install-jupyter-on-my-computer) bu makalenin sonunda.

Jupyter yükleme ve HDInsight üzerinde Apache spark'a bağlayarak katılan dört önemli adımlar vardır.

* Spark kümesi yapılandırın.
* Jupyter not defteri yükleyin.
* PySpark ve Spark çekirdekler Spark ile Sihirli yükleyin.
* HDInsight Spark kümesine erişmek için Spark Sihirli yapılandırın.

Özel çekirdekler ve HDInsight kümesi ile Jupyter not defterleri için Spark Sihirli hakkında daha fazla bilgi için bkz. [için Jupyter not defterlerinde kullanılabilen çekirdekler Apache Spark Linux kümeleri HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Önkoşullar

Burada listelenen önkoşulları Jupyter yüklemek için değildir. Bu, Not defterini yüklendikten sonra Jupyter not defterini bir HDInsight kümesine bağlamak için kullanılır.

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter not defterine bilgisayarınıza yükleyin.

Jupyter not defterleri yükleyebilmek için önce Python yüklemeniz gerekir. [Anaconda dağıtım](https://www.anaconda.com/download/) hem Python hem de Jupyter not defteri yükler.

İndirme [Anaconda yükleyici](https://www.anaconda.com/download/) platform ve Kurulumu çalıştırın. Kurulum Sihirbazı çalıştırırken, yol değişkeninize Anaconda ekleme seçeneği seçtiğinizden emin olun.  Ayrıca bkz [Anaconda kullanarak Jupyter yükleme](https://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-spark-magic"></a>Spark Sihirli yükleyin

1. Spark Sihirli yüklemek için aşağıdaki komutlardan birini girin. Ayrıca bkz [sparkmagic belgeleri](https://github.com/jupyter-incubator/sparkmagic#installation).

    |Küme sürümü | Yükleme komutu |
    |---|---|
    |V3.6 ve v3.5 |`pip install sparkmagic==0.12.7`|
    |v3.4|`pip install sparkmagic==0.2.3`|

1. Olun `ipywidgets` aşağıdaki komutu çalıştırarak düzgün şekilde yüklenir:

    ```cmd
    jupyter nbextension enable --py --sys-prefix widgetsnbextension
    ```

## <a name="install-pyspark-and-spark-kernels"></a>PySpark ve Spark çekirdekler yükleyin

1. WHERE tanımlamak `sparkmagic` aşağıdaki komutu girerek yüklenir:

    ```cmd
    pip show sparkmagic
    ```

    Ardından, çalışan dizininizle yukarıdaki komutla tanımlanan konumla değiştirin.

1. Yeni çalışma dizininizden bir veya daha fazla istenen kernel(s) yüklemek için aşağıdaki komutları girin:

    |Çekirdek | Komut |
    |---|---|
    |Spark|`jupyter-kernelspec install sparkmagic/kernels/sparkkernel`|
    |SparkR|`jupyter-kernelspec install sparkmagic/kernels/sparkrkernel`|
    |PySpark|`jupyter-kernelspec install sparkmagic/kernels/pysparkkernel`|
    |PySpark3|`jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel`|

1. İsteğe bağlı. Sunucu uzantısını etkinleştirmek için aşağıdaki komutu girin:

    ```cmd
    jupyter serverextension enable --py sparkmagic
    ```

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark Sihirli HDInsight Spark kümesine bağlanmak için yapılandırın

Bu bölümde, daha önce bir Apache Spark kümesine bağlanmak için yüklü Spark Sihirli yapılandırın.

1. Python Kabuğu'nu aşağıdaki komutla başlatın:

    ```cmd
    python
    ```

2. Jupyter yapılandırma bilgileri genellikle kullanıcıların giriş dizininde depolanır. Giriş dizini belirlemek için aşağıdaki komutu girin ve adlı bir klasör oluşturun **.sparkmagic**.  Tam yolu yüzdelik.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. Klasördeki `.sparkmagic`, adlı bir dosya oluşturun **config.json** ve içine aşağıdaki JSON kod parçacığını ekleyin.  

    ```json
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
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```

4. Dosyasına aşağıdaki düzenlemelerini yapın:

    |Şablon değeri | Yeni değer |
    |---|---|
    |{USERNAME}|Küme girişi varsayılandır `admin`.|
    |{CLUSTERDNSNAME}|Küme adı|
    |{BASE64ENCODEDPASSWORD}|Gerçek parolanızı parolasını bir base64 kodlamalı.  Bir base64 parola oluşturabileceğiniz [ https://www.url-encode-decode.com/base64-encode-decode/ ](https://www.url-encode-decode.com/base64-encode-decode/).|
    |`"livy_server_heartbeat_timeout_seconds": 60`|Kullanıyorsanız tutmak `sparkmagic 0.12.7` (v3.5 ve v3.6 kümeleri).  Kullanıyorsanız `sparkmagic 0.2.3` (v3.4 kümeleri) yerine `"should_heartbeat": true`.|

    Bir tam örnek dosyasını görebilirsiniz [örnek config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

   > [!TIP]  
   > Oturumlarının sızdırılmaz emin olmak için sinyaller gönderilir. Bir bilgisayar uyku moduna geçer veya kapatıldığında, sinyal gönderilmedi, oturumu olduğu kaynaklanan temizlenir. Kümeleri v3.4 için bu davranışı devre dışı bırakmak istiyorsanız Livy config ayarlayabilirsiniz `livy.server.interactive.heartbeat.timeout` için `0` Ambari arabiriminden. Yukarıdaki 3.5 yapılandırma ayarlamazsanız kümeleri v3.5'için oturum silinmez.

5. Jupyter başlatın. Komut isteminden aşağıdaki komutu kullanın.

    ```cmd
    jupyter notebook
    ```

6. Spark Sihirli kullanılabilir çekirdekler ile kullandığından emin olun. Aşağıdaki adımları uygulayın.

    a. Yeni bir not defteri oluşturun. Sağ Köşeden seçin **yeni**. Varsayılan çekirdek görmelisiniz **Python 2** veya **Python 3** ve yüklediğiniz çekirdekler. Gerçek değerleri yükleme seçimlerinizi bağlı olarak değişiklik gösterebilir.  Seçin **PySpark**.

    ![Jupyter not defterleri](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Jupyter Not Defterleri")

    > [!IMPORTANT]  
    > Seçtikten sonra **yeni** kabuğunuz hatalar için gözden geçirin.  Hata görürseniz `TypeError: __init__() got an unexpected keyword argument 'io_loop'` hortum'ın bilinen bir sorun yaşıyor olabilirsiniz.  Bu durumda, çekirdek durdurun ve ardından aşağıdaki komutla hortum yüklemenizi düşürme: `pip install tornado==4.5.3`.

    b. Aşağıdaki kod parçacığını çalıştırın.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Çıkış başarıyla alabilir, HDInsight kümesine bağlantınızı test edilir.

    Farklı bir kümeye bağlanmak için not defteri yapılandırmasını güncelleştirmek istiyorsanız, config.json yukarıdaki adım 3'te gösterildiği gibi değerleri, yeni bir dizi ile güncelleştirin.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Bilgisayarımda Jupyter neden yüklemeliyim?

Pek çok neden Jupyter bilgisayarınıza yükleyin ve HDInsight üzerinde Apache Spark kümesi bağlanmak isteyebileceğiniz olabilir.

* Jupyter not defterleri Azure HDInsight Spark kümesinde kullanılabilir durumda olsa da, Jupyter bilgisayarınıza yüklemesini, yerel olarak not defterlerinizi oluşturmak, çalışan bir küme uygulamanızı test etmek ve ardından karşıya yükleme seçeneği sunar kümeye not defterleri. Kümeye not defterlerini karşıya yüklemek için çalışan Jupyter Not Defteri veya küme kullanarak karşıya yükleyin veya kümeyle ilişkili depolama hesabına /HdiNotebooks klasörüne kaydedin. Not Defterleri küme üzerinde nasıl depolandığı ile ilgili daha fazla bilgi için [Jupyter not defterleri depolandığı](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Yerel olarak not defterlerini kullanılabilir uygulama ihtiyacınıza göre farklı Spark kümelerine bağlanabilir.
* Bir kaynak denetim sistemi uygulamak ve not defterleri için sürüm denetimi sağlamak için GitHub'ı kullanabilirsiniz. Ayrıca, burada birden çok kullanıcı aynı not defteri ile çalışabilmeniz için işbirliğine dayalı bir ortam olabilir.
* Bir küme bile kalmadan not defterleri ile yerel olarak çalışabilirsiniz. Yalnızca el ile not defterlerinizi veya bir geliştirme ortamı yönetmemeyi defterlerinizi karşı test etmek için bir küme gerekir.
* Küme üzerinde Jupyter yüklemesini yapılandırmak yerine kendi yerel geliştirme ortamınızı yapılandırmak daha kolay olabilir.  Bir veya daha fazla uzak kümelerini yapılandırma olmadan yerel olarak yüklü tüm yazılımların yararlanabilirsiniz.

> [!WARNING]  
> Yerel bilgisayarınızda yüklü Jupyter ile birden çok kullanıcı aynı not defterini kullanarak aynı Spark kümesi üzerinde aynı anda çalıştırabilirsiniz. Böyle bir durumda, birden çok Livy oturumu oluşturulur. Bir sorunla çalıştırın ve, hata ayıklamak isterseniz, karmaşık bir görevin hangi Livy oturumu izlemek için hangi kullanıcıya ait olur.  

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight üzerinde Apache Spark](apache-spark-overview.md)
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)