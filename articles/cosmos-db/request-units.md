---
title: İstek birimleri ve Azure Cosmos DB'de aktarım hızı
description: Belirtin ve Azure Cosmos DB'de istek birimi gereksinimlerini tahmin etme hakkında bilgi edinin
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: rimman
ms.openlocfilehash: 12f91676ac50511bf3d2d33f7fed2029e152dc98
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164995"
---
# <a name="request-units-in-azure-cosmos-db"></a>İstek birimleri Azure cosmos DB

Azure Cosmos DB ile aktarım hızı için kullandığınız sağlama ve saatlik olarak tükettiğiniz depolama ödeme yaparsınız. Her zaman yeterli sistem kaynaklarına, Azure Cosmos veritabanı için kullanılabilir olmasını sağlamak için aktarım hızı sağlanmış olması gerekir. Yeterli kaynaklar karşılaması veya aşması gerekir [Azure Cosmos DB SLA'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Azure Cosmos DB SQL, MongoDB, Cassandra, Gremlin ve tablo gibi birçok API'leri destekler. Her API, kendi veritabanı işlemleri kümesine sahiptir. Bu işlem aralığı basit noktasından okur ve karmaşık sorgular için yazar. Her veritabanı işlemi, bir işlem karmaşıklığı sistem kaynaklarını tüketir. 

Tüm veritabanı işlemlerinin maliyetini Azure Cosmos DB tarafından normalleştirilmiş ve ile ifade edilen *istek birimi* (veya kısaca RU'ları). RU aktarım hızı için para birimi olarak saniyede düşünebilirsiniz. RU / saniye oranı tabanlı bir para birimi olur. Bu, Azure Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, IOPS ve bellek gibi sistem kaynaklarının soyutlar. 

Maliyeti bir 1 KB öğesi 1 istek birimi okuma (veya 1 RU). Diğer tüm veritabanı işlemleri benzer şekilde RU'ları kullanarak maliyet atanır. Azure Cosmos kapsayıcınızı ile etkileşim kurmak için kullanın, API ne olursa olsun, maliyetleri her zaman RU'ları tarafından ölçülür. Veritabanı işlemi yazma olup okuma ya da sorgu maliyetleri her zaman RU ölçülür.

Aşağıdaki görüntüde, üst düzey bir fikir RU gösterilmektedir:

![İstek birimi veritabanı işlemleri kullanma](./media/request-units/request-units.png)

Kapasite planlama ve yönetmek için Azure Cosmos DB, RU sayısı için belirli bir veri kümesi üzerinde bir verilen veritabanı işlemi belirlenimci olmasını sağlar. Herhangi bir veritabanı işlemi tarafından tüketilen RU sayısını izlemek için yanıt üst bilgisi inceleyebilirsiniz. Anladığınızda [RU ücretleri etkileyen faktörleri](request-units.md#request-unit-considerations) ve uygulamanızın aktarım hızı gereksinimleri, düşük maliyetle uygulamanızı çalıştırabilirsiniz.

Saniye başına temelinde halinde uygulamanız 100 RU / saniye için RU sayısını sağlayın. Uygulamanız için sağlanan aktarım hızı ölçeklendirmek için artırabilir veya herhangi bir zamanda RU sayısını azaltın. Artırır veya azaltır 100 RU ölçeklendirebilirsiniz. Değişikliklerinizi programlama yoluyla veya Azure portalını kullanarak yapabilirsiniz. Saatlik olarak faturalandırılır.

Aktarım hızını iki farklı ayrıntı düzeylerinde sağlayabilirsiniz: 

* **Kapsayıcıları**: Daha fazla bilgi için [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).
* **Veritabanları**: Daha fazla bilgi için [bir Azure Cosmos veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>İstek birimi konuları

RU sayısını sağlamak için saniyede tahmin ederken aşağıdaki etmenleri dikkate alın:

* **Öğe boyutunu**: Bir öğenin boyutu arttıkça, okuma veya yazma öğesi için tüketilen RU sayısı da artar.

* **Öğe dizini oluşturma**: Varsayılan olarak, her bir öğe otomatik olarak dizine alınır. Bir kapsayıcıda, öğelerin bazıları dizin kullanılamıyor seçerseniz, daha az RU tüketilir.

* **Öğe özellik sayısı**: Tüm özelliklerde varsayılan dizinleme olduğu varsayıldığında, RU sayısını öğesi özellik sayısı arttıkça öğeyi artırır yazmak için kullanılan.

* **Dizin oluşturulmuş özellikleri**: Her kapsayıcı bir dizin İlkesi, hangi özellikleri varsayılan olarak dizinlenir belirler. Yazma işlemleri için RU kullanımını azaltmak için Dizinlenmiş özelliklerin sayısını sınırlayın.

* **Veri tutarlılığı**: Güçlü ve sınırlanmış eskime durumu tutarlılık düzeyi, diğer tutarlılık düzeyleri rahat karşılaştırıldığında okuma işlemleri gerçekleştirilirken yaklaşık iki kat daha fazla RU tüketir.

* **Sorgu desenleri**: Kaç tane RU bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Sorgu işlemlerinin maliyetini etkileyen faktörler aşağıda verilmiştir: 
    
    - Sorgu sonuç sayısı
    - Koşullar sayısı
    - Koşullarına yapısı
    - Kullanıcı tanımlı işlev sayısı
    - Kaynak veri boyutu
    - Sonuç kümesi boyutu
    - Yansıtmalar

  Azure Cosmos DB, aynı sorgu aynı verileri her zaman aynı sayıda RU'ları üzerinde yinelenen yürütme maliyetlerini garanti eder.

* **Betik kullanımı**: Sorgularla olduğu gibi saklı yordamları ve Tetikleyicileri üzerinde gerçekleştirilen işlemleri karmaşıklığına göre RU kullanır. Uygulamanızı geliştirirken, inceleme [istek ücret üstbilgisi](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) her işlem tüketir, RU kapasite için ne kadar daha iyi anlamak için.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanma hakkında daha fazla bilgi edinin [Azure Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızını](set-throughput.md).
* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md).
* Kullanma hakkında daha fazla bilgi edinin [genel olarak sağlanan aktarım hızını ölçeklendirme](scaling-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md).
* Bilgi edinmek için nasıl [istek birimi ücreti bulmak için bir işlem](find-request-unit-charge.md).
* Bilgi edinmek için nasıl [Azure Cosmos DB'de sağlanan aktarım hızı gerçekleştirerek](optimize-cost-throughput.md).
* Bilgi edinmek için nasıl [okuma ve yazma işlemleri Azure Cosmos DB'de maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md).
* Bilgi edinmek için nasıl [sorgu gerçekleştirerek Azure Cosmos DB'de](optimize-cost-queries.md).
* Bilgi edinmek için nasıl [İzleyici aktarım hızı için ölçümleri kullanma](use-metrics.md).
