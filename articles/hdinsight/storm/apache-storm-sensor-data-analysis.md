---
title: "Apache Storm ve HBase ile algılayıcı verilerini çözümleme | Microsoft Docs"
description: "Apache Storm için bir sanal ağ ile bağlamayı öğrenin. Storm HBase ile algılayıcı verilerini işlemek için bir olay hub'ı kullanın ve D3.js ile görselleştirin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/19/2017
ms.author: larryfr
ms.openlocfilehash: 87c2aece68c5de06d683abf971b6c7ccf7f67a54
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Apache Storm, olay hub'ı ve (Hadoop) hdınsight'ta HBase ile algılayıcı verilerini çözümleme

Apache Storm Hdınsight'ta algılayıcı verilerini işlemek için Azure olay Hub'ından nasıl kullanacağınızı öğrenin. Veriler Hdınsight üzerinde Apache HBase içine depolanır ve D3.js kullanarak görselleştirilen.

Bu belgede kullanılan Azure Resource Manager şablonu bir kaynak grubunda birden çok Azure kaynağını oluşturma gösterilmektedir. Bir Azure sanal ağı, iki Hdınsight kümesi (Storm ve HBase) ve Azure Web uygulaması şablonu oluşturur. Gerçek zamanlı web Panosu bir node.js uygulamasını web uygulamasına otomatik olarak dağıtılır.

> [!NOTE]
> Bu belge ve örnek bu belgedeki bilgiler Hdınsight sürüm 3.6 gerektirir.
>
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="architecture"></a>Mimari

![mimarisi diyagramı](./media/apache-storm-sensor-data-analysis/devicesarchitecture.png)

Bu örnekte aşağıdaki bileşenlerden oluşur:

* **Azure Event Hubs**: algılayıcı toplanan verileri içerir.
* **Hdınsight üzerinde Storm**: olay Hub'ından veri gerçek zamanlı işlenmesini sağlar.
* **Hdınsight'ta HBase**: Storm tarafından işlendikten sonra kalıcı NoSQL veri deposu için veri sağlar.
* **Azure sanal ağ hizmeti**: Hdınsight üzerinde Storm ve HBase Hdınsight kümelerinde arasında güvenli iletişim sağlar.
  
  > [!NOTE]
  > Bir sanal ağ, Java HBase İstemcisi API kullanılırken gereklidir. HBase kümeleri için ortak ağ geçidi üzerinden açık değil. HBase ve Storm kümeleri aynı sanal ağa yükleme Storm kümesi (veya herhangi bir sanal ağ sisteminde) HBase doğrudan erişmek istemci API kullanarak sağlar.

