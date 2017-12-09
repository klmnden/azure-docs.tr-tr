---
title: "MySQL için Azure veritabanındaki kısıtlamaları | Microsoft Docs"
description: "MySQL için Azure veritabanındaki Önizleme sınırlamaları açıklar."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 12/09/2017
ms.openlocfilehash: 65dc158a3a8c88a02d66bff7abe34d457cfef10a
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanındaki sınırlamaları
MySQL hizmeti için Azure veritabanı genel önizlemede değil. Aşağıdaki bölümlerde, kapasite, depolama altyapısı desteği, ayrıcalık desteği, veri işleme ifadesi desteği ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır. Ayrıca bkz. [genel sınırlamaları](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) MySQL veritabanı altyapısı için geçerlidir.

## <a name="service-tier-maximums"></a>Hizmet katmanı üst sınırlar
Azure veritabanı MySQL için bir sunucu oluştururken seçebileceğiniz birden çok hizmet katmanı vardır. Daha fazla bilgi için bkz: [her hizmet katmanında nelerin kullanılabildiğini anlama](concepts-service-tiers.md).  

En fazla sayı bulunmadığını bağlantıları, işlem birimleri ve depolama her hizmet katmanında Önizleme sırasında gibi: 

|                            |                   |
| :------------------------- | :---------------- |
| **En fazla bağlantı**        |                   |
| Temel 50 işlem birimleri     | 50 bağlantıları    |
| Temel 100 işlem birimleri    | 100 bağlantı   |
| Standart 100 işlem birimleri | 200 bağlantıları   |
| Standart 200 işlem birimleri | 400 bağlantıları   |
| Standart 400 işlem birimleri | 800 bağlantıları   |
| Standart 800 işlem birimleri | 1600 bağlantıları  |
| **En fazla işlem birimleri**      |                   |
| Temel hizmet katmanı         | 100 işlem birimleri |
| Standart hizmet katmanı      | 800 işlem birimleri |
| **En fazla depolama**            |                   |
| Temel hizmet katmanı         | 1 TB              |
| Standart hizmet katmanı      | 1 TB              |

Çok fazla bağlantı erişildiğinde, aşağıdaki hata iletisini alabilirsiniz:
> 1040 (08004). hata: Çok fazla bağlantı

## <a name="storage-engine-support"></a>Depolama altyapısı desteği

### <a name="supported"></a>Destekleniyor
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [BELLEK](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Desteklenmeyen
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [KARA DELİK](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARŞİV](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERASYON](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmeyen
- [Süper ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super)

## <a name="data-manipulation-statement-support"></a>Veri işleme ifadesi desteği

### <a name="supported"></a>Destekleniyor
- Yük veri GİRİŞDOSYASI - desteklenir, ancak bir UNC yolu (Azure depolama XSMB bağlanan) yönlendirildiği [Yerel] parametresini belirtmeniz gerekir.

### <a name="unsupported"></a>Desteklenmeyen
- SEÇİN... ÇIKIŞDOSYASI

## <a name="preview-functional-limitations"></a>Önizleme işlevsel sınırlamaları

### <a name="scale-operations"></a>Ölçek işlemleri
- Dinamik sunucularının hizmet katmanları arasında ölçeklendirme şu anda desteklenmiyor. Diğer bir deyişle, temel ve standart hizmet katmanları arasında geçiş yapma.
- Önceden oluşturulmuş sunucusundaki depolama dinamik talep üzerine çıktığını şu anda desteklenmiyor.
- Sunucu depolama boyutunu azaltmak desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürüm yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- Farklı bir hizmet katmanı ve/veya bir işlem birimleri ve depolama boyutu geri izin verilmiyor.
- Silinen bir sunucuya geri yüklenmesi desteklenmez.

## <a name="functional-limitations"></a>İşlev sınırlamaları

### <a name="subscription-management"></a>Abonelik Yönetimi
- Önceden oluşturulmuş sunucuları abonelik ve kaynak grubu arasında dinamik olarak taşıma şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında nelerin kullanılabildiğini](concepts-service-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
