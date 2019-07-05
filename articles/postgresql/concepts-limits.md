---
title: PostgreSQL - tek bir sunucu için Azure veritabanı sınırları
description: Bu makalede, PostgreSQL - bağlantı ve depolama altyapısı seçenekleri sayısı gibi tek bir sunucu için Azure veritabanı'nda sınırları açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/25/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: e4752112acf136d9ffb19a0b7383bc3aff5de5e0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448094"
---
# <a name="limits-in-azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı sınırları
Aşağıdaki bölümlerde, kapasite ve veritabanı hizmetinde işlev sınırlamaları açıklanmaktadır. Kaynak (işlem, bellek, depolama) katmanları hakkında bilgi edinmek istiyorsanız, bkz. [fiyatlandırma katmanları](concepts-pricing-tiers.md) makalesi.


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
|Bellek için İyileştirilmiş| 32| 1987|

Bağlantı sınırı aştıklarında aşağıdaki hata iletisini alabilirsiniz:
> Önemli: ne yazık ki zaten çok fazla sayıda istemci

Azure sistem PostgreSQL sunucusu için Azure veritabanı izlemek için beş bağlantı gerektirir. 

## <a name="functional-limitations"></a>İşlev sınırlamaları
### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanları gelen ve giden dinamik ölçeklendirme şu anda desteklenmiyor.
- Sunucu depolama boyutunu küçültme şu anda desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor. Bir sonraki ana sürümüne yükseltmek istiyorsanız, olması bir [döküm ve geri yükleme](./howto-migrate-using-dump-and-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

> Sürüm 10, PostgreSQL önce dikkat [PostgreSQL sürüm oluşturma ilkesine](https://www.postgresql.org/support/versioning/) kabul bir _ana sürüm_ ilk artışı olacak şekilde yükseltme _veya_ numarası (ikinci Örneğin, 9.5 için 9.6 kabul bir _ana_ sürüm yükseltmesi).
> 10 sürümden itibaren bir ana sürüm yükseltmesi yalnızca ilk sayı bir değişiklik olarak kabul edilir (örneğin, 10.0 için 10.1 olan bir _küçük_ sürüm yükseltmesi ve 10-11 bir _ana_ sürüm yükseltmesi).

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
