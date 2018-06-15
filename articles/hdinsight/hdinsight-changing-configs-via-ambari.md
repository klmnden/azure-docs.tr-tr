---
title: Ambari - Azure Hdınsight küme yapılandırmaları en iyi duruma getirme | Microsoft Docs
description: Hdınsight kümeleri en iyi duruma getirme ve yapılandırmak için Ambari web kullanıcı arabirimini kullanın.
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ashish
ms.openlocfilehash: f3c1edc767ab07bcdd8b09a0e40e291cbd1f3d9a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31406197"
---
# <a name="use-ambari-to-optimize-hdinsight-cluster-configurations"></a>Hdınsight küme yapılandırmaları en iyi duruma getirmek için Ambari kullanın

Hdınsight, büyük ölçekli veri işleme uygulamaları için Apache Hadoop kümelerini sağlar. Yönetme, izleme ve bu karmaşık çok düğümlü küme en iyi duruma getirme zor olabilir. [Apache Ambari](http://ambari.apache.org/) Hdınsight Linux kümeleri izlemek ve yönetmek için bir web arabirimidir.  Windows kümeleri için Ambari kullanmak [REST API](hdinsight-hadoop-manage-ambari-rest-api.md).

Ambari Web kullanıcı arabirimini kullanarak bir giriş için bkz [Ambari Web kullanıcı arabirimini kullanarak tarafından Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)

Oturum açtığınızda Ambari `https://CLUSTERNAME.azurehdidnsight.net` küme kimlik bilgilerinizle. Başlangıç ekranında bir genel bakış Panosu'na görüntüler.

![Ambari Panosu](./media/hdinsight-changing-configs-via-ambari/ambari-dashboard.png)

Ambari web kullanıcı Arabirimi ana bilgisayarları, hizmetleri, uyarılar, yapılandırmaları ve görünümleri yönetmek için kullanılabilir. Ambari, Hdınsight kümesi oluşturma, hizmetleri yükseltme, yığınları ve sürümleri yönetmek, yetkisini veya ana recommission veya hizmetleri kümeye eklemek için kullanılamaz.

## <a name="manage-your-clusters-configuration"></a>Kümenizin yapılandırmasını Yönet

Yapılandırma ayarları, belirli bir hizmet ince ayar yardımcı olur. Bir hizmetin yapılandırma ayarlarını değiştirmek için hizmetten seçin **Hizmetleri** kenar (soldaki) ve ardından gidin **yapılandırmalar** hizmet ayrıntı sayfası sekmesindedir.

![Kenar Hizmetleri](./media/hdinsight-changing-configs-via-ambari/services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>İş Java öbek boyutunu değiştir

İş Java yığın boyutu küme, dosyaların numaralarını ve blokları sayıda üzerindeki yükü gibi birçok faktöre bağlıdır. Bazı iş yükleri fazla veya az bellek gerekmesine rağmen varsayılan boyut 1 GB olan çoğu kümeleriyle iyi çalışır. 

İş Java öbek boyutunu değiştirmek için:

1. Seçin **HDFS** Hizmetleri kenar çubuğundan gidin **yapılandırmalar** sekmesi.

    ![HDFS yapılandırma](./media/hdinsight-changing-configs-via-ambari/hdfs-config.png)

2. Ayar Bul **iş Java yığın boyutu**. Aynı zamanda **filtre** yazın ve belirli bir ayarı bulmak için metin kutusu. Seçin **kalem** ayar adının yanındaki simge.

    ![İş Java yığın boyutu](./media/hdinsight-changing-configs-via-ambari/java-heap-size.png)

3. Metin kutusuna yeni bir değer yazın ve sonra basın **Enter** değişikliği kaydetmek için.

    ![İş Java yığın boyutu Düzenle](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit.png)

4. İş Java yığın boyutu 1 GB ile 2 GB ile değiştirilir.

    ![Düzenlenmiş iş Java yığın boyutu](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

5. Yeşil tıklayarak yaptığınız değişiklikleri kaydetmek **kaydetmek** yapılandırma ekranında üst kısmında düğmesi.

    ![Değişiklikleri kaydet](./media/hdinsight-changing-configs-via-ambari/save-changes.png)

## <a name="hive-optimization"></a>Hive en iyi duruma getirme

Aşağıdaki bölümlerde genel Hive performansı iyileştirmek için yapılandırma seçenekleri açıklanmaktadır.

1. Hive yapılandırma parametreleri değiştirmek için seçin **Hive** Hizmetleri kenar çubuğundan.
2. Gidin **yapılandırmalar** sekmesi.

### <a name="set-the-hive-execution-engine"></a>Hive yürütme altyapısı ayarlayın

Hive iki yürütme altyapısı sağlar: MapReduce ve Tez. Tez MapReduce hızlıdır. Hdınsight Linux kümeleri varsayılan yürütme altyapısı Tez sahiptir. Yürütme altyapısı değiştirmek için:

1. Kovanında **yapılandırmalar** sekmesinde, yazın **yürütme altyapısı** ve filtre kutusuna.

    ![Arama yürütme alt yapısı](./media/hdinsight-changing-configs-via-ambari/search-execution.png)

2. **En iyi duruma getirme** özelliğin varsayılan değeri **Tez**.

    ![En iyi duruma getirme - Tez](./media/hdinsight-changing-configs-via-ambari/optimization-tez.png)

### <a name="tune-mappers"></a>Mappers ayarlama

Hadoop çalışır bölme (*harita*) paralel olarak birden çok dosya ve elde edilen işlem tek bir dosyaya dosyaları. Mappers sayısı bölmelerini sayısına bağlıdır. Aşağıdaki iki yapılandırma parametrelerinin sayısı bölmelerini Tez yürütme altyapısı, sürücü:

* `tez.grouping.min-size`: Gruplandırılmış bir bölme boyutu 16 MB (16,777,216 bayt) varsayılan bir değerle alt sınırı.
* `tez.grouping.max-size`: Gruplandırılmış bir bölme boyutu 1 GB (1.073.741.824 bayt) varsayılan bir değerle üst sınırı.

Bir performans altın kural olarak, gecikme süresini artırmak için daha fazla verimliliğini artırmak için bu parametrelerin her ikisini de azaltın.

Örneğin, dört Eşleyici görevler için 128 MB veri boyutunu ayarlamak için her iki parametre için 32 MB her (33,554,432 bayt) ayarlamalısınız.

1. Sınır parametreleri değiştirmek için gidin **yapılandırmalar** Tez hizmetinin sekmesi. Genişletme **genel** panel ve bulun `tez.grouping.max-size` ve `tez.grouping.min-size` parametreleri.

2. Her iki parametre kümesine **33,554,432** bayt (32 MB).

    ![Tez gruplandırma boyutları](./media/hdinsight-changing-configs-via-ambari/tez-grouping-size.png)
 
Bu değişiklikler, tüm Tez işlerinde sunucunun tamamında etkiler. En iyi sonucu almak için uygun parametre değerlerini seçin.

### <a name="tune-reducers"></a>Reducers ayarlama

ORC ve Snappy yüksek performans sunar. Bununla birlikte, Hive performans sorunlarını neden çok az reducers varsayılan olarak, olabilir.

Örneğin, bir giriş veri boyutu 50 GB olduğunu varsayalım. ORC verileri Snappy sıkıştırmaya biçimlendirmek 1 GB'tır. Hive tahminleri olarak gereken reducers sayısı: (mappers girişine bayt sayısı / `hive.exec.reducers.bytes.per.reducer`).

Varsayılan ayarlarla bu 4 reducers örneğidir.

`hive.exec.reducers.bytes.per.reducer` Parametresi reducer işlenen bayt sayısını belirtir. Varsayılan değer 64 MB'tır. Bu değer aşağı ayarlama paralellik artırır ve performansını artırabilir. Çok düşük ayarlama performansı potansiyel olarak olumsuz etkileyen çok fazla reducers üretebilir. Bu parametre, belirli veri gereksinimlerinizi, sıkıştırma ayarlarını ve diğer çevresel faktörlere dayanır.

1. Parametre değiştirmek için Hive gidin **yapılandırmalar** sekmesinde ve Bul **Reducer başına veri** Ayarları sayfasında parametresi.

    ![Veri Reducer başına](./media/hdinsight-changing-configs-via-ambari/data-per-reducer.png)
 
2. Seçin **Düzenle** 128 MB (134,217,728 bayt) değerine değiştirin ve ENTER tuşuna basın **Enter** kaydetmek için.

    ![Düzenlenen Reducer - başına veri](./media/hdinsight-changing-configs-via-ambari/data-per-reducer-edited.png)
  
    Bir giriş boyutu 128 MB reducer, her veri ile 1024 MB verilen 8 reducers vardır (1024/128).

3. İçin yanlış bir değere **Reducer başına veri** parametre çok sayıda sorgu performansını olumsuz yönde etkileyen reducers neden olabilir. Reducers sayısı üst sınırını ayarlayın `hive.exec.reducers.max` uygun bir değere. 1009 varsayılan değerdir.

### <a name="enable-parallel-execution"></a>Paralel yürütme etkinleştir

Bir Hive sorgusu, bir veya daha fazla aşamada yürütülür. Bağımsız aşamaları paralel olarak çalıştırılabilir varsa, sorgu performansı artırır.

1.  Paralel sorgu yürütme etkinleştirmek için Hive gidin **Config** sekmesinde ve arama `hive.exec.parallel` özelliği. Varsayılan değer false. Tuşuna basın ve true değerini değiştirin **Enter** değeri kaydetmek için.
 
2.  Paralel olarak çalıştırılacak işlerin sayısını sınırlamak için değiştirme `hive.exec.parallel.thread.number` özelliği. Varsayılan değer 8'dir.

    ![Exec paralel yığını](./media/hdinsight-changing-configs-via-ambari/hive-exec-parallel.png)


### <a name="enable-vectorization"></a>Vectorization etkinleştir

Hive veriler satır temelinde işler. Vectorization Hive bir defada bir satır yerine 1.024 satır bloklarındaki verileri işlemek için yönlendirir. Vectorization yalnızca ORC dosya biçimine geçerlidir.

1. Vectorized sorgu yürütme etkinleştirmek için Hive gidin **yapılandırmalar** sekmesinde ve arama `hive.vectorized.execution.enabled` parametresi. Varsayılan değer true Hive 0.13.0 için veya üzeri.
 
2. Sorgunun azaltın tarafı için vectorized yürütülmesine izin verecek biçimde ayarlamak `hive.vectorized.execution.reduce.enabled` parametresi true. Varsayılan değer false.

    ![Vectorized hive yürütme](./media/hdinsight-changing-configs-via-ambari/hive-vectorized-execution.png)

### <a name="enable-cost-based-optimization-cbo"></a>Maliyet tabanlı iyileştirme (BHA) etkinleştir

Varsayılan olarak, Hive bir en iyi sorgu yürütme planı bulmak için kurallar kümesini izler. Maliyet tabanlı iyileştirme (BHA) bir sorguyu yürütmek için birden çok planları değerlendirir ve maliyet her plana atar, ardından bir sorguyu yürütmek için ucuz planı belirler.

BHA etkinleştirmek için Hive gidin **yapılandırmalar** sekmesinde ve arama `parameter hive.cbo.enable`, iki durumlu düğme geçiş **üzerinde**.

![BHA yapılandırma](./media/hdinsight-changing-configs-via-ambari/cbo.png)

BHA etkinleştirilmişse, aşağıdaki ek yapılandırma parametrelerini Hive sorgu performansı artırın:

* `hive.compute.query.using.stats`

    TRUE olarak Hive kullandığında, meta depo içinde depolanan istatistikleri gibi basit sorguları yanıtlamak için `count(*)`.

    ![BHA istatistikleri](./media/hdinsight-changing-configs-via-ambari/hive-compute-query-using-stats.png)

* `hive.stats.fetch.column.stats`

    Sütun istatistikleri BHA etkinleştirildiğinde oluşturulur. Hive meta depo içinde sorguları en iyi duruma getirme depolanan sütun istatistikleri kullanır. Sütun sayısı yüksek olduğunda her sütun için sütun istatistikleri getirme daha uzun sürer. False olarak ayarlandığında, bu ayar meta depo getirilirken sütun istatistikleri devre dışı bırakır.

    ![İstatistikleri set sütun istatistikleri yığını](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-column-stats.png)

* `hive.stats.fetch.partition.stats`

    Satırlar, veri boyutu ve dosya boyutu sayısı gibi temel bir bölümde istatistikleri meta depo içinde depolanır. True olarak istatistiği meta depo getirilen bölüm olduğunda ayarlanır. Yanlış olduğunda, dosya boyutu dosya sisteminden getirildi ve satır şemadan getirilen satır sayısı.

    ![İstatistikleri set bölüm istatistikleri yığını](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-partition-stats.png)

### <a name="enable-intermediate-compression"></a>Ara sıkıştırmayı etkinleştir

Harita görevler reducer görevleri tarafından kullanılan ara dosya oluşturun. Ara sıkıştırma Ara dosya boyutunu küçültür.

Hadoop işlerini genellikle nedeniyle düşük performansa g/ç ' dir. Verileri sıkıştırma g/ç ve genel ağ aktarımı hızlandırabilir.

Kullanılabilir sıkıştırma türleri şunlardır:

| Biçimlendir | Aracı | Algoritması | Dosya uzantısı | Bölümlenebilir? |
| -- | -- | -- | -- | -- |
| Gzip | Gzip | SÖNDÜR | .gz | Hayır |
| Bzip2 | Bzip2 | Bzip2 |.bz2 | Evet |
| LZO | Lzop | LZO | .lzo | Dizine, Evet |
| snappy | Yok | snappy | snappy | Hayır |

Genel kural olarak bölümlenebilir bir sıkıştırma yöntemi sahip olmak önemlidir, aksi takdirde çok az mappers oluşturulur. Giriş verilerini bir metin ise `bzip2` en iyi seçenektir. ORC için Snappy hızlı sıkıştırma seçeneği biçimidir.

1. Ara sıkıştırmasını etkinleştirmek için kovana gidin **yapılandırmalar** sekmesini tıklatın ve ardından `hive.exec.compress.intermediate` parametresi true. Varsayılan değer false.

    ![Hive exec sıkıştırma Ara](./media/hdinsight-changing-configs-via-ambari/hive-exec-compress-intermediate.png)

    > [!NOTE]
    > Ara dosyaları sıkıştırmak için codec yüksek sıkıştırma çıkış olmasa dahi bir sıkıştırma codec maliyeti, daha düşük CPU ile seçin.

2. Ara sıkıştırma codec ayarlamak için özel özellik ekleme `mapred.map.output.compression.codec` için `hive-site.xml` veya `mapred-site.xml` dosyası.

3. Özel bir ayar eklemek için:

    a. Hive gidin **yapılandırmalar** sekmesinde ve seçin **Gelişmiş** sekmesi.

    b. Altında **Gelişmiş** sekmesinde, bulmak ve genişletin **özel hive-site** bölmesi.

    c. Bağlantıya tıklayın **Özellik Ekle** özel hive site bölmesinin altındaki.

    d. Özellik Ekle penceresinde girin `mapred.map.output.compression.codec` anahtar olarak ve `org.apache.hadoop.io.compress.SnappyCodec` değeri olarak.

    e. **Ekle**'ye tıklayın.

    ![Özel özellik yığını](./media/hdinsight-changing-configs-via-ambari/hive-custom-property.png)

    Bu Ara dosyayı Snappy sıkıştırma kullanarak sıkıştırır. Özellik eklendikten sonra özel hive site bölmesinde görüntülenir.

    > [!NOTE]
    > Bu yordamı değiştirir `$HADOOP_HOME/conf/hive-site.xml` dosya.

### <a name="compress-final-output"></a>Son çıktı Sıkıştır

Son yığını çıktı de birleştirilebilir.

1. Son yığını çıktı sıkıştırılacak kovana gidin **yapılandırmalar** sekmesini tıklatın ve ardından `hive.exec.compress.output` parametresi true. Varsayılan değer false.

2. Çıkış sıkıştırma codec seçmek için ekleme `mapred.output.compression.codec` önceki bölümün adım 3'te açıklandığı gibi özel hive site bölmesine özel özellik.

    ![Özel özellik yığını](./media/hdinsight-changing-configs-via-ambari/hive-custom-property2.png)

### <a name="enable-speculative-execution"></a>Kurgusal yürütme etkinleştir

Kurgusal yürütme belirli bir sayıda algılamak ve tek tek görev sonuçları iyileştirerek toplam iş yürütme artırırken yavaş çalışan görev İzleyici kara için yinelenen görevleri başlatır.

Kurgusal yürütme giriş büyük miktarlarda ile uzun süre çalışan MapReduce görevleri için açılmaması gereken.

* Kurgusal yürütülmesine izin verecek biçimde kovana gidin **yapılandırmalar** sekmesini tıklatın ve ardından `hive.mapred.reduce.tasks.speculative.execution` parametresi true. Varsayılan değer false.

    ![Hive mapred azaltmak görevleri kurgusal yürütme](./media/hdinsight-changing-configs-via-ambari/hive-mapred-reduce-tasks-speculative-execution.png)

### <a name="tune-dynamic-partitions"></a>Dinamik bölümleri ayarlama

Hive kayıt her bölüm önceden tanımlanması olmadan bir tabloya eklerken dinamik bölümleri oluşturmak için sağlar. Çok sayıda bölüm ve çok sayıda dosya her bölüm için oluşturulmasına neden olabilir ancak güçlü bir özellik budur.

1. Hive dinamik bölümleri yapmak için `hive.exec.dynamic.partition` parametre değeri true olmalıdır (varsayılan).

2. Dinamik bölüm moduna *katı*. Katı mod içinde en az bir bölüm statik olması gerekir. Bu, diğer bir deyişle, sorguları WHERE yan tümcesinde bölüm filtresi olmadan engeller *katı* tüm bölümleri tarama sorguları engeller. Hive gidin **yapılandırmalar** sekmesini tıklatın ve ardından `hive.exec.dynamic.partition.mode` için **katı**. Varsayılan değer **Gevþek**.
 
3. Oluşturulacak dinamik bölümleri sayısını sınırlamak için değiştirme '' hive.exec.max.dynamic.partitions parametresi. Varsayılan değer 5000 ' dir.
 
4. Düğüm başına dinamik bölümleri toplam sayısını sınırlamak için değiştirme `hive.exec.max.dynamic.partitions.pernode`. Varsayılan değer 2. 000'dir.

### <a name="enable-local-mode"></a>Yerel modunu etkinleştir

Yerel mod, tek bir makinede veya kimi zaman tek bir işlemde bir işin tüm görevleri gerçekleştirmek Hive sağlar. Bu, giriş verilerini küçük ise ve sorgular için bir görevi başlatmadan yükünü genel sorgu yürütme önemli oranda tüketir sorgu performansı artırır.

Yerel modunu etkinleştirmek için add `hive.exec.mode.local.auto` yordamının 3. adımında açıklandığı gibi özel hive site paneline parametresi [Ara sıkıştırmayı etkinleştir](#enable-intermediate-compression) bölümü.

![Hive yürütme modu yerel otomatik](./media/hdinsight-changing-configs-via-ambari/hive-exec-mode-local-auto.png)

### <a name="set-single-mapreduce-multigroup-by"></a>Set tek MapReduce MultiGROUP tarafından

Bu özellik True olarak MultiGROUP tarafından sorguda grubu tarafından ortak anahtarlar ayarlandığında tek bir MapReduce işi oluşturur.  

Bu davranışı etkinleştirmek için add `hive.multigroupby.singlereducer` yordamının 3. adımında açıklandığı gibi özel hive site bölmesine parametresi [Ara sıkıştırmayı etkinleştir](#enable-intermediate-compression) bölümü.

![Tek MapReduce MultiGROUP tarafından Hive ayarlayın](./media/hdinsight-changing-configs-via-ambari/hive-multigroupby-singlereducer.png)

### <a name="additional-hive-optimizations"></a>Ek Hive en iyi duruma getirme

Aşağıdaki bölümlerde ayarlayabileceğiniz ek yığın ile ilgili iyileştirmeler açıklanmaktadır.

#### <a name="join-optimizations"></a>En iyi duruma getirme katılma

Varsayılan bir birleştirme türü kovanında bir *karışık birleştirme*. Hive, özel mappers giriş okuyun ve Ara dosya bir birleşim anahtar/değer çifti yayma. Hadoop, sıralar ve bu çiftlerine karışık aşamasında birleştirir. Bu karışık aşama maliyetlidir. Verilerinizi temel alarak sağ birleşim seçme performansı önemli ölçüde artırabilir.

| Katılım Türü | Ne zaman | Nasıl | Hive ayarları | Yorumlar |
| -- | -- | -- | -- | -- |
| Karışık birleştirme | <ul><li>Varsayılan seçenek</li><li>Her zaman çalışır</li></ul> | <ul><li>Tabloları birinin bölümünden okur</li><li>Demet ve birleştirme anahtarı sıralar</li><li>Her reduce'a bir demet gönderir</li><li>Birleşim azaltın tarafında gerçekleştirilir</li></ul> | Hiçbir önemli Hive gerekli ayarlama | Her zaman çalışır |
| Harita birleştirme | <ul><li>Bir tablo bellekte uygun olamaz</li></ul> | <ul><li>Küçük bir tablo bellek karma tablosuna okur</li><li>Büyük dosya parçası aracılığıyla akışlar</li><li>Karma tablo her kayıttan birleştirir</li><li>Birleşimler Eşleyicisi tarafından olan</li></ul> | `hive.auto.confvert.join=true` | Çok hızlı, ancak sınırlı |
| Sıralama birleştirme demet | Her iki tabloyu varsa: <ul><li>Aynı sıralanmış</li><li>Aynı bucketed</li><li>Sıralanmış/kümelenmiş sütun birleştirme</li></ul> | Her işlem: <ul><li>Her tablodan bir demet okur</li><li>En düşük değere sahip satırı işler</li></ul> | `hive.auto.convert.sortmerge.join=true` | Çok verimli |

#### <a name="execution-engine-optimizations"></a>Yürütme altyapısı en iyi duruma getirme

Hive yürütme altyapısı en iyi duruma getirmek için ek öneriler:

| Ayar | Önerilen | Hdınsight varsayılan |
| -- | -- | -- |
| `hive.mapjoin.hybridgrace.hashtable` | Doğru = daha güvenli, daha yavaş; yanlış daha hızlı = | false |
| `tez.am.resource.memory.mb` | Çoğu için 4 GB üst sınırı | Otomatik olarak ayarlanmış |
| `tez.session.am.dag.submit.timeout.secs` | 300+ | 300 |
| `tez.am.container.idle.release-timeout-min.millis` | 20000+ | 10000 |
| `tez.am.container.idle.release-timeout-max.millis` | 40000+ | 20000 |

## <a name="pig-optimization"></a>Pig en iyi duruma getirme

Pig özellikleri Ambari web Pig sorguları ayarlamak için kullanıcı Arabirimi değiştirilebilir. Ambari pig özelliklerinden doğrudan değiştirme değiştirir Pig özelliklerinde `/etc/pig/2.4.2.0-258.0/pig.properties` dosya.

1. Pig özelliklerini değiştirmek için Pig gidin **yapılandırmalar** sekmesini tıklatın ve ardından **Gelişmiş pig özellikleri** bölmesi.

2. Bulun, açıklamadan çıkarın ve değiştirmek istediğiniz özelliğin değerini değiştirin.

3. Seçin **kaydetmek** yeni değeri kaydetmek için pencerenin sağ üst tarafında üzerinde. Bazı özellikler, hizmet yeniden başlatılmasını gerektirebilir.

    ![Gelişmiş pig özellikleri](./media/hdinsight-changing-configs-via-ambari/advanced-pig-properties.png)
 
> [!NOTE]
> Özellik değerlerinde tüm oturum düzeyi ayarları geçersiz kılar `pig.properties` dosya.

### <a name="tune-execution-engine"></a>Yürütme altyapısı ayarlama

Pig betikleri çalıştırmak iki yürütme motorları kullanılabilir: MapReduce ve Tez. Tez en iyi duruma getirilmiş bir altyapısıdır ve MapReduce hızlıdır.

1. Yürütme altyapısı değiştirmek için **Gelişmiş pig özellikleri** bölmesinde özelliği `exectype`.

2. Varsayılan değer **MapReduce**. Şekilde değiştirin **Tez**.


### <a name="enable-local-mode"></a>Yerel modunu etkinleştir

Hive, yerel mod benzer görece küçük miktarda veri işleriyle hızlandırmak için kullanılır.

1. Yerel modunu etkinleştirmek için ayarlanmış `pig.auto.local.enabled` için **doğru**. Varsayılan değer false.

2. Bir giriş veri boyutu işleriyle küçük `pig.auto.local.input.maxbytes` özellik değeri küçük işleri olarak değerlendirilir. Varsayılan değer 1 GB'dir.


### <a name="copy-user-jar-cache"></a>Kullanıcı jar önbellek kopyalayın

Pig, bunları görev düğümler için kullanılabilir hale getirmek için Dağıtılmış bir önbellek UDF'ler tarafından gerekli JAR dosyalarını kopyalar. Bu Kavanoz sık değiştirmeyin. Etkinleştirilirse, `pig.user.cache.enabled` ayarı aynı kullanıcı tarafından işlerini çalıştırmak için bunları yeniden kullanmak için bir önbellekte yerleştirilecek Kavanoz sağlar. Bu, iş performans küçük bir artış sonuçlanır.

1. Etkinleştirmek için ayarlanmış `pig.user.cache.enabled` true. Varsayılan değer false.

2. Önbelleğe alınan Kavanoz temel yolunu ayarlamak için ayarlayın `pig.user.cache.location` taban yolu. Varsayılan değer `/tmp`.


### <a name="optimize-performance-with-memory-settings"></a>Bellek ayarları ile performansı en iyi duruma getirme

Aşağıdaki bellek ayarları, Pig betiği performansı en iyi duruma yardımcı olur.

* `pig.cachedbag.memusage`: Bir paketi için ayrılan bellek miktarı. Bir paketi diziler bir koleksiyonudur. Bir tanımlama grubu alanları sıralanmış bir kümesini ve bir alan veri parçası. Bir paketi verileri tahsis edilen bellekten ise, geçmiş diske. Kullanılabilir belleğin yüzde 20 temsil eden 0.2 varsayılan değerdir. Bu bellek, uygulamada tüm paketler arasında paylaşılır.

* `pig.spill.size.threshold`: Paketler bu Karmadaki boyut eşiği (bayt cinsinden) daha büyük geçmiş diske. Varsayılan değer 5 MB'tır.


### <a name="compress-temporary-files"></a>Geçici dosyaları sıkıştırın.

Pig işi yürütme sırasında geçici dosyaları oluşturur. Geçici dosyalar sıkıştırma okunurken ya da dosyaları diske yazılırken bir performans artışı sonuçlanır. Aşağıdaki ayarlar, geçici dosyaları sıkıştırmak için kullanılabilir.

* `pig.tmpfilecompression`: Doğru olduğunda, geçici dosya sıkıştırma sağlar. Varsayılan değer false.

* `pig.tmpfilecompression.codec`: Geçici dosyalar sıkıştırma için kullanılacak sıkıştırma codec. Önerilen sıkıştırma codec LZO ve Snappy daha düşük CPU kullanımı için verilebilir.

### <a name="enable-split-combining"></a>Bölünmüş birleştirme etkinleştir

Etkinleştirildiğinde, küçük dosyalar daha az harita görevler için birleştirilir. Bu, çok sayıda küçük dosyalar işleri verimliliğini artırır. Etkinleştirmek için ayarlanmış `pig.noSplitCombination` true. Varsayılan değer false.


### <a name="tune-mappers"></a>Mappers ayarlama

Mappers sayısı özelliğini değiştirerek denetlenir `pig.maxCombinedSplitSize`. Bu, bir tek eşleme görevi tarafından işlenmesi için verilerin boyutunu belirtir. Dosya sistemi'nın varsayılan blok boyutu varsayılan değerdir. Bir düşüş Eşleyici görev sayısı bu değeri sonuçlarında artırma.


### <a name="tune-reducers"></a>Reducers ayarlama

Reducers sayısı temel parametresini hesaplanır `pig.exec.reducers.bytes.per.reducer`. 1 GB varsayılan olarak, parametre reducer işlenen bayt sayısını belirtir. Reducers sayısı üst sınırını ayarlayın `pig.exec.reducers.max` 999 varsayılan özellik.


## <a name="hbase-optimization-with-the-ambari-web-ui"></a>Ambari web kullanıcı Arabirimi ile HBase en iyi duruma getirme

HBase yapılandırmasını değiştirdi **HBase yapılandırmalar** sekmesi. Aşağıdaki bölümlerde HBase performansı etkileyen önemli yapılandırma ayarlarını bazıları açıklanmaktadır.

### <a name="set-hbaseheapsize"></a>HBASE_HEAPSIZE ayarlama

HBase yığın boyutu üst sınırını megabayt tarafından kullanılacak yığın belirtir *bölge* ve *ana* sunucuları. Varsayılan değer 1000 MB'tır. Küme iş yükü için ayarlanmış.

1. Değiştirmek için gidin **Gelişmiş HBase env** HBase bölmesinde **yapılandırmalar** sekmesini tıklatın ve ardından bulmak `HBASE_HEAPSIZE` ayarı.

2. Varsayılan değer 5000 MB olarak değiştirin.

    ![HBASE_HEAPSIZE](./media/hdinsight-changing-configs-via-ambari/hbase-heapsize.png)


### <a name="optimize-read-heavy-workloads"></a>Okuma ağır iş yüklerini en iyi duruma getirme

Aşağıdaki yapılandırmalar okuma ağır iş yüklerinin performansını artırmak önemlidir.

#### <a name="block-cache-size"></a>Blok önbellek boyutu

Okuma önbelleği blok önbelleğidir. Boyutuna göre denetlenir `hfile.block.cache.size` parametresi. Varsayılan değer 0.4, toplam bölge sunucu belleği yüzde 40'olduğu ' dir. Daha büyük blok önbellek boyutu, rastgele okuma daha hızlı olacaktır.

1. Bu parametre değiştirmek için gidin **ayarları** HBase sekmesinde **yapılandırmalar** sekmesini tıklatın ve sonra bulun **RegionServer ayrılan Okuma arabelleği için %**.

    ![HBase blok önbellek boyutu](./media/hdinsight-changing-configs-via-ambari/hbase-block-cache-size.png)
 
2. Değerini değiştirmek için seçin **Düzenle** simgesi.


#### <a name="memstore-size"></a>Memstore boyutu

Tüm düzenlemeleri adlı bellek arabellek depolanan bir *Memstore*. Bu toplam tek bir işlemde diske yazılan veri miktarını artırır ve son düzenlemeleri sonraki erişim hızlandırır. Memstore boyutu aşağıdaki iki parametreyi tarafından belirlenir:

* `hbase.regionserver.global.memstore.UpperLimit`: Birleştirilmiş Memstore kullanabilirsiniz bölge sunucu yüzdesinin üst sınırını tanımlar.

* `hbase.regionserver.global.memstore.LowerLimit`: Birleştirilmiş Memstore kullanabilirsiniz bölge sunucunun minimum yüzdesini tanımlar.

Rastgele Okuma için en iyi duruma getirme Memstore üst ve alt sınırları azaltabilir.


#### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Diskten taranırken getirilen satır sayısı

`hbase.client.scanner.caching` Ayarı okuma satır sayısını tanımlar ne zaman disk `next` yöntemi, bir tarayıcıya çağrılır.  Varsayılan değer 100'dür. Daha yüksek sayı, o kadar az uzak çağrılar istemciden daha hızlı taramaya kaynaklanan bölge sunucuya yapılan. Ancak, bu istemcide bellek baskısı da artırır.

![HBase getirilen satır sayısı](./media/hdinsight-changing-configs-via-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]
> Bir tarayıcı üzerinde sonraki yöntemi çağrıldı arasındaki süre tarayıcı zaman aşımından daha büyük olacak şekilde, değere ayarlı değil. Tarayıcı zaman aşımı süresi tarafından tanımlanan `hbase.regionserver.lease.period` özelliği.


### <a name="optimize-write-heavy-workloads"></a>Yazma yoğunluklu iş yüklerini en iyi duruma getirme

Aşağıdaki yapılandırmalar yazma yoğunluklu iş yüklerinin performansını artırmak önemlidir.


#### <a name="maximum-region-file-size"></a>En fazla bölge dosya boyutu

HBase olarak adlandırılan bir iç dosya biçiminde veri depolayan *Hfıle*. Özellik `hbase.hregion.max.filesize` bir bölgedeki tek bir Hfıle boyutunu tanımlar.  Bir bölgedeki tüm HFiles toplamını bu boyuttan büyükse bir bölge iki bölgeye ayrılır.
 
![HBase HRegion en büyük dosya boyutu](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-max-filesize.png)

Daha büyük bölge dosya boyutu, bölmelerini daha az sayıda. En fazla sonuçlarında yazma performansı değeri belirlemek için dosya boyutunu artırabilir.


#### <a name="avoid-update-blocking"></a>Güncelleştirme engelleme kaçının

* Özellik `hbase.hregion.memstore.flush.size` Memstore Temizlenen boyutunu tanımlar diske. Varsayılan boyut 128 MB'tır.

* Hbase bölge blok çarpanı tarafından tanımlanan `hbase.hregion.memstore.block.multiplier`. Varsayılan değer 4'tür. İzin verilen maksimum 8'dir.

* HBase Memstore ise, güncelleştirmelerinin engeller (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bayt sayısı.

    Memstore 128 * 4 = 512 MB boyutunda olduğunda temizleme boyutu ve blok çarpanı varsayılan değerlerle güncelleştirmeleri engellenir. Count engelleme güncelleştirme azaltmak için değeri artırmak `hbase.hregion.memstore.block.multiplier`.

![HBase bölge blok çarpanı](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-block-multiplier.png)


### <a name="define-memstore-size"></a>Memstore boyutu tanımlayın

Memstore boyutu tarafından tanımlanan `hbase.regionserver.global.memstore.UpperLimit` ve `hbase.regionserver.global.memstore.LowerLimit` parametreleri. Ayar bu değerleri çok her diğer sırasında duraklatır azaltır eşittir (aynı zamanda daha sık temizleme neden) yazar ve artan yazma performansla sonuçlanır.


### <a name="set-memstore-local-allocation-buffer"></a>Memstore yerel ayırma arabellek ayarlayın

Memstore yerel ayırma Arabellek kullanımı özelliği tarafından belirlenir `hbase.hregion.memstore.mslab.enabled`. Etkin olduğunda (true), bu yığın parçalanma ağır yazma işlemi sırasında önler. Varsayılan değer true olur.
 
![hbase.hregion.memstore.mslab.enabled](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-mslab-enabled.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight kümeleri Ambari web kullanıcı Arabirimi ile yönetme](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
