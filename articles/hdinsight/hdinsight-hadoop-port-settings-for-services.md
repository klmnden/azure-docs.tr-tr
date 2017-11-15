---
title: "Hdınsight'ta - Azure Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları | Microsoft Docs"
description: "Hdınsight üzerinde çalışan Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları listesi."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd14aed9-ec25-4bb3-a20c-e29562735a7d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/13/2017
ms.author: larryfr
ms.openlocfilehash: a55180b5d65b268d7c9b51307581a5fe777a26fe
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>Hdınsight'ta Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları

Bu belge Linux tabanlı Hdınsight kümelerinde çalışan Hadoop Hizmetleri tarafından kullanılan bağlantı noktalarının bir listesini sağlar. Ayrıca SSH kullanarak kümeye bağlanmak için kullanılan bağlantı noktaları hakkında bilgi sağlar.

## <a name="public-ports-vs-non-public-ports"></a>Genel olmayan bağlantı noktaları ve ortak bağlantı noktaları

Linux tabanlı Hdınsight kümeleri yalnızca internet'te genel olarak üç bağlantı noktalarını kullanıma; 22, 23 ve 443'tür. Bu bağlantı noktaları, güvenli bir şekilde SSH ve güvenli bir HTTPS protokolü üzerinden kullanıma sunulan Hizmetleri'ni kullanarak kümeye erişmek için kullanılır.

Hdınsight tarafından birden fazla Azure sanal makineleri (küme içindeki düğümler) dahili olarak uygulanan bir Azure sanal ağ üzerinde çalışıyor. Sanal ağ içindeki bağlantı noktalarını Internet üzerinden gösterilmeyen erişebilir. SSH kullanarak baş düğümlerden birinde bağlanıyorsanız, örneğin, baş düğümünden sonra doğrudan küme düğümleri üzerinde çalışan hizmetleri erişebilirsiniz.

> [!IMPORTANT]
> Hdınsight için bir yapılandırma seçeneği olarak bir Azure sanal ağı belirtmezseniz otomatik olarak oluşturulur. Ancak, bu sanal ağa (örneğin, diğer Azure sanal makineler veya istemci geliştirme makinenizdeki) diğer makinelere katılmak olamaz.


