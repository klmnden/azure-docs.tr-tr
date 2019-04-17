---
title: Azure HDInsight 4.0 genel bakış
description: HDInsight 3.6 ile HDInsight 4.0 özelliklerinin karşılaştırılması, sınırlamalar ve yükseltme önerileri.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.topic: overview
ms.date: 04/15/2019
ms.openlocfilehash: af9f8a9e4d67c74910f60c70a0aee5c2439d6209
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609209"
---
# <a name="azure-hdinsight-40-overview"></a>Azure HDInsight 4.0 genel bakış

Azure HDInsight, Azure üzerinde açık kaynaklı Apache Hadoop ve Apache Spark analiz için en popüler hizmetler Kurumsal müşteriler arasında biridir. HDInsight (HDI) 4.0 Apache Hadoop bileşenlerinin bulut dağıtımıdır [Hortonworks Data Platform (HDP) 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html). Bu makalede en güncel Azure HDInsight sürümü hakkında bilgiler verilmekte ve yükseltme yöntemleri anlatılmaktadır.

## <a name="whats-new-in-hdi-40"></a>HDI 4.0'daki yenilikler nelerdir?

### <a name="apache-hive-30-and-llap"></a>3.0 Apache Hive ve LLAP

Apache Hive düşük gecikme süreli analitik işlem (LLAP) kalıcı sorgu sonuçları uzak bulut depolama alanında veriler üzerinde sorgu sunucuları ve bellek içi önbelleğe alma hızlı SQL sunmak için kullanır. Hive LLAP, Hive sorgularını parçalar halinde yürüten bir dizi kalıcı daemon'lardan faydalanır. LLAP üzerinde sorgu yürütme LLAP kullanılmayan Hive ile benzerdir ve çalışan görevleri kapsayıcıların değil LLAP daemon'larının içinde çalışır.

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


### <a name="apache-oozie"></a>Apache Oozie

HDI 4.0 sürümünde bulunan Apache Oozie 4.3.1'de aşağıdaki değişiklikler yapılmıştır:

* Oozie artık Hive eylemlerini çalıştırmaz. Hive CLI kaldırılmış ve yerine BeeLine getirilmiştir.

* **job.properties** dosyanıza bir hariç tutma deseni ekleyerek istenmeyen bağımlılıkları paylaşma kitaplığından hariç tutabilirsiniz.

[Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html) hakkında daha fazla bilgi edinin.

## <a name="how-to-upgrade-to-hdi-40"></a>HDI 4.0 sürümüne yükseltme

Tüm ana sürümlerde olduğu gibi son sürümü üretim ortamına uygulamadan önce bileşenlerinizi ayrıntılı bir testten geçirmeniz önemlidir. HDI 4.0, yükseltme için kullanılabilir durumdadır ancak yanlışlıkları önlemek için HDI 3.6 varsayılan seçenek olarak korunmuştur.

Önceki HDI sürümlerinden HDI 4.0 sürümüne desteklenen bir yükseltme yolu yoktur. Meta veri deposu ve blob veri biçimleri değiştirildiğinden HDI 4.0 önceki sürümlerle uyumlu değildir. Yeni HDI 4.0 ortamınızı geçerli üretim ortamınızdan ayrı tutmanız önemlidir. HDI 4.0 sürümünü geçerli ortamınıza dağıtmanız halinde meta veri deponuz yükseltilir ve bu işlem geri alınamaz.  

## <a name="limitations"></a>Sınırlamalar

* HDI 4.0, MapReduce desteği sunmaz. Apache Tez kullanın. [Apache Tez](https://tez.apache.org/) hakkında daha fazla bilgi edinin.

* Hive View, HDI 4.0 sürümünde mevcut değildir. 

* Apache Zeppelin içindeki kabuk yorumlayıcı, Spark ve Etkileşimli Sorgu kümelerinde desteklenmez.

* Spark-LLAP kümesinde LLAP özelliğini *devre dışı* bırakamazsınız. LLAP özelliğini yalnızca kapatabilirsiniz.

* Azure Data Lake Storage Gen2, Juypter notebook'larını Spark kümesine kaydedemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight Belgeleri](index.yml)
* [Sürüm notları](hdinsight-release-notes.md)
