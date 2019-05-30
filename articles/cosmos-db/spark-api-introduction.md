---
title: Azure Cosmos DB'de Apache Spark ile yerleşik işlem analizi giriş
description: Nasıl yerleşik desteği Azure Cosmos DB'de Apache Spark için işlem analizi ve yapay ZEKA çalıştırmak için kullanabileceğiniz öğrenin
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/23/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: b392f7fd6438b25a741aecb86a72f142d785f0e3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237882"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark-preview"></a>Azure Cosmos DB'de Apache Spark (Önizleme) ile yerleşik işlem analizi 

Azure Cosmos DB'de Apache Spark için yerleşik destek, bir Azure Cosmos hesabında depolanan verileriniz Apache Spark'tan analiz çalıştırmanıza olanak tanır. Global olarak dağıtılmış, Cosmos veritabanlarını doğrudan yürütmek Apache Spark işleri için yerel destek sağlar. Bu özellikler sayesinde geliştiriciler, veri mühendisleri ve veri uzmanları Azure Cosmos DB esnek kullanabilirsiniz, ölçeklenebilir ve yüksek performanslı bir veri platformu hem de çalıştırılacak **OLTP ve OLAP/HTAP** iş yükleri. 

Spark işlem otomatik olarak kullanılabilir tüm Azure bölgelerinde Azure Cosmos hesabınızla ilişkili olan. Spark işlerinde Azure Cosmos DB'nin çok yöneticili özelliğini kullanın ve yazma veya her bölgede yerel kopya karşı sorgu. 

> [!NOTE]
> Azure Cosmos DB'de Apache Spark için yerleşik destek, şu anda sınırlı Önizleme aşamasındadır. Önizleme için kaydolma, gitmek [Önizleme sayfası için kaydolma](https://portal.azure.com/?feature.customportal=false#create/Microsoft.DocumentDB). 

Azure Cosmos DB'de Apache Spark desteği aşağıdaki faydaları sunar:

* Verileri ve coğrafi olarak dağıtılmış kullanıcılar için Öngörüler için en iyi süreye elde edebilirsiniz.

* Çözümünüzü ve alt mimarisi basitleştirebilirsiniz [toplam sahip olma maliyetini](total-cost-ownership.md) (TCO). En az sistem olacaktır sayısı, veri işleme bileşenler ve aralarındaki tüm gereksiz veri taşıma önler.

* Oluşturur bir [güvenlik](secure-access-to-data.md), [Uyumluluk](compliance.md), yönetim altındaki tüm verileri kapsayan denetim sınırı.

* "Always on" sağlar veya [yüksek oranda kullanılabilir](high-availability.md) katı SLA'lar ile desteklenen son kullanıcı analizi.

![Azure Cosmos DB görselleştirme Apache Spark desteği](./media/spark-api-introduction/spark-api-visualization.png)
 
Azure Cosmos DB'de Apache Spark desteği kullanarak oluşturabilir ve yapay ZEKA ve derin öğrenme modelleri, Tahmine dayalı analiz, öneriler, gibi çözümleri dağıtma IOT, müşteri 360, sahtekarlık algılama, metin duyguları, tıklama dizisi analizi. Bu çözümler, doğrudan Azure Cosmos DB verilerinizi karşı çalışır.

Ayarlamak ve akış ETL işi, Azure Cosmos DB veritabanı hizmetinin dışında gidin veya ek eklemek zorunda kalmadan işlem hizmetlerini kullanabilirsiniz. ETL işi gerçekleştirmek ve ölçeklemek geri zaman işin tamamlanıp gerektiğinde işlem ortamını esnek olarak ölçeklendirebilirsiniz.

Azure Cosmos DB'de Apache Spark desteği, Apache Spark çalışma zamanları yerleşik Machine Learning desteği sunar. Spark MLLib, Spark, Azure Machine Learning ve Bilişsel hizmetler için Microsoft Machine Learning çalışma zamanları içerir. Bu özelliklerle veri bilimcileri, veri mühendisleri ve veri analistleri derleme ve makine öğrenimi modellerini doğrudan Azure Cosmos DB, zaman ve düşük maliyetle kesir içinde kullanıma hazır hale getirme.


## <a name="key-benefits"></a>Önemli avantajlar

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Küresel olarak dağıtılan, düşük gecikme süreli işlem analizi ve yapay ZEKA

Apache Spark ile Global olarak dağıtılmış bir Azure Cosmos veritabanı, dünyanın dört bir yanındaki tüm Hızlı süresi öngörü alabilirsiniz. Azure Cosmos DB sağlayan **küresel olarak dağıtılan, düşük gecikme süreli işlem analizi** üç anahtar teknikleri ile elastik ölçekli:

* Azure Cosmos veritabanınızı küresel olarak dağıtılmış olduğundan, tüm verileri alınan yerel olarak (örneğin, kullanıcılar) veri üreticileri bulunduğu. Sorguları en yakın üreticileri ve dünyanın nerede bulunuyor ne olursa olsun, veri tüketicilerinin yerel kopya karşı sunulur. 

