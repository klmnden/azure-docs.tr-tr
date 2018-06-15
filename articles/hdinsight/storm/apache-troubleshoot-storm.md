---
title: Storm Azure Hdınsight kullanarak sorun giderme | Microsoft Docs
description: Apache Storm Azure Hdınsight ile kullanma hakkında sık sorulan soruların yanıtlarını alın.
keywords: Sorun giderme kılavuzu, ortak sorunları azure Hdınsight, Storm, SSS
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: ''
editor: ''
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: raviperi
ms.openlocfilehash: 46f07a1512435fd8ad5cae4df1858f948fe017e1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31409883"
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a>Storm Azure Hdınsight kullanarak sorun giderme

Üst sorunları ve bunların çözümleri için Apache Ambari, Apache Storm yükü ile çalışma hakkında bilgi edinin.

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Bir küme üzerindeki Storm kullanıcı arabirimini nasıl erişirim?
Storm kullanıcı Arabirimi bir tarayıcıdan erişirken için iki seçeneğiniz vardır:

### <a name="ambari-ui"></a>Ambari kullanıcı Arabirimi
1. Ambari panoya gidin.
2. Hizmetler listesinde seçin **Storm**.
3. İçinde **hızlı bağlantılar** menüsünde, select **Storm kullanıcı Arabirimi**.

### <a name="direct-link"></a>Doğrudan bağlantı
Storm kullanıcı Arabirimi şu URL'de erişebilirsiniz:

https://\<küme DNS adına\>/stormui

Örnek:

 https://stormcluster.azurehdinsight.net/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Nasıl ı Storm olay hub'ı spout denetim noktası bilgilerini bir topoloji başka bir bilgisayara aktarmak?

Azure olay hub'larından okumayı topolojileri geliştirirken .jar dosyasına spout Hdınsight Storm olay hub'ı kullanarak, yeni kümede aynı ada sahip bir topoloji dağıtmanız gerekir. Ancak, Apache ZooKeeper eski kümede kaydedilen denetim noktası verileri korumanız gerekir.

### <a name="where-checkpoint-data-is-stored"></a>Denetim noktası verileri depolandığı
Denetim noktası verileri uzaklıkları için olay hub'ı spout ZooKeeper içinde iki kök yollarda tarafından depolanır:
- İşlem dışı spout kontrol noktaları /eventhubspout içinde depolanır.
- İşlem spout denetim noktası verileri / işlem depolanır.

### <a name="how-to-restore"></a>Geri yükleme
ZooKeeper dışında verilerini dışa aktarın ve yeni bir adla ZooKeeper dön verileri almak için kullandığınız kitaplıkları ve komut dosyalarını almak için bkz: [Hdınsight Storm örnekler](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

LIB klasör içeri/dışarı aktarma işlemi uygulamasını içeren .jar dosyaları içerir. Bash klasör veri eski kümede ZooKeeper sunucusundan dışarı aktarmak ve yeni kümede ZooKeeper sunucuya geri alma nasıl oluşturulduğunu gösteren bir örnek komut dosyası içerir.

Çalıştırma [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) komut dosyasından veri içeri ve dışarı aktarmak ZooKeeper düğümleri. Komut dosyası doğru Hortonworks veri Platformu (HDP) sürümüne güncelleştirin. (Bu komut dosyalarını hdınsight'ta genel yapılması çalışıyoruz. Genel komut dosyası herhangi bir düğümün küme değişiklik olmadan kullanıcı tarafından çalıştırabilirsiniz.)

Dışa Aktar komutunu bir Apache Hadoop dağıtılmış dosya sistemi (HDFS) yol (bir Azure Blob Storage veya Azure Data Lake Store deposunda) ayarladığınız bir konumda meta verileri yazar.

### <a name="examples"></a>Örnekler

#### <a name="export-offset-metadata"></a>Uzaklık meta verileri dışarı aktarma
1. ZooKeeper kümeye içinden denetim noktası verilmesi gerekiyor uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirdikten sonra) ZooKeeper uzaklık verisi /stormmetadta/zkdata HDFS yoluna dışarı aktarmak için aşağıdaki komutu çalıştırın:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Uzaklık meta verileri içeri aktarma
1. ZooKeeper kümeye içinden kontrol noktası içeri aktarılacak gereksinimlerini uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirdikten sonra) hedef kümede ZooKeeper sunucuya HDFS yol /stormmetadata/zkdata ZooKeeper uzaklık veri almak için aşağıdaki komutu çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Topolojileri veriler işlenirken başından veya kullanıcının seçtiği zaman damgası başlayabilmeniz için uzaklık meta verilerini silme
1. ZooKeeper kümeye içinden kontrol noktası silinecek gereksinimlerini uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirdikten sonra) geçerli küme içindeki tüm ZooKeeper uzaklık verilerini silmek için aşağıdaki komutu çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Bir kümede nasıl Storm ikili dosyaları bulun?
Geçerli HDP yığını için Storm ikili dosyalarını /usr/hdp/current/storm-client içinde ' dir. Konumun baş düğümler ve çalışan düğümleri için aynıdır.
 
