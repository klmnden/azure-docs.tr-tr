---
title: Azure Hdınsight kullanarak HBase sorun giderme | Microsoft Docs
description: HBase ve Azure Hdınsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
services: hdinsight
documentationcenter: ''
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: nitinver
ms.openlocfilehash: d5d50121cd0375af1b57baadeb40efb237aaea11
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34165290"
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a>Azure Hdınsight kullanarak HBase sorun giderme

Apache Ambari, Apache HBase yükü ile çalışırken, üst sorunları ve bunların çözümleri hakkında bilgi edinin.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Birden çok atanmamış bölgeleri ile nasıl hbck komutu raporları çalıştırılsın mı?

Çalıştırdığınızda görebilirsiniz genel bir hata iletisi `hbase hbck` komuttur "birden çok atanmamış bölgeler veya bölgeler zincirindeki delik."

HBase ana Arabiriminde tüm bölge sunuculara dengesini bölgeler sayısını görebilirsiniz. Daha sonra çalıştırabilirsiniz `hbase hbck` bölge zincirindeki delik görmek için komutu.

Delik olabilir çevrimdışı bölgeleri nedeni, bu nedenle atamaları önce düzeltin. 

Atanmamış bölgeler normal bir duruma getirmek için aşağıdaki adımları tamamlayın:

1. SSH kullanarak Hdınsight HBase kümesi için oturum açın.
2. ZooKeeper kabuğunda bağlanmak için Çalıştır `hbase zkcli` komutu.
3. Çalıştırma `rmr /hbase/regions-in-transition` komut veya `rmr /hbase-unsecure/regions-in-transition` komutu.
4. Dan çıkmak için `hbase zkcli` Kabuk, kullanın `exit` komutu.
5. Apache Ambari kullanıcı arabirimini açın ve ardından etkin HBase ana hizmetini yeniden başlatın.
6. Çalıştırma `hbase hbck` komutunu yeniden (seçenekleri olmadan). Tüm bölgeler atanmış olmadığından emin olmak için bu komutun çıktısı denetleyin.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Zaman aşımı sorunları nasıl hbck komutlarını bölge atamaları kullanırken düzeltme?

### <a name="issue"></a>Sorun

Kullandığınızda zaman aşımı sorunlarıyla ilgili olası bir nedeni `hbck` komutu, birkaç bölgeler uzun bir süredir "içinde geçiş" durumda olduğundan emin olabilir. Bu bölgeler, HBase ana kullanıcı arabiriminde çevrimdışı olarak görebilirsiniz. Çok sayıda bölgeler geçiş denediğinizden HBase ana zaman aşımı olabilir ve bu bölgeler tekrar çevrimiçi duruma getirmek oluşturulamıyor.

### <a name="resolution-steps"></a>Çözüm adımları

1. SSH kullanarak Hdınsight HBase kümesi için oturum açın.
2. ZooKeeper kabuğunda bağlanmak için Çalıştır `hbase zkcli` komutu.
3. Çalıştırma `rmr /hbase/regions-in-transition` veya `rmr /hbase-unsecure/regions-in-transition` komutu.
4. Çıkmak için `hbase zkcli` Kabuk, kullanın `exit` komutu.
5. Ambari Arabiriminde etkin HBase ana hizmetini yeniden başlatın.
6. Çalıştırma `hbase hbck -fixAssignments` yeniden komutu.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Nasıl ı zorla-küme güvenli modda HDFS devre dışı bırak?

### <a name="issue"></a>Sorun

Yerel Hadoop dağıtılmış dosya sistemi (HDFS), Hdınsight kümesinde güvenli modda takıldı.

### <a name="detailed-description"></a>Ayrıntılı açıklama

Aşağıdaki HDFS komutu çalıştırdığınızda bu hata bir hatadan kaynaklanıyor:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

Komut çalıştırmayı denediğinizde görebileceğiniz hata şöyle görünür:

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a>Olası neden

Hdınsight kümesi aşağı ölçeklendirilmiş bir çok az sayıda düğüm. Aşağıda veya HDFS çoğaltma faktörü yakın düğümleri sayısıdır.

### <a name="resolution-steps"></a>Çözüm adımları 

