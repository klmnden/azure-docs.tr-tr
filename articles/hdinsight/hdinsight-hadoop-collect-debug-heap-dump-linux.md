---
title: HDInsight - Azure üzerinde Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme
description: Linux tabanlı HDInsight kümelerinin Apache Hadoop Hizmetleri için hata ayıklama ve analiz için yığın dökümlerini etkinleştirin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: a1b816656e019a214e8c0dc72b79575c49d99e68
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098363"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>Linux tabanlı HDInsight üzerinde Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Yığın dökümlerini değişkenlerin değerleri döküm oluşturulduğu zaman dahil olmak üzere, uygulamanın bellek anlık görüntüsünü içerir. Bu nedenle bunlar çalıştırma zamanında gerçekleşen sorunları tanılamak için kullanışlıdır.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, yalnızca Linux kullanan HDInsight kümeleri ile çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whichServices"></a>Hizmetleri

Aşağıdaki hizmetleri için yığın dökümlerini etkinleştirebilirsiniz:

* **Apache hcatalog** -tempelton
* **Apache hive** -hiveserver2, meta veri deposu, derbyserver
* **mapreduce** -jobhistoryserver
* **Apache yarn** -resourcemanager, nodemanager, timelineserver
* **Apache hdfs** -datanode, secondarynamenode, namenode

Ayrıca eşleme için yığın dökümlerini etkinleştirme ve azaltma işlemlerini HDInsight tarafından çalıştırıldı.

## <a name="configuration"></a>Yığın Dökümü yapılandırma anlama

