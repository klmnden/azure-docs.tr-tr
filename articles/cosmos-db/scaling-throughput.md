---
title: Azure Cosmos DB'de ölçeklendirme aktarım hızı
description: Bu makalede, nasıl Azure Cosmos DB aktarım hızını esnek bir şekilde ölçeklenen açıklanır.
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: dd17b08a16dedf474b2a1eca8fa8034672610c1f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55454445"
---
# <a name="globally-scale-provisioned-throughput"></a>Genel olarak sağlanan aktarım hızını ölçeklendirme 

Azure Cosmos DB'de sağlanan aktarım hızı birimi/saniye istek gösterilir (RU/sn, plural: RU). RU, hem okuma hem de maliyeti ölçmek ve aşağıdaki görüntüde gösterildiği gibi yazma işlemleri Cosmos kapsayıcınızı karşı:

![İstek Birimleri](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

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

