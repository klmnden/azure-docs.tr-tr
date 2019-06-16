---
title: MySQL için Azure veritabanı'nda sınırlamaları
description: Bu makalede, bağlantı ve depolama altyapısı seçenekleri sayısı gibi bir MySQL için Azure veritabanı'nda sınırlamalar açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/6/2018
ms.openlocfilehash: 9088e9ad98633b46dc3a7f0ee9002a0dd9fc5a55
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65551886"
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda sınırlamaları
Aşağıdaki bölümlerde, kapasitesi, depolama altyapısı desteği, destek ayrıcalığına, veri işleme ifadesi desteği ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır. Ayrıca bkz: [genel kısıtlamalar](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) MySQL veritabanı altyapısı için geçerlidir.

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
- [Innodb](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [BELLEK](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Desteklenmiyor
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [KARA DELİK](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARŞİV](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERASYON](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmiyor
- DBA rolü: Birçok sunucu parametreleri ve ayarları yanlışlıkla sunucu performansının düşmesine neden veya DBMS ACID özelliklerini negate. Bu nedenle, bir ürün düzeyinde SLA ve hizmet bütünlüğü korumak için bu hizmeti DBA rol kullanıma sunmuyor. Yeni bir veritabanı örneği oluşturulduğunda bu oluşturulur, varsayılan kullanıcı hesabı, veritabanı yönetilen örneğine DDL ve DML deyimleri çoğunu gerçekleştirmek bu kullanıcı sağlar. 
- Süper ayrıcalık: Benzer şekilde [Süper ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) de sınırlıdır.
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
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor. Bir sonraki ana sürümüne yükseltmek istiyorsanız, olması bir [döküm ve geri yükleme](./concepts-migrate-dump-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- PITR özelliğini kullanırken, yeni sunucuya bağlı olduğu sunucusuyla aynı yapılandırmaları ile oluşturulur.
- Silinen bir sunucuya geri yükleme desteklenmiyor.

### <a name="vnet-service-endpoints"></a>Sanal ağ hizmet uç noktaları
- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

### <a name="storage-size"></a>Depolama boyutu
- Lütfen [fiyatlandırma katmanları](concepts-pricing-tiers.md) fiyatlandırma katmanı başına depolama boyutu sınırları.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- Bağlantı kurulduktan sonra MySQL sunucu örneğinde yanlış sunucu sürümünü görüntüler. Doğru sunucu örneği altyapı sürümü almak için kullanın `select version();` komutu.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında nelerin kullanılabildiğini](concepts-pricing-tiers.md)
- [MySQL veritabanı için desteklenen sürümler](concepts-supported-versions.md)