1. HDFS durumunu Hdınsight kümesinde aşağıdaki komutları çalıştırarak alın:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdiuser@hn0-spark2:~$ hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   Safe mode is ON
   Configured Capacity: 3372381241344 (3.07 TB)
   Present Capacity: 3138625077248 (2.85 TB)
   DFS Remaining: 3102710317056 (2.82 TB)
   DFS Used: 35914760192 (33.45 GB)
   DFS Used%: 1.14%
   Under replicated blocks: 0
   Blocks with corrupt replicas: 0
   Missing blocks: 0
   Missing blocks (with replication factor 1): 0

   -------------------------------------------------
   Live datanodes (8):

   Name: 10.0.0.17:30010 (10.0.0.17)
   Hostname: 10.0.0.17
   Decommission Status : Normal
   Configured Capacity: 421547655168 (392.60 GB)
   DFS Used: 5288128512 (4.92 GB)
   Non DFS Used: 29087272960 (27.09 GB)
   DFS Remaining: 387172253696 (360.58 GB)
   DFS Used%: 1.25%
   DFS Remaining%: 91.85%
   Configured Cache Capacity: 0 (0 B)
   Cache Used: 0 (0 B)
   Cache Remaining: 0 (0 B)
   Cache Used%: 100.00%
   Cache Remaining%: 0.00%
   Xceivers: 2
   Last contact: Wed Apr 05 16:22:00 UTC 2017
   ...

   ```
2. Ayrıca aşağıdaki komutları kullanarak Hdınsight kümesinde HDFS bütünlüğünü kontrol edebilirsiniz:

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting to namenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
   FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
   ....................................................................................................

   ....................................................................................................
   ..................Status: HEALTHY
   Total size:    9330539472 B
   Total dirs:    37
   Total files:   2618
   Total symlinks:                0 (Files currently being written: 2)
   Total blocks (validated):      2535 (avg. block size 3680686 B)
   Minimally replicated blocks:   2535 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       0 (0.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     3.0
   Corrupt blocks:                0
   Missing replicas:              0 (0.0 %)
   Number of data-nodes:          8
   Number of racks:               1
   FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds

   The filesystem under path '/' is HEALTHY
   ```

3. Karar verirseniz vardır, bozuk, eksik veya under-çoğaltılmış blokları veya bu blokları göz ardı edilebilir olduğunu, ad düğümü güvenli mod dışında olması için aşağıdaki komutu çalıştırın:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Nasıl JDBC veya SQLLine bağlantısı düzeltirim Apache Phoenix ile ilgili sorunları?

### <a name="resolution-steps"></a>Çözüm adımları

Phoenix ile bağlanmak için IP adresi etkin ZooKeeper düğümün sağlamanız gerekir. Hangi sqlline.py ZooKeeper hizmete bağlanmaya çalışan çalışır durumda olduğundan emin olun.
1. SSH kullanarak Hdınsight kümesine oturum açın.
2. Aşağıdaki komutu girin:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Ambari Arabiriminden etkin ZooKeeper düğümünün IP adresini elde edebilirsiniz. Git **HBase** > **hızlı bağlantılar** > **ZK\* (etkin)** > **Zookeeper bilgisi**. 

3. Sqlline.py için Phoenix bağlanır ve zaman aşımına yapar, kullanılabilirlik ve Phoenix durumunu doğrulamak için aşağıdaki komutu çalıştırın:

   ```apache
           !tables
           !quit
   ```      
4. Bu komut çalışırsa, sorun yoktur. Kullanıcı tarafından sağlanan IP adresi yanlış olabilir. Ancak, komut uzun bir süre duraklar ve aşağıdaki hata görüntüler, 5. adıma geçin.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Phoenix sistem durumunu tanılamak için baş düğümü (hn0) aşağıdaki komutları çalıştırın. KATALOG tablosu:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   Komutu bir hata aşağıdakine benzer döndürmesi gerekir: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. Ambari Arabiriminde ZooKeeper düğümlerde HMaster hizmetini yeniden başlatmak için aşağıdaki adımları tamamlayın:

    1. İçinde **Özet** HBase, bölümüne gidin **HBase** > **etkin HBase ana**. 
    2. İçinde **bileşenleri** bölümünde, HBase ana hizmetini yeniden başlatın.
    3. Tüm kalan için bu adımları yineleyin **bekleme HBase ana** Hizmetleri. 

