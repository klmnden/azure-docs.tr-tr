---
title: Azure portalı - Hızlı Başlangıç'ı kullanarak HDInsight üzerinde Apache kafka'yı ayarlama
description: Bu hızlı başlangıçta, Azure portalını kullanarak Azure HDInsight’ta bir Apache Kafka kümesi oluşturmayı öğrenirsiniz. Kafka konuları, aboneleri ve tüketicileri hakkında da bilgi edinirsiniz.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/12/2019
ms.openlocfilehash: 61ae6cdf7c31c9a6e40860eb1dc4628bb2d37496
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150897"
---
# <a name="quickstart-create-apache-kafka-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure HDInsight Apache Kafka kümesi oluşturma

Apache Kafka açık kaynaklı, dağıtılmış bir akış platformudur. Yayımla-abone ol ileti kuyruğuna benzer işlevler sağladığı için genellikle ileti aracısı olarak kullanılır. 

Bu hızlı başlangıçta, Azure portalını kullanarak [Apache Kafka](https://kafka.apache.org) kümesi oluşturmayı öğrenirsiniz. Ayrıca Apache Kafka kullanarak ileti göndermek ve almak için verilen yardımcı programları kullanmayı da öğrenirsiniz.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Apache Kafka API'sine yalnızca aynı sanal ağ içindeki kaynaklar tarafından erişilebilir. Bu hızlı başlangıçta, doğrudan SSH kullanarak kümeye erişirsiniz. Diğer hizmetleri, ağları veya sanal makineleri Apache Kafka'ya bağlamak için önce bir sanal ağ oluşturmanız e sonra ağ içinde kaynakları oluşturmanız gerekir. Daha fazla bilgi için [Sanal ağ kullanarak Apache Kafka'ya bağlanma](apache-kafka-connect-vpn-gateway.md) belgesine bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-an-apache-kafka-cluster"></a>Apache Kafka kümesi oluşturma

HDInsight kümesinde Apache Kafka oluşturmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol menüden gidin **+ kaynak Oluştur** > **Analytics** > **HDInsight**.
   
    ![HDInsight kümesi oluşturma](./media/apache-kafka-get-started/create-hdinsight.png)

3. **Temel Bilgiler** bölümünde aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | --- | --- |
    | Küme Adı | HDInsight kümesine ait benzersiz bir ad. |
    | Abonelik | Aboneliğinizi seçin. |
    
   **Küme yapılandırması**’nı görüntülemek için __Küme Türü__’nü seçin.
   
   ![HDInsight üzerinde Apache Kafka kümesi temel yapılandırma](./media/apache-kafka-get-started/custom-basics-kafka.png)

4. Gelen __küme yapılandırması__, aşağıdaki değerleri seçin:

    | Ayar | Değer |
    | --- | --- |
    | Küme Türü | Kafka |
    | Version | Kafka 1.1.0 (HDI 3.6) |

    Seçin **seçin** küme türü ayarlarını kaydedin ve dönmek için __Temelleri__.

    ![Küme türü seçme](./media/apache-kafka-get-started/kafka-cluster-type.png)

5. __Temel Bilgiler__ bölümünde aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | --- | --- |
    | Küme oturum açma kullanıcı adı | Web hizmetlerine veya küme üzerinde barındırılan REST API’lerine erişirken kullanılan oturum açma kimliği. Varsayılan değeri (admin) değiştirmeyin. |
    | Küme oturum açma parolası | Web hizmetlerine veya küme üzerinde barındırılan REST API’lerine erişirken kullanılan oturum açma parolası. |
    | Secure Shell (SSH) kullanıcı adı | SSH üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Varsayılan olarak parola, küme oturum açma parolası ile aynıdır. |
    | Kaynak Grubu | İçinde kümenin oluşturulduğu kaynak grubu. |
    | Location | İçinde kümenin oluşturulacağı Azure bölgesi. |

    Her Azure bölgesi (konum) _hata etki alanları_ sağlar. Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

    Verilerin yüksek kullanılabilirliği için, __üç hata etki alanı__ içeren bir bölge (konum) seçin. Bir bölgedeki hata etki alanlarının sayısı hakkında bilgi almak için [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesine bakın.

   ![Abonelik seçme](./media/apache-kafka-get-started/hdinsight-basic-configuration-2.png)

    Seçin __sonraki__ temel yapılandırmayı tamamlayın.

6. Bu hızlı başlangıçta varsayılan güvenlik ayarlarını değiştirmeden bırakın. Kurumsal Güvenlik paketi hakkında daha fazla bilgi edinmek için [Azure Active Directory Domain Services'i kullanarak bir HDInsight kümesi ile Kurumsal Güvenlik Paketi yapılandırma](../domain-joined/apache-domain-joined-configure-using-azure-adds.md) sayfasını ziyaret edin. Apache Kafka Disk Şifrelemesi'nde kendi anahtarınızı kullanmayı öğrenmek için [Azure HDInsight üzerinde Apache Kafka için kendi anahtarını getir](apache-kafka-byok.md) sayfasını ziyaret edin

   Kümenizi bir sanal ağa bağlamak istiyorsanız, aşağı açılan **Sanal ağ** listesinden bir sanal ağ seçin.

   ![Sanal ağa küme ekleme](./media/apache-kafka-get-started/kafka-security-config.png)

7. **Depolama**’dan bir depolama hesabı seçin veya oluşturun. Bu belgedeki adımlar için diğer alanları varsayılan değerlerinde bırakın. __İleri__ düğmesini kullanarak depolama yapılandırmasını kaydedin. Data Lake depolama Gen2 kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: HDInsight kümelerinde ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

   ![HDInsight depolama hesabı ayarlarını belirleme](./media/apache-kafka-get-started/storage-configuration.png)

8. __Uygulamalar (isteğe bağlı)__ bölümünden __İleri__'yi seçerek varsayılan ayarlarla devam edin.

9. __Küme boyutu__ bölümünden __İleri__'yi seçerek varsayılan ayarlarla devam edin.

    Apache Kafka'nın HDInsight üzerinde kullanılabilirliğini garanti etmek için __çalışan düğümleri sayısı__ girişinin 3 veya daha büyük bir değere ayarlanması gerekir. Varsayılan değer 4'tür.

    **Çalışan düğümü başına disk sayısı** girişi HDInsight üzerinde Apache Kafka'nın ölçeklenebilirliğini yapılandırır. HDInsight üzerinde Apache Kafka, veri depolamak için kümedeki sanal makinelerin yerel diskini kullanır. Apache Kafka yoğun G/Ç kullandığından yüksek aktarım hızı ve düğüm başına daha fazla depolama alanı sağlamak için [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. Yönetilen diskin türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Disk türü, çalışan düğümler (Apache Kafka aracıları) tarafından kullanılan sanal makine boyutuna bağlıdır. Premium diskler otomatik olarak DS ve GS serisi sanal makinelerle kullanılır. Diğer tüm VM türleri standart disk kullanır.

   ![Apache Kafka küme boyutunu ayarlama](./media/apache-kafka-get-started/kafka-cluster-size.png)

10. __Gelişmiş ayarlar__ bölümünden __İleri__'yi seçerek varsayılan ayarlarla devam edin.

11. **Özet**’ten kümenin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirmek için __Düzenle__ bağlantılarını kullanın. Son olarak, seçin **Oluştur** kümeyi oluşturun.

    ![Küme yapılandırma özeti](./media/apache-kafka-get-started/kafka-configuration-summary.png)

    Kümenin oluşturulması 20 dakika sürebilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

1. Apache Kafka kümesinin birincil baş düğümüne bağlanmak için aşağıdaki komutu kullanın. `sshuser` değerini, SSH kullanıcı adıyla değiştirin. Değiştirin `mykafka` , Apache Kafkacluster adı.

    ```bash
    ssh sshuser@mykafka-ssh.azurehdinsight.net
    ```

2. Kümeye ilk kez bağlandığınızda SSH istemciniz ana bilgisayarın orijinalliğinin belirlenemediği yönünde bir uyarı görüntüleyebilir. Ana bilgisayarı SSH istemcinizin güvenilen sunucular listesine eklemek isteyip istemediğiniz sorulduğunda __evet__’i seçin ve sonra __Enter__ tuşuna basın.

3. İstendiğinde, SSH kullanıcısının parolasını girin.

    Bağlandığında, aşağıdaki metne benzer bilgiler görürsünüz:
    
    ```text
    Authorized uses only. All activity may be monitored and reported.
    Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.13.0-1011-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud
    
    83 packages can be updated.
    37 updates are security updates.


    Welcome to Apache Kafka on HDInsight.
    
    Last login: Thu Mar 29 13:25:27 2018 from 108.252.109.241
    ssuhuser@hn0-mykafk:~$
    ```

## <a id="getkafkainfo"></a>Apache Zookeeper ve aracı konak bilgilerini alma

Kafka ile çalışırken bilmeniz gerekir *Apache Zookeeper* ve *Aracısı* konaklar. Bu konaklar Apache Kafka API'si ve Kafka ile gönderilen yardımcı programların birçoğu ile birlikte kullanılır.

Bu bölümde, küme üzerinde Apache Ambari REST API konak bilgilerini alın.

1. Yükleme [jq](https://stedolan.github.io/jq/), bir komut satırı JSON işlemcisine giden. Bu yardımcı program JSON belgelerini ayrıştırmak için kullanılır ve konak bilgilerini ayrıştırma işlemlerinde yararlıdır. Açık SSH bağlantısından yüklemek için komutu girin `jq`:
   
    ```bash
    sudo apt -y install jq
    ```

2. Ortam değişkenlerini ayarlamak. Değiştirin `PASSWORD` ve `CLUSTERNAME` ile küme oturum açma parolasını ve küme adı sırasıyla, sonra komutu girin:

    ```bash
    export password='PASSWORD'
    export clusterNameA='CLUSTERNAME'
    ```

3. Büyük küçük harfleri doğru extract küme adı. Küme adı gerçek büyük küçük harfleri, küme nasıl oluşturulduğuna bağlı olarak, beklenenden farklı olabilir. Bu komut gerçek büyük/küçük harf almak, bir değişkende depolayın ve sonra görünen doğru cased adı ve daha önce belirtilen adı. Aşağıdaki komutu girin:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "https://$clusterNameA.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    echo $clusterName, $clusterNameA
    ```

4. Bir ortam değişkenini Zookeeper konak bilgileriyle ayarlamak için aşağıdaki komutu kullanın. Komut, tüm Zookeeper konaklar alır, ardından yalnızca ilk iki girişe döndürür. Bunun nedeni, bir ana bilgisayarın ulaşılamaz olması durumunda yedeklilik istemenizdir.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Bu komut küme baş düğümünde doğrudan Ambari hizmetini sorgular. Ambari genel adresini kullanarak da erişebilirsiniz `https://$CLUSTERNAME.azurehdinsight.net:80/`. Bazı ağ yapılandırmaları genel adrese erişimi engelleyebilir. Örneğin, bir sanal ağda HDInsight’a erişimi kısıtlamak için Ağ Güvenlik Grupları’nı (NSG) kullanmak gibi.

5. Ortam değişkeninin düzgün şekilde ayarlandığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    echo $KAFKAZKHOSTS
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

6. Bir ortam değişkenini Apache Kafka aracı konak bilgileriyle ayarlamak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

7. Ortam değişkeninin düzgün şekilde ayarlandığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash   
    echo $KAFKABROKERS
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

## <a name="manage-apache-kafka-topics"></a>Apache Kafka konularını yönetme

Kafka, veri akışlarını *konular* içinde depolar. Konuları yönetmek için `kafka-topics.sh` yardımcı programını kullanabilirsiniz.

* **Bir konu oluşturmak için**, SSH bağlantısında aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `$KAFKAZKHOSTS` içinde depolanan konak bilgilerini kullanarak Zookeeper’a bağlanır. Daha sonra **test** adlı bir Apache Kafka konusu oluşturur. 

    * Bu konuda depolanan veriler, sekiz bölüm halinde bölümlenir.

    * Her bölüm, kümedeki üç çalışan düğümü arasında çoğaltılır.

        Üç hata etki alanı sağlayan bir Azure bölgesinde kümeyi oluşturduysanız, 3 çoğaltma katsayısını kullanın. Aksi takdirde 4 çoğaltma katsayısını kullanın.
        
        Üç hata etki alanı içeren bölgelerde 3 çoğaltma katsayısı, çoğaltmaların hata etki alanları arasında yayılmasına olanak sağlar. İki hata etki alanı içeren bölgelerde dört çoğaltma katsayısı, çoğaltmaların etki alanları arasında eşit şekilde yayılmasına olanak sağlar.
        
        Bir bölgedeki hata etki alanlarının sayısı hakkında bilgi almak için [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesine bakın.

        Apache Kafka, Azure hata etki alanları ile uyumlu değildir. Konular için bölüm çoğaltmaları oluşturulurken, çoğaltmalar yüksek kullanılabilirlik için düzgün şekilde dağıtılmayabilir.

        Yüksek kullanılabilirlik sağlamak için kullanın [Apache Kafka bölüm yeniden Dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools). Bu araç, Apache Kafka kümenizin baş düğümüyle kurulan bir SSH bağlantısından çalıştırılmalıdır.

        Apache Kafka verilerinizin en yüksek kullanılabilirliğe sahip olması için aşağıdaki durumlarda konunuz için bölüm çoğaltmalarını yeniden dengelemeniz gerekir:

        * Yeni bir konu veya bölüm oluşturduğunuzda

        * Bir kümenin ölçeğini artırdığınızda

* **Konuları listelemek için** aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, Apache Kafka kümesinde bulunan konuları listeler.

* **Bir konuyu silmek için** aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic topicname --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `topicname` adlı konuyu siler.

    > [!WARNING]  
    > Daha önce oluşturulan `test` konusunu silerseniz, yeniden oluşturmanız gerekir. Bu belgenin ilerleyen kısmındaki adımlarda kullanılacaktır.

`kafka-topics.sh` yardımcı programı ile kullanılabilen komutlar hakkında daha fazla bilgi için aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh
```

## <a name="produce-and-consume-records"></a>Kayıt oluşturma ve kullanma

Kafka, konu başlıklarında *records* (kayıtlar) depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler ve tüketiciler, *Kafka aracısı* hizmetiyle iletişim kurar. HDInsight kümenizdeki her çalışan düğümü bir Apache Kafka aracı konağıdır.

Daha önce oluşturduğunuz test konu başlığında kayıtları depolamak ve ardından bir tüketici kullanarak bunları okumak için aşağıdaki adımları kullanın:

1. Konuya kayıtlar yazmak için SSH bağlantısından `kafka-console-producer.sh` yardımcı programını kullanın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Bu komuttan sonra boş bir satıra ulaşırsınız.

2. Boş satıra bir metin iletisi yazın ve Enter tuşuna basın. Bu şekilde birkaç ileti girin ve sonra normal isteme geri dönmek için **Ctrl + C** tuşlarını kullanın. Her satır, Apache Kafka konusuna ayrı bir kayıt olarak gönderilir.

3. Konudan kayıtları okumak için SSH bağlantısından `kafka-console-consumer.sh` yardımcı programını kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```

    Bu komutla, kayıtlar konu başlığından alınır ve görüntülenir. `--from-beginning` kullanılması, tüketiciye akışın başından başlamasını söyler, böylece tüm kayıtlar alınır.

    Kafka’nın eski bir sürümünü kullanıyorsanız `--bootstrap-server $KAFKABROKERS` değerini `--zookeeper $KAFKAZKHOSTS` ile değiştirin.

4. Tüketiciyi durdurmak için __Ctrl + C__ tuşlarını kullanın.

Ayrıca programlı olarak üretici ve tüketici de oluşturabilirsiniz. Bu API kullanma örneği için bkz: [Apache Kafka üretici ve tüketici API'si HDInsight ile](apache-kafka-producer-consumer-api.md) belge.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç tarafından oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

> [!WARNING]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz.
> 
> HDInsight üzerinde Apache Kafka kümesini silmek Kafka’da depolanmış tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Apache Kafka ile Apache Spark kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
