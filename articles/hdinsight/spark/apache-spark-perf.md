---
title: Performans - Azure Hdınsight için Spark işlerinin en iyi duruma getirme | Microsoft Docs
description: En iyi performansı Spark kümeleri için ortak stratejiler gösterilmektedir.
services: hdinsight
documentationcenter: ''
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: f35ed98efb26dfa0d75a57ca3646f567a7949dae
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164375"
---
# <a name="optimize-spark-jobs"></a>Spark işlerini en iyi duruma getirme

Spark küme yapılandırması, belirli iş yükü için en iyi duruma getirme hakkında bilgi edinin.  En sık karşılaşılan hatalı yapılandırmalar (özellikle yanlış ölçekli yürütücüler), uzun süre çalışan işlemleri ve Kartezyen işlemlerinde neden görevleri nedeniyle bellek baskısı iştir. İşlerini uygun önbelleğe alma ve için sağlayarak hızlandırabilirsiniz [veri eğme](#optimize-joins-and-shuffles). En iyi performans için izlemek ve uzun süre çalışan ve kaynak tüketen Spark iş yürütmeleri gözden geçirin.

Aşağıdaki bölümlerde, ortak Spark iş en iyi duruma getirme ve öneriler açıklanmaktadır.

## <a name="choose-the-data-abstraction"></a>Veri Özet seçin

Soyut veri ve Spark 1.x kullanır RDDs spark 2.x sunulan DataFrames ve veri kümeleri. Aşağıdaki göreli değeri göz önünde bulundurun:

* **DataFrames**
    * En iyi seçenek çoğu durumda
    * Sorgu iyileştirme Catalyst ile sağlar
    * Bütün aşama kodu oluşturma
    * Doğrudan bellek erişimi
    * Çöp toplama (GC) yükü düşük
    * Değil olarak Geliştirici dostu veri kümeleri, olarak herhangi bir derleme zamanı denetimleri veya etki alanı nesnesi programlama olduğundan
* **Veri kümeleri**
    * İyi performans etkisi kabul edilebilir olduğu karmaşık ETL ardışık düzende
    * Burada, performans etkisi önemli olabilir Toplamalarına iyi
    * Sorgu iyileştirme Catalyst ile sağlar
    * Etki alanı nesnesi programlama ve derleme zamanı denetimlerini sağlayarak Geliştirici dostu
    * Serileştirme/seri durumdan çıkarma ek yükü getirir
    * Yüksek GC ek yükü
    * Bütün aşamalı kod oluşturma keser.
* **RDDs**
    * Spark 2.x, gereksinim RDDs, kullanılacak yeni bir özel RDD oluşturmak gerekmedikçe
    * Hiçbir sorgu iyileştirme Catalyst ile
    * Hiçbir bütün aşama kodu oluşturma
    * Yüksek GC ek yükü
    * Spark 1.x eski kullanmalısınız API'leri

## <a name="use-optimal-data-format"></a>En iyi veri biçimini kullanın

Spark csv, json, xml, parquet, orc ve avro gibi birçok biçimlerini destekler. Spark uzatabilirsiniz pek çok daha fazla biçimi dış veri kaynaklarıyla - daha fazla bilgi için destek, görmek için [Spark paketleri](https://spark-packages.org).

En iyi performans için parquet ile biçimdir *snappy sıkıştırma*, Spark varsayılan 2.x. Parquet sütun biçiminde veri depolar ve Spark getirilmiş.

## <a name="select-default-storage"></a>Varsayılan depolama seçin

Yeni bir Spark kümesi oluşturduğunuzda, Azure Blob Storage veya Azure Data Lake Store, kümenin varsayılan depolama alanı olarak seçmek için seçeneğiniz vardır. Kümenizi sildiğinizde, verilerinizi otomatik olarak silinmez için her iki seçenek, uzun vadeli depolama yararı geçici kümeleri için verin. Geçici bir küme oluşturun ve yine de verilerinizi erişebilir.

| Depo türü | Dosya Sistemi | Hız | Geçici | Kullanım örnekleri |
| --- | --- | --- | --- | --- |
| Azure Blob Depolama | **wasb:**//url/ | **Standart** | Evet | Geçici küme |
| Azure Data Lake Store | **Adl:**//url/ | **Daha hızlı** | Evet | Geçici küme |
| Yerel HDFS | **hdfs:**//url/ | **Hızlı** | Hayır | Etkileşimli 7/24 küme |

## <a name="use-the-cache"></a>Önbellek Kullan

Spark farklı yöntemlerle gibi kullanılabilen kendi yerel önbelleğe alma mekanizmaları sağlar `.persist()`, `.cache()`, ve `CACHE TABLE`. Bu yerel önbelleğe alma de küçük veri kümeleri için önbellek Ara sonuçların ihtiyaç duyacağınız ETL ardışık düzen olduğu gibi etkilidir. Önbelleğe alınan bir tablo bölümleme verileri saklamasa beri ancak, Spark yerel önbelleğe alma şu anda iyi bölümlendirme ile çalışmaz. Daha fazla genel ve güvenilir bir önbelleğe alma teknik *depolama katmanı önbelleğe alma*.

* Yerel Spark (önerilmez) önbelleğe alma
    * Küçük veri kümeleri için iyidir.
    * Hangi gelecekte Spark sürümlerde değişebilir bölümlendirme ile çalışmaz yapar.

* (Önerilen) depolama düzeyinde önbelleğe alma
    * Kullanılarak uygulanan [Alluxio](http://www.alluxio.org/).
    * Bellek içi ve SSD önbelleği kullanır.

* Yerel HDFS (önerilir)
    * `hdfs://mycluster` yolu.
    * SSD önbelleği kullanır.
    * Önbellek yeniden gerektiren küme sildiğinizde, önbelleğe alınan veriler kaybolur.

## <a name="use-memory-efficiently"></a>Verimli bellek kullanımı

Spark veri bellek, bellek kaynakları yönetme Spark işlerinin yürütülmesi en iyi duruma getirme önemli nokta olacak şekilde koyarak çalışır.  Kümenizin bellek kullanabildiğinden uygulayabileceğiniz birçok tekniği vardır.

* Daha küçük veri bölümlerini tercih ve veri boyutu, türleri ve bölümleme stratejinizi dağıtımlarında hesap.
* Yeni, daha verimli göz önünde bulundurun [Kryo veri seri hale getirme](https://github.com/EsotericSoftware/kryo), varsayılan Java serileştirme yerine.
* Kendisini ayıran gibi YARN kullanmayı tercih `spark-submit` toplu olarak.
* İzleme ve Spark yapılandırma ayarlarını yapma.

Başvuru için sonraki görüntüde Spark bellek yapısı ve bazı anahtar Yürütücü bellek parametreler gösterilmektedir.

### <a name="spark-memory-considerations"></a>Spark bellek konuları

YARN kullanıyorsanız, YARN her Spark düğümdeki tüm kapsayıcıları tarafından kullanılan bellek maksimum toplamını denetler.  Aşağıdaki diyagramda, anahtar nesneleri ve bunların ilişkileri gösterir.

![YARN Spark bellek yönetimi](./media/apache-spark-perf/yarn-spark-memory.png)

'Yetersiz bellek' iletiler için adres deneyin:

* DAG yönetim seçeneği gözden geçirin. Harita tarafı reducting tarafından azaltmanıza, önceden bölüm (veya bucketize) kaynak verileri, tek seçeneği en üst düzeye çıkarmak ve gönderilen veri miktarını azaltın.
* Tercih `ReduceByKey` sabit bellek sınırına ile `GroupByKey`, toplamalar, Pencereleme ve diğer işlevleri sağlar ancak ann sınırsız bellek sınırı vardır.
* Tercih `TreeReduce`, hangi mu yürütücüler veya bölümleri hakkında daha fazla iş için `Reduce`, sürücüsündeki tüm iş yapar.
* Alt düzey RDD nesneleri yerine DataFrames yararlanın.
* "İlk N", çeşitli toplamalar veya Pencereleme işlemleri gibi eylemleri kapsülleyen Complextype'lar oluşturun.

## <a name="optimize-data-serialization"></a>Veri seri hale getirme en iyi duruma getirme

Spark işlerinin dağıtılır, bu en iyi performans için uygun veri seri hale getirme önemlidir.  Spark için iki seri hale getirme seçeneği vardır:

* Java serileştirme varsayılandır.
* Kryo serileştirme daha yeni bir biçimidir ve daha hızlı neden olabilir ve daha fazla seri hale getirme Java sıkıştırın.  Kryo programınıza sınıflarını Kaydet ve tüm seri hale getirilebilir türler henüz desteklemiyor gerektirir.

## <a name="use-bucketing"></a>Bucketing kullanın

Bucketing veri bölümlendirme için benzer, ancak her demet sütun değerleri kümesi yerine tek basılı tutabilirsiniz. Ürün tanımlayıcıları gibi değerler (içinde milyon veya daha fazla) çok sayıda bölümleme için iyi bucketing çalışır. Bir demet satırın demet anahtarı karma tarafından belirlenir. Nasıl bunlar kümelenmiş sıralanır ve ilgili meta verileri depolamak için kümelenmiş tabloları benzersiz iyileştirmeleri sunar.

Bazı gelişmiş bucketing özellikler şunlardır:

* Sorgu iyileştirme bucketing meta bilgilere göre
* En iyi duruma getirilmiş toplamalar
* En iyi duruma getirilmiş birleşimler

Bölümlendirme ve aynı anda bucketing kullanabilirsiniz.

## <a name="optimize-joins-and-shuffles"></a>Birleştirmeler ve seçeneği en iyi duruma getirme

Bir birleştirme veya karışık yavaş işleri varsa, büyük olasılıkla nedeni *veri eğme*, iş verilerinizi asimetri olduğu. Örneğin, bir harita işi 20 saniye sürebilir ancak burada veri alanına karışık veya bir iş saat sürer.   Veri eğme düzeltmek için tüm anahtarı salt veya kullanmalısınız bir *yalıtılmış salt* yalnızca bazı alt anahtarlar için.  Yalıtılmış bir veri dizesi kullanıyorsanız, alt eşlemi birleşimlerde güvenlik anahtarların yalıtmak için daha fazla filtrelemek. Başka bir seçenek, bir demet sütun getirir ve demet içinde ilk önceden toplama oluşturmaktır.

Başka bir etken yavaş birleştirmeler neden bir birleştirme türü olabilir. Varsayılan olarak, Spark kullanır `SortMerge` birleştirme türü. Bu tür birleşim büyük veri kümeleri için uygundur ancak, sol ve sağ kenarlarının veri birleştirme önce sıralamanız gerekir aksi halde pkı'ya pahalı olmasıdır.

A `Broadcast` birleştirme en uygun daha küçük veri kümeleri için veya bir birleşim tarafındaki diğer taraftaki çok daha küçük olduğu. Bu tür birleşim sütunlardan tüm yürütücüler yayınlar ve bu nedenle daha fazla bellek yayınlar için genel gerektirir.

Ayarlayarak yapılandırmanızda birleştirme türünü değiştirebilirsiniz `spark.sql.autoBroadcastJoinThreshold`, veya DataFrame API'lerini kullanarak bir birleşme ipucu ayarlayabilirsiniz (`dataframe.join(broadcast(df2))`).

```scala
// Option 1
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", 1*1024*1024*1024)

// Option 2
val df1 = spark.table("FactTableA")
val df2 = spark.table("dimMP")
df1.join(broadcast(df2), Seq("PK")).
    createOrReplaceTempView("V_JOIN")
sql("SELECT col1, col2 FROM V_JOIN")
```

Kümelenmiş tabloları kullanıyorsanız sonra bir birleştirme türü, üçüncü sahip `Merge` birleştirme. Doğru önceden bölümlenmiş ve önceden sıralanmış bir veri kümesi pahalı sıralama aşama atlar bir `SortMerge` birleştirme.

Birleştirmeler sırasını, daha karmaşık sorgularda özellikle önemlidir. En seçmeli birleştirmeler ile başlatın. Ayrıca, mümkün olduğunda toplamalar sonra satır sayısını artırmak birleştirmeler taşıyın.

Kartezyen birleşimler, özellikle durumunda paralellik yönetmek için iç içe geçmiş yapılar ekleyebilirsiniz Pencereleme ve belki de Spark işinizde bir veya daha fazla adımları atlayın.

## <a name="customize-cluster-configuration"></a>Küme yapılandırması özelleştirme

Spark küme yükünüzü bağlı olarak, belirleyebilir iyileştirilmiş Spark iş yürütme daha fazla bilgi için varsayılan olmayan bir Spark yapılandırma neden olur.  Varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek ile iş yüklerini sınama Kıyaslama gerçekleştirin.

Ayarlayabileceğiniz bazı ortak Parametreler şunlardır:

* `--num-executors` yürütücüler uygun sayısını ayarlar.
* `--executor-cores` Çekirdek sayısı için her Yürütücü ayarlar. Genellikle diğer işlemleri bazı kullanılabilir belleğin kullanma gibi middle-sized yürütücüler olması gerekir.
* `--executor-memory` yığın boyutu YARN üzerinde denetimleri her Yürütücü için bellek boyutu ayarlar. Yürütme yükü için bazı bellek bırakmanız gerekir.

### <a name="select-the-correct-executor-size"></a>Doğru Yürütücü boyutunu seçin

Yürütücü yapılandırmanızı karar verirken, Java çöp toplama (GC) yükünü göz önünde bulundurun.

* Yürütücü boyutunu azaltmak için faktörleri:
    1. GC genel gider < %10 tutmak için 32 GB aşağıda öbek boyutunu küçültün.
    2. GC genel gider < %10 tutmak için çekirdek sayısını azaltın.

* Yürütücü boyutunu artırmak için faktörleri:
    1. Ek yükü yürütücüler arasındaki iletişimi azaltın.
    2. Daha büyük kümelerinde yürütücüler (N2) arasında açık bağlantı sayısını azaltmak (> 100 yürütücüler).
    3. Bellek kullanımı yoğun görevler için uygun hale getirmek için yığın boyutunu artırın.
    4. İsteğe bağlı: Yürütücü başına bellek yükünü azaltın.
    5. İsteğe bağlı: kullanımı ve eşzamanlılık CPU oversubscribing tarafından artırın.

Genel Yürütücü boyutu seçerken altın kural:
    
1. Yürütücü 30 GB ile başlatın ve kullanılabilir makine çekirdek dağıtın.
2. Daha büyük kümeleri (100 > yürütücüler) için Yürütücü çekirdek sayısını artırın.
3. Artırın veya deneme çalıştırır ve GC yükünü gibi önceki faktörlere bağlı boyutları azaltın.

Eş zamanlı sorgu çalıştırırken, aşağıdakileri göz önünde bulundurun:

1. 30 GB ile yürütücü ve tüm makine çekirdek başlatın.
2. CPU oversubscribing tarafından birden çok paralel Spark uygulamaları oluşturmak (yaklaşık % 30 gecikme geliştirme).
3. Sorguları paralel uygulamalar arasında dağıtın.
4. Artırın veya deneme çalıştırır ve GC yükünü gibi önceki faktörlere bağlı boyutları azaltın.

Zaman Çizelgesi Görünümü, SQL grafiği, Proje istatistikleri ve benzeri bakarak sorgu performansınız aykırı değerlerini veya diğer performans sorunlarını izleyin. Bazen bir veya birkaç yürütücüler diğerlerinden daha yavaştır ve görevleri çalıştırmak için daha uzun sürer. Bu, daha geniş kümeleri (> 30 düğümler) sık gerçekleşir. Bu durumda, Zamanlayıcı yavaş görevlerde dengeleyebilirsiniz şekilde iş görevlerinin daha büyük bir sayıya bölün. Örneğin, en az iki kez olarak pek çok görev Yürütücü çekirdek sayısı olarak uygulamada gerekir. Görevlerle kurgusal yürütülmesi etkinleştirebilirsiniz `conf: spark.speculation = true`.

## <a name="optimize-job-execution"></a>İş yürütme en iyi duruma getirme

* Örneğin, verileri iki kez kullanın ve sonra onu önbelleğe gerekirse, önbelleğe alır.
* Değişkenleri tüm yürütücüler yayınlayın. Değişkenleri yalnızca bir kez daha hızlı arama sonucu, serileştirilir.
* İş parçacığı havuzu çoğu görevler için daha hızlı işlem sonuçlanır sürücüsü kullanın.

Çalışan işleriniz performans sorunları için düzenli olarak izleyin. Belirli sorunları hakkında daha fazla bilgi gerekiyorsa, aşağıdaki performans araçları profil birini göz önünde bulundurun:

* [Intel PAL aracı](https://github.com/intel-hadoop/PAT) CPU, depolama ve ağ bant genişliği kullanımını izler.
* [Oracle Java 8 Mission Control](http://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) Spark ve yürütücü kod profilleri.

Spark 2.x sorgu performansı üzerinde tam aşama kodu oluşturma bağlıdır Tungsten altyapısı anahtarıdır. Bazı durumlarda, tam aşamalı kod oluşturma devre dışı olabilir. Örneğin, değişebilir olmayan bir tür kullanın (`string`) toplama ifadesindeki `SortAggregate` yerine görünür `HashAggregate`. Örneğin, daha iyi performans için aşağıdakileri deneyin ve kod oluşturma yeniden etkinleştirin:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Sonraki adımlar

* [Çalışan Azure Hdınsight'ta Spark işleri hata ayıklama](apache-spark-job-debugging.md)
* [Hdınsight'ta Spark küme kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Uzak bir Spark kümesi göndermek için Spark REST API'sini kullanın](apache-spark-livy-rest-interface.md)
* [Spark ayarlama](https://spark.apache.org/docs/latest/tuning.html)
* [Aslında, Spark ayarlamak için bunu işler nasıl çalışır](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Kryo seri hale getirme](https://github.com/EsotericSoftware/kryo)
