---
title: Hdınsight'ta - Azure ML Hizmetleri faaliyete | Microsoft Docs
description: Azure hdınsight'ta ML Hizmetleri faaliyete öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: caefe30ff567a5e24e1f4c3a11309bd35e06190c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37046148"
---
# <a name="operationalize-ml-services-cluster-on-azure-hdinsight"></a>Azure Hdınsight kümesinde ML Hizmetleri faaliyete

Modelleme verilerinizi tamamlamak için Hdınsight'ta ML Hizmetleri küme kullandıktan sonra tahminlerde modelini faaliyete geçirebilirsiniz. Bu makalede, bu görevi gerçekleştirme hakkında yönergeler sağlar.

## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight ML Hizmetleri kümede**: yönergeler için bkz: [hdınsight'ta ML hizmetleri kullanmaya başlama](r-server-get-started.md).

* **Güvenli Kabuk (SSH) istemcisi**: HDInsight kümesine uzaktan bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için bir SSH istemcisi kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="operationalize-ml-services-cluster-with-one-box-configuration"></a>Bir çalıştırma yapılandırmaya sahip ML Hizmetleri kümeye faaliyete

> [!NOTE]
> Aşağıdaki adımlar, R Server 9.0 ve ML sunucu 9.1 için geçerlidir. ML sunucu 9.3 için başvurmak [operationalization yapılandırmasını yönetmek için yönetim aracını kullanın](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-launch).

