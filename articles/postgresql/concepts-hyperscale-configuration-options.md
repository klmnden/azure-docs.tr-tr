---
title: PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı performans seçenekleri
description: Bilgi işlem, depolama ve bölgeler gibi bir hiper ölçekli (Citus) sunucu grubu için Seçenekler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e8c1c2e51ca14ae9b17f0d7efb20552cdd55139b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077298"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-preview-performance-options"></a>PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı performans seçenekleri

## <a name="compute-and-storage"></a>İşlem ve depolama
 
Çalışan düğümleri için ayrı ayrı bilgi işlem ve depolama ayarları ve düzenleyici düğüm bir hiper ölçekli (Citus) sunucu grubunda seçebilirsiniz.  İşlem kaynakları, temel alınan donanım mantıksal CPU'yu temsil eden sanal çekirdekler sağlanır. Sağlama için depolama alanı boyutu (Citus) hiper ölçekli sunucu grubunuz Düzenleyicisi ve çalışan düğümleri için kullanılabilir kapasiteyi ifade eder. Depolama, veritabanı dosyaları, geçici dosyaları, işlem günlükleri ve Postgres server günlükleri içerir. Ayrıca sağlama depolama toplam miktarı her çalışan ve düzenleyici düğüme g/ç kapasitesi kullanılabilir tanımlar.
 
|                       | Çalışan düğümü           | Koordinatör düğümü      |
|-----------------------|-----------------------|-----------------------|
| İşlem, sanal çekirdek       | 4, 8, 16, 32          | 4, 8, 16, 32          |
| Sanal çekirdek, GiB başına bellek | 8                     | 4                     |
| Depolama boyutu, TiB     | 0.5, 1, 2             | 0.5, 1, 2             |
| Depolama türü          | Genel amaçlı (SSD) | Genel amaçlı (SSD) |
| IOPS                  | En fazla 3 IOPS/GiB      | En fazla 3 IOPS/GiB      |


## <a name="regions"></a>Bölgeler
Hiper ölçekli (Citus) sunucu grupları, Azure şu bölgelerde kullanılabilir:
* Doğu ABD 2
* Güneydoğu Asya
* Batı Avrupa
* Batı ABD 2

## <a name="pricing"></a>Fiyatlandırma
En güncel fiyatlandırma bilgileri için bkz: hizmeti [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/postgresql/).
İstediğiniz yapılandırmanın maliyetini görmek için [Azure portalında](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) aylık maliyeti gösterilir **yapılandırma** sekmesinde belirlediğiniz seçeneklere bağlı. Azure aboneliğiniz yoksa, bir tahmini fiyatı almak için Azure fiyatlandırma hesaplayıcısı'nı kullanabilirsiniz. Üzerinde [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Web sitesi, select **öğeleri ekleme**, genişletin **veritabanları** kategorisi seçin **– PostgreSQL için Azure veritabanı Hiper ölçekli (Citus)** seçenekleri özelleştirmek için.
 
## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [portalda bir hiper ölçekli (Citus) sunucu grubu oluşturma](quickstart-create-hyperscale-portal.md).
