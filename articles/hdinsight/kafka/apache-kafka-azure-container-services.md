---
title: Azure kapsayıcı hizmeti hdınsight'ta Kafka ile kullanma | Microsoft Docs
description: Azure kapsayıcı hizmeti (AKS) barındırılan kapsayıcı görüntülerden Hdınsight'ta Kafka kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2018
ms.author: larryfr
ms.openlocfilehash: 16513cbd775e200a0821e8786ae823b82c67e437
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-azure-container-services-with-kafka-on-hdinsight"></a>Hdınsight üzerinde Kafka ile Azure kapsayıcı hizmetlerini kullanma

Azure kapsayıcı Hizmetleri (AKS) ile Kafka Hdınsight kümesinde kullanmayı öğrenin. Bu belgede yer alan adımlar AKS içinde barındırılan bir Node.js uygulaması Kafka bağlantılarını doğrulamak için kullanın. Bu uygulamanın kullandığı [kafka düğümlü](https://www.npmjs.com/package/kafka-node) Kafka ile iletişim kurmak için paket. Kullandığı [Socket.IO](https://socket.io/) tarayıcı istemci AKS içinde barındırılan arka uç arasındaki ileti güdümlü olayı için.

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Azure kapsayıcı hizmeti barındırılan Kubernetes ortamınıza yönetir ve kapsayıcılı uygulamaları dağıtmak kolay ve hızlı hale getirir. Bir Azure sanal ağı kullanarak, iki hizmet bağlanabilir.

> [!NOTE]
> Hdınsight üzerinde Kafka ile iletişim kurmak Azure kapsayıcı hizmetlerini etkinleştirmek için gereken adımlar bu belgenin odak noktasıdır. Örnek Yapılandırması çalıştığını göstermek için yalnızca bir temel Kafka istemcidir.

## <a name="prerequisites"></a>Önkoşullar

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Bir Azure aboneliği

Bu belge, oluşturma ve aşağıdaki Azure hizmetlerini kullanma konusunda bilgi sahibi olduğunuzu varsayar:

* HDInsight üzerinde Kafka
* Azure Container Service
* Azure Sanal Ağları

Bu belgede ayrıca, gitti olduğunu varsayar [Azure kapsayıcı hizmetlerini Öğreticisi](../../aks/tutorial-kubernetes-prepare-app.md). Bu öğretici bir kapsayıcı hizmeti oluşturur, bir kapsayıcı kayıt defteri Kubernetes bir küme oluşturur ve yapılandırır `kubectl` yardımcı programı.

## <a name="architecture"></a>Mimari

### <a name="network-topology"></a>Ağ topolojisi

Hdınsight ve AKS bir Azure sanal ağı işlem kaynakları için bir kapsayıcı olarak kullanın. Hdınsight AKS arasındaki iletişimi etkinleştirmek için kendi ağları arasındaki iletişimi etkinleştirmeniz gerekir. Bu belgede yer alan adımlar ağlara sanal ağ eşlemesi kullanın. Örneğin, VPN, diğer bağlantılar de çalışması gerekir. Eşleme ile ilgili daha fazla bilgi için bkz: [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md) belge.


Aşağıdaki diyagram, bu belgede kullanılan ağ topolojisini gösterir:

![Bir sanal ağ, başka bir AKS ve eşlemesi kullanmanın bağlı ağlara Hdınsight'ta](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]
> Ad çözümlemesi IP adresleme kullanılmak üzere eşlenmiş ağlar arasında etkin değil. Varsayılan olarak, hdınsight'ta Kafka, istemciler bağlandığında IP adresi yerine ana bilgisayar adlarını döndürmek için yapılandırılır. Bu belgede yer alan adımlar IP'nin Kafka değiştirmek yerine reklam.

## <a name="create-an-azure-container-service-aks"></a>Bir Azure kapsayıcı hizmeti (AKS) oluşturma

Bir AKS kümesi zaten yoksa, bir oluşturmayı öğrenmek için aşağıdaki belgeler birini kullanın:

* [Bir Azure kapsayıcı hizmeti (AKS) kümeyi - Portal dağıtma](../../aks/kubernetes-walkthrough-portal.md)
* [Bir Azure kapsayıcı hizmeti (AKS) kümeyi - CLI dağıtma](../../aks/kubernetes-walkthrough.md)

> [!NOTE]
> AKS yükleme sırasında bir sanal ağ oluşturur. Bu ağ, sonraki bölümde Hdınsight için oluşturulan bir eşlenen.

## <a name="configure-virtual-network-peering"></a>Sanal ağ eşlemesini yapılandırın

1. Gelen [Azure portal](https://portal.azure.com)seçin __kaynak grupları__ve ardından sanal ağ AKS kümeniz için içeren kaynak grubunu bulun. Kaynak grubu adı `MC_<resourcegroup>_<akscluster>_<location>`. `resourcegroup` Ve `akscluster` girişler kümede oluşturduğunuz kaynak grubunun adını ve küme adı. `location` Küme oluşturulduğu konum.

2. Kaynak grubunda seçin __sanal ağ__ kaynak.

3. Seçin __adres alanı__. Adres alanı listelenen not edin.

4. Hdınsight için bir sanal ağ oluşturmak için seçin __+ kaynak oluşturma__, __ağ__ve ardından __sanal ağ__.

    > [!IMPORTANT]
    > Yeni bir sanal ağ için değerler girildiğinde, AKS küme ağı tarafından kullanılan bir örtüşmediğinden bir adres alanı kullanmanız gerekir.

    Aynı __konumu__ sanal ağ için AKS küme için kullanılır.

    Sanal ağ, sonraki adıma geçmeden önce oluşturuluncaya kadar bekleyin.

5. Hdınsight ağı ve AKS küme ağı arasında eşlemeyi yapılandırmak için sanal ağı seçin ve ardından __eşlemeler__. Seçin __+ Ekle__ ve form doldurmak için aşağıdaki değerleri kullanın:

    * __Ad__: Eşleme bu yapılandırma için benzersiz bir ad girin.
    * __Sanal ağ__: sanal ağ için seçmek için bu alanı kullanın **AKS küme**.

    Diğer tüm alanları varsayılan değerinde bırakın ve ardından __Tamam__ eşlemeyi yapılandırmak için.

6. AKS küme ağı arasında Hdınsight ağ eşlemesini yapılandırmak üzere seçin __AKS küme sanal ağ__ve ardından __eşlemeler__. Seçin __+ Ekle__ ve form doldurmak için aşağıdaki değerleri kullanın:

    * __Ad__: Eşleme bu yapılandırma için benzersiz bir ad girin.
    * __Sanal ağ__: sanal ağ için seçmek için bu alanı kullanın __Hdınsight kümesi__.

    Diğer tüm alanları varsayılan değerinde bırakın ve ardından __Tamam__ eşlemeyi yapılandırmak için.

## <a name="install-kafka-on-hdinsight"></a>Hdınsight üzerinde Kafka yükleyin

Kafka Hdınsight kümesi oluştururken, Hdınsight için daha önce oluşturduğunuz sanal ağa katılmanız gerekir. Kafka küme oluşturma ile ilgili daha fazla bilgi için bkz: [Kafka küme oluşturma](apache-kafka-get-started.md) belge.

> [!IMPORTANT]
> Küme oluştururken kullanmanız gerekir __Gelişmiş ayarları__ Hdınsight için oluşturduğunuz sanal ağa bağlanma.

## <a name="configure-kafka-ip-advertising"></a>Kafka IP reklam yapılandırma

IP adresleri etki alanı adları yerine tanıtmak için Kafka yapılandırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısı kullanarak Git https://CLUSTERNAME.azurehdinsight.net. Değiştir __CLUSTERNAME__ Hdınsight kümesinde Kafka adı.

    İstendiğinde, küme için HTTPS kullanıcı adı ve parola kullanın. Küme için Ambari Web kullanıcı Arabirimi görüntülenir.

2. Kafka hakkında bilgileri görüntülemek için seçin __Kafka__ sol taraftaki listeden.

    ![Hizmet listesi Kafka ile vurgulanan](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için seçin __yapılandırmalar__ üst ortasından.

    ![Kafka için yapılandırmalar bağlantıları](./media/apache-kafka-azure-container-services/select-kafka-config.png)

4. Bulunacak __kafka env__ yapılandırma, girin `kafka-env` içinde __filtre__ sağ üst alanını.

    ![Kafka env için Kafka yapılandırma](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

5. IP adresleri tanıtmak için Kafka yapılandırmak için aşağıdaki metni altına ekleyin __kafka env şablonu__ alan:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka dinlediği arabirimi yapılandırmak için girin `listeners` içinde __filtre__ sağ üst alanını.

7. Tüm ağ arabirimleri üzerinde dinlenecek Kafka yapılandırmak için değeri değiştirin __dinleyicileri__ alanı `PLAINTEXT://0.0.0.0:9092`.

8. Yapılandırma değişikliklerini kaydetmek için kullanın __kaydetmek__ düğmesi. Değişiklikleri açıklayan bir metin iletisi girin. Seçin __Tamam__ değişiklikler kaydedildikten sonra.

    ![Kaydet Yapılandırma düğmesi](./media/apache-kafka-azure-container-services/save-button.png)

9. Kafka yeniden başlatılırken hataları önlemek için kullanmak __hizmet eylemleri__ düğmesine tıklayın ve ardından __kapatma üzerinde Bakım modu__. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Hizmet eylemlerle vurgulanmış bakım etkinleştirin](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Kafka yeniden başlatmak için kullanın __yeniden__ düğmesine tıklayın ve ardından __yeniden tüm etkilenen__. Yeniden başlatma onaylayın ve ardından __Tamam__ işlemi tamamlandıktan sonra düğmesine tıklayın.

    ![Etkilenen yeniden başlatma ile tüm düğmesi vurgulanmış yeniden başlatın](./media/apache-kafka-azure-container-services/restart-button.png)

11. Bakım modu devre dışı bırakmak için __hizmet eylemleri__ düğmesine tıklayın ve ardından __Bakım modu devre dışı bırakma__. Seçin **Tamam** bu işlemi tamamlamak için.

## <a name="test-the-configuration"></a>Yapılandırmayı test etme

Bu noktada, Kafka ve Azure kapsayıcı hizmeti eşlenen sanal ağlar üzerinden iletişimi arasındadır. Bu bağlantıyı sınamak için aşağıdaki adımları kullanın:

1. Test uygulama tarafından kullanılan bir Kafka konu oluşturun. Kafka oluşturma konuları hakkında daha fazla bilgi için bkz: [Kafka küme oluşturma](apache-kafka-get-started.md) belge.

2. Örnek uygulamayı indirin [ https://github.com/Blackmist/Kafka-AKS-Test ](https://github.com/Blackmist/Kafka-AKS-Test). 

3. Düzen `index.js` dosya ve aşağıdaki satırları değiştirin:

    * `var topic = 'mytopic'`: Değiştirin `mytopic` bu uygulama tarafından kullanılan Kafka konu adı.
    * `var brokerHost = '176.16.0.13:9092`: Değiştirin `176.16.0.13` Aracısı ana bilgisayarları, küme için bir iç IP adresine sahip.

        Kümedeki ana bilgisayar (workernodes) aracısı iç IP adresini bulmak için bkz [Ambari REST API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-internal-ip-address-of-cluster-nodes) belge. IP adresi, bir etki alanı adı ile başladığı girişlerinin çekme `wn`.

4. Bir komut satırından `src` directory bağımlılıkları yükler ve dağıtım için bir görüntü oluşturmak için Docker kullanın:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]
    > Bu uygulama tarafından gerekli paketleri kullanmak gerekmez şekilde depoya işaretli `npm` yardımcı programını kullanarak onları yükleyin.

5. Azure kapsayıcı kayıt defteri (ACR) için oturum açın ve loginServer adı bulunamadı:

    ```bash
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]
    > Azure kapsayıcı kayıt defteri adı veya olan Azure kapsayıcı hizmeti ile çalışmak için Azure CLI kullanma ile tanınmayan bakın bilmiyorsanız [AKS öğreticileri](../../aks/tutorial-kubernetes-prepare-app.md).

6. Yerel etiketi `kafka-aks-test` , ACR loginServer görüntüsüyle. Ayrıca ekleyin `:v1` resim sürümünü belirtmek için bitiş:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Görüntü kayıt defterine gönderin:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```
    Bu işlemin tamamlanması birkaç dakika sürer.

8. Kubernetes bildirim dosyasını düzenleyin (`kafka-aks-test.yaml`) ve değiştirme `microsoft` ACR loginServer adıyla 4. adımda alınan.

9. Bildirimden uygulama ayarlarını dağıtmak için aşağıdaki komutu kullanın:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. İçin izlemek için aşağıdaki komutu kullanın `EXTERNAL-IP` uygulamanın:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Dış IP adresi atandığında, kullanın __CTRL + C__ izleme çıkmak için

11. Bir web tarayıcısı açın ve hizmet için harici IP adresini girin. Aşağıdaki görüntüye benzer bir sayfa adresindeki ulaşır:

    ![Web sayfasının görüntüsü](./media/apache-kafka-azure-container-services/test-web-page.png)

12. Metin alanına girin ve ardından __Gönder__ düğmesi. Veriler Kafka için gönderilir. Uygulama Kafka tüketicideki iletiyi okur ve ona ekler __Kafka iletilerden__ bölümü.

    > [!WARNING]
    > Birden çok kopyasını bir ileti alabilirsiniz. Bu sorun genellikle bağlandıktan sonra tarayıcınızı yenileyin olur veya uygulama için birden çok tarayıcı Bağlantıları'nı açın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Kafka kullanmaya başlama](apache-kafka-get-started.md)

* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](apache-kafka-mirroring.md)

* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)

* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)
