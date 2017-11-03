---
title: "Azure veritabanı sınırlamalarını PostgreSQL için | Microsoft Docs"
description: "PostgreSQL Azure veritabanındaki sınırlamalar açıklanır."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.openlocfilehash: 38988fc5c0dc05331ea078534cd1a05e9eca2493
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a>PostgreSQL Azure veritabanındaki sınırlamaları
Azure veritabanı PostgreSQL hizmeti için genel önizlemede değil. Aşağıdaki bölümlerde, kapasite ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır.

## <a name="service-tier-maximums"></a>Hizmet katmanı üst sınırlar
Azure veritabanı PostgreSQL için bir sunucu oluştururken seçebileceğiniz birden çok hizmet katmanı içerir. Daha fazla bilgi için bkz: [her hizmet katmanında nelerin kullanılabildiğini anlama](concepts-service-tiers.md).  

Olduğundan maksimum sayısını bağlantıları, işlem birimleri ve depolama her hizmet katmanında hizmet Önizleme sırasında şu şekilde: 

|                            |                   |
| :------------------------- | :---------------- |
| **En fazla bağlantı**        |                   |
| Temel 50 işlem birimleri     | 50 bağlantıları    |
| Temel 100 işlem birimleri    | 100 bağlantılar   |
| Standart 100 işlem birimleri | 200 bağlantıları   |
| Standart 200 işlem birimleri | 300 bağlantıları   |
| Standart 400 işlem birimleri | 400 bağlantıları   |
| Standart 800 işlem birimleri | 500 bağlantılar   |
| **En fazla işlem birimleri**      |                   |
| Temel hizmet katmanı         | 100 işlem birimleri |
| Standart hizmet katmanı      | 800 işlem birimleri |
| **En fazla depolama**            |                   |
| Temel hizmet katmanı         | 1 TB              |
| Standart hizmet katmanı      | 1 TB              |

Çok fazla bağlantı erişildiğinde, aşağıdaki hata iletisini alabilirsiniz:
> Önemli: ne yazık ki zaten çok fazla sayıda istemci

## <a name="preview-functional-limitations"></a>Önizleme işlevsel sınırlamaları
### <a name="scale-operations"></a>Ölçek işlemleri
1.  Dinamik sunucularının hizmet katmanları arasında ölçeklendirme şu anda desteklenmiyor. Diğer bir deyişle, temel ve standart hizmet katmanları arasında geçiş yapma.
2.  Önceden oluşturulmuş sunucusundaki depolama dinamik talep üzerine çıktığını şu anda desteklenmiyor.
3.  Sunucu depolama boyutunu azaltmak desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürüm yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmiyor.

### <a name="subscription-management"></a>Abonelik Yönetimi
- Önceden oluşturulmuş sunucuları abonelik ve kaynak grubu arasında dinamik olarak taşıma şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
1.  Farklı bir hizmet katmanı ve/veya bir işlem birimleri ve depolama boyutu geri izin verilmiyor.
2.  Bırakılan bir sunucuya geri yüklenmesi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Anlamak [her fiyatlandırma katmanının kullanılabilir](concepts-service-tiers.md)
- Anlamak [PostgreSQL veritabanı sürümleri desteklenir](concepts-supported-versions.md)
- Gözden geçirme [yedeklemek ve Azure veritabanındaki bir sunucu için Azure portalını kullanarak PostgreSQL geri yükleme](howto-restore-server-portal.md)
