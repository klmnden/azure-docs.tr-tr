---
title: "Yığın dökümleri hdınsight'ta - Azure Hadoop Hizmetleri için etkinleştirme | Microsoft Docs"
description: "Hata ayıklama ve çözümleme için Hadoop Linux tabanlı Hdınsight kümeleri Hizmetleri'nden yığın dökümleri etkinleştirin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8f151adb-f687-41e4-aca0-82b551953725
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: dcc04e5bba28d0cb32e8633542ab8d3c125003ec
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a>Linux tabanlı hdınsight'ta Hadoop Hizmetleri için yığın dökümleri etkinleştir

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Yığın dökümleri döküm oluşturulduğunda değişkenlerin değerleri de dahil olmak üzere uygulamanın belleğin anlık görüntüsünü içerir. Bu nedenle bunlar çalışma zamanında oluşan sorunları tanılamak için kullanışlıdır.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Linux kullanmak Hdınsight kümeleri ile çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whichServices"></a>Hizmetleri

Aşağıdaki hizmetler için yığın dökümleri etkinleştirebilirsiniz:

* **hcatalog** -tempelton
* **Hive** -hiveserver2, meta depo, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, iş

Ayrıca yığın dökümleri harita etkinleştirmek ve azaltmak işlemleri Hdınsight tarafından çalıştırılan.

## <a name="configuration"></a>Anlama yığın dökümü yapılandırma

Yığın dökümleri seçenekleri geçirerek etkin (bazen çevrilir olarak bilinen veya parametrelerini) bir hizmet başlatıldığında, JVM için. Çoğu Hadoop Hizmetleri için bu seçenekleri geçirmek için hizmetini başlatmak için kullanılan Kabuk komut dosyasını değiştirebilirsiniz.

Her komut dosyasında bir dışarı aktarma için yoktur  **\* \_OPTS**, JVM geçirilen seçenekleri içerir. Örneğin, **hadoop env.sh** komut dosyası, ile başlayan satırı `export HADOOP_NAMENODE_OPTS=` iş hizmeti için seçenekleri içerir.

Eşleme ve azaltma MapReduce hizmeti bir alt işlemi bu işlemleri olduğu gibi işlemleri biraz farklı. Her eşleme ya da azaltmak işlem bir alt kapsayıcıda çalışır ve JVM seçenekleri içeren iki girdi vardır. İçinde yer alan her ikisi de **mapred-site.xml**:

* **mapreduce.Admin.Map.Child.Java.opts**
* **mapreduce.Admin.reduce.Child.Java.opts**

