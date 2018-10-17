---
title: HDInsight 4.0 sürümüne genel bakış (Önizleme) - Azure
description: HDInsight 3.6 ile HDInsight 4.0 özelliklerinin karşılaştırılması, sınırlamalar ve yükseltme önerileri.
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: overview
ms.date: 10/04/2018
ms.openlocfilehash: ade162d0261b765336cbff9ea8a6429f9bd2d871
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801852"
---
# <a name="hdinsight-40-overview-preview"></a>HDInsight 4.0 sürümüne genel bakış (Önizleme)

Azure HDInsight, Azure'da açık kaynak Hadoop ve Spark analizi için kurumsal müşteriler tarafından en çok tercih edilen hizmetlerden biridir. HDInsight (HDI) 4.0, [Hortonworks Data Platform (HDP) 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html) tarafından sağlanan Hadoop bileşenlerinin bulut dağıtımıdır. Bu makalede en güncel Azure HDInsight sürümü hakkında bilgiler verilmekte ve yükseltme yöntemleri anlatılmaktadır.

## <a name="whats-new-in-hdi-40"></a>HDI 4.0'daki yenilikler nelerdir?

### <a name="hive-30-and-llap"></a>Hive 3.0 ve LLAP

Hive düşük gecikme süresine sahip analitik işlem (LLAP) özelliği uzak bulut depolama alanlarındaki verilerde hızlı SQL sorgusu sonucu sunmak için kalıcı sorgu sunucuları ve bellek için önbellek kullanır. Hive LLAP, Hive sorgularını parçalar halinde yürüten bir dizi kalıcı daemon'lardan faydalanır. LLAP üzerinde sorgu yürütme LLAP kullanılmayan Hive ile benzerdir ve çalışan görevleri kapsayıcıların değil LLAP daemon'larının içinde çalışır.

Hive LLAP hizmetinin avantajları şunlardır:

* Performans ve ölçeklenebilirlikten ödün vermeden karmaşık birleşimler, alt sorgular, pencereleme işlevleri, sıralama, kullanıcı tanımlı işlevler ve karmaşık toplamalar gibi ayrıntılı SQL analiz işlemlerini gerçekleştirme becerisi.

* Etkileşimli sorguların verilerin hazırlandığı depolama alanındaki verilerle gerçekleştirilerek analitik işlem için verilerin depolamadan başka bir altyapıya taşınması ihtiyacınız ortadan kaldırma.

* Sorgu sonuçlarını önbelleğe alarak önceden hesaplanan sorgu sonuçlarının yeniden kullanılmasıyla sorgu için gerekli küme görevlerinin çalıştırılması için gerekli sürenin ve kaynakların azaltılması.

### <a name="hive-dynamic-materialized-views"></a>Hive dinamik gerçekleştirilmiş görünümleri

Hive artık veri ambarlarında sorgu işleme süreçlerini hızlandırmak için kullanılan dinamik gerçekleştirilmiş görünümleri veya ilgili özetlerin önceden hesaplanmasını desteklemektedir. Gerçekleştirilmiş görünümler yerel Hive ortamında depolanabilir ve LLAP hızlandırmasından sorunsuz bir şekilde faydalanabilir.

### <a name="hive-transactional-tables"></a>Hive işlem tabloları

HDI 4.0, Hive ambarında bulunan işlem tabloları için bölünmezlik, tutarlılık, bağımsızlık ve kalıcılık (ACID) uyumluluğu gerektiren Apache Hive 3 bileşenine sahiptir. ACID uyumluluğuna sahip tablolar ve tablo verileri için erişim ve yönetim Hive tarafından gerçekleştirilir. Oluşturma, alma, güncelleştirme ve silme (CRUD) tablolarındaki verilerin İyileştirilmiş Satır Sütunu (ORC) dosya biçiminde olması gerekir ancak yalnızca ekleme yapılabilen tablolar tüm dosya biçimlerini destekler.

* ACID v2 hem depolama biçimi hem de yürütme altyapısı alanında performans geliştirmelerine sahiptir. 

* ACID, veri güncelleştirmelerine tam destek sunmak için varsayılan olarak etkinleştirilir.

* Geliştirilmiş ACID özellikleri sayesinde satır düzeyinde güncelleştirme ve silme işlemi gerçekleştirebilirsiniz.

* Bu durum Performans açısından ek yük oluşturmaz.

* Gruplandırma gerekli değildir.

* Spark, Hive Warehouse Connector ile Hive ACID tablolarında veri okuma ve yazma işlemleri gerçekleştirebilir.

