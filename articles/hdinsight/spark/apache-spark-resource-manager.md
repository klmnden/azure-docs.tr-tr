---
title: "Azure hdınsight'ta Apache Spark küme kaynaklarını yönetme | Microsoft Docs"
description: "Nasıl kullanacağınızı öğrenin daha iyi performans için Azure hdınsight'ta Spark kümeleri için kaynakları yönetin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 24f6ce9c22aff727ef597cd7bed0c15b260b8aa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Azure hdınsight'ta Apache Spark küme kaynaklarını yönetme 

Bu makalede Ambari UI, YARN kullanıcı Arabiriminde, gibi arabirimleri erişmeyi öğrenin ve Spark geçmişi sunucunun Spark kümenizle ilişkilendirilmiş. Ayrıca küme yapılandırma en iyi performansı için ince ayar yapma hakkında bilgi.

**Ön koşullar:**

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-the-ambari-web-ui"></a>Ambari Web kullanıcı arabirimini nasıl başlatma?
1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.
2. Spark kümenizin tıklatın **Pano**. İstendiğinde, Spark küme için yönetici kimlik bilgilerini girin.

    ![Ambari başlatma](./media/apache-spark-resource-manager/hdinsight-launch-cluster-dashboard.png "Kaynak Yöneticisi'ni Başlat")
3. Ambari Web kullanıcı ARABİRİMİ'nde, bu ekran görüntüsünde gösterildiği gibi başlatın.

    ![Ambari Web kullanıcı Arabirimi](./media/apache-spark-resource-manager/ambari-web-ui.png "Ambari Web kullanıcı Arabirimi")   

## <a name="how-do-i-launch-the-spark-history-server"></a>Spark geçmişi sunucunun nasıl başlatma?
1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz).
2. Küme dikey penceresinden altında **hızlı bağlantılar**, tıklatın **küme Panosu**. İçinde **küme Panosu** dikey penceresinde tıklatın **Spark geçmişi sunucu**.

    ![Spark geçmişi sunucu](./media/apache-spark-resource-manager/launch-history-server.png "Spark geçmişi sunucu")

    İstendiğinde, Spark küme için yönetici kimlik bilgilerini girin.

## <a name="how-do-i-launch-the-yarn-ui"></a>Yarn kullanıcı arabirimini nasıl Başlat?
YARN kullanıcı arabirimini kullanarak Spark kümesi üzerinde çalışmakta olan uygulamaları izlemek için kullanabilirsiniz.

