---
title: Azure HDInsight'ı kullanarak Storm sorunlarını giderme
description: Azure HDInsight ile Apache Storm kullanma hakkında sık sorulan soruların yanıtlarını alın.
keywords: Azure HDInsight, Storm, SSS, sorun giderme kılavuzu, yaygın sorunlar
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 3866e25cc3c87f569e84b2d5639d25aa9386cc78
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713761"
---
# <a name="troubleshoot-apache-storm-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Apache Storm sorunlarını giderme

Sık karşılaşılan sorunlar ve çözümleri ile çalışmak için öğrenin [Apache Storm](https://storm.apache.org/) yüklerde [Apache Ambari](https://ambari.apache.org/).

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Bir küme üzerindeki Storm kullanıcı arabirimini nasıl erişim sağlanır?
Storm kullanıcı arabirimini bir tarayıcıdan erişirken için iki seçeneğiniz vardır:

### <a name="ambari-ui"></a>Ambari UI
1. Ambari panosuna gidin.
2. Hizmetler listesinde seçin **Storm**.
3. İçinde **hızlı bağlantılar** menüsünde **Storm kullanıcı arabirimini**.

### <a name="direct-link"></a>Doğrudan bağlantı
Storm kullanıcı arabirimini şu URL'den erişebilirsiniz:

https://\<küme DNS adını\>/stormui

Örnek:

 https://stormcluster.azurehdinsight.net/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Nasıl ı Storm olay hub'ı spout denetim noktası bilgilerini bir topolojiden diğerine aktarma?

Azure Event Hubs'dan okumayı topolojiler geliştirirken, .jar dosyasını spout HDInsight Storm olay hub'ı kullanarak, yeni kümede aynı ada sahip bir topoloji dağıtmanız gerekir. Ancak, hassastır denetim noktası verileri korumanız gerekir [Apache ZooKeeper](https://zookeeper.apache.org/) eski kümede.

### <a name="where-checkpoint-data-is-stored"></a>Denetim noktası verilerin depolandığı
Denetim noktası verileri ofsetleri için iki kök yolları, ZooKeeper, olay hub'ı spout tarafından depolanır:
- İşleme uygun olmayan spout denetim noktası /eventhubspout içinde depolanır.
- İşlem spout denetim noktası verileri / işlem depolanır.

### <a name="how-to-restore"></a>Geri yükleme
ZooKeeper dışında verilerini dışarı aktarın ve ardından yeni bir adla ZooKeeper dön verileri almak için kullandığınız kitaplıklar ve komut dosyalarını almak için bkz. [HDInsight Storm örnekleri](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

LIB klasör, içeri/dışarı aktarma işlemi uygulamasını içeren .jar dosyalarını içerir. Bash klasör verilerini eski kümede ZooKeeper sunucusundan dışarı aktarmak ve yeni kümede ZooKeeper sunucuya geri alma yapmayı gösteren bir örnek betiği içerir.

Çalıştırma [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) betiğinden sonra verileri içeri ve dışarı aktarmak, ZooKeeper düğümleri. Betik doğru Hortonworks Data Platform (HDP) sürümüne güncelleştirin. (Bu betikler HDInsight, genel yapmaya çalışıyoruz. Genel komut dosyası herhangi bir düğüm kümesi değişiklik olmadan kullanıcı tarafından çalışır.)

Export komutu bir Apache Hadoop dağıtılmış dosya sistemi (HDFS) yol (Azure Blob Depolama veya Azure Data Lake depolama), ayarladığınız bir konumda meta verileri yazar.

### <a name="examples"></a>Örnekler

#### <a name="export-offset-metadata"></a>Uzaklık meta verileri dışarı aktarma
1. ZooKeeper kümeye verilmesi gerekiyor, kontrol noktasını uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirme sonrasında) ZooKeeper uzaklık veri /stormmetadta/zkdata HDFS yoluna dışarı aktarmak için aşağıdaki komutu çalıştırın:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Uzaklık meta verileri içeri aktarma
1. ZooKeeper kümeye, kontrol noktası içeri aktarılacak gereksinimlerini uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirme sonrasında) hedef küme ZooKeeper sunucusuna HDFS yol /stormmetadata/zkdata ZooKeeper uzaklık veri almak için aşağıdaki komutu çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Topolojileri verileri işleme başına veya kullanıcının seçtiği zaman damgası başlayabilmesi uzaklık meta verilerini silme
1. ZooKeeper kümeye silinmesi gerekiyor, kontrol noktasını uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirdikten sonra) geçerli küme içindeki tüm ZooKeeper uzaklık verilerini silmek için aşağıdaki komutu çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Storm ikili dosyaları bir kümeye nasıl bulabilirim?
Storm ikili dosyaları geçerli HDP yığınının usr içinde var. Konumun baş düğümleri için hem de çalışan düğümleri için aynıdır.
 
