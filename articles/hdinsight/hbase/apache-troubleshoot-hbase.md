---
title: Azure HDInsight'ı kullanarak HBase sorunlarını giderme
description: Azure HDInsight ile HBase ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 37a8882653ffede121d2e2cd3f3357741d8d641a
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361940"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Apache HBase sorunlarını giderme

Apache Ambari yüklerde Apache HBase ile çalışırken sık karşılaşılan sorunlar ve çözümleri hakkında bilgi edinin.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Atanmamış birden fazla bölgeye nasıl hbck komut raporları çalıştırabilir?

Çalıştırdığınızda görebileceğiniz genel bir hata iletisi `hbase hbck` komuttur "birden çok atanmamış bölge veya bölgeler zincirindeki boşluklarını."

HBase Master kullanıcı Arabirimi içinde tüm bölge sunucuları arasında dengesiz bölge sayısı görebilirsiniz. Daha sonra çalıştırabileceğiniz `hbase hbck` bölge zincirindeki boşluklarını görmek için komutu.

Açıkları olabilir çevrimdışı bölgeleri nedeni, bu nedenle atamaları önce düzeltin. 

Atanmamış bölgeleri normal bir duruma getirmek için aşağıdaki adımları tamamlayın:

1. SSH kullanarak HDInsight HBase kümesi için oturum açın.
2. Apache ZooKeeper Kabuk ile bağlanmak için çalıştırın `hbase zkcli` komutu.
3. Çalıştırma `rmr /hbase/regions-in-transition` komutu veya `rmr /hbase-unsecure/regions-in-transition` komutu.
4. Alanından çıkmak için `hbase zkcli` kullanın, Kabuk `exit` komutu.
5. Apache Ambari UI'ı açın ve ardından etkin HBase Master hizmetini yeniden başlatın.
6. Çalıştırma `hbase hbck` komutunu tekrar (hiçbir seçenek olmadan). Tüm bölgeler atanan emin olmak için bu komutun çıktısı denetleyin.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Zaman aşımı sorunlarını nasıl hbck komutları için bölge atamaları kullanırken düzeltebilirim?

### <a name="issue"></a>Sorun

Kullandığınız zaman zaman aşımı sorunlarıyla ilgili olası bir neden `hbck` komutu, çeşitli bölgeleri uzun bir süredir "geçiş içinde" durumda olmadığından emin olabilir. Bu bölgeler, HBase Master kullanıcı Arabirimi çevrimdışı olarak görebilirsiniz. Çok sayıda bölgede geçiş denediğinizden HBase Master zaman aşımı olabilir ve bu bölgelerde tekrar çevrimiçi duruma alınamıyor.

### <a name="resolution-steps"></a>Çözüm adımları

1. SSH kullanarak HDInsight HBase kümesi için oturum açın.
2. Apache ZooKeeper Kabuk ile bağlanmak için çalıştırın `hbase zkcli` komutu.
3. Çalıştırma `rmr /hbase/regions-in-transition` veya `rmr /hbase-unsecure/regions-in-transition` komutu.
4. Çıkmak için `hbase zkcli` kullanın, Kabuk `exit` komutu.
5. Ambari UI içinde etkin HBase Master hizmetini yeniden başlatın.
6. Çalıştırma `hbase hbck -fixAssignments` yeniden komutu.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Nasıl miyim zorla-HDFS güvenli bir küme modunda devre dışı?

### <a name="issue"></a>Sorun

Yerel Apache Hadoop dağıtılmış dosya sistemi (HDFS), HDInsight kümesinde güvenli modda takıldı.

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Aşağıdaki HDFS komutu çalıştırdığınızda bu hata, bir hatadan kaynaklanıyor:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

Komutu çalıştırmayı denediğinizde görebileceğiniz hata şöyle görünür:

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

HDInsight küme aşağı ölçeklendirilebilir bir çok az sayıda düğüm. Aşağıda veya HDFS çoğaltma faktörü yakın düğümler sayısıdır.

### <a name="resolution-steps"></a>Çözüm adımları 

1. HDFS durumunu, HDInsight kümesinde aşağıdaki komutları çalıştırarak alın:

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
2. Ayrıca, aşağıdaki komutları kullanarak HDInsight kümesinde HDFS bütünlüğünü denetleyebilirsiniz:

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

3. Belirlerseniz vardır, bozuk, eksik veya under-çoğaltılmış blokları veya söz konusu bloklar göz ardı edilebilir olduğunu, ad düğümü güvenli mod dışında olması için aşağıdaki komutu çalıştırın:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Nasıl JDBC veya SQLLine bağlantı düzeltirim Apache Phoenix ile ilgili sorunlar?

### <a name="resolution-steps"></a>Çözüm adımları

