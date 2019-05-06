---
title: API Azure Cosmos DB Spark'a giriş
description: İşlem analizi ve yapay ZEKA çalıştırmak için Azure Cosmos DB Spark API'sine nasıl kullanabileceğinizi öğrenin
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/06/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: de920f40f2968942b7ac66414170b43bd9317cfb
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65081097"
---
# <a name="introduction-to-the-azure-cosmos-db-spark-api-preview"></a>Azure Cosmos DB Spark API'si (Önizleme) giriş 

Azure Cosmos DB'de Spark API, bir Azure Cosmos hesabında depolanan verileriniz Apache Spark'tan analiz çalıştırmanıza olanak sağlar.

Spark API'yi Azure Cosmos DB'de küresel olarak dağıtılan, Cosmos veritabanlarını doğrudan yürütmek Apache Spark işleri için yerel destek sağlar. Bu özellikler sayesinde geliştiriciler, veri mühendisleri ve veri uzmanları Azure Cosmos DB esnek kullanabilirsiniz, ölçeklenebilir ve yüksek performanslı bir veri platformu hem de çalıştırılacak **OLTP ve OLAP/HTAP** iş yükleri. 

> [!NOTE]
> Azure Cosmos DB Spark API'si şu anda sınırlı Önizleme aşamasındadır. Önizleme için kaydolma, gitmek [Önizleme için kaydolun](https://aka.ms/cosmos-spark-preview) sayfası. 

Azure Cosmos DB'de Spark API'si aşağıdaki avantajları sunar:

* Verileri ve coğrafi olarak dağıtılmış kullanıcılar için Öngörüler için en iyi süreye elde edebilirsiniz.

* Çözümünüzü ve alt mimarisi basitleştirebilirsiniz [toplam sahip olma maliyetini](total-cost-ownership.md) (TCO). En az sistem olacaktır sayısı, veri işleme bileşenler ve aralarındaki tüm gereksiz veri taşıma önler.

* Oluşturur bir [güvenlik](secure-access-to-data.md), [Uyumluluk](compliance.md), yönetim altındaki tüm verileri kapsayan denetim sınırı.

* "Always on" sağlar veya [yüksek oranda kullanılabilir](high-availability.md) katı SLA'lar ile desteklenen son kullanıcı analizi.

![Azure Cosmos DB Spark API Görselleştirme](./media/spark-api-introduction/spark-api-visualization.png)
 
Azure Cosmos DB Spark API'sini kullanarak, derleme ve yapay ZEKA ve derin öğrenme modelleri, Tahmine dayalı analiz, öneriler, gibi çözümleri dağıtma IOT, müşteri 360, sahtekarlık algılama, metin duyguları, tıklama dizisi analizi. Bu, doğrudan Azure Cosmos DB verilerinizi karşı çalışır.

Ayarlamak ve akış ETL işi, Azure Cosmos DB veritabanı hizmetinin dışında gidin veya ek eklemek zorunda kalmadan işlem hizmetlerini kullanabilirsiniz. ETL işi gerçekleştirmek ve ölçeklemek geri zaman işin tamamlanıp gerektiğinde işlem ortamını esnek olarak ölçeklendirebilirsiniz.

Azure Cosmos DB'de Spark API yerleşik Machine Learning desteği Apache Spark çalışma zamanlarını destekler. Spark MLLib, Spark, Azure Machine Learning ve Bilişsel hizmetler için Microsoft Machine Learning çalışma zamanları içerir. Bu özelliklerle veri bilimcileri, veri mühendisleri ve veri analistleri derleme ve makine öğrenimi modellerini doğrudan Azure Cosmos DB, zaman ve düşük maliyetle kesir içinde kullanıma hazır hale getirme.


## <a name="key-benefits"></a>Önemli avantajlar

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Küresel olarak dağıtılan, düşük gecikme süreli işlem analizi ve yapay ZEKA

Apache Spark ile Global olarak dağıtılmış bir Azure Cosmos veritabanı, dünyanın dört bir yanındaki tüm Hızlı süresi öngörü alabilirsiniz. Azure Cosmos DB sağlayan **küresel olarak dağıtılan, düşük gecikme süreli işlem analizi** üç anahtar teknikleri ile elastik ölçekli:

* Azure Cosmos veritabanınızı küresel olarak dağıtılmış olduğundan, tüm verileri alınan yerel olarak (örneğin, kullanıcılar) veri üreticileri bulunduğu. Sorguları en yakın üreticileri ve dünyanın nerede bulunuyor ne olursa olsun, veri tüketicilerinin yerel kopya karşı sunulur. 

* Tüm analitik sorguları, doğrudan veri bölümlerine tüm gereksiz veri taşıma gerek kalmadan depolanan dizinlenmiş verileri üzerinde yürütülür. 

* Spark, Azure Cosmos DB ile birlikte olduğundan, daha az Ara çevirileri ve veri hareketleri bir daha iyi performans ve ölçeklenebilirlik elde edilen, gerçekleşir.

### <a name="unified-serverless-experience-for-apache-spark"></a>Apache Spark için sunucusuz birleşik deneyim

Çok modelli bir veritabanı Azure Cosmos DB artık, OSS API'lerini desteğini sağlayarak genişletir bir **Apache Spark için sunucusuz deneyimi birleşik** anahtar-değer, belge, grafik, sütun ailesi veri modelleri ile. Farklı veri modelleri, MongoDB, Cassandra, Gremlin, Etcd ve SQL API'leri - tüm işletim aynı temel alınan verileri kullanarak desteklenir. 

Spark API ile yerel olarak yazılan Scala, Python, Java uygulamalarını destekler ve birden çok sıkı bir şekilde tümleşik kitaplık için SQL kullanın. Bu kitaplıklar içerir ([Spark SQL](https://spark.apache.org/sql/)), makine öğrenimi (Spark [MLlib](https://spark.apache.org/mllib/)), akış işleme ([Spark yapılandırılmış akış](https://spark.apache.org/streaming/)) ve grafik işleme (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Bu araçlar çeşitli kullanım örnekleri için Spark API'SİNDEN yararlanarak kolaylaştırır. Spark yönetimiyle uğraşmak zorunda değilsiniz veya Spark kümeleri. Bilindik Apache Spark API'lerini kullanabilirsiniz ve **Jupyter not defterleri** analytics ve SQL API'si veya aynı anda aynı temel alınan veriler üzerinde işlem tabanlı işleme için Cassandra gibi OSS NoSQL API'ları için.

### <a name="no-schema-or-index-management"></a>Şema veya dizin yönetimi yok

Azure Cosmos DB ile geleneksel analitik veritabanlarının aksine veri mühendisleri ile veri Bilim insanları artık zahmetli şema ve Dizin yönetiminin gerekir. Azure Cosmos DB veritabanı altyapısında herhangi bir açık şema veya dizin yönetimi gerektirmez ve Apache Spark sorguların hızlı bir şekilde sunmak için alan tüm veriler otomatik olarak dizinleme yeteneğine sahiptir. 

### <a name="consistency-choices"></a>Tutarlılık seçenekleri

Apache Spark işleri, Azure Cosmos veritabanı veri bölümlerini çalıştırıldığından, sorguları alırsınız [beş iyi tanımlanmış tutarlılık seçeneği](consistency-levels.md). Bu tutarlılık modeli makine öğrenimi algoritması gecikme süresi ve yüksek kullanılabilirlik ödün vermeden en doğru sonuçlar sağlamak için katı tutarlılık seçme esnekliğini sunar. 

### <a name="slas"></a>SLA’lar

Apache Spark işleri gibi sektör lideri kapsamlı Azure Cosmos DB avantajları olacaktır [SLA'lar](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) ayrı bir Apache Spark kümeleri yönetme herhangi ek yükü olmadan... Bu SLA, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayabilir. 

### <a name="mixed-workloads"></a>Karma iş yükleri

Küresel ölçekte bulutta çalışan uygulamalar oluştururken büyük müşterilerin yaşadıkları biri olan işlem ve analiz ayrımı tümleştirmesini Apache Spark Azure Cosmos DB köprüsü. 

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB avantajları hakkında bilgi edinmek için [genel bakış](introduction.md) makalesi.
* [MongoDB için Azure Cosmos DB'nin API'sini kullanmaya başlama](mongodb-introduction.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](cassandra-introduction.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](graph-introduction.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](table-introduction.md)