HBase ana hizmeti Sabitle ve kurtarma işlemini tamamlamak beş dakika kadar sürebilir. Birkaç dakika sonra sistem sqlline.py komutların onaylamak için yineleyin. Yedekleme KATALOĞU tablodur ve onun sorgulanabilir. 

Zaman sistem. KATALOG tablo normal olarak, Phoenix için bağlantı sorunu otomatik olarak çözülmesi gerekir.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Başlayamaz ana sunucu nedeni nedir?

### <a name="error"></a>Hata 

Atomik yeniden adlandırılırken bir hata oluşur.

### <a name="detailed-description"></a>Ayrıntılı açıklama

Başlatma işlemi sırasında birçok başlatma adımları HMaster tamamlar. Bunlar, verileri veri klasörü karalama (.tmp) klasöründen taşıma içerir. HMaster ayrıca herhangi bir yanıt vermeyen bölge sunucusu olup olmadığını görmek için yazma tamamlanan günlükleri (WALs) klasörünü arar ve benzeri. 

Başlatma sırasında temel bir HMaster mu `list` bu klasörlerde komutu. Herhangi bir zamanda bu klasörlerden herhangi biri beklenmeyen bir dosyada HMaster görürse, bir özel durum oluşturur ve başlamıyor.  

### <a name="probable-cause"></a>Olası neden

Dosya oluşturma zaman çizelgesi belirlemek ve ardından dosyanın oluşturulduğu zaman geçici bir işlem kilitlenme olup olmadığını görmek bölge server günlükleri deneyin. (Bunun yapılması yardımcı olmak için HBase desteğine başvurun.) Bu yardımcı olur; böylece basarsa bu hatayı önlemek ve normal işlem kapatmalar olun bize daha sağlam mekanizmaları sağlar.

### <a name="resolution-steps"></a>Çözüm adımları

Çağrı yığını denetleyin ve hangi klasörün soruna neden belirlemek deneyin (örneğin, WALs veya .tmp klasörünü olabilir). Ardından, sorunu dosyayı bulmak bulut Gezgini'nde veya HDFS komutlarını kullanarak deneyin. Genellikle, bu olan bir \*-renamePending.json dosya. ( \*-RenamePending.json dosyasıdır WASB driver'ın Atomik yeniden adlandırma işlemi uygulamak için kullanılan bir günlük dosyası. Bu uygulamada hataları nedeniyle, bu dosyaları işlem çökmesi ve benzeri sonra bırakılabilir.) Bu dosya Cloud Explorer'da veya HDFS komutlarını kullanarak zorla silme. 

Bazen de olabilir aşağıdakine benzer adlı geçici bir dosya *$$$. $$$* bu konumda. HDFS kullanmak zorunda `ls` bulut Gezgini'nde göremiyorum; bu dosyayı görmek için komutu. Bu dosyayı silmek için HDFS komutunu `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Bu komutları çalıştırdıktan sonra HMaster hemen başlamanız gerekir. 

### <a name="error"></a>Hata

Hiçbir sunucu adresinin *hbase: meta* bölge xxx için.

### <a name="detailed-description"></a>Ayrıntılı açıklama

Linux kümenizde belirten bir ileti görebilirsiniz *hbase: meta* tablo çevrimiçi değil. Çalışan `hbck` , bildirebilir "hbase: meta tablo ReplicaID 0 üzerinde herhangi bir bölgeyi bulunamadı." HBase yeniden sonra HMaster başlatılamadı sorun olabilir. HMaster günlüklerde iletiyi görebilirsiniz: "hiçbir sunucu adresinin hbase: bölge hbase için meta: Yedekleme \<bölge adı\>".  

### <a name="resolution-steps"></a>Çözüm adımları

1. HBase Kabuğu'nda (geçerli değişiklik gerçek değerler) aşağıdaki komutları girin:  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Silme *hbase: ad alanı* girişi. Bu girişi yapılıyor aynı hata olabilir ne zaman bildirilen *hbase: ad alanı* tablo taraması.

3. HBase Ambari kullanıcı arabiriminde, çalışır durumda ortaya çıkarmak için etkin HMaster hizmetini yeniden başlatın.  