Apache Phoenix ile bağlantı için etkin bir Apache ZooKeeper düğümü IP adresini sağlamanız gerekir. Hangi sqlline.PY üzerinden ZooKeeper hizmete bağlanmaya çalışan ve çalışıyor olduğundan emin olun.
1. HDInsight kümesine SSH kullanarak oturum açın.
2. Aşağıdaki komutu girin:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Ambari Arabiriminden, etkin ZooKeeper düğümü IP adresini alabilirsiniz. Git **HBase** > **hızlı bağlantılar** > **ZK\* (etkin)** > **Zookeeper bilgisi**. 

3. Sqlline.PY üzerinden Phoenix ile bağlanır ve zaman aşımına oluşturmazsa, Phoenix durumunu ve kullanılabilirliğini doğrulamak için aşağıdaki komutu çalıştırın:

   ```apache
           !tables
           !quit
   ```      
4. Bu komut işe yararsa, hiçbir sorun yoktur. Kullanıcı tarafından sağlanan IP adresi yanlış olabilir. Bununla birlikte, komut uzun bir süre boyunca duraklayacak ve şu hatayı görüntüler, 5. adıma geçin.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Phoenix sistem durumunun tanılamak için baş düğümünden (hn0) aşağıdaki komutları çalıştırın. KATALOG tablosu:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   Komut aşağıdakine benzer bir hata döndürmelidir: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. Apache Ambari UI içinde tüm ZooKeeper düğümleri HMaster hizmetini yeniden başlatmak için aşağıdaki adımları tamamlayın:

    1. İçinde **özeti** HBase bölümüne gidin **HBase** > **etkin HBase Master**. 
    2. İçinde **bileşenleri** bölümünde, HBase Master hizmetini yeniden başlatın.
    3. Tüm kalan için bu adımları tekrarlayarak **bekleme HBase Master** Hizmetleri. 

Bu, HBase Master hizmeti Sabitle ve kurtarma işlemini tamamlamak beş dakikaya kadar sürebilir. Birkaç dakika sonra sistem sqlline.PY üzerinden komutları onaylamak için yineleyin. Yedekleme KATALOĞU tablodur ve BT'nin sorgulanabilir. 

Zaman sistem. KATALOG tablo normal olarak, Phoenix bağlantısı sorunu otomatik olarak çözülmesi gerekir.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Ana sunucu başlayamaz nedeni nedir?

### <a name="error"></a>Hata 

Atomik bir yeniden adlandırma hatası oluşur.

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Başlatma işlemi sırasında birçok başlatma adımları HMaster tamamlar. Bunlar, verileri veri klasörü karalama (.tmp) klasöründen taşıma içerir. HMaster da herhangi bir yanıt vermeyen bölge sunucusu olup olmadığını görmek için tamamlanan yazma günlükleri (WALs) klasörde bakar ve benzeri. 

Başlatma sırasında temel bir HMaster mu `list` bu klasörlerdeki komutu. Herhangi bir zamanda bu klasörlerden birine beklenmeyen bir dosya HMaster görürse, bir özel durum oluşturur ve başlamaz.  

### <a name="probable-cause"></a>Olası neden

Dosya oluşturma zaman çizelgesini belirleyin ve ardından dosyanın oluşturulduğu zaman geçici bir işlem kilitlenmesi olup olmadığını görmek bölge sunucusu günlükleri'ni deneyin. (Bunun yapılması yardımcı olmak için HBase desteğine başvurun.) Böylece bu hatayı ulaşmaktan kaçınmak ve normal işlem kapatmalar sağlamak daha güçlü mekanizmalar sağlayın. Bu yardımcı olur.

### <a name="resolution-steps"></a>Çözüm adımları

Çağrı yığınını kontrol edin ve klasörü soruna neden belirlemeye çalışın (örneğin, WALs veya .tmp klasörü olabilir). Ardından, sorunu dosyayı bulmak Cloud Explorer içinde veya HDFS komutlarını kullanarak deneyin. Genellikle bu, bir \*-renamePending.json dosya. ( \*-RenamePending.json dosyasıdır WASB sürücü Atomik yeniden adlandırma işlemi uygulamak için kullanılan bir günlük dosyası. Bu uygulamada hataları nedeniyle, bu dosyalar işlem çökmesi ve benzeri sonra bırakılabilir.) Cloud Explorer veya HDFS komutlarını kullanarak bu dosyayı zorla silme. 

Bazı durumlarda da olabilir gibi adlı geçici bir dosya *$$$. $$$* bu konumda. HDFS kullanmak zorunda `ls` bu dosyayı görmek için komut; bulut Gezgini'nde göremiyorum. Bu dosyayı silmek için HDFS komutunu `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Bu komutları çalıştırdıktan sonra HMaster hemen başlamanız gerekir. 

### <a name="error"></a>Hata

Hiç sunucu adresi listelenir *hbase: meta* bölge xxx için.

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Linux kümenizde bildiren bir ileti görebilirsiniz *hbase: meta* tabloda çevrimiçi değil. Çalışan `hbck` rapor edebilir "hbase: meta tablo ReplicaID 0 üzerinde herhangi bir bölge bulunamadı." HBase yeniden sonra HMaster başlatılamadı sorun olabilir. HMaster günlükleri aşağıdaki iletiyi görebilirsiniz: "Hbase'de hiç sunucu adresi listelenen: bölge hbase için meta: Yedekleme \<bölge adı\>".  

