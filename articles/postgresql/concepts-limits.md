---
title: "PostgreSQL Azure veritabanındaki sınırlamaları"
description: "Bu makalede, bağlantı ve depolama altyapısı seçenekleri sayısı gibi PostgreSQL Azure veritabanındaki sınırlamalar açıklanır."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ba05308039e9743dd207333476e61a45c0ca166a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>PostgreSQL Azure veritabanındaki sınırlamaları
Azure veritabanı PostgreSQL hizmeti için genel önizlemede değil. Aşağıdaki bölümlerde, kapasite ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır.

## <a name="pricing-tier-maximums"></a>Fiyatlandırma katmanı üst sınırlar
Azure veritabanı PostgreSQL için bir sunucu oluştururken seçebileceğiniz birden çok fiyatlandırma katmanlarına sahip. Daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanı'nda fiyatlandırma katmanlarına](concepts-pricing-tiers.md).  

Aşağıdaki gibi maksimum sayısını bağlantıları, işlem birimleri ve her fiyatlandırma katmanının depolama vardır: 

|Fiyatlandırma Katmanı| İşlem oluşturma| vCore(s)| En fazla bağlantı |
|---|---|---|---|
|Temel| Gen 4| 1| 50 |
|Temel| Gen 4| 2| 100 |
|Temel| Gen 5| 1| 50 |
|Temel| Gen 5| 2| 100 |
|Genel Amaçlı| Gen 4| 2| 150|
|Genel Amaçlı| Gen 4| 4| 250|
|Genel Amaçlı| Gen 4| 8| 480|
|Genel Amaçlı| Gen 4| 16| 950|
|Genel Amaçlı| Gen 4| 32| 1500|
|Genel Amaçlı| Gen 5| 2| 150|
|Genel Amaçlı| Gen 5| 4| 250|
|Genel Amaçlı| Gen 5| 8| 480|
|Genel Amaçlı| Gen 5| 16| 950|
|Genel Amaçlı| Gen 5| 32| 1500|
|Bellek için İyileştirilmiş| Gen 5| 2| 150|
|Bellek için İyileştirilmiş| Gen 5| 4| 250|
|Bellek için İyileştirilmiş| Gen 5| 8| 480|
|Bellek için İyileştirilmiş| Gen 5| 16| 950|
|Bellek için İyileştirilmiş| Gen 5| 32| 1900|

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