Birden çok ikili dosyaları /usr/hdp (örneğin, /usr/hdp/2.5.0.1233/storm) belirli HDP sürümlerde için olabilir. Küme üzerinde çalışan en son sürüme symlinked /usr/hdp/current/storm-client klasörüdür.

Daha fazla bilgi için bkz: [SSH kullanarak Hdınsight kümesine bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) ve [Storm](http://storm.apache.org/).
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Bir Storm kümesine dağıtım topolojisi nasıl belirlerim?
İlk olarak, Hdınsight Storm ile yüklenen tüm bileşenler tanımlayın. Storm kümesi dört düğüm kategorilerini oluşur:

* Ağ geçidi düğümleri
* Baş düğümler
* ZooKeeper düğümleri
* Çalışan düğümü
 
### <a name="gateway-nodes"></a>Ağ geçidi düğümleri
Bir ağ geçidi düğümü, bir ağ geçidi ve bir active Ambari yönetim hizmeti için genel erişim sağlayan ters proxy hizmeti olduğu. Ayrıca, Ambari öncü seçim işler.
 
### <a name="head-nodes"></a>Baş düğümler
Storm baş düğümler aşağıdaki hizmetleri çalıştırın:
* Nimbus
* Ambarı sunucusu
* Ambari ölçümleri sunucu
* Ambari ölçümleri Toplayıcı
 
### <a name="zookeeper-nodes"></a>ZooKeeper düğümleri
Hdınsight üç düğümü ZooKeeper çekirdek ile birlikte gelir. Çekirdek boyutu sabittir ve yapılandırılamayan.
 
Storm hizmetler kümedeki ZooKeeper çekirdek otomatik olarak kullanmak üzere yapılandırılmıştır.
 
### <a name="worker-nodes"></a>Çalışan düğümü
Storm çalışan düğümleri aşağıdaki hizmetleri çalıştırın:
* Gözetmen
* Çalışan Java topolojileri çalıştırmak için makineleri (JVMs)
* Ambari aracı
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Geliştirme için Storm olay hub'ı spout ikili dosyalarını nasıl bulun?
 
Storm olay hub'ı spout .jar dosyaları topolojinizi ile kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.
 
### <a name="java-based-topology"></a>Java tabanlı topolojisi
[HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (Java)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-topoloji (Mono Hdınsight 3.4 + Linux Storm kümeleri üzerinde) temel
[HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Storm event hub'ın en son spout Hdınsight 3.5 + Linux Storm kümeleri için ikili dosyalar
Hdınsight 3.5 + Linux Storm kümeleri ile çalışır son Storm olay hub'ı spout kullanmayı öğrenmek için mvn depodaki bkz [Benioku dosyasını](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Kaynak kodu örnekleri
Bkz: [örnekler](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) okuma ve Azure Event Hub'ın bir Azure Hdınsight kümesinde (Java'da yazılmış) bir Apache Storm topolojisini kullanarak gelen yazma konusunda.
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Storm Log4J yapılandırma dosyalarını kümelerde nasıl bulun?
 
Apache Log4J yapılandırma dosyalarını Storm hizmetleri tanımlamak için.
 
### <a name="on-head-nodes"></a>Baş düğümler üzerinde
Nimbus Log4J yapılandırma okunur/usr/hdp/\<HDP sürüm\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>Alt düğümler üzerinde
Yönetici Log4J yapılandırma okunur/usr/hdp/\<HDP sürüm\>/storm/log4j2/cluster.xml.
 
Çalışan Log4J yapılandırma dosyası okunur/usr/hdp/\<HDP sürüm\>/storm/log4j2/worker.xml.
 
Örnekler: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

### <a name="see-also"></a>Ayrıca Bkz.
[Azure Hdınsight kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)
