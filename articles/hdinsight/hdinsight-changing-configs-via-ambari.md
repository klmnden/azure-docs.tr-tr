---
title: Apache Ambari - Azure HDInsight ile küme yapılandırmalarını en iyi duruma getirme
description: Yapılandırma ve HDInsight kümeleri en iyi duruma getirmek için Apache Ambari web kullanıcı arabirimini kullanın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: hrasheed
ms.openlocfilehash: f0db36fa380d0d1bb7f2b581c4bf8fa1abfaadaf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60698989"
---
# <a name="use-apache-ambari-to-optimize-hdinsight-cluster-configurations"></a>HDInsight küme yapılandırmalarını en iyi duruma getirmek için Apache Ambari kullanın

HDInsight sağlar [Apache Hadoop](https://hadoop.apache.org/) kümeleri büyük ölçekli veri işleme uygulamaları için. Bu karmaşık çok düğümlü küme en iyi duruma getirme yönetme ve izleme zor olabilir. [Apache Ambari](https://ambari.apache.org/) yönetmek ve HDInsight Linux kümeleri izlemek için bir web arabirimidir.  Windows kümeleri için kullanan [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).

Ambari Web kullanıcı arabirimini kullanarak bir giriş için bkz. [yönetme HDInsight kümeleri Apache Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md)

Oturum açın Ambari `https://CLUSTERNAME.azurehdidnsight.net` küme kimlik bilgilerinizle. Bir genel bakış panosunun ilk ekran görüntüler.

![Ambari Panosu](./media/hdinsight-changing-configs-via-ambari/ambari-dashboard.png)

Ambari web kullanıcı Arabirimi, konaklar, hizmetleri, uyarılar, yapılandırmaları ve görünümleri yönetmek için kullanılabilir. Ambari, bir HDInsight kümesi oluşturma, hizmetleri yükseltme, yığın ve sürümlerini yönetmek, yetkisini veya konakları recommission veya hizmetleri kümeye eklemek için kullanılamaz.

## <a name="manage-your-clusters-configuration"></a>Kümenizin yapılandırmasını yönetme

Yapılandırma ayarları belirli bir hizmet ayarlamanıza yardımcı olur. Bir hizmetin yapılandırma ayarlarını değiştirmek için hizmetten seçin **Hizmetleri** (solda), kenar gidin **yapılandırmaları** sekmede hizmet ayrıntı sayfası.

![Kenar Hizmetleri](./media/hdinsight-changing-configs-via-ambari/services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>NameNode Java yığın boyutu değiştirme

NameNode Java yığın boyutu, küme, dosyaların sayısı ve blokları sayıda yükü gibi birçok faktöre bağlıdır. Bazı iş yükleri fazla veya az bellek gerekmesine rağmen varsayılan boyutu 1 GB de çoğu kümede ile çalışır. 

NameNode Java yığın boyutu değiştirmek için:

1. Seçin **HDFS** Hizmetleri kenar gidin **yapılandırmaları** sekmesi.

    ![HDFS yapılandırma](./media/hdinsight-changing-configs-via-ambari/hdfs-config.png)

1. Ayar Bul **NameNode Java yığın boyutu**. Ayrıca **filtre** yazın ve belirli bir ayarı bulmak için metin kutusu. Seçin **kalem** ayarı adının yanındaki simge.

    ![NameNode Java yığın boyutu](./media/hdinsight-changing-configs-via-ambari/java-heap-size.png)

1. Metin kutusuna yeni bir değer yazın ve tuşuna **Enter** yapılan değişiklik kaydedilemiyor.

    ![NameNode Java yığın boyutu Düzenle](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit.png)

1. NameNode Java yığın boyutu 1 GB-2 GB ile değiştirilir.

    ![NameNode Java yığın boyutu düzenlendi](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

1. Yeşil tıklayarak yaptığınız değişiklikleri kaydetmek **Kaydet** yapılandırma ekranın üst kısmındaki düğmesi.

    ![Değişiklikleri kaydet](./media/hdinsight-changing-configs-via-ambari/save-changes.png)

## <a name="apache-hive-optimization"></a>Apache Hive en iyi duruma getirme

Aşağıdaki bölümlerde, Apache Hive genel performansını iyileştirmek için yapılandırma seçenekleri açıklanmaktadır.

1. Hive yapılandırma parametreleri değiştirmek için seçin **Hive** Hizmetleri kenar.
1. Gidin **yapılandırmaları** sekmesi.

### <a name="set-the-hive-execution-engine"></a>Hive yürütme altyapısı ayarlama

Hive iki yürütme altyapısı sağlar: [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) ve [Apache TEZ](https://tez.apache.org/). Tez MapReduce hızlıdır. HDInsight Linux kümeleri varsayılan yürütme altyapısı Tez sahiptir. Yürütme altyapısı değiştirmek için:

1. Hive'da **yapılandırmaları** yazın **yürütme altyapısı** filtre kutusuna.

    ![Arama yürütme altyapısı](./media/hdinsight-changing-configs-via-ambari/search-execution.png)

1. **İyileştirme** özelliğin varsayılan değeri **Tez**.

    ![En iyi duruma getirme - Tez](./media/hdinsight-changing-configs-via-ambari/optimization-tez.png)

### <a name="tune-mappers"></a>Azaltıcının ayarlama

Hadoop bölme dener (*harita*) paralel olarak birden fazla dosya ve elde edilen işlem tek bir dosyaya dosyaları. Azaltıcının bölmelerini sayısına bağlıdır. Aşağıdaki iki yapılandırma parametrelerini bölmelerini Tez yürütme altyapısı için sayısını sürücü:

* `tez.grouping.min-size`: Alt sınır gruplandırılmış bölme boyutu 16 MB (16,777,216 bayt), varsayılan değeri.
* `tez.grouping.max-size`: Gruplandırılmış bölme boyutu 1 GB (1.073.741.824 bayt) varsayılan bir değerle üst sınırı.

Bir performans kuralı karşısında, gecikme süresini iyileştirmek için daha fazla üretilen iş hacmini artırmak için bu parametrelerin her ikisi de azaltır.

Örneğin, dört Eşleyici görevleri için 128 MB veri boyutunu ayarlamak için her iki parametre için 32 MB her (33,554,432 bayt) ayarlamalısınız.

1. Sınırı parametreleri değiştirmek için gidin **yapılandırmaları** Tez hizmetinin sekmesi. Genişletin **genel** paneli ve bulun `tez.grouping.max-size` ve `tez.grouping.min-size` parametreleri.

1. Her iki parametre kümesine **33,554,432** bayt (32 MB).

    ![Tez gruplandırma boyutları](./media/hdinsight-changing-configs-via-ambari/tez-grouping-size.png)
 
Bu değişiklikler, sunucunun tamamında tüm Tez işlerinin etkiler. Bir en iyi sonucu almak için uygun parametre değerlerini seçin.

### <a name="tune-reducers"></a>Ayarlama genişletin

[Apache ORC](https://orc.apache.org/) ve [Snappy](https://google.github.io/snappy/) hem de yüksek performans sunar. Ancak, Hive performans sorunlarını neden çok az sayıda genişletin varsayılan olarak, olabilir.

Örneğin, bir giriş veri boyutu 50 GB olduğunu varsayalım. Verileri ORC Snappy sıkıştırmasıyla biçimlendirme 1 GB'tır. Hive tahminleri olarak gerekli genişletin sayısı: (azaltıcının giriş bayt sayısı / `hive.exec.reducers.bytes.per.reducer`).

Varsayılan ayarlarla bu 4 genişletin örneğidir.

`hive.exec.reducers.bytes.per.reducer` Parametresi Azaltıcı işlenen bayt sayısını belirtir. Varsayılan değer 64 MB'dir. Bu değer aşağı ayarlama paralellik artırır ve performansı artırabilir. Çok düşük ayarlama performansını olumsuz olabilecek çok sayıda genişletin üretebilir. Bu parametre, belirli veri gereksinimlerinizi, sıkıştırma ayarlarını ve diğer çevresel etmenler temel alır.

1. Parametreyi değiştirmek için Hive için gidin **yapılandırmaları** sekmesini ve bulma **Azaltıcı başına veri** Ayarları sayfasında parametresi.

    ![Azaltıcı başına veri](./media/hdinsight-changing-configs-via-ambari/data-per-reducer.png)
 
1. Seçin **Düzenle** 128 MB (134,217,728 bayt) değerine değiştirin ve ENTER tuşuna basın **Enter** kaydetmek için.

    ![Düzenlenen Azaltıcı - başına veri](./media/hdinsight-changing-configs-via-ambari/data-per-reducer-edited.png)
  
    Bir giriş veri Azaltıcı, başına 128 MB ile 1024 MB boyutunu verilen vardır 8 genişletin (1024/128).

1. İçin yanlış bir değere **Azaltıcı başına veri** parametre çok sayıda sorgu performansını olumsuz yönde etkileyen genişletin, neden olabilir. Genişletin sayısı üst sınırını ayarlayın `hive.exec.reducers.max` uygun değeri. 1009 varsayılan değerdir.

### <a name="enable-parallel-execution"></a>Paralel yürütme etkinleştir

Bir Hive sorgusu, bir veya daha fazla aşamalı olarak yürütülür. Bağımsız aşamaları paralel olarak çalıştırılabilir ise, sorgu performansını artırır.

1.  Paralel sorgu yürütme sağlamak Hive gidin **Config** sekmesinde ve arama `hive.exec.parallel` özelliği. Varsayılan değer false'tur. Değeri TRUE ve ENTER tuşuna basın değiştirmek **Enter** değeri kaydetmek için.
 
1.  Paralel olarak çalıştırmak için iş sayısını sınırlamak için değiştirme `hive.exec.parallel.thread.number` özelliği. Varsayılan değer 8'dir.

    ![Exec paralel hive](./media/hdinsight-changing-configs-via-ambari/hive-exec-parallel.png)


### <a name="enable-vectorization"></a>Vektörleştirmeyi etkinleştirin

Hive, veriler satır temelinde işler. Vektörleştirme Hive aynı anda bloklar içeren bir satır yerine 1.024 satır verileri işlemek için yönlendirir. Vektörleştirme yalnızca ORC dosya biçimi için geçerlidir.

1. Bir vektör haline getirilmiş sorgu yürütmesini sağlamak Hive gidin **yapılandırmaları** sekmesinde ve arama `hive.vectorized.execution.enabled` parametresi. Varsayılan değer, Hive 0.13.0 için true ya da üzeri.
 
1. Sorgu azaltma tarafı için vektör haline getirilmiş yürütülmesini etkinleştirmek için `hive.vectorized.execution.reduce.enabled` parametresi true. Varsayılan değer false'tur.

    ![Vektörleştirildi hive yürütme](./media/hdinsight-changing-configs-via-ambari/hive-vectorized-execution.png)

### <a name="enable-cost-based-optimization-cbo"></a>Maliyet tabanlı iyileştirme (BHA) etkinleştir

Varsayılan olarak, Hive, bir en iyi sorgu yürütme planı bulmak için kurallar kümesini izler. Maliyet tabanlı iyileştirme (BHA) bir sorgu yürütmek için birden çok planları değerlendirir ve maliyet her plana atar ve ardından bir sorgu yürütmek için ucuz planı belirler.

BHA sağlamak Hive gidin **yapılandırmaları** sekmesinde ve arama `parameter hive.cbo.enable`, ardından iki durumlu düğmeyi geçiş **üzerinde**.

![BHA yapılandırma](./media/hdinsight-changing-configs-via-ambari/cbo.png)

BHA etkinleştirildiğinde aşağıdaki ek yapılandırma parametrelerini Hive sorgu performansı artırın:

* `hive.compute.query.using.stats`

    TRUE olarak Hive kullandığında, meta veri deposu içinde depolanan istatistikleri gibi basit sorgulara yanıt vermek için `count(*)`.

    ![BHA istatistikleri](./media/hdinsight-changing-configs-via-ambari/hive-compute-query-using-stats.png)

* `hive.stats.fetch.column.stats`

    Sütun istatistikleri BHA etkin olduğunda oluşturulur. Hive meta veri deposu, sorguların iyileştirilmesine saklanan sütun istatistikleri kullanır. Sütun sayısı yüksek olduğunda her sütun için sütun istatistikleri alma daha uzun sürer. False olarak ayarlandığında, bu ayar meta veri deposu getirilirken sütun istatistikleri devre dışı bırakır.

    ![Hive istatistikleri set sütun istatistikleri](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-column-stats.png)

* `hive.stats.fetch.partition.stats`

    Temel bölümü istatistikleri satır, veri boyutu ve dosya boyutu sayısı gibi meta veri deposu depolanır. Ne zaman true olarak istatistikleri meta veri deposu getirilen bölüm ayarlayın. Yanlış olduğunda, dosya boyutu, dosya sisteminden alınan ve satır şemadan getirilen satır sayısı.

    ![Hive istatistikleri set bölüm istatistikleri](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-partition-stats.png)

### <a name="enable-intermediate-compression"></a>Ara sıkıştırmayı etkinleştir

Harita görevleri Azaltıcı görevler tarafından kullanılan ara dosya oluşturun. Ara sıkıştırma Ara dosya boyutunu küçültür.

Hadoop işleri genellikle g/ç performansı düşürdüğünü gösterir. Veri sıkıştırma, g/ç ve genel ağ aktarımı ' hızlandırabilirsiniz.

Kullanılabilir sıkıştırma türleri şunlardır:

| Biçimlendir | Tool | Algoritma | Dosya uzantısı | Bölünebilir mi? |
| -- | -- | -- | -- | -- |
| Gzip | Gzip | SÖNDÜR | .gz | Hayır |
| Bzip2 | Bzip2 | Bzip2 |.bz2 | Evet |
| LZO | Lzop | LZO | .lzo | Evet, dizini varsa |
| Snappy | Yok | Snappy | Snappy | Hayır |

Genel bir kural olarak bölümlenebilir sıkıştırma yöntemi olması önemlidir, aksi takdirde çok az sayıda azaltıcının oluşturulur. Giriş verilerini metin ise `bzip2` en iyi seçenektir. ORC biçimi, Snappy hızlı sıkıştırma seçeneği değil.

1. Hive için Ara sıkıştırmayı etkinleştir gidin **yapılandırmaları** sekmesine ve ardından `hive.exec.compress.intermediate` parametresi true. Varsayılan değer false'tur.

    ![Hive exec sıkıştırma Ara](./media/hdinsight-changing-configs-via-ambari/hive-exec-compress-intermediate.png)

    > [!NOTE]  
    > Ara dosyaları sıkıştırmak için yüksek oranda sıkıştırma çıkış codec almasa bile maliyet, daha düşük CPU ile sıkıştırma codec bileşeni seçin.

1. Ara sıkıştırma codec ayarlamak için özel özellik ekleme `mapred.map.output.compression.codec` için `hive-site.xml` veya `mapred-site.xml` dosya.

1. Özel bir ayarı eklemek için:

    a. Gezinme yığınına **yapılandırmaları** sekmenize **Gelişmiş** sekmesi.

    b. Altında **Gelişmiş** sekmesinde bulun ve genişletin **özel hive-site** bölmesi.

    c. Bağlantıya tıklayın **Özellik Ekle** özel hive site bölmesinin alt kısmındaki.

    d. Özellik Ekle penceresinde girin `mapred.map.output.compression.codec` anahtar olarak ve `org.apache.hadoop.io.compress.SnappyCodec` değeri.

    e. **Ekle**'ye tıklayın.

    ![Hive özel özellik](./media/hdinsight-changing-configs-via-ambari/hive-custom-property.png)

    Bu, Snappy sıkıştırma kullanarak ara dosyası sıkıştırır. Özellik eklendikten sonra özel hive site bölmesinde görünür.

    > [!NOTE]  
    > Bu yordamı değiştirir `$HADOOP_HOME/conf/hive-site.xml` dosya.

### <a name="compress-final-output"></a>Son çıkış Sıkıştır

Son Yığın çıktısı da birleştirilebilir.

1. Hive için son Hive çıktı Sıkıştır gidin **yapılandırmaları** sekmesine ve ardından `hive.exec.compress.output` parametresi true. Varsayılan değer false'tur.

1. Çıkış sıkıştırma codec seçmek için Ekle `mapred.output.compression.codec` önceki bölümün adım 3'te açıklandığı gibi özel hive site bölmesine özel özellik.

    ![Hive özel özellik](./media/hdinsight-changing-configs-via-ambari/hive-custom-property2.png)

### <a name="enable-speculative-execution"></a>Kurgusal yürütmeyi etkinleştir

Kurgusal yürütmeyi algılar ve görev sonuçları iyileştirerek toplam iş yürütme artırırken yavaş çalışan görev İzleyici kara için yinelenen görevleri belirli bir sayıda başlatılır.

Giriş büyük miktarlarda ile uzun süreli MapReduce görevleri için kurgusal yürütmeyi açık olması gerekir.

* Kurgusal yürütmeyi sağlamak için Hive gidin **yapılandırmaları** sekmesine ve ardından `hive.mapred.reduce.tasks.speculative.execution` parametresi true. Varsayılan değer false'tur.

    ![Hive mapred görevleri kurgusal yürütmeyi azaltma](./media/hdinsight-changing-configs-via-ambari/hive-mapred-reduce-tasks-speculative-execution.png)

### <a name="tune-dynamic-partitions"></a>Dinamik bölümleri ayarlama

Hive kayıt her bölüm önceden tanımlanması olmadan bir tabloya eklenirken dinamik bölümleri oluşturmak için sağlar. Çok sayıda bölümleri ve çok sayıda her bölüm için dosyaların oluşturulmasıyla sonuçlanabilir olsalar da güçlü bir özellik budur.

1. Dinamik bölümleri yapmak Hive için `hive.exec.dynamic.partition` parametre değeri true olmalıdır (varsayılan).

1. Dinamik bölüm moduna *katı*. Katı modda, en az bir bölüm statik olması gerekir. Bu, diğer bir deyişle, sorguları WHERE yan tümcesinde bir bölüm filtresi olmadan engeller *katı* tüm bölümleri taraması sorguları engeller. Gezinme yığınına **yapılandırmaları** sekmesine ve ardından `hive.exec.dynamic.partition.mode` için **katı**. Varsayılan değer **Gevþek**.
 
1. Oluşturulacak dinamik bölümlerin sayısını sınırlamak için değiştirmeniz `hive.exec.max.dynamic.partitions` parametresi. Varsayılan değer 5000'dir.
 
1. Düğüm başına dinamik bölümleri toplam sayısını sınırlamak için değiştirme `hive.exec.max.dynamic.partitions.pernode`. Varsayılan değer 2000'dir.

### <a name="enable-local-mode"></a>Yerel modunu etkinleştir

Yerel mod Hive tek bir makinede veya bazı durumlarda tek bir işlemde bir işin tüm görevleri gerçekleştirmenize olanak sağlar. Giriş verilerini küçük ise ve sorgular için bir görevi başlatmadan yükü genel sorgu yürütme önemli oranda tüketir sorgu performansını artırır.

Yerel modunu etkinleştirmek için ekleme `hive.exec.mode.local.auto` parametre 3. adımında açıklandığı gibi özel hive site paneline [Ara sıkıştırmayı etkinleştir](#enable-intermediate-compression) bölümü.

![Hive yürütme modu yerel otomatik](./media/hdinsight-changing-configs-via-ambari/hive-exec-mode-local-auto.png)

### <a name="set-single-mapreduce-multigroup-by"></a>Kümesi tek MapReduce MultiGROUP tarafından

Bu özellik True olarak MultiGROUP tarafından bir sorgu grubu tarafından ortak anahtarları ile ayarlandığında, tek bir MapReduce işi oluşturur.  

Bu davranışı etkinleştirmek için eklemeniz `hive.multigroupby.singlereducer` parametre 3. adımında açıklandığı gibi özel hive site bölmesine [Ara sıkıştırmayı etkinleştir](#enable-intermediate-compression) bölümü.

![Tek MapReduce MultiGROUP tarafından Hive'ı ayarlama](./media/hdinsight-changing-configs-via-ambari/hive-multigroupby-singlereducer.png)

### <a name="additional-hive-optimizations"></a>Ek Hive iyileştirmeler

Aşağıdaki bölümlerde ayarlayabileceğiniz ek Hive ile ilgili iyileştirmeler açıklanmaktadır.

#### <a name="join-optimizations"></a>En iyi duruma getirme katılın

Hive varsayılan birleştirme türü olan bir *shuffle birleştirme*. Hive, özel azaltıcının giriş okuyun ve join anahtar/değer çifti için bir ara dosya yayma. Hadoop, sıralar ve bu çiftler shuffle aşamasında birleştirir. Bu shuffle aşama pahalıdır. Verilerinizi temel alan sağ birleştirme seçerek performansını önemli ölçüde artırabilir.

| Katılım Türü | Zaman | Nasıl | Hive ayarları | Yorumlar |
| -- | -- | -- | -- | -- |
| Shuffle birleştirme | <ul><li>Varsayılan seçenek</li><li>Her zaman çalışır</li></ul> | <ul><li>Tablolardan birinin bölümünden okur</li><li>Demetleri ve birleştirme anahtarı sıralar</li><li>Tek bir demet her reduce'a gönderir.</li><li>Birleştirme azaltın tarafında gerçekleştirilir</li></ul> | Gerekli ayarı yok önemli Hive | Her zaman çalışır. |
| Harita birleştirme | <ul><li>Bir tablo belleğe sığması</li></ul> | <ul><li>Küçük bir tablo bellek karma tabloya okur</li><li>Akışları aracılığıyla büyük dosya</li><li>Karma tablosundaki her kayıt birleştirir</li><li>Birleştirmeler Eşleyicisi tarafından olan</li></ul> | `hive.auto.confvert.join=true` | Çok hızlı ancak sınırlı |
| Sıralama birleştirme Kovası | Her iki tablonun varsa: <ul><li>Aynı sıralandı</li><li>Aynı bucketed</li><li>Sıralı/kümelenmiş sütun birleştirme</li></ul> | Her işlem: <ul><li>Bir demet her tablosundan okur</li><li>En düşük değere sahip satırı işler</li></ul> | `hive.auto.convert.sortmerge.join=true` | Çok da verimli |

#### <a name="execution-engine-optimizations"></a>Yürütme altyapısı iyileştirmeleri

Hive yürütme altyapısı iyileştirmek için ek öneriler:

| Ayar | Önerilen | HDInsight varsayılan |
| -- | -- | -- |
| `hive.mapjoin.hybridgrace.hashtable` | TRUE = daha güvenli, daha yavaş; daha hızlı = false | false |
| `tez.am.resource.memory.mb` | Çoğu için 4 GB'lık üst sınır | Otomatik olarak ayarlanmış |
| `tez.session.am.dag.submit.timeout.secs` | 300+ | 300 |
| `tez.am.container.idle.release-timeout-min.millis` | 20000+ | 10000 |
| `tez.am.container.idle.release-timeout-max.millis` | 40000+ | 20000 |

## <a name="apache-pig-optimization"></a>Apache Pig en iyi duruma getirme

[Apache Pig](https://pig.apache.org/) Ambari web kullanıcı Arabiriminden Pig sorgularınızı ayarlamak için özellikleri değiştirilebilir. Ambari pig özelliklerinden doğrudan değiştirme değiştirir Pig özelliklerinde `/etc/pig/2.4.2.0-258.0/pig.properties` dosya.

1. Pig özelliklerini değiştirmek için Pig için gidin **yapılandırmaları** sekmesine ve ardından **Gelişmiş özellikleri pig** bölmesi.

1. Bulma, açıklamasını kaldırın ve değiştirmek istediğiniz özelliğin değerini değiştirin.

1. Seçin **Kaydet** yeni değeri kaydetmek için pencerenin sağ üst tarafındaki. Bazı özellikler, hizmeti yeniden başlatılması gerekebilir.

    ![Gelişmiş pig özellikleri](./media/hdinsight-changing-configs-via-ambari/advanced-pig-properties.png)
 
> [!NOTE]  
> Özellik değerlerinde oturum düzeyi ayarları geçersiz kılmak `pig.properties` dosya.

### <a name="tune-execution-engine"></a>Yürütme altyapısı ayarlama

İki yürütme altyapısı, Pig betikleri yürütmek kullanılabilir: MapReduce ve Tez. Tez en iyi duruma getirilmiş bir altyapısıdır ve MapReduce hızlıdır.

1. Yürütme altyapısı değiştirmek için **Gelişmiş özellikleri pig** bölmesinde özelliğini bulun `exectype`.

1. Varsayılan değer **MapReduce**. Değiştirin **Tez**.


### <a name="enable-local-mode"></a>Yerel modunu etkinleştir

Benzer şekilde Hive, yerel mod, görece daha küçük miktarda veriler içeren işleri hızlandırmak için kullanılır.

1. Yerel mod etkinleştirmek için `pig.auto.local.enabled` için **true**. Varsayılan değer false'tur.

1. Bir giriş veri boyutu olan işleri küçüktür `pig.auto.local.input.maxbytes` özellik değeri, küçük işleri olacak şekilde değerlendirilir. 1 GB varsayılan değerdir.


### <a name="copy-user-jar-cache"></a>Kullanıcı jar önbellek kopyalayın

Pig görev düğümler için kullanılabilir hale getirmek için Dağıtılmış bir önbellek UDF'ler tarafından gerekli JAR dosyalarını kopyalar. Bu jar dosyaları dışındaki sık değiştirmeyin. Etkinleştirilirse, `pig.user.cache.enabled` ayarı jar dosyaları dışındaki bir önbellekte aynı kullanıcı tarafından işlerini çalıştırmak için bunları yeniden yerleştirilmesini sağlar. İş performansı küçük bir artış sonuçlanır.

1. Etkinleştirmek için `pig.user.cache.enabled` true. Varsayılan değer false'dur.

1. Önbelleğe alınan jar dosyaları dışındaki temel yolunu ayarlamak için ayarlayın `pig.user.cache.location` temel yolu. Varsayılan değer: `/tmp`.


### <a name="optimize-performance-with-memory-settings"></a>Bellek ayarları ile performansı iyileştirme

Aşağıdaki bellek ayarları, Pig betiği performansı en iyi duruma yardımcı olabilir.

* `pig.cachedbag.memusage`: Bir paketi için ayrılmış bellek miktarı. Bir paket tanımlama grubu oluşan bir koleksiyondur. Bir demet alanları kümesini, ve bir alanın veri parçası. Bir paketi verileri tahsis edilen bellekten ise, geçmiş diske. Kullanılabilir bellek yüzde 20'si temsil eden 0.2 varsayılan değerdir. Bu bellek, bir uygulamada tüm paketleri arasında paylaşılır.

* `pig.spill.size.threshold`: Paketleri (bayt cinsinden) bu Saçılma boyutu eşik değerinden daha büyük geçmiş diske. Varsayılan değer 5 MB'dir.


### <a name="compress-temporary-files"></a>Geçici dosyaları sıkıştır

Pig, iş yürütme sırasında geçici dosyaları oluşturur. Geçici dosyalar sıkıştırılıyor dosyaları diske yazma veya okuma performans artışı sonuçlanır. Geçici dosyalar sıkıştırmak için aşağıdaki ayarlar kullanılabilir.

* `pig.tmpfilecompression`: TRUE olduğunda, geçici dosya sıkıştırma sağlar. Varsayılan değer false'tur.

* `pig.tmpfilecompression.codec`: Geçici dosyalar sıkıştırılıyor için kullanılacak sıkıştırma codec bileşeni. Önerilen sıkıştırma codec bileşenleri olan [LZO](https://www.oberhumer.com/opensource/lzo/) ve daha düşük CPU kullanımı için Snappy.

### <a name="enable-split-combining"></a>Bölünmüş birleştirme etkinleştir

Etkin olduğunda, küçük dosyaları daha az harita görevler için birleştirilir. Bu, çok sayıda küçük dosya içeren işleri verimliliğini artırır. Etkinleştirmek için `pig.noSplitCombination` true. Varsayılan değer false'tur.


### <a name="tune-mappers"></a>Azaltıcının ayarlama

Özelliğini değiştirerek azaltıcının sayısını kontrol `pig.maxCombinedSplitSize`. Bu, bir çoklu eşlem görev tarafından işlenecek veri boyutunu belirtir. Dosya sistemi'nın varsayılan blok boyutu varsayılan değerdir. Bu değer sonuçları bir düşüş Eşleyici görevlerin sayısını artırma.


### <a name="tune-reducers"></a>Ayarlama genişletin

Genişletin sayısı hesaplanır parametresinde `pig.exec.reducers.bytes.per.reducer`. Parametre Azaltıcı işlenen bayt sayısını belirtir, varsayılan olarak 1 GB. Genişletin sayısı üst sınırını ayarlayın `pig.exec.reducers.max` 999 varsayılan bir özelliği.


## <a name="apache-hbase-optimization-with-the-ambari-web-ui"></a>Ambari web kullanıcı Arabirimi ile Apache HBase iyileştirme

[Apache HBase](https://hbase.apache.org/) yapılandırma değişiklik **HBase yapılandırmaları** sekmesi. Aşağıdaki bölümlerde HBase performansı etkileyen önemli yapılandırma ayarlarını bazılarını açıklar.

### <a name="set-hbaseheapsize"></a>HBASE_HEAPSIZE ayarlayın

HBase yığın boyutu megabayt tarafından kullanılmak üzere yığın en uzun süreyi belirtir *bölge* ve *ana* sunucuları. Varsayılan değer 1000 MB'dir. Bu, küme iş yükü için ayarlanmalıdır.

1. Değiştirmek için gidin **Gelişmiş HBase env** HBase bölmesinde **yapılandırmaları** sekmesine ve ardından bulun `HBASE_HEAPSIZE` ayarı.

1. Varsayılan değer 5000 MB olarak değiştirin.

    ![HBASE_HEAPSIZE](./media/hdinsight-changing-configs-via-ambari/hbase-heapsize.png)


### <a name="optimize-read-heavy-workloads"></a>Okuma yoğunluklu iş yüklerini en iyi duruma getirme

Aşağıdaki yapılandırmalar okuma yoğunluklu iş yüklerinin performansını artırmak önemlidir.

#### <a name="block-cache-size"></a>Blok önbelleği boyutu

Okuma önbelleği blok önbelleğidir. Boyutuna göre denetlenir `hfile.block.cache.size` parametresi. Varsayılan 0.4, yüzde 40'a toplam bölge sunucu belleği olan değerdir. Büyük blok önbelleği boyutu, rastgele okuma daha hızlı olacaktır.

1. Bu parametreyi değiştirmek için gidin **ayarları** HBase sekmede **yapılandırmaları** sekmesine ve ardından bulun **RegionServer ayrılmış Okuma arabelleği için %**.

    ![HBase blok önbellek boyutu](./media/hdinsight-changing-configs-via-ambari/hbase-block-cache-size.png)
 
1. Değeri değiştirmek için seçin **Düzenle** simgesi.


#### <a name="memstore-size"></a>Memstore boyutu

Tüm düzenlemeleri olarak adlandırılan bellek arabelleği içinde depolanan bir *Memstore*. Bu toplam tek bir işlemde diske yazılan veri miktarını artırır ve en son düzenlemeler sonraki erişim hızlandırır. Memstore boyutu, aşağıdaki iki parametreyi tarafından tanımlanır:

* `hbase.regionserver.global.memstore.UpperLimit`: Memstore birlikte kullanabileceğiniz bölge sunucusu en fazla yüzdesini tanımlar.

* `hbase.regionserver.global.memstore.LowerLimit`: Memstore birlikte kullanabileceğiniz bölge sunucusu minimum yüzdesini tanımlar.

Rastgele Okuma için en iyi duruma getirme Memstore üst ve alt sınırları azaltabilir.


#### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Diski tararken getirilen satır sayısı

`hbase.client.scanner.caching` Ayarını tanımlar okunan satır sayısı ne zaman disk `next` yöntemi, bir tarayıcı üzerinde çağrılır.  Varsayılan değer 100’dür. Daha yüksek sayı ne kadar az uzak çağrılar istemciden daha hızlı taramaya bölge sunucuya yapılan. Ancak, bu bellek baskısı istemci üzerinde de artırır.

![HBase getirilen satır sayısı](./media/hdinsight-changing-configs-via-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]  
> Bir tarayıcı İleri yöntemi çağırmayı arasındaki süreyi tarayıcı zaman aşımından daha büyük olduğunu değeri ayarlanmadı. Tarayıcı zaman aşımı süresi tarafından tanımlanan `hbase.regionserver.lease.period` özelliği.


### <a name="optimize-write-heavy-workloads"></a>Yazma yoğunluklu iş yüklerini en iyi duruma getirme

Aşağıdaki yapılandırmalar, yazma yoğunluklu iş yüklerinin performansını artırmak önemlidir.


#### <a name="maximum-region-file-size"></a>Dosya boyutu en fazla bölge

HBase veri olarak adlandırılan bir iç dosya biçiminde depolar *Hfıle*. Özellik `hbase.hregion.max.filesize` bir bölgedeki tek bir Hfıle boyutunu tanımlar.  Bir bölgede tüm HFiles toplamını bu boyuttan büyükse bir bölge içinde iki bölgeleri ayrılır.
 
![HBase HRegion en büyük dosya boyutu](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-max-filesize.png)

Daha büyük bölge dosya boyutu, bölmelerini daha küçük sayısı. Yazma performansı sonuçları en yüksek değeri belirlemek için dosya boyutunu artırabilirsiniz.


#### <a name="avoid-update-blocking"></a>Güncelleştirme engellemekten kaçınacak

* Özellik `hbase.hregion.memstore.flush.size` Memstore Temizlenen boyutu tanımlar diske. Varsayılan boyutu 128 MB'dir.

* Hbase bölge blok çarpan tarafından tanımlanan `hbase.hregion.memstore.block.multiplier`. Varsayılan değer 4'tür. İzin verilen en fazla 8'dir.

* HBase Memstore ise güncelleştirmeleri engeller (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bayt.

    Memstore 128 * 4 = 512 MB boyutunda sırada temizleme boyut ve blok çarpan varsayılan değerlerle güncelleştirmeleri engellenir. Güncelleştirme sayısı engelleme azaltmak için değeri artırmak `hbase.hregion.memstore.block.multiplier`.

![HBase bölge blok çarpanı](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-block-multiplier.png)


### <a name="define-memstore-size"></a>Memstore boyutu tanımlayın

Memstore boyutu tarafından tanımlanan `hbase.regionserver.global.memstore.UpperLimit` ve `hbase.regionserver.global.memstore.LowerLimit` parametreleri. Bu değerleri birbirine her sırasında duraklamaları azaltır eşittir (Ayrıca daha sık bir temizlemeye neden) yazar ve artan yazma performansla sonuçlanır ayarlanıyor.


### <a name="set-memstore-local-allocation-buffer"></a>Memstore yerel ayırma arabellek ayarlayın

Memstore yerel ayırma Arabellek kullanımı özelliği tarafından belirlenir `hbase.hregion.memstore.mslab.enabled`. Etkin olduğunda (true), bu yığın parçalanma ağır yazma işlemi sırasında engeller. Varsayılan değer true olur.
 
![hbase.hregion.memstore.mslab.enabled](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-mslab-enabled.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Apache Ambari web kullanıcı Arabirimi ile HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
