---
title: "Storm hdınsight'ta - Azure Apache Kafka kullanın | Microsoft Docs"
description: "Apache Kafka, Hdınsight üzerinde Apache Storm ile yüklenir. Kafka için yazma ve ondan, Storm ile sağlanan KafkaBolt ve KafkaSpout Bileşenleri'ni kullanarak okunur öğrenin. Ayrıca Flux framework tanımlayın ve Storm topolojilerini göndermek için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/08/2018
ms.author: larryfr
ms.openlocfilehash: 0c74e46f37319a9d1eb0ea1587087e24312de451
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="use-apache-kafka-with-storm-on-hdinsight"></a>Hdınsight üzerinde Storm ile Apache Kafka kullanın

Apache Storm okuma ve yazma Apache Kafka için nasıl kullanılacağını öğrenin. Bu örnek ayrıca bir Storm topolojisinin verilerden Hdınsight tarafından kullanılan HDFS uyumlu dosya sistemine kaydetmek nasıl gösterir.

> [!NOTE]
> Bu belgede yer alan adımlar, hem Hdınsight üzerinde Storm ve Hdınsight kümesinde bir Kafka içeren bir Azure kaynak grubu oluşturun. Bu kümeleri, hem bir Azure sanal Kafka ile doğrudan iletişim kurmak Storm kümesi sağlayan ağ içinde bulunan küme ' dir.
> 
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümelerini Sil unutmayın.

## <a name="get-the-code"></a>Kodu alma

Bu belgede kullanılan örnek kodunu şu adresten edinilebilir [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).

Bu projeyi derlemek için geliştirme ortamınız için aşağıdaki yapılandırma gerekir:

