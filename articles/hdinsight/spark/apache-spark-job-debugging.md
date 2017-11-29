---
title: "Çalışan Azure Hdınsight'ta Apache Spark işleri hata ayıklama | Microsoft Docs"
description: "YARN kullanıcı Arabiriminde, Spark UI ve Spark geçmişi server izlemek ve Azure Hdınsight Spark kümesinde çalışan işleri hata ayıklamak için kullanın"
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jgao
ms.openlocfilehash: 1eaa5982703c31485c7b73eae780a62a0c5d672a
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Çalışan Azure Hdınsight'ta Apache Spark işleri hata ayıklama

Bu makalede, izlemek ve YARN kullanıcı Arabiriminde, Spark UI ve Spark geçmişi Server kullanarak Hdınsight kümelerinde çalışan Spark işleri hata ayıklama öğrenin. Bu makalede, biz Spark kümesi ile kullanılabilir bir Not Defteri kullanarak Spark işi Başlat **Machine learning: Yemek İnceleme verileri Mllib'i kullanarak Tahmine dayalı analiz**. Örneğin, gönderilen tüm diğer yaklaşımı de kullanarak bir uygulama izlemek için aşağıdaki adımları kullanabilirsiniz **spark gönderme**.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).
* Not Defteri çalıştıran başlamış olması gereken  **[Machine learning: Yemek İnceleme verileri Mllib'i kullanarak Tahmine dayalı analiz](apache-spark-machine-learning-mllib-ipython.md)**. Bu Not çalıştırma hakkında daha fazla yönerge için bağlantıyı izleyin.  

## <a name="track-an-application-in-the-yarn-ui"></a>Bir uygulama YARN kullanıcı arabiriminde izleme
1. YARN kullanıcı arabirimini Başlat. Küme dikey penceresinden tıklayın **küme Panosu**ve ardından **YARN**.
   
    ![YARN kullanıcı arabirimini Başlat](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > Alternatif olarak, Ambari UI YARN Arabiriminden başlatabilirsiniz. Küme dikey penceresinden Ambari kullanıcı arabirimini başlatmak için tıklatın **küme Panosu**ve ardından **Hdınsight küme Panosu**. Ambari Arabiriminden tıklatın **YARN**, tıklatın **hızlı bağlantılar**, etkin Kaynak Yöneticisi'ni tıklatın ve ardından **ResourceManager UI**.    
   > 
   > 
2. Jupyter not defterlerini kullanarak Spark iş başlatıldı çünkü uygulama adına sahip **remotesparkmagics** (Bu defterlerinden başlatılan tüm uygulamalar için addır). İş hakkında daha fazla bilgi almak için uygulama adı karşı uygulama kimliği'ni tıklatın. Bu uygulama görünümünü başlatır.
   
    ![Spark uygulama kimliği bulunamıyor](./media/apache-spark-job-debugging/find-application-id.png)
   
    Jupyter not defterlerine olan başlatılan bu tür uygulamalar için her zaman durumudur **çalıştıran** not defteri çıkana kadar.
3. Uygulama görünümünden uygulama ve Günlükler (stdout/stderr) ile ilişkili kapsayıcıları bulma daha fazla ayrıntıya girebilirsiniz. Bağlama karşılık gelen'i tıklatarak Spark kullanıcı arabirimini başlatabilirsiniz **izleme URL'si**, aşağıda gösterildiği gibi. 
   
    ![Kapsayıcı günlüklerini indirin](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Uygulamanın kullanıcı arabiriminde Spark izleme
Spark Arabiriminde daha önce başlatılan uygulama tarafından kökenli Spark işlere ayrıntıya girebilirsiniz.

1. Uygulama görünümünden Spark kullanıcı arabirimini başlatmak için karşı bağlantısı **izleme URL'si**yukarıdaki ekran görüntüsünde gösterildiği gibi. Jupyter not defteri çalışan uygulama tarafından başlatılan tüm Spark işleri görebilirsiniz.
   
    ![Spark işleri görüntüle](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Tıklatın **yürütücüler** her Yürütücü işleme ve Depolama bilgileri görmek için sekmesi. Tıklayarak, çağrı yığını de alabilirsiniz **iş parçacığı dökümü** bağlantı.
   
    ![Spark yürütücüler görüntüleyin](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Tıklatın **aşamaları** uygulama ile ilişkili aşamaları görmek için sekmesini.
   
    ![Görünüm Spark aşamaları](./media/apache-spark-job-debugging/view-spark-stages.png)
   
    Her aşama için görüntüleyebilirsiniz yürütme istatistikleri, gibi birden çok görev sağlayabilirsiniz aşağıda gösterilen.
   
    ![Görünüm Spark aşamaları](./media/apache-spark-job-debugging/view-spark-stages-details.png) 
4. Aşama ayrıntıları sayfasından DAG görselleştirme başlatabilirsiniz. Genişletme **DAG görselleştirme** aşağıda gösterildiği gibi sayfanın en üstünde bağlantı.
   
    ![Görünüm Spark DAG görselleştirme aşamaları](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG veya doğrudan Aclyic grafik uygulama farklı aşamalarda temsil eder. Her grafiğin mavi kutuda uygulamadan çağrılan bir Spark işlemini temsil eder.
5. Aşama ayrıntıları sayfasından uygulama Zaman Çizelgesi görünümüne de başlatabilirsiniz. Genişletme **olay zaman çizelgesi** aşağıda gösterildiği gibi sayfanın en üstünde bağlantı.
   
    ![Olay Zaman Çizelgesi görünümüne Spark aşamaları](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Bu Spark olayları bir zaman çizelgesi biçiminde görüntüler. Zaman Çizelgesi görünümüne üç düzeyde işleri, işindeki ve aşama içinde üzerinden kullanılabilir. Yukarıdaki resimde belirli bir aşama için Zaman Çizelgesi görünümüne yakalar.
   
   > [!TIP]
   > Seçerseniz **yakınlaştırma etkinleştirmek** onay kutusunu kaydırma sol ve sağ arasında zaman çizelgesi görünümü.
   > 
   > 
6. Diğer sekmeleri Spark, Spark örneğinde de hakkında yararlı bilgiler sağlar.
   
   * Uygulamanızı bir RDDs oluşturursa, depolama sekmesi - depolama sekmesi de hakkında bilgi bulabilirsiniz.
   * Ortam sekmesi - Bu sekme sağlayan çok sayıda Spark örneğinizi hakkında yararlı bilgiler gibi 
     * Scala sürüm
     * Kümeyle ilişkili olay günlüğü dizini
     * Uygulama için Yürütücü çekirdek sayısı
     * Etc.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Tamamlanan İşler kullanarak Spark geçmişi sunucu hakkında bilgi
Bir iş tamamlandığında iş hakkındaki bilgileri Spark geçmişi Server'da kalıcıdır.

1. Spark geçmişi sunucudan ve küme dikey başlatmak için tıklatın **küme Panosu**ve ardından **Spark geçmişi sunucu**.
   
    ![Spark geçmişi sunucu başlatma](./media/apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > Alternatif olarak, Spark geçmişi sunucu kullanıcı Arabiriminden Ambari kullanıcı arabirimini başlatabilirsiniz. Küme dikey penceresinden Ambari kullanıcı arabirimini başlatmak için tıklatın **küme Panosu**ve ardından **Hdınsight küme Panosu**. Ambari Arabiriminden tıklatın **Spark**, tıklatın **hızlı bağlantılar**ve ardından **Spark geçmişi sunucu UI**.
   > 
   > 
2. Listelenen tüm tamamlanan uygulamaları görürsünüz. Daha fazla bilgi için uygulama incelemek için bir uygulama kimliği'ni tıklatın.
   
    ![Spark geçmişi sunucu başlatma](./media/apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Ayrıca bkz.
*  [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a>Veri çözümleyiciler

* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight’ta Spark kullanarak Application Insight telemetri verilerinin analizi](apache-spark-analyze-application-insight-logs.md)
* [Caffe Azure Hdınsight Spark üzerinde dağıtılmış derin learning için kullanın.](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Spark geliştiricileri için

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)


