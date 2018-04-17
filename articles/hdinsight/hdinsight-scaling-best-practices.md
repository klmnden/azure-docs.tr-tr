---
title: Küme boyutlarını - Azure Hdınsight ölçeklendirin | Microsoft Docs
description: İş yükünüzün bir Hdınsight kümesine ölçeklendirin.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: ashish
ms.openlocfilehash: 8b76d7d0441a5c1c25ad17b73083ec0e4feef1fe
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="scale-hdinsight-clusters"></a>Ölçek Hdınsight kümeleri

Hdınsight seçeneği, kümelerinde çalışan düğümü sayısını aşağı ölçeğini artırıp vererek esneklik sağlar. Bu, bir küme saat sonra veya hafta sonları küçültmek ve yoğun iş taleplerini sırasında genişletmek sağlar.

Örneğin, bazı toplu işlemler varsa, günde bir kez veya ayda bir kez gerçekleşir, Hdınsight kümesi zamanlanmış olay önce birkaç dakika yukarı yeterli bellek olacaktır ve CPU işlem güç şekilde genişletilebilir. PowerShell cmdlet'iyle ölçeklendirme otomatikleştirebilirsiniz [ `Set–AzureRmHDInsightClusterSize` ](hdinsight-administer-use-powershell.md#scale-clusters).  Daha sonra işlem yapılmaz ve kullanım yeniden arıza sonra daha az çalışan düğümü Hdınsight kümesine aşağı ölçeklendirebilirsiniz.

* Kümenizi aracılığıyla ölçeklendirmek için [PowerShell](hdinsight-administer-use-powershell.md):

    ```powershell
    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    ```
    
* Kümenizi aracılığıyla ölçeklendirmek için [Azure CLI](hdinsight-administer-use-command-line.md):

    ```
    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
    ```
    
