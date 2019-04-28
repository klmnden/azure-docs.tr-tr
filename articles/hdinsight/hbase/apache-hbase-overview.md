---
title: Azure HDInsight'ta HBase nedir?
description: Hadoop’ta oluşturulan bir NoSQL veritabanı olan HDInsight’ta Apache HBase’e giriş. Kullanım örnekleri hakkında bilgi edinin ve HBase’i diğer Hadoop kümeleriyle karşılaştırın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: d109a1a233aaaf1f3922561106caa15781e31e01
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114451"
---
# <a name="what-is-apache-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-apache-hadoop"></a>HDInsight, Apache HBase nedir: Apache Hadoop için BigTable benzeri özellikler sağlayan bir NoSQL veritabanı
[Apache HBase](https://hbase.apache.org/) üzerinde oluşturulan bir açık kaynak, NoSQL veritabanıdır [Apache Hadoop](https://hadoop.apache.org/) ve sonra Modellenen [Google BigTable](https://cloud.google.com/bigtable/). HBase, sütun aileleri tarafından veritabanında büyük miktarlardaki yapılandırılmamış ve yarı yapılandırılmış veriler için rasgele erişim ve güçlü tutarlılık sağlar.

Kullanıcı açısından bakıldığında, HBase, veritabanına benzer. Verileri satırlar ve sütunlar bir tablo biçiminde depolanır ve satır içindeki veriler sütun ailesi tarafından gruplandırılır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. Veri yedekleme, toplu işleme ve Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan diğer özelliklere dayanabilir.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Apache HBase, Azure HDInsight nasıl uygulanır?

HDInsight HBase, Azure ortamına tümleştirilmiş yönetilen bir küme olarak sunulur. Kümeler verileri doğrudan depolamak için yapılandırılmıştır [Azure depolama](./../hdinsight-hadoop-use-blob-storage.md) düşük gecikme süresi ve performans ve maliyet seçeneklerinde artan esneklik sağlar. Bu, müşterilerin milyonlarca uç noktadan gelen algılayıcı ve telemetri verilerini depolayan hizmetleri oluşturmak ve bu verileri Hadoop işleriyle çözümlemek üzere etkileşimli web siteleri oluşturmalarını sağlar. HBase ve Hadoop Azure’da büyük veri projeleri için yi başlangıç noktalarıdır; bunlar özellikle büyük veri kümeleriyle çalışmak üzere gerçek zamanlı uygulamaları etkinleştirebilir.

HDInsight uygulaması, tabloların otomatik parçalanmasını, okumalar ve yazmalar için yüksek tutarlılık ve otomatik yük devretme sağlamak için HBase’in ölçek genişletmeli mimarisinden yararlanır. Performans, okumalar için bellek içi önbelleğe alma ve yazmalar için yüksek verimlilikli akış tarafından geliştirilmiştir. HBase kümesi sanal ağda oluşturulabilir. Ayrıntılar için bkz. [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Veriler HDInsight HBase’de nasıl yönetilir?
Veriler HBase kabuğunda `create`, `get`, `put` ve `scan` komutları kullanılarak HBase tarafından yönetilebilir. Veriler `put` kullanılarak veritabanına yazılır ve `get` kullanarak okunur. `scan` komutu, bir tablodaki birden çok satırdaki verileri almak için kullanılır. Veriler, HBase REST API’sinin üstünde bir istemci kitaplığı sağlayan HBase C# API’si kullanılarak da yönetilebilir. Bir HBase veritabanı kullanarak da sorgulanabilir [Apache Hive](https://hive.apache.org/). Bu programlama modellerine giriş için bkz [HDInsight, Apache Hadoop ile Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md). Veritabanı barındıran düğümlerde veri işlemeye olanak sağlayan ortak işlemciler de kullanılabilir.

> [!NOTE]  
> Thrift, HDInsight’ta HBase tarafından desteklenmez.

## <a name="scenarios-use-cases-for-apache-hbase"></a>Senaryolar: Apache HBase kullanım örnekleri
Web araması olan kurallı kullanım örneği bigtable (ve uzantılarının, HBase) oluşturuldu. Arama motorları terimleri bunları içeren web siteleriyle eşleştiren dizinler oluşturur. Ancak HBase için uygun olan diğer birçok kullanım örneği vardır; bunların birkaçı bu bölümde listelenmektedir.

* Anahtar değeri deposu
  
    HBase bir anahtar değeri deposu olarak kullanılabilir ve ileti sistemlerini yönetmeye uygundur. Facebook kendi ileti sistemi için HBase kullanır ve Internet iletişimlerini depolamak ve yönetmek için idealdir. WebTable web sayfalarından çıkarılan tabloları aramak ve yönetmek için HBase kullanır.
* Algılayıcı verileri
  
    HBase çeşitli kaynaklardan artımlı olarak toplanan verileri yakalamak için yararlıdır. Bu, sosyal analizler, zaman serileri, etkileşimli panoları eğilimler ve sayaçlar ile güncel tutma ve denetim günlüğü sistemlerini yönetmeyi içerir. Örnek olarak, Bloomberg tüccar terminali ve sunucu sistemlerinin durumuna ilişkin toplanan ölçümleri toplayan ve bunlara erişim imkanı sağlayan Open Time Series Veritabanı (OpenTSDB) verilebilir.
* Gerçek zamanlı sorgu
  
    [Apache Phoenix](https://phoenix.apache.org/) Apache HBase için bir SQL sorgu alt yapısıdır. Buna JDBC sürücüsü olarak erişilir ve bu SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar.
* Bir platform olarak HBase
  
    Uygulamalar, bir veri deposu olarak kullanarak HBase’in üstünde çalışabilir. Örnekler, Phoenix, [OpenTSDB](http://opentsdb.net/), Kiji ve Titan. Uygulamalar HBase ile de tümleştirebilir. Örnekler [Apache Hive](https://hive.apache.org/), [Apache Pig](https://pig.apache.org/), [Solr](https://lucene.apache.org/solr/), [Apache Storm](https://storm.apache.org/), [Apache Flume](https://flume.apache.org/), [ Apache Impala](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/) , [Ganglia](http://ganglia.info/), ve [Apache ayrıntıya](https://drill.apache.org/).

## <a name="next-steps"></a>Sonraki adımlar
* [HDInsight, Apache Hadoop ile Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md)
* [HDInsight Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
* [HDInsight (Hadoop) ile Apache HBase kullanan Java uygulamaları oluşturmak için Apache Maven kullanma](./apache-hbase-build-java-maven-linux.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Apache HBase](https://hbase.apache.org/)
* [Apache HBase Başvuru Kılavuzu](https://hbase.apache.org/book.html)
* [Bigtable: Yapılandırılmış veriler için Dağıtılmış depolama sistemi](https://research.google.com/archive/bigtable.html)
