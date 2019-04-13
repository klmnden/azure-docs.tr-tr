---
title: Azure HDInsight üzerinde Apache Spark kümesi kaynaklarını yönetme
description: Nasıl kullanacağınızı öğrenin daha iyi performans için Azure HDInsight Spark kümelerinde kaynaklarını yönetme.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: hrasheed
ms.openlocfilehash: 023fd8267a557fa57e98a6a57785fb9ebfcb12ab
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523979"
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Azure HDInsight üzerinde Apache Spark kümesi kaynaklarını yönetme 

Arabirimler gibi erişmeyi öğrenin [Apache Ambari](https://ambari.apache.org/) kullanıcı Arabirimi, [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) kullanıcı Arabirimi ve Spark geçmiş sunucusu ile ilişkili, [Apache Spark](https://spark.apache.org/) küme ve nasıl yapılır en iyi performans için küme yapılandırmasını ayarlayın.

**Ön koşullar:**

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="open-the-ambari-web-ui"></a>Ambari Web kullanıcı arabirimini açın

Apache Ambari kümesini izlemek ve yapılandırma değişiklikleri yapmak için kullanılır. Daha fazla bilgi için [yönetme Apache Hadoop, Azure portalını kullanarak HDInsight kümeleri](../hdinsight-administer-use-portal-linux.md#open-the-apache-ambari-web-ui)

## <a name="open-the-spark-history-server"></a>Spark geçmiş sunucusu açın

Spark geçmiş tamamlanmış ve çalışan Spark uygulamaları için kullanıcı Arabirimi web sunucusudur. Spark'ın Web kullanıcı arabiriminin bir uzantısıdır.

**Spark geçmiş sunucusu Web kullanıcı arabirimini açmak için**

1. Gelen [Azure portalında](https://portal.azure.com/), Spark kümesini açın. Daha fazla bilgi için [kümeleri Listele ve Göster](../hdinsight-administer-use-portal-linux.md#showClusters).
2. Gelen **hızlı bağlantılar**, tıklayın **küme Panosu**ve ardından **Spark geçmiş sunucusu**

    ![Spark geçmiş sunucusu](./media/apache-spark-resource-manager/launch-history-server.png "Spark geçmiş sunucusu")

    İstendiğinde, Spark küme için yönetici kimlik bilgilerini girin. Spark geçmiş sunucusu aşağıdaki URL'ye göz atarak da açabilirsiniz:

    ```
    https://<ClusterName>.azurehdinsight.net/sparkhistory
    ```

    Değiştirin `<ClusterName>` ile Spark kümenizin adıdır.

Spark geçmiş sunucusu web kullanıcı Arabirimi gibi görünür:

![HDInsight Spark geçmiş sunucusu](./media/apache-spark-resource-manager/hdinsight-spark-history-server.png)

## <a name="open-the-yarn-ui"></a>Yarn UI'ı açın
YARN UI kullanarak Spark kümesi üzerinde çalışmakta olan uygulamaları izlemek için kullanabilirsiniz.

1. Gelen [Azure portalında](https://portal.azure.com/), Spark kümesini açın. Daha fazla bilgi için [kümeleri Listele ve Göster](../hdinsight-administer-use-portal-linux.md#showClusters).
2. Gelen **hızlı bağlantılar**, tıklayın **küme Panosu**ve ardından **YARN**.

    ![YARN kullanıcı arabirimini Başlat](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]  
   > Alternatif olarak, Ambari UI YARN kullanıcı Arabiriminden da başlatabilirsiniz. Ambari UI başlatmak için tıklayın **küme Panosu**ve ardından **HDInsight küme Panosu**. Ambari Arabiriminden tıklayın **YARN**, tıklayın **hızlı bağlantılar**etkin Kaynak Yöneticisi'ni tıklatın ve ardından **kaynak yöneticisi kullanıcı Arabirimi**.

## <a name="optimize-clusters-for-spark-applications"></a>Kümeleri Spark uygulamaları için en iyi duruma getirme
Uygulama gereksinimlerine bağlı olarak Spark yapılandırması için kullanılabilecek üç anahtar parametreleri `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir yürütücü Spark uygulaması için başlatılan bir işlemdir. Çalışan düğümü üzerinde çalışan ve uygulama için görevleri yürütmek için sorumludur. Varsayılan yürütücü ve her küme için Yürütücü boyutları sayısını çalışan düğümlerine ve çalışan düğümü boyutu sayısı hesaplanır. Bu bilgiler saklanır `spark-defaults.conf` küme baş düğümlerine.

Üç yapılandırma parametrelerini (için küme üzerinde çalışan tüm uygulamalar) küme seviyesinde yapılandırılmış veya her tek tek uygulama için belirtilebilir.

### <a name="change-the-parameters-using-ambari-ui"></a>Ambari UI kullanarak parametreleri değiştirin
1. Ambari UI'ı tıklatın **Spark**, tıklayın **yapılandırmaları**ve ardından **özel spark-varsayılanları**.

    ![Ambari kullanarak parametrelerini ayarlayın](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Varsayılan değerleri kümede aynı anda çalıştırılması dört Spark uygulamaları uygundur. Aşağıdaki ekran görüntüsünde gösterildiği gibi kullanıcı arabiriminden bu değerleri değiştirebilirsiniz:

    ![Ambari kullanarak parametrelerini ayarlayın](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Tıklayın **Kaydet** yapılandırma değişikliklerini kaydetmek için. Sayfanın üst kısmında etkilenen tüm hizmetleri yeniden başlatmanız istenir. **Yeniden Başlat**'a tıklayın.

    ![Hizmetlerini yeniden başlatın](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter not defteri çalışan bir uygulama için parametreleri Değiştir
Jupyter not defteri çalışan uygulamalar için kullandığınız `%%configure` yapılandırma değişiklikleri yapmak için magic. İdeal olarak, birinci kod hücresini çalıştırmadan önce uygulama başına bu tür değişiklikler yapmanız gerekir. Bunun yapılması sağlar yapılandırmayı oluşturulduğu Livy oturumuna uygulanır. Uygulama daha sonraki bir aşamasında yapılandırmasını değiştirmek istiyorsanız, kullanmalısınız `-f` parametresi. Ancak, bunu yaparak uygulamanın tüm devam eden kaybolur.

Aşağıdaki kod parçacığı, Jupyter içinde çalışan bir uygulama yapılandırmasını değiştirme işlemi gösterilmektedir.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Yapılandırma parametreleri, içinde bir JSON dizesi geçirilmelidir ve sonraki satırda Sihirli sonra örnek sütununda gösterildiği gibi olması gerekir.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Parametreler kullanılarak gönderilen bir uygulama için spark-submit Değiştir
Aşağıdaki komut, bir örnek kullanılarak gönderilen bir batch uygulaması için yapılandırma parametreleri değiştirmek nasıl `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>CURL kullanarak gönderilen bir uygulama için parametreleri Değiştir
Aşağıdaki komut, cURL kullanarak gönderilen bir batch uygulaması için yapılandırma parametreleri değiştirmek nasıl bir örneğidir.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="change-these-parameters-on-a-spark-thrift-server"></a>Spark Thrift sunucusu bu parametreleri değiştirmek
Spark Thrift sunucusu, bir Spark kümesi JDBC/ODBC erişim sağlar ve hizmet Spark SQL sorguları için kullanılır. Power BI, Tableau vb. gibi araçlar. Spark uygulaması olarak Spark SQL sorguları çalıştırmak için Spark Thrift sunucusu ile iletişim kurmak için ODBC protokolünü kullanır. Bir Spark kümesi oluşturduğunuzda, Spark Thrift sunucusu iki örneği başlatılır, bir baş düğümlerine. Her Spark Thrift sunucusu, YARN kullanıcı arabiriminde bir Spark uygulaması olarak görülebilir.

Spark Thrift sunucusu Spark Yürütücü dinamik ayırma kullanır ve bu nedenle `spark.executor.instances` kullanılmaz. Bunun yerine, Spark Thrift sunucusu kullanan `spark.dynamicAllocation.minExecutors` ve `spark.dynamicAllocation.maxExecutors` Yürütücü sayısı belirtmek için. Yapılandırma parametrelerini `spark.executor.cores` ve `spark.executor.memory` Yürütücü boyutunu değiştirmek için kullanılır. Aşağıdaki adımlarda gösterildiği gibi bu parametreleri değiştirebilirsiniz:

* Genişletin **spark thrift sparkconf Gelişmiş** parametreleri güncelleştirmek için kategori `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, ve `spark.executor.memory`.

    ![Spark thrift sunucusu yapılandırma](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Genişletin **özel spark thrift sparkconf** parametresi güncelleştirmek için kategori `spark.executor.cores`.

    ![Spark thrift sunucusu yapılandırma](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="change-the-driver-memory-of-the-spark-thrift-server"></a>Spark Thrift sunucusu sürücü belleğini değiştirin
Spark Thrift sunucusu sürücü bellek baş düğüm RAM boyutunu % 25 oranında yapılandırılmış, baş düğümün Toplam RAM boyutu 14 GB'den büyük sağlanır. Aşağıdaki ekran görüntüsünde gösterildiği gibi sürücü bellek yapılandırmasını değiştirmek için Ambari UI kullanabilirsiniz:

* Ambari UI'ı tıklatın **Spark**, tıklayın **yapılandırmaları**, genişletin **spark env Gelişmiş**ve ardından değeri sağlayın **spark_thrift_cmd_opts**.

    ![Spark thrift sunucusu RAM yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="reclaim-spark-cluster-resources"></a>Spark kümesi kaynaklarınıza geri kazan
Spark dinamik ayırma nedeniyle thrift sunucusu tarafından tüketilen kaynak yalnızca iki uygulama ana sunucu için kaynaklardır. Bu kaynakları geri kazanmak için küme üzerinde çalışan Thrift sunucusu Hizmetleri durdurmanız gerekir.

1. Sol bölmeden Ambari UI'ı tıklatın **Spark**.
2. Sonraki sayfada, tıklayın **Spark Thrift sunucuları**.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. Spark Thrift sunucusu üzerinde çalıştığı iki baş görmeniz gerekir. Baş düğümler birine tıklayın.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. Sonraki sayfada, baş düğüm üzerinde çalışan tüm hizmetler listelenir. Listeden Spark Thrift sunucusu yanındaki açılan düğmeyi tıklayın ve ardından **Durdur**.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Diğer baş de bu adımları tekrarlayın.

## <a name="restart-the-jupyter-service"></a>Jupyter hizmetini yeniden başlatın
Ambari Web kullanıcı arabirimini, makalenin başında gösterildiği gibi başlatın. Sol gezinti bölmesinden tıklayın **Jupyter**, tıklayın **hizmet eylemleri**ve ardından **yeniden tüm**. Bu, üzerinde tüm baş düğümler Jupyter hizmetini başlatır.

![Jupyter yeniden](./media/apache-spark-resource-manager/restart-jupyter.png "Jupyter yeniden başlatın")

## <a name="monitor-resources"></a>Kaynakları izleme
Yarn UI makalenin başında gösterildiği gibi başlatın. Ekranın en üstünde Cluster Metrics tablosunda değerlerini kontrol edin **kullanılan bellek** ve **bellek toplam** sütunları. İki değer Kapat ise sonraki uygulamayı başlatmak için yeterli kaynağı olmayabilir. Aynı geçerlidir **sanal çekirdekler kullanılan** ve **VCores Total** sütunları. Varsa Ayrıca, ana görünümünde, bir uygulama içinde stayed **kabul edilen** durumu ve içine geçiş değil **çalıştıran** ya da **başarısız** durumunda, bu da olabilir bir göstergesi, başlatmak için yeterli kaynağı alamıyor.

![Kaynak sınırına](./media/apache-spark-resource-manager/resource-limit.png "kaynak sınırı")

## <a name="kill-running-applications"></a>Çalışan uygulamaları Kaldır
1. Yarn kullanıcı arabiriminde, sol panelden tıklayın **çalıştıran**. Sonlandırılan ve tıklayarak uygulamanın çalışmakta olan uygulamalar listesinden belirlemek **kimliği**.

    ![App1'KILL](./media/apache-spark-resource-manager/kill-app1.png "App1 Sonlandır")

2. Tıklayın **KILL uygulama** üzerinde sağ üst köşede, ardından **Tamam**.

    ![App2 KILL](./media/apache-spark-resource-manager/kill-app2.png "App2 Sonlandır")

## <a name="see-also"></a>Ayrıca bkz.
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Veri analistleri için

* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight içindeki Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight Apache Spark'ı kullanarak Web sitesi günlüğü çözümlemesi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight Apache Spark'ı kullanarak application Insight telemetri verilerinin analizi](apache-spark-analyze-application-insight-logs.md)
* [Azure HDInsight Spark üzerinde dağıtılmış derin öğrenme için Caffe kullanma](apache-spark-deep-learning-caffe.md)

### <a name="for-apache-spark-developers"></a>Apache Spark geliştiricileri için

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklamak amacıyla Intellij Idea için HDInsight araçları eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight üzerinde Apache Spark kümesi ile Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
