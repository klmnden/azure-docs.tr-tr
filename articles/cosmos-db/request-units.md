---
title: İstek birimleri ve Azure Cosmos DB'de aktarım hızı
description: Nasıl belirtileceği hakkında bilgi edinin ve Azure Cosmos DB'de istek birimi gereksinimlerini tahmin etme
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: eabfe503d9b92252ada0014eba4c83390dd6fd97
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236084"
---
# <a name="request-units-in-azure-cosmos-db"></a>İstek birimi olarak Azure Cosmos DB'de

Azure Cosmos DB ile aktarım hızı için kullandığınız sağlama ve saatlik olarak tükettiğiniz depolama ödeme yaparsınız. Yeterli sistem kaynaklarına her zaman karşıladığında veya Cosmos DB SLA'sı için Cosmos veritabanınız için kullanılabilir olmasını sağlamak için aktarım hızı sağlanmış olması gerekir.

Cosmos DB API'leri (SQL, MongoDB, Cassandra, Gremlin ve tablo) çeşitli destekler. Her API, basit noktasından arasında değişen okur ve yazar karmaşık sorgular için kendi veritabanı işlemleri kümesine sahiptir. Her veritabanı işlemi, işlemi karmaşıklığına olarak sistem kaynaklarını tüketir.  Tüm veritabanı işlemlerinin maliyetini Cosmos DB tarafından normalleştirilmiş ve istek birimi (RU) cinsinden ifade edilir. 1 istek birimi bir 1 KB'lik öğe okunacak maliyeti, (1 RU). Diğer tüm veritabanı işlemleri, maliyet açısından RU benzer şekilde atanır. Cosmos kapsayıcınızı ve veritabanı işlemi (okuma, yazma, sorgu) ile etkileşim kurmak için kullandığınız API bağımsız olarak, maliyetleri her zaman RU açısından ölçülür.

RU/sn aktarım hızı için para birimi olarak düşünebilirsiniz. RU/sn Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, IOPS ve bellek gibi sistem kaynaklarının soyutlayan bir oranı tabanlı para birimidir. Aşağıdaki görüntüde, farklı bir veritabanı işlemleri tarafından kullanılan istek birimleri gösterir:

![İstek birimleri veritabanı işlemleri kullanma](./media/request-units/request-units.png)

Kapasite planlama ve yönetmek için Cosmos DB, RU sayısı için belirli bir veri kümesi üzerinde bir verilen veritabanı işlemi belirlenimci olmasını sağlar. Yanıt üst bilgisi inceleyerek herhangi bir veritabanı işlemi tarafından tüketilen RU sayısını takip edebilirsiniz. İstek birimi ücreti ve uygulamanızın aktarım hızı gereksinimleri etkileyen faktörler anladığınızda, uygulamanızı hesaplı bir şekilde çalıştırabilirsiniz.

Saatlik olarak faturalandırılır ancak RU sayısı 100 RU/sn artışlarla saniye başına temelinde uygulama için sağlama. Uygulamanız için sağlanan aktarım hızı ölçeklendirmek için artırabilir veya programlama yoluyla veya Azure portalını kullanarak herhangi bir zamanda (artırır veya azaltır 100 RU) cinsinden RU sayısını azaltın.

Aktarım hızını iki farklı ayrıntı düzeylerinde sağlayabilirsiniz: 

* **Kapsayıcıları**. Daha fazla bilgi için [Cosmos kapsayıcısında aktarım hızını sağlamasını yapma.](how-to-provision-container-throughput.md)
* **Veritabanları**. Daha fazla bilgi için [Cosmos veritabanı aktarım hızını sağlamasını yapma.](how-to-provision-database-throughput.md)

## <a name="request-unit-considerations"></a>İstek birimi konuları

RU/sn sayısı sağlamak için tahmini çalışırken, aşağıdaki faktörleri dikkate almak önemlidir:

* **Öğe boyutunu** - bir öğe boyutu arttıkça, okumak veya öğeyi de artar yazmak için tüketilen RU sayısı.

* **Belge dizine** -varsayılan olarak, her bir öğe otomatik olarak dizine alınır. Bir kapsayıcıda, öğelerin bazıları dizin kullanılamıyor seçerseniz istek birimi daha az tüketilir.

* **Öğe özellik sayısı** -varsayılan dizinleme tüm özelliklerde varsayıldığında, RU sayısı bir öğe, öğe özelliği sayısı arttıkça artırır yazmak için kullanılan.

* **Dizinli Özellikler** -her kapsayıcı bir dizin İlkesi hangi özellikleri varsayılan olarak dizinlenir belirler. Dizinli Özellikler sayısını sınırlayarak yazma işlemleri için istek birimi tüketimini azaltabilirsiniz.

* **Veri tutarlılığı** -diğer tutarlılık düzeyleri rahat karşılaştırıldığında okuma işlemleri gerçekleştirilirken yaklaşık 2 kat daha fazla RU güçlü ve sınırlanmış eskime durumu tutarlılık düzeylerini kullanma.

* **Sorgu desenleri** -kaç tane istek birimi bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Sorgu sonuçları, koşullar sayısı, koşullarına yapısını, kullanıcı tanımlı işlev sayısı, kaynak verilerin boyutu, sonucunun boyutuna sayısını ayarlayın ve projeksiyonları sorgu işlemlerinin maliyetini etkiler. Cosmos DB, aynı sorgu aynı verileri her zaman aynı sayıda RU yineleme yürütmeleri üzerinde maliyet garanti eder.

* **Betik kullanımı** - gibi sorgularla, saklı yordamları ve Tetikleyicileri gerçekleştirilmekte olan işlemlerin kapsamına bağlı RU tüketir. Uygulamanızı geliştirirken, her bir işlemin tükettiği istek birimi kapasite ne kadar daha iyi anlamak için istek ücret üstbilgisi inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Cosmos DB kapsayıcıları ve veritabanları için sağlama aktarım hızı](set-throughput.md)
* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md)
* Daha fazla bilgi edinin [ölçeklendirme aktarım hızı](scaling-throughput.md)
* Bilgi [Cosmos kapsayıcısında aktarım hızını sağlamasını yapma](how-to-provision-container-throughput.md)
* Bilgi [Cosmos veritabanı aktarım hızını sağlamasını yapma](how-to-provision-database-throughput.md)