* [Java JDK 1.8](http://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html) ya da daha yüksek. Java 8 Hdınsight 3.5 veya daha yükseğini gerektirir.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Bir SSH istemcisi (ihtiyacınız `ssh` ve `scp` komutları) - bilgi için bkz: [Hdınsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* Bir metin düzenleyicisi veya IDE.

Geliştirme iş istasyonunuza Java ve JDK yüklediğinizde aşağıdaki ortam değişkenleri ayarlayabilirsiniz. Ancak, bunlar mevcut olduğundan ve sisteminiz için doğru değerleri içerdikleri denetlemeniz gerekir.

* `JAVA_HOME` -JDK yüklendiği dizinine işaret etmelidir.
* `PATH` -aşağıdaki yolları içermelidir:
  
    * `JAVA_HOME` (veya eşdeğer yolu).
    * `JAVA_HOME\bin` (veya eşdeğer yolu).
    * Maven'ın yüklendiği dizin.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

Hdınsight üzerinde Apache Kafka erişim genel internet üzerinden Kafka aracıların sağlamaz. İçin Kafka ettiği herhangi bir şey Kafka kümedeki düğümlerin aynı Azure sanal ağ içinde olmalıdır. Bu örnekte, bir Azure sanal ağında Kafka ve Storm kümeleri bulunur. Aşağıdaki diyagramda, iletişim kümeleri arasında nasıl aktığını gösterir:

![Bir Azure sanal ağı Storm ve Kafka kümelerde diyagramı](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> SSH ve Ambari gibi kümedeki diğer hizmetlerin internet üzerinden erişilebilir. Hdınsight ile kullanılabilen ortak bağlantı noktaları hakkında daha fazla bilgi için bkz: [bağlantı noktaları ve Hdınsight tarafından kullanılan URI](hdinsight-hadoop-port-settings-for-services.md).

Azure sanal ağı, Kafka, oluşturabilir ve Storm el ile kümeleri olsa da, bir Azure Resource Manager şablonunu kullanmak daha kolaydır. Azure sanal ağı, Kafka, dağıtmak ve kümeler Azure aboneliğinize Storm için aşağıdaki adımları kullanın.

1. Azure'da oturum açın ve Azure portalında şablon açmak için aşağıdaki düğmesini kullanın.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-storm-java-kafka%2Fmaster%2Fcreate-kafka-storm-clusters-in-vnet.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Azure Resource Manager şablonu bulunur **https://github.com/Azure-Samples/hdinsight-storm-java-kafka/blob/master/create-kafka-storm-clusters-in-vnet.json**. Aşağıdaki kaynaklar oluşturur:
    
    * Azure kaynak grubu
    * Azure Sanal Ağ
    * Azure Storage hesabı
    * Hdınsight sürüm 3.6 (üç alt düğümleri) üzerindeki Kafka
    * Sürüm 3.6 (üç alt düğümler) hdınsight'ta Storm

  > [!WARNING]
  > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç alt düğümleri içeren bir Kafka küme oluşturur.

2. Üzerinde girişleri doldurmak için aşağıdaki yönergeleri kullanın **özel dağıtım** bölümü:
   
    ![Hdınsight özel dağıtım](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Kaynak grubu**: bir grup oluşturun veya varolan bir tanesini seçin. Bu grup, Hdınsight kümesi içerir.
   
    * **Konum**: coğrafi olarak yakın bir konum seçin.

    * **Temel küme adı**: Bu değer Storm için temel adı olarak kullanılır ve Kafka kümeleri. Örneğin, **hdı** adlı bir Storm kümesi oluşturur **storm hdı** ve adlı Kafka küme **kafka hdı**.
   
    * **Oturum açma kullanıcı adı küme**: Storm ve Kafka kümeleri için yönetici kullanıcı adı.
   
    * **Oturum açma parolası küme**: Storm ve Kafka kümeleri için yönetici kullanıcı parolası.
    
    * **SSH kullanıcı adı**: için Storm ve Kafka kümeleri oluşturmak için SSH kullanıcı.
    
    * **SSH parolası**: Storm ve Kafka kümelerinin SSH kullanıcısının parolası.

3. Okuma **hüküm ve koşullar**ve ardından **hüküm ve koşulları yukarıda belirtildiği ediyorum**.

4. Son olarak, denetleme **panoya Sabitle** ve ardından **satın alma**. Kümeleri oluşturmak için yaklaşık 20 dakika sürer.

Kaynakları oluşturduktan sonra kaynak grubu için bölüm görüntülenir.

![Kaynak grubu bölümüne vnet ve kümeler için](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Hdınsight kümeleri adlarının bildirimi **storm BASENAME** ve **kafka BASENAME**, BASENAME şablona verdiğiniz adı olduğu. Kümeye bağlanırken bu adları daha sonraki adımlarda kullanın.

## <a name="understanding-the-code"></a>Kodu anlama

Bu proje iki topoloji içerir:

* **KafkaWriter**: tarafından tanımlanan **writer.yaml** dosyası, bu topoloji Yazar rastgele cümleleri Kafka için Apache Storm ile sağlanan KafkaBolt kullanarak.

    Bu topoloji bir özel kullanan **SentenceSpout** rastgele cümleleri oluşturmak için bileşen.

* **KafkaReader**: tarafından tanımlanan **reader.yaml** dosyası, bu topoloji Kafka Apache Storm ile sağlanan KafkaSpout kullanarak verileri okur ve sonra stdout verilerini kaydeder.

    Bu topoloji Storm HdfsBolt Storm kümesi için varsayılan depolama verileri yazmak için kullanır.
### <a name="flux"></a>Flux

Topolojileri kullanılarak tanımlanır [Flux](https://storm.apache.org/releases/1.1.2/flux.html). Flux içinde sunulmuştur 0.10.x Storm ve kodun topoloji yapılandırması ayrı olanak sağlar. Flux framework kullanan Topolojileri için topoloji YAML dosyasında tanımlanır. YAML dosya topolojisini bir parçası olarak dahil edilebilir. Bu topoloji gönderdiğinizde kullanılan tek başına dosya de olabilir. Flux çalışma zamanında Bu örnekte kullanılan, değişkeni değiştirme de destekler.

Aşağıdaki parametreleri bu topolojiler için çalışma zamanında ayarlanır:

* `${kafka.topic}`: Topolojileri okuma/yazma için Kafka konu adı.

* `${kafka.broker.hosts}`: Kafka aracıların konakları çalıştırın. Aracısı bilgileri tarafından KafkaBolt için Kafka yazılırken kullanılır.

* `${kafka.zookeeper.hosts}`: Zookeeper Kafka kümesinde çalışır ana bilgisayar.

Flux topolojileri hakkında daha fazla bilgi için bkz: [https://storm.apache.org/releases/1.1.2/flux.html](https://storm.apache.org/releases/1.1.2/flux.html).

## <a name="download-and-compile-the-project"></a>Karşıdan yükle ve projeyi derleme

1. Geliştirme ortamınızı projesinden indirmeniz [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), bir komut satırı açın ve dizinleri proje indirdiğiniz konuma değiştirin.

2. Gelen **hdınsight storm java kafka** dizin, projeyi derlemek ve dağıtım için bir paketi oluşturmak için aşağıdaki komutu kullanın:

  ```bash
  mvn clean package
  ```

    Paket işlemi adlı bir dosya oluşturur `KafkaTopology-1.0-SNAPSHOT.jar` içinde `target` dizin.

3. Hdınsight kümesi üzerinde storm'a paketi kopyalamak için aşağıdaki komutları kullanın. Değiştir **kullanıcıadı** küme için SSH kullanıcı adı. Değiştir **BASENAME** küme oluştururken kullandığınız temel ada sahip.

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    İstendiğinde, kümeler oluşturulurken kullanılan parolayı girin.

## <a name="configure-the-topology"></a>Topolojisini yapılandırma

1. Kafka Aracısı konaklarda bulmak için aşağıdaki yöntemlerden birini kullanın **Kafka** Hdınsight kümesinde:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    > [!IMPORTANT]
    > Aşağıdaki Bash örneği varsayar `$CLUSTERNAME` adını içeren __Kafka__ küme adı. Ayrıca varsayılmaktadır [jq](https://stedolan.github.io/jq/) 1.5 veya daha büyük bir sürümü yüklü. İstendiğinde, küme oturum açma hesabı için parolayı girin.

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    Döndürülen değer aşağıdakine benzer:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > Kümeniz için ikiden fazla Aracısı konakları olsa istemcilere tüm ana bilgisayarlar, tam bir listesi sağlanmaktadır gerekmez. Bir veya iki yeterlidir.

2. Zookeeper bulmak için aşağıdaki yöntemlerden birini barındıran için kullanım __Kafka__ Hdınsight kümesinde:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    > [!IMPORTANT]
    > Aşağıdaki Bash örneği varsayar `$CLUSTERNAME` adını içeren __Kafka__ küme. Ayrıca varsayılmaktadır [jq](https://stedolan.github.io/jq/) yüklenir. İstendiğinde, küme oturum açma hesabı için parolayı girin.

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    Döndürülen değer aşağıdakine benzer:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > İkiden fazla Zookeeper düğümleri olsa da, tüm konaklar istemcileri için tam bir listesi sağlanmaktadır gerekmez. Bir veya iki yeterlidir.

    Bu değer daha sonra kullanılmak üzere kaydedin.

3. Düzen `dev.properties` proje kökündeki dosyasında. Aracısı ve Zookeeper ana bilgisayar bilgilerini eklemek __Kafka__ bu dosyadaki eşleşen satır kümesi. Aşağıdaki örnek, önceki adımları örnek değerleri kullanarak yapılandırılır:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. Kaydet `dev.properties` dosya ve ona karşıya yüklemek için aşağıdaki komutu kullanın **Storm** küme:

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties
    ```

    Değiştir **kullanıcıadı** küme için SSH kullanıcı adı. Değiştir **BASENAME** küme oluştururken kullandığınız temel ada sahip.

## <a name="start-the-writer"></a>Yazıcı Başlat

> [!IMPORTANT]
> Bu bölümdeki adımları Kafka ve Storm kümeleri oluşturmak için bu belgede Azure Resource Manager şablonu bağlantı kullanılan varsayalım. Bu şablonu konuları Kafka küme için otomatik olarak oluşturulmasını sağlar.
>
> Varsayılan olarak, konular, otomatik olarak oluşturulmasını hdınsight'ta Kafka izin vermeyecek şekilde Kafka küme oluşturmak için başka bir yöntem kullandıysanız, konu el ile oluşturmanız gerekir. El ile bir konu oluşturma hakkında daha fazla bilgi için bkz: [hdınsight'ta Kafka başlayarak](./kafka/apache-kafka-get-started.md) belge.

1. Bağlanmak için aşağıdakileri kullanın **Storm** SSH kullanarak küme. Değiştir **kullanıcıadı** küme oluşturulurken kullanılan SSH kullanıcı adı. Değiştir **BASENAME** küme oluşturulurken kullanılan taban adına sahip.

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    İstendiğinde, kümeler oluşturulurken kullanılan parolayı girin.
   
    Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Storm kümesi için SSH bağlantısı yazan topoloji başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    Bu komutla birlikte kullanılan parametreler şunlardır:

    * `org.apache.storm.flux.Flux`: Flux yapılandırmak ve bu topoloji çalıştırmak için kullanın.

    * `--remote`: Nimbus topolojiye gönderin. Topoloji kümedeki çalışan düğümü dağıtılır.

    * `-R /writer.yaml`: Kullanın `writer.yaml` topolojisini yapılandırmak için dosya. `-R` Bu kaynak jar dosyasına dahil olduğunu gösterir. Bu nedenle jar kök dizininde olduğundan `/writer.yaml` yolu.

    * `--filter`: Girdileri doldurmak `writer.yaml` değerleri kullanarak topolojisi `dev.properties` dosya. Örneğin, değeri `kafka.topic` dosyasındaki giriş değiştirmek için kullanılan `${kafka.topic}` topoloji tanımı girişi.

5. Topoloji başladıktan sonra bu veriler Kafka konuya yazıyor doğrulamak için aşağıdaki komutu kullanın:

    > [!IMPORTANT]
    > Değiştir `$KAFKAZKHOSTS` ile Zookeeper barındırmak için bilgi __Kafka__ küme.

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    Bu komut Kafka ile birlikte gelen bir komut dosyası konu izlemek için kullanır. Bir süre sonra konu ile yazılmış rastgele cümleleri döndürme başlamalısınız. Çıktı aşağıdaki örneğe benzer:

        i am at two with nature             
        an apple a day keeps the doctor away
        snow white and the seven dwarfs     
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        four score and seven years ago      
        snow white and the seven dwarfs     
        snow white and the seven dwarfs     
        i am at two with nature             
        an apple a day keeps the doctor away

    Komut dosyası durdurmak için CTRL + c'ı kullanın.

## <a name="start-the-reader"></a>Okuyucu Başlat

> [!NOTE]
> Storm kullanıcı Arabirimi okuyucusunda görüntülerken görebileceğiniz bir __topoloji spout'lar öteleme hata__ bölümü. Bu örnekte, bu hatayı yoksayabilirsiniz.

1. Storm kümesi için SSH oturumundan okuyucu topoloji başlatmak için aşağıdaki komutu kullanın:

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. Topoloji başladıktan sonra Storm kullanıcı arabirimini açın. Bu web kullanıcı Arabirimi adresindedir `https://storm-BASENAME.azurehdinsight.net/stormui`. Değiştir __BASENAME__ küme oluştururken kullanılan taban adına sahip. 

    İstendiğinde, yönetici oturum açma adı kullanın (varsayılan, `admin`) ve küme oluştururken kullanılan parola. Aşağıdaki resme benzeyen bir web sayfasına bakın:

    ![Storm kullanıcı Arabirimi](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Storm kullanıcı Arabirimi seçin __kafka okuyucu__ bağlamak __topoloji özeti__ hakkında bilgileri görüntülemek için bölüm __kafka okuyucu__ topolojisi.

    ![Topoloji özeti kısmını Storm web kullanıcı Arabirimi](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. Günlükçü Cıvata bileşenin örnekleri hakkında bilgi görüntülemek için seçin __Günlükçü Cıvata__ bağlamak __Cıvatalar (her zaman)__ bölümü.

    ![Cıvatalar bölümündeki Günlükçü Cıvata bağlantıyı](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. İçinde __yürütücüler__ bölümünde, bir bağlantıyı seçin __bağlantı noktası__ bileşenin bu örneği günlük bilgilerini görüntülemek için sütun.

    ![Yürütücüler bağlantı](./media/hdinsight-apache-storm-with-kafka/executors.png)

    Günlük Kafka konusundan okunan veriler günlüğünü içerir. Günlük bilgileri aşağıdakine benzer:

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] the cow jumped over the moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: the cow jumped over the moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps the doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a>Topolojileri Durdur

Storm kümesi için bir SSH oturumundan Storm topolojilerini durdurmak için aşağıdaki komutları kullanın:

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgede yer alan adımlar her iki kümeleri aynı Azure kaynak grubu oluşturmak için Azure portalında kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, bu belgede aşağıdaki tarafından oluşturulan tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight üzerinde Storm ile kullanılan daha fazla örnek Topolojileri için bkz: [örnek Storm topolojileri ve bileşenleri](storm/apache-storm-example-topology.md).

Dağıtma ve Linux tabanlı Hdınsight üzerinde topolojileri izleme hakkında daha fazla bilgi için bkz: [dağıtma ve Linux tabanlı Hdınsight üzerinde Apache Storm topolojilerini yönetme](storm/apache-storm-deploy-monitor-topology-linux.md)