---
title: Anahtar değeri deposu olarak Azure Cosmos DB için istek birimi ücreti
description: Bir anahtar/değer deposu olarak kullanıldığında, okuma işlemleri ve basit yazma için istek birimi ücreti, Azure Cosmos DB hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: e81dec7d7556819e60a65a44106426c226c6223d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61043308"
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos DB anahtar değeri deposu – maliyeti genel bakış

Azure Cosmos DB, yüksek oranda kullanılabilir ve büyük ölçekli uygulamaları kolayca oluşturmak için bir Global olarak dağıtılmış çok modelli veritabanı hizmetidir. Varsayılan olarak, Azure Cosmos DB otomatik olarak bu, verimli bir şekilde alan tüm verilerin dizinini oluşturur. Bu hızlı ve tutarlı sağlar [SQL](how-to-sql-query.md) (ve [JavaScript](stored-procedures-triggers-udfs.md)) her türlü veri çubuğunda sorgular. 

Bu makalede, Azure Cosmos DB maliyetini basit yazmak için açıklar ve bir anahtar/değer deposu olarak kullanıldığında, okuma işlemleri. Yazma ekler, değiştirir, siler ve belgelerin upsert eder işlemler içerir. % 99,99 oranında garanti etme yanı sıra tek bölgeli tüm hesaplar ve çok bölgeli tüm hesaplar tutarlılıkla ve % 99,999 kullanılabilirlik SLA'sı tüm çoklu bölge veritabanı hesapları Azure Cosmos DB teklif garanti okunabilirlik < 10 MS'den az gecikme okur ve < 15 ms gecikme süresi (dizinlenmiş) için sırasıyla 99. yüzdebirlik dilimde yazar. 

## <a name="why-we-use-request-units-rus"></a>İstek birimi (RU) neden kullanıyoruz

Azure Cosmos DB performans miktarına bağlı sağlanan [istek birimi](request-units.md) (RU) bölümü için. İkinci bir ayrıntı düzeyinde olduğu ve RU/sn satın sağlama ([saatlik faturalandırma ile karıştırılmamalıdır](https://azure.microsoft.com/pricing/details/cosmos-db/)). RU, uygulama için gerekli aktarım hızı sağlama basitleştiren bir para birimi olarak düşünülmelidir. Müşterilerimizin kapasite birimleri yazma ve okuma arasında ayrım yapma düşünme gerekmez. Tek bir para birimi model RU, sağlanan kapasiteyi okuma ve yazma işlemleri arasında paylaşmak için verimliliği oluşturur. Bu sağlanan kapasite modeli, düşük gecikme süresi ve yüksek kullanılabilirlik garantisi öngörülebilir ve tutarlı bir performans sağlamak için hizmeti sağlar. Son olarak, model üretilen RU kullanıyoruz, ancak her sağlanan RU kaynakları (bellek, çekirdek) tanımlanmış bir miktarı da vardır. RU/sn yalnızca IOPS değil.

Bir Global olarak dağıtılmış bir veritabanı sistemi olarak Cosmos DB, gecikme süresi, aktarım hızı ve tutarlılık yüksek kullanılabilirlik yanı sıra SLA sağlar. yalnızca bir Azure hizmetidir. Sağladığınız aktarım hızı, her biri, Cosmos DB veritabanı hesabıyla ilişkili bölge için uygulanır. Okumalar, Cosmos DB, birden çok, iyi tanımlanmış sağlar [tutarlılık düzeyleri](consistency-levels.md) aralarından seçim yapabileceğiniz. 

Aşağıdaki tabloda, okuma ve yazma işlemleri, 1 KB ve 100 KB belge boyutuna göre gereken RU sayısı gösterilmektedir.

|Öğe boyutu|1 okuma|1 yazma|
|-------------|------|-------|
|1 KB|1 RU|5 ru|
|100 KB|10 RU|50 ru|

## <a name="cost-of-reads-and-writes"></a>Okuma ve yazma işlemleri maliyeti

1.000 sağlarsanız RU/sn, bu miktarları 3.6 milyon RU/saat ve $0.08 saat (ABD ve Avrupa içinde) maliyeti. 1 KB'lık boyut belge bu 3.6-m okuma kullanabilir veya 0,72-m Yazar anlamına gelir (3.6 milyon RU / 5) kullanarak, sağlanan aktarım hızı. Milyon okuma ve yazma işlemleri için normalleştirilmiş, maliyeti $0.022 /m okuma olur ($0.08 / 3.6) ve $0.111/ dk yazar ($0.08 / 0,72). Başına maliyet milyon aşağıdaki tabloda gösterildiği gibi en az olur.

|Öğe boyutu|1-m okuma|1 milyon yazma|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


En temel blob veya okuma işlemi ve yazma milyon işlem başına 5 milyon başına nesne depoları hizmetler ücreti $0.40. En uygun şekilde kullandıysanız, Cosmos DB %98 kadar bu diğer çözümleri (1 KB'lık işlemlerinde) daha ucuz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Yeni makaleler, Azure Cosmos DB kaynak sağlama en iyi duruma getirmek için Takipte kalın. Bu arada, araştırmalarında bizim [RU hesaplayıcı](https://www.documentdb.com/capacityplanner).

