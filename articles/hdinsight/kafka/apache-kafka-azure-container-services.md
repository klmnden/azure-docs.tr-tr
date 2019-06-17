---
title: Azure Kubernetes hizmeti, HDInsight üzerinde Kafka ile kullanma
description: Azure Kubernetes Service (AKS) barındırılan kapsayıcı görüntülerinden HDInsight üzerinde Kafka kullanmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 35ef708cdcedc2d7bafedb8bf3686e4b468177df
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64723676"
---
# <a name="use-azure-kubernetes-service-with-apache-kafka-on-hdinsight"></a>Azure Kubernetes hizmeti, HDInsight üzerinde Apache Kafka ile kullanma

Azure Kubernetes Service (AKS) ile kullanmayı öğrenin [Apache Kafka](https://kafka.apache.org/) HDInsight kümesi üzerinde. Bu belgedeki adımlarda, AKS içinde barındırılan bir Node.js uygulaması Kafka ile bağlantısını doğrulamak için kullanın. Bu uygulamanın kullandığı [kafka düğümlü](https://www.npmjs.com/package/kafka-node) Kafka ile iletişim kurmak için paket. Kullandığı [Socket.io](https://socket.io/) olay odaklı tarayıcı istemci ve AKS barındırılan arka uç arasındaki Mesajlaşma için.

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Azure Kubernetes hizmeti, barındırılan Kubernetes ortamınızı yöneten ve kapsayıcılı uygulamaları dağıtmak hızlı ve kolaylaştırır. Bir Azure sanal ağı kullanarak, iki hizmet bağlanabilirsiniz.

> [!NOTE]  
> HDInsight üzerinde Kafka ile iletişim kurmak Azure Kubernetes hizmeti etkinleştirmek için gerekli adımları bu belgenin odak açıktır. Örnek yapılandırma çalıştığını göstermek için yalnızca bir temel Kafka istemcisidir.

## <a name="prerequisites"></a>Önkoşullar

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Bir Azure aboneliği

Bu belge, oluşturma ve aşağıdaki Azure hizmetlerini kullanma ile ilgili bilgi sahibi olduğunuz varsayılır:

* HDInsight üzerinde Kafka
* Azure Kubernetes Service
* Azure Sanal Ağları

Bu belge, ayrıca, konusunun üzerinden geçtik olduğunu varsayar [Azure Kubernetes Service Öğreticisi](../../aks/tutorial-kubernetes-prepare-app.md). Bu öğreticide, bir kapsayıcı hizmeti oluşturur, bir Kubernetes kümesi, bir kapsayıcı kayıt defteri oluşturur ve yapılandırır `kubectl` yardımcı programı.

## <a name="architecture"></a>Mimari

### <a name="network-topology"></a>Ağ topolojisi

HDInsight hem AKS bir Azure sanal ağ bilgi işlem kaynakları için bir kapsayıcı kullanın. HDInsight ile AKS arasındaki iletişimi etkinleştirmek için kendi ağları arasındaki iletişimi etkinleştirmeniz gerekir. Bu belgedeki adımlarda sanal ağ eşlemesi ağlara kullanın. VPN gibi diğer bağlantılar da işe yaramalıdır. Eşleme ile ilgili daha fazla bilgi için bkz: [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md) belge.


Aşağıdaki diyagram, bu belgede kullanılan ağ topolojisini gösterir:

![Bir sanal ağ, başka bir AKS ve eşlemesini kullanmanın bağlı ağlara HDInsight](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]  
> Bu nedenle IP adresi kullanılır eşlenen ağların ad çözümlemesi etkin değil. Varsayılan olarak, HDInsight üzerinde Kafka, istemciler bağlandığında, IP adresi yerine ana bilgisayar adlarını döndürmek için yapılandırılır. Kafka IP kullanmak için bu belgedeki adımlarda değiştirmek yerine reklam.

## <a name="create-an-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'i (AKS) oluşturma

Bir AKS kümesi zaten yoksa, nasıl oluşturacağınızı öğrenmek için aşağıdaki belgeleri kullanın:

* [Bir Azure Kubernetes Service (AKS) küme dağıtma - Portal](../../aks/kubernetes-walkthrough-portal.md)
* [Bir Azure Kubernetes Service (AKS) kümesi dağıtma - CLI](../../aks/kubernetes-walkthrough.md)

> [!NOTE]  
> AKS, yükleme sırasında bir sanal ağ oluşturur. Bu ağ, sonraki bölümde HDInsight için oluşturulan bir eşlenmiş.

## <a name="configure-virtual-network-peering"></a>Sanal ağ eşlemesini yapılandırma

1. Gelen [Azure portalında](https://portal.azure.com)seçin __kaynak grupları__ve ardından sanal ağ için AKS kümenizi içeren kaynak grubunu bulun. Kaynak grubu adı `MC_<resourcegroup>_<akscluster>_<location>`. `resourcegroup` Ve `akscluster` girdileri kümede oluşturduğunuz kaynak grubu adını ve küme adını alır. `location` Kümenin oluşturulduğu konum.

2. Kaynak grubunda seçin __sanal ağ__ kaynak.

3. Seçin __adres alanı__. Adres alanı listelenen unutmayın.

4. HDInsight için bir sanal ağ oluşturmak için seçin __+ kaynak Oluştur__, __ağ__, ardından __sanal ağ__.

    > [!IMPORTANT]  
    > Değerleri yeni bir sanal ağ için girerken, AKS küme ağı tarafından kullanılan çakışmayacak bir adres alanı kullanmanız gerekir.

    Aynı __konumu__ sanal ağ için AKS kümesi için kullanılır.

    Sanal ağ, sonraki adıma geçmeden önce oluşturuluncaya kadar bekleyin.

5. HDInsight ağ ve AKS küme ağı arasında eşleme yapılandırmak için sanal ağı seçin ve ardından __eşlemeler__. Seçin __+ Ekle__ ve formu doldurmak için aşağıdaki değerleri kullanın:

   * __Ad__: Bu eşleme yapılandırması için benzersiz bir ad girin.
   * __Sanal ağ__: Sanal ağ için seçmek için bu alanı kullanın **AKS kümesi**.

     Diğer alanları varsayılan değerde bırakın ve ardından __Tamam__ eşlemesini yapılandırmak üzere.

6. AKS küme ağı arasında HDInsight ağ eşlemesini yapılandırmak üzere seçin __AKS küme sanal ağ__ve ardından __eşlemeler__. Seçin __+ Ekle__ ve formu doldurmak için aşağıdaki değerleri kullanın:

   * __Ad__: Bu eşleme yapılandırması için benzersiz bir ad girin.
   * __Sanal ağ__: Sanal ağ için seçmek için bu alanı kullanın __HDInsight küme__.

     Diğer alanları varsayılan değerde bırakın ve ardından __Tamam__ eşlemesini yapılandırmak üzere.

## <a name="install-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka yükleyin

HDInsight kümesinde Kafka oluşturma, HDInsight için daha önce oluşturduğunuz sanal ağa eklemeniz gerekir. Kafka kümesi oluşturma hakkında daha fazla bilgi için bkz. [Apache Kafka kümesi oluşturma](apache-kafka-get-started.md) belge.

> [!IMPORTANT]  
> Kümeyi oluştururken kullanmanız gerekir __Gelişmiş ayarlar__ HDInsight için oluşturduğunuz sanal ağa bağlanma.

## <a name="configure-apache-kafka-ip-advertising"></a>Apache Kafka IP reklam yapılandırma

Etki alanı adları yerine IP adreslerini tanıtmak için Kafka yapılandırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısı kullanarak Git https://CLUSTERNAME.azurehdinsight.net. Değiştirin __CLUSTERNAME__ HDInsight kümesinde Kafka adı.

    İstendiğinde, küme için HTTPS kullanıcı adını ve parolayı kullanın. Küme için Ambari Web kullanıcı Arabirimi görüntülenir.

2. Kafka hakkında bilgi görüntülemek için seçin __Kafka__ sol taraftaki listeden.

    ![Hizmet listesi Kafka ile vurgulanmış](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için seçin __yapılandırmaları__ üst ortasından.

    ![Kafka için yapılandırmaları bağlantıları](./media/apache-kafka-azure-container-services/select-kafka-config.png)

4. Bulunacak __kafka env__ yapılandırması girin `kafka-env` içinde __filtre__ alanında sağ üst köşede bulunan.

    ![Kafka env için Kafka yapılandırması](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

5. Kafka IP adresleri tanıtacak şekilde yapılandırmak için alt kısmına aşağıdaki metni ekleyin __kafka env şablon__ alan:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka dinlediği arabirimi yapılandırmak için girin `listeners` içinde __filtre__ sağ üst alan.

7. Kafka, tüm ağ arabirimleri üzerinde dinlemek üzere yapılandırmak için değerini değiştirmek __dinleyicileri__ alanı `PLAINTEXT://0.0.0.0:9092`.

8. Yapılandırma değişikliklerini kaydetmek için kullanın __Kaydet__ düğmesi. Değişiklikleri açıklayan bir mesaj girin. Seçin __Tamam__ değişiklikleri kaydettikten sonra.

    ![Kaydet düğmesi yapılandırma](./media/apache-kafka-azure-container-services/save-button.png)

9. Kafka yeniden başlatılırken hataları önlemek için __hizmet eylemleri__ düğmesini tıklatın ve seçin __bakım modunu aç__. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Hizmet eylemlerle vurgulanmış bakım etkinleştirin](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Kafka yeniden başlatmak için kullanın __yeniden__ düğmesini tıklatın ve seçin __yeniden tüm etkilenen__. Yeniden başlatma işlemini onaylayın ve ardından __Tamam__ işlemi tamamlandıktan sonra düğme.

    ![Etkilenen yeniden başlatma ile tüm düğmesi vurgulanmış yeniden başlatın](./media/apache-kafka-azure-container-services/restart-button.png)

11. Bakım modunu devre dışı bırakmak için __hizmet eylemleri__ düğmesini tıklatın ve seçin __bakım modunu Kapat Kapat__. Seçin **Tamam** bu işlemi tamamlamak için.

## <a name="test-the-configuration"></a>Test yapılandırması

Bu noktada, Kafka ve Azure Kubernetes hizmeti eşlenen sanal ağlarda aracılığıyla iletişime dahildir. Bu bağlantıyı test etmek için aşağıdaki adımları kullanın:

1. Test uygulama tarafından kullanılan bir Kafka konu oluşturun. Kafka konu oluşturma hakkında daha fazla bilgi için bkz: [Apache Kafka kümesi oluşturma](apache-kafka-get-started.md) belge.

2. Örnek uygulamayı indirin [ https://github.com/Blackmist/Kafka-AKS-Test ](https://github.com/Blackmist/Kafka-AKS-Test).

3. Düzen `index.js` dosyasını açıp aşağıdaki satırları değiştirin:

    * `var topic = 'mytopic'`: Değiştirin `mytopic` bu uygulama tarafından kullanılan Kafka konu adına sahip.
    * `var brokerHost = '176.16.0.13:9092`: Değiştirin `176.16.0.13` kümenizin aracı konaklarından iç IP adresi ile.

        İç IP adresi Aracısı kümedeki konaklar (workernodes) bulmak için bkz [Apache Ambari REST API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-internal-ip-address-of-cluster-nodes) belge. IP adresi başladığı etki alanı adı ile girişlerden birini seçin `wn`.

4. Bir komut satırında `src` directory bağımlılıkları yükler ve dağıtım için bir görüntü oluşturmak için Docker'ı kullanma:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]  
    > Bu uygulama için gereken paketleri kullanmaya gerek kalmayacak şekilde depoya işaretli `npm` yardımcı programını kullanarak bunları yükleyin.

5. Azure Container Registry (ACR) için oturum açın ve loginServer adını bulun:

    ```bash
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]  
    > Azure Container Registry adınız ya da Azure CLI kullanarak Azure Kubernetes hizmeti ile çalışacak şekilde erişilen bilmiyorsanız bkz bilmiyorsanız [AKS öğreticileri](../../aks/tutorial-kubernetes-prepare-app.md).

6. Yerel etiketi `kafka-aks-test` , ACR loginserver'ı ile görüntü. Ayrıca `:v1` sonuna görüntü sürümü belirtmek için:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Görüntüyü kayıt defterinize gönderin:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```
    Bu işlemin tamamlanması birkaç dakika sürer.

8. Kubernetes bildirim dosyası Düzenle (`kafka-aks-test.yaml`) ve yerine `microsoft` ACR loginServer adıyla 4. adımda alınan.

9. Bildirimden uygulama ayarlarını dağıtmak için aşağıdaki komutu kullanın:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. İzlemek üzere aşağıdaki komutu kullanın `EXTERNAL-IP` uygulamanın:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Dış IP adresi atandığında, kullanın __CTRL + C__ watch çıkmak için

11. Bir web tarayıcısı açın ve hizmet için dış IP adresini girin. Aşağıdaki görüntüye benzer bir sayfa ulaşırsınız:

    ![Web sayfasının görüntüsü](./media/apache-kafka-azure-container-services/test-web-page.png)

12. Metin alanına girin ve ardından __Gönder__ düğmesi. Kafka için bir veri gönderilmedi. Kafka tüketicisi uygulamada iletiyi okur ve ona ekler __iletileri kafka'dan__ bölümü.

    > [!WARNING]  
    > Birden çok kopyasını bir ileti alabilirsiniz. Bu sorun genellikle bağlandıktan sonra tarayıcınızı yenileyin olur veya uygulamaya birden çok tarayıcı bağlantısı açın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](apache-kafka-get-started.md)

* [MirrorMaker HDInsight üzerinde Apache Kafka'nın bir çoğaltma oluşturmak için kullanın](apache-kafka-mirroring.md)

* [Apache Storm'u HDInsight üzerinde Apache Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)

* [HDInsight üzerinde Apache Kafka ile Apache Spark kullanma](../hdinsight-apache-spark-with-kafka.md)

* [Apache Kafka ile bir Azure sanal ağına bağlanma](apache-kafka-connect-vpn-gateway.md)
