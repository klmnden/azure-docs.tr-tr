---
title: PostgreSQL için Azure veritabanı'nda sınırlamaları
description: Bu makalede, bağlantı ve depolama altyapısı seçenekleri sayısı gibi bir PostgreSQL için Azure veritabanı'nda sınırlamalar açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 1/22/2019
ms.openlocfilehash: 843107b8d251c2073ba9e02beacb16ab7615eca6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60559895"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı'nda sınırlamaları
Aşağıdaki bölümlerde, kapasite ve veritabanı hizmetinde işlev sınırlamaları açıklanmaktadır.

## <a name="maximum-connections"></a>Bağlantı sayısı üst sınırı
Fiyatlandırma katmanı ve sanal çekirdek başına bağlantıları sayısı aşağıdaki gibidir: 

|**Fiyatlandırma Katmanı**| **Sanal çekirdek**| **En fazla bağlantı sayısı** |
|---|---|---|
|Temel| 1| 50 |
|Temel| 2| 100 |
|Genel Amaçlı| 2| 150|
|Genel Amaçlı| 4| 250|
|Genel Amaçlı| 8| 480|
|Genel Amaçlı| 16| 950|
|Genel Amaçlı| 32| 1500|
|Genel Amaçlı| 64| 1900|
|Bellek için İyileştirilmiş| 2| 300|
|Bellek için İyileştirilmiş| 4| 500|
|Bellek için İyileştirilmiş| 8| 960|
|Bellek için İyileştirilmiş| 16| 1900|
|Bellek için İyileştirilmiş| 32| 1900|

Bağlantı sınırı aştıklarında aşağıdaki hata iletisini alabilirsiniz:
> Önemli: ne yazık ki zaten çok fazla sayıda istemci

Azure sistem PostgreSQL sunucusu için Azure veritabanı izlemek için beş bağlantı gerektirir. 

## <a name="functional-limitations"></a>İşlev sınırlamaları
### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanları gelen ve giden dinamik ölçeklendirme şu anda desteklenmiyor.
- Sunucu depolama boyutunu küçültme şu anda desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor. Bir sonraki ana sürümüne yükseltmek istiyorsanız, olması bir [döküm ve geri yükleme](./howto-migrate-using-dump-and-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

### <a name="vnet-service-endpoints"></a>Sanal ağ hizmet uç noktaları
- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

### <a name="restoring-a-server"></a>Bir sunucuya geri yükleme
- PITR özelliğini kullanırken, yeni sunucuya bağlı olduğu sunucusu olarak aynı fiyatlandırma katmanı yapılandırmaları ile oluşturulur.
- Geri yükleme sırasında oluşturulan yeni sunucunun özgün sunucuya vardı güvenlik duvarı kuralları yok. Güvenlik duvarı kuralları bu yeni sunucu için ayrı olarak ayarlanması gerekir.
- Silinen bir sunucuya geri yükleme desteklenmiyor.

### <a name="utf-8-characters-on-windows"></a>Windows üzerinde UTF-8 karakter
- Bazı senaryolarda, UTF-8 karakter tamamen açık kaynak PostgreSQL için Azure veritabanı etkileyen Windows üzerinde PostgreSQL desteklenmez. İş parçacığı Lütfen bakın [hata #15476 postgresql arşiv](https://www.postgresql-archive.org/BUG-15476-Problem-on-show-trgm-with-4-byte-UTF-8-characters-td6056677.html) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
- Anlamak [her fiyatlandırma katmanında nelerin kullanılabildiğini](concepts-pricing-tiers.md)
- Hakkında bilgi edinin [PostgreSQL veritabanı sürümlerinde desteklenir](concepts-supported-versions.md)
- Gözden geçirme [nasıl yedekleme ve Azure portalını kullanarak PostgreSQL için Azure veritabanı'nda bir sunucu geri yükleme](howto-restore-server-portal.md)