1. Kenar düğümüne SSH uygulayın.

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Azure Hdınsight ile SSH kullanma hakkında daha fazla yönerge için bkz: [kullanmak ile SSH Hdınsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Sudo dot net dll ve ilgili sürümü için dizini değiştirin: 

    - Microsoft ML Server 9.1:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Microsoft R Server 9.0 için:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Aralarından seçim yapabileceğiniz seçeneklerle sunulur. İlk seçeneği için aşağıdaki ekran görüntüsünde gösterildiği gibi belirleyin **Operationalization için ML Server'ı Yapılandır**.

    ![one box işlemi](./media/r-server-operationalize/admin-util-one-box-1.png)

4. Şimdi nasıl ML sunucu faaliyete istediğinizi seçmek için seçeneğiyle sunulur. Girerek birinci sunulan seçeneklerden birini **A**.

    ![one box işlemi](./media/r-server-operationalize/admin-util-one-box-2.png)

5. İstendiğinde, girin ve bir yerel yönetici kullanıcı için parolayı yeniden girin.

6. İşlemi başarılı öneren çıktı görmeniz gerekir. Menüden başka bir seçenek seçin istenir. Ana menüye dönmek için select E.

    ![one box işlemi](./media/r-server-operationalize/admin-util-one-box-3.png)

7. İsteğe bağlı olarak, aşağıdaki gibi bir tanılama testi çalıştırarak tanılama denetimleri gerçekleştirebilirsiniz:

    a. Ana menüden seçin **6** tanılama testleri çalıştırmak için.

    ![one box işlemi](./media/r-server-operationalize/diagnostic-1.png)

    b. Tanılama testleri menüsünden seçin **A**. İstendiğinde, yerel yönetici kullanıcı için sağlanan parolayı girin.

    ![one box işlemi](./media/r-server-operationalize/diagnostic-2.png)

    c. Çıktı genel sistem durumu bir geçiş olduğunu gösterdiğini doğrulayın.

    ![one box işlemi](./media/r-server-operationalize/diagnostic-3.png)

    d. Sunulan menü seçeneklerinden girin **E** ana menüye dönmek ve girmeniz **8** yönetim yardımcı programı'ndan çıkmak için.

### <a name="long-delays-when-consuming-web-service-on-spark"></a>Web hizmeti Spark üzerinde kullanırken gecikmelere

Bir Spark işlem bağlamında mrsdeploy ile oluşturulmuş bir web hizmetini tüketmeye çalışırken uzun gecikmeler yaşıyorsanız bazı eksik klasörleri eklemeniz gerekebilir. Spark uygulaması mrsdeoploy işlevleri kullanılarak bir web hizmetinden çağrıldığında '*rserve2*' adlı bir kullanıcıya ait oluyor. Bu soruna geçici bir çözüm olarak:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


Bu aşamada kullanıma hazır hale getirme yapılandırması tamamlanmıştır. Kullanabileceğiniz artık `mrsdeploy` edge düğüm üzerinde operationalization bağlanmak ve özellikleri gibi kullanmaya başlamak için RClient paketi [uzaktan yürütme](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) ve [web Hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services). Kümenizin bir sanal ağda ayarlanıp ayarlanmamasına bağlı olarak, SSH oturumu aracılığıyla bağlantı noktası iletme tüneli ayarlamanız gerekebilir. Aşağıdaki bölümlerde bu tüneli nasıl kuracağınız açıklanmaktadır.

### <a name="ml-services-cluster-on-virtual-network"></a>ML Hizmetleri kümesi sanal ağ üzerinde

12800 numaralı bağlantı noktası üzerinden kenar düğümüne trafik akışına izin verdiğinizden emin olun. Bu şekilde, Kullanıma Hazır Hale Getirme özelliğine bağlanmak için kenar düğümünü kullanabilirsiniz.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


`remoteLogin()` kenar düğümüne bağlanamadığı halde kenar düğümüne SSH uygulayabiliyorsanız, 12800 numaralı bağlantı noktası üzerinde trafiğe izin veren kuralın doğru şekilde ayarlanıp ayarlanmadığını doğrulamanız gerekir. Sorunla karşılaşmaya devam ederseniz, SSH üzerinden bağlantı noktası iletme tüneli oluşturarak bir geçici çözüm uygulayabilirsiniz. Yönergeler için aşağıdaki bölüme bakın:

### <a name="ml-services-cluster-not-set-up-on-virtual-network"></a>ML Hizmetleri küme sanal ağ üzerinde ayarlanmamış

Kümeniz sanal üzerinde ayarlanmamışsa veya sanal ağ üzerinden bağlantı kurma sorunları yaşıyorsanız, SSH bağlantı noktası iletme tünelini kullanabilirsiniz:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

SSH oturumunuz etkin hale geldikten sonra, makinenizin 12800 numaralı bağlantı noktasından giden trafik, SSH aracılığıyla kenar düğümünün 12800 numaralı bağlantı noktasına iletilir. `remoteLogin()` yönteminizde `127.0.0.1:12800` kullandığınızdan emin olun. Bu bağlantı noktası iletme aracılığıyla kenar düğümün operationalization içine günlüğe kaydeder.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>İşlem düğümleri Hdınsight çalışan düğümleri üzerinde kullanıma hazır hale getirilmiş ölçeklendirme

İşlem düğümleri ölçeklendirmek için önce çalışan düğümleri yetkisini alma ve işlem düğümleri yetkisi alınmış çalışan düğümlerine yapılandırın.

### <a name="step-1-decommission-the-worker-nodes"></a>1. adım: çalışan düğümleri yetkisini alma

ML Hizmetleri küme YARN yönetilmez. Çalışan düğümü yetkisi alınmış emin değilseniz, YARN Kaynak Yöneticisi, sunucu tarafından gerçekleştirilecek kaynakları farkında olmadığından beklendiği gibi çalışmaz. Bu durumu önlemek için, işlem düğümlerini ölçeklendirmeden önce çalışan düğümlerinin yetkisinin alınması önerilir.

Çalışan düğümleri yetkisini almak için aşağıdaki adımları izleyin:

1. Kümenin Ambari konsoluna oturum açın ve tıklayın **ana** sekmesi.

2. (Kullanımdan alınmış olması için) çalışan düğümü seçin.

3. Tıklatın **Eylemler** > **konakları seçili** > **ana** > **bakım Modu'nu**. Örneğin, aşağıdaki görüntüde yetkisini almak üzere wn3 ve wn4 seçilmiştir.  

   ![çalışan düğümlerinin yetkisini alma](./media/r-server-operationalize/get-started-operationalization.png)  

* Seçin **Eylemler** > **seçilen Konaklar** > **DataNodes** > tıklatın **Decommission**.
* Seçin **Eylemler** > **seçilen Konaklar** > **NodeManagers** > tıklatın **Decommission**.
* Seçin **Eylemler** > **seçilen Konaklar** > **DataNodes** > tıklatın **durdurmak**.
* Seçin **Eylemler** > **seçilen Konaklar** > **NodeManagers** > tıklayın **durdurmak**.
* Seçin **Eylemler** > **seçilen Konaklar** > **ana** > tıklatın **Durdur tüm bileşenleri**.
* Çalışan düğümlerinin seçimini kaldırın ve baş düğümleri seçin.
* Seçin **Eylemler** > **konakları seçili** > "**ana** > **tüm bileşenleri yeniden**.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>2. adım: Yapılandırma düğümleri her yetkisi alınmış çalışan düğümü üzerinde işlem

1. Yetkisi alınan her çalışan düğümüne SSH uygulayın.

2. Sahip olduğunuz ML Hizmetleri kümesi için ilgili DLL kullanarak yönetim yardımcı programını çalıştırın. ML sunucu 9.1 için şu komutu çalıştırın:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Girin **1** seçeneğini **Operationalization için ML Server'ı Yapılandır**.

4. Girin **C** seçeneğini `C. Compute node`. Bu işlem çalışan düğümündeki işlem düğümünü yapılandırır.

5. Yönetim Yardımcı Programından çıkın.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>3. adım: Ekleme düğümleri ayrıntıları web düğüm üzerinde işlem

Tüm yetkisi alınmış çalışan düğümleri işlem düğümü çalışacak şekilde yapılandırıldıktan sonra kenar düğümüne geri dönün ve ML sunucu web düğümün yapılandırmasında yetkisi alınmış çalışan düğümleri IP adreslerini ekleyin:

1. Kenar düğümüne SSH uygulayın.

2. `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` öğesini çalıştırın.

3. İçin "URI'ler" bölümüne bakın ve alt düğümün IP ve bağlantı noktası ayrıntıları ekleyin.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight kümesinde ML Hizmetleri yönetme](r-server-hdinsight-manage.md)
* [Hdınsight üzerinde ML Hizmetleri kümesi için içerik seçeneklerini işlem](r-server-compute-contexts.md)
* [Hdınsight kümesinde ML Hizmetleri için Azure depolama seçenekleri](r-server-storage.md)
