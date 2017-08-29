---
title: "Apache Kafka'yı Kullanmaya Başlama - Azure HDInsight | Microsoft Docs"
description: "Azure HDInsight üzerinde Apache Kafka kümesi oluşturmayı öğrenin. Konu başlığı, abonelik ve tüketici oluşturmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 03e6996f0f44e04978080b3bd267e924f342b7fc
ms.contentlocale: tr-tr
ms.lasthandoff: 08/15/2017

---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a>HDInsight üzerinde Apache Kafka'yı (önizleme) kullanmaya başlama

Azure HDInsight üzerinde [Apache Kafka](https://kafka.apache.org) kümesi oluşturmayı ve kullanmayı öğrenin. Kafka, HDInsight ile birlikte kullanılabilen, açık kaynaklı bir dağıtılmış akış platformudur. Yayımla-abone ol ileti kuyruğuna benzer işlevler sağladığı için genellikle ileti aracısı olarak kullanılır.

> [!NOTE]
> Kafka’nın şu anda HDInsight ile kullanılabilen iki sürümü vardır: 0.9.0 (HDInsight 3.4) ve 0.10.0 (HDInsight 3.5 ve 3.6). Bu belgedeki adımlarda HDInsight 3.6 üzerinde Kafka kullandığınız kabul edilmiştir.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Kafka kümesi oluşturma

HDInsight kümesinde Kafka oluşturmak için aşağıdaki adımları kullanın:

1. [Azure portalı](https://portal.azure.com)’ndan **+YENİ**, **Akıllı Özellikler ve Analiz** ve ardından **HDInsight**’ı seçin.
   
    ![HDInsight kümesi oluşturma](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. **Temel Bilgiler** bölümünden aşağıdaki bilgileri girin:

    * **Küme Adı**: HDInsight kümesinin adı.
    * **Abonelik**: Kullanılacak abonelik.
    * **Küme oturumu kullanıcı adı** ve **Küme oturumu parolası**: HTTPS üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Ambari Web kullanıcı arabirimi veya REST API gibi hizmetlere erişmek için bu kimlik bilgilerini kullanın.
    * **Güvenli Kabuk (SSH) kullanıcı adı**: SSH üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Varsayılan olarak parola, küme oturum açma parolası ile aynıdır.
    * **Kaynak Grubu**: Kümenin oluşturulduğu kaynak grubu.
    * **Konum**: Kümenin oluşturulacağı Azure bölgesi.
   
 ![Abonelik seçme](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. **Küme türü**’nü seçin, sonra **Küme yapılandırması**’ndan aşağıdaki değerleri ayarlayın:
   
    * **Küme Türü**: Kafka

    * **Sürüm**: Kafka 0.10.0 (HDI 3.6)

    * **Küme Katmanı**: Standart
     
 Son olarak, **Seç** düğmesini kullanarak ayarları kaydedin.
     
 ![Küme türü seçme](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Küme türünü seçtikten sonra __Seç__ düğmesini kullanarak küme türünü ayarlayın. Ardından, __İleri__ düğmesini kullanarak temel yapılandırmayı tamamlayın.

5. **Depolama**’dan bir depolama hesabı seçin veya oluşturun. Bu belgedeki adımlar için diğer alanları varsayılan değerlerinde bırakın. __İleri__ düğmesini kullanarak depolama yapılandırmasını kaydedin.

    ![HDInsight depolama hesabı ayarlarını belirleme](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Devam etmek için __Uygulamalar (isteğe bağlı)__ bölümünden __İleri__'yi seçin. Bu örnek için uygulama gerekmez.

7. Devam etmek için __Küme boyutu__’ndan __İleri__'yi seçin.

    > [!WARNING]
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir.

    ![Kafka kümesi boyutunu ayarlama](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > **Çalışan düğümü başına disk sayısı** girdisi, HDInsight üzerinde Kafka'nın ölçeklenebilirliğini denetler. Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka'nın depolama alanını ve ölçeklenebilirliğini yapılandırma](hdinsight-apache-kafka-scalability.md).

8. Devam etmek için __Gelişmiş ayarlar__’dan __İleri__'yi seçin.

9. **Özet**’ten kümenin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirmek için __Düzenle__ bağlantılarını kullanın. Son olarak, __Oluştur__ düğmesini kullanarak kümeyi oluşturun.
   
    ![Küme yapılandırma özeti](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Kümenin oluşturulması 20 dakika sürebilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

> [!IMPORTANT]
> Aşağıdaki adımları gerçekleştirirken bir SSH istemcisi kullanmanız gerekir. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

Kümeye bağlanmak için istemcinizde SSH kullanın:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

**SSHUSER** ifadesini küme oluşturma sırasında sağladığınız SSH kullanıcı adıyla değiştirin. **CLUSTERNAME** değerini kümenin adıyla değiştirin.

İstendiğinde, SSH hesabı için kullanılan parolayı girin.

Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Zookeeper ve Aracı konak bilgilerini alma

Kafka ile çalışırken konak değerlerini bilmeniz gerekir; *Zookeeper* konakları ve *Aracı* konakları. Bu konaklar Kafka API’si ve Kafka ile gönderilen yardımcı programların birçoğu ile birlikte kullanılır.

Konak bilgilerini içeren ortam değişkenlerini oluşturmak için aşağıdaki adımları kullanın. Bu ortam değişkenleri bu belgedeki adımlarda kullanılır.

1. Küme ile SSH bağlantısından aşağıdaki komutu kullanarak `jq` yardımcı programını yükleyin. Bu yardımcı program JSON belgelerini ayrıştırmak için kullanılır ve aracı konak bilgilerini almak için yararlıdır:
   
    ```bash
    sudo apt -y install jq
    ```

2. Ambari’den alınan bilgilerle ortam değişkenlerini ayarlamak için aşağıdaki komutları kullanın:

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > `CLUSTERNAME=` değerini Kafka kümesinin adı olarak ayarlayın. `PASSWORD=` değerini kümeyi oluştururken kullandığınız oturum açma (yönetici) parolası olarak ayarlayın.

    Aşağıdaki metin `$KAFKAZKHOSTS` içeriğinin bir örneğidir:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    Aşağıdaki metin `$KAFKABROKERS` içeriğinin bir örneğidir:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > `cut` komutu, konak listesini iki konak girdisine düşürmek için kullanılır. Kafka tüketicisi veya üreticisi oluştururken konakların tam listesini sağlamanız gerekmez.
   
    > [!WARNING]
    > Bu oturumdan döndürülen bilgileri her zaman doğru olarak kabul etmeyin. Kümeyi ölçeklendirirseniz yeni aracılar eklenir veya kaldırılır. Bir hata oluşur ve bir düğüm değiştirilirse, düğümün konak adı değişebilir.
    >
    > Geçerli bilgilere sahip olduğunuzdan emin olmak için, kullanmadan hemen önce Zookeeper ve aracı konakların bilgilerini almanız gerekir.

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Kafka, veri akışlarını *topics* (konu başlıkları) adlı kategorilerde depolar. Kafka ile birlikte verilen betiği kullanarak, bir küme baş düğümüne yönelik SSH bağlantısından konu başlığı oluşturun:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Bu komut, `$KAFKAZKHOSTS` içinde depolanan konak bilgilerini kullanarak Zookeeper’a bağlanır ve ardından **test** adlı Kafka konu başlığını oluşturur. Konu başlıklarını listelemek üzere aşağıdaki betiği kullanarak, konu başlığının oluşturulduğunu doğrulayabilirsiniz:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

Bu komut, **test** konu başlıklarını içeren Kafka konu başlıklarını listeler.

## <a name="produce-and-consume-records"></a>Kayıt oluşturma ve kullanma

Kafka, konu başlıklarında *records* (kayıtlar) depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler, kayıtları Kafka *aracılarından* alır. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır.

Daha önce oluşturduğunuz test konu başlığında kayıt depolamak ve ardından bir tüketici kullanarak bunları okumak için aşağıdaki adımları kullanın:

1. SSH oturumundan, Kafka ile birlikte sağlanan bir betik kullanarak kayıtları konu başlığına yazın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Bu komuttan sonra isteme geri döndürülmezsiniz. Bunun yerine, birkaç metin iletisi yazın ve ardından **Ctrl + C** tuşlarını kullanarak, konu başlığına göndermeyi durdurun. Her satır ayrı bir kayıt olarak gönderilir.

2. Konu başlığından kayıt okumak için, Kafka ile birlikte sağlanan bir betik kullanın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Bu komutla, kayıtlar konu başlığından alınır ve görüntülenir. `--from-beginning` kullanılması, tüketiciye akışın başından başlamasını söyler, böylece tüm kayıtlar alınır.

3. Tüketiciyi durdurmak için __Ctrl + C__ tuşlarını kullanın.

## <a name="producer-and-consumer-api"></a>Üretici ve tüketici API’si

[Kafka API’lerin,](http://kafka.apache.org/documentation#api) kullanarak, kayıtları programlama yoluyla da üretebilir ve kullanabilirsiniz. Java üreticisi ve tüketicisi oluşturmak için geliştirme ortamınızdan aşağıdaki adımları kullanın.

> [!IMPORTANT]
> Aşağıdaki bileşenlerin geliştirme ortamınızda yüklü olması gerekir:
>
> * [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) veya OpenJDK gibi eşdeğeri.
>
> * [Apache Maven](http://maven.apache.org/)
>
> * Bir SSH istemcisi ve `scp` komutu. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

1. Örnekleri [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) sayfasından indirebilirsiniz. Üretici/tüketici örneği için `Producer-Consumer` dizinindeki projeyi kullanın. Bu örnek aşağıdaki sınıfları içerir:
   
    * **Çalıştır** - tüketiciyi veya üreticiyi başlatır.

    * **Producer** (Üretici)- konu başlığında 1.000.000 kayıt depolar.

    * **Consumer** (Tüketici) - konu başlığındaki kayıtları okur.

2. Bir jar paketi oluşturmak için dizinleri `Producer-Consumer` dizininin konumuna geçirin ve aşağıdaki komutu kullanın:

    ```
    mvn clean package
    ```

    Bu komut, `kafka-producer-consumer-1.0-SNAPSHOT.jar` adlı dosyayı içeren `target` adlı bir dizin oluşturur.

3. `kafka-producer-consumer-1.0-SNAPSHOT.jar` dosyasını HDInsight kümenize kopyalamak için aşağıdaki komutları kullanın:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcısının parolasını girin.

4. `scp` komutuyla dosyayı kopyalama işlemi tamamlandığında SSH kullanarak kümeye bağlanın. Test konu başlığına kayıt yazmak için şu komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. İşlem tamamlandıktan sonra, konu başlığından okumak için aşağıdaki komutu kullanın:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Okunan kayıtlar, kayıt sayısıyla birlikte gösterilir. Daha önceki bir adımda, betik kullanarak konu başlığına çok sayıda kayıt gönderdiğiniz için 1.000.000’dan fazla kaydın günlüğe kaydedildiğini görebilirsiniz.

6. Tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın.

### <a name="multiple-consumers"></a>Birden çok tüketici

Kafka tüketicileri kayıtları okurken bir tüketici grubu kullanır. Birden çok tüketiciyle aynı grubun kullanılması, konu başlığından yük dengeli okuma yapılmasına neden olur. Gruptaki her bir tüketici, kayıtların bir kısmını alır. Bu işlemi uygulamada görmek için aşağıdaki adımları kullanın:

1. Kümede yeni bir SSH oturumu açın, böylece ikisine birden sahip olursunuz. Her oturumda aynı tüketici grubu kimliğine sahip bir tüketici başlatmak için aşağıdakileri kullanın:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    Bu komut, `mygroup` grup kimliğini kullanarak bir tüketici başlatır.

    > [!NOTE]
    > Bu SSH oturumuna yönelik `$KAFKABROKERS` ayarını yapmak için [Zookeeper ve Aracı konak bilgilerini alma](#getkafkainfo) bölümündeki komutları kullanın.

2. Her oturumun konu başlığından aldığı kayıtları saymasını izleyebilirsiniz. Her iki oturumun toplamı, daha önce bir tüketiciden aldığınız sayıyla aynı olmalıdır.

Aynı gruptaki istemcilerin tüketimi, konu başlığının bölümleri aracılığıyla işlenir. Daha önce oluşturulan `test` konu başlığında sekiz bölüm vardır. Sekiz SSH oturumu açıp tüm oturumlarda bir tüketici başlatırsanız her tüketici, konu başlığının tek bir bölümündeki kayıtları okur.

> [!IMPORTANT]
> Bir tüketici grubunda bölümden daha fazla tüketici örneği olamaz. Bu örnekte, konu başlığındaki bölüm sayısı sekiz olduğu için bir tüketici grubu en fazla bu sayıda tüketici içerebilir. Ya da her biri en fazla sekiz tüketici içeren birden fazla tüketici grubunuz olabilir.

Kafka’ya depolanan kayıtlar bir bölümde alındıkları sırayla depolanır. *Bir bölüm* içindeki kayıtlar için sıralı teslim sağlamak üzere, tüketici örneklerinin bölüm sayısıyla eşleştiği bir tüketici grubu oluşturun. *Konu başlığı içindeki* kayıtların sıralı teslim edilmesini sağlayabilmek için, yalnızca bir tüketici örneği içeren bir tüketici grubu oluşturun.

## <a name="streaming-api"></a>Akış API’si

Akış API’si Kafka’ya sürüm 0.10.0’da eklenmiştir; önceki sürümler, akış işleme için Apache Spark veya Storm kullanır.

1. Henüz yapmadıysanız, örnekleri [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) sayfasından geliştirme ortamınıza indirin. Akış örneği için `streaming` dizinindeki projeyi kullanın.
   
    Bu proje yalnızca, daha önce `test` konu başlığından kayıtları okuyan `Stream` adlı sınıfı içerir. Okunan sözcükleri sayar ve her sözcük ile sayıyı `wordcounts` adlı konu başlığına iletir. `wordcounts` konu başlığı, bu bölümün sonraki bir adımında oluşturulur.

2. Geliştirme ortamınızdaki komut satırından, dizinleri `Streaming` dizininin konumuna geçirin ve ardından aşağıdaki komutu kullanarak bir jar paketi oluşturun:

    ```bash
    mvn clean package
    ```

    Bu komut, `kafka-streaming-1.0-SNAPSHOT.jar` adlı dosyayı içeren `target` adlı bir dizin oluşturur.

3. `kafka-streaming-1.0-SNAPSHOT.jar` dosyasını HDInsight kümenize kopyalamak için aşağıdaki komutları kullanın:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcısının parolasını girin.

4. `scp` komutu dosya kopyalamayı tamamladıktan sonra, SSH kullanarak kümeye bağlanın ve ardından aşağıdaki komutu kullanarak `wordcounts` konu başlığını oluşturun:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. Ardından, aşağıdaki komutu kullanarak akış işlemini başlatın:
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    Bu komut, arka planda akış işlemini başlatır.

6. `test` konu başlığına ileti göndermek için aşağıdaki komutu kullanın. Bu iletiler akış örneği tarafından işlenir:
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. Akış işlemi tarafından `wordcounts` konu başlığına yazılan çıktıyı görüntülemek için aşağıdaki komutu kullanın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > Verileri görüntülemek için, tüketiciye anahtar ve değer için kullanılacak anahtarı ve seri durumdan çıkarma aracını yazdırmasını söylemeniz gerekir. Anahtar adı sözcüktür ve anahtar değeri sayıyı içerir.
   
    Çıktı aşağıdaki metne benzer:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > Bir sözcükle karşılaşılan her durumda sayı artar.

7. Tüketiciden çıkmak için __Ctrl + C__ tuşlarını kullanın, ardından `fg` komutunu kullanarak akış arka plan görevini ön plana geri getirin. Çıkış yapmak için de __Ctrl + C__ tuşlarını kullanın.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Apache Kafka ile çalışmanın temel bilgilerini öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [HDInsight üzerinde Kafka ile verilerinizin yüksek kullanılabilirliğini sağlama](hdinsight-apache-kafka-high-availability.md)
* [HDInsight üzerinde Kafka ile yönetilen diskleri yapılandırarak ölçeklenebilirliği artırma](hdinsight-apache-kafka-scalability.md)
* kafka.apache.org adresindeki [Apache Kafka belgeleri](http://kafka.apache.org/documentation.html).
* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](hdinsight-apache-kafka-mirroring.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](hdinsight-apache-storm-with-kafka.md)
* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](hdinsight-apache-spark-with-kafka.md)
* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](hdinsight-apache-kafka-connect-vpn-gateway.md)