* **Pano Web sitesi**: Gerçek zamanlı veri grafikleri bir örnek Pano.
  
  * Web sitesi Node.js içinde uygulanmıştır.
  * [Socket.IO](http://socket.io/) Storm topolojisini ve Web sitesi arasında gerçek zamanlı iletişim için kullanılır.
    
    > [!NOTE]
    > Socket.IO iletişim için bir uygulama ayrıntılarını kullanmaktır. Ham WebSockets veya SignalR gibi tüm iletişimler çerçevesi kullanabilirsiniz.

  * [D3.js](http://d3js.org/) Web sitesine gönderilen veriler graph için kullanılır.

> [!IMPORTANT]
> Storm ve HBase için bir Hdınsight kümesi oluşturmak için desteklenen bir yöntem olarak iki küme gereklidir.

Olay Hub'ından veri kullanarak topoloji okur `org.apache.storm.eventhubs.spout.EventHubSpout` sınıf ve HBase kullanarak yazma veri `org.apache.storm.hbase.bolt.HBaseBolt` sınıfı. Web sitesi ile iletişim kullanarak gerçekleştirilir [Socket.IO client.java](https://github.com/nkzawa/socket.io-client.java).

Aşağıdaki diyagram topoloji düzeni açıklanmaktadır:

![Topoloji diyagramı](./media/apache-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Bu diyagramda, topolojiye Basitleştirilmiş görünümüdür. Her bileşen örneği, olay hub'ınızdaki her bir bölümü için oluşturulur. Bu örnekler, kümedeki düğümler arasında dağıtılır ve veri aralarında gibi yönlendirilir:
> 
> * Spout ayrıştırıcı için yük dengeli verilerdir.
> * İletileri aynı aygıttan her zaman aynı bileşene akması ayrıştırıcı verilerden Pano ve HBase cihaz Kimliğine göre gruplandırılır.

### <a name="topology-components"></a>Topoloji bileşenleri

* **Event Hubs Spout**: spout 0.10.0 Apache Storm sürümünün parçası olarak sağlanan ve daha yüksek.
  
  > [!NOTE]
  > Bu örnekte kullanılan olay hub'ı spout Hdınsight kümesi sürüm 3.5 veya 3.6 Storm gerektirir.

* **ParserBolt.java**: spout'un yayılan veriler ham JSON ve bazen birden fazla olay aynı anda gösterilen. Bu Cıvata tarafından spout yayılan veriler okur ve JSON ileti ayrıştırır. Cıvata, verileri birden çok alan içeren bir tanımlama grubu sonra yayar.
* **DashboardBolt.java**: Bu bileşen için Java Socket.IO istemci kitaplığı web panoya gerçek zamanlı olarak veri göndermek için nasıl kullanılacağını gösterir.
* **Hayır-hbase.yaml**: yerel modda çalışırken kullanılan topoloji tanımı. HBase bileşenleri kullanmaz.
* **hbase.yaml ile**: topoloji kümede çalışırken kullanılan topoloji tanımı. HBase bileşenlerini kullanın.
* **dev.Properties**: olay hub'ı spout, HBase Cıvata ve Panosu bileşenleri için yapılandırma bilgileri.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Bu örneğini kullanmadan önce geliştirme ortamınızı hazırlamanız gerekir. Bu örnek tarafından kullanılan bir Azure olay hub'ı da oluşturmanız gerekir.

Geliştirme ortamınıza yüklenen aşağıdaki öğeleri gerekir:

* [Node.js](http://nodejs.org/): web Panoda yerel geliştirme ortamınızı önizlemek için kullanılır.
* [Java ve JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Storm topolojisini geliştirmek için kullanılır.
* [Maven](http://maven.apache.org/what-is-maven.html): oluşturun ve projeyi derlemek için kullanılır.
* [Git](http://git-scm.com/): projeyi Github'dan karşıdan yüklemek için kullanılır.
* Bir **SSH** istemci: Linux tabanlı Hdınsight kümelerine bağlanmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

Bir olay hub'ı oluşturmak için adımlarda kullanın [bir olay hub'ı oluşturma](../../event-hubs/event-hubs-create.md) belge.

> [!IMPORTANT]
> Olay hub'ı adı, ad alanı ve anahtarı için RootManageSharedAccessKey kaydedin. Bu bilgiler, Storm topolojisini yapılandırmak için kullanılır.

Hdınsight kümesi gerek yoktur. Bu belgede yer alan adımlar, bu örnek tarafından gereken kaynakları oluşturan bir Azure Resource Manager şablonu sağlar. Şablon aşağıdaki kaynaklar oluşturur:
 
* Bir Azure sanal ağı
* Hdınsight kümesinde bir Storm (Linux tabanlı iki alt düğümleri)
* Hdınsight kümesinde bir HBase (Linux tabanlı iki alt düğümleri)
* Web Pano barındıran bir Azure Web uygulaması

## <a name="download-and-configure-the-project"></a>Karşıdan yükle ve projeyi yapılandırın

Projeyi Github'dan karşıdan yüklemek için aşağıdakileri kullanın.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Komut tamamlandığında, aşağıdaki dizin yapısını vardır:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> Bu belge tam ayrıntılar bu örnekteki kod geçmez. Ancak, kodu tam olarak geçersiz kılınan.

Olay Hub'ından okumak için projeyi yapılandırmak için açın `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` dosya ve olay hub'ı bilgilerinizi aşağıdaki satırları ekleyin:

```bash
eventhub.policy.key: the_key_for_RootManageSharedAccessKey
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Derleme ve yerel olarak test etme

> [!IMPORTANT]
> Topoloji kullanarak yerel olarak çalışan bir Storm geliştirme ortamını gerektirir. Daha fazla bilgi için bkz: [bir Storm geliştirme ortamını ayarlama](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) Apache.org.

Test etmeden önce topoloji çıktısını görüntülemek ve Event Hub'ında depolamak için verileri oluşturmak için panoyu başlatmanız gerekir.

> [!IMPORTANT]
> Bu topoloji HBase bileşeninin, yerel olarak test edilirken etkin değil. Java API HBase kümesi için Azure Sanal kümelerin içeren ağ dışında erişilemez.

### <a name="start-the-web-application"></a>Web uygulamasını Başlat

1. Bir komut istemi açın ve dizinleri değiştirmek `hdinsight-eventhub-example/dashboard`. Web uygulaması tarafından gerekli bağımlılıkların yüklemek için aşağıdaki komutu kullanın:
   
    ```bash
    npm install
    ```

2. Web uygulaması başlatmak için aşağıdaki komutu kullanın:
   
    ```bash
    node server.js
    ```
   
    Aşağıdakine benzer bir ileti görürsünüz:
   
        Server listening at port 3000

3. Bir web tarayıcısı açın ve girin `http://localhost:3000/` adresi olarak. Aşağıdaki görüntüye benzer bir sayfa görüntülenir:
   
    ![Web Panosu](./media/apache-storm-sensor-data-analysis/emptydashboard.png)
   
    Bu komut istemini açık bırakın. Test sonra web sunucusu durdurmak için Ctrl-C kullanın.

### <a name="generate-data"></a>Veri oluştur

> [!NOTE]
> Böylece herhangi bir platformda kullanılabilmesi için bu bölümdeki adımları Node.js kullanın. Diğer dil örnekler için bkz: `SendEvents` dizin.

1. Yeni istemi, kabuk veya terminal açın ve dizinleri değiştirmek `hdinsight-eventhub-example/SendEvents/nodejs`. Uygulama tarafından gerek duyulan bağımlılıklarını yüklemek için aşağıdaki komutu kullanın:

    ```bash
    npm install
    ```

2. Açık `app.js` dosyasını bir metin düzenleyicisinde açın ve daha önce aldığınız olay hub'ı bilgileri ekleyin:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'Your-eventhub-namespace';
    // Event Hub Name
    var hubname ='Your-eventhub-name';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'RootManageSharedAccessKey';
    var my_key = 'Your-Key';
    ```
   
   > [!NOTE]
   > Bu örnekte, kullandığınız varsayılır `RootManageSharedAccessKey` olay hub'ınızı erişmek için.

3. Event Hub'ında yeni girdileri eklemek için aşağıdaki komutu kullanın:
   
    ```bash
    node app.js
    ```
   
    Olay Hub'ına gönderilen verileri içeren birkaç satırlık çıkış bakın:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-the-topology"></a>Oluşturun ve topoloji başlatın

1. Yeni bir komut istemi açın ve dizinleri değiştirmek `hdinsight-eventhub-example/TemperatureMonitor`. Derleme ve topoloji paket için aşağıdaki komutu kullanın: 

    ```bash
    mvn clean package
    ```

2. Yerel modda topoloji başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`Topoloji yerel modunda başlatır.
    * `--filter`kullanan `dev.properties` topoloji tanımı parametrelerinde doldurmak için dosya.
    * `resources/no-hbase.yaml`kullanan `no-hbase.yaml` topoloji tanımı.
 
   Başladıktan sonra topoloji girişleri olay Hub'ından okur ve onları yerel makine üzerinde çalışan panoya gönderir. Aşağıdaki görüntüye benzer web panosunda görünür satırları görmeniz gerekir:
   
    ![verilerle Panosu](./media/apache-storm-sensor-data-analysis/datadashboard.png)

2. Pano çalışırken kullanmak `node app.js` Event Hubs'a yeni veri göndermek için önceki adımlardan gelen komutu. Sıcaklık değerleri rastgele oluşturulur çünkü grafik sıcaklık büyük değişiklikleri göstermek için güncelleştirmeniz gerekir.
   
   > [!NOTE]
   > İçinde olmalıdır **hdınsight-eventhub-örnek/SendEvents/Nodejs** kullanırken dizin `node app.js` komutu.

3. Pano güncelleştirmeleri doğruladıktan sonra Ctrl + C kullanarak topolojisi durdurun. Yerel web sunucusu da durdurmak için Ctrl + C kullanabilirsiniz.

## <a name="create-a-storm-and-hbase-cluster"></a>Bir Storm ve HBase kümesi oluşturma

Bu adımlarda bölüm kullan bir [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-template-deploy.md) bir Azure sanal ağı ve bir Storm ve HBase kümesi sanal ağ oluşturmak için. Şablon ayrıca bir Azure Web uygulaması oluşturur ve panonun bir kopyasını uygulamasına dağıtır.

> [!NOTE]
> Bir sanal ağ Storm kümesi üzerinde çalışan topoloji HBase Java API kullanarak HBase kümesi ile doğrudan iletişim kurabilmesi için kullanılır.

Bu belgede kullanılan Resource Manager şablonu bir ortak blob kapsayıcısında bulunan **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Azure'da oturum açın ve Resource Manager şablonu Azure portalında açmak için aşağıdaki düğmeye tıklayın.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/apache-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Gelen **özel dağıtım** bölümünde, aşağıdaki değerleri girin:
   
    ![Hdınsight parametreleri](./media/apache-storm-sensor-data-analysis/parameters.png)
   
   * **Temel küme adı**: Bu değer Storm için temel adı olarak kullanılır ve HBase kümeleri. Örneğin, **abc** adlı bir Storm kümesi oluşturur **storm abc** ve adlı bir HBase kümesi **hbase abc**.
   * **Oturum açma kullanıcı adı küme**: Storm ve HBase kümeleri için yönetici kullanıcı adı.
   * **Oturum açma parolası küme**: Storm ve HBase kümeleri için yönetici kullanıcı parolası.
   * **SSH kullanıcı adı**: için Storm ve HBase kümeleri oluşturmak için SSH kullanıcı.
   * **SSH parolası**: Storm ve HBase kümeleri için SSH kullanıcısının parolası.
   * **Konum**: kümeleri oluşturulan bölge.
     
     Parametreleri kaydetmek için **Tamam**’a tıklayın.

3. Kullanım **Temelleri** bölümü, bir kaynak grubu oluşturun veya varolan bir tanesini seçin.
4. İçinde **kaynak grubu konumu** için aynı konuma seçilen açılır menüsünde, select **konumu** parametresinde **ayarları** bölümü.
5. Hüküm ve koşulları okuyun ve ardından **hüküm ve koşulları yukarıda belirtildiği ediyorum**.
6. Son olarak, denetleme **panoya Sabitle** ve ardından **satın alma**. Kümeleri oluşturmak için yaklaşık 20 dakika sürer.

Kaynakları oluşturduktan sonra kaynak grubu hakkında bilgi görüntülenir.

![Vnet ve kümeler için kaynak grubu](./media/apache-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Hdınsight kümeleri adlarının bildirimi **storm BASENAME** ve **hbase BASENAME**, BASENAME şablona verdiğiniz adı olduğu. Bu adları kümeye bağlanırken bir sonraki adımda kullanın. Ayrıca Pano sitenin adı Not **basename Pano**. Bu değer, bu belgenin sonraki bölümlerinde kullanılır.

## <a name="configure-the-dashboard-bolt"></a>Pano Cıvata yapılandırın

Bir web uygulaması dağıtılan panoyu veri göndermek için aşağıdaki satırı değiştirin `dev.properties`dosyası:

```yaml
dashboard.uri: http://localhost:3000
```

Değişiklik `http://localhost:3000` için `http://BASENAME-dashboard.azurewebsites.net` ve dosyayı kaydedin. Değiştir **BASENAME** önceki adımda sağlanan temel ada sahip. Panoyu seçin ve URL görüntülemek için daha önce oluşturduğunuz kaynak grubunu da kullanabilirsiniz.

## <a name="create-the-hbase-table"></a>HBase tablosu oluşturma

Hbase'de verileri depolamak için size bir tablo önce oluşturmanız gerekir. Storm yazmak için gereken kaynakları önceden oluşturmak, bir Storm içindeki kaynaklardan oluşturulmaya çalışılırken olarak topoloji birden fazla örneğinin aynı kaynak oluşturulmaya çalışılırken neden olabilir. Topoloji dışındaki kaynaklar oluşturun ve okuma/yazma ve analizi için Storm kullanın.

1. SSH kullanıcı adı ve parola şablona küme oluşturma sırasında sağlanan kullanarak HBase kümesi bağlanmak için SSH kullanın. Örneğin, kullanarak bağlanma `ssh` komutunu aşağıdaki söz dizimini kullanmanız:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Değiştir `sshuser` küme oluştururken verdiğiniz SSH kullanıcı adı. Değiştir `clustername` HBase kümesi ada sahip.

2. SSH oturumundan HBase Kabuğu'nu başlatın.
   
    ```bash
    hbase shell
    ```
   
    Kabuk yüklendikten sonra gördüğünüz bir `hbase(main):001:0>` istemi.

3. HBase Kabuğu'ndan algılayıcı verilerini depolamak üzere bir tablo oluşturmak için aşağıdaki komutu girin:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Tablo aşağıdaki komutu kullanarak oluşturulduğundan emin olun:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Bu bilgileri tabloda 0 satır olduğunu gösteren aşağıdaki örneğe benzer şekilde döndürür.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Girin `exit` HBase Kabuğu'ndan çıkmak için:

## <a name="configure-the-hbase-bolt"></a>HBase Cıvata yapılandırın

İçin HBase Storm kümeden yazmak için HBase Cıvata HBase kümesi yapılandırma ayrıntılarını sağlamanız gerekir.

1. Aşağıdaki örnekler, HBase kümesi Zookeeper çekirdek almak için kullanın:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > `your_HDInsight_cluster_name` değerini HDInsight kümenizin adıyla değiştirin. Yükleme hakkında daha fazla bilgi için `jq` yardımcı programı, bkz: [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > İstendiğinde, Hdınsight yönetici oturum açma için parola girin.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name'
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Değiştir ' Hdınsight kümenizin adıyla your_HDInsight_cluster_name. İstendiğinde, Hdınsight yönetici oturum açma için parola girin.
    >
    > Bu örnek, Azure PowerShell gerektirir. Azure PowerShell kullanma hakkında daha fazla bilgi için bkz: [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    Bu örnekler tarafından döndürülen bilgiler için aşağıdaki metni benzer:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Bu bilgiler, HBase kümesi ile iletişim kurmak için Storm tarafından kullanılır.

2. Değiştirme `dev.properties` dosya ve Zookeeper çekirdek bilgileri aşağıdaki satırı ekleyin:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>, Paketi, derleme ve Hdınsight'a çözümü dağıtma

Geliştirme ortamınızı Storm topolojisini storm kümesine dağıtmak için aşağıdaki adımları kullanın.

1. Gelen `TemperatureMonitor` dizin, projenizden paketini yeni bir yapı gerçekleştirmek ve bir JAR oluşturmak için aşağıdaki komutu kullanın:
   
        mvn clean package
   
    Bu komut adlı bir dosya oluşturur `TemperatureMonitor-1.0-SNAPSHOT.jar in the `hedef ' projenizin dizin.

2. Karşıya yüklemek için SCP kullanma `TemperatureMonitor-1.0-SNAPSHOT.jar` ve `dev.properties` Storm kümenize dosyaları. Aşağıdaki örnekte `sshuser` küme oluştururken verdiğiniz SSH kullanıcı ile ve `clustername` Storm kümesi adı. İstendiğinde, SSH kullanıcı için parola girin.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Dosyaları karşıya yüklemek için birkaç dakika sürebilir.

    Kullanma hakkında daha fazla bilgi için `scp` ve `ssh` komutları, Hdınsight ile bkz [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md)

3. Dosya karşıya sonra SSH kullanarak Storm kümeye bağlanın.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Değiştir `sshuser` SSH kullanıcı adı. Değiştir `clustername` Storm kümesi ada sahip.

4. Topoloji başlatmak için SSH oturumundan aşağıdaki komutu kullanın:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`Topoloji yönetici düğümleri dağıtır Nimbus hizmetine gönderir.
    * `--filter`kullanan `dev.properties` topoloji tanımı parametrelerinde doldurmak için dosya.
    * `-R /with-hbase.yaml`kullanan `with-hbase.yaml` paketindeki topolojisi.

5. Topoloji başlatıldıktan sonra bir tarayıcı açın Azure üzerinde yayımlanan Web sitesine, sonra kullanın `node app.js` olay Hub'ına veri göndermek için komutu. Güncelleştirme bilgileri görüntülemek için web Pano görmeniz gerekir.
   
    ![pano](./media/apache-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase verileri görüntüleme

HBase için bağlanmak ve veri tablosuna yazılan olduğunu doğrulamak için aşağıdaki adımları kullanın:

1. HBase kümeye bağlanmak için SSH kullanın.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Değiştir `sshuser` SSH kullanıcı adı. Değiştir `clustername` HBase kümesi ada sahip.

2. SSH oturumundan HBase Kabuğu'nu başlatın.
   
    ```bash
    hbase shell
    ```
   
    Kabuk yüklendikten sonra gördüğünüz bir `hbase(main):001:0>` istemi.

3. Tablodan satırları görüntüleyin:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Bu komut bilgi benzer şekilde tabloda veri olduğunu gösteren aşağıdaki metni döndürür.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Bu tarama işlemi tablodan en fazla 10 satır döndürür.

## <a name="delete-your-clusters"></a>Kümelerinizi Sil

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Kümeleri silmek için depolama ve web uygulaması aynı anda bunları içeren kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

Storm topolojileri Hdınsight ile daha fazla örnekleri için bkz: [Hdınsight üzerinde Storm için örnek topolojiler](apache-storm-example-topology.md)

Apache Storm hakkında daha fazla bilgi için bkz: [Apache Storm](https://storm.incubator.apache.org/) site.

Hdınsight'ta HBase hakkında daha fazla bilgi için bkz: [Hdınsight genel bakış ile HBase](../hbase/apache-hbase-overview.md).

Socket.IO hakkında daha fazla bilgi için bkz: [Socket.IO](http://socket.io/) site.

D3.js hakkında daha fazla bilgi için bkz: [D3.js - veri güdümlü belgeleri](http://d3js.org/).

Java topolojileri oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight üzerinde Apache Storm için geliştirme Java topolojileri](apache-storm-develop-java-topology.md).

.NET topolojileri oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio kullanarak Hdınsight üzerinde Apache Storm için C# topolojileri](apache-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
