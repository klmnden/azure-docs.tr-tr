---
title: Azure Cosmos DB ile bölgesel varlığı
description: Bu makalede, Azure Cosmos DB ile farklı bulut ortamları hakkında bölgesel varlığını açıklanmaktadır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 851da72a308b164b9a01b7ccfa3d541fc06810ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60926275"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Azure Cosmos DB ile bölgesel varlığı

Azure Cosmos DB azure'da temel bir hizmettir ve varsayılan olarak, her zaman Azure'un kullanılabilir olduğu tüm bölgelerde kullanılabilir durumdadır. Şu anda Azure kullanılabilir [54 bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) dünya çapında. 

[![Azure Cosmos DB kullanılabildiği bölgeler](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB, tüm beş farklı Azure bulut ortamlarında müşterilerine kullanılabilir:

* **Azure genel** bulut, dünya çapında kullanılabilir.

* **Azure Çin 21Vianet** Çin'de ülkenin en büyük internet sağlayıcılarından biri olan 21Vianet arasındaki benzersiz iş ortaklığı aracılığıyla kullanılabilir.

* **Azure Almanya** Alman veri güvenilen kişisi olarak davranan Deutsche Telekom'ın bir yan kuruluşu olan T-Systems International GmbH denetimindeki Almanya'daki veri kalır, müşteri sağlayan bir veri Emanetçisi modeli altında hizmetleri sağlar.

* **Azure kamu** ABD'de ABD devlet kurumları ile bunların iş ortakları için dört bölgelerinde kullanılabilir. 

* **Savunma Bakanlığı (DoD) için Azure kamu** ABD ABD Savunma Bakanlığı için iki bölgede kullanılabilir.

## <a name="regional-presence-with-global-distribution"></a>Bölgesel varlığı ile genel dağıtım

(SQL, MongoDB, Cassandra, Gremlin ve tablo gibi) Azure Cosmos DB tarafından kullanıma sunulan tüm API'leri varsayılan olarak tüm Azure bölgelerinde kullanılabilir. Örneğin, MongoDB olabilir ve yalnızca tüm genel Azure bölgeleri, aynı zamanda Çin, Almanya, kamu ve Savunma Bakanlığı gibi bağımsız bulutlarda Azure Cosmos DB Cassandra API (DoD) bölgeleri açığa.

Azure Cosmos DB, bir [Global olarak dağıtılmış](distribute-data-globally.md) veritabanı hizmeti. Dilediğiniz sayıda Azure bölgesinde Azure Cosmos hesabınızla ilişkilendirebilirsiniz ve verilerinizin otomatik ve şeffaf şekilde çoğaltılır. Ekleyebilir veya bir bölge herhangi bir zamanda Azure Cosmos hesabınızı kaldırın. Anahtar teslim küresel dağıtım özelliği ve birden çok yönetilen çoğaltma protokolü ile Azure Cosmos DB 10 MS'den kısa okuyup 99. yüzdebirlik dilimde gecikme süreleri 99,999 okuma ve yazma kullanılabilirliğini ve sağlanan esnek bir şekilde ölçeklendirme olanağı sunar. aktarım hızı için okur ve Azure Cosmos hesabınızla ilişkili tüm bölgeler arasında yazar. Azure Cosmos DB, ayrıca beş iyi tanımlanmış tutarlılık modeli sunar ve belirli bir tutarlılık modelini, verilere uygulanacak seçebilirsiniz. Son olarak, Azure Cosmos DB tek veritabanı sağlayan kapsamlı bir sektörde hizmetidir [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) kapsamlı sağlanan aktarım hızı, 99. yüzdebirlik dilimde yüksek kullanılabilirlik, gecikme süresi ve tutarlılık. Tüm Azure bulutlarında yukarıdaki özellikleri kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık aşağıdaki makalelerde Azure Cosmos DB'nin temel kavramlar hakkında bilgi edinebilirsiniz:

* [Genel veri dağıtımı](distribute-data-globally.md)
* [Bir Azure Cosmos DB hesabını yönetme](manage-account.md)
* [Azure Cosmos kapsayıcılar ve veritabanları için sağlama aktarım hızı](set-throughput.md)
* [Azure Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
