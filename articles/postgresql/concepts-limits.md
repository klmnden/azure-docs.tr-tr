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
ms.date: 12/04/2017
ms.openlocfilehash: 6dbed1a834d74047178a9f996683d65520047e66
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>PostgreSQL Azure veritabanındaki sınırlamaları
Azure veritabanı PostgreSQL hizmeti için genel önizlemede değil. Aşağıdaki bölümlerde, kapasite ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır.

## <a name="service-tier-maximums"></a>Hizmet katmanı üst sınırlar
Azure veritabanı PostgreSQL için bir sunucu oluştururken seçebileceğiniz birden çok hizmet katmanı içerir. Daha fazla bilgi için bkz: [her hizmet katmanında nelerin kullanılabildiğini anlama](concepts-service-tiers.md).  

Olduğundan maksimum sayısını bağlantıları, işlem birimleri ve depolama her hizmet katmanında hizmet Önizleme sırasında şu şekilde: 

| | |
| :------------------------- | :---------------- |
| **En fazla bağlantı**        |                   |
| Temel 50 işlem birimleri     | 55 bağlantıları    |
| Temel 100 işlem birimleri    | 105 bağlantıları   |
| Standart 100 işlem birimleri | 150 bağlantıları   |
| Standart 200 işlem birimleri | 250 bağlantıları   |
| Standart 400 işlem birimleri | 480 bağlantıları   |
| Standart 800 işlem birimleri | 950 bağlantıları   |
| **En fazla işlem birimleri**      |                   |
| Temel hizmet katmanı         | 100 işlem birimleri |
| Standart hizmet katmanı      | 800 işlem birimleri |
| **En fazla depolama**            |                   |
| Temel hizmet katmanı         | 1 TB              |
| Standart hizmet katmanı      | 1 TB              |

Azure sistemi PostgreSQL server Azure veritabanındaki izlemek için beş bağlantı gerektirir. Çok fazla bağlantı erişildiğinde, aşağıdaki hata iletisini alabilirsiniz:
> Önemli: ne yazık ki zaten çok fazla sayıda istemci


## <a name="preview-functional-limitations"></a>Önizleme işlevsel sınırlamaları
### <a name="scale-operations"></a>Ölçek işlemleri
1.  Dinamik sunucularının hizmet katmanları arasında ölçeklendirme şu anda desteklenmiyor. Diğer bir deyişle, temel ve standart hizmet katmanları arasında geçiş yapma.
2.  Önceden oluşturulmuş sunucusundaki depolama dinamik talep üzerine çıktığını şu anda desteklenmiyor.
3.  Sunucu depolama boyutunu azaltmak desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürüm yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmiyor.

### <a name="subscription-management"></a>Abonelik yönetimi
- Önceden oluşturulmuş sunucuları abonelik ve kaynak grubu arasında dinamik olarak taşıma şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
1.  Farklı bir hizmet katmanı ve/veya bir işlem birimleri ve depolama boyutu geri izin verilmiyor.
2.  Bırakılan bir sunucuya geri yüklenmesi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Anlamak [her fiyatlandırma katmanının kullanılabilir](concepts-service-tiers.md)
- Anlamak [PostgreSQL veritabanı sürümleri desteklenir](concepts-supported-versions.md)
- Gözden geçirme [yedeklemek ve Azure veritabanındaki bir sunucu için Azure portalını kullanarak PostgreSQL geri yükleme](howto-restore-server-portal.md)
