---
title: Azure HDInsight üzerinde çalışan Apache Spark işlerinde hata ayıklama
description: Spark geçmiş sunucusu YARN UI ve Spark UI izlemek ve Azure HDInsight Spark kümesinde çalışan işleri hata ayıklamak için kullanın
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: hrasheed
ms.openlocfilehash: 5e384520c1b8d6cf5e3b182bbddf41a5f4f7f8f6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124297"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Azure HDInsight üzerinde çalışan Apache Spark işlerinde hata ayıklama

Bu makalede, izleme ve hata ayıklama hakkında bilgi edinin [Apache Spark](https://spark.apache.org/) üzerinde çalışan işleri kullanarak HDInsight kümelerini [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) UI, Spark UI ve Spark geçmiş sunucusu. Mevcut bir Not Defteri kullanarak Spark kümesi ile bir Spark işi başlatmak **makine öğrenimi: Gıda denetimi verileri MLLib kullanarak Tahmine dayalı analiz**. Örneğin, gönderilen tüm diğer yaklaşımı de kullanarak bir uygulama izlemek için aşağıdaki adımları kullanabilirsiniz **spark-submit**.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Not Defteri çalıştırma başlaması gereken  **[makine öğrenimi: Gıda denetimi verileri MLLib kullanarak Tahmine dayalı analiz](apache-spark-machine-learning-mllib-ipython.md)**. Bu not defteri çalıştırmak yönergeler için bağlantıyı izleyin.  

## <a name="track-an-application-in-the-yarn-ui"></a>YARN kullanıcı arabiriminde bir uygulama izleme
1. YARN UI başlatın. Tıklayın **Yarn** altında **küme panoları**.
   
    ![YARN kullanıcı arabirimini Başlat](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]  
   > Alternatif olarak, Ambari UI YARN kullanıcı Arabiriminden da başlatabilirsiniz. Ambari UI başlatmak için tıklayın **Ambari giriş** altında **küme panoları**. Ambari Arabiriminden tıklayın **YARN**, tıklayın **hızlı bağlantılar**etkin Kaynak Yöneticisi'ni tıklatın ve ardından **kaynak yöneticisi kullanıcı Arabirimi**. 

2. Jupyter not defterlerini kullanarak Spark işi başlatıldığından, uygulama adına sahip **remotesparkmagics** (Not defterlerinden başlatılan tüm uygulamalara ilişkin ad budur). İşle ilgili daha fazla bilgi almak için uygulama kimliği uygulama adı karşı tıklayın. Bu, uygulama görünümü çalıştırır.
   
    ![Spark uygulaması kimliği bulunamıyor](./media/apache-spark-job-debugging/find-application-id.png)
   
    Jupyter Not defterlerinden başlatılan, böyle uygulamalar için her zaman durumudur **çalıştıran** not defterini çıkana kadar.
3. Uygulama görünümünden uygulama ve günlükleri (stdout/stderr) ile ilişkili kapsayıcılar bulma daha fazla sınırlandıramazsınız detaya gidebilirsiniz. Bağlama karşılık gelen'ı tıklatarak da Spark kullanıcı arabirimini başlatabilirsiniz **izleme URL**, aşağıda gösterildiği gibi. 
   
    ![Kapsayıcı günlüklerini indir](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Uygulamanın kullanıcı arabiriminde Spark izleme
Spark kullanıcı Arabiriminde daha önce başlatıldı uygulama tarafından üretilen Spark işlere detaya gidebilirsiniz.

1. Spark Arabiriminden uygulama görünümü başlatmak için karşı bağlantısı **izleme URL**yukarıdaki ekran görüntüsünde gösterildiği gibi. Jupyter not defteri çalışan uygulama tarafından başlatılan tüm Spark işleri görebilirsiniz.
   
    ![Spark işleri görüntüle](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Tıklayın **yürütücüler** her Yürütücü işleme ve Depolama bilgileri görmek için sekmesinde. Tıklayarak çağrı yığınını alabilirsiniz **iş parçacığı dökümü** bağlantı.
   
    ![Spark Yürütücü görüntüleyin](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Tıklayın **aşamaları** uygulamayla ilişkili aşamaları görmek için sekmesinde.
   
    ![Spark aşamaları görünümünü](./media/apache-spark-job-debugging/view-spark-stages.png)
   
    Her aşama için görüntüleyebilirsiniz yürütme istatistikleri gibi birden çok görevler çalıştırılabileceğini aşağıda gösterilmiştir.
   
    ![Spark aşamaları görünümünü](./media/apache-spark-job-debugging/view-spark-stages-details.png) 
4. Aşama Ayrıntılar sayfasından DAG görselleştirme başlatabilirsiniz. Genişletin **DAG görselleştirme** aşağıda gösterildiği gibi sayfanın en üstündeki bağlayın.
   
    ![Spark aşamaları DAG görselleştirmeyi görüntüleyin](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG ya da doğrudan Aclyic grafik uygulama farklı aşamaları temsil eder. Graftaki her mavi kutu uygulamadan çağrılan bir Spark işlem temsil eder.
5. Aşama Ayrıntılar sayfasından uygulama zaman çizelgesi görünümü da başlatabilirsiniz. Genişletin **olay zaman çizelgesi** aşağıda gösterildiği gibi sayfanın en üstündeki bağlayın.
   
    ![Spark aşamaları olay zaman çizelgesini görüntüleyin](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Bu, bir zaman çizelgesi biçiminde Spark olayları görüntüler. Zaman Çizelgesi Görünümü üç düzeyde işindeki ve aşama içinde işleri arasında kullanılabilir. Yukarıdaki resimde, belirli bir aşama için zaman çizelgesi görünümü yakalar.
   
   > [!TIP]  
   > Seçerseniz **etkinleştirmek** onay kutusu kaydırma sola ve sağa arasında zaman çizelgesi görünümü.

6. Spark Arabirimindeki diğer sekmelere de Spark örneğinde hakkında yararlı bilgiler sağlar.
   
   * Uygulamanızı bir Rdd oluşturursa, depolama sekmesi - olanlar depolama sekmesi hakkında bilgi bulabilirsiniz.
   * Ortam sekmesi - Bu sekme, bir Spark örneği hakkında yararlı bilgiler çok gibi sağlar 
     * Scala sürümü
     * Kümeyle ilişkili olay günlüğü dizini
     * Uygulama için Yürütücü çekirdek sayısı
     * Etc.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Spark geçmiş sunucusu kullanarak tamamlanan işler hakkında bilgi
Bir iş tamamlandığında, işle ilgili bilgiler Spark geçmiş sunucusu kalıcı hale getirilir.

1. Spark geçmiş sunucusu genel bakış dikey penceresinden başlatmak için tıklayın **Spark geçmiş sunucusu** altında **küme panoları**.
   
    ![Spark geçmiş sunucusu başlatma](./media/apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]  
   > Alternatif olarak, Ambari UI Spark geçmiş sunucusu Arabiriminden da başlatabilirsiniz. Ambari UI, genel bakış dikey penceresinden başlatmak için tıklayın **Ambari giriş** altında **küme panoları**. Ambari Arabiriminden tıklayın **Spark**, tıklayın **hızlı bağlantılar**ve ardından **Spark geçmiş sunucusu kullanıcı Arabiriminin**.

2. Listelenen tüm tamamlanan uygulamalar görürsünüz. Bir uygulama kimliği, bir uygulamaya daha fazla bilgi için detaya gitmek için tıklayın.
   
    ![Spark geçmiş sunucusu başlatma](./media/apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Ayrıca bkz.
*  [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
*  [Genişletilmiş Spark geçmiş sunucusu kullanarak Apache Spark işlerinde hata ayıklama](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Veri analistleri için

* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight Apache Spark'ı kullanarak application Insight telemetri verilerinin analizi](apache-spark-analyze-application-insight-logs.md)
* [Azure HDInsight Spark üzerinde dağıtılmış derin öğrenme için Caffe kullanma](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Spark geliştiricileri için

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
