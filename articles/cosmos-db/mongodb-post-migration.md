---
title: Azure Cosmos DB'nin MongoDB kullanarak, geçiş sonrası iyileştirme adımları
description: Bu belge Mongo DB için Azure Cosmos DB'nin API'sine adresinden geçiş sonrası iyileştirme teknikleri sağlar.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: roaror
ms.openlocfilehash: c0c761fef481a1fdaa027f1329e9a4e72d62985a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61331216"
---
# <a name="post-migration-optimization-steps-when-using-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB kullanarak, geçiş sonrası iyileştirme adımları 

MongoDB için API Azure Cosmos DB'nin MongoDB veritabanına depolanan veriler geçiş yaptıktan sonra Azure Cosmos DB'ye bağlanmak ve verileri yönetin. Bu kılavuz, geçişten sonra dikkate almanız gereken adımları sağlar. Bkz: [MongoDB öğretici için Azure Cosmos DB API MongoDB geçişi](../dms/tutorial-mongodb-cosmos-db.md) geçiş adımları için.

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:
- [Uygulamanızı bağlayın](#connect-account)
- [Dizin oluşturma ilkesini en iyi duruma getirme](#indexing)
- [MongoDB için Azure Cosmos DB API için genel dağıtımı yapılandırma](#distribute-data)
- [Kümesi tutarlılık düzeyi](#consistency)

> [!NOTE]
> Yalnızca zorunlu geçiş sonrası adımı, uygulama düzeyinde yeni Azure Cosmos DB hesabınıza işaret edecek şekilde uygulamanızın bağlantı dizesinde ile değişiyor. Diğer tüm geçiş adımları, en iyi duruma getirmeleri önerilir.
>

## <a id="connect-account"></a>Uygulamanızı bağlayın 

1. Yeni bir pencere oturum içinde [Azure portalı](https://www.portal.azure.com/)
2. Gelen [Azure portalında](https://www.portal.azure.com/), açık sol bölmede **tüm kaynakları** menü ve verilerinizi geçirilmiş Azure Cosmos DB hesabı bulunamadı.
3. Açık **bağlantı dizesi** dikey penceresi. Sağ bölme, hesabınıza başarıyla bağlanmak için gereken tüm bilgileri içerir.
4. Uygulamanızı, MongoDB bağlantı için bağlantı bilgilerini Azure Cosmos DB API yansıtmak için uygulamanızın yapılandırma (veya ilgili diğer yerler) kullanın. 
![Bağlantı dizesi](./media/mongodb-post-migration/connection-string.png)

Daha fazla ayrıntı için lütfen bkz [bir MongoDB uygulamasını Azure Cosmos DB'ye bağlanmak](connect-mongodb-account.md) sayfası.

## <a id="indexing"></a>Dizin oluşturma ilkesini en iyi duruma getirme

Tüm veri alanları otomatik olarak, varsayılan olarak, Azure Cosmos DB'ye veri geçişi sırasında dizine eklenir. Çoğu durumda, bu varsayılan dizin oluşturma ilkesi kabul edilebilir. Genel olarak, dizinler kaldırılıyor yazma isteklerine en iyi duruma getirir ve varsayılan dizinleme ilkesinin (yani, otomatik dizin oluşturma) sahip okuma isteklerinin en iyi duruma getirir.

Dizin oluşturma ile ilgili daha fazla bilgi için bkz: [veri MongoDB için Azure Cosmos DB'nin API'SİNDE dizinleme](mongodb-indexing.md) yanı sıra [Azure Cosmos DB'de dizinleme](index-overview.md) makaleler.

## <a id="distribute-data"></a>Verilerinizi genel olarak dağıtma

Azure Cosmos DB, tüm kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/regions/#services) dünya çapında. Azure Cosmos DB hesabınız için varsayılan tutarlılık düzeyini seçtikten sonra bir veya daha fazla Azure bölgeleri (genel dağıtım, gereksinimlerinize bağlı olarak) ilişkilendirebilirsiniz. Yüksek kullanılabilirlik ve iş sürekliliği için en az 2 bölgelerinde çalıştırılan her zaman önerilir. İpuçları için gözden geçirebilirsiniz [çok bölgeli dağıtımlar Azure Cosmos DB'de maliyetini en iyi duruma getirme](optimize-cost-regions.md).

Verilerinizi genel olarak dağıtmak için bkz. [için MongoDB verilerini küresel olarak Azure Cosmos DB'nin API'sini dağıtmak](tutorial-global-distribution-mongodb.md). 

## <a id="consistency"></a>Kümesi tutarlılık düzeyi
Azure Cosmos DB, iyi tanımlanmış 5 sunar [tutarlılık düzeyleri](consistency-levels.md). MongoDB ve Azure Cosmos DB tutarlılık düzeyleri arasındaki eşleme hakkında bilgi edinmek için okumaya devam [tutarlılık düzeyleri ve Azure Cosmos DB API'leri](consistency-levels-across-apis.md). Varsayılan tutarlılık düzeyini oturum tutarlılık düzeyidir. Tutarlılık düzeyi değiştirme isteğe bağlıdır ve uygulamanız için iyileştirin. Azure portalını kullanarak tutarlılık düzeyini değiştirmek için:

1. Git **varsayılan tutarlılık** ayarları dikey penceresi.
2. Seçin, [tutarlılık düzeyi](consistency-levels.md)

Çoğu kullanıcı oturumu varsayılan tutarlılık ayarını, tutarlılık düzeyinde bırakın. Ancak, [çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans ödün](consistency-levels-tradeoffs.md). 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB uygulaması bağlama](connect-mongodb-account.md)
* [Studio 3t'yi kullanarak Azure Cosmos DB hesabına bağlanma](mongodb-mongochef.md)
* [Azure Cosmos DB'nin MongoDB kullanarak genel olarak nasıl dağıtacağınızı okur](mongodb-readpreference.md)
* [MongoDB için Azure Cosmos DB'nin API'si ile verileri süresi dolacak](mongodb-time-to-live.md)
* [Azure cosmos DB tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos DB'yi dizine ekleme](index-overview.md)
* [İstek birimleri Azure cosmos DB](request-units.md)





