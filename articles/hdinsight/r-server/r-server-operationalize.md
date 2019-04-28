---
title: HDInsight - Azure ML Hizmetleri kullanıma hazır hale getirme
description: Azure HDInsight, ML Hizmetleri kullanıma hazır hale getirme hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: 916c4fae8eed9451080f92e97743876e89bd25ea
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124586"
---
# <a name="operationalize-ml-services-cluster-on-azure-hdinsight"></a>ML Hizmetleri Azure HDInsight kümesinde çalışır hale getirme

Verilerinizi modelleme tamamlamak için HDInsight küme ML Hizmetleri kullandıktan sonra Öngörüler bulunmak üzere modelinizi kullanıma hazır hale getirebilirsiniz. Bu makalede, bu görevi gerçekleştirme hakkında yönergeler sağlar.

## <a name="prerequisites"></a>Önkoşullar

* **Bir HDInsight ML Hizmetleri kümesinde**: Yönergeler için [ML Hizmetleri HDInsight kullanmaya başlama](r-server-get-started.md).

* **Güvenli Kabuk (SSH) istemcisi**: Bir SSH istemcisi uzaktan HDInsight kümesine bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="operationalize-ml-services-cluster-with-one-box-configuration"></a>ML Hizmetleri kümesi one-box yapılandırması ile kullanıma hazır hale getirme

> [!NOTE]  
> Aşağıdaki adımlar, R Server 9.0 ve ML Server 9.1 için geçerlidir. ML Server 9.3 için başvurmak [kullanıma hazır hale getirme yapılandırmasını yönetmek için yönetim aracını kullanın](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-launch).

