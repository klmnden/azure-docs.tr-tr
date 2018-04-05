---
title: PostgreSQL Azure veritabanındaki sınırlamaları
description: Bu makalede, bağlantı ve depolama altyapısı seçenekleri sayısı gibi PostgreSQL Azure veritabanındaki sınırlamalar açıklanır.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 7e06cdba7c9c9f7e5c1d621e7421a18c342c0fdb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>PostgreSQL Azure veritabanındaki sınırlamaları
Aşağıdaki bölümlerde, kapasite ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır.

## <a name="pricing-tier-maximums"></a>Fiyatlandırma katmanı üst sınırlar
Azure veritabanı PostgreSQL için bir sunucu oluştururken seçebileceğiniz birden çok fiyatlandırma katmanlarına sahip. Daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanı'nda fiyatlandırma katmanlarına](concepts-pricing-tiers.md).  

Aşağıdaki gibi maksimum sayısını bağlantıları, işlem birimleri ve her fiyatlandırma katmanının depolama vardır: 

|Fiyatlandırma Katmanı| İşlem oluşturma| vCore(s)| En fazla bağlantı |
|---|---|---|---|
|Temel| 4. Nesil| 1| 50 |
|Temel| 4. Nesil| 2| 100 |
|Temel| 5. Nesil| 1| 50 |
|Temel| 5. Nesil| 2| 100 |
|Genel Amaçlı| 4. Nesil| 2| 150|
|Genel Amaçlı| 4. Nesil| 4| 250|
|Genel Amaçlı| 4. Nesil| 8| 480|
|Genel Amaçlı| 4. Nesil| 16| 950|
|Genel Amaçlı| 4. Nesil| 32| 1500|
|Genel Amaçlı| 5. Nesil| 2| 150|
|Genel Amaçlı| 5. Nesil| 4| 250|
|Genel Amaçlı| 5. Nesil| 8| 480|
|Genel Amaçlı| 5. Nesil| 16| 950|
|Genel Amaçlı| 5. Nesil| 32| 1500|
|Bellek için İyileştirilmiş| 5. Nesil| 2| 150|
|Bellek için İyileştirilmiş| 5. Nesil| 4| 250|
|Bellek için İyileştirilmiş| 5. Nesil| 8| 480|
|Bellek için İyileştirilmiş| 5. Nesil| 16| 950|

Bağlantı sınırı aştıklarında, aşağıdaki hata iletisini alabilirsiniz:
> Önemli: ne yazık ki zaten çok fazla sayıda istemci

Azure sistemi PostgreSQL server Azure veritabanındaki izlemek için beş bağlantı gerektirir. 

## <a name="functional-limitations"></a>İşlev sınırlamaları
### <a name="scale-operations"></a>Ölçek işlemleri
1.  Dinamik sunucularının ölçeklendirme fiyatlandırma katmanları arasında şu anda desteklenmiyor. Diğer bir deyişle, temel, genel amaçlı veya bellek için iyileştirilmiş katmanlar arasında geçiş yapma.
2.  Sunucu depolama boyutunu azaltmak şu anda desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürüm yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmiyor.

### <a name="subscription-management"></a>Abonelik yönetimi
- Dinamik olarak sunucularını Abonelikleriniz ve kaynak grupları arasında taşıma şu anda desteklenmiyor.

### <a name="point-in-time-restore-pitr"></a>Noktası-içinde--geri yükleme (PITR)
1.  PITR özelliğini kullanırken, yeni sunucu dayanır sunucusu olarak aynı yapılandırmaya sahip oluşturulur.
2.  Silinen bir sunucuya geri yüklenmesi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Anlamak [her fiyatlandırma katmanının kullanılabilir](concepts-pricing-tiers.md)
- Hakkında bilgi edinin [PostgreSQL veritabanı sürümleri desteklenir](concepts-supported-versions.md)
- Gözden geçirme [yedeklemek ve Azure veritabanındaki bir sunucu için Azure portalını kullanarak PostgreSQL geri yükleme](howto-restore-server-portal.md)