> [!NOTE]
> Komut dosyaları ve mapred-site.xml ayarları, kümedeki düğümler arasında değişiklikleri çoğaltmaya Ambari işleyici olarak değiştirmek için Ambari kullanmanızı öneririz. Bkz: [Ambari kullanarak](#using-ambari) belirli adımlar için bölüm.

### <a name="enable-heap-dumps"></a>Yığın dökümlerini etkinleştirme

Bir OutOfMemoryError oluştuğunda aşağıdaki yığın dökümleri sağlar:

    -XX:+HeapDumpOnOutOfMemoryError

 **+**  Bu seçeneği etkin olduğunu gösterir. Varsayılan devre dışıdır.

> [!WARNING]
> Döküm dosyaları büyük olabileceğinden yığın dökümleri hdınsight'ta Hadoop Hizmetleri için varsayılan olarak etkin değildir. Sorunu yeniden ve döküm dosyaları toplanan sonra bunları devre dışı bırakmak, bunları gidermek için etkinleştirirseniz, unutmayın.

### <a name="dump-location"></a>Konum dökümü

Geçerli çalışma dizini döküm dosyası için varsayılan konumdur. Kontrol edebilirsiniz aşağıdaki seçeneğini kullanarak dosyayı depolanan burada:

    -XX:HeapDumpPath=/path

Örneğin, kullanarak `-XX:HeapDumpPath=/tmp` tmp dizininde depolanması dökümleri neden olur.

### <a name="scripts"></a>Betikler

Bir komut dosyası ayrıca tetikleyebilir olduğunda bir **OutOfMemoryError** oluşur. Örneğin, hata oluştuğunu bilmesi bir bildirim tetikleniyor. Bir komut dosyası üzerinde tetiklemek için aşağıdaki seçeneğini kullanın bir __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> Hadoop dağıtılmış bir sistemde olduğundan, hizmetin çalıştığı kümedeki tüm düğümlerde kullanılan herhangi bir komut dosyası yerleştirilmelidir.
> 
> Betik gerekir ayrıca olması hizmet olarak çalışır ve sağlamalısınız hesabı tarafından erişilebilen bir konumda Yürütme izinleri. Örneğin, komut dosyalarında depolamak isteyebilir `/usr/local/bin` ve `chmod go+rx /usr/local/bin/filename.sh` okuma izni ve izinleri yürütün.

## <a name="using-ambari"></a>Ambari kullanarak

Bir hizmetin yapılandırmasını değiştirmek için aşağıdaki adımları kullanın:

1. Kümeniz için Ambari web kullanıcı arabirimini açın. Https://YOURCLUSTERNAME.azurehdinsight.net URL'dir.

    İstendiğinde, HTTP hesap adını kullanarak site kimlik doğrulaması (varsayılan: Yönetici) ve parola kümeniz için.

   > [!NOTE]
   > İkinci kez tarafından Ambari kullanıcı adı ve parola istenebilir. Bu durumda, aynı hesap adı ve parola girin

2. Sol tarafta listesini kullanarak değiştirmek istediğiniz hizmet alanı seçin. Örneğin, **HDFS**. Orta bölmesinde seçin **yapılandırmalar** sekmesi.

    ![Ambari web HDFS yapılandırmalar sekmesi seçili görüntüsü](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Kullanarak **filtre...**  girişi girin **çevrilir**. Yalnızca bu metni içeren öğeleri görüntülenir.

    ![Filtrelenmiş liste](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Bul  **\* \_OPTS** hizmeti için giriş istediğiniz için yığın dökümleri etkinleştirmek ve etkinleştirmek istediğiniz seçenekleri ekleyin. Aşağıdaki görüntüde, ekledim `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` için **HADOOP\_iş\_OPTS** girişi:

    ![-XX ile HADOOP_NAMENODE_OPTS: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > Ne zaman yığın etkinleştirme dökümleri harita için veya alt işlem arama adlı alanları azaltın **mapreduce.admin.map.child.java.opts** ve **mapreduce.admin.reduce.child.java.opts**.

    Kullanım **kaydetmek** değişiklikleri kaydetmek için düğmesi. Değişiklikleri açıklayan kısa bir not girebilirsiniz.

5. Değişiklikler uygulandıktan sonra **yeniden başlatma gerekli** simge bir veya daha fazla hizmet görüntülenir.

    ![gerekli simgesini ve düğmesi yeniden başlatın](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Yeniden başlatma gerektiren her hizmet seçin ve **hizmet eylemleri** düğmesine **kapatma üzerinde Bakım modu**. Bakım modu, yeniden başlattığınızda hizmetinden üretilmesini uyarıları engeller.

    ![Bakım modu menüsünde Aç](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. Bakım modu etkinleştirildiğinde, kullanın **yeniden** hizmete düğmesi **tüm parametreden yeniden başlatın**

    ![Tüm etkilenen giriş yeniden başlatın](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > girişleri **yeniden** düğmesi diğer hizmetler için farklı olabilir.

8. Hizmetleri yeniden başlattıktan sonra kullanmak **hizmet eylemleri** düğmesine **Bakım modu devre dışı bırakma**. Hizmeti için uyarılar için izlemeyi sürdürmek için bu ambarı.