1. Kenar düğümüne SSH uygulayın.

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Azure HDInsight ile SSH kullanma hakkında yönergeler için bkz: [HDInsight ile SSH kullanma.](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Dot net dll dosyasına sudo uygulayın ve ilgili sürümü için dizini değiştirin: 

    - Microsoft ML Server 9.1 için:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Microsoft R Server 9.0 için:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

1. Aralarından seçim yapabileceğiniz seçeneklerle sunulur. Birinci seçenek için aşağıdaki ekran görüntüsünde gösterildiği gibi seçin **kullanıma hazır hale getirme ML Server'ı Yapılandır**.

    ![one box işlemi](./media/r-server-operationalize/admin-util-one-box-1.png)

1. Artık, ML Server kullanıma hazır hale getirmek istediğiniz şekli seçin seçeneği de sunulur. Sunulan seçeneklerden girerek birinci seçin **A**.

    ![one box işlemi](./media/r-server-operationalize/admin-util-one-box-2.png)

1. İstendiğinde girin ve bir yerel yönetici kullanıcı için parolayı yeniden girin.

1. Öneren işleminin başarılı olduğunu çıkışları görmeniz gerekir. Menüden başka bir seçenek belirlemek için istenir. Ana menüye dönmek için select E.

    ![one box işlemi](./media/r-server-operationalize/admin-util-one-box-3.png)

1. İsteğe bağlı olarak, şu şekilde bir tanılama testi çalıştırarak tanılama denetimleri gerçekleştirebilirsiniz:

    a. Ana menüden **6** tanılama testleri çalıştırmak için.

    ![one box işlemi](./media/r-server-operationalize/diagnostic-1.png)

    b. Tanılama sınamaları menüden **A**. İstendiğinde, yerel yönetici kullanıcı için belirttiğiniz parolayı girin.

    ![one box işlemi](./media/r-server-operationalize/diagnostic-2.png)

    c. Çıkış genel sistem durumu bir geçiş olduğunu gösterdiğini doğrulayın.

    ![one box işlemi](./media/r-server-operationalize/diagnostic-3.png)

    d. Sunulan menü seçeneklerinden girin **E** ana menüye dönmek ve girmeniz **8** için yönetim yardımcı programından çıkın.

### <a name="long-delays-when-consuming-web-service-on-apache-spark"></a>Apache Spark üzerinde Web hizmeti tüketilirken uzun gecikmeler

İle oluşturulmuş bir web hizmetini tüketmeye çalışırken uzun gecikmeler yaşıyorsanız işlem bağlamında mrsdeploy bir Apache Spark, bazı eksik klasörleri eklemeniz gerekebilir. Spark uygulaması mrsdeoploy işlevleri kullanılarak bir web hizmetinden çağrıldığında '*rserve2*' adlı bir kullanıcıya ait oluyor. Bu soruna geçici bir çözüm olarak:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


Bu aşamada kullanıma hazır hale getirme yapılandırması tamamlanmıştır. Artık `mrsdeploy` kenar düğümünün kullanıma hazır hale bağlanmak ve gibi özellikleri kullanmaya başlamak için RClient üzerindeki paketi [uzaktan yürütme](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) ve [web Hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services). Kümenizin bir sanal ağda ayarlanıp ayarlanmamasına bağlı olarak, SSH oturumu aracılığıyla bağlantı noktası iletme tüneli ayarlamanız gerekebilir. Aşağıdaki bölümlerde bu tüneli nasıl kuracağınız açıklanmaktadır.

### <a name="ml-services-cluster-on-virtual-network"></a>Sanal ağda ML Hizmetleri kümesi

12800 numaralı bağlantı noktası üzerinden kenar düğümüne trafik akışına izin verdiğinizden emin olun. Bu şekilde, Kullanıma Hazır Hale Getirme özelliğine bağlanmak için kenar düğümünü kullanabilirsiniz.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


`remoteLogin()` kenar düğümüne bağlanamadığı halde kenar düğümüne SSH uygulayabiliyorsanız, 12800 numaralı bağlantı noktası üzerinde trafiğe izin veren kuralın doğru şekilde ayarlanıp ayarlanmadığını doğrulamanız gerekir. Sorunla karşılaşmaya devam ederseniz, SSH üzerinden bağlantı noktası iletme tüneli oluşturarak bir geçici çözüm uygulayabilirsiniz. Yönergeler için aşağıdaki bölüme bakın:

### <a name="ml-services-cluster-not-set-up-on-virtual-network"></a>ML Hizmetleri küme sanal ağ üzerinde ayarlanmadı

Kümeniz sanal üzerinde ayarlanmamışsa veya sanal ağ üzerinden bağlantı kurma sorunları yaşıyorsanız, SSH bağlantı noktası iletme tünelini kullanabilirsiniz:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

SSH oturumunuz etkin hale geldikten sonra yerel makinenizin 12800 numaralı bağlantı noktası gelen trafik, bağlantı noktasına kenar düğümünün 12800 SSH oturumu aracılığıyla iletilir. `remoteLogin()` yönteminizde `127.0.0.1:12800` kullandığınızdan emin olun. Bu bağlantı noktası iletme yoluyla uç düğümün kullanıma hazır hale getirme halinde günlüğe kaydeder.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>İşlem düğümleri HDInsight çalışan düğümlerinde kullanıma hazır hale getirdiniz ölçeklendirme

İşlem düğümlerini ölçeklendirmek için önce çalışan düğümlerinin yetkisini alma ve işlem düğümleri üzerinde yetkisi alınmış çalışan düğümlerinin yapılandırın.

### <a name="step-1-decommission-the-worker-nodes"></a>1. Adım: Çalışan düğümlerinin yetkisini alma

ML Hizmetleri kümesi aracılığıyla yönetilen değil [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html). Çalışan düğümlerinin yetkisi alınmazsa, YARN kaynak yöneticisi sunucu tarafından alınan kaynakları farkında olmadığından beklendiği gibi çalışmaz. Bu durumu önlemek için, işlem düğümlerini ölçeklendirmeden önce çalışan düğümlerinin yetkisinin alınması önerilir.

Çalışan düğümlerinin yetkisini almak için aşağıdaki adımları izleyin:

1. Kümesinin Ambari konsolunda oturum açın ve tıklayarak **konakları** sekmesi.

1. Çalışan düğümlerini (yetkisi alınmış olması için) seçin.

1. Tıklayın **eylemleri** > **seçili Konaklar** > **konakları** > **bakım Modu'nu**. Örneğin, aşağıdaki görüntüde yetkisini almak üzere wn3 ve wn4 seçilmiştir.  

   ![çalışan düğümlerinin yetkisini alma](./media/r-server-operationalize/get-started-operationalization.png)  

* Seçin **eylemleri** > **seçili ana bilgisayarlar** > **DataNodes** > tıklatın **yetkisini Al**.
* Seçin **eylemleri** > **seçili ana bilgisayarlar** > **NodeManagers** > tıklatın **yetkisini Al**.
* Seçin **eylemleri** > **seçili ana bilgisayarlar** > **DataNodes** > tıklatın **Durdur**.
* Seçin **eylemleri** > **seçili ana bilgisayarlar** > **NodeManagers** > tıklayarak **Durdur**.
* Seçin **eylemleri** > **seçili ana bilgisayarlar** > **konakları** > tıklatın **tüm bileşenleri Durdur**.
* Çalışan düğümlerinin seçimini kaldırın ve baş düğümleri seçin.
* Seçin **eylemleri** > **seçili Konaklar** > "**konakları** > **tüm bileşenleri yeniden Başlat**.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>2. Adım: Her yetkisi alınmış çalışan düğümü üzerinde işlem düğümlerini yapılandırın

1. Yetkisi alınan her çalışan düğümüne SSH uygulayın.

1. Sahip olduğunuz ML Hizmetleri kümesi için ilgili DLL kullanarak yönetici yardımcı programını çalıştırın. ML Server 9.1 için şu komutu çalıştırın:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

1. Girin **1** seçeneğini **kullanıma hazır hale getirme ML Server'ı Yapılandır**.

1. Girin **C** seçeneğini `C. Compute node`. Bu işlem çalışan düğümündeki işlem düğümünü yapılandırır.

1. Yönetim Yardımcı Programından çıkın.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>3. Adım: Web düğümüne işlem düğümlerinin ayrıntılarını ekleme

Tüm yetkisi alınmış çalışan düğümleri işlem düğümü çalışacak şekilde yapılandırıldıktan sonra kenar düğümüne geri dönün ve yetkisi alınmış çalışan düğümlerinin IP adreslerini ML Server web düğümünün yapılandırmasına ekleyin:

1. Kenar düğümüne SSH uygulayın.

1. `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` öğesini çalıştırın.

1. "URI'ler" bölümüne bakın ve çalışan düğümünün IP ve bağlantı noktası ayrıntılarını ekleyin.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde ML Services kümesini yönetme](r-server-hdinsight-manage.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde ML Services kümesi için Azure Depolama seçenekleri](r-server-storage.md)