4. HBase Kabuğu'nda tüm çevrimdışı tabloları getirmek için aşağıdaki komutu çalıştırın:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Ek kaynaklar

[HBase tablosuyla işlenemiyor](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Hata

Benzer şekilde önemli özel durum ile HMaster zaman aşımına uğradı "java.io.IOException: süresi sona erdi 300000ms atanacak ad alanı tablosu için bekliyor."

### <a name="detailed-description"></a>Ayrıntılı açıklama

Birçok tablolar ve HMaster hizmetlerinizi yeniden başlattığınızda atılmış olan olmayan bölgeleri varsa bu sorunla karşılaşabilirsiniz. Yeniden başlatma başarısız olabilir ve yukarıdaki hata iletisini görürsünüz.  

### <a name="probable-cause"></a>Olası neden

Bu, HMaster hizmetiyle bilinen bir sorundur. Genel küme başlangıç görevleri uzun sürebilir. Ad alanı tablo henüz atanmadığından HMaster kapanır. Bu yalnızca büyük olduğu senaryolarda oluşuyor unflushed veri miktarını var ve beş dakikalık bir zaman aşımı yeterli değil.
  
### <a name="resolution-steps"></a>Çözüm adımları

1. Ambari Arabiriminde Git **HBase** > **yapılandırmalar**. Özel hbase-site.xml dosyasında aşağıdaki ayarı ekleyin: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Gerekli hizmetler (HMaster ve muhtemelen diğer HBase Hizmetleri) yeniden başlatın.  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Bir yeniden başlatma hatası bir bölge sunucuda nedeni nedir?

### <a name="issue"></a>Sorun

Bir yeniden başlatma hatası bir bölge sunucuda aşağıdaki en iyi yöntemleri tarafından engellenebilir. HBase bölge sunucuları yeniden başlatmak planlama yaparken ağır iş yükü etkinlik duraklatmak öneririz. Kapatma işlemi sürdüğünden, bölge sunucularıyla bağlanmak bir uygulama devam ederse, bölge sunucu yeniden başlatma işlemi birkaç dakika daha yavaş olacaktır. Ayrıca, önce tüm tablolarını temizlemek için bir fikirdir. Tabloları temizlemek nasıl bir başvuru için bkz: [Hdınsight HBase: HBase kümesi yeniden başlatma zamanını tabloları temizlenerek geliştirmeye nasıl](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

HBase bölge sunucularından Ambari kullanıcı Arabirimi yeniden başlatma işlemi'ı başlattığınızda, bölge sunucuları kapandı, ancak hemen yeniden başlatma hemen bakın. 

Arka planda neler olduğunu aşağıda verilmiştir: 

1. Ambari aracı bölge sunucuya bir durdurma isteği gönderir.
2. Ambari Aracısı 30 düzgün biçimde kapatılamadı saniye için bölge sunucunun bekler. 
3. Uygulamanız ile bölge sunucusuna bağlanmak devam ederse, sunucuyu hemen kapatmak olmaz. Kapatma oluşmadan önce 30 saniyelik zaman aşımı süresi dolar. 
4. 30 saniye sonra ambarı aracı zorla KILL gönderir (`kill -9`) bölge sunucuya komutu. Ambari Aracısı günlüğünde (ilgili çalışan düğümüne /var/günlük/dizin) görebilirsiniz:

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
Bölge sunucu işlemi durdurulmuş olsa bile ani kapatma nedeniyle, işlemle ilişkili bağlantı noktası, yayımlanmamış. Bölge sunucusu başlatıldığında, bu durum aşağıdaki günlüklere gösterildiği gibi bir AddressBindException yol açabilir. Bu, burada başlatmak için bölge sunucu başarısız çalışan düğümleri /var/log/hbase dizininde bölge server.log içinde doğrulayabilirsiniz. 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a>Çözüm adımları

1. Yeniden başlatmadan önce HBase bölge sunucuları üzerindeki yükü azaltmak deneyin. 
2. Alternatif olarak (1. Adım Yardım değil ise), bölge sunucuları çalışan düğümleri üzerinde aşağıdaki komutları kullanarak el ile yeniden başlatmayı deneyin:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

### <a name="see-also"></a>Ayrıca Bkz.
[Azure Hdınsight kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)
