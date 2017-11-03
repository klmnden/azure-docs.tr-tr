---
title: "Anahtar değeri deposu – maliyet genel olarak Azure Cosmos DB | Microsoft Docs"
description: "Düşük maliyetli bir anahtar değeri deposu olarak Azure Cosmos DB kullanma hakkında bilgi edinin."
keywords: "anahtar değeri deposu"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mimig
ms.openlocfilehash: 33eef1b51a5ee00b0fa67096030ed9ce92cf768e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Anahtar değeri deposu – maliyet genel olarak Azure Cosmos DB

Azure Cosmos DB, yüksek oranda kullanılabilir, büyük ölçekli uygulamalara kolayca oluşturmaya yönelik bir genel dağıtılmış, birden çok model veritabanı hizmetidir. Varsayılan olarak, Azure Cosmos DB onu, verimli bir şekilde alır tüm veriler otomatik olarak dizinler. Bu, hızlı ve tutarlı sağlar [SQL](documentdb-sql-query.md) (ve [JavaScript](programming.md)) her türlü veri sorgular. 

Bu makalede Azure Cosmos DB maliyetini için basit yazma ve okuma işlemlerini bir anahtar/değer deposu olarak kullanıldığında. Ekler, değiştirir, siler ve belgelerin upserts işlemleri içeren yazma. % 99,99 yüksek kullanılabilirlik güvence altına almak yanı sıra Azure Cosmos DB teklifleri garanti < 10 ms Gecikmeli okumalar için ve < 15 ms gecikme (dizinlenmiş) için sırasıyla 99 yazar. 

## <a name="why-we-use-request-units-rus"></a>İstek birimleri (RUs) neden kullanırız

Azure Cosmos DB performans miktarına bağlı sağlanan [istek birimlerine](request-units.md) (RU) bölümü için. İkinci bazda olduğunu ve RUs/sn ile RUs/dak satın sağlama ([saatlik faturalandırma ile karıştırılmamalıdır](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs uygulama için gerekli işleme sağlama basitleştiren bir para birimi olarak düşünülmelidir. Müşterilerimizin okuma arasında ayrım yapma düşüncelerinizi ve kapasite birimlerinin yazmak zorunda değildir. RUs tek para birimini modelinin sağlanan kapasite okuma ve yazma işlemleri arasında paylaşmak için verimliliği oluşturur. Bu sağlanan kapasite model düşük gecikme süreli ve yüksek oranda kullanılabilirlik garanti tahmin edilebilir ve tutarlı bir performans sağlamak için hizmeti sağlar. Son olarak, model işleme RU kullanırız ancak sağlanan her RU ayrıca tanımlanan miktarda kaynak (bellek, çekirdek) sahiptir. Saniye başına RU yalnızca IOPS değil.

Genel olarak dağıtılmış veritabanı sistem olarak Cosmos DB gecikme süresi, üretilen iş ve tutarlılık yüksek kullanılabilirlik yanı sıra bir SLA sağlayan yalnızca Azure hizmetidir. Sağlamanız verimlilik her Cosmos DB veritabanı hesabınızla ilişkili bölgelerinin uygulanır. Okumalar, birden çok, iyi tanımlanmış Cosmos DB sunar [tutarlılık düzeylerini](consistency-levels.md) aralarından seçim yapabileceğiniz. 

Aşağıdaki tabloda, RUs sayısı 1 KB ile 100KBs belge boyutuna göre işlemleri yazma ve okuma gerçekleştirmek için gerekli gösterilmektedir.

|Öğesi boyutu|1 okuma|1 yazma|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Okuma ve yazma işlemleri maliyeti

1.000 hazırlıyorsanız, RU/sn, bu miktarı 3,6 m RU/saat ve 0,08 ABD Doları saat (ABD ve Avrupa'da) maliyeti. Bir 1 KB boyutu belge için bu 3,6 m okuma tüketebileceği veya 0,72 m Yazar anlamına gelir (3.6mRU / 5), sağlanan işleme kullanma. Milyon okuma ve yazma işlemleri için normalleştirilmiş, maliyet $0.022 /m okuma olacaktır (0,08 ABD Doları / 3,6) ve $0.111/ m yazar (0,08 ABD Doları / 0,72). Başına maliyet milyon aşağıdaki tabloda gösterildiği gibi en az olur.

|Öğesi boyutu|1m okuma|1m yazma|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


En temel blob veya milyon okuma işlemi ve 5 milyon yazma işlem başına başına nesne depoları hizmetler ücreti $0.40. En iyi şekilde kullandıysanız, Cosmos DB %98 kadar bu diğer çözümlerin (işlemlerinde 1 KB) daha ucuz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Yeni makaleler Azure Cosmos DB kaynak sağlama en iyi duruma getirmek için bizi izlemeye devam edin. Bu arada, kullanılacak çekinmeyin bizim [RU hesaplayıcı](https://www.documentdb.com/capacityplanner).