[Apache Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/hive-overview/content/hive_whats_new_in_this_release_hive.html) hakkında daha fazla bilgi edinin.

### <a name="apache-spark"></a>Apache Spark

Apache Spark, güncelleştirilebilir tabloları ve ACID işlemlerini Hive Warehouse Connector ile alır. Hive Warehouse Connector, tam işlevlere erişmek için Hive işlem tablolarını Spark'ta dış tablo olarak kaydetmenize izin verir. Önceki sürümler yalnızca tablo bölümü değiştirmeyi destekliyordu. Hive Warehouse Connector ayrıca okuma ve yazmaları işlem akışına dönüştürmek ve Spark'tan Hive tablosu akışı gerçekleştirmek için Streaming DataFrames desteği sunar.

Spark yürütücüleri doğrudan Hive LLAP daemon'larına bağlanarak verileri işlemsel bir şekilde alabilir ve bu sayede verilerin denetimi Hive'da kalır.

HDInsight 4.0'da Apache Spark şu senaryoları destekler:

* Raporlama için kullanılan işlem tablosunda makine öğrenmesi modeli eğitimi çalıştırma.
* ACID işlemlerini kullanarak Spark ML'den Hive tablosuna güvenli bir şekilde sütun ekleme.
* Hive akış tablosundaki değişiklik akışında bir Spark akış işi çalıştırma.
* Doğrudan bir Spark Yapılandırılmış Akış işinden ORC dosyası oluşturma.

Artık Hive işlem tablolarına yanlışlıkla doğrudan Spark'tan erişmeye çalışma ve bunun sonucunda tutarsız sonuçlar, yinelenen veriler veya veri bozulmasıyla karşı karşıya kalma konusunda endişelenmenize gerek yok. HDI 4.0'da Spark tabloları ile Hive tabloları ayrı meta veri depolarında saklanmaktadır. Hive Data Warehouse Connector ile Hive işlem tablolarını açıkça Spark dış tabloları olarak kaydedebilirsiniz.

[Apache Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html) hakkında daha fazla bilgi edinin.


### <a name="oozie"></a>Oozie

HDI 4.0 sürümünde bulunan Apache Oozie 4.3.1'de aşağıdaki değişiklikler yapılmıştır:

* Oozie artık Hive eylemlerini çalıştırmaz. Hive CLI kaldırılmış ve yerine BeeLine getirilmiştir.

* **job.properties** dosyanıza bir hariç tutma deseni ekleyerek istenmeyen bağımlılıkları paylaşma kitaplığından hariç tutabilirsiniz.

[Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html) hakkında daha fazla bilgi edinin.

## <a name="how-to-upgrade-to-hdi-40"></a>HDI 4.0 sürümüne yükseltme

Tüm ana sürümlerde olduğu gibi son sürümü üretim ortamına uygulamadan önce bileşenlerinizi ayrıntılı bir testten geçirmeniz önemlidir. HDI 4.0, yükseltme için kullanılabilir durumdadır ancak yanlışlıkları önlemek için HDI 3.6 varsayılan seçenek olarak korunmuştur.

Önceki HDI sürümlerinden HDI 4.0 sürümüne desteklenen bir yükseltme yolu yoktur. Meta veri deposu ve blob veri biçimleri değiştirildiğinden HDI 4.0 önceki sürümlerle uyumlu değildir. Yeni HDI 4.0 ortamınızı geçerli üretim ortamınızdan ayrı tutmanız önemlidir. HDI 4.0 sürümünü geçerli ortamınıza dağıtmanız halinde meta veri deponuz yükseltilir ve bu işlem geri alınamaz.  

## <a name="limitations"></a>Sınırlamalar

* HDI 4.0, MapReduce desteği sunmaz. Bunun yerine Tez kullanabilirsiniz. [Apache Tez](https://tez.apache.org/) hakkında daha fazla bilgi edinin.

* Hive View, HDI 4.0 sürümünde mevcut değildir. 

* Apache Zeppelin içindeki kabuk yorumlayıcı, Spark ve Etkileşimli Sorgu kümelerinde desteklenmez.

* Spark-LLAP kümesinde LLAP özelliğini *devre dışı* bırakamazsınız. LLAP özelliğini yalnızca kapatabilirsiniz.

* Azure Data Lake Storage Gen2, Juypter notebook'larını Spark kümesine kaydedemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight Belgeleri](index.yml)
* [Sürüm notları](hdinsight-release-notes.md)
