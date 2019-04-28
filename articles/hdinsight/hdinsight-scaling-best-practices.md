---
title: Ölçek kümesi boyutları - Azure HDInsight
description: İş yükünüz için bir HDInsight kümesini ölçeklendirin.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: ashish
ms.openlocfilehash: a172024e4662e647b39fe999f1be3cfcef04b5ce
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63763611"
---
# <a name="scale-hdinsight-clusters"></a>HDInsight kümeleri ölçeklendirme

HDInsight, ölçeğini ve, kümede çalışan düğümleri sayısını ölçeğini seçeneği vererek esneklik sağlar. Bu, bir küme saat sonra veya hafta sonu küçültme ve en yüksek iş gereksinimlerini sırasında genişletmek sağlar.

Örneğin, bazı toplu işlem varsa, günde bir kez veya ayda bir kez gerçekleşir, HDInsight kümesi zamanlanmış olay önce birkaç dakika'kurmak için yeterli bellek olacaktır ve CPU işlem gücü ölçeklendirilebilir.  Daha sonra işlem tamamlandı ve yeniden kullanımı arıza sonra HDInsight kümesine daha az çalışan düğümü aşağı ölçeklendirebilirsiniz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="utilities-to-scale-clusters"></a>Ölçek kümeleri için yardımcı programlar

Microsoft kümeleri ölçeklendirmek için aşağıdaki yardımcı programlarını sağlar:

|yardımcı programı | Açıklama|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[Set-AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) - ClusterName \<küme adı > - TargetInstanceCount \<NewSize >|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[Set-AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) - ClusterName \<küme adı > - TargetInstanceCount \<NewSize >|
|[Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)|[az hdınsight yeniden boyutlandırma](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) --resource-group \<kaynak grubu >--ad \<küme adı >--hedef örnek sayısı \<NewSize >|
|[Klasik Azure CLI](hdinsight-administer-use-command-line.md)|Azure hdınsight küme boyutlandırma \<clusterName > \<hedef örnek sayısı >|
|[Azure portal](https://portal.azure.com)|HDInsight kümesi bölmenizi açın, **küme boyutu** sol taraftaki menüden, sonra küme boyutu bölmesinde, çalışan düğümlerinin sayısını yazın ve Kaydet'i seçin.|  

![Küme ölçeklendirme](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Bu yöntemlerden birini kullanarak, HDInsight kümenizin ölçeğini artırıp dakika içinde ölçeklendirebilirsiniz.

> [!IMPORTANT]  
> * With Azure Klasik CLI kullanım dışı bırakılmıştır ve yalnızca klasik dağıtım modeliyle kullanılmalıdır. Diğer tüm dağıtımları için kullanmak [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).  
> * PowerShell AzureRM modülü kullanım dışı bırakılmıştır.  Lütfen kullanın [Az modül](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) mümkün olduğunda.

## <a name="scaling-impacts-on-running-jobs"></a>Ölçeklendirme etkileri işlerini çalıştırma

Olduğunda, **ekleme** düğümleri çalışan HDInsight kümenize, bekleyen veya çalışan tüm işleri etkilenmeyecektir. Ayrıca, yeni işleri güvenli bir şekilde ölçeklendirme işlemi devam ederken gönderilebilir. Ölçeklendirme işlemleri için herhangi bir nedenle başarısız olursa, hata düzgün bir şekilde, kümenin işlevsel bir durumda bırakarak ele alınır.

Ancak, kümeniz tarafından aşağı ölçeklendirme, **kaldırma** düğümler, bekleyen veya çalışan tüm işler başarısız ölçeklendirme işlemi tamamlandığında olur. Bu hata, yeniden başlatma işlemi sırasında hizmetlerinden bazılarını kaynaklanır.

Bu sorunu gidermek için kümenizi ölçeklendirmeden önce tamamlamak, el ile işleri sonlandırmak veya ölçeklendirme işlemini sonlandırdığını sonra sunmaları işlerinin tamamlanmasını bekleyebilirsiniz.

Bekleyen bir listesini ve çalışan işleri görmek için aşağıdaki adımları YARN ResourceManager UI kullanabilirsiniz:

1. [Azure portalda](https://portal.azure.com) oturum açın.
2. Soldan gidin **tüm hizmetleri** > **Analytics** > **HDInsight kümeleri**ve sonra kümenizi seçin.
3. Ana görünümünde gidin **küme panoları** > **Ambari giriş**. Küme oturum açma kimlik bilgilerinizi girin.
4. Ambari Arabiriminden seçin **YARN** Hizmetleri sol menüdeki listesi.  
5. YARN sayfasından seçin **hızlı bağlantılar** ve etkin baş düğümün üzerine gelin ve ardından **ResourceManager kullanıcı Arabirimi**.

    ![ResourceManager kullanıcı Arabirimi](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

ResourceManager kullanıcı Arabirimi ile doğrudan erişebilir `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

Bunların geçerli durumunu yanı sıra, işlerinin listesini görürsünüz. Ekran görüntüsünde, şu anda çalışan bir işi vardır:

![ResourceManager kullanıcı Arabirimi uygulama](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

El ile çalışan bu uygulamayı sonlandırmak için SSH kabuğundan aşağıdaki komutu yürütün:

```bash
yarn application -kill <application_id>
```

Örneğin:

```bash
yarn application -kill "application_1499348398273_0003"
```

## <a name="rebalancing-an-apache-hbase-cluster"></a>Bir Apache HBase kümesi yeniden Dengeleme

Bölge sunucuları, sonraki ölçeklendirme işleminin tamamlanması birkaç dakika içinde otomatik olarak dengelenir. Bölge sunucuları el ile dengelemek için aşağıdaki adımları kullanın:

1. HDInsight kümesine SSH kullanarak bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. HBase kabuğunu başlatın:

        hbase shell

3. Bölge sunucuları el ile dengelemek için aşağıdaki komutu kullanın:

        balancer

## <a name="scale-down-implications"></a>Etkileri ölçeklendirin

Daha önce belirtildiği gibi bekleyen veya çalışan tüm işleri ölçeklendirme işlemi tamamlanma sonlandırılır. Ancak, oluşabilecek aşağı ölçeklendirme için diğer olası etkileri vardır.

## <a name="hdinsight-name-node-stays-in-safe-mode-after-scaling-down"></a>HDInsight ad düğümü, ölçeği sonra güvenli modda kalır.

Kümenizin en az bir çalışan düğümü aşağı küçültme, çalışan düğümleri yamalama nedeniyle veya ölçeklendirme işleminden hemen sonra yeniden başlatılır, Apache HDFS güvenli modda takılırsa haline.

Bu durumun birincil nedeni Hive birkaç kullanmasıdır `scratchdir` dosyaları ve varsayılan olarak her blok üç kopyasını bekliyor ancak olup yalnızca bir yineleme olası en az bir çalışan düğümü için ölçeği azaltın. Dosyaları bir sonucu olarak `scratchdir` haline *under-çoğaltılmış*. Bu hizmetleri ölçeklendirme işleminden sonra yeniden başlatıldığında güvenli modda kalmak HDFS neden olabilir.

Bir ölçeği azaltma girişimi olduğunda, önce diğer çevrimiçi çalışan düğümlerine, HDFS blokları çoğaltmak, fazladan istenmeyen çalışan düğümlerinin yetkisini alma ve ardından güvenli bir şekilde kümenin ölçeğini için Apache Ambari yönetim arabirimleri üzerinde HDInsight'ı kullanır. HDFS, bakım penceresi sırasında güvenli bir moduna girer ve ölçeklendirme tamamlandıktan sonra gelen beklenir. Bu noktada, HDFS güvenli modda takılırsa duruma değil.

HDFS ile yapılandırılmış bir `dfs.replication` 3 ayarlama. Bu nedenle, az üç çalışan düğümü çevrimiçi olduğunda kullanılabilir her dosya bloğun beklenen üç kopya olmadığından geçici dosya bloklarını under-çoğaltılmış.

Güvenli mod dışında HDFS getirmek için bir komut çalıştırabilirsiniz. Geçici dosyalar under-çoğaltılmış olduğundan güvenli mod açıktır tek nedeni olduğunu biliyorsanız, örneğin, ardından güvenli bir şekilde güvenli mod bırakabilirsiniz. Hive geçici karalama dosyaları under-çoğaltılmış dosyalarıdır olmasıdır.

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Güvenli mod bırakarak sonra el ile geçici dosyaları kaldırabilir, veya Hive sonunda otomatik olarak temizlenmesi için bekleyin.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Güvenli modda açıldığında örnek hataları

* H070 Hive oturum açamıyor. org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **Dizini oluşturulamıyor** /tmp/hive/hive/819c215c-6d 87 4311 97 c 8-4f0b9d2adcf0. **Adı düğümüdür güvenli modda**. Bildirilen blokları 75 toplam bloğu 87 0.9900 eşiği ulaşmak için ek 12 blokları gerekir. Canlı datanodes 10 sayısı 0 en düşük sayısı sınırına ulaştı. Eşikleri sınıra ulaşıldıktan sonra güvenli mod otomatik olarak kapatılır.

* Veritabanları alınamıyor bildirimi göndermek için H100 göster: org.apache.thrift.transport.TTransportException: org.apache.http.conn.HttpHostConnectException: Hn0-clustername.servername.internal.cloudapp.net:10001 için [clustername.servername hn0. bağlanma internal.cloudapp.NET/1.1.1.1] başarısız oldu: **Bağlantı reddedildi**

* H020 kuramadı hn0 hdisrv.servername.bx.internal.cloudapp .net bağlantısı: 10001: org.apache.thrift.transport.TTransportException: Http http bağlantısı oluşturulamadı:\//hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: Hn0-başarısız hdisrv.servername.bx.internal.cloudapp.net:10001 için [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] bağlanın: Bağlantı reddedildi: org.apache.thrift.transport.TTransportException: Http http bağlantısı oluşturulamadı:\//hn0-hdisrv.servername.bx.internal.cloudapp.net:10001/. org.apache.http.conn.HttpHostConnectException: Hn0-başarısız hdisrv.servername.bx.internal.cloudapp.net:10001 için [hn0-hdisrv.servername.bx.internal.cloudapp.net/10.0.0.28] bağlanın: **Bağlantı reddedildi**

* Hive günlükleri: [Ana] UYAR: sunucu. HiveServer2 (60 saniye java.lang.RuntimeException içinde yeniden HiveServer2.java:startHiveServer2(442)) – 21, denemede HiveServer2 başlatılırken hata oluştu: Yetkilendirme İlkesi hive Yapılandırması uygulanırken hata oluştu: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.RetriableException): org.apache.hadoop.hdfs.server.namenode.SafeModeException: **Dizini oluşturulamıyor** /tmp/hive/hive/70a42b8a-9437-466e-acbe-da90b1614374. **Adı düğümüdür güvenli modda**.
    Bildirilen blokları 0 toplam bloğu 9 0.9900 eşiği ulaşmak için ek 9 blokları gerekir.
    Canlı datanodes 10 sayısı 0 en düşük sayısı sınırına ulaştı. **Güvenli mod kapalı otomatik olarak eşikleri sınıra ulaşıldıktan sonra**.
    at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1324)

Adı düğüm günlüklerini gözden geçirebilirsiniz `/var/log/hadoop/hdfs/` yakın bir zamanda ne zaman küme ölçeği, ne zaman güvenli mod girdiği görmek için bir klasör. Günlük dosyaları adlı `Hadoop-hdfs-namenode-hn0-clustername.*`.

Önceki hataların temel nedenini Hive HDFS geçici dosyaları sorgular çalıştırılırken bağlı olduğunu ' dir. HDFS güvenli mod girdiğinde, HDFS'ye yazamaz Hive sorguları çalıştırılamıyor. HDFS geçici dosyaların yerel diske Vm'leri ayrı ayrı çalışan düğümüne bağlanmış ve diğer çalışan düğümlerinde en az üç kopyaya arasında çoğaltılan bulunur.

`hive.exec.scratchdir` Hive parametresinde içinde yapılandırılmış `/etc/hive/conf/hive-site.xml`:

```xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>hdfs://mycluster/tmp/hive</value>
</property>
```

### <a name="view-the-health-and-state-of-your-hdfs-file-system"></a>Sistem durumu ve HDFS dosya sisteminin durumunu görüntüleme

Düğüm, güvenli modda olup olmadığını görmek için her ad düğümden bir durum raporu görüntüleyebilirsiniz. Aşağıdaki komutu çalıştırın ve her baş düğümüne SSH uygulayın raporu görüntülemek için:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode get
```

![Güvenli modu kapalı](./media/hdinsight-scaling-best-practices/safe-mode-off.png)

> [!NOTE]  
> `-D` Anahtar, Azure depolama veya Azure Data Lake Storage HDInsight varsayılan dosya sisteminde olduğu için gereklidir. `-D` komutlar yerel HDFS dosya sisteminin karşı yürütmek belirtir.

Ardından, HDFS durumunun ayrıntılarını gösteren bir raporu görüntüleyebilirsiniz:

```
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -report
```

Bu komut, aşağıdaki beklenen dereceye kadar tüm blokları çoğaltıldığı sağlıklı bir kümede sonuçlanır:

![Güvenli modu kapalı](./media/hdinsight-scaling-best-practices/report.png)

HDFS'yi desteklemesi `fsck` eksik gibi çeşitli dosyalarla tutarsızlıkları denetlemek için komut, bir dosya veya under-çoğaltılmış blokları için engeller. Çalıştırılacak `fsck` karşı komutunu `scratchdir` (geçici boş disk) dosyaları:

```
hdfs fsck -D 'fs.default.name=hdfs://mycluster/' /tmp/hive/hive
```

Hiçbir under-çoğaltılmış bloğu ile sağlam bir HDFS dosya sisteminde çalıştırıldığında, aşağıdakine benzer bir çıktı görürsünüz:

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

Buna karşılık, `fsck` bir HDFS dosya sisteminde bazı under-çoğaltılmış bloklara sahip komut yürütüldüğünde, aşağıdakine benzer bir çıkış:

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

Seçerek Ambari UI içinde HDFS durumunu görüntüleyebilirsiniz **HDFS** sol, ya da ile hizmet `https://<HDInsightClusterName>.azurehdinsight.net/#/main/services/HDFS/summary`.

![Ambari HDFS durumu](./media/hdinsight-scaling-best-practices/ambari-hdfs.png)

Bu gibi durumlarda, bir veya daha fazla kritik hata ayrıca etkin ya da bekleme NameNodes görebilirsiniz. NameNode blokları durumunu görüntülemek için uyarı yanındaki NameNode bağlantısını seçin.

![NameNode blokları sistem durumu](./media/hdinsight-scaling-best-practices/ambari-hdfs-crit.png)

Blok çoğaltma hataları Kaldır, karalama dosyalarınızı temizlemek için SSH komutu her düğümü'na gidin ve aşağıdaki komutu çalıştırın:

```
hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
```

> [!NOTE]  
> Bazı iş hala devam ediyorsa, bu komut yığın bozabilir.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode-due-to-under-replicated-blocks"></a>HDInsight under-çoğaltılmış blokları nedeniyle güvenli modda takılı gelen engelleme

Güvenli modda sol HDInsight önlemek için birkaç yol vardır:

* HDInsight ölçeklendirmeden önce tüm Hive işleri durdurur. Alternatif olarak, ölçeği azaltma işlemi Hive işleri çalıştırmayı çakışmasını önlemek için zamanlayın.
* Hive'nın sıfırdan el ile temizlemek `tmp` ölçeği önce HDFS dizin dosyaları.
* Yalnızca üç çalışan düğümü için en düşük HDInsight ölçeklendirin. Bir alt düğüm olarak düşük kullanmaya özen gösterin.
* Gerekirse güvenli modundan ayrılmak için komutu çalıştırın.

Aşağıdaki bölümlerde, bu seçenekler açıklanmaktadır.

#### <a name="stop-all-hive-jobs"></a>Tüm Hive işlerini durdurma

Bir çalışan düğümüne ölçeği önce tüm Hive işleri durdurur. Ardından iş yükünüz zamanlanırsa, Hive iş tamamlandıktan sonra ölçeği yürütün.

Bu, geçici dosyaları (varsa) tmp klasöre sayısını en aza indirmenize yardımcı olur.

#### <a name="manually-clean-up-hives-scratch-files"></a>El ile Hive'nın karalama dosyaları Temizle

Güvenli mod aşağı ölçeklendirmeden önce bu dosyaları el ile temizleyebilirsiniz ardından Hive geçici dosyalar ayrıldı kaçının.

1. Hive hizmetlerini durdurmak ve tüm sorgular ve işleri tamamlandı emin olun.

2. İçeriğini listelemek `hdfs://mycluster/tmp/hive/` dosyalar içerip içermediğini görmek için directory:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```
    
    Dosyaları mevcut olduğunda örnek çıktı şöyledir:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Hive, bu dosyalarda yapılan biliyorsanız bunları kaldırabilirsiniz. Hive Yarn ResourceManager kullanıcı Arabirimi sayfasındaki bakarak çalışan tüm sorguları yok olduğundan emin olun.

    HDFS dosyaları kaldırmak için komut satırı örneğinde:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```
    
#### <a name="scale--hdinsight-to-three-worker-nodes"></a>Üç çalışan düğümü için ölçek HDInsight

Güvenli modda takılırsa, kalıcı bir sorundur ve yalnızca üç çalışan düğümü aşağı ölçeklendirme kaçınmak istiyorsanız önceki adımları seçenekler değildir. Bu, bir düğüm için ölçeği azaltma ile karşılaştırıldığında, maliyet kısıtlamaları nedeniyle uygun olmayabilir. Ancak, yalnızca bir çalışan düğümü ile verilerin üç kopyasını kümeye kullanılabilir HDFS garanti edemez.

#### <a name="run-the-command-to-leave-safe-mode"></a>Güvenli modundan ayrılmak için komutu çalıştırın

Son seçenek, HDFS güvenli moda girer nadir durumlarda için izlemek için ardından bırakın güvenli mod komutu yürütün. Nedeni HDFS girdiğini belirledikten sonra under-çoğaltılmış olan Hive dosyalar nedeniyle güvenli mod, güvenli modundan ayrılmak için aşağıdaki komutu yürütün:

* Linux üzerinde HDInsight:

    ```bash
    hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
    ```
    
* Windows üzerinde HDInsight:

    ```bash
    hdfs dfsadmin -fs hdfs://headnodehost:9000 -safemode leave
    ```
    
## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md)
* [Ölçek kümeleri](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](hdinsight-hadoop-manage-ambari.md)
