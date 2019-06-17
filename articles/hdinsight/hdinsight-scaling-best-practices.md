---
title: Ölçek kümesi boyutları - Azure HDInsight
description: Esnek bir şekilde, iş yüküyle eşleşmesi için bir Azure HDInsight kümesini ölçeklendirin.
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: b85277a4238351b6448c2cf29676ae3d8c118385
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077188"
---
# <a name="scale-hdinsight-clusters"></a>HDInsight kümeleri ölçeklendirme

HDInsight, ölçeğini ve, kümede çalışan düğümleri sayısını ölçeğini seçeneği vererek esneklik sağlar. Bu esneklik saat sonra veya hafta sonu bir küme küçültme ve en yüksek iş gereksinimlerini sırasında genişletin olanak tanır.

Düzenli aralıklarla toplu işlem varsa, kümenizi yeterli bellek ve CPU gücü sahip olacak şekilde HDInsight küme önce bu işlem birkaç dakika'kurmak ölçeklendirilebilir.  Daha sonra işlem tamamlandı ve yeniden kullanımı arıza sonra HDInsight kümesine daha az çalışan düğümü aşağı ölçeklendirebilirsiniz.

Aşağıda açıklanan yöntemlerden birini kullanarak el ile bir kümenin ölçeğini veya kullanın [otomatik ölçeklendirme](hdinsight-autoscale-clusters.md) seçenekleri sistemin otomatik olarak sağlamak için ölçeği yukarı ve aşağı yanıt CPU, bellek ve diğer ölçümleri.

> [!NOTE]  
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.

## <a name="utilities-to-scale-clusters"></a>Ölçek kümeleri için yardımcı programlar

Microsoft kümeleri ölçeklendirmek için aşağıdaki yardımcı programlarını sağlar:

|yardımcı programı | Açıklama|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[Set-AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) - ClusterName \<küme adı > - TargetInstanceCount \<NewSize >|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[Set-AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) - ClusterName \<küme adı > - TargetInstanceCount \<NewSize >|
|[Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)| [az hdınsight yeniden boyutlandırma](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) --resource-group \<kaynak grubu >--ad \<küme adı >--hedef örnek sayısı \<NewSize >|
|[Azure CLI](hdinsight-administer-use-command-line.md)|Azure hdınsight küme boyutlandırma \<clusterName > \<hedef örnek sayısı > |
|[Azure portal](https://portal.azure.com)|HDInsight kümesi bölmenizi açın, **küme boyutu** sol taraftaki menüden, sonra küme boyutu bölmesinde, çalışan düğümlerinin sayısını yazın ve Kaydet'i seçin.|  

![Küme ölçeklendirme](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Bu yöntemlerden birini kullanarak, HDInsight kümenizin ölçeğini artırıp dakika içinde ölçeklendirebilirsiniz.

> [!IMPORTANT]  
> * With Azure Klasik CLI kullanım dışı bırakılmıştır ve yalnızca klasik dağıtım modeliyle kullanılmalıdır. Diğer tüm dağıtımları için kullanmak [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).  
> * PowerShell AzureRM modülü kullanım dışı bırakılmıştır.  Lütfen kullanın [Az modül](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) mümkün olduğunda.

## <a name="impact-of-scaling-operations"></a>Ölçeklendirme işlemlerinin etkisi

Olduğunda, **ekleme** düğümleri çalışan HDInsight kümenize (ölçeği artırma), bekleyen veya çalışan tüm işleri etkilenmez. Ölçeklendirme işlemi devam ederken yeni işleri güvenli bir şekilde gönderilebilir. Ölçeklendirme işlemi herhangi bir nedenle başarısız olursa hata kümenizi işlevsel bir durumda bırakır ele alınacaktır.

Varsa, **Kaldır** düğüm (ölçeği azaltma) tüm bekleyen veya çalışan iş ölçeklendirme işlemi tamamlandığında başarısız. Bu hata, bir ölçeklendirme işlemi sırasında yeniden başlatmayı hizmetlerinden bazılarını kaynaklanır. Kümenizi ölçeklendirme işlemi sırasında el ile olarak takılan güvenli mod alabilirsiniz riski yoktur.

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi değişir:

* Apache Hadoop

    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.

    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Bu davranış tüm çalışan ve farklı bekleyen işleri ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.

* Apache HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, küme baş düğümüne oturum açma ve bir komut istemi penceresinden aşağıdaki komutları çalıştırmadan tarafından el ile bölgesel sunucuları dengeleyebilirsiniz:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    HBase kabuğunu kullanma hakkında daha fazla bilgi için bkz. [HDInsight, Apache HBase örneğiyle çalışmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topolojiyi yeniden dengelemek gerekecektir.

    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgeleri](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![HDInsight Storm ölçek yeniden Dengeleme](./media/hdinsight-scaling-best-practices/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Bir örnek Storm topolojiyi yeniden dengelemek için CLI komutu aşağıda verilmiştir:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

## <a name="how-to-safely-scale-down-a-cluster"></a>Güvenli bir şekilde bir kümeyi ölçeklendirme

### <a name="scale-down-a-cluster-with-running-jobs"></a>Çalışan iş ile bir küme ölçeği

Bir ölçek ölçeği azaltma işlemi sırasında başarısız işlerinizi çalışan girmekten kaçınmak için üç şey deneyebilirsiniz:

1. İşleri kümenizi ölçeklendirmeden önce tamamlanması için bekleyin.
1. İşleri el ile bitmelidir.
1. İşleri ölçeklendirme işlemini sonlandırdığını sonra yeniden gönderin.

Bekleyen bir listesi ve çalışan işleri görmek için YARN kullanabilirsiniz **kaynak yöneticisi kullanıcı Arabirimi**, şu adımları izleyin:

1. Gelen [Azure portalında](https://portal.azure.com/), kümenizi seçin.  Bkz: [kümeleri Listele ve Göster](./hdinsight-administer-use-portal-linux.md#showClusters) yönergeler için. Kümeye yeni bir portal sayfası açılır.
2. Ana görünümünde gidin **küme panoları** > **Ambari giriş**. Küme kimlik bilgilerinizi girin.
3. Ambari Arabiriminden seçin **YARN** Hizmetleri sol menüdeki listesi.  
4. YARN sayfasından seçin **hızlı bağlantılar** ve etkin baş düğümün üzerine gelin ve ardından **ResourceManager kullanıcı Arabirimi**.

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

### <a name="getting-stuck-in-safe-mode"></a>Güvenli modda takılırsa

Bir kümeyi ölçeklediğinizde HDInsight Apache Ambari yönetim arabirimleri önce diğer çevrimiçi çalışan düğümlerine, HDFS blokları çoğaltma ek çalışan düğümlerinin yetkisini almak için kullanır. Bundan sonra HDInsight güvenli bir şekilde küme küçülten. HDFS ölçeklendirme işlemi sırasında güvenli moduna geçer ve ölçeklendirme tamamlandıktan sonra gelen beklenir. Bazı durumlarda, ancak HDFS güvenli modda bir ölçeklendirme işlemi sırasında dosya blok eksik çoğaltma nedeniyle takılı.

Varsayılan olarak, HDFS ile yapılandırılmış bir `dfs.replication` her dosya engelleme kaç kopyasının kullanılabilir denetimleri 3 / ayarlama. Dosya Engelleme her kopyası, farklı bir küme düğümünde depolanır.

HDFS blok kopya sayısı beklenen kullanılamayan algıladığında, HDFS güvenli moduna girer ve Ambari uyarılar oluşturur. HDFS bir ölçeklendirme işlemi için güvenli moda girer, ancak çoğaltma için gerekli düğüm sayısını algılanmayan olduğundan güvenli mod ardından çıkamayacağı kümeye güvenli modda takılı duruma.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Güvenli modda açıldığında örnek hataları

```
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```
org.apache.http.conn.HttpHostConnectException: Connect to hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

Adı düğüm günlüklerini gözden geçirebilirsiniz `/var/log/hadoop/hdfs/` yakın bir zamanda ne zaman küme ölçeği, ne zaman güvenli mod girdiği görmek için bir klasör. Günlük dosyaları adlı `Hadoop-hdfs-namenode-hn0-clustername.*`.

Önceki hataların temel nedenini Hive HDFS geçici dosyaları sorgular çalıştırılırken bağlı olduğunu ' dir. HDFS güvenli mod girdiğinde, HDFS'ye yazamaz Hive sorguları çalıştırılamıyor. HDFS geçici dosyaların yerel diske Vm'leri ayrı ayrı çalışan düğümüne bağlanmış ve diğer çalışan düğümlerinde en az üç kopyaya arasında çoğaltılan bulunur.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Güvenli modda takılı HDInsight engelleme

Güvenli modda sol HDInsight önlemek için birkaç yol vardır:

* HDInsight ölçeklendirmeden önce tüm Hive işleri durdurur. Alternatif olarak, ölçeği azaltma işlemi Hive işleri çalıştırmayı çakışmasını önlemek için zamanlayın.
* Hive'nın sıfırdan el ile temizlemek `tmp` ölçeği önce HDFS dizin dosyaları.
* Yalnızca üç çalışan düğümü için en düşük HDInsight ölçeklendirin. Bir alt düğüm olarak düşük kullanmaya özen gösterin.
* Gerekirse güvenli modundan ayrılmak için komutu çalıştırın.

Aşağıdaki bölümlerde, bu seçenekler açıklanmaktadır.

#### <a name="stop-all-hive-jobs"></a>Tüm Hive işlerini durdurma

Bir çalışan düğümüne ölçeği önce tüm Hive işleri durdurur. Ardından iş yükünüz zamanlanırsa, Hive iş tamamlandıktan sonra ölçeği yürütün.

Ölçeklendirmeden önce Hive işleri durdurma yardımcı tmp klasörüne karalama dosya sayısı (varsa) en aza indirin.

#### <a name="manually-clean-up-hives-scratch-files"></a>El ile Hive'nın karalama dosyaları Temizle

Güvenli mod aşağı ölçeklendirmeden önce bu dosyaları el ile temizleyebilirsiniz ardından Hive geçici dosyalar ayrıldı kaçının.

1. Hangi konuma bakarak Hive geçici dosyalar için kullanılıyor denetleyin `hive.exec.scratchdir` yapılandırma özellik. İçinde bu parametreyi ayarlayın `/etc/hive/conf/hive-site.xml`:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Hive hizmetlerini durdurmak ve tüm sorgular ve işleri tamamlandı emin olun.
2. Yukarıda bulunan karalama dizininin içeriğini listelemek `hdfs://mycluster/tmp/hive/` dosyalar içerip içermediğini görmek için:

    ```bash
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Dosyaları mevcut olduğunda örnek çıktı şöyledir:

    ```output
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

    ```bash
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>Üç veya daha fazla çalışan düğümleri için ölçek HDInsight

Ardından kümelerinizi güvenli modda sık Üçten az çalışan düğümü aşağı ölçeklendirme takılabilir ve önceki adımlar işe yaramazsa, kümenizi güvenli moda tamamen en az üç çalışan düğümü tutarak giderek önleyebilirsiniz.

Yalnızca bir çalışan düğümüne ölçeği daha yüksek maliyetli üç çalışan düğümü korunuyor, ancak kümenizi güvenli modda takılı engeller.

#### <a name="run-the-command-to-leave-safe-mode"></a>Güvenli modundan ayrılmak için komutu çalıştırın

Son seçenek bırakın güvenli mod bağlamını sağlamaktır. Güvenli mod girme HDFS nedeni Hive dosya eksik çoğaltma nedeniyle olduğunu biliyorsanız, güvenli modundan ayrılmak için aşağıdaki komutu çalıştırabilirsiniz:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Bir Apache HBase kümesi ölçeklendirin

Bölge sunucuları, bir ölçeklendirme işlemi tamamlandıktan sonra birkaç dakika içinde otomatik olarak dengelenir. Bölge sunucuları el ile dengelemek için aşağıdaki adımları tamamlayın:

1. HDInsight kümesine SSH kullanarak bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. HBase kabuğunu başlatın:

    ```bash
    hbase shell
    ```

3. Bölge sunucuları el ile dengelemek için aşağıdaki komutu kullanın:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight kümeleri otomatik olarak ölçeklendirme](hdinsight-autoscale-clusters.md)
* [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md)