Birden çok ikili dosyaları belirli HDP sürümleri /usr/hdp (örneğin, /usr/hdp/2.5.0.1233/storm) içinde olabilir. Küme üzerinde çalışan en son sürüme symlinked usr klasördür.

Daha fazla bilgi için [SSH kullanarak HDInsight kümesine bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) ve [Apache Storm](https://storm.apache.org/).
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Bir Storm kümesine dağıtım topolojisi nasıl belirlerim?
İlk olarak, HDInsight Storm ile yüklü tüm bileşenleri tanımlayın. Bir Storm kümesi dört düğüm kategorisi oluşur:

* Ağ geçidi düğümleri
* Baş düğümler
* ZooKeeper düğümleri
* Çalışan düğümleri
 
### <a name="gateway-nodes"></a>Ağ geçidi düğümleri
Bir ağ geçidi ve etkin bir Ambari yönetim hizmeti genel erişim sağlayan ters proxy hizmeti, ağ geçidi düğüm. Ayrıca, Ambari öncü seçimi işler.
 
### <a name="head-nodes"></a>Baş düğümler
Storm baş düğümü, aşağıdaki hizmetleri çalıştırın:
* Nimbus
* Ambari sunucusunun
* Ambari ölçümleri sunucusu
* Ambari ölçümleri Toplayıcı
 
### <a name="zookeeper-nodes"></a>ZooKeeper düğümleri
HDInsight, üç düğümlü ZooKeeper çekirdek ile birlikte gelir. Çekirdek boyutu sabittir ve yapılandırılamaz.
 
Storm kümesi Hizmetleri'nde ZooKeeper çekirdeği otomatik olarak kullanmak üzere yapılandırılır.
 
### <a name="worker-nodes"></a>Çalışan düğümleri
Storm çalışan düğümleri aşağıdaki hizmetleri çalıştırın:
* Gözetmen
* Çalışan Java topolojileri çalıştırmak için sanal (JVMs)
* Ambari aracı
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Geliştirme için Storm olay hub'ı spout ikili dosyaları nasıl bulabilirim?
 
Storm olay hub'ı spout .jar dosyalarını topolojinizi ile kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.
 
### <a name="java-based-topology"></a>Java tabanlı topoloji
[(Java) HDInsight üzerinde Apache Storm ile Azure Event hubs'dan olayları işleme](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-topoloji (HDInsight 3.4 + Linux Storm kümeleri üzerinde Mono) tabanlı
[HDInsight üzerinde Apache Storm ile Azure Event hubs'tan olay işleme (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-apache-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Apache Storm olay hub'ın en son spout ikili dosyaları HDInsight 3.5 + Linux Storm kümeleri
Mvn depo HDInsight 3.5 + Linux Storm kümeleri ile çalıştığı son Storm olay hub'ı spout kullanmayı öğrenmek için bkz [Benioku dosyası](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Kaynak kod örnekleri
Bkz: [örnekler](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) okuma ve yazma (Java dilinde yazılmış) bir Apache Storm topolojisi kullanarak bir Azure HDInsight kümesinde Azure olay Hub'ından nasıl.
 
## <a name="how-do-i-locate-storm-log4j-2-configuration-files-on-clusters"></a>Storm Log4J 2 yapılandırma dosyalarını kümeleri nasıl bulabilirim?
 
Tanımlamak için [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) Storm hizmetler için yapılandırma dosyaları.
 
### <a name="on-head-nodes"></a>Baş düğümler üzerinde
Nimbus Log4J yapılandırma okunur/usr/hdp/\<HDP sürüm\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>Çalışan düğümlerine
Gözetmen Log4J yapılandırma okunur/usr/hdp/\<HDP sürüm\>/storm/log4j2/cluster.xml.
 
Çalışan Log4J yapılandırma dosyasını/usr/hdp/okunur\<HDP sürüm\>/storm/log4j2/worker.xml.
 
Örnekler: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

### <a name="see-also"></a>Ayrıca Bkz.
[Azure HDInsight'ı kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)