* Tüm analitik sorguları, doğrudan veri bölümlerine tüm gereksiz veri taşıma gerek kalmadan depolanan dizinlenmiş verileri üzerinde yürütülür. 

* Spark, Azure Cosmos DB ile birlikte olduğundan, daha az Ara çevirileri ve veri hareketleri bir daha iyi performans ve ölçeklenebilirlik elde edilen, gerçekleşir.

### <a name="unified-serverless-experience-for-apache-spark"></a>Apache Spark için sunucusuz birleşik deneyim

Çok modelli bir veritabanı Azure Cosmos DB artık, OSS API'lerini desteğini sağlayarak genişletir bir **Apache Spark için sunucusuz deneyimi birleşik** anahtar-değer, belge, grafik, sütun ailesi veri modelleri ile. Farklı veri modelleri, MongoDB, Cassandra, Gremlin, Etcd ve SQL API'leri - tüm işletim aynı temel alınan verileri kullanarak desteklenir. 

Azure Cosmos DB'de Apache Spark destek sayesinde, yerel olarak yazılan Scala, Python, Java uygulamalarını destekler ve birkaç sıkıca tümleşik kitaplıkları için SQL kullanın. Bu kitaplıklar içerir ([Spark SQL](https://spark.apache.org/sql/)), makine öğrenimi (Spark [MLlib](https://spark.apache.org/mllib/)), akış işleme ([Spark yapılandırılmış akış](https://spark.apache.org/streaming/)) ve grafik işleme (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Bu araçlar çeşitli kullanım örnekleri için Apache Spark'tan yararlanın kolaylaştırır. Spark yönetimiyle uğraşmak zorunda değilsiniz veya Spark kümeleri. Bilindik Apache Spark API'lerini kullanabilirsiniz ve **Jupyter not defterleri** analytics ve SQL API'si veya aynı anda aynı temel alınan veriler üzerinde işlem tabanlı işleme için Cassandra gibi OSS NoSQL API'ları için.

### <a name="no-schema-or-index-management"></a>Şema veya dizin yönetimi yok

Azure Cosmos DB ile geleneksel analitik veritabanlarının aksine veri mühendisleri ile veri Bilim insanları artık zahmetli şema ve Dizin yönetiminin gerekir. Azure Cosmos DB veritabanı altyapısında herhangi bir açık şema veya dizin yönetimi gerektirmez ve Apache Spark sorguların hızlı bir şekilde sunmak için alan tüm veriler otomatik olarak dizinleme yeteneğine sahiptir. 

### <a name="consistency-choices"></a>Tutarlılık seçenekleri

Apache Spark işleri, Azure Cosmos veritabanı veri bölümlerini çalıştırıldığından, sorguları alırsınız [beş iyi tanımlanmış tutarlılık seçeneği](consistency-levels.md). Bu tutarlılık modeli makine öğrenimi algoritması gecikme süresi ve yüksek kullanılabilirlik ödün vermeden en doğru sonuçlar sağlamak için katı tutarlılık seçme esnekliğini sunar. 

### <a name="comprehensive-slas"></a>Kapsamlı SLA’lar

Apache Spark işleri gibi sektör lideri kapsamlı Azure Cosmos DB avantajları olacaktır [SLA'lar](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) ayrı bir Apache Spark kümeleri yönetme herhangi ek yükü olmadan... Bu SLA, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayabilir. 

### <a name="mixed-workloads"></a>Karma iş yükleri

Küresel ölçekte bulutta çalışan uygulamalar oluştururken büyük müşterilerin yaşadıkları biri olan işlem ve analiz ayrımı tümleştirmesini Apache Spark Azure Cosmos DB köprüsü. 

## <a name="built-in-jupyter-notebooks-support"></a>Jupyter not defterleri için yerleşik destek

Azure Cosmos DB Cassandra, MongoDB, SQL, Gremlin ve tablo gibi tüm API'ler için yerleşik Jupyter not defterleri destekler. Jupyter not defterlerini içinde Azure Cosmos hesapları çalıştırın ve bunlar Geliştirici deneyimini geliştirin. Tüm Azure Cosmos DB API ve veri modelleri için yerleşik not defteri destek etkileşimli sorguları çalıştırmak sağlar. Makine öğrenimi modelleri yürütün ve Azure Cosmos veritabanınızda depolanan verileri analiz edin. Jupyter not defteri deneyimi kullanarak depolanan verileri analiz etmek, derleme ve makine öğrenimi modellerini eğitmenize ve aşağıdaki görüntüde gösterildiği gibi Azure portalında veri çubuğunda çıkarım gerçekleştirin:

![Azure Cosmos DB'de Jupyter not defterleri desteği](./media/spark-api-introduction/jupyter-notebooks-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB avantajları hakkında bilgi edinmek için [genel bakış](introduction.md) makalesi.
* [MongoDB için Azure Cosmos DB'nin API'sini kullanmaya başlama](mongodb-introduction.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](cassandra-introduction.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](graph-introduction.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](table-introduction.md)




