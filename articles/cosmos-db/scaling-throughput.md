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
ms.openlocfilehash: 7e3f6d053e9466f07e15b0c2c1092fece76c98a4
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160673"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Azure Cosmos DB'de ölçeklendirme aktarım hızı

Azure Cosmos DB'de sağlanan aktarım hızı birimi/saniye istek gösterilir (RU/sn, plural: RU). RU, hem okuma hem de maliyeti ölçmek ve aşağıdaki görüntüde gösterildiği gibi yazma işlemleri Cosmos kapsayıcınızı karşı:

![İstek Birimleri](./media/scale-throughput/figure1.png)

RUs Cosmos kapsayıcı veya Cosmos veritabanı sağlayabilirsiniz. Bir kapsayıcı RU, kapsayıcısı üzerinde gerçekleştirilen işlemler için özel olarak kullanılabilir. Bir veritabanı üzerinde RU (hariç tüm kapsayıcıları ile özel olarak atanan RU) Bu veritabanındaki tüm kapsayıcılar arasında paylaşılır.

Esnek ölçeklendirme aktarım hızı için artırabilir veya sağlanan RU/sn istediğiniz zaman azaltabilirsiniz. Daha fazla bilgi için [yapılır sağlama aktarım hızı](set-throughput.md) ve Cosmos kapsayıcılar ve veritabanlarını esnek biçimde ölçeklendirin. Aktarım hızı ölçeklendirme için genel olarak, ekleme veya bölgeler, Cosmos hesabınızda herhangi bir zamanda kaldırın. Daha fazla bilgi için [veritabanı hesabınızı Ekle/Kaldır bölgelerden](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Bir Cosmos hesabıyla birden çok bölgede ilişkilendirme, düşük gecikme süresi elde etmek için pek çok senaryoda önemlidir ve [yüksek kullanılabilirlik](high-availability.md) dünyanın dört bir yanındaki.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>sağlanan aktarım hızı bölgede dağıtılır nasıl

Cosmos kapsayıcı (veya veritabanı) 'R' RU sağlarsanız, Cosmos DB, 'R' RU kullanılabilir olmasını sağlar *her* Cosmos hesabınızla ilişkili bölge. Cosmos DB, yeni bir bölgeye hesabınıza eklediğiniz her zaman 'R' RU yeni eklenen bölgede otomatik olarak sağlar. Cosmos kapsayıcınızı karşı gerçekleştirilen işlemleri, her bölgede 'R' RU'ları almak için garanti edilir. Belirli bir bölgede RU seçmeli olarak atanamaz. Bir Cosmos kapsayıcı (veya veritabanı) RU, Cosmos hesabınızla ilişkili tüm bölgeler için sağlanır.

Bir Cosmos kapsayıcı 'R' RU ile yapılandırıldığından ve 'N' sayıda bölge Cosmos hesapla ardından ilişkili:

- Cosmos hesabı kapsayıcı üzerindeki genel olarak kullanılabilir toplam RU bir tek bir yazma bölgesi ile yapılandırılmışsa, R = n x

- Cosmos hesabı birden çok yazma bölgeleri, küresel olarak kapsayıcı üzerindeki kullanılabilir toplam RU ile yapılandırılmışsa, R = (, N + 1). Ek R RU'ları otomatik olarak bölgeler arasında işlem güncelleştirme çakışmalarını ve koruma entropi trafiği için sağlanır.

Tercih ettiğiniz [tutarlılık modelini](consistency-levels.md) aktarım hızı da etkiler. Oturum, tutarlı ön ek ve nihai tutarlılık sınırlanmış eskime durumu veya güçlü tutarlılık kıyasla yaklaşık 2 x okuma aktarım hızı elde edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makalede yardımıyla aktarım hızı yapılandırma bilgi edinebilirsiniz:

* [Alma ve kapsayıcılar ve veritabanları için aktarım hızı ayarlama](set-throughput.md) 