Yığın dökümlerini seçenekleri geçirerek etkinleştirilir (bazen bölgedeyse olarak bilinen veya parametreleri) bir hizmet başlatıldığında JVM için. Çoğu [Apache Hadoop](https://hadoop.apache.org/) Hizmetleri, bu seçenekler geçirilecek hizmetini başlatmak için kullanılan bir kabuk betiği değiştirebilirsiniz.

Her komut dosyası için bir dışarı aktarma yok  **\* \_OPTS**, JVM için geçirilen seçenekleri içerir. Örneğin, **hadoop env.sh** ile başlayan satırı betik `export HADOOP_NAMENODE_OPTS=` NameNode hizmeti için seçenekleri içerir.

Eşleme ve azaltma işlemlerini MapReduce service'nın bir alt işlemi olarak işlemleri biraz farklı. Her harita veya azaltma işlemi alt kapsayıcıda çalıştırılan ve JVM seçenekleri içeren iki giriş vardır. Hem bulunan **mapred-site.xml**:

* **mapreduce.admin.map.child.java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [!NOTE]  
> Kullanmanızı öneririz [Apache Ambari](https://ambari.apache.org/) betikleri ve mapred-site.xml ayarlarını değiştirmek için değişiklikleri kümedeki düğümler arasında çoğaltmayı Ambari işleme. Bkz: [kullanarak Apache Ambari](#using-apache-ambari) bölümde belirli adımlar için.

### <a name="enable-heap-dumps"></a>Yığın dökümlerini etkinleştirme

Bir OutOfMemoryError oluştuğunda yığın dökümlerini aşağıdaki iki seçenek sağlar:

    -XX:+HeapDumpOnOutOfMemoryError

**+** Bu seçeneği etkin olduğunu gösterir. Varsayılan olarak devre dışı seçeneği kullanılır.

> [!WARNING]  
> Döküm dosyaları büyük olması gibi yığın dökümlerini HDInsight Hadoop Hizmetleri için varsayılan olarak etkin değildir. Sorunu yeniden ve döküm dosyaları toplanan sonra bunları devre dışı bırakmak sorun giderme için etkinleştirdiğinizde ise unutmayın.

### <a name="dump-location"></a>Döküm konumu

Geçerli çalışma dizini döküm dosyasının varsayılan konumdur. Denetleyebileceğiniz aşağıdaki seçeneğini kullanarak dosyanın depolandığı:

    -XX:HeapDumpPath=/path

Örneğin, kullanarak  `-XX:HeapDumpPath=/tmp` /tmp dizininde depolanması dökümleri neden olur.

### <a name="scripts"></a>Betikler

Bir komut dosyası ayrıca tetikleyebilirsiniz olduğunda bir **OutOfMemoryError** gerçekleşir. Örneğin, hata oluştuğunu bilmek için bir bildirim tetikleniyor. Aşağıdaki seçeneği tetiklenecek bir betik kullanın bir __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]  
> Apache Hadoop dağıtılmış bir sistemde olduğundan, hizmetin üzerinde çalıştığı kümedeki tüm düğümlerde kullanılan herhangi bir betik yerleştirilmelidir.
> 
> Betik gerekir ayrıca olması, hizmet olarak çalışır ve sağlamalısınız hesabı tarafından erişilebilen bir konumda Yürütme izinleri. Örneğin, komut dosyalarında depolamak isteyebilirsiniz `/usr/local/bin` ve `chmod go+rx /usr/local/bin/filename.sh` vermek okuma ve Yürütme izinleri.

## <a name="using-apache-ambari"></a>Apache Ambari kullanarak

Bir hizmetin yapılandırmasını değiştirmek için aşağıdaki adımları kullanın:

1. Kümeniz için Ambari web kullanıcı arabirimini açın. URL https://YOURCLUSTERNAME.azurehdinsight.net.

    İstendiğinde, HTTP hesap adını kullanarak siteye kimlik doğrulaması (varsayılan: Yönetici) ve kümeniz için parola.

   > [!NOTE]  
   > İkinci kez Ambari tarafından kullanıcı adı ve parola istenebilir. Bu durumda, aynı hesabı adını ve parolasını girin.

2. Sol taraftaki listesini kullanarak değiştirmek istediğiniz bir hizmet alanı seçin. Örneğin, **HDFS**. Merkezi alanında seçin **yapılandırmaları** sekmesi.

    ![Ambari web HDFS yapılandırmalar sekmesi seçili görüntüsü](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Kullanarak **Filtresi...**  girişi girin **bölgedeyse**. Yalnızca bu metni içeren öğeler görüntülenir.

    ![Filtrelenmiş liste](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Bulma  **\* \_OPTS** hizmeti için giriş istediğiniz için yığın dökümlerini etkinleştirme ve etkinleştirmek istediğiniz seçenekleri ekleyin. Aşağıdaki görüntüde ekledim `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` için **HADOOP\_NAMENODE\_OPTS** girişi:

    ![-XX ile HADOOP_NAMENODE_OPTS: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]  
   > Ne zaman otomatik olarak yığın etkinleştirme veya dökümleri için harita alt işlem, arama için adlandırılmış alanları azaltmak **mapreduce.admin.map.child.java.opts** ve **mapreduce.admin.reduce.child.java.opts**.

    Kullanım **Kaydet** değişiklikleri kaydetmek için düğme. Değişiklikleri açıklayan kısa bir not girebilirsiniz.

5. Değişiklikler uygulandıktan sonra **yeniden başlatma gerekli** simge bir veya daha fazla hizmet görünür.

    ![gerekli simge ve düğmeyi yeniden başlatın](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Yeniden başlatma gerektiren her bir hizmet seçin ve **hizmet eylemleri** düğmesi **bakım modunu aç**. Bakım modu, uyarılar yeniden başlatıldığında hizmetten oluşturulmasını engeller.

    ![Bakım modu menüsünü aç](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. Bakım modu etkinleştirildikten sonra kullanın **yeniden** hizmete düğmesi **yeniden tüm etkileniyor**

    ![Etkilenen tüm giriş yeniden başlatın](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]  
   > girişleri **yeniden** düğmesi için diğer hizmetleri farklı olabilir.

8. Hizmetleri yeniden başlattıktan sonra kullanmak **hizmet eylemleri** düğmesi **bakım modunu Kapat Kapat**. Uyarılar için hizmet için izlemeyi sürdürmek için bu Ambari.

