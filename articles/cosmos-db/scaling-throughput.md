---
title: Azure Cosmos DB'de ölçeklendirme aktarım hızı
description: Bu makalede, nasıl Azure Cosmos DB aktarım hızını esnek bir şekilde ölçeklenen açıklanır.
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/24/2019
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: b645fe210e7eeb073380dcadefead3e1b4d7ccc0
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407445"
---
# <a name="globally-scale-provisioned-throughput"></a>Sağlanan aktarım hızını küresel olarak ölçeklendirme 

Azure Cosmos DB'de sağlanan aktarım hızı birimi/saniye istek gösterilir (RU/sn veya çoğul RU). RU, hem okuma hem de maliyeti ölçmek ve aşağıdaki görüntüde gösterildiği gibi yazma işlemleri Cosmos kapsayıcınızı karşı:

![İstek Birimleri](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

RUs Cosmos kapsayıcı veya Cosmos veritabanı sağlayabilirsiniz. Bir kapsayıcı RU, kapsayıcısı üzerinde gerçekleştirilen işlemler için özel olarak kullanılabilir. Bir veritabanı üzerinde RU (hariç tüm kapsayıcıları ile özel olarak atanan RU) Bu veritabanındaki tüm kapsayıcılar arasında paylaşılır.

Esnek ölçeklendirme sağlanan aktarım hızı için artırabilir veya sağlanan RU/sn istediğiniz zaman azaltabilirsiniz. Daha fazla bilgi için bkz. [yapılır sağlama aktarım hızı](set-throughput.md) ve Cosmos kapsayıcılar ve veritabanlarını esnek bir şekilde ölçeklendirin. Aktarım hızı ölçeklendirme için genel olarak, ekleme veya bölgeler, herhangi bir zamanda Cosmos hesabınızdan kaldırın. Daha fazla bilgi için [veritabanı hesabınızı Ekle/Kaldır bölgelerden](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Bir Cosmos hesabıyla birden çok bölgede ilişkilendirme, düşük gecikme süresi elde etmek için pek çok senaryoda - önemlidir ve [yüksek kullanılabilirlik](high-availability.md) dünyanın dört bir yanındaki.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>sağlanan aktarım hızı bölgede dağıtılır nasıl

Sağlarsanız, *'R'* RU bir Cosmos kapsayıcı (veya veritabanı), Cosmos DB sağlar *'R'* RU kullanılabilir *her* Cosmos hesabınızla ilişkili bölge. Hesabınız için yeni bir bölgeye ekle her zaman otomatik olarak Cosmos DB sağlayan *'R'* yeni eklenen bölgede RU. Cosmos kapsayıcınızı karşı gerçekleştirilen işlemleri Al garanti *'R'* her bölgede RU. Belirli bir bölgede RU seçmeli olarak atanamaz. Bir Cosmos kapsayıcı (veya veritabanı) RU, Cosmos hesabınızla ilişkili tüm bölgelerde sağlanır.

Bir Cosmos kapsayıcı ile yapılandırılmış varsayarak *'R'* RU ve *'N'* ardından Cosmos hesabıyla ilişkili bölge:

- Cosmos hesabı kapsayıcı üzerindeki genel olarak kullanılabilir toplam RU bir tek bir yazma bölgesi ile yapılandırılmışsa = *R* x *N*.

- Cosmos hesabı birden çok yazma bölgeleri, küresel olarak kapsayıcı üzerindeki kullanılabilir toplam RU ile yapılandırılmışsa = *R* x (*N*+ 1). Ek *R* RU otomatik olarak sağlanan işlem güncelleştirme çakışmalarını ve koruma entropi trafiği için bölgeler arasında.

Tercih ettiğiniz [tutarlılık modelini](consistency-levels.md) de aktarım hızını etkiler. Daha fazla gevşek tutarlılık düzeyleri için yaklaşık 2 x okuma aktarım hızı elde edebilirsiniz (örneğin, *oturumu*, *tutarlı ön ek* ve *nihai* tutarlılık) karşılaştırılan güçlü tutarlılık düzeyleri (örn., *sınırlanmış eskime durumu* veya *güçlü* tutarlılık).

## <a name="next-steps"></a>Sonraki adımlar

Sonraki aktarım hızı bir kapsayıcı veya veritabanı yapılandırma konusunda bilgi edinebilirsiniz:

* [Alma ve kapsayıcılar ve veritabanları için aktarım hızı ayarlama](set-throughput.md) 

