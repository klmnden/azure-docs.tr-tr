---
title: Azure Cosmos DB'de ölçeklendirme aktarım hızı
description: Bu makalede, nasıl Azure Cosmos DB aktarım hızını esnek bir şekilde ölçeklenen açıklanır.
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: adbac964e6654e16f6c405b9a5b8669326583f3e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244160"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Azure Cosmos DB'de ölçeklendirme aktarım hızı

Azure Cosmos DB'de sağlanan aktarım hızı birimi/saniye istek gösterilir (RU/sn, plural: RU). RU, hem okuma hem de maliyeti ölçün ve yazma işlemleri Cosmos kapsayıcınızı karşı.

![İstek Birimleri](./media/scale-throughput/figure1.png)

RUs Cosmos kapsayıcı veya Cosmos veritabanı sağlayabilirsiniz. Bir kapsayıcı RU, kapsayıcısı üzerinde gerçekleştirilen işlemler için özel olarak kullanılabilir. Bir veritabanı üzerinde RU (hariç tüm kapsayıcıları ile özel olarak atanan RU) Bu veritabanındaki tüm kapsayıcılar arasında paylaşılır.

Esnek bir biçimde aktarım hızı ölçeği artırılabilen veya azaltılabilen için artırabilir veya sağlanan RU/sn istediğiniz zaman azaltabilirsiniz. Daha fazla bilgi için yapılır sağlama aktarım hızı ve Cosmos kapsayıcılar ve veritabanlarını esnek bir şekilde ölçeklendirin. Aktarım hızı ölçeklendirme için genel olarak, ekleme veya bölgeler, Cosmos hesabınızda herhangi bir zamanda kaldırın. Daha fazla bilgi için bkz: Cosmos hesabınızda bölge ekleme veya kaldırma nasıl yapılır. Bir Cosmos hesabıyla birden çok bölgede ilişkilendirme, düşük gecikme süresi elde etmek için pek çok senaryoda önemlidir ve [yüksek kullanılabilirlik](high-availability.md) dünyanın dört bir yanındaki.

## <a name="throughput-scaling-details"></a>Aktarım hızı ölçeklendirme ayrıntıları

Bir Cosmos kapsayıcı (veya veritabanı) R RU sağlarsanız, Cosmos DB R RU kullanılabilir olmasını sağlar *her* Cosmos hesabınızla ilişkili bölgelerle. Cosmos DB, Cosmos hesabınıza yeni bir bölgeye eklediğiniz her zaman R RU yeni eklenen bölgede otomatik olarak sağlar. Her Cosmos hesabınızla ilişkili bölgelerle R RU'ları almak için Cosmos kapsayıcınızı karşı gerçekleştirilen işlemleri garanti edilir. Belirli bir bölgede RU seçmeli olarak atanamaz - Cosmos hesabınızla ilişkili tüm bölgeler için bir Cosmos kapsayıcı (veya veritabanı) için sağlanan RU'ları sağlanır.

Bir Cosmos kapsayıcı R RU ile yapılandırıldığından ve N bölge Cosmos hesapla ardından ilişkili:

- Cosmos hesabı kapsayıcı üzerindeki genel olarak kullanılabilir toplam RU bir tek bir yazma bölgesi ile yapılandırılmışsa, R = n x
- Cosmos hesabı birden çok yazma bölgeleri, küresel olarak kapsayıcı üzerindeki kullanılabilir toplam RU ile yapılandırılmışsa, R = (, N + 1). Ek R RU'ları otomatik olarak bölgeler arasında işlem güncelleştirme çakışmalarını ve koruma entropi trafiği için sağlanır.

Tercih ettiğiniz [tutarlılık modelini](consistency-levels.md) aktarım hızı da etkiler. Oturum, tutarlı ön ek ve nihai tutarlılık sınırlanmış eskime durumu veya güçlü tutarlılık kıyasla yaklaşık 2 x okuma aktarım hızı elde edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makalede yardımıyla aktarım hızı yapılandırma bilgi edinebilirsiniz:

* [Alma ve kapsayıcılar ve veritabanları için aktarım hızı ayarlama](set-throughput.md) 

