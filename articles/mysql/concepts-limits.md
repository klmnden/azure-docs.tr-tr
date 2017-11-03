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
ms.date: 10/26/2017
ms.openlocfilehash: b3fba38cacf5b5abcdea7f0def8c1d39e653f0a8
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>MySQL (Önizleme) Azure veritabanındaki sınırlamaları
MySQL hizmeti için Azure veritabanı genel önizlemede değil. Aşağıdaki bölümlerde, kapasite ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır. Ayrıca bkz. [genel sınırlamaları](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) MySQL veritabanı altyapısı için geçerlidir.

## <a name="service-tier-maximums"></a>Hizmet katmanı üst sınırlar
Azure veritabanı MySQL için bir sunucu oluştururken seçebileceğiniz birden çok hizmet katmanı vardır. Daha fazla bilgi için bkz: [her hizmet katmanında nelerin kullanılabildiğini anlama](concepts-service-tiers.md).  

En fazla sayı bulunmadığını bağlantıları, işlem birimleri ve depolama her hizmet katmanında Önizleme sırasında gibi: 

|                            |                   |
| :------------------------- | :---------------- |
| **En fazla bağlantı**        |                   |
| Temel 50 işlem birimleri     | 50 bağlantıları    |
| Temel 100 işlem birimleri    | 100 bağlantılar   |
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

## <a name="preview-functional-limitations"></a>Önizleme işlevsel sınırlamaları

### <a name="scale-operations"></a>Ölçek işlemleri
- Dinamik sunucularının hizmet katmanları arasında ölçeklendirme şu anda desteklenmiyor. Diğer bir deyişle, temel ve standart hizmet katmanları arasında geçiş yapma.
- Önceden oluşturulmuş sunucusundaki depolama dinamik talep üzerine çıktığını şu anda desteklenmiyor.
- Sunucu depolama boyutunu azaltmak desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürüm yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmiyor.

### <a name="subscription-management"></a>Abonelik Yönetimi
- Önceden oluşturulmuş sunucuları abonelik ve kaynak grubu arasında dinamik olarak taşıma şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- Farklı bir hizmet katmanı ve/veya bir işlem birimleri ve depolama boyutu geri izin verilmiyor.
- Bırakılan bir sunucuya geri yüklenmesi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında nelerin kullanılabildiğini](concepts-service-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
