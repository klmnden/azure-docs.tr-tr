---
title: Veri özellikleri Hive sorgularını kullanarak, Hadoop küme oluşturma | Microsoft Docs
description: Bir Azure Hdınsight Hadoop kümesinde depolanan verilerin özelliklerini Oluştur Hive sorguları örnekleri.
services: machine-learning
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: bradsev
ms.openlocfilehash: f49eeee2dd26d54674b4619e6c986952718caa47
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-features-for-data-in-a-hadoop-cluster-using-hive-queries"></a>Hive sorgularını kullanarak bir Hadoop kümesinde veri özellikleri oluşturma
Bu belge Hive sorgularını kullanarak, Azure Hdınsight Hadoop kümesi depolanan verilerin özelliklerini oluşturulacağını gösterir. Bu Hive sorguları katıştırılmış Hive User-Defined olduğu için komut dosyalarını sağlanan işlevler (UDF'ler) kullanın.

Özellikleri oluşturmak için gereken işlem bellek yoğun olabilir. Hive sorguları performansını bu gibi durumlarda daha önemli hale gelir ve belirli parametreleri ayarlama tarafından geliştirilebilir. Bu parametreleri ayarlama son bölümünde ele alınmıştır.

Sunulan sorguları örnekler için belirli [NYC ücreti seyahat veri](http://chriswhong.com/open-data/foil_nyc_taxi/) senaryoları burada da sunulmaktadır [GitHub deposunu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Bu sorgular zaten belirtilen veri şeması varsa ve çalıştırmak için gönderilmesi hazırsınız. Son bölümünde kullanıcılar ayarlayabilirsiniz ve böylece Hive sorguları performansı artırılabilir parametreleri de ele alınmıştır.

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Bu **menü** özellikleri veriler için çeşitli ortamlar oluşturmak nasıl açıklayan konulara bağlantılar. Bu görev bir adımdır [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, sahip olduğunuz varsayılmaktadır:

* Bir Azure depolama hesabı oluşturuldu. Yönergeler gerekiyorsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Özelleştirilmiş bir Hadoop kümesine Hdınsight hizmetiyle sağlandı.  Yönergeler gerekiyorsa bkz [Advanced Analytics için Azure Hdınsight Hadoop kümeleri özelleştirme](customize-hadoop-cluster.md).
* Azure Hdınsight Hadoop kümeleri Hive tabloları için verileri karşıya yüklendi. Henüz gelmemiş izleyin [Hive tabloları oluşturma ve yük verileri](move-hive-tables.md) verileri ilk Hive tablolara yüklemek için.
* Küme uzak erişim etkin. Yönergeler gerekiyorsa bkz [Hadoop küme baş düğümü erişim](customize-hadoop-cluster.md).

## <a name="hive-featureengineering"></a>Özellik oluşturma
Bu bölümde, hangi özellikler Hive sorguları oluşturarak yolları bazı örnekleri açıklanmaktadır. Ek özellikler oluşturduktan sonra bunları varolan tablonun sütun olarak ekleyin veya özgün tabloyla katılabilir birincil anahtar ve ek özellikler ile yeni bir tablo oluşturun. Sunulan örnekler şunlardır:

1. [Sıklık tabanlı özelliği oluşturma](#hive-frequencyfeature)
2. [İkili sınıflandırma kategorik değişkenlerin riskleri](#hive-riskfeature)
3. [Datetime alanından özellikleri Ayıkla](#hive-datefeatures)
4. [Metin alanından özellikleri Ayıkla](#hive-textfeatures)
5. [GPS koordinatları arasındaki uzaklığı Hesapla](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Sıklık tabanlı özelliği oluşturma
Genellikle, bir kategorik değişken düzeylerini sıklıkları veya birden çok kategorik değişkenleri düzeylerinden belirli birleşimlerini sıklıkları hesaplamak kullanışlıdır. Kullanıcılar bu sıklıklarını hesaplamak için aşağıdaki betiği kullanabilir:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>İkili sınıflandırma kategorik değişkenlerin riskleri
Yalnızca kullanılan modelleri sayısal özellikleri zaman ikili sınıflandırmasında sayısal olmayan kategorik değişkenleri sayısal özelliklerini dönüştürülmesi gerekir. Bu dönüştürme ile sayısal bir risk her sayısal olmayan düzeyi değiştirerek yapılır. Bu bölümde bir kategorik değişkenin risk değerleri (günlük büyük olasılıkla) hesaplama bazı genel Hive sorguları gösterir.

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Bu örnekte, değişkenleri `smooth_param1` ve `smooth_param2` verilerden hesaplanan risk değerlerinin düzgün şekilde ayarlayın. Risk -INF INF arasındaki aralığı yok. Risk > 0 hedef 1'e eşit olduğunu olasılık 0,5 büyük olduğunu gösterir.

Risk sonra tablosu hesaplanan, kullanıcıların risk değerlerinin bir tabloya risk tabloyla birleştirerek atayabilirsiniz. Hive katılma sorgusu, önceki bölümde sağlandı.

### <a name="hive-datefeatures"></a>Datetime alanlardan özellikleri Ayıkla
Hive, datetime alanları işlemek için UDF'ler kümesiyle birlikte gelir. Kovanında, varsayılan datetime biçimidir ' yyyy-aa-gg 00:00:00 ' (' 1970'ten-01-01 12:21:32 ' gibi). Bu bölümde, gün, ay, bir datetime alanı month ayıklamak örnekler ve varsayılan biçiminde bir tarih/saat dizesine varsayılan biçimlendirin dışında tarih saat biçiminde bir dize olarak bir dönüştürme diğer örnekleri gösterir.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Bu Hive sorgusu varsayar *<datetime field>* varsayılan tarih saat biçiminde değil.

Bir datetime alanı varsayılan biçiminde değilse, datetime alanı UNIX zaman damgası dönüştürmeniz ve UNIX zaman damgası varsayılan biçiminde bir tarih saat dizeye dönüştürmeniz gerekir. Datetime biçim varsayılan olduğunda, kullanıcılar katıştırılmış datetime özellikleri ayıklamak için UDF'ler uygulayabilir.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Bu sorgu, *<datetime field>* gibi düzeni sahip *03/26/2015 12:04:39*,  *<pattern of the datetime field>'* olmalıdır `'MM/dd/yyyy HH:mm:ss'`. Test etmek için kullanıcıların çalıştırabileceği

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* kümeleri sağlandığında bu sorguda tüm Azure Hdınsight Hadoop kümeleri üzerinde varsayılan olarak önceden yüklenmiş olarak gelir.

### <a name="hive-textfeatures"></a>Metin alanları özellikleri Ayıkla
Hive tablosu boşluklarla ayrılmış sözcükler dizesi içeren bir metin alanı olduğunda, aşağıdaki sorgu dizesi ve dizesindeki sözcük sayısını uzunluğu ayıklar.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>GPS koordinat kümeleri arasındaki uzaklıkları Hesapla
Bu bölümde verilen sorgu NYC ücreti seyahat verilere doğrudan uygulanabilir. Bu sorgu amacı özellikleri oluşturmak için Hive katıştırılmış bir matematik işlevinde uygulamak nasıl göstermektir.

Bu sorguda kullanılan adlı, toplama ve dropoff konumları GPS koordinatları alanları *toplama\_boylam*, *toplama\_enlem*, *dropoff\_boylam*, ve *dropoff\_enlem*. Toplama ve dropoff koordinatları arasında doğrudan uzaklığı hesaplamak sorgular şunlardır:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

İki GPS koordinatları arasındaki uzaklığı hesaplamak matematiksel denklemini bulunabilir <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">taşınabilir tür betikleri</a> Peter Lapisu tarafından yazılan site. Bu Javascript işlevi içinde `toRad()` tıpkı *lat_or_lon*pi/180 *, radyan için derece dönüştürür. Burada, *lat_or_lon* enlem veya boylam. Hive işlevi sağlamadığından `atan2`, ancak işlev sağlar `atan`, `atan2` işlevi tarafından gerçekleştirilir `atan` sağlanan tanımı kullanılarak yukarıdaki Hive sorgusu işlevinde <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Çalışma alanı oluştur](./media/create-features-hive/atan2new.png)

Katıştırılmış UDF'ler bulunabilir Hive tam listesi **yerleşik işlevler** bölümünde <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a> Gelişmiş konular: Sorgu hızını artırmak için ayarlama Hive parametreleri
Varsayılan parametre ayarları Hive kümesinin Hive sorguları ve sorguları işlerken veri için uygun olmayabilir. Bu bölümde, kullanıcıların Hive sorguları performansını artırmak için ayarlayabilirsiniz bazı parametreler açıklanmaktadır. Kullanıcıların veri işleme sorguları önce sorguları ayarlama parametresini eklemeniz gerekir.

1. **Java yığın alanı**: büyük veri kümeleri katılma veya uzun kayıtlarının işlenmesinden içeren sorgular için **yığın alana sahip** sık karşılaşılan biridir. Bu hata parametrelerini ayarlayarak önlenebilir *mapreduce.map.java.opts* ve *mapreduce.task.io.sort.mb* istenen değerleri için. Örnek aşağıda verilmiştir:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Bu parametre, Java yığın alanı için 4 GB bellek ayırır ve ayrıca sıralama daha verimli daha fazla bellek ayırarak hale getirir. Başarısızlık hatalarını yığın alanı ilgili herhangi bir işi varsa bu ayırma ile yürütmek için iyi bir fikirdir.

1. **DFS bloğu boyutunu**: Bu parametre en küçük birim dosya sistemi depolar veri ayarlar. DFS blok boyutu 128 MB, ardından tüm veri boyutuna ve kadar küçükse örnek olarak, tek bir blok 128 MB depolanır. 128 MB ek blokları ayrılan büyük veriler. 
2. Dosyaya ilgili ilgili blok bulmak için çok daha fazla isteklerini işlemek ad düğümü olduğu için bir küçük blok boyutu seçme büyük ek yüklerini Hadoop neden olur. Bir önerilen ilgilenme gigabayt ile (veya daha büyük olduğunda) ayarı veriler:

        set dfs.block.size=128m;

2. **Hive katılma işleminde en iyi duruma getirme**: birleştirme işlemleri harita/azaltın Framework'te azaltın aşamasında, bazen yer genellikle alırken muazzam kazançlar ("mapjoins" olarak da bilinir) harita aşamasında birleştirmeler zamanlayarak sağlanabilir. Mümkün olduğunda bunun için Hive yönlendirecek şekilde ayarlayın:
   
       set hive.auto.convert.join=true;

3. **Hive mappers sayısını belirterek**: sırada Hadoop reducers sayısını ayarlamak kullanıcı sağlar, mappers sayısı genellikle kullanıcı tarafından değil ayarlanabilir. Bu sayı denetiminde bazı derecesini izin veren bir eli Hadoop değişkenleri seçmektir *mapred.min.split.size* ve *mapred.max.split.size* her eşleme boyutu görev tarafından belirlenir:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Genellikle, varsayılan değeri:
    
    - *mapred.Min.split.size* 0 ' dır,
    - *mapred.max.split.size* olan **Long.MAX** ve 
    - *DFS.Block.size* 64 MB'tır.

    Veri boyutu verilen görebiliriz gibi "ayarlayarak" Bu parametreleri ayarlama sağlar bize kullanılan mappers sayısını ayarlamak.

4. İşte birkaç daha **Gelişmiş Seçenekler** Hive performansını iyileştirmek için. Bu harita ve görevleri azaltmak için ayrılan bellek ayarlamanıza olanak sağlar ve performans uyguladıkça yararlı olabilir. Aklınızda *mapreduce.reduce.memory.mb* Hadoop kümesindeki her bir çalışan düğümünün fiziksel bellek boyutundan büyük olamaz.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