Ek makineler sanal ağa katılmak için ilk sanal ağ oluşturun ve ardından, Hdınsight kümesi oluştururken belirtmeniz gerekir. Daha fazla bilgi için bkz: [bir Azure sanal ağı kullanarak Hdınsight genişletme özellikleri](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Ortak bağlantı noktaları

Bir Hdınsight kümesindeki tüm düğümlere bir Azure sanal ağında bulunan ve doğrudan internet'ten erişilemez. Ortak ağ geçidi tüm Hdınsight küme türleri arasında ortak olan aşağıdaki bağlantı noktaları, Internet erişim sağlar.

| Hizmet | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| sshd |22 |SSH |İstemciler üzerinde birincil headnode sshd bağlanır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |İstemciler sınır düğümde sshd bağlanır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |İstemciler ikincil headnode üzerinde sshd bağlanır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Ambari web kullanıcı Arabirimi. Bkz: [Ambari Web kullanıcı arabirimini kullanarak Hdınsight yönetme](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API. Bkz: [Ambari REST API kullanarak Hdınsight'ta yönetme](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API. Bkz: [Curl ile Hive kullanma](hadoop/apache-hadoop-use-pig-curl.md), [Curl ile Pig kullanma](hadoop/apache-hadoop-use-pig-curl.md), [Curl ile MapReduce kullanma](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |ODBC kullanarak Hive bağlanır. Bkz: [Hdınsight için Microsoft ODBC sürücüsü ile bağlanma Excel'e](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |JDBC kullanarak Hive bağlanır. Bkz: [Hive JDBC sürücüsü kullanarak hdınsight'ta Hive Bağlan](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) |

Aşağıdakiler için belirli küme türleri kullanılabilir:

| Hizmet | Bağlantı Noktası | Protokol | Küme türü | Açıklama |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST API. Bkz: [HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST API. Bkz: [uzaktan Livy kullanarak Spark gönderme işleri](spark/apache-spark-livy-rest-interface.md) |
| Storm |443 |HTTPS |Storm |Storm web kullanıcı Arabirimi. Bkz: [dağıtma ve Hdınsight üzerinde Storm topolojilerini yönetme](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Kimlik doğrulama

Genel olarak internet'te kullanıma sunulan tüm hizmetlerin kimliğinin doğrulanması gerekir:

| Bağlantı Noktası | Kimlik Bilgileri |
| --- | --- |
| 22 veya 23 |Küme oluşturma sırasında belirtilen SSH kullanıcı kimlik bilgileri |
| 443 |Oturum açma adı (varsayılan: Yönetici) ve küme oluşturma sırasında ayarlanan parola |

## <a name="non-public-ports"></a>Ortak olmayan bağlantı noktaları

> [!NOTE]
> Bazı hizmetler, yalnızca belirli küme türlerinde kullanılabilir. Örneğin, HBase yalnızca HBase kümesi türlerinde kullanılabilir.

> [!IMPORTANT]
> Bazı hizmetler, aynı anda yalnızca bir headnode üzerinde çalıştırın. Birincil headnode hizmetine bağlanmak ve 404 hatası alırsınız çalışırsanız, ikincil headnode kullanarak yeniden deneyin.

### <a name="ambari"></a>Ambari

| Hizmet | Düğümler | Bağlantı Noktası | URL yolu | Protokol | 
| --- | --- | --- | --- | --- |
| Ambari web kullanıcı Arabirimi | Baş düğümler | 8080 | / | HTTP |
| Ambari REST API | Baş düğümler | 8080 | / api/v1 | HTTP |

Örnekler:

* Ambari REST API:`curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| İş web kullanıcı Arabirimi |Baş düğümler |30070 |HTTPS |Web durumunu görüntülemek için kullanıcı Arabirimi |
| İş meta veri hizmeti |Baş düğümler |8020 |IPC |Dosya sistemi meta verileri |
| DataNode |Tüm çalışan düğümleri |30075 |HTTPS |Görünüm durumu, günlükleri, vb. için Web kullanıcı Arabirimi. |
| DataNode |Tüm çalışan düğümleri |30010 |&nbsp; |Veri aktarımı |
| DataNode |Tüm çalışan düğümleri |30020 |IPC |Meta veri işlemleri |
| İkincil iş |Baş düğümler |50090 |HTTP |İş meta veriler için denetim noktası |

### <a name="yarn-ports"></a>YARN bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Resource Manager web kullanıcı Arabirimi |Baş düğümler |8088 |HTTP |Web kullanıcı Arabirimi için kaynak yöneticisi |
| Resource Manager web kullanıcı Arabirimi |Baş düğümler |8090 |HTTPS |Web kullanıcı Arabirimi için kaynak yöneticisi |
| Kaynak Yöneticisi'ni yönetim arabirimi |Baş düğümler |8141 |IPC |Uygulama gönderimleri için (Hive, Pig, Hive sunucu vs.) |
| Kaynak Manager Zamanlayıcısı |Baş düğümler |8030 |HTTP |Yönetim Arabirimi |
| Kaynak Yöneticisi uygulama arabirimi |Baş düğümler |8050 |HTTP |Uygulama Yöneticisi arabirimi adresidir |
| NodeManager |Tüm çalışan düğümleri |30050 |&nbsp; |Kapsayıcı Yöneticisi adresi |
| NodeManager web kullanıcı Arabirimi |Tüm çalışan düğümleri |30060 |HTTP |Kaynak Yöneticisi arabirimi |
| Zaman Çizelgesi adresi |Baş düğümler |10200 |RPC |Zaman Çizelgesi hizmeti RPC hizmeti. |
| Zaman Çizelgesi web kullanıcı Arabirimi |Baş düğümler |8181 |HTTP |Zaman Çizelgesi hizmet web kullanıcı Arabirimi |

### <a name="hive-ports"></a>Hive bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| HiveServer2 |Baş düğümler |10001 |Thrift |Hizmet yığını (Thrift/JDBC) bağlamak için |
| Hive meta depo |Baş düğümler |9083 |Thrift |Kovan meta verileri (Thrift/JDBC) bağlanmak için hizmeti |

### <a name="webhcat-ports"></a>WebHCat bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| WebHCat sunucu |Baş düğümler |30111 |HTTP |Web API üstünde HCatalog ve diğer Hadoop Hizmetleri |

### <a name="mapreduce-ports"></a>MapReduce bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Kaynak |Baş düğümler |19888 |HTTP |MapReduce kaynak web kullanıcı Arabirimi |
| Kaynak |Baş düğümler |10020 |&nbsp; |MapReduce kaynak sunucusu |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Reducers isteyen için aktarımları Ara harita çıkarır |

### <a name="oozie"></a>Oozie

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Oozie sunucu |Baş düğümler |11000 |HTTP |Oozie hizmeti URL'si |
| Oozie sunucu |Baş düğümler |11001 |HTTP |Oozie Yönetim için bağlantı noktası |

### <a name="ambari-metrics"></a>Ambari ölçümleri

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Zaman Çizelgesi (uygulama geçmişi) |Baş düğümler |6188 |HTTP |Zaman Çizelgesi hizmet web kullanıcı Arabirimi |
| Zaman Çizelgesi (uygulama geçmişi) |Baş düğümler |30200 |RPC |Zaman Çizelgesi hizmet web kullanıcı Arabirimi |

### <a name="hbase-ports"></a>HBase bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| HMaster |Baş düğümler |16000 |&nbsp; |&nbsp; |
| HMaster bilgisi Web kullanıcı Arabirimi |Baş düğümler |16010 |HTTP |HBase ana web kullanıcı Arabirimi için bağlantı noktası |
| Bölge sunucu |Tüm çalışan düğümleri |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |İstemciler için ZooKeeper bağlanmak için kullandığınız bağlantı noktası |

### <a name="kafka-ports"></a>Kafka bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Aracısı |Çalışan düğümü |9092 |[Kafka kablo protokolü](http://kafka.apache.org/protocol.html) |İstemci iletişimi için kullanılan |
| &nbsp; |Zookeeper düğümleri |2181 |&nbsp; |İstemciler için Zookeeper bağlanmak için kullandığınız bağlantı noktası |

### <a name="spark-ports"></a>Spark bağlantı noktaları

| Hizmet | Düğümler | Bağlantı Noktası | Protokol | URL yolu | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift sunucuları |Baş düğümler |10002 |Thrift | &nbsp; | Spark SQL (Thrift/JDBC) bağlanmak için hizmeti |
| Livy sunucu | Baş düğümler | 8998 | HTTP | &nbsp; | Deyimler, işleri ve uygulamaları çalıştırmak için hizmeti |

Örnekler:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. Bu örnekte, `10.0.0.11` Livy hizmeti barındıran headnode IP adresidir.