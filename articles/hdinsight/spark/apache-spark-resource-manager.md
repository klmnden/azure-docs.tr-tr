---
title: "Azure hdınsight'ta Apache Spark küme kaynaklarını yönetme | Microsoft Docs"
description: "Nasıl kullanacağınızı öğrenin daha iyi performans için Azure hdınsight'ta Spark kümeleri için kaynakları yönetin."
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: jgao
ms.openlocfilehash: 914811812b7e01f7b58f92c85cb5a16580c45796
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Azure hdınsight'ta Apache Spark küme kaynaklarını yönetme 

Ambari UI, YARN kullanıcı Arabiriminde ve Spark geçmişi Spark kümenizle ilişkilendirilmiş sunucu gibi arabirimleri ulaşma ve en iyi performans için küme yapılandırmayı ayarlamak nasıl öğrenin.

**Ön koşullar:**

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="open-the-ambari-web-ui"></a>Open the Ambari Web UI

Apache Ambari, kümeyi izlemek ve yapılandırma değişiklikleri yapmak için kullanılır. Daha fazla bilgi için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure portalını kullanarak](../hdinsight-administer-use-portal-linux.md#open-the-ambari-web-ui)

## <a name="open-the-spark-history-server"></a>Spark geçmişi sunucu açın

Spark geçmişi tamamlanmış ve çalışan Spark uygulamaları için kullanıcı Arabirimi web sunucusudur. Spark'ın Web kullanıcı Arabirimi, bir uzantısıdır.

**Spark geçmişi sunucu Web kullanıcı arabirimini açmak için**

1. Gelen [Azure portal](https://portal.azure.com/), Spark kümesi'ni açın. Daha fazla bilgi için bkz: [listesi ve Göster kümeleri](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Gelen **hızlı bağlantılar**, tıklatın **küme Panosu**ve ardından **Spark geçmişi sunucu**

    ![Spark geçmişi sunucu](./media/apache-spark-resource-manager/launch-history-server.png "Spark geçmişi sunucu")

    İstendiğinde, Spark küme için yönetici kimlik bilgilerini girin. Aşağıdaki URL'ye göz atarak Spark geçmişi sunucunun da açabilirsiniz:

    ```
    https://<ClusterName>.azurehdinsight.net/sparkhistory
    ```

    Değiştir <ClusterName> ile Spark kümenizin adıdır.

Spark geçmişi sunucu web kullanıcı Arabirimi şuna benzer:

![Hdınsight Spark geçmişi sunucu](./media/apache-spark-resource-manager/hdinsight-spark-history-server.png)

## <a name="open-the-yarn-ui"></a>Yarn kullanıcı arabirimini açın
YARN kullanıcı arabirimini kullanarak Spark kümesi üzerinde çalışmakta olan uygulamaları izlemek için kullanabilirsiniz.

1. Gelen [Azure portal](https://portal.azure.com/), Spark kümesi'ni açın. Daha fazla bilgi için bkz: [listesi ve Göster kümeleri](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Gelen **hızlı bağlantılar**, tıklatın **küme Panosu**ve ardından **YARN**.

    ![YARN kullanıcı arabirimini Başlat](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Alternatif olarak, Ambari UI YARN Arabiriminden başlatabilirsiniz. Ambari kullanıcı arabirimini başlatmak için tıklatın **küme Panosu**ve ardından **Hdınsight küme Panosu**. Ambari Arabiriminden tıklatın **YARN**, tıklatın **hızlı bağlantılar**, etkin Kaynak Yöneticisi'ni tıklatın ve ardından **kaynak yöneticisi kullanıcı Arabirimi**.
   >
   >

## <a name="optimize-clusters-for-spark-applications"></a>Kümeler Spark uygulamalar için en iyi duruma getirme
Uygulama gereksinimleri bağlı olarak Spark yapılandırması için kullanılabilir üç anahtar parametreleri `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir yürütücü Spark uygulama için başlatılan bir işlemdir. Çalışan düğümünde çalışır ve uygulama için görevleri gerçekleştiremeyen sorumludur. Yürütücüler ve her küme için Yürütücü boyutları varsayılan sayısı çalışan düğümleri ve alt düğüm boyutu sayısına göre hesaplanır. Bu bilgiler depolanır `spark-defaults.conf` küme baş düğümler.

Üç yapılandırma parametrelerini (için küme üzerinde çalışan tüm uygulamaları) küme düzeyinde yapılandırılabilir veya her tek tek uygulama için belirtilebilir.

### <a name="change-the-parameters-using-ambari-ui"></a>Ambari kullanıcı arabirimini kullanarak parametrelerini değiştirin
1. Ambari UI'ı tıklatın **Spark**, tıklatın **yapılandırmalar**, genişletin ve ardından **özel spark-varsayılanları**.

    ![Ambari kullanarak parametrelerini](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Aynı anda kümede çalışan dört Spark uygulamalarında iyi varsayılan değerlerdir. Aşağıdaki ekran görüntüsünde gösterildiği gibi kullanıcı arabiriminden bu değerleri değiştirebilirsiniz:

    ![Ambari kullanarak parametrelerini](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Tıklatın **kaydetmek** yapılandırma değişikliklerini kaydetmek için. Sayfanın üst kısmında, etkilenen tüm hizmetleri yeniden başlatmanız istenir. Tıklatın **yeniden**.

    ![Hizmetlerini yeniden başlatın](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter not defteri çalışan bir uygulama için parametrelerini değiştir
Jupyter not defteri çalışan uygulamalar için kullandığınız `%%configure` magic yapılandırma değişikliklerini yapın. İdeal olarak, birinci kod hücresini çalıştırmadan önce uygulama başlangıcında gibi değişiklikler yapmanız gerekir. Bunun yapılması sağlar yapılandırmasını oluşturulduğu Livy oturumuna uygulanır. Uygulamasında daha sonraki bir aşamada yapılandırmasını değiştirmek istiyorsanız, kullanmalısınız `-f` parametresi. Ancak, bu sayede tüm uygulama kayıp sürüyor.

Aşağıdaki kod parçacığında Jupyter'de çalışan bir uygulama yapılandırmasını değiştirmek nasıl gösterir.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Yapılandırma parametreleri de JSON dizesi olarak geçirilmelidir ve sonraki satırında Sihirli sonra örnek sütununda gösterildiği gibi olması gerekir.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Değiştirme parametreleri kullanılarak gönderilen bir uygulama için spark gönderme
Aşağıdaki komut kullanılarak gönderilen bir batch uygulaması için yapılandırma parametreleri değiştirmek nasıl bir örneği verilmiştir `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>CURL kullanılarak gönderilen bir uygulama için parametrelerini değiştir
Aşağıdaki komutu cURL kullanılarak gönderilen bir batch uygulaması için yapılandırma parametreleri değiştirmek nasıl bir örnektir.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="change-these-parameters-on-a-spark-thrift-server"></a>Spark Thrift sunucusunun bu parametrelere değiştirme
Spark Thrift sunucusunun bir Spark kümesi JDBC/ODBC erişim sağlar ve hizmet Spark SQL sorguları için kullanılır. Araçlar Power BI, Tableau vb. gibi çalışır. bir Spark uygulaması olarak Spark SQL sorguları yürütmek için Spark Thrift sunucusuyla iletişim kurmak için ODBC protokolünü kullanır. Spark kümesi oluşturduğunuzda, Spark Thrift sunucusunun iki örneği başlatılır, her baş düğüm üzerinde bir tane. Her Spark Thrift sunucusunun Spark uygulama YARN kullanıcı Arabirimi olarak görünür olur.

Spark Thrift sunucusunun kullandığı Spark dinamik Yürütücü ayırma ve bu nedenle `spark.executor.instances` kullanılmaz. Bunun yerine, Spark Thrift sunucusunun kullandığı `spark.dynamicAllocation.minExecutors` ve `spark.dynamicAllocation.maxExecutors` Yürütücü sayısını belirtmek için. Yapılandırma parametrelerini `spark.executor.cores` ve `spark.executor.memory` Yürütücü boyutunu değiştirmek için kullanılır. Aşağıdaki adımlarda gösterildiği gibi bu parametrelerini değiştirebilirsiniz:

* Genişletme **spark thrift sparkconf Gelişmiş** parametreleri güncelleştirmek için kategori `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, ve `spark.executor.memory`.

    ![Spark thrift sunucusunu yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Genişletme **özel spark thrift sparkconf** parametresi güncelleştirmek için kategori `spark.executor.cores`.

    ![Spark thrift sunucusunu yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="change-the-driver-memory-of-the-spark-thrift-server"></a>Spark Thrift sunucusunun sürücü bellek değiştirme
Spark Thrift sunucusunun sürücü bellek baş düğüm RAM boyutunu % 25 oranında yapılandırılmış, 14 GB'den büyük baş düğüm Toplam RAM boyutu sağlanır. Aşağıdaki ekran görüntüsünde gösterildiği gibi sürücü bellek yapılandırmasını değiştirmek için Ambari UI kullanabilirsiniz:

* Ambari UI'ı tıklatın **Spark**, tıklatın **yapılandırmalar**, genişletin **spark env Gelişmiş**ve ardından değeri sağlayın **spark_thrift_cmd_opts**.

    ![Spark thrift sunucusunun RAM yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="reclaim-spark-cluster-resources"></a>Spark küme kaynaklarını geri kazan
Spark dinamik ayırma nedeniyle thrift sunucu tarafından tüketilen yalnızca iki uygulama yöneticileri için kaynakları kaynaklardır. Bu kaynakları geri kazanmak için küme üzerinde çalışan Thrift Server hizmetlerini durdurmanız gerekir.

1. Sol bölmeden Ambari UI'ı tıklatın **Spark**.
2. Sonraki sayfaya tıklatın **Spark Thrift sunucuları**.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. Spark Thrift sunucusunun çalıştığı iki headnodes görmeniz gerekir. Headnodes birini tıklatın.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. Sonraki sayfada, bu headnode üzerinde çalışan tüm hizmetleri listeler. Listeden Spark Thrift sunucusunun yanında aşağı açılan düğmesine tıklayın ve ardından **durdurmak**.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Diğer headnode de bu adımları yineleyin.

## <a name="restart-the-jupyter-service"></a>Jupyter hizmetini yeniden başlatın
Ambari Web kullanıcı arabirimini makale başına de gösterildiği gibi başlatın. Sol gezinti bölmesinden tıklatın **Jupyter**, tıklatın **hizmet eylemleri**ve ardından **yeniden tüm**. Bu, üzerinde tüm headnodes Jupyter hizmetini başlatır.

![Jupyter yeniden](./media/apache-spark-resource-manager/restart-jupyter.png "yeniden Jupyter")

## <a name="monitor-resources"></a>Kaynakları izleme
Yarn kullanıcı Arabiriminde makale başına de gösterildiği gibi başlatın. Ekranın en üstünde küme ölçümlerini tabloda değerlerini kontrol **kullanılan bellek** ve **bellek toplam** sütun. İki değer Kapat varsa, sonraki uygulamayı başlatmak için yeterli kaynağı olmayabilir. Aynı durum geçerlidir **VCores kullanılan** ve **VCores toplam** sütun. Varsa aynı zamanda, ana görünümünde, bir uygulama içinde stayed **kabul edilen** durumu ve içine geçiş değil **çalıştıran** ya da **başarısız** durumunda, bu da olabilir göstergesidir, başlatmak için yeterli kaynakları alamıyorsanız.

![Kaynak sınırına](./media/apache-spark-resource-manager/resource-limit.png "kaynak sınırı")

## <a name="kill-running-applications"></a>Çalışan uygulamaları Sonlandır
1. Sol panelindeki Yarn kullanıcı arabiriminde tıklatın **çalıştıran**. Çalışan uygulamalar listesinden, uygulamanın sonlandırıldı ve tıklayın belirlemek **kimliği**.

    ![App1 KILL](./media/apache-spark-resource-manager/kill-app1.png "KILL App1")

2. Tıklatın **KILL uygulama** üzerinde sağ üst köşede, ardından **Tamam**.

    ![App2 KILL](./media/apache-spark-resource-manager/kill-app2.png "KILL App2")

## <a name="see-also"></a>Ayrıca bkz.
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

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
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