### <a name="resolution-steps"></a>Çözüm adımları

1. HBase Kabuğu'nda (değişiklik gerçek değerler uygunsa) aşağıdaki komutları girin:  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Silme *hbase: ad alanı* girişi. Bu giriş, yüklenmekte olan aynı hata olabilir raporlandığını *hbase: ad alanı* tablo taranır.

3. HBase çalışır durumda, Ambari UI içinde ortaya çıkarmak için etkin HMaster hizmetini yeniden başlatın.  

4. HBase Kabuğu'nda tüm tabloları çevrimdışı duruma getirmek için aşağıdaki komutu çalıştırın:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Ek okuma

[HBase tablosuyla işlenemiyor](https://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Hata

Benzer şekilde önemli özel durum ile HMaster zaman aşımına "java.io.IOException: Zaman aşımına uğradı ad alanı tablosu için bekleyen atanacak 300000ms."

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Birçok tabloları ve HMaster hizmetlerinizi yeniden başlattığınızda temizlenmiş olmayan bölgeleri varsa bu sorunla karşılaşabilirsiniz. Yeniden başlatma başarısız olabilir ve önceki bir hata iletisi görürsünüz.  

### <a name="probable-cause"></a>Olası neden

Bu, HMaster hizmeti ile bilinen bir sorundur. Küme genel başlangıç görevleri, uzun bir zaman alabilir. Ad alanı tablo henüz atanmadığından HMaster kapanır. Bu yalnızca, büyük senaryolarda oluşur unflushed veri miktarı var ve beş dakikalık bir zaman aşımı yeterli değildir.
  
### <a name="resolution-steps"></a>Çözüm adımları

1. Apache Ambari UI'nızda Git **HBase** > **yapılandırmaları**. Özel hbase-site.xml dosyasında aşağıdaki ayarı ekleyin: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Gerekli hizmetler (HMaster ve büyük olasılıkla diğer HBase Hizmetleri) yeniden başlatın.  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Bir yeniden başlatma hatası bir bölge sunucusu üzerinde nedeni nedir?

### <a name="issue"></a>Sorun

Bir bölge sunucusu üzerinde bir yeniden başlatma hatası aşağıdaki en iyi yöntemleri tarafından engellenebilir. HBase bölge sunucuları yeniden başlatmak planlama yaparken ağır iş yükü etkinlik duraklatma öneririz. Shutdown devam ederken bölge sunucuları ile bağlanmak bir uygulama devam ederse, bölge sunucusu yeniden başlatma işlemi birkaç dakika daha yavaş olur. Ayrıca, önce tüm tabloları Temizle iyi bir fikirdir. Tablolarını temizlemek nasıl bir başvuru için bkz: [HDInsight HBase: Apache HBase kümesi yeniden başlatma zamanı tabloları temizlenerek geliştirmek nasıl](https://web.archive.org/web/20190112153155/ https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

HBase bölge Apache Ambari UI sunuculardan üzerinde yeniden başlatma işlemi'ı başlattığınızda, bölge sunucuları kapanmış, ancak hemen yeniden başlatma anında görün. 

Arka planda neler olduğunu aşağıda verilmiştir: 

1. Ambari aracı için bölge sunucusu bir durdurma isteği gönderir.
2. Ambari aracı 30 düzgün biçimde kapatılamadı saniye için bölge sunucusu için bekler. 
3. Bölge sunucusu ile bağlanmak uygulamanızı devam ederse, sunucuyu hemen kapatıldı olmaz. Kapatma gerçekleşmeden önce 30 saniyelik zaman aşımı süresi dolar. 
4. 30 saniye sonra Ambari zorla KILL gönderir (`kill -9`) bölge sunucusu komutu. Ambari Aracısı günlüğünde (ilgili çalışan düğümü /var/log/dizin) görebilirsiniz:

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
   Bölge sunucusu işlem durdurulmuş olsa bile ani kapatma nedeniyle, işlemle ilişkili bağlantı noktası, serbest bırakılmayabilir. Bölge sunucusu başlatıldığında, bu durum aşağıdaki günlüklere gösterildiği bir AddressBindException için açabilir. Bu, burada başlatmak için bölge sunucusu başarısız çalışan düğümlerinin /var/log/hbase dizininde bölge server.log de doğrulayabilirsiniz. 

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

1. Yeniden başlatmadan önce HBase bölge sunucuları üzerindeki yükü azaltmak bu seçeneği deneyin. 
2. Alternatif olarak (1. adım yardımcı olmaz,), aşağıdaki komutları kullanarak çalışan düğümleri üzerinde bölge sunucuları el ile yeniden başlatmayı deneyin:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

### <a name="see-also"></a>Ayrıca Bkz.
[Azure HDInsight'ı kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)
