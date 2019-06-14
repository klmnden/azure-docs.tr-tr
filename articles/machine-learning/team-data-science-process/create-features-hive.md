---
title: Bir Hadoop kümesi - Team Data Science Process verilerin özelliklerini oluşturma
description: Bir Azure HDInsight Hadoop kümesinde depolanan verilerin özelliklerini oluşturma Hive sorgularının örnekleri.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/21/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a491f923d7755513d84adfe765d595a3a7a80715
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60399355"
---
# <a name="create-features-for-data-in-a-hadoop-cluster-using-hive-queries"></a>Bir Hadoop kümesinde Hive sorgularını kullanarak verilerin özelliklerini oluşturma
Bu belge, Hive sorgularını kullanarak bir Azure HDInsight Hadoop kümesinde depolanan verilerin özelliklerini oluşturma işlemi gösterilmektedir. Bu Hive sorguları katıştırılmış Hive User-Defined betikleri, sağlanan işlevler (UDF'ler) kullanın.

Özellikler oluşturmak için gereken işlemleri, bellek kullanımı yoğun olabilir. Hive sorgu performansı bu gibi durumlarda daha önemli hale gelir ve belirli parametreleri ayarlayarak geliştirilebilir. Bu parametreleri ayarlama son bölümde ele alınmıştır.

Sunulan sorgularının örnekleri için belirli [NYC taksi seyahat verilerini](https://chriswhong.com/open-data/foil_nyc_taxi/) senaryoları burada da sunulmaktadır [GitHub deposu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Bu sorgular zaten belirtilen veri şemasına sahip ve çalıştırmak için gönderilmeye hazır. Son bölümde, kullanıcılar ayarlayabilirsiniz ve böylelikle Hive sorgu performansı artırılabilir parametreleri de ele alınmıştır.

Bu görev bir adımdır [Team Data Science işlem (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md)
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi hazırlandı.  Yönergelere ihtiyacınız varsa bkz [Gelişmiş analiz için Azure HDInsight Hadoop kümelerini özelleştirin](customize-hadoop-cluster.md).
* Azure HDInsight Hadoop kümeleri Hive tablolarında için verileri karşıya yüklendi. Henüz yoksa izleyin [Hive tabloları oluşturma ve yük verileri](move-hive-tables.md) verileri ilk Hive tablolarına yükleme.
* Kümeye uzaktan erişim etkin. Yönergelere ihtiyacınız varsa bkz [Hadoop küme baş düğümüne erişmek](customize-hadoop-cluster.md).

## <a name="hive-featureengineering"></a>Özellik oluşturma
Bu bölümde, hangi özellikleri kullanarak Hive sorguları oluşturma yol çeşitli örneklerini açıklanmaktadır. Ek özellikler oluşturduktan sonra bunları mevcut tabloya sütun olarak ekleyin veya birincil anahtar, özgün tablonun katılabilir ve ek özellikler ile yeni bir tablo oluşturabilirsiniz. Sunulan örnekleri aşağıda verilmiştir:

1. [Sıklığa dayalı özellik oluşturma](#hive-frequencyfeature)
2. [İkili sınıflandırma kategorik değişkenlerinde risklerini](#hive-riskfeature)
3. [Datetime alanı özellikleri ayıklayın](#hive-datefeatures)
4. [Metin alanından özellikleri ayıklayın](#hive-textfeatures)
5. [GPS koordinatlarını arasındaki uzaklık hesaplayın](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Sıklığa dayalı özellik oluşturma
Genellikle, Kategorik bir değişken düzeyleri sıklığını ya da birden fazla kategorik değişken düzeylerinden belirli birleşimlerini sıklığını hesaplamak kullanışlıdır. Kullanıcılar bu frekansları hesaplamak için aşağıdaki betiği kullanabilirsiniz:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>İkili sınıflandırma kategorik değişkenlerinde risklerini
Yalnızca kullanılan modelleri sayısal özelliklerini alırken ikili Sınıflandırma, sayısal olmayan kategorik değişkenleri sayısal özelliklerini dönüştürülmesi gerekir. Bu dönüştürme, her sayısal olmayan düzeyi ile sayısal bir risk değiştirilerek gerçekleştirilir. Bu bölüm, Kategorik bir değişken (günlük ekledikçe) risk değerleri hesaplama genel bazı Hive sorguları gösterir.

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

Bu örnekte, değişken `smooth_param1` ve `smooth_param2` verilerden hesaplanan risk değerlerinin düzgün şekilde ayarlanmıştır. Riskler -INF INF arasındaki aralığı vardır. Bir risk > 0 hedefi 1'e eşit olduğunu olasılık 0,5 büyük olduğunu gösterir.

Risk sonra tablo hesaplanır, kullanıcıların risk değerlerinin bir tabloya risk tabloyla katılarak atayabilirsiniz. Hive katılan sorgu, önceki bölümde sağlanmadı.

### <a name="hive-datefeatures"></a>Özellikleri datetime alanları Ayıkla
Hive, datetime alanları işleme için bir UDF'ler kümesi ile birlikte gelir. Hive, varsayılan datetime biçimi ' yyyy-aa-gg 00:00:00 ' ('1970-01-01 12:21:32 ' gibi). Bu bölümde, bir ay, bir datetime alanı ayın gününü çıkarma örnekleri ve başka bir tarih saat dizesi için varsayılan biçimi varsayılan biçimlendirme dışında bir biçimde bir tarih/saat dizeye Dönüştür diğer örnekler gösterilmektedir.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Bu Hive sorgusu olduğunu varsayar  *\<datetime alanı >* varsayılan tarih/saat biçimi.

Bir datetime alanı varsayılan biçiminde değilse, datetime alanı Unix zaman damgası dönüştürmeniz ve varsayılan biçiminde olan bir tarih saat dizesi Unix zaman damgası dönüştürmek gerekir. Varsayılan tarih ve saat biçim olduğunda, kullanıcılar katıştırılmış tarih ve saat özellikleri ayıklanacak UDF'ler uygulayabilir.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Bu sorgu,  *\<datetime alanı >* desen gibi sahip *26/03/2015 12:04:39*,  *\<datetime alanı desenini >'* olmalıdır `'MM/dd/yyyy HH:mm:ss'`. Kullanıcılar, test etmek için çalıştırabilirsiniz

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* kümeleri sağlandığında bu sorgu tüm Azure HDInsight Hadoop kümelerinde varsayılan olarak önceden yüklenmiş olarak gelir.

### <a name="hive-textfeatures"></a>Özellikleri metin alanları Ayıkla
Hive tablosu boşluklarla ayrılmış sözcük içeren bir metin alanı varsa, aşağıdaki sorguyu dize ve dize sözcük sayısı uzunluğunu ayıklar.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>GPS koordinatlarını kümesi arasındaki uzaklıkları hesaplayın
Bu bölümde belirtilen sorgu için NYC taksi seyahat verilerini doğrudan uygulanabilir. Bu sorgu amacı özellikler oluşturmak için Hive içinde katıştırılmış bir matematiksel işlev uygulamak nasıl göstermektir.

Bu sorguda kullanılan adlı, toplama ve dropoff konumları GPS koordinatlarını alanlar *toplama\_boylam*, *toplama\_enlem*,  *dropoff\_boylam*, ve *dropoff\_enlem*. Toplama ve dropoff koordinatları arasında doğrudan uzaklık hesaplayın sorgular şunlardır:

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

İki GPS koordinatlarını arasındaki uzaklık hesaplayın matematik denklemlerini bulunabilir <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">taşınabilir tür betikleri</a> site, Peter Lapisu tarafından yazıldı. Bu Javascript işlevi olarak `toRad()` tıpkı *lat_or_lon*Dereceyi radyana dönüştürür pi/180,. Burada, *lat_or_lon* enlem veya boylam. Hive işlevi sağlamadığından `atan2`, ancak işlev sağlar `atan`, `atan2` işlevi tarafından gerçekleştirilir `atan` sağlanan tanımı kullanarak yukarıdaki Hive sorgusu işlevinde <a href="https://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Çalışma alanı oluşturma](./media/create-features-hive/atan2new.png)

Katıştırılmış UDF'ler bulunabilir Hive tam listesini **yerleşik işlevler** bölümünde <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a> Gelişmiş konular: Sorgu hızını artırmak için Hive parametrelerini ayarlama
Hive kümesinin varsayılan parametre ayarları Hive sorguları ve sorgular işlenirken veri için uygun olmayabilir. Bu bölümde, kullanıcılar Hive sorgularının performansını geliştirmek için dinleyebilirsiniz bazı parametreler açıklanmaktadır. Veri işleme sorgular önce sorguları ayarlama parametre eklemek kullanıcıların gerekir.

1. **Java yığın alanı**: Büyük veri kümelerini katılma veya uzun kayıtları işleme içeren sorgular için **yığın alanı kalmadı çalıştıran** sık karşılaşılan biridir. Bu hata, parametreleri ayarlayarak önlenebilir *mapreduce.map.java.opts* ve *mapreduce.task.io.sort.mb* için istenen değerleri. Örnek aşağıda verilmiştir:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Bu parametre, Java yığın alanı için 4 GB bellek ayırır ve ayrıca sıralama daha verimli daha fazla bellek ayırarak yapar. Yığın alanı ilgili bir başarısızlık hatalarını herhangi bir iş varsa bu ayırmaları ile yürütmek için iyi bir fikirdir.

1. **DFS bloğu boyutunu**: Bu parametre, en küçük birim dosya sistemi depolar veri ayarlar. DFS blok boyutu düşük ve en fazla 128 MB, ardından boyuttaki veriyi olursa örnek olarak, 128 MB tek bir blok içinde depolanır. 128 MB'den büyük veri, ek blokları atanır. 
2. Ad düğümü dosyasıyla ilgili blok bulmak için çok daha fazla isteklerini işlemek olduğundan küçük blok boyutu seçme büyük ek yüklerini Hadoop neden olur. Bir önerilen ilgilenme gigabayt ile (veya daha büyük olduğunda) ayarı veriler:

        set dfs.block.size=128m;

2. **Hive katılma işleminde en iyi duruma getirme**: Birleştirme işlemleri map/reduce Framework azaltın aşamasında, bazen bir yerde genellikle alırken çok büyük bir kazanç birleşimler ("mapjoins" olarak da bilinir) map aşamasında zamanlama tarafından gerçekleştirilebilir. Mümkün olduğunda bunu yapmak için Hive yönlendirmek için aşağıdakileri ayarlayın:
   
       set hive.auto.convert.join=true;

3. **Hive için azaltıcının sayısını belirten**: Hadoop genişletin sayısını ayarlamasına olanak sağlar, ancak genellikle azaltıcının sayısı olan kullanıcı tarafından ayarlanamaz. Bu sayı denetimde bir ölçüde veren el Hadoop değişkenleri seçmektir *mapred.min.split.size* ve *mapred.max.split.size* her eşleme boyutu görev tarafından belirlenir:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Genellikle, varsayılan değeri:
    
   - *mapred.Min.split.size* 0 ' dır,
   - *mapred.max.split.size* olduğu **Long.MAX** ve 
   - *DFS.Block.size* 64 MB'tır.

     Biz, veri boyutu verilen görebileceğiniz gibi "ayarı" tarafından bu parametreleri ayarlama sağlar bize kullanılan azaltıcının sayısını ayarlamak.

4. İşte birkaç daha **Gelişmiş Seçenekler** Hive performansını iyileştirme için. Bu harita ve görevleri azaltmak için ayrılan bellek ayarlamanıza olanak sağlar ve performans ince ayar yapma yararlı olabilir. Aklınızda *mapreduce.reduce.memory.mb* Hadoop kümesindeki her çalışan düğümüne fiziksel bellek boyutu büyük olamaz.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