* Kümenizi aracılığıyla ölçeklendirmek için [Azure portal](https://portal.azure.com), Hdınsight kümesi Bölmesi'ni açın, **ölçekte küme** sol menüde, sonra ölçek kümesi bölmesi, çalışan düğümü sayısı girin ve Kaydet'i seçin.

    ![Küme ölçeklendirme](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Bu yöntemlerden birini kullanarak Hdınsight kümenize yukarı veya aşağı dakika içinde ölçekleme yapabilirsiniz.

## <a name="scaling-impacts-on-running-jobs"></a>İşlerini çalıştırma ölçeklendirme etkileri

Olduğunda, **ekleme** düğümleri çalışan Hdınsight kümenize, bekleyen veya çalışan işleri değil etkilenir. Ayrıca, ölçeklendirme işlemi devam ederken yeni işleri güvenle gönderilebilir. Ölçeklendirme işlemlerinin herhangi bir nedenle başarısız olursa hata düzgün biçimde, kümenin işlevsel bir durumda bırakır ele alınır.

Ancak, küme tarafından aşağı Ölçeklendirmesi, **kaldırma** düğümleri, bekleyen veya çalışan işleri başarısız ölçeklendirme işlemi tamamlandığında olur. Bazı işlemi sırasında yeniden başlatma hizmetleri nedeniyle bu hatasıdır.

Bu sorunu gidermek için kümenizi ölçeklendirmeden önce tamamlanması, el ile işleri sonlandırma veya ölçekleme işlemi tamamlanmış sonra sunmaları işlerinin tamamlanmasını bekleyebilir.

Bekleyen bir listesini ve çalışan işleri görmek için aşağıdaki adımları YARN ResourceManager UI kullanabilirsiniz:

1. [Azure portalda](https://portal.azure.com) oturum açın.
2. Soldaki menüden seçin **Gözat**seçin **Hdınsight kümeleri**ve, kümeyi seçin.
3. Hdınsight küme bölmesinden seçin **Pano** üst menüsünde Ambari kullanıcı arabirimini açın. Küme oturum açma kimlik bilgilerinizi girin.
4. Tıklatın **YARN** sol menüdeki hizmetlerin listesi üzerinde. YARN sayfasında seçin **hızlı bağlantılar** ve etkin baş düğüm üzerinde üzerine gelin ve ardından **ResourceManager UI**.

    ![ResourceManager kullanıcı Arabirimi](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

ResourceManager kullanıcı Arabirimi olmadan doğrudan erişebilir `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

Geçerli durumlarına birlikte işlerin bir listesini görürsünüz. Ekran görüntüsünde, şu anda çalışan bir iş vardır:

![ResourceManager UI uygulamaları](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

El ile çalışan uygulama sonlandırmak için SSH Kabuğu'ndan aşağıdaki komutu yürütün:

```bash
yarn application -kill <application_id>
```

Örneğin:

```bash
yarn application -kill "application_1499348398273_0003"
```

## <a name="rebalancing-an-hbase-cluster"></a>Bir HBase kümesi yeniden dengelenmesi

Bölge sunucuları otomatik olarak ölçeklendirme işlemi tamamlandıktan sonra birkaç dakika içinde dengeli. El ile bölge sunucuları dengelemek için aşağıdaki adımları kullanın:

1. SSH kullanarak Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. HBase Kabuğu'nu başlatın:

        hbase shell

3. Bölge sunucuları el ile eşitlemek için aşağıdaki komutu kullanın:

        balancer

## <a name="scale-down-implications"></a>Etkileri ölçeklendirme

Daha önce belirtildiği gibi tüm bekleyen veya çalışan işler ölçeklendirme işlemi tamamlandığında sonlandırılır. Ancak, ortaya çıkabilecek aşağı Ölçeklendirmesi diğer olası etkileri vardır.

## <a name="hdinsight-name-node-stays-in-safe-mode-after-scaling-down"></a>Hdınsight ad düğümü, ölçekleme sonra güvenli modda kalır.

![Küme ölçeklendirme](./media/hdinsight-scaling-best-practices/scale-cluster.png)

Önceki görüntüde gösterildiği gibi kümenizi aşağıya doğru en az bir çalışan düğümünün Küçült, düzeltme eki uygulama nedeniyle ya da ölçekleme işlemi hemen sonra alt düğümleri yeniden zaman HDFS güvenli modda durumunda takılıp.

Bu birincil nedenini Hive birkaç kullanmasıdır `scratchdir` dosyaları ve varsayılan olarak her bloğu üç çoğaltmalarının bekliyor ancak olup olmadığını yalnızca bir çoğaltma olası en az bir alt düğüm ölçeklendirmeyi azaltın. Dosyaları bir sonucu olarak `scratchdir` hale *under-çoğaltılmış*. Bu hizmetler ölçek işleminden sonra yeniden başlatıldığında güvenli modda kalmak HDFS neden olabilir.

Bir ölçek girişimi olduğunda, önce kendi HDFS blokları diğer çevrimiçi çalışan düğümlere çoğaltır, fazladan istenmeyen çalışan düğümleri yetkisini alma ve güvenli bir şekilde küme ölçeklendirmeyi azaltın Ambari yönetim arabirimleri üzerine Hdınsight kullanır. HDFS bakım penceresi sırasında güvenli bir moduna girer ve ölçeklendirme tamamlandıktan sonra gelen beklenir. Bu noktada, HDFS güvenli modda durumunda takılıp kalabilir emin olur.

HDFS ile yapılandırılmış bir `dfs.replication` 3'ün ayarlama. Bu nedenle, üçten çalışan düğümleri çevrimiçi olduğunda kullanılabilir her dosya bloğu değil beklenen üç kopyasını olmadığından geçici dosya bloklarını under-çoğaltılmış.

HDFS güvenli modundan çıkarmak için bir komut yürütebilir. Geçici dosyalar under-çoğaltılmış olduğundan güvenli mod açıktır yalnızca neden olduğunu biliyorsanız, örneğin, ardından güvenli bir şekilde güvenli mod bırakabilirsiniz. Hive geçici geçici dosyalar under-çoğaltılmış dosyalarıdır olmasıdır.

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Güvenli mod bırakarak sonra el ile geçici dosyaları kaldırabilir, veya Hive sonunda bunları otomatik olarak temizlemek için bekleyin.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Güvenli mod açıldığında örnek hataları

* H070 Hive oturum açamıyor. org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **dizini oluşturulamıyor**  /tmp/hive/hive / 819c215c - 6d 87 4311 97c 8-4f0b9d2adcf0. **Ad düğümdür güvenli modda**. Bildirilen blokları 75 toplam blokların 87 0.9900 eşik ulaşmak için ek 12 blokları gerekir. Canlı datanodes 10 sayısı 0 minimum sayısı sınırına ulaştı. Eşikleri ulaşıldığında güvenli mod otomatik olarak kapatılır.

* Veritabanları H100 deyimi kurmada göster: org.apache.thrift.transport.TTransportException: org.apache.http.conn.HttpHostConnectException: hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername için Bağlan . internal.cloudapp.NET/1.1.1.1] başarısız oldu: **bağlantı reddedildi**

* H020 bağlantısı kuramadı hn0 hdisrv.servername.bx.internal.cloudapp .net için: 10001: org.apache.thrift.transport.TTransportException: http bağlantısı oluşturulamadı http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: hn0-başarısız hdisrv.servername.bx.internal.cloudapp.net:10001 için [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] bağlantı: bağlantı reddedildi: org.apache.thrift.transport.TTransportException: http bağlantısı oluşturulamadı http://hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: Connect to hn0-hdisrv.servername.bx.internal.cloudapp.net:10001 [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] failed: **Connection refused**

* Hive günlüklerinden: [Temel] UYAR: sunucu. HiveServer2 (HiveServer2.java:startHiveServer2(442)) – 21, denemede HiveServer2 başlatılırken hata oluştu, 60 saniye java.lang.RuntimeException deneyecek: hive yapılandırmasına yetkilendirme ilkesi uygulanırken hata oluştu: org.apache.hadoop.ipc.RemoteException () org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **dizini oluşturulamıyor** /tmp/hive/hive/70a42b8a-9437-466e-acbe-da90b1614374. **Ad düğümdür güvenli modda**.
    Bildirilen blokları 0 0.9900 toplam blokların 9 eşik ulaşmak için ek 9 blokları gerekir.
    Canlı datanodes 10 sayısı 0 minimum sayısı sınırına ulaştı. **Güvenli mod kapatılacak otomatik olarak eşikleri ulaşıldığında**.
    org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1324)

Ad düğümü günlükleri gözden geçirebilirsiniz `/var/log/hadoop/hdfs/` zaman küme ölçeği, ne zaman güvenli mod girilen görmek için zaman yakın klasör. Günlük dosyaları adlı `Hadoop-hdfs-namenode-hn0-clustername.*`.

Kök önceki hatalara Hive HDFS geçici dosyaları sorgu çalıştırılırken bağlı olduğunu nedenidir. HDFS güvenli mod girdiğinde, HDFS için yazılamıyor Hive sorguları çalıştırılamıyor. HDFS geçici dosyaları ayrı ayrı çalışan düğümüne VM'ler bağlanır ve diğer üç çoğaltmalar, en düşük alt düğümlerde arasında çoğaltılan yerel sürücü bulunur.

`hive.exec.scratchdir` Hive parametresinde içinde yapılandırılmış `/etc/hive/conf/hive-site.xml`:

```xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>hdfs://mycluster/tmp/hive</value>
</property>
```

### <a name="view-the-health-and-state-of-your-hdfs-file-system"></a>Sistem durumu HDFS dosya sisteminin durumunu görüntüleme

Düğümleri güvenli modda olup olmadığını görmek için her ad düğümünden bir durum raporu görüntüleyebilirsiniz. Her baş düğüm içine SSH raporu görüntülemek ve aşağıdaki komutu çalıştırmak için:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode get
```

![Güvenli modu devre dışı](./media/hdinsight-scaling-best-practices/safe-mode-off.png)

> [!NOTE]
> `-D` Anahtar, Azure Storage veya Azure Data Lake Store Hdınsight varsayılan dosya sisteminde olduğu için gereklidir. `-D` komutlar yerel HDFS dosya sistemi karşı yürütülürken belirtir.

Ardından, HDFS durumu ayrıntılarını gösteren bir rapor görüntüleyebilirsiniz:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -report
```

Bu komut tüm blokları beklenen derece çoğaltıldığı sağlıklı bir kümede aşağıdaki sonuçlanır:

![Güvenli modu devre dışı](./media/hdinsight-scaling-best-practices/report.png)

HDFS destekleyen `fsck` eksik gibi çeşitli dosyalarla tutarsızlıkları denetlemek için komut, bir dosya veya under-çoğaltılmış blokları için engeller. Çalıştırmak için `fsck` karşı komutu `scratchdir` (Geçici çalışma diski) dosyaları:

```
hdfs fsck -D 'fs.default.name=hdfs://mycluster/' /tmp/hive/hive
```

Hiçbir under-çoğaltılmış bloğu ile sağlıklı bir HDFS dosya sisteminde çalıştırıldığında, aşağıdakine benzer bir çıktı görürsünüz:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:07:01 UTC 2017
..Status: HEALTHY
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
 Minimally replicated blocks:   2 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          4
 Number of racks:               1
FSCK ended at Thu Jul 06 20:07:01 UTC 2017 in 3 milliseconds


The filesystem under path '/tmp/hive/hive' is HEALTHY
```

Buna karşılık, `fsck` komutu bazı under-çoğaltılmış bloklara sahip bir HDFS dosya sisteminde yürütülürse, çıktı aşağıdakine benzer:

```
Connecting to namenode via http://hn0-scalin.name.bx.internal.cloudapp.net:30070/fsck?ugi=sshuser&path=%2Ftmp%2Fhive%2Fhive
FSCK started by sshuser (auth:SIMPLE) from /10.0.0.21 for path /tmp/hive/hive at Thu Jul 06 20:13:58 UTC 2017
.
/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info:  Under replicated BP-1867508080-10.0.0.21-1499348422953:blk_1073741826_1002. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s) and 0 decommissioning replica(s).
.
/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: CORRUPT blockpool BP-1867508080-10.0.0.21-1499348422953 block blk_1073741825

/tmp/hive/hive/e7c03964-ff3a-4ee1-aa3c-90637a1f4591/inuse.info: MISSING 1 blocks of total size 26 B.Status: CORRUPT
 Total size:    53 B
 Total dirs:    5
 Total files:   2
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2 (avg. block size 26 B)
  ********************************
  UNDER MIN REPL'D BLOCKS:      1 (50.0 %)
  dfs.namenode.replication.min: 1
  CORRUPT FILES:        1
  MISSING BLOCKS:       1
  MISSING SIZE:         26 B
  CORRUPT BLOCKS:       1
  ********************************
 Minimally replicated blocks:   1 (50.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       1 (50.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     0.5
 Corrupt blocks:                1
 Missing replicas:              2 (33.333332 %)
 Number of data-nodes:          1
 Number of racks:               1
FSCK ended at Thu Jul 06 20:13:58 UTC 2017 in 28 milliseconds


The filesystem under path '/tmp/hive/hive' is CORRUPT
```

Seçerek Ambari Arabiriminde HDFS durumunu görüntüleyebilirsiniz **HDFS** sol veya ile hizmet `https://<HDInsightClusterName>.azurehdinsight.net/#/main/services/HDFS/summary`.

![Ambari HDFS durumu](./media/hdinsight-scaling-best-practices/ambari-hdfs.png)

Etkin ya da bekleme NameNodes üzerinde bir veya daha fazla kritik hatalar da görebilirsiniz. İş blokları durumunu görüntülemek için uyarı yanındaki iş bağlantıyı seçin.

![İş blokları sistem durumu](./media/hdinsight-scaling-best-practices/ambari-hdfs-crit.png)

Geçici dosyaları temizleme hangi blok çoğaltma hataları, her baş düğüm içine SSH kaldırır ve aşağıdaki komutu çalıştırın:

```
hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
```

> [!NOTE]
> Bazı iş hala devam ediyorsa bu komut yığın bozulabilir.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode-due-to-under-replicated-blocks"></a>Hdınsight under-çoğaltılmış blokları nedeniyle güvenli modda takılmış gelen engelleme

Güvenli modda sol Hdınsight önlemek için birkaç yolu vardır:

* Hdınsight ölçeklendirmeden önce tüm Hive işleri durdurur. Alternatif olarak, Hive işleri çalıştırmayı çakışmasını önlemek için işlem ölçek zamanlayın.
* Hive'nın karalama el ile temizlemek `tmp` ölçeklendirme önce dizin dosyalarında HDFS.
* Yalnızca Hdınsight üç alt düğümler için minimum ölçeklendirin. Bir alt düğümü olarak kadar düşük giderek kaçının.
* Gerekirse güvenli modundan çıkmak için komutu çalıştırın.

Aşağıdaki bölümlerde bu seçenekler açıklanmaktadır.

#### <a name="stop-all-hive-jobs"></a>Tüm Hive işleri durdurur

Bir çalışan düğümüne ölçeklendirme önce tüm Hive işleri durdurur. Ardından yükünüzü zamanlandıysa, Hive iş tamamlandıktan sonra ölçek azaltma yürütün.

Bu, geçici dosyaları (varsa) tmp klasöre sayısını en aza indirmenize yardımcı olur.

#### <a name="manually-clean-up-hives-scratch-files"></a>Hive'nın geçici dosyaları el ile Temizle

Geçici dosyalar Hive ayrıldıysa ölçeği önce bu dosyaları el ile temizleyebilirsiniz sonra güvenli mod kaçının.

1. Hive hizmetleri durdurun ve tüm sorgular ve işleri tamamlandı emin olun.

2. İçeriğini listele `hdfs://mycluster/tmp/hive/` herhangi bir dosya içeriyorsa görmek için directory:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```
    
    Dosyalar varken örnek bir çıktı şöyledir:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Bu dosyalarda Hive yapılır biliyorsanız, bunları kaldırabilirsiniz. Hive Yarn ResourceManager UI sayfasında bakarak çalıştıran herhangi bir sorgu sahip olmadığını unutmayın.

    HDFS dosyaları kaldırmak için örnek komut satırı:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```
    
#### <a name="scale--hdinsight-to-three-worker-nodes"></a>Ölçek Hdınsight üç alt düğümleri

Güvenli modda takılmış, kalıcı bir sorundur ve yalnızca üç alt düğümleri aşağıya doğru ölçeklendirme tarafından kaçınmak isteyebilirsiniz sonra önceki adımları seçenekleri değildir. Bu bir düğüme ölçeklendirme karşılaştırılan maliyet kısıtlamalar nedeniyle uygun olmayabilir. Ancak, yalnızca bir alt düğüm ile verilerin üç çoğaltmalarının küme için kullanılabilir HDFS garanti edemez.

#### <a name="run-the-command-to-leave-safe-mode"></a>Güvenli modda bırakın için komutu çalıştırın

Son seçenek HDFS güvenli moduna girer nadir durumlarda için izlemek için ardından bırakın güvenli mod komutu yürütün. HDFS neden geçtiğini belirledikten sonra güvenli mod under-çoğaltılmış olan Hive dosyalar nedeniyle, güvenli mod bırakmak için aşağıdaki komutu yürütün:

* Hdınsight Linux üzerinde:

    ```bash
    hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
    ```
    
* Windows üzerinde Hdınsight:

    ```bash
    hdfs dfsadmin -fs hdfs://headnodehost:9000 -safemode leave
    ```
    
## <a name="next-steps"></a>Sonraki adımlar

* [Azure Hdınsight giriş](hadoop/apache-hadoop-introduction.md)
* [Ölçek kümeleri](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)