1. Küme dikey penceresinden tıklayın **küme Panosu**ve ardından **YARN**.

    ![YARN kullanıcı arabirimini Başlat](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Alternatif olarak, Ambari UI YARN Arabiriminden başlatabilirsiniz. Küme dikey penceresinden Ambari kullanıcı arabirimini başlatmak için tıklatın **küme Panosu**ve ardından **Hdınsight küme Panosu**. Ambari Arabiriminden tıklatın **YARN**, tıklatın **hızlı bağlantılar**, etkin Kaynak Yöneticisi'ni tıklatın ve ardından **kaynak yöneticisi kullanıcı Arabirimi**.
   >
   >

## <a name="what-is-the-optimum-cluster-configuration-to-run-spark-applications"></a>Spark uygulamaları çalıştırmak için en uygun Küme Yapılandırması nedir?
Uygulama gereksinimleri bağlı olarak Spark yapılandırması için kullanılabilir üç anahtar parametreleri `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir yürütücü Spark uygulama için başlatılan bir işlemdir. Çalışan düğümünde çalışır ve uygulama için görevleri gerçekleştiremeyen sorumludur. Yürütücüler ve her küme için Yürütücü boyutları varsayılan sayısı çalışan düğümleri ve alt düğüm boyutu sayısına göre hesaplanır. Bu bilgiler depolanır `spark-defaults.conf` küme baş düğümler.

Üç yapılandırma parametrelerini (için küme üzerinde çalışan tüm uygulamaları) küme düzeyinde yapılandırılabilir veya her tek tek uygulama için belirtilebilir.

### <a name="change-the-parameters-using-ambari-ui"></a>Ambari kullanıcı arabirimini kullanarak parametrelerini değiştirin
1. Ambari UI'ı tıklatın **Spark**, tıklatın **Contigs**, genişletin ve ardından **özel spark-varsayılanları**.

    ![Ambari kullanarak parametrelerini](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Varsayılan olarak 4 Spark uygulamaları kümede aynı anda çalıştırmak iyi değerlerdir. Aşağıda gösterildiği gibi kullanıcı arabiriminden bu değerleri değiştirebilirsiniz.

    ![Ambari kullanarak parametrelerini](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Tıklatın **kaydetmek** yapılandırma değişikliklerini kaydetmek için. Sayfanın üst kısmında, etkilenen tüm hizmetleri yeniden başlatmanız istenir. Tıklatın **yeniden**.

    ![Hizmetlerini yeniden başlatın](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter not defteri çalışan bir uygulama için parametrelerini değiştir
Jupyter not defteri çalışan uygulamalar için kullandığınız `%%configure` magic yapılandırma değişikliklerini yapın. İdeal olarak, birinci kod hücresini çalıştırmadan önce uygulama başlangıcında gibi değişiklikler yapmanız gerekir. Bunun yapılması sağlar yapılandırmasını oluşturulduğu Livy oturumuna uygulanır. Uygulamasında daha sonraki bir aşamada yapılandırmasını değiştirmek istiyorsanız, kullanmalısınız `-f` parametresi. Ancak, uygulamadaki tüm ilerleme kaybolacak göre yapılıyor.

Aşağıdaki kod parçacığında Jupyter'de çalışan bir uygulama yapılandırmasını değiştirmek nasıl gösterir.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Yapılandırma parametreleri de JSON dizesi olarak geçirilmelidir ve sonraki satırında Sihirli sonra örnek sütununda gösterildiği gibi olması gerekir.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Değiştirme parametreleri kullanılarak gönderilen bir uygulama için spark gönderme
Aşağıdaki komut kullanılarak gönderilen bir batch uygulaması için yapılandırma parametreleri değiştirmek nasıl bir örneği verilmiştir `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>CURL kullanılarak gönderilen bir uygulama için parametrelerini değiştir
Komutu aşağıdaki cURL kullanarak gönderilen bir batch uygulaması için yapılandırma parametreleri değiştirmek nasıl bir örnektir.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Spark Thrift sunucusunun bu parametrelere nasıl değişiyor?
Spark Thrift sunucusunun bir Spark kümesi JDBC/ODBC erişim sağlar ve hizmet Spark SQL sorguları için kullanılır. Araçlar Power BI, Tableau vb. gibi çalışır. bir Spark uygulaması olarak Spark SQL sorguları yürütmek için Spark Thrift sunucusuyla iletişim kurmak için ODBC protokolünü kullanır. Spark kümesi oluşturduğunuzda, Spark Thrift sunucusunun iki örneği başlatılır, her baş düğüm üzerinde bir tane. Her Spark Thrift sunucusunun Spark uygulama YARN kullanıcı Arabirimi olarak görünür olur.

Spark Thrift sunucusunun kullandığı Spark dinamik Yürütücü ayırma ve bu nedenle `spark.executor.instances` kullanılmaz. Bunun yerine, Spark Thrift sunucusunun kullandığı `spark.dynamicAllocation.minExecutors` ve `spark.dynamicAllocation.maxExecutors` Yürütücü sayısını belirtmek için. Yapılandırma parametrelerini `spark.executor.cores` ve `spark.executor.memory` Yürütücü boyutunu değiştirmek için kullanılır. Aşağıdaki adımlarda gösterildiği gibi bu parametreleri değiştirebilirsiniz.

* Genişletme **spark thrift sparkconf Gelişmiş** parametreleri güncelleştirmek için kategori `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, ve `spark.executor.memory`.

    ![Spark thrift sunucusunu yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Genişletme **özel spark thrift sparkconf** parametresi güncelleştirmek için kategori `spark.executor.cores`.

    ![Spark thrift sunucusunu yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-the-driver-memory-of-the-spark-thrift-server"></a>Spark Thrift sunucusunun sürücü bellek nasıl değişiyor?
Spark Thrift sunucusunun sürücü bellek baş düğüm RAM boyutunu % 25 oranında yapılandırılmış, 14 GB'den büyük baş düğüm Toplam RAM boyutu sağlanır. Aşağıda gösterildiği gibi sürücü bellek yapılandırmasını değiştirmek için Ambari kullanıcı Arabirimi kullanabilirsiniz.

* Ambari UI'ı tıklatın **Spark**, tıklatın **yapılandırmalar**, genişletin **spark env Gelişmiş**ve ardından değeri sağlayın **spark_thrift_cmd_opts**.

    ![Spark thrift sunucusunun RAM yapılandırın](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-the-resources-back"></a>I BI ile Spark kümesi kullanmayın. Nasıl kaynakları geri yararlanabilir mi?
Spark dinamik ayırma kullandığımız bu yana thrift sunucu tarafından tüketilen yalnızca iki uygulama yöneticileri için kaynakları kaynaklardır. Bu kaynakları geri kazanmak için küme üzerinde çalışan Thrift Server hizmetlerini durdurmanız gerekir.

1. Sol bölmeden Ambari UI'ı tıklatın **Spark**.
2. Sonraki sayfaya tıklatın **Spark Thrift sunucuları**.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. Spark Thrift sunucusunun çalıştığı iki headnodes görmeniz gerekir. Headnodes birini tıklatın.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. Sonraki sayfada, bu headnode üzerinde çalışan tüm hizmetleri listeler. Listeden Spark Thrift sunucusunun yanında aşağı açılan düğmesine tıklayın ve ardından **durdurmak**.

    ![Thrift sunucuyu yeniden başlatın](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Diğer headnode de bu adımları yineleyin.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-the-service"></a>My Jupyter not defterleri beklendiği gibi çalışmıyor. Hizmet nasıl yeniden başlatabilirsiniz?
Ambari Web kullanıcı arabirimini yukarıda gösterildiği gibi başlatın. Sol gezinti bölmesinden tıklatın **Jupyter**, tıklatın **hizmet eylemleri**ve ardından **yeniden tüm**. Bu Jupyter hizmet tüm headnodes başlatır.

    ![Restart Jupyter](./media/apache-spark-resource-manager/restart-jupyter.png "Restart Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resources"></a>Kaynaklar yetersiz çalıştırdığım olmadığını nasıl anlayabilirim?
Yarn kullanıcı Arabiriminde, yukarıda gösterildiği gibi başlatın. Ekranın en üstünde küme ölçümlerini tabloda değerlerini kontrol **kullanılan bellek** ve **bellek toplam** sütun. 2 değerleri çok yakın varsa, sonraki uygulamayı başlatmak için yeterli kaynağı olmayabilir. Aynı durum geçerlidir **VCores kullanılan** ve **VCores toplam** sütun. Varsa aynı zamanda, ana görünümünde, bir uygulama içinde stayed **kabul edilen** durumu ve içine geçiş değil **çalıştıran** ya da **başarısız** durumunda, bu da olabilir göstergesidir, başlatmak için yeterli kaynakları alamıyorsanız.

    ![Resource Limit](./media/apache-spark-resource-manager/resource-limit.png "Resource Limit")

## <a name="how-do-i-kill-a-running-application-to-free-up-resource"></a>Kaynak boşaltmak için çalışan bir uygulama nasıl KILL?
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
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](apache-spark-eventhub-streaming.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
