---
title: PostgreSQL için Azure veritabanı'nda sınırlamaları
description: Bu makalede, bağlantı ve depolama altyapısı seçenekleri sayısı gibi bir PostgreSQL için Azure veritabanı'nda sınırlamalar açıklanır.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/30/2018
ms.openlocfilehash: 52dfac826de86f67b3143cce49c35088547c4b39
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171781"
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
|Bellek için İyileştirilmiş| 2| 150|
|Bellek için İyileştirilmiş| 4| 250|
|Bellek için İyileştirilmiş| 8| 480|
|Bellek için İyileştirilmiş| 16| 950|

Bağlantı sınırı aştıklarında aşağıdaki hata iletisini alabilirsiniz:
> Önemli: ne yazık ki zaten çok fazla sayıda istemci

Azure sistem PostgreSQL sunucusu için Azure veritabanı izlemek için beş bağlantı gerektirir. 

## <a name="functional-limitations"></a>İşlev sınırlamaları
### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanları gelen ve giden dinamik ölçeklendirme şu anda desteklenmiyor.
- Sunucu depolama boyutunu küçültme şu anda desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor.

### <a name="subscription-management"></a>Abonelik yönetimi
- Abonelikler ve kaynak grupları arasında sunucuları dinamik olarak taşıma şu anda desteklenmiyor.

### <a name="vnet-service-endpoints"></a>Sanal ağ hizmet uç noktaları
- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

### <a name="restoring-a-server"></a>Bir sunucuya geri yükleme
- PITR özelliğini kullanırken, yeni sunucuya bağlı olduğu sunucusu olarak aynı fiyatlandırma katmanı yapılandırmaları ile oluşturulur.
- Geri yükleme sırasında oluşturulan yeni sunucunun özgün sunucuya vardı güvenlik duvarı kuralları yok. Güvenlik duvarı kuralları bu yeni sunucu için ayrı olarak ayarlanması gerekir.
- Silinen bir sunucuya geri yükleme desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
- Anlamak [her fiyatlandırma katmanında nelerin kullanılabildiğini](concepts-pricing-tiers.md)
- Hakkında bilgi edinin [PostgreSQL veritabanı sürümlerinde desteklenir](concepts-supported-versions.md)
- Gözden geçirme [nasıl yedekleme ve Azure portalını kullanarak PostgreSQL için Azure veritabanı'nda bir sunucu geri yükleme](howto-restore-server-portal.md)
