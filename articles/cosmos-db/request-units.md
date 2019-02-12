---
title: İstek birimleri ve Azure Cosmos DB'de aktarım hızı
description: Belirtin ve Azure Cosmos DB'de istek birimi gereksinimlerini tahmin etme hakkında bilgi edinin
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: 3801c19fbef70bf5d67670c4d9acc950291853e6
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990164"
---
# <a name="request-units-in-azure-cosmos-db"></a>İstek birimleri Azure cosmos DB

Azure Cosmos DB ile aktarım hızı için kullandığınız sağlama ve saatlik olarak tükettiğiniz depolama ödeme yaparsınız. Her zaman yeterli sistem kaynaklarına, Azure Cosmos veritabanı için kullanılabilir olmasını sağlamak için aktarım hızı sağlanmış olması gerekir. Azure Cosmos DB SLA karşıladığında veya yeterli kaynaklar ihtiyacınız vardır.

Azure Cosmos DB SQL, MongoDB, Cassandra, Gremlin ve tablo gibi birçok API'leri destekler. Her API, kendi veritabanı işlemleri kümesine sahiptir. Bu işlem aralığı basit noktasından okur ve karmaşık sorgular için yazar. Her veritabanı işlemi, bir işlem karmaşıklığı sistem kaynaklarını tüketir. 

Tüm veritabanı işlemlerinin maliyetini Azure Cosmos DB tarafından normalleştirilmiş ve istek birimi (RU) ifade edilir. Bir 1 KB'lik öğe okumak için 1 istek birimi (RU) maliyetidir. Diğer tüm veritabanı işlemleri, RU'ları kullanarak maliyet benzer şekilde atanır. Azure Cosmos kapsayıcınızı ile etkileşim kurmak için kullanın, API ne olursa olsun, maliyetleri her zaman RU'ları tarafından ölçülür. Veritabanı işlemi yazma olup okuma ya da sorgu maliyetleri her zaman RU ölçülür.

RU aktarım hızı için para birimi olarak saniyede düşünebilirsiniz. RU / saniye oranı tabanlı bir para birimi olur. Bu, Azure Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, IOPS ve bellek gibi sistem kaynaklarının soyutlar. Aşağıdaki görüntüde, farklı bir veritabanı işlemleri tarafından tüketilen RU miktarı gösterilmektedir:

![İstek birimi veritabanı işlemleri kullanma](./media/request-units/request-units.png)

Kapasite planlama ve yönetmek için Azure Cosmos DB, RU sayısı için belirli bir veri kümesi üzerinde bir verilen veritabanı işlemi belirlenimci olmasını sağlar. Herhangi bir veritabanı işlemi tarafından tüketilen RU sayısını izlemek için yanıt üst bilgisi inceleyin. RU ücretleri ve uygulamanızın aktarım hızı gereksinimleri etkileyen faktörler anladığınızda, uygulama maliyetinizi etkili bir şekilde çalıştırabilirsiniz.

Saatlik olarak faturalandırılırsınız. Saniye başına temelinde halinde uygulamanız 100 RU / saniye için RU sayısını sağlayın. Uygulamanız için sağlanan aktarım hızı ölçeklendirmek için artırabilir veya herhangi bir zamanda RU sayısını azaltın. Artırır veya azaltır 100 RU değişikliklerinizi yapın. Değişikliklerinizi programlama yoluyla veya Azure portalını kullanarak yapabilirsiniz.

Aktarım hızını iki farklı ayrıntı düzeylerinde sağlayabilirsiniz: 

* **Kapsayıcıları**. Daha fazla bilgi için [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).
* **Veritabanları**. Daha fazla bilgi için [bir Azure Cosmos veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>İstek birimi konuları

RU sayısını sağlamak için saniyede tahmin ederken aşağıdaki etmenleri dikkate alın:

* **Öğe boyutunu**: Bir öğenin boyutu arttıkça, okuma veya yazma öğesi için tüketilen RU sayısı da artar.

* **Öğe dizini oluşturma**: Varsayılan olarak, her bir öğe otomatik olarak dizine alınır. Bir kapsayıcıda, öğelerin bazıları dizin kullanılamıyor seçerseniz, daha az RU tüketilir.

* **Öğe özellik sayısı**: Varsayılan dizinleme tüm özelliklerde varsayıldığında, RU sayısını öğesi özellik sayısı arttıkça öğeyi artırır yazmak için kullanılan.

* **Dizin oluşturulmuş özellikleri**: Her kapsayıcı bir dizin İlkesi, hangi özellikleri varsayılan olarak dizinlenir belirler. Yazma işlemleri için RU kullanımını azaltmak için Dizinlenmiş özelliklerin sayısını sınırlayın.

* **Veri tutarlılığı**: Güçlü ve sınırlanmış eskime durumu tutarlılık düzeyi, diğer tutarlılık düzeyleri rahat karşılaştırıldığında okuma işlemleri gerçekleştirilirken yaklaşık iki kat daha fazla RU tüketir.

* **Sorgu desenleri**: Kaç tane RU bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Sorgu işlemlerinin maliyetini etkileyen faktörler aşağıda verilmiştir: 
    
    - Sorgu sonuç sayısı.
    - Koşullar sayısı.
    - Koşullarına yapısı.
    - Kullanıcı tanımlı işlevleri sayısı.
    - Kaynak verilerin boyutu.
    - Sonuç kümesinin boyutu.
    - Projeksiyonlar.

  Azure Cosmos DB, aynı sorgu aynı verileri her zaman aynı sayıda RU'ları üzerinde yinelenen yürütme maliyetlerini garanti eder.

* **Betik kullanımı**: Sorgularla olduğu gibi saklı yordamları ve Tetikleyicileri üzerinde gerçekleştirilen işlemleri karmaşıklığına göre RU kullanır. Uygulamanızı geliştirirken, her işlem tüketir, RU kapasite için ne kadar daha iyi anlamak için istek ücret üstbilgisi inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanma hakkında daha fazla bilgi edinin [Azure Cosmos kapsayıcılar ve veritabanları için sağlama aktarım hızı](set-throughput.md).
* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md).
* Kullanma hakkında daha fazla bilgi edinin [genel olarak sağlanan aktarım hızını ölçeklendirme](scaling-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md).
