---
title: MySQL için Azure veritabanındaki sınırlamaları
description: Bu makalede, MySQL, bağlantı ve depolama altyapısı seçenekleri sayısı gibi Azure veritabanındaki sınırlamalar açıklanır.
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 2fa69182b4238cfd19fcc9571e4327512e9528c1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanındaki sınırlamaları
Aşağıdaki bölümlerde, kapasite, depolama altyapısı desteği, ayrıcalık desteği, veri işleme ifadesi desteği ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır. Ayrıca bkz. [genel sınırlamaları](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) MySQL veritabanı altyapısı için geçerlidir.

## <a name="service-tier-maximums"></a>Hizmet katmanı üst sınırlar
Azure veritabanı MySQL için bir sunucu oluştururken seçebileceğiniz birden çok hizmet katmanı vardır. Daha fazla bilgi için bkz: [Azure veritabanı fiyatlandırma katmanlarına MySQL için](concepts-pricing-tiers.md).  

Aşağıdaki gibi maksimum sayısını bağlantıları, işlem birimleri ve depolama her hizmet katmanında vardır: 

|**Fiyatlandırma Katmanı**| **İşlem oluşturma**|**vCore(s)**| **En fazla bağlantı**|
|---|---|---|---|
|Temel| 4. Nesil| 1| 50|
|Temel| 4. Nesil| 2| 100|
|Temel| 5. Nesil| 1| 50|
|Temel| 5. Nesil| 2| 100|
|Genel Amaçlı| 4. Nesil| 2| 300|
|Genel Amaçlı| 4. Nesil| 4| 625|
|Genel Amaçlı| 4. Nesil| 8| 1250|
|Genel Amaçlı| 4. Nesil| 16| 2500|
|Genel Amaçlı| 4. Nesil| 32| 5000|
|Genel Amaçlı| 5. Nesil| 2| 300|
|Genel Amaçlı| 5. Nesil| 4| 625|
|Genel Amaçlı| 5. Nesil| 8| 1250|
|Genel Amaçlı| 5. Nesil| 16| 2500|
|Genel Amaçlı| 5. Nesil| 32| 5000|
|Bellek için İyileştirilmiş| 5. Nesil| 2| 600|
|Bellek için İyileştirilmiş| 5. Nesil| 4| 1250|
|Bellek için İyileştirilmiş| 5. Nesil| 8| 2500|
|Bellek için İyileştirilmiş| 5. Nesil| 16| 5000|

Çok fazla bağlantı erişildiğinde, aşağıdaki hata iletisini alabilirsiniz:
> 1040 (08004). hata: Çok fazla bağlantı

## <a name="storage-engine-support"></a>Depolama altyapısı desteği

### <a name="supported"></a>Desteklenen
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [BELLEK](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Desteklenmiyor
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARŞİV](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERASYON](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmiyor
- DBA rolünün: pek çok sunucu parametreleri ve ayarları yanlışlıkla sunucu performansının düşmesine neden veya DBMS ACID özelliklerini negate. Bu nedenle, bir ürün düzeyinde SLA ve hizmet bütünlüğü korumak için bu hizmet DBA rolünün kullanıma sunmuyor. Yeni bir veritabanı örneği oluşturulduğunda bu oluşturulur, varsayılan kullanıcı hesabı yönetilen veritabanı örneğinde DDL ve DML deyimleri çoğunu gerçekleştirmek bu kullanıcının sağlar. 
- Süper ayrıcalık: benzer şekilde [Süper ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) de sınırlıdır.

## <a name="data-manipulation-statement-support"></a>Veri işleme ifadesi desteği

### <a name="supported"></a>Desteklenen
- Yük veri GİRİŞDOSYASI - desteklenir, ancak bir UNC yolu (Azure depolama XSMB bağlanan) yönlendirildiği [Yerel] parametresini belirtmeniz gerekir.

### <a name="unsupported"></a>Desteklenmiyor
- SEÇİN... ÇIKIŞDOSYASI

## <a name="functional-limitations"></a>İşlev sınırlamaları

### <a name="scale-operations"></a>Ölçek işlemleri
- Dinamik sunucularının ölçeklendirme fiyatlandırma katmanları arasında şu anda desteklenmiyor. Diğer bir deyişle, temel, genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanları arasında geçiş yapma.
- Sunucu depolama boyutunu azaltmak desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürüm yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- Farklı bir hizmet katmanı ve/veya bir işlem birimleri ve depolama boyutu geri izin verilmiyor.
- Silinen bir sunucuya geri yüklenmesi desteklenmez.

## <a name="functional-limitations"></a>İşlev sınırlamaları

### <a name="subscription-management"></a>Abonelik yönetimi
- Önceden oluşturulmuş sunucuları abonelik ve kaynak grubu arasında dinamik olarak taşıma şu anda desteklenmiyor.

## <a name="current-known-issues"></a>Geçerli bilinen sorunlar
- Bağlantı kurulduktan sonra MySQL server örneği yanlış sunucu sürümünü görüntüler. Doğru sunucu örneği sürüm almak için select version() kullanın; MySQL isteminde komutu.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında nelerin kullanılabildiğini](concepts-pricing-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
