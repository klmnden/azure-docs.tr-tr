---
title: MariaDB için Azure veritabanı'nda sınırlamaları
description: Bu makalede, Azure veritabanı'nda MariaDB, bağlantı ve depolama altyapısı seçenekleri sayısı gibi sınırlamalar açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: e191c656c5485377f62073f52dec0b3dbee7537b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61041284"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda sınırlamaları
Aşağıdaki bölümlerde, kapasitesi, depolama altyapısı desteği, destek ayrıcalığına, veri işleme ifadesi desteği ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır.

## <a name="maximum-connections"></a>Bağlantı sayısı üst sınırı
Fiyatlandırma katmanı ve sanal çekirdek başına bağlantıları sayısı aşağıdaki gibidir:

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**| **En fazla bağlantı sayısı**|
|---|---|---|
|Temel| 1| 50|
|Temel| 2| 100|
|Genel Amaçlı| 2| 300|
|Genel Amaçlı| 4| 625|
|Genel Amaçlı| 8| 1250|
|Genel Amaçlı| 16| 2500|
|Genel Amaçlı| 32| 5000|
|Genel Amaçlı| 64| 10000|
|Bellek için İyileştirilmiş| 2| 600|
|Bellek için İyileştirilmiş| 4| 1250|
|Bellek için İyileştirilmiş| 8| 2500|
|Bellek için İyileştirilmiş| 16| 5000|
|Bellek için İyileştirilmiş| 32| 10000|

Bağlantı sınırı aştıklarında aşağıdaki hata iletisini alabilirsiniz:
> HATA 1040 (08004): Bağlantı sayısı çok fazla

## <a name="storage-engine-support"></a>Depolama altyapısı desteği

### <a name="supported"></a>Desteklenen
- [Innodb](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [BELLEK](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Desteklenmiyor
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [KARA DELİK](https://mariadb.com/kb/en/library/blackhole/)
- [ARŞİV](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmiyor
- DBA rolü: Birçok sunucu parametreleri ve ayarları yanlışlıkla sunucu performansının düşmesine neden veya DBMS ACID özelliklerini negate. Bu nedenle, bir ürün düzeyinde SLA ve hizmet bütünlüğü korumak için bu hizmeti DBA rol kullanıma sunmuyor. Yeni bir veritabanı örneği oluşturulduğunda bu oluşturulur, varsayılan kullanıcı hesabı, veritabanı yönetilen örneğine DDL ve DML deyimleri çoğunu gerçekleştirmek bu kullanıcı sağlar.
- Süper ayrıcalık: Benzer şekilde [Süper ayrıcalık](https://mariadb.com/kb/en/library/grant/#global-privileges) de sınırlıdır.
- DEFINER: Süper oluşturma ayrıcalıkları gerektirir ve sınırlıdır. Bir yedekleme kullanarak veriyi içeri aktarma, kaldırma `CREATE DEFINER` el ile veya kullanılarak komutları `--skip-definer` bir mysqldump gerçekleştirirken komutu.

## <a name="data-manipulation-statement-support"></a>Veri işleme ifadesi desteği

### <a name="supported"></a>Desteklenen
- `LOAD DATA INFILE` desteklenir, ancak `[LOCAL]` parametresi belirtilen ve bir UNC yolu (Azure depolama bağlı SMB) yönlendirilir.

### <a name="unsupported"></a>Desteklenmiyor
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>İşlev sınırlamaları

### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanları gelen ve giden dinamik ölçeklendirme şu anda desteklenmiyor.
- Sunucu depolama boyutunu küçültme desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- PITR özelliğini kullanırken, yeni sunucuya bağlı olduğu sunucusuyla aynı yapılandırmaları ile oluşturulur.
- Silinen bir sunucuya geri yükleme desteklenmiyor.

### <a name="subscription-management"></a>Abonelik yönetimi
- Önceden oluşturulmuş sunucuları, abonelik ve kaynak grubu üzerinde dinamik olarak taşıma şu anda desteklenmiyor.

### <a name="vnet-service-endpoints"></a>Sanal ağ hizmet uç noktaları
- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- Bağlantı kurulduktan sonra MariaDB sunucuyu yanlış sunucu sürümünü görüntüler. Doğru sunucu örneği altyapı sürümü almak için kullanın `select version();` komutu.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında nelerin kullanılabildiğini](concepts-pricing-tiers.md)
- [Desteklenen MariaDB veritabanı sürümleri](concepts-supported-versions.md)
